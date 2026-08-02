# ryan-miles.github.io

My personal site. Live at **https://ryan-miles.github.io/**

This repo contains the homepage and nothing else — `index.html` is the whole
site. Pages serves it in *legacy* mode (branch `main`, root), so whatever is
committed here is what goes live. There is no build step.

## Where the project sites live

Sites like **https://ryan-miles.github.io/mermaid-studio/** are *not* in this
repo and are not built from it. Each is its own repository with its own deploy.

| URL | Repo | Local path | Pages mode |
|---|---|---|---|
| https://ryan-miles.github.io/ | `ryan-miles.github.io` (this one) | `~/OneDrive/ryan-miles.github.io/` | legacy, branch `main` |
| https://ryan-miles.github.io/mermaid-studio/ | [`mermaid-studio`](https://github.com/ryan-miles/mermaid-studio) | `~/Developer/mermaid-studio/` | GitHub Actions |

The URLs nest, but that is a GitHub Pages routing rule, not a folder
relationship: a **user site** (`<user>.github.io`) is served at the domain root,
and every **project site** is served at `/<repo-name>/`, wherever its folder
happens to sit on disk.

## Local layout

Code projects live outside OneDrive, one repo per folder:

```
~/Developer/                  <- all code repos
├── mermaid-studio/
└── browser-harness/

~/OneDrive/ryan-miles.github.io/    <- this repo (homepage only)
```

Keeping repos out of OneDrive avoids syncing `node_modules` — roughly 9,000
files for a single Vite project — which competes with real document sync and can
hold file locks. GitHub is the backup for this code, not OneDrive.

Projects are deliberately **not** nested inside this repo. Git cannot store a
nested repo's files in an outer repo; it stores a *gitlink*, a bare commit
pointer that appears on GitHub as an empty grey folder holding none of the
actual files. Separate folders avoid that failure mode entirely.

## Adding a new project site

1. `git init` it in `~/Developer/<name>/` and push to a GitHub repo of the same name.
2. Give it a Pages deploy (see `mermaid-studio/.github/workflows/deploy.yml` for a
   working Actions example). Enable Pages on the repo first — Settings → Pages →
   Source: **GitHub Actions** — because a workflow's `GITHUB_TOKEN` can configure
   an existing Pages site but cannot create one.
3. It appears at `https://ryan-miles.github.io/<name>/` automatically. Nothing in
   this repo needs to change.
