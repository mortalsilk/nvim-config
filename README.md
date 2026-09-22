# LazyVim configuration

```text
                         How LazyVim is organized

  Neovim
    |
    `-- init.lua
          |
          `-- config/lazy.lua
                |
                +-- bootstraps lazy.nvim -------- plugin manager
                |
                `-- builds the plugin spec
                      |
                      +-- LazyVim/LazyVim
                      |     |
                      |     +-- defaults (options, keymaps, autocmds)
                      |     +-- core plugins and sensible integrations
                      |     `-- optional extras selected in lazyvim.json
                      |
                      `-- lua/plugins/*.lua
                            |
                            +-- add plugins
                            +-- override LazyVim plugin options
                            `-- disable or replace defaults

  Load order: options -> lazy.nvim/LazyVim -> plugin specs -> VeryLazy config
```

A personal [LazyVim](https://www.lazyvim.org/) setup centered on Java, Python,
TypeScript, and web development. It stays close to LazyVim's defaults and adds a
small, explicit customization layer for appearance, indentation, Emmet, and
WakaTime.

## Configuration at a glance

```text
  This repository
  |
  +-- LazyVim v8 base
  |     +-- default editor behavior
  |     +-- default keymaps and autocmds
  |     `-- standard UI, LSP, completion, Git, and search plugins
  |
  +-- Language extras
  |     +-- Java
  |     +-- Python
  |     `-- TypeScript
  |
  +-- Web tooling
  |     `-- Emmet language server
  |           `-- HTML, CSS, SCSS, Less, JSX/TSX, Vue, Svelte, Astro
  |
  +-- Appearance
  |     `-- github_light_high_contrast
  |
  +-- Editing
  |     `-- 4-space tabs, expanded to spaces
  |
  `-- Activity tracking
        `-- WakaTime
```

## Repository layout

```text
.
|-- init.lua                    # Neovim entry point
|-- lazyvim.json                # LazyVim version and enabled extras
|-- lazy-lock.json              # Reproducible plugin commit lockfile
|-- stylua.toml                 # Lua formatting rules
|-- .neoconf.json               # Neoconf/lua_ls project settings
`-- lua
    |-- config
    |   |-- lazy.lua            # lazy.nvim bootstrap and global plugin policy
    |   |-- options.lua         # Personal Neovim options
    |   |-- keymaps.lua         # Reserved for personal keymaps
    |   `-- autocmds.lua        # Reserved for personal autocommands
    `-- plugins
        |-- colorscheme.lua      # GitHub theme configuration
        |-- emmet.lua           # Emmet LSP filetypes
        |-- wakatime.lua         # WakaTime integration
        `-- example.lua          # Disabled LazyVim example/template
```

## What is customized

### Language support

The following LazyVim extras are enabled in `lazyvim.json`:

- `lazyvim.plugins.extras.lang.java`
- `lazyvim.plugins.extras.lang.python`
- `lazyvim.plugins.extras.lang.typescript`

These extras let LazyVim assemble the relevant language-server, Treesitter,
formatting, linting, debugging, and ecosystem integrations. Exact plugin commits
are captured in `lazy-lock.json`.

Emmet is also registered through `nvim-lspconfig` for:

```text
html  css  scss  less  javascriptreact  typescriptreact
vue   svelte  astro
```

### Appearance

The configuration installs `projekt0n/github-nvim-theme` at high startup
priority and selects:

```vim
github_light_high_contrast
```

LazyVim's normal `tokyonight` and `habamax` fallback colorschemes remain listed
in the bootstrap configuration.

### Indentation and formatting

Editor indentation is globally set to four spaces:

```text
tabstop=4  shiftwidth=4  softtabstop=4  expandtab=true
```

Lua source in this repository is formatted by StyLua with two-space indentation
and a 120-column width. This is separate from the runtime editor defaults above.

### Keymaps and autocommands

No personal keymaps or autocommands are currently defined. LazyVim's defaults
remain active, and `lua/config/keymaps.lua` and `lua/config/autocmds.lua` are the
intended extension points.

### Plugin behavior

- Custom plugins load eagerly unless a plugin spec says otherwise.
- WakaTime loads during startup.
- lazy.nvim checks for updates periodically without update notifications.
- Plugin versions track Git commits rather than semantic-version tags.
- `gzip`, `tarPlugin`, `tohtml`, `tutor`, and `zipPlugin` are removed from the
  runtime path for a leaner startup.
- `lua/plugins/example.lua` returns an empty spec immediately; everything below
  that return is reference material and has no effect on the active config.

## Startup flow

```text
  nvim
    |
    +-- require("config.lazy")
    |     |
    |     +-- clone stable lazy.nvim if it is missing
    |     +-- prepend lazy.nvim to the runtime path
    |     +-- import LazyVim's core plugin specifications
    |     `-- import every specification under lua/plugins/
    |
    +-- read lazyvim.json and enable language extras
    |
    +-- resolve commits from lazy-lock.json
    |
    `-- load deferred LazyVim configuration at the appropriate events
```

## Installation

### Prerequisites

- A recent Neovim release compatible with LazyVim v8
- Git
- A [Nerd Font](https://www.nerdfonts.com/) for the intended icon experience
- Language-specific runtimes needed by the enabled Java, Python, and TypeScript
  tooling

Back up any existing Neovim configuration, then clone this repository:

```sh
mv ~/.config/nvim ~/.config/nvim.backup
mv ~/.local/share/nvim ~/.local/share/nvim.backup
git clone <repository-url> ~/.config/nvim
nvim
```

On first launch, lazy.nvim is bootstrapped automatically and installs the plugin
set. Open `:Lazy` to inspect installation progress. WakaTime may prompt for its
API key during initial setup.

## Everyday maintenance

Useful commands include:

| Command | Purpose |
| --- | --- |
| `:Lazy` | Inspect, update, sync, or profile plugins |
| `:LazyExtras` | Review and change LazyVim extras |
| `:Mason` | Inspect installed language tools |
| `:LspInfo` | Inspect language servers attached to the current buffer |
| `:checkhealth` | Diagnose Neovim and provider issues |

After intentional plugin updates, commit the resulting `lazy-lock.json` change
to keep installations reproducible.

## Extending the config

```text
  Need to change...
  |
  +-- an editor option?       -> lua/config/options.lua
  +-- a key binding?          -> lua/config/keymaps.lua
  +-- an automatic action?    -> lua/config/autocmds.lua
  +-- a plugin or override?   -> lua/plugins/<name>.lua
  `-- a language extra?       -> :LazyExtras / lazyvim.json
```

Keep additions as small plugin specs rather than editing LazyVim itself. This
preserves the upstream defaults and makes future LazyVim updates easier to audit.

## License

See [LICENSE](LICENSE).
