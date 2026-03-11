# Navigation Menus

## Source of Truth

- If a WordPress menu exists with a matching slug to an Etch loop preset, the **WordPress menu is the source of truth**. The `menuwp-classic` plugin (or `menuwp-pro` / `menuwp`) syncs changes from WordPress menus to the Etch loop preset automatically.
- If no matching WordPress menu exists, the **Etch loop JSON is the source of truth** — edit it directly via the RestlessWP API.

**Do NOT edit the Etch loop preset directly if a matching WordPress menu exists** and one of these plugins is active: `menuwp-classic`, `menuwp-pro`, or `menuwp`. The plugin's conflict detection will prevent future syncs if it detects the Etch data was modified externally.

## Adding Menu Items via REST API

Use `POST /wp/v2/menu-items` with the correct `type`, `object`, and `object_id` fields based on what you're linking to:

| Linking to | `type` | `object` | `object_id` | `url` |
|---|---|---|---|---|
| Page | `post_type` | `page` | page ID | omit (auto-resolved) |
| Post | `post_type` | `post` | post ID | omit (auto-resolved) |
| Custom post type | `post_type` | `{cpt_slug}` | post ID | omit (auto-resolved) |
| Category | `taxonomy` | `category` | term ID | omit (auto-resolved) |
| Custom taxonomy term | `taxonomy` | `{taxonomy_slug}` | term ID | omit (auto-resolved) |
| Post type archive | `post_type_archive` | `{cpt_slug}` | omit | omit (auto-resolved) |
| External URL / anchor | `custom` | omit | omit | full URL required |

Only use `type: "custom"` for external links, anchors (`#`), or URLs with query parameters. For any WordPress content (pages, posts, CPTs, terms), use the proper type so that slug/title changes propagate automatically.

### Example: Adding a page to a dropdown

```bash
curl -sk -X POST -u "user:pass" \
  -H "Content-Type: application/json" \
  -d '{
    "type": "post_type",
    "object": "page",
    "object_id": 4056,
    "title": "Board of Directors",
    "status": "publish",
    "parent": 4028,
    "menus": 7,
    "menu_order": 4
  }' \
  "https://bremerton-foodline-v2.local/wp-json/wp/v2/menu-items"
```

## Menu Ordering

WordPress does **not** auto-renumber `menu_order` values when inserting items. If you insert at `menu_order: 4` and existing items already occupy that number, you'll get ordering collisions. Either:
- Pick an unused number between existing items
- Or renumber all subsequent items in the same request batch

To see current ordering: `GET /wp/v2/menu-items?menus={menu_id}&per_page=50`

## Cache Invalidation

The REST API calls `wp_update_nav_menu_item()` internally, which handles cache invalidation. No manual cache clearing is needed for front-end rendering after adding/updating menu items via the API.

Note: There is a separate caching issue where newly created **menus** (not items) may not appear in the admin's Appearance > Menus page until transients are cleared — see `wp.md` for details.

## Known Limitation: menuwp-classic + REST API

As of March 2026, the `menuwp-classic` plugin does not sync REST API menu changes to Etch for existing menus. The plugin sets a sync-safe transient only when the admin menu editor page loads (`load-nav-menus.php`). REST API requests never set this transient, so the plugin's conflict detection prevents syncing. This is a known bug — after adding menu items via REST API, you may need to open the WordPress admin menu editor and re-save the menu to trigger the sync.
