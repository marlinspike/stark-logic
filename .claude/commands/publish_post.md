# Publish Blog Post

Move a draft blog post to published status and rebuild the site.

## Input

Post identifier: $ARGUMENTS

This can be a slug (e.g., "building-ai-tools-that-last"), a partial title, or left blank to list all drafts.

Only publish one post at a time.

## Process

1. **Find drafts**: Search `content/blog/` for posts with `draft: true` in their front matter.

2. **Match**:
   - If no arguments are given, list all drafts and ask the user which one to publish.
   - If arguments are given, match against slugs (filenames) using partial/substring matching. For example, "fine-tuning" should match "fine-tuning-vs-prompting.md".
   - If exactly one draft matches, proceed with that post.
   - If multiple drafts match, show the user all matches (title, date, slug) and ask them to pick one. Do not publish multiple posts.
   - If no drafts match, tell the user and list all available drafts.

3. **Review**: Show the user the matched post's title, date, and summary, and ask for confirmation before publishing.

4. **Publish**: Change `draft: true` to `draft: false` in the post's front matter.

5. **Build**: Run `hugo` to rebuild the site into `docs/`.

6. **Confirm**: Let the user know the post is now live and included in the build.
