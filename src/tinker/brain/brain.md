# brain — LLM Conversation

## Purpose
Handles all interaction with the Claude API. Receives text input (from ears), maintains conversation history, and returns response text (for voice).

## Public API
- `class Brain`
  - `async def think(text: str) -> str` — send user text, get LLM response
  - `async def think_with_context(text: str, visual_context: str | None = None) -> str` — include visual context from eyes
  - `def reset() -> None` — clear conversation history

## Data Structures
- `Message` — value object with `role: str` and `content: str`
- Conversation history: `list[Message]`

## Constraints / Edge Cases
- API key must be set via `ANTHROPIC_API_KEY` env var
- Handle API rate limits and timeouts gracefully
- Conversation history should be bounded (max N messages) to control token usage
- In mock mode, return canned responses without calling the API
