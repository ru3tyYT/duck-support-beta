# AGENTS.md — AI Agent Instructions

## What This Project Is

A troubleshooting and guide site for **Revolution Macro** (a Bee Swarm Simulator macro) and **RDP setup**. Built with MkDocs + Material for MkDocs theme. Deployed to **Vercel** (primary) and GitHub Pages (legacy).

- **Live site**: https://duck-support.vercel.app
- **Repo**: https://github.com/ru3tyYT/duck-support
- **Content source**: Community-reported issues from https://github.com/RevolutionGuides/revolutionmacroguide

---

## Architecture

```
duck-support/
├── mkdocs.yml                          # Site config, nav, theme, extensions
├── vercel.json                         # Vercel build config
├── requirements.txt                    # Python deps (mkdocs, mkdocs-material)
├── docs/                               # ALL content lives here
│   ├── index.md                        # Homepage
│   ├── revolution-macro/               # Revolution Macro section
│   │   ├── index.md                    #   Section landing page
│   │   ├── troubleshooting.md          #   Triage flow + quick links
│   │   ├── common-errors.md            #   Gameplay/detection errors
│   │   ├── connection-issues.md        #   Backend/loading/rejoin issues
│   │   ├── rdp-issues.md              #   RDP-specific macro issues
│   │   └── mac-issues.md              #   Mac-specific macro issues
│   ├── rdp-troubleshooting/            # RDP section (not macro-specific)
│   │   ├── index.md
│   │   ├── rdp-setup.md
│   │   ├── rdp-wrapper.md
│   │   ├── common-errors.md
│   │   └── connection-issues.md
│   ├── optimization/
│   │   └── index.md
│   └── assets/
│       ├── stylesheets/custom.css      # Custom sidebar styling
│       └── images/
│           ├── rdp/                    # RDP screenshots
│           └── optimization/           # Optimization screenshots
└── site/                               # Generated output (gitignored)
```

---

## Key Files to Know

| File | Purpose | Edit carefully? |
|------|---------|-----------------|
| `mkdocs.yml` | Nav structure, theme, extensions, plugins | YES — breaking nav breaks the site |
| `vercel.json` | Build command, output dir, install command | YES — breaking this breaks deployment |
| `requirements.txt` | Python dependencies for Vercel build | Yes |
| `docs/assets/stylesheets/custom.css` | Sidebar heading styles | No |
| `docs/**/index.md` | Section landing pages with article tables | Moderate |

---

## How to Add Content

### New article in existing section

1. Create `docs/[section]/new-article.md` with frontmatter:
   ```markdown
   ---
   title: Article Title
   description: Brief description
   ---

   # Article Title

   Content here.

   [← Back to Troubleshooting](troubleshooting.md)
   ```

2. Add to `mkdocs.yml` nav under the correct section:
   ```yaml
   - Revolution Macro:
       - revolution-macro/new-article.md
   ```

3. Add a row to the section's `index.md` article table.

### New section

1. Create `docs/new-section/index.md`
2. Add a new top-level nav entry in `mkdocs.yml`
3. Add a card/link on the homepage `docs/index.md`

---

## Content Rules

### Markdown syntax

- **Admonitions** are enabled. Use them for warnings/tips:
  ```markdown
  !!! warning "Title"
      Content (indented 4 spaces)

  !!! tip "Title"
      Content

  !!! note "Title"
      Content

  !!! danger "Title"
      Content
  ```

- **Collapsible sections** use `???` instead of `!!!`:
  ```markdown
  ??? note "Click to expand"
      Hidden content here.
  ```

- **Code blocks** — always specify language:
  ```markdown
  ```cmd
  some command
  ```                        (close the block)
  ```

### Image paths

Articles are in subdirectories like `docs/revolution-macro/`, so images require going up **2 directories**:

```markdown
![Alt text](../../assets/images/rdp/image.png)
```

Place new images in `docs/assets/images/[section]/`.

### Things to AVOID

- Do NOT use `:material-icon-name:` icon syntax — requires extra plugins not installed
- Do NOT reference `site/` paths — that's generated output
- Do NOT add `site/` to git — it's in `.gitignore`
- Do NOT put content outside `docs/` — MkDocs won't find it

---

## Building and Deploying

### Local development

```bash
pip install mkdocs-material    # one-time
mkdocs serve                   # live preview at localhost:8000
mkdocs build                   # generate site/ folder
```

### Deploy to Vercel (production)

```bash
vercel deploy --prod --yes
```

Vercel runs `pip install --break-system-packages -r requirements.txt` then `python -m mkdocs build` and serves the `site/` output. This is configured in `vercel.json`.

### Verify after deploy

```bash
curl -I https://duck-support.vercel.app                        # should be 200
curl -I https://duck-support.vercel.app/revolution-macro/      # should be 200
```

---

## Git Conventions

- **Commit style**: Small, focused commits. One logical change per commit.
  - `Add RDP Setup Guide article`
  - `Enable markdown admonition extensions`
  - `Remove installation issues article`
- **Branch**: Work is done on `main` directly for now.
- **Push**: Always push after committing. Vercel auto-deploys from the linked GitHub repo, but manual `vercel deploy --prod --yes` is faster.

---

## Content Sources

Revolution Macro troubleshooting content comes from GitHub issues at:
https://github.com/RevolutionGuides/revolutionmacroguide/issues

Issues tagged `[Fix]` in the title contain verified community solutions. Other issues are bug reports that may not have fixes yet.

When importing from issues:
1. Extract the problem title and symptoms
2. Extract the fix steps (from `### How you fixed it` section)
3. Restructure into MkDocs format with admonitions
4. Categorize into the correct article (common-errors, connection-issues, rdp-issues, or mac-issues)
5. Add to the troubleshooting.md triage flow if it's a common issue

---

## Current State / Known Issues

- The MkDocs 2.0 compatibility warning during build is cosmetic — the site builds fine with mkdocs-material. Pin `mkdocs<2` in requirements.txt if it becomes a real problem.
- Some Revolution Macro articles reference GitHub issue attachment images (github.com/user-attachments URLs). These could break if the upstream repo deletes them. Consider downloading to `docs/assets/images/revolution-macro/`.
- The `site_url` in `mkdocs.yml` still points to the old GitHub Pages URL. Update to `https://duck-support.vercel.app` if GitHub Pages is no longer used.
- Mac issues article notes some problems have no known fix — keep checking upstream issues for updates.
