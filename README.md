# Motion Maven

**Motion Maven** is a custom-built handheld motion-controlled game system.  
It runs on a Seeed Studio **XIAO ESP32-C3** and features a **128×64 OLED**,  
**ADXL345 accelerometer**, **rotary encoder**, **NeoPixel**, **arcade button**,  
and **piezo speaker**. The player performs physical gestures—tilts, spins,  
and shakes—within shrinking time limits as difficulty increases.

---

## How the Game Works

### Overview
Motion Maven challenges the player to complete a sequence of motion-based actions.  
Each round displays a required move on the OLED screen, and the player must  
perform it before time runs out. Rounds get faster and more difficult as the game progresses.

There are **11 rounds** per game. Each successful move awards points, with bonus points  
for completing the action quickly.

---

## Move Types

### Tilt Moves
Detected using the ADXL345 accelerometer:

- Tilt **LEFT**
- Tilt **RIGHT**
- Tilt **FORWARD**
- Tilt **BACK**

A calibration system establishes a neutral reference point and smoothing filters  
ensure stable readings.

### Spin Moves
Performed using the rotary encoder:

- **Spin Clockwise (CW)**
- **Spin Counterclockwise (CCW)**

The game measures encoder delta from the start of the move to determine direction.

### Shake Move
A fast acceleration spike (detected using magnitude change) registers as a **Shake!** action.

---

## Game Flow

### 1. Power On
Pressing the arcade button powers the device and shows:

- A startup splash animation
- A startup sound
- Automatic calibration on first boot (or manual recalibration later)

### 2. Main Menu
Navigate using the rotary encoder:

- **Start Game**
- **Difficulty** (Easy, Medium, Hard)
- **Recalibrate**

Press the encoder button to select.

### 3. Gameplay
For each round, the OLED displays:

- Round number
- Difficulty
- Target move
- Elapsed time
- Score

If the move is completed correctly → **SUCCESS**  
If the move is incorrect or too slow → **FAIL** (game over)

### 4. Scoring
Each successful move awards:

- **100 base points**
- **Up to +200 bonus points**, depending on time remaining

Harder difficulties reduce the available time and include more move types.

### 5. High Scores
After the game ends, the player may:

- See the final score
- Enter initials (if score qualifies for top 3)
- View stored high scores  
  (Saved in on-device storage using `highscores.py`.)

---

## Hardware Components

- XIAO ESP32-C3 microcontroller  
- 128×64 OLED display (SSD1306 over I²C)  
- ADXL345 accelerometer (I²C)  
- Rotary encoder with push button  
- Arcade button (power toggle)  
- Piezo buzzer  
- NeoPixel RGB LED  
- Perfboard  
- LiPo battery

---

## Visual & Audio Feedback

### NeoPixel States
- **Blue** — Menu / Idle  
- **Yellow** — During gameplay  
- **Green** — Successful move  
- **Red** — Failed move / Game over  

### Piezo Sounds
- Startup jingle  
- Success chime  
- Failure tone  
- Victory fanfare  

### OLED UI
High-contrast text UI showing:

- Current move  
- Timer  
- Score  
- Menu options  
- Calibration prompts  

---

## Software Architecture

### File Structure
- `code.py` — Main game loop & state machine  
- `motion.py` — Calibration, smoothing, tilt classification, shake detection  
- `audio.py` — Piezo sound effects  
- `highscores.py` — Score storage and retrieval  
- `pixel_effects.py` — Optional NeoPixel helpers  

### Main System Responsibilities
- Poll hardware inputs (button, encoder, accelerometer)  
- Update game states (menu, play, result, high scores)  
- Handle scoring and difficulty  
- Render text to OLED  
- Play audio and LED feedback  
- Store high scores persistently  

---

## Features Summary

- Fully motion-controlled gameplay  
- Real-time tilt, spin, and shake detection  
- Multiple difficulty levels  
- Persistent high-score system  
- Animated splash screen  
- Modular, maintainable codebase  
- Runs on compact, low-power embedded hardware  


