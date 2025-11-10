<link rel="stylesheet" href="../assets/css/RC-stylesheet.css" />

# A10:2025 例外的な状況への不適切な対処

## 背景

例外的な状況への不適切な対処は、2025 年に新設されたカテゴリです。このカテゴリには 24 件の CWE が含まれており、不適切なエラー処理、論理エラー、フェイル オープン、その他システムが遭遇する可能性のある異常な状況に起因する関連シナリオに焦点を当てています。このカテゴリには、以前はコード品質の低さと関連付けられていた CWE も含まれています。しかし、以前の分類はあまりにも包括的すぎたため、より具体的なこのカテゴリの方が、より適切なガイダンスを提供できると考えています。

このカテゴリに含まれる主な CWE は以下のとおりです: *CWE-209 機密情報を含むエラー メッセージの生成、CWE-234 パラメータ不足の処理漏れ、CWE-274 不十分な権限の不適切な処理、CWE-476 NULL ポインタの参照解除*、および *CWE-636 安全でないフェイル オーバー（「フェイルオープン」）*。

## スコア表


<table>
  <tr>
   <td>対応する CWE 
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
   <td>24
   </td>
   <td>20.67%
   </td>
   <td>2.95%
   </td>
   <td>100.00%
   </td>
   <td>37.95%
   </td>
   <td>7.11
   </td>
   <td>3.81
   </td>
   <td>769,581
   </td>
   <td>3,416
   </td>
  </tr>
</table>



## 説明

ソフトウェアにおける例外的な状況への不適切な対処とは、プログラムが異常かつ予測不可能な状況を防止、検出、および適切に対応できない場合に発生し、クラッシュ、予期せぬ動作、そして時には脆弱性につながります。これは、以下の 3 つの不備のうち 1 つ以上が関係している可能性があります。それは、アプリケーションが異常な状況の発生を防止しない、発生している状況を認識しない、状況発生後の対応が不十分であるか全く対応しない、といったケースです。

例外的な状況は、入力検証の欠落、不備、または不完全さ、あるいはエラーが発生した関数ではなく、より上位のレベルでエラー処理が行われること、メモリ、権限、ネットワークの問題などの予期せぬ環境状態、一貫性のない例外処理、または全く処理されない例外などによって引き起こされる可能性があります。これらにより、システムは未知の予測不可能な状態に陥ります。アプリケーションが次に実行すべき命令を判断できない場合、例外処理が適切に行われていないことになります。発見が困難なエラーや例外は、アプリケーション全体のセキュリティを長期間にわたって脅かす可能性があります。

例外的な状況への不適切な対処の不備によって、ロジック バグ、オーバー フロー、競合状態、不正なトランザクション、メモリ、状態、リソース、タイミング、認証、認可に関する問題など、さまざまなセキュリティ脆弱性が発生する可能性があります。これらの脆弱性は、システムまたはデータの機密性、可用性、完全性に悪影響を与える可能性があります。攻撃者は、アプリケーションの不完全なエラー処理を悪用して、これらの脆弱性を突きます。

## 防止方法

 例外的な状況に適切に対処するためには、そのような状況（最悪の事態）を想定して計画を立てる必要があります。発生しうるあらゆるシステム エラーは、発生箇所で直接「捕捉」し、適切に処理しなければなりません（つまり、問題を解決し、システムが正常な状態に復旧するように、何らかの有意義な対応を行う必要があります）。処理の一環として、エラーをスローする（ユーザーに分かりやすい形で通知する）、イベントをログに記録する、必要に応じてアラートを発行する、といった対応を含める必要があります。また、万が一見落としがあった場合に備えて、グローバルな例外ハンドラも用意しておく必要があります。理想的には、繰り返し発生するエラーや、継続的な攻撃を示唆するパターンを監視し、何らかの対応、防御、またはブロックを行うための監視ツールやオブザーバビリティ機能も備えているべきです。これにより、エラー処理の弱点を狙うスクリプトやボットをブロックし、対処することができます。

例外的な状況を捕捉して適切に処理することで、プログラムの基盤となるインフラストラクチャが予測不能な状況に対処する必要がなくなるようにすることを確実にします。何らかのトランザクションの途中でエラーが発生した場合は、トランザクションのすべての部分をロール バックして最初からやり直すことが非常に重要です（これは「クローズド フェイル」とも呼ばれます）。トランザクションの途中で復旧を試みると、回復不能なエラーが発生することがよくあります。

可能な限り、レート制限、リソース クォータ、スロットリングなどの制限を適用し、例外的な状況がそもそも発生しないようにします。情報技術において、あらゆるものが無制限であるべきではありません。無制限な状態は、アプリケーションの回復力の低下、サービス拒否攻撃、ブルートフォース攻撃の成功、そして高額なクラウド料金につながるからです。一定の頻度を超えて同一のエラーが繰り返し発生する場合、それらのエラーは、発生頻度と時間枠を示す統計情報としてのみ出力することを検討してください。この情報は、自動ログ記録および監視システムに影響を与えないように、元のメッセージに追加する必要があります（A09:2025 ログ記録とアラートの失敗 TJを参照）。

