<link rel="stylesheet" href=".・/assets/css/RC-stylesheet.css" />

# A05:2025 インジェクション <img src="./assets/TOP_10_Icons_Final_Injection.png" style="height:80px;width:80px" align="right">

## 背景

インジェクションはランキングで 3 位から 5 位に 2 つ順位を落としましたが、[A04:2025-暗号化の失敗](A04_2025-Cryptographic_Failures.md) および [A06:2025-セキュリティが確保されていない設計](A06_2025-Insecure_Design.md) との比較では順位を維持しています。インジェクションは最も多くのテストが実施されているカテゴリの 1 つであり、アプリケーションの 100% が何らかのインジェクションのテストを受けています。このカテゴリには 37 個の CWE があり、全カテゴリ中で最も多くの CVE 数を記録しています。インジェクションには、3 万件を超える CVE を持つクロス サイト スクリプティング（高頻度/低影響）と、1 万 4 千件を超える CVE を持つ SQL インジェクション（低頻度/高影響）が含まれます。*CWE-79 Web ページ生成における入力の不適切な中和」（「クロス サイト スクリプティング」）* の CVE が大量に報告されたことが、このカテゴリの平均的な加重影響度を低下させています。

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
   <td>37
   </td>
   <td>13.77%
   </td>
   <td>3.08%
   </td>
   <td>100.00%
   </td>
   <td>42.93%
   </td>
   <td>7.15
   </td>
   <td>4.32
   </td>
   <td>1,404,249
   </td>
   <td>62,445
   </td>
  </tr>
</table>



## 説明

インジェクション脆弱性とは、攻撃者がプログラムの入力フィールドに悪意のあるコードやコマンド（SQL やシェル コードなど）を挿入し、あたかもシステムの一部であるかのようにシステムを騙して実行させることを可能にするシステム欠陥です。これは、非常に深刻な結果をもたらす可能性があります。

アプリケーションが攻撃に対して脆弱な状況とは、以下のような状況を指します。

* ユーザーが入力したデータは、アプリケーションによって検証、フィルタリング、または無害化されません。
* コンテキスト対応エスケープのない動的クエリまたは非パラメータ化呼び出しが、インタープリタで直接使用されます。
* 無害化されていないデータは、追加の機密レコードを抽出するために、オブジェクト リレーショナル マッピング (ORM) 検索パラメータ内で使用されます。。
* 潜在的に悪意のあるデータが直接使用または連結されます。SQL またはコマンドには、動的クエリ、コマンド、またはストアド プロシージャの構造と悪意のあるデータが含まれています。

一般的なインジェクションには、SQL、NoSQL、OS コマンド、オブジェクト リレーショナル マッピング (ORM)、LDAP、式言語 (EL) またはオブジェクト グラフ ナビゲーション ライブラリ (OGNL) インジェクションなどがあります。この概念はすべてのインタープリタで同一です。すべてのパラメータ、ヘッダー、URL、Cookie、JSON、SOAP、および XML データ入力のソース コード レビューと自動テスト（ファジングを含む）を組み合わせることで、最も効果的に検出できます。静的（SAST）、動的（DAST）、および対話型（IAST）アプリケーション セキュリティ テスト ツールを CI/CD パイプラインに追加することでも、本番環境へのデプロイ前にインジェクション脆弱性を特定するのに役立ちます。

