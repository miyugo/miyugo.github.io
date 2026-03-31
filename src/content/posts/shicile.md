---
title: 诗词乐markdown解析
published: 2026-03-31
description: 诗词乐网站markdown解析
tags: [poems, scripts]
category: scripts
draft: false
---
```
import requests
from bs4 import BeautifulSoup
import re
import os
from urllib.parse import urlparse
from datetime import datetime

def ensure_chinese_directory():
    """确保中文字符目录存在"""
    directory = "诗词文档"
    if not os.path.exists(directory):
        os.makedirs(directory)
    return directory

def extract_pinyin_from_html(html_content):
    """从HTML内容中提取拼音信息"""
    soup = BeautifulSoup(html_content, 'html.parser')
    
    # 查找所有ruby标签
    ruby_tags = soup.find_all('ruby')
    pinyin_map = {}
    
    for ruby in ruby_tags:
        # 获取汉字
        chinese_char = ruby.get_text(strip=True)
        # 获取拼音
        rt_tag = ruby.find('rt')
        if rt_tag:
            pinyin = rt_tag.get_text(strip=True)
            pinyin_map[chinese_char] = pinyin
    
    return pinyin_map

def add_pinyin_to_text(text, pinyin_map):
    """使用从网页提取的拼音映射为文本添加拼音标注"""
    result = ""
    for char in text:
        if char in pinyin_map:
            result += f'<ruby>{char}<rt>{pinyin_map[char]}</rt></ruby>'
        else:
            result += char
    return result

def extract_author_with_pinyin(soup):
    """专门提取作者信息，包括完整的拼音"""
    # 查找作者标签
    author_tag = soup.find("p", class_="card-subtitle mb-2 text-muted mr-1")
    if not author_tag:
        # 如果找不到，尝试其他可能的类名
        author_tag = soup.find("p", class_=re.compile(r"card-subtitle|text-muted|author"))
    
    if author_tag:
        # 直接获取作者HTML内容，保留拼音标注
        author_html = str(author_tag)
        
        # 清理HTML标签，但保留ruby标签
        author_html = re.sub(r'\sclass=".*?"', '', author_html)
        author_html = re.sub(r'\sstyle=".*?"', '', author_html)
        
        # 提取纯文本作者名用于显示
        author_text = author_tag.get_text(strip=True)
        author_text = re.sub(r'[【】\[\]]', '', author_text)
        
        return author_text, author_html
    
    return "未知作者", "未知作者"

def extract_poem_info(url):
    """
    从诗词网页提取所有信息
    """
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
    }
    
    try:
        resp = requests.get(url, headers=headers, timeout=10)
        resp.encoding = "utf-8"
        resp.raise_for_status()
        soup = BeautifulSoup(resp.text, "html.parser")
        
        # 提取标题
        title_tag = soup.find("h2", class_="text-center")
        title = title_tag.get_text(strip=True) if title_tag else "未知标题"
        title = re.sub(r"-拼音版-.*", "", title).strip()
        
        # 专门提取作者信息和拼音
        author, author_html = extract_author_with_pinyin(soup)
        
        # 从整个页面提取拼音映射
        pinyin_map = extract_pinyin_from_html(resp.text)
        
        # 提取拼音正文
        pinyin_section = soup.find("div", class_="pinyin")
        if not pinyin_section:
            ruby_tags = soup.find_all("ruby")
            if ruby_tags:
                for ruby in ruby_tags[:1]:
                    parent = ruby.find_parent("div")
                    if parent and len(parent.find_all("ruby")) > 3:
                        pinyin_section = parent
                        break
        
        # 格式化拼音段落 - 更准确地移除重复的作者信息
        pinyin_paragraphs = []
        if pinyin_section:
            paragraphs = pinyin_section.find_all("p")
            
            # 创建一个集合来跟踪已经处理过的作者信息
            processed_authors = set()
            # 提取纯文本作者名用于比较
            author_clean = re.sub(r'[()（）]', '', author).strip()
            
            for p in paragraphs:
                html = str(p)
                html = re.sub(r'\sclass=".*?"', '', html)
                html = re.sub(r'\sstyle=".*?"', '', html)
                
                paragraph_text = p.get_text(strip=True)
                
                # 检查是否为作者信息段落
                # 作者信息通常包含朝代和作者名，可能用括号括起来
                is_author_paragraph = (
                    '(' in paragraph_text and ')' in paragraph_text and
                    len(paragraph_text) < 20  # 作者段落通常较短
                )
                
                # 如果是作者信息段落，检查是否已经处理过
                if is_author_paragraph:
                    # 标准化作者信息以便比较
                    normalized_author = re.sub(r'[()（）\s]', '', paragraph_text)  # 移除所有括号和空格
                    
                    # 如果这个作者信息已经处理过，跳过这个段落
                    if normalized_author in processed_authors:
                        continue
                    
                    # 或者如果这个作者信息与页面提取的作者匹配，也跳过
                    author_clean_normalized = re.sub(r'[()（）\s]', '', author_clean)
                    if normalized_author == author_clean_normalized:
                        continue
                    
                    # 否则标记为已处理，并保留这个段落
                    processed_authors.add(normalized_author)
                    # 作者信息使用居中对齐
                    html = html.replace('<p', '<p align="center"')
                else:
                    # 正文部分使用两端对齐
                    html = html.replace('<p', '<p align="justify"')
                
                pinyin_paragraphs.append(html)
        
        # 提取注释、译文和赏析
        sections = soup.find_all("h3")
        content_dict = {"注释": "", "白话译文": "", "赏析": ""}
        
        for h3 in sections:
            section_title = h3.get_text(strip=True)
            if section_title in content_dict:
                # 查找内容容器
                content_div = h3.find_next_sibling("div")
                if not content_div:
                    content_div = h3.find_next_sibling("p")
                
                if content_div:
                    # 关键修改：使用 get_text(separator='\n')，这样 <br> 会被替换为 \n 换行符
                    text_content = content_div.get_text(separator='\n')
                    
                    # 优先按换行符（即原 <br> 标签）分割
                    raw_paragraphs = text_content.split('\n')
                    
                    cleaned_paragraphs = []
                    for p in raw_paragraphs:
                        # 对于每个按 <br> 分割出的部分，再兼容处理下按全角空格分割的可能
                        sub_paras = re.split(r'　　', p)
                        cleaned_paragraphs.extend([sp.strip() for sp in sub_paras if sp.strip()])
                    
                    content_dict[section_title] = cleaned_paragraphs
        
        return {
            "title": title,
            "author": author,
            "author_html": author_html,  # 添加完整的作者HTML
            "pinyin_map": pinyin_map,
            "pinyin_paragraphs": pinyin_paragraphs,
            "annotations": content_dict["注释"],
            "translation": content_dict["白话译文"],
            "appreciation": content_dict["赏析"],
            "url": url
        }
        
    except Exception as e:
        print(f"提取诗词信息时出错: {e}")
        return None

def format_annotations_as_ordered_list(annotations_text):
    """将注释格式化为有序列表"""
    if not annotations_text or (isinstance(annotations_text, list) and not annotations_text):
        return "暂无注释"
    
    # 统一将输入作为列表处理（避免旧代码中将列表 " ".join() 缝合成字符串）
    if isinstance(annotations_text, str):
        if "拼音有误" in annotations_text:
            return "暂无注释"
        items = [annotations_text]
    else:
        if any("拼音有误" in str(item) for item in annotations_text):
            return "暂无注释"
        items = annotations_text
    
    # 格式化为有序列表
    formatted_list = []
    for i, item in enumerate(items, 1):
        # 清理文本：移除原有的序号（如"1."、"1、"、"一、"），防止出现 "1. 1. 记：..."
        cleaned_item = re.sub(r'^(\d+[.、]\s*|[一二三四五六七八九十]+、\s*)', '', item)
        formatted_list.append(f"{i}. {cleaned_item}")
    
    return "\n".join(formatted_list)

def format_translation(translation_paragraphs):
    """格式化白话译文 - 每个段落以两个全角空格开头，除最后一段外末尾都添加两个半角空格"""
    if not translation_paragraphs or (isinstance(translation_paragraphs, list) and not translation_paragraphs):
        return "暂无译文"
    
    # 如果是字符串，转换为列表
    if isinstance(translation_paragraphs, str):
        if "拼音有误" in translation_paragraphs:
            return "暂无译文"
        # 尝试按段落分割
        paragraphs = re.split(r'　　', translation_paragraphs)
        translation_paragraphs = [p.strip() for p in paragraphs if p.strip()]
    
    formatted_paragraphs = []
    
    for i, para in enumerate(translation_paragraphs):
        para = para.strip()
        if para:
            # 每个段落以两个全角空格开头
            formatted_para = "　　" + para
            
            # 如果不是最后一个段落，在末尾添加两个半角空格
            if i < len(translation_paragraphs) - 1:
                formatted_para += "  "  # 两个半角空格
            
            formatted_paragraphs.append(formatted_para)
    
    # 使用换行符连接段落
    return "\n".join(formatted_paragraphs)

def format_appreciation(appreciation_paragraphs):
    """格式化赏析 - 每个段落以两个全角空格开头，除最后一段外末尾都添加两个半角空格"""
    if not appreciation_paragraphs or (isinstance(appreciation_paragraphs, list) and not appreciation_paragraphs):
        return "暂无赏析"
    
    # 如果是字符串，转换为列表
    if isinstance(appreciation_paragraphs, str):
        if "拼音有误" in appreciation_paragraphs:
            return "暂无赏析"
        # 尝试按段落分割
        paragraphs = re.split(r'　　', appreciation_paragraphs)
        appreciation_paragraphs = [p.strip() for p in paragraphs if p.strip()]
    
    formatted_paragraphs = []
    
    for i, para in enumerate(appreciation_paragraphs):
        para = para.strip()
        if para:
            # 每个段落以两个全角空格开头
            formatted_para = "　　" + para
            
            # 如果不是最后一个段落，在末尾添加两个半角空格
            if i < len(appreciation_paragraphs) - 1:
                formatted_para += "  "  # 两个半角空格
            
            formatted_paragraphs.append(formatted_para)
    
    # 使用换行符连接段落
    return "\n".join(formatted_paragraphs)

def create_markdown(poem_info):
    """根据提取的信息创建Markdown文件"""
    title = poem_info["title"]
    author = poem_info["author"]
    pinyin_map = poem_info["pinyin_map"]
    
    # 为标题添加拼音（使用从网页提取的拼音映射）
    title_with_pinyin = add_pinyin_to_text(title, pinyin_map)
    
    # 使用直接从网页提取的作者HTML，确保拼音完整
    author_with_pinyin = poem_info["author_html"]
    
    current_date = datetime.now().strftime("%Y-%m-%d")    
    
    # 构建Markdown内容
    md_content = f"""---
title: {title}
published: {current_date}
description: '古诗词'
image: ''
tags: [poems]
category: 'poems'
draft: false 
lang: ''
---

# 全文拼音

<p align="center"><strong>{title_with_pinyin}</strong></p>
"""
    
    if author_with_pinyin and author_with_pinyin != "未知作者":
        # 使用居中对齐
        author_with_pinyin = author_with_pinyin.replace('<p', '<p align="center"')
        md_content += f'{author_with_pinyin}\n\n'
    
    # 添加拼音段落
    if poem_info["pinyin_paragraphs"]:
        md_content += "\n\n".join(poem_info["pinyin_paragraphs"]) + "\n\n"
    else:
        md_content += "> ⚠️ 拼音正文未抓取到，可能是网页结构变化。\n\n"
    
    # 添加注释（有序列表）
    annotations_content = format_annotations_as_ordered_list(poem_info["annotations"])
    
    # 添加白话译文
    translation_content = format_translation(poem_info["translation"])
    
    # 添加赏析
    appreciation_content = format_appreciation(poem_info["appreciation"])
    
    md_content += f"""# 注释

{annotations_content}

# 白话译文

{translation_content}

# 赏析

{appreciation_content}

---

*本文档自动生成于诗词乐网站*
*原文链接: {poem_info["url"]}*
"""
    
    # 生成文件名
    filename = f"{title} - {author}.md"
    filename = re.sub(r"[\\/:*?\"<>|]", "_", filename)
    
    # 保存文件
    directory = ensure_chinese_directory()
    filepath = os.path.join(directory, filename)
    
    with open(filepath, "w", encoding="utf-8") as f:
        f.write(md_content)
    
    return filepath

def process_poem_url(url):
    """处理单个诗词URL"""
    print(f"正在处理: {url}")
    
    # 提取诗词信息
    poem_info = extract_poem_info(url)
    if not poem_info:
        print(f"❌ 无法提取诗词信息: {url}")
        return None
    
    # 创建Markdown文件
    filepath = create_markdown(poem_info)
    
    # 输出统计信息
    print(f"✅ 已生成: {filepath}")
    print(f"📊 统计信息:")
    print(f"   标题: {poem_info['title']}")
    print(f"   作者: {poem_info['author']}")
    print(f"   拼音映射表大小: {len(poem_info['pinyin_map'])}")
    print(f"   拼音段落数: {len(poem_info['pinyin_paragraphs'])}")
    
    # 处理注释、译文和赏析的统计信息
    annotations = poem_info['annotations']
    translation = poem_info['translation']
    appreciation = poem_info['appreciation']
    
    has_annotations = False
    if isinstance(annotations, list):
        has_annotations = len(annotations) > 0 and not any("拼音有误" in str(a) for a in annotations)
    else:
        has_annotations = annotations and "拼音有误" not in annotations
    
    has_translation = False
    if isinstance(translation, list):
        has_translation = len(translation) > 0 and not any("拼音有误" in str(t) for t in translation)
    else:
        has_translation = translation and "拼音有误" not in translation
    
    has_appreciation = False
    if isinstance(appreciation, list):
        has_appreciation = len(appreciation) > 0 and not any("拼音有误" in str(a) for a in appreciation)
    else:
        has_appreciation = appreciation and "拼音有误" not in appreciation
    
    print(f"   注释: {'有' if has_annotations else '无'}")
    print(f"   译文: {'有' if has_translation else '无'}")
    print(f"   赏析: {'有' if has_appreciation else '无'}")
    
    return filepath

def main():
    """主函数 - 可以处理单个或多个URL"""
    # 可以在这里添加多个URL
    urls = [
        "https://www.shicile.com/detail/834029888503801", 
        # 可以添加更多URL，例如:
        # "https://www.shicile.com/detail/9090298923160",   # 岳阳楼记
        # "https://www.shicile.com/detail/540094591574",     # 洛神赋
        # "https://www.shicile.com/detail/7360098413110",   # 出师表
        # "https://www.shicile.com/detail/1290299354172",   # 陋室铭
    ]
    
    # 如果没有指定URL，可以手动输入
    if not urls:
        url = input("请输入诗词URL: ").strip()
        if url:
            urls = [url]
    
    # 处理每个URL
    for url in urls:
        try:
            process_poem_url(url)
            print("-" * 50)
        except Exception as e:
            print(f"❌ 处理URL时出错 {url}: {e}")
            print("-" * 50)

if __name__ == "__main__":
    main()
```
