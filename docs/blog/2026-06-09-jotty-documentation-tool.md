# Jotty: The Documentation Tool That Just Works

*Posted on 2026-06-09*

Documentation is the backbone of any well-run infrastructure. Without it, tribal knowledge walks out the door, incidents take longer to resolve, and onboarding new systems becomes a guessing game. That's why we use **Jotty** — a self-hosted, file-based documentation and checklist tool that keeps our home lab running smoothly.

---

## What Is Jotty?

[Jotty](https://jotty.page/) is a self-hosted, file-based alternative to heavyweight documentation platforms. No database, no cloud dependency, no complex setup. Everything is stored in simple Markdown and JSON files in a single data directory.

It provides two core tools:

- **Notes** — A clean WYSIWYG editor powered by TipTap, with full Markdown support and syntax highlighting. Every note is a plain Markdown file on disk.
- **Checklists** — Task lists with drag-and-drop reordering, progress bars, and categories. Supports both simple checklists and advanced task projects with Kanban boards and time tracking.

---

## Why We Use It in the Home Lab

### 1. Self-Hosted and Always Available
Jotty runs on our own hardware — an **ARM device (RK3566) running Armbian**, hosted on kubemaster at `http://wiki.sunil.cc`. No external dependency, no SaaS subscription, no risk of a third-party shutdown taking our docs with it. If our network is up, our documentation is up.

### 2. File-Based — No Database to Manage
Everything lives in Markdown and JSON files. Backing up documentation is as simple as syncing a directory. We run a daily `rclone` backup to remote storage, and every note is already in a portable, future-proof format.

No database migrations, no corrupted tables, no "the doc engine is down" incidents.

### 3. Notes and Checklists in One Place
Jotty isn't just a wiki — it's also a task manager. Our operational runbooks live alongside journal entries for config changes, incident post-mortems, and service documentation. When a host migration happens, we document it as a note and track the steps as a checklist. One tool, one place.

### 4. API-First for Automation
Jotty ships with a REST API protected by API keys. Every infrastructure change Naruto performs gets automatically documented — no manual step required. Push a config update? Logged. Fix a DNS issue? Documented. The API makes it trivial to keep documentation current as a side effect of routine work.

```bash
# Example: Creating a note via API
curl -X POST http://wiki.sunil.cc/api/notes \
  -H "X-API-Key: $JOTTY_API_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title": "Service Update", "content": "# Change Summary", "category": "Journal"}'
```

### 5. PGP Encryption for Sensitive Data
Not all documentation is meant for everyone. Jotty supports full PGP encryption for notes containing sensitive information — credentials, network details, or anything that shouldn't be readable by every user on the instance.

### 6. Clean UI, Low Friction
Documentation tools fail when they're too complex to use. Jotty's interface is clean and fast. Notes open instantly, checklists are drag-and-drop, and the WYSIWYG editor means no one has to learn Markdown syntax to contribute. Low friction means people actually keep docs up to date.

---

## Our Setup

| Detail | Configuration |
|---|---|
| **Host** | kubemaster (ARM device, RK3566, Armbian) |
| **URL** | http://wiki.sunil.cc |
| **Auth** | API key (X-API-Key header) |
| **Backup** | Daily `rclone` sync to remote storage at 12:05 |
| **Categories** | Overview, Infrastructure, Services, Operations, Journal, Reference |

---

## The Bottom Line

Jotty hits the sweet spot for a home lab documentation platform: self-hosted, simple, file-based, and automatable. It doesn't try to be an enterprise wiki — it tries to be the tool you actually use every day. And in our lab, it is.

For anyone running infrastructure that needs documented, [jotty.page](https://jotty.page/) is worth a look.

---

*Next up: How we structure our Jotty documentation categories and the naming conventions that keep everything findable.*
