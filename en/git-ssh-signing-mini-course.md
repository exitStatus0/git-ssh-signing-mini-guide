---
layout: default
title: Git Commit Signing with SSH Keys
description: Step-by-step guide to signing Git commits with SSH keys
---

# Git Commit Signing with SSH Keys

> Mini-course and practical guide for a YouTube video and a GitHub repository.

---

## Course description

This mini-course explains how to sign Git commits and tags with an SSH key, how to make Git sign commits automatically, and how to verify those signatures locally and in platforms like GitHub, GitLab, and Bitbucket.

Unlike the classic GPG flow, SSH signing is usually easier to set up because many developers already use SSH keys for Git access. GitHub explicitly describes SSH as the simplest option for most individual users, and Git requires version **2.34 or later** for SSH signature verification. GitHub, GitLab, and Bitbucket can all show a **Verified** badge for valid SSH-signed commits when the signing key is associated with your account and the commit metadata matches the platform requirements.

---

## Why this is worth doing

Signing commits solves a different problem than just authenticating with Git over SSH.

Authentication answers the question:

**“Can this machine push to the repository?”**

Commit signing answers another question:

**“Can I cryptographically prove who created this commit?”**

That matters because commit signing:

- adds trust to your history;
- helps reviewers confirm commit origin;
- makes your contributions show up as **Verified** in repository platforms;
- reduces ambiguity in team environments, automation, and open-source collaboration;
- makes tampering and impersonation harder;
- helps organizations enforce signed commits on protected branches.

For a YouTube audience, the strongest message is this:

> SSH signing is one of the simplest “small security upgrades” you can add to your daily Git workflow.

---

## What the student will get by the end

After this guide, the student will be able to:

1. understand what SSH commit signing is and how it differs from plain SSH authentication;
2. generate a dedicated SSH signing key or reuse an existing one;
3. register the public key in GitHub, GitLab, or Bitbucket;
4. configure Git to sign commits and tags with SSH;
5. sign commits manually and automatically;
6. verify signatures locally with `git log --show-signature`;
7. understand why a commit may show as `Verified`, `Unverified`, or have no status.

---

## Recommended lesson structure for the video

### Lesson 1. What commit signing actually does
- Difference between SSH auth and commit signing.
- Why `Verified` matters.
- Why SSH is often easier than GPG.

### Lesson 2. Prerequisites
- Git version.
- Existing SSH keys.
- Verified email in Git hosting platform.

### Lesson 3. Generate a dedicated signing key
- Why it is cleaner to separate auth and signing.
- Generate an Ed25519 key.
- Add it to `ssh-agent`.

### Lesson 4. Upload the signing key to the platform
- GitHub flow.
- GitLab flow.
- Bitbucket note.

### Lesson 5. Configure Git for SSH signing
- `gpg.format ssh`
- `user.signingkey`
- `commit.gpgsign true`
- optional `tag.gpgSign true`

### Lesson 6. Sign and push the first commit
- Create a test commit.
- Push it.
- Check the badge in UI.

### Lesson 7. Verify locally
- `git log --show-signature`
- `allowed_signers`
- what “good signature” means.

### Lesson 8. Troubleshooting
- Wrong Git version.
- Wrong email.
- Key added as auth but not as signing.
- Missing `allowedSignersFile`.

---

## Prerequisites

Before starting, make sure you have:

- **Git 2.34+** for SSH signature verification;
- an SSH key, or permission to create one;
- access to GitHub, GitLab, or Bitbucket;
- `user.name` and `user.email` configured correctly in Git;
- a verified email on your Git hosting platform that matches the committer email.

Check your Git version:

```bash
git --version
```

Check your current Git identity:

```bash
git config --global user.name
git config --global user.email
```

Check existing SSH keys:

```bash
ls -al ~/.ssh
```

---

## Architecture of the setup

There are two common approaches:

### Option A — reuse your current SSH key
This is faster.

Good for:
- personal projects;
- quick setup;
- solo work.

### Option B — create a dedicated SSH key only for signing
This is cleaner and easier to manage.

Good for:
- production or work environments;
- separation of duties;
- easier rotation and auditing;
- cleaner demos in a training video.

**Recommended for the course:** use a dedicated signing key.

---

## Step 1. Generate a dedicated SSH signing key

