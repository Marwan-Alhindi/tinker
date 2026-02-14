# voice — Text-to-Speech

## Purpose
Converts text to speech using piper-tts and plays it through the speaker.

## Public API
- `class Voice`
  - `async def say(text: str) -> None` — synthesize and play speech
  - `async def say_to_file(text: str, path: Path) -> None` — synthesize and save to WAV file
  - `def is_available() -> bool` — check if speaker is accessible

## Data Structures
- Audio output: WAV bytes from piper-tts

## Constraints / Edge Cases
- Speaker may not be available — `is_available()` returns `False`, `say()` logs warning and returns
- Piper voice model set via `PIPER_VOICE` env var (default: `en_US-lessac-medium`)
- Voice model files stored in `models/` directory
- In mock mode, log the text instead of playing audio
- Fall back to `pyttsx3` if piper-tts is not installed
