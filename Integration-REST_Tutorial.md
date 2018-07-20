# Integration Tutorial (REST)

- [はじめに](#はじめに)
  - [目的](#目的)
  - [最終構成](#最終構成)
  - [前提](#前提)
- [接続の作成](#接続の作成)
- [連携フローの定義](#連携フローの定義)
  - [サンプルのレスポンスペイロードの取得](#サンプルのレスポンスペイロードの取得)
- [統合の作成](#統合の作成)
  - [マッピングとアクティブ化](#マッピングとアクティブ化)
  - [統合のテストとモニタリング](#統合のテストとモニタリング)

------

## はじめに

### 目的

本ハンズオンでは、既存のREST APIに対して、Oracle Integration Cloud（以下、OIC）で新たなRESTエンドポイントを構成・公開するために、REST to RESTの連携フローを実装します。

このハンズオンを通じて以下のポイントを理解することができます。

- Source側RESTの実装: REST/JSONでエンドポイントを構成して公開する方法
- Target側RESTの実装: REST APIを呼び出すための設定方法
- JSONデータの構造変換、マッピングする方法
- REST/JSONで構成されたOICエンドポイントのテストする方法

なお本ハンズオンでは、既存のREST APIとして、OICが運用・管理用途で公開しているREST API（Integrationのメタデータを取得するAPI）を利用します。

------

### 最終構成

本ハンズオンの最終構成を以下に示します。

- Target側のREST APIとして、OICの運用・管理用REST APIを利用する
- ハンズオンで作成したOICのエンドポイントへのリクエストは、Target側のREST APIに渡す
- Target側のREST APIからのレスポンスを編集して、呼び出し元に返す

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image3.png" style="width:7.05941in;height:1.37985in" />

完成版をエクスポートしたものは[こちら](Integration-REST_Tutorial.solution.iar)にあります。

------
### 前提

OICでは運用・管理を目的としたREST APIを提供しており、本ハンズオンでは、その中のOICの統合のメタデータを取得するAPIを使用します。OICの運用・管理用のREST APIの一覧は、以下のドキュメントをご覧ください。

- Autonomous Integration Cloud
> [https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/index.html](https://docs.oracle.com/en/cloud/paas/integration-cloud/rest-api/index.html)

- Integration Cloud (User managed)
> [https://docs.oracle.com/en/cloud/paas/integration-cloud-um/rest-api-um/index.html](https://docs.oracle.com/en/cloud/paas/integration-cloud-um/rest-api-um/index.html)

本ハンズオンを実施する前に、アクティブな統合を用意する必要があります。

動作確認を簡単にするため、REST APIを呼び出すためのツール（REST APIテストクライアント）を用意してください。

------

## 接続の作成

RESTアダプタを使用して、REST接続を定義します。
RESTアダプタでAPIのURLや認証情報を登録すると、REST APIへ接続できるようになります。

OICのホーム画面で **Integrations** のページに移動します。
> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image7.png" style="width:2.43564in;height:2.63671in" />

メニューで **接続** をクリックします。
> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image8.png" style="width:2.14863in;height:2.27241in" />

**作成** をクリックします。
> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image9.png" style="width:7.0297in;height:0.8683in" />

**REST**アダプタを選択します。
> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image10.png" />

次の接続情報を入力します。

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

**作成** をクリックします。
> （オプション）電子メール・アドレス に管理者用の E-Mailアドレスを入力します（例：admin@foo.bar.com）

**接続の構成** をクリックし、次の接続プロパティを設定します。

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
<td><p>https://OICのホスト名</p>
<p>ブラウザに現れるOICのホスト名をそのまま利用します</p></td>
</tr>
</tbody>
</table>

**［セキュリティの構成］** をクリックし、次の値を入力します。

| **フィールド**         | **入力値**                        |
|------------------------|-----------------------------------|
| セキュリティ・ポリシー | Basic Authentication              |
| Username               | OICのログイン・ユーザー   |
| Password               | OICのログイン・パスワード |

接続できることを検証するため、**テスト** ボタンをクリックします。画面上部に "正常にテストされました。"のメッセージが表示され、インジケータが100%になっていれば、接続の構成は完了です。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image11.png" style="width:7.0297in;height:0.86114in" />

**保存** をクリックして接続を保存し、**閉じる** をクリックして接続の編集画面を閉じます。

------

## 連携フローの定義

### サンプルのレスポンスペイロードの取得

連携フローを開発するために、REST APIを呼び出した際のレスポンス・ペイロードを収集しておく必要があります。今回はOICの統合メタデータを取得するREST APIを利用しますので、このAPIのレスポンス・ペイロードを事前に取得します。

あわせて、OICが提供する運用向けREST APIの利用方法についても理解してください。

メニューで **統合** をクリックします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image12.png" style="width:2.13861in;height:1.75545in" />

右のトグル・ボタンがチェックされている統合を１つ（何でも良い）選び、統合名をクリックします。以下ではHello worldという統合を例に説明します。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image13.png" style="width:7.04951in;height:1.04176in" />

画面右のメニューアイコンをクリック、**プライマリ情報** のリンクをクリックします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image14.png" style="width:2.86012in;height:2.15842in" />

**識別子** と **バージョン** 情報をメモしておきます。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image15.png" style="width:3.78125in;height:2.00234in" />

**閉じる** をクリックします。

REST APIテストクライアントを起動し、Request URLとして以下のURIを入力します。
```
  https://OICのホスト名/ic/api/integration/v1/integrations/{id}
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

このAPIは基本認証を必要とします。OICのログインに利用するユーザー、パスワードを指定します。

| **フィールド** | **入力値**                        |
|----------------|-----------------------------------|
| Username       | OICのログイン・ユーザー   |
| Password       | OICのログイン・パスワード |

レスポンスのペイロードをダウンロードし、JSONファイルとして保存しておきます。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image17.png" style="width:7.06535in;height:4.41584in" />

------

## 統合の作成

メニューで **統合** をクリックします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image18.png" style="width:2.70833in;height:2.01951in" />

**作成** をクリックし、ダイアログで **基本ルーティング** を選択します。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image19.png" style="width:4.25151in;height:4.5625in" />

次の情報を入力して、**作成** をクリックします。

<table>
<thead>
<tr class="header">
<th><strong>フィールド</strong></th>
<th><strong>入力値</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>統合名</td>
<td>Get Integration Metadata &lt;user name&gt;</td>
</tr>
<tr class="even">
<td>識別子</td>
<td><p><em>入力不要</em></p>
<p>Integration Nameから自動的に導出:<br />
GET_INTEGRAT_METADATA_&lt;USER NAME&gt;</p></td>
</tr>
<tr class="odd">
<td>バージョン</td>
<td><p><em>入力不要</em></p>
<p>デフォルト値を使用: 01.00.0000</p></td>
</tr>
<tr class="even">
<td>パッケージ名</td>
<td><em>入力不要</em></td>
</tr>
<tr class="odd">
<td>説明</td>
<td><em>入力不要</em></td>
</tr>
</tbody>
</table>

画面右側の接続一覧から、先ほど作成した接続を選択し、**トリガーのドラッグ・アンド・ドロップ**へドラッグします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image20.png" style="width:6.94943in;height:4.08298in" />

基本情報の設定画面で次の通りに入力し **次 &gt;** をクリックします。

<table>
<thead>
<tr class="header">
<th><strong>フィールド</strong></th>
<th><strong>入力値</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>エンドポイントにどのような名前をつけるか</td>
<td>REST-Proxy</td>
</tr>
<tr class="even">
<td>エンドポイントで何が行われるか</td>
<td><i>任意の値</i></td>
</tr>
<tr class="odd">
<td>エンドポイントの相対リソースURL</td>
<td>/integration-metadata</td>
</tr>
<tr class="even">
<td>エンドポイントが実行するアクション</td>
<td><em>GET</em></td>
</tr>
<tr class="odd">
<td>エンドポイントのパラメータを追加して確認</td>
<td><em>チェック</em></td>
</tr>
<tr class="even">
<td>エンドポイントを構成してレスポンスを受信</td>
<td><em>チェック</em></td>
</tr>
</tbody>
</table>

リクエスト・パラメータの設定画面で次のパラメータを追加し**［次 &gt;］**をクリックします。

| **名前** | **データ型** |
|----------|--------------|
| id       | string       |

**レスポンス・ペイロード書式を選択** にて、レスポンス・ペイロード・ファイルとして **JSONサンプル** を選択します。**サンプルの場所を指定**、もしくは **enter sample JSON** が表示されるので、**enter sample JSON** の右にある **&lt;&lt;&lt;inline&gt;&gt;&gt;** をクリックして、以下のJSONメッセージを貼り付けます。

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

ダイアログ画面で **次 >** をクリックすると、以下のようなサマリー画面に到達しますので、 **完了** をクリックします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image21.png" style="width:6.0865in;height:4.57953in" />

呼び出し側も同様に、先ほど作成したREST接続を **呼出しのドラッグ・アンド・ドロップ** へドラッグします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image22.png" style="width:6.72277in;height:3.97098in" />

基本情報の設定画面で例えば次のように入力し **次 &gt;** をクリックします。

| **フィールド**                           | **入力値**                               |
|------------------------------------------|------------------------------------------|
| エンドポイントにどのような名前をつけるか | RetrieveMetadata                         |
| エンドポイントで何が行われるか           | *入力不要*                               |
| エンドポイントの相対リソースURL          | /ic/api/integration/v1/integrations/{id} |
| エンドポイントが実行するアクション       | GET                                      |
| エンドポイントのパラメータを追加         | *入力不要*（チェック）                   |
| エンドポイントを構成してレスポンスを受信 | チェック                                 |

リクエスト・パラメータの設定画面ではそのまま **次 &gt;** をクリックします。

レスポンス・ペイロードの設定画面ではレスポンス・ペイロード・ファイルとして **JSONサンプル** を選択し、**ファイルを選択** をクリックして、JSONレスポンス・ペイロードの取得の項で保存したJSONファイルを選択します。取り込みが完了したら **次 &gt;** をクリックします。

サマリーの画面で **完了** をクリックします。

------

### マッピングとアクティブ化

REST-ProxyからRetrieveMetadataにリクエストデータを受け渡すときのデータ・マッピングと、RetrieveMetadataからREST-Proxyにレスポンスデータを受け渡すときのデータ・マッピングを、それぞれ定義します。

**リクエスト・マッピング** のアイコンをクリックし、**＋**アイコンを押してデータ・マッパーを起動します。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image23.png" style="width:7.05796in;height:3.31636in" />

次のソース・フィールドを対応するターゲット・フィールドへマッピングします。ソース・フィールドをドラッグして、ターゲット・フィールドのテキストと重なるように調整します。

| **ソース・フィールド**               | **ターゲット・フィールド**              |
|--------------------------------------|-----------------------------------------|
| execute &gt; QueryParameters &gt; id | execute &gt; TemplateParameters &gt; id |

**検証**を押し、問題ないことを確認後、 **閉じる** をクリックします。

**レスポンス・マッピング**のアイコンをクリックし、**＋**アイコンを押してデータ・マッパーを起動します。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image24.png" />

次のソース・フィールドを対応するターゲット・フィールドへマッピングします。

| **ソース・フィールド**                                 | **ターゲット・フィールド**                             |
|--------------------------------------------------------|--------------------------------------------------------|
| executeResponse &gt; response-wrapper &gt; code        | executeResponse &gt; response-wrapper &gt; code        |
| executeResponse &gt; response-wrapper &gt; compatible  | executeResponse &gt; response-wrapper &gt; compatible  |
| executeResponse &gt; response-wrapper &gt; created     | executeResponse &gt; response-wrapper &gt; created     |
| executeResponse &gt; response-wrapper &gt; createdBy   | executeResponse &gt; response-wrapper &gt; createdBy   |
| executeResponse &gt; response-wrapper &gt; description | executeResponse &gt; response-wrapper &gt; description |
| executeResponse &gt; response-wrapper &gt; endPointURI | executeResponse &gt; response-wrapper &gt; endPointURI |

マッピングが完了すると、画面が次のようになります。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image25.png" style="width:6.9375in;height:2.9555in" />

**検証** をクリックし、問題ないことを確認後、**閉じる** をクリックします。

最後にメッセージ・トラッキングの設定をします。画面右上のメニューを開き、 **トラッキング** をクリックします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image26.png" style="width:2.75in;height:2.0113in" />

ソースのフィールド一覧から **id** をドラッグし、 **トラッキング・フィールド** へドロップします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image27.png" style="width:5.26339in;height:2.32126in" />

これで統合の設定が完了です。右上の **インジケータ** が100%になっていることを確認し、**保存**、**閉じる**をクリックします。

統合の設定が100%完了すると、統合をアクティブにするためのトグル・ボタンが表示されます。**トグル・ボタン**をクリックし、統合をアクティブ化します。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image28.png" style="width:6.75551in;height:1.22953in" />

確認のダイアログでは何も設定せず、そのまま、**アクティブ化**をクリックします。アクティベーションのプロセスが完了するのを待ちます。

------

### 統合のテストとモニタリング

歯車のアイコンをクリックして **エンドポイントURL** のリンクをクリックします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image29.png" style="width:6.75472in;height:1.21339in" />

新しいタブが開きます。（認証を求められる場合はOICへのログイン情報を入力します。）このページで以下の内容を確認してください。

- リソース・エンドポイントやメソッド、必要なパラメータ情報
- トリガー側への設定が、このREST APIの起動方法に反映されていること
- レスポンス・ペイロードとしてアップロードしたJSONファイルが、サンプル・ペイロードとして使われていること

開いたブラウザ・タブのURLの末尾に、 **/swagger** を付加すると、メタデータをOpenAPI 2.0（Swagger）形式で取得できます。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image30.png" style="width:6.62008in;height:3.92283in" />

エンドポイントを、RESTメッセージの送信できるアプリケーションでテストします。エンドポイントのURLをコピーし、Request URLとして貼り付けます。下表に従いQuery Parameterの値を設定し、REST APIを呼び出します。
  
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

JSON形式で期待するレスポンスを受け取っていることを確認してください。
> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image33.png" alt="C:\Users\ANISHIKA\AppData\Local\Microsoft\Windows\INetCache\Content.Word\スクリーンショット 2018-06-12 19.13.08.png" style="width:6.89583in;height:4.3099in" />

モニタリング画面から統合の成功を確認します。OICのホーム画面に移動し、下方にスクロールすると、OIC全体のモニタリング結果を確認できます。

**Monitoring** ボタンをクリックして、詳細を確認します。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image35.png"/>

先ほどテストで実行したメッセージの結果を確認するには、メニューで **トラッキング** をクリックします。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image36.png" style="width:7.06535in;height:4.41584in" />

統合が正常に完了していることを確認します。このとき、メッセージ・トラッキングとして設定したフィールド（id）の値が表示されていることを確認します。

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image37.png" style="width:7.05941in;height:1.05826in" />

トラッキング・ペイロードのリンクをクリックすると、連携の実行結果を確認できます。エラーが発生した場合には発生個所の特定に利用できます。

**成功した場合**

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image38.png" style="width:7.04951in;height:1.37792in" />

**失敗した場合**

> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/Integration-REST_Tutorial/image39.png" style="width:7.0396in;height:1.33296in" />

------

以上でIntegrationのチュートリアルは終了です。