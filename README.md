# Edmunds Luxury Car Forum Analysis

Who competes with whom in the US entry-level luxury car market, based on what people actually say on the Edmunds.com forums. Built for Unstructured Data Analytics in the UT Austin McCombs MSBA program (Fall 2026).

The setup: act as an analytics consultant for JD Power, take 6,000 scraped forum posts, and turn them into competitive insight. The class cared less about getting a number and more about making defensible choices, checking them, and being honest about which results hold up.

**[Open the notebook →](edmunds_luxury_car_analysis.ipynb)**

## What's in it

| Part | Question | Approach |
|---|---|---|
| A | Which brands and attributes matter most? | spaCy candidate extraction, then GPT-4o-mini to merge model names, abbreviations and typos into canonical brands and attributes |
| B | Which brands get discussed together more than chance? | Lexical lift matrix across the top 10 brands |
| C | Does reading for meaning change that? | LLM decides which brands a post is really about, lift is recalculated and compared with B |
| D | What does the market look like? | MDS maps with KMeans clusters, one per method |
| E | Which brands own which attributes? | Brand-attribute lift, with the LLM tying each attribute to the right brand in multi-brand posts |
| F | Which brand do people want to own? | LLM-classified aspirational intent as a share of each brand's mentions |
| G | So what? | Recommendations, split into what's robust and what isn't |

## Findings

- **BMW, Cadillac, Lincoln and Ford cluster together** on both the lexical and the LLM-based maps, so that grouping is the most reliable result here.
- **Each brand has its own attribute.** Nissan goes with reliability (lift 2.25), Toyota with comfort (1.99), Cadillac and Kia with styling (1.94, 1.43), and BMW is spread evenly across performance, price and styling.
- **The two lift methods mostly agree** (correlation ≈ 0.81). The biggest gap, Kia and Hyundai, came mostly from a scraping artifact that inflated the lexical counts. The LLM filtered it out, but the pair still came out closely linked.
- **The aspiration result didn't hold up.** The model said 66% of BMW mentions were aspirational. A hand check of 10 sampled posts found only 1 with real intent to buy. The rest were enthusiasm, spec talk or current owners. The notebook reports this rather than hiding it.

Total LLM spend across every step was under $0.10.

## Running it

The notebook was written in Google Colab. It reads the data and cached intermediate results from Google Drive and gets the OpenAI key from Colab secrets (`OPENAI_API_KEY`). The Edmunds dataset was provided by the course, so it isn't in this repo. To rerun it, point `DRIVE_PATH` at your own copy.

```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```
