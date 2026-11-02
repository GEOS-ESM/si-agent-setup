# NASA Chat with Claude Code

Use this guide only if you are authorized to use NASA Chat. Claude Code uses NASA Chat's Anthropic-compatible gateway. It does not configure Mission AI.

Before configuring Claude Code, [request NASA Chat access and create an API key](api-key.md).

## Install

Use the current [Claude Code setup documentation](https://code.claude.com/docs/en/setup), then verify the installation:

```bash
claude --version
claude doctor
```

## Configure

1. Merge the `env` and `model` settings from the [NASA Chat Claude Code template](../../templates/nasa-chat/claude-code-settings.json.example) into `~/.claude/settings.json`.
2. Make `NASA_CHAT_API_KEY` available through your approved credential mechanism, then pass it to Claude Code in the terminal session where you launch it. Do not put a literal key in `settings.json`, a shell startup file, or command history:

   ```bash
   export ANTHROPIC_API_KEY="${NASA_CHAT_API_KEY:?Supply NASA_CHAT_API_KEY through an approved mechanism}"
   claude
   ```

3. Run `/status` to verify the configured base URL and environment-provided API key.

## Gateway notes

`ANTHROPIC_BASE_URL` is `https://api.asksage.ai.nasa.gov/server/anthropic`, with no trailing `/v1`. Claude Code appends `/v1/messages`; adding `/v1` here produces the invalid `/v1/v1/messages` path and can appear as a model error.

The template disables adaptive thinking and experimental beta request fields because some gateway versions reject them. If the initial API-key approval was declined, Claude Code can return to the login selector and ignore the configured key. Check `/status` for the active credential and gateway URL, then follow the [Claude Code gateway troubleshooting guide](https://code.claude.com/docs/en/llm-gateway-connect#troubleshoot-gateway-errors) or contact the service owner before changing `~/.claude.json`.

## References

- [Claude Code settings](https://code.claude.com/docs/en/settings)
- [Connect Claude Code to an LLM gateway](https://code.claude.com/docs/en/llm-gateway-connect)
- [NASA Chat Claude Code integration reference](https://docs.asksage.ai/integrations/claude-code/)
