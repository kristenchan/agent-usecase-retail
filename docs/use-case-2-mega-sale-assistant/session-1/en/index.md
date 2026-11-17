# Use Case 2 – Mega Sale Assistant (Session 1)

## About this lab

You're working at **Tailwind Traders**, a simulated health and beauty retailer.

> Tailwind Traders is a fictitious company. Read the [company introduction](../../../../README.md).

In this lab, you build the **Mega Sale Assistant**, an agent that answers Mega Sale questions from three documents, by filling in its name, description and instructions yourself. Then you **rewrite** the store procedure part of its instructions as a **skill**, and compare the answers. Unlike Copilot Chat, where each person must find and upload the right files every time, an agent packages the instructions, knowledge and starter prompts once, so everyone gets consistent answers from the same sources.

- **Tool:** Agent Builder in Microsoft 365 Copilot
- **Estimated time:** 45–60 minutes
- **Level:** Beginner, no code

You'll learn how to:

- Create an agent in the **Configure** panel and write structured instructions
- Add several documents as knowledge, and add starter prompts
- Test the agent with questions from different teams, including questions it should decline
- **Rewrite part of the instructions as a skill**, and see why that makes the agent easier to maintain

---

## Before you start

Check each item before the lab. Select a file name to download it, and save the files somewhere easy to find, such as your desktop.

> All files are **fictional** and created for training only.

| ✔ | What you need | Details |
|---|---|---|
| ☐ | A work account with access to **Microsoft 365 Copilot Chat** | Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and check that you can open **Copilot** |
| ☐ | Access to **Agent Builder** in Microsoft 365 Copilot | In Copilot, check that you can see **Agents & Skills** > **New agent** |
| ☐ | File 1 (PDF): [`Mega-Sale-Playbook.pdf`](Mega-Sale-Playbook.pdf) | Sale dates, discount rules, excluded items, Tailwind Rewards double points and coupon stacking rules |
| ☐ | File 2 (Word): [`Store-Execution-Guide.docx`](Store-Execution-Guide.docx) | Display deadlines, checkout procedures, price label errors, out-of-stock handling and staffing |
| ☐ | File 3 (Word): [`Mega-Sale-FAQ.docx`](Mega-Sale-FAQ.docx) | Common questions from staff and customers about the sale |
| ☐ | File 4 (Skill package, .zip): [`store-issue-troubleshooter-skill.zip`](store-issue-troubleshooter-skill.zip) | A ready-made skill for Task 6. **Don't unzip it.** |

---

## Scenario

The Store Operations team at Tailwind Traders' head office answers about **50 questions a day** from stores, by phone and on Teams. The answers exist, but they're spread across three documents:

- the **Mega Sale Playbook** (sale rules, discounts, points and coupons)
- the **Store Execution Guide** (what stores must do and when)
- the **Mega Sale FAQ** (common questions from staff and customers)

People don't have time to search through these documents, so they ask head office, and the same questions are answered again and again. Store teams have an extra need: when something goes wrong at the till, they need **clear steps they can follow right away**, not a paragraph of text.

It gets even busier during **Mega Sale Week**, held every year from 1 to 7 December. It's the busiest week of the year at Tailwind Traders, every team is involved, and the number of questions triples. (**Tailwind Rewards** is the company's loyalty program.)

- *"When does the Mega Sale display need to be finished?"*
- *"Which items are excluded from the sale discount?"*
- *"Do all Tailwind Rewards members get double points?"*
- *"Can the sale discount be combined with a coupon?"*
- *"A price label shows a lower price than the till. What do I do?"*

To free up Store Operations' time, Tailwind Traders wants an agent that store and head office teams can ask first, any time, and that gives store teams step-by-step procedures from the official documents.

**Your task:**

1. Build the **Mega Sale Assistant**, an agent that answers Mega Sale questions from these documents and gives store teams step-by-step procedures.
2. Then **rewrite** the store procedure part of its instructions as a **skill**, and compare the results.

---

## Task 1 – Create the agent

In this task, you create the agent by filling in its name, description and instructions yourself.

### Task 1.1 – Open Agent Builder

1. Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and open **Copilot**.
2. Select **Agents & Skills**, and then select **New agent**. A screen opens with the **Message Agent Builder** box and the **Configure** panel.

### Task 1.2 – Enter the name and description

1. In the **Configure** panel, copy the following text into **Name**:

   ```text
   Mega Sale Assistant
   ```

2. Copy the following text into **Description**:

   ```text
   Answers questions about Mega Sale Week using the official sale documents.
   ```

### Task 1.3 – Add the instructions

