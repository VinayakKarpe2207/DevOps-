# 🕒 Auto Daily Commit with GitHub Actions

This repository demonstrates how to set up an automated GitHub Action that makes a daily commit to the repository. It can be used to keep the repository active, update logs, or trigger scheduled workflows.

## 🚀 Features

- ✅ Automatically runs every day at 00:00 UTC
- ✅ Updates a file (`last_updated.txt`) with the current date and time
- ✅ Commits and pushes changes using the `github-actions[bot]` account
- ✅ Requires no manual intervention once configured

## 🛠 How It Works

The workflow is defined in [`.github/workflows/auto-daily-commit.yml`](.github/workflows/auto-daily-commit.yml).  
It performs the following steps:

1. Checks out the repository
2. Writes the current UTC date and time to `last_updated.txt`
3. Commits and pushes the change

## 🧾 Sample Workflow

```yaml
name: Auto Daily Commit

on:
  schedule:
    - cron: '0 0 * * *'   # Runs at 00:00 UTC
    - cron: '0 6 * * *'   # Runs at 06:00 UTC
    - cron: '0 12 * * *'  # Runs at 12:00 UTC
    - cron: '0 18 * * *'  # Runs at 18:00 UTC

jobs:
  auto-commit:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Modify a file (e.g., update timestamp)
        run: |
          echo "Last updated: $(date -u)" > last_updated.txt

      - name: Commit and Push Changes
        run: |
          git config --global user.name "github-actions[bot]"
          git config --global user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add .
          git commit -m "🤖 Daily update - $(date -u)" || echo "Nothing to commit"
          git push
