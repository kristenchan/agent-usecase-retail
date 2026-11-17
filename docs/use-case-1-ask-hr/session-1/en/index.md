# Use Case 1 – Ask HR (Session 1)

## About this lab

You're working at **Tailwind Traders**, a simulated health and beauty retailer.

> Tailwind Traders is a fictitious company. Read the [company introduction](../../../../README.md).

In this lab, you build **Ask HR**, an agent that answers employees' leave and benefits questions, by describing it in plain language. Then you give it a new ability with a **skill**: drafting a leave request that's checked against the rules.

- **Tool:** Agent Builder in Microsoft 365 Copilot
- **Estimated time:** 40–55 minutes
- **Level:** Beginner, no code

You'll learn how to:

- Create an agent by **describing it in plain language**, and review the instructions Agent Builder writes for you
- Add a policy document as knowledge, and check answers that contain rules and numbers
- Set **privacy boundaries**, so the agent doesn't discuss personal data or other people
- **Add a skill** by describing it, and compare it with a ready-made skill built by someone else

---

## Before you start

Check each item before the lab. Select a file name to download it, and save the files somewhere easy to find, such as your desktop.

> All files are **fictional** and created for training only.

| ✔ | What you need | Details |
|---|---|---|
| ☐ | A work account with access to **Microsoft 365 Copilot Chat** | Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and check that you can open **Copilot** |
| ☐ | Access to **Agent Builder** in Microsoft 365 Copilot | In Copilot, check that you can see **Agents & Skills** > **New agent** |
| ☐ | File 1 (PDF): [`Employee-Leave-and-Benefits-Guide.pdf`](Employee-Leave-and-Benefits-Guide.pdf) | The HR guide the agent answers from: leave types, how to apply, the Mega Sale leave restriction, overtime and benefits |
| ☐ | File 2 (Skill package, .zip): [`leave-request-drafter-skill.zip`](leave-request-drafter-skill.zip) | A ready-made skill for Task 8.2. **Don't unzip it.** |

---

## Scenario

HR at Tailwind Traders spends about **20% of each day** answering employees' leave questions. The answers are in the **Employee Leave & Benefits Guide**, but it's a long document, and the rules depend on your role and the type of leave. So people ask HR, and HR answers the same questions many times a day.

Even when people know the rules, they still need help **writing the leave request**: which details to include, whether they've given enough notice and who has to approve it. (Employees check their leave balance and submit leave requests in **MyHR**, the company's HR system.)

It gets even busier before **Mega Sale Week**, held every year from 1 to 7 December. It's the busiest week of the year at Tailwind Traders, and before it starts, HR gets the same questions from store staff, store managers and head office teams:

- *"Can I take annual leave during the sale?"*
- *"If I work overtime during the sale, can I take time off later?"*
- *"Can I use my staff discount on top of the sale price?"*

To free up HR's time, Tailwind Traders wants an agent that employees can ask first, any time, and that gives the same answer the guide gives.

**Your task:**

1. Build **Ask HR**, an agent that answers leave and benefits questions from the guide.
2. Give it a new ability with a **skill**: drafting a leave request that's checked against the rules.

---

## Task 1 – Create the agent by describing it

You don't need to fill in any forms to start. You describe the agent you want, and Agent Builder creates it for you.

### Task 1.1 – Open Agent Builder

1. Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and open **Copilot**.
2. Select **Agents & Skills**, and then select **New agent**.

### Task 1.2 – Describe the agent

1. Copy the following text, paste it into the **Message Agent Builder** box, and send it:

   ```text
   Create an agent called "Ask HR".
   It answers questions about leave types, how to apply for leave, the Mega Sale
   leave restriction period, overtime and time off in lieu, annual leave
   carry-over and employee benefits such as the staff discount.
   It must answer only from the Employee Leave & Benefits Guide that I provide
   and say which section the answer comes from.
   It can't see anyone's leave balance and must never discuss another
   employee's leave or personal situation.
   The tone should be friendly, clear and short.
   ```

### Task 1.3 – Check what Agent Builder created

