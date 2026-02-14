# body — Motor and Servo Control

## Purpose
Controls physical movement — servos for head/arm positioning, DC motors for wheel drive.

## Public API
- `class Body`
  - `async def move_forward(speed: float, duration: float) -> None` — drive wheels forward
  - `async def move_backward(speed: float, duration: float) -> None` — drive wheels backward
  - `async def turn_left(speed: float, duration: float) -> None` — turn left
  - `async def turn_right(speed: float, duration: float) -> None` — turn right
  - `async def stop() -> None` — stop all motors
  - `async def look(pan: float, tilt: float) -> None` — move head servos
  - `def is_available() -> bool` — check if GPIO is accessible

## Data Structures
- Speed: `float` 0.0–1.0
- Servo angle: `float` in degrees
- Duration: `float` in seconds

## Constraints / Edge Cases
- GPIO only available on Raspberry Pi — `is_available()` returns `False` on other platforms
- Uses `gpiozero` with `pigpio` pin factory for hardware PWM
- Motor speed must be clamped to 0.0–1.0
- Servo angles must be clamped to valid range
- In mock mode, log commands without accessing GPIO
- Must clean up GPIO on shutdown
