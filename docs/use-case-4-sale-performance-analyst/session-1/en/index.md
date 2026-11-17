# Use Case 4 – Sale Performance Analyst (Session 1)

## About this lab

You're working at **Tailwind Traders**, a simulated health and beauty retailer.

> Tailwind Traders is a fictitious company. Read the [company introduction](../../../../README.md).

In this lab, you analyse the same sales data with the same three questions in three rounds: first with **Analyst**, the data agent built into Microsoft 365 Copilot; then with **Sale Performance Analyst**, your own agent that knows Tailwind Traders' business rules; and finally with your agent plus a **skill** that creates a daily flash report. Analyst can analyse any file, but it doesn't know your business rules, so at the end you compare the results and decide which tool fits which job.

- **Tool:** Analyst (built-in agent) and Agent Builder in Microsoft 365 Copilot
- **Estimated time:** 60–75 minutes
- **Level:** Beginner to intermediate, no code

You'll learn how to:

- Use the built-in **Analyst** agent to analyse an Excel file
- Build your own analysis agent with **business rules** in its instructions, and check its numbers yourself
- **Add a skill** that creates a fixed-format daily report
- Compare the three approaches and choose the right one for each job

---

## Before you start

Check each item before the lab. Select a file name to download it, and save the files somewhere easy to find, such as your desktop.

> All files are **fictional** and created for training only.

| ✔ | What you need | Details |
|---|---|---|
| ☐ | A work account with access to **Microsoft 365 Copilot Chat** | Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and check that you can open **Copilot** |
| ☐ | Access to **Analyst** in Microsoft 365 Copilot | In Copilot, look for **Analyst** under **Agents** in the left pane. Analyst needs a Microsoft 365 Copilot licence, and there may be a monthly limit on how many Analyst questions you can ask. This lab uses **three** Analyst questions. |
| ☐ | Access to **Agent Builder** in Microsoft 365 Copilot | In Copilot, check that you can see **Agents & Skills** > **New agent** |
| ☐ | File 1 (Excel): [`Mega-Sale-Sales-Data.xlsx`](Mega-Sale-Sales-Data.xlsx) | The sales data you analyse in every round. One sheet: Date, Store Code, Region, Category, Product, Units Sold, Sales Amount, Stock Remaining |
| ☐ | File 2 (Skill package, .zip): [`daily-flash-report-skill.zip`](daily-flash-report-skill.zip) | A ready-made skill (`SKILL.md` and `flash-report-template.md`) for Task 5. **Don't unzip it.** |

---

## Scenario

Every morning, a sales analyst at Tailwind Traders spends about **an hour** building a sales summary for Management from the sales spreadsheet. Every store's sales are recorded in that one spreadsheet. Many people need answers from it, but few have time to analyse it.

It gets even busier during **Mega Sale Week**, held every year from 1 to 7 December. Management wants the sales summary every morning, plus the best day, the top products and the stores that are running out of stock. Other teams ask too:

- *"What were total sales for each day of the sale?"*
- *"Which 5 products sold the most units?"*
- *"Which stores are about to run out of stock?"*
- *"Which category grew most after day 1?"*
- *"Give me today's flash report."*

Microsoft 365 Copilot already has a built-in data agent called **Analyst**. So why build your own? To find the right tool for each job, Tailwind Traders wants to compare Analyst with its own agent, using **the same file** and **the same three questions**.

**Your task:**

1. Analyse the sales data with the built-in **Analyst**.
2. Build **Sale Performance Analyst**, your own agent that already has the data and knows Tailwind Traders' business rules.
3. Give your agent a **skill** that creates the daily flash report in a fixed format.
4. Compare the results, and decide which tool fits which job.

---

## Task 1 – Get to know the data

Before you ask any AI tool about data, know what's in the file. It's the only way to tell whether an answer is right.

### Task 1.1 – Look at the data

1. Open `Mega-Sale-Sales-Data.xlsx` (File 1 from **Before you start**) in Excel.
2. Look at the columns:

   | Column | Meaning |
   |---|---|
   | **Date** | The sale day (1–7 December) |
   | **Store Code** | The store, for example S03 |
   | **Region** | The region the store belongs to |
   | **Category** | Product category, for example Skincare |
   | **Product** | Product name |
   | **Units Sold** | Units sold that day |
   | **Sales Amount** | Sales value that day, after discounts |
   | **Stock Remaining** | Units left in that store at the end of that day |

