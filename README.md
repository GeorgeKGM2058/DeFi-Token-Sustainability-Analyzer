# DeFi Token Sustainability Analyzer


I built this DeFi Token Sustainability Analyzer because there is a pattern when researching DeFi tokens, 
usually there's plenty of hype, but little clear analysis of long-term token sustainability. 
So I put together this Google Colab notebook to maybe clear some thing up and nice visualizations for easier comparisons. 
It pulls live data from CoinGecko and DefiLlama, calculates my custom risk metrics, creates radar charts for each protocol, 
their supply projections and rankings and then uses Gemini to add clean commentary before exporting everything into a single PDF. 
It uses relative comparisons between the group of my chosen six protocols. 
This is designed as a piece of exploratory work, not financial advice.


## Features of this project

As a live data pipeline, token supply and price is pulled from CoinGecko and TVL, revenue from DefiLlama. 
The data is cached to avoid abusing the APIs and getting limited as a result. 
It then calculates five key indicators (TVL growth, future dilution potential, emission pressure, P/S ratio, and FDV/revenue) 
and combines them into a composite score. Added a weight that changes based on whether revenue flows to token holders.
It creates a radar chart for each protocol with these 5 metrics and uses Google Colab’s Gemini to generate live data driven commentary in every page.
Later it combines all 6 protocols into a 3 year supply projection chart and a bar chart of the combosite ranking calculated before, both also commented on dynamically.
The PDF includes an executive summary and glossary that aim to explain the goal of the report, the inner workings and also give more useful information about each protocol.
I paid extra attention into buildng this with modularity in mind, so in case more protocols are to be added, it will be a quite easy and fast procedure.


## The protocols currently analyzed

I picked these six to cover different styles and characteristics of protocols, an interesting challenge to make the dynamic parts of it work. 
They are supposed to give a mix of yield, stables, perps and staking with different supply and revenue models. 
Namely the special traits of each that made them the chosen ones are below.
 
- Pendle right now is the top protocol for turning yield bearing assets into tradable pieces. 
They can split so one part gives a fixed return and the other has a higher upside.
- Ethena is the main project that creates synthetic dolars, USDe stablecoins. 
It is also one of the first that offers steady returns without relying on traditional lending.
- GMX is a decentralized exchange for futures trading where stakers receive a share of the trading fees that the platform earns.
- Lido is the biggest liquid staking service. 
People get a token that they can trade when they deposit ETH, which still gives them the normal staking rewards.
- Curve Finance is a decentralized exchange focused on stablecoin swaps.
It is famous for its locking system that gives users voting power and fee rewards.
- Sky (it was MakerDAO in the past) is one of the oldest decentralized stablecoin issuers. 
It backs its coins with collateral and uses some of the profits for token buybacks.


### How to run
This notebook is suitable for Google Colab due to its Gemini integration, required for all dynamic commentary here. 
Running elsewhere would require a replacement LLM API that will be able to do a similar job with the same inputs, live data and instructions. 
Google Colab also covers the Python 3.10+ requirement and then the libraries utilized are the following (they are all installed in the first cell in the notebook): 
`requests`, `pandas`, `numpy`, `matplotlib`, `fpdf`.

1. Open the notebook in Colab with one click here:
   [![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/defi_sustainability_report.ipynb)
2. Insert your CoinGecko key (instructions below).
3. Click "Run all" on top (takes 1-2 minutes normally).
4. Download the file generated, `defi_sustainability_report.pdf` from the Colab file browser (on the left).


### Obtaining the API key

This is probably the most time consuming (2 minutes max) part. 
A free CoinGecko Demo API key is required to grab the live token data. 
Obtaining one is simple, fast and free.

1. Sign up at [CoinGecko API](https://www.coingecko.com/en/api) (choosing le free tier)
2. In the notebook, set `API_KEY = "your_key_here"` in the first cell

Luckily the DefiLlama API is public and requires no key to obtain the data.


## The metrics

Here's a short explanation for each metric. Their full definitions, weighting logic, adjustments and the emission pressure formula 
are in the generated report’s glossary and in the notebook in detail.

- TVL growth is the Annualized CAGR (Compound Annual Growth Rate), that shows actual adoption.
- Future dilution potential is the undistributed supply as part of the max supply. (max supply–circulating)/max supply, in a 0-1 scale
- Emission pressure is my custom heuristic. If TVL is growing it’s overhang/growth (floored at 5% so tiny growth doesn’t explode the score). If shrinking it’s (overhang×2)+(|decline|×2). Capped at 10. Trial and error led to this for ranking the protocols fairly.
- P/S ratio is the market capitalization/annualized protocol revenue (30-day avg daily x 365). 
- FDV/Revenue is the fully diluted valuation/annualized protocol revenue.
- Composite score is a weighted sum of the metrics. They get log-scaled and min-max normalized inside the chosen group so outliers don’t break the ranking. A lower score indicates a lower relative risk within the group.


## Methodology and limitations

The data comes from CoinGecko (supply and price) and DefiLlama (TVL history and 30-day average revenue). 
The 3 year supply projections assume a steady linear schedule for simplicity. 
For the same reason I manually adjusted Sky’s numbers for the rebrand, 1 MKR = 24,000 SKY. 
As explained, emission pressure is my own custom heuristic for relative ranking, not a precise forecast. 
The composite scores are always relative to the current peer group so they will change if we swap some protocols. 
To avoid over complicating things, TVL growth uses raw historical data without seasonal adjustments. 
Note this project is not about modeling more complicated events like burns, buybacks or governance votes. 
Also, the dynamic commentary is built live based on different building blocks, being the hardcoded prompts, the live numbers and also depends on Gemini's current state and model quality. 



## Sample output

Here are some screenshots and the report itself, of a sample run for presentation:

- ![Executive Summary](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/assets/executive_summary.png?raw=true)
- ![Metric Definitions](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/assets/metric_definitions.png?raw=true)
- ![Pendle Page](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/assets/pendle_page.png?raw=true)
- ![Relative Dilution Chart](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/assets/relative_dilution_chart.png?raw=true)
- ![Supply Projection Chart](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/assets/supply_projection_chart.png?raw=true)
- ![Composite Ranking](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/assets/composite_ranking.png?raw=true)
- The full sample PDF: [defi_sustainability_report.pdf](https://github.com/GeorgeKGM2058/DeFi-Token-Sustainability-Analyzer/blob/main/reports/defi_sustainability_report.pdf)
               

## Possible ways to expand the project

The project currently achieves the goal of doing analysis and comparisons between these protocols, 
though there's always some room of improvement so a couple propositions are following below. 
The protocols studied here are 6 specific ones so the list is not dynamic enough for a casual user to choose others to add or remove in a simple way and analyze them. 
This could be a direction to expand the project towards but unlike more streamlined assets like stocks, 
defi protocols have unique characteristics that make this quite complicated. 
For example the weighting system and commentary currently require some hardcoding by the user, which is way beyond that end-user thought . 
Another expansion could be to fetch onchain metrics from Dune that would help to make the dilution and emission pressure metrics more accurate. 
But again this would require more API keys and make everything even more complicated.


Built by GeorgeKGM2058.
I originally started this project to enrich my knowledge and practical skills regarding DeFi. 
The thought of adding live generated commentary based on ready inputs felt interesting so I went in to implement it and it turned into an important part of the report. 
