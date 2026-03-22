---
name: oil-painting-technique
description: Paint oil-painting style artwork in JS Paint using layered brush strokes. Use when asked to create oil paintings, abstract art, or brush-stroke style artwork.
---

# Oil Painting Technique for JS Paint

**Prerequisite:** Use the `draw-on-jspaint` skill first to set up the canvas and inject drawing helpers.

## Technique Overview

Oil painting in JS Paint simulates thick brush strokes through overlapping short strokes with the largest brush size. The key is layering: background first, then midground, then foreground details.

## Step-by-Step Process

### 1. Setup Canvas

- Navigate to jspaint.app, inject drawing helpers (see draw-on-jspaint skill)
- Select the **Brush tool** (paintbrush icon in toolbox)
- Select the **largest brush size** from the tool options panel
- Take a screenshot to confirm setup

### 2. Plan the Composition

Before painting, plan 3-4 layers:
- **Layer 1 - Background**: Sky, distant scenery (large strokes, muted colors)
- **Layer 2 - Midground**: Ground, environment (medium strokes)
- **Layer 3 - Subject**: Main subject (medium strokes, brighter colors)
- **Layer 4 - Details**: Eyes, highlights, small features (small strokes, vivid colors)

### 3. Paint Background (Large Brush Strokes)

Use sweeping horizontal or diagonal strokes to fill the background:

```javascript
// Example: Paint a warm sunset sky
// Use horizontal brush strokes with slight variation
const skyColors = ['#1a0533', '#2d1b4e', '#4a2070', '#6b3090']; // dark purples for night
// For each color band, paint horizontal strokes
for (let y = 0; y < 150; y += 8) {
  const colorIdx = Math.floor(y / 40) % skyColors.length;
  window._draw.brushStroke([
    [0, y + Math.random()*4],
    [150, y + Math.random()*4 - 2],
    [300, y + Math.random()*4],
    [450, y + Math.random()*4 - 2],
    [600, y + Math.random()*4]
  ], 3);
}
```

**Important**: Between color changes, you must click the new color in the palette or set it via JS. Take a screenshot after each major color section.

### 4. Paint Subject (Medium Brush Strokes)

Switch to a medium brush size. Build up the subject with overlapping strokes:

```javascript
// Example: Paint a figure silhouette
// Use vertical and curved strokes to suggest form
// Head - circular strokes
for (let angle = 0; angle < Math.PI * 2; angle += 0.3) {
  const cx = 300, cy = 150, r = 25;
  const x1 = cx + Math.cos(angle) * r;
  const y1 = cy + Math.sin(angle) * r;
  const x2 = cx + Math.cos(angle + 0.5) * r;
  const y2 = cy + Math.sin(angle + 0.5) * r;
  window._draw.stroke(x1, y1, x2, y2, 5);
}
```

### 5. Add Details (Small Brush)

Switch to smallest brush. Add eyes, highlights, stars, texture:

```javascript
// Example: Add stars to night sky
for (let i = 0; i < 20; i++) {
  const x = Math.random() * 580 + 10;
  const y = Math.random() * 120 + 10;
  window._draw.stroke(x-1, y, x+1, y, 2);
  window._draw.stroke(x, y-1, x, y+1, 2);
}
```

### 6. Screenshot and Iterate

After each layer:
1. Take a screenshot with `computer` tool (action: screenshot)
2. Evaluate what needs improvement
3. Add more strokes to fix weak areas
4. Repeat until satisfied

## Color Palettes

### Warm Summer Night
- Sky: `#1a0533`, `#2d1b4e`, `#4a2070`, `#ff6b35`
- Skin: `#d4a574`, `#c68642`, `#8d5524`
- Highlights: `#ffd700`, `#ff8c00`, `#ffffff`

### Winter Snow Scene
- Sky: `#87ceeb`, `#b0c4de`, `#708090`, `#4a5568`
- Snow: `#ffffff`, `#f0f0f0`, `#e0e0e0`, `#c0c0c0`
- Dog: `#8b4513`, `#d2691e`, `#deb887`, `#000000`
- Trees: `#2d5016`, `#4a7c2e`, `#1a3a0a`

## Brush Stroke Patterns

### Horizontal sweep (skies, water)
```javascript
window._draw.brushStroke([[x, y], [x+50, y+2], [x+100, y-1], [x+150, y+1]], 4);
```

### Vertical dab (grass, hair)
```javascript
window._draw.stroke(x, y, x + 2, y + 15, 4);
```

### Circular (faces, rounded objects)
```javascript
// Small circular motion
const cx = 200, cy = 200, r = 10;
const pts = [];
for (let a = 0; a < Math.PI*2; a += 0.5) {
  pts.push([cx + Math.cos(a)*r, cy + Math.sin(a)*r]);
}
window._draw.brushStroke(pts, 3);
```

### Cross-hatch texture
```javascript
// Overlapping diagonal strokes for texture
for (let i = 0; i < 5; i++) {
  window._draw.stroke(x + i*3, y, x + i*3 + 15, y + 15, 4);
  window._draw.stroke(x + i*3 + 15, y, x + i*3, y + 15, 4);
}
```

## Tips

- **Overlap strokes** - Real oil paintings have visible overlapping strokes. Don't try to be precise.
- **Color variation** - Use slightly different shades in the same area for richness
- **Large to small** - Always work from large background strokes to small detail strokes
- **Screenshot often** - Check progress after every major section (background, subject, details)
- **Don't over-detail** - The charm is in the loose, impressionistic style
