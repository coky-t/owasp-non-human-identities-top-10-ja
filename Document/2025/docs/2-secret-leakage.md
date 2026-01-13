# NHI2:2025 シークレットの漏洩 (Secret Leakage)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **容易**           | 蔓延度: **普通**<br>検出可能性: **困難** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 漏洩したシークレットが攻撃者を正当なアプリケーションとして認証できるという事実を踏まえると、そのシークレットの悪用するのは非常に簡単です。 | シークレットは API キー、アクセスキー、DB クレデンシャルなどのマシン間通信のための一般的な認証方法です。開発ライフサイクル全体を通じて、シークレットは新機能の構築やマシン間統合のテストに使用されるため、組織内の多くのさまざまなデータストアに拡散する傾向があります。 <br/> 漏洩したシークレットを検出することは、シークレットが出現する可能性のあるデータストアが多種多様であることを考えると困難です。たとえば、シークレットは開発者のエンドポイント、アプリケーションログ、設定ファイル、SaaS プロバイダ、クラウドプラットフォームなどに漏洩する可能性があります。 | シークレットは影響度の高い NHI (API キーやデータベース接続文字列など) のクレデンシャルを保持する傾向があるため、侵害の影響は重大です。 |


## 説明

シークレットの漏洩とは、ソフトウェア開発ライフサイクル全体を通じて、API キー、トークン、暗号鍵、証明書などの機密性の高い NHI が未認可のデータストアに漏洩することを指します。開発者はこれらのシークレットを頻繁に使用して、アプリケーションが組織内のさまざまなサービスやリソースを認証してやり取りできるようにします。しかし、たとえば、ソースコードにハードコードされていたり、プレーンテキストの設定ファイルに保存されていたり、パブリックチャットアプリケーションで送信されて、シークレットが漏洩すると、開示されやすくなります。
開示されたシークレットは重大なセキュリティリスクにつながる可能性があります。シークレットがコードリポジトリ、ログ、開発者のマシン上のマルウェアを介して漏洩すると、脅威アクターはそれを悪用して、システムへの不正アクセス、データの窃取、ネットワーク内での権限の昇格を行うことができます。これは、データ侵害、サービス中断、顧客や利害関係者からの信頼の喪失につながる可能性があります。

## 攻撃シナリオの例

* **Azure SAS トークンの漏洩:** Azure SAS トークンはパブリック GitHub リポジトリにコミットされています。攻撃者はその SAS トークンを使用して、関連する Azure サブスクリプションへの認証を行い、Microsoft Teams の内部メッセージを漏洩します。
* **Delinea Admin API キー:** Delinea Admin API は従業員の公開ファイル共有内のスクリプトに保存されています。企業ネットワーク内での限られた権限を有する攻撃者はその API キーを識別して、PAM から管理者クレデンシャルを読み取り、企業ネットワーク内での権限を昇格します。



## 防御方法

* **可能な場合は一時的なクレデンシャルを使用する**
   - 静的なシークレットを、オンデマンドで生成される短期間有効で一時的なクレデンシャル (AWS STS, Azure Managed Identities, OAuth トークンなど) に置き換えます。
   - 一時的なクレデンシャルは長期的な開示のリスクを軽減します。

* **シークレット管理ツールを使用する**
   - AWS Secrets Manager, Azure Key Vault, HashiCorp Vault などの専用のシークレット管理ツールを使用して、シークレットを安全に保存します。
   - シークレットがソースコード、設定ファイル、スクリプトにハードコードされていないことを確認します。

* **シークレット検出を自動化する**
   - シークレットスキャンツール (GitHub Secret Scanning, TruffleHog, Gitleaks, Kingfisher など) を CI/CD パイプラインに統合して、シークレットがリポジトリにコミットされるのを検出して防止します。

* **シークレットのスコープとパーミッションを制限する**
   - 最小権限の原則に従い、シークレットへのアクセスを、それを必要とするアプリケーションとサービスのみに制限します。
   - ロールベースのアクセス制御 (RBAC) を使用して、きめ細かいパーミッションを適用します。

* **シークレットを定期的に入れ替える**
   - シークレットローテーションのプロセスを自動化して、開示されたクレデンシャルの影響を軽減します。
   - シークレットのバージョン管理と依存サービスの自動更新をサポートするツールを使用します。

## 関連する OWASP リソース
* [OWASP Secrets Management Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
* [OWASP WrongSecrets project](https://github.com/OWASP/wrongsecrets/)

## 参考情報
* 38TB of data accidentally exposed by Microsoft AI researchers - [link](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
* What Caused the Uber Data Breach? - [link](https://www.upguard.com/blog/what-caused-the-uber-data-breach)
* Microsoft Azure Key Vault: Secrets Management Best Practices - [link](https://learn.microsoft.com/en-us/azure/key-vault/secrets/secrets-best-practices)
* HashiCorp Vault: What is Vault? - [link](https://developer.hashicorp.com/vault/docs/what-is-vault)
* GitHub: Best Practices for Securing Your Code -[link](https://docs.github.com/en/code-security)
* AWS Secrets Manager - [link](https://aws.amazon.com/secrets-manager/)

## データポイント
* [CSA NHI Report](https://cloudsecurityalliance.org/artifacts/state-of-non-human-identity-security-survey-report)
    * 31% の回答が NHI 関連のセキュリティインシデントの原因としてシークレット管理の不備を挙げています。 (6/10)
    * 21% の組織がサービスアカウントの管理を最も困難な課題として挙げています。 (6/16)
    * 26% の組織が NHI ツールの最も重要な機能としてシークレットライフサイクルの管理を必要としています。 (1/16)
    * 37% の組織がシークレットを環境変数に保存しているか、アプリケーションコードにハードコードしていると報告しています。
* Verizon DBIR
    * 21% の侵害が初期アクションとして盗まれたクレデンシャルを使用していました。 (1/10)
* 最近の侵害
    * MSFT SAS トークンの侵害 - [リンク](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
    * Uber の侵害 - [リンク](https://www.upguard.com/blog/what-caused-the-uber-data-breach)
    * Internet Archive の侵害 - [リンク](https://www.bleepingcomputer.com/news/security/internet-archive-hacked-data-breach-impacts-31-million-users/)
