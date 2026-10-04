---
name: new-campaign
description: Create a CETV advertising campaign on digital screens in break rooms and venues. Use when the user wants to advertise on CETV, run a screen or digital-signage ad, buy a CETV package, or launch a campaign. Walks through package, dates, uploading their ad image, confirmation, and payment.
---

# Create a CETV campaign

Guide the user from "I want to advertise" to a created, paid campaign using the **CETV** tools. Ask for one or two things at a time and keep it conversational — most users are small-business owners, not marketers.

If the CETV tools aren't available or report that sign-in is needed, ask the user to connect CETV and sign in with their email (a one-time code), then continue.

## 1. Packages and timing

Call `list_packages` and present the packages in a short table: name, price, total plays, duration. Explain that plays are spread across CETV's screen network over the package's duration (packages differ in length and terms — longer ones can be better value), and each ad plays for 15 seconds.

Ask which package they want, and when it should start. The start date must be **after today in US Eastern time** — `list_packages` returns `today_us_eastern`; never offer today or an earlier date. Convert phrases like "next Monday" to `YYYY-MM-DD` and confirm the exact date back to them.

## 2. Business details

Collect:
- **Business name** (`company_name`) — required. It appears on their invoice. Always ask; don't guess it from their email.
- **Campaign name** — something they'll recognize later, e.g. "Fall Sale 2026". Suggest one if they don't care.
- **Notification email** — defaults to the email they signed in with. Only ask if they want a different one.

## 3. The ad (creative)

The ad is a single landscape **JPG or PNG** that the user provides; screens are 1920×1080, so a 16:9 image looks best.

An image attached to the chat can't be passed to the CETV tools directly, so upload it one of two ways:

- **In a chat app** (ChatGPT on web, desktop, or mobile): call `create_upload_link`, give them the link, and tell them it works once and expires in 30 minutes. When they say they're done, call `check_upload`. If the status isn't `uploaded` yet, ask them to finish on the page; if it's `expired`, make a new link.
- **Where you can run shell commands and the file is on disk** (e.g. Codex): call `get_upload_url` with the filename and `image/png` or `image/jpeg`, upload with the returned `curl` command (substituting the real path), and keep the returned `creative_url`. Check the file type first.

If they don't have an ad image yet, let them know they'll need one (a JPG or PNG, ideally 1920×1080) and can come back once it's ready.

## 4. Confirm before creating

Creating the campaign sends a **real invoice**, so always show a summary and get an explicit yes first:

> **Business:** … · **Campaign:** … · **Package:** … ($…, … plays over … days) · **Starts:** … · **Ad:** (the image) · **Notifications to:** …

If they have a **promo code**, include it — a valid code makes the campaign free and no invoice is sent.

## 5. Create and pay

Call `create_campaign` with `company_name`, `campaign_name`, `package_id`, `start_date`, and either `upload_id` (from an upload link) or `creative_url` (from `get_upload_url`), plus `promo_code` / `notification_email` if given.

- **Invoiced** (status `payment_pending`): give them the `payment_link` from the result prominently — they can pay by card right away. Mention the invoice was also emailed. The campaign starts on the start date once it's paid.
- **Promo code** (status `pending`): confirm it's scheduled to start on the start date.

If a tool returns an error, explain it plainly and fix the input (for example, pick a later start date, or re-upload) rather than retrying the same call. Calling `create_campaign` again with identical details returns the same campaign — it never double-charges.

## 6. Wrap up

Tell them they can ask "how is my CETV campaign doing?" any time to see plays delivered (the report updates hourly once the campaign is running), or ask for the payment link again later.
