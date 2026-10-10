# Deckhand

A Google Slides–style editor that runs entirely in your browser. One HTML file, no build step, no database.

## Features

- **Slides:** add, duplicate, delete, drag to reorder; four starting layouts (Title, Title and content, Two columns, Blank)
- **Elements:** text, shapes and images (auto-downscaled to 1600px); drag, resize, snap to slide edges and centre, nudge with arrow keys; bold, italic, size, alignment, text and fill colour; bring forward / send backward
- **Themes:** Simple light, Midnight, Paper, Forest restyle the whole deck
- **Undo / redo:** `Ctrl+Z` / `Ctrl+Shift+Z`, 100 steps
- **Speaker notes** per slide
- **Present mode:** fullscreen, arrow keys or click to advance, timer, `N` toggles speaker notes
- **Autosave** in your browser (IndexedDB); export as **JSON** or **PDF**, import JSON back
- **Generate slides with AI** from a topic (see below)

## Run it

Open `index.html` in a browser, or serve the folder:

```sh
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Host it on GitHub Pages

1. Create a new GitHub repository and push these files to the `main` branch.
2. In the repository go to **Settings → Pages** and set **Source** to **GitHub Actions**.
3. The included workflow (`.github/workflows/pages.yml`) publishes `index.html` on every push to `main`.
   Your site will be at `https://<your-username>.github.io/<repo-name>/`.

Prefer no Actions? Set **Source** to **Deploy from a branch**, choose `main` and `/ (root)`, and delete the workflow file.

```sh
git init -b main
git add .
git commit -m "Add Deckhand"
git remote add origin git@github.com:<your-username>/<repo-name>.git
git push -u origin main
```

## AI generation

Click **Generate** in the editor (or **New with AI** on the home screen), describe the topic, and choose slide count, tone and whether to write speaker notes. The model returns structured JSON that Deckhand maps onto its own layouts, so the result uses your theme and stays editable.

Three ways to connect:

1. **Hosted inside Claude:** no setup.
2. **Your own Anthropic API key:** paste a key in the dialog. It is sent only to `api.anthropic.com` from your browser, and stored in `localStorage` only if you tick *Remember*. The request uses Anthropic's browser-access header, so anyone with access to your browser profile can read a remembered key. Use a key with a spend limit.
3. **No key:** copy the generated prompt into any chat assistant and paste its JSON reply back into the dialog.

The default model is `claude-sonnet-5-5`; change it in the dialog if you like.

## Keyboard shortcuts

| Action | Keys |
| --- | --- |
| Undo / redo | `Ctrl+Z` / `Ctrl+Shift+Z` (or `Ctrl+Y`) |
| Duplicate element (or slide) | `Ctrl+D` |
| Copy / cut / paste element | `Ctrl+C` / `Ctrl+X` / `Ctrl+V` |
| Bold / italic | `Ctrl+B` / `Ctrl+I` |
| Delete element | `Delete` |
| Nudge element | Arrow keys (`Shift` = 10px) |
| Edit selected text | `Enter` or double-click |
| Present from current / first slide | `Ctrl+Enter` / `Ctrl+Shift+Enter` |
| Next / previous slide (presenting) | `→ Space Enter` / `←` |
| Toggle speaker notes (presenting) | `N` |
| Exit presenting | `Esc` |

On macOS use `Cmd` in place of `Ctrl`.

## File format

Exports are plain JSON:

```json
{
  "app": "deckhand", "version": 1,
  "title": "My deck", "theme": "light",
  "slides": [{ "notes": "", "elements": [
    { "type": "text", "x": 90, "y": 170, "w": 780, "h": 110, "text": "Hello", "role": "title", "size": 60, "bold": true, "italic": false, "align": "left", "color": null, "fill": null },
    { "type": "shape", "kind": "ellipse", "x": 600, "y": 120, "w": 260, "h": 260, "fill": null },
    { "type": "image", "img": "abc123", "x": 0, "y": 0, "w": 300, "h": 200 }
  ]}],
  "images": { "abc123": "data:image/jpeg;base64,..." }
}
```

Slides are 960 × 540 units. A `null` colour follows the theme. Imports are validated and sanitised.

## Notes

- Everything is stored in your browser. Clearing site data deletes your decks, so export JSON for anything important.
- PDF export renders each slide as an image, so PDF text is not selectable.
- Slide fonts are system fonts so exports match the editor; the editor UI loads DM Sans from Google Fonts and falls back to system fonts offline.

## License

MIT
