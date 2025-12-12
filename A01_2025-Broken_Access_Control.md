<link rel="stylesheet" href="../../assets/css/RC-stylesheet.css" />

#  A01:2025 アクセス制御の不備 <img src="./assets/TOP_10_Icons_Final_Broken_Access_Control.png" style="height:80px;width:80px" align="right">


## 背景

Top 10 で 1 位を維持し、アプリケーションの 100% で何らかのアクセス制御の不備がテストされました。注目すべき CWE には、*CWE-200: 権限のないアクターによる機密情報の漏洩*、*CWE-201: 送信データを介した機密情報の意図せぬ公開*、*CWE-918: サーバー サイド リクエスト フォージェリ (SSRF)*、*CWE-352: クロスサイト リクエスト フォージェリ (CSRF)* が含まれます。このカテゴリは、提供されたデータの中で最も多くの発生件数を記録し、関連する CVE の数も 2 番目に多くなっています。

## スコア表


<table>
  <tr>
   <td>対応する CWE 数
   </td>
   <td>インシデント割合（最大）
   </td>
   <td>インシデント割合（平均）
   </td>
   <td>カバレッジ（最大）
   </td>
   <td>カバレッジ（平均）
   </td>
   <td>重み付き悪用可能性（最大）
   </td>
   <td>重み付き悪用可能性（平均）
   </td>
   <td>総発生件数
   </td>
   <td>総 CVE 数
   </td>
  </tr>
  <tr>
   <td>40
   </td>
   <td>20.15%
   </td>
   <td>3.74%
   </td>
   <td>100.00%
   </td>
   <td>42.93%
   </td>
   <td>7.04
   </td>
   <td>3.84
   </td>
   <td>1,839,701
   </td>
   <td>32,654
   </td>
  </tr>
</table>



## 説明

アクセス制御は、ユーザーが意図した権限の範囲外で操作できないようにポリシーを執行します。アクセス制御の不備は、通常、許可されていない情報の開示、全データの変更または破壊、あるいはユーザーの権限を超えたビジネス機能の実行につながります。一般的なアクセス制御の脆弱性には、以下のようなものがあります。

* 最小権限の原則（一般的には「デフォルトで拒否」とも呼ばれます）の違反。アクセスは特定の機能、ロール、またはユーザーにのみ付与されるべきであるにもかかわらず、誰でもアクセス可能です。
* URL（パラメータの改ざんや強制ブラウジング）、アプリケーション内部の状態、または HTML ページの変更、あるいは API リクエストを変更する攻撃ツールの使用により、アクセス制御チェックをバイパスします。
* 一意の識別子を与えることで、他のユーザーのアカウントの閲覧または編集を許可します（オブジェクトの安全でない直接参照）。
* POST、PUT、および DELETE のアクセス制御が欠落している、アクセス可能な API。
* 権限の昇格。ログインしていない状態でユーザーとして操作したり、ユーザーとしてログインしている状態で管理者として操作したりできます。
* メタデータ操作（JSON Web Token (JWT) アクセス制御トークンのリプレイまたは改ざん、権限昇格を目的とした Cookie または隠しフィールドの操作、JWT 無効化の悪用など）。
* CORS の設定ミスにより、許可されていないまたは信頼できないオリジンからの API アクセスが許可されます。
* 認証されていないユーザーとして認証済みのページを、または標準ユーザーとして権限のあるページを強制的に閲覧（URL を推測）します。

## 防止方法

アクセス制御は、攻撃者がアクセス制御チェックやメタデータを変更できない、サーバー側の信頼できるコードまたはサーバーレス API に実装されている場合にのみ有効です。

