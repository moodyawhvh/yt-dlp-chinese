> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。
>
> 📖 本文对应原版 README 的 EMBEDDING YT-DLP 章节(Python API 嵌入)。

# 嵌入 YT-DLP(EMBEDDING YT-DLP)

yt-dlp 尽力成为一个良好的命令行程序,因此可以从任何编程语言调用。

你的程序应避免解析普通的 stdout,因为其输出格式未来可能变化。应改用 `-J`、`--print`、`--progress-template`、`--exec` 等选项,生成稳定可复现、便于解析的控制台输出。

在 Python 程序中,你可以用更强大的方式嵌入 yt-dlp:

```python
from yt_dlp import YoutubeDL

URLS = ['https://www.youtube.com/watch?v=YE7VzlLtp-4']
with YoutubeDL() as ydl:
    ydl.download(URLS)
```

多数情况下你还需要各种选项。可用选项见 [`yt_dlp/YoutubeDL.py`](../yt_dlp/YoutubeDL.py#L183),或在 Python shell 中执行 `help(yt_dlp.YoutubeDL)`。如果你已熟悉 CLI,可用 [`devscripts/cli_to_api.py`](https://github.com/yt-dlp/yt-dlp/blob/master/devscripts/cli_to_api.py) 把任意命令行开关翻译成 `YoutubeDL` 参数。

**提示**:如果你要把代码从 youtube-dl 迁到 yt-dlp,一个关键差异是:`YoutubeDL.extract_info` 的返回值**不保证**可 JSON 序列化,甚至不保证是字典——它只是"类字典"。想确保得到可序列化的字典,请按[下方示例](#提取信息)用 `YoutubeDL.sanitize_info` 处理。

## 嵌入示例

#### 提取信息

```python
import json
import yt_dlp

URL = 'https://www.youtube.com/watch?v=YE7VzlLtp-4'

# ℹ️ See help(yt_dlp.YoutubeDL) for a list of available options and public functions
ydl_opts = {}
with yt_dlp.YoutubeDL(ydl_opts) as ydl:
    info = ydl.extract_info(URL, download=False)

    # ℹ️ ydl.sanitize_info makes the info json-serializable
    print(json.dumps(ydl.sanitize_info(info)))
```
#### 用 info-json 下载

```python
import yt_dlp

INFO_FILE = 'path/to/video.info.json'

with yt_dlp.YoutubeDL() as ydl:
    error_code = ydl.download_with_info_file(INFO_FILE)

print('Some videos failed to download' if error_code
      else 'All videos successfully downloaded')
```

#### 提取音频

```python
import yt_dlp

URLS = ['https://www.youtube.com/watch?v=YE7VzlLtp-4']

ydl_opts = {
    'format': 'm4a/bestaudio/best',
    # ℹ️ See help(yt_dlp.postprocessor) for a list of available Postprocessors and their arguments
    'postprocessors': [{  # Extract audio using ffmpeg
        'key': 'FFmpegExtractAudio',
        'preferredcodec': 'm4a',
    }]
}

with yt_dlp.YoutubeDL(ydl_opts) as ydl:
    error_code = ydl.download(URLS)
```

#### 过滤视频

```python
import yt_dlp

URLS = ['https://www.youtube.com/watch?v=YE7VzlLtp-4']

def longer_than_a_minute(info, *, incomplete):
    """Download only videos longer than a minute (or with unknown duration)"""
    duration = info.get('duration')
    if duration and duration < 60:
        return 'The video is too short'

ydl_opts = {
    'match_filter': longer_than_a_minute,
}

with yt_dlp.YoutubeDL(ydl_opts) as ydl:
    error_code = ydl.download(URLS)
```

#### 添加日志器与进度钩子

```python
import yt_dlp

URLS = ['https://www.youtube.com/watch?v=YE7VzlLtp-4']

class MyLogger:
    def debug(self, msg):
        # For compatibility with youtube-dl, both debug and info are passed into debug
        # You can distinguish them by the prefix '[debug] '
        if msg.startswith('[debug] '):
            pass
        else:
            self.info(msg)

    def info(self, msg):
        pass

    def warning(self, msg):
        pass

    def error(self, msg):
        print(msg)


# ℹ️ See "progress_hooks" in help(yt_dlp.YoutubeDL)
def my_hook(d):
    if d['status'] == 'finished':
        print('Done downloading, now post-processing ...')


ydl_opts = {
    'logger': MyLogger(),
    'progress_hooks': [my_hook],
}

with yt_dlp.YoutubeDL(ydl_opts) as ydl:
    ydl.download(URLS)
```

#### 添加自定义后处理器

```python
import yt_dlp

URLS = ['https://www.youtube.com/watch?v=YE7VzlLtp-4']

# ℹ️ See help(yt_dlp.postprocessor.PostProcessor)
class MyCustomPP(yt_dlp.postprocessor.PostProcessor):
    def run(self, info):
        self.to_screen('Doing stuff')
        return [], info


with yt_dlp.YoutubeDL() as ydl:
    # ℹ️ "when" can take any value in yt_dlp.utils.POSTPROCESS_WHEN
    ydl.add_post_processor(MyCustomPP(), when='pre_process')
    ydl.download(URLS)
```


#### 使用自定义格式选择器

```python
import yt_dlp

URLS = ['https://www.youtube.com/watch?v=YE7VzlLtp-4']

def format_selector(ctx):
    """ Select the best video and the best audio that won't result in an mkv.
    NOTE: This is just an example and does not handle all cases """

    # formats are already sorted worst to best
    formats = ctx.get('formats')[::-1]

    # acodec='none' means there is no audio
    best_video = next(f for f in formats
                      if f['vcodec'] != 'none' and f['acodec'] == 'none')

    # find compatible audio extension
    audio_ext = {'mp4': 'm4a', 'webm': 'webm'}[best_video['ext']]
    # vcodec='none' means there is no video
    best_audio = next(f for f in formats if (
        f['acodec'] != 'none' and f['vcodec'] == 'none' and f['ext'] == audio_ext))

    # These are the minimum required fields for a merged format
    yield {
        'format_id': f'{best_video["format_id"]}+{best_audio["format_id"]}',
        'ext': best_video['ext'],
        'requested_formats': [best_video, best_audio],
        # Must be + separated list of protocols
        'protocol': f'{best_video["protocol"]}+{best_audio["protocol"]}'
    }


ydl_opts = {
    'format': format_selector,
}

with yt_dlp.YoutubeDL(ydl_opts) as ydl:
    ydl.download(URLS)
```
