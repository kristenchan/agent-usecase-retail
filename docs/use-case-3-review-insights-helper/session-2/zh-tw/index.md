# 使用案例 3 – Review Insights Helper（第 2 堂）

[English](../en/index.md) | 繁體中文

## 關於本實驗

你在 **Tailwind Traders** 工作，這是一家模擬的健康與美妝零售商。

> Tailwind Traders 是一家虛構的公司。請閱讀[公司介紹](../../../../README.md)。

在本實驗中，你要在 Copilot Studio 中建立 **Review Response Helper**。員工把一則新的顧客評論交給代理程式，代理程式會為評論分類、用 Tailwind Traders 的品牌語氣草擬回覆，並把健康與安全問題升級處理。整個過程都由人來掌控：代理程式只會**草擬**回覆，絕對不會自行張貼；而評論是否提到健康或安全問題，是由員工判斷，不是由 AI 判斷。

- **工具：** Microsoft Copilot Studio（標準架構）
- **預估時間：** 60–75 分鐘
- **難度：** 初階到中階，不需要寫程式（公式只要複製貼上）

你會學到：

- 在 Copilot Studio 中把品牌語氣指南和客戶服務指南加入為**知識**
- 建立一個**主題**，用選擇題、文字、**Number** 和 **Yes/No** 問題收集資訊
- 用**條件**為評論分類，並在主題中用**生成式回答**草擬回覆
- 只在需要時，用**連接器工具**（Office 365 Outlook）寄送升級通知郵件

---

## 開始之前

實驗開始前，請逐一確認以下項目。選取檔案名稱即可下載，並把檔案存放在容易找到的地方，例如桌面。

> 所有檔案都是**虛構的**，僅供訓練使用。

| ✔ | 你需要準備 | 說明 |
|---|---|---|
| ☐ | 可以使用 **Copilot Studio** | 前往 [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)，確認可以開啟 |
| ☐ | 一個可以建立代理程式的**環境** | 使用右上角的**環境**選擇器 |
| ☐ | 一個 **Outlook** 信箱（只有工作 4 需要） | 確認可以用同一個帳戶開啟 Outlook |
| ☐ | 檔案 1（Word）：[`Brand-Voice-Guide.docx`](Brand-Voice-Guide.docx) | 品牌語氣指南：語氣、建議用語與避免用語、主題標籤和規定用語 |
| ☐ | 檔案 2（Word）：[`Customer-Service-Guide.docx`](Customer-Service-Guide.docx) | 客戶服務指南：客訴處理、退款與換貨、健康與安全問題升級處理，以及回覆的該做與不該做 |

---

## 情境

Tailwind Traders 的顧客體驗團隊目前是手動回覆每一則顧客評論。每則回覆大約要花 **10 分鐘**：要拿捏語氣，還要查閱**客戶服務指南**。回覆的品質也落差很大。有些太短，有些承諾了公司無法提供的退款；還有一些提到**皮膚刺激或過敏**的評論，被當成一般客訴來回覆，但健康與安全相關的評論必須盡快升級給**品質團隊**。

到了每年 12 月 1 日到 7 日舉辦的 **Mega Sale Week**（年度大促銷週），工作量更大。Mega Sale Week 期間，7 天內會收到大約 **150 則評論**，每一則都需要回覆：

- *「好喜歡組合優惠，超划算！」*
- *「身體乳的壓頭漏得到處都是。」*
- *「質地很好，但用了之後皮膚發紅又發癢。」*

為了幫助團隊更快、更一致地回覆，而且絕不漏掉任何安全問題，Tailwind Traders 希望有一個代理程式，能回答如何回覆顧客的問題，並處理每一則新評論：為評論分類、草擬符合品牌語氣的回覆讓員工確認，並立即把健康與安全問題升級處理。

**你的任務：**

1. 在 Copilot Studio 中建立 **Review Response Helper**，讓它根據品牌語氣指南和客戶服務指南，回答如何回覆顧客的問題。
2. 建立**處理顧客評論**主題，詢問通路、商品、評分、評論內容，以及評論是否提到健康或安全疑慮。
3. 用固定規則為評論分類，並草擬符合品牌語氣的回覆讓員工確認。
4. 寄電子郵件通知品質團隊健康與安全問題，讓他們能盡快處理。

---

## 工作 1 – 建立代理程式

在這項工作中，你要在 Copilot Studio 中建立 Review Response Helper，並提供品牌語氣指南和客戶服務指南作為回答依據。

