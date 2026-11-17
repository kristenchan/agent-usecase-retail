# Use Case 1 – Ask HR (Session 2)

## About this lab

You're working at **Tailwind Traders**, a simulated health and beauty retailer.

> Tailwind Traders is a fictitious company. Read the [company introduction](../../../../README.md).

In this lab, you build **Ask HR** in Copilot Studio and extend it into the **Leave Pre-check**. Before employees submit a leave request, the agent asks for the details, checks them against the leave rules and emails the result. The Leave Pre-check **doesn't submit or approve leave**. It helps employees get their request right the first time.

- **Tool:** Microsoft Copilot Studio (standard harness)
- **Estimated time:** 60–75 minutes
- **Level:** Beginner to intermediate, no code (copy-and-paste formulas)

You'll learn how to:

- Build a **topic** that asks choice and **date** questions and stores the answers in variables
- **Calculate with dates** using formulas, such as the number of days of leave and the notice given
- Check **conditions with several rules** (And / Or, branch order), so the same request always gets the same answer
- Send the result by email with a **connector** (Office 365 Outlook)

---

## Before you start

Check each item before the lab. Select a file name to download it, and save the files somewhere easy to find, such as your desktop.

> All files are **fictional** and created for training only.

| ✔ | What you need | Details |
|---|---|---|
| ☐ | Access to **Copilot Studio** | Go to [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com) and check that you can open it |
| ☐ | An **environment** you can build in | Use the **environment** picker at the top right |
| ☐ | An **Outlook** mailbox (Task 4 only) | Check that you can open Outlook with the same account |
| ☐ | File 1 (PDF): [`Employee-Leave-and-Benefits-Guide.pdf`](Employee-Leave-and-Benefits-Guide.pdf) | The HR guide the agent answers from: leave types, how to apply, the Mega Sale leave restriction, overtime and benefits. It's the same file as in Session 1, so if you already have it, you don't need to download it again |

---

## Scenario

HR at Tailwind Traders spends about **20% of each day** answering employees' leave questions. The answers are in the **Employee Leave & Benefits Guide**, but it's a long document, and the rules depend on your role and the type of leave.

Even when people know the rules, they still make mistakes when they apply. About **1 in 4** leave requests submitted in **MyHR**, the company's HR system, is sent back because of missing details, not enough notice or the Mega Sale restriction, so managers spend time explaining why.

It gets even busier before **Mega Sale Week**, held every year from 1 to 7 December. Leave is restricted from 28 November to 7 December, and before the sale HR sees the same mistakes again and again:

- Employees request annual leave during the **Mega Sale leave restriction period** (28 November – 7 December) without the extra approval.
- They request 3 or more days with less than **2 weeks' notice**.
- They don't know **who** needs to approve their request.

To cut the number of requests sent back, Tailwind Traders wants an agent that answers leave questions and pre-checks leave requests before employees submit them in MyHR.

**Your task:**

1. Create **Ask HR** in Copilot Studio, so it answers leave and benefits questions from the guide.
2. Build the **Leave pre-check** topic, which asks for the role, leave type and dates.
3. Calculate the number of days and the notice, and check the rules.
4. Email the pre-check result, so the employee can forward it to their manager.

---

## Task 1 – Create the agent

