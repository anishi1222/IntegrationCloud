# Integration Tutorial (REST)

- [はじめに](#はじめに)
  - [目的](#目的)
  - [最終構成](#最終構成)
  - [前提](#前提)
- [接続の作成](#接続の作成)
- [連携フローの定義](#連携フローの定義)
  - [OICの統合メタデータを取得](#OICの統合メタデータを取得)
- [統合の作成](#統合の作成)
  - [データ・マッピングとアクティブ化](#データ・マッピングとアクティブ化)
  - [統合のテストとモニタリング](#統合のテストとモニタリング)

## はじめに

### 目的

本ハンズオンでは、既存のREST APIに対して、Oracle Integration Cloud（以下、OIC）で新たなRESTエンドポイントを構成・公開するために、REST to RESTの連携フローを実装します。

このハンズオンを通じて以下のポイントを理解することができます。

- Source側RESTの実装: REST/JSONでエンドポイントを構成して公開する方法
- Target側RESTの実装: REST APIを呼び出すための設定方法
- JSONデータの構造変換、マッピングする方法
- REST/JSONで構成されたOICエンドポイントのテストする方法

なお本ハンズオンでは、既存のREST APIとして、OICが運用・管理用途で公開しているREST API（Integrationのメタデータを取得するAPI）を利用します。

--------

### 最終構成

本ハンズオンの最終構成を以下に示します。

- Target側のREST APIとして、OICの運用・管理用REST APIを利用する
- ハンズオンで作成したOICのエンドポイントへのリクエストは、Target側のREST APIに渡す
- Target側のREST APIからのレスポンスを編集して、呼び出し元に返す

![image2](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image2.png)

完成版をエクスポートしたものは[こちら](Integration-REST_Tutorial.solution.iar)にあります。

--------

### 前提

OICでは運用・管理を目的としたREST APIを提供しており、本ハンズオンでは、その中のOICの統合のメタデータを取得するAPIを使用します。OICの運用・管理用のREST APIの一覧は、以下のドキュメントをご覧ください。

> **Integration Cloud**
>
> [https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/index.html](https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/index.html)
>
> **Integration Cloud (User managed)**
>
> [https://docs.oracle.com/en/cloud/paas/integration-cloud-um/rest-api-um/index.html](https://docs.oracle.com/en/cloud/paas/integration-cloud-um/rest-api-um/index.html)

本ハンズオンを実施する前に、アクティブな統合を用意する必要があります。

動作確認を簡単にするため、REST APIを呼び出すためのツール（REST APIテストクライアント）を用意してください。

本ハンズオンでは、以下のツールを使用した手順を掲載していますが、既にREST APIテストクライアントをお持ちであれば、特にインストールする必要はありません。

ツール名：Advanced REST client

#### インストール方法

1. Google Chromeを開き、以下のURLへアクセスします。
  [https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo)
1. **Chromeに追加**をクリックします。

    ![image3](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image3.png)

1. Google Chromeの右上にある**アプリ**をクリックします。

    ![image4](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image4.png)

1. **ARC**のアイコンをすると、Advanced REST Clientアプリを起動できます。

    ![image5](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image5.png)

（オプション）この後の手順において、クライアント環境によってはネットワーク上のProxyの影響により、ARC Advanced REST ClientからのHTTPリクエストがタイムアウトになる場合があります。その場合は「ARC PROXY EXTENSION」を追加インストールしてみてください。

![image6](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image6.png)

--------

## 接続の作成

RESTアダプタを使用して、REST接続を定義します。RESTアダプタでAPIのURLや認証情報を登録すると、REST APIへ接続できるようになります。

1. OICのホーム画面で**Integrations**のページに移動します。

    ![image7](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image7.png)

1. メニューで**接続**をクリックします。

    ![image8](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image8.png)

1. **作成**をクリックします。

    ![image10](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image10.png)

1. **REST**アダプタを選択します。

    ![image11](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image11.png)

1. 次の接続情報を入力します。

    <table>
    <thead>
    <tr class="header">
    <th><strong>フィールド</strong></th>
    <th><strong>入力値</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>接続名</td>
    <td>REST-OICAPI &lt;user name&gt;</td>
    </tr>
    <tr class="even">
    <td>識別子</td>
    <td><p><em>入力不要</em></p>
    <p>Connection Nameから自動で導出されます: REST_OICAPI_&lt;user name&gt;</p></td>
    </tr>
    <tr class="odd">
    <td>接続ロール</td>
    <td><em>設定不要</em>（トリガーと呼出し）</td>
    </tr>
    </tbody>
    </table>

1. **作成** をクリックします。
    >（オプション）電子メール・アドレス に管理者用の E-Mailアドレスを入力します。（例: admin@tenant.com）

1. **接続の構成**をクリックし、次の接続プロパティを設定します。

    <table>
    <thead>
    <tr class="header">
    <th><strong>フィールド</strong></th>
    <th><strong>入力値</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>接続タイプ</td>
    <td>REST APIベースURL</td>
    </tr>
    <tr class="even">
    <td>TLSバージョン</td>
    <td><em>設定不要</em></td>
    </tr>
    <tr class="odd">
    <td>接続URL</td>
    <td><p><span class="underline">https://OICのホスト名</span></p>
    <p>ブラウザのURLのホスト名をそのまま利用します</p></td>
    </tr>
    </tbody>
    </table>

1. **セキュリティの構成** をクリックし、次の値を入力します。

    | **フィールド**         | **入力値**                        |
    |------------------------|-----------------------------------|
    | セキュリティ・ポリシー | Basic認証                         |
    | Username               | OICのログイン・ユーザー  |
    | Password               | OICのログイン・パスワード |

1. 接続できることを検証するため、**テスト** ボタンをクリックします。画面上部に "正常にテストされました。"のメッセージが表示され、インジケータが100%になっていれば、接続の構成は完了です。

    ![image12](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image12.png)

1. **保存**をクリックして接続を保存し、**閉じる**をクリックして接続の編集画面を閉じます。

−−−−−−−−

## 連携フローの定義

### OICの統合メタデータを取得

連携フローを開発するために、REST APIを呼び出した際のレスポンス・ペイロードを収集しておく必要があります。今回はOICの統合メタデータを取得するREST APIを利用しますので、このAPIのレスポンス・ペイロードを取得しておきます。

あわせて、OICが提供する運用向けREST APIの利用方法についても理解してください。

1. メニューで**統合**をクリックします。

    ![image13](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image13.png)

1. 右のトグル・ボタンがチェック（緑色）されている統合を１つ（何でも良い）選び、統合名をクリックします。以下ではHello worldという統合を例に説明します。

    > （オプション）もしチェックされてる統合が１つも無い（トグル・ボタンが全て灰色）場合はHello Worldのトグル・ボタンをクリックしアクティブ化してください。**統合のアクティブ化**ウインドウがポップアップするので**アクティブ化**をクリックします。暫くするとトグル・ボタンが灰色から緑色に変わります。
    >
    > ![image14](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image14.png)
    > ![image15](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image15.png)
    > ![image16](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image16.png)

1. 画面右のメニューアイコンをクリック、**プライマリ情報**のリンクをクリックします。

    ![image17](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image17.png)

1. **識別子**と**バージョン**情報をメモしておきます。

    ![image18](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image18.png)

1. **閉じる**をクリックします。

1. REST APIテストクライアントを起動します（本ハンズオンでは、Advanced REST Clientを利用した手順を記載します）。

1. Request URLに次の通りURIを入力します。

    ```http
    https://OICホスト名/ic/api/integration/v1/integrations/{id}
    ```

    <table>
    <thead>
    <tr class="header">
    <th><strong>Name</strong></th>
    <th><strong>Value</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>id</td>
    <td><p>&lt;確認した統合の識別子&gt;|&lt;確認した統合のバージョン番号&gt;</p>
    <p>例えば、識別子がHELLO_WORLD、バージョンが01.02.0000の場合は、idはHELLO_WORLD|01.02.0000</p></td>
    </tr>
    </tbody>
    </table>

1. **SEND**ボタンを押すと、認証情報を求めるポップアップが現れます。下表の通り設定し、**ACCEPT**をクリックします。

    | **フィールド** | **入力値**                        |
    |----------------|-----------------------------------|
    | Username       | OICのログイン・ユーザー  |
    | Password       | OICのログイン・パスワード |

    ![image19](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image19.png)

1. レスポンスのペイロードをダウンロードし、JSONファイルとして保存しておきます。

    ![image20](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image20.png)

--------

## 統合の作成

1. メニューで**統合**をクリックします。

    ![image21](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image21.png)

1. **作成**をクリックし、ダイアログで**基本ルーティング**を選択します。

    ![image22](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image22.png)

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
    <td>統合にどのような名前を付けますか。（統合名）</td>
    <td>Get Integration Metadata &lt;user name&gt;</td>
    </tr>
    <tr class="even">
    <td>識別子</td>
    <td><p><em>入力不要</em></p>
    <p>Integration Nameから自動的に導出されます:<br />
    GET_INTEGRAT_METADATA_&lt;USER NAME&gt;</p></td>
    </tr>
    <tr class="odd">
    <td>バージョン</td>
    <td><p><em>入力不要</em></p>
    <p>デフォルト値を使用します: 01.00.0000</p></td>
    </tr>
    <tr class="even">
    <td>この統合では何がおこなわれますか。（説明）</td>
    <td><em>入力不要</em></td>
    </tr>
    <tr class="odd">
    <td>この統合はどのパッケージに属しますか。（パッケージ名）</td>
    <td><em>入力不要</em></td>
    </tr>
    </tbody>
    </table>

1. 画面右側の接続一覧から、先ほど作成した接続を選択し、**トリガーのドラッグ・アンド・ドロップへドラッグします。**

    ![image23](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image23.png)

1. 基本情報の設定画面で次の通りに入力し**次 &gt;**をクリックします。本演習では指示がないフィールドはデフォルトのままとしてください。

    | **フィールド**                                   | **入力値**            |
    |--------------------------------------------------|-----------------------|
    | エンドポイントにどのような名前を付けますか。     | REST-Proxy            |
    | このエンドポイントで何が行われますか。           | *任意の値*            |
    | エンドポイントの相対リソースURLは何ですか。      | /integration-metadata |
    | エンドポイントでどのアクションを実行しますか。 | GET                   |
    | このエンドポイントのパラメータを追加して確認     | チェック              |
    | このエンドポイントを構成してレスポンスを受信     | チェック              |

1. リクエスト・パラメータの設定画面で次のパラメータを追加し**次 &gt;**をクリックします。

    | **名前** | **データ型** |
    |----------|--------------|
    | id       | string       |

1. **レスポンス・ペイロード書式を選択**にて、レスポンス・ペイロード・ファイルとして**JSONサンプル**を選択します。**サンプルの場所を指定**、もしくは**enter sample JSON**が表示されるので、**enter sample JSON**の右にある **&lt;&lt;&lt;inline&gt;&gt;&gt;** をクリックして、以下のJSONメッセージを貼り付けます。

    ```javascript
    {
    "code": "HELLO_WORLD",
    "compatible": true,
    "created": "2018-04-17T12:04:36.115+0000",
    "createdBy": "ICSAdmin",
    "description": "Example describing simple log and notification actions.",
    "endPointURI": "https://<host>:<port>/flows/rest/HELLO_WORLD/1.0/metadata"
    }
    ```

1. ダイアログ画面で**次 &gt;**をクリックすると、以下のようなサマリー画面に到達しますので、**完了**をクリックします。

    ![image24](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image24.png)

1. 呼び出し側も同様に、先ほど作成したREST接続を**呼出しのドラッグ・アンド・ドロップ**へドラッグします。

    ![image25](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image25.png)

1. 基本情報の設定画面で例えば次のように入力し**次 &gt;**をクリックします。本演習では指示がないフィールドはデフォルトのままとしてください。

    | **フィールド**                                   | **入力値**                               |
    |--------------------------------------------------|------------------------------------------|
    | エンドポイントにどのような名前を付けますか。     | RetrieveMetadata                         |
    | このエンドポイントで何が行われますか。           | *入力不要*                               |
    | エンドポイントの相対リソースURLは何ですか。      | /ic/api/integration/v1/integrations/{id} |
    | エンドポイントでえどのアクションを実行しますか。 | GET                                      |
    | このエンドポイントのパラメータを追加して確認     | *入力不要*（チェック）                   |
    | このエンドポイントを構成してレスポンスを受信     | チェック                                 |

1. リクエスト・パラメータの設定画面ではそのまま**次 &gt;**をクリックします。

1. レスポンス・ペイロードの設定画面ではレスポンス・ペイロード・ファイルとして**JSONサンプル**を選択し、**ファイルを選択**をクリックして、JSONレスポンス・ペイロードの取得の項で保存したJSONファイルを選択します。取り込みが完了したら**次 &gt;**をクリックします。

1. サマリーの画面で**完了**をクリックします。

--------

### データ・マッピングとアクティブ化

REST-ProxyからRetrieveMetadataにリクエストデータを受け渡すときのデータ・マッピングと、RetrieveMetadataからREST-Proxyにレスポンスデータを受け渡すときのデータ・マッピングを、それぞれ定義します。

1. **リクエスト・マッピング**のアイコンをクリックし、**＋**アイコンを押してデータ・マッパーを起動します。

    ![image26](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image26.png)

1. 次のソース・フィールドを対応するターゲット・フィールドへマッピングします。ソース・フィールドをドラッグして、ターゲット・フィールドのテキストと重なるように調整します。

    | **ソース・フィールド**               | **ターゲット・フィールド**              |
    |--------------------------------------|-----------------------------------------|
    | execute &gt; QueryParameters &gt; id | execute &gt; TemplateParameters &gt; id |

    ![image27](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image27.png)

1. **検証**を押し、問題ないことを確認後、**閉じる**をクリックします。

    ![image28](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image28.png)

1. **レスポンス・マッピング**のアイコンをクリックし、**＋**アイコンを押してデータ・マッパーを起動します。

    ![image29](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image29.png)

1. 次のソース・フィールドを対応するターゲット・フィールドへマッピングします。

    | **ソース・フィールド**                                 | **ターゲット・フィールド**                             |
    |--------------------------------------------------------|--------------------------------------------------------|
    | executeResponse &gt; response-wrapper &gt; code        | executeResponse &gt; response-wrapper &gt; code        |
    | executeResponse &gt; response-wrapper &gt; compatible  | executeResponse &gt; response-wrapper &gt; compatible  |
    | executeResponse &gt; response-wrapper &gt; created     | executeResponse &gt; response-wrapper &gt; created     |
    | executeResponse &gt; response-wrapper &gt; createdBy   | executeResponse &gt; response-wrapper &gt; createdBy   |
    | executeResponse &gt; response-wrapper &gt; description | executeResponse &gt; response-wrapper &gt; description |
    | executeResponse &gt; response-wrapper &gt; endPointURI | executeResponse &gt; response-wrapper &gt; endPointURI |

1. マッピングが完了すると、画面が次のようになります。

    ![image30](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image30.png)

1. **検証**をクリックし、問題ないことを確認後、**閉じる**をクリックします。

1. 最後にメッセージ・トラッキングの設定をします。画面右上のメニューを開き、**トラッキング**をクリックします。

    ![image31](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image31.png)

1. ソースのフィールド一覧から**id**をドラッグし、**トラッキング・フィールド**へドロップします。そして**保存**をクリックします。

    ![image32](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image32.png)

1. これで統合の設定が完了です。**右上のインジケータ**が100%になっていることを確認し、**保存**、**閉じる**をクリックします。

1. 統合の設定が100%完了すると、統合をアクティブにするためのトグル・ボタンが表示されます。**トグル・ボタン**をクリックし、統合をアクティブ化します。

    ![image33](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image33.png)

1. 確認のダイアログでは何も設定せず、そのまま、**アクティブ化**をクリックします。アクティベーションのプロセスが完了するのを待ちます。

--------

### 統合のテストとモニタリング

1. 歯車のアイコンをクリックして**エンドポイントURL**のリンクをクリックします。

    ![image34](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image34.png)

1. 新しいタブが開きます（認証を求められる場合はOICへのログイン情報を入力します）。このページでは、リソース・エンドポイントやメソッド、必要なパラメータ情報などを確認してください。

    - リソース・エンドポイントやメソッド、必要なパラメータ情報
    - トリガー側への設定が、このREST APIの起動方法に反映されていること
    - レスポンス・ペイロードとしてアップロードしたJSONファイルが、サンプル・ペイロードとして使われていること

1. Swaggerのリンクをクリックすると、メタデータをOpenAPI 2.0（Swagger）形式で取得できます。

    ![image35](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image35.png)

1. OpenAPI 2.0 (Swagger)形式

    ![image36](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image36.png)

1. エンドポイントを、RESTメッセージの送信できるアプリケーションでテストします。

**Advanced REST clientを使った手順:**

1. エンドポイントのURLをコピーし、Request URLに貼り付けします。このとき、以下のようにパラメータの記載を変更してください。

    ![image37](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image37.png)

    | **変更前** | **変更後** |
    |------------|------------|
    | \[id\]     | ${id}      |

1. **Variables**タブの**ADD VARIABLE**をクリックします。

    ![image38](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image38.png)

1. リクエスト・パラメータを次の通り設定します。

    <table>
    <thead>
    <tr class="header">
    <th><strong>Name</strong></th>
    <th><strong>Value</strong></th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>id</td>
    <td>GET_INTEGRAT_METADATA_&lt;USER NAME&gt;|01.00.0000<br />
    <em><strong>* tips</strong></em>: 自分で作成した統合のメタデータを取得します</td>
    </tr>
    </tbody>
    </table>

1. **SEND**をクリックして作成したREST APIを呼び出します。JSON形式で期待するレスポンスを受け取っていることを確認してください。

    ![image39](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image39.png)

1. モニタリング画面から統合の成功を確認します。OICのホーム画面に移動し、下方にスクロールすると、OIC全体のモニタリング結果を確認できます。

    ![image40](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image40.png)

1. **Monitoring**ボタンをクリックして、詳細を確認します。

    ![image41](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image41.png)

1. 先ほどテストで実行したメッセージの結果を確認するには、メニューで**トラッキング**をクリックします。

    ![image42](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image42.png)

1. 統合が正常に完了していることを確認します。このとき、メッセージ・トラッキングとして設定したフィールド（id）の値が表示されていることを確認します。

    ![image44](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image44.png)

1. トラッキング・ペイロードのリンクをクリックすると、連携の実行結果を確認できます。エラーが発生した場合には発生個所の特定に利用できます。

    **成功した場合**

    ![image46](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image46.png)

    **失敗した場合**

    ![image47](https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image47.png)

--------

以上でIntegrationのチュートリアルは終了です。
