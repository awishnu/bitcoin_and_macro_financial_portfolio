# PHASE 1: DATA PREPARATION & CLEANING

## Step 1: Filter the Date Range (2015-2024)

We need to isolate data from 2015 to 2024.

1. Make a copy of dataset
2. Select the entire dataset, including headers (e.g., A1:I3500).
3. Go to the **Data** tab in the Excel ribbon and click **Filter**. Small dropdown arrows will appear in your header cells.
4. Click the dropdown arrow in the date column (Column A) and filter it from 1/1/2015 to 12/31/2024.

Your view is now filtered to the 2015-2024 period. **It is crucial to keep this filter applied for all subsequent steps.**

## Step 2: Calculate Daily Returns for Each Asset

We will create new columns for the daily percentage return of each asset. The formula is: (Today's Price / Yesterday's Price) - 1

1. **Insert Headers:**
   - In cell J1, type: btc_daily_ret
   - In cell K1, type: gold_daily_ret
   - In cell L1, type: vix_daily_ret
   - In cell M1, type: usd_daily_ret

2. **Calculate Returns:**
   - In cell J3, enter the formula for the Bitcoin daily return:
     ```
     =IF(B3="", "", (B3/B2)-1)
     ```
     *This formula calculates the return and also checks if the current row's price is empty to avoid errors.*
   - Copy this formula from J3 down to the bottom of your filtered data.
   - In cell K3, enter the formula for the Gold daily return:
     ```
     =IF(C3="", "", (C3/C2)-1)
     ```
   - Copy this formula down.
   - In cell L3, enter the formula for the VIX daily return:
     ```
     =IF(D3="", "", (D3/D2)-1)
     ```
   - Copy this formula down.
   - In cell M3, enter the formula for the USD daily return:
     ```
     =IF(E3="", "", (E3/E2)-1)
     ```
   - Copy this formula down.

---

# PHASE 2: UNIVARIATE ANALYSIS (Understanding Each Asset)

## Step 1: Create a Summary Statistics Table

Let's create a clean table to summarize each asset's performance. We'll place it in a clear area, for example, starting at cell **O1**.

1. **Set up the Table Headers:**
   - In cell O1, type: Metric
   - In cell P1, type: Bitcoin
   - In cell Q1, type: Gold
   - In cell R1, type: VIX
   - In cell S1, type: USD

2. **List the Metrics:** In cells O2 to O8, enter the following:
   - O2: Total Return (%)
   - O3: Annualized Return (%)
   - O4: Annualized Volatility (%)
   - O5: Sharpe Ratio (Rf=0%)
   - O6: Max Drawdown (%)
   - O7: Best Day (%)
   - O8: Worst Day (%)

3. **Calculate the Metrics for Bitcoin (Column P):**
   - **P2 - Total Return:** This is the total gain if you bought and held for the entire period.
     ```
     =PRODUCT(1+J:J)-1
     ```
     *Format this cell as a Percentage with 2 decimal places.*
   - **P3 - Annualized Return:** This converts the total return to an average annual rate.
     ```
     =(1+P2)^(1/((MAX(A:A)-MIN(A:A))/365.25))-1
     ```
     *Format as Percentage.*
   - **P4 - Annualized Volatility:** The standard deviation of returns, scaled to a yearly figure. This is a key measure of risk.
     ```
     =STDEV.P(J:J)*SQRT(252)
     ```
     *Format as Percentage.*
   - **P5 - Sharpe Ratio (Assuming 0% Risk-Free Rate):** Measures risk-adjusted return. Higher is better.
     ```
     =P3/P4
     ```
   - **P6 - Max Drawdown:** The largest peak-to-trough decline. This measures the worst loss.
     First, we need to create a "Wealth Index" in a new column. Let's use column T.
     - In T1, type: BTC_Wealth_Index
     - In T2, enter: 100 (This means we start with $100).
     - In T3, enter the formula: =T2*(1+J3)
     - Copy this formula down to the bottom of your data.
     Now, create a "Running Max" column in column U.
     - In U1, type: BTC_Running_Max
     - In U2, enter: =T2
     - In U3, enter: =MAX(T3, U2)
     - Copy this formula down.
     Finally, create a "Drawdown" column in column V.
     - In V1, type: BTC_Drawdown
     - In V3, enter: =T3/U3-1
     - Copy this formula down.
     Now, in cell P6, we can calculate the Max Drawdown:
     ```
     =MIN(V:V)
     ```
     *Format as Percentage.*
   - **P7 - Best Day:**
     ```
     =MAX(J:J)
     ```
     *Format as Percentage.*
   - **P8 - Worst Day:**
     ```
     =MIN(J:J)
     ```
     *Format as Percentage.*

