# eyes — Computer Vision

## Purpose
Captures frames from the camera, detects and recognizes faces, provides visual context to brain.

## Public API
- `class Eyes`
  - `async def see() -> Frame` — capture a single frame
  - `async def detect_faces() -> list[Face]` — capture frame and return detected faces
  - `async def describe_scene() -> str` — capture frame and return text description for brain
  - `def is_available() -> bool` — check if camera is accessible

## Data Structures
- `Frame` — wrapper around numpy array (OpenCV image)
- `Face` — value object with `name: str | None`, `location: tuple`, `confidence: float`

## Constraints / Edge Cases
- Camera may not be available — `is_available()` returns `False`
- Camera index set via `CAMERA_INDEX` env var (default: `0`)
- Face recognition requires pre-enrolled face data
- In mock mode, return synthetic frames/faces without accessing camera
