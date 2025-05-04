# NHI8:2024 環境分離のNHI (Environment Isolation NHI)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **普通**           | 蔓延度: **まれ**<br>検出可能性: **困難** | 技術的影響: **普通**<br>ビジネスへの影響: **限定** |
| 分離されていない NHI の悪用に成功するには、脅威エージェントがまずテスト環境にアクセスする必要があります。とはいえ、テスト環境は本番環境と比べて保護が大幅に不足しがちです。 | 非本番環境と本番環境の間で NHI を再利用することはあまりありません。同じ NHI を使用する可能性のあるワークロードと環境の組み合わせは多岐にわたるため、分離されていない NHI を検出することは困難です。 | 分離された NHI の影響は、関連する NHI の権限に依存します。関連する NHI は、本来、本番環境にあるため、その影響は無視できません。 |


## 説明

環境分離はクラウドアプリケーションのデプロイメントにおける基本的なセキュリティプラクティスであり、開発、テスト、ステージング、本番に別々の環境を使用します。この分離は、ある環境で発生した問題が他の環境、特に実際のユーザーや機密データが存在する本番環境に影響を及ぼさないことを確保します。
サービスアカウント、API キー、ロールなどの非人間アイデンティティ (NHI) は、デプロイメントプロセス時やアプリケーションのライフサイクル全体を通じてしばしば利用されます。しかし、複数の環境、特にテスト環境と本番環境の間で同じ NHI を再使用すると、重大なセキュリティ脆弱性が生じる可能性があります。安全性の低いテスト環境で使用される NHI が本番環境のリソースにアクセスするパーミッションを持つ場合、テスト環境を侵害した攻撃者がこの NHI を活用して本番環境に侵入する可能性があります。
これらのリスクを緩和するには、NHI が動作する環境に基づいて NHI を厳密に分離することが重要です。これには以下があります。


* **環境ごとに個別の NHI を使用すること:** 環境ごとに一意の NHI を割り当て、環境をまたいだアクセスを防ぎます。
* **最小権限の原則を適用すること:** NHI のパーミッションを特定の環境に必要なものだけに制限します。
* **環境固有のアクセス制御を実施すること:** 非本番環境の NHI が本番環境のリソースにアクセスできないようにします。
* **定期的に監査および監視すること:** NHI を継続的に監視して、不正アクセスの試みや異常がないか確認します。

環境とそれに関連する NHI を分離することで、組織は攻撃対象領域を大幅に削減し、潜在的な侵害が環境全体に伝播することを防止できます。

## 攻撃シナリオの例

* **Shared AWS Access Keys Across Environments:** An AWS access key is used in both testing and production environments to access Amazon S3 buckets. While intended for accessing mock data in the testing environment, the key also has permissions to access sensitive production data. If the testing environment is compromised, an attacker could use the shared access key to retrieve or manipulate production data, leading to data breaches or service disruptions.
* **Shared Azure System-assigned Managed Identity across subscriptions:** A system-assigned managed identity allows resources to use it both in a test subscription and a production subscription and has permissions to resources in both subscriptions. This configuration allows processes in the testing environment to access resources in the production environment. If an attacker gains access to the testing environment, they could exploit the managed identity gaining unauthorized access to critical resources and potentially compromising the entire production environment.


## 防御方法
* **Strict Environment Isolation for NHIs:** Assign unique NHIs to each environment (development, testing, staging, production) to ensure that access credentials or identities in one environment cannot be reused in another.
* **Apply the Principle of Least Privilege (PoLP):** Grant NHIs only the minimal permissions required for their specific tasks within their designated environments. This minimizes the potential damage if an NHI is compromised.
* **Enforce Environment-Specific Access Controls:** Configure access policies so that NHIs in non-production environments (e.g., testing) cannot interact with or access resources in the production environment.
* **Segregate Infrastructure for Sensitive Resources:** Use separate resource groups, subscriptions, or accounts to isolate production from non-production environments. This ensures that even with an NHI compromise, the blast radius is limited to its environment.


## 参考情報
* [AWS Recommendations on Workload Isolation](https://aws.amazon.com/solutions/guidance/workload-isolation-on-aws/)

## データポイント
* **CSA NHI Report** - 32% of times configuration error was the cause for NHI-related security incidents. (4/10)
