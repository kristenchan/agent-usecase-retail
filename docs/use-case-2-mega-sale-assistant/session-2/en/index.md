# Use Case 2 – Mega Sale Assistant (Session 2)

## About this lab

You're working at **Tailwind Traders**, a simulated health and beauty retailer.

> Tailwind Traders is a fictitious company. Read the [company introduction](../../../../README.md).

In this lab, you build the **Mega Sale Assistant** in **Copilot Studio** and extend it into the **Store Issue Reporter**: an agent that still answers Mega Sale questions, and can now collect store issue reports, flag urgent ones and email a confirmation. Agent Builder is great for agents that answer and write; when an agent must follow a fixed process every time, you use the Copilot Studio **standard harness**, where you design the conversation step by step with **topics**.

- **Tool:** Microsoft Copilot Studio (standard harness)
- **Estimated time:** 60–75 minutes
- **Level:** Beginner, no code (a few copy-and-paste formulas)

You'll learn how to:

- Create an agent in Copilot Studio and add documents as knowledge
- Build a **topic** that asks questions in a fixed order and saves the answers in **variables**
- Use a **condition** and simple formulas to flag urgent issues and create a report number
- Add a **connector tool** that sends a confirmation email

---

## Before you start

Check each item before the lab. Select a file name to download it, and save the files somewhere easy to find, such as your desktop.

> All files are **fictional** and created for training only.

