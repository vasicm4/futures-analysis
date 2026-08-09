# futures-analysis

Analiza vremenskih serija finansijskih podataka — nedeljne cene WTI nafte, BRENT nafte i NVIDIA akcija, AR(1) modelovanje, test elipse i Monte Carlo simulacija.

Time-series analysis of weekly commodity and equity prices — WTI crude, BRENT crude, and NVIDIA — including AR(1) modeling, the ellipse test, and Monte Carlo simulation.

---

## Project overview / Pregled projekta

This project implements the six-part assignment in `zadatak.md` (Serbian), which analyses weekly financial time series for the period **2013-01-01 to 2023-01-01** and addresses whether the series behave as **weak white noise**, exhibit **mean reversion**, or follow a **stochastic trend**.

**Instruments analysed** (Yahoo Finance tickers, weekly interval):

| Instrument | Ticker | Description |
|---|---|---|
| WTI   | `CL=F` | WTI crude oil futures |
| BRENT | `BZ=F` | BRENT crude oil futures |
| NVDA  | `NVDA` | NVIDIA common stock |

The analysis pipeline:

1. **Data acquisition** → log-prices → log-returns (differences)
2. **White-noise diagnostic** on log-returns (Ljung-Box, ADF, Engle's ARCH)
3. **AR(1) OLS fit** with unit-root classification (|b|<1, b≈1, |b|>1)
4. **Ellipse test** on AR(1) residuals for lag k=1,2,3
5. **Random Walk model** (X_t = X_{t-1} + e_t) with Gaussianity check and 52-week Monte Carlo
6. **AR(1) Monte Carlo** vs Random Walk at the 500-week horizon
7. **Half-life analysis** for the AR(1) coefficient b
8. **Distribution comparison** of even/odd AR(1) residuals (KS test, KDE, ellipse)

Note: GARCH(1,1) from the original task specification (point 6 in `zadatak.md`) is not implemented in this notebook.

---

## Repository structure / Struktura repozitorijuma

| File | Purpose |
|---|---|
| `project.ipynb` | Main analysis notebook (the deliverable). All 11 cells are runnable top-to-bottom. |
| `scrape.ipynb` | Earlier working copy of the same analysis. Kept for reference. |
| `zadatak.md` | Original assignment statement (Serbian), defines the six analysis parts. |
| `test_elipse.md` | Mathematical definition of the ellipse test (eigenvalue decomposition of the covariance matrix of `(e(t), e(t+k))`). |
| `requirements.txt` | Python dependencies. |
| `prices.csv` | Cache of downloaded daily prices, generated automatically on first run of cell 9. |
| `log_cene.png`, `log_razlike.png` | Pre-rendered reference figures from an earlier run. |
| `zadatak (3).pdf`, `test elipse (2).pdf` | Original task PDFs (Serbian). |
| `.venv/` | Local Python virtual environment (not tracked). |

---

## Notebook structure / Struktura notebook-a

`project.ipynb` has 11 cells, organized as follows. Each cell is self-contained but the data flows from cell 1 to cells that follow.

| Cell | Topic | Maps to task |
|---|---|---|
| 0 | Single import block (numpy, pandas, yfinance, statsmodels, scipy, matplotlib) | — |
| 1 | Download weekly prices 2013-2023, build `cene`, `log_cene`, `log_razlike`, plot series | Task 1 (data + plots) |
| 2 | `evaluate_white_noise(series, lags, alpha)` — Ljung-Box + ADF + Engle's ARCH | Task 1 (white noise) |
| 3 | Apply the white-noise test to each log-return series | Task 1 |
| 4 | Manual AR(1) OLS fit + unit-root classification; stores `reziduali` dict | Task 2 |
| 5 | Ellipse test on AR(1) residuals (k=1,2,3) | Task 3 |
| 6 | Random Walk model: Gaussianity check (4A) + 52-week MC simulation (4B) for WTI | Task 4 |
| 7 | Half-life analysis: weeks k such that b^k = 0.5 | Extension of task 2 |
| 8 | AR(1) vs Random Walk Monte Carlo for WTI, 500 weeks ahead, 100 paths, vs actual 2023 prices | Task 5 |
| 9 | Standalone statsmodels-based re-implementation: AR(1) fit, autocovariance, ellipse, ECDF/PDF comparison of even/odd residuals | Validation + extension |
| 10 | Empty placeholder | — |

The two Monte Carlo simulations (cells 6 and 8) reach the same conclusion stated in the task: **simulated paths diverge from reality**, and the AR(1) paths revert to the long-run mean while the Random Walk paths drift.

---

## Setup

### 1. Create virtual environment

```bash
# Create (run once, in the project directory)
python -m venv .venv        # Windows
py -m venv .venv            # Windows, if python is not found
python3 -m venv .venv       # macOS / Linux

# Activate (every new terminal)
.venv\Scripts\activate      # Windows (Command Prompt)
source .venv/bin/activate   # macOS / Linux
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

Required packages: `yfinance`, `pandas`, `numpy`, `matplotlib`, `seaborn`, `statsmodels`, `scipy`. The `arch` and `datetime` entries in `requirements.txt` are not used by `project.ipynb` and can be removed.

### 3. Run the notebook

```bash
jupyter notebook project.ipynb
```

Run all cells top-to-bottom. The first execution of cell 9 will download ~10 years of daily prices via `yfinance` and cache them to `prices.csv` (~250 KB). Subsequent runs load from the cache.

---

## Key findings / Ključni nalazi

- **Log-prices** for all three instruments are non-stationary and very close to a random walk: the AR(1) coefficient b ≈ 1 (e.g. WTI: b ≈ 0.987, BRENT: b ≈ 0.989, NVDA: b ≈ 0.998), giving a very long half-life (tens of years) or effectively no mean reversion.
- **Log-returns** are stationary but show weak autocorrelation. The white-noise test passes for some instruments (linear independence) but fails the ARCH test (volatility clustering) for the equity series — fat tails and conditional heteroscedasticity.
- **Ellipse test** on AR(1) residuals of log-returns: λ1/λ2 ratios near 1 (circle-like) for all instruments, meaning the AR(1) captures the linear structure and the residuals are approximately uncorrelated at the tested lags.
- **Random Walk Monte Carlo** vs reality: the 90% confidence band does not contain the realized 2023 WTI prices in 25/25 weeks — the Gaussian random walk underestimates the true variance and misses the regime shift.
- **AR(1) Monte Carlo** vs Random Walk: AR(1) paths revert to the long-run mean (~`np.exp(mu_long_run)`) while Random Walk paths drift; the difference becomes visible only over long horizons (>100 weeks), which matches the half-life analysis.

---

## Notes & known issues

- **Negative WTI price on 2020-04-20**: WTI crude futures settled at -$37.63 on that date during the COVID-19 demand collapse. This is real market data, not a corruption. The notebook filters non-positive prices before taking `np.log` in cell 9, so the analysis runs without warnings. The cell-1 path uses `.where(... > 0).dropna()` consistently.
- **yfinance multi-ticker shape**: recent versions of `yfinance` return a DataFrame with MultiIndex columns even for a single-ticker download. The notebook handles this with `if isinstance(df, pd.DataFrame): df = df.iloc[:, 0]` guards.
- **`prices.csv` cache**: contains 2518 daily rows for the three tickers. Delete it to force a re-download.
- **Unimplemented task part**: GARCH(1,1) identification (point 6 of the original task) is not done; the notebook stops at AR(1) and Random Walk modeling.

---

## References

- `zadatak.md` — original assignment (Serbian)
- `test_elipse.md` — mathematical definition of the ellipse test
- `zadatak (3).pdf`, `test elipse (2).pdf` — original task PDFs

