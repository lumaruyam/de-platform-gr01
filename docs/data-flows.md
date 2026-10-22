# Data flows

One row per flow: where the data comes from, what it is, who uses it, how often
it moves, and in which format. As-is situation of Albert's Marketplace (L01),
plus the one flow the ML team asks for.

| # | Source | Data | Consumer | Frequency | Format | Notes |
|---|---|---|---|---|---|---|
| 1 | Storefront checkout | `orders`, `order_items`, `payment`, `shipment`, stock | PostgreSQL `marketplace` | Continuous | Database writes | Every order is written twice, a few days apart |
| 2 | Storefront reviews | `review`, `product_reviews`, `review_images` | PostgreSQL `marketplace` | Continuous | Database writes | Free text and photo links |
| 3 | Storefront accounts | `customer` (incl. `pwd`), `shipping_details`, `payment_details` (incl. `card_no`, `cvv`) | PostgreSQL `marketplace` | Continuous | Database writes | Plain-text passwords and full card data |
| 4 | Storefront (load generator) | Clickstream events: `product_viewed`, `added_to_cart`, `order_placed` | Kafka topic `clickstream` | Continuous | JSON events | Carries `buyer_id`, `session_id` |
| 5 | PostgreSQL `marketplace` | Orders, customers, products | Analysts | On demand | SQL on the live tables | Same tables checkout writes to |
| 6 | PostgreSQL `marketplace` | Selected tables (25 legacy CSVs) | Analysts' spreadsheets | Nightly | CSV export | Opened and reworked by hand |
| 7 | *(wanted)* | Units sold per product × country × day | ML team | Daily | *Does not exist yet* | Target: gold `daily_demand` table |

## Under the matrix, answer

- **Which flows carry personal data?**
  Flow 3 (names, phones, e-mails, addresses, plain-text passwords, card numbers
  with CVV); flow 1 (`buyer_id` and `payment_id` link a person to purchases);
  flow 4 (`buyer_id`, `session_id`); flows 5 and 6 (whatever the analyst
  selects: the CSV export includes `customer.csv` with `pwd` and
  `payment_details.csv` with `card_no` / `cvv`).
- **Which consumers read directly from a system that also serves customers?**
  The analysts (flow 5): their queries hit the same PostgreSQL database the
  storefront writes to, so reporting competes with checkout. The nightly CSV
  export (flow 6) reads it too.
- **Where could the same figure be computed twice, in two different ways?**
  Revenue. It can come from `payment` (one transaction per order), from
  `order_items` (`qty × price_at_purchase`), or from spreadsheets built on the
  CSV export. Every order appears twice, a few days apart, so each method that
  deduplicates differently gives a different number.
