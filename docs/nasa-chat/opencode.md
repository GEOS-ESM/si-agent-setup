# NASA Chat with OpenCode V2

Use this guide only if you are authorized to use NASA Chat. It configures the Anthropic-compatible, OpenAI-compatible, xAI (Grok), and Google Gemini NASA Chat providers, but does not configure Mission AI.

Before configuring OpenCode, [request NASA Chat access and create an API key](api-key.md).

## Install

On NASA Discover, OpenCode and GitHub CLI (`gh`) are already installed. Load them with:

```bash
module load other/opencode
module load other/gh
```

Check `opencode --version` after loading the module; these instructions require V2.

On a Mac, install OpenCode V2 and GitHub CLI with Homebrew:

```bash
brew install anomalyco/tap/opencode-v2
brew install gh
```

Elsewhere, install OpenCode using the [OpenCode V2 documentation](https://opencode.ai/v2/docs). Install [GitHub CLI](https://cli.github.com/); its [usage examples](https://cli.github.com/manual/examples) show common commands.

## Sign in to GitHub

`gh` lets the agent work with GitHub issues and pull requests. This login is separate from your NASA Chat API key and does not connect an OpenCode model provider. Use SSH for Git operations as our group recommends:

```bash
gh auth login --git-protocol ssh
```

Answer the prompts as follows:

1. **Where do you use GitHub?** Select `GitHub.com`.
2. **Upload your SSH public key?** If your key is already on your GitHub account, which is the usual case, select `Skip`.
3. **How would you like to authenticate GitHub CLI?** Select `Login with a web browser`. `gh` shows an 8-character one-time code in the form `XXXX-YYYY`. Open `https://github.com/login/device` on your own computer if the remote session cannot open a browser, and enter the code there. Do not share the code.
4. Authorize GitHub CLI when prompted.

Confirm both tools are available:

```bash
opencode --version
gh --version
gh auth status
```

## Configure

1. Merge the [NASA Chat OpenCode V2 template](../../templates/nasa-chat/opencode.jsonc.example) into `~/.config/opencode/opencode.jsonc`. Do not replace unrelated providers, plugins, or preferences.
2. Configure credentials using one of the approved methods below.
3. Start OpenCode and select an available NASA Chat model with `/models`. Select the NASA Chat provider, not a similarly named model from another provider. For example, `nasa_chat_anthropic/claude-sonnet-5-5-gov` selects the NASA Chat gateway.
4. Run `opencode models` to check the available model IDs. Send a harmless prompt to the selected model to verify authentication; appearing in the list alone is not an end-to-end test.

### Credentials

Follow the [credential requirements](../../SECURITY.md#credential-handling) for your service and device. Never put a literal key in this template or commit credentials.

- **As shipped:** every provider's `settings.apiKey` references `"{env:NASA_CHAT_API_KEY}"`. Supply that variable through an approved mechanism to the **OpenCode server process**. For a terminal-only trial, launching `opencode --standalone` from an environment that already has the variable avoids a separately managed server environment. A variable in a new terminal does not automatically update an existing background service; use the [environment instructions](https://opencode.ai/v2/docs/cli/providers#environment) for a managed service. Avoid typing literal keys into shell commands or history.
- **`/connect` alternative:** if local credential storage is approved, remove `settings.apiKey` from the Anthropic, OpenAI, and/or Grok blocks you intend to authenticate this way, then run `/connect` for `nasa_chat_anthropic`, `nasa_chat_openai`, and/or `nasa_chat_grok`. This avoids leaving an unresolved environment reference alongside a saved credential.
- **Google Gemini (`nasa_chat_google`):** keep `NASA_CHAT_API_KEY` available. The template sends it in the required `x-access-tokens` header as well as `apiKey`; `/connect` alone does not populate that custom header. Omit the Google provider block if you do not intend to configure it.

OpenCode saves `/connect` credentials in the **server's SQLite database**, usually `~/.local/share/opencode/opencode.db`. This is a local credential store, not shared configuration; do not commit or share it. See [credential storage](https://opencode.ai/v2/docs/cli/providers#storage).

OpenCode reloads configuration changes automatically; use `/reload` if needed. Terminal preferences are separate, in `~/.config/opencode/cli.json`.

The template does not set a default model; choose the provider and model you want in `/models`.

The Anthropic and OpenAI-compatible base URLs include `/v1`; Google's endpoint uses `/google/v1beta`. The [Claude Code guide](claude-code.md) uses a shorter Anthropic base URL because Claude Code appends `/v1/messages` itself. If an existing local OpenCode provider has a different ID, use that ID in your model selections and authenticate the corresponding provider.

The template blocks `opencode`, `azure`, and `amazon-bedrock` through `policies`. This does not block every other provider or determine whether a model is approved. Use only authorized models and endpoints. Diagnostics may contain resolved credentials; redact them before sharing.

## References

- [OpenCode V2 configuration](https://opencode.ai/v2/docs/config)
- [OpenCode V2 providers](https://opencode.ai/v2/docs/providers)
- [NASA Chat OpenCode integration reference](https://docs.asksage.ai/integrations/opencode/)