4. **Copy Formulas for Other Assets:**
   - Now, copy the entire block of formulas from P2:P8.
   - Paste it into cell Q2 for **Gold**. You must **manually adjust the column references** in each formula in column Q:
     - Q2: =PRODUCT(1+K:K)-1
     - Q3: =(1+Q2)^(1/((MAX(A:A)-MIN(A:A))/365.25))-1
     - Q4: =STDEV.P(K:K)*SQRT(252)
     - ... and so on, changing J to K.
   - Repeat this process for **VIX** in column R (change references to column L) and **USD** in column S (change references to column M).
   - You will also need to create Wealth Index, Running Max, and Drawdown columns for Gold, VIX, and USD (e.g., columns W, X, Y for Gold, etc.).

## Step 2: Create Visualization Charts

1. **Normalized Price Chart (Growth of $100):**
   - We already created the BTC_Wealth_Index in column T. Do the same for the other assets in the next columns (e.g., Gold_Wealth_Index in W, VIX_Wealth_Index in Z, USD_Wealth_Index in AC). Use the same method: start at 100 and multiply by (1 + daily return).
   - Select the columns containing the dates (A) and all four "Wealth_Index" columns (T, W, Z, AC).
   - Go to the **Insert** tab -> **Charts** -> **Line Chart**.
   - Title the chart: "Asset Performance Comparison (Normalized to $100)".
   - This chart will powerfully show how an initial $100 investment in each asset would have grown over time.

2. **Drawdown Chart:**
   - Select the dates (A) and all the "Drawdown" columns you created (V, Y, AB, AE).
   - Go to **Insert** -> **Charts** -> **Line Chart**.
   - Title the chart: "Asset Drawdowns Over Time".
   - This chart shows the pain periods for each investment.

---

# PHASE 3: MULTIVARIATE ANALYSIS (Relationships & Portfolio Construction)

## Step 1: Create a Correlation Matrix

This shows how the assets' daily returns move in relation to each other. Low or negative correlation is good for diversification.

1. **Set up the Matrix:**
   - Let's place this in a new area, e.g., starting at cell **O10**.
   - In cell O10, type: Correlation Matrix
   - In cells O11, P11, Q11, R11, enter: BTC, Gold, VIX, USD
   - In cells N12, N13, N14, N15, enter the same: BTC, Gold, VIX, USD

2. **Calculate Correlations using the CORREL function:**
   - In cell P12 (BTC vs. Gold), enter the formula: =CORREL(J:J, K:K)
   - In cell Q12 (BTC vs. VIX): =CORREL(J:J, L:L)
   - In cell R12 (BTC vs. USD): =CORREL(J:J, M:M)
   - In cell Q13 (Gold vs. VIX): =CORREL(K:K, L:L)
   - In cell R13 (Gold vs. USD): =CORREL(K:K, M:M)
   - In cell R14 (VIX vs. USD): =CORREL(L:L, M:M)
   - The diagonal (e.g., BTC vs BTC in O12) should be 1. You can manually type 1.00 in cells O12, P13, Q14, R15.

3. **Apply Conditional Formatting:**
   - Select the range O12:R15.
   - Go to **Home** -> **Conditional Formatting** -> **Color Scales**. Choose a scale where red is -1 and green is +1 (or vice-versa, depending on your version). This will visually highlight the relationships.

