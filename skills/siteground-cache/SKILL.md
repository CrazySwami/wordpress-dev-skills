# SiteGround Cache Buster Skill

Bypass SiteGround caching (SG CachePress + LiteSpeed) for WordPress development. Adds cache-busting code to child themes for real-time development testing.

---

## IMPORTANT: Staging First Workflow

**ALWAYS deploy to staging before production!**

```
Local Development → Staging Site → Production Site
                    ↑
                    Test here first!
```

SiteGround provides free staging environments. Use them!

---

## Required Information

Before using this skill, Claude will ask for:

1. **FTP/SFTP Credentials**
   - Hostname (e.g., `ftp.example.com`)
   - Username
   - Password
   - Port (usually 21 for FTP, 22 for SFTP)

2. **Site URLs**
   - Staging URL (e.g., `https://staging.example.com`)
   - Production URL (e.g., `https://example.com`)

3. **Theme Path**
   - Child theme folder name (e.g., `theme-child`)

**Store credentials in project's `CLAUDE.local.md`** (gitignored) for future sessions.

---

## Quick Start

```bash
# Add cache busting to a child theme
./add-cache-buster.sh /path/to/child-theme

# Or manually add the PHP snippet to functions.php
```

---

## What It Does

1. **Disables server-side caching for admins** - LiteSpeed Cache + SG CachePress
2. **Adds no-cache headers** - Prevents CDN/proxy caching
3. **Busts browser cache** - Adds timestamp to CSS/JS URLs
4. **Shows version banner** - Visual confirmation theme is loading (admin only)

---

## The Code

### PHP Snippet (add to functions.php)

```php
/**
 * SiteGround Cache Buster for Development
 * Disables caching for logged-in administrators
 * REMOVE OR DISABLE IN PRODUCTION when done testing
 */

// Development mode banner (shows version to confirm theme is active)
add_action('wp_head', 'sg_dev_mode_banner');
function sg_dev_mode_banner() {
    if (current_user_can('administrator')) {
        $theme = wp_get_theme();
        $version = $theme->get('Version');
        $name = $theme->get('Name');
        echo '<style>
            .sg-dev-banner {
                position: fixed;
                bottom: 20px;
                right: 20px;
                background: #34889A;
                color: white;
                padding: 10px 20px;
                border-radius: 8px;
                font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
                font-size: 12px;
                z-index: 999999;
                box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            }
        </style>
        <div class="sg-dev-banner">
            ' . esc_html($name) . ' v' . esc_html($version) . ' - ' . date('M j, H:i') . '
        </div>';
    }
}

// Disable all caching for administrators
add_action('init', 'sg_disable_cache_for_dev');
function sg_disable_cache_for_dev() {
    if (current_user_can('administrator')) {
        // Disable LiteSpeed Cache
        if (!defined('LSCACHE_NO_CACHE')) {
            define('LSCACHE_NO_CACHE', true);
        }

        // Disable SG Optimizer/CachePress
        if (!defined('SG_CACHEPRESS_NO_CACHE')) {
            define('SG_CACHEPRESS_NO_CACHE', true);
        }

        // Send no-cache headers
        nocache_headers();

        // Additional headers for CDN bypass
        header('Cache-Control: no-store, no-cache, must-revalidate, max-age=0');
        header('Pragma: no-cache');
        header('Expires: Thu, 01 Jan 1970 00:00:00 GMT');
    }
}

// Bust browser cache by adding timestamp to theme CSS/JS
add_filter('style_loader_src', 'sg_bust_asset_cache', 999);
add_filter('script_loader_src', 'sg_bust_asset_cache', 999);
function sg_bust_asset_cache($src) {
    if (current_user_can('administrator')) {
        // Only bust cache for theme assets
        $theme_uri = get_stylesheet_directory_uri();
        $parent_uri = get_template_directory_uri();

        if (strpos($src, $theme_uri) !== false || strpos($src, $parent_uri) !== false) {
            $src = add_query_arg('v', time(), $src);
        }
    }
    return $src;
}
```

---

## Usage

### Method 1: Auto-inject via Script

```bash
# Navigate to your project
cd /path/to/wordpress-project

# Run the injector script
/root/.claude/skills/siteground-cache/add-cache-buster.sh ./wp-content/themes/your-child-theme
```

### Method 2: Manual Copy

