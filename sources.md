# Sources for the AI daily briefing

Paste this to the Replit agent.

> Add these RSS feeds to the briefing app as its sources. Keep the source list
> internal: never show it in the briefing UI. All URLs were tested live with
> feedparser and return stories. For a daily briefing, 6 to 8 high-signal feeds
> beat a big pile, so start with the Core set and add the rest only if you want
> more depth.

## Core set (add these first)

- TechCrunch AI — https://techcrunch.com/category/artificial-intelligence/feed/
- VentureBeat AI — https://venturebeat.com/category/ai/feed/
- The Verge — https://www.theverge.com/rss/index.xml
- Hacker News (AI filter) — https://hnrss.org/newest?q=AI+OR+LLM&points=100
- Wired AI — https://www.wired.com/feed/tag/ai/latest/rss
- MIT Technology Review — https://www.technologyreview.com/feed/
- Google AI blog — https://blog.google/technology/ai/rss/
- Import AI (weekly analysis) — https://importai.substack.com/feed

## Primary sources (the AI labs themselves)

Good for the cross-day "follow-up from..." linking, since lab announcements often
become the stories the press covers next.

- OpenAI — https://openai.com/news/rss.xml
- Google DeepMind — https://deepmind.google/blog/rss.xml
- Hugging Face — https://huggingface.co/blog/feed.xml

## General tech (optional, broader context)

- Ars Technica — https://feeds.arstechnica.com/arstechnica/index
- Engadget — https://www.engadget.com/rss.xml
- Simon Willison (practitioner) — https://simonwillison.net/atom/everything/
- Hacker News front page — https://news.ycombinator.com/rss

## Just the URLs (for seeding)

```
https://techcrunch.com/category/artificial-intelligence/feed/
https://venturebeat.com/category/ai/feed/
https://www.theverge.com/rss/index.xml
https://hnrss.org/newest?q=AI+OR+LLM&points=100
https://www.wired.com/feed/tag/ai/latest/rss
https://www.technologyreview.com/feed/
https://blog.google/technology/ai/rss/
https://importai.substack.com/feed
https://openai.com/news/rss.xml
https://deepmind.google/blog/rss.xml
https://huggingface.co/blog/feed.xml
https://feeds.arstechnica.com/arstechnica/index
https://www.engadget.com/rss.xml
https://simonwillison.net/atom/everything/
https://news.ycombinator.com/rss
```
