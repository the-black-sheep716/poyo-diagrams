# Poyo Basin diagrams — interactive embed

This folder is a ready-to-publish web page. `index.html` is one file with everything inside it
(data, pictures, fonts, scripts): it needs no server code, no build and no internet connection to run.
Open it by double-clicking to try it, or put it on a web host and show it inside other pages and apps.

## Put it on GitHub Pages

1. On github.com, create a repository (New repository → public), for example `poyo-diagrams`.
2. Add file → Upload files, drag in everything from this folder (`index.html`, `examples.html`, `.nojekyll`), Commit changes.
3. In the repository open Settings → Pages. Under "Build and deployment", Source: **Deploy from a branch**; Branch: `main`, folder `/(root)`; Save.
4. After a minute or two the page is at `https://YOUR-NAME.github.io/poyo-diagrams/`. Open it to check.
5. Use that address in the code below. Any other web host works the same way: upload `index.html`, use its address.

## Show a diagram inside another page or app

One page serves all four diagrams; the address decides which one opens:

```html
<iframe src="https://YOUR-NAME.github.io/poyo-diagrams/?d=chronogram" title="Poyo Basin chronogram"
        style="width:100%;aspect-ratio:16/9;border:0" loading="lazy" allowfullscreen></iframe>
```

`?d=chronogram`, `?d=actors` (institutions and people), `?d=money` (bars) or `?d=circles`. Use the same address in apps that ask for a web link instead of code. The bare address (no `?d=`) opens the chronogram with buttons to move between the diagrams.

| Parameter | Values | What it does |
|---|---|---|
| `d` | `chronogram`, `actors`, `money`, `circles` | which diagram opens |
| `panel` | `on` (default), `off` | the information panel that opens when something is clicked |
| `tools` | `1` (default), `0` | zoom, fit and full-screen buttons |
| `switch` | `0` (default when `d` is given), `1` | buttons to move between the diagrams (on when there is no `d`) |
| `bg` | `glass` (default), `clear`, `paper`, `white`, a colour such as `f4efe6` | background; `clear` lets the slide or page show through |
| `select` | a number, e.g. `12`; for `actors` a card ID such as `A005` | opens record № 12 (chronogram) or amount № 12 (money) at the start |
| `pad` | `0`–`80` (default 14) | space around the diagram in pixels |
| `wheel` | `0` (default), `1` | `1` = the mouse wheel zooms; otherwise Ctrl / ⌘ + wheel, so the page around can still scroll |

Example: `?d=money&tools=0&bg=clear&select=7`.

## Where it works

- **Any page or app that has an "embed" or "iframe" block** — paste the code or the address. Notion: type `/embed` and paste the address.
- **PowerPoint**: a PowerPoint add-in such as LiveWeb shows a live web page on a slide (needs an internet connection while presenting).
- **Google Slides** cannot show a live page inside a slide. Put the address on a link on the slide, or use the studio's PNG / SVG / video exports.
- **Canva** embeds only addresses that its embed service recognises; if it refuses, use a link or the exports.
- If a frame stays blank, the app or the host refused to show a page from another site. This page itself sets no limit on being shown in a frame.

## For developers: talking to the diagram

The page sends messages to the page around it (`window.parent.postMessage`), all with `source: "poyo-embed"`:

- `{ type: "ready", diagram, title }` once it has drawn;
- `{ type: "select", diagram, kind, id, number, title }` whenever the open record changes (`kind`: `event`, `period`, `amount`, `town`, `actor`, `none`; amounts also carry `amount`, cards carry `title`);
- `{ type: "view", diagram }` when another diagram is opened.

It listens for messages with `target: "poyo-embed"`:

- `{ target: "poyo-embed", type: "select", id: 12 }` opens record or amount № 12;
- `{ target: "poyo-embed", type: "clear" }` closes the panel;
- `{ target: "poyo-embed", type: "show", diagram: "circles" }` opens another diagram;
- `{ target: "poyo-embed", type: "step", by: 1 }` moves to the next (or `-1` previous) record in the chronogram.

## Notes

- The page is a snapshot: the data, edits, colours and pictures of the studio at the moment it was made. To update it, make the page again in the studio (Export → Embed) and replace `index.html`.
- Nothing is stored in the visitor's browser, and nothing is sent anywhere; the page does not need the internet. Links to sources open in a new tab.
- The animation is not part of the embed: export it as a video from the studio.