In this task, you create Ask HR in Copilot Studio and give it the HR guide to answer from.

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
     Ask HR
     ```

   - **Agent settings (Optional)**
     - **Language:** English
     - **Solution:** Keep the default solution.
     - **Schema name**

       ```text
       lab_askhr
       ```

6. Select **Create**.
7. On the **Overview** tab, in the **Details** section, select **Edit**, and fill in the fields below:

   - **Description**

     ```text
     Answers employees' leave and benefits questions and pre-checks leave requests against the rules.
     ```

   - **Instructions**

     ```text
     # Role
     You are Ask HR, the leave and benefits helper.

     # What you do
     - Answer questions about leave types, how to apply, the Mega Sale leave
       restriction period, overtime and time off in lieu, annual leave
       carry-over and employee benefits.
     - Use only the Employee Leave & Benefits Guide. Always mention the section
       the answer comes from.
     - When a user wants to check a leave request or plan leave on specific
       dates, use the "Leave pre-check" topic.

     # What you don't do
     - You can't see anyone's leave balance or leave records. For personal
       balances, tell the user to check MyHR.
     - Never discuss another employee's leave, health or personal situation.
     - You don't submit or approve leave. Employees apply in MyHR.
     ```

8. Select **Save**.

### Task 1.2 – Add knowledge and test

1. Select the **Knowledge** tab.
2. Select **Add knowledge** > **Upload file**, upload `Employee-Leave-and-Benefits-Guide.pdf` (File 1 from **Before you start**), and select **Add to agent**.
3. When the file is **Ready**, copy the following question into the **Test your agent** panel, and send it:

   ```text
   How many days of marriage leave do I get?
   ```

   **Expected result:** The agent answers **5 days** from the guide.

---

## Task 2 – Build the "Leave pre-check" topic

In this task, you build a topic that asks the employee four questions and stores the answers.

### Task 2.1 – Create the topic

1. Select the **Topics** tab, and then select **Add a topic** > **From blank**.
2. Select **Details**, and fill in the topic details:

   - **Name**

     ```text
     Leave pre-check
     ```

   - **Description:**

     ```text
     Checks an employee's leave request against the leave rules before they submit it in MyHR.
     ```

   - **Model description:**

     ```text
     Use this topic when an employee wants to check a leave request, plan leave
     on specific dates, or asks whether they can take leave on certain dates.
     ```

3. Add a **Send a message** node.

   Under the **Trigger** node, select **+** > **Send a message**, and paste:

   ```text
   Let's check your leave request against the rules before you submit it in MyHR. I'll ask 4 quick questions.
   ```

4. Select **Save**.
5. Check that the topic starts when an employee asks to check their leave. Copy the following text into the **Test your agent** panel, and send it:

   ```text
   I want to check my leave
   ```

   **Expected result:** The agent starts the **Leave pre-check** topic and shows your opening message. You don't need to write any trigger phrases: the agent uses the **Model description** to decide when to start the topic.

### Task 2.2 – Ask four questions

Add 4 question nodes.

1. Add the first question, about the role. Under the **Send a message** node, select **+** > **Ask a question**.

   > [!NOTE]
   > If Copilot Studio automatically adds a **Condition** node below a multiple choice question, **delete** it. You'll build your own conditions in Task 3.

   - Question text:

     ```text
     What's your role?
     ```

   - **Identify:** **Multiple choice options**. Add these options:

     ```text
     Store staff
     ```

     ```text
     Head office
     ```

   - Rename the variable to:

     ```text
     Role
     ```

2. Add the second question, about the leave type. Under the **What's your role?** question node, select **+** > **Ask a question**.

   > [!NOTE]
   > If Copilot Studio automatically adds a **Condition** node below a multiple choice question, **delete** it. You'll build your own conditions in Task 3.

   - Question text:

     ```text
     What type of leave?
     ```

   - **Identify:** **Multiple choice options**. Add these options:

     ```text
     Annual leave
     ```

     ```text
     Personal leave
     ```

     ```text
     Sick leave
     ```

     ```text
     Marriage leave
     ```

     ```text
     Bereavement leave
     ```

     ```text
     Other
     ```

   - Rename the variable to:

     ```text
     LeaveType
     ```

3. Add the third question, about the first day of leave. Under the **What type of leave?** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     What's the first day of your leave?
     ```

   - **Identify:** **Date**
   - Rename the variable to:

     ```text
     StartDate
     ```

