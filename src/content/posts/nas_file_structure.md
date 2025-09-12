---
title: 文件目录结构与命名规则
published: 2025-09-12
description: 家用 NAS 文件存储与命名规范
tags: [Nas, File, Structure]
category: Nas
draft: false
---
# 家用 NAS 文件存储与命名规范

## 📌 总体原则
1. **分门别类**：顶层目录清晰，不超过 8–10 个大类。  
2. **统一命名**：文件/文件夹统一使用 `英文+数字+下划线/点`，避免中文或特殊符号（部分系统不兼容）。  
3. **搜索友好**：命名中包含关键检索信息（日期、主题、版本、地点）。  
4. **归档管理**：定期将不常用的文件移至 `Archive`，保持目录整洁。  

---

## 🗂 目录结构与示例

```plaintext
NAS/
├── Videos_视频/
│   ├── Movies_电影/
│   │   ├── Inception (2010)/
│   │   │   ├── Inception.2010.Theatrical.1080p.BluRay.H264.AAC.mkv
│   │   │   └── Inception.2010.DirectorsCut.4K.UHD.H265.DTS.mkv
│   │   ├── The.Matrix.Collection/
│   │   │   ├── The.Matrix.1999.1080p.BluRay.x264.mkv
│   │   │   ├── The.Matrix.Reloaded.2003.1080p.BluRay.x264.mkv
│   │   │   └── The.Matrix.Revolutions.2003.1080p.BluRay.x264.mkv
│   │   └── Inception.2010.1080p.BluRay.x264.mkv
│   │
│   ├── TV_Shows_电视剧/
│   │   ├── BreakingBad (2008)/
│   │   │   ├── Season_01/
│   │   │   │   └── BreakingBad.S01E01.1080p.WEB-DL.AAC.mkv
│   │   │   └── Season_02/
│   │   │       └── BreakingBad.S02E01.1080p.WEB-DL.AAC.mkv
│   │   ├── 西游记 (1986)/
│   │   │   └── Season_01/
│   │   │       └── 西游记.S01E01.1986.1080p.HDTV.AAC.mkv
│   │   ├── 西游记续集 (1999)/
│   │   │   └── Season_01/
│   │   │       └── 西游记续集.S01E01.1999.576p.TVrip.mkv
│   │   ├── 西游记TV动画版 (1999)/
│   │   │   └── Season_01/
│   │   │       └── 西游记TV动画版.S01E01.1999.720p.WEB-DL.mkv
│   │   └── 三体 (2023)/
│   │       ├── Season_01/
│   │       │   └── 三体.S01E01.1080p.WEB-DL.mkv
│   │       └── Season_02/
│   │           └── 三体.S02E01.1080p.WEB-DL.mkv
│   │
│   ├── Anime_动漫/
│   │   └── OnePiece (1997)/
│   │       ├── Season_01/
│   │       │   └── OnePiece.S01E01.HR字幕组.720p.mkv
│   │       └── Movies_剧场版/
│   │           └── OnePiece.Movie.2019.Stampede.1080p.BluRay.mkv
│   │
│   ├── Documentary_纪录片/
│   │   └── Planet.Earth.II (2016)/
│   │       └── Planet.Earth.II.S01E01.1080p.WEB-DL.mkv
│   │
│   ├── Variety_综艺/
│   │   └── RunningMan (2010)/
│   │       └── RunningMan.S01E01.720p.HDTV.mp4
│   │
│   ├── Concerts_演唱会/
│   │   └── JayChou - CarnivalWorldTour (2020).1080p.BluRay.mkv
│   │
│   ├── Learning_学习视频/
│   │   └── PythonCourse/
│   │       └── PythonCourse.第01讲.变量与数据类型.720p.mp4
│   │
│   ├── Vlogs_Vlog/
│   │   └── CaseyNeistat/
│   │       └── CaseyNeistat.NYCWalk.20240912.mp4
│   │
│   ├── Dance_舞蹈/
│   │   └── 1MillionDanceStudio/
│   │       └── 1MillionDanceStudio.Dynamite.20230115.mp4
│   │
│   ├── MV_音乐视频/
│   │   └── TaylorSwift - Lover.1080p.MV.mp4
│   │
│   ├── Shorts_短片/
│   │   └── ShortFilm.Lost.20220801.mp4
│   │
│   └── Adult_成人/
│       └── ABP-123.2021.1080p.WEB-DL.mp4
│
├── Music_音乐/
│   ├── By_Artist_按歌手/
│   │   └── JayChou/
│   │       └── Jay (2000)/
│   │           └── JayChou - 可爱女人.flac
│   └── By_Genre_按流派/
│       ├── Rock/
│       │   └── LinkinPark - Numb.mp3
│       ├── Jazz/
│       │   └── MilesDavis - SoWhat.flac
│       └── Pop/
│           └── TaylorSwift - LoveStory.mp3
│
├── Photos_照片/
│   ├── By_Date_按日期/
│   │   └── 2025/
│   │       └── 2025-09-12/
│   │           └── 20250912_183045.jpg
│   └── By_Source_按来源/
│       ├── Phone_手机/
│       │   └── iPhone_20250912_001.jpg
│       ├── Camera_相机/
│       │   └── Canon_20240901_IMG001.CR2
│       └── Downloads_下载/
│           └── Wallpaper_001.jpg
│
├── Ebooks_电子书/
│   ├── Fiction_小说/
│   │   └── 刘慈欣 - 三体.epub
│   ├── Technology_技术/
│   │   └── LinuxKernelDevelopment.3rd.pdf
│   ├── Education_教育/
│   │   └── 数学 - 高等代数.pdf
│   ├── Magazines_杂志/
│   │   └── Nature.No123.2023.pdf
│   └── Comics_漫画/
│       └── OnePiece.Vol001.1997.cbz
│
├── Documents_文档/
│   ├── Work_工作/
│   │   └── ProjectX_设计文档.docx
│   ├── Study_学习/
│   │   └── 2025_机器学习课程笔记.pdf
│   ├── Finance_财务/
│   │   └── 2024_银行流水.xlsx
│   ├── IDs_证件/
│   │   └── 护照.pdf
│   ├── Resume_简历/
│   │   └── 2025_简历_ZhangYunpeng.pdf
│   ├── KnowledgeBase_知识库/
│   │   ├── Tech_技术/
│   │   │   └── Linux_SSH配置_20250912.md
│   │   ├── Study_学习/
│   │   │   └── 机器学习_监督学习_20250912.md
│   │   ├── Life_生活/
│   │   │   └── 健身打卡_20250912.md
│   │   └── Archive_归档/
│   │       └── 2023_旧日记.md
│   ├── Software_软件/
│   │   ├── Windows/
│   │   │   └── Photoshop.2024.exe
│   │   ├── macOS/
│   │   │   └── Photoshop.2024.dmg
│   │   ├── Linux/
│   │   │   └── htop.3.2.2.deb
│   │   ├── Android/
│   │   │   └── WeChat.9.0.0.apk
│   │   └── iOS/
│   │       └── TestFlight.3.5.0.ipa
│   ├── ISOs_系统镜像/
│   │   ├── Windows_Win11_23H2_x64.iso
│   │   ├── Linux_Ubuntu_22.04_x64.iso
│   │   └── macOS_Sonoma_14.5.dmg
│   └── Misc_其他/
│       └── 发票扫描件.pdf
│
└── Temp_临时/
    └── 未分类下载文件_20250912.mkv
```

