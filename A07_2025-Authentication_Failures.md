<link rel="stylesheet" href="./assets/css/RC-stylesheet.css" />

# A07:2025 認証の失敗 <img src="./assets/TOP_10_Icons_Final_Identification_and_Authentication_Failures.png" style="height:80px;width:80px" align="right">


## 背景

認証の失敗は、このカテゴリに含まれる 36 個の CWE をより正確に反映するためにわずかに名称を変更したものの、7 位を維持しました。標準化されたフレームワークの恩恵を受けているにもかかわらず、このカテゴリは 2021 年から 7 位を維持しています。注目すべき CWE には、*CWE-259 ハード コードされたパスワードの使用*、*CWE-297: ホスト不一致による証明書の不適切な検証*、*CWE-287: 不適切な認証*、*CWE-384: セッション固定*、*CWE-798 ハード コードされた資格情報の使用* が含まれます。

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
   <td>36
   </td>
   <td>15.80%
   </td>
   <td>2.92%
   </td>
   <td>100.00%
   </td>
   <td>37.14%
   </td>
   <td>7.69
   </td>
   <td>4.44
   </td>
   <td>1,120,673
   </td>
   <td>7,147
   </td>
  </tr>
</table>



## 説明

攻撃者がシステムを騙して無効なユーザーや不正なユーザーを正当なユーザーとして認識させてしまう場合、この脆弱性が存在することになります。アプリケーションが以下の条件に該当する場合、認証に脆弱性が存在する可能性があります。

* 攻撃者が漏洩した有効なユーザー名とパスワードのリストを悪用し、クレデンシャル スタッフィングなどの自動攻撃を許します。近年、この種類の攻撃はハイブリッド パスワード攻撃、クレデンシャル スタッフィング（パスワード スプレー攻撃とも呼ばれます）へと拡張され、攻撃者は漏洩した認証情報のバリエーションや増分（例えば、Password1!、Password2!、Password3! など）を使用してアクセス権限を奪取しようとします。
* 総当たり攻撃やその他の自動化されたスクリプト攻撃を許しますが、これらはすぐにはブロックされません。
* デフォルトのパスワード、脆弱なパスワード、またはよく知られているパスワード（例えば、「Password1」や「admin」というユーザー名に「admin」というパスワード）を許します。
* ユーザーが、既に漏洩した認証情報を使用して、新規アカウントを作成できます。
* 安全を確保できない「知識ベースの回答」などの、脆弱または効果のない認証情報の復旧およびパスワード失念時のプロセスの使用を許可します。
* 平文、または弱い暗号化やハッシュ化されたパスワード データ ストアを使用しています（[A04:2025-Cryptographic Failures](A04_2025-Crypto_Failures.md) を参照）。
* 多要素認証が欠落しているか、効果がありません。
* 多要素認証が利用できない場合、脆弱または効果のないフォール バックの使用を許可しています。
* セッション識別子が、URL、隠しフィールド、またはクライアントがアクセスできるその他の安全でない場所に公開されています。
* ログイン成功後に同じセッション識別子が再利用されています。
* ログアウト中または非アクティブ期間中に、ユーザー セッションまたは認証トークン（主にシングル サインオン (SSO) トークン）が正しく無効化されていません。

## 防止方法

