# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repository is

A documentation-only repository containing educational materials for a YouTube mini-guide on **Git commit signing with SSH keys**. There is no code, build system, tests, or linting — all content is Markdown.

## Repository structure

- `README.md` — multilingual landing page (English + Other + Ukrainian) serving as the public-facing hub
- `en/git-ssh-signing-mini-course.md` — full English guide (step-by-step setup, troubleshooting, video script notes)
- `other/git-ssh-signing-mini-course-other.md` — Other-language translation of the same guide
- `ua/git-ssh-signing-mini-course-ua.md` — Ukrainian translation of the same guide
- `mentoring-offer/mentoring-offer/DevOps-Mentoring-Offer.md` — separate Other-language mentoring offer document

## Content conventions

- English guide lives under `en/`, Other content under `other/`, Ukrainian content under `ua/`
- The three language guides mirror each other in structure; changes to one should typically be reflected in the others
- `README.md` links to all three language guides and serves all audiences (sections separated by `---`)
- Technical commands in the guides use `bash` code blocks and should remain copy-pasteable verbatim
- Email placeholders use `your_email@example.com` — do not replace with real addresses
