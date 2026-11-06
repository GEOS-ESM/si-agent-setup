# Mission AI with OpenCode V2

Use this guide only if you are authorized to use NASA Mission AI. It configures the Mission AI LiteLLM provider only and does not configure NASA Chat. If you do not already have a Mission AI API key, contact the SI Team for more information.

## Install

On NASA Discover, OpenCode is already installed. Load it with:

```bash
module load other/opencode
```

Check `opencode --version` after loading the module; these instructions require V2. For optional automatic loading in interactive shells, see [Loading modules on Discover](../clients/discover-modules.md).

On a Mac, install OpenCode V2 with Homebrew:

```bash
brew install anomalyco/tap/opencode-v2
```

Elsewhere, install OpenCode using the [OpenCode V2 documentation](https://opencode.ai/v2/docs).

## Sign in to GitHub

Follow the [GitHub CLI setup guide](../clients/github-cli.md) to let the agent work with GitHub issues and pull requests. This login is separate from your Mission AI API key and does not connect an OpenCode model provider.

## Configure

1. Merge the [Mission AI OpenCode V2 template](../../templates/mission-ai/opencode.jsonc.example) into `~/.config/opencode/opencode.jsonc`. Do not replace unrelated providers, plugins, or preferences.
2. Configure credentials using one of the approved methods below.
3. Start OpenCode and select an available Mission AI model with `/models`. The template separates Claude/Gemini (`nasa_mission_ai`) from GPT (`nasa_mission_ai_openai`), using different provider packages. For example, `nasa_mission_ai_openai/gpt-5.6-luna` selects GPT through Mission AI.
4. Run `opencode models` to check the available model IDs. Send a harmless prompt to the selected model to verify authentication; appearing in the list alone is not an end-to-end test.

### Credentials

Follow the [credential requirements](../../SECURITY.md#credential-handling) for your service and device. Never put a literal key in this template or commit credentials.

- **`/connect`:** if local credential storage is approved, run `/connect` separately for `nasa_mission_ai` and/or `nasa_mission_ai_openai`. If you previously added `settings.apiKey` to those blocks, remove it first.
- **Environment alternative:** the template's provider `env` entries recognize `NASA_MISSION_AI_API_KEY` without further config edits. Supply it through an approved mechanism to the **OpenCode server process**. For a terminal-only trial, launch `opencode --standalone` from an environment that already has it. A variable in a new terminal does not automatically update an existing background service; see the [environment instructions](https://opencode.ai/v2/docs/cli/providers#environment) for a managed service. Avoid typing literal keys into shell commands or history.
- **Both:** for each provider, a saved `/connect` account takes precedence over its provider environment connection. To use the environment connection instead, remove the saved account with `/connect` or `opencode auth logout`.

OpenCode saves `/connect` credentials in the **server's SQLite database**, usually `~/.local/share/opencode/opencode.db`. This is a local credential store, not shared configuration; protect the database and its backups, and do not commit or share them. Environment variables avoid that account store but can be exposed through processes, diagnostics, or shell history. Follow your service's approved method. See [credential storage](https://opencode.ai/v2/docs/cli/providers#storage).

OpenCode reloads configuration changes automatically; use `/reload` if needed. Terminal preferences are separate, in `~/.config/opencode/cli.json`.

The template does not set a default model; choose an available Mission AI model in `/models`.

The OpenCode base URL shown in the template matches a working OpenCode setup without `/v1`. The VS Code template uses a full `/v1/chat/completions` URL; use the URL for the specific client you are configuring. If an existing local OpenCode provider has a different ID, use that ID in your model selections and authenticate the corresponding provider.

The template blocks `opencode`, `azure`, and `amazon-bedrock` through `policies`. This does not block every other provider or determine whether a model is approved. Use only authorized models and endpoints. Diagnostics may contain resolved credentials; redact them before sharing.

The [Claude Code trial](claude-code.md) requires an Anthropic Messages interface, and the [Codex trial](codex.md) requires Responses. Neither interface has been validated here for this deployment; an OpenAI-compatible Chat Completions endpoint alone does not establish support.

## References

- [OpenCode V2 configuration](https://opencode.ai/v2/docs/config)
- [OpenCode V2 providers](https://opencode.ai/v2/docs/providers)
