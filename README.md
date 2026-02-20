# Looker Developer Skills Repository

This repository contains a collection of "skills" designed to assist AI agents and developers in writing high-quality, standardized LookML code. Each skill encapsulates specific instructions, best practices, and examples for different aspects of Looker development.

## Standards and Best Practices

These skills define the "North Star" for our development standards.

*   **[looker-lookml-best-practices](lookml-testing/.agents/skills/looker-lookml-best-practices/SKILL.md)**: The comprehensive guide to LookML coding standards, file organization, and architectural patterns. This is the primary reference for all development.
*   **[looker-lams-best-practices](lookml-testing/.agents/skills/looker-lams-best-practices/SKILL.md)**: Defines the automated linting rules (LAMS) used to enforce the best practices programmatically.

## Core LookML Skills

These skills provide specific instructions for creating and modifying LookML objects.

### Models
*   **[formulate_model_lookml](.agents/skills/model/SKILL.md)**: Instructions for creating and configuring Model files, including connections and includes.

### Explores
*   **[formulate_explore_lookml](.agents/skills/explore/SKILL.md)**: Basic Explore definition, including descriptions, labels, and basic joins.
*   **[formulate_explore_joins](.agents/skills/explore/joins/SKILL.md)**: Detailed guidance on defining joins, relationships, and SQL conditions.
*   **[formulate_explore_advanced](.agents/skills/explore/advanced/SKILL.md)**: Advanced configurations like `access_filter`, `sql_always_where`, and UNNESTing arrays.

### Views
*   **[formulate_view_lookml](.agents/skills/view/SKILL.md)**: Standard View definitions, `sql_table_name`, and file organization (Standard, Extended, Refined).
*   **[formulate_derived_table](.agents/skills/view/derived_table/SKILL.md)**: creating Native Derived Tables (NDT) and SQL Derived Tables (SDT).

### Fields
*   **[formulate_dimension_lookml](.agents/skills/fields/dimension/SKILL.md)**: Creating Dimensions, including HTML, links, and tier types.
*   **[formulate_measure_lookml](.agents/skills/fields/measure/SKILL.md)**: Creating Measures, including aggregation types and drill fields.
*   **[formulate_dimension_group_lookml](.agents/skills/fields/dimension_group/SKILL.md)**: Defining Dimension Groups for time and duration, including timeframe best practices.
*   **[formulate_filter_parameter_lookml](.agents/skills/fields/filter_parameter/SKILL.md)**: Creating Filter-only fields and Parameters for dynamic interactivity.

## Advanced Functionality

### Logic & Security
*   **[formulate_liquid_lookml](.agents/skills/other/liquid/SKILL.md)**: Using Liquid variables for dynamic SQL, HTML, and links.
*   **[formulate_access_grant_lookml](.agents/skills/other/access_grants/SKILL.md)**: Implementing Row-Level and Object-Level security using Access Grants.

### Testing
*   **[tests](.agents/skills/other/tests/SKILL.md)**: Standards for writing LookML tests to ensure data integrity and accuracy.
