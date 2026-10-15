# Personal website playbook

How to build a personal site like madhanirulandi.com for someone new, from zero to live. It uses the same settings and design, with that person's own content. Written after building Madhan's site in Oct 2026.

**What it costs:** the domain (yearly, at Cloudflare's at-cost price). Hosting on GitHub Pages is free.
**Time:** about 1 hour of setup. Then content, which depends on how quickly the facts can be gathered.

---

## 0. Gather the inputs first

Content comes only from real sources. Never invent a number. Ask the person for:

| Item | How to get it |
|---|---|
| LinkedIn profile | On their profile: **More → Save to PDF**, or share the profile link |
| Certificates and licences | Credly / Accredible links, certificate PDFs, issuer emails |
| Photo | A real, recent photo: square, 1000px or larger, plain background, smart clothes |
| Awards | Photos or PDFs of certificates and appreciation letters |
| Resume (optional) | PDF |
| Domain name wanted | e.g. firstlast.com (check it's free on Cloudflare) |

**Aircraft engineer extras**
- **Licence category and authority**, e.g. DGCA AME B1 or B2, or EASA Part-66.
- **Aircraft type ratings**, e.g. A320 family, ATR 72.
- **Recurrent and OEM courses.**
- **Airlines, MROs and bases worked at.**
- **Safety or quality awards.**

**Never publish:**
- Licence numbers, employee IDs, passport details, a home address or phone number.
- Anything from company maintenance manuals or internal reports. Those are confidential to the employer.

---

## 1. Accounts (the person owns them, not you)

1. **GitHub account.** Create it at github.com. Turn on 2FA straight away: Settings → Password and authentication. Save the recovery codes offline.
2. **Cloudflare account.** Create it at dash.cloudflare.com. Turn on 2FA: My Profile → Authentication.

The person signs in and enters passwords and payment details themselves. Claude never types them.

---

## 2. Buy the domain (Cloudflare Registrar)

1. Go to Cloudflare → **Domain Registration → Register Domains**.
2. Search for the name, then buy it. Turn on **auto-renew** and keep the card current.
3. Leave **Registrar Lock** on (it's on by default).

---

## 3. Create the repository and upload the template

1. On GitHub, create a **new public repository** named exactly `<username>.github.io`.
2. Upload every file and folder from this template. Either drag and drop on the repo page (**Add file → Upload files**), or give Claude access to push it.
3. Create a file called `CNAME` in the root. Its only content is the domain, e.g. `firstlast.com`.
4. In `_config.yml`, set `title`, `tagline`, `description`, `author`, `url: "https://firstlast.com"`, `social.links` and `timezone`.
5. In `site.webmanifest`, set `name` and `short_name`.

## 4. Turn on GitHub Pages

1. Go to repository **Settings → Pages**.
2. Under **Source**, choose *Deploy from a branch*, then `main` / `(root)`.
3. Under **Custom domain**, type the domain and click Save.
4. Once DNS works (step 5), tick **Enforce HTTPS**. It can take up to an hour to become available.

## 5. Cloudflare DNS records

Go to Cloudflare → the domain → **DNS → Records**. Add these, all set to **DNS only (grey cloud)**:

| Type | Name | Content |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `<username>.github.io` |

Delete any default parking records Cloudflare added. Grey cloud matters: with the orange proxy, GitHub can't issue the HTTPS certificate.

## 6. Lock it down (do this the same day)

1. **Verify the domain on GitHub.** Go to GitHub account **Settings → Pages → Add a domain**, enter the domain, add the TXT record it shows in Cloudflare DNS, then click **Verify**. This stops anyone else pointing the domain at their own site.
2. **DNSSEC.** In Cloudflare, go to the domain → **DNS → Settings → Enable DNSSEC**.
3. **2FA** on both accounts (step 1).
4. If you use Pages CMS later, give it access to **this repository only**.

---

## 7. Fill in the content

Everything goes in **one file: `_data/profile.yml`**. Every `(placeholder)` gets replaced.

**Writing rules (learned the hard way)**
1. **Sources only.** Every fact and number must come from LinkedIn, a certificate or the person. If a number is missing, ask. Don't guess, and don't "round up" to look better.
2. **Result first, then how.** "Cut defect rework 25% by introducing pre-flight QA checklists", not "Responsible for QA checklists".
3. **Four points per role** is a good default. The current role can have five.
4. **Each point is one idea.** Don't stick two unrelated results together.
5. **Avoid the same round number in several roles.** It reads as made up, even when it's true.
6. **Don't claim certifications they don't hold.** A job title is fine ("Release Train Engineer"); "Certified X" is only for real certificates.
7. **Check dates against public facts,** such as company acquisition dates. Background checks will.
8. **Replace internal jargon** (tool or system nicknames) with plain words. Keep the jargon for interviews.
9. **Pick one spelling style** (UK "programme, licence" or US) and use it everywhere.
10. **Keep the website and LinkedIn identical.** Track the LinkedIn edits in a checklist (see step 13). Claude never edits LinkedIn automatically.

**Aircraft engineer suggestions**
- **Section wording:** use the `labels:` block in `profile.yml`. The template already has aviation wording, such as "Airlines and operators I have worked with" and "Education & Licences".
- **Job icons:** `plane`, `wrench` or `gauge` (also `shield` for safety and quality, `cap` for training).
- **Sector line**, e.g. `"Aviation · Line maintenance, A320 family"`.
- **Metric ideas.** Use only ones he can prove:
  - years licensed;
  - aircraft types certified;
  - AOG recoveries;
  - on-time departures supported;
  - audit findings closed;
  - people trained.
- **Certifications groups**, e.g. "Licences & type ratings" (featured), "OEM & recurrent training", then "Other".
- If there's no teaching or blog, hide those sections in `_data/theme.yml` (`training: false`, `writing: false`).

## 8. Photo

- Use a real photo. Not AI-generated, and don't remove watermarks.
- Retouch gently only: crop, light noise cleanup, edge-aware sharpening, upscaling if it's small. No face reshaping.
- Save it as `assets/images/firstname-headshot.jpg` at 1200×1200 and set `photo:` in `profile.yml` and `image:` in `_config.yml`.
- **To replace a photo later, use a new file name.** Browsers and LinkedIn keep the old image under the same name.

## 9. Badges and award images

- Upload them to `assets/badges/` (the site only loads its own files; this is a security setting).
- **Remove other people's names and emails** from any image before uploading.
- **Never edit an official certificate**, even to fix a misspelt name. Show it as issued, add a `note:`, and ask the issuer for a corrected one.

## 10. Logo and tab icon

- The template ships with a placeholder **FL** monogram in `assets/icons/favicon.svg` and `emblem.svg`. Change it to the person's initials, or ask Claude to design a personal emblem.
- After changing it, regenerate `icon-32.png`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` and `favicon.ico`.
- Raise the `?v=` number on the icon links in `_layouts/default.html` (and `?v=` on the emblem in `_includes/nav.html`), so browsers fetch the new one.

## 11. Look and feel

In `_data/theme.yml`, set:
- `preset` (navy / teal / graphite / emerald / burgundy);
- `display_font` (modern / classic);
- `animate_hero`;
- which sections to `show`.

## 12. Visitor counts (optional)

1. The person signs up at goatcounter.com and picks a code.
2. Put the code in `_config.yml` → `goatcounter: "code"`.

The site's security rules open to GoatCounter only when this is set. It also counts clicks on the email, LinkedIn and resume buttons.

## 13. LinkedIn sync

Keep a checklist doc (`linkedin-pending-changes.md`) of every change the person must copy to LinkedIn by hand: the headline, About, each role's points, certifications and awards. Remind them to turn off **Share with network** before saving each edit.

## 14. Blog (optional)

- Write posts at **pagescms.org** (sign in with GitHub), or add files to `_posts/` named `YYYY-MM-DD-title.md`.
- Posts with a future date go live on that date. The daily GitHub Action in `.github/workflows/weekly-publish.yml` handles it.
- Delete the sample post in `_posts/` before launch.

---

## 15. Before you announce it: QA checklist

- [ ] No `(placeholder)` text left. Search `profile.yml` for "(".
- [ ] The `profile.yml` spacing is valid. If the site stops updating, open the repo's **Actions** tab; the red ✗ names the line.
- [ ] Count the certifications before and after any reordering, so none are lost.
- [ ] Check every page at phone width (390px), laptop width (1280px) and desktop width (1440px+).
- [ ] Click every Verify link.
- [ ] The padlock shows (HTTPS) on both `firstlast.com` and `www.firstlast.com`.
- [ ] Share the link in a private LinkedIn message to yourself and check the preview photo and text.
- [ ] Every number on the site can be explained in an interview.

## 16. Pitfalls we hit on the first site

- **A YAML edit that leaves stray lines silently stops the site updating.** Always validate before committing.
- **A search-and-replace can match the same text in the wrong section** (e.g. a training topic, not a certificate). Scope edits to the section.
- **No inline `style=""` attributes,** because the security policy blocks them. Put styles in `assets/css/style.css`.
- **Image filenames are cached.** Rename to force an update.
- **Cloudflare's orange-cloud proxy breaks GitHub HTTPS.** Keep DNS-only.
- **The hero name font size is tuned for names up to about 16 characters on one line.** For longer names, check desktop and phone and adjust `.hero-copy h1` in the CSS.

---

## 17. Prompt to start the next site with Claude

> Build a personal website for my friend using the template and SETUP-PLAYBOOK.md in my "My Website" project. He's an aircraft engineer. Use only facts from his LinkedIn (I'll share it) and documents he gives us. Follow the writing rules, keep a LinkedIn change checklist, and show me drafts before publishing.