4. Add the fourth question, about the last day of leave. Under the **What's the first day of your leave?** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     What's the last day of your leave?
     ```

   - **Identify:** **Date**
   - Rename the variable to:

     ```text
     EndDate
     ```

> [!TIP]
> With **Date**, the agent stores the answer as a date, which lets you calculate with it. When you test, enter the **day, month and year** in the format `d mmm yyyy` (for example `2 Dec 2026`). Don't use words such as `next Monday` or `tomorrow`.

5. Select **Save**, then copy the following text into the **Test your agent** panel, and send it:

   ```text
   I want to check my leave
   ```

   **Expected result:** The agent asks all four questions, and you can see `StartDate` and `EndDate` stored as dates.

---

## Task 3 – Calculate and check the rules

In this task, you calculate the number of days and the notice, check the leave rules and show the result.

### Task 3.1 – Calculate with dates

Before the agent can check the rules, it needs a few numbers. The leave rules depend on **how many days** the employee wants to take and **how much notice** they're giving. Mega Sale Week also has a special rule: leave is restricted from **28 November to 7 December**, so the agent needs these two dates to check whether the leave falls in that period.

In this task, you add five **Set a variable value** nodes that calculate these values from the dates the employee entered.

1. Under the **What's the last day of your leave?** question node, select **+** > **Variable management** > **Set a variable value**.
   - **What it calculates:** The start of the Mega Sale restriction period: 28 November in the same year as the first day of leave.
   - **Variable:** Create a new variable, and name it:

     ```text
     RestrictionStart
     ```

   - **To value:** Select **Formula**, and paste:

     ```text
     Date(Year(Topic.StartDate), 11, 28)
     ```

2. Under the **RestrictionStart** node, select **+** > **Variable management** > **Set a variable value**.
   - **What it calculates:** The end of the Mega Sale restriction period: 7 December in the same year as the first day of leave.
   - **Variable:** Create a new variable, and name it:

     ```text
     RestrictionEnd
     ```

   - **To value:** Select **Formula**, and paste:

     ```text
     Date(Year(Topic.StartDate), 12, 7)
     ```

3. Under the **RestrictionEnd** node, select **+** > **Variable management** > **Set a variable value**.
   - **What it calculates:** The number of calendar days of leave.
   - **Variable:** Create a new variable, and name it:

     ```text
     LeaveDays
     ```

   - **To value:** Select **Formula**, and paste:

     ```text
     DateDiff(Topic.StartDate, Topic.EndDate, TimeUnit.Days) + 1
     ```

4. Under the **LeaveDays** node, select **+** > **Variable management** > **Set a variable value**.
   - **What it calculates:** The number of days between today and the first day of leave (the notice).
   - **Variable:** Create a new variable, and name it:

     ```text
     NoticeDays
     ```

   - **To value:** Select **Formula**, and paste:

     ```text
     DateDiff(Today(), Topic.StartDate, TimeUnit.Days)
     ```

5. Under the **NoticeDays** node, select **+** > **Variable management** > **Set a variable value**.
   - **What it calculates:** **true** if any day of the leave falls in the restriction period.
   - **Variable:** Create a new variable, and name it:

     ```text
     InRestriction
     ```

   - **To value:** Select **Formula**, and paste:

     ```text
     Topic.StartDate <= Topic.RestrictionEnd && Topic.EndDate >= Topic.RestrictionStart
     ```

> [!TIP]
> The Mega Sale restriction period is **28 November – 7 December every year**. `Year(Topic.StartDate)` takes the year from the date the user entered, so the agent works in any year without changes. If the sale dates change, you only update these two variables.

### Task 3.2 – Check that the dates make sense

If the employee enters a last day that's before the first day, `LeaveDays` is less than 1. In this task, you stop the topic and ask the employee to check their dates.

1. Under the **InRestriction** node, select **+** > **Add a condition**.
2. In the **Condition** branch:
   - Set the condition: `LeaveDays` **is less than** `1`
   - Under the **Condition** branch, select **+** > **Send a message**, and paste:

     ```text
     The last day is before the first day. Please start again and check your dates.
     ```

   - Under the message node, select **+** > **Topic management** > **End current topic**.

3. Leave **All other conditions** empty. The conversation continues below the condition.

### Task 3.3 – Check the restriction rules

Next, the agent checks the Mega Sale restriction. The result depends on the leave type, whether the dates fall in the restriction period and the employee's role. You store the result as text in a new variable, `RestrictionResult`.

You create `RestrictionResult` **before** the condition. Branches are on the same level, so a variable you create inside one branch can't be selected in the others.

1. Below the date check condition from Task 3.2 (where its branches join), select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     RestrictionResult
     ```

   - **To value:** Select **Formula**, and paste the following. It sets the variable to empty text for now; the branches fill it in.

     ```text
     ""
     ```