## Step 2: Define Portfolio Allocations

We will simulate three different portfolios with different risk profiles.

1. **Set up the Portfolio Table:**
   - Let's place this below the correlation matrix, e.g., starting at cell **O17**.
   - In cell O17, type: Portfolio Allocations
   - In cells O18, P18, Q18, R18, enter: BTC, Gold, VIX, USD
   - In cells N19, N20, N21, enter the portfolio names:
     - N19: Conservative
     - N20: Balanced
     - N21: Aggressive

2. **Define the Weights:** Enter the allocation for each asset in each portfolio. The weights for each row must add up to 100%.
   - **Conservative Portfolio (Row 19):** Low risk, less Bitcoin.
     - O19 (BTC): 0.10 (10%)
     - P19 (Gold): 0.50 (50%)
     - Q19 (VIX): 0.10 (10%) *Note: VIX is complex; we treat it as a small hedge.*
     - R19 (USD): 0.30 (30%)
     - In S19, add a check formula: =SUM(O19:R19). It should equal 1.
   - **Balanced Portfolio (Row 20):** Medium risk.
     - O20 (BTC): 0.30 (30%)
     - P20 (Gold): 0.40 (40%)
     - Q20 (VIX): 0.10 (10%)
     - R20 (USD): 0.20 (20%)
     - Check in S20: =SUM(O20:R20)
   - **Aggressive Portfolio (Row 21):** High risk, high Bitcoin allocation.
     - O21 (BTC): 0.60 (60%)
     - P21 (Gold): 0.20 (20%)
     - Q21 (VIX): 0.10 (10%)
     - R21 (USD): 0.10 (10%)
     - Check in S21: =SUM(O21:R21)

## Step 3: Calculate Daily Portfolio Returns

Now we will calculate the daily performance of each portfolio based on the weights above.

1. **Insert Headers for Portfolio Returns:**
   - In your main data sheet, let's use columns AF, AG, AH.
   - In cell AF1, type: Conservative_Ret
   - In cell AG1, type: Balanced_Ret
   - In cell AH1, type: Aggressive_Ret

2. **Calculate the Daily Returns:**
   - The formula for a portfolio's daily return is the weighted sum of its assets' returns.
   - In cell AF3, enter the formula for the Conservative portfolio:
     ```
     =$O$19*J3 + $P$19*K3 + $Q$19*L3 + $R$19*M3
     ```
     *The **$** signs lock the cell references to the weights we defined.*
   - In cell AG3, enter the formula for the Balanced portfolio:
     ```
     =$O$20*J3 + $P$20*K3 + $Q$20*L3 + $R$20*M3
     ```
   - In cell AH3, enter the formula for the Aggressive portfolio:
     ```
     =$O$21*J3 + $P$21*K3 + $Q$21*L3 + $R$21*M3
     ```
   - Copy these three formulas down to the bottom of your dataset.

## Step 4: Calculate Portfolio Performance

We will now create a "Wealth Index" for each portfolio, just like we did for the individual assets.

1. **Create Portfolio Wealth Index Columns:**
   - In columns AI, AJ, AK, create the headers: Conservative_Value, Balanced_Value, Aggressive_Value.
   - In cell AI2, AJ2, AK2, set the starting value: 100 ($100 initial investment).
   - In cell AI3, enter the formula: =AI2*(1+AF3)
   - In cell AJ3, enter: =AJ2*(1+AG3)
   - In cell AK3, enter: =AK3*(1+AH3)
   - Copy these formulas down to the bottom of your data.

## Step 5: Create the Final Performance Comparison

1. **Summary Statistics for Portfolios:**
   - Extend your **Summary Statistics Table** from Phase 2 (the one in O1:S8) to include the portfolios.
   - In cells T1, U1, V1, add the headers: Conservative, Balanced, Aggressive.
   - Now, copy the block of formulas from P2:P8 and paste it into T2, U2, and V2.
   - **Crucially, you must edit the formulas in columns T, U, V to point to the correct return columns:**
     - In T2 (Conservative Total Return): =PRODUCT(1+AF:AF)-1
     - In T4 (Conservative Volatility): =STDEV.P(AF:AF)*SQRT(252)
     - ...and so on for all metrics, changing the column reference to AF for Conservative, AG for Balanced, and AH for Aggressive.
   - For Max Drawdown, you will need to create new Running Max and Drawdown columns for each portfolio, just like we did for the individual assets.

