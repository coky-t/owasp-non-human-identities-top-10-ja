# NHI7:2025 長期間有効なシークレット (Long-Lived Secrets)

| Threat agents/Attack vectors                                                                                                                                                                     | Security Weakness                                                                                                                                                    | Impacts                                                                                                                                                             |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Exploitability: **Hard**                                                                                                                                                                        | Prevalence: **Widespread**<br>Detectability: **Easy**                                                                                                               | Technical: **Severe**<br>Business: **Specific**                                                                                                                    |
| Successfully exploiting a Long-Lived Secret requires the threat agent to first gain access to the secret value. Therefore, Long-Lived Secret attacks depend on a separate initial access vector. | Long-Lived Secrets are extremely common in modern environments. This is due to the challenges associated with rotation and low availability of ephemeral solutions. Detecting Long-Lived Secrets is easy given the majority of secret managers enable users to see the amount of time that has passed since rotation. | Secrets tend to hold credentials for high-impact NHI (such as API Keys and Database connection strings).|


## 説明

Long-lived Secrets refers to the use of sensitive NHIs such as API keys, tokens, encryption keys, and certificates with expiration dates that are too far in the future or that don’t expire at all. Developers frequently use these secrets to enable applications to authenticate and interact with various services and resources within an organization. Oftentimes, these secrets can be breached or leak (see [Secret Leakage](https://owasp.org/www-project-non-human-identities-top-10/2025/2-secret-leakage/)). If a breached secret is long-lived, it provides attackers with access to sensitive services without any time constraints.

## 攻撃シナリオの例

* **Privilege Escalation via Stale Sensitive Access Token:** An attacker with low-level privileges in the corporate network identifies a year-old data dump. The dump contains a sensitive Access Token with admin privileges. The attacker leverages the sensitive Access Token to raise privileges in the network.
* **Session Hijacking via Stolen Long-Lived Cookies:** A web session cookie is set to be long-lived. An infostealer campaign dumps cookies from one browser in the corporate network. The infostealer then sells that cookie to an attacker who leverages the session cookie to breach the corporate network.

## 防御方法

* **Enable Automated Key Rotation:** Automating the rotation of API keys or credentials using cloud-native tools or simple scripts reduces manual effort and ensures credentials are not long-lived.
* **Implement Short-Lived Credentials:** Many cloud platforms like AWS and Azure provide built-in mechanisms to use temporary credentials that automatically expire and refresh after performing the task they made for. 
* **Adopt Zero Trust Principles:** Require re-authentication for NHIs accessing sensitive resources or performing high-risk actions.
* **Enforce Principle of Least Privilege:** Grant only the minimum permissions necessary for the NHI to perform its tasks, reducing the impact of credential compromise.

## 参考情報
* Rabbit Inc. API Key Leak (June 2024) - [link](https://www.doppler.com/blog/updated-data-breaches-caused-by-leaks-in-2024)
* Hugging Face Space Secrets Leak Disclosure (May 2024) - [link](https://huggingface.co/blog/space-secrets-disclosure)
* Snowstorm surrounding the recent Snowflake “hack” (May 2024) - [link](https://medium.com/@ronilichtman/snowstorm-surrounding-the-recent-snowflake-hack-ab7e51e0c5be)
* Employee Personal GitHub Repos Expose Internal Azure and Red Hat Secrets (May 2024) - [link](https://www.aquasec.com/blog/github-repos-expose-azure-and-red-hat-secrets/)
* Microsoft SAS Token Breach (September 2023) - [link](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
* CircleCI Breach (January 2023) - [link](https://circleci.com/blog/jan-4-2023-incident-report/)
* Microsoft Azure Site Recovery Privilege Escalation (July 2022) - [link](https://www.tenable.com/security/research/tra-2022-26)


## データポイント
* [Datadog State of the Cloud 2024](https://www.datadoghq.com/state-of-cloud-security/)
    * 46% of AWS orgs users use long-lived console credentials
    * 60% of keys across cloud providers have age > 1 year
* [CSA NHI Report](hhttps://cloudsecurityalliance.org/artifacts/state-of-non-human-identity-security-survey-report) 
    * 45% of times lack of credential rotation were the cause for NHI-related security incidents (1/10)
    * 26% of organizations need management of secrets lifecycle as the most important capability of an NHI tool (1/16)
    * 51% of organizations have no formal process to offboard or revoke long-lived API keys
* [Orca Security State of the Cloud Security report 2022](https://orca.security/wp-content/uploads/2022/09/2022-State-of-Public-Cloud-Security-Report.pdf)
    * 80% of organizations have KMS rotation disabled
    * 79% of organizations have at least one access key older than 90 days
