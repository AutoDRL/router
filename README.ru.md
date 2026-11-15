# router

**DDNS-клиент (dynamic DNS) промышленного класса · версия 0.1.0**

[简体中文](README.md) | [English](README.en.md) | [**Русский**](README.ru.md) | [Deutsch](README.de.md) | [日本語](README.ja.md)

> **В этом репозитории публикуются только скомпилированные двоичные файлы клиента. Исходный код здесь не публикуется.**
> Сайт проекта: <https://router.roboter.site> · Загрузки: **[GitHub Releases](https://github.com/AutoDRL/router/releases/latest)**

router отслеживает публичные адреса IPv4 и IPv6 машины и синхронизирует с ними записи A и AAAA у вашего DNS-провайдера. Это один нативный исполняемый файл без среды выполнения, пакетного менеджера и внешней базы данных. Управлять им можно через локальный веб-интерфейс, а также запускать как системную службу.

> **Требуется лицензия: бесплатной версии в этом репозитории нет.** Для каждого двоичного файла нужны приобретённая лицензия и активация, привязанная к устройству; без активации установка не может обновлять DNS или обслуживать веб-интерфейс и завершается с кодом 3. Приобретение и активация: <https://router.roboter.site>; адрес службы активации показывает `router license status`.

## Статистика проекта

