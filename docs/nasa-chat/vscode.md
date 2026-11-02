# NASA Chat with Visual Studio Code

Use this guide only if you are authorized to use NASA Chat. The template configures NASA Chat's OpenAI-compatible and Anthropic-compatible custom language models, but does not configure Mission AI.

Before configuring Visual Studio Code, [request NASA Chat access and create an API key](api-key.md).

## Configure

1. In VS Code, run **Chat: Manage Language Models** from the Command Palette.
2. Open `chatLanguageModels.json` from that view. If needed, use **Add Models → Custom Endpoint** to create a provider first. On macOS, the usual location is `~/Library/Application Support/Code/User/chatLanguageModels.json`; use the file opened by your active VS Code profile rather than assuming that path.
3. Merge entries from the [NASA Chat VS Code template](../../templates/nasa-chat/chatLanguageModels.json.example) into the existing JSON array. Update existing NASA Chat groups rather than adding duplicates, and preserve any local `apiKey` secret references. These are VS Code settings, not OpenCode configuration.
4. Run **Developer: Reload Window**.
5. Return to **Chat: Manage Language Models**, choose each NASA Chat provider's settings, select **Update API Key**, and enter your NASA Chat key. Both the OpenAI and Anthropic groups need a configured key.
6. Open Chat and use its model picker to select a model from **NASA Chat (OpenAI)** or **NASA Chat (Anthropic)**. Choose the provider explicitly, not just a matching model name offered by GitHub Copilot or another service. Send a harmless prompt to verify access.

VS Code stores the key in local secret storage. It may add a local reference such as `${input:chat.lm.secret.1234abcd}`; this is not the credential itself. Do not replace it with a literal key or commit it.

## Model coverage and limits

The template covers the OpenAI Chat Completions and Anthropic Messages interfaces. The OpenCode template also includes Grok and native Google Gemini providers; those are not configured by this VS Code template. Do not copy OpenCode provider blocks or Google's native API URL into this file.

Claude limits follow the OpenCode V2 context budgets, with output tokens reserved rather than counted as additional input. The existing GPT limits are conservative template values, not newly verified gateway maxima. Confirm limits with the service owner before raising them.

## Troubleshooting

- **Model missing:** check visibility (eye icon) in **Chat: Manage Language Models** and restart VS Code if a reload does not refresh the picker. In Copilot harness sessions, current VS Code requires the experimental `chat.agentHost.byokModels.enabled` setting for supported BYOK models; organization policy may also restrict BYOK. See the [VS Code model guide](https://code.visualstudio.com/docs/agent-customization/language-models#bring-your-own-language-model-key).
- **401 or 403:** check the key for this provider group, service access, and network requirements. Do not paste keys or unredacted logs into an issue.
- **“The model only supports 128 tools”:** use **Configure Tools** to disable unused MCP or extension tools. VS Code omitted excess tools; this is not an API-key error.
- **Other traffic:** selecting a NASA model does not route every VS Code feature through NASA. Review utility models (`chat.utilityModel` and `chat.utilitySmallModel`), extensions, and telemetry under your organization's policy; see [Security](../../SECURITY.md).

## References

- [VS Code custom endpoint models](https://code.visualstudio.com/docs/agent-customization/language-models#add-a-custom-endpoint-model)