2. **Create the Final Comparison Chart:**
   - Select the columns containing the dates (A) and the three "Portfolio_Value" columns (AI, AJ, AK).
   - Go to **Insert** -> **Charts** -> **Line Chart**.
   - Title the chart: "Portfolio Performance Comparison ($100 Initial Investment)".

---

# PHASE 4: ADVANCED ANALYSIS (Using Momentum & Predictive Features)

## Part A: Momentum Analysis using mom_14c

We will see if the 14-day momentum can be used as a simple trading signal for Bitcoin.

### Step 1: Create a Simple Momentum-Based Strategy

We'll create a strategy that buys Bitcoin when momentum is strongly positive and sells (goes to cash) when it's strongly negative.

1. **Define the Strategy Rules & Columns:**
   - Let's place our strategy parameters in a clear spot. In cell **O23**, type: Momentum Strategy Parameters.
   - In cell **O24**, type: Buy Signal Threshold:
   - In cell **P24**, enter: 0.05 (5% positive momentum)
   - In cell **O25**, type: Sell Signal Threshold:
   - In cell **P25**, enter: -0.03 (-3% negative momentum)
   - Now, in your main data table, create new columns for the strategy:
     - **Column AL:** Momentum_Signal. This will be our Buy/Hold/Sell signal.
     - **Column AM:** Strategy_Return. This will be the return of our momentum strategy.
     - **Column AN:** Strategy_Value. The growth of $100 using this strategy.

2. **Generate the Trading Signal:**
   - In cell AL1, type: Momentum_Signal
   - In cell AL3, enter the formula to create the signal:
     ```
     =IF(I3 >= $P$24, "Buy", IF(I3 <= $P$25, "Sell", AL2))
     ```
     *This formula checks: If momentum is >= 5%, signal "Buy". If momentum is <= -3%, signal "Sell". Otherwise, it keeps the previous day's signal (this is the AL2 part, which creates a holding state).*
   - Copy this formula down the entire column.

3. **Calculate the Strategy's Daily Returns:**
   - In cell AM1, type: Strategy_Return
   - In cell AM3, enter the formula:
     ```
     =IF(AL3="Buy", J3, 0)
     ```
     *This means: If the signal is "Buy", we get the full Bitcoin return for that day. If the signal is "Sell", we are in cash and get 0% return.*
   - Copy this formula down.

4. **Calculate the Strategy's Growth:**
   - In cell AN1, type: Strategy_Value
   - In cell AN2, enter: 100
   - In cell AN3, enter: =AN2*(1+AM3)
   - Copy this formula down.

### Step 2: Compare the Momentum Strategy to Buy & Hold

1. **Add the Momentum Strategy to your Summary Table:**
   - Extend your summary statistics table (from Phases 2 & 3) to the right by adding a new column: W1 - Momentum Strategy.
   - Copy the formulas from the Aggressive portfolio column (V2:V8) into cells W2:W8.
   - **Edit the formulas in column W to point to the Momentum Strategy's return column (AM):**
     - W2 (Total Return): =PRODUCT(1+AM:AM)-1
     - W4 (Volatility): =STDEV.P(AM:AM)*SQRT(252)
     - ...and so on.

2. **Add it to your Performance Chart:**
   - Add the Strategy_Value column (AN) to your main performance comparison chart. Now you can visually see if the momentum strategy outperformed or reduced risk compared to simply buying and holding Bitcoin.

## Part B: Predictive Insight Exploration using y_up_next

Let's do some basic analysis to see if we can find patterns before Bitcoin's "up" days.

### Step 1: Analyze Average Conditions Before Up vs. Down Days

