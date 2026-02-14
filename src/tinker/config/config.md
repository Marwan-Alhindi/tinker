# config — Settings

## Purpose
Loads and validates configuration from environment variables using pydantic-settings.

## Public API
- `class Settings(BaseSettings)`
  - `anthropic_api_key: str`
  - `whisper_model: str = "tiny"`
  - `piper_voice: str = "en_US-lessac-medium"`
  - `camera_index: int = 0`
  - `log_level: str = "INFO"`
  - `mock_mode: bool = False`
- `def get_settings() -> Settings` — singleton accessor

## Data Structures
- `Settings` — pydantic BaseSettings, loaded from `.env` file and environment variables

## Constraints / Edge Cases
- `ANTHROPIC_API_KEY` is required — raise clear error if missing
- All other settings have sensible defaults
- Settings are immutable after creation
- `.env` file is optional (env vars take precedence)
