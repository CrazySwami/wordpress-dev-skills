# WordPress Development Skills for Claude Code

A comprehensive WordPress development and automation toolkit for Claude Code. Set up complete WordPress sites with a single command, using Docker or Playground environments, with white-labeling (FREE plugins only), SEO, and visual QA.

## Quick Start

```bash
# Set up a new WordPress site
/wp-setup

# Audit an existing site
/wp-audit

# Pre-launch checklist
/wp-launch
```

## Skills Included

| Skill | Description |
|-------|-------------|
| **wp-orchestrator** | Master orchestrator - coordinates all skills, slash commands |
| **wp-docker** | Docker Compose WordPress environment with WP-CLI |
| **wp-playground** | WordPress Playground blueprints for instant testing |
| **white-label** | FREE white-labeling with ASE + Branda + Admin Menu Editor |
| **wordpress-dev** | Development best practices, coding standards, CPT creation |
| **wordpress-admin** | Page/post management, WP-CLI, REST API operations |
| **seo-optimizer** | Yoast/Rank Math audit - focus keywords, meta descriptions |
| **visual-qa** | Screenshot automation with GSAP animation handling |
| **brand-guide** | Brand documentation - colors, fonts, imagery, voice/tone |
| **gsap-animations** | GSAP best practices, accessibility, responsive animations |
| **wp-performance** | Core Web Vitals, image/video optimization, caching |

## Slash Commands

| Command | Description |
|---------|-------------|
| `/wp-setup` | Set up a new WordPress site with Docker, install plugins, configure white-labeling |
| `/wp-audit` | Comprehensive site audit - SEO, performance, security, visual QA in parallel |
| `/wp-launch` | Pre-launch checklist and handoff documentation generation |

## Installation

### Method 1: Git Clone (Recommended)

```bash
# Clone into Claude Code skills directory
git clone https://github.com/hustleserver/wordpress-dev-skills.git ~/.claude/skills/wordpress-dev-skills

# Or clone and symlink individual skills
git clone https://github.com/hustleserver/wordpress-dev-skills.git /path/to/wordpress-dev-skills
ln -s /path/to/wordpress-dev-skills/skills/* ~/.claude/skills/
```

### Method 2: Install Script

```bash
curl -sL https://raw.githubusercontent.com/hustleserver/wordpress-dev-skills/main/install.sh | bash
```

### Method 3: Manual Download

