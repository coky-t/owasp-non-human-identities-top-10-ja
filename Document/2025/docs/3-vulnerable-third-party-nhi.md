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

* **IDE Integration with Code Repositories** Developers often configure their IDEs to interact directly with code repositories like GitHub or GitLab. This setup requires supplying the IDE or its extensions with personal access tokens (PATs) or SSH keys to enable features like code syncing, pushing commits, and managing pull requests. If an extension is compromised, these credentials can be exposed, allowing unauthorized access to the developer's repositories and potentially sensitive organizational code.
* **Extensions Accessing Cloud Resources** Extensions that facilitate deployment and testing on virtual machines or cloud services require access to cloud environments. Developers provide these extensions with NHIs such as API keys or access tokens to interact with services like AWS, Azure, or Google Cloud Platform. A malicious extension could use these credentials to access, modify, or delete cloud resources, leading to data breaches or service disruptions.
* **3rd Party Service Provider** Developers integrate a 3rd party service provider such as Sisense to create a BI application. The developers create a privileged NHI such as database credentials and send them to the provider as part of the integration process. If the provider is breached, the attacker can leverage the NHI to gain access to the developer’s environment.


## 防御方法

* **Vet and Limit Third-Party Integrations**  
   - Perform security reviews before integrating third-party tools or services, ensuring vendors follow security best practices.  
   - Grant only the minimum permissions required (principle of least privilege) and regularly audit or revoke unused access.

* **Monitor and Detect Third-Party Behavior**  
   - Continuously monitor third-party activity using logs, API call tracking, and behavior analytics to detect anomalies.  
   - Implement security scanning tools to identify vulnerabilities or malicious behavior in third-party software.

* **Use Ephemeral and Rotating Credentials**  
   - Replace long-term secrets with short-lived, ephemeral credentials (e.g., AWS STS, Azure Managed Identities).  
   - Automate credential rotation to limit the impact of compromised third-party integrations.


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
