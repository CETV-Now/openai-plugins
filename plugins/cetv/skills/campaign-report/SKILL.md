---
name: campaign-report
description: Report on the user's CETV advertising campaigns — status, plays delivered versus the package, daily plays, and payment status. Use when the user asks how their CETV campaign or screen ads are doing, wants a CETV report, or asks whether a campaign has started, been paid, or finished.
---

# CETV campaign report

Use the **CETV** tools. If they aren't available or report that sign-in is needed, ask the user to connect CETV and sign in, then continue.

## 1. Find the campaign

Call `list_campaigns`. If there's exactly one, use it. If there are several, show a compact list (name, status, start–end dates) and ask which one — or summarize all of them briefly if they asked about "my campaigns" in general. If there are none, say so and offer to create one.

## 2. Explain the status

| Status | What to tell the user |
|---|---|
| `payment_pending` | Waiting for payment. Call `get_invoice_link` and share the `payment_link`. It starts on its start date once paid. |
| `pending` | Paid (or free with a promo code) and scheduled — it goes live on its start date. |
| `active` | Running now. Show the report below. |
| `delivered` | Finished — every play in the package has been delivered. Show the final report. |
| anything else | Report it as-is and suggest contacting CETV support (info@cetvnow.com) if it looks wrong. |

## 3. Plays report (active or delivered)

Call `get_campaign_report` with the campaign id. Present:

- **Plays so far:** `total_plays` of `package_plays` (as a percentage too).
- **Pace:** compare progress to time elapsed between the start and end dates. Say whether it's on track, ahead, or behind — plays are paced evenly per day, so small day-to-day variation is normal.
- **Daily plays:** the `daily` buckets as a short table or simple chart, most recent days last. Dates are CETV's broadcast days.
- **Last updated:** `updated_at` — the report refreshes hourly. If `status` is `not_started`, explain that the first numbers appear within about an hour of the campaign going live.

Keep it brief and plain-language; offer more detail rather than leading with it.

Campaigns created from any app the user has connected to CETV (ChatGPT, Claude, and others) all appear here — it's one CETV account.