2. Under the **RestrictionResult** node, select **+** > **Add a condition**.
3. On the new **Condition** node, select **+** (Add condition branch) twice.
   - You now have **three** condition branches plus **All other conditions**.
   - Branches are checked **from left to right**, and the first one that's true is used.

4. In **branch 1**:
   - Set the condition: `LeaveType` is equal to `Sick leave` **Or** `LeaveType` is equal to `Bereavement leave`
   - Under branch 1, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `RestrictionResult`.
   - **To value:** Paste:

     ```text
     ✅ Sick and bereavement leave are not affected by the Mega Sale restriction. Sick leave of 2 or more consecutive days needs a medical certificate.
     ```

5. In **branch 2**:
   - Set the condition: `InRestriction` is equal to `true` **And** `Role` is equal to `Store staff`
   - Under branch 2, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `RestrictionResult`.
   - **To value:** Paste:

     ```text
     ⚠️ Your dates fall in the Mega Sale leave restriction period (28 Nov – 7 Dec). For store staff, annual and personal leave are not normally approved. You need Area Manager approval.
     ```

6. In **branch 3**:
   - Set the condition: `InRestriction` is equal to `true` **And** `Role` is equal to `Head office`
   - Under branch 3, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `RestrictionResult`.
   - **To value:** Paste:

     ```text
     ⚠️ Your dates fall in the Mega Sale leave restriction period (28 Nov – 7 Dec). You need approval from your department head.
     ```

7. In **All other conditions**:
   - Select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `RestrictionResult`.
   - **To value:** Paste:

     ```text
     ✅ Your dates are outside the Mega Sale leave restriction period.
     ```

### Task 3.4 – Check the notice period

Leave of 3 or more days needs 2 weeks' notice. In this task, the agent checks the notice and stores the result in a new variable, `NoticeResult`. As in Task 3.3, you create the variable **before** the condition, so both branches can use it.

