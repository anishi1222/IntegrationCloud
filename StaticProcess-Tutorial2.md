# Static Process Tutorial [2/3]

- [はじめに](#はじめに)
- [目的](#目的)
- [最終構成](#最終構成)
- [アプリケーションの更新](#アプリケーションの更新)
  - [PurchaseRequisition Web Formの更新](#purchaserequisition-web-formの更新)
  - [ビジネスルールの作成](#ビジネスルールの作成)
  - [プロセスのアップデート](#プロセスのアップデート)
  - [デシジョン・アクティビティの実装](#デシジョンアクティビティの実装)
  - [リクエストの承認アクティビティの実装](#リクエストの承認アクティビティの実装)
  - [再送信タスクアクティビティの実装](#再送信タスクアクティビティの実装)
  - [経理部門の承認アクティビティの実装](#経理部門の承認アクティビティの実装)
  - [購買発注アクティビティの実装](#購買発注アクティビティの実装)
  - [経理部門の承認が必要か？ゲートウェイの実装](#経理部門の承認が必要かゲートウェイの実装)
  - [経理部門が承認したか？ ゲートウェイの実装](#経理部門が承認したか-ゲートウェイの実装)
  - [プレーヤを使った動作確認](#プレーヤを使った動作確認)
  - [アプリケーションのアクティブ化](#アプリケーションのアクティブ化)

## はじめに

このTutorialは18.2.3で動作確認しています。

また、StaticProcess-Tutorial1を実施済みであることを前提とします。

## 目的

このパートでは、Lab 1で作成したアプリケーションの変更を通じて、以下の機能を利用するための操作を学習します。

- ビジネス・オブジェクトの作成と利用
- フォームおよびプレゼンテーションの作成
- ビジネスルールの作成
- 承認タスクの実装
- プレーヤを使った動作確認

## 最終構成

このハンズオンでは、Lab 1にて作成したプロセスを基に、ビジネスルールの追加などを実施していきます。最終的に、以下のプロセスを作成します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image1.png" style="width:5.94399in;height:3.87952in" />

------

## アプリケーションの更新

### PurchaseRequisition Web Formの更新

現在のフォームでは、プロセスに関わるユーザー全てが追加・変更可能ですが、これを起票者以外が変更できないようにします。

1. PurchaseRequisitionをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image2.png" style="width:3.97078in;height:2.12359in" />

2. フォーム＞PurchaseRequisitionUIをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image3.png" style="width:5.21348in;height:3.1206in" />

3. 左側のプロパティ＞プレゼンテーションで＋をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image4.png" style="width:4.25843in;height:2.77891in" />

4. ダイアログで、名前に**読み取り専用**、そして基準は**メイン**を選択し、**作成**をクリックします。これにより、現在利用しているフォームを元に新しい画面を作成することができます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image5.png" style="width:2.11469in;height:2.8427in" />

5. **作成**をクリックすると、フォーム自体は元のものと変わりませんが、操作対象が**読み取り専用**に変わっていることがわかります。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image6.png" style="width:3.16854in;height:1.70896in" />

6. 各コンポーネントのプロパティで、読取り専用のチェックをONにします。設定変更の対象は、以下の赤枠で囲んだコンポーネントです。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image7.png" style="width:5.87817in;height:6.23013in" />

7. 表では、データの追加・削除ができないように、**ユーザーは行を追加/削除できます**のチェックを外しておきます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image8.png" style="width:3.57303in;height:1.31873in" />

8. 設定変更が終了したら、保存しておきましょう。

9. プロセス＞PurchaseRequisitionプロセスを開きます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image9.png" style="width:5.34397in;height:2.65169in" />

10. リクエストの承認アクティビティのフォームを先ほど作成した読み取り専用フォームに変更します。アクティビティをクリックして、**プロパティを開く**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image10.png" style="width:2.85393in;height:1.55746in" />

11. **プレゼンテーション**をメインから読み取り専用に切り替えます。フォームの表示内容自体に変更はないため、データ・アソシエーションの変更は必要ありません。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image11.png" style="width:4.41573in;height:3.32993in" />

12. **保存**しておきましょう。

------

### ビジネスルールの作成

金額の大小でもう一段経理部門でチェックをするか、それとも購買部門にそのまま流すかを決定するビジネスルールを定義します。作成する前に、スイムレーンを追加します。

1. Deputy Managerスイムレーンの下にある＋をクリックし、スイムレーンを追加します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image12.png" style="width:1.03236in;height:0.65486in" />

<!-- -->

1. スイムレーンの名前はFinanceとします。もう一つスイムレーンを追加し、そちらはPurchasingという名前にします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image13.png" style="width:5.83729in;height:4.50562in" />

続いて、デシジョンテーブルを作成します。

1. アプリケーションを閉じ、右上の**作成**を再度クリックします。このとき、\[新規デシジョン・モデル\]を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image14.png" style="width:6.19101in;height:2.14524in" />

2. 名前をFinancialApprovalとして、**作成**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image15.png" style="width:2.92243in;height:3.25843in" />

3. DMN作成画面が表示されます。もし表示されない場合は、リロードしてください。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image16.png" style="width:5.77528in;height:3.24103in" />

4. 上図右上の＋をクリックして、Decisionを追加します。名前はrequiresFinancialApprovalとします。Output
    Typeはbooleanを選択し、ロジックはIf-then-elseを選択して、**Create**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image17.png" style="width:4.52809in;height:2.83819in" />

    Createをクリックすると、以下のような画面が現れます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image18.png" style="width:5.33328in;height:3.67416in" />

5. 上図の右側、INPUT DATAをクリックして展開し、Input
    Dataの＋をクリックして入力データを追加します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image19.png" style="width:1.88363in;height:0.56056in" />

6. 以下のデータを追加し、**OK**をクリックします。

    | Name  | Data Types |
    |-------|------------|
    | total | number     |

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image20.png" style="width:3.65169in;height:1.48772in" />

7. 入力データを作成後、判定条件を作成します。以下のように設定します。

    |      |                   |
    |------|-------------------|
    | If   | total &gt;= 50000 |
    | Then | true              |
    | else | false             |

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image21.png" style="width:3.32529in;height:3.62921in" />

8. 作成したルールを外部から呼び出せるようにします。画面左側のSERVICESをクリックして展開し、＋をクリックしてサービスを追加します。名前はCheckFinancialApprovalServiceとし、OKをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image22.png" style="width:5.64267in;height:2.98876in" />

9. Output Decisions、Input Dataに対し、それぞれrequresFinancialApproval、totalをマッピングします。以下のように、Drag & Dropしてください。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image23.png" style="width:6.0729in;height:3.24719in" />

    Drag & Dropの結果、画面上は以下のように設定されているはずです。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image24.png" style="width:3.15265in;height:3.55056in" />

10. **保存**します。

11. 外部からルールを呼び出せるよう、アクティブ化を実施します。画面右上の**アクティブ化**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image25.png" style="width:1.94444in;height:0.63889in" />

12. スナップショット名を指定し、バージョンを設定の上、アクティブ化します。今回は初期設定のままでかまいません。**上書き**のチェックボックスはONにし、**アクティブ化**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image26.png" style="width:3.13483in;height:3.25122in" />

------

### プロセスのアップデート

先ほど作成したルールを追加します。さらに、判定の結果経理部門のチェックを実施するため、承認アクティビティを追加します。

各部品の詳細については、以下ガイドを参照してください。

> **Oracle Integration Cloud** : [
https://docs.oracle.com/en/cloud/paas/integration-cloud-um/user-processes-um/controlling-process-flow-using-gateways.html\#GUID-9E5FAFE6-01D2-45A4-9505-2A885DD18BDB](https://docs.oracle.com/en/cloud/paas/integration-cloud-um/user-processes-um/controlling-process-flow-using-gateways.html#GUID-9E5FAFE6-01D2-45A4-9505-2A885DD18BDB)

> **Autonomous Integration Cloud** : [
https://docs.oracle.com/en/cloud/paas/integration-cloud/user-processes/working-elements.html\#GUID-93DCE0C6-9C2A-4F07-B33D-ABD8357954BB](https://docs.oracle.com/en/cloud/paas/integration-cloud/user-processes/working-elements.html#GUID-93DCE0C6-9C2A-4F07-B33D-ABD8357954BB)

1. アプリケーション・ホーム＞プロセスでPurchaseRequisitionをクリックします。

2. 履行アクティビティをクリックし、上部のメニューの**タイプの変更**をクリックして、赤枠で囲んだデシジョン・アクティビティを選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image27.png" style="width:5.28973in;height:3.21348in" />

3. ラベル名を**経理部門の承認要否**に変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image28.png" style="width:0.88545in;height:0.59206in" />

4. アクティビティのプロパティを開き、以下の操作を実行します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image29.png" style="width:2.13363in;height:0.97738in" />

    1. ドラフト状態の解除

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image30.png" style="width:3.77528in;height:0.98327in" />

    2. デシジョン・モデルの＋をクリックし、FinancialApprovalを選択し、**使用** をクリック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image31.png" style="width:3.20756in;height:3.40449in" />

5. **経理部門の承認要否** アクティビティの後に、排他ゲートウェイを配置し、名前を **経理部門の承認が必要か？** とします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image32.png" style="width:4.73034in;height:2.43122in" />

6. スイムレーンFinanceに承認アクティビティを配置します。アクティビティの名前を**経理部門の承認**とします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image33.png" style="width:6.13712in;height:3.2809in" />

7. **経理部門の承認が必要か？** ゲートウェイを選択し、シーケンスフロー（矢印）を経理部門の承認アクティビティへつなぎます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image34.png" style="width:2.45889in;height:2.69663in" />

8. **経理部門の承認要否** アクティビティ、 **経理部門の承認が必要か？** ゲートウェイをスイムレーンFinanceに移動します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image35.png" style="width:5.4155in;height:1.61798in" />

9. スイムレーンPurchasingに送信アクティビティを配置し、名前を**購買発注**とします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image36.png" style="width:1.72912in;height:1.03254in" />

10. **経理部門の承認が必要か？** ゲートウェイから終了イベント“完了”へのデフォルト・シーケンスフローを削除し、改めて **経理部門の承認が必要か？** ゲートウェイから **購買発注** アクティビティに対してデフォルト・シーケンスフローを接続します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image37.png" style="width:50%;height:50%" />
    
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image38.png" style="width:50%;height:50%" />

    > デフォルト・シーケンスフローは、矢印の根元に斜線があります（条件シーケンスフローには斜線は入りません）。このフローは、条件分岐で設定された条件を全て満たさなかった場合に進むシーケンスフローを意味しています。
    > 条件シーケンスフローとデフォルト・シーケンスフローとの切り替えは、シーケンスフローをクリックして鉛筆アイコンをクリックし、プロパティ画面で、条件フローのチェックをON/OFFします。チェックを外すと、デフォルト・シーケンスフローとして取り扱います。
    > <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image39.png" style="width:75%;height:75%"/>

11. **購買発注アクティビティ** と終了イベント“完了”間をシーケンスフローで接続します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image40.png" style="width:1.55933in;height:2.74157in" />

12. **経理部門の承認** アクティビティの後に排他ゲートウェイを配置し、名前を **経理部門が承認したか？** とします。 **経理部門の承認** アクティビティと、この排他ゲートウェイをシーケンスフローでつなぎます

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image41.png" style="width:3.01154in;height:1.4382in" />

13. **経理部門が承認したか？** ゲートウェイと **購買発注** アクティビティ間を条件シーケンスフローで接続します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image42.png" style="width:1.49495in;height:2.61798in" />

14. **経理部門が承認したか？** ゲートウェイと再送信アクティビティ間をデフォルト・シーケンスフローで接続します。

    レイアウトを整えると、画面上下図のようなフローができあがっているはずです。以後で追加したアクティビティに実装します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image43.png" style="width:7.24806in;height:4.31461in" />

------

### デシジョン・アクティビティの実装

スクリーンショットの流れに従って、ルールによる判断の実装を追加します。

1. 経理部門による承認が必要か？アクティビティをクリックして、**データ・アソシエーションを開く**を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image44.png" style="width:1.96696in;height:0.8506in" />

<!-- -->

1. 入力側、出力側で以下のようにデータ・アソシエーションを設定します。

    **入力側**

    左：PurchaseRequisition &gt; Data Object &gt; putchaseRequisitionUIDataObject &gt; total

    右：body &gt; total

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image45.png" style="width:6.21622in;height:2.5618in" />

    **出力側**

    データ・オブジェクトを作成し、そのオブジェクトにルールの判定結果を格納します。

    1. 画面右側の**データ・オブジェクト**の枠にある、PurchaseRequisition Data ＞ Data Objectを展開

    2. ＋をクリックできるようになるので、これをクリックしてデータ・オブジェクトを追加

    3. 名前はisFinancialApprovalRequired、データ型はシンプル＞ブールを選択

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image46.png" style="width:2.51685in;height:2.30383in" />

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image47.png" style="width:2.86517in;height:2.35008in" />

    4. データ・オブジェクトを作成後、以下のようにデータ・アソシエーションを設定する。設定が完了したら、**適用**を忘れずにクリックする。

        左：bodyOutput &gt; interpretation

        右：PurchaseRequisition &gt; Data Object &gt; isFinancialApprovalRequired

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image48.png" style="width:6.39922in;height:1.8764in" />

リクエストの承認アクティビティの実装
------------------------------------

1. リクエストの承認アクティビティをクリック＞**プロパティを開く**をクリックし、タイトルフィールドの右にあるドロップダウンボックスをプレーン・テキストから式エディタに変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image49.png" style="width:4.01532in;height:1.62921in" />

2. 式エディタで、  

    ```
    "プロジェクト購買申請のチェックおよび承認（" + 
    ```
    まで入力した後、PurchaseRequisitionUIを展開して、projectNameを選択し、**式に挿入**をクリックします。続けて、
    ```
     +”）”
    ```
    を入力します。すると、下図のような状態になっているはずです。

    **検証** をクリックして式に問題がないことを確認した上で **OK** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image50.png" style="width:3.51685in;height:4.27511in" />

3. プレゼンテーションはすでに読み取り専用を選択しているはずです。もし選択されていない場合には、この段階でメインではなく、読み取り専用を選択しておいてください。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image51.png" style="width:2.46236in;height:0.44415in" />

------

### 再送信タスクアクティビティの実装

1. 再送信アクティビティをクリック＞**プロパティを開く**をクリックし、タイトルフィールドの右にあるドロップダウンボックスをプレーン・テキストから式エディタに変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image49.png" style="width:3.95994in;height:1.60674in" />

2. 式エディタで、  

    ```
    "購買申請の修正（" +  
    ```

    まで入力した後、PurchaseRequisitionUIを展開して、projectNameを選択し、**式に挿入**をクリックします。続けて、

    ```
    +”）”
    ```
    を入力します。すると、下図のような状態になっているはずです。

    **検証**をクリックして式に問題がないことを確認して、**OK**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image52.png" style="width:4.03371in;height:4.87234in" />

3. データ・アソシエーション、およびフォームの定義はすでに設定済み（もしくは自動設定済み）なので、手を入れる必要はありません。

------

### 経理部門の承認アクティビティの実装

**経理部門の承認** アクティビティを実装します。

1. **経理部門の承認** アクティビティをクリック＞**プロパティを開く**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image53.png" style="width:1.55809in;height:0.86534in" />

2. フォームの右側にある虫眼鏡アイコンをクリックし、PurchaseRequisitionUIを選択し、OKをクリックします。プレゼンテーションは、読み取り専用を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image54.png" style="width:3.10854in;height:0.97753in" />

3. タイトルフィールドの右にあるドロップダウンボックスをプレーン・テキストから式エディタに変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image55.png" style="width:2.89143in;height:1.11785in" />

4. 式エディタで、  

    ```
    "経理部門でのチェックおよび承認（" +  
    ```

    まで入力した後、PurchaseRequisitionUIを展開して、projectNameを選択し、**式に挿入**をクリックします。続けて、

    ```
     +”）”
    ```
    を入力します。すると、下図のような状態になっているはずです。

    その後、**検証**をクリックし、式に問題がないことを確認して、OKをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image56.png" style="width:2.72476in;height:4.04827in" />

5. **経理部門の承認**アクティビティへのデータ・アソシエーションは自動的に設定されていますので、確認してください。

    **入力側**

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image57.png" style="width:5.77732in;height:1.68869in" />

    **出力側**

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image58.png" style="width:5.80809in;height:2.14977in" />

------

### 購買発注アクティビティの実装

1. 購買発注アクティビティをクリック＞**プロパティを開く**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image59.png" style="width:1.6875in;height:0.85732in" />

2. フォームの虫眼鏡アイコンをクリック、PurchaseRequisitionUIを選択してOKをクリックします。プレゼンテーションは読み取り専用を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image60.png" style="width:2.64143in;height:0.85233in" />

3. タイトルフィールドの右にあるドロップダウンボックスをプレーン・テキストから式エディタに変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image55.png" style="width:2.89143in;height:1.11785in" />

4. 式エディタで、

    ```
    "購買発注（" +  
    ```

    まで入力した後、PurchaseRequisitionUIを展開して、projectNameを選択し、**式に挿入**をクリックします。続けて、

    ```
    +"）"
    ```

    を入力します。すると、下図のような状態になっているはずです。

    その後、 **検証** をクリックし、式に問題がないことを確認して、OKをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image61.png" style="width:3.47732in;height:4.15986in" />

5. 購買発注アクティビティアクティビティへのデータ・アソシエーションは、経理部門の承認アクティビティと同様、自動的に設定されています。

------

### 経理部門の承認が必要か？ゲートウェイの実装

ビジネスルールの結果で条件分岐するゲートウェイを実装します。

1. 経理部門の承認が必要か？ゲートウェイから、経理部門の承認アクティビティへと繋がる条件シーケンスフローをクリックし、鉛筆アイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image62.png" style="width:1.47476in;height:0.81324in" />

2. 条件の右側にある鉛筆アイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image63.png" style="width:2.64143in;height:0.79378in" />

3. 式エディタで、PurchaseRequisitionを展開し、isFinancialApprovalRequiredを選択して、**式に挿入**をクリックし、続けて、

    ```
    == true
    ```
    を入力します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image64.png" style="width:2.73574in;height:4.22733in" />

    **検証** をクリックして式に問題がないことを確認した上で、 **OK** をクリックします。

------

### 経理部門が承認したか？ ゲートウェイの実装

経理部門の承認アクティビティの結果で条件分岐するゲートウェイ（経理部門が承認したか？）を実装します。

1. 経理部門が承認したか？ゲートウェイから購買発注アクティビティへと繋がる条件シーケンスフローをクリックして鉛筆アイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image65.png" style="width:0.99739in;height:1.03043in" />

2. 条件の右側にある鉛筆アイコンをクリックし、式エディタで、 

    ```
    TaskOutcomeDataObject == "APPROVE"
    ```

    とし、 **検証** をクリックして式に問題がないことを確認した上で、 **OK** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image66.png" style="width:3.497in;height:4.43087in" />

------

### プレーヤを使った動作確認

再生機能を使い、作成したプロセスの動作を確認します。

1. **テスト**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image67.png" style="width:4.13483in;height:0.59051in" />

<!-- -->

1. **アクティブ化**をクリックし、テストモードでのアクティブ化を実行します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image68.png" style="width:5.46067in;height:2.12148in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image69.png" style="width:3.88764in;height:1.77043in" />

2. 今回は再生機能を使ってプロセスの流れを確認するため、**再生**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image70.png" style="width:5.29436in;height:0.92135in" />

3. **アプリケーション・プレーヤ**の画面で、左上のPurchaeRequisitionをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image71.png" style="width:1.47343in;height:1.34902in" />

4. 購買申請開始イベント中にある開始ボタンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image72.png"  style="width:0.64143in;height:0.55111in" />

5. ユーザーを選択し、**実行**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image73.png" style="width:2.16692in;height:1.87003in" />

6. 画面フォームに入力し、**送信**をクリックします。

7. 引き続き、**リクエストの承認**でユーザーを選択し、**実行**をクリックし、**フォームの起動**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image74.png" style="width:0.78606in;height:0.60676in" />

8. 画面上の値が前回の入力値を引き継いでいること、読み取り専用フォームを開いていることを確認し、APPROVEをクリックします。

9. 同様の流れでプロセスが完了するまで続けます。途中でREJECTして再送信タスクに遷移するかどうかも確認してください。

10. プロセスが終了するまで実行します。下図は実行例です。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image75.png" style="width:6.3194in;height:4.34831in" />

アプリケーションのアクティブ化
------------------------------

1. 手順はLab 1のアクティブ化と同様です。  
    **公開**をクリックしてコメントを追加後、**公開**をクリックします。

2. 続いて、**アクティブ化**をクリックし、ウィザードに従ってアクティブ化します。以前アクティブ化したものを上書きする場合には、リビジョンは前回と同じものを使い、**オーバーライド**にチェックを入れた上で、**アクティブ化**をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial2/image76.png" style="width:3.36136in;height:3.55919in" />
