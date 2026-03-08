# DeFi Token Sustainability Analyzer

This repository contains a Google Colab notebook that fetches live market and protocol data from CoinGecko and DefiLlama APIs, computes a set of tokenomics risk metrics, and generates a professional PDF report. The report includes per-protocol tables, radar charts for risk visualization, 3-year supply projections, and AI-generated insights using Google's Gemini model. Designed as an exploratory tool for assessing long-term token sustainability in DeFi, it focuses on relative comparisons within a peer group of six protocols.


## Features

- **Live Data Pipeline**: Integrates CoinGecko for token supply and pricing data, and DefiLlama for TVL history and revenue figures, with built-in caching and exponential backoff for rate limiting.
- **Custom Metrics Framework**: Calculates five key indicators (TVL growth, future dilution potential, emission pressure, P/S ratio, FDV/revenue) and combines them into a weighted composite score. Weights adjust dynamically based on whether revenue accrues directly to token holders (e.g., halved P/S emphasis for governance-only tokens like ENA or LDO).
- **Visualizations**: Generates radar charts for individual risk profiles, line charts for dilution projections, and bar charts for composite rankings using Matplotlib.
- **AI-Enhanced Reporting**: Leverages Colab's Gemini API to produce neutral, data-driven commentary on metrics and charts, ensuring structured and factual insights.
- **PDF Export**: Uses FPDF to create a multi-page investor-style report with executive summary, metric glossary, protocol breakdowns, and summary visuals.
- **Extensibility**: Configurable protocol list in the notebook; easy to add more via a dictionary structure.


## Protocols Analyzed

- **Pendle (PENDLE)**: Yield tokenization leader.
- **Ethena (ENA)**: Synthetic dollar and delta-neutral yield innovator.
- **GMX (GMX)**: On-chain perpetuals DEX with revenue sharing.
- **Lido (LDO)**: Dominant liquid staking protocol.
- **Curve Finance (CRV)**: Stablecoin DEX with veCRV locking.
- **Sky (SKY, formerly MakerDAO)**: Decentralized stablecoin issuer with buybacks.

These were selected to represent a mix of DeFi primitives (yield, stables, perps, staking) with varying tokenomics designs.


## Setup

This notebook is optimized for Google Colab due to its native AI integration (`google.colab.ai.generate_text()`). Running elsewhere requires replacing the AI calls with an alternative LLM provider.


### Prerequisites
- Google Colab (Gemini AI commentary)
- Python 3.10+ (pre-installed in Colab).
- Libraries: `requests`, `pandas`, `numpy`, `matplotlib`, `fpdf` (installed via pip in the notebook).


### API Key
A free CoinGecko Demo API key is required for token data:
1. Sign up at [CoinGecko API](https://www.coingecko.com/en/api) (free tier, ~2 minutes).
2. In the notebook, set `API_KEY = "your_key_here"` in the first cell.
DefiLlama API is public and requires no key.


### Installation
No local install needed—run directly in Colab:
1. Open the notebook directly in Colab:
   [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/defi_sustainability_report.ipynb)
2. Insert your CoinGecko key.
3. Go to Runtime > Run all.


## Usage

1. Load the notebook in Colab.
2. Enter your CoinGecko API key.
3. Execute all cells (takes ~1-3 minutes, depending on API responses).
4. Download the generated `defi_sustainability_report.pdf` from the Colab file browser.

The report pulls fresh data on each run, so results reflect current market conditions.


## Metrics Explained

| Metric                  	| Description |
|-------------------------------|-------------|
| TVL Growth (ann.)      	| Annualized CAGR of Total Value Locked over the trailing 12 months. |
| Future Dilution Potential     | Undistributed supply as a fraction of max supply (0-1 scale). |
| Emission Pressure      	| Heuristic: overhang / growth (if positive, floored at 5%), or (overhang x 2) + (|decline| x 2) (if negative). Capped at 10 for relative ranking. |
| P/S Ratio             	| Market cap / annualized protocol revenue (30-day avg daily x 365). |
| FDV / Revenue          	| Fully diluted valuation / annualized revenue. |
| Composite Score        	| Weighted sum of normalized metrics (log-scaled for outliers). Lower score indicates lower relative risk within the peer group. |

See the report's glossary for full definitions and weighting logic.


## Methodology and Limitations

- **Data Sources**: CoinGecko for supply/price. DefiLlama for TVL and fees. Revenue annualized from 30-day trailing average.
- **Assumptions**: 3-year supply projections use linear unlocks for illustration (e.g., overestimates CRV's exponential decay). Sky's figures account for the 1 MKR = 24,000 SKY rebrand, inflating absolute supply scales.
- **Heuristics**: Emission Pressure is a custom-designed relative signal, not a precise dilution forecast. Composite scores are peer-relative and shift with protocol changes.
- **Limitations**: Does not model burns, buybacks, or governance dynamics. AI commentary is generated via prompts, factual but dependent on model quality. TVL growth uses raw historical data without seasonal adjustments.
- **Transparency**: All calculations are explicit in the notebook. This is an exploratory analysis, not financial advice.


## Sample Output

Below are excerpts from a sample report (data from March 2026 run):

![Executive Summary](path/to/exec_summary_screenshot.png)

![Metric Definitions](path/to/metric_definitions_screenshot.png)

![Pendle Page](path/to/pendle_page_screenshot.png)

![Relative Dilution Chart](path/to/relative_dilution_screenshot.png)

![Supply Projection Chart](path/to/supply_projection_screenshot.png)

![Composite Ranking](path/to/composite_ranking_screenshot.png)

Full sample PDF: [defi_sustainability_report.pdf](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/defi_sustainability_report.pdf)


## Possible Future Improvements

- Expand protocol config to user-defined lists.
- Integrate additional data sources (e.g., Dune for on-chain metrics).
- Add interactive dashboards via Streamlit or Plotly.
- Backtest composite scores against historical token performance.


## License


MIT License. See [LICENSE](LICENSE) for details.







