# NHI10:2025 NHI の人間による使用 (Human Use of NHI)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **困難**           | 蔓延度: **普通**<br>検出可能性: **困難** | 技術的影響: **低い**<br>ビジネスへの影響: **限定** |
| NHI の人間による使用を悪用することを成功するには、脅威エージェントがまず環境にアクセスする必要があります。したがって、NHI の人間による使用の攻撃は別の初期アクセスベクトルに依存します。 | 開発者は、問題をデバッグするためにサービスアカウントになりすますことがよくあります。 <br> ほとんどの NHI プロバイダは、NHI を想定したワークロードと NHI を想定した人間とを区別するためのツールを提供していません。 | NHI の人間による使用の影響は、関連する NHI の権限に依存します。 <br> 最小権限が採用されている場合、この影響は低くなります。 |

## 説明
サービスアカウント、API トークン、ワークロードアイデンティティなどの非人間アイデンティティ (NHI) は、クラウドリソースやサービスへのプログラムによるアクセスのために設計されています。これにより、アプリケーション、サービス、自動プロセスが、人間の介入なしに安全に機能するようになります。しかし、アプリケーションの開発や保守において、開発者や管理者がこれらの NHI を、適切な権限を持つ個々の人間のアイデンティティを使用して実行すべき手動タスクに悪用してしまう可能性があります。
このプラクティスは重大なセキュリティリスクをもたらします。
- 必要以上に昇格した権限
 - 詳細な監査と説明責任の欠如
 - 人間と自動の区別がつかないアクティビティ
 - 攻撃者による難読化


## 攻撃シナリオの例
- **Administrators Using Service Account Credentials:**  An IT administrator uses a service account's credentials to log into cloud management consoles for convenience. This service account has extensive permissions intended for automated deployment tasks. The administrator now has access beyond their role's requirements, and any actions they perform are logged under the service account, obscuring accountability.
- **Developers Executing Commands with NHIs:** A developer manually runs scripts or commands using an NHI that has permissions to production environments. If the developer makes an error or performs unauthorized changes, it becomes difficult to trace the activity back to them, as logs attribute the actions to the NHI.
- **Shared API Tokens Among Team Members:** A team shares an API token associated with a service account to access certain resources quickly. This token has broad access rights. If any team member's environment is compromised, the attacker can use the shared token to access sensitive systems, and it would be challenging to identify the source of the breach.
- **Bypassing Security Controls:** An employee uses an NHI to access resources that are restricted under their user account due to policies like MFA requirements or IP address restrictions. This undermines the organization's security posture and can lead to unauthorized data access.
- **Attackers Leveraging NHIs for Persistence:** After compromising an environment, an attacker obtains NHI credentials and uses them to maintain access. Since NHIs are not subject to regular password changes or MFA, the attacker can persist in the environment undetected for extended periods.

## 防御方法
- **Use dedicated identities:** Use dedicated human identities with appropriate roles and permissions for debugging or maintenance tasks.
- **Audit and Monitor NHI Activity:** Use tools or platforms that support auditing and tracking of NHI usage, making human use detectable and accountable.
- **Use Context-Aware Access Controls:** Use conditional access policies that detect and block human access to NHIs based on suspicious patterns.
- **Educate Developers and Administrators:** Provide training on the risks of using NHIs manually and provide alternative access methods to ensure secure access practices.

## 参考情報
- [Microsoft Azure: Best Practices for Securing Service Accounts](https://docs.microsoft.com/en-us/azure/security/fundamentals/service-accounts)
- [AWS Security Blog: Best Practices for Managing AWS Access Keys](https://aws.amazon.com/blogs/security/best-practices-for-managing-aws-access-keys/)
- [Google Cloud: Best practices for managing service account keys](https://cloud.google.com/iam/docs/best-practices-for-managing-service-account-keys)


## データポイント
- [Anetac Report: 75 Percent of Organizations Misuse Service Accounts, Leading to Critical Security Risks](https://cioinfluence.com/security/anetac-report-75-percent-of-organizations-misuse-service-accounts-leading-to-critical-security-risks/)
- CSA NHI Report - 32% of organizations put service accounts as most challenging to manage. (1/16)
- CSA NHI Report - 26% of organizations believe that over 50% of their service accounts are over-privileged
