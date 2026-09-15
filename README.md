# Alishma Bhardwaj — Resume Site

A single static `index.html`. No build step, no dependencies, no framework.
Content is derived verbatim from `Alishma_Bhardwaj_SDE2_Resume_FINAL.tex` — see
`../ALISHMA/JD-Wise-Resume/FACTS.md` before changing any claim.

## What's in it

**Top of page**

- **Download résumé** button (primary blue) serving the bundled PDF, alongside
  copy-email and LinkedIn
- Company logos on each role, on a white plate that stays white in dark mode
- Body text is justified with hyphenation on, matching the LaTeX résumé; falls back to
  left-aligned on phones where justification opens ugly gaps

**Interactive**

- **Clickable skill tags** — tap a skill and the bullets that demonstrate it highlight and
  scroll into view. A tag only becomes clickable if it actually appears in a bullet, so
  nothing leads to an empty result. `Esc` or the Clear button resets it.
- **Sticky nav with scroll-spy** — the current section highlights as you read
- **Scroll progress bar** at the very top
- **Copy-email button** with a confirmation toast (falls back to `mailto:` if the
  clipboard API is unavailable)
- Back-to-top button, timeline hover states on each role, card lift on the stat tiles
- Light/dark toggle — follows your OS by default, your choice persists

**Robust**

- Print stylesheet — `Cmd+P` (or the printer icon) gives a clean PDF; nav, stat row and
  buttons are stripped, roles never split across a page break
- Fully readable with JavaScript disabled — nothing is hidden behind an animation
- `prefers-reduced-motion` respected: all transitions and count-ups turn off
- Responsive to phone width; semantic HTML, real headings, skip link, visible focus rings
- Fonts load from Google Fonts and degrade to the system sans if blocked

## Palette

Bright white + blue, single-hue. Blue tint ramp is monotonic in lightness
(`#eef4ff → #d7e6ff → #a8c9ff → #5fa8ff → #1257e0 → #0a3d91`) and every text color
clears WCAG AA on its surface — accent 6.06:1, body 7.75:1, dark-mode accent 7.62:1.
Dark mode is stepped from the same ramp rather than being an inverted flip.

## Preview locally

```bash
open index.html
```

That's it — no server needed.

---

# Hosting on GitHub Pages

Hosted as a **project site** at `alishmabhardwaj.github.io/resume`.
`AlishmaBhardwaj.github.io` is already taken by the existing portfolio portal,
which this deliberately leaves untouched. Every path in `index.html` is relative,
so serving from a subpath needs no changes.

1. **Create the repo:** [github.com/new](https://github.com/new), named `resume`.
   Public. Do **not** add a README, .gitignore, or license — this folder already
   has files and an initialized repo would conflict on first push.

2. **Push this folder:**

   ```bash
   cd ~/Desktop/alishma-resume-site

   git init
   git config user.name "Alishma Bhardwaj"
   git config user.email "alishma25@gmail.com"

   git add .
   git commit -m "Add interactive resume site"
   git branch -M main
   git remote add origin https://github.com/AlishmaBhardwaj/resume.git
   git push -u origin main
   ```

   The `git config` lines are not optional: the machine's **global** git email is
   `alishmab@amazon.com`. Without overriding it, that work address is baked into
   the public commit history permanently and the commits won't link to the
   personal GitHub account. No `--global` flag means it applies to this repo only.

3. **Authenticate:** at the password prompt, paste a personal access token
   (GitHub rejects account passwords). Create one at
   [github.com/settings/tokens](https://github.com/settings/tokens) →
   *Generate new token (classic)* → tick the **`repo`** scope. macOS Keychain
   stores it after the first push.

4. **Turn Pages on:** repo → **Settings** → **Pages** → *Build and deployment* →
   Source `Deploy from a branch`, Branch `main`, folder `/ (root)` → **Save**.

5. Wait 1–2 minutes, then open **`https://alishmabhardwaj.github.io/resume`**.
   First deploy can take ~5 minutes; the **Actions** tab shows progress.

6. *Optional:* point the RESUME link on the existing portal page at
   `/resume` so both sites connect.

## Updating it later

```bash
cd ~/Desktop/alishma-resume-site
git add .
git commit -m "Update experience"
git push
```

Live within a minute or so. Hard-refresh (`Cmd+Shift+R`) if you see the old version —
GitHub Pages caches for about 10 minutes.

## Custom domain (optional)

If you buy e.g. `alishma.dev`:

1. Repo → Settings → Pages → **Custom domain** → enter it → Save.
   This commits a `CNAME` file to the repo.
2. At your registrar, add DNS records:
   - Apex (`alishma.dev`) → four `A` records: `185.199.108.153`, `185.199.109.153`,
     `185.199.110.153`, `185.199.111.153`
   - `www` → `CNAME` → `<your-username>.github.io`
3. Back in Settings → Pages, tick **Enforce HTTPS** once the cert is issued
   (can take up to 24h).

---

## Notes

**Phone number is deliberately not on the page.** The PDF you email to recruiters has it;
this page is public and gets scraped by bots. To add it back, drop this into the
`<ul class="contact">` block in `index.html`:

```html
<li>
  <a class="chip" href="tel:+917064209567">
    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.7" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><path d="M22 16.9v3a2 2 0 0 1-2.2 2 19.8 19.8 0 0 1-8.6-3.1 19.5 19.5 0 0 1-6-6A19.8 19.8 0 0 1 2.1 4.2 2 2 0 0 1 4.1 2h3a2 2 0 0 1 2 1.7c.1.9.3 1.8.6 2.6a2 2 0 0 1-.5 2.1L8.1 9.5a16 16 0 0 0 6 6l1.1-1.1a2 2 0 0 1 2.1-.5c.8.3 1.7.5 2.6.6a2 2 0 0 1 1.7 2z"/></svg>
    +91 70642 09567
  </a>
</li>
```

**The PDF is now bundled** as `Alishma_Bhardwaj_Resume.pdf` and linked from the
"Download résumé" button at the top. Heads up: **that PDF contains your phone number**,
so publishing the site puts it on the public internet even though the page itself omits it.
To avoid that, compile a phone-free variant of the `.tex` and copy that in instead.

To refresh the download after editing the `.tex`:

```bash
cp ~/Desktop/ALISHMA/Alishma_Bhardwaj_SDE_2_Resume.pdf \
   ~/Desktop/alishma-resume-site/Alishma_Bhardwaj_Resume.pdf
```

**Logos** live in `assets/` and are served from the repo, not hotlinked — the original
Vecteezy and LinkedIn CDN URLs would break or get blocked over time. The Amazon wordmark
came from a stock site; if you ever want to be strict about it, the safest version of an
employer logo on a personal site is either none or one you're licensed to use.

**Keeping the two in sync:** the `.tex` stays the source of record for anything you send
to a company. When you change a bullet there, mirror it here. The wording in this page
matches the FINAL `.tex` as of 2026-09-15.
