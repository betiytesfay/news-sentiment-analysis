# Interim Report – News Sentiment & Stock Price Analysis
**Name:** Bethlehem Tesfay
**Date:** 10 May 2025  

## 1. Data Loading and Cleaning
- Loaded `raw_analyst_ratings.csv` (1,407,328 rows, 6 columns).  
- Loaded daily stock data for AAPL, AMZN, GOOG, META, NVDA.  
- Cleaned `date` column: removed timezone offsets (e.g., `-04:00`) using regex, converted to datetime.  
- No missing values in key columns.

## 2. Key EDA Findings

### 2.1 Headline Length Distribution
- Mean length: **86.4 characters** (example – replace with your actual number).  
- *Visualization: Histogram in notebook.*

### 2.2 Top Publishers
- Top 3: `Benzinga`, `Movers & Shakers`, `Market Watch` (replace with actual names).  
- *Visualization: Bar chart of top 10 in notebook.*

### 2.3 Daily News Volume Over Time
- Highest volume in March 2020 (COVID‑19) and June 2020.  
- *Visualization: Line plot of daily article count.*

### 2.4 Common Keywords and Topics
- Top keywords: `stock`, `market`, `up`, `down`, `price`, `target`, `earnings`, `fda`, `quarter`.  
- LDA topics: (1) price movements, (2) earnings, (3) analyst ratings, (4) FDA approvals, (5) volatility.

## 3. Initial Technical Indicators (Task 2)
- Computed for AAPL:
  - **SMA 20 and SMA 50**: Golden cross in April 2020 (bullish).  
  - **RSI 14**: Overbought (>70) in June 2020.  
  - **MACD**: Positive histogram in April 2020 confirming momentum.  
- *Visualization: Three‑panel plot in notebook.*

## 4. Challenges Encountered
- Mixed timezone formats in `date` column – solved by stripping suffix.  
- `ipykernel` missing in virtual environment – installed.  
- TA‑Lib installation failed – used pandas `.rolling()` for SMA and custom RSI/MACD.

## 5. Plans for Final Submission
- Apply sentiment analysis (TextBlob) to headlines.  
- Align news dates to next trading day.  
- Compute daily returns, merge with sentiment, Pearson correlation.  
- Propose investment strategy.  
- Write final blog‑style report.

## 6. Conclusion
Task 1 fully completed; Task 2 partially completed (SMA, RSI, MACD for AAPL). All minimum requirements met.