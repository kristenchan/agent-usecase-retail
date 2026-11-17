# 使用案例 1 – Ask HR（第 2 堂）

[English](../en/index.md) | 繁體中文

## 關於本實驗

你在 **Tailwind Traders** 工作，這是一家模擬的健康與美容零售商。

> Tailwind Traders 是虛構的公司。請參閱[公司簡介](../../../../README.md)。

在本實驗中，你會在 Copilot Studio 中建立 **Ask HR**，並把它擴充成**請假預檢**（Leave Pre-check）。員工送出請假申請之前，代理程式會詢問相關資料、依請假規定檢查，並用電子郵件寄出結果。請假預檢**不會送出或核准請假**，它的目的是幫助員工一次就把申請填對。

- **工具：** Microsoft Copilot Studio（standard harness）
- **預估時間：** 60–75 分鐘
- **難度：** 入門到中級，不需要寫程式（公式直接複製貼上）

你將學會：

- 建立一個**主題**，用選擇題和**日期**問題提問，並把回答存在變數中
- 用公式**計算日期**，例如請假天數和提前申請的天數
- 檢查**包含多項規則的條件**（And / Or、分支順序），讓同樣的申請每次都得到同樣的答案
- 透過**連接器**（Office 365 Outlook）用電子郵件寄出結果

---

## 開始之前

上課前請逐項確認。點選檔案名稱即可下載，請把檔案存在容易找到的地方，例如桌面。

> 所有檔案皆為**虛構內容**，僅供訓練使用。

| ✔ | 需要準備的項目 | 說明 |
|---|---|---|
| ☐ | 可使用 **Copilot Studio** | 前往 [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com)，確認可以開啟 |
| ☐ | 一個可以建置代理程式的**環境**（environment） | 使用右上角的 **environment** 選擇器 |
| ☐ | 一個 **Outlook** 信箱（僅工作 4 需要） | 確認可以用同一個帳戶開啟 Outlook |
| ☐ | 檔案 1（PDF）：[`Employee-Leave-and-Benefits-Guide.pdf`](Employee-Leave-and-Benefits-Guide.pdf) | 代理程式回答問題所依據的人資指南（員工請假與福利指南）：假別、如何申請、Mega Sale 請假限制、加班與福利。這和第 1 堂使用的是同一個檔案，如果你已經有了，就不需要再下載 |

---

## 情境

Tailwind Traders 的人資每天大約有 **20% 的時間**在回答員工的請假問題。答案都寫在**員工請假與福利指南**裡，但這份文件很長，而且規定會因職務和假別而不同。

就算知道規定，員工申請時還是常常出錯。在公司的人資系統 **MyHR** 中送出的請假申請，大約每 **4 件就有 1 件**被退回，原因是資料不完整、提前申請時間不足，或碰到 Mega Sale 請假限制，主管也得花時間解釋原因。

每年 12 月 1 日到 7 日的 **Mega Sale Week**（年度大促銷週）前會更忙。11 月 28 日到 12 月 7 日限制請假，大促銷前，人資一再看到同樣的錯誤：

- 員工在 **Mega Sale 請假限制期**（11 月 28 日至 12 月 7 日）申請特休，卻沒有取得額外核准。
- 請 3 天以上的假，卻沒有提前 **2 週**申請。
- 不知道申請要由**誰**核准。

為了減少被退回的申請，Tailwind Traders 希望有一個代理程式，能回答請假問題，並在員工到 MyHR 送出申請前先做預檢。

**你的任務：**

1. 在 Copilot Studio 中建立 **Ask HR**，讓它依據指南回答請假與福利問題。
2. 建立**請假預檢**主題，詢問職務、假別和日期。
3. 計算請假天數和提前申請的天數，並檢查規則。
4. 用電子郵件寄出預檢結果，讓員工可以轉寄給主管。

---

## 工作 1 – 建立代理程式

在這項工作中，你要在 Copilot Studio 中建立 Ask HR，並提供人資指南讓它依據回答。

### 工作 1.1 – 建立代理程式

