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

Run the configure-profile script with your SSH public key:

```bash
./bin/configure-profile 'ssh-ed25519 AAAA...'
```

Add Colima's SSH config to your `~/.ssh/config` for jump host access:

```
Include ~/.colima/ssh_config
```

## Usage

Launch a sandbox:

```bash
./bin/launch my-sandbox
```

Enter the sandbox (without SSH agent):

```bash
incus exec my-sandbox -- bash
```

Clone a repo using SSH agent forwarding:

> **⚠️ WARNING:** Never leave an interactive SSH agent forwarding session open while a coding agent is running autonomously. The agent could use your forwarded credentials to authenticate as you to external systems. These scripts run one-off commands so the forwarding session ends immediately.

```bash
./bin/git-clone my-sandbox git@github.com:user/repo.git
./bin/git-clone my-sandbox git@github.com:user/repo.git /workspace/myrepo
```

Run arbitrary commands with SSH agent forwarding:

```bash
./bin/ssh-cmd my-sandbox 'git push origin main'
```

Run Claude Code (inside the sandbox):

```bash
claude
```

## Managing sandboxes

```bash
incus list                       # List running containers
incus stop my-sandbox            # Stop a sandbox
incus delete my-sandbox          # Delete a sandbox
incus delete my-sandbox --force  # Force delete a running sandbox
```

## Notes

- Uses `images:ubuntu/24.04/cloud` (cloud-init enabled image)
- The `default` profile provides networking and root disk
- Claude Code is installed as root; settings are mounted read-only from host
- Claude Code runs with `--dangerously-skip-permissions` and `--disallowed-tools WebSearch` by default
- SSH server is installed for agent forwarding; container IPs aren't directly reachable from macOS, so use Colima as a jump host
- Cloud-init only runs on first boot
