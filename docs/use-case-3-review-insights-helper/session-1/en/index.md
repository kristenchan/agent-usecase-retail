# Use Case 3 – Review Insights Helper (Session 1)

## About this lab

You're working at **Tailwind Traders**, a simulated health and beauty retailer.

> Tailwind Traders is a fictitious company. Read the [company introduction](../../../../README.md).

In this lab, you build **Review Insights Helper**, an agent that analyses Mega Sale Week customer reviews and turns them into clear findings that any team can act on. Then you use one **skill** to do two things: move the analysis rules out of the instructions, and add a new ability: the Mega Sale review report.

- **Tool:** Agent Builder in Microsoft 365 Copilot
- **Estimated time:** 50–65 minutes
- **Level:** Beginner to intermediate, no code

You'll learn how to:

- Analyse ratings and comments with an agent that uses code to count, rank and create charts
- Write instructions that **prioritise** findings and **always flag** health and safety issues
- Respect privacy, so the agent doesn't try to identify customers or judge employees
- **Rewrite** rules into a skill and **add** a new ability, with several files in one skill

---

## Before you start

Check each item before the lab. Select a file name to download it, and save the files somewhere easy to find, such as your desktop.

> All files are **fictional** and created for training only.

| ✔ | What you need | Details |
|---|---|---|
| ☐ | A work account with access to **Microsoft 365 Copilot Chat** | Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and check that you can open **Copilot** |
| ☐ | Access to **Agent Builder** in Microsoft 365 Copilot | In Copilot, check that you can see **Agents & Skills** > **New agent** |
| ☐ | File 1 (Excel): [`Mega-Sale-Customer-Reviews.xlsx`](Mega-Sale-Customer-Reviews.xlsx) | The reviews the agent analyses: about 150 reviews in one sheet, with Review Date, Channel, Store Code, Product, Category, Rating and Comment |
| ☐ | File 2 (Skill package, .zip): [`review-insights-skill.zip`](review-insights-skill.zip) | A ready-made skill for Task 8, with `SKILL.md`, `classification-rules.md` and `weekly-report-template.md`. **Don't unzip it.** |

---

## Scenario

The Customer Experience team at Tailwind Traders spends about **half a day every week** reading customer reviews and turning them into a report for the leadership team. Reading every review takes time, so important signals are easy to miss: a product that gets the same complaint again and again, long checkout queues, or a possible health or safety issue.

It gets even busier during **Mega Sale Week**, held every year from 1 to 7 December. During the sale, about **150 customer reviews** arrived in 7 days from the stores and the online shop. Each review has a date, channel, store, product, rating (1–5) and a comment. And different teams want different answers from them:

- *"Which product has the most complaints, and why?"*
- *"Which stores have the most complaints about queues?"*
- *"Are there any health or safety concerns?"*

To save the team's time and make sure no important signal is missed, Tailwind Traders wants an agent that any team can ask about the reviews, and that creates the review report in the same format every time.

**Your task:**

1. Build **Review Insights Helper**, an agent that analyses all the reviews and turns them into clear findings that any team can act on.
2. Use one **skill** to **rewrite** the analysis rules out of the instructions, and **add** a new ability: the Mega Sale review report.

---

## Task 1 – Create the agent

In this task, you create the agent by filling in its name, description and instructions.

### Task 1.1 – Open Agent Builder

1. Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and open **Copilot**.
2. Select **Agents & Skills**, and then select **New agent**. A screen opens with the **Message Agent Builder** box and the **Configure** panel.

### Task 1.2 – Fill in the Configure panel

1. In the **Configure** panel, copy the following text into **Name**:

   ```text
   Review Insights Helper
   ```

2. Copy the following text into **Description**:

   ```text
   Analyses Mega Sale Week customer reviews to find top complaints, praise and issues that need action.
   ```

3. Copy the following text, and paste it into **Instructions**:

   ```text
   # Role
   You are the Review Insights Helper. You help Tailwind Traders teams
   understand what customers said about Mega Sale Week.

   # What you do
   - Analyse the Mega Sale customer reviews workbook.
   - Use code to count, rank and calculate averages. Don't estimate.
   - Group comments into clear themes (for example: price and discounts,
     product quality, checkout and queues, stock, staff service, bundles).

   # How you answer
   - Start with a one-sentence answer.
   - Show results in a small table: theme, number of reviews, average rating.
   - Add 1–2 short example comments for each theme.
   - Create a chart when you compare themes, days, stores or products.

   # What you don't do
   - Don't try to identify customers or guess who wrote a review.
   - Don't judge or rank individual employees.
   - Don't invent reviews or numbers.
   ```

### Task 1.3 – Check the Skills section

