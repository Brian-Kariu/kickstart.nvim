# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Neovim configuration based on **kickstart.nvim**, customized with additional plugins for Go, Python, TypeScript, database, and data tooling workflows. The main configuration lives in a single `init.lua` file (~1,325 lines).

## Architecture

- **Plugin manager:** lazy.nvim (auto-bootstraps on first run)
- **Leader key:** Space
- **Colorscheme:** tokyonight-night

### File Structure

- `init.lua` — All core config: options, keybindings, plugin specs, LSP/completion/formatting/debugging setup
- `lua/kickstart/plugins/` — Modular plugin configs (debug, lint, neo-tree, autopairs, gitsigns, indent_line)
- `lua/custom/plugins/` — User customizations added on top of kickstart defaults
- `lazy-lock.json` — Plugin version lockfile
- `.stylua.toml` — Lua formatter config (160 col, 2-space indent, single quotes)

### Plugin loading

All plugins are declared as lazy.nvim specs. Core plugins are defined inline in `init.lua`. Optional/modular plugins are imported from `lua/kickstart/plugins/` and `lua/custom/plugins/` via `require('lazy').setup({ import = ... })`.

## Language Tooling

### LSP Servers (configured in init.lua)

gopls, pyright (diagnostics off), ruff, ts_ls (formatting off), biome, lua_ls, dockerls, yamlls, astro-language-server

### Formatters (conform.nvim, format-on-save enabled)

| Language | Formatters |
|---|---|
| Lua | stylua |
| Python | ruff_format, ruff_fix, ruff_organize_imports |
| Go | gofumpt, goimports-reviser |
| JS/TS/JSON | biome |

### Linters (nvim-lint, triggers on BufEnter/BufWritePost/InsertLeave)

markdown (markdownlint-cli2), dockerfile (hadolint), json (jsonlint), text (vale), python (ruff), go (golangci-lint), js/ts (biomejs), yaml (yamllint), bash (shellcheck)

### Debugging (nvim-dap)

- **Go:** Delve (dlv) with remote attach support
- **Python:** debugpy with test class/method keybindings (`<leader>tc`, `<leader>tm`)
- **JS/TS:** vscode-js-debug (pwa-node, pwa-chrome adapters)

## Key Conventions

- Plugin configs that extend kickstart go in `lua/kickstart/plugins/`
- User-specific additions go in `lua/custom/plugins/`
- Mason auto-installs LSP servers, formatters, and linters listed in the `ensure_installed` tables
- Lua code style follows `.stylua.toml`: 160 column width, 2 spaces, single quotes, no call parentheses

## Dependencies

Neovim 0.10+, git, ripgrep, a Nerd Font, plus language-specific tools (go, npm/node, python3)
