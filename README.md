# Toutiao Scraper

![Toutiao Scraper: news articles, videos, comments and the hot board (热榜) without login](https://raw.githubusercontent.com/smoldesk/apify-scrapers/main/assets/toutiao/banner.webp)

Scrape Toutiao (今日头条) news search, 微头条 short posts, comments and the hot board (热榜). No login, no cookies, no API key. Runs on [Apify](https://apify.com/smoldesk/toutiao-scraper).

## What you get

- **Keyword news search** in Chinese (or English brand names), within a date range. About 100 results per keyword.
- **Articles and videos**: title, summary, publish time, author and verification, reads, plays, likes, comments, shares, saves, cover image and Toutiao's content categories.
- **微头条 posts** (`"searchType": "posts"` or `"both"`): Toutiao's Weibo-style short posts with full text, images, hashtags, links, author and verification, reads, likes, comments and shares.
- **Comments and replies** for search results or any article, video or post link: text, likes, author and province.
- **Hot board (热榜)**: rank, topic, heat score, label, image and link. Schedule it hourly to track a story.
- **Only new results** mode for daily or hourly keyword monitoring.
- A clean `markdown` field per article and post for LLM and RAG pipelines.

## Run it

Open the [Toutiao Scraper on Apify](https://apify.com/smoldesk/toutiao-scraper), fill in the form and click Start. Or call it from code with your [Apify token](https://console.apify.com/settings/integrations):

```bash
curl -X POST "https://api.apify.com/v2/acts/smoldesk~toutiao-scraper/run-sync-get-dataset-items?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "searchQueries": ["比亚迪汽车", "新能源汽车"],
    "searchType": "both",
    "postedAfter": "2026-10-01",
    "postedBefore": "2026-10-31",
    "maxResultsPerSearch": 100,
    "includeComments": true,
    "maxCommentsPerArticle": 20
  }'
```

Ready-made tasks on Apify: [Monitor a brand on Toutiao (news + 微头条 posts, new only)](https://apify.com/smoldesk/toutiao-scraper/examples/toutiao-brand-monitoring) · [微头条 posts + comments for sentiment research](https://apify.com/smoldesk/toutiao-scraper/examples/toutiao-posts-comments-sentiment) · [Toutiao hot board (头条热榜), top 50](https://apify.com/smoldesk/toutiao-scraper/examples/toutiao-hot-board).

Get the hot board only: `{"trending": true}`. Posts only: `{"searchQueries": ["瑞幸咖啡"], "searchType": "posts"}`.

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
  "type": "post",
  "url": "https://www.toutiao.com/w/1881069730069524/",
  "text": "【瑞幸咖啡KPL联名“真人杯”引发消费者不满】瑞幸近期与《王者荣耀》KPL十周年联名，将十位电竞选手的真人头像印上杯身和外卖袋……",
  "publishedAt": "2026-11-06T01:18:09+00:00",
  "authorName": "餐宝典",
  "authorVerification": "头条新锐创作者",
  "readCount": 168,
  "hashtags": ["餐饮"],
  "searchQuery": "瑞幸咖啡"
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
| 微头条 post | $4 |
| Comment or reply | $1 |
| Hot-board topic | $1 |

## Limits

- No full article body. Toutiao guards article pages with a browser check, so you get title, summary, counts and link.
- About 100 results per keyword and tab, fewer with a date range. Split long periods into weeks.
- Post search ignores dates on Toutiao's side, so the scraper filters posts by date itself (skipped posts aren't charged).
- A few very broad brand names (比亚迪, sometimes 华为) are captcha-gated for logged-out visitors and get skipped without charge. Use a more specific keyword (比亚迪汽车, 华为手机).
- No author timelines or follower lists.

## FAQ

**Does Toutiao have a public API?**
No open API for search, comments or the hot board. This scraper reads the public pages and JSON any visitor's browser loads and returns clean JSON with English field names.

**How do I scrape Toutiao 微头条 posts?**
Set `"searchType": "posts"` (or `"both"` for news and posts) with your keywords. Each post comes with full text, images, hashtags, author, reads, likes, comments and shares.

**How do I scrape Toutiao comments?**
Set `includeComments` to true for search results, or pass article, video or post (`/w/`) links in `articleUrls`. `maxCommentsPerArticle` and `maxRepliesPerComment` control how deep it goes.

**Can I use it from Python?**
Yes. `pip install apify-client`, then call `client.actor("smoldesk/toutiao-scraper").call(run_input={...})` and read the dataset. Same input as the curl example above.

**Do I need a Toutiao account, a Chinese phone number or a VPN?**
No. Nothing to log in to and no proxy to set up.

**Can I track the Toutiao hot list over time?**
Yes. Schedule a run every hour with `"trending": true` and compare `rank` and `heat` between runs.

## More scrapers

Toutiao is one of 18 no-login scrapers by Smoldesk Studios. See the full list: [smoldesk/apify-scrapers](https://github.com/smoldesk/apify-scrapers).