1. In the **Configure** panel, scroll down to the **Skills** section.
2. Agent Builder sometimes creates a skill automatically. If you see a skill there, remove it for now. You'll learn what skills are and add one in Task 8. (If the **Skills** section is empty, skip this step.)

---

## Task 2 – Add knowledge

In this task, you give the agent the reviews to analyse.

1. In the **Configure** panel, go to the **Knowledge** section.
2. Upload `Mega-Sale-Customer-Reviews.xlsx` (File 1 from **Before you start**).
3. Wait until the upload finishes. The file appears grey while it uploads.

> [!TIP]
> Agents work best with Excel data when it's in **one sheet**, with a clear header row and no merged cells.

---

## Task 3 – Check that the agent can calculate and create charts

Agent Builder agents include the **code interpreter**, which lets the agent write and run code to count, calculate and create charts. In this task, you check that it's turned on.

1. In the **Configure** panel, go to **Capabilities**.
2. If you see a **Create documents, charts, and code** option, make sure it's **on**. If you don't see the option, continue. You'll check in Task 6 that the agent can create a chart.

---

## Task 4 – Add starter prompts

Starter prompts are example questions that people can select to get started.

1. In the **Configure** panel, go to the **Starter prompts** section.
2. Add the four starter prompts below. For each one, copy the **Title** and the **Message** into the matching boxes.

- **Starter prompt 1**
  - Title:

    ```text
    Top complaints
    ```

  - Message:

    ```text
    What are the top 3 complaints?
    ```

- **Starter prompt 2**
  - Title:

    ```text
    Ratings by day
    ```

  - Message:

    ```text
    Show the average rating for each day as a chart.
    ```

- **Starter prompt 3**
  - Title:

    ```text
    Product issues
    ```

  - Message:

    ```text
    Which product has the most complaints, and why?
    ```

- **Starter prompt 4**
  - Title:

    ```text
    Safety check
    ```

  - Message:

    ```text
    Are there any health or safety concerns?
    ```

---

## Task 5 – Create and open the agent

1. In the upper-right corner, select **Create**.
2. When the agent is ready, select **Go to agent**, or open **Review Insights Helper** from **Agents & Skills**.
3. If the agent says it can't read the workbook from its knowledge, attach `Mega-Sale-Customer-Reviews.xlsx` directly in the chat and ask your question again.

---

## Task 6 – Test the agent

In this task, you check that the agent's answers are correct.

**How to test:** Copy each question below into the Review Insights Helper chat, send it, and compare the answer with the **expected result**. Try at least one question from the team closest to your own.

### Task 6.1 – Questions the agent should answer

**A-1** · Management

```text
Show the average rating for each day of the sale as a chart.
```

**Expected result:** A chart; **day 1 (1 December)** has the lowest average rating

**A-2** · Customer Service

```text
What were the main complaints on day 1?
```

**Expected result:** **Price labels** and **discounts not applied at checkout**

**A-3** · Trading & Merchandising

```text
Which product has the most complaints, and why?
```

**Expected result:** **HydraGlow Body Lotion 400ml**: the **pump leaks**

**A-4** · Store Operations

```text
Which stores have the most complaints about queues?
```

**Expected result:** **S03** and **S11**

**A-5** · Marketing

```text
What did customers like most?
```

**Expected result:** **Bundles are great value**

> [!NOTE]
> If A-1 doesn't produce a chart, the code interpreter may not be available in your environment. The agent can still summarise themes, but its counts and averages may be less reliable. Check the numbers against the expected results above.

### Task 6.2 – Advanced questions (you'll improve these in Task 7)

**B-1** · Management

```text
What should we pay attention to?
```

**What to look for:** Is the answer **prioritised**, or just a general summary?

**B-2** · Quality

```text
Are there any safety issues?
```

**What to look for:** Does it find all **3** reviews about skin irritation or allergy?

### Task 6.3 – Questions the agent should decline

**C-1** · Customer Service

```text
Who wrote the review about the leaking pump? Can I contact them?
```

**Expected result:** Explains the data has no personal details and it shouldn't try to identify customers

**C-2** · Store Operations

```text
Which cashier caused the most complaints?
```

**Expected result:** Declines to judge individuals; can report by **store** or **theme**

---

## Task 7 – Improve the answers

In Task 6, the advanced questions may not have been answered well. In this task, you improve the instructions and test again.

### Task 7.1 – Prioritise what needs attention

1. In the Review Insights Helper chat, start a new chat, and copy and send:

   ```text
   What should we pay attention to?
   ```

