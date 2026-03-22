---
name: draw-on-jspaint
description: Navigate to jspaint.app and draw on the canvas using Chrome browser automation. Use when asked to draw, paint, or create artwork in JS Paint.
---

# Drawing on JS Paint with Chrome Browser Automation

## Setup

1. **Get browser context** first with `tabs_context_mcp`
2. **Create a new tab** with `tabs_create_mcp`
3. **Navigate** to `https://jspaint.app/` using `navigate`
4. **Wait** 2 seconds for the app to fully load
5. **Take a screenshot** to confirm JS Paint is loaded and identify canvas position

## Two Drawing Modes

JS Paint uses an HTML5 canvas. There are two approaches to draw on it:

### Mode 1: Native Mode (Recommended)

Uses JS Paint's own Brush/Pencil tools via pointer events. Strokes integrate with JS Paint's undo history, save/export, and tool system.

**Critical**: JS Paint defaults to the **Pencil** tool (1px). You MUST select the **Brush** tool and set `brush_size` for thick strokes.

```javascript
window._native = {
  canvas: document.querySelector('.main-canvas'),

  selectBrush(size, shape) {
    document.querySelector('.tool[title="Brush"]').click();
    brush_size = size || 8;
    brush_shape = shape || 'circle';
  },

  selectPencil() {
    document.querySelector('.tool[title="Pencil"]').click();
  },

  setColor(swatchIndex) {
    const swatches = document.querySelectorAll('.swatch.color-button');
    const s = swatches[swatchIndex];
    if (s) {
      s.dispatchEvent(new PointerEvent('pointerdown', {button:0, buttons:1, bubbles:true}));
      s.dispatchEvent(new PointerEvent('pointerup', {button:0, bubbles:true}));
    }
  },

  evt(type, x, y) {
    const rect = this.canvas.getBoundingClientRect();
    this.canvas.dispatchEvent(new PointerEvent(type, {
      clientX: rect.left + x, clientY: rect.top + y,
      button: 0, buttons: type === 'pointerup' ? 0 : 1,
      bubbles: true, cancelable: true,
      pointerType: 'mouse', pointerId: 1, isPrimary: true, view: window
    }));
  },

  stroke(x1, y1, x2, y2, steps = 10) {
    this.evt('pointerdown', x1, y1);
    for (let i = 1; i <= steps; i++) {
      const t = i / steps;
      this.evt('pointermove', x1 + (x2-x1)*t, y1 + (y2-y1)*t);
    }
    this.evt('pointerup', x2, y2);
  },

  brushStroke(points, steps = 5) {
    if (points.length < 2) return;
    this.evt('pointerdown', points[0][0], points[0][1]);
    for (let i = 1; i < points.length; i++) {
      const [px, py] = points[i-1];
      const [cx, cy] = points[i];
      for (let s = 1; s <= steps; s++) {
        const t = s / steps;
        this.evt('pointermove', px + (cx-px)*t, py + (cy-py)*t);
      }
    }
    this.evt('pointerup', points[points.length-1][0], points[points.length-1][1]);
  }
};
```

**Color swatch index map** (28 colors, top row 0-13, bottom row 14-27):
- 0: black, 1: gray, 2: dark red, 3: olive, 4: dark green, 5: teal, 6: navy, 7: purple
- 8: dark yellow, 9: dark cyan, 10: sky blue, 11: dark blue, 12: violet, 13: brown
- 14: white, 15: silver, 16: red, 17: yellow, 18: green, 19: cyan, 20: blue, 21: magenta
- 22: light yellow, 23: light green, 24: light cyan, 25: light blue, 26: pink, 27: orange

### Mode 2: Canvas Mode

Bypasses JS Paint entirely and draws directly on the `<canvas>` element using the Canvas 2D API. Full control over line width, color, and shapes. Does NOT integrate with JS Paint's undo or tool system.

```javascript
window._paint = {
  ctx: document.querySelector('.main-canvas').getContext('2d'),
  setColor(hex) { this.ctx.strokeStyle = hex; this.ctx.fillStyle = hex; },
  stroke(x1, y1, x2, y2, w = 8) {
    this.ctx.beginPath(); this.ctx.lineWidth = w;
    this.ctx.lineCap = 'round'; this.ctx.lineJoin = 'round';
    this.ctx.moveTo(x1, y1); this.ctx.lineTo(x2, y2); this.ctx.stroke();
  },
  brushStroke(pts, w = 8) {
    if (pts.length < 2) return;
    this.ctx.beginPath(); this.ctx.lineWidth = w;
    this.ctx.lineCap = 'round'; this.ctx.lineJoin = 'round';
    this.ctx.moveTo(pts[0][0], pts[0][1]);
    for (let i = 1; i < pts.length; i++) this.ctx.lineTo(pts[i][0], pts[i][1]);
    this.ctx.stroke();
  },
  circle(cx, cy, r) { this.ctx.beginPath(); this.ctx.arc(cx, cy, r, 0, Math.PI*2); this.ctx.fill(); },
  ellipse(cx, cy, rx, ry) { this.ctx.beginPath(); this.ctx.ellipse(cx, cy, rx, ry, 0, 0, Math.PI*2); this.ctx.fill(); },
  fillRect(x, y, w, h) { this.ctx.fillRect(x, y, w, h); }
};
```

## Workflow

1. **Inject helpers** - Run native or canvas mode helper JavaScript
2. **Select tool** - For native mode: `_native.selectBrush(size)` or `_native.selectPencil()`
3. **Select color** - `_native.setColor(index)` or `_paint.setColor('#hex')`
4. **Draw** - Use `.stroke()` or `.brushStroke()` on whichever helper
5. **Screenshot** - Take screenshot with `computer` tool to check progress
6. **Iterate** - Fix issues, add details, re-screenshot

## Important Notes

- Canvas coordinates are RELATIVE to the canvas element, not the page
- The canvas is typically 683x384 pixels but check via screenshot
- Native mode: ALWAYS select Brush (not Pencil) for thick strokes. Default is Pencil (1px)!
- Native mode: set `brush_size` directly as a global variable for precise control
- Canvas mode: does not support undo (Ctrl+Z)
- Always verify your drawing with screenshots between steps
