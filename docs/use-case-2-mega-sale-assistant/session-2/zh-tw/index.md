# 使用案例 2 – Mega Sale Assistant（第 2 堂）

[English](../en/index.md) | 繁體中文

## 關於本實驗

你在 **Tailwind Traders** 工作，這是一家模擬的美妝與保健零售商。

> Tailwind Traders 是虛構的公司。請閱讀[公司簡介](../../../../README.md)。

在本實驗中，你要在 **Copilot Studio** 中建立 **Mega Sale Assistant**，並將它擴充成 **Store Issue Reporter**：這個代理程式除了繼續回答 Mega Sale 問題，還能收集門市問題回報、標示緊急問題，並寄出確認信。Agent Builder 很適合用來建立負責回答和撰寫內容的代理程式；當代理程式每次都必須依照固定流程執行時，就要使用 Copilot Studio 的 **standard harness**（標準架構），透過**主題**一步一步設計對話。

- **工具：** Microsoft Copilot Studio（standard harness）
- **預估時間：** 60–75 分鐘
- **難度：** 入門，不需要寫程式（只需要複製貼上幾個公式）

你將學會：

- 在 Copilot Studio 中建立代理程式，並加入文件作為知識
- 建立一個依固定順序提問的**主題**，並將答案存入**變數**
- 使用**條件**和簡單的公式標示緊急問題，並產生回報編號
- 加入一個寄送確認信的**連接器工具**

---

## 開始之前

請在實驗開始前逐項確認。選取檔案名稱即可下載，並將檔案存放在容易找到的地方，例如桌面。

> 所有檔案皆為**虛構內容**，僅供訓練使用。

