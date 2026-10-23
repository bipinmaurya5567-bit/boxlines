<div align="center">

# Boxlines

**A small, fast whiteboard for drawing boxes and connecting them.**
Sketch an idea, wire it together, save it as a file.

![Vanilla JS](https://img.shields.io/badge/vanilla-JavaScript-f7df1e?style=flat-square&logo=javascript&logoColor=000)
![SVG](https://img.shields.io/badge/rendering-SVG-4b47c9?style=flat-square)
![rough.js](https://img.shields.io/badge/hand--drawn-rough.js-e03131?style=flat-square)
![No build](https://img.shields.io/badge/build_step-none-2f9e44?style=flat-square)

</div>

---

## What it is

Boxlines is a deliberately simple take on the Excalidraw idea. It does a handful of things and does them well:

| | |
|---|---|
| **Boxes** | Draw as many as you like. Move, resize, duplicate, delete. |
| **Connectors** | Link boxes with arrows that stay attached and follow the boxes when you move them. |
| **Text** | Type inside any box. It wraps and the box grows to fit. |
| **Outline** | Colour, width (thin, regular, bold), and style (solid, dashed, dotted). |
| **Fill** | Pick any inside colour, or none. |
| **Look** | Switch between clean lines and a hand-drawn sketch style. |
| **Save** | Download your diagram as a `.json` file and open it again later. |

Three small files, plain HTML, CSS and JavaScript. No framework, no bundler, no install.

## Quick start

**Run it locally**

1. Download or clone this repository.
2. Open `index.html` in any modern browser.

That is all. If you prefer a local server:

```bash
npx serve .
```

**Host it on GitHub Pages**

1. Push this repository to GitHub.
2. Go to **Settings, Pages**.
3. Choose **Deploy from a branch**, select `main` and `/ (root)`, then save.

Your editor will be live at `https://<your-username>.github.io/<repository-name>/`.

> The page loads two things from the internet: the [rough.js](https://roughjs.com) library for the sketch look and the Figtree and Patrick Hand fonts from Google Fonts. Offline, the editor still works. You get clean lines and system fonts instead.

## How to use it

1. **Add a box.** Pick the Box tool and drag on the canvas, or just double-click empty space.
2. **Write in it.** Double-click a box, or select it and press `Enter`. Press `Esc` when you are done.
3. **Connect boxes.** Pick the Connector tool and drag from one box to another. Or select a box and drag from one of the four dots that appear around it.
4. **Style it.** Select anything and use the panel on the left. With nothing selected, the panel sets the style for the next shapes you draw.
5. **Save it.** Click **Save** to download `diagram.json`. Use **Open** (or drag the file onto the canvas) to load it later.

Your work is also autosaved in your browser, so a refresh will not lose it.

## Keyboard and mouse

| Action | Shortcut |
|---|---|
| Select tool | `V` |
| Box tool | `R` |
| Connector tool | `A` |
| Pan tool | `H` |
| Edit text in the selected box | `Enter` |
| Finish editing | `Esc` or `Ctrl` + `Enter` |
| Delete selection | `Delete` or `Backspace` |
| Duplicate | `Ctrl` + `D` |
| Select all | `Ctrl` + `A` |
| Undo / Redo | `Ctrl` + `Z` / `Ctrl` + `Shift` + `Z` (or `Ctrl` + `Y`) |
| Save | `Ctrl` + `S` |
| Nudge selection | Arrow keys (hold `Shift` for 10 px) |
| Add to selection | `Shift` + click, or drag a selection box |
| Pan | Hold `Space` and drag, middle mouse button, or scroll |
| Zoom | `Ctrl` + scroll wheel, or the zoom buttons |
| Fit diagram to screen | Click the zoom percentage |

On a phone or tablet, tap the **Style** button to open the style panel.

## The save file

A saved diagram is plain JSON, so it is easy to read, diff and version:

```json
{
  "type": "boxlines",
  "version": 1,
  "elements": [
    {
      "id": "k3j9x0aa",
      "type": "rect",
      "x": 0, "y": 40, "w": 170, "h": 80,
      "stroke": "#1e1e1e", "fill": "#a5d8ff",
      "strokeWidth": 2, "strokeStyle": "solid",
      "rough": 1, "round": true, "seed": 1234567,
      "text": "Request comes in", "fontSize": 18
    },
    {
      "id": "p8d2m1qz",
      "type": "arrow",
      "a": { "id": "k3j9x0aa", "x": 0, "y": 0 },
      "b": { "id": "w5n7c4rt", "x": 0, "y": 0 },
      "stroke": "#1e1e1e", "strokeWidth": 2, "strokeStyle": "solid",
      "rough": 1, "seed": 7654321
    }
  ]
}
```

An arrow end is either bound to a box (`id` is set and the end snaps to that box) or free (`id` is `null` and `x`, `y` are its position). Files are validated when opened: unknown element types are dropped, colours and numbers are checked, and text is always rendered as text, never as HTML.

> This is Boxlines' own format. It is not the `.excalidraw` format, so Excalidraw cannot open these files.

## How it works

- **Rendering.** Everything is drawn as SVG inside one pan and zoom group. Sketch mode asks rough.js for the wobbly paths and caches them per element, so a box keeps the same hand-drawn shape until you change it.
- **Connectors.** An arrow stores which boxes it is attached to, not fixed coordinates. On every render each attached end is placed where the line between the two box centres crosses the box edge, plus a small gap. Move a box and the arrow follows with no extra bookkeeping.
- **Text.** Words are measured with a canvas, wrapped to the box width, and centred. While you type, a transparent textarea sits over the box so you get native editing.
- **History.** Each finished action stores a snapshot of the diagram, which powers undo and redo.
- **Dark mode.** The page follows your system theme. Diagram colours are inverted on dark backgrounds so a black outline stays readable.

## Project layout

```
.
├── index.html      Page structure and the toolbar, canvas and panel markup
├── css/
│   └── style.css   Theme colours (light and dark), layout and component styles
├── js/
│   └── app.js      Editor logic: shapes, connectors, text, styling, history, save and open
├── LICENSE
└── README.md
```

## Limits worth knowing

- Text lives inside boxes only. Arrows have no labels yet.
- Arrows are straight lines that aim at the box centre, so two arrows between the same pair of boxes overlap.
- There is no image or PNG export yet.
- There is no live collaboration. Share the `.json` file instead.

## Ideas for later

- Labels on arrows and curved or elbow connectors
- Free-standing text, ellipses and diamonds
- Export to PNG and SVG
- Pinch to zoom on touch screens

## Credits

Boxlines is an independent project inspired by [Excalidraw](https://excalidraw.com), which is open source under the MIT licence. It shares no code with Excalidraw and is not affiliated with it. The hand-drawn look comes from [rough.js](https://github.com/rough-stuff/rough) by Preet Shihn, also MIT licensed.
