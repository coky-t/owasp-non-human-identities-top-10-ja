# NHI5:2025 過剰特権の NHI (Overprivileged NHI)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **困難**           | 蔓延度: **広範**<br>検出可能性: **普通** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 過剰特権の NHI の悪用に成功するには、脅威エージェントがまず環境にアクセスする必要があります。したがって、過剰特権の NHI は個別の初期アクセスベクトルに依存します。 | NHI の権限を適切に設定するのは非常に困難で時間のかかる作業であるため、NHI に過剰な権限が与えられることは非常に一般的です。過剰特権の非人間アイデンティティの検出は環境の種類によって異なります。クラウド環境では検出を容易にするツールを提供していますが、オンプレミス環境では同様の機能が組み込まれていないため、そのようなアイデンティティの検出は非常に困難になります。 | 関連する権限の量が多いため、過剰特権の NHI の影響は大きくなります。これらは広範囲に影響を及ぼす管理者アカウントである傾向があります。 |


## 説明

サービスアカウント、API トークン、ワークロードアイデンティティなどの非人間アイデンティティ (NHI) はクラウドリソースとサービスへのプログラムによるアクセスのために設計されています。これらによりアプリケーション、サービス、自動化されたプロセスが人間の介入なしに安全に機能できます。しかし、アプリケーション開発や保守の際に、開発者や管理者が誤って NHI に機能要件を超える過剰な権限を割り当て、侵害が発生した場合の潜在的な影響範囲を不必要に広げてしまう可能性があります。
過剰特権の NHI が、アプリケーションの脆弱性、マルウェア、その他のセキュリティ侵害によって侵害された場合、攻撃者は過剰な権限を悪用して以下を行うことができます。

* **機密データにアクセスする:** 機密ファイル、データベース、ユーザー情報へ不正アクセスします。
* **権限を昇格する:** システム内でより高いレベルのアクセス権を取得し、管理者レベルまたはルートレベルに達する可能性があります。
* **ネットワーク内でラテラルムーブする:** NHI が到達できる組織のネットワーク内で他のシステムやサービスにアクセスします。
* **悪意のあるソフトウェアをインストールする:** マルウェア、ランサムウェア、その他の悪意のあるツールをデプロイして、システムをさらに侵害します。
* **クラウドアカウント全体を乗っ取る:** クラウドルートアカウントまたは管理者に関連するアイデンティティが漏洩すると、完全な制御やアカウントの乗っ取りにつながる可能性があります。

## 攻撃シナリオの例

* **過剰な権限を持つウェブサーバーユーザー:** ウェブサーバーは、他のアプリケーション、システムファイル、機密データディレクトリにもアクセスできる Linux マシン上のローカルユーザーアカウントで実行します。ウェブサーバーにリモートコード実行を許可する脆弱性がある場合、攻撃者はこれを悪用してウェブサーバープロセスを制御できます。ユーザーアカウントの過剰な権限により、攻撃者は他のアプリケーションにアクセスや変更したり、機密データを盗んだり、不正なシステム変更を行う可能性があります。
* **過剰な権限を持つ VM:** Jenkins EC2 インスタンスは EKS と ECS の権限のみが必要であるにもかかわらず、誤って AWS AdministratorAccess 管理ポリシーが割り当てられています。インスタンスの脆弱性を悪用して、攻撃者は初期アクセスを獲得し、過剰な権限を活用してクラウド環境をナビゲートし、S3 バケットから機密データを盗み出します。
* **過剰な権限を持つ OAuth アプリケーション:** 開発者は開発中の OAuth アプリケーションを本番 Azure アカウントにインストールします。そのアプリは Azure Blob Storage 内の特定のディレクトリへの読み取りアクセスのみを必要としているにもかかわらず、AppRoleAssignment.ReadWriteAll 権限を付与します。これにより悪意のあるエンティティがそのアプリケーションを入手した場合に与えることができる損害の影響が大幅に増加します。
* **過剰な権限を持つデータベースサービスアカウント:** 管理されたデータベースサービスは、アカウントの管理者権限を持つサービスアカウントで動作します。攻撃者がデータベースへのアクセスに成功した場合、サービスアカウントの高レベルの権限を使用して、クラウドアカウント全体にアクセスし、アクションを実行できます。
* **広範なネットワークアクセスを持つ制限のないアプリケーションユーザー:** データベースアプリケーションはサーバー上で管理者権限を持つサービスアカウントで動作します。攻撃者がデータベースソフトウェアの脆弱性を悪用すると、サービスアカウントの高レベルの権限を使用して任意のコマンドを実行したり、マルウェアをインストールしたり、新しいユーザーアカウントを作成し、システム全体の侵害につながる可能性があります。


