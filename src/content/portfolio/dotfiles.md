---
title: "Dotfiles"
date: "2026-03-13T00:00:00.000Z"
description: "Personal configuration files synced across machines via Git. Includes Claude Code settings, custom commands, and shell configurations."
technologies: ["Bash", "Git", "Lua", "Node.js"]
logo: "/portfolio/logos/dotfiles.svg"
github: "https://github.com/hbeneke/dotfiles"
license: "GPL-3.0"
licenseUrl: "https://www.gnu.org/licenses/gpl-3.0.html"
featured: false
order: 3
version: "0.1.5"
changelog:
  - version: "0.1.5"
    date: "2026-08-05T00:00:00.000Z"
    changes:
      - "Global Claude Code instructions and custom keybindings are now part of the repo and get symlinked on install"
      - "Added a commit-analyzer subagent that reads the diff and writes the conventional commit message"
      - "Synced the Claude settings actually in use: vim editor mode, fullscreen interface, medium effort level and the enabled plugins"
      - "Neovim now soft-wraps long lines at word boundaries and keeps the indentation on wrapped lines"
      - "Refreshed the README with the new files, the extra agent and a section describing the settings"
  - version: "0.1.4"
    date: "2026-06-30T00:00:00.000Z"
    changes:
      - "The sync script now warns instead of failing when main already contains everything from develop"
      - "Fixed develop being left behind after a release: the version bump is now fast-forwarded back automatically"
      - "Applied the same fix to the git-flow template shipped to other projects"
  - version: "0.1.1"
    date: "2026-05-12T00:00:00.000Z"
    changes:
      - "Added a browsable static site that renders the dotfiles tree, so they can be read without cloning the repo"
      - "Added AstroNvim configuration with a cross-platform installer"
      - "Added a set of Claude Code subagents routed to the cheapest capable model instead of running everything on the largest one"
      - "Added a git-flow bootstrap CLI that sets up the main/develop flow, hooks and version bumping in any project"
      - "Rewrote the statusline in Node.js so it works on Windows, macOS and Linux alike"
      - "Split the repo into dotfiles content and website, and published it under GPL-3.0"
  - version: "0.1.0"
    date: "2026-03-13T00:00:00.000Z"
    changes:
      - "Initial setup with Claude Code configuration, custom commands, and install script"
---