1. 前往 Microsoft Copilot Studio [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com) 並登入。
2. 確認已選取正確的**環境**（environment）。
3. 在 **Home** 頁面上選取 **Agent**，或在左側導覽中選取 **Agents**。
4. 在 **New agent** 旁選取向下箭號（**⌄**），然後選取 **Agent**（**Standard**）。

   > [!NOTE]
   > 我們使用的是支援主題（topics）的 **standard harness**。

5. 為代理程式命名，並檢查它的設定：

   - **Name**

     ```text
     Ask HR
     ```

   - **Agent settings (Optional)**
     - **Language：** English
     - **Solution：** 保留預設的方案。
     - **Schema name**

       ```text
       lab_askhr
       ```

   > [!NOTE]
   > **Language** 請維持 **English**。我們會在 **Instructions** 中要求代理程式一律使用繁體中文回覆。

6. 選取 **Create**。
7. 在 **Overview** 索引標籤的 **Details** 區段中，選取 **Edit**，然後填寫下列欄位：

   - **Description**

     ```text
     回答員工的請假與福利問題，並依請假規定預檢請假申請。
     ```

   - **Instructions**

     ```text
     # 角色
     你是 Ask HR，負責解答請假與福利問題的小幫手。

     # 你負責的事
     - 回答以下問題：假別、如何申請請假、Mega Sale 請假限制期、加班與補休、
       特休遞延，以及員工福利。
     - 只依據「員工請假與福利指南」回答。每次都要註明答案出自哪個章節。
     - 當使用者想要檢查請假申請，或想在特定日期請假時，
       請使用「請假預檢」主題。

     # 你不做的事
     - 你看不到任何人的假別餘額或請假紀錄。如果問到個人餘額，
       請使用者到 MyHR 查看。
     - 絕對不要談論其他員工的請假、健康或個人狀況。
     - 你不會送出或核准請假。員工需自行到 MyHR 申請。

     # 回覆方式
     - 一律使用繁體中文回覆。
     ```

8. 選取 **Save**。

### 工作 1.2 – 加入知識並測試

1. 選取 **Knowledge** 索引標籤。
2. 選取 **Add knowledge** > **Upload file**，上傳 `Employee-Leave-and-Benefits-Guide.pdf`（**開始之前**的檔案 1），然後選取 **Add to agent**。
3. 當檔案狀態變成 **Ready** 後，把下列問題複製到 **Test your agent** 面板中送出：

   ```text
   婚假可以請幾天？
   ```

   **預期結果：** 代理程式依據指南回答 **5 天**（「2.4 婚假」）。

---

## 工作 2 – 建立「請假預檢」主題

在這項工作中，你要建立一個主題，向員工問四個問題，並把回答存起來。

### 工作 2.1 – 建立主題

1. 選取 **Topics** 索引標籤，然後選取 **Add a topic** > **From blank**。
2. 選取 **Details**，填寫主題的詳細資料：

   - **Name**

     ```text
     請假預檢
     ```

   - **Description：**

     ```text
     在員工到 MyHR 送出請假申請前，依請假規定檢查申請內容。
     ```

   - **Model description：**

     ```text
     當員工想要預檢或檢查請假申請、規劃特定日期的請假，
     或詢問某些日期能不能請假時，使用這個主題。
     ```

3. 加入 **Send a message** 節點。

   在 **Trigger** 節點下方，選取 **+** > **Send a message**，然後貼上：

   ```text
   在你到 MyHR 送出申請之前，我們先依規定檢查一下你的請假申請。我會問你 4 個簡單的問題。
   ```

4. 選取 **Save**。
5. 確認員工要求檢查請假時，主題會啟動。把下列文字複製到 **Test your agent** 面板中送出：

   ```text
   我想預檢我的請假申請
   ```

   **預期結果：** 代理程式啟動**請假預檢**主題，並顯示你的開場訊息。你不需要撰寫任何觸發片語：代理程式會根據 **Model description** 決定何時啟動這個主題。

### 工作 2.2 – 提出四個問題

