# Advanced HTML & Markdown for Looker Dashboards

High-quality dashboards (like Looker's SaaS Pulse) use HTML within `type: text` tiles to achieve custom branding and improved navigation.

## 1. High-Impact Headers
Use these for section breaks to create a professional, "app-like" feel.

### Multi-Color Title Template
```yaml
title_text: "<font color='#0A0909' size='4.5' weight='bold'>Core Topic :</font> <font color='#CA3313' size='4.5'>Sub-Topic</font>"
subtitle_text: "<font color='#0A0909' size='4.5' weight='bold'>Contextual</font> <font color='#CA3313' size='4.5'>Description</font>"
```

### Centered Icon with Image
```yaml
body_text: "<p style='text-align:center'><img src='https://cdn-icons-png.flaticon.com/512/4862/4862975.png' height='60'></p>"
```

## 2. Custom Navigation Menus
Create a horizontal navigation bar at the top or bottom of a tile to link related dashboards.

### Template with Sticky SVG Icons
Use `position: absolute; bottom: 0;` to pin a navigation menu to the bottom of a text tile, creating a consistent "app bar" feel.

```html
<div style="height: 100%; position: relative;">
  <!-- Your main tile content here -->
  <p style="text-align: center;"><img src="..." height="60"></p>

  <nav style="font-size: 18px; position: absolute; bottom: 0; width: 100%; text-align: center; background: rgba(255,255,255,0.9); padding: 5px 0;">
    <a style="padding: 10px; font-weight: bold; color: #0A0909;" href="/dashboards/1">
      <svg style="height: 16px; fill: #0A0909;" viewBox="0 0 20 20">...</svg> Dashboard A
    </a>
  </nav>
</div>
```

## 3. Recommended Styling Rules
- **Color Contrast**: Ensure text colors used in `<font>` tags have high contrast against the dashboard background (typically `#ffffff` or `#f7f7f7`).
- **Responsive Images**: Use `height='60'` or `width='100%'` to ensure images scale appropriately within tiles.
- **Link Decoration**: Use `text-decoration: none` in `<a>` tags to maintain a clean UI, but keep the color distinct.
- **Icon Alignment**: Use `vertical-align: middle` on SVG icons to align them perfectly with the adjacent text.
