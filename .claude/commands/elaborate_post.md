# Elaborate LinkedIn Post into Blog Post

Take a LinkedIn post from the `LIPosts/` folder and expand it into a full blog post with deeper research, sharper analysis, and more fleshed-out ideas.

## Input

Post identifier: $ARGUMENTS

This can be a filename, partial filename, or left blank to list available posts in `LIPosts/`.

## Process

1. **Find the LinkedIn post**:
   - Look in the `LIPosts/` folder for matching files.
   - If arguments are given, match using partial/substring matching against filenames.
   - If exactly one file matches, use it.
   - If multiple files match, show the user all matches and ask them to pick one.
   - If no arguments are given, list all files in `LIPosts/` and ask the user which one to elaborate.
   - If no files match, tell the user and list what's available.

2. **Read and analyze**: Read the LinkedIn post carefully. Identify the core ideas, arguments, and any claims made. Note what's surface-level or underdeveloped — these are the areas to dig into.

3. **Research deeply**: Use WebSearch to research each key idea from the post. Go beyond what the LinkedIn post says:
   - Find concrete data, studies, and named sources that support or challenge the claims
   - Look for real-world examples, case studies, and specific tools/companies
   - Find counterarguments or nuances the short post couldn't cover
   - Look for recent developments that add context

4. **Generate a slug**: Create a URL-friendly slug from the topic.

5. **Write the post**: Create the blog post as a Markdown file at `content/blog/<slug>.md` with Hugo front matter:
   ```
   ---
   title: "<Post Title>"
   date: <today's date in YYYY-MM-DD format>
   summary: "<1-2 sentence summary for the blog listing>"
   draft: true
   ---
   ```

   The blog post should go *well beyond* the LinkedIn post. The LinkedIn post is a seed — the blog post is the full argument.

   Content approach:
   - Don't just restate the LinkedIn post at length — dig deeper into *why* the ideas matter
   - Challenge assumptions the LinkedIn post made, then resolve them with evidence
   - Add concrete examples, numbers, and named sources from your research
   - Explore implications and second-order effects the short post couldn't cover
   - Take stronger, more nuanced positions than the LinkedIn format allowed
   - Include practical takeaways readers can actually use

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

6. **Humanize**: After writing the post, use the `humanizer` skill to review and clean up the post so it reads naturally and doesn't sound AI-generated. Apply the skill to the blog post file you just created.

7. **Confirm**: Show the user the final post title, slug, word count, and a brief summary of what was written. Remind them the post is a **draft** and to preview with `hugo server --buildDrafts`, then use `/publish_post` when ready.
