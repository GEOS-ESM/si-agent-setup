# GitHub CLI setup

GitHub CLI (`gh`) lets an agent work with GitHub issues and pull requests. Its login is separate from your NASA service API key and does not authenticate a model provider.

## Install

On NASA Discover, load the installed module:

```bash
module load other/gh
```

For optional automatic loading in interactive shells, see [Loading modules on Discover](discover-modules.md).

On a Mac, install it with `brew install gh`. Elsewhere, see the [GitHub CLI installation instructions](https://cli.github.com/).

## Sign in

On Discover, the browser login flow may not work. Before logging in, create a **personal access token (classic)** on your own computer:

1. Open [GitHub personal access tokens](https://github.com/settings/tokens), select **Generate new token** > **Generate new token (classic)**, and give it a descriptive name.
2. Select `repo` and `read:org`. If your `gh` version requests `gist` as well, select it too. Do not select additional scopes unless needed.
3. Choose an expiration consistent with your organization's policy. **No expiration** avoids renewal, but an expiring token is safer: `repo` permits broad read/write access to repositories you can access, including private repositories. Use a non-expiring token only if your policy allows it.
4. Generate and copy the token. If your organization requires SSO, authorize the token for that organization on GitHub.

Treat the token like a password. Do not put it in a command, shell history, configuration file, screenshot, or repository. `gh` stores credentials in a system credential store when available and may otherwise use a plaintext file; check `gh auth status` and use this login only if the storage is approved for the device. Follow the [credential requirements](../../SECURITY.md#credential-handling).

Use SSH for Git operations as our group recommends:

```bash
gh auth login --git-protocol ssh
```

Answer the prompts as follows:

1. **Where do you use GitHub?** Select `GitHub.com`.
2. **Upload your SSH public key?** If your key is already on your GitHub account, which is the usual case, select `Skip`.
3. **How would you like to authenticate GitHub CLI?** On Discover, select `Paste an authentication token`, then paste the token at the prompt. On a machine where browser login works, you may instead select `Login with a web browser`, enter the one-time code at `https://github.com/login/device`, and authorize GitHub CLI. Do not share the code.

Confirm the installation and login:

```bash
gh --version
gh auth status
```

See the [GitHub CLI examples](https://cli.github.com/manual/examples) for common commands.
