# Use Case 3 – Review Insights Helper (Session 2)

## About this lab

You're working at **Tailwind Traders**, a simulated health and beauty retailer.

> Tailwind Traders is a fictitious company. Read the [company introduction](../../../../README.md).

In this lab, you build the **Review Response Helper** in Copilot Studio. A staff member gives the agent a new customer review. The agent classifies it, drafts a reply in the Tailwind Traders brand voice, and escalates health and safety issues. A person stays in control: the agent only **drafts** replies and never posts them, and the staff member, not the AI, decides whether a review mentions a health or safety issue.

- **Tool:** Microsoft Copilot Studio (standard harness)
- **Estimated time:** 60–75 minutes
- **Level:** Beginner to intermediate, no code (copy-and-paste formulas)

You'll learn how to:

- Add the Brand Voice Guide and the Customer Service Guide as **knowledge** in Copilot Studio
- Build a **topic** that collects details with choice, text, **Number** and **Yes/No** questions
- Classify reviews with **conditions**, and draft replies with **generative answers** inside a topic
- Send an escalation email with a **connector tool** (Office 365 Outlook), only when it's needed

---

## Before you start

Check each item before the lab. Select a file name to download it, and save the files somewhere easy to find, such as your desktop.

> All files are **fictional** and created for training only.

