# Mission AI with Visual Studio Code

Use this guide only if you are authorized to use NASA Mission AI. The template configures the Mission AI LiteLLM custom language model only and does not configure NASA Chat. If you do not already have a Mission AI API key, contact the SI Team for more information.

## Configure

1. In VS Code, run **Chat: Manage Language Models** from the Command Palette.
2. Open `chatLanguageModels.json` from that view. If needed, use **Add Models → Custom Endpoint** to create a provider first. On macOS, the usual location is `~/Library/Application Support/Code/User/chatLanguageModels.json`; use the file opened by your active VS Code profile rather than assuming that path.
3. Merge entries from the [Mission AI VS Code template](../../templates/mission-ai/chatLanguageModels.json.example) into the existing JSON array. Update the existing Mission AI group rather than adding duplicates, and preserve any local `apiKey` secret reference. These are VS Code settings, not OpenCode configuration.
4. Run **Developer: Reload Window**.
5. Return to **Chat: Manage Language Models**, choose the Mission AI provider settings, select **Update API Key**, and enter your Mission AI key.
6. Open Chat and use its model picker to select a model from **NASA Mission AI LiteLLM**. Choose the provider explicitly, not just a matching model name offered by GitHub Copilot or another service. Send a harmless prompt to verify access.

VS Code stores the key in local secret storage. It may add a local reference such as `${input:chat.lm.secret.1234abcd}`; this is not the credential itself. Do not replace it with a literal key or commit it.

## Model coverage and limits

The template uses the gateway's `/v1/chat/completions` interface for Claude, GPT, and Gemini. It does not depend on OpenCode being installed or use OpenCode's provider packages. The LiteLLM Connector extension is not required: this deployment returned 403 for `/model/info` during testing, so that extension could not discover models even though `/v1/models` worked.

GPT limits use gateway-reported input and output maxima. Claude and Gemini 3.8 Flash also reserve output within the OpenCode V2 total context budget. Gemini 4 Argon's limits were not reported by the gateway; its smaller template values are provisional, not verified maxima. Confirm them with the service owner before raising them.

## Troubleshooting

- **Model missing:** check visibility (eye icon) in **Chat: Manage Language Models** and restart VS Code if a reload does not refresh the picker. In Copilot harness sessions, current VS Code requires the experimental `chat.agentHost.byokModels.enabled` setting for supported BYOK models; organization policy may also restrict BYOK. See the [VS Code model guide](https://code.visualstudio.com/docs/agent-customization/language-models#bring-your-own-language-model-key).
- **401 or 403:** check the key for the Mission AI group, service access, and network requirements. Do not paste keys or unredacted logs into an issue.
- **“The model only supports 128 tools”:** use **Configure Tools** to disable unused MCP or extension tools. VS Code omitted excess tools; this is not an API-key error.
- **Other traffic:** selecting a NASA model does not route every VS Code feature through NASA. Review utility models (`chat.utilityModel` and `chat.utilitySmallModel`), extensions, and telemetry under your organization's policy; see [Security](../../SECURITY.md).

## References

- [VS Code custom endpoint models](https://code.visualstudio.com/docs/agent-customization/language-models#add-a-custom-endpoint-model)
