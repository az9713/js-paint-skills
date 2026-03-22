---
name: pencil-portrait-technique
description: Draw pencil/charcoal style portraits in JS Paint using fine strokes and shading. Use when asked to draw portraits, pencil sketches, or charcoal drawings.
---

# Pencil & Charcoal Portrait Technique for JS Paint

**Prerequisite:** Use the `draw-on-jspaint` skill first to set up the canvas and inject drawing helpers.

## Technique Overview

Pencil portraits use thin lines, cross-hatching for shading, and a grayscale palette. The approach is outline first, then progressive shading to build depth and detail.

## Step-by-Step Process

### 1. Setup Canvas

- Navigate to jspaint.app, inject drawing helpers (see draw-on-jspaint skill)
- Select the **Pencil tool** (or smallest Brush)
- Set color to **black** (#000000)
- The pencil tool draws 1px lines - perfect for fine detail
- Take a screenshot to confirm setup

### 2. Portrait Proportions Guide

Standard face proportions on canvas (assuming ~400px height area):

```
Canvas center: (300, 200)
Face oval:
  - Top of head: y = 80
  - Chin: y = 280
  - Left edge: x = 240
  - Right edge: x = 360

Feature lines:
  - Eyes: y = 170 (roughly halfway down the face)
  - Nose bottom: y = 220
  - Mouth: y = 245
  - Eyebrow line: y = 155

Eye spacing:
  - Left eye center: x = 270
  - Right eye center: x = 330
  - One eye-width apart
```

### 3. Draw Face Outline (Phase 1)

Use thin black strokes to sketch the face oval:

```javascript
// Draw face oval outline
const cx = 300, cy = 180;
const rx = 60, ry = 100; // horizontal and vertical radius
const points = [];
for (let angle = 0; angle < Math.PI * 2; angle += 0.15) {
  points.push([
    cx + Math.cos(angle) * rx,
    cy + Math.sin(angle) * ry
  ]);
}
points.push(points[0]); // close the shape
window._draw.brushStroke(points, 2);
```

### 4. Draw Features (Phase 2)

#### Eyes
```javascript
// Left eye - almond shape
window._draw.brushStroke([
  [252, 170], [260, 165], [270, 163], [280, 165], [288, 170],
  [280, 174], [270, 176], [260, 174], [252, 170]
], 2);

// Left pupil
const pts = [];
for (let a = 0; a < Math.PI*2; a += 0.3) {
  pts.push([270 + Math.cos(a)*5, 170 + Math.sin(a)*5]);
}
window._draw.brushStroke(pts, 2);

// Right eye - mirror
window._draw.brushStroke([
  [312, 170], [320, 165], [330, 163], [340, 165], [348, 170],
  [340, 174], [330, 176], [320, 174], [312, 170]
], 2);

// Right pupil
const pts2 = [];
for (let a = 0; a < Math.PI*2; a += 0.3) {
  pts2.push([330 + Math.cos(a)*5, 170 + Math.sin(a)*5]);
}
window._draw.brushStroke(pts2, 2);
```

#### Nose
```javascript
// Simple nose - two curves and nostrils
window._draw.brushStroke([
  [300, 175], [296, 195], [290, 215], [295, 220], [300, 218]
], 2);
window._draw.brushStroke([
  [300, 218], [305, 220], [310, 215]
], 2);
```

#### Mouth
```javascript
// Upper lip
window._draw.brushStroke([
  [280, 243], [290, 238], [300, 241], [310, 238], [320, 243]
], 2);
// Lower lip
window._draw.brushStroke([
  [280, 243], [290, 250], [300, 253], [310, 250], [320, 243]
], 2);
```

#### Eyebrows
```javascript
// Left eyebrow
window._draw.brushStroke([
  [250, 157], [260, 152], [270, 150], [280, 151], [288, 154]
], 2);
// Right eyebrow
window._draw.brushStroke([
  [312, 154], [320, 151], [330, 150], [340, 152], [350, 157]
], 2);
```

### 5. Add Shading (Phase 3)

Use cross-hatching with lighter gray colors for shadows:

```javascript
// Shadow under left cheekbone - diagonal hatching lines
// Use dark gray (#404040) color first
for (let i = 0; i < 8; i++) {
  const x = 245 + i * 3;
  const y = 190 + i * 2;
  window._draw.stroke(x, y, x + 12, y + 12, 3);
}

// Shadow on right side of nose
for (let i = 0; i < 5; i++) {
  const x = 305 + i * 2;
  const y = 185 + i * 3;
  window._draw.stroke(x, y, x + 8, y + 10, 3);
}

// Under-chin shadow
for (let i = 0; i < 12; i++) {
  const x = 260 + i * 7;
  window._draw.stroke(x, 275, x + 5, 285, 3);
}
```

### 6. Draw Hair (Phase 4)

Use flowing strokes with black and dark gray:

```javascript
// Hair - flowing strokes from top of head downward
// Left side
for (let i = 0; i < 15; i++) {
  const startX = 260 - i * 2;
  const startY = 85 + i * 3;
  window._draw.brushStroke([
    [startX, startY],
    [startX - 10, startY + 30],
    [startX - 15, startY + 60],
    [startX - 10, startY + 90]
  ], 3);
}

// Right side
for (let i = 0; i < 15; i++) {
  const startX = 340 + i * 2;
  const startY = 85 + i * 3;
  window._draw.brushStroke([
    [startX, startY],
    [startX + 10, startY + 30],
    [startX + 15, startY + 60],
    [startX + 10, startY + 90]
  ], 3);
}

// Top of head hair
for (let i = 0; i < 10; i++) {
  const x = 260 + i * 8;
  window._draw.brushStroke([
    [x, 85], [x + 3, 78], [x + 5, 82]
  ], 2);
}
```

### 7. Neck and Shoulders (Phase 5)

```javascript
// Neck
window._draw.stroke(285, 275, 280, 320, 5);
window._draw.stroke(315, 275, 320, 320, 5);

// Shoulders
window._draw.brushStroke([[280, 320], [250, 330], [220, 340], [200, 355]], 3);
window._draw.brushStroke([[320, 320], [350, 330], [380, 340], [400, 355]], 3);
```

## Grayscale Palette

For pencil portraits, use only these colors from JS Paint's palette:
- **Black** (#000000) - Outlines, darkest shadows, pupils
- **Dark Gray** (#808080) - Medium shadows, hair depth
- **Light Gray** (#C0C0C0) - Light shading, subtle shadows
- **White** (#FFFFFF) - Highlights, erasing

## Shading Techniques

### Cross-hatching (medium shadow)
```javascript
// Parallel diagonal lines, then crossing diagonals
for (let i = 0; i < n; i++) {
  window._draw.stroke(x + i*3, y, x + i*3 + 10, y + 10, 3); // /
}
for (let i = 0; i < n; i++) {
  window._draw.stroke(x + i*3 + 10, y, x + i*3, y + 10, 3); // \
}
```

### Stippling (light shadow)
```javascript
// Small dots for subtle shading
for (let i = 0; i < n; i++) {
  const dx = x + Math.random() * w;
  const dy = y + Math.random() * h;
  window._draw.stroke(dx, dy, dx + 1, dy + 1, 1);
}
```

### Contour lines (following form)
```javascript
// Curved lines that follow the shape of the face
window._draw.brushStroke(curvePoints, 2);
```

## Tips

- **Light hand first** - Start with light gray outlines, darken later
- **Build up gradually** - Multiple light layers > one heavy layer
- **Eyes are key** - Spend extra time on eyes, they define the portrait
- **Screenshot after each phase** - Check proportions early, fixing later is harder
- **Leave highlights** - Don't shade everywhere; white space = light on skin
- **Hair = many strokes** - Individual flowing lines, not solid blocks
