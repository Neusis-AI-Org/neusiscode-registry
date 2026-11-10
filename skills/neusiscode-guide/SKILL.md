---
name: neusiscode-guide
description: Neusis Code install, upgrade and uninstall; the Neusis Cloud provider, models and API key; the Neusis registry. For other configuration, use customize-neusiscode.
---

## Install and upgrade

- macOS and Linux: `curl -fsSL https://get.neusis.ai/install.sh | bash`. It needs bash; piped to `sh` it fails on Debian and Ubuntu.
- Windows (PowerShell): `irm https://get.neusis.ai/install.ps1 | iex`
- Upgrade: re-run the same command. There is no `upgrade` command and no auto-update.
- Uninstall: there is no command. Delete `~/.neusis/bin`, then remove the `# Neusis Code` line and the `export PATH` line below it from each of `~/.bashrc`, `~/.bash_profile`, `~/.zshrc` and `~/.profile` that has them. On Windows, delete `%USERPROFILE%\.neusis\bin` and remove it from the user `Path`. Neusis Code's own files are in `~/.config/neusiscode` (settings), `~/.local/share/neusiscode` (sessions, logs), `~/.cache/neusiscode` and `~/.local/state/neusiscode`; Windows uses the same folders under `%USERPROFILE%`.

## Neusis Cloud

- The provider is `neusiscode`. The installer sets the default model `neusiscode/auto`: Neusis Cloud picks a model from the session's first message and keeps it for that session.
- `neusiscode models neusiscode` lists the models. Switch with `/models` in the TUI, or set `"model": "neusiscode/<id>"`.
- The API key is `provider.neusiscode.options.apiKey` in `~/.config/neusiscode/neusiscode.json`. Edit that value to change it; re-running the installer keeps the old key.
- The `websearch` tool is off by default on Neusis Cloud. `webfetch` works.

## Neusis registry

- The installer's `registry` key syncs https://github.com/Neusis-AI-Org/neusiscode-registry: the Neusis Cloud models and shared skills, agents and commands.
- It sits beneath the user's own config, so their settings win, except `disabled_providers`, `instructions` and `plugin`, which are combined with the registry's.
- Releases after v0.3.1 refresh it in the background and ask for a restart when it changes; v0.3.1 and earlier download it at every start.

Docs: https://docs.neusis.ai/docs/neusiscode/overview/
