# VGM-0001 — engineering log

Plain static site. No framework, no build step. Public content is rendered from
`data.js`; you manage everything through `admin.html`, a local-only editor.

## Files

| File | What it is | Goes public? |
|------|------------|--------------|
| `index.html` | The public site | **Yes** (deployed) |
| `data.js` | Public projects / vault / revisions | **Yes** (deployed) |
| `admin.html` | Local editor — open on your machine only | No (gitignored-optional) |
| `data-private.js` | Your private, yours-only items | **No** — gitignored, never deployed |
| `.gitignore` | Keeps `data-private.js` out of git | — |

Private items physically never leave your computer, so they can't leak: the public
site doesn't even load `data-private.js`.

## First-time publish to GitHub Pages

1. Create a repo on GitHub (public or private — both work; Pages serves publicly either way).
2. In this folder:
   ```bash
   git init
   git add index.html data.js admin.html README.md .gitignore
   git commit -m "Initial site"
   git branch -M main
   git remote add origin https://github.com/<you>/<repo>.git
   git push -u origin main
   ```
   > `data-private.js` is intentionally **not** added — `.gitignore` blocks it. Good.
3. GitHub → repo **Settings → Pages** → Source: **Deploy from a branch** → Branch: **main**, folder **/(root)** → Save.
4. Wait ~1 minute. Site is live at `https://<you>.github.io/<repo>/`.
   (For a root domain `https://<you>.github.io/`, name the repo `<you>.github.io`.)

## Updating projects (your normal workflow)

1. Double-click **`admin.html`** to open it locally in Chrome or Edge.
2. Add / edit / reorder projects, vault blocks, revisions. Toggle each **PUBLIC** or **PRIVATE**.
3. Click **Save data.js** (and **Save data-private.js** if you changed private items).
   - In Chrome/Edge it overwrites the files in place — point the save dialog at this folder.
   - In other browsers it downloads; move the file back into this folder, replacing the old one.
4. Publish the public changes:
   ```bash
   git add data.js
   git commit -m "Update projects"
   git push
   ```
   Pages redeploys in about a minute. Private items stay on your machine only.

## Notes

- **Making a public item private later** removes it from the live site on your next push,
  but earlier commits still exist in git history. If something was truly published, treat it
  as having been public.
- `admin.html` exposes no private data by itself (the private data lives only in your local
  `data-private.js`). It's listed in `.gitignore`'s comments if you'd rather not deploy the
  editor at all — uncomment that line.
- Want to preview the public site exactly as visitors see it? Open `index.html` locally — it
  loads only `data.js`, so private items won't show.
