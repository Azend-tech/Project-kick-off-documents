# Editor Config

Purpose: Standardize IDE settings for consistency (VS Code, Visual Studio).

Abbreviations: IDE (Integrated Development Environment), EOL (End of Line).

## .editorconfig (example)
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

## VS Code recommendations
- Extensions: ESLint, Prettier (if used), EditorConfig, GitLens, commitlint support, YAML, Markdown lint.
- Settings (workspace):
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

Rationale: consistent formatting reduces diff noise; EOL set to LF for cross-platform consistency; validation catches lint errors early.

## Visual Studio
- Enable .editorconfig respect.
- Enforce code style in solution (.editorconfig or analyzer rules).

## Project-Specific Overrides
- Formatters: Prettier for JS/TS/React; dotnet-format for C#; use ESLint for linting JS/TS.
- Tabs: 2 spaces for JS/TS/React; 4 spaces for C#; align .editorconfig accordingly.
- Analyzers: enable Roslyn analyzers in .NET solutions; enable ESLint recommended rules in frontend.