1. Below the restriction condition from Task 3.3 (where its branches join), select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     NoticeResult
     ```

   - **To value:** Select **Formula**, and paste:

     ```text
     ""
     ```

2. Under the **NoticeResult** node, select **+** > **Add a condition**.
3. In the **Condition** branch:
   - Set the condition: `LeaveDays` is greater than or equal to `3` **And** `NoticeDays` is greater than or equal to `0` **And** `NoticeDays` is less than `14`
   - Under the **Condition** branch, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `NoticeResult`.
   - **To value:** Paste:

     ```text
     ⚠️ Leave of 3 or more days needs 2 weeks' notice. Talk to your manager before you apply.
     ```

4. In **All other conditions**:
   - Select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `NoticeResult`.
   - **To value:** Paste:

     ```text
     ✅ Notice period OK.
     ```

### Task 3.5 – Show the result

Finally, the agent shows the employee all the results in one message.

1. Below the notice condition from Task 3.4 (where its branches join), select **+** > **Send a message**, and paste the text below. Use **{x}** to insert the variables:

   ```text
   📋 Leave pre-check result

   Role: {Topic.Role}
   Leave type: {Topic.LeaveType}
   Dates: {Topic.StartDate} to {Topic.EndDate} ({Topic.LeaveDays} days)

   {Topic.RestrictionResult}
   {Topic.NoticeResult}

   Next step: submit your request in MyHR.
   ```

2. Select **Save**.

### Task 3.6 – Test the rules

**How to test:**

- Start a new test session for each test.
- Send each input below in order, as your answer to the agent's question.
- Enter dates with the **day, month and year** (for example `2 Dec 2026`).

**A.** Store staff, 3 days of annual leave during the sale

1. Send:

   ```text
   I want to check my leave
   ```

2. Role:

   ```text
   Store staff
   ```

3. Leave type:

   ```text
   Annual leave
   ```

4. First day:

   ```text
   3 Dec 2026
   ```

5. Last day:

   ```text
   5 Dec 2026
   ```

**Expected result:** ⚠️ Restriction, **Area Manager** approval

**B.** Head office, 4 days of annual leave during the sale

1. Send:

   ```text
   I want to check my leave
   ```

2. Role:

   ```text
   Head office
   ```

3. Leave type:

   ```text
   Annual leave
   ```

4. First day:

   ```text
   6 Dec 2026
   ```

5. Last day:

   ```text
   9 Dec 2026
   ```

**Expected result:** ⚠️ Restriction, **department head** approval

**C.** Store staff, 2 days of sick leave during the sale

1. Send:

   ```text
   I want to check my leave
   ```

2. Role:

   ```text
   Store staff
   ```

3. Leave type:

   ```text
   Sick leave
   ```

4. First day:

   ```text
   3 Dec 2026
   ```

5. Last day:

   ```text
   4 Dec 2026
   ```

**Expected result:** ✅ Not affected; medical certificate reminder

**D.** Head office, 3 days of annual leave with short notice

1. Send:

   ```text
   I want to check my leave
   ```

2. Role:

   ```text
   Head office
   ```

3. Leave type:

   ```text
   Annual leave
   ```

4. First day:

   ```text
   23 Nov 2026
   ```

5. Last day:

   ```text
   25 Nov 2026
   ```

**Expected result:** ✅ Outside restriction; ⚠️ notice warning

**E.** Store staff, last day before the first day

1. Send:

   ```text
   I want to check my leave
   ```

2. Role:

   ```text
   Store staff
   ```

3. Leave type:

   ```text
   Annual leave
   ```

4. First day:

   ```text
   4 Dec 2026
   ```

5. Last day:

   ```text
   2 Dec 2026
   ```

**Expected result:** Asks you to check your dates

> [!NOTE]
> The test dates are set for running this lab on **18 November 2026**. The notice check counts the days from **today**, so on another date the ⚠️ notice result may be different.

---

## Task 4 – Email the pre-check result

In this task, you make the agent email the result **to you**, so you can forward it to your manager. First you add the Outlook tool to the agent, then you call it from the **Leave pre-check** topic.

If the Office 365 Outlook connector is unavailable or **Blocked by policy**, skip this task and go to [Note: If the Outlook connector isn't available](#note-if-the-outlook-connector-isnt-available) at the end of this page.

### Task 4.1 – Add the Outlook tool to the agent

1. Go to the **Tools** tab, and select **Add a tool**.
2. Select **Connector**, search for `Office 365 Outlook`, and select **Send an email (V2)**.
3. Set up the connection:
   - Select **Connection**. If it shows **Not connected**, select **Create new connection**.
   - **Display name:**

     ```text
     Lab Ask HR
     ```

   - Select **Create**, and sign in with your Microsoft 365 work account.
4. Select **Add and configure**.
5. On the tool's page, under **Additional details**, set **When this will be used** to **Only when referenced by topics or agents**.

   > This way, the agent sends an email only from the **Leave pre-check** topic, and never decides on its own to send one.

6. Leave the **Inputs** as they are, and select **Save**.

> [!NOTE]
> Don't fill in the inputs on the tool's page. The tool's inputs can't use the topic's variables, such as `Topic.StartDate`. In Task 4.2, you put the email text into **global** variables first, so the tool can use them.

### Task 4.2 – Prepare the email text

The tool's inputs can only use **global** variables and **system** variables, not topic variables. In this task, you build the email subject and body from the topic's variables, and store them in two global variables.

1. Go to the **Topics** tab, and open the **Leave pre-check** topic.
2. Under the result message node from Task 3.5, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     EmailSubject
     ```

   - Select the variable to open **Variable properties**, and under **Usage**, select **Global (any topic can access)**. The name changes to `Global.EmailSubject`.
   - **To value:** Select **Formula**, and paste:

     ```text
     "[Leave Pre-check] " & System.User.DisplayName & " – " & Text(Topic.StartDate, "dd mmm") & " to " & Text(Topic.EndDate, "dd mmm")
     ```

