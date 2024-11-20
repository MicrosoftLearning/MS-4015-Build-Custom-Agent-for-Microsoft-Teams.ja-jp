---
lab:
  title: カスタム エージェントを作成する
---
<!--
Edit the metadata above to manage the list of exercises in the home page of the GitHub site that gets generated.
You can delete the module and edit index.md in the root of the repo to customize the display so that only the exercises are listed
To enable GitHub page publishing, edit the Page settings for the repo and publish from the main branch
-->

# カスタム エージェントを作成してテストする

この演習では、カスタム エージェントを作成するための基礎として機能する Azure OpenAI リソースを作成します。

この演習の所要時間は約 **30** 分です。 <!-- update with estimated duration -->

**注:** 学習者は、独自の環境でこのラボを完了する必要があります。

##  タスク 1:Azure OpenAI リソースを作成する 

まず、...

1. お使いの Edge ブラウザーで、**https://portal.azure.com** に移動します。
1. このラボ環境で提供された資格情報を使用して、Azure portal にサインインします。
2. 画面の左上で、**[+ リソースの作成]** を選択します。
1. 検索ボックスに「**azure openai**」と入力し、Enter キーを押します。
1. **Azure OpenAI** という結果がオプションとして表示されるはずです。 このオプションの左下隅は、**[作成]** とラベルが付けられたボタンです。 **[作成]** > **Aure OpenAI**を押します。
1. **[Azure OpenAI の作成] ** ページで 次のフィールドを設定します。**注:** このラボは学習者自身の環境で完了されるべきであるため、学習者は **"サブスクリプション"**、**"価格レベル"** および **"リソース グループ"** フィールドの値を選択するときに独自の裁量で判断する必要があります。
   
   a. **[サブスクリプション]** このフィールドに入力するときは、独自の裁量で判断します。
   
   b. **[リソース グループ]** : このフィールドに入力するときは、独自の裁量で判断します。
   
   c. **[名前]** : 選択した名前を入力します。
   
   d. **[価格レベル]**: 既定値は **Standard S0** ですが、このフィールドに入力するときは独自の裁量で判断します。
   
[**次へ**] を選択します。

7. 次のページの [**ネットワーク**] タブで、**[インターネットを含むすべてのネットワーク がこのリソースにアクセスできる]** オプションを選択します。
[**次へ**] を選択します。
8. **[タグ]** タブの次のページの [名前] フィールドと [値] フィールドは空白のままにしておきます。
[**次へ**] を選択します。
9. 次のページの **[確認と送信]** タブで、 **[作成]** を押します。
10. この新しく作成された Azure OpenAI リソースが作成されているページに移動します。 "**デプロイを実行しています**" と表示されます。 このリソースのデプロイが完了するまで、数秒待ちます。 リソースがデプロイされたら、**[リソースに移動]** ボタンをクリックします
11. **結果:** 新しく作成された Azure OpenAI リソースが表示されたページにいます。 確認するには、ページの左上隅にあるリソースの名前を確認します。 この名前は、上記の手順 5c で選択した名前と一致している必要があります。


## タスク 2: Azure OpenAI モデルに RAG を実装する

それでは、...

