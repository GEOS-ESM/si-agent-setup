# Mission Intelligence with Codex CLI

Use this guide only if you are authorized to use NASA Mission Intelligence. This configures the Mission Intelligence LiteLLM gateway, not NASA Chat. If you do not already have a Mission Intelligence API key, contact the SI Team for more information.

**Compatibility note:** The [Mission Intelligence VS Code template](../../templates/mission-ai/chatLanguageModels.json.example) documents `/v1/chat/completions`. Codex requires `/v1/responses` with streaming and tool calls. This deployment's Responses compatibility has not yet been validated; use the setup below as a local trial, and contact the service owner if the gateway does not support it.

## Install

Install the Codex CLI using the current [Codex installation guide](https://developers.openai.com/codex/cli/), then check it is available:

```bash
codex --version
```

## Configure

1. Merge the [Mission Intelligence Codex provider template](../../templates/mission-ai/codex.config.toml.example) into **your user-level** `~/.codex/config.toml`. Keep existing settings and other provider definitions. Do not put this provider in a repository's `.codex/config.toml`: Codex ignores project-level provider and authentication settings.
2. Make `NASA_MISSION_AI_API_KEY` available through an approved credential mechanism in the environment where you launch Codex. Codex reads it as a bearer token via `env_key`; the template contains no key itself. Do not place a literal key in the TOML or commit one. A variable set in one terminal may not be available to a separately launched GUI client.
3. Start a new session with the provider and an approved Mission Intelligence OpenAI model (for example, `gpt-6-luna`):

   ```bash
   codex -c 'model_provider="nasa_mission_ai"' -m gpt-6-luna
   ```

The template does not change your default provider or model. Plain `codex` continues to use your existing defaults. In the session, `/status` shows the active model, but the model name alone does not identify the provider: the launch-time `model_provider` override selects Mission Intelligence. `/model` changes the model, not the provider; start a new session to switch providers.

If you receive a 401, confirm that the key in the **launching terminal** is a valid Mission Intelligence LiteLLM key. A successful key check does not prove Responses compatibility. If authentication succeeds but streaming or tool calls fail, ask the service owner whether this deployment supports Codex's `/v1/responses` interface. Share errors only after redacting any keys or tokens.

## References

- [Codex custom model providers](https://developers.openai.com/codex/config-file/config-advanced#custom-model-providers)
- [Codex gateway compatibility](https://developers.openai.com/codex/enterprise/gateway-compatibility)
