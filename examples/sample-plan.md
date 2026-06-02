# Implementation Plan: Snake Game

## Objective
Create a web-based Snake game as a single HTML file with dark theme, keyboard/touch controls, and scoring system.

## Project Context
- Tech stack: Vanilla HTML5 + CSS3 + JavaScript (no frameworks)
- Project structure: New project, single file `index.html` in project root
- Code style: ES6+, camelCase, 2-space indent, comments in English

## Steps

### Step 1: Create index.html
- File: `index.html`
- Action: Create
- Description: Full single-file Snake game with inline CSS and JS
- Code notes:
  - HTML: `<canvas>` element, score display, control buttons, message overlay
  - CSS: Dark theme (#1a1a2e background), flexbox centering, responsive canvas
  - JS: Game state machine (IDLE/RUNNING/PAUSED/GAME_OVER), 20x20 grid
  - Controls: Arrow keys + WASD + Space (pause), touch swipe (30px threshold)
  - Food: Random spawn avoiding snake body, glow effect (shadowBlur)
  - Snake: Rounded rectangles, gradient color head→tail
  - Speed: Start 150ms, decrease 10ms per 5 food eaten (min 60ms)
  - Score: +10 per food, localStorage high score

## Pitfalls
- Touch events need `passive: false` for `preventDefault()`
- Food spawn must retry if overlapping snake body
- Prevent reverse direction (can't go left while moving right)
- Clean up setInterval on `beforeunload`
