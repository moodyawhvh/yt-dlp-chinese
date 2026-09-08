<div align="center">

# yt-dlp 中文文档

[![原项目](https://img.shields.io/badge/原项目-yt--dlp--yt--dlp-blue?style=flat-square&logo=github)](https://github.com/yt-dlp/yt-dlp)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

> 本文汉化自 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 的官方 README,仅覆盖核心内容,完整说明以英文原版为准。代部署 / 定制服务 / 技术咨询 请添加微信:uaycar

## 项目简介

yt-dlp 是一个功能丰富的命令行音视频下载工具,支持数千个网站。本项目是 youtube-dl 的分支,基于已停止维护的 youtube-dlc 继续开发。

## 安装

可以通过官方发布的二进制文件、pip 或第三方包管理器安装 yt-dlp,详细说明见官方 Wiki 的 Installation 页面。

### 发布文件(代表性条目)

| 文件 | 说明 |
|:-----|:-----|
| [yt-dlp](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp) | 平台无关的 zipimport 二进制,需要 Python(Linux/BSD 推荐) |
| [yt-dlp.exe](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.exe) | Windows(Win8+)独立 x64 可执行文件(Windows 推荐) |
| [yt-dlp_macos](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_macos) | macOS(10.15+)通用独立可执行文件(macOS 推荐) |
| [yt-dlp_linux](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_linux) | Linux(glibc 2.17+)独立 x86_64 可执行文件 |
| [yt-dlp_linux_aarch64](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_linux_aarch64) | Linux(glibc 2.17+)独立 aarch64 可执行文件 |
| [yt-dlp_arm64.exe](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_arm64.exe) | Windows(Win10+)独立 ARM64 可执行文件 |
| [yt-dlp.tar.gz](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.tar.gz) | 源码包(内含 manpage、shell 补全等) |

所有发布文件均附带 SHA2-256SUMS / SHA2-512SUMS 校验和与 GPG 签名,可按官方说明验证完整性。

### 更新

使用发布二进制时可用 `yt-dlp -U` 更新;通过 pip 安装的用户重新执行安装命令即可。

二进制有三个发布通道:`stable`(默认,约每月发布)、`nightly`(每日有变更即发布,官方推荐普通用户使用)、`master`(每次推送到 master 后发布,最新但可能不稳定)。

`--update` / `-U` 只在当前通道内更新;`--update-to CHANNEL` 可切换通道;`--update-to [CHANNEL@]TAG` 可升级/降级到指定版本;`--update-to <owner>/<repository>` 可更新到其他仓库的通道(跨仓库更新不做校验,需谨慎)。示例:

```bash
yt-dlp --update-to master
yt-dlp --update-to stable@2023.07.06
yt-dlp --update-to 2023.10.07
```

stable 版遇到问题时,提交 bug 报告前请先升级到 nightly:

```bash
# stable 可执行文件切换到 nightly
yt-dlp --update-to nightly

# 通过 pip 安装 nightly
python -m pip install -U --pre "yt-dlp[default]"
```

使用超过 90 天的旧版本会提示升级,可在命令或配置文件中加 `--no-update` 关闭该提示。

### 依赖

支持 Python 3.10+(CPython)与 3.11+(PyPy)。其余依赖均为可选,但强烈建议:

- **ffmpeg / ffprobe**:合并分离的视频与音频流,以及各类后处理任务所需。注意需要的是 ffmpeg 二进制文件,而不是 PyPI 上同名的 Python 包;官方在 yt-dlp/FFmpeg-Builds 提供自己的构建。
- **yt-dlp-ejs**:完整支持 YouTube 所需,运行它需要一个 JavaScript 运行时/引擎,如 deno(推荐)、node.js、bun 或 QuickJS。
- **curl_cffi**(可选,推荐):基于 curl-impersonate 的浏览器请求指纹模拟,支持 Chrome / Edge / Safari 指纹,部分使用 TLS 指纹检测的站点需要它,如 `pip install "yt-dlp[default,curl-cffi]"`。
- 元数据类:**mutagen**(部分格式嵌入封面)、**AtomicParsley**、xattr 等。
- 网络与其他:**certifi**、**brotli**、**websockets**、**requests**、**pycryptodomex**(AES-128 HLS 解密)、**secretstorage**(Linux 下读取 Chromium 系浏览器 cookies)等。

独立发布二进制已内置标注 `*` 的依赖;缺少依赖时 yt-dlp 会警告,`--verbose` 输出顶部可查看当前可用的依赖。

### 编译

构建独立可执行文件需要 Python 与 pyinstaller:

```bash
python devscripts/install_deps.py --include-group pyinstaller
python devscripts/make_lazy_extractors.py
python -m bundle.pyinstaller
```

Unix 平台无关二进制需要 python(3.10+)、zip、make(GNU)、pandoc、pytest,装好后运行 `make`(或只编译二进制:`make yt-dlp`)。

## 用法与选项

```
yt-dlp [OPTIONS] [--] URL [URL...]
```

完整选项非常多,可按类别查阅英文原版 USAGE AND OPTIONS 一节,主要包括:通用选项(-h、--version、-U、--update-to、-i 忽略错误等)、网络选项(代理、限速、重试、源 IP 绑定等)、地理限制绕过(--geo-verification-proxy 等)、视频选择(--playlist-items、--match-filter 等)、下载选项(--concurrent-fragments、--limit-rate、--resume-download 等)、文件系统选项(-o 输出模板、-a 批量文件、--restrict-filenames 等)、缩略图选项、网络快捷方式选项、详细输出与模拟选项(--simulate、--print 等)、变通方案(--sleep-requests、--cookies-from-browser 等)、视频格式选项、字幕选项(--write-subs、--sub-langs、--convert-subs 等)、认证选项、后处理选项(--extract-audio、--embed-thumbnail、--embed-metadata、--remux-video 等)、SponsorBlock 选项、提取器选项(--extractor-args)与预设别名(--preset-alias)。

## 配置

- 可把常用选项写入配置文件:Windows 为 `%APPDATA%\yt-dlp\config`,Linux/macOS 为 `~/.config/yt-dlp/config` 或 `/etc/yt-dlp.conf`;也可在程序同目录放置 `yt-dlp.conf`,或用 `--config-location` 指定,`--ignore-config` 可禁用。
- 配置文件支持 `#` 注释、`\` 续行,以及按平台的条件分节(如 `[windows]`、`[os=mac]`)。
- 支持 netrc 认证:`--netrc` 读取 `~/.netrc` 中的机器凭据。
- 程序会读取部分环境变量(如代理相关的 `HTTP_PROXY` 等),详见原版说明。

## 输出模板

用 `-o` 自定义输出文件名,例如:

```bash
yt-dlp -o "%(title)s [%(id)s].%(ext)s"
```

常用字段:`%(title)s`(标题)、`%(id)s`(ID)、`%(ext)s`(扩展名)、`%(uploader)s`(上传者)、`%(upload_date)s`(上传日期)、`%(playlist_index)s`(播放列表序号)、`%(autonumber)s`(自动编号)等;还支持算术运算、默认值(`%(字段|默认值)s`)等语法,详见原版 OUTPUT TEMPLATE 一节。

## 格式选择

用 `-f` 选择格式,例如:

```bash
# 最高画质且不超过 1080p
yt-dlp -f "bv*[height<=1080]+ba/b"

# 只要 mp4
yt-dlp -f "b[ext=mp4]"

# 交互式选择
yt-dlp -f -
```

支持按分辨率、扩展名、语言、体积、码率等过滤(如 `[height<=720]`、`[ext=mp4]`),用 `-S` 排序(如 `-S "res,ext"`)。默认策略是优先 bestvideo*+bestaudio 合并,无法合并时回退到 best。

## 插件与嵌入

- 插件(Plugins):可安装第三方提取器与后处理器,放在 `yt_dlp_plugins` 包目录或配置目录的 `plugins/` 下,详见原版 PLUGINS 一节。
- 嵌入(Embedding yt-dlp):Python 程序可直接调用其 API:

```python
import yt_dlp

with yt_dlp.YoutubeDL() as ydl:
    ydl.download(["视频URL"])
```

## 与 youtube-dl 的差异(代表性新特性)

- 支持更多网站,直播流(DASH/HLS)下载更完善
- 分片多线程下载(--concurrent-fragments),断点续传更健壮
- 更强的格式选择与排序语法,更多输出模板字段
- --cookies-from-browser 直接读取浏览器 cookies
- 丰富的后处理:嵌入元数据/封面、按章节切割、SponsorBlock 集成
- 浏览器指纹模拟(Impersonation)能力
- 插件系统与 --extractor-args 提取器参数
- 可切换的更新通道与 --update-to

完整差异(含默认行为差异与弃用选项)见原版 CHANGES FROM YOUTUBE-DL 一节。

## 参与贡献与文档

- 贡献指南见原项目 CONTRIBUTING.md:提交 Issue 前请先阅读模板与常见问题,开发者说明包含环境搭建与测试方法。
- 更多文档见官方 Wiki(FAQ、安装指南、提取器开发等):https://github.com/yt-dlp/yt-dlp/wiki
- 完整命令行帮助请运行 `yt-dlp --help`。

## 声明与联系

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

本文为 yt-dlp/yt-dlp 官方 README 的中文翻译,仅供学习交流;所有代码与内容版权归原项目作者所有,遵循其原始许可证(Unlicense;部分打包二进制因包含第三方组件适用 GPLv3+ 等许可)。

如果觉得有用,请给原项目点个 Star!⭐
