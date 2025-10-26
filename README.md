name: Daily Commit

on:
  schedule:
    - cron: '25 8 * * *'  # Runs daily at 08:25 UTC
  workflow_dispatch:

permissions:
  contents: write  # REQUIRED for pushing commits

jobs:
  daily-update:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          persist-credentials: false

      - name: Add daily update by 23f2003782@ds.study.iitm.ac.in
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          echo "Last updated on $(date -u)" > daily_update.txt
          git config user.name "github-actions"
          git config user.email "email@ds.study.iitm.ac.in"
          git add daily_update.txt
          git commit -m "Automated daily update $(date -u)" || echo "No changes"
          git push https://x-access-token:${GITHUB_TOKEN}@github.com/${{ github.repository }} HEAD:main
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add daily-log.txt
          git commit -m "Auto commit at $(date -u)" || echo "No changes to commit"
          git push