Значки ниже обновляются в реальном времени сервисом [Shields.io](https://shields.io) на основе публичных показателей GitHub. Счётчики загрузок учитывают файлы, опубликованные в Releases; число загрузок каждого файла показано на странице соответствующего Release.

| Показатель | Актуальное значение |
| --- | --- |
| Всего загрузок Release | [![Release downloads](https://img.shields.io/github/downloads/AutoDRL/router/total?style=flat-square&label=downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases) |
| Загрузки последнего Release | [![Latest release downloads](https://img.shields.io/github/downloads/AutoDRL/router/latest/total?style=flat-square&label=latest%20downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |
| Форки | [![Forks](https://img.shields.io/github/forks/AutoDRL/router?style=flat-square&label=forks&logo=github&logoColor=white)](https://github.com/AutoDRL/router/forks) |
| Звёзды | [![Stars](https://img.shields.io/github/stars/AutoDRL/router?style=flat-square&label=stars&logo=github&logoColor=white)](https://github.com/AutoDRL/router/stargazers) |
| Наблюдатели | [![Watchers](https://img.shields.io/github/watchers/AutoDRL/router?style=flat-square&label=watchers&logo=github&logoColor=white)](https://github.com/AutoDRL/router/watchers) |
| Последний релиз | [![Latest release](https://img.shields.io/github/v/release/AutoDRL/router?style=flat-square&label=release&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |

## Возможности

- **31 DNS-провайдер**: Alibaba Cloud, Tencent Cloud, Huawei Cloud, Volcano Engine, Baidu Cloud, AWS Route 53, Cloudflare, GoDaddy, Namecheap, DigitalOcean, Porkbun, NameSilo, dynv6, Vercel, deSEC, ClouDNS, Gcore, NS1, Name.com, Spaceship, Dynadot, DNSPod, Now.cn, Eranet, Tnet.hk и другие.
- **Независимое обновление IPv4 и IPv6**: записи A и AAAA включаются и настраиваются отдельно.
- **Пять источников адреса**: HTTP-эндпоинт с отказоустойчивостью, локальный интерфейс, внешняя команда, DNS-проверка и статическое значение.
- **Политика адресов и повторы**: частные, loopback, link-local, CGNAT- и документационные диапазоны отклоняются; пока адрес не изменился, удалённые запросы не выполняются; при ограничении частоты включается экспоненциальная задержка с учётом `Retry-After`.
- **Встроенный веб-интерфейс**: создание администратора при первом запуске, хеширование пароля Argon2id, защита от CSRF, ограничение попыток входа, журнал в реальном времени, метрики Prometheus и проверки состояния.
- **Пять языков**: упрощённый китайский, английский, русский, немецкий и японский — для CLI, журналов, веб-интерфейса, ошибок REST API и встроенной справки.
- **Наблюдение через guardian**: `router run` запускает guardian, который контролирует рабочий процесс и перезапускает его после аварийного завершения — для круглосуточной работы без оператора.
- **Проверяемая поставка**: каждый архив снабжён контрольной суммой SHA-256; файл запускается без установки зависимостей.

## Выбор сборки

В версии 0.1.0 доступны **34 сборки**: 31 сборка для одного провайдера, китайская сборка (`cn`), международная сборка (`global`) и полная сборка со всеми провайдерами. Каждая сборка доступна для всех 10 целевых платформ, перечисленных ниже.

| Сборка | Метка в имени архива | Провайдеров | Для чего подходит |
| --- | --- | --- | --- |
| Полная | нет | 31 | Несколько провайдеров, сомнения в выборе или системный установщик для x86_64 Linux / Windows |
| Китайская | `cn` | 13 | Только DNS-сервисы материкового Китая; зарубежные провайдеры не компилируются |
| Международная | `global` | 20 | Только DNS-сервисы за пределами материкового Китая; провайдеры материкового Китая не компилируются |
| Для одного провайдера | ID провайдера, например `cloudflare` | 1 | Один провайдер, минимальный двоичный файл и минимальный набор зависимостей |

Сборки `cn` и `global` обе включают два нейтральных компонента (HiPM DNS Manager и универсальный callback). Поэтому их наборы пересекаются, но вместе они всё равно покрывают все 31 провайдера. Если в конфигурации указан провайдер, которого нет в текущем двоичном файле, перейдите на сборку, содержащую его; `router provider list` показывает, что именно включено в текущий файл.

<details>
<summary><strong>Китайская сборка (cn): 11 провайдеров материкового Китая + 2 нейтральных компонента</strong></summary>

Alibaba Cloud DNS (`aliyun`), Alibaba Cloud ESA (`aliesa`), Tencent Cloud DNSPod (`tencent`), Tencent Cloud EdgeOne (`edgeone`), Volcano Engine DNS (`trafficroute`), Huawei Cloud DNS (`huawei`), Baidu Cloud DNS (`baidu`), DNSPod (`dnspod`), DNSLA (`dnsla`), RainYun (`rainyun`), Now.cn (`nowcn`), HiPM DNS Manager (`hipmdnsmgr`), универсальный callback (`callback`).

</details>

<details>
<summary><strong>Международная сборка (global): 18 зарубежных провайдеров + 2 нейтральных компонента</strong></summary>

AWS Route 53 (`route53`), Cloudflare (`cloudflare`), GoDaddy (`godaddy`), Namecheap (`namecheap`), DigitalOcean (`digitalocean`), Porkbun (`porkbun`), NameSilo (`namesilo`), dynv6 (`dynv6`), Vercel (`vercel`), deSEC (`desec`), ClouDNS (`cloudns`), Gcore (`gcore`), NS1 (`nsone`), Name.com (`namecom`), Spaceship (`spaceship`), Dynadot (`dynadot`), Eranet (`eranet`), Tnet.hk (`tnethk`), HiPM DNS Manager (`hipmdnsmgr`), универсальный callback (`callback`).

</details>

Правила именования архивов (`<target>` описан в следующем разделе):

| Сборка | Linux / macOS / Android | Windows |
| --- | --- | --- |
| Полная | `router-v0.1.0-<target>.tar.gz` | `router-v0.1.0-<target>.zip` |
| Китайская | `router-v0.1.0-cn-<target>.tar.gz` | `router-v0.1.0-cn-<target>.zip` |
| Международная | `router-v0.1.0-global-<target>.tar.gz` | `router-v0.1.0-global-<target>.zip` |
| Для одного провайдера | `router-v0.1.0-<provider>-<target>.tar.gz` | `router-v0.1.0-<provider>-<target>.zip` |

Полная сборка использует имя архива без метки варианта, например `router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz`. Системные установщики (`.deb`, `.rpm`, `.apk`, `.pkg.tar.zst`, `.msi`) публикуются только для полной сборки; `cn`, `global` и сборки для одного провайдера поставляются только архивами.

## Поддерживаемые платформы

| Операционная система | Архитектура | `<target>` в имени архива | Примечание |
| --- | --- | --- | --- |
| Linux | x86_64 | `x86_64-unknown-linux-gnu` | glibc |
| Linux | aarch64 | `aarch64-unknown-linux-gnu` | glibc |
| Linux | armv7 | `armv7-unknown-linux-gnueabihf` | hard-float |
| Linux | i686 | `i686-unknown-linux-gnu` | glibc |
| macOS | Intel | `x86_64-apple-darwin` | `.tar.gz`; macOS 10.12 или новее |
| macOS | Apple silicon | `aarch64-apple-darwin` | `.tar.gz`; macOS 11.0 или новее |
| Windows | x86_64 | `x86_64-pc-windows-msvc` | `.zip` |
| Android / Termux | aarch64 | `aarch64-linux-android` | Bionic libc |
| Android / Termux | armv7 | `armv7-linux-androideabi` | Bionic libc |
| Android / Termux | x86_64 | `x86_64-linux-android` | Bionic libc |

Примечания:

- Архивы Linux `.tar.gz` в версии 0.1.0 динамически слинкованы с glibc 2.17 и требуют glibc 2.17 или новее; статического архива musl нет. Пользователям Alpine следует устанавливать `.apk` полной сборки (в текущем релизе только для x86_64); двоичный файл внутри статически слинкован с musl и не требует glibc.
- Сборки Android используют Bionic libc и требуют Android 7.0 (API 24) или новее. Они предназначены для Termux или оболочки Android; в обычном дистрибутиве Linux они не запускаются.
- Сборок для 32-битной Windows, Windows on ARM и 32-битной macOS нет. Выбирайте архив по выводу `uname -m` (в Windows — по значению `$env:PROCESSOR_ARCHITECTURE`).
- Каждый архив содержит один исполняемый файл: `router` (`router.exe` в Windows).

## Проверка загрузки

У каждого архива есть файл `.sha256`; он также есть у установщика Windows MSI. Контрольные суммы пакетов Linux перечислены в `router_0.1.0_checksums.txt`. Проверьте файл до запуска:

```bash
# Linux
sha256sum -c router-v0.1.0-cn-x86_64-unknown-linux-gnu.tar.gz.sha256

# macOS
shasum -a 256 -c router-v0.1.0-aarch64-apple-darwin.tar.gz.sha256
```

```powershell
# Windows: прочитайте ожидаемое значение, затем вычислите хеш загруженного файла
Get-Content .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip.sha256
Get-FileHash .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip -Algorithm SHA256
```

Значения должны совпасть. Если проверка не прошла, не запускайте файл — загрузите его заново. Если ошибка повторяется, сообщите о ней через issue на странице релиза. Для установщиков без подписи кода SHA-256 подтверждает целостность файла (он не повреждён и не изменён), но не заменяет аутентификацию издателя.

## Быстрый старт (архивы)

### Linux

```bash
tar -xzf router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz
chmod +x router
./router config init
./router run
```

### macOS

```bash
tar -xzf router-v0.1.0-aarch64-apple-darwin.tar.gz   # для Intel: x86_64-apple-darwin
chmod +x router
xattr -d com.apple.quarantine ./router 2>/dev/null || true
./router config init
./router lang set ru
./router run
```

Если macOS не может подтвердить разработчика, сборка не нотаризована Apple. Сначала проверьте SHA-256, затем разрешите запуск в разделе «Системные настройки → Конфиденциальность и безопасность». В 0.1.0 нет установщика macOS `.pkg`; используйте архив `.tar.gz`.

### Windows

В PowerShell:

```powershell
Expand-Archive .\router-v0.1.0-x86_64-pc-windows-msvc.zip -DestinationPath .\router
cd .\router
.\router.exe config init
.\router.exe lang set ru
.\router.exe run
```

### Android / Termux

```bash
tar -xzf router-v0.1.0-aarch64-linux-android.tar.gz
chmod +x router
./router config init
./router run
```

Затем откройте **<http://127.0.0.1:9876>** в браузере. При первом открытии потребуется задать имя администратора и пароль; после этого можно добавлять провайдеров, учётные данные и домены. По умолчанию сервер слушает только loopback-адрес и не доступен из сети.

Для работы в фоне используйте `./router run --daemon`. Для долгосрочной эксплуатации установите полную сборку как системную службу (см. ниже); другие сборки можно запускать под управлением собственного супервизора. В контейнерах и при отладке можно запустить один процесс с `--no-guardian`.

## Системные установщики (только полная сборка)

Перечисленные ниже установщики доступны только для x86_64; для других архитектур используйте соответствующий архив. Каждый установщик содержит конфигурацию по умолчанию, созданную тем же двоичным файлом, а обновление сохраняет существующую конфигурацию.

| Платформа | Пример файла | Команда установки |
| --- | --- | --- |
| Debian / Ubuntu | `router_0.1.0_amd64.deb` | `sudo dpkg -i router_0.1.0_amd64.deb` |
| RHEL / Fedora / openSUSE | `router-0.1.0-1.x86_64.rpm` | `sudo rpm -U router-0.1.0-1.x86_64.rpm` |
| Alpine | `router_0.1.0_x86_64.apk` (статический musl) | `sudo apk add --allow-untrusted router_0.1.0_x86_64.apk` |
| Arch Linux | `router-0.1.0-1-x86_64.pkg.tar.zst` | `sudo pacman -U router-0.1.0-1-x86_64.pkg.tar.zst` |
| Windows x64 | `router-v0.1.0-x86_64.msi` | Запустите `msiexec /i router-v0.1.0-x86_64.msi` от имени администратора |

Пути установки и запуск:

- **Linux (пакеты deb, rpm и Arch, дистрибутивы с systemd)**: двоичный файл — `/usr/bin/router`, конфигурация — `/etc/router/router.yaml`, в пакет входит unit systemd. Пакет включает unit при установке, но не запускает его; запустите службу командой `sudo router service start`. Служба работает от непривилегированного пользователя `router`. Не выполняйте `router service install` для установки, управляемой пакетом: команда записывает отдельный unit в `/etc/systemd/system/router.service`, перекрывая unit из пакета.
- **Alpine (`.apk`)**: устанавливает `/usr/bin/router` и `/etc/router/router.yaml`. Пакет также содержит unit systemd, но Alpine не использует systemd, поэтому при установке служба не регистрируется и не включается. В 0.1.0 нет unit OpenRC; запускайте `router run --daemon` или добавьте собственный сервис OpenRC либо супервизор.
- **macOS (архив)**: скопируйте `router` в `/usr/local/bin/router` и выполните `sudo router service install`, чтобы установить и запустить LaunchDaemon; конфигурация службы находится в `/Library/Application Support/router/router.yaml`. Для удаления выполните `sudo router service uninstall`, затем удалите `/usr/local/bin/router`. В версии 0.1.0 нет `.pkg`; используйте архив из раздела «Быстрый старт».
- **Windows**: двоичный файл — `C:\Program Files\router\router.exe`, конфигурация — `%ProgramData%\router\router.yaml`. В PowerShell с правами администратора выполните `& "C:\Program Files\router\router.exe" service install`, чтобы зарегистрировать задачу для запуска при загрузке. Перед удалением продукта выполните `service uninstall`.
- **Termux**: после установки `termux-services` команда `router service install` создаёт службу `runit` и управляет ею.

## Конфигурация и использование

Конфигурация хранится в YAML-файле; веб-интерфейс, CLI и ручное редактирование используют один и тот же файл:

```bash
router config path                 # показать используемый путь к конфигурации
router config init                 # создать конфигурацию по умолчанию
router config validate             # проверить конфигурацию
router config show                 # показать её (конфиденциальные поля скрыты)
```

Пути по умолчанию:

- Архивы: `$XDG_CONFIG_HOME/router/router.yaml`, иначе `~/.config/router/router.yaml`; в Windows — `%APPDATA%\router\router.yaml`.
- Пакет Linux: `/etc/router/router.yaml`.
- Служба macOS (`router service install`): `/Library/Application Support/router/router.yaml`.
- MSI Windows: `%ProgramData%\router\router.yaml`.

Другой файл задаётся через `-c/--config` или переменную окружения `ROUTER_CONFIG`. Минимальный пример (используется Cloudflare, поэтому нужна одна из сборок: полная, международная или `cloudflare`; в китайской сборке используйте одного из её провайдеров):

```yaml
schema_version: 1
language: "ru"
web:
  listen: "127.0.0.1:9876"
update:
  interval_secs: 300
providers:
  - id: cloudflare
    credentials:
      api_token: "env:CLOUDFLARE_API_TOKEN"
    ipv4:
      enabled: true
      source: { type: url, urls: ["https://ipv4.icanhazip.com"] }
      domains: ["home.example.com"]
```

`env:ИМЯ` читает учётные данные из окружения процесса. Если router работает как системная служба, убедитесь, что переменная доступна этому процессу, или введите данные через веб-интерфейс. `router provider list --json` показывает обязательные поля и ссылку на документацию для каждого провайдера.

Основные команды:

| Команда | Назначение |
| --- | --- |
| `router provider list` | Показать провайдеров, включённых в этот двоичный файл |
| `router provider test <id>` | Проверить учётные данные и разрешение зоны |
| `router update-now` | Немедленно выполнить один цикл синхронизации |
| `router config validate` | Проверить конфигурацию |
| `router healthcheck --ready` | Проверить состояние; ошибка, если в последнем цикле все записи завершились ошибкой |
| `router password reset` | Сбросить пароль для входа в веб-интерфейс |
| `router docs ru` | Прочитать встроенную справку |
| `router version --json` | Показать версию, целевую платформу и сведения о сборке |

## Языки

Поддерживаются коды `zh`, `en`, `ru`, `de`, `ja`. Один язык применяется к CLI, журналам, веб-интерфейсу, ошибкам REST API и встроенной справке:

```bash
router lang list
router lang show
router lang set ru       # сохранить для следующего запуска
router --lang ru provider list   # только для этой команды
ROUTER_LANG=ja router provider list
```

Язык, выбранный в веб-интерфейсе, сразу применяется к работающей службе. Новая конфигурация по умолчанию использует упрощённый китайский; чтобы служба запускалась на другом языке, выполните `router lang set ru` (или `en` / `de` / `ja`) до первого запуска.

## Безопасность

- Загружайте файлы только со страницы официального релиза и проверяйте SHA-256 перед запуском.
- По умолчанию веб-интерфейс слушает `127.0.0.1:9876`. Для внешнего доступа задайте надёжный пароль и завершайте HTTPS на обратном прокси. При работе за прокси добавьте внешнее имя хоста в `web.allowed_hosts`, иначе запросы с несовпадающим заголовком `Host` будут отклонены.
- Конфигурация содержит API-учётные данные и хеш пароля. Разрешите доступ к файлу только владельцу; не добавляйте его в репозиторий или публичный бэкап.
- Используйте DNS API-токены с минимальными правами, только для нужных зон и записей.
- Для долгосрочной работы предпочитайте системную службу: менеджер служб запустит её при загрузке и будет следить за процессом. `--no-guardian` применяйте только в контейнерах и при отладке.

## Платная лицензия и активация

**Все двоичные файлы в этом репозитории — платные коммерческие сборки. Бесплатной, общественной или пробной версии не существует.** Полная сборка, `cn`, `global` и сборки для отдельного провайдера одинаково требуют приобретённой лицензии и активации, привязанной к устройству, прежде чем они начнут обновлять DNS или обслуживать веб-интерфейс.

Коммерческая лицензия стоит **19,90 доллара США, оплачивается один раз и действует пожизненно на одном устройстве**. Каждый код активации привязан ровно к одному устройству; подписки, продления, бесплатной версии и многопользовательской лицензии нет. Для второго устройства нужен отдельный код активации.

```bash
router license status
router license device-code
router license activate <КОД-АКТИВАЦИИ>
```

- `router license device-code` выводит код устройства. Передайте его в систему лицензирования и оплатите лицензию, чтобы получить подписанный код активации.
- `router license activate <КОД-АКТИВАЦИИ>` активирует устройство офлайн; клиенту не требуется постоянный сетевой обмен.
- Если лицензия отсутствует, повреждена или привязана к другому устройству, `router run`, `router update-now` и веб-интерфейс отказывают в обслуживании и завершаются с кодом 3. Первая активация должна выполняться через CLI.
- Лицензии привязаны к отпечатку устройства; в контейнерах рекомендуется явно задать `ROUTER_INSTANCE_ID`.
- `license.mode` в конфигурации не может отключить или ослабить проверку лицензии в опубликованных сборках: они всегда требуют активации, привязанной к устройству.

Приобретение, развёртывание и поддержка: **<https://router.roboter.site>** или **roboter.site@aliyun.com**.

## Устранение неполадок

| Симптом | Что сделать |
| --- | --- |
| `command not found` или программа не найдена | Распакуйте архив и запускайте `./router` из каталога, в который распакован архив (Windows: `.\router.exe`), либо добавьте этот каталог в `PATH`. |
| `Permission denied` | Выполните `chmod +x router`. В macOS, если разработчик не подтверждён, см. примечание о карантине в разделе «Быстрый старт». |
| `cannot execute binary file` или ошибка формата | Загружен архив для другой платформы или архитектуры; проверьте `uname -m` и скачайте архив для нужной платформы. |
| Сообщение, что провайдер не входит в сборку | Провайдера нет в текущей сборке; используйте полную, китайскую, международную сборку или сборку для этого провайдера. |
| Веб-интерфейс недоступен | Проверьте `web.listen` в конфигурационном файле (путь к нему показывает `router config path`), локальный брандмауэр и состояние процесса (`router healthcheck`). |
| Конфигурация отклонена | Запустите `router config validate`, чтобы увидеть проблемное поле; сначала сделайте резервную копию, затем исправьте файл через веб-интерфейс. |
| Язык не изменился | `router lang show` покажет источник. Параметр командной строки действует только для этого вызова; изменение в веб-интерфейсе сразу применяется к работающей службе. |
| Опубликованная сборка сразу завершается с кодом 3 | Она не активирована или лицензия недействительна; выполните `router license status` и `router license activate <КОД-АКТИВАЦИИ>`. |
| Linux сообщает, что glibc слишком старый | Архивы Linux версии 0.1.0 требуют glibc 2.17 или новее. Обновите систему или в Alpine установите пакет `.apk` (в этом релизе — для x86_64), внутри которого находится статически слинкованный musl-бинарник. Статический musl отдельным архивом не поставляется. |

## Уведомление о коммерческой лицензии

router и его опубликованные двоичные файлы — **закрытое коммерческое программное обеспечение AutoDRL**. Они не распространяются по MIT, Apache-2.0 или любой другой лицензии с открытым исходным кодом, бесплатная версия не предоставляется, все права защищены.

Копировать, изменять, распространять, перепродавать, сдавать в аренду, сублицензировать или декомпилировать программное обеспечение запрещено, кроме случаев, предусмотренных подписанным письменным коммерческим соглашением с AutoDRL. Загрузка, хранение или установка двоичного файла не даёт права на его запуск; обслуживание доступно только после действительной активации.

Полное уведомление — в [LICENSE](LICENSE). Документация в этом репозитории описывает продукт и не предоставляет лицензию на использование двоичных файлов. Приобретение и активация: **<https://router.roboter.site>**.

Репозиторий распространяет только двоичные файлы и не содержит исходного кода. Документ описывает версию 0.1.0.

[简体中文](README.md) | [English](README.en.md) | [**Русский**](README.ru.md) | [Deutsch](README.de.md) | [日本語](README.ja.md)
