---
name: formulate_explore_lookml
description: Use this skill when you need to create or modify a LookML Explore. This includes defining the Explore, joins, access grants, and basic configuration.
---

# Instructions

## 1. Core Standards

1.  **Naming Convention**: `snake_case` for the Explore name.
2.  **Required Parameters**:
    -   `description`: **100% Coverage**. Every Explore MUST have a description.
    -   `label`: A user-friendly name for the Explore in the UI.
    -   `view_name`: Defaults to explore name, but explicit definition is safer.
3.  **Joins**:
    -   `relationship`: **Required** (one_to_one, many_to_one).
    -   `sql_on`: **Required**. Use `${left.id} = ${right.id}` syntax.
    -   `type`: defaults to `left_outer`. Use `inner` or `full_outer` explicitly if needed.
4.  **Formatting**:
    -   Do NOT use `from` to rename views just for aesthetics. Use `view_label` instead.
    -   Exception: Polymorphic joins, Self-joins, Rescoping extensions.

## 2. Advanced Configuration

-   `always_filter`: specific filters that users can change but cannot remove.
-   `sql_always_where`: specific restrictions that users _cannot_ change.
-   `persist_with`: Link explore cache to datagroups (e.g., `default_datagroup`).
-   `fields`: Use inclusive lists to strictly control content when necessary (`ALL_FIELDS*`, `-view.field`).

# Examples

## Basic Explore

```lookml
explore: orders {
  label: "Orders"
  description: "Analyze order data, including user and product details."
  view_name: orders
  
  join: users {
    relationship: many_to_one
    sql_on: ${orders.user_id} = ${users.id} ;;
  }
}
```

## Explore with Filters & Caching

```lookml
explore: events {
  label: "Web Events"
  description: "User interaction events."
  persist_with: default_datagroup

  # Users can change this filter, but it defaults to '7 days'
  always_filter: {
    filters: [events.created_date: "7 days"]
  }

  # Users CANNOT change this filter.
  sql_always_where: ${events.is_test_data} = false ;;
  
  join: sessions {
    relationship: many_to_one
    sql_on: ${events.session_id} = ${sessions.session_id} ;;
  }
}
```
