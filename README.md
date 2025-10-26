name: Daily Commit

on:
  schedule:
    - cron: '25 8 * * *' # Runs daily at 08:25 UTC
  workflow_dispatch: # allows manual trigger

permissions:
  contents: write # REQUIRED for pushing commits

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
          echo "Daily update $(date)" >> daily_log.txt
          git config user.name "github-actions"
          git config user.email "github-actions@github.com"
          git add .
          git commit -m "Daily update $(date)" || echo "No changes to commit"
          git push