* 可能な場合は、多要素認証を導入しその使用を強制することで、自動クレデンシャル スタッフィング、総当たり攻撃、盗難された認証情報の再利用攻撃を防止します。
* 可能な場合は、パスワード マネージャーの使用を推奨し、ユーザーがより適切な選択を行えるようにします。
* 特に管理者ユーザーについては、デフォルトの認証情報で出荷またはデプロイしてはなりません。
* 新規または変更したパスワードを、最悪のパスワード上位 10,000 件のリストと比較するなど、脆弱なパスワードに対するチェックを実装します。
* 新規アカウントの作成時およびパスワードの変更時には、既知の侵害された認証情報のリストと照合します（例: [haveibeenpwned.com](https://haveibeenpwned.com) を使用）。
* パスワードの長さ、複雑さ、および変更に関するポリシーを、記憶シークレットまたはその他の最新の証拠に基づくパスワード ポリシーに関する [National Institute of Standards and Technology (NIST) 800-63b's guidelines in section 5.1.1](https://pages.nist.gov/800-63-3/sp800-63b.html#:~:text=5.1.1%20Memorized%20Secrets) に準拠させます。
* 侵害の疑いがない限り、パスワードの変更を強制してはなりません。侵害の疑いがある場合は、直ちにパスワードのリセットを強制します。
* 登録、認証情報の復旧、および API パスウェイが、アカウント列挙攻撃に対して堅牢化されていることを確実にします。すべての結果に同じメッセージ（「ユーザー名またはパスワードが無効です」）を使用します。
* ログイン失敗回数を制限するか、遅延を増やします。ただし、サービス拒否（DoS）シナリオを作成しないよう注意します。クレデンシャル スタッフィング、総当たり攻撃、その他の攻撃が検知または疑われる場合は、すべての失敗を記録し、管理者に警告します。
* ログイン後に高エントロピーの新しいランダム セッション ID を生成するような、サーバー側のセキュアな組み込みセッション マネージャーを使用します。セッション ID は URL に含めず、セキュアな Cookie に安全に保存し、ログアウト、アイドル タイムアウト、および絶対タイム アウト後に無効にする必要があります。
* 認証、ID、およびセッション管理には、既成の信頼性の高いシステムを使用するのが理想的です。可能な限り、堅牢化され十分にテストされたシステムを購入して活用することで、このリスクを移転します。

## 攻撃シナリオの例

**シナリオ #1:** 既知のユーザー名とパスワードの組み合わせリストを利用するクレデンシャル スタッフィングは、今や非常に一般的な攻撃です。最近では、攻撃者が人間の一般的な行動に基づいてパスワードを「増分」したり、調整したりすることが判明しています。例えば、「Winter2025」を「Winter2026」に、「ILoveMyDog6」を「ILoveMyDog7」や「ILoveMyDog5」に変更するなどです。このようなパスワード試行の調整は、ハイブリッド クレデンシャル スタッフィング攻撃またはパスワード スプレー攻撃と呼ばれ、従来の攻撃よりもさらに効果的です。アプリケーションが自動化された脅威（総当たり、スクリプト、ボット）やクレデンシャル スタッフィングに対する防御を実装していない場合、そのアプリケーションはパスワード オラクルとして利用され、認証情報の有効性を判断し、不正アクセスを許す可能性があります。

**シナリオ #2:** 認証攻撃の成功の多くは、パスワードを唯一の認証要素として使い続けることに起因しています。かつてはベスト プラクティスと考えられていたパスワードの変更と複雑さの要件は、ユーザーにパスワードの再利用と脆弱なパスワードの使用を促します。組織は、NIST 800-63 に従ってこれらの慣行を中止し、すべての重要なシステムで多要素認証の使用を強制することが推奨されます。

**シナリオ #3:** アプリケーションのセッション タイムアウトが正しく実装されていません。あるユーザーは、公共のコンピュータを使用してアプリケーションにアクセスし、「ログアウト」を選択する代わりにブラウザ タブを閉じて立ち去ってしまいます。また、シングル サインオン (SSO) セッションをシングル ログアウト (SLO) で終了できない場合も、この問題の好例です。つまり、1 回のログインで、例えばメール リーダー、文書管理システム、チャット システムなどにログインできますが、ログアウトは現在のシステムからのみ行われます。被害者がログアウトに成功したと思っても、一部のアプリケーションへの認証が残っている状態で攻撃者が同じブラウザを使用すると、被害者のアカウントにアクセスできます。オフィスや企業では、機密性の高いアプリケーションが正しく終了されておらず、同僚がロック解除されたコンピュータに（一時的に）アクセスできる場合にも、同様の問題が発生する可能性があります。

## 参考情報

* [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)
* [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/stable-en/01-introduction/05-introduction)


## 対応する CWE の一覧

* [CWE-258 Empty Password in Configuration File](https://cwe.mitre.org/data/definitions/258.html)
* [CWE-259 Use of Hard-coded Password](https://cwe.mitre.org/data/definitions/259.html)
* [CWE-287 Improper Authentication](https://cwe.mitre.org/data/definitions/287.html)
* [CWE-288 Authentication Bypass Using an Alternate Path or Channel](https://cwe.mitre.org/data/definitions/288.html)
* [CWE-289 Authentication Bypass by Alternate Name](https://cwe.mitre.org/data/definitions/289.html)
* [CWE-290 Authentication Bypass by Spoofing](https://cwe.mitre.org/data/definitions/290.html)
* [CWE-291 Reliance on IP Address for Authentication](https://cwe.mitre.org/data/definitions/291.html)
* [CWE-293 Using Referer Field for Authentication](https://cwe.mitre.org/data/definitions/293.html)
* [CWE-294 Authentication Bypass by Capture-replay](https://cwe.mitre.org/data/definitions/294.html)
* [CWE-295 Improper Certificate Validation](https://cwe.mitre.org/data/definitions/295.html)
* [CWE-297 Improper Validation of Certificate with Host Mismatch](https://cwe.mitre.org/data/definitions/297.html)
* [CWE-298 Improper Validation of Certificate with Host Mismatch](https://cwe.mitre.org/data/definitions/298.html)
* [CWE-299 Improper Validation of Certificate with Host Mismatch](https://cwe.mitre.org/data/definitions/299.html)
* [CWE-300 Channel Accessible by Non-Endpoint](https://cwe.mitre.org/data/definitions/300.html)
* [CWE-302 Authentication Bypass by Assumed-Immutable Data](https://cwe.mitre.org/data/definitions/302.html)
* [CWE-303 Incorrect Implementation of Authentication Algorithm](https://cwe.mitre.org/data/definitions/303.html)
* [CWE-304 Missing Critical Step in Authentication](https://cwe.mitre.org/data/definitions/304.html)
* [CWE-305 Authentication Bypass by Primary Weakness](https://cwe.mitre.org/data/definitions/305.html)
* [CWE-306 Missing Authentication for Critical Function](https://cwe.mitre.org/data/definitions/306.html)
* [CWE-307 Improper Restriction of Excessive Authentication Attempts](https://cwe.mitre.org/data/definitions/307.html)
* [CWE-308 Use of Single-factor Authentication](https://cwe.mitre.org/data/definitions/308.html)
* [CWE-309 Use of Password System for Primary Authentication](https://cwe.mitre.org/data/definitions/309.html)
* [CWE-346 Origin Validation Error](https://cwe.mitre.org/data/definitions/346.html)
* [CWE-350 Reliance on Reverse DNS Resolution for a Security-Critical Action](https://cwe.mitre.org/data/definitions/350.html)
* [CWE-384 Session Fixation](https://cwe.mitre.org/data/definitions/384.html)
* [CWE-521 Weak Password Requirements](https://cwe.mitre.org/data/definitions/521.html)
* [CWE-613 Insufficient Session Expiration](https://cwe.mitre.org/data/definitions/613.html)
* [CWE-620 Unverified Password Change](https://cwe.mitre.org/data/definitions/620.html)
* [CWE-640 Weak Password Recovery Mechanism for Forgotten Password](https://cwe.mitre.org/data/definitions/640.html)
* [CWE-798 Use of Hard-coded Credentials](https://cwe.mitre.org/data/definitions/798.html)
* [CWE-940 Improper Verification of Source of a Communication Channel](https://cwe.mitre.org/data/definitions/940.html)
* [CWE-941 Incorrectly Specified Destination in a Communication Channel](https://cwe.mitre.org/data/definitions/941.html)
* [CWE-1390 Weak Authentication](https://cwe.mitre.org/data/definitions/1390.html)
* [CWE-1391 Use of Weak Credentials](https://cwe.mitre.org/data/definitions/1391.html)
* [CWE-1392 Use of Default Credentials](https://cwe.mitre.org/data/definitions/1392.html)
* [CWE-1393 Use of Default Password](https://cwe.mitre.org/data/definitions/1393.html)