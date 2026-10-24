# amazon review scraper export to csv: The 3 Working Methods Compared, Real Credit Costs, and Which Plan Fits Your Volume

Most people searching this want one thing: pull Amazon product reviews into a spreadsheet without fighting CAPTCHAs or writing a fragile scraper from scratch. There are three realistic routes — a browser extension, a Python script, or a scraping API with structured export — and the right one depends almost entirely on how many listings you need and whether the reviews need to be complete or just "what's on the page."

This article walks through all three, then goes deep on the API route using ScraperAPI, whose Amazon Reviews endpoint exports review data in JSON or CSV and prices every Amazon page at a flat 5 API credits. Before the details, here's the short version.

> **Quick pick:** If you need reviews from more than a handful of listings, an API is the only route that scales. ScraperAPI's Hobby plan ($49/month) covers 100,000 credits — about 20,000 Amazon pages, since every Amazon request costs 5 credits — and a 7-day trial with 5,000 credits requires no credit card. 👉 [Start the free trial and test the CSV export first](https://www.scraperapi.com/signup?fp_ref=coupons)

## Three ways to get Amazon reviews into a CSV file

There are exactly three practical methods, and each has a hard ceiling you should know before committing.

**1. Browser extensions.** Tools like AmzDL sit inside your own Chrome session, read the review pages as you browse, and output both .xlsx and .csv files with fields like star rating, title, full review text, author, date, verified-purchase flag, product variant, and helpful votes. Review export there is a Pro feature at $7.99/month or a $79.99 one-time license. This is the fastest option for a dozen listings this week — no API key, no code, no proxy.

**2. Python scripts.** Open-source examples like ScrapingBee's official amazon-review-scraper repo show the pattern: send a request per product URL, let the API handle proxies and headless rendering, parse the review list with CSS selectors, and write rows (name, rating, date, title, body) to a CSV with pandas. You need Python 3.8+ and an API key; ScrapingBee gives 1,000 free credits to evaluate.

**3. Scraping APIs with structured endpoints.** ScraperAPI's Amazon Reviews API takes a product ASIN and returns review and rating data in JSON or CSV, with the anti-bot work (proxy rotation, CAPTCHA handling, automated retries) handled server-side. This is the route for anything beyond casual volume: hundreds of ASINs, scheduled monitoring, or feeding a data pipeline.

## Why Amazon reviews are genuinely harder to scrape than normal pages

The blockers are structural, and they explain why "just parse the HTML" tutorials keep failing.

First, Amazon's review widget is lazy-loaded. It doesn't exist in the initial HTML — it appears after JavaScript scrolling. As the ScrapingBee repo puts it plainly: "The hard part is not parsing the HTML. It is loading the lazy review widget reliably in a headless browser, rotating IPs so Amazon does not block you, and matching the right regional storefront." Any tool that skips this gets empty or partial exports.

Second, there's a volume ceiling most people discover too late — and it comes from Amazon itself, not from any tool. Amazon shows at most about 10 review pages per sort order, which works out to roughly 100 reviews per listing. A product with 12,000 ratings still only exposes around 100 reviews in its default "Top reviews" order. No browser extension can get past this, because the reviews simply aren't rendered on the page. Only API-based tools that paginate through dedicated review endpoints can pull deeper history.

Third, Amazon fingerprints aggressively. A US IP requesting amazon.de is an obvious mismatch, so serious tools tie geo-targeting to the storefront you're scraping. Never log in to an account for scraping — authenticated scraping violates Amazon's terms, and public review data is the defensible boundary.

## ScraperAPI plans and pricing: the full table

ScraperAPI prices by API credits, not by page. All paid plans include JavaScript rendering, premium proxies, CAPTCHA handling, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee. Annual billing takes 10% off every tier. Here's the complete lineup currently shown on the official pricing page:

