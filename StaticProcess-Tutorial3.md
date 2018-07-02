# Static Process Tutorial [3/3]

- [はじめに](#はじめに)
- [目的](#目的)
- [最終構成](#最終構成)
- [インテグレーションの用意](#インテグレーションの用意)
- [アプリケーションの更新](#アプリケーションの更新)
  - [Integrationとの統合設定](#integrationとの統合設定)
  - [プロセスからIntegrationの呼び出し](#プロセスからintegrationの呼び出し)
  - [プレーヤを使った動作確認](#プレーヤを使った動作確認)
  - [Integrationの実行確認](#integrationの実行確認)

------

## はじめに

このWorkshopは18.2.3で動作確認しています。

また、少なくともハンズオンのLab 1を実施している必要があります。

------

## 目的

- このパートでは、Tutorial 1、2で作成したProcessのアプリケーションをIntegrationと連携させる手順を学びます。
- サンプルの構成としてLab2のアプリケーションを利用しますが、Lab1のアプリケーションでもハンズオンを実施することが出来ます。

------

## 最終構成

このハンズオンでは、Tutorial 1、2にて作成したプロセスを基に、Integrationとの連携機能を作成します。最終構成は以下の通りです。

**サービス** にて、Integrationを実行します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image1.png" style="width:6.76389in;height:4.23472in" />

**サービス** にて呼びだすIntegrationは、『受け取ったリクエストデータをそのままレスポンスとして返却する』処理を行います。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image2.png" style="width:1.35423in;height:3.32584in" />

------

## インテグレーションの用意

Process Cloudと関連付けをするIntegratinを確認します。

1. ホーム画面から、Integrationsをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image3.png" style="width:3.37079in;height:3.47663in" />

2. 今回は、サンプルとして初期構築時に用意されている **Echo** を利用します。統合画面の **Echo** にあるトグル・ボタンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image4.png" style="width:5.76469in;height:1.23166in" />

3. アクティブ化をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image5.png" style="width:4.30322in;height:3.95463in" />

4. 以下の状態になれば、Integrationの準備は完了です。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image6.png" style="width:5.66277in;height:0.55988in" />

------

## アプリケーションの更新

### Integrationとの統合設定

ProcessからIntegrationを呼び出すための設定をします。

1. ホームからProcessの画面に戻り、アプリケーション＞PurchaseRequisitionをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image7.png" style="width:3.97078in;height:2.12359in" />

2. 統合＞統合の参照　をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image8.png" style="width:5.29198in;height:5.10508in" />

3. Echoを選択し、 **作成** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image9.png" style="width:4.41558in;height:4.36752in" />

------

### プロセスからIntegrationの呼び出し

プロセスからIntegrationを呼び出す設定を追加します。

1. プロセス＞PurchaseRequisitionをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image10.png" style="width:5.34397in;height:2.65169in" />

2. スイムレーンDeputy Managerにサービスアクティビティを配置します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image11.png" style="width:5.87625in;height:2.8784in" />

3. サービスアクティビティのプロパティを開きます

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image12.png" style="width:2.86517in;height:1.31006in" />

4. タイプから **サービス・コール** を選択し、右の鉛筆マークをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image13.png" style="width:3.40185in;height:2.86517in" />

5. タイプが **統合** になっていることを確認し、虫眼鏡のアイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image14.png" style="width:3.44944in;height:2.33015in" />

6. **Echo** を選択し、 **OK** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image15.png" style="width:3.51428in;height:2.47191in" />

7. サービスアクティビティの **データ・アソシエーションを開く** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image16.png" style="width:2.93258in;height:1.32159in" />

8. **Echo** に連携させるデータをマッピングします。

    今回は、プロジェクト名を **Echo** に連携し、プロジェクト概要を **Echo** から受け取るよう設定します。入力は以下のように設定します。

    - 左：PurchaseRequisition ＞ Data Object ＞ purchaseRequisitionUIDataObject ＞ projectName
    - 右：message

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image17.png" style="width:6.6285in;height:3.83146in" />

    出力は以下のように設定します。

    - 左：body ＞ message
    - 右：PurchaseRequisition ＞ Data Object ＞ purchaseRequisitionUIDataObject ＞ projectDescription

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image18.png" style="width:6.2358in;height:3.66145in" />

9. **適用** をクリックします。

10. 保存します。

------

### プレーヤを使った動作確認

再生機能を使い、作成したプロセスの動作を確認します。

1. **テスト** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image19.png" style="width:4.13483in;height:0.59051in" />

2. **アクティブ化** をクリックし、テストモードでのアクティブ化を実行します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image20.png" style="width:5.46067in;height:2.12148in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image21.png" style="width:3.88764in;height:1.77043in" />

3. 今回は再生機能を使ってプロセスの流れを確認するため、 **再生** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image22.png" style="width:5.29436in;height:0.92135in" />

4. **アプリケーション・プレーヤ** の画面で、左上のPurchaeRequisitionをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image23.png" style="width:1.47343in;height:1.34902in" />

5. 購買申請開始イベント中にある開始ボタンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image24.png" style="width:0.64143in;height:0.55111in" />

6. ユーザーを選択し、 **実行** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image25.png"  style="width:2.16692in;height:1.87003in" />

7. 画面フォームに入力し、 **送信** をクリックします。今回は、プロジェクト名をIntegrationへ連携し、プロジェクト概要として返却された値を受け取るので、プロジェクト概要は空欄にしておいてください。以下に入力例を記載します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image26.png" style="width:5.74142in;height:4.21352in" />

8. 引き続き、 **リクエストの承認** でユーザーを選択し、 **実行** をクリックし、 **フォームの起動** をクリックします。 **プロジェクト概要** に **プロジェクト名** で設定した値が入力されました。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image27.png"  style="width:0.78606in;height:0.60676in" />
    <br/>
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image28.png" style="width:5.92119in;height:4.35943in" />

    プロセスが終了するまで実行します。

------

### Integrationの実行確認

Integrationの画面を開いて、どのようなリクエストを受け付けたのか確認してみましょう。

1. ホーム画面をスクロールすると、モニタリング結果が表示されています。**Monitoring**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image29.png" style="width:5.65153in;height:2.95574in" />

2. 左のメニューから**トラッキング**をクリックします。実行結果が、**message：＜＜フォームで入力したプロジェクト名＞＞**になっていることを確認します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial3/image30.png" style="width:6.14591in;height:1.61788in" />

------

これで3パートに分かれていたStatic Processのチュートリアルはすべて終了です。
