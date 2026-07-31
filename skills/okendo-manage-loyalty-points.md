---
name: Manage Okendo loyalty points
description: Look up a loyalty customer, inspect earning/redemption rules, and adjust, earn or redeem points.
api: Okendo Merchant REST API
base_url: https://api.okendo.io/enterprise
operations: [lookupCustomer, getLoyaltyCustomer, listEarningRules, listRedemptionRules, createEarningTransaction, createRedemptionTransaction, createAdjustmentTransaction, setMinimumVipTier]
---

# Manage Okendo loyalty points

Adjust a shopper's Okendo Loyalty balance and tier via the Merchant REST API.

## Authentication
- HTTP Basic `Authorization: Basic base64("<okendo_user_id>:<api_key>")` plus `okendo-api-version: 2025-02-01`.

## Steps
1. **Resolve the customer** — `POST /customer_lookup` (`lookupCustomer`) with the shopper email to get the `customerId`.
2. **Read loyalty state** — `GET /loyalty/customers` (`getLoyaltyCustomer`) for the current points balance and VIP tier.
3. **Discover rules** — `GET /loyalty/earning_rules` (`listEarningRules`) and `GET /loyalty/redemption_rules` (`listRedemptionRules`) to find valid rule ids.
4. **Grant or redeem points**:
   - Trigger a custom earning rule — `POST /loyalty/earning_transactions` (`createEarningTransaction`).
   - Redeem a reward — `POST /loyalty/redemption_transactions` (`createRedemptionTransaction`).
   - Manual balance correction — `POST /loyalty/adjustment_transactions` (`createAdjustmentTransaction`).
5. **Promote a VIP** — `POST /loyalty/customer_minimum_vip_tiers` (`setMinimumVipTier`) to floor a customer's tier.

## Conventions
- No documented idempotency key — do not retry point-mutating POSTs without first re-reading the balance with `getLoyaltyCustomer`.
- Subscribe to `loyalty_*` webhook topics to observe the resulting transaction events (see asyncapi/okendo-webhooks.yml).
