# Static Process Tutorial [1/3]

- [目的](#目的)
- [最終構成](#最終構成)
- [前提条件](#前提条件)
- [作業手順](#作業手順)
  - [スペースの作成](#スペースの作成)
  - [アプリケーションの作成](#アプリケーションの作成)
  - [言語の変更](#言語の変更)
  - [プロセスの作成](#プロセスの作成)
  - [スイムレーンの作成](#スイムレーンの作成)
  - [リクエスト送信アクティビティの実装](#リクエスト送信アクティビティの実装)
  - [フォームの実装](#フォームの実装)
  - [アクティビティの名称変更](#アクティビティの名称変更)
  - [リクエストの承認アクティビティの設定](#リクエストの承認アクティビティの設定)
  - [再送信アクティビティの設定](#再送信アクティビティの設定)
  - [履行アクティビティの設定](#履行アクティビティの設定)
  - [承認ゲートウェイの実装](#承認ゲートウェイの実装)
  - [データアソシエーション](#データアソシエーション)
  - [テストモードへのアプリケーションのアクティブ化](#テストモードへのアプリケーションのアクティブ化)
  - [テストモードでのアプリケーション実行](#テストモードでのアプリケーション実行)
  - [本番モードへのアプリケーションアクティブ化](#本番モードへのアプリケーションアクティブ化)
  - [本番モードでのアプリケーション実行](#本番モードでのアプリケーション実行)

------

## 目的

このチュートリアルは、Oracle Integration Cloud（以下OIC）のProcessにおける以下の基本的な操作を学習・理解することを目的としています。

- スペースの作成
- アプリケーションの作成
- プロセスの作成
- 作成したプロセスのアクティブ化

## 最終構成

このチュートリアルでは、General ForemanがWebの入力画面から入力した内容を、Deputy Managerが確認し、承認を行うプロセスを作成します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image1.png" style="width:6.76875in;height:2.55625in" />

## 前提条件

- このチュートリアルはOIC 18.2.3で動作確認しています。
- ブラウザはMozilla Firefox、Google Chromeの利用を推奨します。

## 作業手順

### スペースの作成

OICのホーム画面左のメニューから **ProcessBuilder** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image2.png" style="width:6.76875in;height:3.50625in" />

スペースを作成します。

スペースを作成することで、アプリケーションをチームメンバーへ共有できます。

1. 左のメニューから **スペース** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image3.png" style="width:3.8764in;height:2.52629in" />

2. 右上の **作成** ボタンをクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image4.png" style="width:2.80899in;height:1.49264in" />

3. Spaceの名前をCapital Improvement とします。

4. **作成** をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image5.png" style="width:3.8427in;height:2.32397in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image6.png" style="width:6.76875in;height:1.81736in" />

------

### アプリケーションの作成

アプリケーションを作成します。最終的に完成するプロセスについては、[最終構成](#最終構成)を参照してください。

1. Capital Improvementをクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image7.png" style="width:5.40449in;height:2.01885in" />

2.  **作成** を選択し、 **新規アプリケーション** をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image8.png" style="width:2.24719in;height:2.42066in" />

3. アプリケーション名を **PurchaseRequisition** とします。

4.  **作成** をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image9.png" style="width:4.42638in;height:4.93258in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image10.png" style="width:7.05618in;height:3.64139in" />

### 言語の変更

1. 右上のメニューをクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image11.png" style="width:1.21868in;height:1.1573in" />

2. デフォルトは英語なので、これを日本語に変更します。 **他の言語を追加** をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image12.png" style="width:3.41667in;height:0.88889in" />

3. 左側の日本語を選択して＞をクリックし、右側の英語を選択して＜をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image13.png" style="width:6.13483in;height:2.27405in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image14.png" style="width:6.16854in;height:2.26249in" />

4. OKをクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image15.png" style="width:3.81944in;height:0.66667in" />

------

### プロセスの作成

このハンズオンでは、General ForemanがWebの入力画面から入力した内容を、Deputy Managerが確認し、承認を行うプロセスを作成します。

1. **プロセス** タブを選択します。

2. **フォーム承認パターン** を選択します。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image16.png" style="width:5.98876in;height:3.27548in" />

3. プロセスの名前をPurchaseRequistionとし、 **Create** をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image17.png" style="width:6.17978in;height:3.4871in" />

4. PurchaseRequistionプロセスが作成されます。 **PurchaseRequistion** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image18.png" style="width:4.89888in;height:3.02216in" />

    下図のような画面が現れます。この画面でプロセスを定義していきます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image19.png" style="width:6.76875in;height:3.95208in" />

------

### スイムレーンの作成

プロセスに必要な役割を決定し、そのロールをスイムレーンとして定義します。

1. Process Ownerというスイムレーン名をクリックし、 **General Foreman** に変更します。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image20.png" style="width:2.19841in;height:2.14607in" />

2. Process Reviewerのスイムレーン名を **Deputy Manager** に変更します。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image21.png" style="width:1.83246in;height:4.14607in" />

3. ページ下部の＋マークからスイムレーンを追加できます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image22.png" style="width:4.83275in;height:0.77695in" />

4. 追加したスイムレーンの名前をProcess Owner からFinanceに変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image23.png" style="width:2.40355in;height:2.16854in" />

5. スイムレーンを選択し、ゴミ箱のマークをクリックすると、スイムレーンを削除できます。 **Finance** のスイムレーンを削除しましょう。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image24.png" style="width:4.16648in;height:4.30337in" />

------

### リクエスト送信アクティビティの実装

入力画面のフレームを作成します。

1. **Submit Request** をダブルクリックし、タイトルを購買申請に変更します。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image25.png" style="width:1.44985in;height:1.08989in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image26.png" style="width:0.98027in;height:0.82023in" />

2. **購買申請** のアイコンをダブルクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image26.png" style="width:0.98027in;height:0.82023in" />

3. タイトルを **購買申請フォーム** と指定します。

4. フォームの右側にある＋をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image27.png" style="width:5.93364in;height:3.39326in" />

5. フォーム名として **PurchaseRequisitionUI** を入力し、 **作成** をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image28.png" style="width:3.42739in;height:3.41573in" />

**注意** : こまめに **保存** ボタンをクリックしておくことをおすすめします。

------

### フォームの実装

入力画面を作成します。

アプリケーションのアクティブ化後、作成した入力画面からプロセスを開始できます。

1. **購買申請** をクリックし、右側のアイコンをクリックして、 **フォームを開く** を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image29.png" style="width:3.17977in;height:1.9375in" />

2. フォーム作成画面が現れます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image30.png" style="width:6.04494in;height:4.02686in" />

3. 以下の表に従ってコンポーネントを配置します。

    | コンポーネント | 名前                   | ラベル                 |
    |----------------|------------------------|------------------------|
    | 入力テキスト   | ProjectName            | プロジェクト名         |
    | 入力テキスト   | ProjectID              | プロジェクトID         |
    | 日付           | ProjectStartDate       | プロジェクト開始日     |
    | 日付           | ProjectExpectedEndDate | プロジェクト終了予定日 |
    | テキスト領域   | ProjectDescription     | プロジェクトの概要     |
    | 入力テキスト   | RequisitionID          | 申請番号               |
    | 日付           | RequisitionDate        | 申請日付               |
    | 表             | ItemTable              | 購買申請品目リスト     |
    | 金額           | Total                  | 合計金額               |

    なお、画面作成にあたっての注意点は以下の通りです。

    - コンポーネントの名前は必ず半角英数字を使用する
    - ラベルは日本語でもよい
    - バインドはコンポーネント名に従って自動的に作成されるため、手を付けない

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image31.png" style="width:6.47138in;height:4.68539in" />

4. 画面コンポーネントの設定を変更します。
    - プロジェクト開始日、終了予定日、申請日付
      - プロパティ＞形式で、yyyy-mm-ddに変更  

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image32.png" style="width:3.11256in;height:2.92135in" />

    - 購買申請品目リスト
      - プロパティ＞列の＋アイコンをクリックして4列にし、以下のように列名を設定  

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image33.png" style="width:2.9375in;height:2.08333in" />

      - 各列にコンポーネントを貼り付け

        | 列名 | コンポーネント | 名前      | ラベル |
        |------|----------------|-----------|--------|
        | 品名 | 入力テキスト   | ItemName  | 品名   |
        | 単価 | 金額           | UnitPrice | 単価   |
        | 数量 | 番号           | Qty       | 数量   |
        | 小計 | 金額           | LineTotal | 小計   |

      - **ユーザーは行を追加/削除できます** にチェックを入れる

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image34.png" style="width:2.6875in;height:0.3125in" />

    - 単価
      - プロパティ＞一般＞通貨で日本円 (JPY) を選択
      - **変更時** イベントを追加

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image35.png" style="width:2.90625in;height:0.875in" />

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image36.png" style="width:2.90625in;height:2.35417in" />

      - 鉛筆アイコンをクリック
      - **アクション** をクリック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image37.png" style="width:5.24235in;height:1.95506in" />

      - 値が変更された時点で小計を更新する仕組みを追加するため、下図のようにアクションを設定します。終了後、 **OK** をクリックします。

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image38.png" style="width:7.16683in;height:3.79775in" />


      - ファンクション
        - 指定した値に対する操作を設定できます。たとえば、2つの値を加算したり、2つの文字列を連結したり、テーブル行にある項目を合計したりできます。
        - 詳細は以下のドキュメントをご覧ください。

            **Oracle Integration Cloud**
            > <https://docs.oracle.com/en/cloud/paas/integration-cloud-um/user-processes-um/adding-dynamic-behaviors-form.html#GUID-CE7E708C-C663-45B6-A734-72DC29DB406B>

            **Autonomous Integration Cloud**
            > <https://docs.oracle.com/en/cloud/paas/integration-cloud/user-processes/adding-dynamic-behaviors-form.html\#GUID-CE7E708C-C663-45B6-A734-72DC29DB406B>

      - コントロール
        - 操作対象のコンポーネントを設定できます。
      - その他
        - **タイプ**で設定できる項目の詳細は、以下のガイドを参照してください。

            **Oracle Integration Cloud**
            > <https://docs.oracle.com/en/cloud/paas/integration-cloud-um/user-processes-um/adding-dynamic-behaviors-form.html#GUID-2CCBD7FF-BFE4-4C76-81BF-3050385C886C>

            **Autonomous Integration Cloud**
            > <https://docs.oracle.com/en/cloud/paas/integration-cloud/user-processes/adding-dynamic-behaviors-form.html#GUID-2CCBD7FF-BFE4-4C76-81BF-3050385C886C>

            **参考：ポーランド記法**
            > <https://ja.wikipedia.org/wiki/%E3%83%9D%E3%83%BC%E3%83%A9%E3%83%B3%E3%83%89%E8%A8%98%E6%B3%95>

    - 数量
      - プロパティ＞最小で、0を指定
      - **変更時** イベントを追加し、単価で追加したアクションと同じものを設定

    - 小計
      - プロパティ＞通貨で日本円 (JPY) を選択
      - **読取り専用** にチェック
      - **変更時** イベントを追加
      - 鉛筆アイコンをクリック
      - 小計の変更時に合計金額が更新されるよう、以下のようにアクションを設定し、終了後、 **OK** をクリック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image39.png" style="width:6.76875in;height:2.97639in" />

    - 合計金額
      - プロパティ＞通貨で日本円 (JPY) を選択
      - **読取り専用** にチェック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image40.png" style="width:6.76875in;height:4.89861in" />

5. （オプション）「プレビュー」ボタンを押すと、作成したフォームの動作を確認できます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image41.png" style="width:6.76875in;height:1.45764in" />

------

### アクティビティの名称変更

アクティビティ名を変更します。

1. Approve Requestアクティビティをクリックし、「リクエストの承認」アクティビティへ名前を変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image42.png" style="width:1.76659in;height:1.29213in" />
    ➡︎
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image43.png" style="width:1.22531in;height:0.83146in" />

2. Resubmitアクティビティをクリックし、「再送信」アクティビティへ名前を変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image44.png" style="width:1.40449in;height:1.25508in" />
    ➡︎
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image45.png" style="width:1.02779in;height:0.88764in" />

3. FulFillアクティビティをクリックし、「履行」アクティビティへ名前を変更します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image46.png" style="width:1.22893in;height:1.1236in" />
    ➡︎
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image47.png" style="width:1.39326in;height:1.04494in" />

------

### リクエストの承認アクティビティの設定

アクティビティと、先ほど作成したWebフォームを紐づけます。

1. リクエストのアクティビティをクリックし、右に出るメニューアイコンから **プロパティを開く** を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image48.png" style="width:3.26188in;height:1.60674in" />

2.  **フォーム** の虫眼鏡アイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image49.png" style="width:4.10112in;height:0.68352in" />

3. 先ほど作成した、PurchaseRequisitionUIを選択し、 **OK** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image50.png" style="width:4.04494in;height:2.87874in" />

------

### 再送信アクティビティの設定

1. リクエストの承認アクティビティと同様に、 **再送信** アクティイビティにて、作成したPurchaseRequisitionUIを指定します。

------

### 履行アクティビティの設定

このハンズオンでは、「履行」のアクティビティをドラフトとして取り扱います。

ドラフトを使うと、すべてのアクティビティの定義が完了していなくても、プロセスの一部をテスト・実行できます。

1. プロパティを開きます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image51.png" style="width:3.34831in;height:1.78577in" />

2. ドラフトのチェックボックスをONにします。<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image52.png" style="width:5.12359in;height:1.39089in" />

3. この設定で、履行アクティビティはグレーに変わります。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image53.png" style="width:1.23761in;height:1.07865in" />

4. ここまで作成した内容を保存しておきます。

------

### 承認ゲートウェイの実装

ゲートウェイを定義することによって、フローを分岐および統合させることが出来ます。

このハンズオンでは、「リクエストの承認」アクティビティの承認結果に応じて、フローを変更する処理を設定します。

その他のゲートウェイの種類については、以下ガイドを参照してください。

  **Oracle Integration Cloud**
  > <https://docs.oracle.com/en/cloud/paas/integration-cloud-um/user-processes-um/controlling-process-flow-using-gateways.html#GUID-9E5FAFE6-01D2-45A4-9505-2A885DD18BDB>

  **Autonomous Integration Cloud**
  > <https://docs.oracle.com/en/cloud/paas/integration-cloud/user-processes/controlling-process-flow-using-gateways.html#GUID-9E5FAFE6-01D2-45A4-9505-2A885DD18BDB>

1. 「リクエストの承認」アクティビティをクリックします。

    アクションの内容を確認することで、「リクエストの承認」結果として返却する値を確認できます。

    「リクエストの承認」アクティビティは『APPROVE』または『REJECT』を返却します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image54.png" style="width:6.76875in;height:4.26319in" />

2. ゲートウェイからの分岐 **No**  をクリックします。

3.  **条件** の鉛筆アイコンをクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image55.png" style="width:5.75319in;height:5.32584in" />

4. 式エディタに、
    ```
    TaskOutcomeDataObject != "APPROVE"
    ```
   と設定します。TaskOutcomeDataObjectは変数リストで選択し、 **式に挿入** をクリックします（直接記述することもできます）。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image56.png" style="width:5.13483in;height:7.86709in" />

3.  **検証** をクリックして、式が構文上問題ないことを確認します。

4. OKをクリックします。

------

### データアソシエーション

購買申請、リクエストの承認、再送信の各アクティビティでフォームとプロセス・データ・オブジェクト間でデータをマッピングします。この作業をデータ・アソシエーションと呼びます。

このハンズオンでは、アクティビティとフォームを紐付けたことで、自動的にデータ・アソシエーションが完了しています。そのため、設定内容を確認するだけにとどめます。

1. 購買申請アクティビティで **データ・アソシエーションを開く** を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image57.png" style="width:2.79351in;height:1.67416in" />

2. 以下の画面が開き、既にデータ・アソシエーションが完了していることがわかります。右側のデータ・オブジェクト側でpurchaseRequisitionUIDataObjectを展開し、画面の項目（formArg）と一致していることを確認してください。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image58.png" style="width:5.8492in;height:2.64045in" />

3. 設定変更せず、 **取消** をクリックします。

4. 他のアクティビティでも既にデータ・アソシエーションが完了していることを確認してください。

------

### テストモードへのアプリケーションのアクティブ化

では、作成したプロセスを検証してみましょう。

1.  **テスト** アイコンをクリックします。 **現在のアプリケーション検証に成功しました** と表示されていればアクティブ化が可能です。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image59.png" style="width:0.71875in;height:0.48958in" />

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image60.png" style="width:5.71667in;height:2.66215in" />

2.  **アクティブ化** をクリックします。  
    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image61.png" style="width:1.52792in;height:1.10112in" />

3. クリック後、 **テスト用にアクティブ化** というダイアログが出ます。この中で、 **自分を全てのロールに追加** というチェックボックスが出てきますが、このチェックは外さずに、 **アクティブ化** をクリックしてください。この設定を有効化しておくと、テストモードでは、各スイムレーンにアクティブ化時に操作しているユーザーを割り当てるため、ユーザーの切り替えの手間を省くことができます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image62.png" style="width:4.40449in;height:2.02173in" />

4. アプリケーションのアクティブ化が成功することを確認してください。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image63.png" style="width:4.5in;height:1.69444in" />

------

### テストモードでのアプリケーション実行

1. アクティブ化完了後、 **テスト・モードで試行** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image64.png" style="width:5.95506in;height:0.8138in" />

2. PRというアイコンが存在するので、このアイコンをクリックします（背景色は環境によって変わります）。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image65.png" style="width:2.29056in;height:2in" />

3. PurchaseRequisitionUIが現れますので、適宜入力します。以下は入力例です。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image66.png" style="width:6.76875in;height:6.17292in" />

4. 入力が完了したら、 **送信** を押します。

5. **My Tasks** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image67.png" style="width:1.7633in;height:2.17021in" />

6. 先ほど送信したPurchaseRequisitionが1個表示されているはずです。これをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image68.png" style="width:6.76875in;height:1.61042in" />

7. 入力した内容が正しく表示されていることを確認します。その上で、APPROVE/REJECTのいずれかを選択し、クリックします。 **APPROVE** すると、プロセスは終了します。REJECTすると、再送信アクティビティに遷移します。今回は **APPROVE** を選択します。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image69.png" style="width:5.54374in;height:5.43056in" />

8. 先ほど実行したプロセスインスタンスの状況を確認するため、 **Processes** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image70.png" style="width:2.4871in;height:3in" />

9. フィルターアイコンをクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image71.png" />

10. 左側のステータスから、 **Completed** にチェックを入れます。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image72.png" style="width:2.875in;height:2.79167in" />

11. 先ほど完了したプロセスインスタンス（PurchaseRequisition）が現れるので、この行をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image73.png" style="width:5.36597in;height:0.89901in" />

    グラフィカル・ビューでプロセスのどの経路を通ったかがわかります

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image74.png" style="width:5.97708in;height:4.82177in" />

12. **ツリー・ビュー** にすると、各アクティビティでのデータの出入りがわかります。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image75.png" style="width:6.76875in;height:5.16806in" />

13. 例えば、 **Approve Request** の **Instance entered the
    activity** をクリックすると、XML形式でどのようなデータが流れてきて、フォームに入るのかがわかります

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image76.png" style="width:5.72222in;height:4.13888in" />

------

### 本番モードへのアプリケーションアクティブ化

作成したプロセスをテストモードで実行して問題がないことを確認後、本番環境へアクティブ化します。

1. Processの編集画面に戻ります。

2. 本番モードへのアクティブ化にあたっては、データベースに永続化して公開する必要があります。 **公開** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image77.png" style="width:4.69711in;height:0.93056in" />

3. ダイアログでコメントを入力し、 **公開** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image78.png" style="width:5.26551in;height:6.125in" />

4.  **アクティブ化** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image79.png" style="width:5.22161in;height:0.75in" />

5.  **新規バージョンのアクティブ化** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image80.png" style="width:6.10602in;height:2.84722in" />

6. ウィザードに従ってアクティブ化を進めます。

    - **バージョンの選択** スナップショットは最終公開バージョン
    - **アプリケーションは正常に検証されました** が表示されたことを確認し、 **オプション** をクリック
    - リビジョンIDは1.0、オーバーライド、デフォルトを強制使用にチェックを入れて、 **アクティブ化** をクリック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image81.png" style="width:5.10151in;height:5.36111in" />
    - **アプリケーションは正常にアクティブ化されました** が表示されたことを確認し、 **終了** をクリック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image82.png" style="width:5.37495in;height:5.66667in" />

------

### 本番モードでのアプリケーション実行

1. Process Workspaceを開き、Integration Cloudのトップページから **My Tasks** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image2.png" style="width:6.76875in;height:3.50625in" />

2. **Workspace** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image83.png" style="width:3.11927in;height:1.04511in" />

3. テストモードとは異なり、本番モードでは、この時点では、ユーザーやグループがスイムレーンに割り付けられていません。そのため、スイムレーン（＝ロール）にユーザーやグループを割り付ける必要があります。

    - 画面左のメニューから **管理** を選択します。

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image84.png" style="width:2.51948in;height:3.40179in" />

    - 検索ボックスに **PurchaseRequisition** を入力し、設定したいロールのみを表示します。

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image85.png" style="width:3.9375in;height:1.36005in" />

    - 今回ロールにユーザーもしくはグループを割り付ける対象のロールと割り付けるユーザーは以下の通りです。

        | ロール                              | 割り当てるユーザーもしくはグループ |
        |-------------------------------------|------------------------------------|
        | PurchaseRequisition.Deputy Manager  | ご自身のユーザーアカウント         |
        | PurchaseRequisition.General Foreman | ご自身のユーザーアカウント         |

4. ユーザーやグループの割り付け方法は以下の通りです。

    - ユーザーやグループを割り付けたいロールを選択した状態で、 **メンバーの追加** をクリック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image86.png" style="width:5.50089in;height:2.97309in" />

    - ユーザー、もしくはグループを検索し、割り付けたいユーザーもしくはグループにチェックを入れ、選択済として表示されていることを確認してから、 **OK** をクリック（複数ユーザーやグループを追加することもできる）。

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image87.png" style="width:4.70625in;height:2.71743in" />

    - **保存** をクリック

        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image88.png" style="width:5.57083in;height:2.99089in" />

5. 左のメニューからWorkspaceのホーム画面に戻ります。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image89.png" style="width:2.04464in;height:2.00067in" />

6. 左のメニューから **自分のアプリ** をクリックします。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image90.png" style="width:1.92857in;height:2.50998in" />

7. テストモードの場合と同様、丸囲みのPRを確認できるはずです。これをクリックして、プロセスを開始します。以後はテストモードの場合と同様です。

    <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/StaticProcess-Tutorial1/image91.png" style="width:2.75893in;height:2.42708in" />
