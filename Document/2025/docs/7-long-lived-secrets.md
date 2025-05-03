# NHI7:2025 長期間有効なシークレット (Long-Lived Secrets)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **困難**           | 蔓延度: **広範**<br>検出可能性: **容易** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 長期間有効なシークレットの悪用を成功するには、脅威エージェントがまずそのシークレット値にアクセスする必要があります。したがって、長期間有効なシークレット攻撃は別の初期アクセスベクトルに依存します。 | 長期間有効なシークレットは現代の環境では極めて一般的です。これは、入れ替えに伴う課題と、一時的なソリューションの可用性の低さに起因しています。大部分のシークレットマネージャは、ユーザーが入れ替え後の経過時間を確認できるため、長期間有効なシークレットの検出は容易です。 | シークレットは影響の大きい NHI (API キーやデータベース接続文字列など) のシークレットを保持する傾向があります。 |


## 説明

長期間有効なシークレットとは、API キー、トークン、暗号鍵、証明書などの機密性の高い NHI のうち、有効期限が長すぎるものか、有効期限がまったくないものの使用を指します。開発者は、アプリケーションが組織内のさまざまなサービスやリソースを認証および操作できるようにするために、これらのシークレットを頻繁に使用します。多くの場合、これらのシークレットが侵害されたり漏洩する可能性があります ([シークレットの漏洩](2-secret-leakage.md) を参照)。侵害されたシークレットが長期間有効であると、攻撃者は時間的な制約なしに機密性の高いサービスにアクセスできます。

## 攻撃シナリオの例

* **古くなった機密アクセストークンによる権限昇格:** 企業ネットワークで低レベルの権限を持つ攻撃者が一年前のデータダンプを発見します。そのダンプは管理者権限を持つ機密アクセストークンを含みます。攻撃者はこの機密アクセストークンを利用して、ネットワーク内で権限を昇格します。
* **盗まれた長期間有効なクッキーによるセッションハイジャック:** ウェブセッションクッキーは長期間有効であるように設定されています。インフォスティーラーキャンペーンでは企業ネットワーク内の一つのブラウザからクッキーをダンプします。それからインフォスティーラーはそのクッキーを攻撃者に販売し、攻撃者はそのセッションクッキーを利用して企業ネットワークを侵害します。

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