1. Download the [latest release](https://github.com/hustleserver/wordpress-dev-skills/releases)
2. Extract to `~/.claude/skills/`
3. Skills are automatically available

## Requirements

- **Python 3.9+** - For SEO audit and visual QA scripts
- **Playwright** - For screenshot automation (`pip install playwright && playwright install`)
- **curl** - For API requests
- **Docker** (optional) - For local WordPress database access

## Usage

Once installed, ask Claude Code:

### WordPress Development

```
"Create a custom post type for properties"
"Review this code for WordPress security issues"
"Generate a meta box for property details"
"What are the best practices for enqueueing scripts?"
```

### SEO Optimization

```
"Run an SEO audit on all pages"
"Check SEO for the about page"
"Fix missing featured images"
"Update meta descriptions to include focus keywords"
```

### Visual QA

```
"Take screenshots of all pages"
"Run visual QA on the portfolio page"
"Check responsive layouts at all breakpoints"
```

### Brand Guide

```
"Generate a brand style guide"
"Document the color palette"
"Create typography guidelines"
```

### White-Labeling (FREE Plugins)

```
"White label the admin for client handoff"
"Configure the login page with client branding"
"Set up custom login URL and security"
"Apply Branda settings for admin bar"
```

### Docker Environment

```
"Set up a Docker WordPress environment"
"Start the WordPress containers"
"Run WP-CLI commands in Docker"
```

### WordPress Playground

```
"Test this in WordPress Playground"
"Create a Playground blueprint with WooCommerce"
"Share a Playground demo link"
```

### GSAP Animations

```
"Add scroll-triggered animations to this section"
"Make sure animations respect prefers-reduced-motion"
"Create a staggered card entrance animation"
"Set up responsive animations for mobile"
```

### Performance Optimization

```
"Optimize images for Core Web Vitals"
"Compress videos for web delivery"
"Configure LiteSpeed Cache settings"
"Run a speed test on all pages"
```

### Project Orchestration

```
"Set up a new WordPress project"
"Audit this WordPress site"
"Prepare site for launch"
"Generate a handoff document for the client"
```

## Skill Details

### wordpress-dev

Provides comprehensive WordPress development guidance:

- **Coding Standards**: PHP, JS, CSS conventions
- **Custom Post Types**: Complete CPT registration guide
- **Security**: Sanitization, escaping, nonces, SQL safety
- **Performance**: Caching, query optimization, asset loading
- **Hooks & Filters**: Actions and filters reference
- **Template Hierarchy**: Theme template structure

Includes code templates for:
- Custom Post Type registration
- Custom Taxonomy registration
- Meta Box with save handling
- REST API endpoints
- Plugin skeleton

### seo-optimizer

Audits WordPress pages for SEO completeness:

- Focus keyword presence and usage
- Meta description quality (length, keyword inclusion)
- Featured image presence and metadata
- SEO title optimization

Supports Yoast SEO and Rank Math plugins.

### visual-qa

Automated visual testing with Playwright:

- Full-page scroll to trigger GSAP/ScrollTrigger animations
- Screenshots at 10 viewports (desktop, tablet, mobile)
- Parallel analysis with Haiku sub-agents

Default viewports:
- Desktop: 1920px, 1440px, 1280px
- Tablet: 768px, 1024px, 744px
- Mobile: 390px, 393px, 375px, 412px

### wordpress-admin

WordPress site management:

- Create/update pages and posts
- Configure Yoast SEO settings
- Upload and manage media
- WP-CLI command reference
- REST API endpoints

### white-label

Complete WordPress white-labeling using FREE plugins only:

- **ASE**: Admin cleanup, security (login URL, XML-RPC, author slugs)
- **Branda**: Login page customization (logo, colors, backgrounds)
- **Admin Menu Editor**: Menu organization, role-based hiding
- WP-CLI automation for programmatic configuration
- Client handoff checklist

### wp-docker

Docker Compose WordPress environment:

- MariaDB 10.11 with health checks
- WordPress PHP 8.3 Apache
- WP-CLI container for automation
- Automated setup script with plugin installation
- Environment variable configuration

### wp-playground

WordPress Playground blueprints:

- Instant browser-based WordPress testing
- Pre-configured blueprints with plugins
- CLI usage with `@wp-playground/cli`
- URL parameters for quick customization
- Snapshot export for sharing

### gsap-animations

GSAP animation best practices:

- WordPress enqueue patterns for GSAP/ScrollTrigger
- Animation patterns (fade, stagger, parallax, text reveal, curtain)
- Accessibility with `prefers-reduced-motion` support
- Responsive animations with `gsap.matchMedia()`
- Visual QA integration with `completeAllAnimations()` helper
- Reusable animation library with data attributes

### wp-performance

Core Web Vitals and performance optimization:

- LCP, INP, CLS targets and measurement
- EWWW Image Optimizer configuration
- FFmpeg video compression (H.264/H.265)
- LiteSpeed Cache settings optimization
- CSS/JS defer and minification
- Database optimization queries
- Automated speed testing script

### wp-orchestrator

Master WordPress project orchestrator:

- Coordinates all WordPress skills
- Discovery interview phases (project type, requirements, brand)
- Site audit workflows (SEO, visual, performance, security)
- Todo list generation for project phases
- Parallel agent patterns using Haiku sub-agents
- Audit report and handoff documentation templates
- Pre-launch checklist generation

## Configuration

### For Docker-based WordPress

Update database config in `skills/seo-optimizer/audit.py`:

```python
DB_CONTAINER = "your-db-container-name"
DB_USER = "your-db-user"
DB_PASS = "your-db-password"
DB_NAME = "your-db-name"
```

### For Visual QA

Update base URL in `skills/visual-qa/screenshot.py`:

```python
DEFAULT_BASE_URL = "https://your-site.com"
DEFAULT_OUTPUT = "/path/to/screenshots"
```

## Directory Structure

```
wordpress-dev-skills/
├── plugin.json              # Plugin manifest
├── README.md                # This file
├── CHANGELOG.md             # Version history
├── install.sh               # Installation script
├── skills/
│   ├── wp-orchestrator/     # Master orchestrator
│   ├── wp-docker/           # Docker environment
│   │   ├── SKILL.md
│   │   └── templates/       # docker-compose.yml, wp-setup.sh
│   ├── wp-playground/       # WordPress Playground
│   │   ├── SKILL.md
│   │   └── blueprints/      # base.json, woocommerce.json
│   ├── white-label/         # Admin white-labeling
│   ├── wordpress-dev/       # Development best practices
│   ├── wordpress-admin/     # Admin management
│   ├── seo-optimizer/       # SEO audit
│   ├── visual-qa/           # Visual testing
│   ├── brand-guide/         # Brand documentation
│   ├── gsap-animations/     # GSAP best practices
│   └── wp-performance/      # Performance optimization
├── commands/
│   ├── wp-setup.md          # /wp-setup slash command
│   ├── wp-audit.md          # /wp-audit slash command
│   └── wp-launch.md         # /wp-launch slash command
└── hooks/
    └── wp-session-init.sh   # WordPress session initialization
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

MIT License - See [LICENSE](LICENSE) for details.

## Support

- [Issues](https://github.com/hustleserver/wordpress-dev-skills/issues)
- [Discussions](https://github.com/hustleserver/wordpress-dev-skills/discussions)

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.
