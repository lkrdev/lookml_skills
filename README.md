# Looker Developer Skills Repository

This repository contains a collection of "skills" designed to assist AI agents and developers in writing high-quality, standardized LookML code. Each skill encapsulates specific instructions, best practices, and examples for different aspects of Looker development.

## Installation & Usage

To use these skills in your own projects—especially to enhance AI assistants like **Gemini**, **Cursor**, **Antigravity**, or **Claude Code**—we recommend adding this repository to your project.

### Installation via npx (Recommended)

The easiest way to install and update the skills in your repository is by using `npx`. This will copy the latest skills directory (default: `.agents/skills/lookml-:skill-name`) directly into your current working directory.

```bash
npx @lkr-dev/lookml-skills
```

By default, the installer will skip any files that haven't changed, and will prompt you to confirm if there is a conflict. 

**Options:**
- `--override`: Automatically overwrite any existing files that have conflicts WITHOUT prompting.

```bash
npx @lkr-dev/lookml-skills --override
```

**For AI Tools:**
*   **Cursor**: Ensure the `.agents` folder is indexed. You may need to expressly verify `.cursorignore` does not exclude it.
*   **Claude Code**: Claude can inherently read files locally.
*   **Gemini / Antigravity**: Point your context to the `.agents/skills` directory when prompting.

## Core LookML Skills

These skills provide specific instructions for creating and modifying LookML objects.

### Models
*   **[lookml-model](.agents/skills/lookml-model/SKILL.md)**: Instructions for creating and configuring Model files, including connections and includes.

### Explores
*   **[lookml-explore](.agents/skills/lookml-explore/SKILL.md)**: Basic Explore definition, including descriptions, labels, and basic joins.
*   **[lookml-explore-joins](.agents/skills/lookml-explore/references/joins.md)**: Detailed guidance on defining joins, relationships, and SQL conditions.
*   **[lookml-explore-advanced](.agents/skills/lookml-explore/references/advanced.md)**: Advanced configurations like `access_filter`, `sql_always_where`, and UNNESTing arrays.

### Views
*   **[lookml-view](.agents/skills/lookml-view/SKILL.md)**: Standard View definitions, `sql_table_name`, and file organization (Standard, Extended, Refined).
*   **[lookml-view-derived-table](.agents/skills/lookml-view/references/derived_table.md)**: Creating Native Derived Tables (NDT) and SQL Derived Tables (SDT).
*   **[lookml-refinements](.agents/skills/lookml-refinements/SKILL.md)**: Deep dive into LookML includes, refinements (layering), and project structure best practices.
*   **[lookml-sets](.agents/skills/lookml-sets/SKILL.md)**: Guide to using LookML sets for grouping fields, controlling visibility, and managing drill paths.

### Fields
*   **[lookml-fields](.agents/skills/lookml-fields/SKILL.md)**: Overview of LookML field types (Dimension, Measure, Filter, Parameter) and the role of the `sql` parameter in each.
*   **[lookml-fields-dimension](.agents/skills/lookml-fields/references/dimension.md)**: Creating Dimensions, including HTML, links, and tier types.
*   **[lookml-fields-measure](.agents/skills/lookml-fields/references/measure.md)**: Creating Measures, including aggregation types and drill fields.
*   **[lookml-fields-dimension-group](.agents/skills/lookml-fields/references/dimension_group.md)**: Defining Dimension Groups for time and duration, including timeframe best practices.
*   **[lookml-fields-filter-parameter](.agents/skills/lookml-fields/references/filter_parameter.md)**: Creating Filter-only fields and Parameters for dynamic interactivity.
*   **[lookml-fields-value-format](.agents/skills/lookml-fields/references/value_format.md)**: Standard and custom formatting for numeric and currency fields.

## Advanced Functionality

### Logic & Security
*   **[lookml-liquid](.agents/skills/lookml-liquid/SKILL.md)**: Using Liquid variables for dynamic SQL, HTML, and links.
*   **[lookml-access-grants](.agents/skills/lookml-access-grants/SKILL.md)**: Implementing `access_grant` and `required_access_grants` for row-level security.
*   **[lookml-tests](.agents/skills/lookml-tests/SKILL.md)**: Writing LookML tests for Views and Explores.
