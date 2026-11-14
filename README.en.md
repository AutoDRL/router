# router

**Commercial-grade DDNS (dynamic DNS) client · Version 0.1.0**

[简体中文](README.md) | [**English**](README.en.md) | [Русский](README.ru.md) | [Deutsch](README.de.md) | [日本語](README.ja.md)

> **This repository distributes compiled client binaries only. The source code is not published here.**
> Project website: <https://router.roboter.site> · Downloads: **[GitHub Releases](https://github.com/AutoDRL/router/releases/latest)**

router watches the machine's public IPv4 and IPv6 addresses and keeps the A and AAAA records at your DNS provider in sync with them. It runs as a single native executable with no runtime, package manager or external database; you can manage it through a local web interface or install it as a system service.

> **Licence requirement: no free edition is published in this repository.** Every binary requires a purchased licence and device-bound activation; an unactivated installation cannot provide DNS updates or the web interface and exits with status 3. Purchase and activation: <https://router.roboter.site>; the activation service address is shown by `router license status`.

## Project statistics

The badges below are updated in real time by [Shields.io](https://shields.io) from GitHub's public metrics. Download totals cover assets published in Releases; per-asset counts are shown on each Release page. The badges begin showing actual data once the repository is public and the first Release is published.

| Metric | Live value |
| --- | --- |
| Total Release downloads | [![Release downloads](https://img.shields.io/github/downloads/AutoDRL/router/total?style=flat-square&label=downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases) |
| Latest Release downloads | [![Latest release downloads](https://img.shields.io/github/downloads/AutoDRL/router/latest/total?style=flat-square&label=latest%20downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |
| Forks | [![Forks](https://img.shields.io/github/forks/AutoDRL/router?style=flat-square&label=forks&logo=github&logoColor=white)](https://github.com/AutoDRL/router/forks) |
| Stars | [![Stars](https://img.shields.io/github/stars/AutoDRL/router?style=flat-square&label=stars&logo=github&logoColor=white)](https://github.com/AutoDRL/router/stargazers) |
| Watchers | [![Watchers](https://img.shields.io/github/watchers/AutoDRL/router?style=flat-square&label=watchers&logo=github&logoColor=white)](https://github.com/AutoDRL/router/watchers) |
| Latest release | [![Latest release](https://img.shields.io/github/v/release/AutoDRL/router?style=flat-square&label=release&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |

## Highlights

- **31 DNS providers**: Alibaba Cloud, Tencent Cloud, Huawei Cloud, Volcano Engine, Baidu Cloud, AWS Route 53, Cloudflare, GoDaddy, Namecheap, DigitalOcean, Porkbun, NameSilo, dynv6, Vercel, deSEC, ClouDNS, Gcore, NS1, Name.com, Spaceship, Dynadot, DNSPod, Now.cn, Eranet, Tnet.hk and more.
- **Independent IPv4 and IPv6 updates**: A and AAAA records are enabled and configured separately.
- **Five address sources**: HTTP endpoint with failover, local interface, external command, DNS probe and a static value.
- **Address policy and retries**: private, loopback, link-local, CGNAT and documentation ranges are rejected; remote lookups are skipped while the address is unchanged; rate limits trigger exponential backoff and honour `Retry-After`.
- **Built-in web interface**: first-run administrator setup, Argon2id password hashing, CSRF protection, login rate limiting, live logs, Prometheus metrics and health endpoints.
- **Five languages**: Simplified Chinese, English, Russian, German and Japanese, covering the CLI, logs, web interface, REST API errors and embedded help.
- **Guardian supervision**: `router run` starts a guardian that supervises the worker and restarts it after an abnormal exit, for unattended 24/7 operation.
- **Verifiable delivery**: every archive has a SHA-256 checksum, and the downloaded binary runs without installing dependencies.

## Choose a build

Version 0.1.0 ships **34 builds**: 31 single-provider builds, a pure-China build (`cn`), a global build (`global`) and the full build with every provider. Each build is available for all 10 targets listed below.

| Build | Archive marker | Providers | Recommended for |
| --- | --- | --- | --- |
| Full | none | 31 | Mixed providers, an uncertain choice, or when you want a Linux / Windows system installer |
| China | `cn` | 13 | Only mainland-China DNS services; no overseas provider is compiled in |
| Global | `global` | 20 | Only DNS services outside mainland China; no mainland-China provider is compiled in |
| Single-provider | provider ID, for example `cloudflare` | 1 | One provider only, with the smallest binary and dependency surface |

Both `cn` and `global` include the two neutral components (HiPM DNS Manager and the generic callback). Their provider counts therefore overlap, but together they still cover all 31 providers exactly once. If your configuration names a provider that is not compiled into the binary you are running, switch to a build that includes it; `router provider list` shows exactly what the current binary contains.

<details>
<summary><strong>China build (cn): 11 mainland-China providers + 2 neutral components</strong></summary>

Alibaba Cloud DNS (`aliyun`), Alibaba Cloud ESA (`aliesa`), Tencent Cloud DNSPod (`tencent`), Tencent Cloud EdgeOne (`edgeone`), Volcano Engine DNS (`trafficroute`), Huawei Cloud DNS (`huawei`), Baidu Cloud DNS (`baidu`), DNSPod (`dnspod`), DNSLA (`dnsla`), RainYun (`rainyun`), Now.cn (`nowcn`), HiPM DNS Manager (`hipmdnsmgr`), generic callback (`callback`).

</details>

<details>
<summary><strong>Global build (global): 18 overseas providers + 2 neutral components</strong></summary>

AWS Route 53 (`route53`), Cloudflare (`cloudflare`), GoDaddy (`godaddy`), Namecheap (`namecheap`), DigitalOcean (`digitalocean`), Porkbun (`porkbun`), NameSilo (`namesilo`), dynv6 (`dynv6`), Vercel (`vercel`), deSEC (`desec`), ClouDNS (`cloudns`), Gcore (`gcore`), NS1 (`nsone`), Name.com (`namecom`), Spaceship (`spaceship`), Dynadot (`dynadot`), Eranet (`eranet`), Tnet.hk (`tnethk`), HiPM DNS Manager (`hipmdnsmgr`), generic callback (`callback`).

</details>

Archive naming (`<target>` values are listed in the next section):

| Build | Linux / macOS / Android | Windows |
| --- | --- | --- |
| Full | `router-v0.1.0-<target>.tar.gz` | `router-v0.1.0-<target>.zip` |
| China | `router-v0.1.0-cn-<target>.tar.gz` | `router-v0.1.0-cn-<target>.zip` |
| Global | `router-v0.1.0-global-<target>.tar.gz` | `router-v0.1.0-global-<target>.zip` |
| Single-provider | `router-v0.1.0-<provider>-<target>.tar.gz` | `router-v0.1.0-<provider>-<target>.zip` |

The full build uses the archive name without a variant marker, for example `router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz`. System installers (`.deb`, `.rpm`, `.apk`, `.pkg.tar.zst`, `.msi`) are published for the full build only; `cn`, `global` and single-provider builds are archives only.

## Supported platforms

| Operating system | Architecture | `<target>` in the archive name | Notes |
| --- | --- | --- | --- |
| Linux | x86_64 | `x86_64-unknown-linux-gnu` | glibc |
| Linux | aarch64 | `aarch64-unknown-linux-gnu` | glibc |
| Linux | armv7 | `armv7-unknown-linux-gnueabihf` | hard float |
| Linux | i686 | `i686-unknown-linux-gnu` | glibc |
| macOS | Intel | `x86_64-apple-darwin` | `.tar.gz`; macOS 10.12 or newer |
| macOS | Apple silicon | `aarch64-apple-darwin` | `.tar.gz`; macOS 11.0 or newer |
| Windows | x86_64 | `x86_64-pc-windows-msvc` | `.zip` |
| Android / Termux | aarch64 | `aarch64-linux-android` | Bionic libc |
| Android / Termux | armv7 | `armv7-linux-androideabi` | Bionic libc |
| Android / Termux | x86_64 | `x86_64-linux-android` | Bionic libc |

Notes:

- Linux `.tar.gz` archives for 0.1.0 link against glibc 2.17 and require glibc 2.17 or newer; no static musl archive is provided. Alpine users should install the full build's `.apk` (published for x86_64 only); the binary inside it is a statically linked musl build and does not require glibc.
- Android builds link against Bionic libc and require Android 7.0 (API 24) or newer. They target Termux or an Android shell and do not run on a regular Linux distribution.
- There is no 32-bit Windows, Windows on ARM or 32-bit macOS build. Match the archive to `uname -m` (on Windows, `$env:PROCESSOR_ARCHITECTURE`).
- Each archive contains a single executable: `router` (`router.exe` on Windows).

## Verify the download

Every archive has a `.sha256` file, and so does the Windows MSI installer; the checksums of the Linux packages are listed together in `router_0.1.0_checksums.txt`. Verify the file before running it:

```bash
# Linux
sha256sum -c router-v0.1.0-cn-x86_64-unknown-linux-gnu.tar.gz.sha256

# macOS
shasum -a 256 -c router-v0.1.0-aarch64-apple-darwin.tar.gz.sha256
```

```powershell
# Windows: read the expected value, then hash the downloaded file
Get-Content .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip.sha256
Get-FileHash .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip -Algorithm SHA256
```

The two values must match. If the check fails, do not run the file; download it again. If it still fails, report it through the release page's issue tracker. For installers without a code signature, SHA-256 proves integrity — that the file was not corrupted or altered — but it is not a substitute for publisher authentication.

## Quick start (archives)

### Linux

```bash
tar -xzf router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz
chmod +x router
./router config init
./router run
```

### macOS

```bash
tar -xzf router-v0.1.0-aarch64-apple-darwin.tar.gz   # Intel: use x86_64-apple-darwin instead
chmod +x router
xattr -d com.apple.quarantine ./router 2>/dev/null || true
./router config init
./router lang set en
./router run
```

If macOS says it cannot verify the developer, the build is not notarized by Apple. Confirm the SHA-256 first, then allow it under “System Settings → Privacy & Security”. 0.1.0 does not ship a macOS `.pkg` installer; use the `.tar.gz` archive.

### Windows

In PowerShell:

```powershell
Expand-Archive .\router-v0.1.0-x86_64-pc-windows-msvc.zip -DestinationPath .\router
cd .\router
.\router.exe config init
.\router.exe lang set en
.\router.exe run
```

### Android / Termux

```bash
tar -xzf router-v0.1.0-aarch64-linux-android.tar.gz
chmod +x router
./router config init
./router run
```

Then open **<http://127.0.0.1:9876>** in a browser. The first visit asks you to create an administrator username and password; after that you can add providers, credentials and domains. The default listener is loopback-only and is not exposed to the network.

Use `./router run --daemon` to run in the background. For long-running deployments, install the full build as a system service (see below); other builds can run under your own supervisor. Containers and debugging sessions can use `--no-guardian` to start a single process.

## System installers (full build only)

Each installer ships a default configuration generated by the same binary version, and upgrades preserve your existing configuration.

| Platform | Example file | Install command |
| --- | --- | --- |
| Debian / Ubuntu | `router_0.1.0_amd64.deb` | `sudo dpkg -i router_0.1.0_amd64.deb` |
| RHEL / Fedora / openSUSE | `router-0.1.0-1.x86_64.rpm` | `sudo rpm -U router-0.1.0-1.x86_64.rpm` |
| Alpine | `router_0.1.0_x86_64.apk` (static musl) | `sudo apk add --allow-untrusted router_0.1.0_x86_64.apk` |
| Arch Linux | `router-0.1.0-1-x86_64.pkg.tar.zst` | `sudo pacman -U router-0.1.0-1-x86_64.pkg.tar.zst` |
| Windows x64 | `router-v0.1.0-x86_64.msi` | Run `msiexec /i router-v0.1.0-x86_64.msi` as administrator |

Install locations and how to start the service:

- **Linux (deb, rpm and Arch packages, on systemd distributions)**: the binary is `/usr/bin/router`, the configuration is `/etc/router/router.yaml`, and the package includes a systemd unit. The package enables the unit during installation but does not start it; start it with `sudo router service start`. The unit runs under the unprivileged `router` account. Do not run `router service install` on a package-managed installation: it writes a separate unit under `/etc/systemd/system/router.service`, which overrides the packaged unit.
- **Alpine (`.apk`)**: installs `/usr/bin/router` and `/etc/router/router.yaml`. The package also places a systemd unit, but Alpine does not use systemd, so installation neither registers nor enables a service. 0.1.0 provides no OpenRC unit; run `router run --daemon` or add your own OpenRC service or supervisor.
- **macOS (archive)**: copy `router` to `/usr/local/bin/router` and run `sudo router service install` to install and start the LaunchDaemon; the service configuration is `/Library/Application Support/router/router.yaml`. Uninstall with `sudo router service uninstall`, then remove `/usr/local/bin/router`. 0.1.0 does not provide a `.pkg`; use the archive from Quick start.
- **Windows**: the binary is `C:\Program Files\router\router.exe`, the configuration is `%ProgramData%\router\router.yaml`. From an elevated PowerShell, run `& "C:\Program Files\router\router.exe" service install` to register a boot task. Run `service uninstall` before removing the product.
- **Termux**: after installing `termux-services`, `router service install` writes and manages a `runit` service.

## Configuration and use

The configuration is a YAML file; the web interface, the CLI and manual edits all use the same file:

```bash
router config path                 # print the configuration path in use
router config init                 # create the default configuration
router config validate             # validate it
router config show                 # print it (sensitive fields redacted by default)
```

Default paths:

- Archives: `$XDG_CONFIG_HOME/router/router.yaml`, falling back to `~/.config/router/router.yaml`; on Windows, `%APPDATA%\router\router.yaml`.
- Linux package: `/etc/router/router.yaml`.
- macOS service (`router service install`): `/Library/Application Support/router/router.yaml`.
- Windows MSI: `%ProgramData%\router\router.yaml`.

Use `-c/--config` or the `ROUTER_CONFIG` environment variable to choose another file. A minimal example (Cloudflare, so it requires the full build, the global build or the `cloudflare` build; with the China build, use a provider it contains):

```yaml
schema_version: 1
language: "en"
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

`env:NAME` reads a credential from the process environment. For a system service, make sure the variable is visible to that service, or enter the credential through the web interface. `router provider list --json` shows the required fields and documentation link for each provider.

Common commands:

| Command | Purpose |
| --- | --- |
| `router provider list` | List the providers compiled into this binary |
| `router provider test <id>` | Test credentials and zone resolution |
| `router update-now` | Run one synchronisation cycle now |
| `router config validate` | Validate the configuration |
| `router healthcheck --ready` | Check health; fails when every record failed in the last cycle |
| `router password reset` | Reset the web login password |
| `router docs en` | Read the embedded help document |
| `router version --json` | Show version, target and build information |

## Languages

The supported codes are `zh`, `en`, `ru`, `de` and `ja`. One language covers the CLI, logs, web interface, REST API errors and embedded help:

```bash
router lang list
router lang show
router lang set en       # persist for the next start
router --lang en provider list   # this invocation only
ROUTER_LANG=ja router provider list
```

A language selected in the web interface applies to the running service immediately. A new configuration defaults to Simplified Chinese; to make an instance use another language from the start, run `router lang set en` (or `ru` / `de` / `ja`) before the first start.

## Security

- Download only from the official release page and verify the SHA-256 before running.
- The default listener is `127.0.0.1:9876`. If you expose the web interface, set a strong password and terminate HTTPS in a reverse proxy. Behind a proxy, add the external hostname to `web.allowed_hosts`; otherwise state-changing requests are rejected when the `Host` header does not match.
- The configuration contains API credentials and a password hash. Restrict it to the owner and never commit it or place it in a public backup.
- Use least-privilege DNS API tokens scoped to the zones and records router must change.
- Prefer a system service for long-running deployments so the service manager starts it at boot and supervises it. Use `--no-guardian` only in containers or for debugging.

## Paid licensing and activation

**Every binary published in this repository is a paid commercial build. There is no free, community or unlicensed trial edition.** Full, `cn`, `global` and single-provider builds alike require a purchased licence and device-bound activation before they serve DNS updates or the web interface.

```bash
router license status
router license device-code
router license activate <ACTIVATION-CODE>
```

- `router license device-code` prints a device code. Submit it to the licensing system and complete payment to receive a signed activation code.
- `router license activate <ACTIVATION-CODE>` activates the target device offline; the client does not depend on a continuous online heartbeat.
- When the licence is missing, expired, tampered with or bound to another device, `router run`, `router update-now` and the web interface refuse service and exit with status 3. The first activation must be completed through the CLI.
- Licences are bound to a device fingerprint; container deployments should set `ROUTER_INSTANCE_ID` explicitly.
- `license.mode` in the configuration cannot disable or downgrade licence enforcement in published builds; they always require device-bound activation.

Purchase, renewal, deployment and support enquiries: **<https://router.roboter.site>**.

## Troubleshooting

| Symptom | Action |
| --- | --- |
| `command not found`, or the program cannot be located | Unpack the archive and run `./router` from its directory (Windows: `.\router.exe`), or add the directory to `PATH`. |
| `Permission denied` | Run `chmod +x router`. On macOS, see the quarantine note under Quick start if the developer cannot be verified. |
| `cannot execute binary file` or a format error | You downloaded an archive for another platform or architecture; check `uname -m` and download the matching target. |
| The provider is reported as not built in | It is not part of the current build; switch to the full, China, global or that provider's single-provider build. |
| The web interface is unreachable | Check `web.listen` in the file printed by `router config path`, the local firewall, and whether the process is up (`router healthcheck`). |
| The configuration is rejected | Run `router config validate` to see the failing field; back up the file first, then correct it in the web interface. |
| The language did not change | `router lang show` reports the active source. A command-line override wins for that invocation; a change in the web interface applies to the running service immediately. |
| A published build exits immediately with status 3 | It is not activated, or the licence is invalid; run `router license status` and `router license activate <ACTIVATION-CODE>`. |
| Linux reports that glibc is too old | The 0.1.0 Linux archives require glibc 2.17 or newer. Upgrade the system, or on Alpine install the `.apk` package (x86_64 in this release), which contains a statically linked musl build. Static musl is not shipped as a separate archive. |

## Commercial licence notice

router and its published binaries are **proprietary commercial software of AutoDRL**. They are not released under MIT, Apache-2.0 or any other open-source licence, no free edition is offered, and all rights are reserved.

You may not copy, modify, distribute, resell, rent, sublicense or reverse-engineer the software except under a written commercial agreement signed by AutoDRL. Downloading, possessing or installing a binary does not grant a right to run it; only a validly activated installation may provide service.

See [LICENSE](LICENSE) for the full notice. The documentation in this repository describes the product and grants no licence to any binary. Purchase and activation: **<https://router.roboter.site>**.

This repository distributes binaries only and does not contain source code. This document describes version 0.1.0.

[简体中文](README.md) | [**English**](README.en.md) | [Русский](README.ru.md) | [Deutsch](README.de.md) | [日本語](README.ja.md)