加入 4 個問題節點。

1. 加入第一個問題，詢問職務。在 **Send a message** 節點下方，選取 **+** > **Ask a question**。

   > [!NOTE]
   > 如果 Copilot Studio 自動在選擇題下方加入 **Condition** 節點，請將它**刪除**。你會在工作 3 中建立自己的條件。

   - 問題文字：

     ```text
     請問你的職務是什麼？
     ```

   - **Identify：** **Multiple choice options**。加入下列選項：

     ```text
     門市人員
     ```

     ```text
     總部人員
     ```

   - 把變數重新命名為：

     ```text
     Role
     ```

2. 加入第二個問題，詢問假別。在 **請問你的職務是什麼？** 問題節點下方，選取 **+** > **Ask a question**。

   > [!NOTE]
   > 如果 Copilot Studio 自動在選擇題下方加入 **Condition** 節點，請將它**刪除**。你會在工作 3 中建立自己的條件。

   - 問題文字：

     ```text
     請問你要請哪一種假？
     ```

   - **Identify：** **Multiple choice options**。加入下列選項：

     ```text
     特休
     ```

     ```text
     事假
     ```

     ```text
     病假
     ```

     ```text
     婚假
     ```

     ```text
     喪假
     ```

     ```text
     其他
     ```

   - 把變數重新命名為：

     ```text
     LeaveType
     ```

3. 加入第三個問題，詢問請假的第一天。在 **請問你要請哪一種假？** 問題節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     請問請假的第一天是哪一天？
     ```

   - **Identify：** **Date**
   - 把變數重新命名為：

     ```text
     StartDate
     ```

4. 加入第四個問題，詢問請假的最後一天。在 **請問請假的第一天是哪一天？** 問題節點下方，選取 **+** > **Ask a question**。
   - 問題文字：

     ```text
     請問請假的最後一天是哪一天？
     ```

   - **Identify：** **Date**
   - 把變數重新命名為：

     ```text
     EndDate
     ```

> [!TIP]
> 選擇 **Date** 時，代理程式會把回答存成日期，之後就能用它來計算。測試時，請以 `d mmm yyyy` 格式輸入**日、月、年**（例如 `2 Dec 2026`），月份用英文縮寫。請勿輸入「下週一」「明天」這類字詞。

5. 選取 **Save**，然後把下列文字複製到 **Test your agent** 面板中送出：

   ```text
   我想預檢我的請假申請
   ```

   **預期結果：** 代理程式依序問完四個問題，而且你可以看到 `StartDate` 和 `EndDate` 已存成日期。

---

## 工作 3 – 計算並檢查規則

在這項工作中，你要計算請假天數和提前申請的天數、檢查請假規定，然後顯示結果。

### 工作 3.1 – 計算日期

代理程式要先算出幾個數字，才能檢查規則。請假規定取決於員工要請**幾天**，以及**提前多久**申請。Mega Sale Week 還有一項特別規定：**11 月 28 日到 12 月 7 日**限制請假，所以代理程式需要這兩個日期，才能檢查請假是否落在這段期間。

在這項工作中，你要加入五個 **Set a variable value** 節點，根據員工輸入的日期計算這些值。

1. 在 **請問請假的最後一天是哪一天？** 問題節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **計算內容：** Mega Sale 請假限制期的開始日：與請假第一天同一年的 11 月 28 日。
   - **Variable：** 建立新變數，命名為：

     ```text
     RestrictionStart
     ```

   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     Date(Year(Topic.StartDate), 11, 28)
     ```

2. 在 **RestrictionStart** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **計算內容：** Mega Sale 請假限制期的結束日：與請假第一天同一年的 12 月 7 日。
   - **Variable：** 建立新變數，命名為：

     ```text
     RestrictionEnd
     ```

   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     Date(Year(Topic.StartDate), 12, 7)
     ```

3. 在 **RestrictionEnd** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **計算內容：** 請假的日曆天數。
   - **Variable：** 建立新變數，命名為：

     ```text
     LeaveDays
     ```

   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     DateDiff(Topic.StartDate, Topic.EndDate, TimeUnit.Days) + 1
     ```

