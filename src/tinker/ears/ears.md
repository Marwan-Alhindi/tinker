# ears — Speech-to-Text

## Purpose
Captures audio from the microphone and transcribes it to text using faster-whisper.

## Public API
- `class Ears`
  - `async def listen() -> str` — record audio until silence detected, return transcribed text
  - `async def listen_continuous() -> AsyncIterator[str]` — stream transcriptions continuously
  - `def is_available() -> bool` — check if microphone is accessible

## Data Structures
- Audio buffer: raw PCM bytes from microphone
- Transcription result: `str`

## Constraints / Edge Cases
- Microphone may not be available — return gracefully, `is_available()` returns `False`
- Silence detection threshold should be configurable
- Whisper model size set via `WHISPER_MODEL` env var (default: `tiny`)
- In mock mode, return canned text without accessing microphone
