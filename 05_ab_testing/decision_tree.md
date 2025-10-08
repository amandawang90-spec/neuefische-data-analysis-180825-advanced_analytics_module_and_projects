# 🧭 Hypothesis Testing Decision Tree
## Step 1️⃣: What type of variable are you testing?

| Variable Type                      | Typical Example                            | Go To... |
| ---------------------------------- | ------------------------------------------ | -------- |
| **Continuous (numeric)**           | Sales amount, weight, time, AOV            | → Step 2 |
| **Categorical (proportion/count)** | Click vs no-click, purchase vs no-purchase | → Step 4 |


## Step 2️⃣: Continuous Data — Are you testing means?

✅ Yes → You are dealing with a mean comparison test (Z-test or T-test).
Now ask:

➤ Do you know the population standard deviation (σ)?

- Yes → use Z-test

- No → use T-test

## Step 3️⃣: How many samples do you have?
| Scenario                  | Description                                      | Test                             | Example                              |
| ------------------------- | ------------------------------------------------ | -------------------------------- | ------------------------------------ |
| **1 sample**              | Compare a sample mean to a known population mean | 1-sample Z or T test             | “Is the average temperature = 75°C?” |
| **2 independent samples** | Compare two unrelated groups                     | Independent 2-sample T or Z test | “AOV: Control vs Variant”            |
| **2 related samples**     | Same group measured twice (before/after)         | Paired-sample T-test             | “Sales before vs after campaign”     |


## Step 4️⃣: Categorical / Proportion Data

Ask:

➤ Are you testing one group’s proportion vs a benchmark?

✅ Use 1-proportion Z-test
Example: “Is conversion rate different from 5%?”

➤ Are you comparing two proportions (e.g. A/B test on conversion)?

✅ Use 2-proportion Z-test
Example: “Did the new landing page increase sign-ups?”

➤ More than two categories or groups?

✅ Use Chi-square test of independence
Example: “Is purchase rate related to device type?”

## Step 5️⃣: For special data types

| Data Type                          | Typical Scenario              | Test                                          |
| ---------------------------------- | ----------------------------- | --------------------------------------------- |
| **Count data (discrete events)**   | # of calls/hour, # of defects | Poisson test                                  |
| **Skewed continuous / non-normal** | Response times, income        | Nonparametric test (Mann–Whitney U, Wilcoxon) |
| **Correlations / relationships**   | Two continuous variables      | Pearson’s r or Spearman’s rho test            |




## Hypothesis Test Selection Guide (Decision Tree)

```text
                ┌───────────────┐
                │ Type of Data? │
                └───────┬───────┘
                        │
        ┌───────────────┴───────────────┐
        │                               │
  Continuous (Means)             Categorical (Proportions)
        │                               │
        ▼                               ▼
Do you know σ?                    1 or 2 groups?
   │                                │
   ├── Yes → Z-test                 ├── 1 → 1-proportion Z-test
   └── No  → T-test                 └── 2 → 2-proportion Z-test
        │
        ▼
How many samples?
   │
   ├── 1 → One-sample test
   ├── 2 independent → Two-sample (independent) test
   └── 2 related → Paired-sample test

```