1. Look at the **Configure** panel.
2. Check that Agent Builder has filled in the **Name**, **Description** and **Instructions** for you.
3. Scroll down to the **Skills** section. Agent Builder sometimes creates a skill from your description, and the generated **Instructions** may mention it. If you see a skill there, remove it for now. You'll learn what skills are and add one step by step in Task 8. (If the **Skills** section is empty, skip this step.)

> [!TIP]
> **This is the main idea of Agent Builder:** you describe what you want in plain language, and Agent Builder turns it into a working agent. You can always open the **Configure** panel to see and fine-tune what it created.

---

## Task 2 – Improve the instructions

The generated instructions are a good start. If you want to change them, you can edit the **Instructions** directly to make them clearer and more specific. In this task, you replace them with a version that's already been improved.

### Task 2.1 – Read the generated instructions

1. Stay in the **Configure** panel.
2. Read the text in **Instructions**. Which parts of your description from Task 1.2 can you find?

### Task 2.2 – Replace the instructions

1. Select all the text in **Instructions**, and delete it.
2. Copy the following text, and paste it into **Instructions**:

   ```text
   # Role
   You are Ask HR, the leave and benefits helper.

   # What you do
   - Answer questions about leave types, how to apply, the Mega Sale leave
     restriction period, overtime and time off in lieu, annual leave
     carry-over and employee benefits.
   - Use only the Employee Leave & Benefits Guide. Always mention the section
     the answer comes from.

   # How you answer
   - Keep answers friendly, clear and short. Use bullet points.
   - If the answer depends on the employee's role (store staff or head
     office) or the type of leave, ask one short clarifying question first.

   # What you don't do
   - You can't see anyone's leave balance or leave records. For personal
     balances, tell the user to check MyHR.
   - Never discuss another employee's leave, health or personal situation.
   - The guide doesn't cover local labour laws. For legal entitlements in a
     specific country, suggest contacting the local HR team.
   - If the guide doesn't answer the question, say so and suggest contacting HR.
   ```

> [!TIP]
> Good instructions usually cover four things: **Role** (who the agent is), **What you do** (scope and sources), **How you answer** (format and tone), and **What you don't do** (boundaries).

---

## Task 3 – Add knowledge

In this task, you give the agent the HR guide to answer from.

1. In the **Configure** panel, go to the **Knowledge** section.
2. Upload `Employee-Leave-and-Benefits-Guide.pdf` (File 1 from **Before you start**).
3. Wait until the upload finishes. The file appears grey while it uploads.

---

## Task 4 – Add starter prompts

Starter prompts are example questions that people can select to get started.

1. In the **Configure** panel, go to the **Starter prompts** section.
2. Add the four starter prompts below. For each one, copy the **Title** and the **Message** into the matching boxes.

- **Starter prompt 1**
  - Title:

    ```text
    Leave during the sale
    ```

  - Message:

    ```text
    Can I take leave during Mega Sale Week?
    ```

- **Starter prompt 2**
  - Title:

    ```text
    Marriage leave
    ```

  - Message:

    ```text
    I'm getting married. How does marriage leave work?
    ```

- **Starter prompt 3**
  - Title:

    ```text
    Overtime
    ```

  - Message:

    ```text
    Can I take time off instead of overtime pay?
    ```

- **Starter prompt 4**
  - Title:

    ```text
    Staff discount
    ```

  - Message:

    ```text
    Can I use my staff discount together with the sale discount?
    ```

---

## Task 5 – Create and open the agent

1. In the upper-right corner, select **Create**.
2. When the agent is ready, select **Go to agent**, or open **Ask HR** from **Agents & Skills**.

---

## Task 6 – Test the agent

In this task, you check that the agent's answers are correct.

**How to test:** Copy each question below into the Ask HR chat, send it, and compare the answer with the **expected result**. Try at least one question from the role closest to your own.

### Task 6.1 – Questions the agent should answer

**A-1** · Store staff

```text
Can I carry over unused annual leave?
```

**Expected result:** Up to **5 days**, used by **31 March** of the following year, with the section named

**A-2** · Everyone

```text
Can I use my staff discount together with the sale discount?
```

**Expected result:** **No**; the better discount applies, and Tailwind Rewards points still apply

**A-3** · Store manager

```text
Does a team member need a medical certificate for sick leave?
```

**Expected result:** Yes, for **2 or more consecutive days**

**A-4** · Head office staff

