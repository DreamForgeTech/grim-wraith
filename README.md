[轻音_README.md](https://github.com/user-attachments/files/34867778/_README.md)
# 轻音 QingYin

一个轻量、纯本地的 Android 音乐播放器。不联网也能播，歌词可以联网补全，主题、语言、标签全部可自定义。

由 [造梦科技 Dreamforgetech](https://github.com/Dreamforgetech) 开发维护。

---

## 功能特点

### 播放
- 扫描本地音频（MediaStore + 按文件夹递归），分钟级完成首轮扫描
- 后台前台服务播放，通知栏控制、耳机线控、播放模式（顺序 / 单曲 / 随机）
- 悬浮歌词小窗，可拖动，随歌词偏移实时刷新

### 浏览与整理
- 三种视图：全部歌曲 / 按文件夹 / 按歌手
- 文件夹视图进入时自动展开到最后一个文件夹层级，叶子文件夹保持折叠
- 歌手分组默认折叠，未知歌手固定排在最后
- 实时搜索过滤

### 歌词
- 默认歌词源为网易云音乐，并自动依次回退：网易云 → LRCLIB → QQ 音乐 → 酷狗 → lyrics.ovh
- 支持自定义歌词接口，占位符 `{title}` `{artist}`，返回 LRC 或纯文本均可
- 获取歌词的同时回填歌手、专辑、歌名等元数据
- 歌词逐行高亮，点击歌词行直接跳转到对应时间
- 一键整体偏移 ±0.5s，另有「微调」小窗，可输入任意毫秒（支持负数与小数）
- 偏移结果立即生效、立即刷新界面与悬浮窗，并直接写入 MP3 的 `USLT` 歌词标签，清空应用数据也不会丢失

### 标签编辑
- 播放页进入编辑，可修改：标题、歌手、专辑、年份、流派、音轨号、备注、歌词
- 只替换改动过的 ID3v2 帧，封面（APIC）与未知帧原样保留
- 保存后立即写入 MP3 并刷新媒体库，界面数据同步更新

### 外观与语言
- 8 套深色主题：默认蓝、绿色、粉色、紫罗兰、落日橙、青碧、玫瑰红等，强调色、卡片、输入框、圆角同步换色
- 界面中英文切换，安装后首次启动会提示选择语言
- 运行时切换语言，无需重启应用

### 定时关闭
- 支持 15 / 30 / 60 / 90 分钟定时，显示剩余时间
- 到点自动停止播放并关闭悬浮窗
- 可选「到时自动关机」（需要 root 权限；无 root 时仅停止播放）

---

## 安装

下载最新 APK 直接安装即可，无需 root。

首次使用请授予「所有文件访问权限」，否则无法扫描歌曲和写入标签。

## 构建

环境要求：JDK 17、Android SDK（compileSdk 34）、Gradle 8.x。

```bash
git clone https://github.com/Dreamforgetech/<repo>.git
cd <repo>

export JAVA_HOME=/path/to/jdk-17
export ANDROID_HOME=/path/to/android-sdk

gradle --no-daemon assembleDebug
# 产物：app/build/outputs/apk/debug/app-debug.apk
```

或直接用 Android Studio 打开工程运行。

## 技术说明

| 项目 | 说明 |
| --- | --- |
| 语言 | Java（无 AndroidX，兼容最低 Android 4.1） |
| minSdk / targetSdk / compileSdk | 16 / 29 / 34 |
| 包名 | `com.qingyin.player` |
| 网络 | `HttpURLConnection` + `org.json`，无第三方依赖 |
| ID3 | 自研 ID3v2 读写，保留未知帧与封面 |
| 歌词缓存 | 应用私有目录 + MP3 `USLT` 标签双写 |

### 目录结构

```
app/src/main/java/com/qingyin/player/
├── MainActivity.java            主界面：扫描、列表、视图切换、搜索
├── NowPlayingActivity.java      播放页：歌词、偏移、进度、悬浮窗
├── TagEditActivity.java         标签编辑页
├── SettingsActivity.java        设置：主题、语言、歌词源、定时关闭
├── PlayerService.java           前台服务：通知、悬浮窗、歌词轮询、定时关闭
├── data/                        媒体库、封面、歌词存储与解析
├── id3/                         ID3v2 标签读写
├── lyrics/                      多源歌词抓取、LRC 解析
├── play/                        播放核心
├── ui/                          列表与歌词适配器
└── util/                        主题、偏好、语言、工具
```

### 权限

| 权限 | 用途 |
| --- | --- |
| `READ_EXTERNAL_STORAGE` / `WRITE_EXTERNAL_STORAGE` | 读取音频、写回标签 |
| `MANAGE_EXTERNAL_STORAGE` | Android 11+ 全盘扫描与写入（可选授权） |
| `INTERNET` | 联网获取歌词 |
| `SYSTEM_ALERT_WINDOW` | 悬浮歌词小窗 |
| `FOREGROUND_SERVICE` | 后台播放 |
| `POST_NOTIFICATIONS` | 播放通知 |

## 已知限制

- 自动关机依赖 root 权限，普通设备上只能停止播放
- 歌词接口均为公开第三方接口，可能因对方调整而失效
- 未适配平板与横屏布局

## 反馈

访问团队主页：[github.com/Dreamforgetech](https://github.com/Dreamforgetech)

---

<sub>Made by 造梦科技 · Dreamforgetech</sub>
