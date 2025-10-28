# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Social Media Trends Analysis System** - a Python microservices platform that identifies, analyzes, and reports trending topics across multiple social media platforms (YouTube, TikTok, Instagram, Facebook) based on configurable guidelines (e.g., "finances", "technology", "health").

The system provides demographic segmentation by platform, geographic location, age range, and gender using NLP (Natural Language Processing) for automatic topic identification, sentiment analysis, and named entity recognition.

## Project Status

**Current Phase**: Planning & Documentation
- ✅ Conceptual planning complete
- ✅ JSON response structure defined
- ✅ **Free tools research complete** (10 free APIs + integrations identified)
- ✅ Technical stack decisions made
- ⏳ Technical implementation pending

## Core Architecture

### Microservices Design

The system follows a microservices architecture with these primary components:

1. **API Gateway** (FastAPI) - Single entry point, authentication, rate limiting
2. **Lineamientos Service** - CRUD for guidelines configuration (keywords, hashtags, platforms)
3. **Collectors** - One per social platform (YouTube, TikTok, Instagram, Facebook)
   - Each collector handles platform-specific API integration
   - Normalizes data to common format
   - Manages platform-specific rate limits
4. **NLP Service** - Text processing, topic modeling (BERTopic), sentiment analysis, NER
5. **Analytics Service** - Data aggregation, metrics calculation, demographic analysis
6. **Message Queue** (Celery + RabbitMQ/Redis) - Asynchronous task orchestration
7. **PostgreSQL** - Primary data store with TimescaleDB extension for time series
8. **Redis** - Caching layer and message broker

### Data Flow

```
User Request → API Gateway → Lineamientos Service (config)
                           → Analytics Service (queries)

Background: Message Queue → Collectors (parallel) → PostgreSQL (raw data)
                         → NLP Service (processing) → PostgreSQL (topics)
                         → Analytics (aggregation) → Redis (cache)
```

### API Response Hierarchy

All trend analysis endpoints return data in this 4-level hierarchy:
1. **Platform** (youtube, tiktok, instagram, facebook)
2. **Geographic Location** (country → region → city)
3. **Age Range** (18-24, 25-34, 35-44, etc.)
4. **Gender** (masculino, femenino, no_binario, desconocido)
   - Contains: identified topics, metrics, content examples

See `documentacion/ESTRUCTURA_JSON_RESPUESTA.md` for complete JSON schema.

## Data Sources (10+ Total)

### Core Strategy: FREE Tools First

The project prioritizes **100% free data sources** for MVP, with paid alternatives only for Twitter (official API is $100/mo).

**Cost Structure**:
- **Option A (100% Free)**: $0/mo - 7+ platforms using free APIs + scraping
- **Option B (Hybrid)**: $20/mo - All free tools + TwitterAPI.io ($0.15/1k tweets)
- **vs Enterprise competitors**: Brandwatch ($1,000+/mo), Sprinklr ($2,000+/mo)

### Social Media Platforms (Primary)

**YouTube Data API v3**
- **Purpose**: Long-form educational content, tutorials
- **Data**: Videos, comments, engagement metrics
- **Rate Limit**: 10,000 units/day (free tier)
- **Demographics**: Inferred (no direct API access)

**TikTok Research API + Creative Center**
- **Purpose**: Short-form viral trends, Gen Z/Millennial audience
- **Data**: Videos, hashtags, engagement, trending audios
- **Rate Limit**: Research API requires approval; Creative Center is FREE
- **Demographics**: Limited, primarily geographic
- **Creative Center**: Official free tool for trending hashtags by category

**Instagram Graph API (Meta)**
- **Purpose**: Visual content, influencer marketing
- **Data**: Posts, hashtags, Instagram Insights
- **Rate Limit**: 200 requests/hour per user
- **Demographics**: Full access via Insights (age, gender, location)

**Facebook Graph API (Meta)**
- **Purpose**: Community discussions, public pages
- **Data**: Posts, reactions, page insights
- **Rate Limit**: 200 requests/hour per user
- **Demographics**: Most comprehensive (age, gender, location, education, interests)

### Additional FREE Platforms (Expanding Coverage)

**Reddit API (PRAW)**
- **Purpose**: Authentic conversations in specialized communities
- **Data**: Posts, comments, discussions by subreddit
- **Cost**: FREE (with reasonable limits)
- **Demographics**: 63.6% male, 41% ages 18-34, statistical
- **Value**: Deep discussions, honest opinions, niche communities (r/Colombia, r/personalfinance)