### 工作 1.1 – 建立代理程式

1. 前往 Microsoft Copilot Studio：[https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)，並登入。
2. 確認選取了正確的**環境**。
3. 在 **Home** 頁面上，選取 **Agent**。你也可以在左側導覽中選取 **Agents**。
4. 在 **New agent** 旁邊，選取向下箭號（**⌄**），然後選取 **Agent**（**Standard**）。

   > [!NOTE]
   > 我們使用的是支援主題的**標準架構**（standard harness）。

5. 為代理程式命名並檢查設定：

   - **Name**

     ```text
     Review Response Helper
     ```

   - **Agent settings (Optional)**
     - **Language：** English
     - **Solution：** 保留預設的解決方案。
     - **Schema name**

       ```text
       lab_reviewhelper
       ```

6. 選取 **Create**。
7. 在 **Overview** 索引標籤的 **Details** 區段中，選取 **Edit**，然後填寫下列欄位：

   - **Description**

     ```text
     協助員工為顧客評論分類、草擬符合品牌語氣的回覆，並將健康與安全問題升級處理。
     ```

   - **Instructions**

     ```text
     # 角色
     你是 Review Response Helper，協助負責回覆顧客評論的員工。

     # 你要做的事
     - 根據品牌語氣指南和客戶服務指南，回答如何回覆顧客的問題。
     - 當使用者需要協助處理某一則顧客評論時，使用
       「處理顧客評論」主題。

     # 回覆規則
     - 回覆都是草稿，由員工檢查。你絕對不會自行張貼回覆。
     - 在公開回覆中，絕對不要承認法律責任、討論賠償或承諾退款。
       請顧客與客戶服務團隊聯絡。
     - 絕對不要做出醫療宣稱。
     - 不要試圖辨識顧客身分。

     # 回覆方式
     一律使用繁體中文回覆。
     ```

8. 選取 **Save**。

### 工作 1.2 – 新增知識並測試

1. 選取 **Knowledge** 索引標籤。
2. 選取 **Add knowledge** > **Upload file**，上傳**開始之前**的 `Brand-Voice-Guide.docx`（檔案 1）和 `Customer-Service-Guide.docx`（檔案 2），然後選取 **Add to agent**。
3. 檔案狀態變成 **Ready** 後，把下列問題複製到 **Test your agent** 面板中送出：

   ```text
   公開回覆客訴時，絕對不能說什麼？
   ```

   **預期結果：** 代理程式根據客戶服務指南回答（例如：不要承認法律責任、不要公開討論賠償）。

---

## 工作 2 – 建立「處理顧客評論」主題

在這項工作中，你要建立一個主題，向員工詢問關於評論的五個問題，並儲存答案。

### 工作 2.1 – 建立主題

1. 選取 **Topics** 索引標籤，然後選取 **Add a topic** > **From blank**。
2. 選取 **Details**，填寫主題詳細資料：

   - **Name**

     ```text
     處理顧客評論
     ```

   - **Description:**

     ```text
     收集顧客評論的詳細資料、為評論分類、草擬回覆，並將健康與安全問題升級處理。
     ```

   - **Model description:**

     ```text
     當員工需要協助回覆、分類或升級處理某一則顧客評論或顧客留言時，
     使用這個主題。
     ```

3. 新增 **Send a message** 節點。

   在 **Trigger** 節點下方，選取 **+** > **Send a message**，然後貼上：

   ```text
   我們來處理這則評論。我會先問你 5 個簡單的問題，再草擬一則回覆讓你確認。
   ```

4. 選取 **Save**。
5. 確認員工請求協助處理評論時，主題會啟動。把下列文字複製到 **Test your agent** 面板中送出：

   ```text
   幫我回覆一則顧客評論
   ```

   **預期結果：** 代理程式啟動**處理顧客評論**（Handle a customer review）主題，並顯示你的開場訊息。你不需要撰寫任何觸發片語：代理程式會根據 **Model description** 決定何時啟動這個主題。

### 工作 2.2 – 詢問五個問題

代理程式需要先取得評論的詳細資料，才能為評論分類並草擬回覆。在這項工作中，你要新增 5 個問題節點。其中兩個使用特殊類型：**Number** 會把評分儲存為數字，讓代理程式可以比較大小；**Boolean** 會把安全問題的答案儲存為 true 或 false。

