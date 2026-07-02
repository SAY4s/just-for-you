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

## 4. Configure delivery settings (you only, before sending the link)

**Important: do this on your own device, not a shared/public computer** — the token is stored in that browser's local storage.

1. Open your live site.
2. Trigger the hidden settings panel: press the **`S` key five times quickly**, or tap the tiny dot in the bottom-right corner five times (works on mobile).
3. Fill in:
   - **Repo:** `yourname/date-answers`
   - **File path:** `responses.json`
   - **Branch:** `main`
   - **Token:** your fine-grained PAT
4. Click **Save to this browser**.
5. Close the settings modal.

Your partner never sees this panel or the token — it's saved only in your browser's local storage, not in the page's source code, so it's never sent to them.

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