* パブリック リソースを除き、デフォルトで拒否します。
* アクセス制御機構は一度実装し、アプリケーション全体で再利用します。これには、クロス オリジン リソース共有 (CORS) の使用を最小限に抑えることも含まれます。
* モデル レベルのアクセス制御では、ユーザーが任意のレコードを作成、読み取り、更新、または削除できるようにするのではなく、レコードの所有権を強制する必要があります。
* アプリケーション固有のビジネス制限要件は、ドメイン モデルによって強制される必要があります。
* Web サーバーのディレクトリ一覧表示を無効にし、ファイルのメタデータ (.git など) とバックアップ ファイルが Web ルート内に存在しないことを確実にします。
* アクセス制御の失敗（例: 繰り返し失敗する場合）をログに記録し、必要に応じて管理者に警告します。
* 自動攻撃ツールによる被害を最小限に抑えるため、API およびコントローラーへのアクセスにレート制限を実装します。
* ステートフル セッション識別子は、ログアウト後にサーバー上で無効化する必要があります。ステートレス JWT トークンは、攻撃者の攻撃機会を最小限に抑えるため、有効期間を短くする必要があります。より長期間有効な JWT の場合、アクセスを取り消すには OAuth 標準に従うことを強く推奨します。
* 簡潔で宣言的なアクセス制御を提供する、確立されたツールキットまたはパターンを使用します。

開発者と QA スタッフは、単体テストと統合テストに機能的なアクセス制御を含める必要があります。

## 攻撃シナリオの例

**シナリオ #1:** アプリケーションは、アカウント情報にアクセスする SQL 呼び出しで未検証のデータを使用します。


```
pstmt.setString(1, request.getParameter("acct"));
ResultSet results = pstmt.executeQuery( );
```

攻撃者は、ブラウザの「acct」パラメータを変更するだけで、任意のアカウント番号を送信できます。正しく検証されていない場合、攻撃者は任意のユーザーのアカウントにアクセスできます。

```
https://example.com/app/accountInfo?acct=notmyacct
```


**シナリオ #2:** 攻撃者は、ブラウザで特定の URL を強制的にアクセスするだけです。管理ページにアクセスするには管理者権限が必要です。


```
https://example.com/app/getappInfo
https://example.com/app/admin_getappInfo
```

認証されていないユーザーがどちらかのページにアクセスできる場合、それは欠陥です。管理者以外のユーザーが管理ページにアクセスできる場合も、それは欠陥です。

**シナリオ #3:** アプリケーションはすべてのアクセス制御をフロント エンドに集約しています。攻撃者は、ブラウザで JavaScript コードが実行されているため `https://example.com/app/admin_getappInfo` にアクセスできませんが、コマンド ラインから以下のコマンドを実行するだけでアクセス可能です。

```
$ curl https://example.com/app/admin_getappInfo
```

## 参考情報