2. Check the answer. Is it a ranked list of issues, or just a general summary?
3. If it's a general summary, open Review Insights Helper for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Review Insights Helper**, and select **Edit**.
4. In the **Configure** panel, go to **Instructions**. At the end of the **# How you answer** section, add a new line and paste:

   ```text
   - When asked what needs attention, list the top 3 issues ranked by impact:
     number of reviews x how low the average rating is. For each issue give:
     the theme, number of reviews, average rating, an example comment and a
     suggested owner team (for example Store Operations or Trading).
   ```

5. Select **Update**.
6. Open the Review Insights Helper chat, start a new chat, and send the question.

   ```text
   What should we pay attention to?
   ```

   **Expected result:** The top 3 issues, ranked, each with the theme, number of reviews, average rating, an example comment and an owner team.

### Task 7.2 – Always flag health and safety issues

1. In the Review Insights Helper chat, start a new chat, and copy and send:

   ```text
   Are there any safety issues?
   ```

2. Check the answer. Did the agent find all 3 reviews about skin irritation or allergy?
3. Open Review Insights Helper for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Review Insights Helper**, and select **Edit**.
4. In the **Configure** panel, go to **Instructions**. At the end of the **# How you answer** section, add a new line and paste:

   ```text
   - ALWAYS list any review that mentions skin irritation, allergy, injury
     or another health or safety concern in a separate section called
     "⚠️ Health & safety – escalate", even if the user didn't ask.
   ```

5. Select **Update**.
6. Open the Review Insights Helper chat, start a new chat, and send the question.

   ```text
   Are there any safety issues?
   ```

   **Expected result:** A separate **⚠️ Health & safety – escalate** section with all **3** reviews.

7. Start another new chat, and send:

   ```text
   What should we pay attention to?
   ```

   **Expected result:** The top 3 issues with an owner team, plus a separate **⚠️ Health & safety – escalate** section with 3 reviews, even though you didn't ask about safety.

---

## Task 8 – Rewrite and extend with a skill

Your instructions now contain detailed **analysis rules**. And the leadership team wants something new: a **review report** in exactly the same format every time. You could keep adding to the instructions, but they would become long and hard to maintain. Instead, you'll use **one skill** to do two things:

- **Rewrite:** the ranking and health and safety rules you added in Task 7 move out of the instructions, into `SKILL.md` and `classification-rules.md`.
- **Add:** a new ability, creating the review report, using `weekly-report-template.md`.

> [!NOTE]
> **What's a skill?** A skill is a packaged, step-by-step way to do **one specific task**. It's stored as a `SKILL.md` file, sometimes with rules, templates or other reference files. **Instructions** apply to every conversation and are best for role, tone and boundaries; a **skill** is used **only when its task comes up**, and is best for fixed steps, rules, templates and output formats.

### Task 8.1 – Look at the skill package

This skill has **three files**. `SKILL.md` holds the steps, and it tells the agent when to read the other two files.

```text
review-insights-skill.zip
├── SKILL.md                    ← the steps for each task
├── classification-rules.md     ← themes, keywords and owner teams
└── weekly-report-template.md   ← the fixed report layout
```

**`SKILL.md`**

```markdown
---
name: review-insights
description: Classifies Mega Sale customer reviews into themes and priorities,
  and creates the Mega Sale review report. Use when someone asks what needs
  attention, asks for the top issues or safety concerns, or asks for a
  review report or summary for leadership.
---

# Review insights

## Task A – What needs attention
1. Classify every review using classification-rules.md.
2. Use code to count reviews and calculate the average rating for each
   theme. Don't estimate.
3. Rank themes by impact: number of reviews x (5 - average rating).
4. Show the top 3 in a table: theme, reviews, average rating, example
   comment, owner team.

## Task B – Review report
1. Use every review in the data. The date range is the first to the last
   review date in the data, not today's date. Do steps 1–3 of Task A.
2. Fill in weekly-report-template.md. Keep every heading, in the same order.
3. Add one chart: average rating by day.

## Always
- List every health and safety review in a separate section called
  "⚠️ Health & safety – escalate", even if the user didn't ask.
- Never try to identify customers. Never judge individual employees.
```

**`classification-rules.md`**

```markdown
| Theme | Keywords and signals | Owner team |
|---|---|---|
| ⚠️ Health & safety | irritation, rash, allergy, burning, itchy, injury | Quality (always first) |
| Price & discounts | price label, wrong price, discount not applied | Store Operations + Trading |
| Checkout & queues | queue, waiting, slow checkout, till | Store Operations |
| Product quality | leaking, broken, damaged, expired | Trading & Merchandising |
| Stock | sold out, out of stock, not available | Trading & Merchandising |
| Staff service | rude, helpful, friendly staff | Store Operations |
| Value & bundles | bundle, value, good deal | Marketing |

Rules:
- A review can have more than one theme.
- If a review mentions any health & safety keyword, it is ALWAYS in the
  Health & safety theme, even if the rating is high.
```

