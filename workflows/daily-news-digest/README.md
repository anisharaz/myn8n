# Daily News Digest

Every day at 6:00 AM IST: read 15 RSS feeds (10 technical, 5 world news), keep items from the last 26 hours, let an AI agent pick and summarize what matters to you, and email one HTML digest (TL;DR, Technical, World News).

```
Trigger -> Feed List -> Read RSS -> Filter & Normalize -> Digest Agent -> Render HTML -> Send Email (SMTP)
                                                           (LLM sub-node)
```

## Setup after import

1. Create an OpenAI API credential (any OpenAI-compatible endpoint: set its Base URL and API key) and select it on the **LLM (OpenAI-compatible)** node. Set the model name there (for example `auto` on a router gateway).
2. In **Digest Agent** > System Message, replace `[PASTE YOUR INTERESTS HERE]`.
3. In **Send Email (SMTP)**, set the SMTP credential and the from/to addresses.
4. Run once manually to test, then activate the workflow. Timezone is set to Asia/Kolkata in workflow settings.

## Editing feeds

Edit the `feeds` object in the **Feed List** code node. The category key (`technical` or `world`) decides the digest section.

## Notes

- Items older than 26h are dropped (arXiv is exempt because its dates are daily batches). Each feed is capped at 20 items (arXiv 25) to protect the model's context window. Change `WINDOW_MS` / `CAP` in **Filter & Normalize**.
- A feed that fails (blocked, down) is skipped and the rest still go out. Some sites block n8n's default user agent, so check the first run's Read RSS output.
- The agent returns plain JSON text and **Render HTML** parses it and builds the email, so the layout stays the same every day. If the JSON is invalid the run fails and the failure alert fires.
- Set `errorWorkflow` by importing `workflows/failure-alert` and selecting it under Settings > Error workflow.
