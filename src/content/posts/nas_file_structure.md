---
title:文件目录结构与命名规则
published: 2025-09-11
description: A simple example of a Markdown blog post.
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
/NAS
├── Videos_视频
│   ├── Movies_电影
│   │   ├── The.Matrix.Collection
│   │   │   ├── The.Matrix.1999.1080p.BluRay.x264.mkv
│   │   │   ├── The.Matrix.Reloaded.2003.1080p.BluRay.x264.mkv
│   │   │   └── The.Matrix.Revolutions.2003.1080p.BluRay.x264.mkv
│   │   └── Inception.2010.1080p.BluRay.x264.mkv
│   │
│   ├── TVShows_电视剧
│   │   ├── Breaking.Bad (2008)
│   │   │   ├── Season.01/Breaking.Bad.S01E01.mkv
│   │   │   └── Season.02/Breaking.Bad.S02E01.mkv
│   │   └── 三体 (2023)
│   │       ├── Season.01/三体.S01E01.1080p.WEB-DL.mkv
│   │       └── Season.02/三体.S02E01.1080p.WEB-DL.mkv
│   │
│   ├── Anime_动漫
│   │   ├── Attack.on.Titan (2013)
│   │   │   ├── Season.01/Shingeki.no.Kyojin.S01E01.mkv
│   │   │   └── Season.02/Shingeki.no.Kyojin.S02E01.mkv
│   │   └── 名侦探柯南
│   │       ├── Season.01/Conan.S01E01.mkv
│   │       └── Season.02/Conan.S02E01.mkv
│   │
│   └── Variety_综艺
│       ├── Running.Man (2010)
│       │   ├── Season.01/Running.Man.S01E01.mkv
│       │   └── Season.02/Running.Man.S02E01.mkv
│       └── 王牌对王牌 (2016)
│           ├── Season.01/王牌对王牌.S01E01.mkv
│           └── Season.02/王牌对王牌.S02E01.mkv
│
├── Music_音乐
│   ├── ByArtist_按歌手
│   │   ├── 周杰伦/Jay (2000)/01_可爱女人.flac
│   │   └── The Beatles/Abbey Road (1969)/01_Come Together.mp3
│   │
│   └── ByGenre_按流派
│       ├── Pop/Example_Song1.mp3
│       ├── Rock/Example_Song2.mp3
│       ├── Jazz/Example_Song3.mp3
│       ├── Classical/Example_Song4.flac
│       └── OST/Example_Soundtrack.mp3
│
├── Photos_照片
│   ├── ByDate_按时间
│   │   ├── 2025/202501_TokyoTrip/20250101_TokyoTower.jpg
│   │   └── 2024/202408_FamilyParty/20240815_GroupPhoto.jpg
│   │
│   └── BySource_按来源
│       ├── Phone/2025_Phone_Snapshot.jpg
│       ├── Camera/2025_Canon_Travel001.jpg
│       ├── Downloaded/Wallpaper_001.jpg
│       └── Screenshots/2025_PC_Screenshot.png
│
├── Documents_文档
│   ├── Work_工作/2025_项目A合同_v1.0.pdf
│   ├── Study_学习/2025_机器学习课程笔记.pdf
│   ├── Finance_财务/2024_银行流水.xlsx
│   ├── IDs_证件/护照.pdf
│   ├── Resume_简历/2025_简历_ZhangYunpeng.pdf
│   └── Others_其他/临时文档.docx
│
├── Software_软件
│   ├── Windows
│   │   ├── Office/Office_2021_Pro_Plus_x64.iso
│   │   ├── Design/Photoshop_2024_v25.1.0.exe
│   │   ├── Dev/VSCode_1.93.0_x64.exe
│   │   └── Tools/Everything_1.4.1.exe
│   │
│   ├── macOS
│   │   ├── Office/Office_2021_Pro_Plus.dmg
│   │   ├── Design/Photoshop_2024_v25.1.0.dmg
│   │   └── Tools/iTerm2_3.5.0.dmg
│   │
│   ├── Linux
│   │   ├── Tools/htop_3.2.2_amd64.deb
│   │   ├── Dev/docker_27.0.0_amd64.rpm
│   │   └── Desktop/Gnome_Extension_45.2.zip
│   │
│   ├── Android
│   │   ├── Social/WeChat_9.0.0.apk
│   │   ├── Tools/Termux_0.118.apk
│   │   └── Media/VLC_3.6.0.apk
│   │
│   └── iOS
│       ├── Social/Telegram_10.2.ipa
│       ├── Tools/TestFlight_3.5.0.ipa
│       └── Media/Spotify_9.1.ipa
│
├── ISOs_系统镜像
│   ├── Windows/Win11_23H2_x64.iso
│   ├── Linux/Ubuntu_24.04_LTS_x64.iso
│   └── macOS/macOS_Sonoma_14.5.dmg
│
├── Others_其他/未分类资料
│
└── Archive_归档
    ├── OldMovies
    ├── OldDocs
    └── OldSoftware
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
