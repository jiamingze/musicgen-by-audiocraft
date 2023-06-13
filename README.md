# AI 文本生成音乐-AudioCraft

近期Meta在github上开源了一个AI生成音乐工具，audiocraft（https://github.com/facebookresearch/audiocraft),可以通过文本生成音乐。
接下来我们通过两种方式来体验下：
<li> 在线访问生成
<li> 本地部署方式

### 1. 在线访问生成
打开https://huggingface.co/spaces/facebook/MusicGen, 我们可以看到如下页面，在Describe your music中可以输入我们文本prompt，同时也可以上传参考音乐（可选）。Examples中一个5个示例，点击示例同样也可以进行生成音乐体验。点击Generate后，数分钟后可以生成音乐。
  
### 2.本地部署方式
<b>2.1 创建环境（可省略）</b>
```shell
conda create -n musicgen python=3.9
```
<b>2.2 安装audiocraft</b>
```shell
pip install 'torch>=2.0'
pip install ffmpeg

# 选择以下中任意一种方式
pip install -U audiocraft  # stable release

pip install -U git+https://git@github.com/facebookresearch/audiocraft#egg=audiocraft  # bleeding edge
pip install -e .  # or if you cloned the repo locally
```
<b>2.3 模型下载（可忽略)</b>

audiocraft为我们提供了四个预训练的模型，可以选择其中任何一个进行下载安装：
- `small`: 300M model, text to music only - [🤗 Hub](https://huggingface.co/facebook/musicgen-small)
- `medium`: 1.5B model, text to music only - [🤗 Hub](https://huggingface.co/facebook/musicgen-medium)
- `melody`: 1.5B model, text to music and text+melody to music - [🤗 Hub](https://huggingface.co/facebook/musicgen-melody)
- `large`: 3.3B model, text to music only - [🤗 Hub](https://huggingface.co/facebook/musicgen-large)

<b>2.4 jupyter中进行音乐生成</b>
```python
from audiocraft.models import MusicGen
from audiocraft.utils.notebook import display_audio

# 在m1上可能会报错，可以使用MusicGen.get_pretrained('small', device='cpu')
# 具体模型可以根据从2.3中选择，若本地未下载模型，执行该代码后可以自动下载相关模型
model = MusicGen.get_pretrained('small')  

# promt生成音乐
output = model.generate(
    descriptions=[
        '80s pop track with bassy drums and synth',
        '90s rock song with loud guitars and heavy drums',
    ],
    progress=True
)
display_audio(output, sample_rate=32000)
```
