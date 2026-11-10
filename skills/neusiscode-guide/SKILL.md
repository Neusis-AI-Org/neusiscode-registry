---
name: neusiscode-guide
description: Neusis Code product facts - installing, upgrading or uninstalling Neusis Code, the Neusis Cloud provider, its models and API key, and the Neusis registry. For agents, skills, commands, MCP servers, permissions or other configuration, use customize-neusiscode.
---

## Install and upgrade

- macOS and Linux: `curl -fsSL https://get.neusis.ai/install.sh | bash`. It needs bash; piped to `sh` it fails on Debian and Ubuntu.
- Windows (PowerShell): `irm https://get.neusis.ai/install.ps1 | iex`
- Upgrade: re-run the same command. There is no `upgrade` command and no auto-update.
- Uninstall: there is no command. Delete `~/.neusis/bin` and the `# Neusis Code` PATH line the installer added to the shell profile (Windows: `%USERPROFILE%\.neusis\bin` and its entry in the user `Path`). Settings are in `~/.config/neusiscode`, sessions in `~/.local/share/neusiscode`; Windows uses the same folders under `%USERPROFILE%`.

## Neusis Cloud

- The provider is `neusiscode`. The installer sets the default model `neusiscode/auto`, which lets Neusis Cloud pick the model for each request.
- `neusiscode models neusiscode` lists the models. Switch with `/models` in the TUI, or set `"model": "neusiscode/<id>"`.
- The API key is `provider.neusiscode.options.apiKey` in `~/.config/neusiscode/neusiscode.json`. Edit that value to change it; re-running the installer keeps the old key.
- The `websearch` tool is not available on Neusis Cloud. `webfetch` works.

## Neusis registry

- The installer's `registry` key syncs https://github.com/Neusis-AI-Org/neusiscode-registry: the Neusis Cloud models and shared skills, agents and commands.
- It sits beneath the user's own config, so their settings win, except `disabled_providers`, which is combined with the registry's list.
- Neusis Code refreshes it in the background and shows a notice to restart when it changes.

Docs: https://docs.neusis.ai/docs/neusiscode/overview/
