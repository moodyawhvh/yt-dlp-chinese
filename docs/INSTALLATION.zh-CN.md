> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。
>
> 📖 本文对应原版 README 的 INSTALLATION 章节(安装指南)。

# 安装

你可以通过[官方二进制文件](#release-files)、[pip](https://pypi.org/project/yt-dlp) 或第三方包管理器安装 yt-dlp。详细安装说明见[官方 Wiki](https://github.com/yt-dlp/yt-dlp/wiki/Installation)。


## 发布文件(RELEASE FILES)

#### 推荐

文件|说明
:---|:---
[yt-dlp](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp)|平台无关的 [zipimport](https://docs.python.org/3/library/zipimport.html) 二进制,需要 Python(**Linux/BSD 推荐**)
[yt-dlp.exe](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.exe)|Windows(Win8+)独立 x64 二进制(**Windows 推荐**)
[yt-dlp_macos](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_macos)|通用 MacOS(10.15+)独立可执行文件(**MacOS 推荐**)

#### 备选

文件|说明
:---|:---
[yt-dlp_linux](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_linux)|Linux(glibc 2.17+)独立 x86_64 二进制
[yt-dlp_linux.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_linux.zip)|免打包 Linux(glibc 2.17+)x86_64 可执行文件(不支持自动更新)
[yt-dlp_linux_aarch64](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_linux_aarch64)|Linux(glibc 2.17+)独立 aarch64 二进制
[yt-dlp_linux_aarch64.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_linux_aarch64.zip)|免打包 Linux(glibc 2.17+)aarch64 可执行文件(不支持自动更新)
[yt-dlp_linux_armv7l.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_linux_armv7l.zip)|免打包 Linux(glibc 2.31+)armv7l 可执行文件(不支持自动更新)
[yt-dlp_musllinux](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_musllinux)|Linux(musl 1.2+)独立 x86_64 二进制
[yt-dlp_musllinux.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_musllinux.zip)|免打包 Linux(musl 1.2+)x86_64 可执行文件(不支持自动更新)
[yt-dlp_musllinux_aarch64](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_musllinux_aarch64)|Linux(musl 1.2+)独立 aarch64 二进制
[yt-dlp_musllinux_aarch64.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_musllinux_aarch64.zip)|免打包 Linux(musl 1.2+)aarch64 可执行文件(不支持自动更新)
[yt-dlp_x86.exe](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_x86.exe)|Windows(Win8+)独立 x86(32 位)二进制
[yt-dlp_win_x86.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_win_x86.zip)|免打包 Windows(Win8+)x86(32 位)可执行文件(不支持自动更新)
[yt-dlp_arm64.exe](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_arm64.exe)|Windows(Win10+)独立 ARM64 二进制
[yt-dlp_win_arm64.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_win_arm64.zip)|免打包 Windows(Win10+)ARM64 可执行文件(不支持自动更新)
[yt-dlp_win.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_win.zip)|免打包 Windows(Win8+)x64 可执行文件(不支持自动更新)
[yt-dlp_macos.zip](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp_macos.zip)|免打包 MacOS(10.15+)可执行文件(不支持自动更新)

#### 其他

文件|说明
:---|:---
[yt-dlp.tar.gz](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.tar.gz)|源码压缩包
[SHA2-512SUMS](https://github.com/yt-dlp/yt-dlp/releases/latest/download/SHA2-512SUMS)|GNU 风格 SHA512 校验和
[SHA2-512SUMS.sig](https://github.com/yt-dlp/yt-dlp/releases/latest/download/SHA2-512SUMS.sig)|SHA512 校验和的 GPG 签名
[SHA2-256SUMS](https://github.com/yt-dlp/yt-dlp/releases/latest/download/SHA2-256SUMS)|GNU 风格 SHA256 校验和
[SHA2-256SUMS.sig](https://github.com/yt-dlp/yt-dlp/releases/latest/download/SHA2-256SUMS.sig)|SHA256 校验和的 GPG 签名

验证 GPG 签名所用的公钥见 [public.key](https://github.com/yt-dlp/yt-dlp/blob/master/public.key),示例:

```
curl -L https://github.com/yt-dlp/yt-dlp/raw/master/public.key | gpg --import
gpg --verify SHA2-256SUMS.sig SHA2-256SUMS
gpg --verify SHA2-512SUMS.sig SHA2-512SUMS
```

#### 许可说明

yt-dlp 本体以 [Unlicense](../LICENSE) 授权,但许多发布文件包含其他项目的代码,各自遵循不同许可。

最值得注意的是:PyInstaller 打包的可执行文件包含 GPLv3+ 授权的代码,因此合并作品整体以 [GPLv3+](https://www.gnu.org/licenses/gpl-3.0.html) 授权。

zipimport 版 Unix 可执行文件(`yt-dlp`)与源码包(`yt-dlp.tar.gz`)包含 [`meriyah`](https://github.com/meriyah/meriyah) 的 [ISC](https://github.com/meriyah/meriyah/blob/main/LICENSE.md) 授权代码与 [`astring`](https://github.com/davidbonnet/astring) 的 [MIT](https://github.com/davidbonnet/astring/blob/main/LICENSE) 授权代码。

详见 [THIRD_PARTY_LICENSES.txt](../THIRD_PARTY_LICENSES.txt)。

git 仓库、PyPI 源码包与 PyPI 构建包(wheel)只包含 [Unlicense](../LICENSE) 授权的代码。

**注意**:手册页(manpage)、shell 补全文件等在[源码压缩包](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp.tar.gz)内提供。


## 更新(UPDATE)

如果你使用的是[官方发布二进制](#release-files),可用 `yt-dlp -U` 自更新。

如果[用 pip 安装](https://github.com/yt-dlp/yt-dlp/wiki/Installation#with-pip),重新执行当初的安装命令即可。

其他第三方包管理器请见[官方 Wiki](https://github.com/yt-dlp/yt-dlp/wiki/Installation#third-party-package-managers)或其自身文档。

<a id="update-channels"></a>

二进制发布目前有三个通道:`stable`、`nightly` 和 `master`。

* `stable` 是默认通道,按(大致)每月一次的节奏发布。虽然其大部分改动已经过 `nightly` 或 `master` 通道用户的检验,但最新的 `stable` 发布往往已经"过时",更容易因外部变化失效(即网站自行改动导致 yt-dlp 失效)。
* `nightly` 通道在代码库发生变更的每一天、UTC 午夜前不久发布,相当于项目开发进度的快照,**推荐普通用户使用**。可从 [yt-dlp/yt-dlp-nightly-builds](https://github.com/yt-dlp/yt-dlp-nightly-builds/releases) 获取,或安装 `yt-dlp` PyPI 包的开发版(pip 加 `--pre` 参数)。
* `master` 通道在每次向 master 分支推送后发布"金丝雀"版本,始终包含最新的修复与特性,但可能有缺陷或回归。可从 [yt-dlp/yt-dlp-master-builds](https://github.com/yt-dlp/yt-dlp-master-builds/releases) 获取。

使用 `--update`/`-U` 时,发布二进制只会在当前通道内更新。
`--update-to CHANNEL` 可在出现更新版本时切换到其他通道;`--update-to [CHANNEL@]TAG` 还可以跨通道升降级到指定标签。

你也可以用 `--update-to <repository>`(`<owner>/<repository>` 形式)更新到完全不同仓库的通道。但请谨慎对待更新来源——不同仓库的二进制不会经过任何校验。

示例:

* `yt-dlp --update-to master` 切换到 `master` 通道并更新到其最新发布
* `yt-dlp --update-to stable@2023.07.06` 升级/降级到 `stable` 通道的 `2023.07.06` 标签
* `yt-dlp --update-to 2023.10.07` 升级/降级到当前通道上的 `2023.10.07` 标签(如存在)
* `yt-dlp --update-to example/yt-dlp@2023.09.24` 升级/降级到 `example/yt-dlp` 仓库的 `2023.09.24` 发布

**重要**:遇到 `stable` 版问题,提交缺陷报告前请先安装或更新到 `nightly` 版:
```
# 从 stable 可执行/二进制更新到 nightly:
yt-dlp --update-to nightly

# 用 pip 安装 nightly:
python -m pip install -U --pre "yt-dlp[default]"
```

当运行的 yt-dlp 版本超过 90 天未更新时,会出现建议升级的警告。可在命令或配置文件中加 `--no-update` 抑制该警告。


## 依赖(DEPENDENCIES)

支持 Python 3.10+(CPython)与 3.11+(PyPy);其他版本与实现不保证可用。

所有依赖均为可选,但强烈建议安装 `ffmpeg`、`ffprobe`、`yt-dlp-ejs` 及一个受支持的 JavaScript 运行时/引擎。

### 强烈推荐

* [**ffmpeg** 与 **ffprobe**](https://www.ffmpeg.org) - [合并独立音视频流](#format-selection)以及各类[后处理](#post-processing-options)任务必需。许可[取决于具体构建](https://www.ffmpeg.org/legal.html)。

    ffmpeg 十分关键,官方在 [yt-dlp/FFmpeg-Builds](https://github.com/yt-dlp/FFmpeg-Builds) 提供自有构建。过去这些构建曾带补丁以解决 yt-dlp 用户的常见问题,目前与上游 ffmpeg 等价。详见[其 readme](https://github.com/yt-dlp/FFmpeg-Builds#patches-applied)。

    **重要**:你需要的是 ffmpeg *可执行程序*,**而不是** [PyPI 上同名的 Python 包](https://pypi.org/project/ffmpeg)。

* [**yt-dlp-ejs**](https://github.com/yt-dlp/ejs) - 完整支持 YouTube 所需。以 [Unlicense](https://github.com/yt-dlp/ejs/blob/main/LICENSE) 授权,捆绑 [MIT](https://github.com/davidbonnet/astring/blob/main/LICENSE) 与 [ISC](https://github.com/meriyah/meriyah/blob/main/LICENSE.md) 组件。

    运行 yt-dlp-ejs 还需要一个 JavaScript 运行时/引擎,如 [**deno**](https://deno.land)(推荐)、[**node.js**](https://nodejs.org)、[**bun**](https://bun.sh) 或 [**QuickJS**](https://bellard.org/quickjs/)。见 [Wiki:EJS](https://github.com/yt-dlp/yt-dlp/wiki/EJS)。

### 网络

* [**certifi**](https://github.com/certifi/python-certifi)\* - 提供 Mozilla 根证书包。[MPLv2](https://github.com/certifi/python-certifi/blob/master/LICENSE)
* [**brotli**](https://github.com/google/brotli)\* 或 [**brotlicffi**](https://github.com/python-hyper/brotlicffi) - [Brotli](https://en.wikipedia.org/wiki/Brotli) 内容编码支持,均为 MIT <sup>[1](https://github.com/google/brotli/blob/master/LICENSE) [2](https://github.com/python-hyper/brotlicffi/blob/master/LICENSE)</sup>
* [**websockets**](https://github.com/aaugustin/websockets)\* - WebSocket 下载支持。[BSD-3-Clause](https://github.com/aaugustin/websockets/blob/main/LICENSE)
* [**requests**](https://github.com/psf/requests)\* - HTTP 库,提供 HTTPS 代理与持久连接支持。[Apache-2.0](https://github.com/psf/requests/blob/main/LICENSE)

#### 浏览器指纹模拟(Impersonation)

以下依赖用于模拟浏览器请求特征。部分使用 TLS 指纹检测的网站可能需要。

* [**curl_cffi**](https://github.com/lexiforest/curl_cffi)(推荐)- [curl-impersonate](https://github.com/lexiforest/curl-impersonate) 的 Python 绑定,提供 Chrome、Edge、Safari 模拟目标。[MIT](https://github.com/lexiforest/curl_cffi/blob/main/LICENSE)
  * 可通过 `curl-cffi` extra 安装,如 `pip install "yt-dlp[default,curl-cffi]"`
  * 目前多数构建已内置,*除了* `yt-dlp`(Unix zipimport 二进制)与 `yt-dlp_x86`(Windows 32 位)


### 元数据

* [**mutagen**](https://github.com/quodlibet/mutagen)\* - 特定格式下的 `--embed-thumbnail`。[GPLv2+](https://github.com/quodlibet/mutagen/blob/master/COPYING)
* [**AtomicParsley**](https://github.com/wez/atomicparsley) - 当 `mutagen`/`ffmpeg` 不可用时,对 `mp4`/`m4a` 做 `--embed-thumbnail`。[GPLv2+](https://github.com/wez/atomicparsley/blob/master/COPYING)
* [**xattr**](https://github.com/xattr/xattr)、[**pyxattr**](https://github.com/iustin/pyxattr) 或 [**setfattr**](http://savannah.nongnu.org/projects/attr) - 在 **Mac** 与 **BSD** 上写 xattr 元数据(`--xattrs`),许可分别为 [MIT](https://github.com/xattr/xattr/blob/master/LICENSE.txt)、[LGPL2.1](https://github.com/iustin/pyxattr/blob/master/COPYING)、[GPLv2+](http://git.savannah.nongnu.org/cgit/attr.git/tree/doc/COPYING)

### 其他

* [**pycryptodomex**](https://github.com/Legrandin/pycryptodome)\* - 解密 AES-128 HLS 流及各类其他数据。[BSD-2-Clause](https://github.com/Legrandin/pycryptodome/blob/master/LICENSE.rst)
* [**phantomjs**](https://github.com/ariya/phantomjs) - 部分提取器运行 JavaScript 用(YouTube 已不再使用),近期将被弃用。[BSD-3-Clause](https://github.com/ariya/phantomjs/blob/master/LICENSE.BSD)
* [**secretstorage**](https://github.com/mitya57/secretstorage)\* - **Linux** 上 `--cookies-from-browser` 读取 **Gnome** 密钥环、解密 **Chromium** 系浏览器 cookie 时使用。[BSD-3-Clause](https://github.com/mitya57/secretstorage/blob/master/LICENSE)
* 任何想配合 `--downloader` 使用的外部下载器

### 已弃用

* [**rtmpdump**](http://rtmpdump.mplayerhq.hu) - 下载 `rtmp` 流;可用 `--downloader ffmpeg` 改用 ffmpeg。[GPLv2+](http://rtmpdump.mplayerhq.hu)

使用或再分发上述依赖,须同意其各自的许可条款。

独立发布二进制已内置 Python 解释器及标注 **\*** 的依赖包。

缺少任务所需依赖时,yt-dlp 会给出警告;当前可用的依赖都显示在 `--verbose` 输出的开头。


## 自行构建(COMPILE)

### 独立 PyInstaller 构建

构建独立可执行文件需要 Python 与 `pyinstaller`(按需加上 yt-dlp 的[可选依赖](#dependencies));产物架构与所用 Python 的 CPU 架构一致。

运行以下命令:

```
python devscripts/install_deps.py --include-group pyinstaller
python devscripts/make_lazy_extractors.py
python -m bundle.pyinstaller
```

部分系统上可能需要用 `py` 或 `python3` 代替 `python`。

`python -m bundle.pyinstaller` 接受所有可传给 `pyinstaller` 的参数,例如 `--onefile/-F`、`--onedir/-D`,[详见其文档](https://pyinstaller.org/en/stable/usage.html#what-to-generate)。

**注意**:低于 4.4 的 PyInstaller 版本[不支持](https://github.com/pyinstaller/pyinstaller#requirements-and-tested-platforms) Windows 商店版 Python(除非使用虚拟环境)。

**重要**:官方不支持绕过 `python -m bundle.pyinstaller` 直接运行 `pyinstaller`,不一定能正常工作。

### 平台无关二进制(UNIX)

需要构建工具:`python`(3.10+)、`zip`、`make`(GNU)、`pandoc`\* 与 `pytest`\*。

装好后直接运行 `make`。

也可以运行 `make yt-dlp` 只编译二进制、不更新其他附加文件(带 **\*** 的工具此时不需要)。

### 相关脚本

* **`devscripts/install_deps.py`** - 安装 yt-dlp 依赖。
* **`devscripts/update-version.py`** - 按当前日期更新版本号。
* **`devscripts/set-variant.py`** - 设置可执行文件的构建变体。
* **`devscripts/make_changelog.py`** - 用简短提交信息生成 markdown 更新日志并更新 `CONTRIBUTORS`。
* **`devscripts/make_lazy_extractors.py`** - 生成懒加载提取器;构建任何变体的二进制前执行可提升启动速度。设置非空环境变量 `YTDLP_NO_LAZY_EXTRACTORS` 可强制禁用懒加载。

注意:更多信息见各脚本 `--help`。

### 分叉本项目

在 GitHub 上 fork 后,可以运行 fork 的[构建工作流](../.github/workflows/build.yml)自动把选定版本构建为 artifact;也可以运行[发布工作流](../.github/workflows/release.yml)或启用 [nightly 工作流](../.github/workflows/release-nightly.yml)来创建完整的(预)发布。
