---
name: formulate_liquid_lookml
description: Use this skill to use Liquid variables in LookML for dynamic SQL, HTML, and Links.
---

# Instructions

1.  **Syntax**:
    *   `{{ value }}`: Output syntax (inserts text).
    *   `{% if condition %}`: Tag syntax (logic).
2.  **Common Variables**:
    *   `value`: Raw value from DB.
    *   `rendered_value`: Formatted value (as seen in UI).
    *   `_filters['view.field']`: User-selected filter values.
    *   `parameter_name._parameter_value`: Selected parameter value.
3.  **Best Practices**:
    *   **SQL Injection**: Always use `| sql_quote` when inserting `_filters` into SQL (e.g., in `sql_always_where` or Derived Tables).
    *   **Performance**: Avoid `{{ field._value }}` in links if possible, as it adds the field to the GROUP BY (use `row['view.field']` for links if the field is already in the query).

# Liquid Variable Definitions

The following table describes the Liquid variables that you can use with LookML and where they can be used.

**Usage Key:**
*   **A**: Works with the `action` parameter.
*   **DV**: Works with the `default_value` (for dashboards) parameter.
*   **DE**: Works with the `description` parameter at the field level.
*   **F**: Works with the `filters` (for dashboard elements) parameter.
*   **H**: Works with the `html` parameter.
*   **LA**: Works with field-level label parameters (`label`, `view_label`, `group_label`, `group_item_label`).
*   **LI**: Works with the `link` parameter.
*   **S**: Works with all LookML parameters that begin with `sql` (e.g. `sql`, `sql_on`, `sql_table_name`).

| Variable | Definition | Usage |
| :--- | :--- | :--- |
| `value` | The raw value of the field returned by the database query. | A, H, LI |
| `rendered_value` | The value of the field with Looker's default formatting. | A, H, LI |
| `filterable_value` | The value of the field formatted for use as a filter in a Looker URL. | A, H, LI |
| `link` | The URL to Looker's default drill link. | A, H, LI, S |
| `linked_value` | The value of the field with Looker's default formatting and default linking. | A, H, LI |
| `_filters['view_name.field_name']` | The user filters applied to the specified field. | A, DE, H, LA, LI |
| `{% date_start date_filter_name %}` | The beginning date in a date filter. | S |
| `{% date_end date_filter_name %}` | The ending date in a date filter. | S |
| `{% condition filter_name %} sql_or_lookml_reference {% endcondition %}` | The value of the filter applied to the reference as SQL. | S |
| `{% parameter parameter_name %}` | The value of the parameter filter. | DE, LA, S |
| `parameter_name._parameter_value` | Injects the value of the parameter filter into a logical statement. | DE, H, LA, LI, S |
| `_user_attributes['name_of_attribute']` | The value of the user attribute. | A, DE, H, LA, LI, S, DV, F |
| `_localization['localization_key']` | Returns the value associated with a localization key. | DV, F |
| `_model._name` | The name of the model for this field. | A, DE, H, LA, LI, S |
| `_view._name` | The name of the view for this field. | A, DE, H, LA, LI, S |
| `_explore._name` | The name of the Explore for this field. | A, DE, H, LA, LI, S |
| `_explore._dashboard_url` | The relative URL for the current dashboard. | H, LI |
| `_field._name` | The name of the field itself. | A, DE, H, LA, LI, S |
| `_query._query_timezone` | The time zone in which the query was run. | A, DE, H, LA, LI, S |
| `view_name._in_query` | Returns `true` if any field from the view is included in the query. | DE, LA, LI, S |
| `view_name.field_name._in_query` | Returns `true` if the field appears in the query or filters. | DE, LA, LI, S |
| `view_name.field_name._is_selected` | Returns `true` if the field appears in the query data table. | DE, LA, LI, S |
| `view_name.field_name._is_filtered` | Returns `true` if the field is included in a filter. | DE, LA, LI, S |

# Examples

## Dynamic HTML (Conditional Formatting)

```lookml
dimension: status {
  html:
    {% if value == 'complete' %}
      <span style="color: green">{{ rendered_value }}</span>
    {% else %}
      <span style="color: red">{{ rendered_value }}</span>
    {% endif %} ;;
}
```

## Dynamic SQL (Table Selection)

```lookml
view: events {
  sql_table_name:
    {% if events.date._in_query %}
      events_daily
    {% else %}
      events_all
    {% endif %} ;;
}
```

## Templated Filters (Derived Table)

```lookml
view: customer_facts {
  derived_table: {
    sql:
      SELECT customer_id, SUM(amount)
      FROM orders
      WHERE {% condition order_date %} created_at {% endcondition %}
      GROUP BY 1 ;;
  }
  
  filter: order_date {
    type: date
  }
}
```
