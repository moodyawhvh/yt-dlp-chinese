<div align="center">

# yt-dlp 中文翻译版

**[中文版] yt-dlp — 功能丰富的命令行音视频下载工具**

[![原项目](https://img.shields.io/badge/原项目-yt-dlp--yt-dlp-blue?style=flat-square&logo=github)](https://github.com/yt-dlp/yt-dlp)
[![中文文档](https://img.shields.io/badge/中文文档-README.zh--CN.md-orange?style=flat-square)](README.zh-CN.md)
[![GitHub Stars](https://img.shields.io/github/stars/yt-dlp/yt-dlp?style=flat-square&label=原项目Stars)](https://github.com/yt-dlp/yt-dlp/stargazers)
[![微信联系](https://img.shields.io/badge/微信-uaycar-brightgreen?style=flat-square&logo=wechat)](#)

</div>

---

> 这是 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 的中文翻译版本。
> 完整源代码请访问原项目:https://github.com/yt-dlp/yt-dlp

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

## 📖 项目简介

yt-dlp 是一个功能丰富的命令行音视频下载工具,支持数千个网站。它是 youtube-dl 的社区分支,基于已停更的 youtube-dlc 继续开发,以更新快、特性多、格式处理能力强著称,是当前最主流的开源命令行下载器之一。本仓库提供其官方 README 的中文翻译版本,方便中文用户快速了解与上手。

## ✨ 主要特性

- 支持数千个网站的音视频下载(支持站点列表见原项目 supportedsites.md)
- 相比 youtube-dl 新增大量特性:更强的格式选择语法、更丰富的后处理能力
- 跨平台运行:Windows、Linux、macOS 均有官方发布的独立可执行文件,也支持 pip 安装
- 三大更新通道 stable / nightly / master,支持 `yt-dlp -U` 与 `--update-to` 灵活升级
- 强大的输出模板(OUTPUT TEMPLATE)系统,可自定义文件名与目录结构
- 灵活的格式选择(FORMAT SELECTION):按分辨率、编码、语言、体积过滤与排序
- 丰富的后处理:提取音频、嵌入元数据与封面、字幕下载转换、SponsorBlock 集成
- 插件系统(Plugins)与 Python API 嵌入(Embedding),方便二次开发
- 浏览器指纹模拟(Impersonation,基于 curl_cffi),应对 TLS 指纹检测
- 支持浏览器 cookies 导入、代理、限速、多线程分片下载、断点续传等

## 📁 文件说明

| 文件 | 说明 |
|:-----|:-----|
| README.md | 本文件(中文简介) |
| README.zh-CN.md | 详细中文文档(完整汉化) |

## 🚀 快速开始

1. 通过 pip 安装(推荐):
```bash
python -m pip install -U "yt-dlp[default]"
```
2. Windows 也可直接下载最新版 yt-dlp.exe:
https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.exe
(macOS 对应 yt-dlp_macos,Linux 对应 yt-dlp,见原项目 Releases 页)
3. 更新到最新版:
```bash
yt-dlp -U
```
4. 切换更新通道(如官方推荐的 nightly):
```bash
yt-dlp --update-to nightly
```
5. 基本下载:
```bash
yt-dlp <视频URL>
```
6. 自定义输出文件名模板:
```bash
yt-dlp -o "%(title)s [%(id)s].%(ext)s" <视频URL>
```
7. 强烈建议安装 ffmpeg / ffprobe(合并音视频与后处理必需);完整支持 YouTube 还需 yt-dlp-ejs 与 JavaScript 运行时(如 deno)。
8. 查看全部选项:
```bash
yt-dlp --help
```

完整源代码与最新版本请访问原项目:https://github.com/yt-dlp/yt-dlp

## 📞 联系方式

**代部署 / 定制服务 / 技术咨询 请添加微信:uaycar**

---

本项目为 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 的中文翻译版本,所有代码版权归原项目作者所有,遵循其原始许可证。

**如果觉得有用,请给原项目点个 Star!** ⭐