Use **Ed25519** unless you have an organization-specific requirement for another algorithm.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/id_ed25519_signing
```

What this does:

- creates a private key: `~/.ssh/id_ed25519_signing`
- creates a public key: `~/.ssh/id_ed25519_signing.pub`
- associates a comment, usually your email, with the key

When prompted:
- choose a strong passphrase;
- do not leave the private key unprotected unless you fully understand the risk.

Start `ssh-agent` and add the key:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_signing
```

Show the public key:

```bash
cat ~/.ssh/id_ed25519_signing.pub
```

On macOS, copy it to clipboard:

```bash
pbcopy < ~/.ssh/id_ed25519_signing.pub
```

---

## Step 2. Add the key to your Git hosting platform

### GitHub

In GitHub, go to:

**Settings → SSH and GPG keys → New SSH key**

Then:

1. enter a title, for example `MacBook signing key`;
2. choose key type **Signing**;
3. paste the public key;
4. save it.

Important detail:

If you want to use the **same SSH key** both for Git authentication and for commit signing in GitHub, you must upload it **twice**: once as an authentication key and once as a signing key.

CLI alternative:

```bash
gh ssh-key add ~/.ssh/id_ed25519_signing.pub --type signing --title "MacBook signing key"
```

### GitLab

In GitLab, add the key with usage type:

- **Signing**, or
- **Authentication & Signing**

Also make sure the commit email matches a **verified email** in your GitLab account.

### Bitbucket

Bitbucket also supports SSH commit signing, but the verified SSH-signed commit support is focused on commits pushed from the CLI.

---

## Step 3. Tell Git to use SSH for signing

Configure Git globally:

```bash
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519_signing.pub
```

What these options mean:

- `gpg.format ssh` tells Git to use SSH signatures instead of OpenPGP/GPG;
- `user.signingkey` points Git to the **public** SSH key file that identifies which key should be used for signing.

Optional, but highly recommended:

```bash
git config --global commit.gpgsign true
git config --global tag.gpgSign true
```

This makes Git sign:

- every commit by default;
- every tag by default.

You can inspect the config:

```bash
git config --global --list | grep -E 'gpg.format|user.signingkey|commit.gpgsign|tag.gpgSign'
```

---

## Step 4. Create the first signed commit

Inside any repository:

```bash
git checkout -b demo/ssh-signing
echo "SSH signing demo" > signing-demo.txt
git add signing-demo.txt
git commit -S -m "feat: add SSH signing demo"
```

If `commit.gpgsign=true` is already set, you can simply run:

```bash
git commit -m "feat: add SSH signing demo"
```

Push the branch:

```bash
git push -u origin demo/ssh-signing
```

Expected result:

- Git asks for the key passphrase if needed;
- the commit is pushed normally;
- the platform UI should show a **Verified** badge once it can validate the signature.

---

## Step 5. Verify the result in the platform UI

### On GitHub
A valid SSH-signed commit can appear as **Verified** or, in some cases, **Partially verified** depending on repository and vigilant-mode conditions.

### On GitLab
A valid SSH-signed commit appears as **Verified**.

### On Bitbucket
SSH-signed commits can also be verified, but the documented flow is centered on CLI-pushed commits.

If the signature is present but the platform cannot trust or associate it correctly, the commit may appear as **Unverified**.

---

## Step 6. Verify signatures locally

Platform verification is useful, but local verification is an important part of understanding how SSH signing works.

For SSH signatures, local verification depends on an **allowed signers file**.

Create it:

```bash
mkdir -p ~/.ssh
touch ~/.ssh/allowed_signers
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
```

Add your public key to the file:

```bash
echo "$(git config --get user.email) namespaces=\"git\" $(cat ~/.ssh/id_ed25519_signing.pub)" >> ~/.ssh/allowed_signers
```

Now inspect signatures:

```bash
git log --show-signature -1
```

You want to see output similar to:

```text
Good "git" signature for your_email@example.com with ED25519 key ...
```

### Why `allowed_signers` matters

SSH signing does not work like GPG’s web-of-trust model. Git needs a file that explicitly tells it which public keys you trust for verification. That is what `gpg.ssh.allowedSignersFile` is for.

---

## Step 7. Sign tags too

If you want signed releases:

```bash
git tag -s v0.1.0 -m "v0.1.0"
git push origin v0.1.0
```

If `tag.gpgSign=true` is enabled, tags will also be signed by default.

This is especially useful for:

- release tags;
- public repositories;
- internal delivery pipelines;
- proving the origin of release points.

---

## Common reasons why `Verified` does not appear

### 1. Git is too old
SSH signature verification requires **Git 2.34+**.

### 2. The key was not added as a signing key
This happens often on GitHub if the key was uploaded only for authentication.