We will calculate the average value of key indicators on days *before* Bitcoin goes up versus days before it goes down.

1. **Set up a Summary Table:**
   - Let's place this below our other tables, e.g., starting at cell **O27**.
   - In cell O27, type: Conditions Before Next-Day BTC Move
   - In cell O28, type: Metric
   - In cell P28, type: Before UP Days (y_up_next=1)
   - In cell Q28, type: Before DOWN Days (y_up_next=0)
   - In cell R28, type: Difference

2. **List the Metrics and Calculate Averages:**
   - In cells O29 to O32, list: Gold Price, VIX Level, USD Strength, BTC Momentum (mom_14c).
   - Now, we use the AVERAGEIF function.
     - In cell P29 (Avg. Gold before UP days): =AVERAGEIF(H:H, 1, C:C)
       *This means: Average column C (gold_close) where column H (y_up_next) equals 1.*
     - In cell Q29 (Avg. Gold before DOWN days): =AVERAGEIF(H:H, 0, C:C)
     - In cell R29 (Difference): =P29-Q29
     - **P30** (Avg. VIX before UP days): =AVERAGEIF(H:H, 1, D:D)
     - **Q30** (Avg. VIX before DOWN days): =AVERAGEIF(H:H, 0, D:D)
     - **P31** (Avg. USD before UP days): =AVERAGEIF(H:H, 1, E:E)
     - **Q31** (Avg. USD before DOWN days): =AVERAGEIF(H:H, 0, E:E)
     - **P32** (Avg. Momentum before UP days): =AVERAGEIF(H:H, 1, I:I)
     - **Q32** (Avg. Momentum before DOWN days): =AVERAGEIF(H:H, 0, I:I)
   - Copy the difference formula (R29) down to R32.

**Interpretation:**
- Look at the Difference column (R). A positive value means that metric was, on average, higher before a Bitcoin up day.
- For example, if the BTC Momentum difference is strongly positive, it suggests that positive momentum tends to continue into the next day.
- If the VIX difference is negative, it might suggest that Bitcoin tends to go up more often after periods of *lower* fear.

---

# PHASE 5: DASHBOARD & REPORTING

We will create a new, clean sheet dedicated solely to presenting the key results.

## Step 1: Create a New Dashboard Sheet

1. Right-click on an existing sheet tab and select **Insert** -> **Worksheet**. Name this new sheet **"Dashboard"**.

## Step 2: Design the Dashboard Layout

We will structure the dashboard with clear sections. Here is a suggested layout:

- **Top:** Title and Key Takeaways
- **Left:** Performance Charts
- **Right:** Key Statistics and Allocations
- **Bottom:** Advanced Analysis Insights

Let's build it step-by-step.

## Step 3: Populate the Dashboard with Content

### Part A: Title and Key Takeaways

1. In cell A1, type the main title: **"Bitcoin & Macro-Financial Portfolio Analysis (2014-2024)"**.
   - Select cells A1:F1, go to **Home** -> **Merge & Center**. Make the font large and bold (e.g., Size 16).
2. In cell A3, type: **"Key Takeaways"**.
3. In cells A4 to A7, we will manually write our conclusions based on the data. For example:
   - A4: • Bitcoin dominated in absolute returns but with extreme volatility and drawdowns (>80%).
   - A5: • Diversification into Gold and USD significantly reduced portfolio risk and drawdowns.
   - A6: • The Aggressive (60% BTC) portfolio captured strong growth while the Balanced (30% BTC) portfolio offered a smoother ride.
   - A7: • A simple momentum strategy reduced volatility but underperformed a pure Bitcoin Buy & Hold in this period.

### Part B: Performance Charts

We will link the charts from our data sheet to the dashboard.

1. Go back to your data sheet (Sheet1).
2. Select the **"Portfolio Performance Comparison ($100 Initial Investment)"** chart you created in Phase 3.
3. Copy it (**Ctrl+C**).
4. Go to your Dashboard sheet and paste it (**Ctrl+V**). Place it in a clear spot, e.g., starting at cell A9.
5. Resize the chart to make it a focal point.
6. Repeat this process for the **"Asset Performance Comparison (Normalized to $100)"** chart from Phase 2. Place it next to the first chart, e.g., starting at cell I9.

