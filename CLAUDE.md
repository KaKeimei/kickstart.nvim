# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Neovim configuration based on [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) — a minimal, well-commented starting point designed to be read and modified. The plugin manager is `lazy.nvim`, which auto-installs on first Neovim launch.

## Architecture

**Entry point**: `init.lua` — a single ~1000-line file containing all core settings, keymaps, autocommands, and plugin specs. This is intentional for kickstart: everything visible in one place.

**Plugin organization**:
- Inline in `init.lua` — most plugin specs live here
- `lua/kickstart/plugins/` — optional plugins that can be uncommented in init.lua (autopairs, gitsigns, neo-tree, indent_line, debug, lint)
- `lua/custom/plugins/init.lua` — user-added plugins go here (currently empty)

**Filetype plugins**: `ftplugin/java.lua` configures `nvim-jdtls` (Eclipse JDT LS) with Lombok support, pointing to a local jdt-language-server installation at `/Users/qiming.he/workspace/folder`.

**Lock file**: `lazy-lock.json` pins exact plugin versions for reproducibility.

## Key Conventions

- **Leader key**: `<Space>`
- **Which-key groups**: `<leader>c` (code), `<leader>d` (document), `<leader>r` (rename), `<leader>s` (search/telescope), `<leader>w` (workspace), `<leader>t` (toggle), `<leader>h` (git hunks)
- **LSP keymaps** are defined inside a `LspAttach` autocmd so they only apply when an LSP is active
- Arrow keys are intentionally disabled to enforce hjkl navigation
- Paste in visual mode (`p`) does not yank the replaced text

## Lua Formatting

`.stylua.toml` is present — use `stylua` for formatting Lua files:
```
stylua <file>.lua
```

## Adding Plugins

1. **Optional kickstart plugins**: uncomment the relevant line near the bottom of `init.lua` (e.g., `require 'kickstart.plugins.debug'`)
2. **Custom plugins**: add specs to `lua/custom/plugins/init.lua`
3. **Inline**: add a plugin spec table directly in the `lazy.setup({...})` call in `init.lua`

## LSP Configuration

LSPs are managed by Mason. Currently configured servers:
- `lua_ls` — Lua (with Neovim API awareness via `lazydev.nvim`)
- `yamlls` — YAML with schema store (GitHub Actions, Kubernetes, and custom schemas)

To add a new LSP, add its name to the `servers` table around line 632 in `init.lua`.

## Health Check

Run `:checkhealth` (or `:checkhealth kickstart`) inside Neovim to verify external dependencies: git, make, unzip, ripgrep, and Neovim 0.10+.
