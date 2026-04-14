# Hermes Agent Profiles

Run multiple independent Hermes agents on the same machine — each with its own config, API keys, memory, sessions, skills, and gateway.

## What are profiles?

A profile is a fully isolated Hermes environment. Each profile gets its own directory containing its own `config.yaml`, `.env`, `SOUL.md`, memories, sessions, skills, cron jobs, and state database. Profiles let you run separate agents for different purposes — a coding assistant, a personal bot, a research agent — without any cross-contamination.

When you create a profile, it automatically becomes its own command. Create a profile called `coder` and you immediately have `coder chat`, `coder setup`, `coder gateway start`, etc.

## Quick Start

```bash
hermes profile create coder # creates profile + "coder" command alias
coder setup                  # configure API keys and model
coder chat                   # start chatting
```

That's it. `coder` is now a fully independent agent. It has its own config, its own memory, its own everything.

## Creating a Profile

### Blank Profile
```bash
hermes profile create mybot
```
Creates a fresh profile with bundled skills seeded. Run `mybot setup` to configure API keys, model, and gateway tokens.

### Clone Config Only (`--clone`)
```bash
hermes profile create work --clone
```
Copies your current profile's `config.yaml`, `.env`, and `SOUL.md` into the new profile. Same API keys and model, but fresh sessions and memory. Edit `~/.hermes/profiles/work/.env` for different API keys, or `~/.hermes/profiles/work/SOUL.md` for a different personality.

### Clone Everything (`--clone-all`)
```bash
hermes profile create backup --clone-all
```
Copies **everything** — config, API keys, personality, all memories, full session history, skills, cron jobs, plugins. A complete snapshot. Useful for backups or forking an agent that already has context.

### Clone from a Specific Profile
```bash
hermes profile create work --clone --clone-from coder
```

---

## Version History

| Version | Date | Summary | Author |
|------|------|------|------|
| v1.0.2 | 2026-04-14 | Strict alignment with official documentation (Profiles guide) | openrouter/free |
| v1.0.1 | 2026-04-14 | Removed outdated tutorials, optimized for pure guide | openrouter/free |
| v1.0.0 | 2026-04-14 | Initial official Profiles deployment guide | openrouter/free |

**Official Documentation**: [hermes-agent.nousresearch.com/docs/user-guide/profiles](https://hermes-agent.nousresearch.com/docs/user-guide/profiles)