3. Notice what's **not** in the file: there's no cost or margin information.

### Task 1.2 – Note the business rules

Tailwind Traders' **business rules** aren't written in the file. Note them:

- **Low stock** means **fewer than 20 units** remaining in a store.
- For stock questions, use the **most recent date** in the data.
- Management wants **the conclusion first**, then the numbers.

### Task 1.3 – Read the three test questions

You'll ask the same three questions in every round.

**Q1** · Finance

```text
Which day and which category performed best during the sale?
```

**Q2** · Store Operations

```text
Which products are at risk of running out of stock?
```

**Q3** · Management

```text
Summarise Mega Sale performance for the management team.
```

Use these key facts to check the answers in every round:

<details>
<summary><strong>Key facts in the data</strong></summary>

| Pattern in the data | Fictional value |
|---|---|
| Highest sales day | Day 1 (1 December) |
| Top category by sales | Skincare |
| Top-5 product by units | HydraGlow Hydrating Toner 200ml |
| Low stock (< 20 units) | HydraGlow Hydrating Toner in three stores |
| Lowest total sales | Store S07 |
| Not in the data | Cost or margin information |

</details>

### Task 1.4 – Prepare your comparison sheet

Copy this table into a note or a Word document. You'll fill it in as you go.

| | Round 1: Analyst | Round 2: Your agent | Round 3: Agent + skill |
|---|---|---|---|
| Are the numbers correct? (check the key facts) | | | |
| Does Q2 use the "fewer than 20 units" rule? | | | |
| Does Q3 lead with the conclusion? | | | |
| How long did each answer take? | | | |
| What did you have to explain or upload first? | | | |
| Could a colleague get the same result easily? | | | |

---

## Task 2 – Round 1: Use the built-in Analyst

In this task, you ask Analyst the three test questions about the sales file, without any extra context.

### Task 2.1 – Open Analyst and attach the file

