# simone-dev.com — Complete Frontend Analysis

## 🎨 Visual Identity: Cyberpunk Sci-Fi Space Theme

The site is a **full-screen WebGL/Three.js 3D experience** rendered on an `<canvas>` element using an **OffscreenCanvas** + Web Worker architecture. The entire UI floats over a deep-space starfield background with a central 3D spiral object that unfurls into project cards.

---

## 1. Color Palette

### Primary Background
| Token | Hex | Role |
|-------|-----|------|
| `--bg-deep` | `#0E0E17` | Page/body background — an ultra-dark navy-charcoal |
| `--bg-dark` | `#000000` | Scene preloader background |

### Accent Colors
| Token | Hex | RGB | Role |
|-------|-----|-----|------|
| **Cyan / Electric Blue** | `#5EF6FF` | `rgb(94, 246, 255)` | **Primary accent** — used everywhere: loading rings, text glow, button borders, link text, UI chrome |
| **Neon Yellow** | `#FEE801` | `rgb(254, 232, 1)` | Hover/active state accent on buttons |
| **Red/Coral** | `#F75049` | `rgb(247, 80, 73)` | Glitch chromatic aberration left-shift, red planet orb |
| **Blue** | `#2770D4` | `rgb(39, 112, 212)` | Glitch chromatic aberration right-shift, button gradient |
| **Green (Matrix)** | `#41AF2A` | `rgb(65, 175, 42)` | Matrix rain effect on "About Me" card, tech/stack accent |

### Neutral Palette (from utility classes)
| Class | Hex | Usage |
|-------|-----|-------|
| `.color-1` | `#232323` | Dark text on light contexts |
| `.color-2` | `#1A1A1A` | Darker text variant |
| `.color-3` | `#0F0F0F` | Near-black |
| `.color-4` | `#3D3D3D` | Muted gray |
| `.color-5` | `#F3F8F7` | **Light text on dark** — minty off-white |
| `.color-6` | `#DFEDEC80` | Muted teal-white with 50% alpha |
| `.color-7` | `#41AF2A` | Green accent |
| `.color-8` | `#888888` | Mid-gray muted text |

