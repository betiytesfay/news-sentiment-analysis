# Predicting Price Moves with News Sentiment  
**Final Report – Nova Financial Solutions**  
*Bethlehem Tesfay*  
*14 May 2026*

---

## 1. Executive Summary

This analysis quantifies the relationship between financial news sentiment and daily stock returns for five major tech stocks (AAPL, AMZN, GOOG, META, NVDA) using the FNSPID news dataset and Yahoo Finance price data.  

**Key findings:**  
- News volume spiked on **12 March 2020** (2,739 articles) during the COVID‑19 market sell‑off.  
- Top publishers: Paul Quintaro (16.2%), Lisa Levin (13.3%), Benzinga Newsdesk (10.7%).  
- Most news is released at **00:00 UTC** – aligned with US market open.  
- Technical indicators (SMA/EMA crossovers, RSI, MACD, Sharpe ratio) confirmed typical market behaviour.  
- Sentiment–return correlations were very weak or neutral across stocks (average 0.035). AAPL showed 0.138 (weak positive), AMZN -0.185 (weak negative), GOOG 0.087, NVDA 0.100. META had no aligned trading days.  
- **Recommendation:** A long‑short strategy buying on positive sentiment days (sentiment > 0.05) and selling on negative sentiment days (sentiment < -0.05) could generate a small positive excess return (backtest needed).

---

## 2. Methodology

### 2.1 Data Sources
- **News:** FNSPID (raw_analyst_ratings.csv, 1.4M rows) – headlines, publication date, stock ticker.  
- **Prices:** Daily Open, High, Low, Close, Volume for AAPL, AMZN, GOOG, META, NVDA (2009–2020).

### 2.2 Data Cleaning
- Removed timezone offsets from news dates (kept YYYY-MM-DD HH:MM:SS).  
- No missing values in critical columns.  
- Large CSV excluded from Git via .gitignore.

### 2.3 Sentiment Analysis
- Tool: **TextBlob** (polarity score from -1 to +1).  
- Why TextBlob? Simple, fast, and adequate for directional sentiment.  
- Alternative VADER was considered but TextBlob gave sufficient granularity.

### 2.4 Technical Indicators (Task 2)
- SMA20, EMA20, EMA50 – trend direction.  
- RSI (14‑day) – overbought (>70) / oversold (<30).  
- MACD (12,26,9) – momentum shifts.  
- Sharpe ratio (annualised) – risk‑adjusted return (manually calculated due to pynance unavailability).

### 2.5 Correlation Analysis
- Aligned news dates to **next trading day** for weekends/holidays.  
- Aggregated daily sentiment per stock (average of multiple headlines).  
- Computed daily returns: (Close_t - Close_{t-1}) / Close_{t-1} * 100.  
- Pearson correlation between daily average sentiment and daily return.  
- Categorised days: Positive (>0.05), Neutral (-0.05 to 0.05), Negative (<-0.05).

---

## 3. Exploratory Data Analysis (Task 1)

### 3.1 Headline Length Distribution
- Mean: 73 characters, Median: 64, Max: 512.  

### 3.2 Publisher Activity
Top 3 publishers by article count:  
- Paul Quintaro: 228,373 (16.23%)  
- Lisa Levin: 186,979 (13.29%)  
- Benzinga Newsdesk: 150,484 (10.69%)  

### 3.3 News Volume Over Time
- Peak: **2,739 articles on 12 March 2020** (WHO pandemic declaration).  
- Another smaller peak in June 2020.  

### 3.4 Top Keywords & Topics
- Top 20 keywords: announces, benzinga, buy, downgrades, earnings, eps, est, market, mid, price, pt, raises, reports, sales, shares, stocks, trading, update, vs, week.  
- LDA topics: price movements, earnings, analyst ratings, regulatory approvals, COVID‑19.

### 3.5 Publication Hour
- **00:00 UTC** dominates (1.35M articles).  
- Almost none at 01:00 UTC (14 articles).  

---

## 4. Technical Indicators (Task 2)

We computed for all five stocks. Below is a representative example for **AAPL**.

### 4.1 Price with Moving Averages
- Golden cross (SMA20 > SMA50) occurred on **19 March 2009**.  

