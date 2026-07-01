# ZRA MCP Output Shapes

Sanitized output shapes observed from the live production streamable endpoint on 2026-07-01. These are field/type guides only; do not treat them as exhaustive schemas.

## Conversation Tools

### `search_conversations`

Top-level: `conversations[]`, `from`, `to`, `next_page_token`, `page_size`.

Conversation item fields observed:

- `conversation_id`
- `conversation_start_time`
- `conversation_title`
- `conversation_type`
- `duration_sec`
- `host_email`
- `host_id`
- `participants[]`
- `team_id`
- `team_name`
- optionally `deal_id`, `deal_name`

### `get_conversation_analysis`

Top-level: `results[]`.

Result item fields observed:

- `conversation_id`
- `analysis`

Analysis fields observed:

- `deal_memo`
- `good_questions[]`
- `indicators[]`
- `metrics[]`
- `sentiment`
- `topics`

Targeted analysis may return only the requested analysis fields.

### `get_conversation_transcript`

Top-level: `speaker_defs[]`, `transcript`.

Speaker fields observed:

- `speaker_id`
- `display_name`
- `role`

### `get_conversation_comments`

Top-level: `data_items[]`, `total`.

Empty `data_items` is normal.

### `get_scorecard_sessions`

Top-level: `sessions[]`.

Session fields observed:

- `session_id`
- `conversation_id`
- `conversation_title`
- `scorecard_id`
- `scorecard_name`
- `score`
- `score_percent`
- `total_score`
- `score_items[]`
- `display_name`
- `user_id`
- `create_date`
- `is_auto_scored`

## Deal Tools

### `search_deals`

Top-level: `data_items[]`, `has_activities`, `last_activity_date_range`, `next_page_token`, `page_size`, `sort_by`.

Deal item fields observed:

- `deal_id`
- `deal_name`
- `deal_stage`
- `amount`
- `currency`
- `probability`
- `deal_close_date`
- `created_date`
- `last_activity_date`
- `last_stage_change_date`
- `customer_account_id`
- `customer_company_display_name`
- `deal_owner_id`
- `deal_owner_display_name`

### `get_deal_detail_v2`

Top-level: `data_items[]`, `total`.

Detail item fields observed:

- `deal_id`
- `deal_name`
- `deal_stage`
- `deal_amount`
- `deal_currency`
- `probability`
- `deal_close_date`
- `created_date`
- `last_activity_date`
- `last_stage_change_date`
- `activity_count`
- `contact_count`
- `customer_account_id`
- `customer_company_name`
- `deal_owner_id`
- `deal_owner_display_name`
- `deal_owner_email`
- `key_contacts[]`

### `get_deal_activities_v2`

Top-level: `data_items[]`, `next_page_token`, `page_size`.

Activity item fields observed:

- `activity_type`
- `subject`
- `start_time`
- `stage_during`
- `participants[]`
- optionally `content` when `include_content:true`

### `get_deal_analysis`

May return an empty object for some sampled deals. Treat empty output as "no deal analysis returned," not as a tool failure.

### `get_deal_stages`

Top-level: `stages[]`, `total`.

Stage item fields observed:

- `name`
- `sort_order`
- `closed`
- `won`

## Customer and Scope Tools

### `search_indicators`

Top-level: `data_items[]`, `total`.

Indicator item fields observed:

- `indicator_id`
- `indicator_name`
- `indicator_description`
- `detection_type`
- `support_language`
- `trigger_keywords[]`

### `get_customer_accounts`

Top-level: `accounts[]`, `next_page_token`, `page_size`.

Account item fields observed:

- `account_id`
- `name`
- `website`
- `domain`
- `industry`
- `annual_revenue`
- `currency`
- `employees`
- `owner_id`
- `owner_name`
- `deal_count`
- `contact_count`
- `crm_type`

### `get_customer_contacts`

Top-level: `contacts[]`, `next_page_token`, `page_size`.

Contact item fields observed:

- `contact_id`
- `name`
- `email`
- `job_title`
- `deal_count`
- `last_engagement_date`
- optionally `recent_deal`
- optionally `recent_meetings[]`
- optionally `custom_data`

### `search_internal_users`

May return an empty object or result set for broad keywords. Use more specific name/email fragments when resolving users.

### `get_manager_team_and_member`

Top-level: `data_items[]`, `total`.

Overview item fields observed:

- `team_id`
- `team_name`
- `managers[]`

Detail item fields observed:

- `team_id`
- `team_name`
- `members[]`