4. 在 **LeaveDays** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **計算內容：** 從今天到請假第一天之間的天數（也就是提前申請的天數）。
   - **Variable：** 建立新變數，命名為：

     ```text
     NoticeDays
     ```

   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     DateDiff(Today(), Topic.StartDate, TimeUnit.Days)
     ```

5. 在 **NoticeDays** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **計算內容：** 只要請假期間有任何一天落在限制期內，就是 **true**。
   - **Variable：** 建立新變數，命名為：

     ```text
     InRestriction
     ```

   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     Topic.StartDate <= Topic.RestrictionEnd && Topic.EndDate >= Topic.RestrictionStart
     ```

> [!TIP]
> Mega Sale 請假限制期是**每年 11 月 28 日至 12 月 7 日**。`Year(Topic.StartDate)` 會從使用者輸入的日期取出年份，所以代理程式不需要修改，每年都能使用。如果大促銷的日期改變，只要更新這兩個變數即可。

### 工作 3.2 – 檢查日期是否合理

如果員工輸入的最後一天早於第一天，`LeaveDays` 就會小於 1。在這項工作中，你要讓主題停止，並請員工檢查日期。

1. 在 **InRestriction** 節點下方，選取 **+** > **Add a condition**。
2. 在 **Condition** 分支中：
   - 設定條件：`LeaveDays` **is less than** `1`
   - 在 **Condition** 分支下方，選取 **+** > **Send a message**，然後貼上：

     ```text
     最後一天早於第一天。請重新開始，並檢查你輸入的日期。
     ```

   - 在訊息節點下方，選取 **+** > **Topic management** > **End current topic**。

3. **All other conditions** 保持空白。對話會在條件下方繼續進行。

### 工作 3.3 – 檢查限制規定

接下來，代理程式要檢查 Mega Sale 請假限制。結果取決於假別、日期是否落在限制期內，以及員工的職務。你要把結果以文字形式存在新變數 `RestrictionResult` 中。

請在條件**之前**建立 `RestrictionResult`。各分支位於同一層，所以在某個分支內建立的變數，無法在其他分支中選取。

1. 在工作 3.2 的日期檢查條件下方（各分支會合的地方），選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新變數，命名為：

     ```text
     RestrictionResult
     ```

   - **To value：** 選取 **Formula**，然後貼上下列內容。這會先把變數設為空白文字，之後再由各分支填入。

     ```text
     ""
     ```

2. 在 **RestrictionResult** 節點下方，選取 **+** > **Add a condition**。
3. 在新的 **Condition** 節點上，選取 **+**（Add condition branch）兩次。
   - 現在你有**三個**條件分支，加上 **All other conditions**。
   - 分支會**由左到右**檢查，並使用第一個成立的分支。

4. 在**分支 1** 中：
   - 設定條件：`LeaveType` is equal to `病假` **Or** `LeaveType` is equal to `喪假`
   - 在分支 1 下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `RestrictionResult`。
   - **To value：** 貼上：

     ```text
     ✅ 病假與喪假不受 Mega Sale 請假限制影響。連續請病假 2 天以上，需附醫師證明。
     ```

5. 在**分支 2** 中：
   - 設定條件：`InRestriction` is equal to `true` **And** `Role` is equal to `門市人員`
   - 在分支 2 下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `RestrictionResult`。
   - **To value：** 貼上：

     ```text
     ⚠️ 你的請假日期落在 Mega Sale 請假限制期（11/28 – 12/7）內。門市人員的特休與事假原則上不予核准，需經區域經理核准。
     ```

6. 在**分支 3** 中：
   - 設定條件：`InRestriction` is equal to `true` **And** `Role` is equal to `總部人員`
   - 在分支 3 下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `RestrictionResult`。
   - **To value：** 貼上：

     ```text
     ⚠️ 你的請假日期落在 Mega Sale 請假限制期（11/28 – 12/7）內，需經部門主管核准。
     ```

