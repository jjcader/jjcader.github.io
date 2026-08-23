# yourusername.github.io
testing linux thing
My personal site. Static HTML/CSS/JS, no build step, no dependencies.
Live at <https://yourusername.github.io>

---

## Structure

```
.
├── index.html          the entire site — markup, styles and script
├── images/             photos, web-sized (max ~1600px wide)
├── assets/             cv.pdf, favicon, anything downloadable
├── .gitignore          files git should never track
├── .gitattributes      line-ending normalisation (Windows ↔ macOS)
├── .nojekyll           tells GitHub Pages not to run Jekyll
└── README.md           this file
```

## Working on it

Open `index.html` in a browser to preview. That's it — no server needed.

For a slightly more realistic preview (correct relative paths, no
`file://` quirks), run a local server from the repo folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## The routine — every single time

```bash
git pull            # BEFORE you touch anything
# ... edit ...
git status          # what did I change?
git add .           # stage everything
git commit -m "describe the change"
git push            # send it to GitHub
```

Pushing to `main` redeploys the live site automatically, usually within
about a minute.

**Pull first. Always.** This repo is edited from two machines. Skipping
the pull is how you end up with a merge conflict.

## Deployment

GitHub Pages, from the `main` branch, root folder.
Settings → Pages in the repo if it ever needs changing.

## Notes to self

- Resize photos before committing — nothing over ~1600px wide or ~400KB.
- Full-res originals go in `images/_originals/`, which is gitignored.
- Colours are all defined in the `:root` block at the top of `index.html`.