| Plan | Monthly price | Annual (per month) | API credits | Concurrent threads | Buy |
| --- | --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000 | 5 | [Sign up for 1,000 free credits](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Hobby | $49 | $44.10 | 100,000 | 20 | [Start the Hobby 7-day trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [Start the Startup 7-day trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [Start the Business 7-day trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [Start the Scaling 7-day trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [Start the Professional 7-day trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [Start the Advanced 7-day trial](https://www.scraperapi.com/signup?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22M+ | 500+ | [Contact sales for a custom plan](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

All paid plans also come with a 7-day trial including 5,000 API credits, no credit card required — so you can confirm the CSV output matches what you need before anything bills.

For perspective on where this sits in the market: ScraperAPI's own comparison content notes that Oxylabs charges $249/month for about 104,000 results, while ScraperAPI's Business plan at $299/month delivers 600,000 successful requests on Amazon. Those numbers come from ScraperAPI's blog, so treat the framing accordingly, but the credit math on both sides is public.

## What your credits actually buy: the 5-credit math nobody mentions

Here's the detail most pricing comparisons skip: 100,000 credits does not mean 100,000 pages.

ScraperAPI charges by request type. A standard page costs 1 credit. **Amazon costs 5 credits per request** — whether that request is a product page or a page of reviews. Google and Bing cost 25, LinkedIn 30, and sites behind Cloudflare, Datadome, or PerimeterX add 10 credits per request when the protection is bypassed. The pricing page lets you check the exact cost of any URL with a Domain Cost Estimator, and you can set a `max_cost` per scrape so a single job can't blow through your balance.

Run the numbers for review scraping specifically:

- **Hobby (100,000 credits)** ≈ 20,000 Amazon pages
- **Startup (1,000,000 credits)** ≈ 200,000 Amazon pages
- **Business (3,000,000 credits)** ≈ 600,000 Amazon pages

How many products that translates to depends on how deep you go into each listing's reviews — the credits are spent per request, so a product page plus five review pages costs 30 credits, not 5. Budget accordingly.

Two more things that affect real cost. Failed requests get their credits refunded — you pay for successes, which matters a lot when a target site is flaky. And if your credits run out mid-month, the default behavior is a pause until renewal, not a surprise overage bill; auto-upgrade to the next tier is opt-in.

## Hidden limits that change which plan you should pick

These restrictions live on the pricing page and in plan details rather than in marketing copy, and they're the reason the cheapest plan is sometimes the wrong one.

- **Geotargeting on Hobby and Startup is US & EU only.** If you need reviews from regional storefronts outside those zones — and review content differs by country — you need Business ($299) or above, which unlocks global geotargeting across a 40M+ IP pool in 50+ countries. ScraperAPI's Amazon endpoints also cover 21 international marketplaces, including amazon.com, .co.uk, .de, .co.jp, and .com.au.
- **Analytics history is 30 days on Hobby and Startup**, unlimited from Business up. If you're monitoring sentiment over months rather than doing one-off pulls, this matters.
- **Pay-as-you-go past your credit limit is only available on Scaling and above.** On lower tiers, you either wait for renewal or upgrade.
- **DataPipeline projects cap at 10,000 Amazon product pages each** — generous, but worth knowing if you're scheduling recurring bulk jobs.
- **Free plan caps at 5 concurrent connections**, and the Hobby plan runs 20. If your workflow sends hundreds of requests per second, you'll hit 429 responses (which cost nothing — you just retry after throttling down).

If you only scrape US listings on a moderate scale, none of these bite, and Hobby or Startup is the right call. 👉 [Check the current plan prices and trial terms](https://www.scraperapi.com/pricing?fp_ref=coupons)

## Getting reviews to CSV with ScraperAPI, step by step

The practical flow looks like this:

1. **Sign up and grab your API key.** The 7-day trial includes 5,000 credits with no card required; the permanent free plan adds 1,000 credits at 5 concurrent connections.
2. **Point the Amazon Reviews structured endpoint at your ASINs.** Structured Data Endpoints return clean JSON for Amazon without you writing parsers — and review data can be exported as CSV. The structured format is the difference between "a blob of HTML" and a spreadsheet-ready file.
3. **Set your parameters.** Match your geo-targeting to the storefront you're scraping. Set a `max_cost` if you want a hard ceiling on any single job.
4. **Schedule recurring pulls if needed.** DataPipeline handles up to 10,000 Amazon product pages per project on a visual interface or dedicated endpoints; the Async Scraper Service takes a POST request and processes millions of URLs in the background.
5. **Load the CSV into your analysis tool.** Pivot tables, Google Sheets, or an LLM prompt all work directly without cleaning.

One workflow tip worth borrowing from the scraper community: reviews rarely change within 24 hours. Cache results locally and only re-scrape what's actually moved, and your credit budget stretches considerably further.

## How the three routes compare side by side

|  | Browser extension | Python script + scraping API | ScraperAPI (Amazon SDE) |
| --- | --- | --- | --- |
| Setup effort | Install and browse | Write and maintain code | One HTTP request per page |
| Reviews per listing | ~100 (Amazon's page cap) | Depends on implementation | Deep pagination via review endpoint |
| Anti-bot handling | Your own browser session | Handled by the underlying API | Server-side, incl. DataDome/PerimeterX bypass |
| Cost model | $7.99/mo or $79.99 once (AmzDL Pro) | Per-credit (ScrapingBee: 1,000 free) | 5 credits per Amazon page; failed requests refunded |
| Best for | A dozen listings, this week | Devs who want control | Recurring or large-scale review monitoring |

There's no single winner here. An extension is genuinely the better tool when you want this week's top reviews from a handful of listings and don't want to touch code. The honest limitation is scale: with Amazon capping rendered reviews at about 100 per listing, building a dataset of 50 products' full review history means 50 manual passes at best, and zero automation.

## FAQ

**How many reviews can I actually export from one Amazon listing?**
From a browser extension: about 100, because Amazon renders at most 10 review pages per sort order regardless of total rating count. Through an API that paginates a dedicated reviews endpoint, you can go deeper — but any tool claiming to export reviews Amazon doesn't display is using a different data source than the live page.

**Is scraping Amazon reviews legal?**
Public review data is the generally accepted boundary in the scraping ecosystem, and the tooling built around it (including the open-source repos referenced here) explicitly sticks to public pages and advises against authenticated scraping. For anything beyond personal research, review Amazon's Conditions of Use and your local regulations before republishing data.

**Do I need a headless browser to scrape Amazon reviews?**
Yes, in the sense that the review widget requires JavaScript — but not on your machine. Both ScraperAPI and ScrapingBee do the rendering server-side, so you never install Puppeteer, Playwright, or Selenium.

**What does ScraperAPI cost per 1,000 Amazon review pages?**
On Hobby, $49 buys 100,000 credits, which at 5 credits per Amazon page works out to about $2.45 per 1,000 pages; third-party analysis puts the Enterprise tier around $0.475 per 1,000.

## Choosing by scenario

- **You need a spreadsheet of this week's reviews from a few listings, today:** a Chrome extension does it fastest; AmzDL's export runs $7.99/month or $79.99 once and outputs both xlsx and csv.
- **You're a developer who wants full control over parsing and storage:** the ScrapingBee-style open-source pattern with a Python SDK gets you there, with 1,000 free evaluation credits.
- **You need scheduled monitoring, hundreds of ASINs, reviews from non-US storefronts, or CSV at scale without maintaining infrastructure:** this is where ScraperAPI's Amazon Reviews endpoint earns its price — flat 5 credits per page, credits refunded on failures, and a trial that costs nothing to verify. 👉 [Try the 7-day trial with 5,000 credits and export your first batch](https://www.scraperapi.com/signup?fp_ref=coupons)

Before you commit to any paid plan, do one last thing: estimate your real monthly volume in Amazon requests (product pages and review pages both count at 5 credits), check it against the table above, and run the Domain Cost Estimator on a sample listing to confirm. If you're inside Hobby's 20,000-page allowance and only scrape US or EU storefronts, start small and upgrade only when the geotargeting or concurrency limits actually get in your way.