---

## 📝 文件命名规范

### 🎬 影视资源
- **电影**
  ```
  片名.年份.分辨率.来源.编码格式.扩展名
  示例: Inception.2010.1080p.BluRay.x264.mkv
  ```
- **电视剧 / 动漫 / 综艺**
  ```
  剧名.SxxExx.年份.分辨率.来源.扩展名
  示例: Breaking.Bad.S01E01.2008.1080p.WEB-DL.mkv
  示例: Shingeki.no.Kyojin.S02E01.2017.1080p.WEBRip.mkv
  ```

---

### 🎵 音乐
- **按歌手**
  ```
  歌手/专辑名 (年份)/序号_歌曲名.格式
  示例: The Beatles/Abbey Road (1969)/01_Come Together.mp3
  ```
- **按类型**
  ```
  流派/歌曲名.格式
  示例: Jazz/Autumn Leaves.flac
  ```

---

### 📷 照片
- **按时间**
  ```
  YYYY/MMDD_事件/文件名
  示例: 2025/202501_TokyoTrip/20250101_TokyoTower.jpg
  ```
- **按来源**
  ```
  来源/文件名
  示例: Phone/2025_Phone_Snapshot.jpg
  ```

---

### 📄 文档
```
年份_主题_版本.扩展名
示例: 2025_简历_ZhangYunpeng_v1.0.pdf
```

---

### 💾 软件
- **Windows**
  ```
  软件名_版本号_架构.扩展名
  示例: Office_2021_Pro_Plus_x64.iso
  ```
- **macOS**
  ```
  软件名_版本号.dmg
  示例: Photoshop_2024_v25.1.0.dmg
  ```
- **Linux**
  ```
  软件名_版本号_架构.包格式
  示例: htop_3.2.2_amd64.deb
  ```
- **Android**
  ```
  应用名_版本号.apk
  示例: WeChat_9.0.0.apk
  ```
- **iOS**
  ```
  应用名_版本号.ipa
  示例: TestFlight_3.5.0.ipa
  ```

---

### 💿 系统镜像
```
系统名_版本号_架构.扩展名
示例: Win11_23H2_x64.iso
示例: Ubuntu_24.04_LTS_x64.iso
```