```text
Can I take time off instead of overtime pay?
```

**Expected result:** Yes, time off in lieu at **1:1**, used within **60 days**

**A-5** · New joiner

```text
How much annual leave do I get?
```

**Expected result:** **7 to 18 days**, depending on years of service

### Task 6.2 – Advanced questions (you'll improve these in Task 7)

**B-1** · Anyone

```text
I'm getting married next month. How do I apply for leave?
```

**What to look for:** Does it give **everything** you need: days, timing, notice and how to apply?

**B-2** · Anyone

```text
Can I take annual leave during Mega Sale Week?
```

**What to look for:** Does it ask whether you're **store staff or head office**? The answer is different for each.

### Task 6.3 – Questions the agent should decline

**C-1** · Store staff

```text
How many days of annual leave do I have left?
```

**Expected result:** Explains it can't see personal balances and points to **MyHR**

**C-2** · Store manager

```text
What leave did my colleague Amy take this week?
```

**Expected result:** Declines; doesn't discuss other employees

**C-3** · Head office staff

```text
What's the legal maternity leave in my country?
```

**Expected result:** Explains the guide doesn't cover local law; suggests the local HR team

---

## Task 7 – Improve the answers

In Task 6, the advanced questions may not have been answered well. In this task, you improve the instructions and test again.

### Task 7.1 – Make leave answers complete

1. In the Ask HR chat, copy and send:

   ```text
   I'm getting married next month. How do I apply for leave?
   ```

2. Check the answer. Does it cover the number of days, when to take the leave, the documents needed, the notice period and how to apply?
3. If something is missing, open Ask HR for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Ask HR**, and select **Edit**.
4. In the **Configure** panel, go to **Instructions**. At the end of the **# How you answer** section, add a new line and paste:

   ```text
   - When someone asks how to take a specific type of leave, always cover:
     1) number of days, 2) when it must be taken, 3) documents needed,
     4) how much notice is needed, 5) who approves and how to apply (MyHR).
   ```

5. Select **Update**.
6. Open the Ask HR chat, start a new chat, and send the question.

   ```text
   I'm getting married next month. How do I apply for leave?
   ```

   **Expected result:** The answer covers all five points: 5 days, within 3 months of registration, documents, 2 weeks' notice (because it's more than 3 days) and how to apply in MyHR.

### Task 7.2 – Ask about the role before answering

1. In the Ask HR chat, start a new chat, and copy and send:

   ```text
   Can I take annual leave during Mega Sale Week?
   ```

2. Check the answer. Did the agent ask whether you're store staff or head office staff?
3. If it didn't, open Ask HR for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Ask HR**, and select **Edit**.
4. In the **Configure** panel, go to **Instructions**. At the end of the **# How you answer** section, add a new line and paste:

   ```text
   - For any question about leave between 28 November and 7 December, first
     ask: "Are you store staff or head office staff supporting the sale?"
   ```

5. Select **Update**.
6. Start a new chat to test as **store staff**. Send the question:

   ```text
   Can I take annual leave during Mega Sale Week?
   ```

   When the agent asks about your role, copy and send:

   ```text
   store staff
   ```

   **Expected result:** Annual leave is not normally approved during the restriction period; exceptions need **Area Manager** approval.

7. Start another new chat to test as **head office** staff. Send the question:

   ```text
   Can I take annual leave during Mega Sale Week?
   ```

   When the agent asks about your role, copy and send:

   ```text
   head office
   ```

   **Expected result:** Leave needs **department head** approval.

---

## Task 8 – Add a new skill

Ask HR can now **explain** the rules. But employees also want help **doing** something: writing the leave request they'll send to their manager. That's a separate, repeatable task with fixed steps and a fixed format, so instead of making the instructions longer, you'll add it as a **skill**.

> [!NOTE]
> **What's a skill?** A skill is a packaged, step-by-step way to do **one specific task**. It's stored as a `SKILL.md` file, sometimes with templates or reference files. **Instructions** apply to every conversation; a **skill** is used **only when its task comes up**, and follows the same steps every time.

You'll add the skill in two ways: first by **describing** it (Task 8.1), then by **uploading** a ready-made skill that someone else has designed (Task 8.2).

### Task 8.1 – Describe the skill

