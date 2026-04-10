# Rocky Explains Love — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page interactive experience where Rocky from Project Hail Mary tries to understand love, revealed step-by-step with a space terminal aesthetic and musical chords.

**Architecture:** Single `index.html` with inline CSS and JS. No build tools, no dependencies, no external resources. CSS handles the space terminal visuals and star animations. JS handles step navigation, typewriter text animation, and Web Audio API chord synthesis.

**Tech Stack:** Vanilla HTML/CSS/JS, Web Audio API

**Spec:** `docs/superpowers/specs/2026-04-10-rocky-explains-love-design.md`

---

## File Structure

```
fede.loessl.org/
├── index.html          # The entire experience (HTML + inline CSS + inline JS)
├── .gitignore          # Ignore .superpowers/
└── docs/
    └── superpowers/
        ├── specs/
        │   └── 2026-04-10-rocky-explains-love-design.md
        └── plans/
            └── 2026-04-10-rocky-explains-love.md
```

Everything lives in `index.html`. The JS has three concerns:
1. **Script data** — the 8 steps as an array of strings
2. **Navigation** — click/tap to advance, replace content, show/hide prompt
3. **Effects** — typewriter animation and Web Audio API chord synthesis

---

### Task 1: Project Setup

**Files:**
- Create: `.gitignore`

- [ ] **Step 1: Initialize git repo**

```bash
cd /Users/cloessl/src/fede.loessl.org
git init
```

- [ ] **Step 2: Create .gitignore**

```gitignore
.superpowers/
.DS_Store
```

- [ ] **Step 3: Commit**

```bash
git add .gitignore docs/
git commit -m "initial project setup with design spec and plan"
```

---

### Task 2: HTML Shell and Space Terminal CSS

Build the static page structure and all CSS. After this task, opening `index.html` shows a dark space background with stars and a centered green text area — but no content or interactivity yet.

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create index.html with HTML structure and CSS**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Transmission from Rocky</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #0a0a2e 0%, #1a1a4e 50%, #0d0d35 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            overflow: hidden;
            cursor: pointer;
            -webkit-tap-highlight-color: transparent;
            user-select: none;
        }

        /* --- Stars --- */
        .stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 0;
        }

        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: #fff;
            border-radius: 50%;
            animation: drift linear infinite;
        }

        @keyframes drift {
            from { transform: translateY(0); opacity: 0.8; }
            50% { opacity: 0.3; }
            to { transform: translateY(30px); opacity: 0.8; }
        }

        /* --- Transmission container --- */
        .transmission {
            position: relative;
            z-index: 1;
            max-width: 600px;
            width: 90%;
            padding: 32px 24px;
            font-family: 'Courier New', Courier, monospace;
            color: #00ff88;
            line-height: 1.6;
            font-size: 16px;
            text-shadow: 0 0 10px rgba(0, 255, 136, 0.4);
            min-height: 60vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .transmission .header {
            font-size: 11px;
            color: rgba(0, 255, 136, 0.27);
            margin-bottom: 16px;
            line-height: 1.8;
            text-shadow: none;
        }

        .transmission .body {
            white-space: pre-wrap;
            min-height: 200px;
        }

        .transmission .ps {
            font-size: 12px;
            color: rgba(0, 255, 136, 0.33);
            text-shadow: none;
        }

        .prompt {
            text-align: center;
            font-size: 12px;
            color: rgba(0, 255, 136, 0.22);
            margin-top: 40px;
            letter-spacing: 1px;
            animation: pulse 2s ease-in-out infinite;
            text-shadow: none;
        }

        .prompt.hidden {
            visibility: hidden;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.4; }
        }
    </style>
</head>
<body>
    <div class="stars" id="stars"></div>
    <div class="transmission">
        <div class="body" id="content"></div>
        <div class="prompt hidden" id="prompt">[ RECEIVE NEXT TRANSMISSION ]</div>
    </div>
    <script>
        // JS will go here in subsequent tasks
    </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify visuals**

