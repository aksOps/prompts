---
description: 'Strict Single-Line Standard for Git Commit Messages.'
applyTo: '**'
---

# Git Commit Message Standards

## Your Mission
Generate precise, semantic, and standardized commit messages. You must strictly adhere to the **Single-Line Conventional Commits** pattern. Commit messages are the history of the project; keep them clean, readable, and parseable.

## Introduction
We enforce a strict format to ensure our git history is readable by both humans and machines. Every commit message must be a single line that clearly describes *what* changed and *why*, using the standard Conventional Commits types.

## Message Fundamentals

### The Strict Pattern
Every commit message **MUST** follow this exact regex pattern:
`^(feat|fix|docs|style|refactor|test|chore|perf|ci|build|revert)(\([a-z0-9-]+\))?: [a-z].+$`

**Format:**
```
<type>(<scope>): <subject>
```

**Rules:**
1.  **Single Line Only:** No body, no footer. One line.
2.  **Length:** Maximum 72 characters.
3.  **Lowercase:** Type and scope must be lowercase. Subject must start lowercase.
4.  **Imperative Mood:** "add feature" not "added feature".
5.  **No Trailing Punctuation:** Do not end with a period.

### Allowed Types
- `feat`: A new feature
- `fix`: A bug fix
- `docs`: Documentation only changes
- `style`: Changes that do not affect the meaning of the code (white-space, formatting, etc)
- `refactor`: A code change that neither fixes a bug nor adds a feature
- `perf`: A code change that improves performance
- `test`: Adding missing tests or correcting existing tests
- `chore`: Changes to the build process or auxiliary tools and libraries
- `ci`: Changes to our CI configuration files and scripts
- `build`: Changes that affect the build system or external dependencies

## Safety & Security

### Data Protection
- ❌ **NEVER** include secrets, tokens, or keys in a commit message.
- ❌ **NEVER** include diff dumps or large code snippets.
- ✅ **ALWAYS** sanitize PII (Personally Identifiable Information).

## Examples

### ✅ Good Examples
- `feat(auth): add jwt token validation`
- `fix(login): handle null user response`
- `docs(readme): update installation steps`
- `style(css): format landing page`
- `chore(deps): upgrade react to v18`

### ❌ Bad Examples
- `Fixed bug` (Unknown type, vague)
- `Feat: Added Login` (Capitalized type, past tense)
- `refactor: cleanup.` (Trailing period)
- `wip` (Not allowed)
- `update file` (Meaningless)

## Anti-patterns
- **"WIP":** Work in progress commits should be squashed or properly named before pushing.
- **"Misc changes":** If you can't name it, you probably reviewed it poorly.
- **Multi-line:** Do not add a body. If it requires a body, break it into smaller commits.