7. 在 **All other conditions** 中：
   - 選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `RestrictionResult`。
   - **To value：** 貼上：

     ```text
     ✅ 你的請假日期不在 Mega Sale 請假限制期內。
     ```

### 工作 3.4 – 檢查提前申請期

請假 3 天以上需要提前 2 週申請。在這項工作中，代理程式會檢查提前申請的天數，並把結果存在新變數 `NoticeResult` 中。和工作 3.3 一樣，請在條件**之前**建立變數，讓兩個分支都能使用。

1. 在工作 3.3 的限制條件下方（各分支會合的地方），選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新變數，命名為：

     ```text
     NoticeResult
     ```

   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     ""
     ```

2. 在 **NoticeResult** 節點下方，選取 **+** > **Add a condition**。
3. 在 **Condition** 分支中：
   - 設定條件：`LeaveDays` is greater than or equal to `3` **And** `NoticeDays` is greater than or equal to `0` **And** `NoticeDays` is less than `14`
   - 在 **Condition** 分支下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `NoticeResult`。
   - **To value：** 貼上：

     ```text
     ⚠️ 請假 3 天以上需要提前 2 週申請。申請前請先和主管討論。
     ```

4. 在 **All other conditions** 中：
   - 選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 選取 `NoticeResult`。
   - **To value：** 貼上：

     ```text
     ✅ 提前申請時間符合規定。
     ```

### 工作 3.5 – 顯示結果

最後，代理程式用一則訊息向員工顯示所有結果。

1. 在工作 3.4 的提前申請條件下方（各分支會合的地方），選取 **+** > **Send a message**，然後貼上下列文字。使用 **{x}** 插入變數：

   ```text
   📋 請假預檢結果

   職務：{Topic.Role}
   假別：{Topic.LeaveType}
   日期：{Topic.StartDate} 至 {Topic.EndDate}（{Topic.LeaveDays} 天）

   {Topic.RestrictionResult}
   {Topic.NoticeResult}

   下一步：請到 MyHR 送出申請。
   ```

2. 選取 **Save**。

### 工作 3.6 – 測試規則

**測試方式：**

- 每個測試都要開始新的測試工作階段。
- 依序送出下列每個輸入，作為你對代理程式問題的回答。
- 輸入日期時，請包含**日、月、年**（例如 `2 Dec 2026`）。

**A.** 門市人員，大促銷期間請 3 天特休

1. 送出：

   ```text
   我想預檢我的請假申請
   ```

2. 職務：

   ```text
   門市人員
   ```

3. 假別：

   ```text
   特休
   ```

4. 第一天：

   ```text
   3 Dec 2026
   ```

   > [!NOTE]
   > 日期請以「日 月 年」格式輸入，月份用英文縮寫，例如 `3 Dec 2026`；請勿輸入「下週一」「明天」這類相對日期。

5. 最後一天：

   ```text
   5 Dec 2026
   ```

**預期結果：** ⚠️ 落在限制期內，需經**區域經理**核准

**B.** 總部人員，大促銷期間請 4 天特休

1. 送出：

   ```text
   我想預檢我的請假申請
   ```

2. 職務：

   ```text
   總部人員
   ```

3. 假別：

   ```text
   特休
   ```

4. 第一天：

   ```text
   6 Dec 2026
   ```

5. 最後一天：

   ```text
   9 Dec 2026
   ```

**預期結果：** ⚠️ 落在限制期內，需經**部門主管**核准

**C.** 門市人員，大促銷期間請 2 天病假

1. 送出：

   ```text
   我想預檢我的請假申請
   ```

2. 職務：

   ```text
   門市人員
   ```

3. 假別：

   ```text
   病假
   ```

4. 第一天：

   ```text
   3 Dec 2026
   ```

5. 最後一天：

   ```text
   4 Dec 2026
   ```

**預期結果：** ✅ 不受限制影響；提醒需附醫師證明

**D.** 總部人員，提前申請時間不足的 3 天特休

1. 送出：

   ```text
   我想預檢我的請假申請
   ```

2. 職務：

   ```text
   總部人員
   ```

3. 假別：

   ```text
   特休
   ```

4. 第一天：

   ```text
   23 Nov 2026
   ```

5. 最後一天：

   ```text
   25 Nov 2026
   ```

**預期結果：** ✅ 不在限制期內；⚠️ 提前申請時間不足的警告

**E.** 門市人員，最後一天早於第一天

1. 送出：

   ```text
   我想預檢我的請假申請
   ```

2. 職務：

   ```text
   門市人員
   ```

3. 假別：

   ```text
   特休
   ```

4. 第一天：

   ```text
   4 Dec 2026
   ```

5. 最後一天：

   ```text
   2 Dec 2026
   ```

**預期結果：** 請你檢查輸入的日期

> [!NOTE]
> 測試日期是以 **2026 年 11 月 18 日**進行本實驗來設定的。提前申請的檢查是從**今天**開始計算天數，所以在其他日期測試時，⚠️ 提前申請的結果可能會不同。

---

## 工作 4 – 用電子郵件寄出預檢結果

在這項工作中，你要讓代理程式把結果寄**給你自己**，讓你可以轉寄給主管。你會先把 Outlook 工具加到代理程式中，再從**請假預檢**主題呼叫它。

如果無法使用 Office 365 Outlook 連接器，或顯示 **Blocked by policy**，請略過這項工作，前往本頁最後的[附註：如果無法使用 Outlook 連接器](#附註如果無法使用-outlook-連接器)。

### 工作 4.1 – 把 Outlook 工具加到代理程式

1. 前往 **Tools** 索引標籤，選取 **Add a tool**。
2. 選取 **Connector**，搜尋 `Office 365 Outlook`，然後選取 **Send an email (V2)**。
3. 設定連線：
   - 選取 **Connection**。如果顯示 **Not connected**，請選取 **Create new connection**。
   - **Display name：**

     ```text
     Lab Ask HR
     ```

   - 選取 **Create**，並用你的 Microsoft 365 公司帳戶登入。
4. 選取 **Add and configure**。
5. 在工具頁面的 **Additional details** 下，將 **When this will be used** 設為 **Only when referenced by topics or agents**。

   > 這樣一來，代理程式只會從**請假預檢**主題寄出電子郵件，絕不會自行決定寄信。

6. **Inputs** 保持原樣，然後選取 **Save**。

> [!NOTE]
> 請不要在工具頁面填寫輸入。工具的輸入無法使用主題的變數，例如 `Topic.StartDate`。在工作 4.2 中，你會先把電子郵件內容放進**全域**變數，讓工具可以使用。

### 工作 4.2 – 準備電子郵件內容

工具的輸入只能使用**全域**變數和**系統**變數，不能使用主題變數。在這項工作中，你要用主題的變數組出電子郵件的主旨和內文，並存在兩個全域變數中。

1. 前往 **Topics** 索引標籤，開啟**請假預檢**主題。
2. 在工作 3.5 的結果訊息節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新變數，命名為：

     ```text
     EmailSubject
     ```

   - 選取變數以開啟 **Variable properties**，然後在 **Usage** 下選取 **Global (any topic can access)**。名稱會變成 `Global.EmailSubject`。
   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     "[請假預檢] " & System.User.DisplayName & " – " & Text(Topic.StartDate, "dd mmm") & " 至 " & Text(Topic.EndDate, "dd mmm")
     ```

