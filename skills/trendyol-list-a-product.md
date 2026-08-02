---
name: List a product on Trendyol
description: Create and publish a product on the Trendyol marketplace, then set its price and stock.
api: https://developers.trendyol.com/reference/createproducts
operations: [getCategoryTree, getCategoryAttributes, getBrands, createProducts, getBatchRequestResult, updatePriceAndInventory]
---

# List a product on Trendyol

Use the Trendyol Marketplace (Partner) API to create a product and make it sellable.

## Auth
- HTTP Basic: `Authorization: Basic base64(apiKey:apiSecret)`.
- Mandatory `User-Agent: {sellerId} - SelfIntegration` (or `{sellerId} - {CompanyName}`) — requests without it get 403.
- Base URL: `https://apigw.trendyol.com/integration`.

## Steps
1. **Resolve the category.** Call `getCategoryTree` and pick the deepest (leaf) `categoryId` — only leaf categories are valid.
2. **Resolve required attributes.** Call `getCategoryAttributes` for that `categoryId` to learn the required/allowed attributes (use `attributeValueIds` array, or `attributeValue` string for free-text).
3. **Resolve the brand.** Call `getBrands` (or `getBrandsByName`) to get the `brandId`.
4. **Create the product.** Call `createProducts` with up to **1000 items** per request. `listPrice` must be ≥ `salePrice`. This is **asynchronous** and returns a `batchRequestId`.
5. **Confirm.** Poll `getBatchRequestResult` with the `batchRequestId`; check each item `status`/`failureReasons`. Results are viewable for ~4 hours.
6. **Set price & stock.** Once approved, call `updatePriceAndInventory` (max 1000 items). Do not resubmit an identical request within 15 minutes.

## Rules
- Batch limits: max 1000 items/request; max 20,000 stock per product.
- Errors come back as `{ "errors": [ { "key", "message" } ] }` — see errors/trendyol-problem-types.yml.
- Rate limit: 50 requests / 10s per endpoint → 429 `too.many.requests`.
