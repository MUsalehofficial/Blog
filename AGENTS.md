# Blog publishing instructions

Use these instructions for HTB writeups and technical articles in this repository.

## Safety and approval gates

- Preserve `_config.yml`, the Chirpy theme, the custom domain, and `.github/workflows/pages-deploy.yml` unless the user explicitly requests a change.
- Never publish HTB content before the machine is retired/public and the user confirms publication is allowed.
- Redact flags, passwords, tokens, private IP addresses, personal data, and active credentials. Use obvious placeholders where a secret is needed for explanation.
- First return the complete article as a draft in chat. Do not create a branch, commit, pull request, merge, or deploy until the user approves the draft.
- After draft approval, create a branch named `post/<slug>`, add the article under `_drafts/<slug>.md`, and open a draft pull request for review.
- Only after explicit publish approval, move the file to `_posts/YYYY-MM-DD-<slug>.md`, update the pull request, and make it ready for review. Do not merge unless the user explicitly asks.
- A merge to `main` is the publication event. The existing `Build and Deploy` workflow handles production deployment.

## Post requirements

- Start from `_templates/htb-writeup.md` or `_templates/tech-article.md`.
- Use an accurate, specific title and a unique lowercase kebab-case slug.
- Keep `description` between 120 and 160 characters, written for humans rather than stuffed with keywords.
- Use exactly two hierarchical categories. Reuse existing category and tag spelling where possible.
- Add focused tags, normally 3–8.
- Set `image.path` to a repository-local 1200 × 630 social image and write meaningful `image.alt` text. If no image exists, keep a clear TODO and flag it during review; do not invent a path.
- Prefer original explanations and reproducible commands. Distinguish observed results from assumptions and cite external facts.
- For HTB posts, include the authorization note and avoid unnecessary spoilers in the title or description.

## Review before publication

Check technical accuracy, secret redaction, filenames, front matter, image paths, internal links, and Markdown rendering. Confirm `bundle exec jekyll build --drafts` and HTML-Proofer pass before requesting publication approval.
