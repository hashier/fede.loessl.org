# Rocky Explains Love — Design Spec

A single-page interactive experience at `fede.loessl.org` where Rocky (from Project Hail Mary) tries to understand human love through a series of hypotheses, each revealed by click/tap. The tone is funny, personal, and builds to a genuinely sweet conclusion.

## Concept

Rocky doesn't understand human love. He observes his friend's relationship with Federica and forms hypotheses, each one wrong but getting warmer. The page is an alien transmission — Federica clicks through Rocky's attempts until he arrives at: "Love = Federica."

## Visual Design

### Aesthetic: Space Terminal
- **Background:** Deep space dark (#0a0a2e to #1a1a4e gradient)
- **Text:** Green monospace (#00ff88), Courier New / system monospace
- **Glow:** Subtle text-shadow on the green text (0 0 10px #00ff8866)
- **Stars:** Small white dots drifting slowly in the background via CSS animation
- **Layout:** Centered single column, max-width ~600px, generous line-height (1.6+)

### No ASCII Art
Rocky is a voice, not a picture. The terminal transmission aesthetic is stronger without trying to draw him.

### Typography
- Transmission headers: small, dim green (11px, #00ff8844)
- Body text: 15-16px, full brightness #00ff88
- P.S. text: smaller, dimmer (#00ff8855, 12px)

## Interaction

### Flow
- 8 steps, each revealed by clicking/tapping anywhere on the page
- Each new step **replaces** the previous one (not a scroll — one screen at a time)
- A subtle prompt at the bottom of each step indicates there's more ("[ RECEIVE NEXT TRANSMISSION ]" or similar)
- The prompt disappears on the final step

### Text Animation
- Each step's text appears with a typewriter/decrypt effect — characters appear one by one
- Speed: fast enough to not be annoying (~30-50ms per character), with slight pauses at line breaks
- The "click to continue" prompt appears only after the text finishes rendering
- Clicking during the typewriter animation completes the text instantly (skip animation)

### Mobile
- Tap anywhere to advance (same as click)
- Text sizes comfortable on mobile
- No hover-dependent interactions

## Sound

### Web Audio API Musical Chords
- Generated at runtime — no audio files to load
- Each step transition plays a short warm chord (2-3 layered sine/triangle waves)
- Chords are gentle and brief (~0.5-1s decay)
- Slight variation between steps (different root notes, ascending progression)
- The final "amaze amaze amaze" step gets a fuller, warmer chord (more voices, longer sustain)
- Sound only triggers on user interaction (click/tap) — no autoplay issues
- Graceful degradation: if Web Audio API unavailable, page works silently

## Script

### Step 1 — The Opening
```
▸ ERIDIAN TRANSMISSION DETECTED
▸ SOURCE: ROCKY
▸ FREQUENCY: ♫♫♫
▸ DECRYPTING...

I have question.
Very important question.

What is... love?
```

### Step 2 — Hypothesis 1: Pasta
```
I ask friend. Friend no help. Friend just smile.

So I make hypothesis.

Hypothesis 1:
Love = when human make food for other human.

Very specific human. Italian human.
Food is... very good. Friend eat too much. Every time.

But no. This is not love.
This is just... pasta.
```

### Step 3 — Hypothesis 2: Love Bites
```
Hypothesis 2:
Love = when human bite other human.

This make no sense. On Erid, bite = danger.
But this human call it "love bite."

I check data. Friend not injured. Friend... happy?

Humans are very confusing species.
```

### Step 4 — Hypothesis 3: The Blanket
```
Hypothesis 3:
Love = when human share one big blanket
even though other human want two normal blanket.

Two blanket is logical. Efficient. Good engineering.
One blanket is... chaos.

But she insist. One blanket. Together.
And he stay under the one blanket.

Every night.

...interesting.
```

### Step 5 — Getting Warmer
```
New data.

I observe: friend think about one specific human
847 times per day.

I count. I am very good at counting.

This seem... inefficient.
But friend say no. Friend say this is best part of day.

All 847 times.
```

### Step 6 — The Realization
```
I analyze all data. Run all models.

Love is not pasta. Not bite. Not blanket.

Love is when entire universe is very big
and very cold and very dark

but you find one person
and suddenly...

not so cold.
```

### Step 7 — The Landing
```
So.

Federica.

I check my friend's data one more time.
I look at all the evidence.

Conclusion:

Love = Federica.

This is not hypothesis. This is fact.
Peer reviewed. By me. Rocky. Very good scientist.
```

### Step 8 — The Sign-Off
```
Amaze amaze amaze. ♫♫♫

End transmission.

P.S. — He love you very much.
Even when you bite him.

Especially when you bite him.

Humans. So strange. So good.
```

## Technical

### Architecture
- **Single file:** `index.html` — all HTML, CSS, and JS inline
- **No dependencies:** No frameworks, no build tools, no external resources
- **No tracking:** No analytics, no cookies, no external requests

### Hosting
- Static file hosting — GitHub Pages, Cloudflare Pages, or any static server
- Domain: `fede.loessl.org`

### Browser Support
- Modern browsers (Chrome, Firefox, Safari, Edge — last 2 versions)
- Mobile Safari and Chrome on iOS/Android
- Graceful degradation: if Web Audio API unavailable, works silently

### File Structure
```
fede.loessl.org/
├── index.html          # The entire experience
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-04-10-rocky-explains-love-design.md
```