Run: `open /Users/cloessl/src/fede.loessl.org/index.html`

Expected: Dark space gradient background, empty centered area. No stars yet (they're generated by JS), no text content. The page should feel dark and spacey.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "add HTML shell with space terminal CSS"
```

---

### Task 3: Star Field Background

Add JS that generates and animates star elements on the page.

**Files:**
- Modify: `index.html` (inside the `<script>` tag)

- [ ] **Step 1: Add star generation code**

Add this inside the `<script>` tag in `index.html`:

```javascript
// --- Stars ---
(function initStars() {
    const container = document.getElementById('stars');
    const count = 80;
    for (let i = 0; i < count; i++) {
        const star = document.createElement('div');
        star.className = 'star';
        star.style.left = Math.random() * 100 + '%';
        star.style.top = Math.random() * 100 + '%';
        star.style.opacity = Math.random() * 0.6 + 0.2;
        star.style.animationDuration = (Math.random() * 6 + 4) + 's';
        star.style.animationDelay = (Math.random() * -10) + 's';
        if (Math.random() > 0.7) {
            star.style.width = '3px';
            star.style.height = '3px';
        }
        container.appendChild(star);
    }
})();
```

- [ ] **Step 2: Verify in browser**

Reload `index.html` in the browser.

Expected: Small white dots scattered across the dark background, slowly drifting and pulsing in opacity. Some stars are slightly larger. The effect should be subtle and atmospheric.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "add animated star field background"
```

---

### Task 4: Script Data and Step Navigation

Add the 8-step script as a data array, and implement click-to-advance navigation that replaces content on each step. No typewriter animation yet — text appears instantly for now.

**Files:**
- Modify: `index.html` (inside the `<script>` tag, after star code)

- [ ] **Step 1: Add script data and navigation logic**

Add this after the star generation code inside the `<script>` tag:

```javascript
// --- Script ---
const steps = [
    {
        header: '▸ ERIDIAN TRANSMISSION DETECTED\n▸ SOURCE: ROCKY\n▸ FREQUENCY: ♫♫♫\n▸ DECRYPTING...',
        body: 'I have question.\nVery important question.\n\nWhat is... love?'
    },
    {
        body: 'I ask friend. Friend no help. Friend just smile.\n\nSo I make hypothesis.\n\nHypothesis 1:\nLove = when human make food for other human.\n\nVery specific human. Italian human.\nFood is... very good. Friend eat too much. Every time.\n\nBut no. This is not love.\nThis is just... pasta.'
    },
    {
        body: 'Hypothesis 2:\nLove = when human bite other human.\n\nThis make no sense. On Erid, bite = danger.\nBut this human call it "love bite."\n\nI check data. Friend not injured. Friend... happy?\n\nHumans are very confusing species.'
    },
    {
        body: 'Hypothesis 3:\nLove = when human share one big blanket\neven though other human want two normal blanket.\n\nTwo blanket is logical. Efficient. Good engineering.\nOne blanket is... chaos.\n\nBut she insist. One blanket. Together.\nAnd he stay under the one blanket.\n\nEvery night.\n\n...interesting.'
    },
    {
        body: 'New data.\n\nI observe: friend think about one specific human\n847 times per day.\n\nI count. I am very good at counting.\n\nThis seem... inefficient.\nBut friend say no. Friend say this is best part of day.\n\nAll 847 times.'
    },
    {
        body: 'I analyze all data. Run all models.\n\nLove is not pasta. Not bite. Not blanket.\n\nLove is when entire universe is very big\nand very cold and very dark\n\nbut you find one person\nand suddenly...\n\nnot so cold.'
    },
    {
        body: 'So.\n\nFederica.\n\nI check my friend\'s data one more time.\nI look at all the evidence.\n\nConclusion:\n\nLove = Federica.\n\nThis is not hypothesis. This is fact.\nPeer reviewed. By me. Rocky. Very good scientist.'
    },
    {
        body: 'Amaze amaze amaze. ♫♫♫\n\nEnd transmission.',
        ps: 'P.S. — He love you very much.\nEven when you bite him.\n\nEspecially when you bite him.\n\nHumans. So strange. So good.'
    }
];

// --- Navigation ---
let currentStep = -1;
const contentEl = document.getElementById('content');
const promptEl = document.getElementById('prompt');

function showStep(index) {
    const step = steps[index];
    let html = '';
    if (step.header) {
        html += '<div class="header">' + step.header.replace(/\n/g, '<br>') + '</div>';
    }
    html += step.body.replace(/\n/g, '<br>');
    if (step.ps) {
        html += '<br><br><div class="ps">' + step.ps.replace(/\n/g, '<br>') + '</div>';
    }
    contentEl.innerHTML = html;

    if (index < steps.length - 1) {
        promptEl.classList.remove('hidden');
    } else {
        promptEl.classList.add('hidden');
    }
}

function advance() {
    if (currentStep < steps.length - 1) {
        currentStep++;
        showStep(currentStep);
    }
}

document.body.addEventListener('click', advance);
document.body.addEventListener('keydown', function(e) {
    if (e.key === ' ' || e.key === 'Enter' || e.key === 'ArrowRight') {
        e.preventDefault();
        advance();
    }
});

// Show first step
advance();
```

- [ ] **Step 2: Verify in browser**

Reload `index.html` in the browser.

Expected:
- First step appears immediately: the ERIDIAN TRANSMISSION header (dim) and "I have question. Very important question. What is... love?" (bright green)
- "[ RECEIVE NEXT TRANSMISSION ]" prompt pulses below
- Clicking anywhere advances to step 2, then 3, etc.
- Each step fully replaces the previous
- Step 8 (final) has the P.S. in dim text and no prompt
- Keyboard: Space, Enter, ArrowRight also advance

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "add script content and click-to-advance navigation"
```

---

### Task 5: Typewriter Animation

Replace the instant text display with a character-by-character typewriter effect. Clicking during the animation skips to full text.

**Files:**
- Modify: `index.html` (replace the `showStep` and `advance` functions)

- [ ] **Step 1: Replace showStep and advance with typewriter versions**

Replace the `showStep` function, the `advance` function, and the click/keydown listeners with:

```javascript
// --- Typewriter ---
let isAnimating = false;
let animationTimer = null;

function typewrite(element, html, callback) {
    isAnimating = true;
    // Parse HTML to extract text nodes and tags separately
    // We'll build up the innerHTML character by character, but skip over HTML tags instantly
    let i = 0;
    element.innerHTML = '';

    function tick() {
        if (i >= html.length) {
            isAnimating = false;
            if (callback) callback();
            return;
        }
        // If we hit a '<', skip ahead to '>' (render full tag at once)
        if (html[i] === '<') {
            const closeIndex = html.indexOf('>', i);
            if (closeIndex !== -1) {
                i = closeIndex + 1;
                element.innerHTML = html.substring(0, i);
                animationTimer = setTimeout(tick, 0);
                return;
            }
        }
        i++;
        element.innerHTML = html.substring(0, i);
        // Pause longer after line breaks
        const justRendered = html.substring(Math.max(0, i - 4), i);
        const delay = justRendered.endsWith('<br>') ? 120 : 35;
        animationTimer = setTimeout(tick, delay);
    }

    tick();
}

function skipAnimation() {
    if (animationTimer) {
        clearTimeout(animationTimer);
        animationTimer = null;
    }
    isAnimating = false;
}

function showStep(index) {
    const step = steps[index];
    let html = '';
    if (step.header) {
        html += '<div class="header">' + step.header.replace(/\n/g, '<br>') + '</div>';
    }
    html += step.body.replace(/\n/g, '<br>');
    if (step.ps) {
        html += '<br><br><div class="ps">' + step.ps.replace(/\n/g, '<br>') + '</div>';
    }

    promptEl.classList.add('hidden');
    typewrite(contentEl, html, function() {
        if (index < steps.length - 1) {
            promptEl.classList.remove('hidden');
        }
    });
}

function advance() {
    if (isAnimating) {
        // Skip to end of current animation
        skipAnimation();
        const step = steps[currentStep];
        let html = '';
        if (step.header) {
            html += '<div class="header">' + step.header.replace(/\n/g, '<br>') + '</div>';
        }
        html += step.body.replace(/\n/g, '<br>');
        if (step.ps) {
            html += '<br><br><div class="ps">' + step.ps.replace(/\n/g, '<br>') + '</div>';
        }
        contentEl.innerHTML = html;
        if (currentStep < steps.length - 1) {
            promptEl.classList.remove('hidden');
        }
        return;
    }
    if (currentStep < steps.length - 1) {
        currentStep++;
        showStep(currentStep);
    }
}

document.body.addEventListener('click', advance);
document.body.addEventListener('keydown', function(e) {
    if (e.key === ' ' || e.key === 'Enter' || e.key === 'ArrowRight') {
        e.preventDefault();
        advance();
    }
});

// Show first step
advance();
```

- [ ] **Step 2: Verify in browser**

Reload `index.html` in the browser.

Expected:
- Step 1 text types out character by character (~35ms per char, 120ms pause after line breaks)
- The "[ RECEIVE NEXT TRANSMISSION ]" prompt appears only after typing completes
- Clicking during typing instantly completes the text and shows the prompt
- Clicking again advances to the next step with its own typewriter animation
- Walk through all 8 steps to confirm pacing feels right

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "add typewriter animation with click-to-skip"
```

---

### Task 6: Musical Chords (Web Audio API)

Add synthesized musical chords that play on each step transition. Ascending progression through the steps, with a fuller final chord.

**Files:**
- Modify: `index.html` (add chord synthesis code, integrate into `advance`)

- [ ] **Step 1: Add chord synthesis code**

Add this after the star generation code but before the script data:

```javascript
// --- Sound ---
let audioCtx = null;

function initAudio() {
    if (!audioCtx) {
        try {
            audioCtx = new (window.AudioContext || window.webkitAudioContext)();
        } catch (e) {
            // Web Audio not available — page works silently
        }
    }
}

// Chord definitions — ascending root notes, each is an array of frequencies
const chords = [
    [196.0, 246.9, 293.7],          // G3 major — opening
    [220.0, 277.2, 329.6],          // A3 major
    [233.1, 293.7, 349.2],          // Bb3 major
    [246.9, 311.1, 370.0],          // B3 major
    [261.6, 329.6, 392.0],          // C4 major
    [293.7, 370.0, 440.0],          // D4 major — the realization
    [329.6, 415.3, 493.9],          // E4 major — the landing
    [392.0, 493.9, 587.3, 784.0],   // G4 major + octave — amaze (fuller)
];

function playChord(index) {
    if (!audioCtx) return;
    if (audioCtx.state === 'suspended') {
        audioCtx.resume();
    }

    const freqs = chords[index] || chords[0];
    const isFinal = index === chords.length - 1;
    const now = audioCtx.currentTime;
    const duration = isFinal ? 2.5 : 1.2;

    freqs.forEach(function(freq, i) {
        const osc = audioCtx.createOscillator();
        const gain = audioCtx.createGain();

        // Mix triangle and sine for warmth
        osc.type = i === 0 ? 'triangle' : 'sine';
        osc.frequency.value = freq;

        // Gentle envelope
        const volume = isFinal ? 0.08 : 0.06;
        gain.gain.setValueAtTime(0, now);
        gain.gain.linearRampToValueAtTime(volume, now + 0.08);
        gain.gain.exponentialRampToValueAtTime(0.001, now + duration);

        osc.connect(gain);
        gain.connect(audioCtx.destination);
        osc.start(now);
        osc.stop(now + duration);
    });
}
```

- [ ] **Step 2: Integrate sound into advance function**

In the `advance` function, add `initAudio()` at the top of the function, and add `playChord(currentStep)` right after `currentStep++` (so it plays when advancing to a new step, not when skipping animation):

```javascript
function advance() {
    initAudio();
    if (isAnimating) {
        // Skip to end of current animation
        skipAnimation();
        const step = steps[currentStep];
        let html = '';
        if (step.header) {
            html += '<div class="header">' + step.header.replace(/\n/g, '<br>') + '</div>';
        }
        html += step.body.replace(/\n/g, '<br>');
        if (step.ps) {
            html += '<br><br><div class="ps">' + step.ps.replace(/\n/g, '<br>') + '</div>';
        }
        contentEl.innerHTML = html;
        if (currentStep < steps.length - 1) {
            promptEl.classList.remove('hidden');
        }
        return;
    }
    if (currentStep < steps.length - 1) {
        currentStep++;
        playChord(currentStep);
        showStep(currentStep);
    }
}
```

- [ ] **Step 3: Verify in browser**

Reload and click through all 8 steps.

Expected:
- Each step plays a brief warm chord on transition
- Chords ascend in pitch from step to step
- Step 8 (final) plays a fuller, longer chord with 4 voices
- Chords are gentle — background atmosphere, not jarring
- Skipping animation (clicking during typewriter) does NOT replay the chord
- If Web Audio is unavailable, page works silently

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "add synthesized musical chords on step transitions"
```

---

### Task 7: Polish and Final Verification

Small visual refinements and a full end-to-end walkthrough.

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add a smooth fade transition between steps**

Add this CSS rule inside the `<style>` tag:

```css
.transmission .body {
    white-space: pre-wrap;
    min-height: 200px;
    transition: opacity 0.15s;
}
```

Update `showStep` to fade out briefly before typing starts — wrap the typewrite call:

```javascript
function showStep(index) {
    const step = steps[index];
    let html = '';
    if (step.header) {
        html += '<div class="header">' + step.header.replace(/\n/g, '<br>') + '</div>';
    }
    html += step.body.replace(/\n/g, '<br>');
    if (step.ps) {
        html += '<br><br><div class="ps">' + step.ps.replace(/\n/g, '<br>') + '</div>';
    }

    promptEl.classList.add('hidden');
    contentEl.style.opacity = '0';
    setTimeout(function() {
        contentEl.style.opacity = '1';
        typewrite(contentEl, html, function() {
            if (index < steps.length - 1) {
                promptEl.classList.remove('hidden');
            }
        });
    }, 150);
}
```

- [ ] **Step 2: Add meta tags for sharing**

Add these inside `<head>`, after the `<title>`:

```html
<meta name="description" content="A transmission from Rocky">
<meta property="og:title" content="Transmission from Rocky">
<meta property="og:description" content="An important message has been received.">
<meta name="robots" content="noindex">
```

The `noindex` keeps search engines from indexing this personal page.

- [ ] **Step 3: Full end-to-end walkthrough**

Open `index.html` in a browser. Walk through every step and verify:

1. Stars animate in background
2. Step 1 types out with header in dim green, body in bright green
3. Chord plays on first click
4. "[ RECEIVE NEXT TRANSMISSION ]" appears after typing completes
5. Clicking during typing skips to full text (no double chord)
6. Each subsequent step: content replaces, new chord (ascending pitch), typewriter animates
7. Step 8: P.S. appears in dim text, no prompt, fuller chord
8. After step 8, clicking does nothing
9. Space/Enter/ArrowRight also advance
10. Resize to mobile width (~375px) — text stays readable, tap works

- [ ] **Step 4: Test on phone**

Open on a phone (or use browser dev tools mobile emulation at 375px width).

Expected: Full experience works with tap. Text is readable. No horizontal scroll.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "add fade transitions, meta tags, and final polish"
```
