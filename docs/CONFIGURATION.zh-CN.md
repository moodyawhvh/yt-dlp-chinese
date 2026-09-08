> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。
>
> 📖 本文对应原版 README 的 CONFIGURATION 章节(配置文件)。

# 配置(CONFIGURATION)

你可以把任何受支持的命令行选项写进配置文件来配置 yt-dlp。配置按以下位置依次加载:

1. **主配置(Main Configuration)**:
    * 通过 `--config-locations` 指定的文件
1. **便携配置(Portable Configuration)**:(便携安装推荐)
    * 使用二进制时:与二进制同目录的 `yt-dlp.conf`
    * 从源码运行时:`yt_dlp` 上级目录中的 `yt-dlp.conf`
1. **主目录配置(Home Configuration)**:
    * `-P` 指定的主目录中的 `yt-dlp.conf`
    * 未指定 `-P` 时,搜索当前目录
1. **用户配置(User Configuration)**:
    * `${XDG_CONFIG_HOME}/yt-dlp.conf`
    * `${XDG_CONFIG_HOME}/yt-dlp/config`(Linux/macOS 推荐)
    * `${XDG_CONFIG_HOME}/yt-dlp/config.txt`
    * `${APPDATA}/yt-dlp.conf`
    * `${APPDATA}/yt-dlp/config`(Windows 推荐)
    * `${APPDATA}/yt-dlp/config.txt`
    * `~/yt-dlp.conf`
    * `~/yt-dlp.conf.txt`
    * `~/.yt-dlp/config`
    * `~/.yt-dlp/config.txt`

    另见:[环境变量说明](#环境变量说明)
1. **系统配置(System Configuration)**:
    * `/etc/yt-dlp.conf`
    * `/etc/yt-dlp/config`
    * `/etc/yt-dlp/config.txt`

例如,使用下面这份配置文件后,yt-dlp 会总是提取音频、保留 mtime、走指定代理,并把所有视频保存到主目录的 `YouTube` 文件夹:
```
# 以 # 开头的行是注释

# 总是提取音频
-x

# 保留 mtime
--mtime

# 使用此代理
--proxy 127.0.0.1:3128

# 所有视频保存到主目录的 YouTube 文件夹
-o ~/YouTube/%(title)s.%(ext)s
```

**注意**:配置文件中的选项与普通命令行完全相同;因此 `-` 或 `--` 之后**不能有空格**,应写 `-o`、`--proxy` 而不是 `- o`、`-- proxy`。必要时还要像 UNIX shell 一样加引号。

想让某次运行完全无视所有配置文件,可用 `--ignore-config`。如果 `--ignore-config` 出现在某个配置文件内部,则不再加载后续配置。例如把它写进便携配置,主目录、用户与系统配置都不会再加载。此外,(为向后兼容)如果系统配置文件里出现 `--ignore-config`,用户配置也不会加载。

### 配置文件编码

配置文件若带 UTF BOM 则按 BOM 解码,否则按系统区域设置编码解码。

想用其他编码解码,可在文件开头加 `# coding: ENCODING`(如 `# coding: shift-jis`)。该行之前不能有任何字符,连空格或 BOM 都不行。

### 用 netrc 认证

对支持认证的提取器(需要 `--username` 与 `--password` 提供登录信息的),可以配置自动凭据存储,免得每次运行都把凭据放命令行、把明文密码留在 shell 历史里。做法是按提取器逐一在 [`.netrc` 文件](https://stackoverflow.com/tags/.netrc/info)中登记:在 `--netrc-location` 指定的位置创建 `.netrc`,并收紧权限为仅本人可读写:
```
touch ${HOME}/.netrc
chmod a-rwx,u+rw ${HOME}/.netrc
```
之后按以下格式为提取器添加凭据,其中 *extractor* 为提取器名称的小写:
```
machine <extractor> login <username> password <password>
```
例如:
```
machine youtube login myaccount@gmail.com password my_youtube_password
machine twitch login my_twitch_account_name password my_twitch_password
```
要启用 `.netrc` 认证,传 `--netrc` 参数或写进[配置文件](#配置configuration)即可。

`.netrc` 文件的默认位置是 `~`(见下文)。

`.netrc` 的缺点是密码以明文保存;替代方案是配置自定义 shell 命令来提供凭据,使用 `--netrc-cmd` 参数:该命令需以 netrc 格式输出凭据、成功时返回 `0`(其他返回值视为错误)。命令中的 `{}` 会被替换为提取器名称,便于为不同提取器选取对应凭据。

例如使用以 `.authinfo.gpg` 保存的加密 `.netrc`:
```
yt-dlp --netrc-cmd 'gpg --decrypt ~/.authinfo.gpg' 'https://www.youtube.com/watch?v=YE7VzlLtp-4'
```


### 环境变量说明
* 环境变量在 UNIX 上通常写作 `${VARIABLE}`/`$VARIABLE`,Windows 上为 `%VARIABLE%`;本文档中一律以 `${VARIABLE}` 表示
* yt-dlp 在 Windows 上也允许路径类选项使用 UNIX 风格变量,如 `--output`、`--config-locations`
* `${XDG_CONFIG_HOME}` 未设置时默认为 `~/.config`,`${XDG_CACHE_HOME}` 默认为 `~/.cache`
* Windows 上,`~` 优先指向 `${HOME}`;否则指向 `${USERPROFILE}` 或 `${HOMEDRIVE}${HOMEPATH}`
* Windows 上,`${USERPROFILE}` 一般是 `C:\Users\<用户名>`,`${APPDATA}` 是 `${USERPROFILE}\AppData\Roaming`
