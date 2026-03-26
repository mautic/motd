# Mautic MOTD (Message of the Day)

This repository contains the **Message of the Day (MOTD)** configuration for the Mautic project.

The messages are stored in [`motd.json`](motd.json) and are intended to be consumed by Mautic (or supporting services) to display time-bound announcements, tips, and fundraising messages.

## Repository contents

- `.github/` — GitHub configuration (workflows, templates, etc.)
- `motd.json` — The MOTD configuration file (categories + messages)

## File format

`motd.json` contains:

- `categories`: A map of category keys to metadata (currently just `label`).
- `messages`: An array of message objects.

### Categories

Each category entry looks like:

- `label` (string): The UI label for that category.

Example keys in this repo:
- `general`
- `tip`
- `fundraiser`

### Messages

Each message object contains:

- `category` (string): Must match a key in `categories`.
- `content` (object): Channel-specific lines of text to display.
  - `cli` (string[]): One or more lines shown in CLI (command-line interface) contexts.
  - `ui` (string[]): One or more lines shown in UI (user interface) contexts.
  - At least one of `cli` or `ui` must be present.
- `start` (string | null): Start timestamp (ISO-8601, UTC) or `null` for always active.
- `end` (string | null): End timestamp (ISO-8601, UTC) or `null` for no end date.

Example message:

```json
{
  "category": "general",
  "content": {
    "cli": [
      "Mautic 7.1 is now available!",
      "See https://mautic.org/blog for upgrade notes."
    ],
    "ui": [
      "Mautic 7.1 is now available!",
      "See <a href='https://mautic.org/blog'>here</a> for upgrade notes."
    ]
  },
  "start": "2026-03-30T00:00:00Z",
  "end": "2026-04-30T23:59:59Z"
}
```

## Authoring guidelines

- Prefer short, actionable copy.
- Put links on their own line when possible.
- Provide at least one of `content.cli` or `content.ui` for every message.
- Add `content.ui` when UI-specific wording/markup is needed.
- Use `start`/`end` for announcements that should automatically expire.
- Use `null` `start`/`end` for evergreen tips or long-running messages.
- Keep `category` values consistent with `categories`.

## Validation checklist

Before committing changes to `motd.json`:

- [ ] Valid JSON (no trailing commas)
- [ ] Every `message.category` exists in `categories`
- [ ] Every `message.content` contains at least one of `cli` or `ui` as a string array
- [ ] `start`/`end` are either `null` or valid ISO-8601 timestamps (UTC recommended, e.g. `2026-03-30T00:00:00Z`)
- [ ] No expired announcements remain unless intentionally kept for history
