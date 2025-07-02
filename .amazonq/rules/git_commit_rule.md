# Git Commit Rule

When I ask you to commit my code or changes:

1. Determine which files are new, modified, or deleted using `git status`
2. Stage all relevant files using `git add` for new/modified files or `git rm` for deleted files
3. Generate a commit message following these best practices:
   - Use the imperative mood ("Add feature" not "Added feature")
   - Start with a concise summary line (50 chars or less)
   - Include a more detailed explanation after a blank line if needed
   - Reference any relevant issue numbers with #issue-number
   - Structure the message in the format: `<type>(<scope>): <description>`
     - Types: feat, fix, docs, style, refactor, test, chore
     - Scope: optional component name affected (in parentheses)
4. Execute the commit with the generated message
5. Confirm the commit was successful and show the commit hash

Example of a good commit message:
```
feat(auth): implement OAuth2 authentication flow

- Add OAuth2 client configuration
- Create login and callback endpoints
- Store tokens securely in user session

Closes #123
```

Do not ask for confirmation before committing unless I specifically request it. Always show me the generated commit message before executing the commit.