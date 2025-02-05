# NHI1:2025 不適切なオブボーディング (Improper Offboarding)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **容易**           | 蔓延度: **広範**<br>検出可能性: **困難** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 不適切にオフボードされた NHI を悪用するには状況に大きく依存します。内部脅威のケースを考えると、不適切にオフボードされたアイデンティティを悪用するために必要なクレデンシャルを特定することは非常に簡単です。 | サービスアカウントなどの NHI をオフボーディングする現在の機能は不十分であり、組織は既存の手段を使用することはほとんどありません。そのため、多くの NHI は不要になった後や元の所有者が退職した後でも適切にオフボードされません。 <br> セキュリティチームには適切にオフボードされなかった古い NHI を検出するためのツールがありません。そのような NHI を検出する既存の技法は、コンパイルに長い時間がかかる不完全な情報に依存しています。 | 潜在的な内部脅威は組織についての深い洞察を持つため、不適切にオフボードされた NHI を悪用すると、重要なシステムの侵害、機密データの流出、高度な永続化手法の使用につながる可能性があります。 |

## 説明
不適切なオブボーディングとは、不要になったサービスアカウントやアクセスキーなどの非人間アイデンティティ (NHI) の不適切な非アクティブ化や削除を指します。この状況は、アプリケーションが廃止されたとき、サービスがオフラインになったとき、またはこれらの NHI の元の所有者や管理者が組織を離れたときによく発生します。NHI を適切にオフボードしないと、重大なセキュリティリスクをもたらします。監視されていないサービスや廃止されたサービスは脆弱なままになることがあり、関連する NHI が攻撃者に悪用され、機密性の高いシステムやデータに不正アクセスされる可能性があります。さらに、管理されていない NHI は高い権限を保持する可能性があり、セキュリティ侵害による潜在的な損害を増幅します。

## 攻撃シナリオの例
- **管理されていない Kubernetes サービスアカウント:** 廃止されたサービスに属する Kubernetes クラスタはアクティブなサービスアカウントを持ち続けます。攻撃者がこの監視されていないクラスタにアクセスした場合、これらのサービスアカウントを悪用して組織のインフラストラクチャ内の他のリソースとやり取りし、データ流出やさらなる侵害につながる可能性があります。
- **失効していないクレデンシャルを悪用する元従業員:** 自動サービスを管理していた従業員が組織を去りますが、それらのサービスに関連付けられた NHI は無効化や移管されていません。元従業員は依然として有効なクレデンシャルを悪用して組織のシステムにリモートアクセスし、不正なデータアクセス、サービス中断、さらには妨害行為につながる可能性があります。
- **権限昇格とラテラルムーブメントに使用される残存アプリ:** ワークロードをテストするように設計されたテスト環境で作成されたアプリケーションは、後に機密性の高い本番環境に接続されてテストスイートを完了しますが、ワークロードが移管されて本番サーバーで実行しても廃止されません。その後、安全でないテスト環境に到達した攻撃はこのアプリケーションを使用して組織内でラテラルに移動できます。

## 防御方法
- Implement an offboarding process that reviews all NHIs associated with the departing employee. For each NHI, determine if it is still required. If not, decommission it; otherwise, transfer ownership to another employee and rotate any credentials the departing employee may have had access to during its creation.
- Automate offboarding steps wherever possible by integrating HR systems with identity and access management (IAM) tools.
- Regularly audit active NHIs to identify ongoing human usage and block potential misuse.

## 参考情報
- [Cloud Security Alliance: Decommissioning Orphaned and Stale Non-Human Identities](https://cloudsecurityalliance.org/blog/2024/06/03/decommissioning-orphaned-and-stale-non-human-identities)
- [Ex-Employee Accessed Former Company's System and Deleted Resources](https://www.channelnewsasia.com/singapore/former-employee-hack-ncs-delete-virtual-servers-quality-testing-4402141)
- [Microsoft Breach by Midnight Blizzard Threat Actor](https://msrc.microsoft.com/blog/2024/01/microsoft-actions-following-attack-by-nation-state-actor-midnight-blizzard/)

## データポイント
- CSA NHI Report - 31% answers put insufficient NHI offboarding as one of the top 3 most concerning NHI threats. (5/10)
- CSA NHI Report - 32% of times orphaned identities were the cause for NHI-related security incidents. (4/10)
- CSA NHI Report - 15% of organizations need automated provisioning and de-provisioning as the most important capability of an NHI tool. (9/16)
- CSA NHI Report - 51% of organizations have no formal process to offboard or revoke long lived API keys.
- Recent Breach - [Microsoft Breach](https://medium.com/@ronilichtman/how-to-protect-yourself-from-the-microsoft-oauth-attack-powershell-scripts-included-71b398034b8d)