1. 新增第一個問題，詢問通路。在 **Send a message** 節點下方，選取 **+** > **Ask a question**。

   > [!NOTE]
   > 如果 Copilot Studio 在選擇題下方自動新增了 **Condition** 節點，請把它**刪除**。你會在工作 3 建立自己的條件。

   - 問題文字：

     ```text
     這則評論是在哪裡發表的？
     ```

   - **Identify：** **Multiple choice options**。新增下列選項：

     ```text
     門市問卷
     ```

     ```text
     網路商店
     ```

   - 把變數重新命名為：

     ```text
     Channel
     ```

2. 新增第二個問題，詢問商品。在 **這則評論是在哪裡發表的？** 問題節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     評論是關於哪一項商品？（沒有特定商品請輸入「無」）
     ```

   - **Identify：** **User's entire response**
   - 把變數重新命名為：

     ```text
     ProductName
     ```

3. 新增第三個問題，詢問評分。在 **評論是關於哪一項商品？（沒有特定商品請輸入「無」）** 問題節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     顧客給了幾分（1–5）？
     ```

   - **Identify：** **Number**
   - 把變數重新命名為：

     ```text
     Rating
     ```

4. 新增第四個問題，詢問評論內容。在 **顧客給了幾分（1–5）？** 問題節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     請貼上評論內容。
     ```

   - **Identify：** **User's entire response**
   - 把變數重新命名為：

     ```text
     ReviewText
     ```

5. 新增第五個問題，詢問健康與安全。在 **請貼上評論內容。** 問題節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     這則評論是否提到健康或安全疑慮，例如皮膚刺激、過敏反應或受傷？（請回答 Yes 或 No）
     ```

   - **Identify：** **Boolean**（Yes/No）
   - 把變數重新命名為：

     ```text
     SafetyConcern
     ```

   > [!TIP]
   > **為什麼要由人來判斷安全問題？** 判斷某件事是否屬於健康或安全問題，是**高風險的判斷**。在這個設計中，由人來回答這個問題，代理程式再根據答案套用固定規則。讓 AI 負責撰寫，把關鍵決策留給人和規則。

   > [!NOTE]
   > 代理程式的語言是英文，**Boolean** 問題只能辨識英文的是／否，所以測試時請輸入 `Yes` 或 `No`。

6. 選取 **Save**，然後把下列文字複製到 **Test your agent** 面板中送出：

   ```text
   幫我回覆一則顧客評論
   ```

   **預期結果：** 代理程式會問完五個問題，而且你可以看到 `Rating` 儲存為數字，`SafetyConcern` 儲存為 true/false。

---

## 工作 3 – 為評論分類並草擬回覆

在這項工作中，你要新增為評論分類的規則、顯示分類結果，再讓代理程式用生成式回答草擬回覆。

### 工作 3.1 – 為評論分類

代理程式用固定規則為每則評論分類，所以同一則評論每次都會得到相同的分類。分類結果取決於安全問題的答案和評分。你要把結果以文字儲存在新變數 `Severity` 中。

請在條件**之前**建立 `Severity`。各個分支位於同一層，在某個分支中建立的變數，無法在其他分支中選取。

1. 在 **這則評論是否提到健康或安全疑慮，例如皮膚刺激、過敏反應或受傷？（請回答 Yes 或 No）** 問題節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新變數，並命名為：

     ```text
     Severity
     ```

   - **To value：** 選取 **Formula**，然後貼上下列內容。這會先把變數設為空白文字，之後再由各個分支填入值。

     ```text
     ""
     ```

2. 在 **Severity** 節點下方，選取 **+** > **Add a condition**。
3. 在新的 **Condition** 節點上，選取 **+**（Add condition branch）兩次。
   - 現在你有**三個**條件分支，外加 **All other conditions**。
   - 系統會**由左到右**檢查各個分支，並使用第一個成立的分支。

4. 在**分支 1** 中：
   - 設定條件：`SafetyConcern` is equal to `true`
   - 在分支 1 下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `Severity`。
   - **To value：** 貼上：

     ```text
     🔴 需升級處理
     ```

5. 在**分支 2** 中：
   - 設定條件：`Rating` is less than or equal to `2`
   - 在分支 2 下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `Severity`。
   - **To value：** 貼上：

     ```text
     🟠 優先
     ```

6. 在**分支 3** 中：
   - 設定條件：`Rating` is equal to `3`
   - 在分支 3 下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `Severity`。
   - **To value：** 貼上：

     ```text
     🟡 一般
     ```

