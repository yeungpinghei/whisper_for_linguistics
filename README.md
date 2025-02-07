# Whisper for Linguistics: Transcribe linguistic data using automatic speech recognition

### What is automatic speech recognition (ASR)?
Automatic speech recognition is a technology that allows computers to convert human speech into text. It is also known as **computer speech recognition** or **speech-to-text**. Some examples of ASR in our daily life include automatic captions on YouTube, voice input on our phone, etc.

### Why do we need ASR in sociolinguistic research?
Research in variationist sociolinguistics often involves analyzing large amount of naturalistic speech, e.g. sociolinguistic interviews. Audio recordings must be transcribed before any analysis can take place. Conventionally, data transcription is done manually which is super time-consuming and labor-intensive! In fact, transcribing an interview takes about **ten** hours for every hour of recorded speech ([Schilling, 2013](https://doi.org/10.1017/CBO9780511980541)). If you’re a busy grad student like me, you’d want to use ASR tools like [whisperX](https://github.com/m-bain/whisperX) to help you with data transcription. It will save you a LOT of time.

# Let’s get started!
<img width="1216" align="center" alt="whisperx-arch" src="https://raw.githubusercontent.com/m-bain/whisperX/refs/heads/main/figures/pipeline.png">

* [WhisperX](https://github.com/m-bain/whisperX) is a multilingual ASR model based on [OpenAI's Whisper](https://github.com/openai/whisper)
* It is open-source, which means that it is freely available to the public
* It supports [28 different languages](https://github.com/m-bain/whisperX/blob/f2da2f858e99e4211fe4f64b5f2938b007827e17/whisperx/alignment.py#L24-L58)
* Key features:
    1. Automatically detects the languages being spoken and handles code-switching
    2. Separates the voices of different speakers (speaker diarization)
    3. Provides accurate timestamps for the transcription
* If you are analyzing a language not supported by whisperX, you may consider using [whisper.cpp](https://github.com/ggerganov/whisper.cpp) instead.

# How to install whisperX:
### 1. Before installing whisperX, make sure that you have [miniconda](https://docs.anaconda.com/miniconda/install/) and [ffmpeg](https://www.ffmpeg.org/download.html) installed. ✅✅
You may enter the following commands in your Terminal/Command Prompt to check whether you have `miniconda` and `ffmpeg` installed.
```
conda --version
ffmpeg -version
```
If they are properly installed, it should return the version numbers. If not, it should return an error message.

### 2. Go to Terminal/Command Prompt and enter the following commands
```
conda create --name whisperx python=3.10
conda activate whisperx
```
### 3. Install WhisperX
```
pip install whisperx
```
### 4. Sign up for [Hugging Face](https://huggingface.co/)
<img width="400" align="center" src="https://github.com/yeungpinghei/whisper_for_linguistics/blob/main/figures/hugging_face.png" alt="Hugging Face log in">

### 5. Accept user agreement for [Segmentation](https://huggingface.co/pyannote/segmentation-3.0) and [Speaker-Diarization-3.1](https://huggingface.co/pyannote/speaker-diarization-3.1)

### 6. Click on your profile at the at the top-right corner and choose **Access Tokens**

### 7. Create a Hugging Face access token (read). Choose **Read** for token type.
 
### 8. Save the access token. We will need this later.
<img width="500" align="center" src="https://github.com/yeungpinghei/whisper_for_linguistics/blob/main/figures/access_token.png" alt="access token">

## How to use whisperX:
### 1. Download the example audio file `example.wav` from the GitHub repository. It is an excerpt from my Malaysian English interview data.
### 2. Go to Terminal/Command Prompt and run the following command
```
whisperx /your_directory/example.wav --model small --compute_type int8 --output_format srt --output_dir /your_directory/ --diarize --hf_token [your access token]
```

* Change `your_directory` to the directory where you saved the audio file
* The option `--model` determines the language model being used.
There are many different [models](https://github.com/ggerganov/whisper.cpp/blob/master/models/README.md#available-models) available. The smaller ones are faster but less accurate, while the bigger ones are slower but more accurate. Here I am using the `small` model since it is a good compromise between speed and accuracy.
* `—compute_type` should be set to `int8` if you are using a Mac. Otherwise, you may skip this option.
* `--output_format` determines the format of the output transcript. You may choose between `srt`, `vtt`, `txt`, `tsv`, `json`, or `all` for all formats
* `--output_dir` determines where the output file is saved
* `--diarize` activates speaker diarization (separating the speech of different speakers). You can use the options `--min_speakers` `--max_speakers` to specify the number of speakers.
* If you want to perform speaker diarization, you must provide your Hugging Face access token using the option `--hf_token`. Replace `[your access token]` with your access token.
* WhisperX automatically detects the language in the recordings, but you may also specify it using the option `--language`. The list of all supported languages can be found [here](https://github.com/m-bain/whisperX/blob/f2da2f858e99e4211fe4f64b5f2938b007827e17/whisperx/alignment.py#L24-L58).

### 3. If the program runs successfully, it should generate an .srt file that looks like this:
```
1
00:00:00,831 --> 00:00:02,032
[SPEAKER_01]: Nope, I wouldn't say so.

2
00:00:02,132 --> 00:00:08,635
[SPEAKER_01]: Like, I mean, close in the sense that it's still within the vicinity of the greater Kuala Lumpur area, right?

3
00:00:08,715 --> 00:00:11,037
[SPEAKER_01]: Or what we would call the Kuala Lumpur Valley.

4
00:00:11,237 --> 00:00:16,579
[SPEAKER_01]: But I think it would take at least like 30 to 40 minutes drive by car to get there.

5
00:00:17,420 --> 00:00:17,760
[SPEAKER_00]: Okay.
```

### 4. You can then import the .srt file to [ELAN](https://archive.mpi.nl/tla/elan) or convert it to a Praat .TextGrid file using [this Python script](https://github.com/yeungpinghei/srt-to-praat).

