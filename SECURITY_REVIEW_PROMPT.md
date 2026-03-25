# Classics in the Key of Post-Hardcore — Security & Hardening Audit (v1)

*Your task is a comprehensive security audit of a public GitHub repo and GitHub Pages site that is about to be shared widely on Reddit and social media. The goal is to lock down the repo, the site, and the owner's personal exposure before increased visibility brings increased risk.*

Act as a veteran security engineer with deep experience in GitHub platform security, static site hardening, and personal OPSEC for public open-source projects. You understand that even a site with zero JavaScript still has an attack surface — through the repo, the hosting platform, the git history, the external dependencies, and the social links that tie a pseudonymous identity to a real person.

## Context

**Classics in the Key of Post-Hardcore** is a **pure static HTML/CSS site** hosted on **GitHub Pages** from the `main` branch of a public repo. It's a personal creative project — public domain poems adapted into post-hardcore songs with AI tools (Claude for lyrics, Suno for music). The site shows poem/song side-by-side breakdowns, structural analysis, and Suno prompt documentation.

**What's about to happen:** The creator (`vegaPDX`) plans to share this on Reddit and other platforms. This will increase traffic, visibility, and exposure to both constructive engagement and potential malicious actors — people who may attempt to deface the repo, scrape personal information, phish the owner, or exploit the GitHub Pages hosting.

**What this project is NOT:**
- No JavaScript anywhere — zero `<script>` tags, zero inline handlers, zero `.js` files
- No build tools, no `package.json`, no `node_modules`, no bundler
- No backend, no API, no database, no authentication
- No forms, no user input, no `<input>`, `<textarea>`, or `<form>` elements
- No localStorage, no cookies, no client-side state
- No iframes, no embeds, no third-party widgets
- No images, audio, or video files — all media is linked externally

**What this project IS:**
- 37 static files: HTML pages, one CSS file, two markdown docs
- Pure read-only content delivery via hyperlinks
- External links to: Suno (songs), Poetry Foundation / poets.org / allpoetry.com / thepoetryhour.com (source poems), Claude (attribution), GitHub (repo)
- Single external CSS dependency: Google Fonts (Cormorant, Libre Baskerville, Work Sans) loaded via `@import` in `styles.css`
- Deployed on GitHub Pages from `main` branch with no custom domain

## File Inventory (37 files)

```
/
├── index.html                          Hub page — links to volumes
├── prompts.html                        Suno prompts index — base templates, techniques, song cards
├── styles.css                          Shared stylesheet (Google Fonts @import, CSS custom properties)
├── README.md                           Project documentation
├── CONTRIBUTING.md                     "Contributions not open. Fork it."
├── SECURITY_REVIEW_PROMPT.md           This file (not deployed)
├── vol1/
│   ├── index.html                      Volume 1 song grid
│   ├── the-old-lie.html                12 song breakdown pages (poem + lyrics + analysis)
│   ├── the-floor-gave-out.html
│   ├── same-as-everyone.html
│   ├── the-weight-of-her-hand.html
│   ├── and-god-did-nothing.html
│   ├── nothing-would-come-out.html
│   ├── three-seconds.html
│   ├── marcus-has-my-pen.html
│   ├── jesus.html
│   ├── followed-every-rule.html
│   ├── none-of-it-was-real.html
│   └── there-was-never-anyone-there.html
├── vol2/
│   ├── index.html                      Volume 2 song grid
│   ├── friend-of-a-friend.html         2 song breakdown pages
│   └── my-fathers-son.html
└── song_prompts/
    ├── the-old-lie.html                14 Suno prompt detail pages
    ├── the-floor-gave-out.html
    ├── same-as-everyone.html
    ├── the-weight-of-her-hand.html
    ├── and-god-did-nothing.html
    ├── nothing-would-come-out.html
    ├── three-seconds.html
    ├── marcus-has-my-pen.html
    ├── jesus.html
    ├── followed-every-rule.html
    ├── none-of-it-was-real.html
    ├── there-was-never-anyone-there.html
    ├── friend-of-a-friend.html
    └── my-fathers-son.html
```

---

## Audit Area 1: Personal Information Exposure (CRITICAL)

The repo is public. The git history is public. When this gets shared on Reddit, people will look.

### 1a. Git history email leak

