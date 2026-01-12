# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based blog site with a custom theme implementation. The site uses a hierarchical category system with a sidebar navigation and customized layouts based on the Minima theme structure.

## Development Commands

### Local Development
```bash
bundle exec jekyll serve
```
This starts a local development server with auto-regeneration on file changes. The server watches for file updates and rebuilds automatically.

**Important**: Changes to `_config.yml` require a server restart to take effect.

### Build
```bash
bundle exec jekyll build
```
Generates the static site in the `_site` directory.

### Dependencies
```bash
bundle install
```
Install or update Ruby gem dependencies from the Gemfile.

## Architecture

### Theme Customization
The site uses a customized version of the Minima theme. Instead of using the theme gem, theme files are vendored locally in:
- `_layouts/`: Page layout templates
- `_includes/`: Reusable HTML partials
- `_sass/`: Stylesheet files

### Category System
The blog implements a **hierarchical category system** with client-side filtering using slash-separated category names:
- Categories are defined in post front matter as arrays: `categories: [jekyll/update]`
- The system supports up to 3 levels: `level1/level2/level3`
- Category parsing happens in `_includes/sidebar.html` and `_pages/categories.md`
- The sidebar navigation (`_includes/sidebar.html`) automatically builds a nested menu from categories
- Category state (open/closed) is persisted in localStorage

**Category Filtering**:
- All category links use hash-based routing: `/#category-name` (e.g., `/#jekyll/update`)
- Clicking a category navigates to the home page with the filter applied via URL hash
- Client-side JavaScript in `_layouts/home.html` filters posts based on the hash
- Hierarchical filtering: parent categories show posts from all subcategories (e.g., `#jekyll` shows posts from `jekyll`, `jekyll/update`, `jekyll/tutorial`, etc.)
- Active category is highlighted in the sidebar
- Post items have `data-categories` attribute for filtering
- Empty state message displays when no posts match the filter

### Layout Hierarchy
1. `default.html`: Base layout including header, sidebar, content area, and footer
2. `home.html`: Extends default layout, displays post list with client-side filtering JavaScript
3. `post.html`: Extends default layout for individual blog posts
4. `page.html`: Extends default layout for static pages

### Key Components

**Sidebar (`_includes/sidebar.html`)**:
- Displays profile information
- Dynamically generates hierarchical category navigation from site.categories
- Category links use hash routing: `href="{{ '/#' | append: category }}"`
- Uses JavaScript for interactive submenu toggling with hover effects
- Persists category expansion state in localStorage
- Active category highlighting via `.active-category` CSS class

**Home Page (`_layouts/home.html`)**:
- Displays all posts with filtering capability
- Post items include `data-categories` attribute with comma-separated categories
- Client-side JavaScript filters posts based on URL hash
- Hierarchical matching logic: parent categories include all child posts
- Updates sidebar highlighting when filter changes
- Shows empty state when no posts match

**Categories Page (`_pages/categories.md`)**:
- Displays full 3-level category tree with collapsible sections
- Shows post counts for each category: `(X)`
- Category links navigate to home page with hash filter
- Visual hierarchy: different indentation, font sizes, and colors per level
- Uses inline JavaScript for toggle functionality (▶/▼)

### Post Structure
Blog posts must:
- Be placed in `_posts/` directory
- Follow naming convention: `YYYY-MM-DD-title.markdown`
- Include front matter with layout, title, date, and categories array

### Site Configuration
Key settings in `_config.yml`:
- Site metadata (title, description, email)
- Custom `_pages` directory included in build
- SASS directory configured to `_sass`
- Plugins: jekyll-feed, jekyll-seo-tag

### Styling
Main stylesheet: `_sass/minima.scss`
- Dark theme: Dark background (`#1e1e1e`), light text (`#eeeeee`), red accent color (`#ff5c57`)
- Sidebar styles: Fixed position, 350px width, custom profile section
- Category system styles:
  - `.active-category`: Highlights selected category in sidebar
  - `.no-posts`: Empty state styling
  - `.post-item`: Smooth opacity transitions for filtering
  - Submenu animations: `max-height` transition (1.5s ease) for expand/collapse

## Important Implementation Details

### Adding/Modifying Categories
When working with the category system:
- Category links MUST use `{{ '/#' | append: category | relative_url }}` format to work from any page
- The filtering JavaScript only exists in `_layouts/home.html`, so categories redirect there
- Hierarchical matching uses `category.startsWith(filterCategory + '/')` logic
- Parent categories automatically include all child category posts

### Modifying the Filtering Logic
The filtering system consists of three coordinated parts:
1. **Data layer**: `data-categories` attributes on post items in `_layouts/home.html`
2. **Hash routing**: Category links with `/#category` format
3. **JavaScript**: Client-side filtering logic that reads hash and shows/hides posts

All three must stay synchronized. When modifying filtering, update all three locations.
