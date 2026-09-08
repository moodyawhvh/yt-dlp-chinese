> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。
>
> 📖 本文对应原版 README 的 OUTPUT TEMPLATE 章节(输出模板)。

# 输出模板(OUTPUT TEMPLATE)

`-o` 选项用于指定输出文件名的模板,`-P` 选项用于指定各类文件的保存路径。

**tl;dr:** 直接[看示例](#输出模板示例)。

`-o` 最简单的用法是下载单个文件时不带任何模板参数,如 `yt-dlp -o funny_video.flv "https://some/video"`(像这样硬编码扩展名*不*推荐,可能破坏部分后处理)。

模板中也可以包含特殊序列,在下载每个视频时被替换。特殊序列可按 [Python 字符串格式化](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting)语法书写,如 `%(NAME)s`、`%(NAME)05d`,即百分号 + 括号中的字段名 + 格式化操作。

字段名本身(括号内部分)还支持以下特殊格式:

1. **对象遍历**:元数据中的字典与列表可用点 `.` 分隔符逐层访问,如 `%(tags.0)s`、`%(subtitles.en.-1.ext)s`;支持 Python 切片冒号 `:`,如 `%(id.3:7)s`、`%(id.6:2:-1)s`、`%(formats.:.format_id)s`;花括号 `{}` 可构建只含特定键的字典,如 `%(formats.:.{format_id,height})#j`;空字段名 `%()s` 表示整个 infodict,如 `%(.{id,title})s`。注意此方式可访问的字段不全在下文列表中,用 `-j` 查看
1. **算术运算**:数值字段可用 `+`、`-`、`*` 做简单运算,如 `%(playlist_index+10)03d`、`%(n_entries+1-playlist_index)d`
1. **日期/时间格式化**:日期时间字段可按 [strftime 格式](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes)格式化,用 `>` 与字段名分隔,如 `%(duration>%H-%M-%S)s`、`%(upload_date>%Y-%m-%d)s`、`%(epoch-3600>%H-%M-%S)s`
1. **备选字段**:多个备选字段用 `,` 分隔,如 `%(release_date>%Y,upload_date>%Y|Unknown)s`
1. **替换值**:用 `&` 分隔符按 [`str.format` 迷你语言](https://docs.python.org/3/library/string.html#format-specification-mini-language)指定替换值;字段*非空*时将用替换值代替字段实际内容。替换在备选字段之后处理,因此*任何*备选字段非空都会触发替换,如 `%(chapters&has chapters|no chapters)s`、`%(title&TITLE={:>20}|NO TITLE)s`
1. **默认值**:字段为空时的字面默认值,用 `|` 分隔符指定,会覆盖 `--output-na-placeholder`,如 `%(uploader|Unknown)s`
1. **更多转换类型**:除常规类型 `diouxXeEfFgGcrs` 外,yt-dlp 额外支持:`B` = 字**B**ytes,`j` = **j**son(`#` 美化输出、`+` 启用 Unicode),`h` = HTML 转义,`l` = 逗号分隔的**l**ist(`#` 改为 `\n` 换行分隔),`q` = 终端安全的**q**uoted 字符串(`#` 将列表拆成多个参数),`D` = 加十进制**D**后缀(如 10M)(`#` 用 1024 作因子),`S` = 文件名净化(**S**anitize)(`#` 更严格)
1. **Unicode 规范化**:类型 `U` 做 NFC [Unicode 规范化](https://docs.python.org/3/library/unicodedata.html#unicodedata.normalize);`#` 标志改为 NFD,`+` 转换标志做 NFKC/NFKD 兼容等价规范化,如 `%(title)+.100U` 为 NFKC

概括起来,字段的一般语法为:
```
%(name[.keys][addition][>strf][,alternate][&replacement][|default])[flags][width][.precision][length]type
```

此外,你可以为各类元数据文件单独设置输出模板(与总输出模板区分),写法是文件类型 + 冒号 `:` + 模板。支持的文件类型:`subtitle`、`thumbnail`、`description`、`annotation`(已弃用)、`infojson`、`link`、`pl_thumbnail`、`pl_description`、`pl_infojson`、`chapter`、`pl_video`。例如 `-o "%(title)s.%(ext)s" -o "thumbnail:%(title)s/%(title)s.%(ext)s"` 会把缩略图放进与视频同名的文件夹。某类模板为空则该类文件不写出,如 `--write-thumbnail -o "thumbnail:"` 只为播放列表写缩略图、不为单个视频写。

<a id="outtmpl-postprocess-note"></a>

**注意**:由于后处理(合并等),实际输出文件名可能不同。要获取全部后处理完成后的文件名,用 `--print after_move:filepath`。

可用字段如下:

 - `id`(字符串):视频标识符
 - `title`(字符串):视频标题
 - `fulltitle`(字符串):忽略直播时间戳与通用标题的完整视频标题
 - `ext`(字符串):视频文件扩展名
 - `alt_title`(字符串):视频的副标题
 - `description`(字符串):视频描述
 - `display_id`(字符串):视频的另一种标识符
 - `uploader`(字符串):上传者全名
 - `uploader_id`(字符串):上传者昵称或 id
 - `uploader_url`(字符串):上传者主页 URL
 - `license`(字符串):视频许可名称
 - `creators`(列表):视频创作者
 - `creator`(字符串):视频创作者,逗号分隔
 - `timestamp`(数值):视频可用的 UNIX 时间戳
 - `upload_date`(字符串):视频上传日期,UTC(YYYYMMDD)
 - `release_timestamp`(数值):视频发布的 UNIX 时间戳
 - `release_date`(字符串):视频发布日期,UTC(YYYYMMDD)
 - `release_year`(数值):视频或专辑发行年份(YYYY)
 - `modified_timestamp`(数值):视频最后修改的 UNIX 时间戳
 - `modified_date`(字符串):视频最后修改日期,UTC(YYYYMMDD)
 - `channel`(字符串):视频所在频道全名
 - `channel_id`(字符串):频道 id
 - `channel_url`(字符串):频道 URL
 - `channel_follower_count`(数值):频道关注者数
 - `channel_is_verified`(布尔):频道是否在平台认证
 - `location`(字符串):视频拍摄地
 - `duration`(数值):视频时长(秒)
 - `duration_string`(字符串):视频时长(HH:mm:ss)
 - `view_count`(数值):平台上的观看次数
 - `concurrent_view_count`(数值):当前正在观看的人数
 - `like_count`(数值):好评数
 - `dislike_count`(数值):差评数
 - `repost_count`(数值):转发数
 - `average_rating`(数值):用户平均评分,量纲取决于页面
 - `comment_count`(数值):评论数(部分提取器的评论在下载末尾才获取,该字段可能不可用)
 - `save_count`(数值):收藏/保存次数
 - `age_limit`(数值):年龄限制(岁)
 - `live_status`(字符串):`"not_live"`、`"is_live"`、`"is_upcoming"`、`"was_live"`、`"post_live"`(曾为直播,VOD 尚未处理完)之一
 - `is_live`(布尔):是直播还是固定时长视频
 - `was_live`(布尔):是否原为直播
 - `playable_in_embed`(字符串):是否允许在其他站点的内嵌播放器中播放
 - `availability`(字符串):`"private"`、`"premium_only"`、`"subscriber_only"`、`"needs_auth"`、`"unlisted"` 或 `"public"`
 - `media_type`(字符串):站点分类的媒体类型,如 `"episode"`、`"clip"`、`"trailer"`
 - `start_time`(数值):URL 指定的播放起始秒数
 - `end_time`(数值):URL 指定的播放结束秒数
 - `extractor`(字符串):提取器名称
 - `extractor_key`(字符串):提取器键名
 - `epoch`(数值):信息提取完成时的 Unix 时间戳
 - `autonumber`(数值):每次下载自增的编号,从 `--autonumber-start` 开始,前导零补足 5 位
 - `video_autonumber`(数值):每个视频自增的编号
 - `n_entries`(数值):播放列表中已提取条目总数
 - `playlist_id`(字符串):视频所在播放列表标识符
 - `playlist_title`(字符串):视频所在播放列表名称
 - `playlist`(字符串):有 `playlist_title` 用之,否则用 `playlist_id`
 - `playlist_count`(数值):播放列表条目总数;未提取完整列表时可能未知
 - `playlist_index`(数值):视频在播放列表中的序号,按最终序号补前导零
 - `playlist_autonumber`(数值):视频在播放列表下载队列中的位置,按列表总长补前导零
 - `playlist_uploader`(字符串):播放列表上传者全名
 - `playlist_uploader_id`(字符串):播放列表上传者昵称或 id
 - `playlist_channel`(字符串):上传该播放列表的频道显示名
 - `playlist_channel_id`(字符串):上传该播放列表的频道标识符
 - `playlist_webpage_url`(字符串):播放列表页面 URL
 - `webpage_url`(字符串):视频网页 URL;再次交给 yt-dlp 应得到相同结果
 - `webpage_url_basename`(字符串):网页 URL 的 basename
 - `webpage_url_domain`(字符串):网页 URL 的域名
 - `original_url`(字符串):用户输入的 URL(播放列表条目则同 `webpage_url`)
 - `categories`(列表):视频所属分类
 - `tags`(列表):视频标签
 - `cast`(列表):演员表

[过滤格式](#filtering-formats)中的所有字段同样可用。

属于某个逻辑章节/段落的视频可用:

 - `chapter`(字符串):视频所属章节名称
 - `chapter_number`(数值):视频所属章节编号
 - `chapter_id`(字符串):视频所属章节 id

某剧集(series)或节目某一集的视频可用:

 - `series`(字符串):该集所属剧集/节目名称
 - `series_id`(字符串):该集所属剧集/节目 id
 - `season`(字符串):该集所属季名称
 - `season_number`(数值):该集所属季编号
 - `season_id`(字符串):该集所属季 id
 - `episode`(字符串):该集标题
 - `episode_number`(数值):该集在季内的编号
 - `episode_id`(字符串):该集 id

属于音乐专辑曲目或其一部分的媒体可用:

 - `track`(字符串):曲目名称
 - `track_number`(数值):曲目在专辑/唱片中的编号
 - `track_id`(字符串):曲目 id
 - `artists`(列表):曲目艺术家
 - `artist`(字符串):曲目艺术家,逗号分隔
 - `genres`(列表):曲目流派
 - `genre`(字符串):曲目流派,逗号分隔
 - `composers`(列表):作曲者
 - `composer`(字符串):作曲者,逗号分隔
 - `album`(字符串):曲目所属专辑
 - `album_type`(字符串):专辑类型
 - `album_artists`(列表):专辑全部艺术家
 - `album_artist`(字符串):专辑全部艺术家,逗号分隔
 - `disc_number`(数值):曲目所属唱片/物理介质编号

仅在使用 `--download-sections` 时可用;对含内嵌章节的视频配合 `--split-chapters` 时,`chapter:` 前缀亦可用:

 - `section_title`(字符串):章节标题
 - `section_number`(数值):章节在文件中的编号
 - `section_start`(数值):章节起始秒数
 - `section_end`(数值):章节结束秒数

仅在 `--print` 中可用:

 - `urls`(字符串):所有请求格式的 URL,每行一个
 - `filename`(字符串):视频文件名。注意[实际文件名可能不同](#outtmpl-postprocess-note)
 - `formats_table`(表格):`--list-formats` 打印的格式表
 - `thumbnails_table`(表格):`--list-thumbnails` 打印的缩略图表
 - `subtitles_table`(表格):`--list-subs` 打印的字幕表
 - `automatic_captions_table`(表格):`--list-subs` 打印的自动字幕表

 仅在视频下载完成后可用(`post_process`/`after_move`):

 - `filepath`:下载视频文件的实际路径

仅在 `--sponsorblock-chapter-title` 中可用:

 - `start_time`(数值):章节起始秒数
 - `end_time`(数值):章节结束秒数
 - `categories`(列表):章节所属的 [SponsorBlock 分类](https://wiki.sponsor.ajay.app/w/Types#Category)
 - `category`(字符串):章节所属的最小 SponsorBlock 分类
 - `category_names`(列表):分类的友好名称
 - `name`(字符串):最小分类的友好名称
 - `type`(字符串):章节的 [SponsorBlock 动作类型](https://wiki.sponsor.ajay.app/w/Types#Action_Type)

输出模板中引用上述序列时,会被替换为对应字段的实际值。例如 `-o %(title)s-%(id)s.%(ext)s`,对一个标题为 `yt-dlp test video`、id 为 `YE7VzlLtp-4` 的 mp4 视频,会在当前目录生成 `yt-dlp test video-YE7VzlLtp-4.mp4`。

**注意**:部分序列不保证存在,取决于具体提取器拿到的元数据。缺失时以 `--output-na-placeholder` 提供的占位符替换(默认 `NA`)。

**提示**:用 `-j` 输出查看特定 URL 有哪些字段可用。

数值序列可使用[数值相关格式化](https://docs.python.org/3/library/stdtypes.html#printf-style-string-formatting);如 `%(view_count)05d` 会输出前导零补足 5 位的观看数,如 `00042`。

输出模板也可以包含任意层级路径,如 `-o "%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s"`,会按路径模板把每个视频下载到对应目录;缺失目录会自动创建。

在模板中输出字面百分号用 `%%`。输出到 stdout 用 `-o -`。

当前默认模板为 `%(title)s [%(id)s].%(ext)s`。

某些场景下你不希望文件名里出现"中"、空格、`&` 之类的特殊字符,例如要把文件传到 Windows 或经过 8 位不安全信道。这时加 `--restrict-filenames` 可得到更受限的文件名。

#### 输出模板示例

```bash
$ yt-dlp --print filename -o "test video.%(ext)s" ptd1NN40vMw
test video.webm    # Literal name with correct extension

$ yt-dlp --print filename -o "%(title)s.%(ext)s" ptd1NN40vMw
To'y!🤯😂🤦🏻‍♂️.webm    # All kinds of weird characters

$ yt-dlp --print filename -o "%(title)s.%(ext)s" ptd1NN40vMw --restrict-filenames
To_y.webm    # Restricted file name

# Download YouTube playlist videos in separate directory indexed by video order in a playlist
$ yt-dlp -o "%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s" "https://www.youtube.com/playlist?list=PLwiyx1dc3P2JR9N8gQaQN_BCvlSlap7re"

# Download YouTube playlist videos in separate directories according to their uploaded year
$ yt-dlp -o "%(upload_date>%Y)s/%(title)s.%(ext)s" "https://www.youtube.com/playlist?list=PLwiyx1dc3P2JR9N8gQaQN_BCvlSlap7re"

# Prefix playlist index with " - " separator, but only if it is available
$ yt-dlp -o "%(playlist_index&{} - |)s%(title)s.%(ext)s" YE7VzlLtp-4 "https://www.youtube.com/user/TheLinuxFoundation/playlists"

# Download all playlists of YouTube channel/user keeping each playlist in separate directory:
$ yt-dlp -o "%(uploader)s/%(playlist)s/%(playlist_index)s - %(title)s.%(ext)s" "https://www.youtube.com/user/TheLinuxFoundation/playlists"

# Download Udemy course keeping each chapter in separate directory under MyVideos directory in your home
$ yt-dlp -u user -p password -P "~/MyVideos" -o "%(playlist)s/%(chapter_number)s - %(chapter)s/%(title)s.%(ext)s" "https://www.udemy.com/java-tutorial"

# Download entire series season keeping each series and each season in separate directory under C:/MyVideos
$ yt-dlp -P "C:/MyVideos" -o "%(series)s/%(season_number)s - %(season)s/%(episode_number)s - %(episode)s.%(ext)s" "https://videomore.ru/kino_v_detalayah/5_sezon/367617"

# Download video as "C:\MyVideos\uploader\title.ext", subtitles as "C:\MyVideos\subs\uploader\title.ext"
# and put all temporary files in "C:\MyVideos\tmp"
$ yt-dlp -P "C:/MyVideos" -P "temp:tmp" -P "subtitle:subs" -o "%(uploader)s/%(title)s.%(ext)s" YE7VzlLtp-4 --write-subs

# Download video as "C:\MyVideos\uploader\title.ext" and subtitles as "C:\MyVideos\uploader\subs\title.ext"
$ yt-dlp -P "C:/MyVideos" -o "%(uploader)s/%(title)s.%(ext)s" -o "subtitle:%(uploader)s/subs/%(title)s.%(ext)s" YE7VzlLtp-4 --write-subs

# Stream the video being downloaded to stdout
$ yt-dlp -o - YE7VzlLtp-4
```