さらに、厳格な入力検証（受け入れざるを得ない潜在的に危険な文字についてはサニタイズまたはエスケープ処理を行う）、そして一元化されたエラー処理、ログ記録、監視、アラート機能、およびグローバル例外ハンドラを実装する必要があります。アプリケーション内で例外的な状況を処理する機能が複数存在すべきではなく、すべて一箇所で、常に同じ方法で処理される必要があります。また、このセクションで述べたすべての事項についてプロジェクトのセキュリティ要件を策定し、プロジェクトの設計段階で脅威モデリングやセキュア設計レビューを実施し、コードレビューまたは静的解析を行うとともに、最終システムに対して負荷テスト、パフォーマンス テスト、および侵入テストを実施する必要があります。

可能であれば、組織全体で例外的な状況への対処方法を統一すル必要があります。そうすることで、この重要なセキュリティ対策におけるコードのエラーをレビューおよび監査しやすくなります。

## 攻撃シナリオの例

**要修正**: サンプルをより現代的なものに更新してください。

**Scenario #1:** Resource exhaustion via mishandling of exceptional conditions (Denial of Service) could be caused if the application catches exceptions when files are uploaded, but doesn’t properly release resources after. Each new exception leaves resources locked or otherwise unavailable, until all resources are used up.

**Scenario #2:** Sensitive data exposure via improper handling or database errors that reveals the full system error to the user. The attacker continues to force errors in order to use the sensitive system information to create a better SQL injection attack. The sensitive data in the user error messages are reconnaissance. 

**Scenario #3:** State corruption in financial transactions could be caused by an attacker interrupting a multi-step transaction via network disruptions. Imagine the transaction order was: debit user account, credit destination account, log transaction. If the system doesn’t properly roll back the entire transaction (fail closed) when there is an error part way through, the attacker could potentially drain the user’s account, or possibly a race condition that allows the attacker to send money to the destination multiple times.


## 参考情報

OWASP MASVS‑RESILIENCE

- [OWASP Cheat Sheet: Logging](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)

- [OWASP Cheat Sheet: Error Handling](https://cheatsheetseries.owasp.org/cheatsheets/Error_Handling_Cheat_Sheet.html)

- [OWASP Application Security Verification Standard (ASVS): V16.5 Error Handling](https://github.com/OWASP/ASVS/blob/master/5.0/en/0x25-V16-Security-Logging-and-Error-Handling.md#v165-error-handling)

- [OWASP Testing Guide: 4.8.1 Testing for Error Handling](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/08-Testing_for_Error_Handling/01-Testing_For_Improper_Error_Handling)

* [Best practices for exceptions (Microsoft, .Net)](https://learn.microsoft.com/en-us/dotnet/standard/exceptions/best-practices-for-exceptions)

* [Clean Code and the Art of Exception Handling (Toptal)](https://www.toptal.com/developers/abap/clean-code-and-the-art-of-exception-handling)

* [General error handling rules (Google for Developers)](https://developers.google.com/tech-writing/error-messages/error-handling)

## 対応する CWE の一覧
* [CWE-209	Generation of Error Message Containing Sensitive Information](https://cwe.mitre.org/data/definitions/209.html)
* [CWE-215	Insertion of Sensitive Information Into Debugging Code](https://cwe.mitre.org/data/definitions/215.html)
* [CWE-234	Failure to Handle Missing Parameter](https://cwe.mitre.org/data/definitions/234.html)
* [CWE-235	Improper Handling of Extra Parameters](https://cwe.mitre.org/data/definitions/235.html)
* [CWE-248	Uncaught Exception](https://cwe.mitre.org/data/definitions/248.html)
* [CWE-252	Unchecked Return Value](https://cwe.mitre.org/data/definitions/252.html)
* [CWE-274	Improper Handling of Insufficient Privileges](https://cwe.mitre.org/data/definitions/274.html)
* [CWE-280	Improper Handling of Insufficient Permissions or Privileges](https://cwe.mitre.org/data/definitions/280.html)
* [CWE-369	Divide By Zero](https://cwe.mitre.org/data/definitions/369.html)
* [CWE-390	Detection of Error Condition Without Action](https://cwe.mitre.org/data/definitions/390.html)
* [CWE-391	Unchecked Error Condition](https://cwe.mitre.org/data/definitions/391.html)
* [CWE-394	Unexpected Status Code or Return Value](https://cwe.mitre.org/data/definitions/394.html)
* [CWE-396	Declaration of Catch for Generic Exception](https://cwe.mitre.org/data/definitions/396.html)
* [CWE-397	Declaration of Throws for Generic Exception](https://cwe.mitre.org/data/definitions/397.html)
* [CWE-460	Improper Cleanup on Thrown Exception](https://cwe.mitre.org/data/definitions/460.html)
* [CWE-476	NULL Pointer Dereference](https://cwe.mitre.org/data/definitions/476.html)
* [CWE-478	Missing Default Case in Multiple Condition Expression](https://cwe.mitre.org/data/definitions/478.html)
* [CWE-484	Omitted Break Statement in Switch](https://cwe.mitre.org/data/definitions/484.html)
* [CWE-550	Server-generated Error Message Containing Sensitive Information](https://cwe.mitre.org/data/definitions/550.html)
* [CWE-636	Not Failing Securely ('Failing Open')](https://cwe.mitre.org/data/definitions/636.html)
* [CWE-703	Improper Check or Handling of Exceptional Conditions](https://cwe.mitre.org/data/definitions/703.html)
* [CWE-754	Improper Check for Unusual or Exceptional Conditions](https://cwe.mitre.org/data/definitions/754.html)
* [CWE-755	Improper Handling of Exceptional Conditions](https://cwe.mitre.org/data/definitions/755.html)
* [CWE-756	Missing Custom Error Page](https://cwe.mitre.org/data/definitions/756.html)