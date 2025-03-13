# NHI6:2025 安全でないクラウドデプロイメントの設定 (Insecure Cloud Deployment Configurations)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **普通**           | 蔓延度: **普通**<br>検出可能性: **容易** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 一般的に、誤って設定されたパイプラインは組織の範囲内でセットアップされているため、発見するのは困難です。しかし、脅威アクターがシンプルな読み取りアクセスを獲得すると、比較的簡単に環境を偵察し、脆弱な設定を発見できます。 | CI/CD パイプラインの管理におけるリスクが認識されるようになり、その結果、多くの CI/CD プロバイダが OIDC ベースのアクセスをサポートし、ユーザーにそれを使用するよう促しています。しかし、多くの組織は依然としてキャッチアップする必要があり、ハードコードされたクレデンシャルや安全でない OIDC ベースの認証を使用しています。 <br> CI/CD の設定ミスは組織の「本拠地」で発生するため、簡単に検索できます。さらに、現在知られている設定ミスは詳細に文書化されています。 | CI/CD の設定ミスの侵害に成功すると、ほとんどのパイプラインには高い権限のアクセスが付与されているため、サプライチェーン攻撃や環境への不正アクセスにつながる可能性があります。 |

## 説明

Continuous Integration and Continuous Deployment (CI/CD) applications enable developers to automate the process of building, testing, and deploying code to production environments. These integrations often require the CI/CD pipelines to authenticate with cloud services, which is typically achieved using either dedicated service accounts with static credentials or OpenID Connect (OIDC) for federated identity management.

Using dedicated service accounts with static credentials in CI/CD pipelines is considered insecure. Static credentials can be inadvertently exposed through code repositories, logs, or configuration files. If compromised, these credentials can provide attackers with persistent and potentially privileged access to production environments, bypassing multi-factor authentication (MFA) and other security measures.

OIDC offers a more secure alternative by allowing CI/CD pipelines to obtain short-lived, dynamically generated tokens for authentication. However, misconfigurations in OIDC setups can introduce vulnerabilities. If the identity tokens are not properly validated or there are no strict conditions on token claims—such as the `sub` (subject) claim—unauthorized users might exploit these weaknesses to gain access to cloud resources.

Proper configuration and management of CI/CD integrations are crucial to maintain the security of the production environment. This includes enforcing the principle of least privilege, implementing robust credential management practices, and ensuring that OIDC tokens are properly validated and restricted to authorized entities.


## 攻撃シナリオの例

* **AWS IAM Roles with Misconfigured OIDC Trust Relationships**: AWS roles that allow OIDC access through `AssumeRoleWithWebIdentity` can be misconfigured if they trust public OIDC providers like GitHub or GitLab without properly restricting the `sub` claim. Without this restriction, any user on these platforms could potentially assume the role, leading to unauthorized access to AWS resources.

* **Hard-Coded Azure Service Principal Credentials**: An Azure Service Principal intended for use within a GitHub Action might have its credentials hard-coded into the pipeline's configuration files. If these files are stored in a publicly accessible repository or are otherwise exposed, attackers can obtain the credentials and authenticate as the service principal, gaining access to Azure resources with the associated permissions.

## 防御方法
* **Use OIDC for Secure Authentication**
  - Replace static credentials with short-lived, dynamically generated tokens using OIDC.
  - Validate tokens strictly, including issuer, audience, and claims.
* **Enforce Least Privilege**
  - Limit CI/CD pipeline permissions to only what is necessary.
  - Restrict trust relationships in IAM roles and OIDC configurations.
* **Avoid Hard-Coded Credentials**
  - Store secrets securely with tools like AWS Secrets Manager, Azure Key Vault, or HashiCorp Vault.
  - Regularly scan repositories and configurations for exposed credentials.

## 参考情報

* [Exploiting Misconfigured GitLab OIDC AWS IAM Roles](https://hackingthe.cloud/aws/exploitation/Misconfigured_Resource-Based_Policies/exploiting_misconfigured_gitlab_oidc_aws_iam_roles/)

* [Security Recommendations for AWS Access in GitHub Actions](https://github.com/aws-actions/configure-aws-credentials#security-recommendations)



## データポイント

* **CSA NHI Report** - 32% of times configuration error was the cause for NHI-related security incidents. (4/10)
