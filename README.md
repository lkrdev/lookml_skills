# Looker Developer Skills Repository

This repository contains a collection of "skills" designed to assist AI agents and developers in writing high-quality, standardized LookML code. Each skill encapsulates specific instructions, best practices, and examples for different aspects of Looker development.

## Installation & Usage

To use these skills in your own projects—especially to enhance AI assistants like **Gemini**, **Cursor**, **Antigravity**, or **Claude Code**—we recommend adding this repository to your project.

### Installation via npx (Recommended)

The easiest way to install and update the skills in your repository is by using `npx`. This will copy the latest `.agents/skills` directory directly into your current working directory.

```bash
npx @lkrdev/lookml-skills
```

By default, the installer will skip any files that haven't changed, and will prompt you to confirm if there is a conflict. 

**Options:**
- `--override`: Automatically overwrite any existing files that have conflicts without prompting.

```bash
npx @lkrdev/lookml-skills --override
```

**For AI Tools:**
*   **Cursor**: Ensure the `.agents` folder is indexed. You may need to expressly verify `.cursorignore` does not exclude it.
*   **Claude Code**: Claude can inherently read files locally.
*   **Gemini / Antigravity**: Point your context to the `.agents/skills` directory when prompting.

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
*   **[formulate_liquid_lookml](.agents/skills/liquid/SKILL.md)**: Using Liquid variables for dynamic SQL, HTML, and links.
*   **[formulate_access_grants](.agents/skills/access_grants/SKILL.md)**: Implementing `access_grant` and `required_access_grants` for row-level security.
*   **[formulate_lookml_tests](.agents/skills/tests/SKILL.md)**: Writing LookML tests for Views and Explores.
