# W4-D2 · LinkedIn Post Approval (n8n)

An n8n workflow that has AI draft a LinkedIn post, and saves it only after a human approves it.

## Flow

1. **On form submission**: a web form with one required field, `Topic`.
2. **Writer Agent** (Gemini 2.5 Flash): drafts a 120–150 word post with a hook in line one, one CTA and at most 3 hashtags.
3. **Reviewer Agent** (Gemini 2.5 Flash): scores the draft on hook, clarity, CTA and format, rewrites the weakest part and returns only the final post.
4. **Send for Approval** (Gmail, *Send message and wait for response*): emails the post with Approve and Reject buttons. The run waits up to 1 day.
5. **Approved?** (IF): checks `{{ $json.data.approved }}` is true.
6. **Save Approved Post** (Google Sheets, Append Row): writes `Date | Topic | Final Post | Status`.
   If you reject the post, nothing is written.

## Setup

1. In n8n, go to **Workflows → Import from File** and select `LinkedIn Post Approval.json`.
2. Add these credentials: Google Gemini API, Gmail OAuth2 and Google Sheets OAuth2.
3. In **Send for Approval**, replace `YOUR_EMAIL@gmail.com` with your address.
4. In **Save Approved Post**, select a sheet whose headers are `Date`, `Topic`, `Final Post` and `Status`.
