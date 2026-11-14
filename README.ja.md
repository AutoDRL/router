# router

**商用グレードの DDNS（Dynamic DNS）クライアント · バージョン 0.1.0**

[简体中文](README.md) | [English](README.en.md) | [Русский](README.ru.md) | [Deutsch](README.de.md) | [**日本語**](README.ja.md)

> **このリポジトリはコンパイル済みのクライアントバイナリのみを配布しています。ソースコードは公開していません。**
> プロジェクトサイト: <https://router.roboter.site> · ダウンロード: **[GitHub Releases](https://github.com/AutoDRL/router/releases/latest)**

router は、マシンのパブリック IPv4 / IPv6 アドレスを監視し、DNS プロバイダー上の A / AAAA レコードを最新の状態に保ちます。単一のネイティブ実行ファイルとして動作し、ランタイム、パッケージマネージャー、外部データベースを必要としません。ローカルの Web インターフェースから管理することも、システムサービスとして常時稼働させることもできます。

> **ライセンス要件：このリポジトリに無料版はありません。** すべてのバイナリは購入済みライセンスとデバイスバインドのアクティベーションが必要です。未アクティベートのインストールは DNS 更新や Web インターフェースを提供できず、終了コード 3 で終了します。購入とアクティベーション: <https://router.roboter.site>。アクティベーションサービスのアドレスは `router license status` に表示されます。

## プロジェクト統計

