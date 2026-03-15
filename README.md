# commit-review-bot

  A bot that automatically reviews commits pushed to your GitHub repositories using Claude AI and sends the results to
  Discord.

  ## Overview

  This repository provides a **Reusable GitHub Actions Workflow** that:
  1. Detects push events on monitored repositories
  2. Extracts the commit diff
  3. Performs a strict code review via Claude API (quality, bugs, security, performance)
  4. Posts the review results to a Discord channel

  ## Architecture

  Monitored Repository
    └─ .github/workflows/review.yml  ← calls reusable workflow
          │ on: push
          ↓
  This Repository (github-commit-review-bot)
    └─ .github/workflows/reusable-review.yml  ← review logic
          │ 1. Get git diff
          │ 2. Call Claude API
          │ 3. Post to Discord
          ↓
  Discord Channel

  ## Setup

  ### 1. Add secrets to your monitored repository

  | Secret | Description |
  |--------|-------------|
  | `ANTHROPIC_API_KEY` | Your Anthropic API key |
  | `DISCORD_WEBHOOK_URL` | Your Discord channel webhook URL |

  ### 2. Add the workflow to your monitored repository

  Create `.github/workflows/review.yml`:

  ```yaml
  name: Commit Review

  on:
    push:
      branches:
        - main

  jobs:
    review:
      uses: YOUR_GITHUB_USERNAME/github-commit-review-bot/.github/workflows/reusable-review.yml@main
      secrets:
        ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        DISCORD_WEBHOOK_URL: ${{ secrets.DISCORD_WEBHOOK_URL }}

  Review Criteria

  Claude reviews each commit diff against the following criteria:

  - Code Quality — readability, naming conventions, code duplication
  - Bugs & Logic Errors — potential bugs, edge cases, null handling
  - Security — vulnerabilities, hardcoded secrets, injection risks
  - Performance — inefficient algorithms, N+1 queries
  - Best Practices — language/framework conventions

  Discord Notification Format

  🔍 Commit Review: <commit message>
  📁 Repository: owner/repo
  👤 Author: username
  🔗 <commit URL>

  【Review Result】
  ⚠️ Issues Found

  <Claude's review content>

  Requirements

  - GitHub Actions enabled on your monitored repository
  - Anthropic API key (https://console.anthropic.com)
  - Discord Webhook URL

  License

  MIT

  ---
