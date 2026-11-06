# NASA Chat with OpenCode V2

Use this guide only if you are authorized to use NASA Chat. It configures the Anthropic-compatible, OpenAI-compatible, xAI (Grok), and Google Gemini NASA Chat providers, but does not configure Mission AI.

Before configuring OpenCode, [request NASA Chat access and create an API key](api-key.md).

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

Follow the [GitHub CLI setup guide](../clients/github-cli.md) to let the agent work with GitHub issues and pull requests. This login is separate from your NASA Chat API key and does not connect an OpenCode model provider.

## Configure

1. Merge the [NASA Chat OpenCode V2 template](../../templates/nasa-chat/opencode.jsonc.example) into `~/.config/opencode/opencode.jsonc`. Do not replace unrelated providers, plugins, or preferences.
2. Configure credentials using one of the approved methods below.
3. Start OpenCode and select an available NASA Chat model with `/models`. Select the NASA Chat provider, not a similarly named model from another provider. For example, `nasa_chat_anthropic/claude-sonnet-5-5-gov` selects the NASA Chat gateway.
4. Run `opencode models` to check the available model IDs. Send a harmless prompt to the selected model to verify authentication; appearing in the list alone is not an end-to-end test.

### Credentials

Follow the [credential requirements](../../SECURITY.md#credential-handling) for your service and device. Never put a literal key in this template or commit credentials.

- **`/connect`:** if local credential storage is approved, run `/connect` separately for each provider you use: `nasa_chat_anthropic`, `nasa_chat_openai`, and/or `nasa_chat_grok`. If you previously added `settings.apiKey` to those blocks, remove it first.
- **Environment alternative:** the template's provider `env` entries recognize `NASA_CHAT_API_KEY` without further config edits. Supply it through an approved mechanism to the **OpenCode server process**. For a terminal-only trial, launch `opencode --standalone` from an environment that already has it. A variable in a new terminal does not automatically update an existing background service; see the [environment instructions](https://opencode.ai/v2/docs/cli/providers#environment) for a managed service. Avoid typing literal keys into shell commands or history.
- **Both:** for each provider, a saved `/connect` account takes precedence over its provider environment connection. To use the environment connection instead, remove the saved account with `/connect` or `opencode auth logout`.
- **Google Gemini (`nasa_chat_google`):** supply `NASA_CHAT_API_KEY` to the server even when using `/connect`. The template sends it in the required `x-access-tokens` header as well as `apiKey`; `/connect` alone does not populate that header. If you also connect Gemini, use the same key in both places to avoid mismatched credentials. Omit the Google provider block if you do not intend to configure it.

OpenCode saves `/connect` credentials in the **server's SQLite database**, usually `~/.local/share/opencode/opencode.db`. This is a local credential store, not shared configuration; protect the database and its backups, and do not commit or share them. Environment variables avoid that account store but can be exposed through processes, diagnostics, or shell history. Follow your service's approved method. See [credential storage](https://opencode.ai/v2/docs/cli/providers#storage).

OpenCode reloads configuration changes automatically; use `/reload` if needed. Terminal preferences are separate, in `~/.config/opencode/cli.json`.

The template does not set a default model; choose the provider and model you want in `/models`.

The Anthropic and OpenAI-compatible base URLs include `/v1`; Google's endpoint uses `/google/v1beta`. The [Claude Code guide](claude-code.md) uses a shorter Anthropic base URL because Claude Code appends `/v1/messages` itself. If an existing local OpenCode provider has a different ID, use that ID in your model selections and authenticate the corresponding provider.

The template blocks `opencode`, `azure`, and `amazon-bedrock` through `policies`. This does not block every other provider or determine whether a model is approved. Use only authorized models and endpoints. Diagnostics may contain resolved credentials; redact them before sharing.

## References

- [OpenCode V2 configuration](https://opencode.ai/v2/docs/config)
- [OpenCode V2 providers](https://opencode.ai/v2/docs/providers)
- [NASA Chat OpenCode integration reference](https://docs.asksage.ai/integrations/opencode/)
