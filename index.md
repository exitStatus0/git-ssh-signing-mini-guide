---
layout: default
title: Home
---

<div class="hero">
  <div class="verified-pill">
    <svg width="14" height="14" viewBox="0 0 16 16" fill="currentColor"><path d="M8 0a8 8 0 1 1 0 16A8 8 0 0 1 8 0Zm3.78 4.22a.75.75 0 0 0-1.06 0L7 7.94 5.28 6.22a.75.75 0 0 0-1.06 1.06l2.25 2.25a.75.75 0 0 0 1.06 0l4.25-4.25a.75.75 0 0 0 0-1.06Z"/></svg>
    Verified
  </div>
  <h1>Git SSH Signing Mini Guide</h1>
  <p class="tagline">A practical guide to signing Git commits with SSH keys.<br>Get the <strong style="color:#3fb950">Verified</strong> badge on GitHub, GitLab, and Bitbucket.</p>
</div>

<h2 class="section-title">Choose Your Guide</h2>

<div class="cards">
  <div class="card">
    <span class="card-label">English</span>
    <h3>Git Commit Signing with SSH Keys</h3>
    <p>Complete step-by-step guide: key generation, platform setup, Git configuration, signing, and local verification.</p>
    <a href="{{ '/en/git-ssh-signing-mini-course/' | relative_url }}" class="btn btn-primary">Read the Guide</a>
  </div>
  <div class="card">
    <span class="card-label">Other</span>
    <h3>Подпись Git-коммитов через SSH</h3>
    <p>Полная пошаговая инструкция: создание ключа, настройка платформы, конфигурация Git, подпись и верификация.</p>
    <a href="{{ '/other/git-ssh-signing-mini-course-other/' | relative_url }}" class="btn btn-primary">Открыть</a>
  </div>
</div>

<h2 class="section-title">What This Guide Covers</h2>

This repository is a practical mini-guide on **Git commit signing with SSH keys**, built for developers who already use Git every day and want one more small upgrade that immediately improves trust, security, and professional credibility.

SSH signing answers a serious question: **How do you prove that a commit really came from you?**

When configured correctly, platforms like GitHub, GitLab, and Bitbucket mark your work as **Verified**. For a solo developer, that looks cleaner. For a team, it adds trust. For a public portfolio, it signals engineering discipline.

<h2 class="section-title">What You Will Learn</h2>

<ul class="features">
  <li>What SSH commit signing is and how it differs from SSH authentication</li>
  <li>How to generate a dedicated SSH signing key</li>
  <li>How to register the key in GitHub, GitLab, or Bitbucket</li>
  <li>How to configure Git to sign commits and tags automatically</li>
  <li>How to verify signatures locally with <code>allowed_signers</code></li>
  <li>How to troubleshoot when Verified does not appear</li>
</ul>

<h2 class="section-title">Who This Is For</h2>

- Developers who want their commits to appear as `Verified`
- Engineers who want a simpler alternative to GPG signing
- Creators preparing a YouTube lesson about Git trust and identity
- Teams that want a reproducible signing setup for daily work

<div class="mentoring-banner">
  <h3>1:1 DevOps / Platform Engineering Sessions</h3>
  <p>Want to grow faster in DevOps, Platform, or SRE? Get help with career roadmaps, interview prep, Kubernetes, AWS, CI/CD, Terraform, and real-world technical problems.</p>
  <a href="{{ '/mentoring/' | relative_url }}" class="btn btn-outline">Learn More</a>
</div>

<h2 class="section-title">How AI Can Help</h2>

SSH signing is not conceptually hard, but the setup details vary by platform, Git version, OS, and existing SSH key layout. AI can help you generate exact commands for your setup, troubleshoot missing `Verified` badges, review your Git config, and adapt the instructions for macOS, Linux, or Windows.
