# 電子相本平台 

## 產品概述

本產品為基於樹梅派4的電子相本系統，結合本地網路內的NFS伺服器目錄，從指定路徑中隨機播放圖片與影片，實現一個智能且彈性的多媒體展示平台。使用者可設定多個資料夾來源，並排除特定目錄，支援常見的圖片與影片格式。

## 目標平台

- 樹梅派4 (Raspberry Pi 4)
- Windows PC（支援 SMB 協定）
- 顯示器（透過 HDMI 或內建顯示器）

## 功能需求

### 多媒體播放功能

- 隨機播放選定目錄中之檔案（包含子目錄）。
- 支援的格式：
  - 圖片：png, jpg, jpeg
  - 影片：mp4, mov, avi
- 保留原始比例顯示，內容自動最大化。
- 支援全螢幕播放模式。

### 目錄設定

- 支援多個NFS掛載目錄作為來源。
- 可設定例外（排除）目錄，避免該路徑中的內容被播放。

### 播放控制

- 可設定播放間隔時間（以秒為單位），適用於圖片。
- 影片播放完畢自動跳下一個媒體檔。

### 使用者介面（可選）

- 簡易設定檔或網頁介面供使用者管理播放目錄、排除路徑、間隔設定。

## 技術需求

- 支援網路磁碟掛載：
  - 樹梅派平台使用 NFS 掛載（需於開機時自動掛載）
  - Windows 平台使用 SMB 掛載（可透過網路磁碟方式自動連接）
- 程式語言：Rust

## 非功能需求

- 系統需穩定運作，適合長時間無人看管的播放情境。
- 啟動後自動進入播放模式。
- 播放錯誤需有記錄，並自動跳過。

## 設定檔範例

使用 YAML 格式進行設定

```
mount_type: nfs  # 可選值：nfs 或 smb
media_dirs:
  - /mnt/nfs/a
  - /mnt/nfs/b
exclude_dirs:
  - /mnt/nfs/photos/private
  - /mnt/nfs/videos/raw
interval_seconds: 10
supported_formats:
  - jpg
  - png
  - mp4
  - mov
  - avi
fullscreen: true
log_file: /var/log/digital_frame.log
```
```
mount_type: smb  # 可選值：nfs 或 smb
media_dirs:
  - \\\\winserver\\media\\shared1
  - \\\\winserver\\media\\shared2
exclude_dirs:
  - \\\\winserver\\media\\shared1\\private
interval_seconds: 10
supported_formats:
  - jpg
  - png
  - mp4
  - mov
  - avi
fullscreen: true
log_file: /var/log/digital_frame.log

```

## 未來擴充

- 支援排程播放（例如依時間播放特定目錄）
- 支援顯示當前檔案名稱或路徑（可選開關）
- 支援觸控或按鍵切換媒體

## 優先順序

1. 基本播放功能（圖片與影片）
2. 多目錄與排除目錄設定
3. 播放間隔與隨機化控制
4. 原比例最大化顯示
5. 設定方式（初期可用設定檔，未來可導入UI）