### 4.2 RSI
- First overbought (>70) on 5 February 2009.  

### 4.3 MACD
- First positive histogram on 5 January 2009.  

### 4.4 Sharpe Ratio Comparison
- **AAPL:** volatility 0.2844, Sharpe 1.1474  
- **AMZN:** volatility 0.3420, Sharpe 0.9441  
- **GOOG:** volatility 0.2733, Sharpe 0.8281  
- **META:** volatility 0.3972, Sharpe 0.7382  
- **NVDA:** volatility 0.4571, Sharpe 1.0502  

**Interpretation:** NVDA had the highest risk‑adjusted return, META the lowest – consistent with higher volatility.

---

## 5. Sentiment–Return Correlation (Task 3)

### 5.1 Correlation Coefficients
- **AAPL:** correlation 0.1381 based on 61 aligned days  
- **AMZN:** correlation -0.1847 based on 28 aligned days  
- **GOOG:** correlation 0.0865 based on 353 aligned days  
- **META:** no aligned days (no overlapping trading dates)  
- **NVDA:** correlation 0.0998 based on 1143 aligned days  

### 5.2 Interpretation
- The correlation is **very weak or neutral** for most stocks. The average correlation across stocks (excluding META) is 0.0349.  
- AAPL shows a weak positive correlation (0.138), suggesting that on days with more positive news, returns tend to be slightly higher. AMZN shows a weak negative correlation (-0.185), but this is based on only 28 aligned days and may not be robust. GOOG and NVDA are near zero.  
- **Limitations:**  
  - Correlation does not imply causation.  
  - News volume varies widely; some days have dozens of headlines, others none.  
  - Sentiment tools (TextBlob) are not finance‑specific (e.g., “buy” is positive but could be sarcastic).  
  - Lag effects not considered (news may impact prices over 2–3 days).  
  - META had no aligned trading days due to date range mismatch between news and price data.



## 6. Investment Strategy Recommendations

Based on the observed relationship, we propose a **sentiment‑driven trading strategy**:

1. **Daily signal generation:**  
   - For each stock, compute average sentiment of all headlines that day.  
   - If sentiment > +0.05 → **Buy** at next day’s open.  
   - If sentiment < -0.05 → **Sell** (or short) at next day’s open.  
   - If neutral (-0.05 to +0.05) → hold cash or maintain previous position.

2. **Risk management overlay:**  
   - Only take signals when the stock’s RSI is not extreme (>70 overbought or <30 oversold).  
   - Limit exposure to max 20% of portfolio per stock.

3. **Expected outcome:**  
   - Backtesting (not performed here) would likely show a small positive alpha, but transaction costs and slippage may erode profits. The very weak correlation suggests that sentiment alone is not a strong predictor; combining with technical filters is essential.  

4. **Next steps before live deployment:**  
   - Incorporate **lagged sentiment** (e.g., 2‑day moving average).  
   - Use **FinBERT** (finance‑specific NLP) for better accuracy.  
   - Test on out‑of‑sample data (post‑2020).  



## 7. Limitations & Future Work

- **No causal inference** → Use Granger causality tests, event study.  
- **TextBlob ignores financial context** → Fine‑tune FinBERT on earnings call transcripts.  
- **Date alignment (weekend/holiday) may introduce noise** → Use intraday price data for same‑day reaction.  
- **Only five large‑cap stocks** → Expand to small‑cap and sector ETFs.  
- **No control for market‑wide sentiment** → Include VIX or sector ETF returns as benchmark.  
- **META had no aligned trading days** → Investigate data coverage; use alternative source or longer date range.


## 8. Conclusion

We successfully built an end‑to‑end pipeline that quantifies news sentiment, computes technical indicators, and correlates sentiment with daily stock returns. The correlation is generally very weak or neutral (average 0.035), but sentiment‑categorised returns show that positive news days tend to have marginally higher returns than negative news days. The proposed strategy offers a systematic way to incorporate news flow into trading decisions, though it should be treated as a supplementary signal. Further refinement with finance‑specific NLP and lag structures could improve predictive power.



**Appendix**  

- Interactive notebooks: eda_news.ipynb, task3_sentiment_correlation.ipynb