3. 在 **EmailSubject** 節點下方，選取 **+** > **Variable management** > **Set a variable value**。
   - **Variable：** 建立新變數，命名為：

     ```text
     EmailBody
     ```

   - 選取變數以開啟 **Variable properties**，然後在 **Usage** 下選取 **Global (any topic can access)**。名稱會變成 `Global.EmailBody`。
   - **To value：** 選取 **Formula**，然後貼上：

     ```text
     "<p><b>請假預檢結果</b></p>" &
     "<p><b>員工：</b>" & System.User.DisplayName & "<br>" &
     "<b>職務：</b>" & Text(Topic.Role) & "<br>" &
     "<b>假別：</b>" & Text(Topic.LeaveType) & "<br>" &
     "<b>日期：</b>" & Text(Topic.StartDate, "dd mmm yyyy") & " 至 " & Text(Topic.EndDate, "dd mmm yyyy") &
     "（" & Text(Topic.LeaveDays) & " 天）</p>" &
     "<p>" & Topic.RestrictionResult & "<br>" & Topic.NoticeResult & "</p>" &
     "<p>這只是預檢結果，請到 MyHR 送出申請。</p>"
     ```

4. 選取 **Save**。

### 工作 4.3 – 從主題寄出電子郵件

1. 在 **EmailBody** 節點下方，選取 **+** > **Add a tool**，然後選取 **Send an email (V2)**。
2. 在 **Inputs** 區段中，依下列方式設定每個輸入。不要直接輸入變數名稱，請從清單中**選取**。
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
   📧 我已經把這份結果寄到你的信箱，你可以轉寄給主管。
   ```

4. 選取 **Save**。

### 工作 4.4 – 測試電子郵件

1. 開始新的測試工作階段，依序送出下列每個輸入。
   - 送出：

     ```text
     我想預檢我的請假申請
     ```

   - 職務：

     ```text
     門市人員
     ```

   - 假別：

     ```text
     特休
     ```

   - 第一天：

     ```text
     3 Dec 2026
     ```

   - 最後一天：

     ```text
     5 Dec 2026
     ```

   如果測試面板要求你連線或允許存取，請選取 **Allow**。

2. 檢查你的收件匣。

   **預期結果：** 你會收到一封主旨為 `[請假預檢] <你的姓名> – 03 Dec 至 05 Dec` 的電子郵件。

---

## 工作 5 – 測試完整流程

在這項工作中，你要一起測試全部四個步驟：詢問（Ask）、收集（Collect）、判斷（Decide）和寄送（Send）。

1. 開始新的測試工作階段，然後送出：

   ```text
   促銷商品可以用員工折扣嗎？
   ```

   **預期結果：** 代理程式依據知識回答：不行，以較優惠的折扣為準（「7.1 員工折扣」）（**詢問**）。

2. 送出：

   ```text
   12 月 3 日到 5 日可以請假嗎？
   ```

   **預期結果：** 代理程式啟動**請假預檢**主題（**收集**）。

3. 回答代理程式的問題。依序送出下列每個輸入。
   - 職務：

     ```text
     門市人員
     ```

   - 假別：

     ```text
     特休
     ```

   - 第一天：

     ```text
     3 Dec 2026
     ```

   - 最後一天：

     ```text
     5 Dec 2026
     ```

   **預期結果：** 代理程式計算並檢查規則（**判斷**），顯示 ⚠️ 落在限制期內、需經區域經理核准，然後用電子郵件寄出結果（**寄送**）。

---

## 總結

你在 **Ask HR** 中建立了**請假預檢**：

- ✅ **詢問：** 依據員工指南回答問題
- ✅ **收集：** 選擇題和**日期**問題
- ✅ **判斷：** 日期計算，以及包含**多項規則**的條件（And / Or、分支順序）
- ✅ **寄送：** 員工可以轉寄給主管的電子郵件

### 重點

- 像「請假 3 天以上需要提前 2 週申請」這類業務規則，應該**每次都給出相同的答案**。
- 把規則寫成**條件**（而不只是寫在指示中），結果就能預測，也容易檢查。

---

## 附註：如果無法使用 Outlook 連接器

如果無法使用 Office 365 Outlook 連接器，或顯示 **Blocked by policy**，請略過工作 4，改在結果訊息中加一行文字。

1. 開啟**請假預檢**主題。
2. 在工作 3.5 的結果訊息最後，新增一行，然後貼上：

   ```text
   請複製這份結果，並分享給你的主管。
   ```

3. 選取 **Save**。

> 你的預檢功能仍然可以正常運作。員工改為複製結果並分享給主管，而不是轉寄電子郵件。
