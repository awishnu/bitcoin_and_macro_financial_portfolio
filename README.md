# Bitcoin & Macro-Financial Portfolio Analysis (2015-2024)

## 📘 Overview

This repository contains a comprehensive analysis of portfolio strategies that combine Bitcoin with traditional macro-financial assets (Gold, VIX, USD) over the period from 2015 to 2024. The project evaluates the risk-return trade-offs of different portfolio allocations and compares a simple momentum-based trading strategy against a classic Buy & Hold approach.

## 🧱 1. Dataset Information  
**File:** `df_all_for_kaggle.csv`  
**Source:** [kaggle](https://www.kaggle.com/datasets/pablomartis1/historical-data-btc-vix-gold-usd)  
**Time Periode:** January 2015 - December 2024  
**Column:** `date`, `close_usd`, `gold_close`, `vix_close`, `usd_close`, `btc_weekend_ret`, `ret_btc_next_bday`, `y_up_next,mom_14c`.

## 🎯 Key Takeaways

* **Bitcoin's Dominant Returns & Extreme Volatility:** Bitcoin significantly outperformed all other assets in absolute returns but experienced extreme volatility.
* **Diversification is Key:** Portfolios that included Gold and USD saw dramatically reduced risk and drawdowns. The low correlation between Bitcoin and these traditional assets provided a crucial cushion during market downturns.
* **Aggressive vs. Balanced Portfolios:**
  * The **Aggressive Portfolio (60% BTC)** captured substantial growth.
  * The **Balanced Portfolio (30% BTC)** offered a much smoother equity curve and superior risk-adjusted returns (Sharpe Ratio).
* **Momentum Strategy Underperformance:** While the tested momentum strategy successfully reduced volatility and drawdown, it ultimately underperformed a pure Bitcoin Buy & Hold strategy in terms of total return during this specific bull-market-dominated period.

## 📊 Asset Correlation Matrix

The low correlation between Bitcoin and traditional safe-haven assets is the foundation of this analysis.

| Asset | Bitcoin | Gold | VIX | USD |
|:------|--------:|-----:|----:|----:|
| **Bitcoin** | 1.00 | 0.10 | -0.17 | -0.07 |
| **Gold** | 0.10 | 1.00 | 0.00 | -0.42 |
| **VIX** | -0.17 | 0.00 | 1.00 | 0.04 |
| **USD** | -0.07 | -0.42 | 0.04 | 1.00 |

## 📈 Performance Comparison
<img width="1448" height="733" alt="image" src="https://github.com/user-attachments/assets/8ec46b72-3ade-4e3f-85fb-2c7b948ea9d7" />

## 🧮 Portfolio Allocations

Three distinct portfolio strategies were simulated:

| Portfolio | BTC | Gold | VIX | USD |
|:----------|----:|-----:|----:|----:|
| **Conservative** | 10% | 50% | 10% | 30% |
| **Balanced** | 30% | 40% | 10% | 20% |
| **Aggressive** | 60% | 20% | 10% | 10% |

We allso simulate a strategy that buys only Bitcoin when momentum is strongly positive (base on "mom_14c" column) and sells when it's strongly
<img width="1440" height="726" alt="image" src="https://github.com/user-attachments/assets/e82f2c00-3e6a-4776-a761-3b88e2771ebb" />

## 📈 Performance Summary

### Portfolio Performance (Initial Investment: $100)

| Portfolio | Total Return | Volatility | Sharpe Ratio |
|----------:|-------------:|-----------:|-------------:|
| **Conservative** | 650.06% | 15.65% | 142.69% |
| **Balanced** | 2,032.94% | 23.33% | 171.79% |
| **Aggressive** | 15,042.27% | 40.83% | 159.76% |

### Strategy Comparison: Buy & Hold vs. Momentum

| Strategy | Total Return | Max Drawdown |
|:---------|-------------:|-------------:|
| **Buy & Hold (BTC)** | 29,620.95% | -83.04% |
| **Momentum Strategy** | 13,329,419.54% | -24.41% |

> *Note: The extraordinarily high return for the Momentum Strategy should be verified for potential data or calculation artifacts, though it successfully achieved a lower drawdown.*

## 📎 Project Files  
```
📂 health-risk-sql-analysis/
├── data/
│   ├── Lifestyle_and_Health_Risk_Prediction_Synthetic_Dataset.csv
├── results/
│   ├── 03_import_and_test.csv
├── sql/
│   ├── 01_create_database.csv
│   ├── 02_create_the_table_scheme.csv
│   ├── 03_import_and_test_csv.csv
│   ├── 04_cleean_and_inspect_data.csv
│   ├── 05_analysis.csv
└── README.md
└── setup_instruction.md
```

## 🔚 Conclusion

This analysis demonstrates that while Bitcoin alone offers unparalleled returns, its extreme risk makes it unsuitable for most investors as a standalone asset. However, by strategically allocating a portion of a portfolio to Bitcoin and diversifying with uncorrelated assets like Gold and USD, investors can achieve an excellent balance of high growth and managed risk. The **Balanced Portfolio (30% BTC)** emerged as a particularly compelling option, offering strong returns with significantly lower volatility and a superior Sharpe Ratio compared to the more aggressive alternatives.