1. 新しく作成されたAzure OpenAI リソースのページで、ページの上部にあるリボンの **[Azure OpenAI Studio に移動する]** をクリックします。
2. **[Azure OpenAI サービスへようこそ]** というタイトルの新しいページで、画面の左側にあるナビゲーション メニューの **[チャット]** をクリックします。
3. **チャット プレイグラウン** というタイトルの新しいページで、**[セットアップ]** の下にある **[+ 新しいデプロイの作成]** > **[基本モデルから]** を選択します。
4. **[チャット完了モデルを選択する]** というタイトルのポップアップ ウィンドウで、下にスクロールし、オプション **[gpt-4o]** > **[確認]** を選択します。
5. **[GPT-4o モデルをデプロイする]** ウィンドウで、すべてを既定の設定のままにして、**[デプロイ]** を選択します。
6. **チャット プレイグラウンド**ページで、画面の下部の近くにある **[データの追加]** を選択し、**[+ データ ソースの追加]** を選択します。
7. **[データ ソースの選択または追加]** ウィンドウで、**[データ ソースの選択]** のドロップダウンを選択し、**[ファイルのアップロード (プレビュー)]** を選択します。
8. **[データ ソース]** の次のページで、**[データ ソースの選択]** のドロップダウンが **[ファイルのアップロード (プレビュー)]** に設定されていることを確認します。
   
   a. **[サブスクリプション]** フィールドで、既定値が選択されていることを確認します。
   
    b. **"Azure Blob Storage リソースの選択"** フィールドで、**[新しい Azure Blob Storage リソースの作成]** を選択し、「**ストレージ アカウントの作成**」というタイトルが付けられた新しいウィンドウの **[基本]** タブの下で、**"サブスクリプション"** と **"リソース グループ"** フィールドが既定値に設定されていることを確認します。この場合、**"リソース グループ"** のみに使用可能な値を選択します。 **[インスタンスの詳細] **で、**ストレージ アカウント名** を設定します。 他のフィールドはそのままにします。 **[Review + create](レビュー + 作成)** を選択します。 **[確認と作成]** タブで、**[作成]** ボタンを選択します。 Azure Blob Storage リソースのデプロイには少し時間がかかります。
   
   c. **チャット プレイグラウンド**のウィンドウに戻ります。 **"Azure Blob Storage リソースの選択"** フィールドの横にある更新ボタンを選択し、上記の手順 b で作成したリソースを選択します。 **[CORS をオンにする]** ボタンを選択します。
   
9. **[Azure AI 検索リソースの選択]** フィールドでは、**[新しい Azure AI 検索リソースの作成]** を選択します。  **[サブスクリプション]** と **[リソース グループ]** フィールドが、選択した値に設定されていることを確認します。 **注:** このラボは学習者自身の環境で完了されるべきであるため、学習者は、**[サブスクリプション]** と **[リソース グループ]** フィールドの値を選択するときに、独自の裁量で判断する必要があります。 **[リソース グループ]** のドロップダウン値をクリックして、選択するオプションを選択します。 **サービス名**を入力し、他のすべてのフィールドが既定値に設定されていることを確認したら、**[確認と作成]** > **[作成]** を選択します。 Azure AI 検索リソースのデプロイには少し時間がかかります。
10. **チャット プレイグラウンド**のウィンドウに戻ります。 **[Azure Blob Storage リソースの選択]** フィールドの横にある更新ボタンを選択し、上記の手順 9 で作成したリソースを選択します。
11. **[インデックス名を入力]** > **[次へ]** フィールドに名前を入力します。 この名前をコピーして、アクセス可能な場所に貼り付けます。これは、今後のタスクで必要になります。
12. **[ファイルのアップロード]** セクションで、**[ファイルを参照する]** を選択します。エクスプローラーで、**[ドキュメント]** に移動します。次の 3 つのファイルをすべて選択します。**ContosoAI ChipEnhance Perks Program.docx**、**ContosoAI Insurance Plans.docx**、および**Overview of ContosoAI.docx**。 > **[開く ]** を選択します。これで、3 つのファイルがウィンドウの **[ファイルのアップロード]** ページに表示されているはずです。**[ファイルのアップロード]** > **[次へ]** を選択します。
13. **[データ管理]** セクションで、すべてを既定値のままにして、**[次へ]** を選択します。
14. **[データ接続]** で、**[API キー]** > **[次へ]** > **[保存して閉じる]** を選択します。
15. **[チャット プレイグラウンド]** ウィンドウで、ウィンドウの左上にあるリボンにある **[コードを表示]** を選択します。
16. **サンプル コード**ウィンドウで、最初のフィールドの右側にあるドロップダウンを選択して、**json** を選択し、**[キー認証]** タブに切り替えます。
    
    a. 今後のタスクで必要になる次の値をコピーして貼り付けます。**エンドポイント**、**API キー**、**Azure 検索リソース キー**。 このウィンドウを開いたままにして、今後のタスクのこれらの値を収集することもできます。

 ## タスク 3: テスト ツールと Teams でカスタム エージェントを作成してテストする

それでは、...

