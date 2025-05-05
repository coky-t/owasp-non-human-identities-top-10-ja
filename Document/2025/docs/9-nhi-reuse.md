# NHI9:2025 NHI の再使用 (NHI Reuse)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **困難**           | 蔓延度: **広範**<br>検出可能性: **困難** | 技術的影響: **低い**<br>ビジネスへの影響: **限定** |
| 再使用された NHI の悪用に成功するには、脅威エージェントがまず環境にアクセスする必要があります。したがって、NHI の再使用は別の初期アクセスベクトルに依存します。 | ワークロードごとに NHI を合わせることは困難であるため、NHI は非常によく再使用されます。よくあるケースとしては、複数のワークロードに単一の AWS IAM ロールを使用したり、複数のワークロードに単一の API キーを使用するものがあります。NHI を使用するワークロードは多岐にわたるため、NHI の再使用を検出するには困難です。 | NHI の再使用の影響は、関連する NHI の権限に依存します。最小権限が採用される場合、この影響は低くなります。 |

## 説明

サービスアカウント、API キー、マシンクレデンシャルなどの非人間アイデンティティ (NHI) は、アプリケーションやサービスが必要なリソースを認証してアクセスできるようにする上で重要な役割を果たします。しかし、異なるアプリケーション、サービス、コンポーネント間で同じ NHI を再使用すると、たとえそれらが一緒にデプロイされるとしても、重大なセキュリティリスクをもたらします。ある領域で NHI が侵害されると、攻撃者はそれを悪用して、同じクレデンシャルを使用するシステムの他の部分に不正にアクセスする可能性があります。

これらのリスクに加えて、NHI の再使用は侵害の緩和措置を複雑にし、監査に影響を及ぼす可能性があります。

これらのリスクを最小限に抑えるには、アプリケーションやサービスごとに固有の NHI を割り当てることが不可欠です。このアプローチは最小権限の原則を遵守し、NHI ごとに特定の機能に必要なパーミッションのみを持つように確保します。NHI を分離することで、組織は潜在的な侵害を封じ込め、攻撃者が環境内でラテラルムーブすることを防止できます。

## 攻撃シナリオの例

* **Kubernetes サービスアカウントの再使用**: Kubernetes クラスタでは、オーケストラタスクを担う重要なポッドを含む、複数のポッドが同じ Kubernetes サービスアカウントを共有できます。一つのポッドに脆弱性があり、侵害された場合、攻撃者は共有サービスアカウントを使用してクラスタ全体でアクションを実行できます。これにより、機密データへの不正アクセス、ワークロードの操作、クラスタのリソースの完全な制御につながる可能性があります。
* **アプリケーション間での共有される API キー**: サードパーティサービスにアクセスするために、組織は複数のアプリケーションで同じ API キーを使用しています。あるアプリケーションが侵害され、API キーが漏洩すると、攻撃者はそのキーを使用して、共有キーを使用するすべてのアプリケーションのデータにアクセスしたり操作することができ、広範な侵害につながる可能性があります。
* **再使用されるクラウドクレデンシャル**: 組織内のさまざまなサービスが、同じクラウドクレデンシャル (AWS IAM ロール、Azure サービスプリンシパルなど) を利用して、クラウドリソースとやり取りします。攻撃者は安全性の低いサービスからこれらのクレデンシャルを入手すると、より安全性の高いサービスが使用する重要なリソースにアクセスでき、分離メカニズムをバイパスして攻撃をエスカレートできます。

## 防御方法

* **Assign Unique NHIs to Each Application or Service**
   - Ensure that each logical system component is granted a unique NHI which it operates under
   - Ensure that the ability to authenticate as the assigned NHI is limited to the associated system component
* **Assign Unique NHIs in Each Environment**
   - Ensure that each logical environment uses distinct NHIs when interacting with other systems or environments
* **Enforce the Principle of Least Privilege**
   - Ensure that NHIs are only granted the minimal level of access required for their function
* **Audit and Review the Use of NHIs**
   - Audit all NHIs along with their access grants and assigned system components
   - Review, on a regular basis, that NHIs are not reused and that they continue to follow the principle of least privilege for their function

## 参考情報

* [Kubernetes: Managing Service Accounts](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/)
* [OWASP: Principle of Least Privilege](https://owasp.org/www-community/Access_Control)
* [AWS Identity and Access Management Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
* [Azure Service Principal Security](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal)
* [Google Cloud: Best Practices for Managing Service Accounts](https://cloud.google.com/iam/docs/best-practices-service-accounts)
* [Chronicle cross-customer bucket access](https://cloud.google.com/support/bulletins#gcp-2023-028)

## データポイント

- **Cloud Vulnerability Database** \- Chronicle cross-customer bucket access
- **CSA NHI Report** \- 14% of organizations need consumer identification as the most important capability of an NHI tool. (11/16)
- **Recent Breach** \- .env file Breach \- [link](https://medium.com/@ronilichtman/large-scale-extortion-via-secrets-in-env-files-why-secret-vaults-just-arent-enough-9b4c568724ca)
