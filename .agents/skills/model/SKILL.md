---
name: formulate_model_lookml
description: Use this skill when you need to create or modify a LookML Model file (.model.lkml). This includes defining connections, includes, and configuring model-level settings.
---

# Instructions

1.  **Define the Model File**: A model file generally corresponds to a single database connection and includes Explores.
2.  **Required Parameters**:
    *   `connection: "connection_name"`: Must match a connection defined in Looker Admin.
    *   `include: "pattern"`: Specifies which view and dashboard files are available to the model.
3.  **Best Practices**:
    *   **Includes**: Avoid `include: "*.view"` if possible to prevent performance issues and namespace clutter. Use specific paths or wildcards like `include: "/views/users.view"` or `include: "/views/marketing/*.view"`.
    *   **Label**: Use `label:` to provide a user-friendly name for the model in the UI.
    *   **Week Start Day**: Set `week_start_day:` if the business logic requires a specific start day (e.g., `monday`).

# Examples

## Basic Model

```lookml
connection: "thelook"

# Include all views in the views/ folder
include: "/views/*.view"

# Include all dashboards
include: "/*.dashboard"

# Define an Explore (usually better to define in separate files for large projects, but acceptable here for small ones)
explore: orders {
  join: users {
    type: left_outer
    sql_on: ${orders.user_id} = ${users.id} ;;
    relationship: many_to_one
  }
}
```

## Model with Specific Settings

```lookml
connection: "snowlooker"

label: "eCommerce Analytics"

# Set valid week start day
week_start_day: monday

# Include specific folders
include: "/views/finance/*.view"
include: "/views/marketing/*.view"
```