7. 在 **All other conditions** 中：
   - 選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `Severity`。
   - **To value：** 貼上：

     ```text
     🟢 感謝
     ```

> [!NOTE]
> **安全**規則刻意放在第一個。提到過敏反應的評論，**即使評分是 5 分**，也會升級處理。

### 工作 3.2 – 顯示分類結果

員工需要看到分類結果，才知道該怎麼處理這則評論。在這項工作中，代理程式會用訊息顯示分類結果。

1. 在工作 3.1 的分類條件下方（各分支會合的地方），選取 **+** > **Send a message**，然後貼上下列文字。使用 **{x}** 插入變數：

   ```text
   分類：{Topic.Severity}
   ```

2. 選取 **Save**。

### 工作 3.3 – 用生成式回答草擬回覆

現在由代理程式撰寫回覆。撰寫回覆需要彈性，所以這裡使用 AI，而不是固定規則。你要使用**生成式回答**（Generative answers）節點，讓代理程式在固定的主題**中**，根據你的知識撰寫文字。這個公式會把評論的詳細資料，以及回覆必須遵守的規則交給 AI。

1. 在 **分類：{Topic.Severity}** 訊息節點下方，選取 **+** > **Advanced** > **Generative answers**。
2. 在 **Input** 中，選取 **…** / **{x}** > **Formula**，然後貼上：

   ```text
   "請依照品牌語氣指南和客戶服務指南，用繁體中文為這則顧客評論草擬一則簡短的公開回覆（最多 150 字）。" &
   "分類：" & Topic.Severity & "。" &
   "通路：" & Text(Topic.Channel) & "。商品：" & Topic.ProductName & "。" &
   "評分：" & Text(Topic.Rating) & " 分（滿分 5 分）。" &
   "評論內容：" & Topic.ReviewText & "。" &
   "如果分類是「需升級處理」，只表達關心、為這次的經驗致歉，" &
   "並請顧客私下與客戶服務團隊聯絡。" &
   "不要討論原因、責任或賠償。" &
   "絕對不要承諾退款，也絕對不要做出醫療宣稱。"
   ```

3. 資料來源保持為代理程式的知識。
4. 在 **Generative answers** 節點下方，選取 **+** > **Send a message**，然後貼上：

   ```text
   ✏️ 這是草稿，張貼前請先檢查。
   ```

5. 選取 **Save**。

### 工作 3.4 – 測試四種分類

**測試方式：**

- 每項測試都開始新的測試工作階段。
- 依序送出下列每個輸入，作為你對代理程式問題的回答。

**A.** 滿意的顧客，評分 5

1. 送出：

   ```text
   幫我回覆一則顧客評論
   ```

2. 通路：

   ```text
   網路商店
   ```

3. 商品：

   ```text
   無
   ```

4. 評分：

   ```text
   5
   ```

5. 評論內容：

   ```text
   好喜歡組合優惠，超划算！
   ```

6. 健康或安全疑慮：

   ```text
   No
   ```

**預期結果：** 🟢 感謝；一則簡短、溫暖的感謝回覆

**B.** 好壞參半的評論，評分 3

1. 送出：

   ```text
   幫我回覆一則顧客評論
   ```

2. 通路：

   ```text
   門市問卷
   ```

3. 商品：

   ```text
   無
   ```

4. 評分：

   ```text
   3
   ```

5. 評論內容：

   ```text
   折扣很不錯，但結帳排隊排很久。
   ```

6. 健康或安全疑慮：

   ```text
   No
   ```

**預期結果：** 🟡 一般；表達感謝並提到會改善

**C.** 不滿意的顧客，評分 1

1. 送出：

   ```text
   幫我回覆一則顧客評論
   ```

2. 通路：

   ```text
   網路商店
   ```

3. 商品：

   ```text
   身體乳
   ```

4. 評分：

   ```text
   1
   ```

5. 評論內容：

   ```text
   身體乳的壓頭漏得到處都是。
   ```

6. 健康或安全疑慮：

   ```text
   No
   ```

**預期結果：** 🟠 優先；致歉並提供下一步；**沒有承諾退款**

**D.** 健康與安全問題，評分 4

1. 送出：

   ```text
   幫我回覆一則顧客評論
   ```

2. 通路：

   ```text
   網路商店
   ```

3. 商品：

   ```text
   HydraGlow Face Cream 50ml
   ```

4. 評分：

   ```text
   4
   ```

