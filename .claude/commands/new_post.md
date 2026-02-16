# New Blog Post

Write a blog post for reuben.cleet.us based on the key ideas provided below.

## Input

Key ideas: $ARGUMENTS

## Process

1. **Research**: Use WebSearch to research the key ideas thoroughly. Look for recent developments, concrete data points, specific examples, and named sources. Gather enough material to write with authority and specificity.

2. **Generate a slug**: Create a URL-friendly slug from the topic (e.g., "building-ai-tools-that-last").

3. **Write the post**: Create the blog post as a Markdown file at `content/blog/<slug>.md` with Hugo front matter:
   ```
   ---
   title: "<Post Title>"
   date: <today's date in YYYY-MM-DD format>
   summary: "<1-2 sentence summary for the blog listing>"
   draft: true
   ---
   ```

   These posts are often elaborations on Reuben's LinkedIn posts — taking a short-form idea and expanding it into a full argument with depth, examples, and nuance.

   Writing guidelines:
   - Write in first person from Reuben's perspective as a hands-on technologist
   - Be specific and concrete — use real examples, real numbers, real tools
   - Keep it practical — readers should learn something useful
   - Aim for 800-3000 words depending on the topic's depth
   - Use short paragraphs and clear structure with `##` headings
   - Have opinions and take positions — don't just report neutrally
   - Vary sentence length and structure naturally
   - No emojis, no promotional language, no filler phrases

   Typography:
   - Use **bold** sparingly to emphasize key terms or phrases that carry the argument
   - Use *italics* sparingly for nuance, emphasis on a single word, or when referencing titles
   - Don't overdo either — if everything is emphasized, nothing is

4. **Humanize**: After writing the post, use the `humanizer` skill to review and clean up the post so it reads naturally and doesn't sound AI-generated. Apply the skill to the blog post file you just created.

5. **Confirm**: Show the user the final post title, slug, word count, and a brief summary of what was written. Remind them the post is a **draft** and to preview with `hugo server --buildDrafts`, then use `/publish_post` when ready.
