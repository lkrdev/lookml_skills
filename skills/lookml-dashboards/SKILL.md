---
name: lookml-dashboards
description: Design and implementation of high-quality Looker dashboards using LookML. Covers layout optimization, KPI comparisons, mixed series charts, advanced markdown styling, and performance best practices.
---

# LookML Dashboard Best Practices

This skill distills the key characteristics of high-quality dashboards. For a deep dive into technical details, see the [Dashboard Parameter Reference](references/dashboard_parameters.md), [Table Calculation Reference](references/table_calculations.md), and the [Boilerplate Template](references/boilerplate_dashboard.md).

## 1. Dashboard Layout and Structure

- **Layout Method**: Standardize on `layout: newspaper`. It provides a 24-column grid for flexible element positioning.
- **Sectioning**: Use `type: text` elements as section headers to create a visual narrative.
  - **Width**: Use `width: 24` to span the full dashboard.
  - **Visual Hierarchy**: Place high-level KPIs at the top row, followed by trend visualizations, and detailed breakdowns/tables at the bottom.
- **Element Sizing**:
  - **KPIs**: Typically `width: 4` to `6` and `height: 3` to `4`.
  - **Trends (Area/Line)**: Typically `width: 12` to `24` and `height: 6` to `8`.

## 2. Tabs Implementation

Tabs are critical for organizing complex dashboards into logical sections (e.g., "Overview", "Deep Dive").

- **Dashboard Level**:
  ```yaml
  tabs:
    - name: overview
      label: Overview
    - name: details
      label: Details
  ```
- **Element Level**: Assign elements to tabs using the `tab_name` parameter.
- **Constraint**: Tabs are only supported with `layout: newspaper`.

## 3. KPI Comparisons (Single Value)

Effective KPIs show context using comparisons.

- **Comparison Types**:
  - `comparison_type: change`: Best for Period-over-Period (YoY, WoW) analysis.
  - `comparison_type: progress_percentage`: Best for tracking against goals.
- **Logic**: Use `dynamic_fields` with table calculations (`offset`, `index`) to calculate the comparison value.
- **Static Goals**: For hardcoded targets, use a table calculation with a constant value (e.g., `expression: '10000'`).
- **Reverse Colors**: Set `comparison_reverse_colors: true` for metrics where "down" is good (e.g., Wait Time).

## 4. Advanced Analytics Patterns (Dynamic Fields)

High-quality dashboards often use table calculations to provide deeper insights beyond raw metrics.

- **Period-over-Period (PoP)**: Use `offset(${measure}, 1)` to compare the current value to the previous row.
- **Cumulative Trends**: Use `running_total(${measure})` for growth charts (e.g., "Cumulative ARR this Quarter").
- **Return/Retention Rates**: Use `index(${measure}, 1)` as a denominator to calculate rates relative to a starting point.
- **Goal Tracking**: Combine `running_total` with a calculated `goal` line to show progress vs linear targets.

## 5. Mixed Series and Dual Axes

Correlate different metric types (e.g., Sessions and Conversion Rate) in one tile.

- **Series Types**: Override specific series using `series_types`.
  ```yaml
  series_types:
    sessions.overall_conversion: line
    events.sessions_count: column
  ```
- **Dual Axes**: Configure `y_axes` with left and right orientations (for vertical charts) or top and bottom (for horizontal bar charts) to handle different scales.
  > [!IMPORTANT]
  > When configuring `y_axes`, the nested `series` parameter **must** contain a list of objects specifying `id` (and optionally `name`), rather than a list of simple strings. Passing plain strings will result in a Looker frontend error: `"Cannot create property 'id' on string '...'"`:
  ```yaml
  y_axes:
    - label: "Left Axis"
      orientation: left
      series:
        - id: events.sessions_count
          name: "Sessions Count"
    - label: "Right Axis"
      orientation: right
      series:
        - id: sessions.overall_conversion
          name: "Conversion Rate"
  ```
- **Data Totals**: For `looker_grid` and `looker_column`, always enable `show_totals: true` and `row_total: right` where appropriate to provide cumulative context.
- **Merged Queries**: For insights that require data from multiple Explores (e.g., Sales from `order_items` vs Traffic from `events`), use `merged_queries`.
  - **Listening**: Each sub-query must have its own `listen` parameter to respond to dashboard filters correctly.
  - **Join Logic**: Define the `join_fields` carefully to ensure the data aligns across queries.