1. In the **Configure** panel, go to **Instructions**.
2. Copy the following text, and paste it into **Instructions**. Notice the **Store issue answers** section: you'll move it into a skill in Task 6.

   ```text
   # Role
   You are the Mega Sale Assistant. You help store and head office teams
   find answers about Mega Sale Week quickly.

   # What you do
   - Answer questions about sale dates, discount rules, excluded items,
     Tailwind Rewards points, coupons, store preparation, checkout issues,
     price labels, out-of-stock items and staffing.
   - Use only the Mega Sale Playbook, the Store Execution Guide and the
     Mega Sale FAQ.
   - Always say which document (and section, if available) the answer
     comes from.

   # How you answer
   - Keep answers short and friendly.
   - Use bullet points for lists.
   - If the question is unclear, ask one short clarifying question.

   # Store issue answers
   When someone asks what to do about a problem in store (for example a
   discount not applied at checkout, a wrong price label, an out-of-stock
   promoted item or a display problem):
   - Start with a one-line summary of the issue.
   - Give numbered steps in the order staff should follow them. Start with
     the most urgent step.
   - Say who must approve any exception (for example, a supervisor approval
     code for a manual price override).
   - Say when to escalate and to whom, if the documents say so.
   - End with: "Source: <document name> – <section>".
   - Keep it short enough to read at the till.

   # What you don't do
   - If the documents don't contain the answer, say: "I can't find this in
     the Mega Sale documents. Please check with your Area Manager or the
     Mega Sale project team." Don't guess.
   - Don't share or discuss personal information about employees or
     customers (for example, salaries, contact details or performance).
   - Don't comment on competitors' prices or promotions.
   ```

> [!TIP]
> Good instructions usually cover four things: **Role** (who the agent is), **What you do** (scope and sources), **How you answer** (format and tone), and **What you don't do** (boundaries).

---

## Task 2 – Add knowledge

In this task, you give the agent the three Mega Sale documents to answer from.

1. In the **Configure** panel, go to the **Knowledge** section.
2. Upload the three documents (Files 1–3 from **Before you start**):
   - `Mega-Sale-Playbook.pdf`
   - `Store-Execution-Guide.docx`
   - `Mega-Sale-FAQ.docx`
3. Wait until all three files finish uploading. Files appear grey while they upload. Don't upload the `.zip` file here; you'll use it in Task 6.

---

## Task 3 – Add starter prompts

Starter prompts are example questions that people can select to get started.

1. In the **Configure** panel, go to the **Starter prompts** section.
2. Add the four starter prompts below. For each one, copy the **Title** and the **Message** into the matching boxes.

- **Starter prompt 1**
  - Title:

    ```text
    Key dates
    ```

  - Message:

    ```text
    What are the key dates for Mega Sale Week?
    ```

- **Starter prompt 2**
  - Title:

    ```text
    Excluded items
    ```

  - Message:

    ```text
    Which items are excluded from the sale discount?
    ```

- **Starter prompt 3**
  - Title:

    ```text
    Points and coupons
    ```

  - Message:

    ```text
    How do Tailwind Rewards points and e-coupons work during the sale?
    ```

- **Starter prompt 4**
  - Title:

    ```text
    Checkout issue
    ```

  - Message:

    ```text
    The discount didn't apply at checkout. What should I do?
    ```

---

## Task 4 – Create and open the agent

1. In the upper-right corner, select **Create**.
2. In the **Configure** panel, scroll down to the **Skills** section. Agent Builder sometimes creates a skill from your instructions automatically. If you see a skill there, remove it for now and select **Update**. You'll learn what skills are and add one step by step in Task 6. (If the **Skills** section is empty, skip this step.)
3. When the agent is ready, select **Go to agent**, or open **Mega Sale Assistant** from **Agents & Skills**.

   **Expected result:** The Mega Sale Assistant opens and shows your four starter prompts.

---

## Task 5 – Test the agent

In this task, you check that the agent's answers are correct.

**How to test:** Copy each question below into the Mega Sale Assistant chat, send it, and compare the answer with the **expected result**. Try at least one question from your own team.

### Task 5.1 – Questions the agent should answer

**A-1** · Store Operations

```text
When does the store display need to be ready?
```

**Expected result:** By **22:00 on 30 November**, with the source document named

**A-2** · Trading & Merchandising

```text
Which items are excluded from the sale discount?
```

**Expected result:** Prescription medicines, infant formula, gift cards and clearance items

**A-3** · Marketing

```text
Do all members get double points?
```

**Expected result:** Yes, double Tailwind Rewards points for all members during the sale

**A-4** · Finance

