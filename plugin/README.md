# Local SEO Plugin for Claude Code

Complete local SEO automation. Point at any service-area business website and get a full optimization pipeline covering Google Business Profile, technical health, website SEO, authority building, content strategy, and monthly reporting.

## Installation

```bash
# Development / testing (session-only):
claude --plugin-dir /path/to/super-seo/plugin

# Install for current project:
claude plugin install local-seo@super-seo --scope project

# Install for all projects:
claude plugin install local-seo@super-seo --scope user
```

## Quick Start

```
/seo https://example-plumbing.com
```

This auto-discovers the business details from the website, sets up context, and offers a quick audit or full 12-week pipeline.

## Commands

| Command | Purpose |
|---------|---------|
| `/seo [url]` | Quick start -- point at a website and go |
| `/seo --audit [url]` | Quick 3-part audit (technical + GBP + citations) |
| `/seo --full [url]` | Start full 12-week pipeline |

## Skills (24 audit types)

| Skill | Sub-commands |
|-------|-------------|
| `local-seo-context` | setup, switch, update, status |
| `local-seo-gbp` | category-audit, attributes-audit, competitor-reviews, review-strategy, posts-strategy, services-optimization, description-optimization, photo-audit, video-strategy, direct-booking |
| `local-seo-website` | technical-audit, keyword-gap, money-page-audit, service-pages, gsc-analysis, review-sentiment |
| `local-seo-authority` | backlink-audit, citation-audit, search-intent, spam-audit |
| `local-seo-content` | gap-analysis, entity-optimization, competitor-posts, monthly-report |
| `local-seo-pipeline` | start, resume, status, week N, reset |

## Requirements

- Claude Code CLI
- No paid SEO tools required (uses WebSearch + WebFetch)
- Optional: Google Search Console CSV exports for deeper analysis

## License

Private. Not for redistribution.
