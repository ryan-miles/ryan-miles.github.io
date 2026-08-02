# ryan-miles.github.io

Personal site, plus project sites nested underneath it for convenience.

Live: **https://ryan-miles.github.io/**

## Folder layout

```
OneDrive/ryan-miles.github.io/     <-- git repo A  (this repo)
├── .git/
├── .gitignore                     ignores mermaid-studio/
├── index.html                     the homepage itself
├── README.md                      this file
└── mermaid-studio/                <-- git repo B  (SEPARATE repo, ignored here)
    ├── .git/                          its own history and remote
    ├── .github/workflows/deploy.yml   its own auto-deploy
    ├── src/
    └── README.md
```

## Two repos, not one

They live in the same folder tree but are completely independent. Nothing is
shared — not history, not remotes, not deploys.

| Folder | GitHub repo | Pages mode | Live URL |
|---|---|---|---|
| `.` | [ryan-miles.github.io](https://github.com/ryan-miles/ryan-miles.github.io) | legacy — branch `main`, root | https://ryan-miles.github.io/ |
| `mermaid-studio/` | [mermaid-studio](https://github.com/ryan-miles/mermaid-studio) | GitHub Actions workflow | https://ryan-miles.github.io/mermaid-studio/ |

The two URLs nest, but that is a GitHub Pages routing rule, not a consequence of
the folder nesting: a **user site** (`<user>.github.io`) is served at the domain
root, and every **project site** is served at `/<repo-name>/`. `mermaid-studio`
would resolve to that same URL no matter where its folder sat on disk.

## Working on each

Git commands apply to whichever repo you are standing in. From this folder you
are in repo A; `cd mermaid-studio` puts you in repo B.

```bash
# homepage
cd ~/OneDrive/ryan-miles.github.io
git add index.html && git commit -m "..." && git push     # live in ~1 min

# the app
cd ~/OneDrive/ryan-miles.github.io/mermaid-studio
npm run dev                                               # local preview
git add -A && git commit -m "..." && git push             # Actions builds + deploys
```

### Do not `git add` mermaid-studio/ from this repo

It is in `.gitignore` for a real reason. Git refuses to store a nested repo's
files in an outer repo; it stores a *gitlink* — a bare pointer to a commit SHA.
The result looks fine locally but appears on GitHub as an empty grey folder you
cannot open, and the files are not actually backed up by this repo.

If it ever gets added by accident:

```bash
git rm --cached mermaid-studio        # no -r; drops the gitlink, keeps the files
```

The correct place to back up the app is its own remote, by pushing from inside
`mermaid-studio/`.

## Deploy behaviour differs between the two

- **This repo** uses legacy Pages: whatever is committed to `main` at the root is
  what gets served. There is no build step.
- **mermaid-studio** builds from source via Actions (`.github/workflows/deploy.yml`)
  and publishes `dist-web/`. Build output is gitignored there — never commit it.

So a broken build in one cannot take down the other.