## 防御方法

* **最小権限の原則を適用する:** 各アイデンティティには特定のタスクに必要なパーミッションのみを割り当て、絶対に必要な場合を除き、いかなる形式の管理者権限も避けます。
* **パーミッションを定期的に監査およびレビューする:** アイデンティティに付与されたパーミッションが厳密に必要であることを確認するために、それらを継続的に評価します。特権アイデンティティを監査し、潜在的な誤用や過剰プロビジョニングを検出して対処します。
* **予防的なガードレールを確立する:** 組織レベルで拒否ポリシーを実装し、過度に寛容な構成を禁止し、厳格なアクセス制御を実施します。
* **Just-in-Time (JIT) アクセスを活用する:** 一時的でオンデマンドの権限昇格を可能にするツールを活用して、必要な場合にのみ定義された時間枠内で高レベルのアクセスを許可します。

## 参考情報
* .env File Breach (August 2024) - [link1](https://unit42.paloaltonetworks.com/large-scale-cloud-extortion-operation/), [link2](https://medium.com/@ronilichtman/large-scale-extortion-via-secrets-in-env-files-why-secret-vaults-just-arent-enough-9b4c568724ca)
* Microsoft Midnight Blizzard breach (January 2024) - [link1](https://msrc.microsoft.com/blog/2024/01/microsoft-actions-following-attack-by-nation-state-actor-midnight-blizzard/), [link2](https://medium.com/@ronilichtman/how-to-protect-yourself-from-the-microsoft-oauth-attack-powershell-scripts-included-71b398034b8d)
* Microsoft SAS Token Breach (September 2023) - [link](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
* CircleCI Breach (January 2023) - [link](https://circleci.com/blog/jan-4-2023-incident-report/)
* Uber Breach (September 2022) - [link](https://www.upguard.com/blog/what-caused-the-uber-data-breach)
* Verkada Breach (March 2021) - [link](https://www.verkada.com/security-update/report/)

## データポイント
* [Datadog State of the Cloud 2024](https://www.datadoghq.com/state-of-cloud-security/)
    * 17.6% have excessive data access, such as listing and accessing data from all S3 buckets in the account
    * 10% of clusters have a dangerous node role that has full administrator access, allows for privilege escalation, has overly permissive data access (e.g., all S3 buckets), or allows for lateral movement across all workloads in the account
    * Over one in three Google Cloud VMs (33%) have sensitive permissions to a project
* [CSA NHI Report](https://cloudsecurityalliance.org/artifacts/state-of-non-human-identity-security-survey-report) 
    * 33% answers put over-privileged accounts as one of the top 3 most concerning NHI threats (3/10)
    * 37% of times over-privileged identities were the cause for NHI-related security incidents (2/10)
    * 22% of organizations need managing permissions as the most important capability of an NHI tool (5/16)
    * 26% of organizations believe that over 50% of their service accounts are over-privileged
* [Orca Security State of the Cloud Security report 2022](https://orca.security/wp-content/uploads/2022/09/2022-State-of-Public-Cloud-Security-Report.pdf)
    * 44% of environments have at least one privileged identity access management (IAM) role.
    * 23% have at least one EC2 Instance with Administrator IAM role.