1. Copy the PHP code above
2. Paste at the end of your child theme's `functions.php`
3. Upload via FTP
4. Visit site as admin - you should see the version banner

### Method 3: Via Claude

Just ask:
- "Add SiteGround cache busting to this theme"
- "Enable dev mode for SiteGround"
- "Add cache buster to functions.php"

---

## How Each Part Works

### 1. LiteSpeed Cache Bypass
```php
define('LSCACHE_NO_CACHE', true);
```
LiteSpeed Cache plugin checks for this constant and skips caching when set.

### 2. SG CachePress Bypass
```php
define('SG_CACHEPRESS_NO_CACHE', true);
```
SiteGround's caching plugin respects this constant.

### 3. HTTP Headers
```php
nocache_headers();
header('Cache-Control: no-store, no-cache, must-revalidate, max-age=0');
```
Tells browsers and CDNs not to cache the response.

### 4. Asset Cache Busting
```php
add_query_arg('v', time(), $src);
```
Adds `?v=1704567890` to CSS/JS URLs. Since the timestamp changes every second, browsers always fetch fresh files.

---

## Removing for Production

When you're done developing, either:

1. **Delete the code** from functions.php
2. **Wrap in a constant check:**

```php
// Add to wp-config.php: define('WP_DEV_MODE', true);
if (defined('WP_DEV_MODE') && WP_DEV_MODE) {
    // All the cache busting code here
}
```

---

## Troubleshooting

### Banner not showing?
- Make sure you're logged in as Administrator
- Check if child theme is activated (Appearance > Themes)
- Check for PHP errors in the error log

### Still seeing cached content?
1. Try incognito/private browser window
2. Clear browser cache manually
3. Check SiteGround Site Tools > Speed > Caching > Purge Cache
4. Check if Cloudflare is in front (need to purge there too)

### CSS changes not appearing?
- View page source and check if `?v=` timestamp is on CSS URLs
- Hard refresh: Ctrl+Shift+R (Windows) or Cmd+Shift+R (Mac)

---

## Files in This Skill

```
/root/.claude/skills/siteground-cache/
├── SKILL.md                 # This documentation
├── add-cache-buster.sh      # Auto-inject script
├── cache-buster.php         # Standalone PHP snippet
└── remove-cache-buster.sh   # Removal script
```

---

## Deployment Workflow

### Step 1: Get Credentials (Ask User)

Before deploying, ask the user:
```
I need FTP credentials to deploy to SiteGround. Please provide:
1. FTP Host (e.g., ftp.yourdomain.com)
2. FTP Username
3. FTP Password
4. Staging site path (e.g., staging.yourdomain.com/public_html)
5. Production site path (e.g., yourdomain.com/public_html)
```

### Step 2: Deploy to Staging FIRST

```bash
# Always test on staging first!
lftp -u "user,password" -e "
    set ssl:verify-certificate no
    mirror -R ./child-theme staging.example.com/public_html/wp-content/themes/child-theme
    bye
" ftp://ftp.example.com
```

### Step 3: Verify on Staging

1. Visit staging site as admin
2. Confirm dev banner appears (bottom-right)
3. Test CSS/JS changes are visible
4. Check for PHP errors

### Step 4: Deploy to Production (After Testing)

```bash
# Only after staging is verified!
lftp -u "user,password" -e "
    set ssl:verify-certificate no
    mirror -R ./child-theme example.com/public_html/wp-content/themes/child-theme
    bye
" ftp://ftp.example.com
```

### Step 5: Clear Production Cache

1. Log into WordPress admin
2. Click "Purge SG Cache" in admin bar
3. Or: SiteGround Site Tools > Speed > Caching > Flush

---

## Example CLAUDE.local.md Template

Store this in your project (gitignored):

```markdown
# SiteGround Credentials (DO NOT COMMIT)

## FTP Access
- Host: ftp.example.com
- User: user@example.com
- Pass: your-password
- Port: 21

## Site URLs
- Staging: https://staging.example.com
- Production: https://example.com

## Theme Paths
- Staging: staging.example.com/public_html/wp-content/themes/theme-child
- Production: example.com/public_html/wp-content/themes/theme-child
```

---

## Related

- **wp-docker** - Local WordPress development
- **wp-performance** - Production caching optimization
- **visual-qa** - Screenshot testing after changes
