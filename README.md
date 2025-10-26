name: Daily Commit

on:
  schedule:
    - cron: '25 8 * * *' # Runs daily at 08:25 UTC
  workflow_dispatch: # Allow manual run

permissions:
  contents: write  # REQUIRED to push changes

jobs:
  commit:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          persist-credentials: false

      - name: Configure Git identity
        run: |
          git config --global user.name "github-actions"
          git config --global user.email "github-actions@github.com"

      - name: Make daily update
        run: |
          echo "Daily update at $(date)" >> daily_log.txt
          git add daily_log.txt
          git commit -m "Daily update: $(date)" || echo "No changes to commit"

      - name: Push changes
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          git push https://x-access-token:${GITHUB_TOKEN}@github.com/${{ github.repository }} HEAD:${{ github.ref }}

