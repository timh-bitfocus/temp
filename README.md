# 20170626154838_add_oidc.rb

```
DROP TABLE `credentials_oidc`;

DROP TABLE `oidc_config`;

DROP TABLE `oidc_nonce`;

DROP TABLE `oidc_config_default_new_user_role`;

DROP TABLE `oidc_group`;

DROP TABLE `oidc_group_role`;

DROP TABLE `oidc_config_default_new_user_group`;

DROP TABLE `oidc_user_attribute`;

DROP TABLE `oidc_user_attribute_attribute`;
```

20170705113149_content_type_specific_id_columns_metadata.rb

```
UPDATE `content_metadata`
    SET `content_metadata`.`content_id` = `content_metadata`.`look_id`
    WHERE `content_metadata`.`content_type` = 'look';

UPDATE `content_metadata`
    SET `content_metadata`.`content_id` = `content_metadata`.`dashboard_id`
    WHERE `content_metadata`.`content_type` = 'dashboard';

UPDATE `content_metadata`
    SET `content_metadata`.`content_id` = `content_metadata`.`space_id`
    WHERE `content_metadata`.`content_type` = 'space';

ALTER TABLE `content_metadata`
    DROP COLUMN `content_metadata`.`look_id`,
    DROP COLUMN `content_metadata`.`dashboard_id`,
    DROP COLUMN `content_metadata`.`space_id`;
```

20170712130513_move_content_favorite_to_content_metadata.rb

```
ALTER TABLE `content_metadata`
    DROP COLUMN `content_metadata`.`content_metadata_id`;
```

20170721105949_db_connection_sql_runner_precache_tables.rb

```
ALTER TABLE `db_connection`
    DROP COLUMN `db_connection`.`sql_runner_precache_tables`;
```

20170724093715_dashboard_element_title_text_to_text.rb

```
ALTER TABLE `dashboard_element`
    DROP COLUMN `dashboard_element`.`title`;
```

20170724140728_scheduler_include_links.rb

```
ALTER TABLE `scheduled_plan`
    DROP COLUMN `scheduled_plan`.`include_links`;

ALTER TABLE `scheduled_job`
    DROP COLUMN `scheduled_job`.`include_links`;
```

20170726132341_schedule_with_datagroup.rb

```
ALTER TABLE `scheduled_plan`
    DROP COLUMN `scheduled_plan`.`datagroup`;

ALTER TABLE `scheduled_job`
    DROP COLUMN `scheduled_job`.`datagroup`;
```

20170726143744_add_whitelabel_looker_mentions_and_links.rb

```
ALTER TABLE `whitelabel_configuration`
    DROP COLUMN `whitelabel_configuration`.`allow_looker_mentions`,
    DROP COLUMN `whitelabel_configuration`.`allow_looker_links`;
```

20170728132600_create_annotations.rb

```
DROP TABLE `annotation`;

DROP TABLE `annotation_category`;
```

20170802133401_dashboard_filter_required.rb

```
ALTER TABLE `dashboard_filter`
    DROP COLUMN `dashboard_filter`.`required`;
```

20170809144552_set_whitelabel_links_to_default_true.rb

```
no changes needed
```

20170810133625_create_merge_query_result_maker_tables.rb

```
DROP TABLE `merge_query`;

DROP TABLE `merge_query_source_query`;

DROP TABLE `result_maker`;

ALTER TABLE `slug`
    DROP COLUMN `slug`.`result_maker_id`;
```

20170816142232_scheduler_add_query_id.rb

```
ALTER TABLE `scheduled_plan`
    DROP COLUMN `scheduled_plan`.`query_id`;

ALTER TABLE `scheduled_job`
    DROP COLUMN `scheduled_job`.`query_id`;
```

20170823134644_add_smtp_settings.rb

```
DROP TABLE `smtp_settings`;
```

20170829113238_content_titles_descriptions_move_to_content_metadata.rb

Sort of complex anti-migration, I don't 100% trust myself to do this right without getting eyes on what the log throws back at us when this migration fails. This is how I am broadly interpreting this:

For each row in `content_metadata` where `look_id` is not null, look up the row in `look` where ``id` = `content_metadata`.`look_id`` and update `look`.`title` and `look`.`description` with the values in `content_metadata`.`title` and `content_metadata`.`description`

Do the same above but replace `look` and `look_id` with `dashboard` and `dashboard_id`.

Do the same but replace `dashboard` and `dashboard_id` with

then:

```
ALTER TABLE `content_metadata`
    DROP COLUMN `content_metadata`.`title`,
    DROP COLUNN `content_metadata`.`description`;
```

20170907151633_dashboard_identifiers.rb

```
ALTER TABLE `dashboard`
    DROP COLUMN `dashboard`.`lookml_link_id`;

ALTER TABLE `dashboard_element`
    DROP COLUMN `dashboard_element`.`lookml_link_id`;

ALTER TABLE `dashboard_filter`
    DROP COLUMN `dashboard_filter`.`lookml_link_id`;
```

20170911115734_ensure_expanded_content_metadata_description_column.rb

```
no changes needed
```

20170913110908_add_oidc_user_id_attribute.rb

```
DELETE FROM `user_attribute_v2`
    WHERE `user_attribute_v2`.`name` = 'oidc_user_id';
```

20170920142237_add_result_maker_to_history.rb

```
ALTER TABLE `history`
    DROP COLUMN `result_maker_id`;
```

20170921162511_add_after_connect_statements_to_dbconnection.rb

```
ALTER TABLE `db_connection`
    DROP COLUMN `db_connection`.`after_connect_statements`;
```