* [OWASP Proactive Controls: C1: Implement Access Control](https://top10proactive.owasp.org/archive/2024/the-top-10/c1-accesscontrol/)
* [OWASP Application Security Verification Standard: V8 Authorization](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x17-V8-Authorization.md)
* [OWASP Testing Guide: Authorization Testing](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/README)
* [OWASP Cheat Sheet: Authorization](https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html)
* [PortSwigger: Exploiting CORS misconfiguration](https://portswigger.net/blog/exploiting-cors-misconfigurations-for-bitcoins-and-bounties)
* [OAuth: Revoking Access](https://www.oauth.com/oauth2-servers/listing-authorizations/revoking-access/)


## 対応する CWE の一覧

* [CWE-22 Improper Limitation of a Pathname to a Restricted Directory ('Path Traversal')](https://cwe.mitre.org/data/definitions/22.html)
* [CWE-23 Relative Path Traversal](https://cwe.mitre.org/data/definitions/23.html)
* [CWE-36 Absolute Path Traversal](https://cwe.mitre.org/data/definitions/36.html)
* [CWE-59 Improper Link Resolution Before File Access ('Link Following')](https://cwe.mitre.org/data/definitions/59.html)
* [CWE-61 UNIX Symbolic Link (Symlink) Following](https://cwe.mitre.org/data/definitions/61.html)
* [CWE-65 Windows Hard Link](https://cwe.mitre.org/data/definitions/65.html)
* [CWE-200 Exposure of Sensitive Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/200.html)
* [CWE-201 Exposure of Sensitive Information Through Sent Data](https://cwe.mitre.org/data/definitions/201.html)
* [CWE-219 Storage of File with Sensitive Data Under Web Root](https://cwe.mitre.org/data/definitions/219.html)
* [CWE-276 Incorrect Default Permissions](https://cwe.mitre.org/data/definitions/276.html)
* [CWE-281 Improper Preservation of Permissions](https://cwe.mitre.org/data/definitions/281.html)
* [CWE-282 Improper Ownership Management](https://cwe.mitre.org/data/definitions/282.html)
* [CWE-283 Unverified Ownership](https://cwe.mitre.org/data/definitions/283.html)
* [CWE-284 Improper Access Control](https://cwe.mitre.org/data/definitions/284.html)
* [CWE-285 Improper Authorization](https://cwe.mitre.org/data/definitions/285.html)
* [CWE-352 Cross-Site Request Forgery (CSRF)](https://cwe.mitre.org/data/definitions/352.html)
* [CWE-359 Exposure of Private Personal Information to an Unauthorized Actor](https://cwe.mitre.org/data/definitions/359.html)
* [CWE-377 Insecure Temporary File](https://cwe.mitre.org/data/definitions/377.html)
* [CWE-379 Creation of Temporary File in Directory with Insecure Permissions](https://cwe.mitre.org/data/definitions/379.html)
* [CWE-402 Transmission of Private Resources into a New Sphere ('Resource Leak')](https://cwe.mitre.org/data/definitions/402.html)
* [CWE-424 Improper Protection of Alternate Path](https://cwe.mitre.org/data/definitions/424.html)
* [CWE-425 Direct Request ('Forced Browsing')](https://cwe.mitre.org/data/definitions/425.html)
* [CWE-441 Unintended Proxy or Intermediary ('Confused Deputy')](https://cwe.mitre.org/data/definitions/441.html)
* [CWE-497 Exposure of Sensitive System Information to an Unauthorized Control Sphere](https://cwe.mitre.org/data/definitions/497.html)
* [CWE-538 Insertion of Sensitive Information into Externally-Accessible File or Directory](https://cwe.mitre.org/data/definitions/538.html)
* [CWE-540 Inclusion of Sensitive Information in Source Code](https://cwe.mitre.org/data/definitions/540.html)
* [CWE-548 Exposure of Information Through Directory Listing](https://cwe.mitre.org/data/definitions/548.html)
* [CWE-552 Files or Directories Accessible to External Parties](https://cwe.mitre.org/data/definitions/552.html)
* [CWE-566 Authorization Bypass Through User-Controlled SQL Primary Key](https://cwe.mitre.org/data/definitions/566.html)
* [CWE-601 URL Redirection to Untrusted Site ('Open Redirect')](https://cwe.mitre.org/data/definitions/601.html)
* [CWE-615 Inclusion of Sensitive Information in Source Code Comments](https://cwe.mitre.org/data/definitions/615.html)
* [CWE-639 Authorization Bypass Through User-Controlled Key](https://cwe.mitre.org/data/definitions/639.html)
* [CWE-668 Exposure of Resource to Wrong Sphere](https://cwe.mitre.org/data/definitions/668.html)
* [CWE-732 Incorrect Permission Assignment for Critical Resource](https://cwe.mitre.org/data/definitions/732.html)
* [CWE-749 Exposed Dangerous Method or Function](https://cwe.mitre.org/data/definitions/749.html)
* [CWE-862 Missing Authorization](https://cwe.mitre.org/data/definitions/862.html)
* [CWE-863 Incorrect Authorization](https://cwe.mitre.org/data/definitions/863.html)
* [CWE-918 Server-Side Request Forgery (SSRF)](https://cwe.mitre.org/data/definitions/918.html)
* [CWE-922 Insecure Storage of Sensitive Information](https://cwe.mitre.org/data/definitions/922.html)

* [CWE-1275 Sensitive Cookie with Improper SameSite Attribute](https://cwe.mitre.org/data/definitions/1275.html)