### Key Takeaway
> **The palette is fundamentally dark (#0E0E17) with electric cyan (#5EF6FF) as the hero accent.** Chromatic aberration uses red (#F75049) shifted left and blue (#2770D4) shifted right for a CRT/glitch feel. White (#FFF) is used sparingly for high-contrast headlines.

---

## 2. Typography

### Font Families (4 custom fonts)

| Font | Format | Weight | Role |
|------|--------|--------|------|
| **Syncopate-Bold** | TTF | 700 | **PRIMARY — used for everything** via `* { font-family: Syncopate-Bold }`. All-caps geometric sans-serif, very wide letterforms |
| **Syncopate-Regular** | TTF | 400 | Regular weight variant (not heavily used) |
| **Nexokora** | OTF | 400 | Display/hero font — the huge "SIMONE" and "ANDREOTTI" glowing titles use a custom futuristic typeface rendered in 3D |
| **Press Start 2P** | TTF | 1-999 | Retro pixel font — used in preloader text and retro/arcade contexts |

### Type Scale (fluid `clamp()` system)

| Class | Min → Max | Line Height | Letter Spacing |
|-------|-----------|-------------|----------------|
| `h1` / `.text__title-1` | `5rem → 8rem` (80→128px) | 1.1 | -0.03em |
| `h2` / `.text__title-2` | `4rem → 6rem` (64→96px) | 1.1 | -0.02em |
| `h3` / `.text__title-3` | `3.5rem → 4.5rem` | 1.13 | -0.02em |
| `h4` / `.text__title-4` | `3rem → 4rem` | 1.15 | -0.02em |
| `h5` / `.text__title-5` | `2.5rem → 3rem` | 1.2 | -0.02em |
| `h6` / `.text__title-6` | `1.75rem → 2rem` | 1.2 | -0.02em |
| `.text__body-5xl` | `4.5rem → 8rem` | 1.15 | -0.03em |
| `.text__body-4xl` | `4rem → 6rem` | 1.1 | -0.02em |
| `.text__body-lg` | `1.625rem → 2rem` | 1.2 | -0.02em |
| `.text__body-md` | `1.5rem` (fixed) | 1.3 | -0.02em |
| `.text__body-base` | `1.25rem` (fixed) | 1.4 | -0.01em |
| `.text__body-sm` | `1.125rem` (fixed) | 1.4 | -0.01em |
| `.text__body-xs` | `1rem` (fixed) | 1.4 | -0.0075em |
| `.text__body-2xs` | `0.875rem` (fixed) | 1.4 | -0.005em |

### Typography Characteristics
- **Negative letter-spacing** everywhere (-0.01em to -0.03em) — creates tight, futuristic feel
- **Uppercase** (`text-transform: uppercase`) is the default treatment for all display text
- **Font smoothing**: Both `-webkit-font-smoothing: antialiased` and `-moz-osx-font-smoothing: grayscale` are applied globally
- **Fluid typography** using `clamp()` with viewport-width calculations — breakpoint at 992px

---

## 3. Layout System

### Grid System
- **12-column grid** on desktop (`repeat(12, 1fr)`)
- **8-column** on tablet (≤992px)
- **4-column** on mobile (≤576px)
- Gap: `clamp(1rem, ... , 2rem)` — fluid gap sizing
- Grid auto-flow: `row` on desktop, `dense` on mobile

### Width Containers
Three container widths with granular breakpoints from 576px to 1700px:
- `.width-limit` — standard container (1120px → 1586px)
- `.width-limit-large` — wider container
- `.width-limit-small` — narrower container

### The Page is a Single Full-Screen Canvas
```
body → background: #0E0E17
  └── main.v2-home
       └── section.v2-scene-section
            └── #v2-container (width: 100%, height: 100vh)
                 └── canvas#v2-canvas (OffscreenCanvas)
```

The entire site is rendered as one viewport-locked canvas with **no traditional scrolling content**. Scroll events are intercepted by JavaScript to drive the 3D camera/animation instead.

---

## 4. Animation System

### Preloader
- **3 concentric SVG rings** spinning at different speeds and directions
- Outer ring: 4.8s forward, 65% opacity
- Middle ring: 3.2s reverse, 85% opacity  
- Inner ring: 2s forward, 100% opacity
- All stroked with `#5EF6FF` (electric cyan)
- Loading text pulses with `pulse-fade` (3s ease-in-out, fading in/out)
- Text has `text-shadow: 0 0 8px rgba(94,246,255,0.5)` — cyan glow

### Glitch/Cyberpunk Button Animations (the "VISIT PROJECT" CTA)
This is the most complex CSS animation on the site — **6 layered keyframe animations running simultaneously:**

1. **`modal-link-idle-jitter`** (1.05s, stepped) — Random position jitter + chromatic text-shadow shifts
2. **`modal-link-idle-flicker`** (1.15s, stepped) — Opacity fluctuations simulating a broken CRT
3. **`modal-link-idle-break`** (2.2s, stepped) — `clip-path` polygon distortion + hue-rotate + letter-spacing shifts
4. **`modal-link-chaos-drift`** (4s, stepped) — Subtle random position/rotation drift
5. **`modal-link-scan`** (0.95s, stepped, on `::before`) — Scanline overlay animation
6. **`modal-link-glitch-slice`** (0.95s, stepped, on `::after`) — Horizontal glitch slice with color gradient

### Button Design (CTA)
```css
/* Sci-fi cut-corner polygon shape */
clip-path: polygon(12px 0, 100% 0, 100% calc(100% - 12px), calc(100% - 12px) 100%, 0 100%, 0 12px);

/* Gradient from deep dark to translucent blue */
background: linear-gradient(130deg, #0e0e17f5, #2770d452);

/* Chromatic aberration text shadow */
text-shadow: -1.5px 0 rgba(247,80,73,0.45), 1.5px 0 rgba(39,112,212,0.5);

/* Backdrop glass effect */
backdrop-filter: blur(6px);
```

### Close Button
- 80×80px square with `backdrop-filter: blur(5px)`
- Subtle `#5EF6FF` border at 10% opacity
- Corner accent marks (12px triangles) using `::before` and `::after` pseudo-elements
- Hover: scales, glows cyan

---

## 5. 3D Scene Architecture (Three.js / WebGL)

### Phase 0 — Hero Landing
- **Deep space starfield** background with particle stars
- **Giant 3D spiral/tentacle** in the center — organic, dark, iridescent metallic material with chromatic reflections (blue, red, green highlights)
- **Hero typography** — "SIMONE" at top and "ANDREOTTI" at bottom in massive neon-outlined futuristic type (Nexokora font rendered as 3D geometry or CSS with glow)
- **"EXPLORE" button** — positioned left-center inside a glowing red planet/orb with orbital ring
- **Social links** — right side, 3 floating spheres (Linktree, LinkedIn, GitHub) with concentric ring animations, connected by a vertical line

### Phase 1 — Spiral Card Gallery (scroll-driven)
- Scrolling triggers the camera to zoom into the spiral
- **Cards are 3D planes** arranged in a spiral formation around a **glowing golden/cyan sphere** (the "sun" at the center)
- Cards have **video textures** playing on their surfaces (Matrix rain, Tarot cards, Minigame, etc.)
- As you scroll, the focused card rotates to face the camera
- **Tag ticker** appears on the left side — vertically scrolling repeated category labels ("INFO", "LAB", "WORK", "STACK")

### Phase 2 — Card Modal
- Clicking a card opens it: the card title appears as a huge headline (white, Syncopate-Bold uppercase)
- The 3D sphere zooms to fill the background with a cyan/golden glow
- Description text + "VISIT PROJECT" glitch button + close button appear below

### Return to Phase 0
- After scrolling through all cards, the user returns to Phase 0 with an **alternate message** ("Here again? Did you like this?") — tracked with session state

---

## 6. Key CSS Techniques Used

| Technique | Where |
|-----------|-------|
| `clamp()` fluid typography | All text sizes |
| `clip-path: polygon()` | Sci-fi angular button shapes |
| `backdrop-filter: blur()` | Glass-effect modals, buttons, close buttons |
| `text-shadow` (chromatic aberration) | Glitch text effects with red/blue splits |
| `mix-blend-mode: screen` | Glitch light-streak overlays |
| `steps()` timing function | All glitch animations — creates discrete/stepped visual artifacts |
| `repeating-linear-gradient` | CRT scanline overlays |
| `filter: hue-rotate() contrast() saturate()` | Dynamic color shifting during glitch |
| `will-change: transform` | Canvas GPU acceleration |
| OffscreenCanvas + Web Worker | Rendering pipeline moved off main thread |
| `overscroll-behavior: none` | Prevents browser bounce/pull-to-refresh |
| `user-select: none` | Prevents text selection (immersive experience) |
| Service Worker | Offline caching / PWA capability |
| `perspective: 1000px` | 3D rendering context for CSS transforms |

---

## 7. Interaction Patterns

| Interaction | Behavior |
|-------------|----------|
| **Scroll** | Drives 3D camera movement (scroll-jacking), transitions between phases |
| **Click card** | Opens modal with zoom animation on the central sphere |
| **Close button** | Dismisses modal, returns to spiral view |
| **Hover on CTA** | Intensifies glitch effect, changes border to yellow (#FEE801) |
| **Return visit** | Different hero text messages (session-aware via localStorage/cookies) |
| **Social links** | 3D orb buttons with ring animations on hover |

---

## 8. Responsive Breakpoints

| Breakpoint | Grid | Behavior |
|------------|------|----------|
| ≥1700px | 12-col | Max container ~1586px |
| ≥1200px | 12-col | Standard desktop |
| ≤992px | 8-col | Tablet — mobile grid, tag ticker moves to top |
| ≤768px | 8-col | Smaller tablet |
| ≤576px | 4-col | Mobile — minimal gaps (12px) |

---

## 9. Tech Stack (from source)

- **Framework**: Astro v5.16.0 (SSG)
- **3D Engine**: Three.js (rendered via OffscreenCanvas + Web Worker)
- **Hosting**: Cloudflare Pages (CF beacon, email protection, CDN)
- **PWA**: Service Worker with persistent storage
- **Fonts**: Self-hosted custom .ttf/.otf files

---

## 10. Design DNA Summary

> **In one line**: A dark cyberpunk space portfolio where a 3D spiral unfurls into project cards, wrapped in CRT-glitch aesthetics with electric cyan accents.

### The "vibe" is built from:
1. **Ultra-dark background** (#0E0E17) — nearly void-black with a hint of blue
2. **Electric cyan** (#5EF6FF) as the sole hero accent — used for glow, borders, text, icons
3. **Stepped/discrete glitch animations** — `steps(2, end)` or `steps(3, end)` — never smooth, always digital/broken
4. **Chromatic aberration** — red-left / blue-right text shadow splits
5. **CRT scanlines** — repeating-linear-gradient overlays
6. **Cut-corner polygonal shapes** — clip-path for buttons giving a sci-fi HUD aesthetic
7. **Uppercase wide-tracked typography** — Syncopate-Bold with tight negative letter-spacing
8. **Glassmorphism** — backdrop-filter: blur() on interactive overlays
9. **Full-screen 3D immersion** — no traditional page scrolling, camera movement instead
10. **Session memory** — returning users see different messages

This is NOT a traditional CSS website — it's a **WebGL-first experience** with CSS used only for the 2D HUD overlay (preloader, modal, tag ticker, buttons). The main visual content is entirely Three.js rendered.
