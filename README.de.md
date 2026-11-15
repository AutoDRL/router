# router

**DDNS-Client (Dynamic DNS) für den Produktiveinsatz · Version 0.1.0**

[简体中文](README.md) | [English](README.en.md) | [Русский](README.ru.md) | [**Deutsch**](README.de.md) | [日本語](README.ja.md)

> **Dieses Repository stellt ausschließlich kompilierte Client-Binärdateien bereit. Der Quellcode wird hier nicht veröffentlicht.**
> Projektwebsite: <https://router.roboter.site> · Downloads: **[GitHub Releases](https://github.com/AutoDRL/router/releases/latest)**

router überwacht die öffentlichen IPv4- und IPv6-Adressen des Rechners und hält die A- und AAAA-Einträge beim DNS-Anbieter aktuell. Das Programm läuft als einzelne native ausführbare Datei ohne Laufzeitumgebung, Paketmanager oder externe Datenbank. Sie können es über eine lokale Weboberfläche verwalten oder als Systemdienst betreiben.

> **Lizenzpflicht: In diesem Repository gibt es keine kostenlose Version.** Jede Binärdatei erfordert eine erworbene Lizenz und eine gerätegebundene Aktivierung; ohne Aktivierung kann die Installation weder DNS-Einträge aktualisieren noch die Weboberfläche bereitstellen und beendet sich mit Status 3. Kauf und Aktivierung: <https://router.roboter.site>; die Adresse des Aktivierungsdienstes zeigt `router license status` an.

## Projektstatistik

