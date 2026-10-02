# Data dictionary

The sale table contains **3,565 clean sales**: 3,132 for 73 miniatures games and 433 for 23 large non-miniatures games. The game table contains one row per game. Game titles are retained so readers can interpret variation; seller and listing identifiers are omitted.

## Sale rows

| Column | Meaning |
|---|---|
| `game` | Game title. |
| `sample` | `miniatures` or `sampleA` (large non-miniatures games). |
| `sale_date` | Sale date, `YYYY-MM-DD`. |
| `condition` | Listed condition; `New` is sealed, while `Like New`, `Very Good`, and `Good` form the used group in summary tables. |
| `edition` | Classified edition of the sold copy, such as Kickstarter, retail, or unknown. |
| `resale_usd` | Sale price converted to nominal US dollars at the sale month. Excludes shipping. |
| `campaign_cost_usd` | Like-for-like cost of the matched contents in the original campaign, converted to nominal US dollars at the campaign month. Excludes shipping. |
| `ratio_nominal` | `resale_usd / campaign_cost_usd − 1`. A value of `-0.20` means a 20% discount. |
| `ratio_real` | `resale_usd / (campaign_cost_usd × US CPI at sale month / US CPI at campaign month) − 1`. |
| `years_after_delivery` | Years from estimated delivery to sale; negative means the sale preceded the estimate. Estimated delivery is the latest promised month for the matched campaign contents plus six months. This is a proxy, not an observed delivery date. |
| `resale_currency` | Currency in which the sale was listed, before conversion to USD. USD is only a proxy for a US seller. |

## Game rows

| Column | Meaning |
|---|---|
| `game`, `sample` | Title and sample; together identify a row. |
| `backers` | Original campaign backer count, where available. |
| `campaign_month` | Original campaign end month, `YYYY-MM`. |
| `core_usd` | Original standard core pledge in nominal US dollars. |
| `n_sales` | Number of clean sales, including sales before estimated delivery. |
| `median_real_after_delivery` | Median of real sale ratios after estimated delivery, across all conditions and currencies. |
| `share_below_cost` | Share of post-delivery sales with a negative real ratio. |
| `retail_edition_seen` | `y` if a classified retail-edition resale was observed, `n` if none was observed. An `n` does not establish that no retail edition exists. |
| `n_units` | Number of mapped contents units; available for miniatures games. |
| `exclusive_text`, `retailer_text` | `1` or `0` keyword indicators from campaign text; available for miniatures games. These do not establish actual edition contents or retail availability. |
| `licensed_ip`, `sequel` | `1` or `0` game descriptors; available for miniatures games. |
| `publisher` | Clean publisher name where available. |

## Summary tables

The break-even and USD-only tables group post-delivery sales into half-open year bands: 0–1, 1–2, 2–3, 3–5, and 5+ years. `n_sales` and `n_games` give each bin's size. `median_nominal` and `median_real` are median ratios; `share_below_cost_real` is a fraction. The median is taken in log-ratio space to match the analysis. The USD-only table repeats the full-currency miniatures series for direct comparison.

The backer-bucket table uses only miniatures games with at least five post-delivery USD-listed sales and a backer count. `median_game_real` is the median of game medians; `games_below_cost` counts games with a negative median. `median_usd_sales_per_year` divides each game's qualifying sales count by at least one year of observed post-delivery span, then takes the bucket median. The under-2,000 cutoff was chosen after inspecting the results.

The edition-comparison table contains nine miniatures games with at least three post-delivery retail sales and three Kickstarter-edition sales each. It uses the **most recent** campaign cost for each matched bundle, unlike the original-campaign basis of the sale and game tables. `retail_vs_kickstarter_gap` is the ratio of the two median resale-to-campaign multiples minus one. Edition contents may differ.

## Filters and limits

Sales need a priced matched campaign bundle, known contents, an estimated delivery month, and a listing lasting at least one hour when timestamps are available. Sales with unmatched add-ons or a sold bundle larger than the priced bundle are excluded. For seven non-miniatures game entries that also contain older-edition sales, only identified Kickstarter-edition sales are retained. Ratios use matched contents rather than a generic core pledge; the separate recent sold-listing check described in the article is not included here. Original listing prices and matched campaign costs are converted with monthly exchange rates. US CPI adjusts the campaign cost to the sale month; the latest available monthly value is carried forward when a sale is later. Shipping and buyer fees are excluded. The data represent observed trades and may miss games that never delivered or rarely resell.
