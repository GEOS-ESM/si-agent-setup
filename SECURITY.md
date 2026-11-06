# Security Requirements

## Never commit

- API keys, bearer tokens, cookies, OAuth refresh tokens, or passwords
- Client certificates, private keys, CA bundles, or credential helper output
- Internal-only service endpoints, network topology, or model inventories unless explicitly approved for this repository. The approved service endpoints and model IDs in the templates are intentionally included for GMAO users.
- Prompts, source code, logs, screenshots, or conversation exports containing non-public data

Repository history is durable. Removing a secret from a later commit does not make the secret safe. If a credential is exposed, revoke or rotate it immediately and notify the service owner.

## Credential handling

Use the organization-approved credential mechanism. When a client supports environment-variable or file references, keep the secret outside the client configuration file. Do not put credential exports in a shell startup file unless the service owner has approved that approach and the file permissions are appropriate.

Do not use an interactive client command that persists credentials locally unless it is approved for the service and device. OpenCode's `/connect` stores credentials in the server's local database; protect that database and its backups. Environment variables can also expose secrets through processes, diagnostics, or shell history. Use the provider-approved method for your device.

## Data and network controls

Before using an agent, confirm data classification, retention, logging, model-training, tool-access, and external-traffic rules with the service owner. A model endpoint redirect does not automatically stop client telemetry, updates, package downloads, web fetches, or extensions from contacting other services.

Use only approved models and endpoints. Do not bypass proxy, certificate, VPN, or device-management controls.

## Reporting

Report suspected credential exposure, unexpected outbound traffic, or a data-handling concern through the established GMAO security and service-owner channels. Do not include secrets in the report.
