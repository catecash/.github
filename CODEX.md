# Codex quick start: GitHub organization profile

## Start with Codex

Open this repository as the Codex workspace and ask Codex to read `AGENTS.md` (if present), `README.md`, and this guide. Tell it your operating system, target network, and whether you want a local preview or a hosted instance. Let it inspect the current branch, available tools, and configuration before it changes anything. Ask it to report the commands it ran and the observed result.

## Credentials

Give Codex **paths and variable names**, never secret values in a prompt. Put API keys, RPC cookies, OAuth credentials, wallet material, and SSH keys in ignored local files or restricted host secret storage. If a service needs a secret, mount a read-only file from outside the Docker build context. Keep server-side secrets out of Git, image layers, build arguments, and container environment variables. Client API keys embedded in browser or mobile builds can be extracted by users; use only keys intended for public clients. Public payout and receiving addresses are configuration, not private keys. Have Codex check tracked and staged files for secrets before any commit or push.

## Install and verify

No runtime installation. Edit `profile/README.md` and the artwork in this repository. Preview the Markdown and verify links and image paths.

## Configuration for this repository

No API key is needed to edit the profile. GitHub authentication is needed only for an authorized push.

## Launch and monitor

There is no service instance. Publishing means committing and pushing the reviewed profile changes to its intended GitHub branch.

## Prompt to give Codex

> Review this organization profile against the current project docs, update stale claims, preview the Markdown, and prepare a focused commit.

Ask Codex to state the target repository, branch, network, and proposed changes before any push or hosted deployment. For production, have it follow the repository’s release runbook and verify the running instance after the change.