以下のバッジは、GitHub の公開指標をもとに [Shields.io](https://shields.io) がリアルタイムで生成します。ダウンロード数は Release で公開されたアセットの合計で、各ファイルのダウンロード数は各 Release ページに表示されます。リポジトリが公開され、最初の Release が公開されると、バッジに実際のデータが表示されます。

| 指標 | 最新値 |
| --- | --- |
| Release 合計ダウンロード数 | [![Release downloads](https://img.shields.io/github/downloads/AutoDRL/router/total?style=flat-square&label=downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases) |
| 最新 Release のダウンロード数 | [![Latest release downloads](https://img.shields.io/github/downloads/AutoDRL/router/latest/total?style=flat-square&label=latest%20downloads&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |
| Fork 数 | [![Forks](https://img.shields.io/github/forks/AutoDRL/router?style=flat-square&label=forks&logo=github&logoColor=white)](https://github.com/AutoDRL/router/forks) |
| Star 数 | [![Stars](https://img.shields.io/github/stars/AutoDRL/router?style=flat-square&label=stars&logo=github&logoColor=white)](https://github.com/AutoDRL/router/stargazers) |
| Watcher 数 | [![Watchers](https://img.shields.io/github/watchers/AutoDRL/router?style=flat-square&label=watchers&logo=github&logoColor=white)](https://github.com/AutoDRL/router/watchers) |
| 最新バージョン | [![Latest release](https://img.shields.io/github/v/release/AutoDRL/router?style=flat-square&label=release&logo=github&logoColor=white)](https://github.com/AutoDRL/router/releases/latest) |

## 特長

- **31 社の DNS プロバイダー**: Alibaba Cloud、Tencent Cloud、Huawei Cloud、Volcano Engine、Baidu Cloud、AWS Route 53、Cloudflare、GoDaddy、Namecheap、DigitalOcean、Porkbun、NameSilo、dynv6、Vercel、deSEC、ClouDNS、Gcore、NS1、Name.com、Spaceship、Dynadot、DNSPod、Now.cn、Eranet、Tnet.hk など。
- **IPv4 / IPv6 を個別に更新**: A レコードと AAAA レコードは個別に有効化し、それぞれのドメインを設定できます。
- **5 種類のアドレス取得元**: HTTP エンドポイント（フェイルオーバー対応）、ローカルインターフェース、外部コマンド、DNS プローブ、固定値。
- **アドレスポリシーと再試行**: プライベート、ループバック、リンクローカル、CGNAT、ドキュメント用の範囲を拒否します。アドレスが変わらない間はリモート照会を省略し、レート制限時は `Retry-After` を尊重した指数バックオフを行います。
- **組み込み Web インターフェース**: 初回起動時の管理者作成、Argon2id によるパスワードハッシュ、CSRF 対策、ログイン試行制限、リアルタイムログ、Prometheus メトリクス、ヘルスチェックエンドポイント。
- **5 言語**: 簡体字中国語、英語、ロシア語、ドイツ語、日本語。CLI、ログ、Web インターフェース、REST API のエラー、組み込みヘルプで同じ言語を使用できます。
- **guardian による監視**: `router run` は guardian を起動します。guardian がワーカープロセスを監視し、異常終了した場合は自動的に再起動します。無人での 24 時間 365 日運用に対応します。
- **検証可能な配布**: すべてのアーカイブに SHA-256 チェックサムを添付しています。依存関係をインストールせずに実行できます。

## ビルドの選択

バージョン 0.1.0 では **34 種類のビルド**を提供します。31 種類の単一プロバイダービルド、中国国内向け（`cn`）、海外向け（`global`）、すべてのプロバイダーを含むフルビルドです。各ビルドは、以下に示す 10 種類の対象プラットフォームすべてで利用できます。

| ビルド | アーカイブ名の識別子 | プロバイダー数 | 適した用途 |
| --- | --- | --- | --- |
| フル | なし | 31 | 複数のプロバイダーを使う、選択に迷う、または Linux / Windows のシステムインストーラーが必要な場合 |
| 中国国内向け | `cn` | 13 | 中国本土の DNS サービスのみを使用。海外プロバイダーはコンパイルされません |
| 海外向け | `global` | 20 | 中国本土以外の DNS サービスのみを使用。中国本土のプロバイダーはコンパイルされません |
| 単一プロバイダー | プロバイダー ID（例: `cloudflare`） | 1 | 1 社のみを使用し、バイナリと依存関係の範囲を最小にしたい場合 |

`cn` と `global` には、どちらも 2 つの中立コンポーネント（HiPM DNS Manager と汎用コールバック）が含まれます。そのためプロバイダーは重複しますが、合わせて 31 社すべてを網羅します。実行中のバイナリに含まれないプロバイダーを設定で指定した場合は、そのプロバイダーを含むビルドに切り替えてください。`router provider list` を実行すると、現在のバイナリに含まれるプロバイダーを正確に確認できます。

<details>
<summary><strong>中国国内向け（cn）: 中国本土の 11 社 + 2 つの中立コンポーネント</strong></summary>

Alibaba Cloud DNS (`aliyun`)、Alibaba Cloud ESA (`aliesa`)、Tencent Cloud DNSPod (`tencent`)、Tencent Cloud EdgeOne (`edgeone`)、Volcano Engine DNS (`trafficroute`)、Huawei Cloud DNS (`huawei`)、Baidu Cloud DNS (`baidu`)、DNSPod (`dnspod`)、DNSLA (`dnsla`)、RainYun (`rainyun`)、Now.cn (`nowcn`)、HiPM DNS Manager (`hipmdnsmgr`)、汎用コールバック (`callback`)。

</details>

<details>
<summary><strong>海外向け（global）: 海外の 18 社 + 2 つの中立コンポーネント</strong></summary>

AWS Route 53 (`route53`)、Cloudflare (`cloudflare`)、GoDaddy (`godaddy`)、Namecheap (`namecheap`)、DigitalOcean (`digitalocean`)、Porkbun (`porkbun`)、NameSilo (`namesilo`)、dynv6 (`dynv6`)、Vercel (`vercel`)、deSEC (`desec`)、ClouDNS (`cloudns`)、Gcore (`gcore`)、NS1 (`nsone`)、Name.com (`namecom`)、Spaceship (`spaceship`)、Dynadot (`dynadot`)、Eranet (`eranet`)、Tnet.hk (`tnethk`)、HiPM DNS Manager (`hipmdnsmgr`)、汎用コールバック (`callback`)。

</details>

アーカイブの命名規則（`<target>` の値は次の節を参照）:

| ビルド | Linux / macOS / Android | Windows |
| --- | --- | --- |
| フル | `router-v0.1.0-<target>.tar.gz` | `router-v0.1.0-<target>.zip` |
| 中国国内向け | `router-v0.1.0-cn-<target>.tar.gz` | `router-v0.1.0-cn-<target>.zip` |
| 海外向け | `router-v0.1.0-global-<target>.tar.gz` | `router-v0.1.0-global-<target>.zip` |
| 単一プロバイダー | `router-v0.1.0-<provider>-<target>.tar.gz` | `router-v0.1.0-<provider>-<target>.zip` |

フルビルドはバリアント識別子のない名前を使用します（例: `router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz`）。システムインストーラー（`.deb`、`.rpm`、`.apk`、`.pkg.tar.zst`、`.msi`）はフルビルドのみで提供されます。`cn`、`global`、単一プロバイダービルドはアーカイブのみです。

## 対応プラットフォーム

| OS | アーキテクチャ | アーカイブ名の `<target>` | 備考 |
| --- | --- | --- | --- |
| Linux | x86_64 | `x86_64-unknown-linux-gnu` | glibc |
| Linux | aarch64 | `aarch64-unknown-linux-gnu` | glibc |
| Linux | armv7 | `armv7-unknown-linux-gnueabihf` | hard-float |
| Linux | i686 | `i686-unknown-linux-gnu` | glibc |
| macOS | Intel | `x86_64-apple-darwin` | `.tar.gz`、macOS 10.12 以降 |
| macOS | Apple Silicon | `aarch64-apple-darwin` | `.tar.gz`、macOS 11.0 以降 |
| Windows | x86_64 | `x86_64-pc-windows-msvc` | `.zip` |
| Android / Termux | aarch64 | `aarch64-linux-android` | Bionic libc |
| Android / Termux | armv7 | `armv7-linux-androideabi` | Bionic libc |
| Android / Termux | x86_64 | `x86_64-linux-android` | Bionic libc |

補足:

- 0.1.0 の Linux `.tar.gz` アーカイブは glibc 2.17 にリンクされ、glibc 2.17 以降が必要です。静的 musl アーカイブは提供しません。Alpine ではフルビルドの `.apk`（今回のリリースでは x86_64 のみ）をインストールしてください。パッケージ内のバイナリは静的にリンクされた musl ビルドで、glibc は不要です。
- Android ビルドは Bionic libc にリンクされ、Android 7.0（API 24）以降が必要です。Termux または Android のシェル向けで、通常の Linux ディストリビューションでは動作しません。
- 32 ビット Windows、Windows on ARM、32 ビット macOS のビルドはありません。`uname -m`（Windows では `$env:PROCESSOR_ARCHITECTURE`）に一致するアーカイブを選択してください。
- 各アーカイブには実行ファイルが 1 つだけ含まれます（`router`、Windows では `router.exe`）。

## ダウンロードの検証

すべてのアーカイブに `.sha256` ファイルが付属し、Windows MSI インストーラーにもあります。Linux パッケージのチェックサムは `router_0.1.0_checksums.txt` にまとめて記載されています。実行前に検証してください:

```bash
# Linux
sha256sum -c router-v0.1.0-cn-x86_64-unknown-linux-gnu.tar.gz.sha256

# macOS
shasum -a 256 -c router-v0.1.0-aarch64-apple-darwin.tar.gz.sha256
```

```powershell
# Windows: 期待値を読み取り、ダウンロードしたファイルのハッシュを計算します
Get-Content .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip.sha256
Get-FileHash .\router-v0.1.0-cn-x86_64-pc-windows-msvc.zip -Algorithm SHA256
```

2 つの値が一致しなければなりません。検証に失敗した場合は実行せず、再ダウンロードしてください。それでも失敗する場合は、リリースページの issue で報告してください。コード署名のないインストーラーでは、SHA-256 はファイルの整合性（破損や改ざんがないこと）を証明しますが、発行者の認証を代替するものではありません。

## クイックスタート（アーカイブ）

### Linux

```bash
tar -xzf router-v0.1.0-x86_64-unknown-linux-gnu.tar.gz
chmod +x router
./router config init
./router run
```

### macOS

```bash
tar -xzf router-v0.1.0-aarch64-apple-darwin.tar.gz   # Intel の場合は x86_64-apple-darwin
chmod +x router
xattr -d com.apple.quarantine ./router 2>/dev/null || true
./router config init
./router lang set ja
./router run
```

macOS で開発元を確認できないと表示された場合、そのビルドは Apple の公証を受けていません。先に SHA-256 を確認し、「システム設定 > プライバシーとセキュリティ」で実行を許可してください。0.1.0 は macOS の `.pkg` インストーラーを提供しません。`.tar.gz` アーカイブを使用してください。

### Windows

PowerShell で:

```powershell
Expand-Archive .\router-v0.1.0-x86_64-pc-windows-msvc.zip -DestinationPath .\router
cd .\router
.\router.exe config init
.\router.exe lang set ja
.\router.exe run
```

### Android / Termux

```bash
tar -xzf router-v0.1.0-aarch64-linux-android.tar.gz
chmod +x router
./router config init
./router run
```

その後、ブラウザで **<http://127.0.0.1:9876>** を開きます。初回アクセス時には管理者のユーザー名とパスワードの作成を求められます。作成後、プロバイダー、認証情報、ドメインを追加できます。既定ではループバックアドレスのみをリッスンし、ネットワークには公開されません。

バックグラウンドで実行するには `./router run --daemon` を使用します。長期運用ではフルビルドをシステムサービスとしてインストールしてください（下記参照）。他のビルドは、既存のスーパーバイザーの下で実行できます。コンテナやデバッグでは `--no-guardian` を指定すると単一プロセスで起動します。

## システムインストーラー（フルビルドのみ）

各インストーラーには、同じバージョンのバイナリが生成した既定の設定ファイルが同梱されます。アップグレード時は既存の設定が保持されます。

| プラットフォーム | ファイル例 | インストールコマンド |
| --- | --- | --- |
| Debian / Ubuntu | `router_0.1.0_amd64.deb` | `sudo dpkg -i router_0.1.0_amd64.deb` |
| RHEL / Fedora / openSUSE | `router-0.1.0-1.x86_64.rpm` | `sudo rpm -U router-0.1.0-1.x86_64.rpm` |
| Alpine | `router_0.1.0_x86_64.apk`（静的 musl） | `sudo apk add --allow-untrusted router_0.1.0_x86_64.apk` |
| Arch Linux | `router-0.1.0-1-x86_64.pkg.tar.zst` | `sudo pacman -U router-0.1.0-1-x86_64.pkg.tar.zst` |
| Windows x64 | `router-v0.1.0-x86_64.msi` | 管理者として `msiexec /i router-v0.1.0-x86_64.msi` を実行 |

インストール先と起動方法:

- **Linux（deb・rpm・Arch パッケージ、systemd ディストリビューション）**: バイナリは `/usr/bin/router`、設定ファイルは `/etc/router/router.yaml` です。パッケージには systemd ユニットが含まれます。パッケージはインストール時にユニットを有効化しますが、すぐには起動しません。`sudo router service start` で起動してください。ユニットは特権を持たない `router` アカウントで動作します。パッケージ管理のインストールでは `router service install` を実行しないでください。別のユニットが `/etc/systemd/system/router.service` に書き込まれ、パッケージのユニットを置き換えます。
- **Alpine（`.apk`）**: `/usr/bin/router` と `/etc/router/router.yaml` を配置します。パッケージには systemd ユニットも含まれますが、Alpine は systemd を使用しないため、インストール時にサービスの登録や有効化は行われません。0.1.0 は OpenRC ユニットを提供しません。`router run --daemon` で起動するか、独自の OpenRC サービスまたはスーパーバイザーを追加してください。
- **macOS（アーカイブ）**: `router` を `/usr/local/bin/router` にコピーし、`sudo router service install` を実行すると LaunchDaemon がインストールされて起動します。サービスの設定ファイルは `/Library/Application Support/router/router.yaml` です。削除するには `sudo router service uninstall` を実行し、`/usr/local/bin/router` を削除します。0.1.0 には `.pkg` がありません。クイックスタートのアーカイブを使用してください。
- **Windows**: バイナリは `C:\Program Files\router\router.exe`、設定ファイルは `%ProgramData%\router\router.yaml` です。管理者権限の PowerShell で `& "C:\Program Files\router\router.exe" service install` を実行すると、起動時のタスクが登録されます。製品を削除する前に `service uninstall` を実行してください。
- **Termux**: `termux-services` をインストールした後、`router service install` で `runit` サービスを作成・管理できます。

## 設定と使い方

設定は YAML ファイルです。Web インターフェース、CLI、手動編集のいずれも同じファイルを使用します:

```bash
router config path                 # 使用中の設定ファイルのパスを表示
router config init                 # 既定の設定ファイルを作成
router config validate             # 設定を検証
router config show                 # 設定を表示（機密項目は既定で伏せられます）
```

既定のパス:

- アーカイブ: `$XDG_CONFIG_HOME/router/router.yaml`、未設定時は `~/.config/router/router.yaml`。Windows では `%APPDATA%\router\router.yaml`。
- Linux パッケージ: `/etc/router/router.yaml`。
- macOS サービス（`router service install`）: `/Library/Application Support/router/router.yaml`。
- Windows MSI: `%ProgramData%\router\router.yaml`。

`-c/--config` または環境変数 `ROUTER_CONFIG` で別のファイルを指定できます。最小設定の例（Cloudflare を使用するため、フルビルド、海外向けビルド、または `cloudflare` 単一プロバイダービルドが必要です。中国国内向けビルドを使用する場合は、そのビルドに含まれるプロバイダーに置き換えてください）:

```yaml
schema_version: 1
language: "ja"
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

`env:変数名` はプロセスの環境から認証情報を読み取ります。システムサービスとして動かす場合は、その環境変数がサービスから参照できることを確認するか、Web インターフェースから認証情報を入力してください。`router provider list --json` で、各プロバイダーに必要な項目とドキュメントへのリンクを確認できます。

よく使うコマンド:

| コマンド | 用途 |
| --- | --- |
| `router provider list` | このバイナリに含まれるプロバイダーを一覧表示 |
| `router provider test <id>` | 認証情報とゾーン解決をテスト |
| `router update-now` | 周期を待たずに 1 回実行 |
| `router config validate` | 設定を検証 |
| `router healthcheck --ready` | 状態を確認。直近のサイクルですべてのレコードが失敗していれば、コマンドは失敗として終了します。 |
| `router password reset` | Web ログインのパスワードをリセット |
| `router docs ja` | 組み込みヘルプを表示 |
| `router version --json` | バージョン、対象、ビルド情報を表示 |

## 言語

対応コードは `zh`、`en`、`ru`、`de`、`ja` です。1 つの言語設定が CLI、ログ、Web インターフェース、REST API のエラー、組み込みヘルプに適用されます:

```bash
router lang list
router lang show
router lang set ja       # 次回起動時から有効になるよう設定ファイルへ保存
router --lang ja provider list   # このコマンドのみ
ROUTER_LANG=ja router provider list
```

Web インターフェースで選択した言語は、実行中のサービスに即座に反映されます。新しい設定の既定言語は簡体字中国語です。インスタンスを最初から別の言語で起動するには、初回起動前に `router lang set ja`（または `en` / `ru` / `de`）を実行してください。

## セキュリティ

- 必ず公式リリースページからダウンロードし、実行前に SHA-256 を検証してください。
- 既定のリッスンアドレスは `127.0.0.1:9876` です。Web インターフェースを外部に公開する場合は、強力なパスワードを設定し、リバースプロキシで HTTPS を終端してください。プロキシ配下では外部ホスト名を `web.allowed_hosts` に追加してください。追加しない場合、`Host` ヘッダーが一致しない書き込み要求は拒否されます。
- 設定ファイルには API 認証情報とパスワードハッシュが含まれます。所有者のみが読めるようにし、リポジトリや公開バックアップに置かないでください。
- router が更新するゾーンとレコードだけに権限を絞った DNS API トークンを使用してください。
- 長期運用ではシステムサービスを使用し、OS 起動時の自動起動とプロセスの監視をサービスマネージャーに任せてください。`--no-guardian` はコンテナやデバッグ時のみ使用します。

## 有償ライセンスとアクティベーション

**このリポジトリで公開されるすべてのバイナリは有償の商用ビルドです。無料版、コミュニティ版、未認証の試用版はありません。** フル、`cn`、`global`、単一プロバイダーの各ビルドはいずれも、DNS 更新や Web インターフェースを提供する前に、購入済みライセンスとデバイスにバインドされたアクティベーションが必要です。

```bash
router license status
router license device-code
router license activate <アクティベーションコード>
```

- `router license device-code` はデバイスコードを出力します。これをライセンスシステムに送信し、購入を完了すると署名済みアクティベーションコードが発行されます。
- `router license activate <アクティベーションコード>` は対象デバイスをオフラインでアクティベートします。クライアントは常時接続のハートビートに依存しません。
- ライセンスがない、期限切れ、改ざん、または別デバイスにバインドされている場合、`router run`、`router update-now`、Web インターフェースはサービス提供を拒否し、終了コード 3 で終了します。最初のアクティベーションは CLI から行う必要があります。
- ライセンスはデバイスフィンガープリントにバインドされます。コンテナでは `ROUTER_INSTANCE_ID` を明示的に設定することを推奨します。
- 設定の `license.mode` では、公開ビルドのライセンス検証を無効化またはダウングレードできません。公開ビルドは常にデバイスバインドのアクティベーションを要求します。

購入、更新、導入、サポートに関するお問い合わせ: **<https://router.roboter.site>**。

## トラブルシューティング

| 症状 | 対処 |
| --- | --- |
| `command not found`、またはプログラムが見つからない | アーカイブを展開し、`router` のあるディレクトリで `./router` を実行します（Windows は `.\router.exe`）。またはそのディレクトリを `PATH` に追加します。 |
| `Permission denied` | `chmod +x router` を実行します。macOS で開発元を確認できない場合は、クイックスタートの隔離属性の説明を参照してください。 |
| `cannot execute binary file` または形式エラー | 別のプラットフォームやアーキテクチャのアーカイブをダウンロードしています。`uname -m` を確認し、一致する対象をダウンロードしてください。 |
| プロバイダーがビルドに含まれていないと表示される | 現在のビルドには含まれていません。フル、中国国内向け、海外向け、またはそのプロバイダーの単一ビルドに切り替えてください。 |
| Web インターフェースにアクセスできない | `router config path` が示す設定の `web.listen`、ローカルファイアウォール、プロセスの状態（`router healthcheck`）を確認してください。 |
| 設定が拒否される | `router config validate` で問題の項目を確認します。先に設定をバックアップし、Web インターフェースで修正してください。 |
| 言語が変わらない | `router lang show` で有効な取得元を確認します。コマンドライン指定はその実行のみ有効で、Web インターフェースの変更は実行中のサービスに即座に反映されます。 |
| 公開ビルドが終了コード 3 ですぐ終了する | 未アクティベート、またはライセンスが無効です。`router license status` と `router license activate <アクティベーションコード>` を実行してください。 |
| Linux で glibc が古いと表示される | 0.1.0 の Linux アーカイブは glibc 2.17 以降が必要です。システムを更新するか、Alpine では `.apk`（今回のリリースでは x86_64）をインストールしてください。パッケージ内は静的にリンクされた musl ビルドです。静的 musl を単独のアーカイブとして提供することはありません。 |

## 商用ライセンスに関する告知

router およびその公開バイナリは **AutoDRL のプロプライエタリ商用ソフトウェア**です。MIT、Apache-2.0、その他のオープンソースライセンスでは提供されず、無料版も存在せず、すべての権利は留保されています。

AutoDRL が署名した書面による商用契約がない限り、本ソフトウェアの複製、変更、配布、転売、貸与、再許諾、リバースエンジニアリングを禁止します。バイナリのダウンロード、所持、インストールは実行権を付与するものではなく、有効にアクティベートされたインストールのみがサービスを提供できます。

完全な告知は [LICENSE](LICENSE) を参照してください。このリポジトリのドキュメントは製品を説明するものであり、バイナリに関するいかなるライセンスも付与しません。購入とアクティベーション: **<https://router.roboter.site>**。

このリポジトリはバイナリのみを配布しており、ソースコードは含まれていません。本ドキュメントはバージョン 0.1.0 に対応しています。

[简体中文](README.md) | [English](README.en.md) | [Русский](README.ru.md) | [Deutsch](README.de.md) | [**日本語**](README.ja.md)
