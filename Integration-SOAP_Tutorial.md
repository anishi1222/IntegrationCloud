# Integration Tutorial (SOAP)

- [はじめに](#はじめに)
  - [テスト・ツールの用意](#テスト・ツールの用意)
- [Calculationサンプルの作成](#Calculationサンプルの作成)
  - [目的](#目的)
  - [SOAP接続の作成](#soap接続の作成)
  - [統合の作成](#統合の作成)
  - [データ・マッピングと統合のアクティブ化](#データ・マッピングと統合のアクティブ化)
  - [統合のテストとモニタリング](#統合のテストとモニタリング)
  - [ここまでのまとめ](#ここまでのまとめ)
- [Calculationサンプルのエンリッチメント](#Calculationサンプルのエンリッチメント)
  - [エンリッチメントの目的](#エンリッチメントの目的)
  - [統合のドラフト作成とバージョニング](#統合のドラフト作成とバージョニング)
  - [新バージョンの統合へエンリッチメントの追加](#新バージョンの統合へエンリッチメントの追加)
  - [レスポンス・エンリッチメントのデータ・マッピング](#レスポンス・エンリッチメントのデータ・マッピング)
  - [新規バージョンのテストとモニタリング](#新規バージョンのテストとモニタリング)
  - [まとめ](#まとめ)

--------------------

## はじめに

本ハンズオンはOIC-**18.4.1**で動作確認済みです。

### テスト・ツールの用意

作成した統合のテストにSOAPメッセージを使用します。そのためSOAPメッセージの送信ソフトやアプリなどが必要ですが、SOAPのヘッダーにWSS Username Token/WS Timestampを組み込む必要があります。本ハンズオンではSOAP UIの利用を推奨しております。  
  
SOAP UI を使用する場合は、以下の作業を完了しておいてください。

**SOAP UIのインストール**

SOAP UI クライアントを以下からインストールしてください。`http://www.soapui.org/downloads/soapui/open-source.html`

OIC では、セキュリティ脆弱性 SSL poodleによる攻撃を回避するため、TLS1.1 をサポートしています。

以下の手順で SOAP UI クライアントを更新してください。

  1. soapui インストール・ディレクトリの下の bin へ移動します (例 `C:\program files (x86)\SmartBear\SoapUI-*\bin`)
  2. ファイル SoapUI-\*.vmoptions を開きます
  3. 次の行を最後に追記してください。 `-Dsoapui.https.protocols=SSLv3,TLSv1.1`

--------------------

## Calculationサンプルの作成

### 目的

本章では、最初の integration 作成までを解説します。本章では OICのユーザ・インタフェースを紹介し、以下の手順を説明します。

- シンプルなintegrationフローの作成
- シンプルなSOAPベースのWebサービスとの連携
- 連携元 (ソース) –連携先 (ターゲット) 間のデータ・マッピング
- Integrationのアクティベーションとテスト

これから作成するフローは、単純なリクエスト/レスポンス・パターンです。ソース・アプリケーションは、OICを介してターゲット・アプリケーションからデータを取得するためにリクエストを発行し、同期でそのレスポンスを取得します。

### SOAP接続の作成

1. OICのホームでハンバーガーメニューをクリックし、左側に現れるメニューから統合をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image3.png" style="width:6.76378in" />

1. 左側のメニューで**接続**のリンクをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image4.png" style="width:6.76389in" />

1. 連携元との接続を作成するため、画面右上の**作成**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image5.png" style="width:6.76389in" />

1. **SOAP**アダプターを選択します。検索画面でSOAPと入力して探し出すこともできます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image6.png" style="width:3.64213in" />

1. 次の情報を入力して、**作成**をクリックします。

    <table>
    <thead>
    <tr class="header">
    <th><strong>フィールド</strong></th>
    <th><strong>入力値</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>名前</td>
    <td>Calc &lt;User Name&gt;</td>
    </tr>
    <tr class="even">
    <td>識別子</td>
    <td><p><em>入力不要</em></p>
    <p>Connection Nameから自動で導出されます。 CALC_&lt;USER NAME&gt;</p></td>
    </tr>
    <tr class="odd">
    <td>ロール</td>
    <td><em>設定不要</em>（トリガーと呼出し）</td>
    </tr>
    <tr class="even">
    <td>説明</td>
    <td><em>任意</em></td>
    </tr>
    </tbody>
    </table>

1. （オプション）電子メール・アドレスに管理者用の E-Mail アドレスを入力します。例 `admin@tenant.com`
1. **接続の構成** をクリックしてWSDL URL を設定します。次のURLを入力します。

    | **フィールド** | **入力値**                                    |
    |----------------|-----------------------------------------------|
    | WSDL URL       | `http://www.dneonline.com/calculator.asmx?WSDL` |

    **注意** WSDL URLを入力した後、前後に余分な空白が含まれていないことを確認してください。他のパラメータはデフォルトのままにしておきます。

    **接続プロパティ**の**OK**をクリックします。  

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image7.png" style="width:5.91406in" />

1. **セキュリティの構成**をクリックし、セキュリティ・ポリシーを**セキュリティ・ポリシーなし**に変更します。**資格証明**の**OK**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image8.png" style="width:6.4252in" />

1. 接続できることを検証するため、**テスト**ボタンをクリックします。クリックするとダイアログが現れますが、ここでは**検証とテスト**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image9.png" style="width:3.125in" />

    その後、“正常にテストされました” のメッセージが表示されるはずです。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image11.png" style="width:6.76389in" />

1. **右上のインジケータ**が100%になっており、接続の構成が完了していることを確認した上で、接続を保存し、**閉じる**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image12.png" style="width:2.07638in" />

### 統合の作成

1. 画面左側のメニューから、**統合**のリンクをクリックします。左側のメニューが表示されていない場合は、赤枠で囲んだハンバーガーメニューをクリックしてください。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image13.png" style="width:6.76389in" />

1. 右側上部にある**作成**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image14.png" style="width:6.76389in" />

1. ダイアログで**基本ルーティング**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image15.png" style="width:3.67165in" />

1. 次の情報を入力して、**作成**をクリックします。

    <table>
    <thead>
    <tr class="header">
    <th><strong>フィールド</strong></th>
    <th><strong>入力値</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>統合にどのような名前を付けますか。</td>
    <td>Calc &lt;User Name&gt;</td>
    </tr>
    <tr class="even">
    <td>識別子</td>
    <td><p><em>入力不要</em></p>
    <p>Integration Name から自動的に導出されます。 CALC_&lt;USER NAME&gt;</p></td>
    </tr>
    <tr class="odd">
    <td>バージョン</td>
    <td><p><em>入力不要</em></p>
    <p>デフォルト値を使用します。 01.00.0000</p></td>
    </tr>
    <tr class="even">
    <td>この統合はどのパッケージに属しますか。</td>
    <td><em>指定しない</em></td>
    </tr>
    </tbody>
    </table>

1. 接続一覧からCalcの接続アイコンを**トリガーのドラッグ・アンド・ドロップへドラッグします。**

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image16.png" style="width:6.76378in" />

1. 設定ウィザードに従い、以下の内容を設定します。

    | **フィールド**                               | **入力値**           |
    |----------------------------------------------|----------------------|
    | エンドポイントにどのような名前を付けますか。 | AddService           |
    | このエンドポイントで何が行われますか。       | *任意*               |
    | 操作の選択                                   | *Add*                |
    | SoapAction検証の無効化                       | デフォルト（いいえ） |
    | ヘッダーの構成                               | デフォルト（いいえ） |

1. サマリーの画面は次のようになっているはずです。**完了**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image17.png" style="width:4.84921in" />

1. 呼び出し側にも同様に、**Calc**の接続アイコンを**呼出しのドラッグ・アンド・ドロップ**へドラッグします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image18.png" style="width:6.76389in" />

1. 設定ウィザードに従い、以下の内容を設定します。

    | **フィールド**                               | **入力値**           |
    |----------------------------------------------|----------------------|
    | エンドポイントにどのような名前を付けますか。 | CalculatorService    |
    | このエンドポイントでは何が行われますか。     | *任意*               |
    | ポートの選択                                 | *CalculatorSoap12*   |
    | 操作の選択                                   | *Add*                |
    | ヘッダー構成                                 | デフォルト（いいえ） |

### データ・マッピングと統合のアクティブ化

続いて、ソースから受信したメッセージをターゲットのWebサービスで使用する形式へ変換します。

1. **リクエスト・マッピング**のアイコンをクリックし、**＋**アイコンを押してデータ・マッパーを起動します

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image19.png" style="width:5.30827in" />

1. 次のソース・フィールドを対応するターゲット・フィールドへマッピングします。

    | **ソース・フィールド** | **ターゲット・フィールド** |
    |------------------------|----------------------------|
    | Add/intA               | Add/intA                   |
    | Add/intB               | Add/intB                   |

    ソース・フィールドのテキストをドラッグして、ターゲット・フィールドのテキストと重なるように調整します。マッピングが完了すると、画面が次のようになります。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image20.png" style="width:6.76389in" />

1. 右上の**検証**をクリックして問題ないことを確認し、**閉じる**をクリックします。

1. **レスポンス・マッピング**のアイコンをクリックし、**＋**アイコンを押してデータ・マッパーを起動します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image21.png" style="width:5.20709in" />

1. 次のソース・フィールドとターゲット・フィールドをマッピングします。

    | **ソース・フィールド** | **ターゲット・フィールド** |
    |------------------------|----------------------------|
    | AddResponse/AddResult  | AddResponse/AddResult      |

    ドラッグして、ソース・フィールドのテキストがターゲット・フィールドのテキストと重なるように調整します。マッピングが完了すると、画面が次のようになります。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image22.png" style="width:6.76389in" />

1. 右上の**検証**をクリックして問題ないことを確認し、**閉じる**をクリックします。

1. 編集内容を**保存**します。

1. 最後にメッセージ・トラッキングの設定をします。OICでは、処理されたメッセージをトラッキングでき、モニタリングの機能で確認できます。その際に個々のメッセージを判別するため、リクエスト・ペイロードでキーとなるフィールドを設定できます。

    画面右上のメニューから**トラッキング**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image23.png" style="width:2.3374in" />

1. 画面左にリクエスト・ペイロードのフィールドが並んでいます。この中から任意のフィールドをキーにしてトラッキングすることができます。ここではintAとintBを**トラッキング・フィールド**へドラッグ・ドロップします。設定が完了したら**終了**をクリックします。これで統合の設定が完了です。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image24.png" style="width:6.59055in" />

1. **右上のインジケータ**が100%になっており、統合の設定が完了していることを確認します:

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image25.png" style="width:0.72913in" />

1. **保存**アイコンを押して編集を保存し、**閉じる**をクリックします。

1. 統合の設定が100%完了すると、統合をアクティブにするためのトグル・ボタンが表示されます。**トグル・ボタン**をクリックし、統合をアクティブ化します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image26.png" style="width:5.14134in" />

1. 確認のダイアログではそのまま、**アクティブ化**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image27.png" style="width:3.3815in" />

1. 統合がアクティブ化されました。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image28.png" style="width:5.14291in" />

### 統合のテストとモニタリング

1. **インフォメーション・アイコン** **(ｉ)**をクリックすると、この統合のエンドポイントに対するWSDLが確認できます。このWSDLに対してSOAPメッセージを送信することでテストを実施します。**WSDLのURLをコピー**します（下図の赤枠で囲んである部分）

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image29.png" style="width:5.46811in" /><br/>

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image30.png" style="width:5.38819in" />

    **注意** 統合をアクティブ化すると以下のような通知が表示されます。この通知にあるエンドポイントURLに埋め込まれたハイパーリング（赤線部）は表示上のWSDLのURLとは異なるためクリックすると統合トップ画面に遷移します。

1. SOAPメッセージの送信できるアプリケーションでテストしてください。

    **SOAP UIを使った手順**

    Step1: **SOAP**アイコンをクリックし、**プロジェクト名**、確認した**エンドポイントURL**を入力します。(URLの読込にOICへログインが必要となるケースがあります。ログイン名とパスを入力します。)

    \[オプション手順\] エンドポイントURLのWSDLの取得に失敗することがあります。SOAP UIのProxy設定が必要になる場合があるのでハンズオン環境のネットワークに応じてProxyを設定してください。下図はAutomaticに設定した例です。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image32.png" style="width:5.37014in" />

    作成されたプロジェクトを展開してRequest1を開き、リクエストの構成画面を開きます:

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image34.png" style="width:4.65669in" />

    Step2: ペイロード編集画面の左下にある**Auth**タブでベーシック認証を追加します。OICのログイン・ユーザー/パスを追加ください。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image35.png" style="width:4.77874in" />

    Step3: ペイロードの**&lt;tem:intA&gt;?&lt;/tem:intA&gt;**と**&lt;tem:intB&gt;?&lt;/tem:intB&gt;**の「❓」に半角で整数値を入れます。以下の例では1と2を入れています。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image36.png" style="width:2.2752in" />

    Step4: ペイロード上で右クリックして、メニューから「**Add WSS Username Token**」と「**Add WS-Timestamp**」の両方をそれぞれ選択し、Headerを構成します。いずれも設定はデフォルトのままでかまいません。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image37.png" style="width:1.7586in" />

    Step5: 左上の**Submit**ボタンを押してテスト・メッセージを送信します。ここまでの設定がうまくいっていれば、AddResultには足し算の結果が入っているはずです。以下の例では、3が返っています。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image38.png" style="width:3.87008in" />

1. モニタリング画面からメッセージの成功を確認します。先ほどWSDLを取得したダイアログに現れた**インスタンスのトラッキング**のリンクをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image39.png" style="width:5.46811in" />

1. メッセージが正常に完了していることを確認します。この時、メッセージ・トラッキングとして設定したフィールドの値が表示されている事を確認します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image40.png" style="width:6.76389in" />

1. フィールドの値がリンクになっているのでこれをクリックすると、連携の実行結果を確認できます。エラーが発生した場合には、発生個所の特定に利用します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image41.png" style="width:6.76389in" />

--------------------

### ここまでのまとめ

この章を通じて、最初のintegrationを作成する手順を解説しました。OICのUIを紹介し、次の作業をどう実施するのかを確認しました。

- シンプルなintegrationフローの作成
- シンプルなSOAPベースのWebサービスとの連携
- 連携元 (ソース) –連携先 (ターゲット) 間のデータ・マッピング
- Integrationのアクティベーションとテスト

--------------------

## Hello World サンプルのエンリッチメント

### エンリッチメントの目的

本章では、2つのターゲット・サービスを起動してメッセージを処理する連携フローを作成します。リクエストされたメッセージは、1つ目のターゲットを起動してレスポンスを受け取り、更にそのレスポンスを使用して2つ目のターゲットを起動します。

そのため、全部で以下3種類のメッセージ・ペイロードを扱うことになります。

- リクエスト・メッセージ
- レスポンス・メッセージ
- エンリッチメントのレスポンス・メッセージ

ソースやターゲットの設定が完了した後、データ・マッパーを使用し、ソース側Webサービスのメッセージ構造から1つ目のターゲットの構造へデータ・マッピングを行います。次に、1つ目のターゲットからのレスポンスの構造から2つ目のターゲットの構造へデータ・マッピングを行います。その後、統合をアクティブ化してメッセージのやりとりの結果をモニタリングします。また補足作業として、統合の非アクティブ化についても実施します。

先ずは前章で作成した統合のコピーを作成し、それをカスタマイズしていきます。

### 統合のドラフト作成とバージョニング

構築済みの統合に対して別バージョンを作成することで、全く同じ接続、データ・マッピングの構成でバージョンだけが異なる統合のコピーが作成できます。その上で、コピーされた統合を編集することができます。

1. 画面左上のハンバーガーメニューをクリックしてメニューを開き、**＜**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image42.png" style="width:2.10591in" />

1. **デザイナ**をクリックし、**統合**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image43.png" style="width:1.34567in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image44.png" style="width:1.35118in" />

1. 統合の右端にある**アクション・メニュー**のアイコンを開き、**バージョンの作成**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image45.png" style="width:1.47244in" />

    バージョン番号に**1.01.0000**入力し、**バージョンの作成**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image46.png" style="width:4.4315in" />

    **補足** バージョン番号の最初の桁がメジャー・バージョンにあたり、メジャー・バージョンが重複する統合のうち、両方を同時にアクティブ化することはできません。どちらかがアクティブ化された状態でもう一方をアクティベーションすると、アクティブ化されていた方は非アクティブ化され切り替わります。

### 新バージョンの統合へエンリッチメントの追加

1. 統合の名称 **Calc XXXX (1.1.0)**をクリックして編集を開始します。連携フローに、レスポンス方向のエンリッチメントを追加していきます。構成済の接続一覧が表示されていない場合、画面右にある、**パレットを表示**アイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image47.png" style="width:1.8689in" />

1. 接続一覧からCalcの接続アイコンをレスポンスの矢印上へドラッグします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image48.png" style="width:6.76389in" />

1. 設定ウィザードに従って、以下の内容を設定していきます。終了したら**完了**をクリックします。

    | **フィールド**                               | **入力値**                   |
    |----------------------------------------------|------------------------------|
    | エンドポイントにどのような名前を付けますか。 | Square                       |
    | このエンドポイントでは何が行われますか。     | *任意*                       |
    | ポートの選択                                 | *CalculatorSoap12*           |
    | 操作の選択                                   | *Multiply*                   |
    | ヘッダーの構成                               | *デフォルトのまま（いいえ）* |

    サマリー画面は以下のようになっています。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image49.png" style="width:4.85in" />

1. 警告ダイアログが表示されますが、**\[更新\]**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image50.png" style="width:2.8878in" />

    これは既に設定されているレスポンス・データのマッピングが、新たに加えられたエンリッチメントのレスポンス・ペイロードによって影響を受ける可能性があることを示しています。

### レスポンス・エンリッチメントのデータ・マッピング

1. ソースとターゲットの間にある**レスポンス・エンリッチメント・マッピング**をクリックし、

    **＋**アイコンを押してデータ・マッパーを起動します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image51.png" style="width:5.4685in" />

1. 次のソース・フィールドとターゲット・フィールドをマッピングします。

    | **ソース・フィールド**                      | **ターゲット・フィールド** |
    |---------------------------------------------|----------------------------|
    | AddResponse &gt; AddResult                  | Multiply &gt; intA         |
    | $SourceApplicationObject &gt; Add &gt; intB | Multiply &gt; intB         |

    ドラッグして、ソース・フィールドのテキストがターゲット・フィールドのテキストと重なるように調整します。マッピングが完了すると、画面が次のようになっています。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image52.png" style="width:6.76389in" />

1. **検証**して問題がないことを確認後、**閉じる**をクリックします。

1. エンリッチメントで得られるペイロードを利用したいので、マッピングを再定義します。レスポンス・マッピングのアイコンをクリックし、編集アイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image53.png" style="width:5.4689in" />

1. 既にマッピングされているものを削除し、新たなマッピングを構成します。ターゲット・フィールドの「AddResponse &gt; AddResult」のフィールド名を右クリックして、**マッピングの削除**を選択します。確認ダイアログが現れますので、**削除**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image54.png" style="width:4.03504in" />

1. 次のソース・フィールドとターゲット・フィールドをマッピングします。

    | **ソース・フィールド**                                                         | **ターゲット・フィールド** |
    |--------------------------------------------------------------------------------|----------------------------|
    | $ResponseEnrichmentApplicationObject &gt; MultiplyResponse &gt; MultiplyResult | AddResponse &gt; AddResult |

1. マッピングを**検証**し、**閉じる**をクリックします。

1. ここまでの統合の編集を**保存**して**閉じる**をクリックします。

1. 新規バージョンの統合（1.1.0）の**トグル・ボタン**をクリックしてアクティベーションします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image55.png" style="width:5.24606in" />

1. 確認ダイアログで、**アクティブ化**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image56.png" style="width:4.4315in" />

1. 新規バージョンがアクティブ化されました。

    **補足**

    新規バージョンがアクティブ化されると同時に、以前のバージョンは非アクティブ化されます。OICではバージョン番号の最初の桁だけがメジャー・バージョンとして扱われ、メジャー・バージョンが重複している統合を同時にアクティブ化することはできません。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image57.png" style="width:5.14016in" />

### 新規バージョンのテストとモニタリング

1. SOAPメッセージの送信できるアプリケーションでテストします。<br/>WSDLのURL、リクエスト・ペイロードには変更がありませんが、WSS Username Token、WS-TimeStampは再設定する必要があります。SOAP UIを使う場合は、前章の手順を参考にしてください。

1. レスポンスのペイロードのAddResult要素が変更され、(intA+intB)\*intBの結果が入っていることを確認してください。

    レスポンスの例
    ```xml
    <Envelope xmlns:env="http://schemas.xmlsoap.org/soap/envelope/">
        <Header />
        <Body>
            <AddResponse xmlns:mime="http://schemas.xmlsoap.org/wsdl/mime/"
                            xmlns:soap12="http://schemas.xmlsoap.org/wsdl/soap12/"
                            xmlns:soapenc="http://schemas.xmlsoap.org/soap/encoding/"
                            xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/"
                            xmlns:http="http://schemas.xmlsoap.org/wsdl/http/"
                            xmlns:tm="http://microsoft.com/wsdl/mime/textMatching/"
                            xmlns:nstrgmpr="http://tempuri.org/"
                            xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/">
                 <AddResult>6</AddResult>
            </AddResponse>
       </Body>
    </Envelope>
    ```

1. モニタリング画面で状況を確認します。画面左上のハンバーガーメニューをクリックしてメニューを開き、**＜**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image58.png" style="width:1.33504in" />

1. **モニタリング**を開きます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image59.png" style="width:1.34055in" />

1. **ダッシュボード**を開きます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image60.png" style="width:1.35118in" />

1. トレースを有効化している場合、画面右の**アクティビティ・ストリーム**をクリックすると、トレースを確認できるとともに、トレース情報をダウンロードできます（なお、デフォルトではトレースは無効化されています）。このファイルはトレース情報を含んだサーバー・ログで、このログから統合による処理プロセスやペイロードの情報を確認できます。またアクティビティ・ストリームや診断ログは、サポートへ問い合わせをする際の状況確認にも利用します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image61.png" style="width:6.76389in" />

1. 画面左メニューから**トラッキング**を開きます。

1. メッセージが正常に完了していることを確認します。この時、メッセージ・トラッキングとして設定したフィールドの値が表示されている事を確認します。またバージョンが**1.1.0**であることを確認します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image62.png" style="width:5.13976in" />

1. フィールドの値がリンクになっているのでこれをクリックし、連携の実行結果を確認します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-SOAP_Tutorial/image63.png" style="width:6.76389in" />

### まとめ

本章では次の手順を解説しました。

- 構成済の統合に対する別バージョンの作成
- SOAP Webサービスを使ったメッセージのエンリッチメント
- 統合のトレースを有効化した場合のアクティブ化とモニタリング