**Mastodon API**
- **Purpose**: Open-source decentralized alternative to Twitter/X
- **Data**: Toots (posts), real-time streaming, federated timeline
- **Cost**: FREE (100% open source, no rate limits if self-hosted)
- **Demographics**: Not directly available
- **Value**: Twitter alternative, chronological timeline, open API

**NewsAPI.org**
- **Purpose**: Context and news correlation with social trends
- **Data**: 150,000+ news sources, 55 countries, 14 languages
- **Cost**: FREE tier for development
- **Value**: Understand why topics are trending, track social shares of articles

**Snscrape / Socialreaper**
- **Purpose**: Web scraping when official APIs are unavailable/expensive
- **Data**: Twitter, Facebook, Instagram, Reddit without API keys
- **Cost**: FREE (open-source Python libraries)
- **Value**: Backup for Twitter (official API is $100/mo), no rate limits

**Pinterest Trends**
- **Purpose**: Visual/product trends, predictive search behavior
- **Data**: Trending searches by category
- **Cost**: FREE (manual access)
- **Demographics**: 80% female (implicit)
- **Value**: Users search 2-6 months before purchasing decisions

### Complementary Sources

**Google Trends (CRITICAL)**
- **Purpose**: Search intent data, trend validation, content gap detection
- **Data**: Interest over time, related queries, geographic distribution
- **Tool**: pytrends (Python library)
- **Cost**: 100% FREE (confirmed)
- **Why Critical**: Captures active search intent (not passive consumption), predicts trends 2-4 weeks early, validates if social media trends are real or algorithmic bubbles

**Talkwalker Alerts**
- **Purpose**: FREE Google Alerts alternative for brand/keyword monitoring
- **Data**: Mentions across 150M+ websites, blogs, forums, news, Twitter
- **Cost**: FREE (unlimited alerts)
- **Value**: Complements APIs with web/blog coverage

**TikTok Creative Center**
- **Purpose**: Official TikTok trending data
- **Data**: Trending hashtags by industry, viral audios, top creators
- **Cost**: FREE (official TikTok tool)
- **Why Valuable**: Real-time trending data filtered by category (e.g., "finance")

## Database Schema

### Core Tables
- **lineamientos** - Guidelines configuration (keywords, hashtags, platforms)
- **contenido_recolectado** - Raw collected content from social platforms
- **temas_identificados** - Topics extracted by NLP
- **contenido_tema** - Many-to-many relationship (content ↔ topics)
- **demografia_tema** - Demographic aggregations per topic
- **tendencias** - Time series data (TimescaleDB)

### Key Indexes
- Platform, publication date, lineamiento_id on `contenido_recolectado`
- Topic_id, platform, demographics on `demografia_tema`
- Date/time on `tendencias`

## NLP Pipeline

1. **Text Cleaning**: Remove stopwords, normalize, lemmatize (spaCy `es_core_news_lg`)
2. **Topic Modeling**: BERTopic for semantic topic extraction
3. **Named Entity Recognition**: Extract products, places, people
4. **Sentiment Analysis**: Score from -1 (negative) to +1 (positive)
5. **Classification**: Match content to configured guidelines

## Technology Stack

- **Backend**: Python 3.11+, FastAPI
- **Data Processing**: Pandas, NumPy
- **NLP**: spaCy (Spanish models `es_core_news_lg`), Transformers (BERT), BERTopic, Gensim
- **Social Media APIs** (Official):
  - `google-api-python-client` (YouTube Data API v3)
  - `facebook-sdk` (Instagram/Facebook Graph API)
  - `praw` (Reddit API)
  - `Mastodon.py` (Mastodon API)
  - `newsapi-python` (NewsAPI.org)
- **Social Media APIs** (Alternative/Scraping):
  - `pytrends` (Google Trends - unofficial but reliable) **100% FREE**
  - `snscrape` (Twitter/FB/IG scraping without API keys) **FREE**
  - `socialreaper` (Multi-platform scraping) **FREE**
  - TwitterAPI.io or Apify (paid alternatives: $0.15-0.25/1k tweets)
  - TikTok Creative Center (manual/Selenium scraping)
- **Web Scraping**: BeautifulSoup4, Selenium, Requests
- **Database**: PostgreSQL 15+ with TimescaleDB extension
- **Cache**: Redis
- **Task Queue**: Celery
- **Message Broker**: RabbitMQ or Redis
- **Containerization**: Docker + Docker Compose

## Development Phases

### Phase 1: MVP (4-6 weeks) - 100% FREE Stack
- Infrastructure setup (Docker, PostgreSQL, Redis)
- Basic API Gateway (FastAPI)
- Lineamientos CRUD
- **FREE Collectors (5)**:
  - Meta Graph API (Facebook/Instagram) - Demographics ✅
  - YouTube Data API v3
  - Reddit API (PRAW)
  - Google Trends (pytrends)
  - Talkwalker Alerts (email parsing)
