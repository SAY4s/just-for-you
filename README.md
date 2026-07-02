# will you? 💌

A cute single-page date-invite app. Deploy to GitHub Pages, send the link, and answers get committed to a JSON file in one of your GitHub repos.

## 1. Create the repo that will store answers

You can use the **same repo** you deploy this site from, or a separate private one — a separate private repo is recommended so your partner's answer file isn't sitting in the same public repo as the site code.

1. Create a repo, e.g. `yourname/date-answers` (can be private).
2. It can start empty — the app creates `responses.json` on first submit.

## 2. Create a scoped GitHub token

Go to **GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**.

- **Repository access:** "Only select repositories" → choose just the answers repo.
- **Permissions:** Repository permissions → **Contents: Read and write**. Leave everything else as "No access."
- Set an expiration (e.g. 30 days — plenty of time for a date request, and it self-destructs after).
- Copy the token (starts with `github_pat_...`) — you won't see it again.

This way, if the token is ever exposed, the damage is limited to one repo, one permission, one file.

## 3. Deploy to GitHub Pages

1. Push `index.html` to a repo (public repo, since GitHub Pages on the free tier needs public — or use a private repo if you're on GitHub Pro/Enterprise).
2. Repo → Settings → Pages → Deploy from branch → `main` / root.
3. Your site will be live at `https://yourusername.github.io/reponame/`.

## 4. Configure delivery settings via a published Google Sheet

Instead of the token living in the page's source (which would get it auto-revoked by GitHub's secret scanning on a public repo) or requiring per-device setup, the app fetches its config at submit-time from a Google Sheet you publish as CSV.

1. Create a new Google Sheet.
2. In cell `A1`, put a single row: `repo,path,branch,token` — for example:
   ```
   yourname/date-answers,responses.json,main,github_pat_xxxxxxxxxxxx
   ```
3. `File → Share → Publish to web` → format **Comma-separated values (.csv)** → Publish.
4. Copy the resulting URL (looks like `https://docs.google.com/spreadsheets/d/e/.../pub?output=csv`).
5. Paste that URL into `REMOTE_CONFIG_URL` near the top of the `GITHUB SUBMISSION` section in `index.html`, then redeploy.

**Be aware:** a "published to web" Google Sheet is accessible to anyone with the link — it isn't private, and it isn't protected by your Google account login. It's just a link that isn't sitting inside your public git history where GitHub's automated scanner would catch and revoke it. Treat that link with the same care you'd treat the token itself: don't post it publicly, and if you ever suspect it's been shared or guessed, revoke the token immediately from GitHub → Settings → Developer settings and generate a fresh one.

Your partner never sees this URL or the token directly — the page fetches it in the background when they hit submit.

There's still a hidden settings panel in the app (tap the tiny dot bottom-right 5×, or press `S` 5×) that lets you override the config with a locally-stored value for testing — useful if you want to test against a different repo without editing the Sheet.

## 5. Send the link

Just send the plain GitHub Pages URL. When your partner fills out the flow and hits **Send to my heart!**, their browser talks to the GitHub API directly and appends their answer to `responses.json` in your repo — using the token saved in *your* browser during step 4.

## About the token and security

Being upfront about the limits here:

- Anything shipped in client-side JavaScript on a public page can, in principle, be read by anyone who opens dev tools on that page. This app avoids putting the token in the page's source or GitHub history by keeping it in `localStorage`, set locally by you — but `localStorage` itself is only as private as the device it's on.
- The fine-grained, single-repo, contents-only token above is the actual safety net: even in a worst case, exposure is capped to one file in one repo, not your whole GitHub account.
- Don't reuse this token elsewhere, and let it expire/rotate periodically.

## Customizing

- Colors/palette: CSS variables at the top of the `<style>` block (`--pink-500`, `--lilac`, etc).
- Copy/wording: search the HTML for the step headings (e.g. "Will you go on a date with me?").
- Location/day/time options: edit the `<button class="choice-card">` blocks in steps 2–4.
