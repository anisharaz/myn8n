# myn8n

My n8n workflows, stored as JSON.

## Layout

```
workflows/<name>/workflow.json   exported workflow (credentials are never included)
workflows/<name>/README.md       what it does and how to set it up
docker-compose.yml               local self-hosted n8n
```

## Run n8n locally

```
cp .env.example .env
docker compose up -d      # http://localhost:5678
```

## Import / export

- Import: Workflows > Import from File > pick `workflow.json`
- CLI import: `docker compose exec n8n n8n import:workflow --input=/path/workflow.json`
- Export after editing in the UI: `docker compose exec n8n n8n export:workflow --id=<id> --output=/home/node/.n8n/out.json`, or use Download in the UI, then overwrite the file here.

## Workflows

| Workflow | Schedule | Description |
|---|---|---|
| [daily-news-digest](workflows/daily-news-digest) | 6 AM IST daily | RSS feeds, summarized by an AI agent, emailed |
