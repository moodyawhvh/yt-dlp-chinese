> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。

# 维护者

本文件列出 yt-dlp 的维护者及其主要贡献。更多细节见[更新日志](Changelog.md)。

你也可以查看 [yt-dlp 全体贡献者](CONTRIBUTORS)与 [youtube-dl 作者列表](https://github.com/ytdl-org/youtube-dl/blob/master/AUTHORS)。

## 核心维护者

核心维护者负责评审与合并贡献、发布版本,并把握项目的整体方向。

**你可以通过 `maintainers@yt-dlp.org` 联系核心维护者。** 该邮箱**不是**支持渠道。需要帮助或想报告缺陷,请[提交 Issue](https://github.com/yt-dlp/yt-dlp/issues/new/choose)。

### [coletdjnz](https://github.com/coletdjnz)

[![gh-sponsor](https://img.shields.io/badge/_-Github-white.svg?logo=github&labelColor=555555&style=for-the-badge)](https://github.com/sponsors/coletdjnz)

* 全面重构网络栈,实现 `requests` 与 `curl_cffi`(`--impersonate`)HTTP 客户端支持
* 重构插件架构,使插件可在所有 yt-dlp 发行形式(exe、pip 等)中安装
* 实现外部 JavaScript 运行时/引擎支持
* 维护 YouTube 支持
* 新增并修复多个其他站点的支持

### [bashonly](https://github.com/bashonly)

* 重写并维护构建/发布流水线与自更新器:可执行文件、自动/nightly/master 发布、`--update-to`
* 全面改造外部下载器的 cookie 处理
* 协助实现外部 JavaScript 运行时/引擎支持
* 新增 `--cookies-from-browser` 对 Firefox 容器的支持
* 维护 YouTube、Vimeo、Twitter、TikTok 等站点支持
* 新增多个站点支持


### [Grub4K](https://github.com/Grub4K)

[![gh-sponsor](https://img.shields.io/badge/_-Github-white.svg?logo=github&labelColor=555555&style=for-the-badge)](https://github.com/sponsors/Grub4K) [![ko-fi](https://img.shields.io/badge/_-Ko--fi-red.svg?logo=kofi&labelColor=555555&style=for-the-badge)](https://ko-fi.com/Grub4K)

* `--update-to`、自更新器重写、自动/nightly/master 发布
* 重构 `traverse_obj` 等内部实现,大量核心重构与缺陷修复
* 为并行下载实现完善的进度报告
* 实现外部 JavaScript 运行时/引擎支持
* 改进/修复/新增 Bundestag、crunchyroll、pr0gramm、Twitter、WrestleUniverse 等


## 已离任的核心维护者

### [pukkandan](https://github.com/pukkandan)

[![ko-fi](https://img.shields.io/badge/_-Ko--fi-red.svg?logo=kofi&labelColor=555555&style=for-the-badge)](https://ko-fi.com/pukkandan)
[![gh-sponsor](https://img.shields.io/badge/_-Github-white.svg?logo=github&labelColor=555555&style=for-the-badge)](https://github.com/sponsors/pukkandan)

* 本分叉(fork)创始人
* 2021-2024 年首席维护者


### [shirt](https://github.com/shirt-dev)

[![ko-fi](https://img.shields.io/badge/_-Ko--fi-red.svg?logo=kofi&labelColor=555555&style=for-the-badge)](https://ko-fi.com/shirt)

* 分片下载的多线程(`-N`)与 aria2c 支持
* HLS 媒体初始化段(media initialization)与不连续点(discontinuity)支持
* 自更新器(`-U`)


### [Ashish0804](https://github.com/Ashish0804)

[![ko-fi](https://img.shields.io/badge/_-Ko--fi-red.svg?logo=kofi&labelColor=555555&style=for-the-badge)](https://ko-fi.com/ashish0804)

* 新增 BiliIntl、DiscoveryPlusIndia、OlympicsReplay、PlanetMarathi、ShemarooMe、Utreon、Zee5 等站点支持
* 为 Hotstar、ParamountPlus、Rumble、SonyLIV、Trovo、TubiTv、Voot 等添加播放列表/系列下载
* 改进/修复 HiDive、HotStar、Hungama、LBRY、LinkedInLearning、Mxplayer、SonyLiv、TV2、Vimeo、VLive 等


### [sepro](https://github.com/seproDev)

* 体验改进:缺少 ffmpeg 时告警、双击 exe 时提示
* 协助实现外部 JavaScript 运行时/引擎支持
* 代码清理:移除失效提取器、标记损坏提取器、启用/应用 ruff 规则
* 改进/修复/新增 ArdMediathek、DRTV、Floatplane、MagentaMusik、Naver、Nebula、OnDemandKorea、Vbox7 等


## 维护者

维护者是项目代码库的管家,可评审并合并 Pull Request。

- [doe1080](https://github.com/doe1080)


## 分诊维护者

分诊维护者是高频贡献者,可管理 Issue 与 Pull Request。

- [gamer191](https://github.com/gamer191)
- [garret1317](https://github.com/garret1317)
- [pzhlkj6612](https://github.com/pzhlkj6612)
- [DTrombett](https://github.com/dtrombett)
- [grqz](https://github.com/grqz)
- [InvalidUsernameException](https://github.com/InvalidUsernameException)
