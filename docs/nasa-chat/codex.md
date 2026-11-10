# NASA Chat with Codex CLI

Use this guide only if you are authorized to use NASA Chat. This configures NASA Chat's OpenAI-compatible gateway, not Mission Intelligence.

Before configuring Codex, [request NASA Chat access and create an API key](api-key.md).

## Install

Install the Codex CLI using the current [Codex installation guide](https://developers.openai.com/codex/cli/), then check it is available:

```bash
codex --version
```

## Configure

1. Merge the [NASA Chat Codex provider template](../../templates/nasa-chat/codex.config.toml.example) into **your user-level** `~/.codex/config.toml`. Keep existing settings and other provider definitions. Do not put this provider in a repository's `.codex/config.toml`: Codex ignores project-level provider and authentication settings.
2. Make `NASA_CHAT_API_KEY` available through an approved credential mechanism in the environment where you launch Codex. The template maps that variable to Ask Sage's `x-access-tokens` header; it contains no key itself. Do not place a literal key in the TOML or commit one. A variable set in one terminal may not be available to a separately launched GUI client.
3. Start a new session with the provider and an approved NASA Chat OpenAI model (for example, `gpt-5.6-luna`):

   ```bash
   codex -c 'model_provider="nasa_chat"' -m gpt-5.6-luna
   ```

The template does not change your default provider or model. Plain `codex` continues to use your existing defaults. In the session, `/status` shows the active model, but the model name alone does not identify the provider: the launch-time `model_provider` override selects NASA Chat. `/model` changes the model, not the provider; start a new session to switch providers.

Codex sends requests to the Responses API under the configured `/server/openai/v1` base URL. The [Ask Sage Codex integration reference](https://docs.asksage.ai/integrations/codex/) documents that interface. If requests fail, check the credential environment, selected model, and gateway response without sharing keys or unredacted logs.

## References

- [Codex custom model providers](https://developers.openai.com/codex/config-file/config-advanced#custom-model-providers)
- [NASA Chat Codex integration reference](https://docs.asksage.ai/integrations/codex/)