**Known issue:** The git commit history contains the personal email `chris.vega@gmail.com` in author/committer fields from early commits. Later commits use the GitHub noreply address `37459308+vegaPDX@users.noreply.github.com`.

**Audit tasks:**
- Run `git log --format="%an <%ae>" | sort -u` and `git log --format="%cn <%ce>" | sort -u` to confirm all unique author/committer identities.
- Determine how many commits expose `chris.vega@gmail.com` vs. the noreply address.
- Assess options and trade-offs:
  - **Option A:** `git filter-branch` or `git filter-repo` to rewrite history and replace the personal email. This is a force-push that rewrites all commit SHAs. Document the exact commands.
  - **Option B:** Accept the exposure. The email is already in GitHub's event log and may have been cached by third-party scrapers. Rewriting history doesn't guarantee removal.
  - **Option C:** Add a `.mailmap` file to display the noreply address in `git log` output (cosmetic only — raw commits still contain the original email).
- For whichever option is recommended, provide the exact steps.

### 1b. Username and identity linkage

**Audit tasks:**
- The pseudonym `vegaPDX` is used on GitHub and Suno. Search the codebase and README for any references that could link this pseudonym to a real name, location, employer, or other identifying information beyond what's already public.
- The README says "A project by **vegaPDX**." Is there anything else in the deployed HTML files that leaks identity?
- Check if the GitHub profile (`github.com/vegaPDX`) has a public bio, location, company, or linked social accounts that increase exposure. (Note: the auditor should check this manually via the GitHub API or web.)

### 1c. GitHub account security recommendations

**Document recommendations for:**
- Two-factor authentication (2FA) — is it enabled? It should be.
- GitHub personal access tokens — are any tokens scoped to this repo? Should they be revoked before Reddit exposure?
- GitHub email privacy settings — is "Block command line pushes that expose my email" enabled?
- Signed commits — should the owner enable GPG or SSH commit signing to prevent impersonation?

---

## Audit Area 2: Repository Security & Access Control (HIGH)

A public repo shared on Reddit is a target for social engineering, malicious PRs, and automated attacks.

### 2a. Branch protection

**Audit tasks:**
- Check if `main` branch has branch protection rules. It likely does not (there are no GitHub Actions or CODEOWNERS file).
- **Recommend enabling:** Require pull request reviews before merging, restrict who can push to `main`, require status checks, prevent force pushes and deletions.
- Provide the exact `gh api` or GitHub web steps to configure this.

### 2b. CODEOWNERS

**Audit tasks:**
- No `.github/CODEOWNERS` file exists. Recommend creating one so that all changes require review by the repo owner.
- Provide the exact file content.

### 2c. GitHub Actions & workflows

**Audit tasks:**
- No `.github/workflows/` directory exists. This is good (no CI/CD to compromise), but also means no automated validation.
- Assess whether a minimal GitHub Actions workflow would add value — e.g., HTML validation, link checking, or a "this PR modifies HTML files" label.
- **Risk assessment:** If the owner ever enables GitHub Actions, could a malicious PR's workflow exfiltrate secrets or modify the deployment? Document the default permissions for `GITHUB_TOKEN` in public repos and recommend restrictive defaults.

### 2d. Interaction limits

**Audit tasks:**
- Before Reddit exposure, should the owner enable GitHub interaction limits (restrict new users from opening issues/PRs)?
- Should issues be disabled entirely since CONTRIBUTING.md says contributions aren't open?
- Should Discussions be enabled as an alternative to Issues for community engagement?

---

## Audit Area 3: GitHub Pages Hosting Security (HIGH)

### 3a. Content Security Policy

