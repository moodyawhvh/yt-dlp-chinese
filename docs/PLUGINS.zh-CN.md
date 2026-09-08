> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。
>
> 📖 本文对应原版 README 的 PLUGINS 章节(插件系统)。

# 插件(PLUGINS)

注意:**所有**插件都会被导入,即使从未被调用,而且**插件代码不会经过任何检查**。**请只在你信任代码的前提下自行承担风险使用插件!**

插件的 `<type>` 可以是 `extractor`(提取器)或 `postprocessor`(后处理器)。
- 提取器插件无需在命令行启用,输入 URL 匹配时自动调用。
- 提取器插件优先于内置提取器。
- 后处理器插件通过 `--use-postprocessor NAME` 调用。


插件从命名空间包 `yt_dlp_plugins.extractor` 与 `yt_dlp_plugins.postprocessor` 加载。

换言之,磁盘上的文件结构形如:

        yt_dlp_plugins/
            extractor/
                myplugin.py
            postprocessor/
                myplugin.py

yt-dlp 会在很多位置(见下)查找 `yt_dlp_plugins` 命名空间文件夹,并从**全部**位置加载插件。
将环境变量 `YTDLP_NO_PLUGINS` 设为非空值可完全禁用插件加载。

已知插件见[官方 Wiki](https://github.com/yt-dlp/yt-dlp/wiki/Plugins)。

## 安装插件

插件可通过多种方式与位置安装。

1. **配置目录**:
   插件包(内含 `yt_dlp_plugins` 命名空间文件夹)可直接放入以下标准[配置位置](%E9%85%8D%E7%BD%AEconfiguration):
    * **用户插件**
      * `${XDG_CONFIG_HOME}/yt-dlp/plugins/<包名>/yt_dlp_plugins/`(Linux/macOS 推荐)
      * `${XDG_CONFIG_HOME}/yt-dlp-plugins/<包名>/yt_dlp_plugins/`
      * `${APPDATA}/yt-dlp/plugins/<包名>/yt_dlp_plugins/`(Windows 推荐)
      * `${APPDATA}/yt-dlp-plugins/<包名>/yt_dlp_plugins/`
      * `~/.yt-dlp/plugins/<包名>/yt_dlp_plugins/`
      * `~/yt-dlp-plugins/<包名>/yt_dlp_plugins/`
    * **系统插件**
      * `/etc/yt-dlp/plugins/<包名>/yt_dlp_plugins/`
      * `/etc/yt-dlp-plugins/<包名>/yt_dlp_plugins/`
2. **可执行文件所在目录**:插件包同样可以安装到可执行文件位置的 `yt-dlp-plugins` 目录(便携安装推荐):
    * 二进制:`<根目录>/yt-dlp.exe` 旁,即 `<根目录>/yt-dlp-plugins/<包名>/yt_dlp_plugins/`
    * 源码:`<根目录>/yt_dlp/__main__.py` 旁,即 `<根目录>/yt-dlp-plugins/<包名>/yt_dlp_plugins/`

3. **pip 及 `PYTHONPATH` 中的其他位置**
    * 插件包可用 `pip` 安装管理,示例见 [yt-dlp-sample-plugins](https://github.com/yt-dlp/yt-dlp-sample-plugins)。
      * 注意:用 pip 安装的多个插件包之间,插件文件名必须唯一。
    * `PYTHONPATH` 中的任何路径都会被搜索 `yt_dlp_plugins` 命名空间文件夹。
      * 注意:PyInstaller 构建不适用此方式。


根目录含 `yt_dlp_plugins` 命名空间文件夹的 `.zip`、`.egg`、`.whl` 压缩包同样可作为插件包。

* 例如 `${XDG_CONFIG_HOME}/yt-dlp/plugins/mypluginpkg.zip`,其中 `mypluginpkg.zip` 内含 `yt_dlp_plugins/<type>/myplugin.py`

用 `--verbose` 运行 yt-dlp 可确认插件是否已加载。

## 开发插件

模板插件包见 [yt-dlp-sample-plugins](https://github.com/yt-dlp/yt-dlp-sample-plugins) 仓库;插件开发指南见 Wiki 的 [Plugin Development](https://github.com/yt-dlp/yt-dlp/wiki/Plugin-Development) 章节。

每个文件中所有名称以 `IE`/`PP` 结尾的公开类,会分别作为提取器/后处理器导入。下划线前缀视为私有(如 `_MyBasePluginIE` 不导入),`__all__` 亦受支持。模块名加下划线前缀同样可排除(如 `_myplugin.py`)。

想用一个子类替换现有提取器,设置 `plugin_name` 类关键字参数即可(如 `class MyPluginIE(ABuiltInIE, plugin_name='myplugin')` 会用 `MyPluginIE` 替换 `ABuiltInIE`)。由于该提取器替换了父类,应按上述方法把子类提取器设为私有,避免被重复导入。

如果你是插件作者,请给仓库添加 [yt-dlp-plugins](https://github.com/topics/yt-dlp-plugins) 主题以便被发现。

如何编写与测试提取器见[开发者指南](https://github.com/yt-dlp/yt-dlp/blob/master/CONTRIBUTING.md#developer-instructions)。
