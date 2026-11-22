# Customer Order Agent

A Make-based conversational ordering prototype for a sample stationery shop. Customers request multiple products, review official prices, and explicitly approve a draft before it becomes a confirmed order.

## Features
- Natural-language product questions and multiple-item order previews.
- Official catalogue prices calculated by the order automation.
- Stored drafts and explicit conversational approval.
- Confirmation of the reviewed draft using its draft_id.
- Handling for repeated confirmation, changed orders, and declined approval.
- Saved-order lookup and business knowledge tools.

## Architecture
Webpage → agent webhook → Make AI Agent → HTTP tool → order automation → data store → response.

The preview path iterates through items, calculates official prices, aggregates the results, and stores an awaiting_confirmation draft. The confirmation tool sends action=confirm_draft with that draft's ID. The automation checks the record and updates record_type from draft to order and status from awaiting_confirmation to confirmed. An already-confirmed route returns the existing result.

The model handles conversation and tool selection. The automation calculates prices and updates records. Confirmation does not establish payment or delivery.

## Repository contents
- index.html: standalone agent chat extracted from the existing project webpage; the legacy order form is excluded.
- business-knowledge.txt: sample catalogue and policies.
- agent-tests.txt: original manual test checklist.
- CASE-STUDY.md: diagnosis, improvements, and validation notes.
- LINKEDIN.md: profile and post copy.

## Setup
This repository is a frontend and implementation case study, not an export of the Make scenarios. Scenario blueprints are not included. Recreate/configure the Make agent and order workflows before running it.

1. Configure an agent webhook accepting message and conversation_id.
2. Connect the knowledge, order lookup, preview, and confirmation tools.
3. Have the agent scenario return its customer-facing answer as text.
4. Replace YOUR_AGENT_WEBHOOK_URL in index.html locally with your agent webhook.
5. Serve the page locally and test against your own Make setup. The browser must be allowed to read the webhook response.

Do not commit live webhook URLs or credentials. Browser-side URLs remain visible to visitors; removing them from GitHub is not authentication.

## Validation and limits
The project owner reported passing preview/confirmation, repeated confirmation without a duplicate, changed-order reapproval, and declined-confirmation tests. These are manual reports, not automated test results. The extracted standalone page has not been retested against the live workflows.

This is a learning/capstone prototype built with AI assistance. Production work still includes authenticated customer/session ownership, draft expiry, stronger approval enforcement, rate limits, monitoring, and broader failure/concurrency tests. Prompt rules alone do not guarantee authorization or resistance to every prompt-injection attempt.
