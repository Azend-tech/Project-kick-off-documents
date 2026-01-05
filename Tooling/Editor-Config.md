# Editor Configuration

This document standardizes IDE settings across the team so everyone's code looks consistent and follows the same rules.

## Shared `.editorconfig` File

Create an `.editorconfig` file in the root of your repository with settings like this:

```ini
root = true
[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{js,ts,jsx,tsx,cs}]
indent_style = space
indent_size = 2

[*.cs]
indent_size = 4
```

This tells every editor (VS Code, Visual Studio, JetBrains IDEs, etc.) to use UTF-8 encoding, Unix line endings, and consistent indentation.

## VS Code Setup

Install these extensions: ESLint, Prettier (if your team uses it), EditorConfig, GitLens, commitlint support, YAML, and Markdown linters.

Add these settings to your workspace config (`.vscode/settings.json`):

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "files.eol": "\n",
  "editor.tabSize": 2,
  "eslint.validate": ["javascript", "typescript", "typescriptreact"],
  "files.trimTrailingWhitespace": true
}
```

This automatically formats code when you save, enforces Unix line endings, and validates your code with ESLint as you type.

## Visual Studio

Enable `.editorconfig` support in Tools > Options > Text Editor. Enforce code style rules with Roslyn analyzers (.NET) or editor config settings.

## Project-Specific Overrides
- Formatters: Prettier for JS/TS/React; dotnet-format for C#; use ESLint for linting JS/TS.
- Tabs: 2 spaces for JS/TS/React; 4 spaces for C#; align .editorconfig accordingly.
- Analyzers: enable Roslyn analyzers in .NET solutions; enable ESLint recommended rules in frontend.
