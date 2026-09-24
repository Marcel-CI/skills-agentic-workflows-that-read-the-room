---
name: update-github-info
description: Keep the GitHub Info page current with reviewed GitHub Blog, Changelog, and Awesome Copilot workflow updates.
on:
  schedule: daily
  workflow_dispatch:
permissions:
  contents: read
  issues: read
  pull-requests: read
tools:
  github:
    toolsets:
      - repos
  web-fetch:
  bash:
    - "curl -fsSL https://github.blog/*"
    - "curl -fsSL https://awesome-copilot.github.com/workflows/"
  edit:
network:
  allowed:
    - defaults
    - github.blog
    - github.com
    - awesome-copilot.github.com
safe-outputs:
  create-pull-request:
    max: 1
    base-branch: main
    draft: true
---

# Update GitHub Info

Keep the repository's GitHub information page current and propose every change for Mona's review.

## Required workflow

1. Read `notes/mona-notes.md` from the repository using the GitHub repository API tools. Follow those notes when drafting the update.
2. Use the web-fetch tool to read:
   - https://github.blog/latest/
   - https://github.blog/changelog/
  - https://awesome-copilot.github.com/workflows/
  If web-fetch is unavailable in the session, use the restricted curl commands
  only for these three public URLs and do not use bash to read repository files.
3. Use the GitHub repository API tools, rather than terminal, CLI, or sandboxed commands, to read any repository guidance or reference files needed for this task.
4. Update `site/content/github-info.md` with concise, practical developer-focused information from the fetched sources. Mention the source for every Blog or Changelog update. Preserve the existing Markdown style and avoid unrelated changes.
5. Review the resulting diff for accuracy and scope. If there is a useful update, use the `create_pull_request` safe output to open a draft pull request targeting `main` for Mona to review. Include a concise summary of the sources and changes in the pull request body. Do not write directly to `main`.
6. If no meaningful update is available, do not create a pull request and report that no change was needed.
