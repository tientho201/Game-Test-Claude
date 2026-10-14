# 🫘 Bean Rush 3D

**English** · [Tiếng Việt](README.vi.md)

A 3D obstacle-course party race in the style of **Fall Guys**, written in plain HTML + CSS + JavaScript with **Three.js**. It runs straight in the browser: no build step, no server.

▶️ **Play online:** ([Bean Rush 3D](https://testgameclaude.vercel.app/))

📊 **Project report** (prompt, process, testing, cost): [REPORT.md](REPORT.md) · [Bản tiếng Việt](REPORT.vi.md)

## ▶️ One-step setup

**Open `index.html` in a browser** (double-click the file). That's it.

> Three.js ships in `vendor/three.min.js`, so the game also works **offline**. If that file is missing, the game loads Three.js from a CDN instead.
> Optional, if you prefer a local server: `npx http-server .`, then open `http://localhost:8080`.

## 🎮 Controls

| Key | Action |
|---|---|
| `W A S D` / arrow keys | Move (relative to the camera) |
| `Space` | Jump (with jump buffering and coyote time) |
| `Shift` / `E` | Dive forward |
| Mouse | Click the screen to lock the pointer, then move to orbit the camera |
| `Esc` / `P` | Pause |
| Phone | Virtual joystick + **NHẢY** (jump) / **LAO** (dive) buttons; swipe to orbit the camera |

## 🏁 Rules

12 beans race: you plus 11 AI bots. **Only the first 8 across the line qualify**, and there is a 3-minute limit. Fall into the pink slime and you respawn at the last checkpoint.

The course has 7 sections:

1. **Windmills**: low spinning sweeper bars you have to jump.
2. **Floating steps**: platforms sliding sideways over the slime.
3. **Mystery doors**: 5 doors per wall, only 2 break open (randomized every round).
4. **Falling tiles**: 136 hexagonal tiles that shake and drop after you step on them, then come back after 6 seconds.
5. **Hammers**: a narrow bridge with 4 swinging pendulum hammers.
6. **Spinning discs**: 3 rotating discs with sweeping bars; you get carried by the rotation.
7. **Ball ramp**: climb the slope, dodge giant rolling balls, and dive through the finish gate.

## ✨ Features

- **Hand-written physics**: capsule collisions against oriented boxes (OBB), cylinders and spheres. Characters standing on moving or rotating platforms are carried along. Knockback uses the velocity of the contact point. Beans bump and push each other. The simulation runs in 120 Hz sub-steps, so it is smooth at any display refresh rate.
- **AI bots**: follow waypoints, jump gaps and sweeper bars, wait for sliding platforms to line up, time the hammers, dodge balls, remember fake doors and prefer doors that are already broken.
- **Effects**: squash & stretch, state-driven limb animation, confetti, dust, slime splashes, flying door debris, shader-based slime sea and sky, a course fly-over intro, and a camera that pulls in when a wall blocks the view.
- **Complete UI**: character customization (name, 10 colors, 5 accessories), how-to-play panel, countdown, HUD (timer, progress bar with every racer's position, rank, qualified count), section announcements, pause menu, results screen, personal best.
- **Synthesized Web Audio sound**: effects and background music generated in code, no audio files.
- **Settings**: mouse sensitivity, invert Y, graphics quality, shadows, volume, music, FPS counter. Saved in the browser.

> The in-game interface is in Vietnamese.

## 📁 Structure

```
index.html          ← the whole game (HTML + CSS + JS)
vendor/three.min.js ← Three.js r128 (MIT), for offline play
README.md           ← this file (English)
README.vi.md        ← Vietnamese README
REPORT.md           ← project report (English)
REPORT.vi.md        ← project report (Vietnamese)
```