5. 評論內容：

   ```text
   質地很好，但用了之後皮膚發紅又發癢。
   ```

6. 健康或安全疑慮：

   ```text
   Yes
   ```

**預期結果：** 🔴 需升級處理，即使評分是 4 分；一則表達關心的回覆，請顧客私下聯絡；**不**討論原因

---

## 工作 4 – 將健康與安全問題升級處理

需要升級處理的評論，必須盡快送到品質團隊手上。在這項工作中，你要讓代理程式**只**針對 🔴 需升級處理的評論寄送升級通知郵件。在本實驗中，郵件會寄**給你自己**，代替品質團隊的信箱。你會先把 Outlook 工具加入代理程式，再從**處理顧客評論**主題中呼叫它。

如果無法使用 Office 365 Outlook 連接器，或連接器顯示 **Blocked by policy**，請略過這項工作，並前往本頁最後的[附註：如果無法使用 Outlook 連接器](#附註如果無法使用-outlook-連接器)。

### 工作 4.1 – 將 Outlook 工具加入代理程式

1. 前往 **Tools** 索引標籤，選取 **Add a tool**。
2. 選取 **Connector**，搜尋 `Office 365 Outlook`，然後選取 **Send an email (V2)**。
3. 設定連線：
   - 選取 **Connection**。如果顯示 **Not connected**，請選取 **Create new connection**。
   - **Display name：**

     ```text
     Lab Review Response Helper
     ```

   - 選取 **Create**，並用你的 Microsoft 365 公司帳戶登入。
4. 選取 **Add and configure**。
5. 在工具頁面的 **Additional details** 下，把 **When this will be used** 設為 **Only when referenced by topics or agents**。

   > 這樣代理程式只會從**處理顧客評論**主題寄送郵件，絕對不會自行決定寄信。

6. **Inputs** 保持不變，選取 **Save**。

> [!NOTE]
> 不要在工具頁面上填寫輸入。工具的輸入無法使用主題的變數，例如 `Topic.ReviewText`。在工作 4.2 中，你會先把郵件文字放進**全域**變數，讓工具可以使用。

### 工作 4.2 – 準備郵件文字

工具的輸入只能使用**全域**變數和**系統**變數，不能使用主題變數。在這項工作中，你要用主題的變數組成郵件主旨和內文，並儲存在兩個全域變數中。

這些節點要新增在工作 3.1 分類條件的**分支 1**（`SafetyConcern` is equal to `true`）中，也就是它的 **Severity** 節點下方。只有健康與安全相關的評論會進入分支 1，所以代理程式絕對不會為了其他評論寄信給品質團隊。郵件也會在代理程式草擬回覆**之前**寄出，所以升級處理不會受到 AI 步驟影響。

1. 前往 **Topics** 索引標籤，開啟**處理顧客評論**主題。
2. 在分類條件的**分支 1** 中，於 **Severity** 節點下方選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新變數，並命名為：

     ```text
     EmailSubject
     ```

   - 選取變數以開啟 **Variable properties**，在 **Usage** 下選取 **Global (any topic can access)**。名稱會變成 `Global.EmailSubject`。
   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     "[健康安全升級] 顧客評論 – " & Topic.ProductName
     ```

3. 在 **EmailSubject** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新變數，並命名為：

     ```text
     EmailBody
     ```

   - 選取變數以開啟 **Variable properties**，在 **Usage** 下選取 **Global (any topic can access)**。名稱會變成 `Global.EmailBody`。
   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     "<p><b>顧客評論的健康與安全問題升級通知</b></p>" &
     "<p><b>通路：</b>" & Text(Topic.Channel) & "<br>" &
     "<b>商品：</b>" & Topic.ProductName & "<br>" &
     "<b>評分：</b>" & Text(Topic.Rating) & " / 5<br>" &
     "<b>評論內容：</b>" & Topic.ReviewText & "<br>" &
     "<b>回報人：</b>" & System.User.DisplayName & "</p>" &
     "<p>請依照客戶服務指南中的健康與安全問題升級處理流程進行處理。</p>"
     ```

4. 選取 **Save**。

### 工作 4.3 – 從主題寄送郵件

1. 在 **EmailBody** 節點下方，選取 **+** > **Add a tool**，然後選取 **Send an email (V2)**。
2. 在 **Inputs** 區段中，依下列方式設定每個輸入。不要自己輸入變數名稱，請從清單中**選取**。
   - **To**
     - **Fill using：** 選取 **Custom value**。
     - **Value：** 選取值方塊，前往 **System** 索引標籤，然後選取 **User.Email**。
   - **Subject**
     - **Fill using：** 選取 **Custom value**。
     - **Value：** 選取值方塊，前往 **Custom** 索引標籤，然後選取 **Global.EmailSubject**。
   - **Body**
     - **Fill using：** 選取 **Custom value**。
     - **Value：** 選取值方塊，前往 **Custom** 索引標籤，然後選取 **Global.EmailBody**。

3. 在 **Send an email (V2)** 節點下方，選取 **+** > **Send a message**，然後貼上：

   ```text
   📧 已寄送升級通知郵件給品質團隊。
   ```

4. 選取 **Save**。

### 工作 4.4 – 測試郵件

1. 開始新的測試工作階段，並依序送出下列每個輸入。
   - 送出：

     ```text
     幫我回覆一則顧客評論
     ```

   - 通路：

     ```text
     網路商店
     ```

   - 商品：

     ```text
     HydraGlow Face Cream 50ml
     ```

   - 評分：

     ```text
     4
     ```

   - 評論內容：

     ```text
     質地很好，但用了之後皮膚發紅又發癢。
     ```

   - 健康或安全疑慮：

     ```text
     Yes
     ```

   如果測試面板要求你連線或允許存取，請選取 **Allow**。

2. 檢查你的收件匣。

   **預期結果：** 你會收到一封主旨為 `[健康安全升級] 顧客評論 – HydraGlow Face Cream 50ml` 的郵件。工作 3.4 的測試 A、B 和 C 不會寄出郵件。

---

## 工作 5 – 測試完整流程

在這項工作中，你要從頭到尾測試整個代理程式：回答問題、收集評論資訊、分類、升級處理，以及草擬回覆。

1. 開始新的測試工作階段，然後送出：

   ```text
   顧客要求賠償時，我們應該怎麼回覆？
   ```

   **預期結果：** 代理程式根據客戶服務指南回答。

2. 送出：

   ```text
   幫我回覆一則顧客評論
   ```

   **預期結果：** 代理程式啟動**處理顧客評論**主題。

3. 回答代理程式的問題。依序送出下列每個輸入。
   - 通路：

     ```text
     網路商店
     ```

   - 商品：

     ```text
     HydraGlow Face Cream 50ml
     ```

   - 評分：

     ```text
     4
     ```

   - 評論內容：

     ```text
     質地很好，但用了之後皮膚發紅又發癢。
     ```

   - 健康或安全疑慮：

     ```text
     Yes
     ```

   **預期結果：** 代理程式寄出升級通知郵件，把評論分類為 🔴 需升級處理，然後草擬一則表達關心的回覆。

---

## 總結

你建立了 **Review Response Helper**：

- ✅ 根據品牌指南和客戶服務指南回答問題
- ✅ 用選擇題、文字、**數字**和 **Yes/No** 問題收集評論資訊
- ✅ 用規則為評論分類（安全優先），並在固定的主題中**用 AI 草擬回覆**
- ✅ 只針對健康與安全問題寄送升級通知郵件

### 重點

最好的代理程式會結合**規則**和 **AI**：

- 對每次都必須相同的決策使用**規則**（條件），例如升級處理安全問題。
- 對需要彈性的工作使用 **AI**（生成式回答），例如撰寫回覆。
- 高風險的判斷和要公開發布的內容，都由**人**來掌控。

---

## 附註：如果無法使用 Outlook 連接器

如果無法使用 Office 365 Outlook 連接器，或連接器顯示 **Blocked by policy**，請略過工作 4，改為顯示升級處理訊息。

1. 開啟**處理顧客評論**主題。
2. 在工作 3.1 分類條件的**分支 1**（`SafetyConcern` is equal to `true`）中，於 **Severity** 節點下方選取 **+** > **Send a message**，然後貼上：

   ```text
   🔴 健康與安全問題：請立即將這則評論回報給品質團隊。
   ```

3. 選取 **Save**。
4. 開始新的測試工作階段，執行工作 3.4 的測試 **D**。

   **預期結果：** 代理程式顯示健康與安全訊息，把評論分類為 🔴 需升級處理，並草擬一則表達關心的回覆。

> 即使沒有連接器，分類和回覆草稿仍然可以正常運作。唯一的差別是，由人把評論回報給品質團隊，而不是由代理程式寄送郵件。