Die folgenden Badges werden von [Shields.io](https://shields.io) in Echtzeit aus den öffentlichen GitHub-Kennzahlen erzeugt. Die Download-Zähler erfassen die in Releases veröffentlichten Assets; die Anzahl je Datei steht auf der jeweiligen Release-Seite.

| Kennzahl | Aktueller Wert |
| --- | --- |
| Release-Downloads gesamt | [![Release downloads](https://img.shields.io/github/downloads/AutoDRL/router/total?style=flat-square&label=downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases) |
| Downloads des neuesten Release | [![Latest release downloads](https://img.shields.io/github/downloads/AutoDRL/router/latest/total?style=flat-square&label=latest%20downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |
| Forks | [![Forks](https://img.shields.io/github/forks/AutoDRL/router?style=flat-square&label=forks&logo=github&logoColor=white)](https://github.com/AutoDRL/router/forks) |
| Stars | [![Stars](https://img.shields.io/github/stars/AutoDRL/router?style=flat-square&label=stars&logo=github&logoColor=white)](https://github.com/AutoDRL/router/stargazers) |
| Watcher | [![Watchers](https://img.shields.io/github/watchers/AutoDRL/router?style=flat-square&label=watchers&logo=github&logoColor=white)](https://github.com/AutoDRL/router/watchers) |
| Neueste Version | [![Latest release](https://img.shields.io/github/v/release/AutoDRL/router?style=flat-square&label=release&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |

## Funktionsumfang

- **31 DNS-Anbieter**: Alibaba Cloud, Tencent Cloud, Huawei Cloud, Volcano Engine, Baidu Cloud, AWS Route 53, Cloudflare, GoDaddy, Namecheap, DigitalOcean, Porkbun, NameSilo, dynv6, Vercel, deSEC, ClouDNS, Gcore, NS1, Name.com, Spaceship, Dynadot, DNSPod, Now.cn, Eranet, Tnet.hk und weitere.
- **Unabhängige Aktualisierung von IPv4 und IPv6**: A- und AAAA-Einträge werden getrennt aktiviert und konfiguriert.
- **Fünf Adressquellen**: HTTP-Endpunkt mit Ausweichadressen, lokale Schnittstelle, externes Kommando, DNS-Abfrage und fester Wert.
- **Adressrichtlinie und Wiederholungen**: private, Loopback-, Link-Local-, CGNAT- und Dokumentationsbereiche werden abgelehnt; solange sich die Adresse nicht ändert, entfallen wiederholte externe Abfragen; bei Ratenbegrenzung greift exponentielles Backoff unter Beachtung von `Retry-After`.
- **Integrierte Weboberfläche**: Einrichtung des Administrators beim ersten Start, Argon2id-Passwort-Hashing, CSRF-Schutz, Begrenzung der Anmeldeversuche, Live-Protokoll, Prometheus-Metriken und Integritätsendpunkte.
- **Fünf Sprachen**: vereinfachtes Chinesisch, Englisch, Russisch, Deutsch und Japanisch — für CLI, Protokolle, Weboberfläche, REST-API-Fehler und die eingebettete Hilfe.
- **Guardian-Überwachung**: `router run` startet einen Guardian, der den Arbeitsprozess überwacht und ihn nach einem abnormalen Ende neu startet — für den unbeaufsichtigten 24/7-Betrieb.
- **Nachvollziehbare Auslieferung**: Jedes Archiv besitzt eine SHA-256-Prüfsumme; die Binärdatei läuft ohne Installation zusätzlicher Abhängigkeiten.

## Variante auswählen

Version 0.1.0 umfasst **34 Varianten**: 31 Varianten für je einen Anbieter, eine reine China-Variante (`cn`), eine internationale Variante (`global`) und die Vollversion mit allen Anbietern. Jede Variante ist für alle 10 unten aufgeführten Zielplattformen verfügbar.

| Variante | Kennung im Dateinamen | Anbieter | Geeignet für |
| --- | --- | --- | --- |
| Vollversion | keine | 31 | Gemischte Anbieter, unklare Wahl oder wenn ein Linux-/Windows-Systeminstallationspaket benötigt wird |
| China | `cn` | 13 | Ausschließlich DNS-Dienste aus dem chinesischen Festland; keine ausländischen Anbieter werden kompiliert |
| International | `global` | 20 | Ausschließlich DNS-Dienste außerhalb des chinesischen Festlands; keine Anbieter aus dem Festland werden kompiliert |
| Ein Anbieter | Anbieter-ID, z. B. `cloudflare` | 1 | Nur ein Anbieter, kleinste Binärdatei und kleinster Abhängigkeitsumfang |

`cn` und `global` enthalten beide die zwei neutralen Komponenten (HiPM DNS Manager und den generischen Callback). Ihre Anbieterzahlen überschneiden sich daher, zusammen decken sie aber weiterhin alle 31 Anbieter ab. Wenn Ihre Konfiguration einen Anbieter nennt, der in der laufenden Binärdatei nicht enthalten ist, wechseln Sie zu einer passenden Variante; `router provider list` zeigt genau, was die aktuelle Binärdatei enthält.

<details>
<summary><strong>China-Variante (cn): 11 Anbieter aus dem chinesischen Festland + 2 neutrale Komponenten</strong></summary>

Alibaba Cloud DNS (`aliyun`), Alibaba Cloud ESA (`aliesa`), Tencent Cloud DNSPod (`tencent`), Tencent Cloud EdgeOne (`edgeone`), Volcano Engine DNS (`trafficroute`), Huawei Cloud DNS (`huawei`), Baidu Cloud DNS (`baidu`), DNSPod (`dnspod`), DNSLA (`dnsla`), RainYun (`rainyun`), Now.cn (`nowcn`), HiPM DNS Manager (`hipmdnsmgr`), generischer Callback (`callback`).

</details>

<details>
<summary><strong>Internationale Variante (global): 18 ausländische Anbieter + 2 neutrale Komponenten</strong></summary>

AWS Route 53 (`route53`), Cloudflare (`cloudflare`), GoDaddy (`godaddy`), Namecheap (`namecheap`), DigitalOcean (`digitalocean`), Porkbun (`porkbun`), NameSilo (`namesilo`), dynv6 (`dynv6`), Vercel (`vercel`), deSEC (`desec`), ClouDNS (`cloudns`), Gcore (`gcore`), NS1 (`nsone`), Name.com (`namecom`), Spaceship (`spaceship`), Dynadot (`dynadot`), Eranet (`eranet`), Tnet.hk (`tnethk`), HiPM DNS Manager (`hipmdnsmgr`), generischer Callback (`callback`).

</details>

Namensschema der Archive (`<target>` ist im nächsten Abschnitt beschrieben):

| Variante | Linux / macOS / Android | Windows |
| --- | --- | --- |
| Vollversion | `router-v0.1.0-<target>.tar.gz` | `router-v0.1.0-<target>.zip` |
| China | `router-v0.1.0-cn-<target>.tar.gz` | `router-v0.1.0-cn-<target>.zip` |
| International | `router-v0.1.0-global-<target>.tar.gz` | `router-v0.1.0-global-<target>.zip` |
| Ein Anbieter | `router-v0.1.0-<provider>-<target>.tar.gz` | `router-v0.1.0-<provider>-<target>.zip` |

Die Vollversion verwendet den Archivnamen ohne Variantenkennung, zum Beispiel `router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz`. Systeminstallationspakete (`.deb`, `.rpm`, `.apk`, `.pkg.tar.zst`, `.msi`) erscheinen nur für die Vollversion; `cn`, `global` und Anbieter-Varianten werden ausschließlich als Archive angeboten.

## Unterstützte Plattformen

| Betriebssystem | Architektur | `<target>` im Dateinamen | Hinweis |
| --- | --- | --- | --- |
| Linux | x86_64 | `x86_64-unknown-linux-gnu` | glibc |
| Linux | aarch64 | `aarch64-unknown-linux-gnu` | glibc |
| Linux | armv7 | `armv7-unknown-linux-gnueabihf` | Hard-Float |
| Linux | i686 | `i686-unknown-linux-gnu` | glibc |
| macOS | Intel | `x86_64-apple-darwin` | `.tar.gz`; macOS 10.12 oder neuer |
| macOS | Apple Silicon | `aarch64-apple-darwin` | `.tar.gz`; macOS 11.0 oder neuer |
| Windows | x86_64 | `x86_64-pc-windows-msvc` | `.zip` |
| Android / Termux | aarch64 | `aarch64-linux-android` | Bionic libc |
| Android / Termux | armv7 | `armv7-linux-androideabi` | Bionic libc |
| Android / Termux | x86_64 | `x86_64-linux-android` | Bionic libc |

Hinweise:

- Die Linux-`.tar.gz`-Archive in 0.1.0 sind gegen glibc 2.17 gelinkt und erfordern glibc 2.17 oder neuer; ein statisches musl-Archiv gibt es nicht. Alpine-Nutzer sollten das `.apk` der Vollversion installieren (in dieser Version nur für x86_64); die enthaltene Binärdatei ist statisch mit musl gelinkt und benötigt kein glibc.
- Android-Varianten sind gegen Bionic libc gelinkt und erfordern Android 7.0 (API 24) oder neuer. Sie sind für Termux oder eine Android-Shell gedacht; auf einer regulären Linux-Distribution laufen sie nicht.
- Es gibt keine Variante für 32-Bit-Windows, Windows on ARM oder 32-Bit-macOS. Wählen Sie das Archiv passend zu `uname -m` (unter Windows: `$env:PROCESSOR_ARCHITECTURE`).
- Jedes Archiv enthält genau eine ausführbare Datei: `router` (unter Windows `router.exe`).

## Download prüfen

Jedes Archiv besitzt eine `.sha256`-Datei, ebenso das Windows-MSI-Installationspaket; die Prüfsummen der Linux-Pakete stehen gemeinsam in `router_0.1.0_checksums.txt`. Prüfen Sie die Datei vor dem Ausführen:

```bash
# Linux
sha256sum -c router-v0.1.0-cn-x86_64-unknown-linux-gnu.tar.gz.sha256

# macOS
shasum -a 256 -c router-v0.1.0-aarch64-apple-darwin.tar.gz.sha256
```

```powershell
# Windows: Sollwert lesen, dann die heruntergeladene Datei hashen
Get-Content .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip.sha256
Get-FileHash .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip -Algorithm SHA256
```

Beide Werte müssen übereinstimmen. Wenn die Prüfung fehlschlägt, führen Sie die Datei nicht aus, sondern laden Sie sie erneut herunter. Bei wiederholtem Fehler melden Sie das Problem über das Issue-System der Release-Seite. Bei Installationspaketen ohne Codesignatur belegt SHA-256 die Integrität — dass die Datei nicht beschädigt oder verändert wurde —, ersetzt aber nicht die Authentifizierung des Herausgebers.

## Schnellstart (Archive)

### Linux

```bash
tar -xzf router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz
chmod +x router
./router config init
./router run
```

### macOS

```bash
tar -xzf router-v0.1.0-aarch64-apple-darwin.tar.gz   # Intel: stattdessen x86_64-apple-darwin
chmod +x router
xattr -d com.apple.quarantine ./router 2>/dev/null || true
./router config init
./router lang set de
./router run
```

Meldet macOS, dass der Entwickler nicht verifiziert werden kann, wurde der Build nicht von Apple notarisiert. Prüfen Sie zuerst die SHA-256-Prüfsumme und erlauben Sie den Start dann unter „Systemeinstellungen → Datenschutz & Sicherheit“. 0.1.0 bietet kein macOS-`.pkg`-Installationspaket; verwenden Sie das `.tar.gz`-Archiv.

### Windows

In PowerShell:

```powershell
Expand-Archive .\router-v0.1.0-x86_64-pc-windows-msvc.zip -DestinationPath .\router
cd .\router
.\router.exe config init
.\router.exe lang set de
.\router.exe run
```

### Android / Termux

```bash
tar -xzf router-v0.1.0-aarch64-linux-android.tar.gz
chmod +x router
./router config init
./router run
```

Öffnen Sie anschließend **<http://127.0.0.1:9876>** in einem Browser. Beim ersten Aufruf werden ein Administratorname und ein Passwort angelegt; danach können Sie Anbieter, Anmeldedaten und Domains hinzufügen. Standardmäßig lauscht der Dienst nur auf der Loopback-Schnittstelle und ist nicht aus dem Netzwerk erreichbar.

Für den Hintergrundbetrieb verwenden Sie `./router run --daemon`. Für den dauerhaften Betrieb installieren Sie die Vollversion als Systemdienst (siehe unten); andere Varianten können unter einem vorhandenen Supervisor laufen. In Containern und bei der Fehlersuche startet `--no-guardian` einen einzelnen Prozess.

## Systeminstallationspakete (nur Vollversion)

Jedes Installationspaket enthält eine Standardkonfiguration, die von derselben Binärversion erzeugt wurde; bei einer Aktualisierung bleibt Ihre bestehende Konfiguration erhalten.

| Plattform | Beispieldatei | Installationsbefehl |
| --- | --- | --- |
| Debian / Ubuntu | `router_0.1.0_amd64.deb` | `sudo dpkg -i router_0.1.0_amd64.deb` |
| RHEL / Fedora / openSUSE | `router-0.1.0-1.x86_64.rpm` | `sudo rpm -U router-0.1.0-1.x86_64.rpm` |
| Alpine | `router_0.1.0_x86_64.apk` (statisches musl) | `sudo apk add --allow-untrusted router_0.1.0_x86_64.apk` |
| Arch Linux | `router-0.1.0-1-x86_64.pkg.tar.zst` | `sudo pacman -U router-0.1.0-1-x86_64.pkg.tar.zst` |
| Windows x64 | `router-v0.1.0-x86_64.msi` | `msiexec /i router-v0.1.0-x86_64.msi` als Administrator ausführen |

Installationsorte und Start:

- **Linux (deb-, rpm- und Arch-Pakete auf systemd-Distributionen)**: Die Binärdatei liegt unter `/usr/bin/router`, die Konfiguration unter `/etc/router/router.yaml`; das Paket enthält eine systemd-Unit. Das Paket aktiviert die Unit bei der Installation, startet sie aber nicht; starten Sie sie mit `sudo router service start`. Die Unit läuft unter dem unprivilegierten Konto `router`. Führen Sie `router service install` nicht bei einer paketverwalteten Installation aus: Der Befehl schreibt eine separate Unit nach `/etc/systemd/system/router.service`, die die Unit des Pakets überschreibt.
- **Alpine (`.apk`)**: installiert `/usr/bin/router` und `/etc/router/router.yaml`. Das Paket enthält zwar eine systemd-Unit, Alpine verwendet jedoch kein systemd, daher wird bei der Installation kein Dienst registriert oder aktiviert. 0.1.0 bietet keine OpenRC-Unit; starten Sie `router run --daemon` oder legen Sie einen eigenen OpenRC-Dienst oder Supervisor an.
- **macOS (Archiv)**: Kopieren Sie `router` nach `/usr/local/bin/router` und führen Sie `sudo router service install` aus, um den LaunchDaemon zu installieren und zu starten; die Dienstkonfiguration liegt unter `/Library/Application Support/router/router.yaml`. Zum Entfernen führen Sie `sudo router service uninstall` aus und löschen anschließend `/usr/local/bin/router`. Die Version 0.1.0 bietet kein `.pkg`; verwenden Sie das Archiv aus dem Schnellstart.
- **Windows**: Die Binärdatei liegt unter `C:\Program Files\router\router.exe`, die Konfiguration unter `%ProgramData%\router\router.yaml`. Führen Sie in einer PowerShell mit Administratorrechten `& "C:\Program Files\router\router.exe" service install` aus, um eine geplante Aufgabe für den Systemstart einzurichten. Vor dem Entfernen des Produkts führen Sie `service uninstall` aus.
- **Termux**: Nach der Installation von `termux-services` legt `router service install` einen `runit`-Dienst an und verwaltet ihn.

## Konfiguration und Bedienung

Die Konfiguration ist eine YAML-Datei; Weboberfläche, CLI und manuelle Bearbeitung verwenden dieselbe Datei:

```bash
router config path                 # verwendeten Konfigurationspfad anzeigen
router config init                 # Standardkonfiguration anlegen
router config validate             # Konfiguration prüfen
router config show                 # Konfiguration anzeigen (sensible Felder standardmäßig ausgeblendet)
```

Standardpfade:

- Archive: `$XDG_CONFIG_HOME/router/router.yaml`, sonst `~/.config/router/router.yaml`; unter Windows `%APPDATA%\router\router.yaml`.
- Linux-Paket: `/etc/router/router.yaml`.
- macOS-Dienst (`router service install`): `/Library/Application Support/router/router.yaml`.
- Windows-MSI: `%ProgramData%\router\router.yaml`.

Mit `-c/--config` oder der Umgebungsvariablen `ROUTER_CONFIG` wählen Sie eine andere Datei. Ein minimales Beispiel mit Cloudflare. Dafür ist die Vollversion, die internationale Variante oder die `cloudflare`-Variante erforderlich; in der China-Variante verwenden Sie einen ihrer Anbieter:

```yaml
schema_version: 1
language: "de"
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

`env:NAME` liest Anmeldedaten aus der Umgebung des Prozesses. Bei einem Systemdienst muss die Variable für diesen Dienst sichtbar sein; alternativ tragen Sie die Daten über die Weboberfläche ein. `router provider list --json` zeigt die erforderlichen Felder und den Dokumentationslink jedes Anbieters.

Häufige Befehle:

| Befehl | Zweck |
| --- | --- |
| `router provider list` | In dieser Binärdatei enthaltene Anbieter auflisten |
| `router provider test <id>` | Anmeldedaten und Zonenauflösung prüfen |
| `router update-now` | Sofort einen Synchronisierungszyklus ausführen |
| `router config validate` | Konfiguration prüfen |
| `router healthcheck --ready` | Zustand prüfen; schlägt fehl, wenn im letzten Zyklus alle Einträge fehlschlugen |
| `router password reset` | Passwort für die Weboberfläche zurücksetzen |
| `router docs de` | Eingebettete Hilfe lesen |
| `router version --json` | Version, Zielplattform und Build-Informationen anzeigen |

## Sprachen

Unterstützt werden `zh`, `en`, `ru`, `de` und `ja`. Eine Sprache gilt für CLI, Protokolle, Weboberfläche, REST-API-Fehler und die eingebettete Hilfe:

```bash
router lang list
router lang show
router lang set de       # dauerhaft; gilt ab dem nächsten Start
router --lang de provider list   # nur für diesen Aufruf
ROUTER_LANG=ja router provider list
```

Eine in der Weboberfläche gewählte Sprache gilt sofort für den laufenden Dienst. Eine neue Konfiguration verwendet standardmäßig vereinfachtes Chinesisch; damit die Instanz von Anfang an eine andere Sprache verwendet, führen Sie vor dem ersten Start `router lang set de` (oder `en` / `ru` / `ja`) aus.

## Sicherheit

- Laden Sie Dateien nur von der offiziellen Release-Seite herunter und prüfen Sie vor dem Ausführen die SHA-256-Prüfsumme.
- Der Standard-Listener ist `127.0.0.1:9876`. Wenn Sie die Weboberfläche öffentlich bereitstellen, setzen Sie ein starkes Passwort und terminieren Sie HTTPS an einem Reverse-Proxy. Hinter einem Proxy muss der externe Hostname in `web.allowed_hosts` stehen; andernfalls werden schreibende Anfragen abgelehnt, weil der `Host`-Header nicht passt.
- Die Konfiguration enthält API-Anmeldedaten und einen Passwort-Hash. Beschränken Sie den Zugriff auf den Eigentümer und legen Sie die Datei niemals in einem Repository oder einer öffentlichen Sicherung ab.
- Verwenden Sie DNS-API-Token mit minimalen Rechten, nur für die Zonen und Einträge, die router ändern soll.
- Bevorzugen Sie für den Dauerbetrieb einen Systemdienst, damit der Dienstmanager ihn beim Start ausführt und überwacht. `--no-guardian` ist für Container und die Fehlersuche gedacht.

## Kostenpflichtige Lizenz und Aktivierung

**Jede in diesem Repository veröffentlichte Binärdatei ist ein kostenpflichtiger kommerzieller Build. Es gibt keine kostenlose, Community- oder Testversion.** Vollversion, `cn`, `global` und Anbieter-Builds benötigen gleichermaßen eine erworbene Lizenz und eine gerätegebundene Aktivierung, bevor sie DNS-Einträge aktualisieren oder die Weboberfläche bereitstellen.

Eine kommerzielle Lizenz kostet **einmalig 19,90 US-Dollar für die lebenslange Nutzung auf einem Gerät**. Jeder Aktivierungscode ist an genau ein Gerät gebunden; es gibt kein Abonnement, keine Verlängerung, keine kostenlose Version und keine Mehrgerätelizenz. Ein zweites Gerät benötigt einen zweiten Aktivierungscode.

```bash
router license status
router license device-code
router license activate <AKTIVIERUNGSCODE>
```

- `router license device-code` gibt einen Gerätecode aus. Übermitteln Sie ihn an das Lizenzsystem und schließen Sie den Kauf ab, um einen signierten Aktivierungscode zu erhalten.
- `router license activate <AKTIVIERUNGSCODE>` aktiviert das Zielgerät offline; der Client benötigt keinen laufenden Online-Heartbeat.
- Fehlt die Lizenz, wurde sie manipuliert oder ist sie an ein anderes Gerät gebunden, verweigern `router run`, `router update-now` und die Weboberfläche den Dienst und beenden sich mit Status 3. Die erste Aktivierung muss über die CLI erfolgen.
- Lizenzen sind an einen Gerätefingerabdruck gebunden; in Containern sollte `ROUTER_INSTANCE_ID` ausdrücklich gesetzt werden.
- `license.mode` in der Konfiguration kann die Lizenzprüfung veröffentlichter Builds nicht deaktivieren oder herabstufen; sie verlangen immer eine gerätegebundene Aktivierung.

Kauf, Bereitstellung und Support: **<https://router.roboter.site>** oder **roboter.site@aliyun.com**.

## Fehlerbehebung

| Symptom | Vorgehen |
| --- | --- |
| `command not found` oder Programm nicht auffindbar | Entpacken Sie das Archiv und starten Sie `./router` aus seinem Verzeichnis (Windows: `.\router.exe`), oder nehmen Sie das Verzeichnis in `PATH` auf. |
| `Permission denied` | Führen Sie `chmod +x router` aus. Auf macOS siehe den Quarantäne-Hinweis im Schnellstart, falls der Entwickler nicht verifiziert werden kann. |
| `cannot execute binary file` oder Formatfehler | Sie haben ein Archiv für eine andere Plattform oder Architektur geladen; prüfen Sie `uname -m` und laden Sie das passende Archiv herunter. |
| Anbieter wird als nicht enthalten gemeldet | Er gehört nicht zur aktuellen Variante; wechseln Sie zur Vollversion, China-, internationalen oder Anbieter-Variante. |
| Weboberfläche nicht erreichbar | Prüfen Sie `web.listen` in der Konfigurationsdatei (Pfad über `router config path`), die lokale Firewall und den Prozesszustand (`router healthcheck`). |
| Konfiguration wird abgelehnt | Führen Sie `router config validate` aus, um das fehlerhafte Feld zu sehen; sichern Sie die Datei vor der Korrektur und korrigieren Sie sie in der Weboberfläche. |
| Sprache wurde nicht übernommen | `router lang show` nennt die aktive Quelle. Eine Befehlszeilen-Option gilt nur für diesen Aufruf; eine Änderung in der Weboberfläche gilt sofort für den laufenden Dienst. |
| Veröffentlichter Build beendet sich sofort mit Status 3 | Er ist nicht aktiviert oder die Lizenz ist ungültig; führen Sie `router license status` und `router license activate <AKTIVIERUNGSCODE>` aus. |
| Linux meldet eine zu alte glibc | Die Linux-Archive in 0.1.0 erfordern glibc 2.17 oder neuer. Aktualisieren Sie das System oder installieren Sie unter Alpine das `.apk` (in dieser Version für x86_64), das eine statisch mit musl gelinkte Binärdatei enthält. Ein statisches musl-Archiv wird nicht separat angeboten. |

## Hinweis zur kommerziellen Lizenz

router und seine veröffentlichten Binärdateien sind **proprietäre kommerzielle Software von AutoDRL**. Sie stehen nicht unter MIT, Apache-2.0 oder einer anderen Open-Source-Lizenz, es wird keine kostenlose Version angeboten, und alle Rechte sind vorbehalten.

Sie dürfen die Software nicht kopieren, verändern, verbreiten, weiterverkaufen, vermieten, unterlizenzieren oder zurückentwickeln, außer im Rahmen einer von AutoDRL unterzeichneten schriftlichen kommerziellen Vereinbarung. Herunterladen, Besitz oder Installation einer Binärdatei begründet kein Recht, sie auszuführen; nur eine gültig aktivierte Installation darf den Dienst bereitstellen.

Die vollständige Erklärung steht in [LICENSE](LICENSE). Die Dokumentation in diesem Repository beschreibt das Produkt und gewährt keine Lizenz an einer Binärdatei. Kauf und Aktivierung: **<https://router.roboter.site>**.

Dieses Repository verteilt ausschließlich Binärdateien und enthält keinen Quellcode. Dieses Dokument beschreibt Version 0.1.0.

[简体中文](README.md) | [English](README.en.md) | [Русский](README.ru.md) | [**Deutsch**](README.de.md) | [日本語](README.ja.md)
