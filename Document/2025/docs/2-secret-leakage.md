# NHI2:2025 シークレットの漏洩 (Secret Leakage)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **容易**           | 蔓延度: **普通**<br>検出可能性: **困難** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 漏洩したシークレットが攻撃者を正当なアプリケーションとして認証できるという事実を踏まえると、そのシークレットの悪用するのは非常に簡単です。 | シークレットは API キー、アクセスキー、DB クレデンシャルなどのマシン間通信のための一般的な認証方法です。開発ライフサイクル全体を通じて、シークレットは新機能の構築やマシン間統合のテストに使用されるため、組織内の多くのさまざまなデータストアに拡散する傾向があります。 <br/> 漏洩したシークレットを検出することは、シークレットが出現する可能性のあるデータストアが多種多様であることを考えると困難です。たとえば、シークレットは開発者のエンドポイント、アプリケーションログ、設定ファイル、SaaS プロバイダ、クラウドプラットフォームなどに漏洩する可能性があります。 | シークレットは影響度の高い NHI (API キーやデータベース接続文字列など) のクレデンシャルを保持する傾向があるため、侵害の影響は重大です。 |


## 説明

シークレットの漏洩とは、ソフトウェア開発ライフサイクル全体を通じて、API キー、トークン、暗号鍵、証明書などの機密性の高い NHI が未認可のデータストアに漏洩することを指します。開発者はこれらのシークレットを頻繁に使用して、アプリケーションが組織内のさまざまなサービスやリソースを認証してやり取りできるようにします。しかし、たとえば、ソースコードにハードコードされていたり、プレーンテキストの設定ファイルに保存されていたり、パブリックチャットアプリケーションで送信されて、シークレットが漏洩すると、開示されやすくなります。
開示されたシークレットは重大なセキュリティリスクにつながる可能性があります。シークレットがコードリポジトリ、ログ、開発者のマシン上のマルウェアを介して漏洩すると、脅威アクターはそれを悪用して、システムへの不正アクセス、データの窃取、ネットワーク内での権限の昇格を行うことができます。これは、データ侵害、サービス中断、顧客や利害関係者からの信頼の喪失につながる可能性があります。

## 攻撃シナリオの例

* **Azure SAS Token Leakage:** An Azure SAS Token is committed to a public Github repository. Attackers use that SAS Token to authenticate to the associated Azure subscription and leak internal Microsoft Teams messages.
* **Delinea Admin API Key:** A Delinea Admin API Key is stored in a script in an employee-public file share. An attacker with limited privileges in the corporate network identifies the API Key, reads an admin credential from the PAM and escalates their privilege in the corporate network.



## 防御方法

* **Use Ephemeral Credentials Where Possible**
   - Replace static secrets with short-lived, ephemeral credentials that are generated on-demand (e.g., AWS STS, Azure Managed Identities, or OAuth tokens).
   - Ephemeral credentials reduce the risk of long-term exposure.

* **Use Secret Management Tools**
   - Store secrets securely using dedicated secret management tools such as AWS Secrets Manager, Azure Key Vault, or HashiCorp Vault.
   - Ensure secrets are not hardcoded in source code, configuration files, or scripts.

* **Automate Secret Detection**
   - Integrate secret scanning tools (e.g., GitHub Secret Scanning, TruffleHog, Gitleaks) into CI/CD pipelines to detect and prevent secrets from being committed to repositories.

* **Restrict Secret Scope and Permissions**
   - Follow the principle of least privilege by restricting access to secrets to only the applications and services that require them.
   - Use role-based access control (RBAC) to enforce fine-grained permissions.

* **Rotate Secrets Regularly**
   - Automate the process of secret rotation to reduce the impact of exposed credentials.
   - Use tools that support secret versioning and automated updates in dependent services.

## Related OWASP Resources
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
    * 31% of times poor secrets management was the cause for NHI-related security incidents. (6/10)
    * 21% of organizations put service accounts as most challenging to manage. (6/16)
    * 26% of organizations need management of secrets lifecycle as the most important capability of an NHI tool. (1/16)
    * 37% of organizations report secrets are stored in environment variables or hard-coded into application code.
* Verizon DBIR
    * 21% of breaches initial action was use of stolen creds (1/10)
* Recent Breaches
    * MSFT SAS Token Breach - [link](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
    * Uber Breach - [link](https://www.upguard.com/blog/what-caused-the-uber-data-breach)
    * Internet Archive breach - [link](https://www.bleepingcomputer.com/news/security/internet-archive-hacked-data-breach-impacts-31-million-users/)
