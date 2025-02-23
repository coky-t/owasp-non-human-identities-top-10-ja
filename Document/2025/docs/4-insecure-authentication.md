# NHI4:2025 安全でない認証 (Insecure Authentication)

| 脅威エージェントと攻撃ベクトル | セキュリティ上の弱点                     | 影響度                                             |
|--------------------------------|------------------------------------------|----------------------------------------------------|
| 悪用可能性: **容易**           | 蔓延度: **広範**<br>検出可能性: **容易** | 技術的影響: **普通**<br>ビジネスへの影響: **限定** |
| 攻撃者は、安全でない認証を使用する NHI を検出すると、既存の技法やツールを使用して NHI を悪用し、侵害できます。 | レガシーアプリケーションはほぼすべての認証に存在し、通常は暗黙的な OAuth フローや MFA のないサービスアカウントなどのレガシーや安全でない認証方式を使用します。 <br> 安全でない認証のタイプに応じて、検出可能性は利用可能な単純な検出機能から、識別が困難な特定の安全でない認証違反者までさまざまです。 | 安全でないプロトコルは高いアクセスを付与された機密性の高いプロセスを容易にするためによく使用されます。安全でない認証を使用する NHI の悪用に成功すると、アカウントの乗っ取りや権限昇格につながる可能性があります。 |

## 説明
開発者は SaaS アプリケーションやクラウド環境に内部や外部 (サードパーティ) のサービスを統合して、エクスペリエンスを向上したり、操作を容易にすることがよくあります。これらのサービスはシステム内のリソースにアクセスする必要があり、認証クレデンシャルが必要になります。さまざまなプラットフォームで複数の認証方式を利用できるため、開発者は特定のユースケースに最も安全で適切なオプションを慎重に選択しなければなりません。
しかし、一部の認証方式は非推奨であったり、既知の攻撃に対して脆弱であったり、時代遅れのセキュリティプラクティスのために脆弱であるとみなされていたりします。安全でなかったり、時代遅れの認証メカニズムを利用することは、組織を不正アクセス、データ侵害、コンプライアンス違反などの重大なリスクにさらす可能性があります。開発者や組織は、利用可能な認証オプションを評価し、業界のベストプラクティスに準拠し、堅牢なセキュリティ機能を提供して OAuth 2.1 や OpenID Connect (OIDC) などの標準化されたプロトコルに準拠する方式を選択することが不可欠です。


## 攻撃シナリオの例
- **非推奨の OAuth フロー:** 以前の OAuth バージョン (OAuth 1.0 および OAuth 2.0) の特定のフローはセキュリティ脆弱性のために非推奨となりました。たとえば、以下のものです。
    - **暗黙的フロー:** シングルページアプリケーションでよく使用されますが、URL にアクセストークンを開示して、傍受やレプレイ攻撃を受けやすくなるため、現在は推奨されていません。
    - **PKCE を使用しない認可コードフロー:** 傍受やクロスサイトリクエストフォージェリ (CSRF) 攻撃に脆弱です。最新の実装では Proof Key for Code Exchange (PKCE) 拡張を使用してセキュリティを強化すべきです。
