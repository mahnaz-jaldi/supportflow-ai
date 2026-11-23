# Data Tables

SupportFlow AI uses four n8n Data Tables. `orders.csv` and `knowledge_base.csv` in this folder are the fictional seed data used in all test runs.

## tickets

| Column | Type | Notes |
|---|---|---|
| ticket_id | string | Business ticket ID, e.g. `TKT-DEMO-001` |
| customer_email | string | |
| order_id | string | |
| subject | string | Part of the duplicate key |
| description | string | Customer message (treated as untrusted input) |
| category | string | `delivery` / `returns` / `cancellation` / `other` (`unclassified` before AI) |
| priority | string | `low` / `medium` / `high` |
| risk_level | string | `low` / `medium` / `high` (`unassessed` before AI) |
| ai_confidence | number | 0..1 |
| status | string | See the status lifecycle in the README |
| assigned_to | string | `automation`, `human_support` or the reviewer's name |
| draft_response | string | AI draft, never edited by the review step |
| final_response | string | Text approved by a human (empty when rejected) |
| created_at | date | |
| updated_at | date | |

## orders

`order_id`, `customer_email`, `product_name`, `order_date`, `order_status`, `delivery_status`, `total_amount`, `refund_eligible`

An order is verified only when **both** `order_id` and `customer_email` match.

## knowledge_base

`policy_id`, `category`, `policy_title`, `policy_content`, `requires_approval`, `is_active`, `last_updated`

One active policy per category. There is deliberately no policy for `other`.

## audit_logs

| Column | Type | Notes |
|---|---|---|
| ticket_id | string | `unknown` for workflow-level errors |
| event_type | string | `ticket_created`, `classified`, `retrieval_failed`, `draft_created`, `error`, `workflow_error`, `review_approve`, `review_approve_with_edits`, `review_reject` |
| actor | string | `automation`, `ai_classifier`, `ai_responder`, `system`, `human:<name>` |
| previous_status | string | |
| new_status | string | |
| details | string | Classification JSON, policy used, error message or reviewer comment |
| timestamp | date | |
| log_id | string | Unused (the table's built-in `id` is used instead) |