```text
Can a customer use an e-coupon on top of the sale price?
```

**Expected result:** Yes, **one** Tailwind Rewards e-coupon can be combined with the sale discount

### Task 5.2 – Store issue questions (you'll test these again in Task 6)

Write down how each answer is structured. You'll compare them after you add the skill.

**B-1** · Customer Service

```text
The discount didn't apply at checkout. What should I do?
```

**What to look for:** Numbered steps: check eligibility → re-scan → manual override with a **supervisor approval code**; source named

**B-2** · Store staff

```text
A price label shows a lower price than the till. What do I do?
```

**What to look for:** Correct the label immediately and **honour the lower price** for that customer; source named

### Task 5.3 – Questions the agent should decline

**C-1** · Anyone

```text
What's the salary of the store manager at our branch?
```

**Expected result:** Declines; doesn't discuss personal information

**C-2** · Marketing

```text
Is our competitor's sale cheaper than ours this week?
```

**Expected result:** Declines; doesn't comment on competitors

**C-3** · Customer Service

```text
What's the return policy for online orders in Japan?
```

**Expected result:** Says it can't find this in the Mega Sale documents and suggests who to ask

---

## Task 6 – Rewrite part of the instructions as a skill

In this task, you move the **Store issue answers** section of the instructions into a skill, and compare the answers.

Your agent works. But one section of its instructions, **Store issue answers**, is about a single task with fixed steps and a fixed format. As the sale gets closer, that section will keep growing (new issue types, new escalation rules), and **everything in the instructions applies to every conversation**, even questions about points or dates. Moving that section into a skill means:

- The main instructions stay **short and focused** on role and boundaries.
- The store procedure is used **only** when someone asks about a store problem.
- The Store Operations team can **update the procedure** without touching the rest of the agent, and the same skill can be **reused** in other agents.

> [!NOTE]
> **What's a skill?** A skill is a packaged, step-by-step way to do **one specific task**. It's stored as a `SKILL.md` file, sometimes with templates or reference files. **Instructions** apply to every conversation and are best for role, tone and boundaries, such as "Answer only from the Mega Sale documents." A **skill** is used **only when its task comes up** and is best for fixed steps, checks and output formats, such as "Store issue troubleshooter: what to do now, who approves, when to escalate."

### Task 6.1 – Look at the skill package

`store-issue-troubleshooter-skill.zip` (File 4 from **Before you start**) contains two files:

```text
store-issue-troubleshooter-skill.zip
├── SKILL.md          ← the steps and the answer format
└── issue-types.md    ← which document covers each type of store issue
```

Here's the `SKILL.md`. Compare it with the **Store issue answers** section in your instructions. It's the same idea, written as a clear procedure with an exact answer format.

```markdown
---
name: store-issue-troubleshooter
description: Gives store teams a step-by-step procedure for a Mega Sale store
  problem, such as a discount not applied at checkout, a wrong price label,
  an out-of-stock promoted item or a display problem. Use when someone asks
  what to do about a problem in store.
---

# Store issue troubleshooter

## Steps
1. Identify the issue type using issue-types.md. If it isn't clear, ask one
   short question.
2. Find the procedure in the document listed for that issue type.
3. Answer using the format below. Don't skip or rename any heading.

## Format
**Issue:** <one line>

**🛠️ What to do now**
1. <most urgent step>
2. <next step>

**✅ Who approves:** <approver, or "No approval needed">

**📣 Escalate if:** <when and to whom, or "Not covered in the documents –
ask your Area Manager">

**Source:** <document name> – <section>

## Rules
- Use only the Mega Sale documents. Never invent approval codes, phone
  numbers or names.
- Keep the answer under 120 words so it can be read at the till.
```

And here's `issue-types.md`:

```markdown
| Issue type | Examples | Where to look |
|---|---|---|
| Discount not applied | Sale price not showing at the till | Store Execution Guide – Checkout; Mega Sale Playbook – Discount rules |
| Price label error | Label and till price don't match | Store Execution Guide – Price labels |
| Out-of-stock promoted item | Promoted product sold out | Store Execution Guide – Out of stock |
| Display problem | Display not ready, materials missing | Store Execution Guide – Displays |
| Coupon or points problem | E-coupon rejected, points missing | Mega Sale Playbook – Tailwind Rewards; Mega Sale FAQ |
```

### Task 6.2 – Upload the skill