**Audit tasks:**
- No CSP meta tag exists on any page. While there's no JavaScript to exploit, a CSP would provide defense-in-depth.
- Draft a CSP meta tag appropriate for this site:
  - `default-src 'none'` (deny everything by default)
  - `style-src 'self' https://fonts.googleapis.com` (local CSS + Google Fonts CSS)
  - `font-src https://fonts.gstatic.com` (Google Fonts font files)
  - `img-src 'none'` (no images exist)
  - `script-src 'none'` (no JavaScript exists — this explicitly blocks any injected scripts)
  - `connect-src 'none'` (no network requests)
  - `frame-ancestors 'none'` (prevent framing — note: `frame-ancestors` doesn't work in meta tags, but `frame-src 'none'` does)
- Verify this CSP wouldn't break any existing functionality. Test against every external resource loaded by the site.
- Provide the exact `<meta>` tag to add to every HTML file's `<head>`.

### 3b. Additional security headers via meta tags

**Audit tasks:**
- `X-Content-Type-Options: nosniff` — can this be set via meta tag, or only via HTTP headers? (GitHub Pages doesn't allow custom HTTP headers without Cloudflare/custom domain.)
- `Referrer-Policy` — currently unset. When users click Suno or Poetry Foundation links, the full page URL is sent as the referrer. Should `<meta name="referrer" content="strict-origin-when-cross-origin">` be added?
- `X-Frame-Options` — cannot be set via meta tag. The CSP `frame-ancestors` approach is the only option in a meta tag, but it has limited browser support in meta tags. Document this limitation.

### 3c. HTTPS enforcement

**Audit tasks:**
- GitHub Pages serves HTTPS by default. Verify that the "Enforce HTTPS" checkbox is enabled in the repo's Pages settings.
- Are there any hardcoded `http://` URLs in the codebase that should be `https://`? Search all files.

### 3d. Custom 404 page

**Audit tasks:**
- No custom `404.html` exists. GitHub Pages will show its default 404. Should a custom 404 be created that matches the site's style and links back to the homepage? This prevents a jarring experience if someone guesses a URL.

---

## Audit Area 4: External Link Integrity (MEDIUM)

### 4a. Link inventory and validation

**Audit tasks:**
- Compile a complete list of every external URL in the codebase (HTML files + README.md).
- Verify each URL returns HTTP 200. Flag any broken links (404s, redirects, timeouts).
- Specific concern: Poetry Foundation and allpoetry.com URLs may change. thepoetryhour.com is a smaller site that may go offline. Flag any links to less-stable domains.

### 4b. Link security attributes

**Audit tasks:**
- Every external link should have `target="_blank" rel="noopener noreferrer"`. Search all HTML files for `<a` tags with `href="http` and verify these attributes are present.
- The README.md contains markdown links. These are rendered by GitHub's markdown renderer, which automatically adds `rel="noopener noreferrer"`. Confirm this is the case — no action needed for README links.

### 4c. Google Fonts as an external dependency

**Audit tasks:**
- `styles.css` loads Google Fonts via `@import url('https://fonts.googleapis.com/...')`. This is the site's only third-party runtime dependency.
- **Risk:** If `fonts.googleapis.com` is compromised or returns malicious CSS, it could affect rendering. This is an extremely low-probability event but worth documenting.
- **Alternative:** Self-host the font files. Download the WOFF2 files, add them to the repo, and replace the `@import` with local `@font-face` declarations. This eliminates the external dependency and improves load time. Document the trade-off (repo size increase vs. security/performance gain).
- Some song pages also have `<link rel="preconnect" href="https://fonts.googleapis.com">` and `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`. Verify these are consistent across all pages or flag inconsistencies.

---

## Audit Area 5: Content Integrity & Defacement Prevention (MEDIUM)

### 5a. Monitoring for unauthorized changes

**Audit tasks:**
- With the repo public and potentially receiving attention from Reddit, how would the owner know if someone submitted a malicious PR or if a compromised token was used to push directly?
- Recommend enabling GitHub notifications for all pushes to `main`.
- Recommend enabling "Watch" on the repo for all activity.
- Consider whether a simple GitHub Action that posts to a notification channel on any push would add value.

### 5b. robots.txt and sitemap

**Audit tasks:**
- No `robots.txt` exists. The site will be indexed by search engines (which is likely desired). Should a `robots.txt` be added that allows all crawling, or should any paths be restricted?
- No `sitemap.xml` exists. For a 37-page site, a sitemap would help search engines index all pages. Assess whether this is worth adding.

---

## Audit Area 6: Social Sharing & Open Graph Metadata (MEDIUM)

This is relevant because the site is about to be shared on Reddit.

### 6a. Open Graph tags

**Audit tasks:**
- No Open Graph tags exist on any page. When shared on Reddit, Discord, Twitter/X, etc., the link preview will either be blank or use the first text it finds.
- Recommend adding to at minimum `index.html`:
  - `<meta property="og:title" content="...">`
  - `<meta property="og:description" content="...">`
  - `<meta property="og:type" content="website">`
  - `<meta property="og:url" content="...">`
- Note: `og:image` requires an actual image file. Assess whether a simple social card image should be created and added to the repo.
- Should individual song pages have unique OG tags? Assess effort vs. value.

### 6b. Twitter/X card metadata

**Audit tasks:**
- No `<meta name="twitter:card">` tags exist. If the link is shared on Twitter/X, there will be no rich preview.
- Recommend adding Twitter card meta tags alongside OG tags.

---

## Audit Area 7: CSS-Specific Security (LOW)

### 7a. CSS injection vectors

**Audit tasks:**
- The site uses no JavaScript, so CSS is the only code that executes. Review `styles.css` for:
  - Any `url()` references beyond Google Fonts
  - Any `expression()` or `-moz-binding` (legacy CSS execution vectors)
  - Any `@import` beyond the Google Fonts import
- Verify that no HTML file has inline `<style>` blocks (a few song prompt pages may use inline styles — check `style=` attributes).

### 7b. CSS custom properties

**Audit tasks:**
- `styles.css` uses CSS custom properties (`:root` variables). These are all hardcoded values. Confirm no custom property value could be influenced by external input (in this architecture, they can't — but document this for completeness).

---

## Audit Area 8: Dead Code, Orphaned Files & Repo Hygiene (LOW)

### 8a. .gitignore

**Audit tasks:**
- No `.gitignore` file exists. While the repo is clean, a `.gitignore` prevents accidental commits of OS files (`.DS_Store`, `Thumbs.db`), editor configs (`.vscode/`, `.idea/`), or any future build artifacts.
- Provide a recommended `.gitignore` for this project type.

### 8b. Orphaned references

**Audit tasks:**
- The `vol2/index.html` page says "1 of 12 songs" in the hub card on `index.html`, but Volume 2 now has 2 songs. Check if the hub page song count is accurate.
- Search for any HTML pages that reference files or anchors that don't exist (broken internal links).
- Check that all "Next Song" / "Previous Song" navigation links in song pages point to valid pages and are in the correct order.

### 8c. SECURITY_REVIEW_PROMPT.md

**Audit tasks:**
- This file is in the repo root and will be served by GitHub Pages. While it's documentation and not sensitive, it reveals audit methodology to potential attackers.
- **Recommendation:** Either add it to `.gitignore` (won't help since it's already committed), move it out of the repo, or accept that it's public. Assess the risk.

### 8d. Typos in content

**Audit tasks:**
- `prompts.html` line 37 has "multple" (should be "multiple"). Search all HTML files for obvious typos that would undermine credibility when shared publicly.

---

## Audit Area 9: Accessibility (LOW)

### 9a. Semantic HTML

**Audit tasks:**
- Verify all pages use semantic HTML5 elements (`<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`).
- Check heading hierarchy — does every page have exactly one `<h1>` and a logical heading order?
- Are navigation links properly grouped in `<nav>` elements?

### 9b. External link indicators

**Audit tasks:**
- External links (Suno, Poetry Foundation, etc.) open in new tabs via `target="_blank"`. Is there a visual indicator or `aria-label` that tells users the link opens externally?
- Screen reader users should know when a link leaves the site.

### 9c. Color contrast

**Audit tasks:**
- The accent color is `#9b1b30` (deep red) on `#fafafa` background. Check WCAG AA contrast ratio.
- `--text-dim: #777777` on `#fafafa` — this may fail WCAG AA for small text. Check and flag.
- `--text-secondary: #4a4a4a` on `#fafafa` — check contrast.

---

## Format your review as:

1. **Executive Summary:** Overall security posture, risk level for Reddit exposure, and the single most important action to take before sharing publicly.

2. **Critical Issues (act before sharing):** Personal information exposure, account security gaps, and anything that could be exploited by a motivated individual who sees the Reddit post.

3. **High Issues (act soon after sharing):** Repository hardening, CSP, branch protection — things that reduce ongoing risk.

4. **Medium Issues (act when convenient):** Link integrity, social sharing metadata, content accuracy.

5. **Low Issues (nice to have):** Accessibility, CSS hygiene, repo tidiness.

6. **Actionable Fixes:** For every Critical and High issue, provide the exact commands, file contents, or GitHub settings changes needed. No vague recommendations — concrete steps the owner can execute immediately.

7. **GitHub Settings Checklist:** A single checklist of every GitHub repo and account setting that should be verified or changed, with the exact navigation path (Settings > ...) for each.
