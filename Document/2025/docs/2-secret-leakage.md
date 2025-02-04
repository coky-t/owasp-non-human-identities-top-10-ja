# NHI2:2025 シークレットの漏洩 (Secret Leakage)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| Exploitability: **Easy**                                                                                                                                                                        | Prevalence: **Common**<br>Detectability:  **Hard**                                                                        | Technical: **Severe**<br>Business: **Specific**                                                                                                                    |
| Successfully exploiting a Leaked Secret is extremely easy given the fact that the secret enables the attacker to authenticate as the legitimate application. | Secrets are a common authentication method for machine-to-machine communication including API Keys, Access Keys, DB credentials. Throughout the development lifecycle, secrets are used to build new features and test machine-to-machine integrations and therefore tend to proliferate in the organization to many different data stores.<br />Detecting Leaked Secrets is difficult given the sheer variety of data stores in which they may appear. For example, Secrets can leak to developer endpoints, application logs, configuration files, SaaS providers, Cloud platforms and more. | Secrets tend to hold credentials for high-impact NHI (such as API Keys and Database connection strings), therefore impact on breach is Severe. |


## 説明

Secret Leakage refers to the leakage of sensitive NHIs such as API keys, tokens, encryption keys, and certificates to unsanctioned data stores throughout the software development lifecycle. Developers frequently use these secrets to enable applications to authenticate and interact with various services and resources within an organization. However, when secrets are leaked —for instance, hard-coded into source code, stored in plain text configuration files, or sent over public chat applications —they become susceptible to exposure.
Exposed secrets can lead to significant security risks. If a secret is leaked, whether through code repositories, logs, or malware on a developer's machine, threat actors can exploit it to gain unauthorized access to systems, steal data, or escalate privileges within the network. This can result in data breaches, service disruptions, and a loss of trust from customers and stakeholders.

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