| ✔ | 需要準備的項目 | 說明 |
|---|---|---|
| ☐ | 可以使用 **Microsoft Copilot Studio** 的公司帳戶 | 前往 [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)，並使用公司帳戶登入 |
| ☐ | 一個可以建置的**環境** | 在右上角選取 **environment** 選擇器，選擇你的 **developer environment** 或本課程提供的環境 |
| ☐ | 一個 **Outlook** 信箱（僅工作 4 需要） | 你可以用同一個帳戶開啟 Outlook。如果 Office 365 Outlook 連接器被封鎖，請依照[附註：如果無法使用 Outlook 連接器](#附註如果無法使用-outlook-連接器)操作。你仍然可以完成整個實驗，做出一個完整、可運作的代理程式 |
| ☐ | 檔案 1（PDF）：[`Mega-Sale-Playbook.pdf`](Mega-Sale-Playbook.pdf) | Mega Sale 活動手冊：特賣日期、折扣規則、不適用折扣商品、Tailwind Rewards 雙倍點數、優惠券併用規則 |
| ☐ | 檔案 2（Word）：[`Store-Execution-Guide.docx`](Store-Execution-Guide.docx) | 門市執行指南：陳列完成期限、結帳處理程序、價格標籤錯誤、缺貨處理、人力安排 |
| ☐ | 檔案 3（Word）：[`Mega-Sale-FAQ.docx`](Mega-Sale-FAQ.docx) | Mega Sale 常見問題：員工及顧客對這次特賣的常見問題 |

這些檔案和第 1 堂使用的檔案相同。如果你已經有這些檔案，就不需要再下載一次。

---

## 情境

總部的**門市營運團隊**每天要透過電話和 Teams 回答門市大約 **50 個問題**。門市問題（例如陳列損壞、缺貨和價格錯誤）的回報格式五花八門，而且常常缺少門市代碼或商品名稱等關鍵資訊，團隊只好再打電話回去詢問。

每年 12 月 1 日到 7 日的 **Mega Sale Week（年度大促銷週）** 期間會更忙。特賣期間，問題數量會增加到平常的**三倍**，門市也會遇到像這樣的問題：

- *商品賣完了。*
- *貨架標籤上的價格不對。*
- *收銀台結帳時沒有套用特價折扣。*

緊急的價格問題卻和其他所有問題排在同一個佇列中等待處理。

為了第一次就收到完整的回報，並優先處理緊急問題，Tailwind Traders 希望有一個代理程式，能回答門市的 Mega Sale 問題，並用相同格式收集每一筆問題回報。

**你的任務：**

1. 在 Copilot Studio 中建立 **Mega Sale Assistant**，讓它根據特賣文件回答 Mega Sale 問題。
2. 建立 **回報門市問題** 主題，詢問門市代碼、問題類型、商品和詳細說明。
3. 將影響顧客支付價格的問題標示為緊急，並產生回報編號。
4. 以電子郵件寄出回報確認信，讓 Mega Sale 專案團隊可以後續追蹤。

---

## 工作 1 – 建立代理程式

在這項工作中，你要在 Copilot Studio 中建立 Mega Sale Assistant，並提供特賣文件讓它根據這些文件回答。

### 工作 1.1 – 建立代理程式

1. 前往 Microsoft Copilot Studio [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com) 並登入。
2. 確認已選取正確的 **environment**。
3. 在 **Home** 頁面選取 **Agent**。你也可以在左側導覽列選取 **Agents**。
4. 選取 **New agent** 旁的向下箭號（**⌄**），再選取 **Agent**（**Standard**）。

   > [!NOTE]
   > 我們使用的是支援主題的 **standard harness**。

5. 為代理程式命名，並檢查設定：

   - **Name**

     ```text
     Mega Sale Assistant
     ```

   - **Agent settings (Optional)**
     - **Language：** English
     - **Solution：** 保留預設的 solution。
     - **Schema name**

       ```text
       lab_megasale
       ```

   > [!NOTE]
   > **Language** 請保持 **English**。我們會在指示中加上「一律使用繁體中文回覆」，讓代理程式用中文回答。

6. 選取 **Create**。
7. 在 **Overview** 索引標籤的 **Details** 區段中選取 **Edit**，並填寫下列欄位：

   - **Description**

     ```text
     回答 Mega Sale Week 的相關問題，並協助門市在特賣期間回報問題。
     ```

   - **Instructions**

     ```text
     # 角色
     你是 Mega Sale Assistant。你協助門市和總部團隊處理
     Mega Sale Week 的相關事務。

     # 你要做的事
     - 只根據提供的知識，回答有關特賣日期、折扣規則、不適用折扣商品、
       Tailwind Rewards 點數、優惠券、門市準備工作、結帳問題、價格標籤、
       缺貨商品及人力安排的問題。一律註明來源文件。
     - 當使用者想回報門市發生的問題時，使用「回報門市問題」主題。

     # 回答方式
     - 回答簡短、親切。說明處理程序時使用編號步驟。

     # 回覆方式
     - 一律使用繁體中文回覆。

     # 你不做的事
     - 如果知識中沒有答案，請直接說明，並建議洽詢區域經理或
       Mega Sale 專案團隊。不要猜測。
     - 不討論員工或顧客的個人資訊。
     - 不評論競爭對手的價格或促銷活動。
     ```

8. 選取 **Save**。

### 工作 1.2 – 加入知識並測試

1. 選取 **Knowledge** 索引標籤。
2. 選取 **Add knowledge** > **Upload file**，上傳 `Mega-Sale-Playbook.pdf`、`Store-Execution-Guide.docx` 和 `Mega-Sale-FAQ.docx`（**開始之前** 中的檔案 1–3），再選取 **Add to agent**。
3. 三個檔案都顯示 **Ready** 後，將下列問題複製到 **Test your agent** 面板並送出：

   ```text
   哪些商品不適用特價折扣？
   ```

   **預期結果：** 代理程式回答**處方藥、嬰兒配方奶粉、禮物卡及出清商品**，並附上來源參考。

---

## 工作 2 – 建立「回報門市問題」主題

在這項工作中，你要建立一個主題，向門市人員詢問四個問題並儲存答案。**主題**是由你設計的對話路徑：你可以精確決定代理程式要問哪些問題、依什麼順序提問，讓每一筆回報都包含相同的資訊。

### 工作 2.1 – 建立主題

1. 選取 **Topics** 索引標籤，再選取 **Add a topic** > **From blank**。
2. 選取 **Details**，並填寫主題詳細資料：

   - **Name**

     ```text
     回報門市問題
     ```

   - **Description:**

     ```text
     收集 Mega Sale Week 期間門市問題的詳細資訊，標示緊急問題，並寄出確認信。
     ```

   - **Model description:**

     ```text
     當門市人員想回報 Mega Sale Week 期間發生的問題時使用這個主題，
     例如商品缺貨、價格標籤錯誤、結帳時折扣未生效或顧客客訴。
     ```

3. 加入 **Send a message** 節點。

   在 **Trigger** 節點下方，選取 **+** > **Send a message**，並貼上：

   ```text
   我會協助你將門市問題回報給 Mega Sale 專案團隊。只需要提供 4 項簡單資訊。
   ```

4. 選取 **Save**。
5. 確認門市人員想回報問題時，代理程式會啟動這個主題。將下列文字複製到 **Test your agent** 面板並送出：

   ```text
   我要回報門市問題
   ```

   **預期結果：** 代理程式啟動 **回報門市問題** 主題，並顯示你設定的開場訊息。你不需要撰寫任何觸發片語（trigger phrases）：代理程式會根據 **Model description** 判斷什麼時候要啟動這個主題。

### 工作 2.2 – 詢問四個問題

每一筆回報都需要相同的四項資訊，這樣門市營運團隊就不必再打電話回去詢問。每個答案都會存到一個**變數**中，方便之後在摘要和電子郵件中使用。

加入 4 個提問節點。

1. 加入第一個問題，詢問門市代碼。在 **Send a message** 節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     你的門市代碼是什麼？（例如 S012）
     ```

   - **Identify：** **User's entire response**
   - 將變數重新命名為：

     ```text
     StoreCode
     ```

2. 加入第二個問題，詢問問題類型。在 **你的門市代碼是什麼？** 提問節點下方，選取 **+** > **Ask a question**。

   > [!NOTE]
   > 如果 Copilot Studio 在選擇題下方自動加入 **Condition** 節點，請將它**刪除**。你會在工作 3 建立自己的條件。

   - 問題文字：

     ```text
     是哪一種問題？
     ```

   - **Identify：** **Multiple choice options**。加入下列選項：

     ```text
     缺貨
     ```

     ```text
     價格標籤錯誤
     ```

     ```text
     結帳時折扣未生效
     ```

     ```text
     顧客客訴
     ```

     ```text
     其他
     ```

   - 將變數重新命名為：

     ```text
     IssueType
     ```

3. 加入第三個問題，詢問受影響的商品。在 **是哪一種問題？** 提問節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     哪一項商品受到影響？請輸入商品名稱，或輸入 N/A。
     ```

   - **Identify：** **User's entire response**
   - 將變數重新命名為：

     ```text
     ProductName
     ```

4. 加入第四個問題，詢問問題的詳細說明。在 **哪一項商品受到影響？** 提問節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     請用一、兩句話說明問題。
     ```

   - **Identify：** **User's entire response**
   - 將變數重新命名為：

     ```text
     IssueDetails
     ```

> [!TIP]
> **什麼是變數？** 變數就像一個貼有標籤的盒子，用來存放使用者的答案，方便你在對話後段使用（例如放進摘要或電子郵件中）。`Topic.StoreCode` 的意思是「這個主題中的 StoreCode 盒子」。

5. 選取 **Save**，然後將下列文字複製到 **Test your agent** 面板並送出：

   ```text
   我要回報門市問題
   ```

   **預期結果：** 代理程式依序詢問四個問題，並以按鈕顯示問題類型。你的答案會存放在 `StoreCode`、`IssueType`、`ProductName` 和 `IssueDetails` 中。

---

## 工作 3 – 加入規則和摘要

在這項工作中，你要產生回報編號、標示緊急問題，並向門市人員顯示回報摘要。

### 工作 3.1 – 產生回報編號

每一筆回報都需要專屬的編號，方便門市和總部日後查詢。在這項工作中，你要用目前的日期和時間產生編號。

1. 在 **請用一、兩句話說明問題。** 提問節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **計算內容：** 根據日期和時間產生的回報編號，例如 `MS-1201-093015`。
   - **Variable：** 建立新的變數，並命名為：

     ```text
     ReportID
     ```

   - **To value：** 選取 **Formula**，並貼上：

     ```text
     "MS-" & Text(Now(), "mmdd-hhmmss")
     ```

### 工作 3.2 – 標示緊急問題

並不是每個問題都一樣緊急。影響**顧客支付價格**的問題（**價格標籤錯誤**或**結帳時折扣未生效**）需要立即處理。在這項工作中，代理程式會設定優先順序、將它存到新的變數 `Priority` 中，並針對緊急問題提供門市人員應先執行的步驟。

請在條件**之前**建立 `Priority`。各個分支位於同一層級，所以在某個分支中建立的變數，無法在其他分支中選取。

1. 在 **ReportID** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新的變數，並命名為：

     ```text
     Priority
     ```

   - **To value：** 選取 **Formula**，並貼上下列內容。這會先將變數設為空白文字，之後再由各分支填入值。

     ```text
     ""
     ```

2. 在 **Priority** 節點下方，選取 **+** > **Add a condition**。
3. 在 **Condition** 分支中：
   - 設定條件：`IssueType` is equal to `價格標籤錯誤` **Or** `IssueType` is equal to `結帳時折扣未生效`
   - 在 **Condition** 分支下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `Priority`。
   - **To value：** 貼上：

     ```text
     緊急
     ```

   - 在 **Condition** 分支的 **Priority** 節點下方，選取 **+** > **Send a message**，並貼上：

     ```text
     ⚠️ 這個問題已標示為「緊急」，因為它會影響顧客支付的價格。
     在總部審核你的回報期間，請依照門市執行指南處理：
     • 價格標籤錯誤：立即更正標籤，並以標示的較低價格售予這位顧客。
     • 結帳時折扣未生效：確認商品符合折扣資格並重新掃描，然後手動改價，並輸入主管核准碼。
     ```

4. 在 **All other conditions** 中：
   - 選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `Priority`。
   - **To value：** 貼上：

     ```text
     一般
     ```

### 工作 3.3 – 顯示摘要

最後，代理程式會在一則訊息中顯示回報的所有詳細資料，讓門市人員可以檢查內容，並記下回報編號。

1. 在工作 3.2 的緊急條件下方（各分支會合的地方），選取 **+** > **Send a message**，並貼上下列文字。請使用 **{x}** 插入變數：

   ```text
   ✅ 你的回報已登錄。

   回報編號：{Topic.ReportID}
   優先順序：{Topic.Priority}
   門市：{Topic.StoreCode}
   問題類型：{Topic.IssueType}
   商品：{Topic.ProductName}
   詳細說明：{Topic.IssueDetails}
   ```

2. 選取 **Save**。

### 工作 3.4 – 測試兩條路徑

在這項工作中，你要測試緊急路徑和一般路徑。

**測試方式：**

- 每次測試都開始新的測試工作階段。
- 依序送出下列每一項輸入，作為你對代理程式提問的回答。

**A.** 價格標籤錯誤（緊急）

1. 送出：

   ```text
   我要回報門市問題
   ```

2. 門市代碼：

   ```text
   S012
   ```

3. 問題類型：

   ```text
   價格標籤錯誤
   ```

4. 商品：

   ```text
   HydraGlow Hydrating Toner 200ml
   ```

5. 詳細說明：

   ```text
   貨架標籤上的價格比收銀台的價格低。
   ```

**預期結果：** 先出現緊急訊息，接著摘要顯示 **優先順序：緊急** 和一個回報編號。

**B.** 缺貨（一般）

1. 送出：

   ```text
   我要回報門市問題
   ```

2. 門市代碼：

   ```text
   S012
   ```

3. 問題類型：

   ```text
   缺貨
   ```

4. 商品：

   ```text
   HydraGlow Hydrating Toner 200ml
   ```

5. 詳細說明：

   ```text
   這項商品在特賣第一天早上就賣完了。
   ```

**預期結果：** 沒有緊急訊息；摘要顯示 **優先順序：一般** 和一個回報編號。

---

## 工作 4 – 寄送確認信

在這項工作中，你要讓代理程式以電子郵件寄出回報確認信。在本實驗中，這封信會寄**給你自己**，代替 Mega Sale 專案團隊的信箱，所以你不需要其他人的電子郵件地址。你會先將 Outlook 工具加入代理程式，再從 **回報門市問題** 主題中呼叫它。

如果 Office 365 Outlook 連接器無法使用或顯示 **Blocked by policy**，請略過這項工作，前往本頁最後的[附註：如果無法使用 Outlook 連接器](#附註如果無法使用-outlook-連接器)。

### 工作 4.1 – 將 Outlook 工具加入代理程式

1. 前往 **Tools** 索引標籤，選取 **Add a tool**。
2. 選取 **Connector**，搜尋 `Office 365 Outlook`，再選取 **Send an email (V2)**。
3. 設定連線：
   - 選取 **Connection**。如果顯示 **Not connected**，請選取 **Create new connection**。
   - **Display name：**

     ```text
     Lab Mega Sale Assistant
     ```

   - 選取 **Create**，並使用你的 Microsoft 365 公司帳戶登入。
4. 選取 **Add and configure**。
5. 在工具頁面的 **Additional details** 下，將 **When this will be used** 設為 **Only when referenced by topics or agents**。

   > 這樣代理程式只會從 **回報門市問題** 主題寄出電子郵件，絕不會自行決定寄信。

6. **Inputs** 保持原樣，選取 **Save**。

> [!NOTE]
> 請不要在工具頁面上填寫輸入值。工具的輸入值無法使用主題的變數，例如 `Topic.ReportID`。在工作 4.2 中，你會先將電子郵件內容放進**全域**變數，讓工具可以使用。

### 工作 4.2 – 準備電子郵件內容

工具的輸入值只能使用**全域**變數和**系統**變數，不能使用主題變數。在這項工作中，你要用主題的變數組出電子郵件的主旨和內文，並存到兩個全域變數中。

1. 前往 **Topics** 索引標籤，開啟 **回報門市問題** 主題。
2. 在工作 3.3 的摘要訊息節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新的變數，並命名為：

     ```text
     EmailSubject
     ```

   - 選取這個變數以開啟 **Variable properties**，並在 **Usage** 下選取 **Global (any topic can access)**。名稱會變成 `Global.EmailSubject`。
   - **To value：** 選取 **Formula**，並貼上：

     ```text
     "[門市問題] " & Topic.ReportID & " – " & Topic.Priority & " – 門市 " & Topic.StoreCode
     ```

3. 在 **EmailSubject** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新的變數，並命名為：

     ```text
     EmailBody
     ```

   - 選取這個變數以開啟 **Variable properties**，並在 **Usage** 下選取 **Global (any topic can access)**。名稱會變成 `Global.EmailBody`。
   - **To value：** 選取 **Formula**，並貼上：

     ```text
     "<p>收到一筆新的 Mega Sale 門市問題回報。</p>" &
     "<p><b>回報編號：</b>" & Topic.ReportID & "<br>" &
     "<b>優先順序：</b>" & Topic.Priority & "<br>" &
     "<b>門市：</b>" & Topic.StoreCode & "<br>" &
     "<b>問題類型：</b>" & Text(Topic.IssueType) & "<br>" &
     "<b>商品：</b>" & Topic.ProductName & "<br>" &
     "<b>詳細說明：</b>" & Topic.IssueDetails & "<br>" &
     "<b>回報人：</b>" & System.User.DisplayName & "</p>"
     ```

4. 選取 **Save**。

> [!NOTE]
> `System.User.Email` 和 `System.User.DisplayName` 是**系統變數**：代理程式會根據登入的使用者自動填入，所以你不需要另外詢問。

### 工作 4.3 – 從主題寄出電子郵件

1. 在 **EmailBody** 節點下方，選取 **+** > **Add a tool**，再選取 **Send an email (V2)**。
2. 在 **Inputs** 區段中，依下列方式設定每個輸入值。請不要自行輸入變數名稱，而是從清單中**選取**。
   - **To**
     - **Fill using：** 選取 **Custom value**。
     - **Value：** 選取值方塊，前往 **System** 索引標籤，再選取 **User.Email**。
   - **Subject**
     - **Fill using：** 選取 **Custom value**。
     - **Value：** 選取值方塊，前往 **Custom** 索引標籤，再選取 **Global.EmailSubject**。
   - **Body**
     - **Fill using：** 選取 **Custom value**。
     - **Value：** 選取值方塊，前往 **Custom** 索引標籤，再選取 **Global.EmailBody**。

3. 在 **Send an email (V2)** 節點下方，選取 **+** > **Send a message**，並貼上：

   ```text
   📧 確認信已寄出。請保留你的回報編號，以便後續追蹤。
   ```

4. 選取 **Save**。

### 工作 4.4 – 測試電子郵件

1. 開始新的測試工作階段，並依序送出下列每一項輸入。
   - 送出：

     ```text
     我要回報門市問題
     ```

   - 門市代碼：

     ```text
     S012
     ```

   - 問題類型：

     ```text
     價格標籤錯誤
     ```

   - 商品：

     ```text
     HydraGlow Hydrating Toner 200ml
     ```

   - 詳細說明：

     ```text
     貨架標籤上的價格比收銀台的價格低。
     ```

   如果測試面板要求你連線或允許存取，請選取 **Allow**。

2. 檢查你的收件匣。

   **預期結果：** 你會收到一封主旨類似 `[門市問題] MS-1201-093015 – 緊急 – 門市 S012` 的電子郵件。回報編號會顯示你測試時的日期和時間。

---

## 工作 5 – 測試完整流程

在這項工作中，你要測試完整的對話：詢問（Ask）、收集（Collect）、判斷（Decide）和寄送（Send）。

1. 開始新的測試工作階段，並送出：

   ```text
   門市陳列的完成期限是什麼時候？
   ```

   **預期結果：** 代理程式根據知識回答：**11 月 30 日 22:00**（**詢問**）。

2. 送出：

   ```text
   我的門市出了問題，需要回報
   ```

   **預期結果：** 代理程式啟動 **回報門市問題** 主題（**收集**）。

3. 回答代理程式的問題。依序送出下列每一項輸入。
   - 門市代碼：

     ```text
     S012
     ```

   - 問題類型：

     ```text
     結帳時折扣未生效
     ```

   - 商品：

     ```text
     HydraGlow Hydrating Toner 200ml
     ```

   - 詳細說明：

     ```text
     這項商品在收銀台結帳時沒有套用 30% 折扣。
     ```

   **預期結果：** 代理程式顯示緊急訊息（**判斷**），接著顯示摘要，並寄出確認信（**寄送**）。如果你依照[附註：如果無法使用 Outlook 連接器](#附註如果無法使用-outlook-連接器)操作，就不會寄出電子郵件。

---

## 總結

你在 **Mega Sale Assistant** 中建立了 **Store Issue Reporter**：

- ✅ **詢問（Ask）：** 根據 Mega Sale 文件回答問題
- ✅ **收集（Collect）：** 一個包含四個問題和變數的主題
- ✅ **判斷（Decide）：** 一個標示緊急問題的條件，加上回報編號
- ✅ **寄送（Send）：** 一個寄送確認信的連接器工具

### 重點

- **主題**讓代理程式每次都依相同順序問相同的問題，確保每一筆回報都完整。
- **條件**和簡單的公式讓代理程式可以套用規則，例如標示緊急問題。
- **詢問 → 收集 → 判斷 → 寄送** 幾乎適用於你團隊中的任何申請：維修申請、補貨申請、IT 問題、活動報名等等。想一想你的團隊中，有哪個流程常常收到資訊不完整的申請？那就很適合做成你的下一個代理程式。

---

## 附註：如果無法使用 Outlook 連接器

如果 Office 365 Outlook 連接器無法使用或顯示 **Blocked by policy**，請略過工作 4，改在摘要訊息中加入一行文字。

1. 開啟 **回報門市問題** 主題。
2. 在工作 3.3 的摘要訊息最後，新增一行並貼上：

   ```text
   請複製這份摘要，並傳送給 Mega Sale 專案團隊。
   ```

3. 選取 **Save**。
4. 用工作 5 的步驟測試代理程式。

> 你的代理程式仍然可以**收集**完整的回報，並**判斷**優先順序。唯一沒有做到的步驟，就是自動寄出回報。
