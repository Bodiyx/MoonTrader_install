# MoonTrader Installation for Linux

Automatic MoonTrader installer for Linux with support for:
- MoonTrader installation
- MTGuardian
- daily active-profile backup

> **Russian version**: [README.md](README.md)

## Supported systems

- Ubuntu 20.04+
- Ubuntu 22.04+
- Ubuntu 24.04+
- Debian 12+
- Architectures: `AMD64`, `ARM64`

## Quick start

### 1. Connect to the server

```bash
ssh user@your-server-ip
```

### 2. Switch to root

```bash
# Ubuntu
sudo su

# Debian
su -
```

### 3. Run the installer

```bash
wget -O - https://raw.githubusercontent.com/Bodiyx/MoonTrader_install/master/Linux/install.sh | bash <(cat) </dev/tty
```

## Installation modes

### Automatic installation

Use this when you want a ready-to-use Linux setup from scratch.

Enabled by default:
- MoonTrader installation
- `chrony` time sync
- firewall setup
- `Fail2Ban`
- daily active-profile backup

Not enabled by default:
- `MT Guardian`

### Custom installation

Use this when you want to choose each component manually.

Available options:
- `MT Link: Official`
- `MT Link: Dropbox`
- `Setup Time (chrony)`
- `Setup Firewall`
- `Setup Fail2Ban`
- `Setup MT Guardian`
- `Setup Profile Backup`

MoonTrader source selection rules:
- only `Official` enabled -> install from official source
- only `Dropbox` enabled -> install from Dropbox source
- both disabled -> skip MoonTrader installation
- both enabled -> installer asks you to keep only one source

## Backup-only mode

This mode is for an existing server where MoonTrader is already installed and you only want to add automatic profile backup.

Steps:
1. Choose `Custom installation`
2. Disable `MT Link: Official`
3. Disable `MT Link: Dropbox`
4. Enable `Setup Profile Backup`
5. Keep other options only if you need them

MoonTrader will not be reinstalled in this mode.

## How profile backup works

For you as the server owner, this means the installer automatically finds the active profile, archives it every day, and keeps only the latest 5 backups.

Technical details:
- active profile is read from `/root/.config/moontrader-data/data/default.profile`
- archives are stored in `/root/.config/moontrader-data/backup`
- daily schedule is created in `/etc/cron.d/moontrader-profile-backup`
- backup script is installed to `/usr/local/bin/moontrader-profile-backup.sh`

### Files created on the server

- backup script: `/usr/local/bin/moontrader-profile-backup.sh`
- cron job: `/etc/cron.d/moontrader-profile-backup`
- log file: `/var/log/moontrader-profile-backup.log`
- backup directory: `/root/.config/moontrader-data/backup`

### Manual backup run

```bash
/usr/local/bin/moontrader-profile-backup.sh
```

### Quick backup check

```bash
ls -l /usr/local/bin/moontrader-profile-backup.sh
cat /etc/cron.d/moontrader-profile-backup
ls -la /root/.config/moontrader-data/backup
```

## MoonTrader management

### Main commands

| Command | Description |
|---------|-------------|
| `MoonTrader` | Start with update |
| `MoonTrader --no-update` | Start without update |
| `MoonTrader --stop` | Stop the core |
| `MoonTrader --status` | Show process status |
| `MoonTrader --attach` | Attach to tmux session |
| `MoonTrader --help` | Show help |

### tmux usage

```bash
# Start MoonTrader
MoonTrader

# Attach to the tmux session
MoonTrader --attach

# List tmux sessions
tmux list-sessions
```

## MTGuardian

### Installation

1. Choose `Custom installation`
2. Enable `Setup MT Guardian`
3. Enter Telegram bot settings

### Management

```bash
systemctl status mtguardian
systemctl start mtguardian
systemctl stop mtguardian
systemctl restart mtguardian
tail -f ~/MTGuardian/MTGuardian.log
```

## File structure

```text
~/
├── moontrader/
│   ├── MTCore
│   └── start_mt.sh
├── .config/moontrader-data/
│   ├── backup/
│   └── data/
│       ├── default.profile
│       ├── logs/
│       └── archive/
└── MTGuardian/
    ├── MTGuardian
    ├── MTGuardian.settings
    └── MTGuardian.log
```

## Troubleshooting

### MoonTrader

```bash
MoonTrader --status
ps aux | grep MTCore
tmux list-sessions
```

### MTGuardian

```bash
systemctl status mtguardian
journalctl -u mtguardian -f
```

### Backup

```bash
tail -f /var/log/moontrader-profile-backup.log
/usr/local/bin/moontrader-profile-backup.sh
ls -la /root/.config/moontrader-data/backup
```
