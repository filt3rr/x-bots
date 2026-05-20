# FutureButNotNow

A suite of fully automated Twitter/X bots that run on a daily schedule via GitHub Actions. Each bot targets a different content angle — viral Reddit trends, Amazon affiliate products, and polarizing news commentary — with Slack notifications on every run.

---

## Bots

### TrendParasite
Scrapes Reddit hot posts across multiple subreddits, scores each by virality and recency, then uses GPT-4 to generate an opinionated, context-aware tweet. Maintains a 24-hour deduplication cache across runs to avoid repeating topics.

**Schedule:** 2× daily — 2:00 PM and 6:30 PM EST

### ProductBot
Picks a curated product from a rotating list and uses OpenAI to write a compelling tweet with an Amazon affiliate link. Tracks used products to avoid repeats and logs every post to CSV.

**Schedule:** 1× daily — 9:00 AM EST

### RightLeftBot
Fetches the top US news headline and generates a sharply polarizing tweet from either a left-leaning or right-leaning perspective using OpenAI. Political angle is randomized per run.

**Schedule:** On demand (currently disabled from auto-schedule)

---

## Stack

| Layer | Technology |
|-------|-----------|
| Language | Python 3.11 |
| Tweet generation | OpenAI GPT-4 / GPT-4o-mini |
| Twitter/X posting | Tweepy (API v2) |
| Reddit data | PRAW |
| News data | newsdata.io |
| Scheduling | GitHub Actions (cron) |
| Alerts | Slack Webhooks |

---

## Project Structure

```
FutureButNotNow/
├── trendparasite/
│   ├── trend_sniffer.py        # Reddit scraping, scoring, GPT-4 tweet gen
│   └── requirements.txt
├── productbot/
│   ├── productbot_git.py       # Affiliate tweet generation and posting
│   ├── product_list.txt        # Curated product list
│   └── requirements.txt
├── Product Bot V2/
│   ├── product_bot_v2.py
│   ├── productbot-metrics.py
│   └── productbot-post.py
├── RightLeftBot/
│   ├── rightleftbot.py         # News fetch + polarizing tweet gen
│   └── requirements.txt
├── utils/
│   └── slack_notifier.py       # Shared Slack alert utility
└── .github/workflows/
    └── futurebutnotnow.yml     # Main scheduler (cron + manual dispatch)
```

---

## Setup

### 1. Clone

```bash
git clone https://github.com/filt3rr/FutureButNotNow.git
cd FutureButNotNow
```

### 2. Add GitHub Secrets

Go to **Settings → Secrets and variables → Actions** and add the following:

| Secret | Required by |
|--------|-------------|
| `OPENAI_API_KEY` | All bots |
| `TWITTER_API_KEY` | All bots |
| `TWITTER_API_SECRET` | All bots |
| `TWITTER_ACCESS_TOKEN` | All bots |
| `TWITTER_ACCESS_SECRET` | All bots |
| `REDDIT_CLIENT_ID` | TrendParasite |
| `REDDIT_CLIENT_SECRET` | TrendParasite |
| `REDDIT_USERNAME` | TrendParasite |
| `REDDIT_PASSWORD` | TrendParasite |
| `REDDIT_USER_AGENT` | TrendParasite |
| `NEWS_API_KEY` | RightLeftBot |
| `SLACK_WEBHOOK_URL` | All bots (optional) |

### 3. Run manually

Any bot can be triggered immediately from **Actions → FutureButNotNow Bot Scheduler → Run workflow** and selecting the bot from the dropdown.

---

## Schedule

| Bot | UTC | EST |
|-----|-----|-----|
| ProductBot | 13:00 | 9:00 AM |
| TrendParasite (afternoon) | 18:00 | 2:00 PM |
| TrendParasite (evening) | 22:30 | 6:30 PM |

Adjust timing by editing the cron expressions in `.github/workflows/futurebutnotnow.yml`.

---

## License

MIT