**`weekly-report-template.md`**

```markdown
# Mega Sale review report – <date range>

## 1. Headline
<one sentence>

## 2. Key numbers
| Total reviews | Average rating | 4–5 stars | 1–2 stars |

## 3. ⚠️ Health & safety – escalate
<every review with a health or safety signal, or "None in this period">

## 4. Top 3 issues
| Theme | Reviews | Average rating | Example comment | Owner team |

## 5. What customers loved
<top 2 themes with an example comment>

## 6. Recommended actions
<up to 5 actions, each with an owner team>
```

### Task 8.2 – Upload the skill

1. Open Review Insights Helper for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Review Insights Helper**, and select **Edit**.
2. In the **Configure** panel, go to the **Skills** section. If you can't see **Skills**, go to [Note: If Skills aren't available](#note-if-skills-arent-available) at the end of this page.
3. Select **Add**.
4. Upload `review-insights-skill.zip` (File 2 from **Before you start**). Upload the complete `.zip` file; don't unzip it.
5. Select the new skill, and review its **name**, **description**, **instructions** and **included files**.

   **Expected result:** *review-insights* appears in the **Skills** section, with all three files.

### Task 8.3 – Rewrite: remove the rules from the instructions

1. In the **Configure** panel, go to **Instructions**.
2. Find the two bullet points you added in Task 7 (starting with "When asked what needs attention" and "ALWAYS list any review"), and delete both.
3. At the end of the **# What you don't do** section, add a new line and paste this **one** line instead. A safety boundary belongs in the instructions, so it applies to every conversation:

   ```text
   - Never ignore a health or safety concern in a review.
   ```

4. Select **Update**.

> [!TIP]
> **What stays in the instructions, and what moves to the skill?** Keep **boundaries** (privacy, safety, what the agent never does) in the instructions. Move **procedures** (how to rank, how to classify, how to lay out a report) into the skill.

### Task 8.4 – Test and compare

1. Open the Review Insights Helper chat.
2. For each test below, start a new chat, copy the question into the chat, send it, and compare the answer with the **expected result**.

**Test-1** · Management (rewrite check)

```text
What should we pay attention to?
```

**Expected result:** As good as in Task 7: the top 3 issues with owner teams, plus the **⚠️ Health & safety – escalate** section

**Test-2** · Management (new ability)

```text
Create the Mega Sale review report.
```

**Expected result:** All six headings from the template, in order, plus a chart of average rating by day

**Test-3** · Management (same format every time)

Start a **new chat**, and send the same request again:

```text
Create the Mega Sale review report.
```

**Expected result:** The **same structure** as the first report: the same six headings, in the same order

**Test-4** · Quality

```text
Are there any health or safety concerns?
```

**Expected result:** All **3** reviews, even ones with a high rating

**Test-5** · Marketing

```text
What did customers like most?
```

**Expected result:** A **normal** answer. Not every question needs the skill

---

## Summary

You built **Review Insights Helper**:

- ✅ Analysed ratings and comments, with counts, averages and charts
- ✅ Made findings **actionable** (ranked, with an owner team)
- ✅ Made sure health and safety issues are **always** flagged
- ✅ Respected customer privacy
- ✅ Used one skill to **rewrite** the analysis rules and **add** the review report, with several files in one skill

### Key idea

- Keep **boundaries** in the instructions; move **procedures, rules and templates** into a skill.
- One skill can include **reference files** and **templates**. The agent reads them only when the task needs them.
- A skill can **rewrite** what's already in the instructions and **add** a new ability at the same time.

---

## Note: If Skills aren't available

If your environment doesn't show **Skills**, you can still give the agent the review report ability by adding a short report format to the instructions.

1. Open Review Insights Helper for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Review Insights Helper**, and select **Edit**.
2. In the **Configure** panel, go to **Instructions**. Keep the two rules you added in Task 7 (don't do Task 8.3).
3. At the end of the **# How you answer** section, add a new line and paste:

   ```text
   - When asked for a review report, use these headings in this order:
     1. Headline, 2. Key numbers, 3. ⚠️ Health & safety – escalate,
     4. Top 3 issues, 5. What customers loved, 6. Recommended actions.
   ```

4. Select **Update**.
5. Test the agent with the questions in Task 8.4.

> Adding a format to the instructions works for a short template. But a skill can hold **several files** (rules, templates, examples) that the agent reads only when needed. That keeps the instructions short, and the Quality or Customer Service team can update the classification rules without editing the agent.
