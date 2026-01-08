# 次のステップ

OWASP Top 10 は、設計上、最も重大なリスク 10 件に限定されています。すべての OWASP Top 10 には、掲載に向けて綿密に検討されたものの、最終的に採用されなかった「選ばれる一歩手前の」リスクが含まれています。他のリスクの方がより蔓延しており、影響も大きいからです。

成熟したアプリケーション セキュリティ プログラムの構築を目指す組織、セキュリティ コンサルタント、あるいは製品の適用範囲を拡大したいと考えているツール ベンダーにとって、以下の 2 つの問題は、特定して対策を講じる価値が十分にあります。

## X01:2025 アプリケーションの回復力の欠如

### 背景

これは 2021 年のサービス拒否攻撃を名称変更したものです。これは根本原因ではなく結果を説明していたため、名称が変更されました。このカテゴリは、レジリエンス（回復力）の問題に関連する脆弱性を説明する CWE に焦点を当てています。このカテゴリのスコアは、A10:2025-例外的な状況への不適切な対処 と非常に近いものでした。関連する CWE には、*CWE-400 制御不能なリソース消費、CWE-409 高度に圧縮されたデータの不適切な処理（データ増幅）、CWE-674 制御不能な再帰*、*CWE-835 到達不可能な終了条件を持つループ（「無限ループ」）* などがあります。

### スコア表


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
   <td>16
   </td>
   <td>20.05%
   </td>
   <td>4.55%
   </td>
   <td>86.01%
   </td>
   <td>41.47%
   </td>
   <td>7.92
   </td>
   <td>3.49
   </td>
   <td>865,066
   </td>
   <td>4,423
   </td>
  </tr>
</table>



### 解説

このカテゴリは、アプリケーションがストレス、障害、そして障害から回復できないエッジ ケースへの対応方法におけるシステム的な弱点を代表しています。アプリケーションが予期せぬ状況、リソース制約、その他の有害事象を適切に処理、耐久、回復できない場合、（最も一般的には）可用性の問題だけでなく、データ破損、機密データの漏洩、連鎖障害、セキュリティ管理策のバイパスなどにも容易につながる可能性があります。

