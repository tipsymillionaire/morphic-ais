# Configuration Guide

This guide covers the optional features and their configuration in Morphic.

## Table of Contents

- [Chat History Storage](#chat-history-storage)
- [Search Providers](#search-providers)
- [Additional AI Providers](#additional-ai-providers)
- [Other Features](#other-features)

## Chat History Storage

### Using Upstash Redis (Recommended for production)

Follow the detailed setup guide at [Building your own RAG chatbot with Upstash](https://upstash.com/blog/rag-chatbot-upstash#setting-up-upstash-redis)

1. Create a database at [Upstash Console](https://console.upstash.com/redis)
2. Navigate to the Details tab and find the "Connect your database" section
3. Copy the REST API credentials from the .env section
4. Configure your `.env.local`:

```bash
NEXT_PUBLIC_ENABLE_SAVE_CHAT_HISTORY=true
USE_LOCAL_REDIS=false
UPSTASH_REDIS_REST_URL=[YOUR_UPSTASH_REDIS_REST_URL]
UPSTASH_REDIS_REST_TOKEN=[YOUR_UPSTASH_REDIS_REST_TOKEN]
```

### Using Local Redis

1. Ensure Redis is installed and running locally
2. Configure your `.env.local`:

```bash
NEXT_PUBLIC_ENABLE_SAVE_CHAT_HISTORY=true
USE_LOCAL_REDIS=true
LOCAL_REDIS_URL=redis://localhost:6379
```

## Search Providers

### SearXNG Configuration

SearXNG can be used as an alternative search backend with advanced search capabilities.

#### Basic Setup

1. Set up SearXNG as your search provider:

```bash
SEARCH_API=searxng
SEARXNG_API_URL=http://localhost:8080
SEARXNG_SECRET=""  # generate with: openssl rand -base64 32
```

#### Docker Setup

1. Ensure you have Docker and Docker Compose installed
2. Two configuration files are provided in the root directory:
   - `searxng-settings.yml`: Contains main configuration for SearXNG
   - `searxng-limiter.toml`: Configures rate limiting and bot detection

#### Advanced Configuration

1. Configure environment variables in your `.env.local`:

```bash
# SearXNG Base Configuration
SEARXNG_PORT=8080
SEARXNG_BIND_ADDRESS=0.0.0.0
SEARXNG_IMAGE_PROXY=true

# Search Behavior
SEARXNG_DEFAULT_DEPTH=basic  # Set to 'basic' or 'advanced'
SEARXNG_MAX_RESULTS=50  # Maximum number of results to return
SEARXNG_ENGINES=google,bing,duckduckgo,wikipedia  # Comma-separated list of search engines
SEARXNG_TIME_RANGE=None  # Time range: day, week, month, year, or None
SEARXNG_SAFESEARCH=0  # 0: off, 1: moderate, 2: strict

# Rate Limiting
SEARXNG_LIMITER=false  # Enable to limit requests per IP
```

#### Advanced Search Features

- `SEARXNG_DEFAULT_DEPTH`: Controls search depth
  - `basic`: Standard search
  - `advanced`: Includes content crawling and relevance scoring
- `SEARXNG_MAX_RESULTS`: Maximum results to return
- `SEARXNG_CRAWL_MULTIPLIER`: In advanced mode, determines how many results to crawl
  - Example: If `MAX_RESULTS=10` and `CRAWL_MULTIPLIER=4`, up to 40 results will be crawled

#### Customizing SearXNG

You can modify `searxng-settings.yml` to:

- Enable/disable specific search engines
- Change UI settings
- Adjust server options

Example of disabling specific engines:

```yaml
engines:
  - name: wikidata
    disabled: true
```

For detailed configuration options, refer to the [SearXNG documentation](https://docs.searxng.org/admin/settings/settings.html#settings-yml)

#### Troubleshooting

- If specific search engines aren't working, try disabling them in `searxng-settings.yml`
- For rate limiting issues, adjust settings in `searxng-limiter.toml`
- Check Docker logs for potential configuration errors:

```bash
docker-compose logs searxng
```

## Additional AI Providers

### Google Generative AI

```bash
GOOGLE_GENERATIVE_AI_API_KEY=[YOUR_API_KEY]
```

### Anthropic

```bash
ANTHROPIC_API_KEY=[YOUR_API_KEY]
```

### Groq

```bash
GROQ_API_KEY=[YOUR_API_KEY]
```

### Ollama

```bash
OLLAMA_BASE_URL=http://localhost:11434
```

### Azure OpenAI

```bash
AZURE_API_KEY=[YOUR_API_KEY]
AZURE_RESOURCE_NAME=[YOUR_RESOURCE_NAME]
NEXT_PUBLIC_AZURE_DEPLOYMENT_NAME=[YOUR_DEPLOYMENT_NAME]
```

## Other Features

### Share Feature

```bash
NEXT_PUBLIC_ENABLE_SHARE=true
```

### Video Search

```bash
SERPER_API_KEY=[YOUR_API_KEY]
```

### Alternative Retrieve Tool

```bash
JINA_API_KEY=[YOUR_API_KEY]
```
