# Countdown widget — 24 April 2026

This is a small, embeddable countdown widget that shows the number of days from today to 24 April 2026.

How to use
1. Copy `index.html` to the root of this repository (or to the `docs/` folder).
2. Enable GitHub Pages:
   - Go to the repository Settings → Pages.
   - For "Source", select the `main` branch and root (or `docs/` if you put the file there).
   - Save. Your site will be published at: `https://<your-username>.github.io/<repo-name>/`
     (Example: `https://FGAdriaanse.github.io/Countdown-timer/`)
3. After the Pages site is live (may take a minute), embed it into Notion:
   - Open your Notion page.
   - Type `/embed` and choose Embed.
   - Paste the GitHub Pages URL (e.g. `https://FGAdriaanse.github.io/Countdown-timer/`) and press Enter.
   - Resize the block as needed.

Notes and troubleshooting
- The widget uses the visitor's local time to compute days. Partial days are rounded up so that any time on a day counts that day as remaining.
- If Notion shows a blank preview right away, wait a minute for Pages to finish building and try again.
- If you prefer a single-file hosted URL, keep the provided `index.html` at the site root.
- If you want me to push these files to the repo and enable GitHub Pages for you, tell me and I can create a commit (I will need permission to push or you can run the commands below).

Commands to add & push locally
```bash
# from your local repo clone
git checkout -b add-countdown-widget
# copy index.html into the repo root
git add index.html README.md
git commit -m "Add countdown widget for 24 April 2026"
git push origin add-countdown-widget
# open a PR or merge into main, then enable Pages as described above
```