## 6. Styling and Aesthetics

- **Monotone Interpolation**: For line/area charts, use `interpolation: monotone` for a modern, smooth visual.
- **Consistent Coloring**: Define `series_colors` for entity dimensions (e.g., Brand names) so they remain consistent across all tiles.
- **Advanced Grid Styling**:
  - **Cell Visualizations**: Use `series_cell_visualizations` to embed bar charts in table cells for metrics like "Cost" or "CSAT".
  - **Conditional Formatting**: Use in grids and KPIs to highlight status (Red/Yellow/Green).

## 7. Specialized Visualizations and Customizations

Don't limit yourself to basic charts. Use specialized types for specific insights:

- **Scatter Plot**: Used for Quadrant or RFM analysis. Enable `quadrants_enabled: true`.
- **Donut Multiples (`looker_donut_multiples`)**: Excellent for comparing segments across a primary dimension. Use `inner_radius: 50` for a modern feel.
- **Reference Lines**: Use `reference_lines` on line/area charts to show targets or "danger zones" directly on the trend.
- **Google Maps**: Use for geographic density. Set `map_tile_provider: light` or `streets`.

## 8. Dashboard Filters

- **Filter Type**: Prefer `type: field_filter` for automatic suggestions.
- **UI Configuration (`ui_config`)**:
  Modern dashboards support customized filter controls using `ui_config` containing `type` and `display` parameters.

  #### A. Display Position (`display` options)
  - `inline`: Use for **primary, high-impact filters** that establish the baseline context of the dashboard (e.g., date ranges, primary focus entities like a "Select Station" lookup). These sit directly on the dashboard filter bar.
  - `popover`: Use for **secondary filters** that are useful for segmentation and drilling but shouldn't clutter the top-level interface (e.g., "User Type", "Gender", "Region ID"). Clicking the filter opens a popover dropdown.
  - `overflow`: Use for **infrequently used, tertiary filters** to keep the filter bar clean and uncluttered.

  #### B. Control Type Selection (`type` options)

  _Single Selection_:
  - `dropdown_menu`: Best for long single-select lists (e.g., Station Name, State) where option lists are dynamic or exceed 5-10 items.
  - `button_toggles`: Best for binary choices (e.g., Is Subscriber: Yes/No) or very short lists (2-3 items) for a clean toggle experience.
  - `radio_buttons`: Ideal for mutually exclusive choices of short lists (2-5 items).

  _Multiple Selection_:
  - `tag_list`: Best for dynamic multi-select of long lists (e.g., selecting multiple stations or cities) where selected items display as compact tags.
  - `button_group`: Best for multi-select options of very short lists (2-4 items max, e.g., seasons or user tiers) where they fit cleanly as inline buttons.
  - `checkboxes`: Best for multi-select of medium lists (3-8 options).

  _Other_:
  - `advanced`: Use for complex conditional inputs (e.g., text search patterns, relative or custom numeric/date criteria).

  #### C. LookML Boilerplate

  ```yaml
  filters:
    - name: select_station
      title: "Select Station"
      type: field_filter
      model: nyc_citibike_trips
      explore: trips
      field: start_station.name
      ui_config:
        type: dropdown_menu
        display: inline
  ```

- **Cascading Filters**: Use `listens_to_filters` to create dependencies (e.g., City filter listens to State).

## 9. Advanced Markdown (SaaS Pulse Style)

The `saas_pulse` dashboard demonstrates how to use HTML within text tiles for branding and navigation. For specific code snippets and templates, see the [Markdown & HTML Reference](references/markdown_html_templates.md).

### Visual Branding with `<font>`

Use the `<font>` tag for specific sizes and colors in titles and subtitles.

```yaml
title_text: "<font color='#0A0909' size='4.5' weight='bold'>Main Topic :</font> <font color='#CA3313' size='4.5'>Metrics</font>"
```

### Navigation Menus

Create custom navigation bars using `<nav>`, `<a>`, and `<svg>` tags.

```yaml
body_text: |
  <nav style="font-size: 18px; text-align: center;">
    <a style="padding: 10px; font-weight: bold; color: #0A0909;" href="/dashboards/1">Summary</a>
    <a style="padding: 10px; color: #0A0909;" href="/dashboards/2">Web Analytics</a>
  </nav>
```

