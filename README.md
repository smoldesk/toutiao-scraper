# Toutiao Scraper

![Toutiao Scraper: news articles, videos, comments and the hot board (热榜) without login](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/toutiao/banner.webp)

Scrape Toutiao (今日头条) news search, comments and the hot board (热榜). No login, no cookies, no API key. Runs on [Apify](https://apify.com/smoldesk/toutiao-scraper).

## What you get

- **Keyword news search** in Chinese (or English brand names), within a date range. About 100 results per keyword.
- **Articles and videos**: title, summary, publish time, author and verification, reads, plays, likes, comments, shares, saves, cover image and Toutiao's content categories.
- **Comments and replies** for search results or any article link: text, likes, author and province.
- **Hot board (热榜)**: rank, topic, heat score, label, image and link. Schedule it hourly to track a story.
- **Only new results** mode for daily or hourly keyword monitoring.
- A clean `markdown` field per article for LLM and RAG pipelines.

## Run it

Open the [Toutiao Scraper on Apify](https://apify.com/smoldesk/toutiao-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~toutiao-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "searchQueries": ["比亚迪", "新能源汽车"],
    "postedAfter": "2026-10-01",
    "postedBefore": "2026-10-31",
    "maxResultsPerSearch": 100,
    "includeComments": true,
    "maxCommentsPerArticle": 20
  }'
```

Get the hot board only: `{"trending": true}`.

## Sample output

```json
{
  "type": "article",
  "url": "https://www.toutiao.com/article/7703867117984154152/",
  "title": "胡晓庆揭秘比亚迪仰望U9X极速测试惊险瞬间：360km/h撞击护栏、试车员下车不去医院“搁那看”",
  "publishedAt": "2026-11-03T08:59:07+00:00",
  "authorName": "IT之家",
  "authorVerification": "IT之家官方账号",
  "readCount": 11426,
  "likesCount": 108,
  "commentsCount": 63,
  "savesCount": 12,
  "searchQuery": "比亚迪"
}
```

```json
{
  "type": "trend",
  "rank": 7,
  "title": "黄金从暴涨到暴跌有哪些启示",
  "heat": 14441430,
  "label": "interpretation",
  "url": "https://www.toutiao.com/trending/7703953161530117686/"
}
```

Export as JSON, CSV or Excel, or pipe it into Google Sheets, Make, Zapier or n8n.

## Pricing

Pay per result. No monthly fee.

| Result | Per 1,000 |
|---|---|
| Article or video | $2 |
| Comment or reply | $1 |
| Hot-board topic | $1 |

## Limits

- No full article body. Toutiao guards article pages with a browser check, so you get title, summary, counts and link.
- About 100 results per keyword, fewer with a date range. Split long periods into weeks.
- No author timelines or follower lists.

## FAQ

**Does Toutiao have a public API?**
No open API for search, comments or the hot board. This scraper reads the public pages and JSON any visitor's browser loads and returns clean JSON with English field names.

**How do I scrape Toutiao comments?**
Set `includeComments` to true for search results, or pass article/video links in `articleUrls`. `maxCommentsPerArticle` and `maxRepliesPerComment` control how deep it goes.

**Can I use it from Python?**
Yes. `pip install apify-client`, then call `client.actor("smoldesk/toutiao-scraper").call(run_input={...})` and read the dataset. Same input as the curl example above.

**Do I need a Toutiao account, a Chinese phone number or a VPN?**
No. Nothing to log in to and no proxy to set up.

**Can I track the Toutiao hot list over time?**
Yes. Schedule a run every hour with `"trending": true` and compare `rank` and `heat` between runs.

## More scrapers

Toutiao is one of 15 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