### 3. Wrong committer email
Your commit email must match a verified email in the platform account.

Check it:

```bash
git config user.email
git config --global user.email
```

### 4. You configured the wrong key path
`user.signingkey` should point to the correct `.pub` file.

### 5. `ssh-agent` does not have the private key loaded
Fix:

```bash
ssh-add ~/.ssh/id_ed25519_signing
```

### 6. Local verification fails because `allowed_signers` is missing
UI verification and local verification are related but not identical. A platform may show **Verified** even when your local machine cannot verify signatures yet, because local Git still needs `gpg.ssh.allowedSignersFile` configured.

### 7. You rewrote history and are checking old assumptions
Always check the exact commit SHA after rebases, squashes, or amended commits.

---

## Suggested talking points for the YouTube explanation

### Core message
“SSH signing is not about getting access to the repo. It is about proving authorship of the commit.”

### Strong comparison phrase
“SSH auth proves you can push. SSH signing proves the commit came from a trusted key.”

### Practical security angle
“This is one of the rare security improvements that is both real and cheap: a few commands, and your history becomes more trustworthy.”

### Clean teaching angle
“If you already use SSH for Git, then commit signing with SSH is usually simpler than the classic GPG route.”

---

## Demo flow for the video

Use this exact sequence in the screencast:

1. show an unsigned commit in the repository history;
2. explain the trust problem in 20–30 seconds;
3. generate a dedicated signing key;
4. add the key to GitHub as **Signing**;
5. configure Git with `gpg.format ssh` and `user.signingkey`;
6. create a test commit;
7. push it;
8. refresh GitHub and show the **Verified** badge;
9. verify locally with `git log --show-signature`;
10. break the setup intentionally once, for example wrong email or missing signing key, and show the failure mode.

That makes the video more credible because viewers see both the happy path and the typical mistake.

---

## Minimal command cheat sheet

```bash
# 1) Generate a signing key
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/id_ed25519_signing

# 2) Add to ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_signing

# 3) Configure Git for SSH signing
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519_signing.pub
git config --global commit.gpgsign true
git config --global tag.gpgSign true

# 4) Create signed commit
git add .
git commit -S -m "feat: signed commit with SSH"

# 5) Local verification
mkdir -p ~/.ssh
touch ~/.ssh/allowed_signers
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers
echo "$(git config --get user.email) namespaces=\"git\" $(cat ~/.ssh/id_ed25519_signing.pub)" >> ~/.ssh/allowed_signers
git log --show-signature -1
```

---

## Final outcome to summarize in the video

By the end of the setup, the viewer should have:

- a dedicated SSH signing key;
- that key added to their Git hosting account;
- Git configured to sign commits automatically;
- at least one signed commit in history;
- a visible **Verified** badge in the platform UI;
- local verification working through `allowed_signers`.

This is the clean final message for the audience:

> You did not just make Git “work”. You made your commit history more trustworthy.

---

## Optional advanced additions for a follow-up lesson

If you want to expand this into a second video or bonus section, add:

- per-repository signing config instead of global config;
- separate work and personal signing keys;
- signing tags for releases;
- key rotation strategy;
- organization policies with required signed commits;
- using the same key for auth and signing vs separate keys;
- SSH certificates and corporate CA-based flows.

---

## Suggested repository file structure

```text
/git-ssh-signing-mini-course
  ├── README.md
  ├── assets/
  │   ├── verified-badge.png
  │   ├── terminal-config.png
  │   └── allowed-signers-example.png
  └── scripts/
      └── demo-setup.sh
```

If this markdown becomes the main material for GitHub, rename it to:

```text
README.md
```

---

## Sources

This guide was prepared using official documentation from Git, GitHub, GitLab, and Bitbucket:

- GitHub Docs — About commit signature verification
- GitHub Docs — Telling Git about your signing key
- GitHub Docs — Signing commits
- GitHub Docs — Adding a new SSH key to your GitHub account
- Git documentation — `git-config` (`gpg.format`, `gpg.ssh.allowedSignersFile`)
- GitLab Docs — Sign commits with SSH keys
- Bitbucket Cloud Docs — Use SSH keys to sign commits

---

## Notes for future editing

Good places to personalize before publishing:

- replace `your_email@example.com` with your real demo email;
- add screenshots from GitHub UI;
- add a short troubleshooting section for macOS Keychain or Linux desktop environments;
- optionally add a separate block for Windows PowerShell or Git Bash.
