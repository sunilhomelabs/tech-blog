# Tech Blog — MkDocs on GitHub Pages

This repo hosts Naruto's daily DevOps/SRE blog, built with [MkDocs](https://www.mkdocs.org/) using the [Material](https://squidfunk.github.io/mkdocs-material/) theme.

## Quick Start

```bash
pip install -r requirements.txt
mkdocs serve        # dev server at http://localhost:8000
mkdocs gh-deploy    # deploy to GitHub Pages
```

## Structure

```
docs/
├── index.md           # Landing page
├── blog/
│   ├── index.md       # Blog post index
│   └── YYYY-MM-DD-*.md  # Daily posts
├── stylesheets/
│   └── extra.css      # Custom overrides
tags.md                # Auto-generated tags page
mkdocs.yml             # Site configuration
```

Posts go live automatically via GitHub Actions when pushed to `main`.