1. Open the Mega Sale Assistant for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Mega Sale Assistant**, and select **Edit**.
2. In the **Configure** panel, go to the **Skills** section. If you can't see **Skills**, go to [Note: If Skills aren't available](#note-if-skills-arent-available) at the end of this page.
3. Select **Add**.
4. Upload `store-issue-troubleshooter-skill.zip`. Upload the complete `.zip` file; don't unzip it.
5. Select the new skill, and review its **name**, **description**, **instructions** and included files.

   **Expected result:** *store-issue-troubleshooter* appears in the **Skills** section.

### Task 6.3 – Remove the old section from the instructions

This is the "rewrite" step: the procedure now lives in the skill, so you remove it from the instructions.

1. In the **Configure** panel, go to **Instructions**.
2. Delete the whole **# Store issue answers** section (from the heading down to "Keep it short enough to read at the till.").

   **Expected result:** Your instructions now have only four sections: **Role**, **What you do**, **How you answer** and **What you don't do**.

3. Select **Update**.

### Task 6.4 – Test and compare

1. Open the Mega Sale Assistant chat and start a new chat.
2. Copy each question below into the chat, send it, and compare the answer with the **expected result**. Test-1 and Test-2 are the store issue questions from Task 5.2.

**Test-1** · Customer Service

```text
The discount didn't apply at checkout. What should I do?
```

**Expected result:** The **🛠️ What to do now / ✅ Who approves / 📣 Escalate if / Source** format

**Test-2** · Store staff

```text
A price label shows a lower price than the till. What do I do?
```

**Expected result:** The same format, with the correct procedure

**Test-3** · Store staff

```text
A customer's e-coupon was rejected at the till. What should I do?
```

**Expected result:** The same format; the source is the Playbook or the FAQ

**Test-4** · Store Operations

```text
Our display materials haven't arrived. What should we do?
```

**Expected result:** The same format; if the documents don't cover it, **"ask your Area Manager"**, not a guess

**Test-5** · Marketing

```text
Do all members get double points?
```

**Expected result:** A **normal** answer, not the troubleshooting format. The skill is used only for store problems

**Compare with your notes from Task 5.2:**

| | Before (instructions only) | After (skill) |
|---|---|---|
| Are the answers in the same format every time? | | |
| Is it clear who approves and when to escalate? | | |
| Are non-store questions still answered normally? | | |
| How long are the instructions now? | | |

---

## Task 7 – Share the agent

1. In Copilot, select **Agents & Skills**, open the **…** menu next to **Mega Sale Assistant**, and select **Share**.
2. Choose who can use the agent (for example, specific people), and add a colleague.
3. Copy the link and send it to your colleague.

> [!IMPORTANT]
> **Before you share, remember:**
> - **Uploaded files are shared with the agent.** Anyone who can use the agent can get answers based on the files you uploaded. Only upload content that everyone you share with is allowed to see.
> - **SharePoint knowledge respects permissions.** If you use SharePoint files as knowledge instead, the agent only answers from content each user already has access to.
> - **Check the answers.** Agents can make mistakes. Encourage colleagues to check important answers against the source document.

---

## Summary

You built the **Mega Sale Assistant**:

- ✅ Created an agent with structured instructions in the **Configure** panel
- ✅ Added three documents as knowledge and four starter prompts
- ✅ Tested it with questions from different teams, including questions it should decline
- ✅ **Rewrote the store procedure as a skill**, and kept the instructions short
- ✅ Learned how to share it responsibly

### Key idea

- **Instructions** say who the agent is and what it must never do. They apply to every conversation.
- Instructions grow over time. When one part is about **a single, repeatable task** with fixed steps or a fixed format, move it into a **skill**.
- The agent uses the skill **only when it's needed**, and the skill can be updated or reused on its own.

---

## Note: If Skills aren't available

If your environment doesn't show **Skills**, keep the **Store issue answers** section in your instructions (skip Tasks 6.2 and 6.3). The agent still works, and you can make its store issue answers more consistent by adding the answer format from `SKILL.md`.

1. Open the Mega Sale Assistant for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Mega Sale Assistant**, and select **Edit**.
2. In the **Configure** panel, go to **Instructions**.
3. At the end of the **# Store issue answers** section, add a new line and paste:

   ```text
   Use this format:
   **Issue:** <one line>

   **🛠️ What to do now**
   1. <most urgent step>
   2. <next step>

   **✅ Who approves:** <approver, or "No approval needed">

   **📣 Escalate if:** <when and to whom, or "Not covered in the documents –
   ask your Area Manager">

   **Source:** <document name> – <section>
   ```

4. Select **Update**.
5. Test the agent with the questions in Task 6.4.

> Adding steps to the instructions works, but every rule you add makes the instructions longer, and instructions apply to **every** conversation. A skill keeps the main instructions short, is used **only** when it's needed, and can be reused in other agents.
