> [!WARNING]
> ## ⚠️ Репозиторий более не поддерживается
>
> Репозиторий переведён в архивный режим и **больше не развивается и не поддерживается автором**.
>
> - Код остаётся доступен «as is» для ознакомления и использования на свой страх и риск.
> - Работоспособность с актуальными версиями зависимостей и внешних сервисов **не гарантируется**.
> - Репозиторий можно свободно форкать — если вы хотите продолжить разработку, создавайте форк и развивайте его у себя.

# Windows Server install

- Скрипт: 
  - устанавливает актуальную версию MoonTrader в папку C:/MoonTrader
  - устанавливает обновления Windows
  - оптимизирует систему отключая лишние службы и по возможности телеметрию
  - устанавливает защиту от перебора паролей RDP - IPBan https://github.com/DigitalRuby/IPBan
  - открывает нужные порты в FireWall для корректной работы MoonTrader
  - удаляет Windows Defender
- Проверен на:
  - Windows Server 2019 v1809
- Примечания:
  - при большом количестве обновлений Windows, пропорционально долго будет их ставить
  - при большом количестве установленных обновлений Windows, первая перезагрузка сервера будет дольше обычного

# Установка

1. Открыть консоль PowerShell от администратора

<p align="center"> 
<img src="https://user-images.githubusercontent.com/47079447/159792911-c469d0ca-2f3d-4919-9f23-2c1f21688196.gif">
</p>

2. Вставить и запустить команды:
```bash
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```
```bash
iex ((New-Object System.Net.WebClient).DownloadString('https://raw.githubusercontent.com/SlippingForest/MoonTrader_install/master/WindowsServer/install.ps1'))
```
3. После выполнения скрипта, перезагрузить сервер(ответить "y")

# Пути к файлам MoonTrader

Каталог MoonTrader после установки
```bash
dir C:/MoonTrader/
```


