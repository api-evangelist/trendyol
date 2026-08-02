---
name: Fulfill an order package
description: Retrieve new order packages, mark them picking/invoiced, upload the invoice, and print the shipping label.
api: https://developers.trendyol.com/reference/getshipmentpackages
operations: [getShipmentPackages, updatePackage, uploadInvoiceFile, createCommonLabel, getCommonLabel]
---

# Fulfill an order package

## Auth
- HTTP Basic + mandatory `User-Agent` header; base URL `https://apigw.trendyol.com/integration`.

## Steps
1. **Pull orders.** Call `getShipmentPackages` for the seller. For high volume (>10,000) use `getShipmentPackagesStream` (cursor pagination, `nextPageToken`, ordered by `lastModifiedDate` DESC) — `getShipmentPackages` is capped at 10,000 records.
2. **Start fulfillment.** Call `updatePackage` (updatePackageStatus) to move the package to `Picking`.
3. **Invoice.** After preparing the shipment, either `uploadInvoiceFile` (multipart PDF/JPEG/PNG) or `sendInvoiceLink`, then set the package to `Invoiced` via `updatePackage`.
4. **Label (optional common label).** For TEX/Aras "Trendyol pays" shipments, call `createCommonLabel` (after PICKING/INVOICED), then `getCommonLabel` to retrieve the ZPL barcode.

## Rules
- Status transitions are order-package scoped; webhooks (asyncapi/trendyol-webhooks.yml) can push these status changes instead of polling.
- Errors: `{ "errors": [ { "key", "message" } ] }`. Rate limit 50 req/10s.
