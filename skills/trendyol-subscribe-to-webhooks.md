---
name: Subscribe to order webhooks
description: Register a webhook endpoint to receive Trendyol order-package status events instead of polling.
api: https://developers.trendyol.com/reference/createwebhook
operations: [createWebhook, getWebhooks, updateWebhook, deactivateWebhook]
---

# Subscribe to order webhooks

## Auth
- HTTP Basic + mandatory `User-Agent` header; base URL `https://apigw.trendyol.com/integration`.

## Steps
1. **Create.** Call `createWebhook` with your public HTTPS callback URL and the order-package statuses you want (e.g. Created, Picking, Invoiced, Shipped, Delivered, Cancelled, Returned).
2. **Verify.** Call `getWebhooks` to confirm the subscription and its status.
3. **Update.** Use `updateWebhook` to change the URL or subscribed statuses.
4. **Pause/remove.** Use `deactivateWebhook` (and `activateWebhook`) to toggle delivery, or `deleteWebhook` to remove.

## Rules
- Delivery is at-least-once — make your handler idempotent on the order/package id.
- See asyncapi/trendyol-webhooks.yml for the event catalog and payload docs.
