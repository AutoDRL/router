# router

**商业级 DDNS（动态 DNS）客户端 · 版本 0.1.0**

[**简体中文**](README.md) | [English](README.en.md) | [Русский](README.ru.md) | [Deutsch](README.de.md) | [日本語](README.ja.md)

> **本仓库只发布编译好的客户端二进制文件，不发布源代码。**
> 项目网站：<https://router.roboter.site> · 下载地址：**[GitHub Releases](https://github.com/AutoDRL/router/releases/latest)**

router 持续检测本机的公网 IPv4 / IPv6 地址，并把变化同步到 DNS 服务商处的 A / AAAA 记录。单个原生可执行文件即可运行，不依赖运行时、包管理器或外部数据库；既可以在本机用 Web 界面管理，也可以作为系统服务长期运行。

> **授权要求：本仓库不提供免费版。** 所有二进制必须先购买授权并完成设备绑定激活；未激活的安装无法提供 DNS 更新或 Web 管理服务，并会以状态码 3 退出。购买与激活：<https://router.roboter.site>；激活服务地址可通过 `router license status --json` 查看。

## 项目统计

以下徽章由 [Shields.io](https://shields.io) 根据 GitHub 的公开数据实时更新。下载量统计所有已发布 Release 的资产；每个安装包的具体下载次数可在对应的 Release 页面查看。

| 指标 | 实时数据 |
| --- | --- |
| Release 下载总量 | [![Release downloads](https://img.shields.io/github/downloads/AutoDRL/router/total?style=flat-square&label=downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases) |
| 最新 Release 下载量 | [![Latest release downloads](https://img.shields.io/github/downloads/AutoDRL/router/latest/total?style=flat-square&label=latest%20downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |
| Fork 数量 | [![Forks](https://img.shields.io/github/forks/AutoDRL/router?style=flat-square&label=forks&logo=github&logoColor=white)](https://github.com/AutoDRL/router/forks) |
| Star 数量 | [![Stars](https://img.shields.io/github/stars/AutoDRL/router?style=flat-square&label=stars&logo=github&logoColor=white)](https://github.com/AutoDRL/router/stargazers) |
| Watcher 数量 | [![Watchers](https://img.shields.io/github/watchers/AutoDRL/router?style=flat-square&label=watchers&logo=github&logoColor=white)](https://github.com/AutoDRL/router/watchers) |
| 最新版本 | [![Latest release](https://img.shields.io/github/v/release/AutoDRL/router?style=flat-square&label=release&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |

## 主要特性

- **31 家 DNS 服务商**：阿里云、腾讯云、华为云、火山引擎、百度云、AWS Route 53、Cloudflare、GoDaddy、Namecheap、DigitalOcean、Porkbun、NameSilo、dynv6、Vercel、deSEC、ClouDNS、Gcore、NS1、Name.com、Spaceship、Dynadot、DNSPod、Now.cn、Eranet、Tnet.hk 等。
- **IPv4 / IPv6 独立更新**：A 与 AAAA 记录分别启用、分别配置域名。
- **多种公网地址来源**：HTTP 接口（支持故障转移）、本机网卡、外部命令、DNS 探测、固定值。
- **地址策略与重试**：拒绝私网、回环、链路本地、CGNAT 与文档网段；IP 未变化时跳过远端查询；触发限流时指数退避并尊重 `Retry-After`。
- **本机 Web 管理界面**：首次访问创建管理员账号，Argon2id 密码、CSRF 防护、登录限流、实时日志、Prometheus 指标、健康检查端点。
- **五种界面语言**：简体中文、English、Русский、Deutsch、日本語；CLI、日志、Web 界面、REST API 错误和内嵌帮助使用同一语言。
- **守护进程监督**：`router run` 默认由守护进程监督工作进程，工作进程异常退出后自动拉起；适合 7×24 长期运行。
- **可验证交付**：每个归档都有 SHA-256 校验和；二进制无需安装依赖即可解压运行。

## 选择版本

0.1.0 提供 **34 个构建**：31 个单服务商版本、一个纯国内版（`cn`）、一个纯海外版（`global`），以及包含全部服务商的完整版。每个构建都覆盖下列 10 个目标平台。

| 构建 | 归档中的标识 | 服务商数 | 推荐场景 |
| --- | --- | --- | --- |
| 完整版 | 无额外标识 | 31 | 不确定选哪个、同时管理多家服务商，或需要 x86_64 Linux / Windows 系统安装包 |
| 国内版 | `cn` | 13 | 只使用中国大陆的 DNS 服务；不编译任何海外服务商 |
| 海外版 | `global` | 20 | 只使用中国大陆以外的 DNS 服务；不编译任何中国大陆服务商 |
| 单服务商版 | 服务商 ID，例如 `cloudflare` | 1 | 只使用一家服务商，希望获得最小的二进制和最小的依赖面 |

`cn` 与 `global` 都包含两个中立组件（HiPM DNS Manager 和通用回调），因此两边数量之和大于 31，但去重后仍是全部 31 家服务商。配置中引用了未编译进当前二进制的服务商时，请换用相应版本；`router provider list` 只显示当前二进制真正带有的服务商。

<details>
<summary><strong>国内版（cn）：11 家中国大陆服务商 + 2 个中立组件</strong></summary>

Alibaba Cloud DNS（`aliyun`）、Alibaba Cloud ESA（`aliesa`）、Tencent Cloud DNSPod（`tencent`）、Tencent Cloud EdgeOne（`edgeone`）、Volcano Engine DNS（`trafficroute`）、Huawei Cloud DNS（`huawei`）、Baidu Cloud DNS（`baidu`）、DNSPod（`dnspod`）、DNSLA（`dnsla`）、RainYun（`rainyun`）、Now.cn（`nowcn`）、HiPM DNS Manager（`hipmdnsmgr`）、通用回调（`callback`）。

</details>

<details>
<summary><strong>海外版（global）：18 家海外服务商 + 2 个中立组件</strong></summary>

AWS Route 53（`route53`）、Cloudflare（`cloudflare`）、GoDaddy（`godaddy`）、Namecheap（`namecheap`）、DigitalOcean（`digitalocean`）、Porkbun（`porkbun`）、NameSilo（`namesilo`）、dynv6（`dynv6`）、Vercel（`vercel`）、deSEC（`desec`）、ClouDNS（`cloudns`）、Gcore（`gcore`）、NS1（`nsone`）、Name.com（`namecom`）、Spaceship（`spaceship`）、Dynadot（`dynadot`）、Eranet（`eranet`）、Tnet.hk（`tnethk`）、HiPM DNS Manager（`hipmdnsmgr`）、通用回调（`callback`）。

</details>

归档命名规则（`<target>` 的取值见下一节）：

| 构建 | Linux / macOS / Android | Windows |
| --- | --- | --- |
| 完整版 | `router-v0.1.0-<target>.tar.gz` | `router-v0.1.0-<target>.zip` |
| 国内版 | `router-v0.1.0-cn-<target>.tar.gz` | `router-v0.1.0-cn-<target>.zip` |
| 海外版 | `router-v0.1.0-global-<target>.tar.gz` | `router-v0.1.0-global-<target>.zip` |
| 单服务商版 | `router-v0.1.0-<provider>-<target>.tar.gz` | `router-v0.1.0-<provider>-<target>.zip` |

完整版使用不带变体标识的归档名，例如 `router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz`。系统安装包（`.deb`、`.rpm`、`.apk`、`.pkg.tar.zst`、`.msi`）只随完整版发布；`cn`、`global` 和单服务商版只提供压缩包。

## 支持平台

| 操作系统 | 架构 | 归档中的 `<target>` | 说明 |
| --- | --- | --- | --- |
| Linux | x86_64 | `x86_64-unknown-linux-gnu` | glibc |
| Linux | aarch64 | `aarch64-unknown-linux-gnu` | glibc |
| Linux | armv7 | `armv7-unknown-linux-gnueabihf` | 硬浮点 |
| Linux | i686 | `i686-unknown-linux-gnu` | glibc |
| macOS | Intel | `x86_64-apple-darwin` | `.tar.gz`；macOS 10.12 及以上 |
| macOS | Apple Silicon | `aarch64-apple-darwin` | `.tar.gz`；macOS 11.0 及以上 |
| Windows | x86_64 | `x86_64-pc-windows-msvc` | `.zip` |
| Android / Termux | aarch64 | `aarch64-linux-android` | Bionic libc |
| Android / Termux | armv7 | `armv7-linux-androideabi` | Bionic libc |
| Android / Termux | x86_64 | `x86_64-linux-android` | Bionic libc |

说明：

- 0.1.0 的 Linux `.tar.gz` 压缩包链接 glibc 2.17，要求系统不旧于 glibc 2.17；不提供静态 musl 压缩包。Alpine 用户请安装完整版的 `.apk`（当前发布仅含 x86_64）；包内的二进制是静态链接的 musl 构建，不依赖 glibc。
- Android 构建链接 Bionic libc，最低支持 Android 7.0（API 24），面向 Termux 或 Android 自带 shell，不能在普通 Linux 发行版上运行。
- 当前不提供 32 位 Windows、Windows on ARM 和 32 位 macOS 构建。请选择与 `uname -m`（Windows 为 `$env:PROCESSOR_ARCHITECTURE`）一致的归档。
- 每个归档只包含 `router`（Windows 为 `router.exe`）一个可执行文件。

## 校验下载

每个归档都附带 `.sha256` 文件；Windows MSI 安装包也附带一个，Linux 安装包的各校验值统一列在 `router_0.1.0_checksums.txt` 中。下载后先校验，再运行：

```bash
# Linux
sha256sum -c router-v0.1.0-cn-x86_64-unknown-linux-gnu.tar.gz.sha256

# macOS
shasum -a 256 -c router-v0.1.0-aarch64-apple-darwin.tar.gz.sha256
```

```powershell
# Windows：读取期望值，再计算下载文件的哈希
Get-Content .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip.sha256
Get-FileHash .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip -Algorithm SHA256
```

两个值必须一致。校验失败时不要运行该文件，请重新下载；如果仍然失败，请通过发布页的 issue 反馈。对于未使用代码签名的安装包，SHA-256 只能证明文件完整、未被损坏或篡改，不能替代发布者身份认证。

## 快速开始（压缩包）

### Linux

```bash
tar -xzf router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz
chmod +x router
./router config init
./router run
```

### macOS

```bash
tar -xzf router-v0.1.0-aarch64-apple-darwin.tar.gz   # Intel 机型改用 x86_64-apple-darwin
chmod +x router
xattr -d com.apple.quarantine ./router 2>/dev/null || true
./router config init
./router lang set zh
./router run
```

macOS 如果提示无法验证开发者，是因为该构建没有 Apple 公证；请先用上一步的 SHA-256 校验确认文件，再在“系统设置 → 隐私与安全性”中允许运行。0.1.0 不提供 macOS `.pkg` 安装包，请使用 `.tar.gz` 压缩包。

### Windows

在 PowerShell 中：

```powershell
Expand-Archive .\router-v0.1.0-x86_64-pc-windows-msvc.zip -DestinationPath .\router
cd .\router
.\router.exe config init
.\router.exe lang set zh
.\router.exe run
```

### Android / Termux

```bash
tar -xzf router-v0.1.0-aarch64-linux-android.tar.gz
chmod +x router
./router config init
./router run
```

然后在浏览器打开 **<http://127.0.0.1:9876>**。首次访问会要求创建管理员用户名和密码；保存后即可添加服务商、凭据和域名。默认只监听本机回环地址，不会直接暴露到公网。

需要后台运行时可用 `./router run --daemon`；长期运行建议将完整版安装为系统服务（见下文），其他版本可交给已有的进程监管工具运行。容器和调试场景可用 `--no-guardian` 只启动一个进程。

## 系统安装包（仅完整版）

以下安装包仅提供 x86_64 版本；其他架构请使用对应压缩包。安装包内置一份由同一版本二进制生成的默认配置，升级时默认保留已有配置。

| 平台 | 文件示例 | 安装命令 |
| --- | --- | --- |
| Debian / Ubuntu | `router_0.1.0_amd64.deb` | `sudo dpkg -i router_0.1.0_amd64.deb` |
| RHEL / Fedora / openSUSE | `router-0.1.0-1.x86_64.rpm` | `sudo rpm -U router-0.1.0-1.x86_64.rpm` |
| Alpine | `router_0.1.0_x86_64.apk`（静态 musl） | `sudo apk add --allow-untrusted router_0.1.0_x86_64.apk` |
| Arch Linux | `router-0.1.0-1-x86_64.pkg.tar.zst` | `sudo pacman -U router-0.1.0-1-x86_64.pkg.tar.zst` |
| Windows x64 | `router-v0.1.0-x86_64.msi` | 以管理员身份运行 `msiexec /i router-v0.1.0-x86_64.msi` |

安装位置与启动方式：

- **Linux（deb、rpm、Arch 安装包，systemd 发行版）**：程序安装在 `/usr/bin/router`，配置为 `/etc/router/router.yaml`，并随包提供 systemd 单元。安装包会在安装时启用该单元，但不会立即启动；执行 `sudo router service start` 启动它。该单元以非特权 `router` 账户运行。请勿对由安装包管理的实例执行 `router service install`：它会另外写入 `/etc/systemd/system/router.service`，覆盖安装包提供的单元。
- **Alpine（`.apk`）**：安装 `/usr/bin/router` 和 `/etc/router/router.yaml`。包内也会放入 systemd 单元，但 Alpine 不使用 systemd，因此安装时不会注册或启用服务。0.1.0 不提供 OpenRC 单元；请用 `router run --daemon` 运行，或自行编写 OpenRC 服务 / 使用现有进程监管工具。
- **macOS（压缩包）**：将 `router` 复制到 `/usr/local/bin/router`，执行 `sudo router service install` 安装并启动 LaunchDaemon；服务配置位于 `/Library/Application Support/router/router.yaml`。卸载时执行 `sudo router service uninstall`，再删除 `/usr/local/bin/router`。0.1.0 不提供 `.pkg`，请使用“快速开始”中的压缩包。
- **Windows**：程序安装在 `C:\Program Files\router\router.exe`，配置为 `%ProgramData%\router\router.yaml`。以管理员身份打开 PowerShell，执行 `& "C:\Program Files\router\router.exe" service install`，会注册开机计划任务；卸载前先执行 `service uninstall`。
- **Termux**：安装 `termux-services` 后，`router service install` 可写入并管理 `runit` 服务。

## 配置与使用

配置是 YAML 文件，Web 界面、CLI 和手工编辑共用同一个文件：

```bash
router config path                 # 显示实际使用的配置文件路径
router config init                 # 首次创建默认配置
router config validate             # 校验配置
router config show                 # 查看配置（默认隐藏敏感字段）
```

默认路径：

- 压缩包：`$XDG_CONFIG_HOME/router/router.yaml`；未设置时使用 `~/.config/router/router.yaml`；Windows 为 `%APPDATA%\router\router.yaml`。
- Linux 安装包：`/etc/router/router.yaml`。
- macOS 服务（`router service install`）：`/Library/Application Support/router/router.yaml`。
- Windows MSI：`%ProgramData%\router\router.yaml`。

使用 `-c/--config` 或环境变量 `ROUTER_CONFIG` 可以指定其他路径。最小配置示例（使用 Cloudflare，需要完整版、海外版或 `cloudflare` 单服务商版；使用国内版时请改为相应服务商）：

```yaml
schema_version: 1
language: "zh-CN"
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

`env:变量名` 会从进程环境读取凭据。安装为系统服务时，请确认该环境变量对服务进程可见，或通过 Web 界面填写凭据。可用 `router provider list --json` 查看每家服务商需要的字段和说明。

常用命令：

| 命令 | 作用 |
| --- | --- |
| `router provider list` | 列出当前二进制包含的服务商 |
| `router provider test <id>` | 测试凭据和 zone 解析 |
| `router update-now` | 立即执行一次同步 |
| `router config validate` | 校验配置 |
| `router healthcheck --ready` | 检查运行状态；上一轮全部记录失败时返回失败 |
| `router password reset` | 重置 Web 登录密码 |
| `router docs zh` | 阅读内置帮助文档 |
| `router version --json` | 显示版本、目标和构建信息 |

## 语言切换

支持 `zh`、`en`、`ru`、`de`、`ja`，同一语言覆盖 CLI、日志、Web 界面、REST API 错误和内嵌帮助：

```bash
router lang list
router lang show
router lang set zh       # 写入配置，下次启动生效
router --lang zh provider list   # 只对本次命令生效
ROUTER_LANG=ja router provider list
```

Web 界面中的语言选择会立即对正在运行的服务生效。默认新配置使用简体中文；希望整个实例使用其他语言时，在首次启动前执行 `router lang set en`（或 `ru` / `de` / `ja`）。

## 安全建议

- 只从官方发布页下载，并在运行前校验 SHA-256。
- 默认监听 `127.0.0.1:9876`。如果需要公网访问，必须设置强密码，并使流量经过 HTTPS 反向代理；反向代理场景请把外部域名加入 `web.allowed_hosts`，否则写操作会因 `Host` 不匹配而被拒绝。
- 配置文件包含 API 凭据和密码哈希，应设为仅所有者可读；不要提交到版本库或放入公开备份。
- 使用最小权限的 DNS API Token，只授予需要更新的域名或 zone。
- 长期运行优先使用系统服务，让服务管理器负责开机启动和进程监督；容器与调试场景才使用 `--no-guardian`。

## 付费授权与激活

**本仓库发布的所有二进制都是付费商业授权版本，不存在免费版、社区版或未授权试用版。** 无论完整版、`cn`、`global` 还是单服务商版，都必须先购买授权并在目标设备上完成绑定激活，才能提供 DNS 更新或 Web 管理服务。

商用授权为**一次性付款 19.90 美元（US$19.90），终生使用**，每份激活码只绑定一台设备；没有订阅、续期、免费版或多设备授权。第二台设备需要单独购买第二份激活码。

```bash
router license status
router license device-code
router license activate <激活码>
```

- `router license device-code` 生成设备码；将此设备码提交到授权系统并完成付费后，会获得签名激活码。
- `router license activate <激活码>` 在目标设备上完成离线激活；客户端不依赖持续联网心跳。
- 未激活、激活文件被篡改或设备不匹配时，`router run`、`router update-now` 和 Web 管理服务拒绝提供服务，并以状态码 3 退出；首次激活必须通过 CLI 完成。
- 授权与设备指纹绑定；容器部署建议显式设置 `ROUTER_INSTANCE_ID`。
- 配置中的 `license.mode` 不能关闭或降级发布版的授权校验；发布版始终强制设备绑定。

购买、部署与技术支持请通过项目官网 **<https://router.roboter.site>** 联系我们，或发送邮件至 **roboter.site@aliyun.com**。

## 常见问题

| 现象 | 处理 |
| --- | --- |
| `command not found` 或提示找不到程序 | 先解压归档，并在 `router` 所在目录执行 `./router`（Windows 为 `.\router.exe`），或把该目录加入 `PATH`。 |
| `Permission denied` | 执行 `chmod +x router`。macOS 若提示无法验证开发者，见“快速开始”的隔离属性说明。 |
| 提示 `cannot execute binary file` 或格式错误 | 下载了其他平台/架构的归档；用 `uname -m` 确认架构后重新下载。 |
| 提示服务商未编译进当前二进制 | 该服务商不属于当前变体；换用完整版、国内版、海外版或该服务商的单服务商版。 |
| 无法访问 Web 界面 | 检查 `router config path` 指向的配置中的 `web.listen`、本机防火墙，以及是否用 `router healthcheck` 启动成功。 |
| 配置被拒绝 | 执行 `router config validate` 查看具体字段；可先备份配置，再用 Web 界面修正。 |
| 语言没有生效 | `router lang show` 查看当前来源；命令行临时值优先于配置文件，Web 界面修改对运行中的服务立即生效。 |
| 发布版启动即退出，状态码为 3 | 未激活或授权无效；执行 `router license status` 和 `router license activate <激活码>`。 |
| Linux 提示 glibc 版本过旧 | 0.1.0 的 Linux 归档要求 glibc 2.17 或更高版本；请升级系统，或在 Alpine 上安装 `.apk`（本次发布为 x86_64），其中包含静态链接的 musl 构建。静态 musl 不以单独的压缩包形式发布。 |

## 商业授权声明

router 及其发布二进制是 **AutoDRL 的专有商业软件**，不按 MIT、Apache-2.0 或任何开源许可证发布，也不提供免费版；所有权利保留。

除 AutoDRL 另行签署的书面商业授权协议外，不得复制、修改、分发、转售、出租、再许可或反向工程本软件。下载、持有或安装二进制不构成运行授权；只有完成有效激活的安装才能提供服务。

完整声明见 [LICENSE](LICENSE)。本仓库的说明文档仅描述产品，不授予任何二进制使用许可。购买与激活：**<https://router.roboter.site>**。

本仓库仅分发编译产物，不提供源代码。文档对应版本 0.1.0。

[简体中文](README.md) | [English](README.en.md) | [Русский](README.ru.md) | [Deutsch](README.de.md) | [日本語](README.ja.md)
