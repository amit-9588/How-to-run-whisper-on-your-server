# How-to-run-whisper-on-your-server

## What is whisper and how you can run ?
- Whisper is an open-source automatic speech recognition (ASR) system developed by OpenAI that can transcribe speech in multiple languages with high accuracy.

### What Whisper Returns
- Whisper returns different types of outputs depending on how you use it. The main return types are:

1. Basic Transcription
Returns a string containing the transcribed text
Example: `"This is a test recording"`

2. Detailed Output (when return_dict=True)
Returns a dictionary containing:

```
{
    'text': str,               # Transcribed text
    'segments': list[dict],     # Detailed segment information
    'language': str             # Detected language code
}
```

3. Segment-Level Information
Each segment in the 'segments' list contains:
```
{
    'id': int,                  # Segment number
    'seek': int,                # Time offset
    'start': float,             # Start time in seconds
    'end': float,               # End time in seconds
    'text': str,                # Text of the segment
    'tokens': list[int],        # Token IDs
    'temperature': float,       # Sampling temperature
    'avg_logprob': float,       # Average log probability
    'compression_ratio': float, # Compression ratio
    'no_speech_prob': float     # Probability of no speech
}
```
### ⚙️ Whisper Model Variants
Whisper comes in several model sizes with different capabilities:

|Model	 |Parameters |Relative Speed	| Languages |Supported	Best For       |
|--------|-----------|----------------|-----------|--------------------------|
|tiny	   |39M	       |32x	            |96+	      |Mobile/embedded           |
|base	   |74M	       |16x	            | 96+       |	Basic transcription      |
|small   |244M       |6x	            |96+	      |Balance of speed/accuracy |
|medium  |769M	     | 2x	            | 96+	      |High accuracy             |
|large   |1550M	     | 1x	            |96+	      |Best accuracy             |
|large-v2|1550M	     | 1x	            |96+	      |Improved accuracy         |


### Whisper can perform different tasks based on your prompt:
- Transcription (default)
- Translation (to English)
- Language identification
- Voice activity detection

## ✅ Requirements
- Python 3.8+
- FFmpeg
- CUDA-capable GPU (optional but speeds up inference significantly)
- Pip / virtualenv (recommended)

### OpenAI's Whisper can perform speech translation, but with some important limitations:

## How Whisper Handles Translation
### Translation to English Only:
- Whisper can translate speech from other languages into English
- It cannot translate between non-English language pairs (e.g., French to Spanish)
- The output is always English text

### Two-Step Process:
- First transcribes the audio in the original language
- Then translates that transcription to English

## Key Limitations
### Directionality:
- Only supports translation to English
- Cannot translate English to other languages

### Quality Variance:
- Works best with widely-spoken languages
- Less accurate for low-resource languages

### Not Perfect:
- More accurate for transcription than translation
- May miss nuances and idioms

## When to Use Whisper Translation
Good for:
- Quick understanding of foreign language audio
- Creating English subtitles for foreign content
- rocessing multilingual meetings where English output is acceptable

Not ideal for:
- Professional translation needs
- Non-English language pairs
- Situations requiring cultural/language nuance

## 🧠 Step-by-Step Installation and Usage
### 1. Install Python & FFmpeg & venv:
```
sudo apt update
sudo apt install python3 ffmpeg
sudo apt install python3-venv
```
macOS (with Homebrew):
```
brew install ffmpeg
```

### 2. Create and Activate Virtual Environment (Optional but Recommended)
```
python3 -m venv whisper-env
source whisper-env/bin/activate
```
### 3. Install Whisper
```
pip install git+https://github.com/openai/whisper.git
```
This installs Whisper and its dependencies, including torch.

### 4. Transcribe Audio
```
whisper path/to/audio.mp3 --model medium
```

Or in Python:
```
import whisper

model = whisper.load_model("medium")  # or "base", "small", "large"
result = model.transcribe("path/to/audio.mp3")
print(result["text"])
```

### 💡 Tips
- Use `.mp3`, `.wav`, `.m4a`, or `.ogg` audio files.
- Convert video to audio using FFmpeg if needed:
```
ffmpeg -i input.mp4 -ar 16000 -ac 1 -c:a pcm_s16le output.wav
```

(optional) 
if you want to run whisper on the server(aws) and you want to upload some file from the local system then you can use this command :
```
scp -i key.pem /path/to/audio.mp3 ubuntu@your-server-ip:~
```
