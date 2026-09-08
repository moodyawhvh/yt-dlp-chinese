> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。
>
> 📖 本文对应原版 README 的 FORMAT SELECTION 章节(格式选择)。

# 格式选择(FORMAT SELECTION)

**不**传任何选项时,yt-dlp 默认尝试下载最佳可用画质。
这通常等价于 `-f bestvideo*+bestaudio/best`。但若启用多音频流(`--audio-multistreams`),默认格式变为 `-f bestvideo+bestaudio/best`;同理,若 ffmpeg 不可用、或你用 yt-dlp 向 `stdout` 流式输出(`-o -`),默认变为 `-f best/bestvideo+bestaudio`。

**弃用警告**:新版 yt-dlp 可借助 ffmpeg 同时向 stdout 流式输出多种格式。因此未来版本中,该场景的默认值将改为与普通下载一致的 `-f bv*+ba/b`。想保留 `-f b/bv+ba` 行为的,建议在配置里显式指定。

格式选择的一般语法是 `-f FORMAT`(或 `--format FORMAT`),其中 `FORMAT` 是一个*选择器表达式*,描述你想下载的格式。

**tl;dr:** 直接[看示例](#格式选择示例)。

最简单的情况是直接指定某个格式,如 `-f 22` 下载格式代码为 22 的格式。用 `--list-formats` 或 `-F` 可查看特定视频的全部可用格式代码。注意格式代码是提取器相关的。

也可以使用文件扩展名(目前支持 `3gp`、`aac`、`flv`、`m4a`、`mp3`、`mp4`、`ogg`、`wav`、`webm`)下载该扩展名下、以单一文件形式提供的最佳画质格式,如 `-f webm` 下载单文件的 `webm` 最佳格式。

可用 `-f -` *对每个视频*交互式输入格式选择器。

还可以用特殊名称选择特殊格式:

 - `all`:分别选中**所有格式**
 - `mergeall`:选中并**合并所有格式**(必须配合 `--audio-multistreams`、`--video-multistreams` 或两者)
 - `b*`、`best*`:最佳画质且**含有视频或音频任一**的格式(即 `vcodec!=none or acodec!=none`)
 - `b`、`best`:最佳画质且**同时含有**视频与音频的格式。等价于 `best*[vcodec!=none][acodec!=none]`
 - `bv`、`bestvideo`:最佳画质的**纯视频**格式。等价于 `best*[acodec=none]`
 - `bv*`、`bestvideo*`:最佳画质且**含视频**的格式,可能也含音频。等价于 `best*[vcodec!=none]`
 - `ba`、`bestaudio`:最佳画质的**纯音频**格式。等价于 `best*[vcodec=none]`
 - `ba*`、`bestaudio*`:最佳画质且**含音频**的格式,可能也含视频。等价于 `best*[acodec!=none]`([勿用!](https://github.com/yt-dlp/yt-dlp/issues/979#issuecomment-919629354))
 - `w*`、`worst*`:最差画质且含视频或音频任一的格式
 - `w`、`worst`:最差画质且同时含视频与音频的格式。等价于 `worst*[vcodec!=none][acodec!=none]`
 - `wv`、`worstvideo`:最差画质的纯视频格式。等价于 `worst*[acodec=none]`
 - `wv*`、`worstvideo*`:最差画质且含视频的格式,可能也含音频。等价于 `worst*[vcodec!=none]`
 - `wa`、`worstaudio`:最差画质的纯音频格式。等价于 `worst*[vcodec=none]`
 - `wa*`、`worstaudio*`:最差画质且含音频的格式,可能也含视频。等价于 `worst*[acodec!=none]`

例如下载最差的纯视频格式用 `-f worstvideo`。但一般不建议使用 `worst` 及相关选项:`worst` 选中的是"各方面都最差"的格式,而你多数时候想要的其实是体积最小的视频。所以通常用 `-S +size` 更好,更严格的写法是 `-S +size,+br,+res,+fps`,而不是 `-f worst`。详见[格式排序](#格式排序)。

用 `best<type>.<n>` 可选中某类型的第 n 优格式。例如 `best.2` 选第二优的合并格式;`bv*.3` 选第三优的含视频流格式。

下载多个视频而它们的可用格式不同,可用斜杠给出优先顺序。左侧格式优先;如 `-f 22/17/18` 会先下 22,不可用再试 17,再不行试 18,都没有则报错无合适格式。

想下载同一视频的多个格式,用逗号分隔,如 `-f 22,17,18` 会下载这三个格式(可用时)。更复杂的组合示例:`-f 136/137/mp4/bestvideo,140/m4a/bestaudio`。

用 `-f <format1>+<format2>+...` 可把多个格式的视频与音频合并为单文件(需要 ffmpeg);如 `-f bestvideo+bestaudio` 下载最佳纯视频与最佳纯音频并用 ffmpeg 混流。

**弃用警告**:下面描述的行为复杂且反直觉,将来会被移除、多流将默认启用,取而代之会新增一个把格式限制为单音频/视频的操作符。

除非使用 `--video-multistreams`,含视频流的格式只取第一个,其余忽略;`--audio-multistreams` 与音频流同理。例如 `-f bestvideo+best+bestaudio --video-multistreams --audio-multistreams` 会下载并合并全部 3 个格式,产物含 2 路视频流与 2 路音频流;而 `-f bestvideo+best+bestaudio --no-video-multistreams` 只下载并合并 `bestvideo` 与 `bestaudio`,`best` 被忽略——因为已选中另一个含视频流的格式(`bestvideo`)。可见格式顺序很重要:`-f best+bestaudio --no-audio-multistreams` 只下载 `best`;`-f bestaudio+best --no-audio-multistreams` 则忽略 `best` 只下载 `bestaudio`。

## 过滤格式(Filtering Formats)

可以在方括号中写条件过滤格式,如 `-f "best[height=720]"`(不带选择器的过滤器按 `best` 解释,如 `-f "[filesize>10M]"`)。

以下数值字段可用于 `<`、`<=`、`>`、`>=`、`=`(等于)、`!=`(不等于)比较:

 - `filesize`:字节数(若预先已知)
 - `filesize_approx`:字节数估计值
 - `width`:视频宽度(若已知)
 - `height`:视频高度(若已知)
 - `aspect_ratio`:视频宽高比(若已知)
 - `tbr`:音视频平均码率([kbps](## "1000 bits/sec"))
 - `abr`:音频平均码率([kbps](## "1000 bits/sec"))
 - `vbr`:视频平均码率([kbps](## "1000 bits/sec"))
 - `asr`:音频采样率(赫兹)
 - `fps`:帧率
 - `audio_channels`:音频声道数
 - `stretched_ratio`:视频像素的 `width:height`(非正方形时)

字符串字段支持 `=`(等于)、`^=`(前缀)、`$=`(后缀)、`*=`(包含)、`~=`(正则匹配):

 - `url`:视频 URL
 - `ext`:文件扩展名
 - `acodec`:音频编解码器名
 - `vcodec`:视频编解码器名
 - `container`:容器格式名
 - `protocol`:实际下载所用协议,小写(`http`、`https`、`rtmp`、`rtmpe`、`f4m`、`ism`、`http_dash_segments`、`m3u8` 或 `m3u8_native`)
 - `language`:语言代码
 - `dynamic_range`:视频动态范围
 - `format_id`:格式的简短描述
 - `format`:人类可读的格式描述
 - `format_note`:格式的附加信息
 - `resolution`:宽高的文本描述

任意字符串比较前可加 `!` 取反,如 `!*=`(不包含)。字符串比较的对象若含空格或 `._-` 以外的特殊字符,须用单/双引号包裹。

**注意**:上述字段均不保证存在,完全取决于具体提取器从网站拿到的元数据。提取器提供的其他字段同样可用于过滤。

值未知的格式会被排除,除非在操作符后加问号(`?`)。过滤器可以组合:`-f "bv[height<=?720][tbr>500]"` 选取不超过 720p(或高度未知)且码率大于 500 kbps 的视频。过滤器也可配合 `all` 使用,下载满足条件的全部格式,如 `-f "all[vcodec=none]"` 选取所有纯音频格式。

格式选择器还可用圆括号分组,如 `-f "(mp4,webm)[height<480]"` 下载高度低于 480 的最佳预合并 mp4 与 webm 格式。

## 格式排序(Sorting Formats)

用 `-S`(`--format-sort`)可以改变"最佳"的判定标准,一般格式为 `--format-sort field1,field2...`。

可用字段:

 - `hasvid`:优先含视频流的格式
 - `hasaud`:优先含音频流的格式
 - `ie_pref`:格式偏好(extractor preference)
 - `lang`:提取器判定的语言偏好(如原声优于解说音轨)
 - `quality`:格式质量
 - `source`:来源偏好
 - `proto`:下载协议(`https`/`ftps` > `http`/`ftp` > `m3u8_native`/`m3u8` > `http_dash_segments` > `websocket_frag` > `f4f`/`f4m`)
 - `vcodec`:视频编解码器(`av01` > `vp9.2` > `vp9` > `h265` > `h264` > `vp8` > `h263` > `theora` > 其他)
 - `acodec`:音频编解码器(`flac`/`alac` > `wav`/`aiff` > `opus` > `vorbis` > `aac` > `mp4a` > `mp3` > `ac4` > `eac3` > `ac3` > `dts` > 其他)
 - `codec`:等价于 `vcodec,acodec`
 - `vext`:视频扩展名(`mp4` > `mov` > `webm` > `flv` > 其他);使用 `--prefer-free-formats` 时优先 `webm`
 - `aext`:音频扩展名(`m4a` > `aac` > `mp3` > `ogg` > `opus` > `webm` > 其他);使用 `--prefer-free-formats` 时顺序变为 `ogg` > `opus` > `webm` > `mp3` > `m4a` > `aac`
 - `ext`:等价于 `vext,aext`
 - `filesize`:精确文件大小(若预先已知)
 - `fs_approx`:近似文件大小
 - `size`:精确文件大小,无则近似值
 - `height`:视频高度
 - `width`:视频宽度
 - `res`:视频分辨率,按较短的一边计算
 - `fps`:视频帧率
 - `hdr`:动态范围(`DV` > `HDR12` > `HDR10+` > `HDR10` > `HLG` > `SDR`)
 - `channels`:音频声道数
 - `tbr`:总平均码率([kbps](## "1000 bits/sec"))
 - `vbr`:视频平均码率([kbps](## "1000 bits/sec"))
 - `abr`:音频平均码率([kbps](## "1000 bits/sec"))
 - `br`:平均码率([kbps](## "1000 bits/sec")),取 `tbr`/`vbr`/`abr`
 - `asr`:音频采样率(赫兹)

**弃用警告**:其中不少字段存在(暂未文档化的)别名,未来版本可能移除;建议只用这里列出的字段名。

除非特别说明,所有字段均按降序排序。加 `+` 前缀反转,如 `+res` 偏好分辨率更小的格式。此外,字段后可加 `:` 分隔的偏好值,如 `res:720` 偏好更大但不超过 720p 的视频;没有低于 720p 的就选最小视频。`codec` 与 `ext` 可给两个偏好值,依次作用于视频与音频,如 `+codec:avc:m4a`(等价于 `+vcodec:avc,+acodec:m4a`)把视频编解码偏好设为 `h264` > `h265` > `vp9` > `vp9.2` > `av01` > `vp8` > `h263` > `theora`,音频偏好设为 `mp4a` > `aac` > `vorbis` > `opus` > `mp3` > `ac3` > `dts`。用 `~` 作分隔符可让排序偏好最接近给定值的项,如 `filesize~1G` 偏好文件大小最接近 1 GiB 的格式。

无论用户怎么排序,`hasvid` 与 `ie_pref` 始终拥有最高优先级;可用 `--format-sort-force` 改变该行为。除此之外的默认顺序为:`lang,quality,res,fps,hdr:12,vcodec,channels,acodec,size,br,asr,proto,ext,hasaud,source,id`。提取器可以覆盖该默认顺序,但不能覆盖用户给定的顺序。

注意 hdr 的默认值是 `hdr:12`,即不偏好杜比视界(Dolby Vision)——因为 DV 格式与多数设备尚未完全兼容;未来可能调整。

当格式选择器为 `worst` 时,选中排序后的最后一项,即"各方面都最差"的格式;多数时候你想要的其实是体积最小的视频,所以更推荐 `-f best -S +size,+br,+res,+fps`。

多次使用 `-S`/`--format-sort` 时,后面的排序参数会被拼接到前面的前面,重复字段只保留优先级最高的一条。如 `-S proto -S res` 等价于 `-S res,proto`;`-S res:720,fps -S vcodec,res:1080` 等价于 `-S vcodec,res:1080,fps`。用 `--format-sort-reset` 可忽略此前所有 `-S`/`--format-sort` 并重置为默认顺序。

**提示**:`-v -F` 可查看格式如何被排序(从最差到最佳)。

## 格式选择示例

```bash
# Download and merge the best video-only format and the best audio-only format,
# or download the best combined format if video-only format is not available
$ yt-dlp -f "bv+ba/b"

# Download best format that contains video,
# and if it doesn't already have an audio stream, merge it with best audio-only format
$ yt-dlp -f "bv*+ba/b"

# Same as above
$ yt-dlp

# Download the best video-only format and the best audio-only format without merging them
# For this case, an output template should be used since
# by default, bestvideo and bestaudio will have the same file name.
$ yt-dlp -f "bv,ba" -o "%(title)s.f%(format_id)s.%(ext)s"

# Download and merge the best format that has a video stream,
# and all audio-only formats into one file
$ yt-dlp -f "bv*+mergeall[vcodec=none]" --audio-multistreams

# Download and merge the best format that has a video stream,
# and the best 2 audio-only formats into one file
$ yt-dlp -f "bv*+ba+ba.2" --audio-multistreams


# The following examples show the old method (without -S) of format selection
# and how to use -S to achieve a similar but (generally) better result

# Download the worst video available (old method)
$ yt-dlp -f "wv*+wa/w"

# Download the best video available but with the smallest resolution
$ yt-dlp -S "+res"

# Download the smallest video available
$ yt-dlp -S "+size,+br"



# Download the best mp4 video available, or the best video if no mp4 available
$ yt-dlp -f "bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4] / bv*+ba/b"

# Download the best video with the best extension
# (For video, mp4 > mov > webm > flv. For audio, m4a > aac > mp3 ...)
$ yt-dlp -S "ext"



# Download the best video available but no better than 480p,
# or the worst video if there is no video under 480p
$ yt-dlp -f "bv*[height<=480]+ba/b[height<=480] / wv*+ba/w"

# Download the best video available with the largest height but no better than 480p,
# or the best video with the smallest resolution if there is no video under 480p
$ yt-dlp -S "height:480"

# Download the best video available with the largest resolution but no better than 480p,
# or the best video with the smallest resolution if there is no video under 480p
# Resolution is determined by using the smallest dimension.
# So this works correctly for vertical videos as well
$ yt-dlp -S "res:480"



# Download the best video (that also has audio) but no bigger than 50 MB,
# or the worst video (that also has audio) if there is no video under 50 MB
$ yt-dlp -f "b[filesize<50M] / w"

# Download the largest video (that also has audio) but no bigger than 50 MB,
# or the smallest video (that also has audio) if there is no video under 50 MB
$ yt-dlp -f "b" -S "filesize:50M"

# Download the best video (that also has audio) that is closest in size to 50 MB
$ yt-dlp -f "b" -S "filesize~50M"



# Download best video available via direct link over HTTP/HTTPS protocol,
# or the best video available via any protocol if there is no such video
$ yt-dlp -f "(bv*+ba/b)[protocol^=http][protocol!*=dash] / (bv*+ba/b)"

# Download best video available via the best protocol
# (https/ftps > http/ftp > m3u8_native > m3u8 > http_dash_segments ...)
$ yt-dlp -S "proto"



# Download the best video with either h264 or h265 codec,
# or the best video if there is no such video
$ yt-dlp -f "(bv*[vcodec~='^((he|a)vc|h26[45])']+ba) / (bv*+ba/b)"

# Download the best video with best codec no better than h264,
# or the best video with worst codec if there is no such video
$ yt-dlp -S "codec:h264"

# Download the best video with worst codec no worse than h264,
# or the best video with best codec if there is no such video
$ yt-dlp -S "+codec:h264"



# More complex examples

# Download the best video no better than 720p preferring framerate greater than 30,
# or the worst video (still preferring framerate greater than 30) if there is no such video
$ yt-dlp -f "((bv*[fps>30]/bv*)[height<=720]/(wv*[fps>30]/wv*)) + ba / (b[fps>30]/b)[height<=720]/(w[fps>30]/w)"

# Download the video with the largest resolution no better than 720p,
# or the video with the smallest resolution available if there is no such video,
# preferring larger framerate for formats with the same resolution
$ yt-dlp -S "res:720,fps"



# Download the video with smallest resolution no worse than 480p,
# or the video with the largest resolution available if there is no such video,
# preferring better codec and then larger total bitrate for the same resolution
$ yt-dlp -S "+res:480,codec,br"
```
