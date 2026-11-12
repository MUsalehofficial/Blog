---
title: "HTB Unclaimed: Pwned and Lessons Learned"
description: "A spoiler-free reflection on completing HTB Unclaimed, covering AI tool security, MSSQL, DPAPI, SQL Agent, and Backup Operators."
date: 2026-11-12 12:00:00 +0300
categories: [Hack The Box, Pwned]
tags: [htb, unclaimed, ai-security, mssql, dpapi, sql-agent, backup-operators]
image:
  path: /assets/img/posts/htb-unclaimed/unclaimed-cover.png
  alt: "Locked blue suitcase traveling through a secure data-center baggage system"
---

> **Spoiler-free notice:** Unclaimed is still an active Hack The Box machine. This page contains no commands, credentials, flags, attack path, or solution details.

## Unclaimed: Pwned

I completed **Unclaimed** on 8 November 2026. It was a memorable Windows challenge that rewarded careful reasoning across several security boundaries rather than treating every discovered component in isolation.

![Hack The Box Unclaimed completion confirmation](/assets/img/posts/htb-unclaimed/unclaimed-pwned.png)

## Topics explored

The machine brought together several broad areas:

- AI tool security and the risks created when language-model features can reach privileged back-end functions
- Microsoft SQL Server identities, permissions, and execution contexts
- Windows Data Protection API (DPAPI) and responsible credential protection
- SQL Server Agent jobs, service accounts, and automation boundaries
- The security impact of membership in Windows **Backup Operators**

## Lessons learned

### AI refusal is not an authorization boundary

An AI assistant refusing a request is useful behavior, but it is not a security control. Authorization must be enforced by the application and every downstream tool, independently of the model's wording or decision.

### Service identity matters

Database engines, scheduled jobs, proxies, and operating-system processes can run under different identities. Understanding those boundaries is essential when assessing both exposure and impact.

### Automation credentials require strong protection

Credentials used by scheduled services deserve the same governance as interactive administrator accounts: least privilege, rotation, auditing, and secure storage.

### Encryption must be evaluated as a system

DPAPI is an important Windows protection mechanism, but its effectiveness depends on how keys, context, permissions, and surrounding application data are managed.

### Privileges can matter more than account names

An account does not need to be called "Administrator" to possess sensitive capabilities. Effective privileges and group memberships provide a more accurate picture of risk.

## Detailed walkthrough

**Detailed walkthrough will be released after official retirement.**

Until then, I will not publish hints, commands, credentials, flags, screenshots of the attack path, or other details that could spoil the machine.

## Notify me when it is released

<style>
  .release-notice {
    position: relative;
    overflow: hidden;
    margin: 1.5rem 0;
    padding: clamp(1.4rem, 4vw, 2.2rem);
    border: 1px solid rgba(72, 149, 239, 0.3);
    border-radius: 1.25rem;
    background:
      radial-gradient(circle at 100% 0%, rgba(72, 149, 239, 0.18), transparent 42%),
      linear-gradient(145deg, #151f2f 0%, #101722 100%);
    box-shadow: 0 18px 45px rgba(0, 0, 0, 0.24);
  }

  .release-notice::before {
    content: "";
    position: absolute;
    inset: 0 auto 0 0;
    width: 4px;
    background: linear-gradient(#4cc9f0, #4895ef);
  }

  .release-notice__badge {
    display: inline-flex;
    align-items: center;
    gap: 0.4rem;
    margin-bottom: 0.85rem;
    padding: 0.35rem 0.7rem;
    border: 1px solid rgba(76, 201, 240, 0.35);
    border-radius: 999px;
    color: #8be1f7;
    background: rgba(76, 201, 240, 0.08);
    font-size: 0.75rem;
    font-weight: 700;
    letter-spacing: 0.06em;
    text-transform: uppercase;
  }

  .release-notice h3 {
    margin: 0 0 0.55rem;
    color: #f7f9fc;
    font-size: clamp(1.35rem, 3vw, 1.8rem);
  }

  .release-notice__intro {
    max-width: 44rem;
    margin: 0 0 1.25rem;
    color: #b8c5d8;
    line-height: 1.65;
  }

  .release-notice__form {
    display: flex;
    align-items: stretch;
    gap: 0.75rem;
  }

  .release-notice__field {
    position: relative;
    flex: 1 1 19rem;
  }

  .release-notice__icon {
    position: absolute;
    top: 50%;
    left: 1rem;
    transform: translateY(-50%);
    color: #7f90aa;
    font-size: 1.05rem;
    pointer-events: none;
  }

  .release-notice input[type="email"] {
    width: 100%;
    min-height: 3.25rem;
    padding: 0.75rem 1rem 0.75rem 2.8rem;
    border: 1px solid #33445c;
    border-radius: 0.8rem;
    color: #f7f9fc;
    background: rgba(8, 13, 21, 0.72);
    outline: none;
    transition: border-color 160ms ease, box-shadow 160ms ease;
  }

  .release-notice input[type="email"]::placeholder {
    color: #7f90aa;
  }

  .release-notice input[type="email"]:focus {
    border-color: #4cc9f0;
    box-shadow: 0 0 0 3px rgba(76, 201, 240, 0.16);
  }

  .release-notice button {
    min-height: 3.25rem;
    padding: 0.75rem 1.25rem;
    border: 0;
    border-radius: 0.8rem;
    color: #071019;
    background: linear-gradient(135deg, #4cc9f0, #5aa2ff);
    box-shadow: 0 9px 24px rgba(72, 149, 239, 0.25);
    font-weight: 800;
    white-space: nowrap;
    cursor: pointer;
    transition: transform 160ms ease, box-shadow 160ms ease;
  }

  .release-notice button:hover {
    transform: translateY(-2px);
    box-shadow: 0 13px 28px rgba(72, 149, 239, 0.36);
  }

  .release-notice button:focus-visible {
    outline: 3px solid rgba(139, 225, 247, 0.65);
    outline-offset: 3px;
  }

  .release-notice__privacy {
    display: flex;
    gap: 0.45rem;
    margin: 0.85rem 0 0;
    color: #8f9eb3;
    font-size: 0.82rem;
    line-height: 1.5;
  }

  @media (max-width: 640px) {
    .release-notice__form {
      flex-direction: column;
    }

    .release-notice__field {
      flex-basis: auto;
    }

    .release-notice button {
      width: 100%;
    }
  }
</style>

<aside class="release-notice" aria-labelledby="release-notice-title">
  <div class="release-notice__badge">Release alert</div>
  <h3 id="release-notice-title">Get the walkthrough when it drops</h3>
  <p class="release-notice__intro">
    Join the notification list and receive one email when the detailed Unclaimed walkthrough is published after official retirement.
  </p>
  <form class="release-notice__form" action="https://formspree.io/f/mrpqgrky" method="POST">
    <div class="release-notice__field">
      <span class="release-notice__icon" aria-hidden="true">✉</span>
      <input id="unclaimed-email" name="email" type="email" autocomplete="email" inputmode="email" aria-label="Email address" placeholder="you@example.com" required>
    </div>
    <input name="article" type="hidden" value="HTB Unclaimed walkthrough">
    <input name="_subject" type="hidden" value="Unclaimed walkthrough notification signup">
    <button type="submit">Notify me <span aria-hidden="true">→</span></button>
  </form>
  <p class="release-notice__privacy">
    <span aria-hidden="true">🔒</span>
    <span>One release notification only. No spam, and no walkthrough details before official retirement.</span>
  </p>
</aside>