## 10. Performance and Interactivity

- **Cross-Filtering**: Enable `crossfilter_enabled: true` for intuitive dashboard-wide filtering.
- **Tile Notes**: Provide on-dashboard documentation using the `note` parameter. High-quality tiles often use `note_state: collapsed` and `note_display: hover`.
- **Auto Run**: Set `auto_run: false` for dashboards with many heavy queries.
- **Query Timezone**: Always use `query_timezone: user_timezone`.

## 11. Embedding and White-Labeling

For dashboards intended to be embedded in external applications:

- **Embed Style**: Use the `embed_style` parameter to match the host application's aesthetic.
  ```yaml
  embed_style:
    background_color: "#f7f7f7"
    show_title: true
    title_color: "#3a4245"
    show_filters_bar: true
    tile_text_color: "#3a4245"
  ```

## 11. Dashboard Polish (Small Details)

The final 5% of effort often determines if a dashboard feels professional.

- **Clean Legends**: Always set `show_view_names: false` to prevent long, redundant field names (e.g., "Orders Count" instead of "Orders Orders Count").
- **Renaming Series**: Use `series_labels` within a tile to provide user-friendly names for specific measures without changing the underlying LookML.
- **Stacking Strategy**:
  - Use `stacking: normal` for volume-based area/column charts.
  - Use `stacking: percent` to show composition over time.
- **Top-N Analysis**: Use `limit_displayed_rows: true` with `limit_displayed_rows_values` to show "Top 10" or "Bottom 5" without over-complicating the query.
- **Dynamic Legends**: Set `legend_position: center` or `right` based on the tile's width to maximize data real estate.

## 12. Dashboard Validation, Outlier Handling, and Data Quality

A highly polished dashboard is only as good as the data it represents. Always perform investigative data validation using Looker MCP queries to identify anomalies, and apply proactive handling techniques either in the underlying lookML model or in the dashboard element query elements.

### A. Investigative Validation

Before finalizing a dashboard, execute representative queries of your elements using the `query` tool to verify the top results. Check for:

- **Null Groupings**: Verify if the top dimensions are `null` (e.g., `start_station.name: null` having massive volumes due to mismatched outer joins).
- **Outliers**: Identify if a single data point holds disproportionately large weight, skewing visual scales.

### B. Element-Level Null Exclusion

When joining dimension tables where matches might be incomplete (e.g., historical trips referencing retired stations), always add static element-level `filters` to exclude `null` values:

```yaml
- name: top_busy_stations
  title: "Top Busy Stations"
  model: nyc_citibike_trips
  explore: trips
  type: looker_grid
  fields: [start_station.name, trips.count]
  filters:
    start_station.name: "-NULL" # Excludes null groups from taking over top slots
```

### C. Map Scale and Clutter Control

Geographic maps (`looker_map`) can easily become unreadable due to overlapping data points or huge outlier regions. Apply these safeguards:

- **Filter Outliers**: Filter out `null` location/dimension attributes to prevent them from centering massive circles.
- **Marker Bounds**: Limit point sizes to a professional range to prevent overlapping "balloons" from covering other coordinates:
  ```yaml
  map_plot_mode: points
  map_marker_radius_max: 8
  map_marker_radius_min: 2
  ```

## 13. Dashboard Quality Checklist

- [ ] Uses `newspaper` layout on a 24-column grid.
- [ ] Visual hierarchy: KPIs -> Trends -> Details.
- [ ] Every KPI has a comparison context (PoP or Goal).
- [ ] Dual axes are used where metric scales differ significantly.
- [ ] `crossfilter_enabled: true` is set.
- [ ] Tiles include `notes` for metric definitions where appropriate.
- [ ] Large dashboards are organized into `tabs`.
- [ ] HTML section headers and navigation menus are implemented.
- [ ] Dimension colors are consistent across all tiles.
- [ ] `embed_style` is configured if the dashboard is for external use.
- [ ] Elements joining nullable dimensions have explicit `-NULL` filters to prevent unmapped groupings.
- [ ] Map tiles have configured marker radius bounds (`map_marker_radius_max`, `map_marker_radius_min`) to handle overlapping density nicely.
