# Установка MoonTrader для Linux

Автоматический установщик MoonTrader для Linux с поддержкой:
- установки MoonTrader
- MTGuardian
- ежедневного бэкапа активного профиля

> **English version**: [README_EN.md](README_EN.md)

## Поддерживаемые системы

- Ubuntu 20.04+
- Ubuntu 22.04+
- Ubuntu 24.04+
- Debian 12+
- Архитектуры: `AMD64`, `ARM64`

## Быстрая установка

### 1. Подключитесь к серверу

```bash
ssh user@your-server-ip
```

### 2. Получите root-права

```bash
# Ubuntu
sudo su

# Debian
su -
```

### 3. Запустите установщик

```bash
wget -O - https://raw.githubusercontent.com/Bodiyx/MoonTrader_install/master/Linux/install.sh | bash <(cat) </dev/tty
```

## Варианты установки

### Automatic installation

Этот режим нужен, когда вы ставите сервер с нуля и хотите получить готовую базовую настройку.

Что включено по умолчанию:
- установка MoonTrader
- настройка времени `chrony`
- настройка брандмауэра
- настройка `Fail2Ban`
- ежедневный бэкап активного профиля

Что не включено:
- `MT Guardian`

### Custom installation

Этот режим нужен, когда вы хотите сами выбрать, что именно добавить на сервер.

Доступные опции:
- `MT Link: Official`
- `MT Link: Dropbox`
- `Setup Time (chrony)`
- `Setup Firewall`
- `Setup Fail2Ban`
- `Setup MT Guardian`
- `Setup Profile Backup`

Правила выбора источника MoonTrader:
- включён только `Official` -> MoonTrader ставится из official source
- включён только `Dropbox` -> MoonTrader ставится из Dropbox source
- оба выключены -> установка MoonTrader пропускается
- оба включены -> установщик попросит оставить только один источник

## Режим только для бэкапа

Это сценарий для уже работающего сервера, где MoonTrader уже установлен, а вам нужно только добавить автоматический бэкап.

Что сделать:
1. Выберите `Custom installation`
2. Выключите `MT Link: Official`
3. Выключите `MT Link: Dropbox`
4. Включите `Setup Profile Backup`
5. Оставьте остальные пункты по ситуации

В этом режиме MoonTrader не переустанавливается.

## Как работает бэкап профиля

Для вас это означает простую схему: установщик сам определяет активный профиль, каждый день архивирует его и хранит только последние 5 архивов.

Технически это устроено так:
- активный профиль берётся из `/root/.config/moontrader-data/data/default.profile`
- архивы складываются в `/root/.config/moontrader-data/backup`
- запуск по расписанию создаётся через `/etc/cron.d/moontrader-profile-backup`
- сам скрипт бэкапа ставится в `/usr/local/bin/moontrader-profile-backup.sh`

### Что создаётся на сервере

- backup script: `/usr/local/bin/moontrader-profile-backup.sh`
- cron job: `/etc/cron.d/moontrader-profile-backup`
- log file: `/var/log/moontrader-profile-backup.log`
- backup directory: `/root/.config/moontrader-data/backup`

### Ручной запуск бэкапа

```bash
/usr/local/bin/moontrader-profile-backup.sh
```

### Быстрая проверка бэкапа

```bash
ls -l /usr/local/bin/moontrader-profile-backup.sh
cat /etc/cron.d/moontrader-profile-backup
ls -la /root/.config/moontrader-data/backup
```

## Управление MoonTrader

### Основные команды

| Команда | Описание |
|---------|----------|
| `MoonTrader` | Запуск с обновлением |
| `MoonTrader --no-update` | Запуск без обновления |
| `MoonTrader --stop` | Остановка ядра |
| `MoonTrader --status` | Статус процессов |
| `MoonTrader --attach` | Подключение к tmux-сессии |
| `MoonTrader --help` | Справка |

### Работа с tmux

```bash
# Запуск MoonTrader
MoonTrader

# Подключение к tmux-сессии
MoonTrader --attach

# Проверка tmux-сессий
tmux list-sessions
```

## MTGuardian

### Установка

1. Выберите `Custom installation`
2. Включите `Setup MT Guardian`
3. Введите параметры Telegram-бота

### Управление

```bash
systemctl status mtguardian
systemctl start mtguardian
systemctl stop mtguardian
systemctl restart mtguardian
tail -f ~/MTGuardian/MTGuardian.log
```

## Структура файлов

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

## Устранение неполадок

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

### Бэкап

```bash
tail -f /var/log/moontrader-profile-backup.log
/usr/local/bin/moontrader-profile-backup.sh
ls -la /root/.config/moontrader-data/backup
```