- **Optional**: Snscrape for Twitter (free scraping) OR TwitterAPI.io ($20/mo)
- Basic NLP (LDA topic modeling, spaCy Spanish)
- Simple query API with 4-level hierarchy
- **Cross-validation**: Compare social trends with Google Trends search data
- **First insight**: Content gap detection + demographic segmentation

### Phase 2: Multi-Platform Enhancement (4-6 weeks)
- TikTok Creative Center (scraping/manual)
- Mastodon API integration
- NewsAPI.org for news context
- Celery task queue for async processing
- Advanced NLP (BERTopic, sentiment analysis with Transformers)
- **Demographic Inference Service**:
  - Direct: Facebook/Instagram API demographics
  - Inferred: Twitter (NLP on bios/locations), YouTube (comments analysis)
  - Statistical: TikTok (platform-wide data from Statista/DataReportal)
- Full hierarchical JSON response structure
- **Correlation insights**: Multi-platform trend validation, early prediction

### Phase 3: Production Ready (4-6 weeks)
- ML-based demographic inference
- Time series and trending detection
- Real-time alerts
- Report exports (PDF, Excel)
- Advanced dashboard
- Performance optimization
- Comprehensive testing

## Key Metrics

- **Engagement Rate**: (Likes + Comments + Shares) / Views × 100
- **Trending**: Growth velocity > 15% in 24h + min 50 mentions + above-average engagement
- **Sentiment**: Range -1 to +1 (negative to positive)
- **Velocity**: % growth vs previous period

## Important Notes

- All text processing optimized for **Spanish language** (spaCy `es_core_news_lg`)
- **Cost-effective strategy**: MVP can be built with $0-20/mo (vs $1,000+ for competitors)
- **Google Trends is 100% FREE** (confirmed): Captures search intent, validates trends, predicts 2-4 weeks early
- **Twitter challenge**: Official API is $100/mo minimum. Solutions:
  - Option A: Snscrape (free scraping)
  - Option B: TwitterAPI.io ($0.15/1k tweets = $20/mo for 133k tweets)
- Demographic data precision varies by platform:
  - **Direct (95-98%)**: Facebook/Instagram via Meta Graph API
  - **Inferred (60-80%)**: Twitter (NLP), YouTube (comments + Trends correlation)
  - **Statistical (50-60%)**: TikTok (apply platform-wide demographics)
- Rate limits are strict - implement aggressive caching and multiple API keys rotation
- Data retention: raw content stored 6 months, then archived to object storage
- API responses use materialized views (refreshed hourly) for performance
- **10+ data sources**: 4 core social platforms + Reddit + Mastodon + NewsAPI + Google Trends + Talkwalker + TikTok Creative Center + Pinterest Trends

## Documentation

### Strategic Planning
- `documentacion/PLANIFICACION_CONCEPTUAL.md` - Complete conceptual planning with Google Trends & TikTok Creative Center integration
- `documentacion/ESTRUCTURA_JSON_RESPUESTA.md` - API response structure specification (4-level hierarchy)
- `documentacion/HERRAMIENTAS_ANALISIS_TENDENCIAS.md` - Analysis of 18 marketing/trends tools (Google Trends, Brandwatch, VidIQ, etc.)

### **NEW: Free Tools Research** ⭐
- `documentacion/HERRAMIENTAS_PLANIFICACION_CONTENIDOS.md` - **Comprehensive research on FREE data collection tools**
  - **10 free APIs identified** with strategic analysis (WHAT, WHY, FOR WHAT)
  - Detailed comparison: Free vs Paid alternatives
  - Platform-specific strategies for demographic inference
  - Cost analysis: $0/mo (free stack) vs $20/mo (hybrid) vs $1,000+/mo (competitors)
  - Integration strategies (NO code implementation - strategic document only)
  - MCP (Model Context Protocol) servers available for social media analytics
  - **Key findings**:
    - Meta Graph API: Only free source with complete demographics
    - Twitter: Official API $100/mo → Use TwitterAPI.io ($0.15/1k) or Snscrape (free scraping)
    - TikTok: No API for demographics → Use Creative Center + statistical demographics
    - Reddit, Mastodon, NewsAPI: Free APIs with specific use cases
    - Google Trends: 100% free (pytrends library)

### Technical Implementation
- Next: Collectors implementation guide (pending)
- Next: NLP pipeline implementation (pending)
- Next: Demographic inference system (pending)
