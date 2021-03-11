## Thank you!

We appreciate your contribution to this project! Please be sure to read the **contributing guide** to learn how you can help make this project better!

If you've created this issue with the 'bug' label, rest assured that we've added it to backlog for triage.

name: Learning GitHub Script

on:
  issues:
    types: [opened]

jobs:
  comment:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Comment on new issue
        uses: actions/github-script@0.8.0
        with:
          github-token: ${{secrets.GITHUB_TOKEN}}
          script: |
             const fs = require('fs')
             const issueBody = fs.readFileSync(".github/ISSUE_RESPONSES/comment.md", "utf8")
             github.issues.createComment({
             issue_number: context.issue.number,
             owner: context.repo.owner,
             repo: context.repo.repo,
             body: issueBody
             })

      - name: Add issue to project board
        if: contains(github.event.issue.labels.*.name, 'bug')
        uses: actions/github-script@0.8.0
        with:
          github-token: ${{secrets.GITHUB_TOKEN}}
          script: |
            github.projects.createCard({
            column_id: 13329579,
            content_id: context.payload.issue.id,
            content_type: "Issue"
            });
