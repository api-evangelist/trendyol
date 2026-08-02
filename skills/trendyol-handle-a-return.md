---
name: Handle a customer return (claim)
description: Retrieve return claims, approve returned items, or raise a rejection request.
api: https://developers.trendyol.com/reference/getclaims
operations: [getClaims, approveClaimLineItems, getClaimIssueReasons, createClaimIssue]
---

# Handle a customer return (claim)

## Auth
- HTTP Basic + mandatory `User-Agent` header; base URL `https://apigw.trendyol.com/integration`.

## Steps
1. **Pull claims.** Call `getClaims` to list return/claim orders (page-number pagination).
2. **Approve.** For items returned to your warehouse, call `approveClaimLineItems` to accept the return.
3. **Or reject.** To dispute an item: call `getClaimIssueReasons` to get a valid `claimIssueReasonId`, then `createClaimIssue` with that reason and attach evidence (pdf/jpeg) as multipart `form-data (file)`.
4. **Audit (optional).** Use `getClaimItemAudits` to inspect a claim's status progression and who acted.

## Rules
- Errors: `{ "errors": [ { "key", "message" } ] }`. Rate limit 50 req/10s → 429 `too.many.requests`.