LLM では、関連する種類のインジェクション脆弱性が一般的になっています。これらについては、[OWASP LLM Top 10](https://genai.owasp.org/llm-top-10/)、特に [LLM01:2025 Prompt Injection](https://genai.owasp.org/llmrisk/llm01-prompt-injection/) で個別に説明されています。


## 防止方法


インジェクションを防ぐ最善の方法は、データをコマンドやクエリから分離することです。

* 推奨される選択肢は、安全な API を使用することです。これにより、インタープリタの使用を完全に回避したり、パラメータ化されたインターフェースを提供したり、オブジェクト リレーショナル マッピング ツール（ORM）に移行したりします。

    **注:** パラメータ化されている場合でも、PL/SQL または T-SQL がクエリとデータを連結したり、EXECUTE IMMEDIATE または exec() を使用して悪意のあるデータを実行したりすると、ストアド プロシージャによって SQL インジェクションが発生する可能性があります。

データとコマンドを分離できない場合は、以下の手法を用いて脅威を軽減できます。

* サーバー側での入力検証を実施します。テキスト エリアやモバイル アプリケーションの API など、多くのアプリケーションでは特殊文字が必要となるため、これは完全な防御策ではありません。
* 残りの動的クエリについては、そのインタープリタ固有のエスケープ構文を使用して特殊文字をエスケープします。

    **注:** テーブル名、列名などの SQL 構造はエスケープできないため、ユーザーが指定した構造名は危険です。これはレポート作成ソフトウェアでよく見られる問題です。
    
    **警告:** これらの手法では、複雑な文字列を解析してエスケープする必要があるため、エラーが発生しやすくなり、基盤となるシステムに小さな変更が加えられた場合に堅牢性が損なわれます。

## 攻撃シナリオの例

**シナリオ #1:** アプリケーションは、以下の脆弱な SQL 呼び出しの構築に信頼できないデータを使用します。

```
String query = "SELECT \* FROM accounts WHERE custID='" + request.getParameter("id") + "'";
```


**シナリオ #2:** 同様に、アプリケーションがフレームワークを盲目的に信頼すると、依然として脆弱なクエリが発生する可能性があります (例: ハイバネート クエリ言語 (HQL))。

```
Query HQLQuery = session.createQuery("FROM accounts WHERE custID='" + request.getParameter("id") + "'");
```

In both cases, the attacker modifies the ‘id’ parameter value in their browser to send: ' UNION SLEEP(10);--. For example:
どちらの場合も、攻撃者は、ブラウザの `id` パラメータの値を `' UNION SLEEP(10);--` に変更して送信します。例えば、

```
http://example.com/app/accountView?id=' UNION SELECT SLEEP(10);--
```

と変更すると、両方のクエリの意味が変わり、accounts テーブルのすべてのレコードが返されるようになります。より危険な攻撃では、データが変更または削除されたり、ストアド プロシージャが呼び出されたりする可能性があります。

## 参考情報

* [OWASP Proactive Controls: Secure Database Access](https://owasp.org/www-project-proactive-controls/v3/en/c3-secure-database)
* [OWASP ASVS: V5 Input Validation and Encoding](https://owasp.org/www-project-application-security-verification-standard)
* [OWASP Testing Guide: SQL Injection,](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection) [Command Injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/12-Testing_for_Command_Injection), and [ORM Injection](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05.7-Testing_for_ORM_Injection)
* [OWASP Cheat Sheet: Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/Injection_Prevention_Cheat_Sheet.html)
* [OWASP Cheat Sheet: SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
* [OWASP Cheat Sheet: Injection Prevention in Java](https://cheatsheetseries.owasp.org/cheatsheets/Injection_Prevention_Cheat_Sheet_in_Java.html)
* [OWASP Cheat Sheet: Query Parameterization](https://cheatsheetseries.owasp.org/cheatsheets/Query_Parameterization_Cheat_Sheet.html)
* [OWASP Automated Threats to Web Applications – OAT-014](https://owasp.org/www-project-automated-threats-to-web-applications/)
* [PortSwigger: Server-side template injection](https://portswigger.net/kb/issues/00101080_serversidetemplateinjection)
* [Awesome Fuzzing: a list of fuzzing resources](https://github.com/secfigo/Awesome-Fuzzing) 



## 対応する CWE の一覧

* [CWE-20 Improper Input Validation](https://cwe.mitre.org/data/definitions/20.html)
* [CWE-74 Improper Neutralization of Special Elements in Output Used by a Downstream Component ('Injection')](https://cwe.mitre.org/data/definitions/74.html)
* [CWE-76 Improper Neutralization of Equivalent Special Elements](https://cwe.mitre.org/data/definitions/76.html)
* [CWE-77 Improper Neutralization of Special Elements used in a Command ('Command Injection')](https://cwe.mitre.org/data/definitions/77.html)
* [CWE-78 Improper Neutralization of Special Elements used in an OS Command ('OS Command Injection')](https://cwe.mitre.org/data/definitions/78.html)
* [CWE-79 Improper Neutralization of Input During Web Page Generation ('Cross-site Scripting')](https://cwe.mitre.org/data/definitions/79.html)
* [CWE-80 Improper Neutralization of Script-Related HTML Tags in a Web Page (Basic XSS)](https://cwe.mitre.org/data/definitions/80.html)
* [CWE-83 Improper Neutralization of Script in Attributes in a Web Page](https://cwe.mitre.org/data/definitions/83.html)
* [CWE-86 Improper Neutralization of Invalid Characters in Identifiers in Web Pages](https://cwe.mitre.org/data/definitions/86.html)
* [CWE-88 Improper Neutralization of Argument Delimiters in a Command ('Argument Injection')](https://cwe.mitre.org/data/definitions/88.html)
* [CWE-89 Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')](https://cwe.mitre.org/data/definitions/89.html)
* [CWE-90 Improper Neutralization of Special Elements used in an LDAP Query ('LDAP Injection')](https://cwe.mitre.org/data/definitions/90.html)
* [CWE-91 XML Injection (aka Blind XPath Injection)](https://cwe.mitre.org/data/definitions/91.html)
* [CWE-93 Improper Neutralization of CRLF Sequences ('CRLF Injection')](https://cwe.mitre.org/data/definitions/93.html)
* [CWE-94 Improper Control of Generation of Code ('Code Injection')](https://cwe.mitre.org/data/definitions/94.html)
* [CWE-95 Improper Neutralization of Directives in Dynamically Evaluated Code ('Eval Injection')](https://cwe.mitre.org/data/definitions/95.html)
* [CWE-96 Improper Neutralization of Directives in Statically Saved Code ('Static Code Injection')](https://cwe.mitre.org/data/definitions/96.html)
* [CWE-97 Improper Neutralization of Server-Side Includes (SSI) Within a Web Page](https://cwe.mitre.org/data/definitions/97.html)
* [CWE-98 Improper Control of Filename for Include/Require Statement in PHP Program ('PHP Remote File Inclusion')](https://cwe.mitre.org/data/definitions/98.html)
* [CWE-99 Improper Control of Resource Identifiers ('Resource Injection')](https://cwe.mitre.org/data/definitions/99.html)
* [CWE-103 Struts: Incomplete validate() Method Definition](https://cwe.mitre.org/data/definitions/103.html)
* [CWE-104 Struts: Form Bean Does Not Extend Validation Class](https://cwe.mitre.org/data/definitions/104.html)
* [CWE-112 Missing XML Validation](https://cwe.mitre.org/data/definitions/112.html)
* [CWE-113 Improper Neutralization of CRLF Sequences in HTTP Headers ('HTTP Response Splitting')](https://cwe.mitre.org/data/definitions/113.html)
* [CWE-114 Process Control](https://cwe.mitre.org/data/definitions/114.html)
* [CWE-115 Misinterpretation of Output](https://cwe.mitre.org/data/definitions/115.html)
* [CWE-116 Improper Encoding or Escaping of Output](https://cwe.mitre.org/data/definitions/116.html)
* [CWE-129 Improper Validation of Array Index](https://cwe.mitre.org/data/definitions/129.html)
* [CWE-159 Improper Handling of Invalid Use of Special Elements](https://cwe.mitre.org/data/definitions/159.html)
* [CWE-470 Use of Externally-Controlled Input to Select Classes or Code ('Unsafe Reflection')](https://cwe.mitre.org/data/definitions/470.html)
* [CWE-493 Critical Public Variable Without Final Modifier](https://cwe.mitre.org/data/definitions/493.html)
* [CWE-500 Public Static Field Not Marked Final](https://cwe.mitre.org/data/definitions/500.html)
* [CWE-564 SQL Injection: Hibernate](https://cwe.mitre.org/data/definitions/564.html)
* [CWE-610 Externally Controlled Reference to a Resource in Another Sphere](https://cwe.mitre.org/data/definitions/610.html)
* [CWE-643 Improper Neutralization of Data within XPath Expressions ('XPath Injection')](https://cwe.mitre.org/data/definitions/643.html)
* [CWE-644 Improper Neutralization of HTTP Headers for Scripting Syntax](https://cwe.mitre.org/data/definitions/644.html)

* [CWE-917 Improper Neutralization of Special Elements used in an Expression Language Statement ('Expression Language Injection')](https://cwe.mitre.org/data/definitions/917.html)
