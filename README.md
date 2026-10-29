# whisper-model

**English** | [Bahasa Indonesia](README.id.md)

Storage for the **faster-whisper small** model used by the Claude skill **video-audio-transcribe**, a skill that lets Claude "hear" the audio of videos uploaded to chat.

## Why this repo exists

Claude can only see the images (frames) of a video, not hear its sound. The fix is to convert the audio to text with Whisper, then have Claude read that text alongside the video frames.

The catch: the container where Claude runs code can't reach HuggingFace, where Whisper models are normally downloaded. GitHub isn't blocked, so the model is stored here as a **Release**.

## Contents

| Release | File | Size |
|---|---|---|
| `model-v1` | `faster-whisper-small.tar.gz` | ~426 MB |

Inside the archive:

```
faster-whisper-small/
├── model.bin
├── config.json
├── tokenizer.json
└── vocabulary.txt
```

Direct download link:

```
https://github.com/ashfy/whisper-model/releases/download/model-v1/faster-whisper-small.tar.gz
```

## How the skill works

1. A video is uploaded to a Claude chat.
2. The skill downloads the model from this repo's Release.
3. Audio is extracted with ffmpeg and transcribed by faster-whisper (Indonesian by default).
4. Video frames are extracted and arranged into timestamped grids.
5. Claude combines the transcript and frames into one analysis.

Test result: a 42-second video was transcribed in about 21 seconds.

## Rebuilding the Release

The Release is created automatically by GitHub Actions via `.github/workflows/model.yml`. If the Release is deleted or you want to rebuild it:

1. Delete the old `model-v1` Release and tag (if they still exist).
2. Open the **Actions** tab → **Siapkan model Whisper** → **Run workflow** (branch `main`).
3. Wait for the green check mark (~2–5 minutes).

The workflow downloads `Systran/faster-whisper-small` from HuggingFace, packs it as a `.tar.gz`, and uploads it to the `model-v1` Release.

## Important

- This repo must stay **Public**, because Claude's container can't log in to GitHub.
- Don't rename the repo, tag, or file without updating the skill, since the skill downloads from the link above.

## License & sources

- Model: [Systran/faster-whisper-small](https://huggingface.co/Systran/faster-whisper-small), a CTranslate2 conversion of [OpenAI Whisper](https://github.com/openai/whisper) (MIT license).
- Engine: [faster-whisper](https://github.com/SYSTRAN/faster-whisper) (MIT license).
