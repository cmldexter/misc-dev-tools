# misc-dev-tools
Collection of mini dev tools and scripts to speed up a dev environment

## How to Install

To make these scriptlets globally available on your system, you can add the `bin` directory to your shell's `$PATH`. 

Add the following snippet to your shell profile (`~/.zshrc` or `~/.bashrc`):

```bash
# Add misc-dev-tools to PATH
export PATH="$HOME/Documents/GitHub/misc-dev-tools/bin:$PATH"
```

After modifying your profile, reload your shell (e.g. `source ~/.zshrc`) or restart your terminal. You can then run any script in the `bin/` directory (like `sync-gitlab-github`) directly from any folder.

## Scripts

### `sync-gitlab-github`
Fetches all remote branches from a specified `github` remote and pushes them directly to an `origin` (GitLab) remote to ensure both repositories are synchronized. 

**When to use:** Use this script when you maintain mirrors between GitHub and GitLab and need a quick, forceful synchronization to pull all branches from GitHub and push them identically to GitLab. It's particularly useful in cases where automatic CI/CD mirroring fails or trails behind.

### `ansible-ssh-update`
A Python utility that dynamically creates or updates a local SSH configuration file (`~/.ssh/config.d/ansible-ssh-config`) by parsing an Ansible `hosts.yml` inventory. It automatically extracts `ansible_host`, `ansible_user`, and `ansible_ssh_private_key_file` mappings so you can natively run `ssh <hostname>` directly from your terminal.

**When to use:** When you are managing infrastructure via Ansible and want to seamlessly SSH into those same nodes without looking up their IP addresses or remembering the private key paths. See `samples/hosts.sample.yml` for an example of a supported inventory format.

### `fetch-kubeconfig`
A simple bash wrapper that securely connects to your remote Kubernetes master node, extracts the administrative `rke2.yaml` kubeconfig file, safely replaces the `127.0.0.1` loopback address with your server's reachable hostname/IP, and places it in your local `~/.kube/` directory.

**When to use:** When provisioning a new Kubernetes cluster and needing a 1-click way to grab the administrative kubeconfig securely so you can run local `kubectl` commands.
