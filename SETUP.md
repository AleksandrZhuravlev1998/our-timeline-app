# Our Timeline — GitHub Sync Setup

This makes your phone app and Yiqian's web page share one timeline through GitHub.

- **Photos + text/dates sync** both ways. **Videos stay only on your phone.**
- Sync happens **when the app opens, after each edit, and on the ⟳ button**.
- The merge is per-moment: the newest edit wins, deletions propagate. Neither of you
  overwrites the other's separate moments.

There are **two repos** so it's free *and* your content stays private:

| Repo | Visibility | Holds | Purpose |
|------|-----------|-------|---------|
| `our-timeline-app`  | **public**  | the app shell (this folder's `docs/`) | serves the web page (GitHub Pages) — no personal data, no token |
| `our-timeline-data` | **private** | `data/timeline.json` (created automatically) | your actual moments + photos; invite Yiqian here |

> **Why two?** Free GitHub Pages can only publish from a *public* repo. The public repo
> has only code (safe to be public). All your real content lives in the *private* data repo
> and is only ever read with a token. *(If you'd rather have one private repo + Pages, that
> works too but needs a paid **GitHub Pro** plan — see the note at the bottom.)*

---

## Part A — You (one-time, ~10 min)

### 1. Create the two repos
On https://github.com → **New repository**, twice:
- `our-timeline-app` → **Public**
- `our-timeline-data` → **Private**

(Don't add a README to either — keep them empty.)

### 2. Push the app shell to the public repo
In a terminal (replace `YOUR-USERNAME`):

```
cd "C:\Users\ichig\Documents\Claude Projects\Our Journey\github-repo"
git init -b main
git add .
git commit -m "Our Timeline — shared web app"
git remote add origin https://github.com/YOUR-USERNAME/our-timeline-app.git
git push -u origin main
```
(First push opens a browser to sign in to GitHub — that's normal.)

### 3. Turn on GitHub Pages (public repo)
`our-timeline-app` → **Settings → Pages** → *Build and deployment* → Source **Deploy from a branch** → Branch **main**, folder **/docs** → **Save**.
After ~1 minute it shows your site URL, e.g.
`https://YOUR-USERNAME.github.io/our-timeline-app/` — **this is the link Yiqian opens.**

### 4. Create a fine-grained access token
https://github.com → your avatar → **Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token**:
- **Resource owner:** you
- **Repository access:** *Only select repositories* → **`our-timeline-data`**
- **Permissions → Repository permissions → Contents:** **Read and write**
- Generate, then **copy the token** (starts `github_pat_…`). You won't see it again.

### 5. Connect your phone app
Open the app → tap **⚙** (header) → enter:
- **Repo owner:** `YOUR-USERNAME`
- **Repo name:** `our-timeline-data`
- **Branch:** `main`
- **Token:** the `github_pat_…` you copied
→ **Save & Sync.**

This first sync **uploads your current moments** (with photos) into `our-timeline-data/data/timeline.json`. Nothing on your phone is lost.
> Do this step **before** Yiqian connects, so the repo is seeded with your real photos.

### 6. Invite Yiqian
`our-timeline-data` → **Settings → Collaborators → Add people** → her GitHub username or email.
She gets an invite link/email to accept.

---

## Part B — Yiqian (one-time, ~5 min)

1. Make a free GitHub account (if she doesn't have one) and **accept the invite**.
2. Create **her own** fine-grained token — same as step 4 above:
   Resource owner **YOUR-USERNAME**’s `our-timeline-data` *(it appears in her list because she's a collaborator)*, **Contents: Read and write**, copy it.
   *(If a fine-grained token can't see the repo, a Classic token with the `repo` scope also works.)*
3. Open the **Pages URL** from step 3 in her phone/laptop browser. (Tip: "Add to Home Screen" makes it feel like an app.)
4. Tap **⚙** → enter **owner = YOUR-USERNAME**, **repo = `our-timeline-data`**, **branch = main**, **token = her token** → **Save & Sync.**
5. She'll see your timeline, and any moment she adds syncs back to your phone.

---

## Day-to-day

- Open the app → it pulls the latest first. Add/edit/delete → it pushes automatically.
- Tap **⟳** anytime to force a sync. Status shows next to it.
- Offline is fine — changes sync next time you're online.
- **Videos** you add stay on your phone only (by design). Photos are shared.

## Troubleshooting

- **"Sync failed (401/403)"** → token wrong, expired, or missing *Contents: Read and write*. Re-create it (step 4) and re-enter in ⚙.
- **"Sync failed (404)"** → owner/repo name typo, or token not scoped to `our-timeline-data`.
- **Her photos/edits not showing** → tap ⟳; check you're both pointed at the **same** `our-timeline-data` repo.

## Alternative: one private repo (needs GitHub Pro)
If you have **GitHub Pro**, you can skip the public repo: put `docs/` in the *private*
`our-timeline-data` repo, enable Pages there, and point both apps at that one repo. Same app,
fewer repos, but Pages-from-private needs the paid plan.
