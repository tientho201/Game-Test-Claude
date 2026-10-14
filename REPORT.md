# 📊 Project Report: Bean Rush 3D

**English** · [Tiếng Việt](REPORT.vi.md) · Back to [README](README.md)

This report covers what was asked, how it was built, how it was tested, and what it cost. The whole project was built by Claude Code in one cloud session on **2026-10-14**.

## 1. Summary

| Item | Result |
|---|---|
| Deliverable | A playable Fall Guys-style 3D obstacle race in the browser |
| Tech | HTML + CSS + vanilla JavaScript + Three.js r128 (no build step) |
| Setup | One step: open `index.html` in a browser (works offline) |
| Code size | `index.html`: 2,112 lines, about 118 KB (the whole game) |
| Library | `vendor/three.min.js`: about 603 KB (MIT license) |
| Console errors in tests | 0 (desktop and mobile viewports) |
| Total cost (session meter, see §7) | **≈ $5.68**, snapshot taken after the game was finished |

## 2. The prompts

These are the user's messages to Claude, verbatim (in Vietnamese), with an English translation.

**Prompt 1: the main request**

> Hãy xây dựng một [trò chơi thể loại Fall Guys 3D / hoặc một trang dashboard tài chính tương tác] hoàn chỉnh bằng HTML/TailwindCSS/JS thuần (hoặc Three.js chạy mượt 60fps).
> Yêu cầu bắt buộc:
> * Có giao diện người dùng hoàn chỉnh, trực quan, không được dùng placeholder
> * Tự tích hợp sẵn logic điều khiển, hiệu ứng chuyển động và xử lý va chạm.
> * Xuất toàn bộ mã nguồn ra dưới dạng file có thể chạy ngay được kèm hướng dẫn setup trong 1 bước.

*Translation:* Build a complete [Fall Guys-style 3D game / or an interactive finance dashboard] in plain HTML/TailwindCSS/JS (or Three.js running smoothly at 60fps). Mandatory: a complete, intuitive UI with no placeholders; built-in control logic, motion effects and collision handling; all source code delivered as files that run immediately, with a one-step setup guide.

**Prompt 2:** "làm sao tôi thấy game , chạy local được không" (*How do I see the game? Can it run locally?*)

**Prompt 3:** "tạo pull request cho nhánh qua nhánh main. Tạo cho tôi thêm readme report, báo cáo cho tôi về prompt, chi phí, các thứ cho tôi. các readme là tiếng anh nhưng vẫn có bản tiếng việt trỏ qua cho người dùng đọc" (*Open a pull request from the branch into main. Add a README report covering the prompt, cost and so on. READMEs in English, with a linked Vietnamese version.*)

## 3. How the request was interpreted

| Decision | Choice | Reason |
|---|---|---|
| Game or dashboard | **Fall Guys-style 3D game** | The prompt offered either; the game exercises all three mandatory points (controls, motion, collisions) directly. |
| Rendering | **Three.js** | The prompt allowed it as the alternative to Tailwind; a real 3D game needs WebGL. |
| Styling | Hand-written CSS instead of Tailwind | Tailwind's CDN needs internet; plain CSS keeps the game fully offline. |
| Physics | Written from scratch | No physics library needed; keeps the project to one file and lets each obstacle behave exactly as designed. |
| Library delivery | Vendored `three.min.js` + CDN fallback | "Run immediately" should not depend on a network. The build container could not reach cdnjs (HTTP 403), so the official npm package `three@0.128.0` was used to get the file. |
| UI language | Vietnamese | The user wrote in Vietnamese. |
| Assets | All generated in code | Textures are drawn on `<canvas>`, sound is synthesized with Web Audio, so there are no image or audio files to go missing. |

## 4. What was built

**Course (7 sections, about 330 m long)**: windmill sweepers, sliding platforms over slime, mystery doors (2 of 5 real per wall), 136 falling hex tiles, a pendulum-hammer bridge, 3 spinning discs, a ramp with rolling balls, and a finish gate. There are 7 checkpoints.

**Game systems inside `index.html`**