### Part C: Key Statistics and Allocations

This section will display the most important numbers from our analysis.

1. **Portfolio Performance Snapshot:**
   - In cell I3, type: Portfolio Performance Snapshot.
   - Below it, we will create a small table. In cells I4 to L4, type: Portfolio, Total Return, Volatility, Sharpe Ratio.
   - Manually link the data from your summary table in Sheet1:
     - In I5, type: Conservative
     - In J5, enter: ='Sheet1'!T2 (This links to the Conservative Total Return)
     - In K5, enter: ='Sheet1'!T4 (Links to Conservative Volatility)
     - In L5, enter: ='Sheet1'!T5 (Links to Conservative Sharpe Ratio)
     - Format J5 and K5 as **Percentages**. Format L5 as **Number with 2 decimals**.
   - Repeat for Balanced (row 6) and Aggressive (row 7), linking to the corresponding cells in Sheet1 (columns U and V).

2. **Correlation Matrix:**
   - In cell A25, type: Asset Correlation Matrix.
   - Go to your Sheet1, copy the range containing your correlation matrix (e.g., O10:R15).
   - Go back to the Dashboard, right-click on cell A26, and select **Paste Special** -> **Paste Link**. This will create a live-linked table.
   - Re-apply the **Conditional Formatting** (Color Scales) to this pasted range to make it visual.

3. **Portfolio Allocations (Visual):**
   - In cell I25, type: Portfolio Allocations.
   - Below, create a table for the weights. In cells I26 to L26, type: Portfolio, BTC, Gold, USD/VIX.
   - Manually enter the weights from your Sheet1 table for the three portfolios (rows 27, 28, 29).
   - Select the data for the **Aggressive** portfolio (cells I29:L29).
   - Go to **Insert** -> **Charts** -> **Pie or Doughnut Chart**. This creates a simple visual of the portfolio makeup. Place this chart below the allocation table.

### Part D: Advanced Analysis Insights

1. **Momentum Strategy Summary:**
   - In cell I40, type: Momentum Strategy vs. Buy & Hold.
   - Create a small table below (cells I41:L43) with headers: Strategy, Total Return, Max Drawdown.
   - Link the data:
     - I42: Buy & Hold (BTC)
     - J42: ='Sheet1'!P2
     - L42: ='Sheet1'!P6
     - I43: Momentum Strategy
     - J43: ='Sheet1'!W2
     - L43: ='Sheet1'!W6
   - Format returns and drawdowns as percentages.

2. **Predictive Insights Snapshot:**
   - In cell A40, type: Predictive Signals Snapshot (Avg. Values).
   - Copy the "Conditions Before Next-Day BTC Move" table from Sheet1 (the AVERAGEIF results) and **Paste Special -> Paste Link** it onto the dashboard starting at cell A41.

## Step 4: Final Polish

1. **Formatting:** Use borders, bold text, and shading to clearly separate different sections of your dashboard.
2. **Check Links:** Ensure all linked cells on the Dashboard are displaying the correct values from Sheet1.
3. **Tidy Up:** Adjust the placement and size of charts and tables so the dashboard is clean, uncluttered, and tells a clear story.

**Your Final Dashboard is Complete!**

You now have a single, powerful sheet that summarizes the entire project. It tells a compelling story:

1. **The Story:** The titles and key takeaways provide the narrative.
2. **The Proof:** The charts and statistics provide the evidence.
3. **The Insight:** The correlation matrix, allocations, and advanced analysis show the "why" and "how."

This dashboard effectively demonstrates that while Bitcoin is a uniquely powerful asset, its integration into a diversified portfolio with traditional macro assets can help manage its legendary volatility, and that simple quantitative signals can be explored to navigate its price movements.

**Congratulations on completing this comprehensive financial analysis project!** You have successfully transformed raw data into actionable investment insights.
