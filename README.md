# Looker Developer Skills Repository

This repository contains a collection of "skills" designed to assist AI agents and developers in writing high-quality, standardized LookML code. Each skill encapsulates specific instructions, best practices, and examples for different aspects of Looker development.

## Installation & Usage

To use these skills in your own projects—especially to enhance AI assistants like **Gemini**, **Cursor**, **Antigravity**, or **Claude Code**—we recommend adding this repository to your project.

### Method 1: Git Submodule (Recommended)

This method keeps the skills repo as a distinct, updatable dependency. It is ideal for maintaining the latest best practices across multiple projects.

```bash
# Add the skills repo as a submodule
git submodule add git@github.com:lkrdev/lookml_skills.git skills

# Initialize and update
git submodule update --init --recursive
```

**For AI Tools:**
*   **Cursor**: Ensure the `skills` folder is indexed. You may need to expressly verify `.cursorignore` does not exclude it.
*   **Claude Code**: Claude can inherently read files in submodules.
*   **Gemini / Antigravity**: Point your context to the `.agents/skills` directory when prompting.

### Method 2: Sparse Checkout (Minimalist)

Use this if you **only** want the `.agents/skills` folder and nothing else from this repository, without the overhead of a full clone history for other files.

```bash
# 1. Create a directory for the skills and initialize a new git repo
mkdir looker-skills && cd looker-skills
git init
git remote add origin git@github.com:lkrdev/lookml_skills.git

# 2. Configure sparse checkout to only include the '.agents/skills' directory
git config core.sparseCheckout true
echo ".agents/skills/" >> .git/info/sparse-checkout

# 3. Pull the content
git pull origin main
```

**Note for AI Context:**
Once downloaded, you can symlink this folder or configure your AI tool to watch this specific directory for instructions.

### Method 3: Direct Download (Simplest)

If you prefer not to use Git, you can simply download the latest version of the skills.

1.  Download the **[ZIP file](https://github.com/lkrdev/lookml_skills/archive/refs/heads/main.zip)**.
2.  Unzip `lookml_skills-main.zip`.
3.  Rename the folder to `skills` and move it into your project.

**For AI Tools:**
Simply point your context to the `.agents/skills` directory inside the `skills` folder.


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
