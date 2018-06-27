# Dynamic Process Tutorial / Emergency Room

- [目的](#目的)
  - [ユースケース](#ユースケース)
  - [ハンズオン動作確認環境](#ハンズオン動作確認環境)
  - [ハンズオン実施上の注意](#ハンズオン実施上の注意)
- [演習手順](#演習手順)
  - [アプリケーションの作成](#アプリケーションの作成)
  - [アクティビティやオブジェクトの作成](#アクティビティやオブジェクトの作成)
    - [ヒューマンタスクの作成](#ヒューマンタスクの作成)
    - [ステージの作成](#ステージの作成)
    - [マイルストンの作成](#マイルストンの作成)
    - [フォームの作成](#フォームの作成)
  - [プロセス開始イベントの設定](#プロセス開始イベントの設定)
  - [ヒューマンタスクとフォームの関連付け](#ヒューマンタスクとフォームの関連付け)
  - [ヒューマンタスクへのデータアソシエーション](#ヒューマンタスクへのデータアソシエーション)
  - [ロールと権限の設定](#ロールと権限の設定)
  - [ヒューマンタスクへのユーザー割当て](#ヒューマンタスクへのユーザー割当て)
  - [ステージやアクティビティの開始および終了条件設定](#ステージやアクティビティの開始および終了条件設定)
    - [スクリーニング・ステージ](#スクリーニング・ステージ)
    - [処置ステージ](#処置ステージ)
    - [退院ステージ](#退院ステージ)
    - [スクリーニング済マイルストン](#スクリーニング済マイルストン)
    - [処置済マイルストン](#処置済マイルストン)
    - [退院済マイルストン](#退院済マイルストン)
    - [手術の手続きアクティビティ](#手術の手続きアクティビティ)
    - [処置アクティビティ](#処置アクティビティ)
  - [テストモードでの動作確認](#テストモードでの動作確認)
    - [テストモードでのアクティブ化](#テストモードでのアクティブ化)
    - [テストモードでの実行](#テストモードでの実行)
  - [本番モードでの動作確認](#本番モードでの動作確認)
    - [アプリケーションの公開](#アプリケーションの公開)
    - [本番モードへのアクティブ化](#本番モードへのアクティブ化)
    - [本番モードでの実行](#本番モードでの実行)

------

## 目的

この演習では、動的プロセス（Dynamic Process）の基本的な動作を理解することを目的としています。

### ユースケース

救急病院の患者受け入れをモデルとして、搬送から退院までの流れをモデルにした、簡単な動的プロセス・アプリケーションを作成します。

### ハンズオン動作確認環境

このハンズオンは、Oracle Integration Cloud (User Managed)18.2.5で動作確認しています。

Mozilla FirefoxもしくはGoogle Chromeを使うことを推奨します。

### ハンズオン実施上の注意

このハンズオンは、すでにStatic Process（通常のProcess）での開発経験がある人を対象にしています。

完成版をエクスポートしたものは[こちら](EmergencyProcess.exp)にあります。

------

## 演習手順

### アプリケーションの作成

Integration Cloudのホーム左側のProcess Builderをクリックし、Process Builderでアプリケーションを作成していきます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image1.png" style="width:5.87835in;height:3.67402in" />

右上の **作成** をクリックし、メニューから **新規アプリケーション** を選択します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image2.png" style="width:1.91451in;height:1.72464in" />

アプリケーション名はEmergency Processとし、**作成** をクリックします。スペースは**スペースの新規作成**を選択し、スペース名として**Dynamic Process**を指定します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image3.png" style="width:2.46698in;height:3.0963in" />

右側の **作成** をクリックし、**新規動的プロセス** を選択します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image4.png" style="width:1.23611in;height:0.83333in" />

名前は **Emergency Process**、パターンは **None** を選択し、 **Create** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image5.png" style="width:4.80568in;height:2.71015in" />

以下のような初期画面が現れるはずです。この画面でステージ（フェーズ）、マイルストン、アクティビティなどを追加していきます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image6.png" style="width:5.3437in;height:4.2752in" />

------

### アクティビティやオブジェクトの作成

続いて、アクティビティを作成します。動的プロセスでは、ヒューマン・タスクもしくはBPMNプロセスをアクティビティにすることができます。ヒューマンタスクはプロセスが無くても単独でアクティビティにすることができます。BPMNプロセス・アクティビティには任意のBPMNアクティビティを含めることができます。

#### ヒューマン・タスク・アクティビティの作成

アクティビティ追加フィールドで以下の操作をしてヒューマンタスクを作成します。

- Human Task Activityを選択
- 名前は **処置**
- <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image7.png" style="width:0.18056in;height:0.18056in" />アイコンをクリックしてアクティビティを作成

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image8.png" style="width:2.51389in;height:0.94444in" />

作成した結果、以下のようにアクティビティが表示されているはずです。このうち、<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image9.png" style="width:0.16654in;height:0.16654in" />はこのアクティビティが必須であること、<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image10.png" style="width:0.16763in;height:0.17561in" />は必須項目が設定済みではあるものの、1個未設定の構成があることを表しています。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image11.png" style="width:2.60354in;height:0.95709in" />

同様に、下表に従ってヒューマンタスクを作成します。

| 名前           |
|----------------|
| スクリーニング |
| 退院の手続き   |
| 手術の手続き   |
| 退院判定       |

#### ステージの作成

続いて、ステージ（Stage）を作成します。ステージはフェーズと置き換えて考えていただくとわかりやすいかもしれません。ステージを使って、フェーズ内のアクティビティを編成します。ステージは直列（一つずつステージが進む）、並列（複数のステージが同時並行に進む）のいずれかを選択できます。

※デフォルトでは、全てのステージが同時に利用可能です。

ステージ追加フィールドで名前を指定し、<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image7.png" style="width:0.18056in;height:0.18056in" />をクリックしてステージを作成します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image12.png" style="width:1.79167in;height:0.34722in" />

作成するステージの名前は下表に従います。

| 名前           |
|----------------|
| スクリーニング |
| 処置           |
| 退院           |

作成が完了すると、以下のような構成になっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image13.png" style="width:5.87835in;height:3.67402in" />

各ステージに作成済みのアクティビティを下表に従い配置します。残ったアクティビティは、任意のステージで実施可能なアクティビティとして機能します。

| ステージ        | アクティビティ    |
|----------------|----------------|
| スクリーニング   | スクリーニング |
| 処置           | 処置、 退院判定 |
| 退院           | 退院の手続き |

この結果、下図のような表示になっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image14.png" style="width:5.8777in;height:3.5885in" />

#### マイルストンの作成

**スクリーニング** ステージのアクティビティ追加フィールドで以下の操作をしてマイルストンを作成します。

- Milestoneを選択
- 名前は **スクリーニング済**
- <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image7.png" style="width:0.18056in;height:0.18056in" />アイコンをクリックしてマイルストンを作成

同様の操作で、下表に従い、各ステージにマイルストンを作成します。

| ステージ        | マイルストン    |
|----------------|----------------|
| 処置           | 処置済 |
| 退院           | 退院済 |

作成が完了すると、下図のようになっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image15.png" style="width:5.87835in;height:3.67402in" />

#### フォームの作成

ヒューマンタスクで利用するフォームを作成します。

画面左側のメニューで **フォーム** を選択し、画面右側に現れる **作成** をクリックし、名前をPatientForm（半角、空白を入れない）として **作成** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image16.png" style="width:2in;height:2in" />

初期状態では、プレゼンテーション名が **メイン** になっているはずです。この名前を **スタート** に変更します。プロパティ＞プレゼンテーション＞**名前**で **メイン** を **スタート** に書き換えます。この **スタート** プレゼンテーションでは、入院患者の氏名を記録するために使います。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image17.png" style="width:1.80556in;height:1.88889in" />

2個のテキストフィールドを貼り付けます。名前、ラベルは以下の通り設定します。バインドは名前を基に生成しますので、明示的に設定する必要はありません。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image18.png" style="width:4.02778in;height:1.29167in" />

| 名前 | ラベル | （バインド）| コンポーネント |
|------------|----------|----------------|----------------|
| LastName        | 姓 | lastName | テキスト |
| FirstName        | 名 | firstName | テキスト |

**スタート**をベースにして、新たにプレゼンテーションを作成します。プロパティ＞フォーム＞プレゼンテーションの＋をクリックし、名前は **スクリーニング** 、基準は **スタート** を選択し、 **作成** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image19.png" style="width:3.06965in;height:2.40594in" />

スクリーニングでは、テキスト領域を追加し、下表に従ってプロパティを設定します。

| 名前        | ラベル    | （バインド）    | コンポーネント    |
|------------|----------|----------------|----------------|
| Symptoms        | 状態 | Symptoms | テキスト領域 |

作成したプレゼンテーションは以下のようになっているはずです。姓、名は読み取り専用にしておきます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image20.png" style="width:5.87756in;height:3.67323in" />

**処置**プレゼンテーションを作成します。スクリーニングと同じように、既存のプレゼンテーションを基に作成しますが、今回の場合は **スクリーニング** を基に作成します。作成したプレゼンテーションの **状態** の下に、繰返し可能セクションを追加します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image21.png" style="width:5.87756in;height:3.67323in" />

繰返しセクションではユーザーが行を追加/削除できるようにしておきます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image22.png" style="width:2.08396in;height:1.72085in" />

繰返しセクション内に日時とテキスト領域を追加し、下表に従ってプロパティを設定します。

<table>
<theader>
<tr class="odd">
<th>名前</th><th>ラベル</th><th>（バインド）</th><th>計算された値</th><th>形式</th><th>読取り専用</th><th>コンポーネント</th></tr></theader>
<tbody>
<tr class="odd">
<td>TreatmentDatetime</td><td>処置日時</td><td>treatmentDatetime</td><td><p>True</p>
<p><b>編集</b> をクリックし、タイプは <b>ファンクション</b> 、ファンクションは <b>現在の日時</b> を選択</p></td><td>yyyy-MM-dd</td><td>True</td><td>日時</td></tr>
<tr class="even">
<td>Treatment</td>
<td>処置</td>
<td>Treatment</td>
<td>N/A</td>
<td>N/A</td>
<td>False</td>
<td>テキスト領域</td>
</tr>
</tbody>
</table>

作成したプレゼンテーションは以下のようになっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image23.png" style="width:5.87835in;height:3.67402in" />

**手術の手続き** プレゼンテーションを作成します。このプレゼンテーションは **スタート** を基に作成します。作成したプレゼンテーションに、チェック・ボックスを追加し、下表に従ってプロパティを設定します。姓名は読み取り専用にしておきます。

| 名前        | ラベル    | （バインド）    | コンポーネント    |
|------------|----------|----------------|----------------|
| IsConcented   | 手術への同意 | isConcented | チェック・ボックス |

作成したプレゼンテーションは以下のようになっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image24.png" style="width:5.87835in;height:3.67402in" />

**退院判定** プレゼンテーションを作成します。このプレゼンテーションは **スタート** を基に作成します。作成したプレゼンテーションに、チェック・ボックスを追加し、下表に従ってプロパティを設定します。姓名は読み取り専用にしておきます。

| 名前        | ラベル    | （バインド）    | コンポーネント    |
|------------|----------|----------------|----------------|
| IsTreatmentCompleted | 退院可 | isTreatmentCompleted | チェック・ボックス |

作成したプレゼンテーションは以下のようになっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image25.png" style="width:5.87835in;height:3.67402in" />

**退院の手続き**プレゼンテーションを作成します。このプレゼンテーションも **手術前の手続き** と同様、**スタート**を基に作成します。作成したプレゼンテーションに、チェック・ボックスを追加し、下表に従ってプロパティを設定します。

| 名前        | ラベル    | （バインド）    | コンポーネント    |
|------------|----------|----------------|----------------|
| IsReadyForDischage | 退院の準備完了 | isReadyForDischage | チェック・ボックス |

作成したプレゼンテーションは以下のようになっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image26.png" style="width:5.87835in;height:3.67402in" />

------

### プロセス開始イベントの設定

各動的プロセスは、フォームまたはデータ入力によって開始されます。動的プロセスのためのフォーム入力とプレゼンテーション入力を設定します。

動的プロセスを構成するため、EmergencyProcessタブをクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image27.png" style="width:2.80556in;height:0.68056in" />

プロセス名の左側にある<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image28.png" style="width:0.20833in;height:0.23611in" />をクリックすると、動的プロセスの開始条件を設定できます。下表に従って条件を設定し、 **Define** をクリックします。

| 起動条件 | From Title | Form | Presentation | Interface Argument |
|------------|----------|----------------|--------|--------|
| With Form Only | 搬送者情報の登録 | PatientForm | スタート | formArg |

------

### ヒューマンタスクとフォームの関連付け

ヒューマンタスクに先ほど作成したフォームのプレゼンテーションを関連付けます。

EmergencyProcessタブで、ヒューマンタスクの **スクリーニング** をダブルクリックする、もしくはシングルクリックで現れる<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image29.png" style="width:0.20833in;height:0.22222in" />をクリックします。いずれの方法でも、アクティビティのプロパティ設定画面が現れます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image30.png" style="width:1.94722in;height:0.64384in" />

プロパティ画面のGeneralタブ下方にあるImplementationのGeneralをクリックします。実装に関連するプロパティ設定画面が現れます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image31.png" style="width:3.74173in;height:3.6in" />

FormはPatientForm、Presentationはスクリーニングをそれぞれ選択します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image32.png" style="width:1.86111in;height:0.98611in" />

TitleやTask Summaryを任意で設定します。ここで指定した内容は、実行時にフォームの最上部に表示されます。

**Close** をクリックします。

 その他のヒューマンタスクについても同様に設定していきます。設定内容は下表をご覧ください。

| アクティビティ | Form | Presentation |
|------------|----------|----------------|
| 処置 | PatientForm | 処置 |
| 退院判定 | PatientForm | 退院判定 |
| 手術の手続き | PatientForm | 手術の手続き |
| 退院の手続き | PatientForm | 退院の手続き |

------

### ヒューマンタスクへのデータアソシエーション

各ヒューマンタスクでは、データアソシエーション（データ・マッピング）を使って、データ入出力を定義する必要があります。データアソシエーションは、フロー要素間で渡される情報を定義します。

**スクリーニング**ヒューマンタスクをクリックし、<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image33.png" style="width:0.23611in;height:0.23611in" />をクリックし、Data Association &gt; Inputを選択します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image34.png" style="width:3.51389in;height:1.19444in" />

自動マッピング<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image35.png" style="width:0.27778in;height:0.25in" />をクリックします。これをクリックすると、データ型が同じものを自動的にマッピングします。もちろん、手作業でマッピングしてもかまいません。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image36.png" style="width:4.2748in;height:0.93504in" />

左上にある **出力** タブをクリックし、自動マッピングをクリックします。その後、 **適用** をクリックします。もしoutcomeのデータ・アソシエーションが出る場合は、下図のように **削除** をクリックしてください。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image37.png" style="width:4.2748in;height:1.26929in" />

その他のヒューマンタスク（処置、退院判定、手術の手続き、退院の手続き）も同様の設定をします。

------

### ロールと権限の設定

動的プロセスの柔軟性は、動的プロセスのロールがもたらします。ロールを作成し、そのロールを適用するプロセス要素、そして付与する権限を定義します。

右上にある **Roles** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image38.png" style="width:2.15278in;height:0.31944in" />

**ロール** タブでは、動的プロセス用に定義されたロールを列挙しています。事前構成済みのデフォルトのロールとして、Process OwnerとProcess Viewerの2つが提供されています。今回はこれらのロールを、動的プロセスに適用します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image39.png" style="width:2.06944in;height:1.27778in" />

鉛筆アイコンをクリックし、編集していきます。編集項目および編集内容は下表をご覧ください。権限を新たに作成する場合は、**＋** アイコンをクリックします。編集が完了したら **Save** をクリックします。

<table>
<thead>
<tr class="header">
<th></th>
<th>Process Owner</th>
<th>Process Viewer</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<th>新規ロール名</th>
<td>医師</td>
<td>看護師</td>
</tr>
<tr class="even">
<th>Members</th>
<td>OIC/AICにログインするユーザー</td>
<td>OIC/AICにログインするユーザー（Doctorと異なるユーザーが望ましい）</td>
</tr>
<tr class="odd">
<th>既存のPermission</th>
<td>名前を <b>プロセスの全アクションの実行</b> に変更</td>
<td>名前を <b>プロセス全体の閲覧</b> に変更</td>
</tr>
<tr class="even">
<th>新規追加するPermission</th>
<td>N/A</td>
<td>以下の2個を追加<br>
Name: <b>スクリーニング・ステージでの編集</b><br>
Items: <b>スクリーニング・ステージ（アクティビティではないので注意）</b><br>
Actions: <b>Update</b><br><br>
Name: <b>退院の手続き</b><br>
Items: <b>退院の手続き</b><br>
Actions: <b>Update</b></td>
</tr>
</tbody>
</table>

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image40.png" style="width:2.49173in;height:3.6in" /> <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image41.png" style="width:2.49173in;height:3.59173in" />

------

### ヒューマンタスクへのユーザー割当て

各ヒューマンタスクにはユーザーやグループ、ロールを割り当てる必要があります。

**医師**ロールを **処置** アクティビティに割り当てます。

 1. **処置**アクティビティをダブルクリックもしくはシングルクリックで<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image29.png" style="width:0.20833in;height:0.22222in" />をクリック
 1. プロパティ画面のGeneralタブ下方にあるImplementationのGeneralをクリック
 1. Assigneesの鉛筆アイコンをクリック  
        <img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image42.png" style="width:1.88889in;height:0.52778in" />
 1. 現れたウィンドウの右上方にある＋をクリックして、Identity Type、Type、Valueを指定する。今回の場合は、Identity TypeはRole、TypeはLiteral、Valueは医師を設定する。
 1. 設定が完了したら、**Save** をクリック

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image43.png" style="width:4.16667in;height:2.91667in" />

その他のアクティビティに対しても、下表の設定に従って、同様の手順でユーザーを割り当てていきます。

| アクティビティ名 | Identity Type | Type | Value |
|------------|----------|----------------|---|
| スクリーニング | Role | Literal | 看護師 |
| 手術の手続き | Role | Literal | 医師 |
| 退院判定 | Role | Literal | 医師 |
| 退院の手続き | Role | Literal | 看護師 |

------

### ステージやアクティビティの開始および終了条件設定

ここまでで動作可能な動的プロセスを作成できていますが、実際の運用を想定し、条件に応じたステージやアクティビティの有効化・無効化を実現するための設定をしていきます。

救急搬送から退院までの流れは以下の通りです。

1. 搬送
1. スクリーニング
1. 処置
    - 複数回発生する可能性があります
    - 手術が必要な場合があります
1. 退院

上記の流れを意識して、各アクティビティの開始・終了条件を設定します。

#### スクリーニング・ステージ

**スクリーニング** ステージは1回のみ実行しますので、現状の設定のままでかまいません。

#### 処置ステージ

1. **処置** ステージのプロパティを開きます。
1. Conditionsタブに移動し、開始条件（Activation）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **スクリーニング** ステージの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。
1. 同様に、終了条件（Termination）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **退院判定** アクティビティの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。

#### 退院ステージ

1. **退院** ステージのプロパティを開きます。
1. Conditionsタブに移動し、開始条件（Activation）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **処置** ステージの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。
1. 同様に、終了条件（Termination）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **退院の手続き** アクティビティの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。

#### スクリーニング済マイルストン

スクリーニング・アクティビティが完了すると、自動的に到達しますが、明示的に設定するには以下のようにします。

1. **スクリーニング済** マイルストンのプロパティを開きます。
1. Conditionsタブに移動し、完了条件（Completion）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **スクリーニング** アクティビティの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。

#### 処置済マイルストン

退院判定アクティビティが完了すると、自動的に到達しますが、明示的に設定するには以下のようにします。

1. **処置済** マイルストンのプロパティを開きます。
2. Conditionsタブに移動し、完了条件（Completion）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **退院判定** アクティビティの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。

#### 退院済マイルストン

退院の手続きアクティビティが完了すると、自動的に到達しますが、明示的に設定するには以下のようにします。

1. **退院済**マイルストンのプロパティを開きます。
2. Conditionsタブに移動し、完了条件（Completion）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **退院の手続き** アクティビティの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。

#### 手術の手続きアクティビティ

手術の手続きアクティビティは、処置ステージでのみ利用できるようにします。

1. **手術の手続き** アクティビティのプロパティを開きます。
2. Conditionsタブに移動し、開始条件（Activation）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、**スクリーニング** アクティビティの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。
3. 同様に、終了条件（Termination）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **処置** ステージの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。

#### 処置アクティビティ

処置アクティビティは、処置ステージで任意の回数利用できるようにします。

1. **処置**アクティビティのプロパティを開きます。
2. Generalタブに移動し、MarkersのRepeatableにチェックを入れます。
3. Conditionタブに移動し、終了条件（Termination）の右にある**＋**をクリックして条件を追加します。Eventsの右にある**＋**をクリックし、 **退院判定** アクティビティの完了（Complete）を条件として設定します。設定後、下部のCreateをクリックしてください。Labelは適宜変更してかまいません。

この結果、下図のようなプロセス構造になっているはずです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image44.png" style="width:5.87835in;height:3.67402in" />

------

### テストモードでの動作確認

テスト機能を使うと、アプリケーションを検証し、テストモードでアクティブ化し、利用できるようになります。準備が整うまで、アプリケーションを本番モードでアクティブ化しないことをお奨めします。

すでに基本的な動的プロセスは構成済みなので、このタイミングで作成した動的プロセスがどのように機能するかを確認してみましょう。変更の結果を表示したいときにはいつでもすぐにアプリケーションをテストできます。

#### テストモードでのアクティブ化

**テスト** をクリックします。
<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image45.png" style="width:0.51389in;height:0.31944in" />

**現在のアプリケーション検証に成功しました** が出ていることを確認し、 **アクティブ化** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image46.png" style="width:4.34722in;height:3.02778in" />

**自分を全てのロールに追加** にチェックが入っていることを確認し、 **アクティブ化** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image47.png" style="width:2.93056in;height:1.30556in" />

#### テストモードでの実行

アクティブ化が完了したら、<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image48.png" style="width:1.27778in;height:0.30556in" />をクリックします。Testing ModeがOnの状態のブラウザが開きます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image49.png" style="width:5.87835in;height:3.67402in" />

Emergency Processをクリックします。 **スタート** フォームが現れるので、姓名を入力し、Submit（送信）をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image50.png" style="width:5.87835in;height:3.67402in" />

Submit（送信）が完了すると、Emergency Processインスタンスが作成されたことを示す表示が現れます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image51.png" style="width:5.87835in;height:3.67402in" />

左のメニューから、Dynamic Processes（動的プロセス）をクリックします。プロセスインスタンスがリスト表示されているので、クリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image52.png" style="width:5.87835in;height:3.67402in" />

インスタンス作成時に入力した姓名が表示され、上部にはインジケータが出ています。そしてアクティビティはスクリーニングのみ選択できることを確認した上で、スクリーニング・アクティビティの右にあるドロップダウンメニューから、Open（開く）をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image53.png" style="width:5.87835in;height:3.67402in" />

スクリーニング・プレゼンテーションが表示されていることを確認し、状態を入力し、Submit（発行）をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image54.png" style="width:5.87835in;height:3.67402in" />

インジケータが進展して、スクリーニング済のマイルストンに到達し、選択できるアクティビティは手術の手続き、処置、退院判定の3種類に変わっていることを確認してください。今回は手術の手続き、処置、退院判定のアクティビティを順に操作していきます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image55.png" style="width:5.87835in;height:3.67402in" />

操作すると以下のような挙動を示すことを確認してください。

- 手術の手続きアクティビティを実行すると、処置と退院判定のみ選択できる
- 処置アクティビティは何度実行しても新たなアクティビティインスタンスが生成される
- 退院判定アクティビティを実行すると、処置ステージが終了、処置済マイルストンに到達する
- 処置済マイルストンに到達すると、退院の手続きアクティビティのみ選択できる

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image56.png" style="width:5.87835in;height:3.67402in" />

- 最後に退院の手続きアクティビティを実行します。この結果、Emergency Processインスタンスは終了し、選択できるアクティビティがなくなること、退院済マイルストンに到達することを確認します。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image57.png" style="width:5.87835in;height:3.67402in" />

------

### 本番モードでの動作確認

テストモードでの動作確認が終了したら、アクティブ化した上で、本番モードで実行します。アクティブ化対象のアプリケーションは、データベースに永続化（公開）したものです。そのため、まだ公開していない場合は事前に公開する必要があります。

#### アプリケーションの公開

上部メニューの**公開**をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image58.png" style="width:3.3189in;height:0.37559in" />

下図のようなウィンドウが開きますので、コメントを入れて **公開** をクリックします。このタイミングで、スナップショットを採取することもできます。公開が完了すると、本番モードでアクティブ化できる準備が整います。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image59.png" style="width:3.93346in;height:4.54173in" />

#### 本番モードへのアクティブ化

上部メニューの **アクティブ化** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image60.png" style="width:3.07638in;height:0.38268in" />

下図のようなアクティブ化の画面が現れます。下図の例はまだアプリケーションをアクティブ化していない状態です。 **新規バージョンのアクティブ化** をクリックして、ウィザードを開始いします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image61.png" style="width:5.87835in;height:3.67402in" />

**スナップショットの選択** では最終公開バージョンを選択し、 **Validate** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image62.png" style="width:5.87835in;height:3.67402in" />

**アプリケーションは正常に検証されました** というメッセージを確認し、 **オプション** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image63.png" style="width:5.87835in;height:3.67402in" />

リビジョンIDとして数値（小数も可能）を指定します。オーバーライド、デフォルトを強制使用は任意です（下図の例では1.0、全てのチェック・ボックスにチェックを入れています）。入力が完了したら、 **アクティブ化** をクリックします。

- オーバーライド：チェックを入れると、現在実行中のインスタンスが失効します
- デフォルトを強制使用：チェックを入れると、当該バージョンがアプリケーションのデフォルトバージョンとして取り扱われます

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image64.png" style="width:5.87835in;height:3.67402in" />

アクティブ化完了を待ちます。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image65.png" style="width:5.87835in;height:3.67402in" />

アクティブ化完了を確認し、 **終了** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image66.png" style="width:5.87835in;height:3.67402in" />

#### 本番モードでの実行

Workspaceでアプリケーションを操作していきます。注意点は、テストモードとは異なり、各アクティビティを実行するユーザーは、各アクティビティに割り当てたユーザーもしくはグループ、アプリケーション・ロールに含まれるユーザーである必要があります。

左上の **ホーム** アイコンをクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image67.png" style="width:1.8042in;height:1.17391in" />

Integration Cloudのホームで、左のメニューにある **My Tasks** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image68.png" style="width:5.87835in;height:3.67402in" />

My Tasksの右に現れる **Workspace** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image69.png" style="width:1.84058in;height:0.45652in" />

左のメニューにある **自分のアプリ** をクリックします。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image70.png" style="width:1.74301in;height:2.29831in" />

下図のようなEPという文字が入ったアイコンが現れるはずです（背景色はランダムです）。アプリケーションの起動はこのアイコンをクリックします。以後はテストモードと同じです。

<img src="https://raw.githubusercontent.com/anishi1222/IntegrationCloud/images/DynamicProcess-Tutorial/image71.png" style="width:5.87835in;height:3.67402in" />

これで動的プロセスのチュートリアルは完了です。お好みで、このサンプルにルールやアクティビティを追加して拡張してもよいでしょう。