1. Open Ask HR for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Ask HR**, and select **Edit**.
2. In the **Configure** panel, scroll down to the **Skills** section. If you can't see **Skills**, go to [Note: If Skills aren't available](#note-if-skills-arent-available) at the end of this page.
3. Copy the following text, paste it into the **Message Agent Builder** box on the left, and send it:

   ```text
   Add a reusable skill called "Leave request drafter" to this agent.
   When an employee says they want to take leave, the skill always follows
   the same steps:
   1. Ask for anything that's missing: leave type, first and last day of
      leave, role (store staff or head office) and who will cover their work.
   2. Check the request against the Employee Leave & Benefits Guide: notice
      period, the Mega Sale leave restriction (28 November to 7 December)
      and who must approve it.
   3. Show a short pre-check result: looks OK, needs extra approval, or
      not enough notice. Explain why in one sentence.
   4. Write a leave request message to the employee's manager in a fixed
      format: subject line, dates, number of days, leave type, cover
      arrangement and a polite closing.
   5. Remind the employee to submit the request in MyHR. Never say the
      leave is approved.
   Package this as a skill so the output looks the same every time.
   ```

4. Wait for Agent Builder to finish. Its changes appear in the **Configure** panel.
5. In the **Skills** section, select the new skill, and review its **name**, **description** and **instructions**.

> [!NOTE]
> **The description matters.** The agent decides when to use a skill by reading its description. A good description says **what the skill does** and **when to use it**, for example: *"Use when an employee wants to take, apply for or request leave."*

If no skill appears after a minute, continue with Task 8.2.

### Task 8.2 – Upload a ready-made skill

Skills don't have to be written from scratch. You can upload a skill that a colleague, your HR team or IT has already designed and tested. In this task, you replace the skill from Task 8.1 with the ready-made skill in `leave-request-drafter-skill.zip`.

1. In the **Configure** panel, go to the **Skills** section.
2. Remove the skill you created in Task 8.1. Two skills for the same task can confuse the agent. (If no skill was created in Task 8.1, skip this step.)
3. Select **Add**.
4. Upload `leave-request-drafter-skill.zip` (File 2 from **Before you start**). Upload the complete `.zip` file; don't unzip it.
5. Select the new skill, and review its **name**, **description**, **instructions** and included files.

<details>
<summary><strong>See the SKILL.md inside the package</strong></summary>

```markdown
---
name: leave-request-drafter
description: Drafts a leave request message for a Tailwind Traders employee
  and pre-checks it against the leave rules. Use when an employee wants to
  take, apply for or request leave, or asks for help writing a leave request.
---

# Leave request drafter

## Step 1 – Collect the details
Ask only for what's missing, in one short message:
- Leave type (for example annual, personal, marriage or time off in lieu)
- First and last day of leave
- Role: store staff, or head office staff supporting the sale
- Who will cover their work

## Step 2 – Pre-check the rules
Use the Employee Leave & Benefits Guide. Check:
1. Notice: at least 3 working days; at least 2 weeks for 3 or more
   consecutive days.
2. Mega Sale restriction (28 November – 7 December):
   - Store staff: annual and personal leave aren't normally approved;
     exceptions need Area Manager approval.
   - Head office staff supporting the sale: department head approval.
   - Sick leave and bereavement leave aren't restricted.
3. Any rule for that leave type (for example, marriage leave is 5 days,
   taken within 3 months of the marriage registration).

## Step 3 – Show the pre-check result
Show exactly one of these, with one sentence explaining why and the
section of the guide:
- ✅ Looks OK to submit
- ⚠️ Needs extra approval (say who)
- ❌ Not enough notice (say how much notice is needed)

## Step 4 – Draft the message
Use this format:

Subject: Leave request – <leave type> – <first day> to <last day>

Hi <manager>,

I'd like to request <number> day(s) of <leave type> from <first day>
to <last day>.
Cover: <name> will look after <work>.
<Only if extra approval is needed: "I understand this is during the Mega
Sale restriction period and needs <approver> approval.">

I'll submit the request in MyHR once you're happy with it.

Thank you,
<employee name>

## Rules
- Never say the leave is approved. Only the manager and MyHR can approve it.
- Never guess the employee's leave balance; tell them to check MyHR.
- Show how you counted the number of days.
- Keep the whole answer under 200 words.
```

</details>

**Compare:** Look at the skill Agent Builder wrote in Task 8.1 and this ready-made skill. Which one has clearer steps? Which has a more precise output format? Which description makes it easier for the agent to know **when** to use the skill?

### Task 8.3 – Save and test

1. Select **Update** to save the agent.
2. Open the Ask HR chat and start a new chat.
3. Copy each request below into the chat, send it, and compare the answer with the **expected result**. Answer the agent's follow-up questions with any names you like.

**Test-1** · Store staff

```text
I'm store staff. I want annual leave on 3 and 4 December.
```

**Expected result:** Asks who will cover; **⚠️ needs Area Manager approval** (Mega Sale restriction); drafts the message in the fixed format

**Test-2** · Head office staff

```text
I'd like 3 days of annual leave starting tomorrow.
```

**Expected result:** **❌ Not enough notice**: 3 or more days need **2 weeks'** notice

**Test-3** · Anyone

```text
I'm getting married. I'd like marriage leave for 5 days starting one month from today.
```

**Expected result:** **✅ Looks OK** (enough notice); mentions the 3-month rule; drafts the message

**Test-4** · Anyone

```text
Just approve my leave for next Friday.
```

**Expected result:** Explains it **can't approve** leave; offers to draft a request instead

**Test-5** · Anyone

```text
Can I take annual leave during Mega Sale Week?
```

**Expected result:** A normal answer, **without** drafting a request. The skill is used only when someone wants to request leave

> [!NOTE]
> **Running this lab between late October and early December?** Tests 2 and 3 use dates relative to today, so they may land in the Mega Sale restriction period (28 November – 7 December). If they do, the agent should also mention the extra approval. That's the correct result.

---

## Task 9 – Share the agent

1. In Copilot, select **Agents & Skills**, open the **…** menu next to **Ask HR**, and select **Share**.
2. Choose who can use the agent (for example, specific people), and add a colleague.
3. Copy the link and send it to your colleague.

> [!IMPORTANT]
> **Before you share, remember:**
> - **Uploaded files are shared with the agent.** Anyone who can use the agent can get answers based on the files you uploaded. Only upload content that everyone you share with is allowed to see.
> - **Check the answers.** Agents can make mistakes. Encourage colleagues to check important answers against the guide.

---

## Summary

You built **Ask HR**:

- ✅ Created an agent by **describing it in plain language**
- ✅ Reviewed and improved the generated instructions
- ✅ Added a policy guide as knowledge and tested answers with checkable facts
- ✅ Set privacy boundaries for personal data
- ✅ **Added a new skill** by describing it, and by uploading a ready-made skill

### Key idea

- **Instructions** say who the agent is and what it must never do. They apply to every conversation.
- A **skill** gives the agent a new, repeatable ability with fixed steps. It's used only when that task comes up.
- You can **describe** a skill and let Agent Builder write it, or **upload** a skill that someone else has designed.

---

## Note: If Skills aren't available

If your environment doesn't show **Skills**, you can still give the agent the leave request ability by adding the steps to the instructions.

1. Open Ask HR for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Ask HR**, and select **Edit**.
2. In the **Configure** panel, go to **Instructions**.
3. Go to the end of the instructions, add a new line, and paste:

   ```text
   # When someone wants to request leave
   1. Ask for anything missing: leave type, first and last day, role
      (store staff or head office) and who will cover their work.
   2. Check notice (3 working days; 2 weeks for 3+ days) and the Mega Sale
      restriction (28 Nov – 7 Dec: store staff need Area Manager approval;
      head office needs department head approval).
   3. Show one result: ✅ Looks OK / ⚠️ Needs extra approval / ❌ Not enough notice.
   4. Draft a message to the manager: subject line, dates, number of days,
      leave type, cover and a polite closing. Remind them to submit in MyHR.
   5. Never say the leave is approved.
   ```

4. Select **Update**.
5. Test the agent with the requests in Task 8.3.

> Adding steps to the instructions works, but every rule you add makes the instructions longer, and instructions apply to **every** conversation. A skill keeps the main instructions short, is used **only** when it's needed, and can be reused in other agents.
