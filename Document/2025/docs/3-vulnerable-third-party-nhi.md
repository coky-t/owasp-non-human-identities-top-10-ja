# NHI3:2025 脆弱なサードパーティ NHI (Vulnerable Third-Party NHI)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **普通**           | 蔓延度: **普通**<br>検出可能性: **困難** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 脆弱なサードパーティアプリケーションを見つけることは、簡単なことではなく、実現にはある程度の労力が必要です。しかし、一旦侵入されると、サードパーティの顧客/ユーザー/クライアントにアクセスするのは簡単です。 | サードパーティアプリケーションは、VSCode 拡張機能やビジネスアプリケーションなど、開発者によってよく使用されます。多くの場合、このような拡張機能の所有者は、ベストプラクティスに従わない小規模コミュニティや個人開発者であり、そのため脆弱性にさらされています。サードパーティの実装は通常、組織から隠されています。さらに、侵害が公表されることはまれであり、公表されたとしても詳細は不完全であるため、監視することは困難になります。事前通告なしに侵害されたサードパーティを検出することはほぼ不可能です。 | サードパーティアプリケーションは通常、ソースコード、シークレット、ファイルなどの機密性の高いマテリアルへの広範なアクセスを許されています。開発者のエコシステムが侵害されると、サプライチェーン攻撃、個人データの窃取、重要システムへの影響につながる可能性があります。 |


## 説明

サードパーティ非人間アイデンティティ (NHI) は、統合開発環境 (IDE) とその拡張機能の使用、およびサードパーティ SaaS の使用を通じて、開発ワークフローに広範囲に統合されています。たとえば、Visual Studio Code (VSCode) には機能を強化する拡張機能の膨大なマーケットプレイスがあります。これらの拡張機能は、多くの場合、コードの読み取り、環境変数へのアクセス、他のシステムリソースとのやり取りなど、開発者のマシンへの重要なアクセスを必要とします。
その機能を実行するために、サードパーティはバージョン管理システム、データベース、仮想マシン、クラウド環境などの外部サービスと統合する必要があるかもしれません。この統合にはアクセストークン、API キー、SSH キーなどの機密性の高い NHI をサードパーティに提供する必要があります。サードパーティ拡張機能がセキュリティ脆弱性や悪意のあるアップデートによって侵害された場合、悪用されて、これらのクレデンシャルを窃取したり、付与された権限を悪用する可能性があります。
さらに、サードパーティはコードベース内にハードコードされたクレデンシャルや機密情報を含む環境変数にさらされる可能性があります。サードパーティがこれらの要素にアクセスし、それが悪意のあるものになったり、すでに悪意のある場合、この情報を流出する可能性があります。したがって、適切な審査やセキュリティ対策を講じずにサードパーティ NHI に依存すると、組織に重大なリスクをもたらします。

## 攻撃シナリオの例

* **コードリポジトリと IDE の統合** 開発者は GitHub や GitLab などのコードリポジトリと直接やり取りするように IDE を設定することがよくあります。このセットアップでは IDE やその拡張機能に個人アクセストークン (PAT) や SSH キーを提供して、コードの同期、コミットのプッシュ、プルリクエストの管理などの機能を有効にする必要があります。拡張機能が侵害されると、これらのクレデンシャルは開示されて、開発者のリポジトリや機密性の高い組織のコードへの不正アクセスが可能になります。
* **クラウドリソースにアクセスする拡張機能** 仮想マシンやクラウドサービス上でのデプロイメントやテストを容易にする拡張機能にはクラウド環境へのアクセスが必要です。開発者はこれらの拡張機能に API キーやアクセストークンなどの NHI を提供して、AWS, Azure, Google Cloud Platform などのサービスとやり取りします。悪意のある拡張機能はこれらのクレデンシャルを使用して、クラウドリソースにアクセス、変更、削除し、データ侵害やサービス中断を引き起こす可能性があります。
* **サードパーティサービスプロバイダ** 開発者は Sisense などのサードパーティサービスプロバイダを統合して BI アプリケーションを作成します。開発者はデータベースクレデンシャルなどの特権 NHI を作成して、統合プロセスの一環としてプロバイダに送信します。プロバイダが侵害されると、攻撃者は NHI を利用して開発者の環境にアクセスできます。


## 防御方法

* **サードパーティ統合を審査して制限する**
   - サードパーティツールやサービスを統合する前にセキュリティレビューを実行し、ベンダーがセキュリティベストプラクティスに従っていることを確認します。
   - 必要な最低限のパーミッションのみを付与し (最小権限の原則)、未使用のアクセスを定期的に監査するか取り消します。

* **サードパーティの動作を監視して検出する**
   - ログ、API コール追跡、動作分析を使用してサードパーティのアクティビティを継続的に監視し、場を検出します。
   - セキュリティスキャンツールを導入して、サードパーティソフトウェアの脆弱性や悪意のある動作を特定します。

* **一時的なクレデンシャルとクレデンシャルローテーションを使用する**
   - 長期間有効なシークレットを、短期間有効で一時的なクレデンシャル (AWS STS, Azure Managed Identities など) に置き換えます。
   - シークレットローテーションを自動化して、侵害されたサードパーティ統合の影響を制限します。


## 参考情報
* [JetBrains Security Issue Affecting GitHub Plugin](https://blog.jetbrains.com/security/2024/06/updates-for-security-issue-affecting-intellij-based-ides-2023-1-and-github-plugin/)
* [Malicious VSCode Extensions Stealing Data](https://blog.checkpoint.com/securing-the-cloud/malicious-vscode-extensions-with-more-than-45k-downloads-steal-pii-and-enable-backdoors/)
* [Deep Dive into VSCode Extension Vulnerabilities](https://snyk.io/blog/visual-studio-code-extension-security-vulnerabilities-deep-dive/)
* [Making sense out of the Sisense hack](https://medium.com/@ronilichtman/making-sense-out-of-the-sisense-hack-f61a3d9b80a7)


## データポイント
* [Datadog State of the Cloud 2024]((https://www.datadoghq.com/state-of-cloud-security/))
    * 10% third party integration to AWS are overprivileged.
    * 2% third party integration to AWS are vulnerable to confused deputy vulnerability.
    * Initial access to 365 is made through malicious 3d-party OAuth apps.
* [CSA NHI Report](https://cloudsecurityalliance.org/artifacts/state-of-non-human-identity-security-survey-report)
    * 38% answers put supply chain attacks as one of the top 3 most concerning NHI threats. (2/10)
    * 16% answers put malicious suppliers as one of the top 3 most concerning NHI threats. (9/10)
    * 29% of times compromised external integrations were the cause for NHI-related security incidents. (7/10)
    * 21% of organizations put managing requests for third-party tools and services as most challenging to manage. (10/16)
    * 26% of organizations need visibility into third-party vendors as the most important capability of an NHI tool. (1/16)
    * 38% of organizations reported limited to no visibility into third-party vendors.
* Recent Breaches
    * Sisense Breach - [link](https://medium.com/@ronilichtman/making-sense-out-of-the-sisense-hack-f61a3d9b80a7)
