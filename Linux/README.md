> [!WARNING]
> ## ⚠️ Репозиторий более не поддерживается
>
> Репозиторий переведён в архивный режим и **больше не развивается и не поддерживается автором**.
>
> - Код остаётся доступен «as is» для ознакомления и использования на свой страх и риск.
> - Работоспособность с актуальными версиями зависимостей и внешних сервисов **не гарантируется**.
> - Репозиторий можно свободно форкать — если вы хотите продолжить разработку, создавайте форк и развивайте его у себя.

---


# 🚀 Скрипт установки торгового терминала MoonTrader

Автоматический установщик торгового терминала MoonTrader для Linux систем с поддержкой мониторинга MTGuardian.

> **English version**: [README_EN.md](README_EN.md)

## 📋 Содержание

- [Системные требования](#-системные-требования)
- [Поддерживаемые системы](#-поддерживаемые-системы)
- [Быстрая установка](#-быстрая-установка)
- [Варианты установки](#-варианты-установки)
- [Управление MoonTrader](#-управление-moontrader)
- [MTGuardian - Мониторинг](#-mtguardian---мониторинг)
- [Структура файлов](#-структура-файлов)
- [Устранение неполадок](#-устранение-неполадок)

## ⚙️ Системные требования

- **ОС**: Ubuntu 20.04+, Debian 12+
- **Архитектура**: AMD64 (x86_64), ARM64 (aarch64)
- **Права**: root доступ
- **MoonTrader требования**: см. [официальный сайт](https://moontrader.com)

## 🖥️ Поддерживаемые системы

| Система | Версия | Статус |
|---------|--------|--------|
| Ubuntu | 20.04 LTS | ⚠️ Устаревшая операционная система |
| Ubuntu | 22.04 LTS | ✅ Протестировано |
| Ubuntu | 24.04 LTS | ✅ Протестировано |
| Debian | 12 | ⚠️ Протестировано, но не полностью |


## 🚀 Быстрая установка

### 1. Подключиться по SSH
Используйте SSH-клиент для подключения к серверу (выбор клиента на ваше усмотрение):
- **Windows**: например, [MobaXterm](https://mobaxterm.mobatek.net/), [mRemoteNG](https://mremoteng.org/) (ссылки приведены для примера и могут быть неактуальны)
- **Linux/Mac**: встроенный SSH-клиент в терминале

```bash
ssh user@your-server-ip
```

### 2. Повысить права до Root
```bash
# Для Ubuntu
sudo su

# Для Debian
su -
```

### 3. Установка
```bash
wget -O - https://raw.githubusercontent.com/SlippingForest/MoonTrader_install/master/Linux/install.sh | bash <(cat) </dev/tty
```

## 🔧 Варианты установки

### Автоматическая установка
- ✅ Установка MoonTrader
- ✅ Настройка времени (chrony)
- ✅ Настройка брандмауэра
- ✅ Настройка Fail2Ban
- ❌ MTGuardian (мониторинг)

### Пользовательская установка
- ✅ Выбор источника загрузки (официальный/Dropbox)
- ✅ Настройка времени (chrony)
- ✅ Настройка брандмауэра
- ✅ Настройка Fail2Ban
- ✅ **MTGuardian (мониторинг)**

## 🎮 Управление MoonTrader

### Основные команды

| Команда | Описание |
|---------|----------|
| `MoonTrader` | Запуск с обновлением |
| `MoonTrader --no-update` | Запуск без обновления |
| `MoonTrader --stop` | Остановка ядра |
| `MoonTrader --status` | Статус процессов |
| `MoonTrader --attach` | Подключение к tmux сессии |
| `MoonTrader --help` | Справка |

### Работа с tmux

#### Запуск в tmux сессии
```bash
# Запуск MoonTrader (создает сессию 'mt' автоматически)
MoonTrader

# Отключение от сессии (Ctrl+B, затем D)
# MoonTrader продолжит работать в фоне
```

#### Подключение к сессии
```bash
# Подключение к существующей сессии
tmux attach -t mt

# Или через команду MoonTrader
MoonTrader --attach
```

### Остановка и очистка

#### Остановка MoonTrader
```bash
# Мягкая остановка
MoonTrader --stop

# Принудительная остановка (если не реагирует)
pkill -f MTCore
```

#### Очистка данных
```bash
# Очистка профиля (сброс к настройкам по умолчанию)
rm -rf ~/.config/moontrader-data/data

# Очистка логов
rm -rf ~/.config/moontrader-data/data/logs/

# Очистка архивных данных
rm -rf ~/.config/moontrader-data/data/archive/
```

## 🛡️ MTGuardian - Мониторинг

MTGuardian - система мониторинга MoonTrader с уведомлениями в Telegram.

### Установка MTGuardian
1. Выберите **Custom installation** при установке
2. Включите опцию **Setup MT Guardian**
3. Введите данные Telegram бота:
   - **Server Name**: имя вашего сервера или свое название
   - **Bot Token**: токен Telegram бота
   - **Chat ID**: ID чата для уведомлений

### Управление MTGuardian
```bash
# Статус службы
systemctl status mtguardian

# Запуск службы
systemctl start mtguardian

# Остановка службы
systemctl stop mtguardian

# Перезапуск службы
systemctl restart mtguardian

# Просмотр логов
tail -f ~/MTGuardian/MTGuardian.log
```

### Управление автозапуском MTGuardian

```bash
# Включить автозапуск
sudo systemctl enable mtguardian

# Отключить автозапуск
sudo systemctl disable mtguardian

# Проверить статус
sudo systemctl is-enabled mtguardian

# Полная остановка (MTCore продолжит работать)
sudo systemctl stop mtguardian && sudo systemctl disable mtguardian
```

### Настройка Telegram бота
1. Создайте бота через [@BotFather](https://t.me/BotFather)
2. Получите токен бота
3. Узнайте Chat ID через [@myidbot](https://t.me/myidbot) или подобный

## 📁 Структура файлов

```
~/
├── moontrader/                    # Ядро MoonTrader
│   ├── MTCore                     # Исполняемый файл
│   └── start_mt.sh                   # Скрипт запуска
├── .config/moontrader-data/       # Конфигурация и данные
│   └── data/
│       ├── default.profile       # Профиль по умолчанию
│       ├── logs/                 # Логи
│       └── archive/              # Архивные данные
└── MTGuardian/                   # Система мониторинга
    ├── MTGuardian                # Основной скрипт
    ├── MTGuardian.settings       # Настройки
    └── MTGuardian.log           # Логи мониторинга
```

### Быстрая навигация
```bash
# Переход в каталог ядра
cd ~/moontrader

# Переход в каталог конфигурации
cd ~/.config/moontrader-data/

# Переход в каталог MTGuardian
cd ~/MTGuardian
```

## 🔧 Устранение неполадок

### Проблемы с запуском
```bash
# Проверка статуса
MoonTrader --status

# Проверка процессов
ps aux | grep MTCore

# Проверка tmux сессий
tmux list-sessions

# Подключение к сессии MoonTrader (если существует)
tmux attach -t mt
```

### Проблемы с MTGuardian
```bash
# Проверка службы
systemctl status mtguardian

# Просмотр логов
journalctl -u mtguardian -f

# Проверка конфигурации
cat ~/MTGuardian/MTGuardian.settings
```

### Проблемы с сетью
```bash
# Проверка брандмауэра
iptables -L

# Проверка портов
netstat -tulpn | grep 4242
```

---

**Удачной торговли! 🚀📈**
