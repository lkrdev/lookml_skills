# Boilerplate High-Quality Dashboard

Use this template as a starting point for any new LookML dashboard. it incorporates the visual hierarchy, technical logic, and navigation patterns identified in the Looker demo environment.

```yaml
- dashboard: high_quality_template
  title: "Dashboard Title"
  description: "A concise description of the dashboard's purpose and target persona."
  preferred_viewer: dashboards-next
  layout: newspaper
  query_timezone: user_timezone
  crossfilter_enabled: true
  
  # 1. Organization: Tabs
  tabs:
    - name: overview
      label: "Executive Summary"
    - name: details
      label: "Operational Details"

  # 2. Global Filters
  filters:
    - name: date_filter
      title: "Date Range"
      type: date_filter
      default_value: "last 90 days"
      ui_config:
        type: relative_timeframes
        display: inline

  elements:
    # 3. Branding & Navigation Header
    - name: header_tile
      type: text
      tab_name: overview
      title_text: "<font color='#0A0909' size='4.5' weight='bold'>Business Pulse :</font> <font color='#CA3313' size='4.5'>Overview</font>"
      body_text: |
        <div style="height: 100%; position: relative;">
          <p>Contextual summary of high-level performance.</p>
          <nav style="position: absolute; bottom: 0; width: 100%; text-align: center;">
            <a style="padding: 10px; font-weight: bold; color: #0A0909;" href="#">Overview</a>
            | 
            <a style="padding: 10px; color: #0A0909;" href="#">Deep Dive</a>
          </nav>
        </div>
      row: 0
      col: 0
      width: 24
      height: 3

    # 4. KPI Row (with Comparisons)
    - title: "Total Revenue"
      name: revenue_kpi
      model: your_model
      explore: your_explore
      type: single_value
      fields: [revenue_measure, date_dimension]
      filters:
        date_dimension: "2 periods ago for 2 periods"
      dynamic_fields:
        - table_calculation: pop_change
          label: "vs Prior Period"
          expression: "${revenue_measure} / offset(${revenue_measure}, 1) - 1"
          value_format_name: percent_1
      show_comparison: true
      comparison_type: change
      tab_name: overview
      row: 3
      col: 0
      width: 6
      height: 4

    # 5. Trend Tile (with Monotone Interpolation & Dual Axis)
    - title: "Revenue vs Conversion Rate"
      name: revenue_trend
      model: your_model
      explore: your_explore
      type: looker_line
      fields: [date_month, revenue_measure, conversion_rate_measure]
      interpolation: monotone
      point_style: circle_outline
      y_axes:
        - orientation: left
          label: "Revenue"
          series: [{id: revenue_measure}]
        - orientation: right
          label: "Conversion Rate"
          series: [{id: conversion_rate_measure}]
      tab_name: overview
      row: 7
      col: 0
      width: 18
      height: 8

    # 6. Detailed Data Grid (with Cell Viz)
    - title: "Top Performing Segments"
      name: segment_table
      model: your_model
      explore: your_explore
      type: looker_grid
      fields: [segment_dimension, revenue_measure, orders_count]
      series_cell_visualizations:
        revenue_measure:
          is_active: true
          palette:
            palette_id: google-sequential-0
            collection_id: google
      show_totals: true
      tab_name: details
      row: 0
      col: 0
      width: 24
      height: 10
```
