# Jotty: Self-Hosted Documentation That Just Works

*Posted on 2026-06-09*

Documentation is the silent backbone of every well-run infrastructure — yet it's the first thing to fall behind when things get busy. We keep ours in **Jotty**, a self-hosted, file-based documentation tool designed to stay out of your way.

---

## What Is Jotty?

[Jotty](https://jotty.page/) is a lightweight alternative to heavy wiki platforms. No database. No cloud subscription. No vendor lock-in. Everything is stored as plain Markdown and JSON files on disk.

It gives you two core tools:

- **Notes** — A WYSIWYG editor powered by TipTap with Markdown support and syntax highlighting. Every note is a plain `.md` file.
- **Checklists** — Task lists with drag-and-drop reordering, progress bars, categories, and optional Kanban boards with time tracking.

---

## Why We Use It

### Self-Hosted, Always Available
Jotty runs on our own ARM device (RK3566 / Armbian). No external dependency, no SaaS bill, no risk of a third-party shutdown taking our docs with it. If the lab is up, our documentation is up.

### File-Based Storage
No database migrations, no corrupted tables, no "the wiki is down" incidents. Every note is a Markdown file on disk. Backing up is as simple as syncing a directory:

```bash
rclone sync /data/jotty remote:jotty-backup
```

We run this daily at 12:05 — automated, unattended, reliable.

### Notes + Checklists in One Tool
Operational runbooks live alongside incident post-mortems, config change logs, and service documentation. When a migration happens, we write a note for the plan and track the steps as a checklist. One place for everything.

### API-First for Automation
Every infrastructure change Naruto performs gets auto-documented via Jotty's REST API. No manual entry required — it happens as a side effect of routine work.

```bash
curl -X POST https://your-instance/api/notes \
  -H "X-API-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "DNS Update",
    "content": "# Change Summary\nUpdated Pi-hole upstream...",
    "category": "Operations"
  }'
```

### PGP Encryption
Sensitive credentials and network details can be encrypted per-note using PGP. Not everything in the wiki needs to be readable by everyone.

### Clean, Fast UI
Documentation tools fail when they're friction to use. Jotty's interface is fast and minimal — WYSIWYG editing means nobody has to learn Markdown to contribute, but the underlying files stay portable.

---

## Our Setup

| Detail | Configuration |
|---|---|
| **Host** | ARM device (RK3566, Armbian) |
| **Auth** | API key via `X-API-Key` header |
| **Backup** | Daily `rclone sync` at 12:05 UTC |
| **Categories** | Infrastructure, Services, Operations, Journal, Reference |

---

## Why Not Something Else?

- **Confluence?** — Overkill for a home lab. Heavy, slow, expensive.
- **Bookstack?** — Nice, but adds a database layer we don't need.
- **Git-based wikis?** — Great for code, poor for quick edits and checklists.
- **Notion?** — SaaS. Our docs shouldn't live on someone else's servers.

Jotty sits in the sweet spot: self-hosted but not database-heavy, file-based but with a proper API, simple but not limiting.

---

## The Bottom Line

Jotty doesn't try to be an enterprise wiki. It tries to be the documentation tool you actually use — and in our lab, it is. Self-hosted, file-based, automatable, and always available.

If you're running infrastructure that needs documenting, give it a look at **[jotty.page](https://jotty.page/)**.

---

*Next up: How we structure our documentation categories and naming conventions to keep everything findable.*
