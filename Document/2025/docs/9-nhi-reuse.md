# NHI9:2025 NHI の再使用 (NHI Reuse)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **困難**           | 蔓延度: **広範**<br>検出可能性: **困難** | 技術的影響: **低い**<br>ビジネスへの影響: **限定** |
| 再使用された NHI の悪用に成功するには、脅威エージェントがまず環境にアクセスする必要があります。したがって、NHI の再使用は別の初期アクセスベクトルに依存します。 | ワークロードごとに NHI を合わせることは困難であるため、NHI は非常によく再使用されます。よくあるケースとしては、複数のワークロードに単一の AWS IAM ロールを使用したり、複数のワークロードに単一の API キーを使用するものがあります。NHI を使用するワークロードは多岐にわたるため、NHI の再使用を検出するには困難です。 | NHI の再使用の影響は、関連する NHI の権限に依存します。最小権限が採用される場合、この影響は低くなります。 |

## 説明

Non-Human Identities (NHIs), such as service accounts, API keys, and machine credentials, play a critical role in enabling applications and services to authenticate and access necessary resources. However, reusing the same NHI across different applications, services, or components—even if they are deployed together—introduces significant security risks. If an NHI is compromised in one area, an attacker can exploit it to gain unauthorized access to other parts of the system that use the same credentials. 

In addition to these risks, the reuse of NHIs can complicate breach mitigation actions and impact audits.   

To minimize these risks, it's essential to assign unique NHIs to each application or service. This approach adheres to the principle of least privilege, ensuring that each NHI has only the permissions necessary for its specific function. By isolating NHIs, organizations can contain potential breaches and prevent attackers from moving laterally within the environment.

## 攻撃シナリオの例

* **Kubernetes Service Account Reuse**: In a Kubernetes cluster, multiple pods can share the same Kubernetes service account, including critical pods responsible for orchestration tasks. If one pod has a vulnerability and gets compromised, an attacker can use the shared service account to perform actions across the cluster. This could lead to unauthorized access to sensitive data, manipulation of workloads, or even full control over the cluster's resources.
* **Shared API Keys Between Applications**: An organization uses the same API key for multiple applications to access a third-party service. If one application is compromised and the API key is exposed, the attacker can use it to access or manipulate data across all applications that use the shared key, potentially leading to a widespread breach.
* **Reused Cloud Credentials**: Different services within an organization utilize the same cloud credentials (e.g., AWS IAM roles, Azure service principals) to interact with cloud resources. If an attacker obtains these credentials from a less secure service, they can access critical resources used by more secure services, bypassing isolation mechanisms and escalating the attack.

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