| ✔ | What you need | Details |
|---|---|---|
| ☐ | A work account with access to **Microsoft Copilot Studio** | Go to [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com) and sign in with your work account |
| ☐ | An **environment** you can build in | At the top right, select the **environment** picker. Choose your **developer environment** or the environment provided for this class |
| ☐ | An **Outlook** mailbox (for Task 4 only) | You can open Outlook with the same account. If the Office 365 Outlook connector is blocked, follow [Note: If the Outlook connector isn't available](#note-if-the-outlook-connector-isnt-available). You'll still finish the lab with a complete, working agent |
| ☐ | File 1 (PDF): [`Mega-Sale-Playbook.pdf`](Mega-Sale-Playbook.pdf) | Sale dates, discount rules, excluded items, Tailwind Rewards double points, coupon stacking rules |
| ☐ | File 2 (Word): [`Store-Execution-Guide.docx`](Store-Execution-Guide.docx) | Display deadlines, checkout procedures, price label errors, out-of-stock handling, staffing |
| ☐ | File 3 (Word): [`Mega-Sale-FAQ.docx`](Mega-Sale-FAQ.docx) | Common questions from staff and customers about the sale |

These are the same files as in Session 1. If you already have them, you don't need to download them again.

---

## Scenario

The **Store Operations** team at head office answers about **50 questions a day** from stores, by phone and Teams. Store issues, such as broken displays, missing stock and price errors, arrive in different formats and often without key details like the store code or product name, so the team has to call back and ask again.

It gets even busier during **Mega Sale Week**, held every year from 1 to 7 December. During the sale, the volume **triples**, and stores run into problems like these:

- *An item sells out.*
- *A shelf label shows the wrong price.*
- *The sale discount doesn't apply at the till.*

Urgent price problems wait in the same queue as everything else.

To get complete reports the first time and deal with urgent issues first, Tailwind Traders wants an agent that answers stores' Mega Sale questions and collects every issue report in the same format.

**Your task:**

1. Create the **Mega Sale Assistant** in Copilot Studio, so it answers Mega Sale questions from the sale documents.
2. Build the **Report a store issue** topic, which asks for the store code, issue type, product and details.
3. Flag issues that affect the price customers pay as urgent, and create a report number.
4. Email a confirmation of the report, so the Mega Sale project team can follow it up.

---

## Task 1 – Create the agent

In this task, you create the Mega Sale Assistant in Copilot Studio and give it the sale documents to answer from.

### Task 1.1 – Create the agent

1. Go to Microsoft Copilot Studio at [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com) and sign in.
2. Confirm that the correct **environment** is selected.
3. On the **Home** page, select **Agent**. Alternatively, select **Agents** in the left navigation.
4. Next to **New agent**, select the down arrow (**⌄**), and then select **Agent** (**Standard**).

   > [!NOTE]
   > We're using the **standard harness**, which supports topics.

5. Name your agent and check its settings:

   - **Name**

     ```text
     Mega Sale Assistant
     ```

   - **Agent settings (Optional)**
     - **Language:** English
     - **Solution:** Keep the default solution.
     - **Schema name**

       ```text
       lab_megasale
       ```

6. Select **Create**.
7. On the **Overview** tab, in the **Details** section, select **Edit**, and fill in the fields below:

   - **Description**

     ```text
     Answers questions about Mega Sale Week and helps stores report issues during the sale.
     ```

   - **Instructions**

     ```text
     # Role
     You are the Mega Sale Assistant. You help store and head office teams
     with Mega Sale Week.

     # What you do
     - Answer questions about sale dates, discount rules, excluded items,
       Tailwind Rewards points, coupons, store preparation, checkout issues,
       price labels, out-of-stock items and staffing, using only the
       knowledge provided. Always mention the source document.
     - When a user wants to report a problem in a store, use the
       "Report a store issue" topic.

     # How you answer
     - Keep answers short and friendly. Use numbered steps for procedures.

     # What you don't do
     - If the knowledge doesn't contain the answer, say so and suggest
       contacting the Area Manager or the Mega Sale project team. Don't guess.
     - Don't discuss personal information about employees or customers.
     - Don't comment on competitors' prices or promotions.
     ```

8. Select **Save**.

### Task 1.2 – Add knowledge and test

1. Select the **Knowledge** tab.
2. Select **Add knowledge** > **Upload file**, upload `Mega-Sale-Playbook.pdf`, `Store-Execution-Guide.docx` and `Mega-Sale-FAQ.docx` (Files 1–3 from **Before you start**), and select **Add to agent**.
3. When all three files are **Ready**, copy the following question into the **Test your agent** panel, and send it:

   ```text
   Which items are excluded from the sale discount?
   ```

   **Expected result:** The agent answers **prescription medicines, infant formula, gift cards and clearance items**, with a reference to the source.

---

## Task 2 – Build the "Report a store issue" topic

In this task, you build a topic that asks the store employee four questions and stores the answers. A **topic** is a conversation path that you design: you decide exactly which questions the agent asks and in what order, so every report contains the same information.

### Task 2.1 – Create the topic

1. Select the **Topics** tab, and then select **Add a topic** > **From blank**.
2. Select **Details**, and fill in the topic details:

   - **Name**

     ```text
     Report a store issue
     ```

   - **Description:**

     ```text
     Collects the details of a store issue during Mega Sale Week, flags urgent issues and emails a confirmation.
     ```

   - **Model description:**

     ```text
     Use this topic when a store employee wants to report a problem during
     Mega Sale Week, such as an out-of-stock item, a price label error,
     a discount not applied at checkout or a customer complaint.
     ```

3. Add a **Send a message** node.

   Under the **Trigger** node, select **+** > **Send a message**, and paste:

   ```text
   I'll help you report a store issue to the Mega Sale project team. I just need 4 quick details.
   ```

4. Select **Save**.
5. Check that the topic starts when a store employee wants to report an issue. Copy the following text into the **Test your agent** panel, and send it:

   ```text
   I want to report a store issue
   ```

   **Expected result:** The agent starts the **Report a store issue** topic and shows your opening message. You don't need to write any trigger phrases: the agent uses the **Model description** to decide when to start the topic.

### Task 2.2 – Ask four questions

Every report needs the same four details, so the Store Operations team never has to call back and ask again. Each answer is stored in a **variable**, so you can use it later in the summary and the email.

Add 4 question nodes.

1. Add the first question, about the store code. Under the **Send a message** node, select **+** > **Ask a question**.
   - Question text:

     ```text
     What is your store code? (for example, S012)
     ```

   - **Identify:** **User's entire response**
   - Rename the variable to:

     ```text
     StoreCode
     ```

2. Add the second question, about the issue type. Under the **What is your store code?** question node, select **+** > **Ask a question**.

   > [!NOTE]
   > If Copilot Studio automatically adds a **Condition** node below a multiple choice question, **delete** it. You'll build your own conditions in Task 3.

   - Question text:

     ```text
     What type of issue is it?
     ```

   - **Identify:** **Multiple choice options**. Add these options:

     ```text
     Out of stock
     ```

     ```text
     Price label error
     ```

     ```text
     Discount not applied at checkout
     ```

     ```text
     Customer complaint
     ```

     ```text
     Other
     ```

   - Rename the variable to:

     ```text
     IssueType
     ```

3. Add the third question, about the product. Under the **What type of issue is it?** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     Which product is affected? Enter the product name, or type N/A.
     ```

   - **Identify:** **User's entire response**
   - Rename the variable to:

     ```text
     ProductName
     ```

4. Add the fourth question, about the issue details. Under the **Which product is affected?** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     Please describe the issue in one or two sentences.
     ```

   - **Identify:** **User's entire response**
   - Rename the variable to:

     ```text
     IssueDetails
     ```

> [!TIP]
> **What's a variable?** A variable is a labelled box that stores the user's answer, so you can use it later in the conversation (for example, in a summary or an email). `Topic.StoreCode` means "the StoreCode box in this topic".

5. Select **Save**, then copy the following text into the **Test your agent** panel, and send it:

   ```text
   I want to report a store issue
   ```

   **Expected result:** The agent asks all four questions in order, and shows the issue types as buttons. Your answers are stored in `StoreCode`, `IssueType`, `ProductName` and `IssueDetails`.

---

## Task 3 – Add rules and a summary

In this task, you create a report number, flag urgent issues and show the store employee a summary of the report.

### Task 3.1 – Create a report number

Every report needs its own number, so stores and head office can refer to it later. In this task, you create the number from the current date and time.

1. Under the **Please describe the issue in one or two sentences.** question node, select **+** > **Variable management** > **Set a variable value**.
   - **What it calculates:** A report number based on the date and time, such as `MS-1201-093015`.
   - **Variable:** Create a new variable, and name it:

     ```text
     ReportID
     ```

   - **To value:** Select **Formula**, and paste:

     ```text
     "MS-" & Text(Now(), "mmdd-hhmmss")
     ```

### Task 3.2 – Flag urgent issues

Not every issue is equally urgent. Problems that affect the **price a customer pays** (a **Price label error** or a **Discount not applied at checkout**) need action straight away. In this task, the agent sets the priority, stores it in a new variable, `Priority`, and gives staff the first steps to follow for urgent issues.

You create `Priority` **before** the condition. Branches are on the same level, so a variable you create inside one branch can't be selected in the others.

1. Under the **ReportID** node, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     Priority
     ```

   - **To value:** Select **Formula**, and paste the following. It sets the variable to empty text for now; the branches fill it in.

     ```text
     ""
     ```

2. Under the **Priority** node, select **+** > **Add a condition**.
3. In the **Condition** branch:
   - Set the condition: `IssueType` is equal to `Price label error` **Or** `IssueType` is equal to `Discount not applied at checkout`
   - Under the **Condition** branch, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `Priority`.
   - **To value:** Paste:

     ```text
     Urgent
     ```

   - Under the **Priority** node in the **Condition** branch, select **+** > **Send a message**, and paste:

     ```text
     ⚠️ This issue is marked URGENT because it affects the price customers pay.
     While head office reviews your report, please follow the Store Execution Guide:
     • Price label error: correct the label now and honour the lower displayed price for this customer.
     • Discount not applied: check the item is eligible, re-scan it, then use a manual price override with a supervisor approval code.
     ```

4. In **All other conditions**:
   - Select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `Priority`.
   - **To value:** Paste:

     ```text
     Standard
     ```

### Task 3.3 – Show a summary

Finally, the agent shows the store employee all the details of the report in one message, so they can check it and keep the report number.

1. Below the urgent condition from Task 3.2 (where its branches join), select **+** > **Send a message**, and paste the text below. Use **{x}** to insert the variables:

   ```text
   ✅ Your report has been logged.

   Report ID: {Topic.ReportID}
   Priority: {Topic.Priority}
   Store: {Topic.StoreCode}
   Issue type: {Topic.IssueType}
   Product: {Topic.ProductName}
   Details: {Topic.IssueDetails}
   ```

2. Select **Save**.

### Task 3.4 – Test both paths

In this task, you test the urgent path and the standard path.

**How to test:**

- Start a new test session for each test.
- Send each input below in order, as your answer to the agent's question.

**A.** Price label error (urgent)

1. Send:

   ```text
   I want to report a store issue
   ```

2. Store code:

   ```text
   S012
   ```

3. Issue type:

   ```text
   Price label error
   ```

4. Product:

   ```text
   HydraGlow Hydrating Toner 200ml
   ```

5. Details:

   ```text
   The shelf label shows a lower price than the till.
   ```

**Expected result:** The urgent message appears, then the summary shows **Priority: Urgent** and a report number.

**B.** Out of stock (standard)

1. Send:

   ```text
   I want to report a store issue
   ```

2. Store code:

   ```text
   S012
   ```

3. Issue type:

   ```text
   Out of stock
   ```

4. Product:

   ```text
   HydraGlow Hydrating Toner 200ml
   ```

5. Details:

   ```text
   This item sold out on the first morning of the sale.
   ```

**Expected result:** No urgent message; the summary shows **Priority: Standard** and a report number.

---

## Task 4 – Email a confirmation

In this task, you make the agent email a confirmation of the report. In this lab, the email goes **to you**, to stand in for the Mega Sale project team's mailbox, so you don't need anyone else's email address. First you add the Outlook tool to the agent, then you call it from the **Report a store issue** topic.

If the Office 365 Outlook connector is unavailable or **Blocked by policy**, skip this task and go to [Note: If the Outlook connector isn't available](#note-if-the-outlook-connector-isnt-available) at the end of this page.

### Task 4.1 – Add the Outlook tool to the agent

1. Go to the **Tools** tab, and select **Add a tool**.
2. Select **Connector**, search for `Office 365 Outlook`, and select **Send an email (V2)**.
3. Set up the connection:
   - Select **Connection**. If it shows **Not connected**, select **Create new connection**.
   - **Display name:**

     ```text
     Lab Mega Sale Assistant
     ```

   - Select **Create**, and sign in with your Microsoft 365 work account.
4. Select **Add and configure**.
5. On the tool's page, under **Additional details**, set **When this will be used** to **Only when referenced by topics or agents**.

   > This way, the agent sends an email only from the **Report a store issue** topic, and never decides on its own to send one.

6. Leave the **Inputs** as they are, and select **Save**.

> [!NOTE]
> Don't fill in the inputs on the tool's page. The tool's inputs can't use the topic's variables, such as `Topic.ReportID`. In Task 4.2, you put the email text into **global** variables first, so the tool can use them.

### Task 4.2 – Prepare the email text

The tool's inputs can only use **global** variables and **system** variables, not topic variables. In this task, you build the email subject and body from the topic's variables, and store them in two global variables.

1. Go to the **Topics** tab, and open the **Report a store issue** topic.
2. Under the summary message node from Task 3.3, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     EmailSubject
     ```

   - Select the variable to open **Variable properties**, and under **Usage**, select **Global (any topic can access)**. The name changes to `Global.EmailSubject`.
   - **To value:** Select **Formula**, and paste:

     ```text
     "[Store Issue] " & Topic.ReportID & " – " & Topic.Priority & " – Store " & Topic.StoreCode
     ```

3. Under the **EmailSubject** node, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     EmailBody
     ```

   - Select the variable to open **Variable properties**, and under **Usage**, select **Global (any topic can access)**. The name changes to `Global.EmailBody`.
   - **To value:** Select **Formula**, and paste:

     ```text
     "<p>A new Mega Sale store issue has been reported.</p>" &
     "<p><b>Report ID:</b> " & Topic.ReportID & "<br>" &
     "<b>Priority:</b> " & Topic.Priority & "<br>" &
     "<b>Store:</b> " & Topic.StoreCode & "<br>" &
     "<b>Issue type:</b> " & Text(Topic.IssueType) & "<br>" &
     "<b>Product:</b> " & Topic.ProductName & "<br>" &
     "<b>Details:</b> " & Topic.IssueDetails & "<br>" &
     "<b>Reported by:</b> " & System.User.DisplayName & "</p>"
     ```

4. Select **Save**.

> [!NOTE]
> `System.User.Email` and `System.User.DisplayName` are **system variables**: the agent fills them in automatically from the signed-in user, so you don't need to ask for them.

### Task 4.3 – Send the email from the topic

1. Under the **EmailBody** node, select **+** > **Add a tool**, and select **Send an email (V2)**.
2. In the **Inputs** section, set each input as follows. Don't type the variable names; **select** them from the list.
   - **To**
     - **Fill using:** Select **Custom value**.
     - **Value:** Select the value box, go to the **System** tab, and select **User.Email**.
   - **Subject**
     - **Fill using:** Select **Custom value**.
     - **Value:** Select the value box, go to the **Custom** tab, and select **Global.EmailSubject**.
   - **Body**
     - **Fill using:** Select **Custom value**.
     - **Value:** Select the value box, go to the **Custom** tab, and select **Global.EmailBody**.

3. Under the **Send an email (V2)** node, select **+** > **Send a message**, and paste:

   ```text
   📧 A confirmation email has been sent. Keep your Report ID for follow-up.
   ```

4. Select **Save**.

### Task 4.4 – Test the email

1. Start a new test session, and send each input below in order.
   - Send:

     ```text
     I want to report a store issue
     ```

   - Store code:

     ```text
     S012
     ```

   - Issue type:

     ```text
     Price label error
     ```

   - Product:

     ```text
     HydraGlow Hydrating Toner 200ml
     ```

   - Details:

     ```text
     The shelf label shows a lower price than the till.
     ```

   If the test panel asks you to connect or allow access, select **Allow**.

2. Check your inbox.

   **Expected result:** You receive an email with a subject like `[Store Issue] MS-1201-093015 – Urgent – Store S012`. The report number shows the date and time of your test.

---

## Task 5 – Test the whole flow

In this task, you test the complete conversation: Ask, Collect, Decide and Send.

1. Start a new test session, and send:

   ```text
   When is the store display deadline?
   ```

   **Expected result:** The agent answers from knowledge: **22:00 on 30 November** (**Ask**).

2. Send:

   ```text
   I need to report a problem in my store
   ```

   **Expected result:** The agent starts the **Report a store issue** topic (**Collect**).

3. Answer the agent's questions. Send each input below in order.
   - Store code:

     ```text
     S012
     ```

   - Issue type:

     ```text
     Discount not applied at checkout
     ```

   - Product:

     ```text
     HydraGlow Hydrating Toner 200ml
     ```

   - Details:

     ```text
     The 30% discount isn't applied at the till for this product.
     ```

   **Expected result:** The agent shows the urgent message (**Decide**), then the summary, and sends the confirmation email (**Send**). If you followed [Note: If the Outlook connector isn't available](#note-if-the-outlook-connector-isnt-available), no email is sent.

---

## Summary

You built the **Store Issue Reporter** in the **Mega Sale Assistant**:

- ✅ **Ask:** Answers from the Mega Sale documents
- ✅ **Collect:** A topic with four questions and variables
- ✅ **Decide:** A condition that flags urgent issues, plus a report number
- ✅ **Send:** A connector tool that emails a confirmation

### Key idea

- A **topic** makes the agent ask the same questions in the same order every time, so every report is complete.
- **Conditions** and simple formulas let the agent apply rules, such as flagging urgent issues.
- **Ask → Collect → Decide → Send** works for almost any request in your team: maintenance requests, stock requests, IT issues, event registrations and more. Think of one process in your team where people often send incomplete requests. That's a good candidate for your next agent.

---

## Note: If the Outlook connector isn't available

If the Office 365 Outlook connector is unavailable or **Blocked by policy**, skip Task 4 and add a line to the summary message instead.

1. Open the **Report a store issue** topic.
2. In the summary message from Task 3.3, go to the end, add a new line, and paste:

   ```text
   Please copy this summary and send it to the Mega Sale project team.
   ```

3. Select **Save**.
4. Test the agent with the steps in Task 5.

> Your agent still **collects** complete reports and **decides** their priority. Sending the report automatically is the only step you're not doing.
