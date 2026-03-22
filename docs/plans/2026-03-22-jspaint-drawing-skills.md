# JS Paint Drawing Skills - Reverse Engineering the Video Experiments

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Reverse-engineer the drawing skills from the AllAboutAI video and replicate the JS Paint experiments using Claude Code + Chrome browser automation.

**Architecture:** Create 4 Claude Code skills in `.claude/skills/` that teach Claude how to draw on jspaint.app using JavaScript mouse event dispatch. A base skill handles canvas interaction, while technique skills (oil painting, pencil portrait, comparison) layer on top. Then execute the 3 painting experiments from the video.

**Tech Stack:** Claude Code skills (markdown), Chrome MCP tools (javascript_tool, computer/screenshot), jspaint.app canvas API via DOM events.

---

## Phase 1: Create Skills

### Task 1: Create `draw-on-jspaint.md` base skill

**Files:**
- Create: `.claude/skills/draw-on-jspaint.md`

The foundational skill that teaches Claude how to:
- Navigate to jspaint.app
- Identify the canvas element
- Select tools (brush, pencil, fill) via JavaScript clicks
- Select colors from the palette
- Draw on the canvas by dispatching mousedown/mousemove/mouseup events
- Take screenshots to verify progress
- Clear canvas for fresh starts

### Task 2: Create `oil-painting-technique.md` skill

**Files:**
- Create: `.claude/skills/oil-painting-technique.md`

Technique skill for oil painting style:
- Brush stroke simulation (varying pressure via multiple overlapping strokes)
- Color layering (background → midground → foreground)
- Warm/cool palette selection
- Compositional planning (sky, ground, subject)
- Texture via short overlapping brush movements

### Task 3: Create `pencil-portrait-technique.md` skill

**Files:**
- Create: `.claude/skills/pencil-portrait-technique.md`

Technique skill for pencil/charcoal portraits:
- Fine line strokes using smallest brush
- Shading via cross-hatching (overlapping angled lines)
- Grayscale palette (black, dark gray, medium gray, light gray)
- Portrait composition (oval face, feature placement)
- Detail layering (outline → shadows → details → highlights)

### Task 4: Create `compare-drawing.md` skill

**Files:**
- Create: `.claude/skills/compare-drawing.md`

Comparison/iteration skill:
- Take screenshot of current canvas state
- Visual comparison against reference or goal
- Identify areas needing improvement
- Iterate: fix issues, re-screenshot, re-compare
- Stop when satisfied with quality

## Phase 2: Execute Experiments

### Task 5: Oil painting - Abstract female on hot summer night

Using draw-on-jspaint + oil-painting-technique skills:
1. Open jspaint.app in Chrome
2. Plan composition: warm summer sky, female figure, night atmosphere
3. Paint background (dark warm sky with stars)
4. Paint subject (female figure with oil brush strokes)
5. Add details and highlights
6. Screenshot and iterate

### Task 6: Oil painting - Dog playing in snow

Using draw-on-jspaint + oil-painting-technique skills:
1. Open jspaint.app (fresh canvas)
2. Plan composition: snowy landscape, playful dog
3. Paint background (white/blue snow, winter sky)
4. Paint dog (4 legs, tail, body)
5. Add snow details, movement
6. Screenshot and iterate

### Task 7: Pencil portrait - Female

Using draw-on-jspaint + pencil-portrait-technique skills:
1. Open jspaint.app (fresh canvas)
2. Plan face proportions (oval, eye line, nose, mouth)
3. Draw outline with fine strokes
4. Add shading and shadows
5. Detail eyes, hair, features
6. Screenshot and iterate
