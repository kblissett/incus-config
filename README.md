# LLM Coding Sandbox

Incus profile for creating sandboxed containers with Claude Code pre-installed.

## Prerequisites

- Claude Code installed on your host with settings at `~/.claude/settings.json`

### Install Incus (macOS)

```bash
brew install incus colima
colima start --runtime incus --cpu 8 --memory 8
```

## Setup (one-time)

1. Create the profile and load the cloud-init config:

```bash
incus profile create llm-coding-sandbox
incus profile edit llm-coding-sandbox < llm-coding-sandbox.yaml
```

2. Add your Claude settings to the profile:

```bash
incus profile device add llm-coding-sandbox claude-settings disk source=$HOME/.claude/settings.json path=/root/.claude/settings.json readonly=true
```

## Usage

Launch a sandbox:

```bash
incus launch images:ubuntu/24.04/cloud my-sandbox --profile default --profile llm-coding-sandbox
```

Wait for cloud-init to finish (installs updates, vim, and Claude Code):

```bash
incus exec my-sandbox -- cloud-init status --wait
```

Enter the sandbox:

```bash
incus exec my-sandbox -- bash
```

Run Claude Code:

```bash
claude
```

## Managing sandboxes

```bash
# List running containers
incus list

# Stop a sandbox
incus stop my-sandbox

# Delete a sandbox
incus delete my-sandbox

# Force delete a running sandbox
incus delete my-sandbox --force
```

## Notes

- Uses `images:ubuntu/24.04/cloud` (cloud-init enabled image)
- The `default` profile provides networking and root disk
- Claude Code is installed as root; settings are mounted read-only from host
- Cloud-init only runs on first boot