1. Go to [https://m365.cloud.microsoft](https://m365.cloud.microsoft) and open **Copilot**.
2. In the left pane, under **Agents**, select **Analyst**. If you don't see it, select **All agents** and search for **Analyst**. If you can't open Analyst, go to [Note: If Analyst isn't available](#note-if-analyst-isnt-available) at the end of this page.
3. In the message box, select **+**, and then attach or upload `Mega-Sale-Sales-Data.xlsx` from your computer.

### Task 2.2 – Ask the three questions

1. Copy **Q1** and send it. Don't add any extra context.

   ```text
   Which day and which category performed best during the sale?
   ```

2. Watch Analyst work. It shows its **reasoning steps**, and you can open the **code** it runs to check its work.
3. When the answer is complete, copy **Q2** and send it in the same conversation:

   ```text
   Which products are at risk of running out of stock?
   ```

4. When the answer is complete, copy **Q3** and send it in the same conversation:

   ```text
   Summarise Mega Sale performance for the management team.
   ```

5. Fill in the **Round 1** column of your comparison sheet.

**What to notice:**

- Analyst is **thorough**: it plans, writes code and checks its work. This can take a minute or more.
- For **Q2**, Analyst doesn't know Tailwind Traders' **"fewer than 20 units"** rule, so it may use its own definition of "at risk".
- For **Q3**, the format is Analyst's own choice. Ask again in a new chat and the layout may be different.
- Next time, you'll need to **attach the file and explain the rules again**.

> [!TIP]
> To get a better answer from Analyst, you could add the business rules to your question, for example: *"Low stock means fewer than 20 units."* That works, but **every person** would have to remember to do it **every time**. Keep this in mind for Round 2.

---

## Task 3 – Round 2: Build your own Sale Performance Analyst

In this task, you create your own agent by filling in the **Configure** panel, and give it the data and the business rules that Analyst didn't know.

### Task 3.1 – Open Agent Builder

1. In Copilot, select **Agents & Skills**, and then select **New agent**.
2. A screen opens with the **Message Agent Builder** box and the **Configure** panel. In this lab, you fill in the **Configure** panel.

### Task 3.2 – Fill in the Configure panel

1. In the **Configure** panel, copy the following text into **Name**:

   ```text
   Sale Performance Analyst
   ```

2. Copy the following text into **Description**:

   ```text
   Analyses Mega Sale Week sales data using Tailwind Traders' business rules, with charts and plain-language summaries.
   ```

3. Copy the following text, and paste it into **Instructions**. Notice the **Business rules** section: this is what Analyst didn't know.

   ```text
   # Role
   You are the Sale Performance Analyst. You help people understand Mega Sale
   Week sales results.

   # What you do
   - Analyse the Mega Sale sales data workbook.
   - Use code to calculate totals, averages, rankings and trends. Don't
     estimate numbers.
   - Create a chart when it helps (bar chart for comparisons, line chart for
     trends over days).

   # Business rules
   - Mega Sale Week runs from 1 to 7 December. Day 1 is 1 December.
   - "Low stock" means fewer than 20 units remaining in a store. For stock
     questions, use the most recent date in the data.
   - Sales Amount is after discounts. The data has no cost or margin
     information.

   # How you answer
   - Start with the conclusion in one sentence, then show the key numbers in
     a small table.
   - Add up to 3 bullet points: "What this means".
   - State any assumptions (for example, which days or stores you included).
   - Round money and units to whole numbers.

   # What you don't do
   - Don't make up data that isn't in the workbook. If the data can't answer
     the question (for example, margin), say so.
   - Don't judge or rank individual employees.
   ```

4. Scroll down to the **Skills** section. If Agent Builder created a skill automatically, remove it for now. You'll add a skill in Task 5. (If the **Skills** section is empty, skip this step.)

### Task 3.3 – Add the data and check charts

1. In the **Configure** panel, go to the **Knowledge** section.
2. Upload `Mega-Sale-Sales-Data.xlsx` (File 1 from **Before you start**), and wait until the upload finishes.
3. Agent Builder agents include the **code interpreter**, which lets the agent calculate and create charts. If you see a **Create documents, charts, and code** option in **Capabilities**, make sure it's **on**.

> [!TIP]
> Agents work best with Excel data when it's in **one sheet**, with a clear header row and no merged cells.

### Task 3.4 – Add starter prompts

1. In the **Configure** panel, go to the **Starter prompts** section.
2. Add the three starter prompts below. For each one, copy the **Title** and the **Message** into the matching boxes.

- **Starter prompt 1**
  - Title:

    ```text
    Best day and category
    ```

  - Message:

    ```text
    Which day and which category performed best during the sale?
    ```

- **Starter prompt 2**
  - Title:

    ```text
    Low stock
    ```

  - Message:

    ```text
    Which products are at risk of running out of stock?
    ```

- **Starter prompt 3**
  - Title:

    ```text
    Management summary
    ```

  - Message:

    ```text
    Summarise Mega Sale performance for the management team.
    ```

### Task 3.5 – Create and open the agent

1. In the upper-right corner, select **Create**.
2. When the agent is ready, select **Go to agent**, or open **Sale Performance Analyst** from **Agents & Skills**.

> [!NOTE]
> If the agent says it can't read the workbook from its knowledge, attach `Mega-Sale-Sales-Data.xlsx` directly in the chat and ask your question again.

---

## Task 4 – Round 2: Test your agent

In this task, you ask your agent the same three questions, and check one number yourself.

### Task 4.1 – Ask the three questions

You don't need to attach a file or explain any rules.

1. In the Sale Performance Analyst chat, select the **Best day and category** starter prompt, or copy and send **Q1**:

   ```text
   Which day and which category performed best during the sale?
   ```

   **Expected result:** **1 December** and **Skincare**.

2. Select the **Low stock** starter prompt, or copy and send **Q2**:

   ```text
   Which products are at risk of running out of stock?
   ```

   **Expected result:** Uses **fewer than 20 units**; finds **HydraGlow Hydrating Toner** in **three stores**.

3. Select the **Management summary** starter prompt, or copy and send **Q3**:

   ```text
   Summarise Mega Sale performance for the management team.
   ```

   **Expected result:** Starts with the conclusion, then a small table and up to 3 "What this means" points.

4. Fill in the **Round 2** column of your comparison sheet.

### Task 4.2 – Check the numbers yourself

AI can make mistakes with data, so always check one number yourself.

1. In Excel, filter **Date** to 1 December and check the total of **Sales Amount**.
2. In the Sale Performance Analyst chat, copy and send:

   ```text
   What were total sales on 1 December?
   ```

3. Compare the two numbers. If they're different, copy and send:

   ```text
   Show me the code you used.
   ```

---

## Task 5 – Round 3: Add a skill for the daily flash report

Every morning during the sale, Management wants a **flash report** for the previous day. It needs to look **exactly the same every day**, so it can be compared and forwarded quickly. That's a repeatable task with a fixed format, so instead of making the instructions longer, you'll add it as a **skill**.

> [!NOTE]
> **What's a skill?** A skill is a packaged, step-by-step way to do **one specific task**. It's stored as a `SKILL.md` file, sometimes with templates or reference files. **Instructions** apply to every conversation; a **skill** is used **only when its task comes up**, and follows the same steps every time.

### Task 5.1 – Look at the skill package

The ready-made skill in `daily-flash-report-skill.zip` contains two files:

```text
daily-flash-report-skill.zip
├── SKILL.md                  ← the steps
└── flash-report-template.md  ← the fixed report layout
```

**`SKILL.md`**

```markdown
---
name: daily-flash-report
description: Creates the Mega Sale daily flash report for one sale day, in a
  fixed format for Management. Use when someone asks for a flash report,
  daily report or daily summary for one sale day, or for the latest day
  in the data.
---

# Daily flash report

## Steps
1. Confirm the report date. If the user didn't give one, or asked for
   "today" or "yesterday", use the most recent date in the data and say
   so. If the date isn't in the data, list the dates that are.
2. Use code for every number. Don't estimate.
3. Compare the report date with the day before (skip this for 1 December).
4. Fill in flash-report-template.md. Keep every heading, in the same order.
5. Add one bar chart: sales by category for the report date.

## Rules
- Low stock = fewer than 20 units remaining in a store on the report date.
- Show money and units as whole numbers.
- Keep the text (not counting tables and the chart) under 150 words.
- If something can't be calculated from the data, write "Not in the data".
```

**`flash-report-template.md`**

```markdown
# ⚡ Mega Sale flash report – <date> (Day <n> of 7)

## Headline
<one sentence: the most important result of the day>

## Key numbers
| Total sales | vs previous day | Units sold | Top category |

## Top 5 products (by units)
| # | Product | Units | Sales |

## 🚨 Low-stock alert (fewer than 20 units)
| Store | Product | Units left |

## Actions for today
<up to 3 actions, each with an owner team>
```

### Task 5.2 – Upload the skill

1. Open Sale Performance Analyst for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Sale Performance Analyst**, and select **Edit**.
2. In the **Configure** panel, go to the **Skills** section. If you can't see **Skills**, go to [Note: If Skills aren't available](#note-if-skills-arent-available) at the end of this page.
3. Select **Add**.
4. Upload `daily-flash-report-skill.zip` (File 2 from **Before you start**). Upload the complete `.zip` file; don't unzip it.
5. Select the new skill, and review its **name**, **description**, **instructions** and included files.
6. Optional: in the **Starter prompts** section, add one more starter prompt:
   - Title:

     ```text
     Flash report
     ```

   - Message:

     ```text
     Create the flash report for 2 December.
     ```

7. Select **Update**.

### Task 5.3 – Test the skill

1. Open the Sale Performance Analyst chat and start a new chat.
2. Copy each request below into the chat, send it, and compare the answer with the **expected result**.

**Test-1** · Management

```text
Create the flash report for 2 December.
```

**Expected result:** All five headings from the template; a comparison with 1 December; a category chart

**Test-2** · Management

```text
Create the flash report for 5 December.
```

**Expected result:** **Exactly the same layout** as Test-1: the same five headings in the same order, and the low-stock alert uses **fewer than 20 units**

**Test-3** · Store Operations

```text
Create the latest flash report.
```

**Expected result:** Uses the **most recent date** in the data (7 December) and says so

**Test-4** · Finance

```text
Which day and which category performed best during the sale?
```

**Expected result:** A **normal** answer (Q1), not a flash report. The skill is used only when it's needed

3. Copy **Q3** and send it again:

   ```text
   Summarise Mega Sale performance for the management team.
   ```

4. Fill in the **Round 3** column of your comparison sheet.

---

## Task 6 – Compare and decide

In this task, you use your comparison sheet to decide which tool fits which job.

1. Look at your completed comparison sheet.
2. Discuss with a colleague: **which tool would you use for which job?**
3. Compare your ideas with what most people find.

<details>
<summary><strong>What most people find</strong></summary>

| | Analyst (built-in) | Your own agent | Your agent + skill |
|---|---|---|---|
| **Best for** | One-off, deep analysis of **any** file | Everyday questions about **the same data**, by many people | A **repeatable output** in a fixed format |
| **Setup** | None | About 15 minutes, once | A few more minutes, once |
| **Business rules** | You explain them every time | Built into the instructions | Built in, plus a fixed procedure |
| **Consistency** | The answer layout can change between chats | The same rules every time | The same rules **and** the same layout every time |
| **Speed** | Slower; reasons step by step | Faster for routine questions | Fast; follows the template |
| **Sharing** | Everyone sets it up themselves | Share the agent once | Share the agent once; the skill can be reused in other agents |
| **Licence** | Needs a Microsoft 365 Copilot licence; may have a monthly limit | Depends on your organisation's Copilot and agent settings | Same as your own agent |

**In short:**

- Use **Analyst** when you have a new or unusual question about any data and want deep reasoning.
- Build **your own agent** when many people ask the same kinds of questions about the same data and need the same business rules.
- **Add a skill** when there's a task that must produce the same output every time, like a daily report.

</details>

---

## Summary

You built **Sale Performance Analyst**:

- ✅ Used the built-in **Analyst** to analyse an Excel file
- ✅ Built the **Sale Performance Analyst** with Tailwind Traders' business rules
- ✅ **Added a skill** that creates the daily flash report in a fixed format
- ✅ Checked the numbers yourself
- ✅ Compared the three approaches and decided which one fits which job

### Key idea

- **Analyst** is a powerful general-purpose analyst, but it doesn't know your business rules.
- **Your own agent** knows your data and your rules, so everyone gets consistent answers without extra explanation.
- **A skill** turns a repeatable task into the same output every time.

---

## Note: If Analyst isn't available

If you can't open Analyst, you can still complete Round 1 from a typical result.

1. Read the typical Analyst result below:
   - **Q1:** Correctly finds that **1 December** was the best day and **Skincare** the top category. It shows a chart and the Python code it used.
   - **Q2:** Lists products with low stock, but uses its own threshold (for example, the lowest 10% of stock or "less than 2 days of sales"). It may not match the **fewer than 20 units** rule.
   - **Q3:** A detailed, well-written analysis, but long. The structure is Analyst's choice, and it changes between chats.
   - **Effort:** You had to attach the file. To apply the business rules, you'd need to type them into every question.
2. Fill in the **Round 1** column of your comparison sheet from this result.
3. Continue with Task 3.

> Analyst needs a Microsoft 365 Copilot licence, and there may be a monthly limit on how many Analyst questions you can ask. Your own agent doesn't need Analyst, so you can do Rounds 2 and 3 without it.

---

## Note: If Skills aren't available

If your environment doesn't show **Skills**, you can still give the agent the flash report ability by adding the steps to the instructions.

1. Open Sale Performance Analyst for editing: in Copilot, select **Agents & Skills**, open the **…** menu next to **Sale Performance Analyst**, and select **Edit**.
2. In the **Configure** panel, go to **Instructions**.
3. Go to the end of the instructions, add a new line, and paste:

   ```text
   # Daily flash report
   When asked for a flash report or daily report for a date, use these
   headings in this order: Headline, Key numbers (total sales, vs previous
   day, units sold, top category), Top 5 products by units, Low-stock alert
   (fewer than 20 units), Actions for today (up to 3, with an owner team).
   Add one bar chart of sales by category. Keep the text under 150 words.
   ```

4. Select **Update**.
5. Test the agent with Test-1 to Test-4 in Task 5.3.

> Adding steps to the instructions works, but every rule you add makes the instructions longer, and instructions apply to **every** conversation. A skill keeps the main instructions short, is used **only** when it's needed, and can be reused in other agents.
