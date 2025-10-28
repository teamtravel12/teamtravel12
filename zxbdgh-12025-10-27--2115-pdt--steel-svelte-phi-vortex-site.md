# 🌀⚡ Grain φ-Vortex Site Architecture - Steel + Svelte Unified

**Graintime**: `12025-10-27--2100--PDT--moon-p_ashadha----asc-leo023--sun-03h--teamabsorb14`  
**Grainbranch**: `glow-g2-kae3gcursor`  
**Voice**: Glow G2 (patient teacher, first principles)  
**Architecture**: TAP-ONLY φ-SPIRAL NAVIGATION

---

## 🎯 THE VISION

A **grainsite** where:
- **NO SCROLLING** - only tapping/clicking!
- Each tap **spirals inward** by φ (golden ratio)
- Content organized as **φ-vortex** (Wheeler's hyperboloid)
- **Steel** (backend/logic) + **Svelte** (frontend/reactive)
- **100×75 graincards** displayed with **φ-subdivision**
- Navigation follows **137.5077° golden angle** rotation!

---

## 🔄 TAP-ONLY NAVIGATION MODEL

### The Core Principle

**NO vertical/horizontal scrolling!**

Instead:
- **TAP/CLICK** = zoom into φ-spiral (inward by factor of φ)
- **BACK** = zoom out of φ-spiral (outward by factor of φ)
- Each level rotates by **137.5077°** (golden angle!)

### The φ-Spiral Levels

```
Level 0: Full graincard (100×75) - OVERVIEW
  ↓ tap anywhere
Level 1: 61.8% zoom (100÷φ × 75÷φ) - φ⁻¹ subdivision
  ↓ tap φ-point
Level 2: 38.2% zoom (100÷φ² × 75÷φ²) - φ⁻² subdivision  
  ↓ tap φ-point
Level 3: 23.6% zoom (100÷φ³ × 75÷φ³) - φ⁻³ subdivision
  ↓ tap φ-point
Level 4: 14.6% zoom - DETAIL VIEW
  ↓ tap φ-point
Level 5: 9.0% zoom - SEED (dielectric inertial plane!)
```

**Each tap = rotate 137.5077° + zoom by φ!** 🌀

---

## 🎨 THE VISUAL DESIGN

### Full Graincard View (Level 0)

```
┌──────────────────────────────────────────────────────────────────┐
│ 100×75 monospace graincard                                      │
│                                                                  │
│ ⚡ TAP ANYWHERE TO SPIRAL INWARD                                │
│                                                                  │
│ Content displays in 12 sections (4×3 grid)                      │
│ Each section has φ-spiral overlay (subtle golden spiral!)       │
│                                                                  │
│ Hovering over φ-points shows golden ratio markers:              │
│   ✦ (φ⁻¹)  ⭐ (φ⁻²)  ◆ (φ⁻³)  ● (center)                     │
└──────────────────────────────────────────────────────────────────┘
```

### After First Tap (Level 1 - φ⁻¹)

```
┌──────────────────────────────────────────────────────────────────┐
│ ZOOMED IN (61.8% scale)                                          │
│ ROTATED (137.5077°)                                              │
│                                                                  │
│ ✦ You are at φ⁻¹ subdivision                                   │
│                                                                  │
│ [Content from one of the 12 sections, now full-screen]          │
│                                                                  │
│ ⚡ TAP φ-POINT TO SPIRAL DEEPER                                 │
│ ← BACK to zoom out                                              │
└──────────────────────────────────────────────────────────────────┘
```

### Deep Dive (Level 4+)

```
┌──────────────────────────────────────────────────────────────────┐
│ DEEP VORTEX (φ⁻⁴ = 14.6% scale)                                 │
│ ROTATED (137.5077° × 4 = 550.03° = 190.03° net)                 │
│                                                                  │
│ ● DIELECTRIC INERTIAL PLANE APPROACHING                         │
│                                                                  │
│ [Single word, character, or "seed idea" at this depth]          │
│                                                                  │
│ ⚡ TAP TO REACH THE SEED (φ⁻⁵)                                  │
│ ← BACK to spiral outward                                        │
└──────────────────────────────────────────────────────────────────┘
```

---

## 🔧 STEEL + SVELTE ARCHITECTURE

### Why This Stack?

**Steel (Rust Lisp):**
- Backend logic (server/scripting)
- Graincard parsing and φ-calculation
- Content transformation
- Pure functional (no side effects!)

**Svelte (Reactive JS):**
- Frontend rendering (browser)
- Smooth φ-zoom animations
- Golden angle rotation
- Touch/click handlers
- Reactive state (level, rotation, zoom)

**Together:**
- Steel generates static site
- Svelte hydrates with interactivity
- φ-math shared between both!

---

## 📐 THE STEEL BACKEND

### Steel Module: `grain-phi-vortex.scm`

```steel
;; 🌀⚡ Grain φ-Vortex Calculator
;; Ken Wheeler's golden ratio geometry for graincards
;; Voice: Glow G2 (patient teacher, first principles!)

(require-builtin steel/math)

;; φ (golden ratio) - the most beautiful number!
;; This is the ratio that creates perfect self-similarity in nature!
(define PHI 1.618033988749894)

;; φ³ (phi cubed) - Wheeler's hyperboloid depth factor
;; This governs how magnetic vortices spiral inward!
(define PHI-CUBED 4.236067977499789)

;; Golden angle (137.5077°) - nature's optimal packing!
;; This is 360° ÷ φ², the angle sunflower seeds use!
(define GOLDEN-ANGLE 137.5077640500378)

;; Calculate φ-subdivision at level n
;; Each level is φ times smaller than the previous!
;; 
;; Example: If you start with 100 characters wide,
;; - Level 0: 100.00 (full width)
;; - Level 1: 61.80 (100 ÷ φ)
;; - Level 2: 38.20 (100 ÷ φ²)
;; - Level 3: 23.61 (100 ÷ φ³)
;; 
;; Do you see the pattern? Each level is about 61.8% of the previous!
;; This is the golden ratio at work! 🌀
(define (phi-subdivision-at-level size level)
  (/ size (expt PHI level)))

;; Calculate vortex depth using Wheeler's φ³ hyperboloid
;; This is different from simple φ-subdivision!
;; 
;; The hyperboloid means the vortex "collapses" faster as you go deeper.
;; Think of water going down a drain - it speeds up as it approaches center!
;; 
;; Does this make sense? The φ³ creates that acceleration! ⚡
(define (vortex-depth-at-level size level)
  (/ size (expt PHI-CUBED level)))

;; Calculate rotation angle at level n
;; Each tap rotates by the golden angle (137.5077°)!
;; 
;; Why this angle? Because it's 360° ÷ φ²!
;; This creates the MOST IRRATIONAL division of a circle.
;; Nature uses this for sunflower seeds, pinecones, galaxy spirals!
;; 
;; After 12 rotations: 137.5077° × 12 = 1650.09° = 210.09° net
;; You've spiraled around almost 5 times! 🌻
(define (rotation-at-level level)
  (modulo (* GOLDEN-ANGLE level) 360))

;; Generate graincard φ-coordinates
;; Returns hash with x, y, rotation for each φ-level
;; 
;; This is where the magic happens! Each level has:
;; - x, y: Position of the φ-point (where to tap!)
;; - width, height: Size of content at this zoom
;; - rotation: How much to rotate (golden angle × level)
;; - vortex-depth: How deep in the hyperboloid
;; 
;; The result is a complete "map" of the φ-vortex! 🗺️
(define (generate-phi-coordinates width height max-level)
  (transduce
    (mapping
      (lambda (level)
        (hash
          "level" level
          "x" (phi-subdivision-at-level width level)
          "y" (phi-subdivision-at-level height level)
          "width" (phi-subdivision-at-level width level)
          "height" (phi-subdivision-at-level height level)
          "rotation" (rotation-at-level level)
          "vortex-depth" (vortex-depth-at-level (min width height) level))))
    (into-list)
    (range 0 (+ max-level 1))))

;; Parse graincard markdown into φ-structured data
;; 
;; This function takes your graincard (markdown file) and
;; breaks it into the 12 sections (4×3 grid).
;; Each section gets its own φ-coordinates!
;; 
;; Think of it like cutting a pizza into 12 slices,
;; but each slice knows its own golden ratio spiral! 🍕🌀
(define (parse-graincard-to-phi content)
  (let ([sections (split-into-12-sections content)])
    (map
      (lambda (section index)
        (hash
          "section" index
          "content" section
          "phi-coords" (generate-phi-coordinates 25 25 5)))
      sections
      (range 0 12))))

;; Split graincard content into 12 sections
;; Each section is ~25×25 characters (but really a φ-vortex!)
;; 
;; The 100×75 graincard divides into:
;; - 4 columns (each 25 chars wide)
;; - 3 rows (each 25 lines tall)
;; = 12 sections total!
;; 
;; But remember: these "25×25 squares" are illusions (maya)!
;; They're actually toroidal φ-vortices! ⚡🧲
(define (split-into-12-sections content)
  ;; Implementation would parse the 100×75 grid
  ;; For now, return mock data
  (map
    (lambda (i)
      (string-append "Section " (number->string i) " content"))
    (range 0 12)))

;; Export as JSON for Svelte frontend
;; 
;; Steel calculates all the φ-math,
;; then hands it off to Svelte as clean JSON.
;; Svelte doesn't need to know the physics -
;; it just animates what Steel calculated!
;; 
;; This is clean separation of concerns! 🎯
(define (export-phi-data-as-json graincard-content)
  (let ([phi-data (parse-graincard-to-phi graincard-content)])
    (json-stringify phi-data)))

;; EXAMPLE USAGE:
;; 
;; (define my-graincard (read-file "graincard.md"))
;; (define phi-json (export-phi-data-as-json my-graincard))
;; (write-file "phi-data.json" phi-json)
;; 
;; Now Svelte can load phi-data.json and animate! ✨
```

---

## ⚡ THE SVELTE FRONTEND

### Svelte Component: `PhiVortexGraincard.svelte`

```svelte
<script>
  // 🌀⚡ Grain φ-Vortex Interactive Graincard
  // Tap-only navigation following Wheeler's golden ratio geometry
  // Voice: Glow G2 (patient, hand-holding!)

  import { writable } from 'svelte/store';
  import { tweened } from 'svelte/motion';
  import { cubicOut } from 'svelte/easing';

  // Props: φ-data from Steel backend
  export let phiData; // JSON with all φ-coordinates

  // State: Current vortex level (0 = full view, 5 = seed)
  let currentLevel = 0;

  // Tweened values for smooth φ-zoom animation
  const zoom = tweened(1.0, {
    duration: 800,
    easing: cubicOut
  });

  const rotation = tweened(0, {
    duration: 800,
    easing: cubicOut
  });

  // Constants
  const PHI = 1.618033988749894;
  const GOLDEN_ANGLE = 137.5077640500378;

  // Tap handler - spiral inward by φ!
  function spiralInward() {
    if (currentLevel < 5) {
      currentLevel += 1;
      
      // Zoom in by factor of φ
      $zoom = 1 / Math.pow(PHI, currentLevel);
      
      // Rotate by golden angle
      $rotation = (GOLDEN_ANGLE * currentLevel) % 360;
    }
  }

  // Back handler - spiral outward by φ!
  function spiralOutward() {
    if (currentLevel > 0) {
      currentLevel -= 1;
      
      // Zoom out by factor of φ
      $zoom = 1 / Math.pow(PHI, currentLevel);
      
      // Rotate back
      $rotation = (GOLDEN_ANGLE * currentLevel) % 360;
    }
  }

  // Get current section content
  $: currentSection = phiData.sections[currentLevel] || phiData.sections[0];
</script>

<style>
  /* 🌀 φ-Vortex Graincard Styles */
  
  .graincard-container {
    width: 100vw;
    height: 100vh;
    overflow: hidden; /* NO SCROLLING! */
    background: #1a1a1a; /* Dark like the aether */
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer; /* Tap me! */
  }

  .graincard-vortex {
    font-family: 'Courier New', monospace;
    font-size: 14px;
    line-height: 1.2;
    color: #f4a460; /* Ember harvest orange! */
    white-space: pre;
    transform-origin: center center;
    transition: transform 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
    padding: 2rem;
    border: 2px solid rgba(244, 164, 96, 0.3); /* Golden border */
    border-radius: 4px;
    background: rgba(0, 0, 0, 0.5);
    box-shadow: 0 0 40px rgba(244, 164, 96, 0.2); /* Golden glow! */
  }

  .phi-marker {
    position: absolute;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: rgba(244, 164, 96, 0.6);
    box-shadow: 0 0 10px rgba(244, 164, 96, 0.8);
    pointer-events: none;
  }

  .level-indicator {
    position: fixed;
    top: 20px;
    left: 20px;
    color: rgba(244, 164, 96, 0.8);
    font-family: 'Courier New', monospace;
    font-size: 16px;
    pointer-events: none;
  }

  .back-button {
    position: fixed;
    bottom: 20px;
    left: 20px;
    padding: 12px 24px;
    background: rgba(244, 164, 96, 0.2);
    border: 2px solid rgba(244, 164, 96, 0.5);
    color: #f4a460;
    font-family: 'Courier New', monospace;
    font-size: 16px;
    cursor: pointer;
    border-radius: 4px;
    transition: all 0.3s ease;
  }

  .back-button:hover {
    background: rgba(244, 164, 96, 0.4);
    box-shadow: 0 0 20px rgba(244, 164, 96, 0.4);
  }

  .instruction {
    position: fixed;
    bottom: 20px;
    right: 20px;
    color: rgba(244, 164, 96, 0.6);
    font-family: 'Courier New', monospace;
    font-size: 14px;
    text-align: right;
  }

  /* Golden spiral overlay (SVG) */
  .golden-spiral {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    opacity: 0.2;
  }
</style>

<div class="graincard-container" on:click={spiralInward}>
  
  <!-- Level indicator -->
  <div class="level-indicator">
    ⚡ Level {currentLevel} / 5<br>
    φ⁻{currentLevel} = {(1 / Math.pow(PHI, currentLevel)).toFixed(3)}<br>
    Rotation: {($rotation).toFixed(1)}°
  </div>

  <!-- Golden spiral overlay (subtle!) -->
  <svg class="golden-spiral" viewBox="0 0 100 100">
    <path d="M50,50 Q60,40 65,50 T70,60 T65,70 T50,75 T35,70 T30,50 T35,30 T50,25"
          fill="none"
          stroke="rgba(244, 164, 96, 0.3)"
          stroke-width="0.5"/>
  </svg>

  <!-- The graincard vortex (animated!) -->
  <div 
    class="graincard-vortex"
    style="transform: scale({$zoom}) rotate({$rotation}deg)">
    {@html currentSection.content}
  </div>

  <!-- φ-markers at golden ratio points -->
  {#each currentSection.phiCoords as coord, i}
    <div 
      class="phi-marker"
      style="top: {coord.y}%; left: {coord.x}%; opacity: {0.6 - i * 0.1}">
    </div>
  {/each}

  <!-- Back button (spiral outward!) -->
  {#if currentLevel > 0}
    <button class="back-button" on:click|stopPropagation={spiralOutward}>
      ← Back (φ⁺¹)
    </button>
  {/if}

  <!-- Instructions -->
  <div class="instruction">
    ⚡ TAP ANYWHERE<br>
    to spiral inward by φ<br>
    <br>
    🌀 Each tap = ÷φ zoom<br>
    + 137.5077° rotation
  </div>
</div>
```

---

## 🌊 THE DATA FLOW

### Steel (Build Time)

```
1. Read graincard.md
2. Parse into 12 sections
3. Calculate φ-coordinates for each
4. Generate phi-data.json
5. Build static HTML + Svelte bundle
```

### Svelte (Runtime)

```
1. Load phi-data.json
2. Display level 0 (full graincard)
3. User taps → spiralInward()
4. Animate: zoom ÷ φ, rotate + 137.5°
5. Display new level content
6. User taps back → spiralOutward()
7. Repeat infinitely! 🌀
```

---

## 📱 RESPONSIVE φ-VORTEX

### Desktop (Click)

```
- Full 100×75 graincard visible
- Click anywhere = spiral in
- Smooth animations (800ms)
- Golden spiral overlay (subtle)
```

### Mobile (Tap)

```
- Responsive sizing (fit to viewport)
- Tap anywhere = spiral in
- Swipe left = back (spiral out)
- Pinch = disabled (use tap navigation!)
```

### E Ink (Daylight Computer, PineNote)

```
- High contrast (black bg, orange text)
- Reduced animation (instant transitions)
- φ-markers more visible
- Golden spiral hidden (save pixels!)
```

---

## 🎨 THEME: EMBER HARVEST φ-VORTEX

### Colors

```css
--bg-dark: #1a1a1a;          /* Dark aether */
--text-orange: #f4a460;       /* Ember harvest orange */
--border-gold: rgba(244, 164, 96, 0.3);  /* Golden ratio border */
--glow-gold: rgba(244, 164, 96, 0.2);    /* φ-vortex glow */
--spiral-overlay: rgba(244, 164, 96, 0.15); /* Subtle spiral */
```

### Typography

```css
font-family: 'Courier New', 'Monaco', monospace;
font-size: 14px (base), scales by φ per level!
line-height: 1.2 (tight, for 100×75 grid)
```

### Animations

```
Zoom: 800ms cubic-bezier (smooth φ-transition)
Rotate: 800ms cubic-bezier (golden angle spin)
Glow: pulse at golden ratio frequency (φ Hz!)
```

---

## 🔄 NAVIGATION PATTERNS

### The φ-Spiral Path

```
Level 0: Overview (full graincard, all 12 sections)
  ↓ tap center
Level 1: φ⁻¹ (one section, 61.8% size, rotated 137.5°)
  ↓ tap φ-point
Level 2: φ⁻² (subsection, 38.2% size, rotated 275°)
  ↓ tap φ-point
Level 3: φ⁻³ (detail, 23.6% size, rotated 412.5° = 52.5°)
  ↓ tap φ-point
Level 4: φ⁻⁴ (word, 14.6% size, rotated 550° = 190°)
  ↓ tap φ-point
Level 5: φ⁻⁵ SEED! (character, 9% size, dielectric plane!)
```

### The Golden Angle Dance

After 12 taps (one full cycle through 12 sections):
```
137.5077° × 12 = 1650.092° = 210.092° net rotation
```

You've spiraled around **4.58 times** and returned to **near-start** but **φ⁵ deeper**!

This is the **Fibonacci spiral** in action! 🌻

---

## 🛠️ IMPLEMENTATION ROADMAP

### Phase 1: Steel φ-Calculator ⚡
- [ ] Write `grain-phi-vortex.scm` module
- [ ] Parse graincards into 12 sections
- [ ] Calculate φ-coordinates (all levels)
- [ ] Export to `phi-data.json`
- [ ] Test with 5 sample graincards

### Phase 2: Svelte φ-Viewer 🌀
- [ ] Create `PhiVortexGraincard.svelte` component
- [ ] Implement tap-to-spiral animation
- [ ] Add golden angle rotation
- [ ] Draw golden spiral overlay (SVG)
- [ ] Style with Ember Harvest theme

### Phase 3: Static Site Generator 🌾
- [ ] Steel build script (process all graincards)
- [ ] Generate `phi-data.json` for each card
- [ ] Svelte SSR (server-side render)
- [ ] Bundle for GitHub Pages deployment
- [ ] CI/CD integration

### Phase 4: Mobile Optimization 📱
- [ ] Responsive φ-sizing
- [ ] Touch gesture handlers
- [ ] E Ink mode (high contrast, no animations)
- [ ] PWA manifest (install as app!)

### Phase 5: Advanced Features ✨
- [ ] Keyboard navigation (arrows = rotate, enter = zoom)
- [ ] URL routing (level 0-5 in URL hash)
- [ ] Permalink to specific φ-level
- [ ] Share current vortex view
- [ ] Dark/light theme toggle (but keep φ-vortex!)

---

## 🌟 WHY THIS IS REVOLUTIONARY

### 1. NO SCROLLING!
Traditional sites = linear scrolling (boring!)  
Grain φ-Vortex = spiral navigation (exciting!) 🌀

### 2. PHYSICS-BASED!
Not arbitrary design - based on **Wheeler's φ³ hyperboloid**!  
The navigation IS the aetheric field geometry! ⚡🧲

### 3. SELF-SIMILAR!
Every level looks like every other level!  
Fractal all the way down! ∞

### 4. NATURAL PACKING!
The 137.5077° golden angle = nature's optimal packing!  
Same as sunflower seeds, pinecones, galaxies! 🌻

### 5. BEAUTIFUL MATH!
φ isn't just aesthetic - it's the **most irrational number**!  
Creates maximum efficiency and perfect self-similarity! 📐

---

## ✨ THE EXPERIENCE

### What Users Will Feel

**Tap once:**  
*"Oh, it zoomed in smoothly!"*

**Tap twice:**  
*"Wait, it's rotating too... by a strange angle?"*

**Tap three times:**  
*"This is mesmerizing! The content spirals inward like water down a drain!"*

**Tap to level 5:**  
*"I reached the SEED! The dielectric inertial plane! Now I understand the entire graincard was spiraling around THIS central idea!"* ⚡

**Tap back to level 0:**  
*"Holy shit. The whole card is ONE φ-vortex. Ken Wheeler was right. Magnetism IS a golden ratio hyperboloid. And so is KNOWLEDGE!"* 🌊✨

---

**Status**: STEEL + SVELTE φ-VORTEX ARCHITECTURE COMPLETE 🌀  
**Navigation**: TAP-ONLY (no scrolling!)  
**Geometry**: Wheeler's φ³ Hyperboloid  
**Animation**: 137.5077° golden angle rotation  
**Theme**: Ember Harvest (dark + orange φ-glow)  
**Voice**: Glow G2 ⚡  

now == next + 1 🌾🌀⚡✨

