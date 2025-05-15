# KeePassXC Configuration & SFTP Sync Scripts

This repository contains my personal KeePassXC configuration and helper scripts to automate secure backup and synchronization of my encrypted database over SFTP. By combining a single GTK passphrase prompt with SSH-agent integration and automatic merge logic, I can seamlessly pull, open, edit, and push my KeePassXC vault without risk of conflicts or data loss.

## Contents

- `keepassxc.ini` - Customized KeePassXC settings (general, SSH agent, browser integration).
- `bin/keepassxc-custom` - Main wrapper script:

  - Pulls the remote database via SFTP.
  - Launches KeePassXC and auto-unlocks the vault with `--pw-stdin`.
  - Checks remote modification time; if changed, merges remote and local copies.
  - Pushes the updated database back to the server.

## Prerequisites

- KeePassXC (version 2.7.x or later)
- `keepassxc-cli`
- SSH client (with `ssh`, `scp`, `ssh-add`)
- `zenity` (for GTK dialogs)
- `lsof` (optional, for safety checks)
- A running SSH-agent or ability to use `ssh-add`

## Installation

1. Clone this repository into your KeePassXC config directory:

   ```bash
   git clone git@github.com:your-user/keepassxc-setup.git ~/.config/keepassxc
   ```

2. Make the sync script executable:

   ```bash
   chmod +x ~/.config/keepassxc/bin/keepassxc-custom
   ```

3. Ensure your SSH key and KeePassXC vault use the same passphrase.
4. Configure the variables in `bin/keepassxc-custom` (SSH_KEY, REMOTE, LOCAL, etc.).

## Usage

Run the sync script to pull, open, and push your KeePassXC database:

```bash
keepassxc-custom
```

The script will prompt once for your passphrase, handle data transfer, and provide GTK notifications at each step.

## Security & Backup

- **Database files (`*.kdbx`) are _not_ tracked** in Git; they remain only on your local machine and remote server.
- KeePassXC entry history retains per-entry changes. For full-file backups, consider storing encrypted snapshots on your server or a secure backup service.

## License

This repository is released under the MIT License. See [LICENSE](LICENSE) for details.
