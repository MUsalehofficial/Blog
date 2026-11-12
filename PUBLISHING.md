# AI-assisted publishing workflow

This workflow keeps the live site unchanged until a reviewed post is merged to `main`.

## One-time connection

Connect your AI assistant to GitHub and grant access to `MUsalehofficial/Blog`. Repository contents and pull requests are sufficient for drafting and review; workflow or administration access is not required for ordinary posts.

## Reusable request

Use a prompt like this with your connected AI assistant:

> Using @GitHub and the publishing instructions in `MUsalehofficial/Blog`, draft an HTB writeup (or technical article) from the notes I provide. Use the appropriate `_templates` file, create SEO title and description, choose focused categories and tags, and propose a 1200 × 630 social image path and alt text. Redact secrets and flag unsupported claims. Return the complete draft in chat and stop for my review. Do not write to GitHub, commit, open a pull request, merge, or deploy until I explicitly approve each stage.

## Review and publish stages

1. **Draft:** The assistant returns the full article in chat. Review accuracy, tone, redaction, SEO metadata, and the social image plan.
2. **Repository review:** After approval, ask it to create `post/<slug>`, save the article in `_drafts/<slug>.md`, and open a draft pull request. This does not publish the post.
3. **Publish approval:** When the pull request is ready, explicitly approve publication. The post is moved to `_posts/YYYY-MM-DD-<slug>.md`, `published` is set to `true` or removed, and the review checks must pass.
4. **Merge:** Ask for the pull request to be merged only after the final preview and checks are satisfactory.
5. **Automatic deployment:** The existing `Build and Deploy` workflow runs on the merge to `main`, tests the generated site, and deploys GitHub Pages.

If the deployment fails, leave the previous live site in place, inspect the failed job, and fix the pull request or follow-up branch rather than changing Pages settings.