| System | What it does |
|---|---|
| Collision | Capsule (two spheres) against oriented boxes, vertical cylinders and spheres; walkable-slope detection; 2 resolve iterations per step |
| Moving platforms | Characters on a moving/rotating collider are carried using the collider's previous and current transform |
| Knockback | Impulse from the contact point's velocity, plus stun with a wobble animation |
| Movement | Ground/air acceleration, jump buffer (0.14 s), coyote time (0.1 s), dive with recovery |
| Simulation | Variable frame time split into ~120 Hz sub-steps (smooth at 60/120/144 Hz) |
| AI bots (11) | Waypoints, edge detection, platform alignment, hammer timing, ball dodging, door memory, unstick logic |
| Effects | Instanced particles (confetti, dust, slime), squash & stretch, shader slime sea and sky |
| Camera | Intro fly-over on a spline, orbit camera, pull-in when walls block the view |
| UI | Menu with customization, how-to-play, settings, countdown, HUD, toasts, pause, results, best time |
| Input | Keyboard, mouse with pointer lock (drag fallback), touch joystick and buttons |
| Audio | Synthesized effects and a procedural background music loop |

## 5. Process timeline (UTC)

| Time | Step |
|---|---|
| 16:13 | Session started; repository was empty |
| 16:15–16:20 | Vendored Three.js (cdnjs blocked → npm registry) |
| 16:20–16:30 | Wrote the full game in `index.html` |
| 16:30–16:39 | Automated browser tests, screenshot review, fixes (see §6) |
| 16:40 | First commit pushed to `claude/hopeful-gauss-459e8k` |
| 16:44 | Playable copy published as a private Claude artifact |
| 16:48 | Usage snapshot used in §7 |
| after 16:49 | This report, English/Vietnamese READMEs, pull request |

## 6. Testing

Testing used headless Chromium (Playwright) with software WebGL. Each run loaded the page, went through the menu, intro and countdown, played, and captured screenshots plus console errors. A test hook (`window.__beanRush.update`) fast-forwarded a full 150-second race without rendering.

**Problems found and fixed**

| # | Problem found | Fix |
|---|---|---|
| 1 | Bots too slow: first finisher at ~115 s, stuck at hammers and balls | Bots now time the hammers, dodge balls, and jump sweepers more reliably. First finishers now arrive at ~55 s |
| 2 | Menu: a bot blocked the camera; logo cut off at 720p | Player shown alone in front of the pack; bot labels hidden in menu; tighter menu layout |
| 3 | Start: the player was hidden behind the back row of bots | Player spawns in the last row |
| 4 | Camera ended up behind door walls, showing only the wall | Camera pulls in when a wall or door is between it and the player |
| 5 | Mobile: timer and qualified counter overlapped | Separate compact HUD layout under 820 px width |
| 6 | Mobile menu: the bean was hidden behind the logo | Camera framing adjusted for portrait screens |
| 7 | 136 hex tiles were drawn twice (shadow pass) | Tiles no longer cast shadows |

**Final measurements**

| Check | Result |
|---|---|
| Console / page errors | 0 |
| Full race simulation | Bots finish between ~55 s and ~125 s; the round ends correctly |
| Game logic per frame | ~0.8 ms (12 characters, ~200 colliders) |
| Draw calls per frame | ~690, shadow pass included |
| Frame rate | **Not measured on a real GPU.** The test machine only had software rendering (~5 FPS there). On weak hardware, lower *Graphics quality* or turn off *Shadows* in Settings |

## 7. Cost and usage

These numbers come from the cloud session's own usage meter (`get_session`). The snapshot was taken at **16:48 UTC**, after the game was built, tested, pushed and published. Writing this report and the pull request happens after the snapshot, so the final total is slightly higher.

| Metric | Value |
|---|---|
| **Cost (USD)** | **$5.68** |
| Output tokens (code, text, tool calls) | 149,749 |
| Cache read tokens | 4,795,555 |
| Cache write tokens | 215,824 |
| Uncached input tokens | 554 |
| Context used | 259,720 of 1,000,000 tokens |

**How to read this**

- **Cache reads dominate the token count.** Each tool step re-reads the conversation so far; prompt caching makes those re-reads much cheaper than fresh input.
- **Output tokens are mostly the game itself.** The 118 KB `index.html` plus edits and test scripts make up most of the 150K output tokens.
- **The dollar figure is an estimate.** It is what the session's meter reports, priced like the API. On a Claude subscription plan this usage counts against your plan limits rather than appearing as a separate bill. Check your account's usage page for the authoritative number.

## 8. Known limitations

- The in-game UI is Vietnamese only (these docs are bilingual).
- One round only; no multiplayer networking. The other 11 racers are AI.
- Text uses the *Baloo 2* font from Google Fonts; offline, the browser uses a system font instead.
- 60 FPS on real hardware has not been measured (see §6).

## 9. How to run

Open `index.html` in a browser. Full instructions: [README.md](README.md) · [README.vi.md](README.vi.md).