3. Under the **EmailSubject** node, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     EmailBody
     ```

   - Select the variable to open **Variable properties**, and under **Usage**, select **Global (any topic can access)**. The name changes to `Global.EmailBody`.
   - **To value:** Select **Formula**, and paste:

     ```text
     "<p><b>Leave pre-check result</b></p>" &
     "<p><b>Employee:</b> " & System.User.DisplayName & "<br>" &
     "<b>Role:</b> " & Text(Topic.Role) & "<br>" &
     "<b>Leave type:</b> " & Text(Topic.LeaveType) & "<br>" &
     "<b>Dates:</b> " & Text(Topic.StartDate, "dd mmm yyyy") & " to " & Text(Topic.EndDate, "dd mmm yyyy") &
     " (" & Text(Topic.LeaveDays) & " days)</p>" &
     "<p>" & Topic.RestrictionResult & "<br>" & Topic.NoticeResult & "</p>" &
     "<p>This is a pre-check only. Please submit your request in MyHR.</p>"
     ```

4. Select **Save**.

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
   📧 I've emailed you this result. You can forward it to your manager.
   ```

4. Select **Save**.

### Task 4.4 – Test the email

1. Start a new test session, and send each input below in order.
   - Send:

     ```text
     I want to check my leave
     ```

   - Role:

     ```text
     Store staff
     ```

   - Leave type:

     ```text
     Annual leave
     ```

   - First day:

     ```text
     3 Dec 2026
     ```

   - Last day:

     ```text
     5 Dec 2026
     ```

   If the test panel asks you to connect or allow access, select **Allow**.

2. Check your inbox.

   **Expected result:** You receive an email titled `[Leave Pre-check] <your name> – 03 Dec to 05 Dec`.

---

## Task 5 – Test the whole flow

In this task, you test all four steps together: Ask, Collect, Decide and Send.

1. Start a new test session, and send:

   ```text
   Can I use my staff discount on sale items?
   ```

   **Expected result:** The agent answers from knowledge: no, the better discount applies (**Ask**).

2. Send:

   ```text
   Can I take leave from 3 to 5 December?
   ```

   **Expected result:** The agent starts the **Leave pre-check** topic (**Collect**).

3. Answer the agent's questions. Send each input below in order.
   - Role:

     ```text
     Store staff
     ```

   - Leave type:

     ```text
     Annual leave
     ```

   - First day:

     ```text
     3 Dec 2026
     ```

   - Last day:

     ```text
     5 Dec 2026
     ```

   **Expected result:** The agent calculates and checks the rules (**Decide**), shows ⚠️ restriction and Area Manager approval, then emails the result (**Send**).

---

## Summary

You built the **Leave Pre-check** in **Ask HR**:

- ✅ **Ask:** Answers from the employee guide
- ✅ **Collect:** Choice and **date** questions
- ✅ **Decide:** Date calculations and conditions with **several rules** (And / Or, branch order)
- ✅ **Send:** An email the employee can forward to their manager

### Key idea

- Business rules such as "3 or more days needs 2 weeks' notice" should give **the same answer every time**.
- Putting them in a **condition** (not just in instructions) makes the result predictable and easy to check.

---

## Note: If the Outlook connector isn't available

If the Office 365 Outlook connector is unavailable or **Blocked by policy**, skip Task 4 and add a line to the result message instead.

1. Open the **Leave pre-check** topic.
2. In the result message from Task 3.5, go to the end, add a new line, and paste:

   ```text
   Copy this result and share it with your manager.
   ```

3. Select **Save**.

> Your pre-check still works. The employee copies the result and shares it with their manager instead of forwarding an email.
