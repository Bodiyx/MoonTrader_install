# Windows Server install

Скрипт:
- устанавливает актуальную версию MoonTrader в `C:/MoonTrader`
- устанавливает обновления Windows
- оптимизирует систему, отключая лишние службы и по возможности телеметрию
- устанавливает защиту от перебора паролей RDP через IPBan
- открывает нужные порты в Firewall для корректной работы MoonTrader
- удаляет Windows Defender

Проверен на:
- Windows Server 2019 v1809

## Установка

1. Откройте PowerShell от имени администратора.
2. Выполните команды:

```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/Bodiyx/MoonTrader_install/master/WindowsServer/install.ps1'))
```

3. После выполнения скрипта перезагрузите сервер, если установщик попросит подтвердить перезапуск.

## Пути к файлам

```powershell
dir C:/MoonTrader/
```
