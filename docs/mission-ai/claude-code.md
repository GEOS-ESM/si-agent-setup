# Mission AI with Claude Code (unvalidated trial)

Use this guide only if you are authorized to use NASA Mission AI. It does not configure NASA Chat. If you need a Mission AI API key, contact the SI Team.

**Compatibility is unverified.** The repository contains a Claude Code template, but a working Chat Completions endpoint does not establish support for Claude Code. Before trying it, ask the service owner to confirm that this deployment supports Anthropic Messages at `/v1/messages`, including streaming and tool calls, and accepts these model IDs and gateway credentials. Otherwise use the [OpenCode](opencode.md) or [VS Code](vscode.md) guide.

## Local trial

1. Install Claude Code using its [setup documentation](https://code.claude.com/docs/en/setup). Check `claude --version` and `claude doctor`.
2. Merge the `env` and `model` settings from the [Mission AI trial template](../../templates/mission-ai/claude-code-settings.json.example) into your local `~/.claude/settings.json`, preserving unrelated settings. Do not combine its `ANTHROPIC_BASE_URL` with the NASA Chat template; the same setting selects one gateway at a time.
3. Supply `NASA_MISSION_AI_API_KEY` through an approved credential mechanism. In the terminal where you launch Claude Code:

   ```bash
   export ANTHROPIC_API_KEY="${NASA_MISSION_AI_API_KEY:?Supply NASA_MISSION_AI_API_KEY through an approved mechanism}"
   claude
   ```

   Do not type a literal key into shell commands or history, or save it in the template.
4. Run `/status` to check the gateway and active credential. Use a harmless prompt to test the connection before depending on it. If authentication, streaming, or tool calls fail, contact the service owner rather than assuming the OpenAI-compatible API also supports Claude Code.

The trial template uses `https://proxy.fast.luna.nasa.gov` without `/v1` because Claude Code appends `/v1/messages`. Its model IDs match the OpenCode V2 template, but have not been independently validated on the Anthropic interface.

## References

- [Connect Claude Code to an LLM gateway](https://code.claude.com/docs/en/llm-gateway-connect)
- [Gateway API compatibility](https://code.claude.com/docs/en/llm-gateway-protocol)
