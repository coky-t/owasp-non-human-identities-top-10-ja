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
 - 攻撃者による隠蔽


## 攻撃シナリオの例
- **サービスアカウントのクレデンシャルを使用する管理者:** IT 管理者は、利便性のためにサービスアカウントのクレデンシャルを使用して、クラウド管理コンソールにログインします。このサービスアカウントは自動デプロイメントタスクのための広範なパーミッションを有しています。管理者は自分の役割の要件を超えるアクセスを持つようになり、実行するすべてのアクションはサービスアカウントの元にログ記録されるため、説明責任が不明瞭になります。
- **NHI でコマンドを実行する開発者:** 開発者は、本番環境へのパーミッションを持つ NHI を使用して、スクリプトやコマンドを手動で実行します。開発者がエラーを起こしたり、認可されていない変更を実行すると、そのアクションが NHI に関連付けられてログ記録されるため、そのアクティビティを追跡することが困難になります。
- **チームメンバー間で共有される API トークン:** チームは、特定のリソースに素早くアクセスするために、サービスアカウントに関連付けられた API トークンを共有します。このトークンには広範なアクセス権限があります。チームメンバーの環境が侵害された場合、攻撃者は共有トークンを使用して機密システムにアクセスでき、侵害の原因を特定することが困難になります。
- **セキュリティコントロールのバイパス:** 従業員が NHI を使用して、MFA 要件や IP アドレス制限などのポリシーによりユーザーアカウントでは制限されているリソースにアクセスします。これは組織のセキュリティ態勢を損ない、不正なデータアクセスにつながる可能性があります。
- **永続的に NHI を活用する攻撃者:** 環境を侵害した後、攻撃者は NHI クレデンシャルを取得し、それを使用してアクセスを維持します。NHI は定期的なパスワード変更や MFA の対象ではないため、攻撃者は長期間にわたって検知されずに環境にとどまることができます。

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
