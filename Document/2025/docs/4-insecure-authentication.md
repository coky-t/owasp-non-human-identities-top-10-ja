# NHI4:2025 安全でない認証 (Insecure Authentication)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **容易**           | 蔓延度: **広範**<br>検出可能性: **容易** | 技術的影響: **普通**<br>ビジネスへの影響: **限定** |
| 攻撃者は、安全でない認証を使用する NHI を検出すると、既存の技法やツールを使用して NHI を悪用し、侵害できます。 | レガシーアプリケーションはほぼすべての認証に存在し、通常は暗黙的な OAuth フローや MFA のないサービスアカウントなどのレガシーや安全でない認証手法を使用します。 <br> 安全でない認証のタイプに応じて、検出可能性は利用可能な単純な検出機能から、識別が困難な特定の安全でない認証違反者までさまざまです。 | 安全でないプロトコルは高いアクセスを付与された機密性の高いプロセスを容易にするためによく使用されます。安全でない認証を使用する NHI の悪用に成功すると、アカウントの乗っ取りや権限昇格につながる可能性があります。 |

## 説明
Developers frequently integrate internal and external (third-party) services into their SaaS applications and cloud environments to enhance their experience or facilitate operation. These services require access to resources within these systems, necessitating authentication credentials. Multiple authentication methods are available across various platforms, and developers must judiciously select the most secure and appropriate option for their specific use case.
However, some authentication methods are deprecated, vulnerable to known attacks, or considered weak due to outdated security practices. Utilizing insecure or obsolete authentication mechanisms can expose organizations to significant risks, including unauthorized access, data breaches, and compliance violations. It is imperative for developers and organizations to evaluate all available authentication options, adhere to industry best practices, and choose methods that provide robust security features and adherence to standardized protocols like OAuth 2.1 and OpenID Connect (OIDC).


## 攻撃シナリオの例
- **Deprecated OAuth Flows:** Certain flows from earlier OAuth versions (OAuth 1.0 and OAuth 2.0) have been deprecated due to security vulnerabilities. For example:
    - **Implicit Flow:** Commonly used for single-page applications, it is now discouraged because it exposes access tokens in the URL, making them susceptible to interception and replay attacks.
    - **Authorization Code Flow without PKCE:** Vulnerable to interception and Cross-Site Request Forgery (CSRF) attacks. Modern implementations should use the Proof Key for Code Exchange (PKCE) extension to enhance security.
- **Non-Standard OAuth Implementations:** Some platforms deviate from official OAuth standards by implementing custom behaviors, such as converting access tokens into cookies or generating JSON Web Tokens (JWTs) on demand. These non-standard practices can introduce unanticipated vulnerabilities, as they may lack the security considerations outlined in the official specifications, potentially leading to security breaches.
- **Use of Credential-Based Authentication over Credential-less Methods:** Cloud providers offer credential-less authentication mechanisms, such as intra-cloud access using instance profiles or OIDC federation. Relying on static, credential-based authentication (like long-lived API keys or passwords) is discouraged because these credentials can be exposed in breaches, code repositories, or logs. Credential-less methods provide temporary, scoped credentials that reduce the risk of credential leakage and misuse.
- **App Passwords Bypassing MFA:** Platforms like Microsoft and Google provide app-specific passwords to support older applications that do not support modern authentication protocols. These passwords bypass MFA, meaning that even if a user has MFA enabled, the app password can be used to access the account without additional verification. Attackers who obtain an app password can exploit this to gain unauthorized access, effectively nullifying the security benefits of MFA and converting the user account into an insecure service account.
- **Legacy Authentication Protocols Using Username and Password:** Some applications continue to use outdated or proprietary authentication flows that rely on direct transmission of usernames and passwords, mimicking<br> OAuth-like behavior without adhering to its security standards. These methods lack the protections offered by official OAuth flows, making them susceptible to credential interception, replay attacks, and man-in-the-middle exploits.


## 防御方法
- **Adopt Modern Authentication Standards:** Use OAuth 2.1 and OIDC for secure authentication and avoid deprecated flows like Implicit Flow or Authorization Code Flow without PKCE.
- **Leverage Credential-less Methods:** Replace static credentials with temporary, scoped tokens through instance profiles or OIDC federation.
- **Standardize OAuth Implementations:** Avoid custom practices that deviate from OAuth standards to minimize security gaps.
- **Conduct Regular Security Audits:** Periodically review authentication methods to identify and eliminate deprecated or insecure configurations.

## Related OWASP Resources
* [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

## 参考情報
- [Salesforce: Disabling Insecure Authorization Flows](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_disable_username_password_flow.htm&type=5)
- [OAuth 2.0 Security Best Current Practice - Implicit Grant](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics#name-implicit-grant)
- [Salesforce: Username-Password OAuth Flow](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_username_password_flow.htm&language=en_US&type=5)
- [Auth0: Implicit Flow with Form Post](https://auth0.com/docs/get-started/authentication-and-authorization-flow/implicit-flow-with-form-post)
- [Microsoft Support: Using App Passwords with Apps that Don't Support Two-Step Verification](https://support.microsoft.com/en-us/account-billing/using-app-passwords-with-apps-that-don-t-support-two-step-verification-5896ed9b-4263-e681-128a-a6f2979a7944)
- [Google Account Help: Sign in Using App Passwords](https://support.google.com/accounts/answer/185833?hl=en)

## データポイント
- CSA NHI Report - 22% answers put deprecated access methods as one of the top 3 most concerning NHI threats. (8/10)
- Recent Breach - [MSFT SAS Token Breach](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
- Recent Breach - [Uber Breach](https://www.upguard.com/blog/what-caused-the-uber-data-breach)
- Recent Breach - [CircleCI Breach](https://circleci.com/blog/jan-4-2023-incident-report/)
- Recent Breach - [Cloudflare Breach](https://medium.com/@ronilichtman/how-cloudflare-got-hoktad-part-one-d5bb75dac3f0)
- Recent Breach - [Snowflake Breach](https://medium.com/@ronilichtman/snowstorm-surrounding-the-recent-snowflake-hack-ab7e51e0c5be)
- Recent Breach - [.env file Breach](https://medium.com/@ronilichtman/large-scale-extortion-via-secrets-in-env-files-why-secret-vaults-just-arent-enough-9b4c568724ca)
