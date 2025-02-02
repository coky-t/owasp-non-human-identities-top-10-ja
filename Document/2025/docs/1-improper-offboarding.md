# NHI1:2025 不適切なオブボーディング (Improper Offboarding)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **容易**           | 蔓延度: **広範**<br>検出可能性: **困難** | 技術的影響: **重大**<br>ビジネスへの影響: **限定** |
| 不適切にオフボードされた NHI を悪用するには状況に大きく依存します。内部脅威のケースを考えると、不適切にオフボードされたアイデンティティを悪用するために必要なクレデンシャルを特定することは非常に簡単です。 | サービスアカウントなどの NHI をオフボーディングする現在の機能は不十分であり、組織は既存の手段を使用することはほとんどありません。そのため、多くの NHI は不要になった後や元の所有者が退職した後でも適切にオフボードされません。 <br> セキュリティチームには適切にオフボードされなかった古い NHI を検出するためのツールがありません。そのような NHI を検出する既存の技法は、コンパイルに長い時間がかかる不完全な情報に依存しています。 | 潜在的な内部脅威は組織についての深い洞察を持つため、不適切にオフボードされた NHI を悪用すると、重要なシステムの侵害、機密データの流出、高度な永続化手法の使用につながる可能性があります。 |

## 説明
Improper offboarding refers to the inadequate deactivation or removal of non-human identities (NHIs) such as service accounts and access keys when they are no longer needed. This situation often arises when applications are deprecated, services are taken offline, or when the original owners or administrators of these NHIs leave the organization. Failure to properly offboard NHIs poses significant security risks. Unmonitored and deprecated services may remain vulnerable, and their associated NHIs can be exploited by attackers to gain unauthorized access to sensitive systems and data. Additionally, orphaned NHIs may retain elevated permissions, amplifying the potential damage from any security breach.

## 攻撃シナリオの例
- **Orphaned Kubernetes Service Accounts:** A Kubernetes cluster belonging to a decommissioned service retains active service accounts. If an attacker gains access to this unmonitored cluster, they could exploit these service accounts to interact with other resources within the organization's infrastructure, potentially leading to data exfiltration or further compromise.
- **Ex-Employee Exploiting Unrevoked Credentials:** An employee who managed automated services leaves the organization, but the NHIs associated with those services are not disabled or transferred. The ex-employee could misuse still-valid credentials to access the organization's systems remotely, leading to unauthorized data access, service disruptions, or even sabotage.
- **Leftover Apps Used for Privilege Escalation & Lateral Movement:** An application created in a test environment, designated to test a workload, is later connected to a sensitive production environment to complete the testing suite, and is not decommissioned once the workload is transferred to run in a production server. Then, an attack reaching the less secure test environment can use this application to move laterally within the organization.

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