さらに、[X02:2025 メモリ管理の失敗](#x022025-memory-management-failures) も、アプリケーションまたはシステム全体の障害につながる可能性があります。

### 防止方法

この種類の脆弱性を防止するには、システムの障害発生と復旧を想定した設計が必要です。

* 制限、クォータ、フェイル オーバー機能を追加し、特にリソースを最も消費する動作に注意します。
* リソースを大量に消費するページを特定し、事前に計画を立てます。特に、不要な「ガジェット」や、大量のリソース（CPU、メモリなど）を必要とする機能を、未知のユーザーや信頼できないユーザーに公開しないようにすることで、攻撃対象領域を削減します。
* 許可リストとサイズ制限を使用して厳格な入力検証を行い、徹底的にテストします。
* レスポンスのサイズを制限し、生のレスポンスをクライアントに返さないようにします（サーバー側で処理します）。
* デフォルトで safe/closed に設定し（決して開かない）、デフォルトで拒否し、エラーが発生した場合はロール バックします。
* リクエスト スレッドで同期呼び出しをブロックしないようにします（非同期/非ブロッキングを使用する、タイムアウトを設定する、同時実行数を制限するなど）。
* エラー処理機能を慎重にテストします。
* サーキット ブレーカー、バルク ヘッド、リトライ ロジック、グレースフル デグラデーションなどの回復力パターンを実装します。
* パフォーマンス テストと負荷テストを実施します。リスク選好がある場合は、カオス エンジニアリングを追加します。
* 合理的かつ費用対効果の高い範囲で、冗長性を実装および設します。
* 監視、可観測性、アラート機能を実装します。
* RFC 2267 に従って、無効な送信元アドレスをフィルタリングします。
* フィンガー プリント、IP アドレス、または振舞いに基づいて、既知のボット ネットを動的にブロックします。
* プルーフ・オブ・ワーク：*攻撃者*側でリソースを消費する操作を開始します。この操作は、通常のユーザーには大きな影響を与えませんが、大量のリクエストを送信しようとするボットには影響を与えます。システムの全体的な負荷が上昇した場合、特に信頼性の低いシステムやボットのように見えるシステムに対しては、プルーフ・オブ・ワークの難易度を上げます。
* 非アクティブ状態と最終タイム アウトに基づいて、サーバー側のセッション時間を制限します。
* セッションにバインドされた情報の保存を制限します。

### 攻撃シナリオの例

**シナリオ #1:** 攻撃者は意図的にアプリケーション リソースを消費し、システム内で障害を引き起こし、サービス拒否を引き起こします。具体的には、メモリ枯渇、ディスク容量の枯渇、CPU の飽和、あるいは無限接続の確立などが挙げられます。

**シナリオ #2:** 入力ファジングにより、細工された応答が生成され、アプリケーションのビジネス ロジックが破壊されます。

**シナリオ #3:** 攻撃者はアプリケーションの依存関係に着目し、API やその他の外部サービスを停止させることで、アプリケーションの動作を継続不能にします。


### 参考情報

* [OWASP Cheat Sheet: Denial of Service](https://cheatsheetseries.owasp.org/cheatsheets/Denial_of_Service_Cheat_Sheet.html)
* [OWASP MASVS‑RESILIENCE](https://mas.owasp.org/MASVS/11-MASVS-RESILIENCE/)
* [ASP.NET Core Best Practices (Microsoft)](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/best-practices?view=aspnetcore-9.0)
* [Resilience in Microservices: Bulkhead vs Circuit Breaker (Parser)](https://medium.com/@parserdigital/resilience-in-microservices-bulkhead-vs-circuit-breaker-54364c1f9d53)
* [Bulkhead Pattern (Geeks for Geeks)](https://www.geeksforgeeks.org/system-design/bulkhead-pattern/)
* [NIST Cybersecurity Framework (CSF)](https://www.nist.gov/cyberframework)
* [Avoid Blocking Calls: Go Async in Java (Devlane)](https://www.devlane.com/blog/avoid-blocking-calls-go-async-in-java)

## 対応する CWE の一覧

* [CWE-73  External Control of File Name or Path](https://cwe.mitre.org/data/definitions/73.html)
* [CWE-183 Permissive List of Allowed Inputs](https://cwe.mitre.org/data/definitions/183.html)
* [CWE-256 Plaintext Storage of a Password](https://cwe.mitre.org/data/definitions/256.html)
* [CWE-266 Incorrect Privilege Assignment](https://cwe.mitre.org/data/definitions/266.html)
* [CWE-269 Improper Privilege Management](https://cwe.mitre.org/data/definitions/269.html)
* [CWE-286 Incorrect User Management](https://cwe.mitre.org/data/definitions/286.html)
* [CWE-311 Missing Encryption of Sensitive Data](https://cwe.mitre.org/data/definitions/311.html)
* [CWE-312 Cleartext Storage of Sensitive Information](https://cwe.mitre.org/data/definitions/312.html)
* [CWE-313 Cleartext Storage in a File or on Disk](https://cwe.mitre.org/data/definitions/313.html)
* [CWE-316 Cleartext Storage of Sensitive Information in Memory](https://cwe.mitre.org/data/definitions/316.html)
* [CWE-362 Concurrent Execution using Shared Resource with Improper Synchronization ('Race Condition')](https://cwe.mitre.org/data/definitions/362.html)
* [CWE-382 J2EE Bad Practices: Use of System.exit()](https://cwe.mitre.org/data/definitions/382.html)
* [CWE-419 Unprotected Primary Channel](https://cwe.mitre.org/data/definitions/419.html)
* [CWE-434 Unrestricted Upload of File with Dangerous Type](https://cwe.mitre.org/data/definitions/434.html)
* [CWE-436 Interpretation Conflict](https://cwe.mitre.org/data/definitions/436.html)
* [CWE-444 Inconsistent Interpretation of HTTP Requests ('HTTP Request/Response Smuggling')](https://cwe.mitre.org/data/definitions/444.html)
* [CWE-451 User Interface (UI) Misrepresentation of Critical Information](https://cwe.mitre.org/data/definitions/451.html)
* [CWE-454 External Initialization of Trusted Variables or Data Stores](https://cwe.mitre.org/data/definitions/454.html)
* [CWE-472 External Control of Assumed-Immutable Web Parameter](https://cwe.mitre.org/data/definitions/472.html)
* [CWE-501 Trust Boundary Violation](https://cwe.mitre.org/data/definitions/501.html)
* [CWE-522 Insufficiently Protected Credentials](https://cwe.mitre.org/data/definitions/522.html)
* [CWE-525 Use of Web Browser Cache Containing Sensitive Information](https://cwe.mitre.org/data/definitions/20.html)
* [CWE-539 Use of Persistent Cookies Containing Sensitive Information](https://cwe.mitre.org/data/definitions/525.html)
* [CWE-598 Use of GET Request Method With Sensitive Query Strings](https://cwe.mitre.org/data/definitions/598.html)
* [CWE-602 Client-Side Enforcement of Server-Side Security](https://cwe.mitre.org/data/definitions/602.html)
* [CWE-628 Function Call with Incorrectly Specified Arguments](https://cwe.mitre.org/data/definitions/628.html)
* [CWE-642 External Control of Critical State Data](https://cwe.mitre.org/data/definitions/642.html)
* [CWE-646 Reliance on File Name or Extension of Externally-Supplied File](https://cwe.mitre.org/data/definitions/646.html)
* [CWE-653 Improper Isolation or Compartmentalization](https://cwe.mitre.org/data/definitions/653.html)
* [CWE-656 Reliance on Security Through Obscurity](https://cwe.mitre.org/data/definitions/656.html)
* [CWE-657 Violation of Secure Design Principles](https://cwe.mitre.org/data/definitions/657.html)
* [CWE-676 Use of Potentially Dangerous Function](https://cwe.mitre.org/data/definitions/676.html)
* [CWE-693 Protection Mechanism Failure](https://cwe.mitre.org/data/definitions/693.html)
* [CWE-799 Improper Control of Interaction Frequency](https://cwe.mitre.org/data/definitions/799.html)
* [CWE-807 Reliance on Untrusted Inputs in a Security Decision](https://cwe.mitre.org/data/definitions/807.html)
* [CWE-841 Improper Enforcement of Behavioral Workflow](https://cwe.mitre.org/data/definitions/841.html)
* [CWE-1021 Improper Restriction of Rendered UI Layers or Frames](https://cwe.mitre.org/data/definitions/1021.html)
* [CWE-1022 Use of Web Link to Untrusted Target with window.opener Access](https://cwe.mitre.org/data/definitions/1022.html)
* [CWE-1125 Excessive Attack Surface](https://cwe.mitre.org/data/definitions/1125.html)


## X02:2025 メモリ管理の失敗

### 背景 

Java、C#、JavaScript/TypeScript (node.js)、Go、そして「安全な」Rust といった言語はメモリ セーフです。メモリ管理の問題は、C や C++ といったメモリ セーフではない言語で発生する傾向があります。このカテゴリは、関連する CVE が 3 番目に多いにもかかわらず、コミュニティ調査では最も低いスコアとなり、データでも低い数値となっています。これは、従来のデスクトップ アプリケーションよりも Web アプリケーションが主流になっているためだと考えられます。メモリ管理の脆弱性は、CVSS スコアが最も高いことがよくあります。

### スコア表


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
   <td>24
   </td>
   <td>2.96%
   </td>
   <td>1.13%
   </td>
   <td>55.62%
   </td>
   <td>28.45%
   </td>
   <td>6.75
   </td>
   <td>4.82
   </td>
   <td>220,414
   </td>
   <td>30,978
   </td>
  </tr>
</table>



### 解説

アプリケーションが自らメモリ管理せざるを得ない場合、ミスが発生しやすくなります。メモリ セーフ言語の使用は増加傾向にありますが、世界中で運用されているレガシー システム、メモリ セーフではない言語の使用を必要とする新規の低レベル システム、そしてメイン フレーム、IoT デバイス、ファームウェアなど、独自でメモリを管理せざるを得ない可能性のあるシステムと連携する Web アプリケーションは依然として多く存在します。代表的な CWE としては、*CWE-120 入力サイズ チェックなしのバッファ コピー（「クラシック バッファオ ーバーフロー」）* と *CWE-121 スタック ベース バッファ オーバーフロー* が挙げられます。

メモリ管理の失敗は、以下のような場合に発生する可能性があります。

* 変数に十分なメモリを割り当てていません。
* 入力を検証せず、ヒープ、スタック、バッファのオーバーフローを引き起こしています。
* 変数の型が保持できるよりも大きなデータ値を格納しています。
* 未割り当てのメモリ空間またはアドレス空間を使用しようとしています。
* オフ バイ ワン エラー（0 ではなく 1 からカウントする）を引き起こしています。
* 解放済みのオブジェクトにアクセスしようとしています。
* 初期化されていない変数を使用しています。
* メモリ リークが発生しているか、アプリケーションが失敗するまで利用可能なメモリをすべて使い切っています。

メモリ管理の失敗は、アプリケーション、さらにはシステム全体の障害につながる可能性があります。[X01:2025 アプリケーションの回復力の欠如](#x012025-lack-of-application-resilience) も参照してください。

### 防止方法

メモリ管理の失敗を防ぐ最善の方法は、メモリ セーフな言語を使用することです。例としては、Rust、Java、Go、C#、Python、Swift、Kotlin、JavaScript などがあります。新しいアプリケーションを開発する際には、メモリ セーフな言語への移行は学習に見合う価値があることを組織に強く納得させましょう。完全なリファクタリングを行う場合は、可能かつ実現可能な場合は、メモリ セーフな言語での書き換えを強く推奨してください。

メモリセーフな言語を使用できない場合は、以下の手順を実行します。

* メモリ管理エラーの悪用を困難にする次のサーバー機能を有効にします: アドレス空間配置のランダム化 (ASLR)、データ実行保護 (DEP)、構造化例外処理上書き保護 (SEHOP)。
* アプリケーションのメモリ リークを監視します。
* システムへのすべての入力を慎重に検証し、想定を満たさない入力はすべて拒否します。
* 使用している言語を調査し、安全でない関数と安全性の高い関数のリストを作成し、そのリストをチーム全体と共有します。可能であれば、セキュア コーディングのガイドラインまたは標準に追加します。例えば、C 言語では、strcpy() よりも strncpy()、strcat() よりも strncat() を優先します。
* 使用している言語またはフレームワークでメモリ安全性ライブラリが提供されている場合は、それらを使用します。例えば、Safestringlib や SafeStr などです。
* 可能な限り、生の配列やポインタではなく、マネージド バッファとマネージド 文字列を使用します。
* メモリの問題や選択した言語に焦点を当てたセキュア コーディングのトレーニングを受講します。トレーナーに、メモリ管理の失敗が懸念されることを伝えます。
* コード レビューや静的解析を実施します。
* StackShield、StackGuard、Libsafe などのメモリ管理を支援するコンパイラ ツールを使用します。
* システムへのすべての入力に対してファジングを実施します。
* ペネトレーション テストを実施する場合は、メモリ管理の失敗が懸念事項であり、テスト中に特に注意するようテスターに​​伝えます。
* すべてのコンパイラ エラーと警告を修正します。プログラムがコンパイルされるからといって、警告を無視してはなりません。
* 基盤となるインフラストラクチャが定期的にパッチ適用、スキャン、堅牢化されていることを確実にします。
* 基盤となるインフラストラクチャを、特に潜在的なメモリ脆弱性やその他の障害がないか監視します。
* アドレス スタックをオーバーフロー攻撃から保護するために、[カナリア](https://en.wikipedia.org/wiki/Buffer_overflow_protection#Canaries) の使用を検討します。

### 攻撃シナリオの例

**シナリオ #1:** バッファ オーバーフローは最もよく知られているメモリ脆弱性です。これは、攻撃者がフィールドに許容量を超える情報を送信し、基になる変数用に作成されたバッファをオーバーフローさせる状況です。攻撃が成功すると、オーバーフローした文字列がスタック ポインタを上書きし、攻撃者がプログラムに悪意のある命令を挿入できるようになります。

**シナリオ #2:** ユーズ アフター フリー（UAF）は頻繁に発生するため、ブラウザのバグ報奨金プログラムでもよく取り上げられます。DOM 要素を操作する JavaScript を処理する Web ブラウザを想像してみてください。攻撃者は、オブジェクト（DOM 要素など）を作成し、その参照を取得する JavaScript ペイロードを作成します。巧妙な操作によって、ブラウザはオブジェクトのメモリを解放しますが、そのオブジェクトへのダングリング ポインタは保持されます。ブラウザがメモリが解放されたことに気付く前に、攻撃者は同じメモリ空間を占有する新しいオブジェクトを割り当てます。ブラウザが元のポインタを使用しようとすると、そのポインタは攻撃者が制御するデータを指すようになります。このポインタが仮想関数テーブルへのポインタだった場合、攻撃者はコード実行を自身のペイロードにリダイレクトできます。

**シナリオ #3:** ユーザー入力を受け付け、適切に検証またはサニタイズせずに、ログ出力関数に直接渡すネットワーク サービスがあるとします。ユーザーからの入力は、syslog("%s", user_input) ではなくフォーマットを指定しない syslog(user_input) としてログ出力関数に渡されます。攻撃者は、スタック メモリの読み取り（機密データの漏洩）のための %x や、メモリ アドレスへの書き込みのための %n などのフォーマット指定子を含む悪意のあるペイロードを送信します。複数のフォーマット指定子を連鎖させることで、スタックをマッピングし、重要なアドレスを特定して上書きすることができます。これは、フォーマット文字列の脆弱性（制御されていない文字列フォーマット）です。

注: 最新のブラウザは、[ブラウザ サンドボックス](https://www.geeksforgeeks.org/ethical-hacking/what-is-browser-sandboxing/#types-of-browser-sandboxing)、ASLR、DEP/NX、RELRO、PIE など、多段階の防御策を用いてこのような攻撃を防御しています。ブラウザに対するメモリ管理エラー攻撃は、簡単に実行できません。

### 参考情報

* [OWASP community pages: Memory leak,](https://owasp.org/www-community/vulnerabilities/Memory_leak) [Doubly freeing memory,](https://owasp.org/www-community/vulnerabilities/Doubly_freeing_memory) [& Buffer Overflow](https://owasp.org/www-community/vulnerabilities/Buffer_Overflow)
* [Awesome Fuzzing: a list of fuzzing resources](https://github.com/secfigo/Awesome-Fuzzing) 
* [Project Zero Blog](https://googleprojectzero.blogspot.com)
* [Microsoft MSRC Blog](https://www.microsoft.com/en-us/msrc/blog)

## 対応する CWE の一覧

* [CWE-14 Compiler Removal of Code to Clear Buffers](https://cwe.mitre.org/data/definitions/14.html)
* [CWE-119 Improper Restriction of Operations within the Bounds of a Memory Buffer](https://cwe.mitre.org/data/definitions/119.html)
* [CWE-120 Buffer Copy without Checking Size of Input ('Classic Buffer Overflow')](https://cwe.mitre.org/data/definitions/120.html)
* [CWE-121 Stack-based Buffer Overflow](https://cwe.mitre.org/data/definitions/121.html)
* [CWE-122 Heap-based Buffer Overflow](https://cwe.mitre.org/data/definitions/122.html)
* [CWE-124 Buffer Underwrite ('Buffer Underflow')](https://cwe.mitre.org/data/definitions/124.html)
* [CWE-125 Out-of-bounds Read](https://cwe.mitre.org/data/definitions/125.html)
* [CWE-126 Buffer Over-read](https://cwe.mitre.org/data/definitions/126.html)
* [CWE-190 Integer Overflow or Wraparound](https://cwe.mitre.org/data/definitions/190.html)
* [191 Integer Underflow (Wrap or Wraparound)](https://cwe.mitre.org/data/definitions/191.html)
* [CWE-196 Unsigned to Signed Conversion Error](https://cwe.mitre.org/data/definitions/196.html)
* [CWE-367 Time-of-check Time-of-use (TOCTOU) Race Condition](https://cwe.mitre.org/data/definitions/367.html)
* [CWE-415 Double Free](https://cwe.mitre.org/data/definitions/415.html)
* [CWE-416 Use After Free](https://cwe.mitre.org/data/definitions/416.html)
* [CWE-457 Use of Uninitialized Variable](https://cwe.mitre.org/data/definitions/457.html)
* [CWE-459 Incomplete Cleanup](https://cwe.mitre.org/data/definitions/459.html)
* [CWE-467 Use of sizeof() on a Pointer Type](https://cwe.mitre.org/data/definitions/467.html)
* [CWE-787 Out-of-bounds Write](https://cwe.mitre.org/data/definitions/787.html)
* [CWE-788 Access of Memory Location After End of Buffer](https://cwe.mitre.org/data/definitions/788.html)
* [CWE-824 Access of Uninitialized Pointer](https://cwe.mitre.org/data/definitions/824.html)

## X03:2025 AI が生成するコードにおける不適切な信頼（「バイブ コーディング」）

### 背景

現在、世界中で AI が話題となり活用されており、ソフトウェア開発者も例外ではありません。AI が生成するコードに関連する CVE や CWE は現時点では存在しませんが、AI が生成するコードには人間が書いたコードよりも多くの脆弱性が含まれることが多いことは周知の事実であり、文書化されています。

### 解説

ソフトウェア開発の慣行は変化しつつあり、AI の支援を受けて記述されたコードだけでなく、人間の監督をほぼ一切行わずに記述・コミットされたコード（いわゆるバイブ コーディング）も含まれるようになっています。ブログやウェブサイトからコード スニペットを安易にコピーすることは決して良い考えではありませんでしたが、今回のケースでは問題がさらに深刻化しています。良質で安全なコード スニペットは、かつても今も稀であり、システムの制約により AI によって統計的に無視される可能性があります。

### 防止方法

コードを書くすべての人に、AI を使用する際に以下の点を考慮することを強く推奨します。

* 提出するすべてのコードは、たとえ AI によって書かれたものやオンライン フォーラムからコピーしたものであっても、完全に読み理解できる必要があります。コミットするすべてのコードに対して、あなたは責任を負います。
* AI 支援コードはすべて、脆弱性がないか徹底的にレビューする必要があります。理想的には、自身の目で確認するだけでなく、この目的のために開発されたセキュリティ ツール（静的解析など）も活用します。 [OWASP Cheat Sheet Series: Secure Code Review](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Code_Review_Cheat_Sheet.html) に記載されている従来のコード レビュー手法の活用も検討します。
* 理想的には、自身の独自のコードを記述し、AI に改善を提案させ、AI のコードを確認し、結果に満足するまで AI に修正をさせます。
* 組織のセキュリティ コーディング ガイドライン、標準、ポリシーなど、独自に収集およびレビューした安全なコード サンプルと文書を RAG (Retrieval Augmented Generation) サーバーで使用することを検討し、RAG サーバーでポリシーや標準を適用するようにします。
* 選択した AI で使用するために、プライバシーとセキュリティのガードレールを実装するツールの購入を検討します。
* プライベート AI の購入を検討します。理想的には、AI が組織のデータ、クエリ、コード、またはその他の機密情報に基づいてトレーニングされないことを規定する契約 (プライバシー契約を含む) を締結します。
* IDE と AI の間にモデル コンテキスト プロトコル (MCP) サーバーを実装することを検討し、選択したセキュリティ ツールの使用を強制するように設定します。
* SDLC の一環としてポリシーとプロセスを実装し、開発者 (およびすべての従業員) に組織内で AI をどのように使用すべきか、また使用すべきでないかを通知します。
* IT セキュリティのベスト プラクティスを考慮した、効果的で適切なプロンプトのリストを作成します。理想的には、社内のセキュア コーディング ガイドラインも考慮する必要があります。開発者は、このプロンプトをプログラムの出発点として利用できます。
* AI は、システム開発ライフサイクルの各フェーズにおいて、効果的かつ安全な活用方法の両面で重要な要素となる可能性があるため、賢く活用します。
* 実際、複雑な機能、ビジネス クリティカルなプログラム、または長期間使用されるプログラムには、バイブ コーディングを使用することは推奨され<u>**ません**</u>。
* シャドウ AI の使用に対する技術的なチェックと安全対策を実装します。
* 開発者に対して、ポリシー、安全な AI の使用、ソフトウェア開発で AI を使用するためのベスト プラクティスについてトレーニングを行います。

### 参考情報

* [OWASP Cheat Sheet: Secure Code Review](https://cheatsheetseries.owasp.org/cheatsheets/Secure_Code_Review_Cheat_Sheet.html)


### 対応する CWE の一覧


-なし-


