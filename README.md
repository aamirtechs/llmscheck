# llmscheck

**Generators write llms.txt files. Nobody checks them. llmscheck is the auditor.**

Paste your `llms.txt` (and optionally your `robots.txt`) and get a graded audit in the browser: structure checks, link hygiene, and contradictions between the two files, scored 0 to 100 with a copyable Markdown report. Everything runs locally. Your files are never uploaded, stored, or sent anywhere.

Live tool: open `index.html` in any browser. No build, no server, no accounts.

## Why this exists

The llms.txt standard spread fast, and quality did not keep up:

- About **844,000 websites** had deployed llms.txt by November 2025. (Adoption tracking cited from llms.txt ecosystem reports, November 2025.)
- A **September 2026 StackScan census** found **19.9%** of llms.txt files are empty or describe a closed site.
- The same census counted about **548,000 sites** serving an llms.txt that contradicts their own robots.txt, inviting crawlers to pages the site also tells them not to crawl.

Generators are everywhere: every SEO suite and CMS plugin will happily write an llms.txt for you. A validator and auditor is the missing half. Nobody proofreads the file after the generator writes it, and the failure modes (empty files, duplicate links, relative URLs, robots contradictions) are exactly the ones a generator will not warn you about.

## The honest caveat

AI crawler demand for llms.txt is still low. Major AI crawlers do not send meaningful traffic because of llms.txt yet, and nobody can honestly promise AI visibility from this file. llmscheck sells **accuracy and hygiene**: if you publish an llms.txt, it should at least be correct, consistent, and not contradicting your robots.txt. That is the whole pitch.

## What it checks

Each check reports Pass, Warn, or Fail with a plain explanation:

- File is not empty
- File size sanity (with a pointer to llms-full.txt when the index gets huge)
- Exactly one H1 title
- Blockquote summary present
- H2 sections present, and no empty sections
- Links in Markdown list format under sections
- Every link is an absolute http(s) URL (relative links fail)
- No duplicate links
- No anchor fragments or tracking parameters on links
- Optional section used the way the spec intends
- Details area present before the first section
- Markdown sanity: unclosed code fences and stray HTML

**robots.txt contradiction check** (when you paste robots.txt too): every llms.txt link whose path falls under a robots.txt `Disallow` rule is flagged with the exact link and the rule it breaks. A reference to llms-full.txt also gets a warning to keep that file in sync.

Plus:

- Score 0 to 100 with a grade: Healthy, Needs work, or Broken
- A copyable audit report in Markdown, ready for an issue, ticket, or client email
- A "Try fetching a site" helper that attempts to load `https://<domain>/llms.txt` into the editor. Many sites block cross-origin reads, so a failed fetch shows a friendly paste-it-instead note and is never presented as a missing file
- A deliberately broken sample (missing H1, duplicate link, relative link, empty section, a robots.txt that contradicts a planted link) so you can watch every failure class fire in one click

## How to run

Open `index.html` in a browser. That is it. To use it on your own site, drop the file anywhere static hosting works (GitHub Pages included) or just run it from disk.

## Roadmap

- A command line version for CI pipelines: fail the build when llms.txt scores Broken
- Scheduled re-audits for agencies watching client sites
- Sitemap cross-check: links in llms.txt that no longer exist in sitemap.xml
- llms-full.txt consistency checks beyond the reference warning

## License

MIT, Copyright (c) 2026 Amir Hussain. See [LICENSE](LICENSE).

Part of a small family of local-first tools by the same author: SAFER USDOT Extractor, Load Board Fraud Lens, RateCon Shield, DetentionDesk, and LinkedIn Post Explainer.