1. **Visual Studio Code** を開きます。
2. Visual Studio Code ウィンドウの右側で、 **[Teams Toolkit]** アイコンを選択します。**[新しいアプリの作成]** を選択します。ドロップダウンで **[カスタム エンジンのエージェント]** を選択します (注: Teams Toolkit のバージョンに応じて、**カスタム Copilot** を選択する必要があるかもしれません)。**基本的な AI チャットボット** > **JavaScript** > **Azure OpenAI**。
3. 画面の上部にある空白のボックスに、最初に次のように入力します。

   a. 前のタスクから **[API キー]** を押し、 **[Enter]** を押します。

   b. 前のタスクから **[エンドポイント]** を押し、 **Enter** を押します。

   c. **Azure Open AI デプロイ名**については、「**gpt-4o** > 」を入力し **[Enter]** を押します。

   d. **プロジェクト ルーム フォルダーが配置されるフォルダーを選択します** については、**既定フォルダー** を選択します。

   e. **[入力アプリケーション名]** には任意の名前を入力します。ポップアップ ウィンドウで **[Enter]** を押し、**はい、作成者を信頼します** を選択します。

   f. 上記の手順 a ~ f から新しく作成したアプリの新しい VS Code ウィンドウで、画面の左側にある **[Teams ツールキット]** アイコンに移動します。

   g. **[アカウント]** セクションで、 **[Microsoft 365 にサインイン]** をクリックします。 ブラウザで新しいウィンドウが開きます。 指定された資格情報を使用してログインします。

   h. アプリの VS Code ページに戻ります。 これで、[**アカウント] の下に **[カスタム アプリのアップロードが有効になっている]** という単語の緑色のチェック マークが表示されます。

   i. **[アカウント]** セクションで、**[Azure にサインイン]** をクリックします。 すべてのポップアップ ウィンドウで **[OK]** をクリックします。 ブラウザで新しいウィンドウが開きます。 指定された資格情報を使用してログインします。
   
4. アプリの VS Code ウィンドウで **src/prompts/chat/skprompt.txt** に移動します。 ファイル内のテキストを削除し、次の文章を貼り付けます。「次は AI アシスタントとの会話です。AI アシスタントは、特定のコンテキストに関する質問に回答する専門家です。」 

応答は、80 語以下の短いジャーナルスタイルにする必要があります。 

5. アプリの VS Code ウィンドウで、プロンプト/チャットの下にある **config.json** ファイルに移動します。 角かっこ内の現在のコードを削除し、次のコードを角かっこ内に貼り付けます。

```json
"data_sources": [ 
    { 
        "type": "azure_search", 
        "parameters": { 
            "endpoint": "AZURE-AI-SEARCH-ENDPOINT", 
            "index_name": "YOUR-INDEX_NAME",
            "in_scope": false,
            "authentication": { 
                "type": "api_key", 
                "key": "AZURE-AI-SEARCH-KEY" 
            } 
        } 
    } 
]
```

6. 上記のコードでは、次の値を、前のタスクから保存した値に置き換えます (注: 値は引用符で囲む必要があります)。

   a. **AZURE-AI-SEARCH-ENDPOINT** は、前のタスクの **[エンドポイント]** です。

   b. **index_name** は、前のタスクのタスク 2 ステップ 11 の **[インデックス名]** です。

   c. **[キー]** は、前のタスクの **[Azure 検索リソース キー]** です。

7.  **src/app/app.js** ファイルに移動し、azureEndpoint 行の直後に **OpenAIModel**内に次の変数を追加します。config.azureOpenAIEndpoint 

    a. azureApiVersion: '2024-02-15-preview'、
   
8. **[ファイル]** > **[すべて保存]**
    
9. キーボードの **Ctrl + Shift + d** を押すと、左上に緑色のドロップダウンとが表示されます。Wordのデバッグ > ドロップダウンを選択> **Test Toolでデバッグ** を選択 > **[F5]** を押します。
10.カスタム エンジン エージェントは、選択したデバッグ ツール内で実行され、ブラウザーで開きます。 タスク 2 のステップ 12 でアップロードされた RAG データに関連する質問をボットに問い合わせることができます。
11. アプリの VS Code ウィンドウに戻ります。 **[デバッグ]** ボタンのドロップダウンを選択し、**[Teams (Edge) で デバッグ]** を選択して、**[F5]** または [gren] 再生ボタンを押します。
13. Edge ブラウザで新しいウィンドウが開きます。 サインインを求めるメッセージが表示されます。 指定されたログイン情報を使用してサインインします。 正常にサインインしたら ウィンドウを閉じます。
14. 手順 11 をもう一度繰り返します。 新しく作成したアプリのタイトルが表示されたウィンドウが表示されます。 **[追加]** > **[開く]** を選択します。
15. おめでとうございます! RAG データ ファイルに含まれている質問をエージェントに問い合わせることができるようになりました。 