- **非標準の OAuth 実装:** 一部のプラットフォームでは、アクセストークンを Cookie に変換したり、オンデマンドで JSON Web Token (JWT) を生成するなど、カスタム動作を実装することで公式の OAuth 標準から逸脱しています。このような非標準のものは、公式仕様で説明されているセキュリティ上の考慮事項を欠いている可能性があるため、予期しない脆弱性をもたらし、セキュリティ侵害につながる可能性があります。
- **クレデンシャルレス方式ではなくクレデンシャルベース認証の使用:** クラウドプロバイダは、インスタンスプロファイルや OIDC フェデレーションを使用したクラウド内アクセスなど、クレデンシャルレス認証メカニズムを提供しています。静的でクレデンシャルベースの認証 (長期間有効な API キーやパスワードなど) に依存することは、これらのクレデンシャルが侵害、コードリポジトリ、ログで開示される可能性があるため、推奨されていません。クレデンシャルレス方式は一時的なスコープ付きクレデンシャルを提供して、クレデンシャルの漏洩や誤用を軽減します。
- **MFA をバイパスするアプリパスワード:** Microsoft や Google などのプラットフォームはアプリ固有のパスワードを提供して、最新の認証プロトコルをサポートしていない古いアプリケーションをサポートしています。これらのパスワードは MFA をバイパスします。つまり、ユーザーが MFA を有効にしていても、アプリパスワードを使用して追加検証なしでアカウントにアクセスできます。アプリパスワードを入手した攻撃者はこれを悪用して不正アクセスを行い、MFA のセキュリティ上の利点を事実上無効にし、ユーザーアカウントを安全でないサービスアカウントに変換できます。
- **ユーザー名とパスワードを使用するレガシー認証プロトコル:** 一部のアプリケーションでは、ユーザー名とパスワードの直接送信に依存する古い認証フローや独自の認証フローを使い続け、セキュリティ標準に準拠せずに OAuth のような動作を模倣しています。これらの方法には公式の OAuth フローで提供される保護がないため、クレデンシャルの傍受、リプレイ攻撃、中間者攻撃の影響を受けやすくなります。


## 防御方法
- **最新の認証標準を採用する:** 安全な認証には OAuth 2.1 と OIDC を使用し、暗黙的フローや PKCE を使用しない認可コードフローなどの非推奨のフローを避けます。
- **クレデンシャルレス方式を活用する:** 静的クレデンシャルを、インスタンスプロファイルまたは OIDC フェデレーションを介した一時的なスコープ付きトークンに置き換えます。
- **OAuth 実装を標準化する:** OAuth 標準から逸脱するカスタムプラクティスを避け、セキュリティギャップを最小限に抑えます。
- **定期的なセキュリティ監査を実施する:** 認証方式を定期的に見直し、非推奨な構成や安全でない構成を特定して排除します。

## 関連する OWASP リソース
* [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

## 参考情報
- [Salesforce: Disabling Insecure Authorization Flows](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_disable_username_password_flow.htm&type=5)
- [OAuth 2.0 Security Best Current Practice - Implicit Grant](https://datatracker.ietf.org/doc/html/draft-ietf-oauth-security-topics#name-implicit-grant)
- [Salesforce: Username-Password OAuth Flow](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_username_password_flow.htm&language=en_US&type=5)
- [Auth0: Implicit Flow with Form Post](https://auth0.com/docs/get-started/authentication-and-authorization-flow/implicit-flow-with-form-post)
- [Microsoft Support: Using App Passwords with Apps that Don't Support Two-Step Verification](https://support.microsoft.com/en-us/account-billing/using-app-passwords-with-apps-that-don-t-support-two-step-verification-5896ed9b-4263-e681-128a-a6f2979a7944)
- [Google Account Help: Sign in Using App Passwords](https://support.google.com/accounts/answer/185833?hl=en)

## データポイント
- CSA NHI Report - 22% の回答者が非推奨のアクセス方式を最も懸念される NHI 脅威のトップ 3 の一つに挙げています。 (8/10)
- 最近の侵害 - [MSFT SAS トークンの侵害](https://www.wiz.io/blog/38-terabytes-of-private-data-accidentally-exposed-by-microsoft-ai-researchers)
- 最近の侵害 - [Uber の侵害](https://www.upguard.com/blog/what-caused-the-uber-data-breach)
- 最近の侵害 - [CircleCI の侵害](https://circleci.com/blog/jan-4-2023-incident-report/)
- 最近の侵害 - [Cloudflare の侵害](https://medium.com/@ronilichtman/how-cloudflare-got-hoktad-part-one-d5bb75dac3f0)
- 最近の侵害 - [Snowflake の侵害](https://medium.com/@ronilichtman/snowstorm-surrounding-the-recent-snowflake-hack-ab7e51e0c5be)
- 最近の侵害 - [.env file の侵害](https://medium.com/@ronilichtman/large-scale-extortion-via-secrets-in-env-files-why-secret-vaults-just-arent-enough-9b4c568724ca)
