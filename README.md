# MoonTrader Install

Скрипты установки и базовой настройки MoonTrader для:
- Linux
- macOS
- Windows Server

Основной новый сценарий для Linux: можно не только ставить MoonTrader с нуля, но и отдельно включать ежедневный бэкап активного профиля на уже работающем сервере.

## Куда идти за инструкцией

- Linux (RU): [Linux/README.md](Linux/README.md)
- Linux (EN): [Linux/README_EN.md](Linux/README_EN.md)
- macOS: [MacOS/README.md](MacOS/README.md)
- Windows Server: [WindowsServer/README.md](WindowsServer/README.md)

## Быстрый старт

### Linux

1. Подключитесь к серверу по SSH.
2. Получите root-права: `sudo su` или `su -`.
3. Запустите установщик:

```bash
wget -O - https://raw.githubusercontent.com/Bodiyx/MoonTrader_install/master/Linux/install.sh | bash <(cat) </dev/tty
```

Что умеет Linux-установщик:
- автоматическая установка MoonTrader
- пользовательский режим с выбором компонентов
- ежедневный бэкап активного профиля
- режим `backup-only`, когда можно добавить только бэкап без переустановки MoonTrader

### macOS

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/Bodiyx/MoonTrader_install/master/MacOS/install.sh)"
```

### Windows Server

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/Bodiyx/MoonTrader_install/master/WindowsServer/install.ps1'))
```

## Что важно для Linux-бэкапа

- активный профиль определяется автоматически из `/root/.config/moontrader-data/data/default.profile`
- архивы складываются в `/root/.config/moontrader-data/backup`
- хранится 5 последних архивов с ежедневной ротацией
- ручной запуск: `/usr/local/bin/moontrader-profile-backup.sh`

## Что дальше

- Полная Linux-инструкция и управление бэкапом: [Linux/README.md](Linux/README.md)
- Linux documentation in English: [Linux/README_EN.md](Linux/README_EN.md)
- Установка на macOS: [MacOS/README.md](MacOS/README.md)
- Установка на Windows Server: [WindowsServer/README.md](WindowsServer/README.md)