| ✔ | What you need | Details |
|---|---|---|
| ☐ | Access to **Copilot Studio** | Go to [https://copilotstudio.microsoft.com](https://copilotstudio.microsoft.com) and check that you can open it |
| ☐ | An **environment** you can build in | Use the **environment** picker at the top right |
| ☐ | An **Outlook** mailbox (Task 4 only) | Check that you can open Outlook with the same account |
| ☐ | File 1 (Word): [`Brand-Voice-Guide.docx`](Brand-Voice-Guide.docx) | Tone of voice, words to use and avoid, hashtags and required wording |
| ☐ | File 2 (Word): [`Customer-Service-Guide.docx`](Customer-Service-Guide.docx) | Complaint handling, refunds and exchanges, health and safety escalation, and reply dos and don'ts |

---

## Scenario

The Customer Experience team at Tailwind Traders replies to every customer review by hand. Each reply takes about **10 minutes**: getting the tone right and checking the **Customer Service Guide**. Replies also vary a lot. Some are too short, some promise refunds the business can't give, and some reviews about **skin irritation or allergies** are answered like any other complaint, when health and safety reviews must be escalated quickly to the **Quality team**.

It gets even busier during **Mega Sale Week**, held every year from 1 to 7 December. During Mega Sale Week, about **150 reviews** arrive in 7 days, and each one needs a reply:

- *"Love the bundle deals, great value!"*
- *"The pump on my body lotion leaked everywhere."*
- *"Nice texture but my skin got red and itchy after using it."*

To help the team reply faster and more consistently, and never miss a safety issue, Tailwind Traders wants an agent that answers questions about how to reply to customers and handles each new review: it classifies the review, drafts an on-brand reply for staff to check, and escalates health and safety issues straight away.

**Your task:**

1. Create **Review Response Helper** in Copilot Studio, so it answers questions about replying to customers from the Brand Voice Guide and the Customer Service Guide.
2. Build the **Handle a customer review** topic, which asks for the channel, the product, the rating, the review text and whether the review mentions a health or safety concern.
3. Classify the review with fixed rules, and draft an on-brand reply for staff to check.
4. Email the Quality team about health and safety issues, so they can act on them quickly.

---

## Task 1 – Create the agent

In this task, you create Review Response Helper in Copilot Studio and give it the Brand Voice Guide and the Customer Service Guide to answer from.

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
     Review Response Helper
     ```

   - **Agent settings (Optional)**
     - **Language:** English
     - **Solution:** Keep the default solution.
     - **Schema name**

       ```text
       lab_reviewhelper
       ```

6. Select **Create**.
7. On the **Overview** tab, in the **Details** section, select **Edit**, and fill in the fields below:

   - **Description**

     ```text
     Helps staff classify customer reviews, draft on-brand replies and escalate health and safety issues.
     ```

   - **Instructions**

     ```text
     # Role
     You are the Review Response Helper for staff who reply to customer reviews.

     # What you do
     - Answer questions about how to reply to customers, using the Brand Voice
       Guide and the Customer Service Guide.
     - When a user wants help with a specific customer review, use the
       "Handle a customer review" topic.

     # Rules for replies
     - Replies are drafts for staff to review. You never post replies.
     - Never admit legal responsibility, discuss compensation or promise refunds
       in a public reply. Invite the customer to contact Customer Service.
     - Never make medical claims.
     - Don't try to identify customers.
     ```

8. Select **Save**.

### Task 1.2 – Add knowledge and test

1. Select the **Knowledge** tab.
2. Select **Add knowledge** > **Upload file**, upload `Brand-Voice-Guide.docx` (File 1) and `Customer-Service-Guide.docx` (File 2) from **Before you start**, and select **Add to agent**.
3. When the files are **Ready**, copy the following question into the **Test your agent** panel, and send it:

   ```text
   What should we never say in a public reply to a complaint?
   ```

   **Expected result:** The agent answers from the Customer Service Guide (for example: don't admit responsibility, don't discuss compensation publicly).

---

## Task 2 – Build the "Handle a customer review" topic

In this task, you build a topic that asks the staff member five questions about a review and stores the answers.

### Task 2.1 – Create the topic

1. Select the **Topics** tab, and then select **Add a topic** > **From blank**.
2. Select **Details**, and fill in the topic details:

   - **Name**

     ```text
     Handle a customer review
     ```

   - **Description:**

     ```text
     Collects the details of a customer review, classifies it, drafts a reply and escalates health and safety issues.
     ```

   - **Model description:**

     ```text
     Use this topic when a staff member wants help replying to, classifying or
     escalating a specific customer review or customer comment.
     ```

3. Add a **Send a message** node.

   Under the **Trigger** node, select **+** > **Send a message**, and paste:

   ```text
   Let's handle this review. I'll ask 5 quick questions, then draft a reply for you to check.
   ```

4. Select **Save**.
5. Check that the topic starts when a staff member asks for help with a review. Copy the following text into the **Test your agent** panel, and send it:

   ```text
   Help me reply to a customer review
   ```

   **Expected result:** The agent starts the **Handle a customer review** topic and shows your opening message. You don't need to write any trigger phrases: the agent uses the **Model description** to decide when to start the topic.

### Task 2.2 – Ask five questions

The agent needs the details of the review before it can classify it and draft a reply. In this task, you add 5 question nodes. Two of them use special types: **Number** stores the rating as a number, so the agent can compare it, and **Boolean** stores the safety answer as true or false.

1. Add the first question, about the channel. Under the **Send a message** node, select **+** > **Ask a question**.

   > [!NOTE]
   > If Copilot Studio automatically adds a **Condition** node below a multiple choice question, **delete** it. You'll build your own conditions in Task 3.

   - Question text:

     ```text
     Where was the review posted?
     ```

   - **Identify:** **Multiple choice options**. Add these options:

     ```text
     Store survey
     ```

     ```text
     Online shop
     ```

   - Rename the variable to:

     ```text
     Channel
     ```

2. Add the second question, about the product. Under the **Where was the review posted?** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     Which product is it about? (or type N/A)
     ```

   - **Identify:** **User's entire response**
   - Rename the variable to:

     ```text
     ProductName
     ```

3. Add the third question, about the rating. Under the **Which product is it about? (or type N/A)** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     What rating did the customer give (1–5)?
     ```

   - **Identify:** **Number**
   - Rename the variable to:

     ```text
     Rating
     ```

4. Add the fourth question, about the review text. Under the **What rating did the customer give (1–5)?** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     Please paste the review text.
     ```

   - **Identify:** **User's entire response**
   - Rename the variable to:

     ```text
     ReviewText
     ```

5. Add the fifth question, about health and safety. Under the **Please paste the review text.** question node, select **+** > **Ask a question**.
   - Question text:

     ```text
     Does the review mention a health or safety concern, such as skin irritation, an allergic reaction or an injury?
     ```

   - **Identify:** **Boolean** (Yes/No)
   - Rename the variable to:

     ```text
     SafetyConcern
     ```

   > [!TIP]
   > **Why ask a person about safety?** Deciding whether something is a health or safety issue is a **high-risk judgement**. In this design, a person answers it, and the agent applies fixed rules based on that answer. Use AI for writing; keep critical decisions with people and rules.

6. Select **Save**, then copy the following text into the **Test your agent** panel, and send it:

   ```text
   Help me reply to a customer review
   ```

   **Expected result:** The agent asks all five questions, and you can see `Rating` stored as a number and `SafetyConcern` stored as true/false.

---

## Task 3 – Classify the review and draft a reply

In this task, you add rules that classify the review, show the classification, and then let the agent draft a reply with generative answers.

### Task 3.1 – Classify the review

The agent classifies each review with fixed rules, so the same review always gets the same classification. The result depends on the safety answer and the rating. You store it as text in a new variable, `Severity`.

You create `Severity` **before** the condition. Branches are on the same level, so a variable you create inside one branch can't be selected in the others.

1. Under the **Does the review mention a health or safety concern, such as skin irritation, an allergic reaction or an injury?** question node, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     Severity
     ```

   - **To value:** Select **Formula**, and paste the following. It sets the variable to empty text for now; the branches fill it in.

     ```text
     ""
     ```

2. Under the **Severity** node, select **+** > **Add a condition**.
3. On the new **Condition** node, select **+** (Add condition branch) twice.
   - You now have **three** condition branches plus **All other conditions**.
   - Branches are checked **from left to right**, and the first one that's true is used.

4. In **branch 1**:
   - Set the condition: `SafetyConcern` is equal to `true`
   - Under branch 1, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `Severity`.
   - **To value:** Paste:

     ```text
     🔴 Escalate
     ```

5. In **branch 2**:
   - Set the condition: `Rating` is less than or equal to `2`
   - Under branch 2, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `Severity`.
   - **To value:** Paste:

     ```text
     🟠 Priority
     ```

6. In **branch 3**:
   - Set the condition: `Rating` is equal to `3`
   - Under branch 3, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `Severity`.
   - **To value:** Paste:

     ```text
     🟡 Standard
     ```

7. In **All other conditions**:
   - Select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Select `Severity`.
   - **To value:** Paste:

     ```text
     🟢 Thank you
     ```

> [!NOTE]
> The **safety** rule is first on purpose. A review that mentions an allergic reaction is escalated **even if the rating is 5**.

### Task 3.2 – Show the classification

The staff member needs to see the classification, so they know how to handle the review. In this task, the agent shows it in a message.

1. Below the classification condition from Task 3.1 (where its branches join), select **+** > **Send a message**, and paste the text below. Use **{x}** to insert the variable:

   ```text
   Classification: {Topic.Severity}
   ```

2. Select **Save**.

### Task 3.3 – Draft a reply with generative answers

Now the agent writes the reply. Writing a reply needs flexibility, so you use AI here, not fixed rules. You use a **generative answers** node, which lets the agent write text using your knowledge **inside** a fixed topic. The formula gives the AI the review details and the rules the reply must follow.

1. Under the **Classification: {Topic.Severity}** message node, select **+** > **Advanced** > **Generative answers**.
2. For **Input**, select **…** / **{x}** > **Formula**, and paste:

   ```text
   "Draft a short public reply (maximum 80 words) to this customer review, " &
   "following the Brand Voice Guide and the Customer Service Guide. " &
   "Classification: " & Topic.Severity & ". " &
   "Channel: " & Text(Topic.Channel) & ". Product: " & Topic.ProductName & ". " &
   "Rating: " & Text(Topic.Rating) & " out of 5. " &
   "Review: " & Topic.ReviewText & ". " &
   "If the classification is Escalate, only express care, apologise for the " &
   "experience and ask the customer to contact Customer Service privately. " &
   "Do not discuss the cause, responsibility or compensation. " &
   "Never promise a refund. Never make medical claims."
   ```

3. Leave the data sources as the agent's knowledge.
4. Under the **Generative answers** node, select **+** > **Send a message**, and paste:

   ```text
   ✏️ This is a draft. Please check it before you post it.
   ```

5. Select **Save**.

### Task 3.4 – Test the four classifications

**How to test:**

- Start a new test session for each test.
- Send each input below in order, as your answer to the agent's question.

**A.** Happy customer, rating 5

1. Send:

   ```text
   Help me reply to a customer review
   ```

2. Channel:

   ```text
   Online shop
   ```

3. Product:

   ```text
   N/A
   ```

4. Rating:

   ```text
   5
   ```

5. Review text:

   ```text
   Love the bundle deals, great value!
   ```

6. Health or safety concern:

   ```text
   No
   ```

**Expected result:** 🟢 Thank you; a short, warm thank-you reply

**B.** Mixed review, rating 3

1. Send:

   ```text
   Help me reply to a customer review
   ```

2. Channel:

   ```text
   Store survey
   ```

3. Product:

   ```text
   N/A
   ```

4. Rating:

   ```text
   3
   ```

5. Review text:

   ```text
   Good discounts but the checkout queue was long.
   ```

6. Health or safety concern:

   ```text
   No
   ```

**Expected result:** 🟡 Standard; thanks and mentions improvement

**C.** Unhappy customer, rating 1

1. Send:

   ```text
   Help me reply to a customer review
   ```

2. Channel:

   ```text
   Online shop
   ```

3. Product:

   ```text
   Body lotion
   ```

4. Rating:

   ```text
   1
   ```

5. Review text:

   ```text
   The pump on my body lotion leaked everywhere.
   ```

6. Health or safety concern:

   ```text
   No
   ```

**Expected result:** 🟠 Priority; apology and next step; **no refund promise**

**D.** Health and safety issue, rating 4

1. Send:

   ```text
   Help me reply to a customer review
   ```

2. Channel:

   ```text
   Online shop
   ```

3. Product:

   ```text
   HydraGlow Face Cream 50ml
   ```

4. Rating:

   ```text
   4
   ```

5. Review text:

   ```text
   Nice texture but my skin got red and itchy after using it.
   ```

6. Health or safety concern:

   ```text
   Yes
   ```

**Expected result:** 🔴 Escalate, even though the rating is 4; a caring reply that asks the customer to get in touch privately; **no** discussion of the cause

---

## Task 4 – Escalate health and safety issues

Escalated reviews must reach the Quality team quickly. In this task, you make the agent send an escalation email **only** for 🔴 Escalate reviews. In this lab, the email goes **to you**, to stand in for the Quality team's mailbox. First you add the Outlook tool to the agent, then you call it from the **Handle a customer review** topic.

If the Office 365 Outlook connector is unavailable or **Blocked by policy**, skip this task and go to [Note: If the Outlook connector isn't available](#note-if-the-outlook-connector-isnt-available) at the end of this page.

### Task 4.1 – Add the Outlook tool to the agent

1. Go to the **Tools** tab, and select **Add a tool**.
2. Select **Connector**, search for `Office 365 Outlook`, and select **Send an email (V2)**.
3. Set up the connection:
   - Select **Connection**. If it shows **Not connected**, select **Create new connection**.
   - **Display name:**

     ```text
     Lab Review Response Helper
     ```

   - Select **Create**, and sign in with your Microsoft 365 work account.
4. Select **Add and configure**.
5. On the tool's page, under **Additional details**, set **When this will be used** to **Only when referenced by topics or agents**.

   > This way, the agent sends an email only from the **Handle a customer review** topic, and never decides on its own to send one.

6. Leave the **Inputs** as they are, and select **Save**.

> [!NOTE]
> Don't fill in the inputs on the tool's page. The tool's inputs can't use the topic's variables, such as `Topic.ReviewText`. In Task 4.2, you put the email text into **global** variables first, so the tool can use them.

### Task 4.2 – Prepare the email text

The tool's inputs can only use **global** variables and **system** variables, not topic variables. In this task, you build the email subject and body from the topic's variables, and store them in two global variables.

You add these nodes inside **branch 1** of the classification condition from Task 3.1 (`SafetyConcern` is equal to `true`), under its **Severity** node. Only health and safety reviews reach branch 1, so the agent never emails the Quality team about other reviews. The email is also sent **before** the agent drafts the reply, so the escalation doesn't depend on the AI step.

1. Go to the **Topics** tab, and open the **Handle a customer review** topic.
2. In **branch 1** of the classification condition, under the **Severity** node, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     EmailSubject
     ```

   - Select the variable to open **Variable properties**, and under **Usage**, select **Global (any topic can access)**. The name changes to `Global.EmailSubject`.
   - **To value:** Select **Formula**, and paste:

     ```text
     "[H&S Escalation] Customer review – " & Topic.ProductName
     ```

3. Under the **EmailSubject** node, select **+** > **Variable management** > **Set a variable value**.
   - **Variable:** Create a new variable, and name it:

     ```text
     EmailBody
     ```

   - Select the variable to open **Variable properties**, and under **Usage**, select **Global (any topic can access)**. The name changes to `Global.EmailBody`.
   - **To value:** Select **Formula**, and paste:

     ```text
     "<p><b>Health & safety escalation from a customer review</b></p>" &
     "<p><b>Channel:</b> " & Text(Topic.Channel) & "<br>" &
     "<b>Product:</b> " & Topic.ProductName & "<br>" &
     "<b>Rating:</b> " & Text(Topic.Rating) & " / 5<br>" &
     "<b>Review:</b> " & Topic.ReviewText & "<br>" &
     "<b>Reported by:</b> " & System.User.DisplayName & "</p>" &
     "<p>Please follow the health and safety escalation process in the Customer Service Guide.</p>"
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
   📧 Escalation email sent to the Quality team.
   ```

4. Select **Save**.

### Task 4.4 – Test the email

1. Start a new test session, and send each input below in order.
   - Send:

     ```text
     Help me reply to a customer review
     ```

   - Channel:

     ```text
     Online shop
     ```

   - Product:

     ```text
     HydraGlow Face Cream 50ml
     ```

   - Rating:

     ```text
     4
     ```

   - Review text:

     ```text
     Nice texture but my skin got red and itchy after using it.
     ```

   - Health or safety concern:

     ```text
     Yes
     ```

   If the test panel asks you to connect or allow access, select **Allow**.

2. Check your inbox.

   **Expected result:** You receive an email titled `[H&S Escalation] Customer review – HydraGlow Face Cream 50ml`. Tests A, B and C from Task 3.4 don't send an email.

---

## Task 5 – Test the whole flow

In this task, you test the whole agent from start to finish: answering a question, collecting the review, classifying it, escalating it and drafting a reply.

1. Start a new test session, and send:

   ```text
   How should we reply to a customer who wants compensation?
   ```

   **Expected result:** The agent answers from the Customer Service Guide.

2. Send:

   ```text
   Help me reply to a customer review
   ```

   **Expected result:** The agent starts the **Handle a customer review** topic.

3. Answer the agent's questions. Send each input below in order.
   - Channel:

     ```text
     Online shop
     ```

   - Product:

     ```text
     HydraGlow Face Cream 50ml
     ```

   - Rating:

     ```text
     4
     ```

   - Review text:

     ```text
     Nice texture but my skin got red and itchy after using it.
     ```

   - Health or safety concern:

     ```text
     Yes
     ```

   **Expected result:** The agent sends the escalation email, classifies the review as 🔴 Escalate, and then drafts a caring reply.

---

## Summary

You built the **Review Response Helper**:

- ✅ Answers questions from the brand and customer service guides
- ✅ Collects review details with choice, text, **number** and **Yes/No** questions
- ✅ Classifies reviews with rules (safety first) and **drafts replies with AI** inside a fixed topic
- ✅ Sends an escalation email only for health and safety issues

### Key idea

The best agents combine **rules** and **AI**:

- Use **rules** (conditions) for decisions that must always be the same, such as escalating safety issues.
- Use **AI** (generative answers) for work that benefits from flexibility, such as writing a reply.
- Keep **people** in control of high-risk judgements and of what gets published.

---

## Note: If the Outlook connector isn't available

If the Office 365 Outlook connector is unavailable or **Blocked by policy**, skip Task 4 and show an escalation message instead.

1. Open the **Handle a customer review** topic.
2. In **branch 1** of the classification condition from Task 3.1 (`SafetyConcern` is equal to `true`), under the **Severity** node, select **+** > **Send a message**, and paste:

   ```text
   🔴 Health & safety issue: please report this review to the Quality team now.
   ```

3. Select **Save**.
4. Start a new test session, and run test **D** from Task 3.4.

   **Expected result:** The agent shows the health and safety message, classifies the review as 🔴 Escalate and drafts a caring reply.

> Your classification and draft reply still work without the connector. The only difference is that a person reports the review to the Quality team instead of the agent sending an email.
