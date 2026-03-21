---
description: "Deterministic LLM-first SEO audits for websites, blog posts, and GitHub repositories. Orchestrates 16 specialized skills, 10 specialist agents, and 33 evidence collection scripts."
argument-hint: "<url_or_repo> [--type audit|page|technical|content|schema|sitemap|images|geo|programmatic|competitors|hreflang|plan|github|article|links|aeo]"
---

# SEO Command (Agentic / Claude / Codex)

LLM-first SEO analysis with 16 specialized sub-skills, 10 specialist agents, and 33 scripts for website, blog, and GitHub repository optimization.

## CRITICAL BEHAVIORAL RULES

1. **Execute steps in order.** Do NOT skip ahead, reorder, or merge steps.
2. **Write output files.** Each audit MUST produce its output files before completion.
3. **Stop at checkpoints.** When you reach a checkpoint, STOP and wait for user approval.
4. **Halt on failure.** If any step fails, STOP immediately and present the error.
5. **Use only local agents.** All agent references use agents bundled with this plugin.
6. **Never enter plan mode autonomously.** This command IS the plan — execute it.
7. **INP not FID** — FID was removed September 9, 2024. Never reference FID.
8. **FAQ schema is restricted** — FAQPage limited to government/healthcare only.
9. **HowTo schema is deprecated** — Never recommend.
10. **JSON-LD only** — Always use `<script type="application/ld+json">`.
11. **E-E-A-T everywhere** — As of December 2025, E-E-A-T applies to ALL competitive queries.
12. **Mobile-first is complete** — 100% since July 5, 2024.
13. **LLM-first, resilient pipeline** — If any script fails, continue with LLM reasoning (confidence: `Likely`).

## Pre-flight Checks

### 1. Check for existing session
Look for `FULL-AUDIT-REPORT.md`, `ACTION-PLAN.md`, or `GITHUB-SEO-REPORT.md` in the current directory. If found, ask user whether to resume or start fresh.

### 2. Initialize state
Create a state tracker:
```json
{
  "url": "<target>",
  "type": "<audit_type>",
  "started_at": "<ISO8601>",
  "phase": "preflight",
  "scripts_run": [],
  "scripts_failed": [],
  "agents_delegated": []
}
```

### 3. Parse arguments
Extract URL/repo and audit type from user input.

## Deterministic Trigger Mapping

For prompt reliability in Codex/agent IDEs, map common user wording to a fixed workflow:

- If user says `perform seo analysis on <url>` (or similar generic SEO request with a URL), treat it as a **single-URL full audit**.
- If no explicit sub-skill is specified, run the full/page audit path with **LLM-first reasoning** and script-backed evidence.
- For full/page audits, always produce:
  - `FULL-AUDIT-REPORT.md` (detailed findings)
  - `ACTION-PLAN.md` (prioritized fixes)
- If `generate_report.py` is run, also return the saved HTML path (for example `SEO-REPORT.html`).

---

## Phase 1: Task Identification — Interactive

### Step 1: Identify the Task

Parse the user's request to determine which sub-skill(s) to activate:

| Command | Sub-Skill | Description |
|---------|-----------|-------------|
| `seo audit <url>` | seo-audit | Full website audit with scoring |
| `seo page <url>` | seo-page | Deep single-page analysis |
| `seo technical <url>` | seo-technical | Technical SEO checks |
| `seo content <url>` | seo-content | Content quality & E-E-A-T |
| `seo schema <url>` | seo-schema | Schema detection/validation/generation |
| `seo sitemap <url>` | seo-sitemap | Sitemap analysis & generation |
| `seo images <url>` | seo-images | Image optimization audit |
| `seo geo <url>` | seo-geo | AI search optimization (GEO) |
| `seo programmatic <url>` | seo-programmatic | Programmatic SEO safeguards |
| `seo competitors <url>` | seo-competitor-pages | Comparison/alternatives pages |
| `seo hreflang <url>` | seo-hreflang | International SEO validation |
| `seo plan <url>` | seo-plan | Strategic SEO planning |
| `seo github <repo_or_url>` | seo-github | GitHub repository discoverability |
| `seo article <url>` | seo-article | Article data extraction & LLM optimization |
| `seo links <url>` | seo-links | External backlink profile & link health |
| `seo aeo <url>` | seo-aeo | Answer Engine Optimization |

**Routing logic:**
- **Full audit**: Read `skills/seo-audit/SKILL.md` — crawl multiple pages, delegate to agents, score and report
- **Single page**: Read `skills/seo-page/SKILL.md` — deep dive on one URL
- **Specific area**: Read the matching `skills/seo-*/SKILL.md` file
- **Strategic plan**: Read `skills/seo-plan/SKILL.md` and the matching template from `skills/seo-plan/assets/` for the detected industry
- **GitHub repository SEO**: Read `skills/seo-github/SKILL.md` and use GitHub scripts with `--provider auto`
- **Generic `perform seo analysis on <url>` request**: treat as single-page full audit, read `skills/seo-page/SKILL.md`

---

## Phase 2: Evidence Collection — Execution

### Step 2: Collect Evidence

**Primary method (LLM-first)** — use the built-in `read_url_content` tool first:
```
read_url_content(url)  →  returns parsed HTML content directly
```
Use this as the baseline evidence for reasoning.

**Deterministic verification (recommended when script execution is available)**:
```bash
# Fetch/parse raw HTML for structured checks
python3 <PLUGIN_DIR>/skills/seo-page/scripts/fetch_page.py <url> --output /tmp/page.html
python3 <PLUGIN_DIR>/skills/seo-page/scripts/parse_html.py /tmp/page.html --url <url> --json

# Optional: generate shareable HTML dashboard artifact
python3 <PLUGIN_DIR>/skills/seo-audit/scripts/generate_report.py <url> --output SEO-REPORT.html
```

> **Do not use third-party mirrors (e.g., `r.jina.ai`) as primary evidence when direct site fetch or bundled scripts are available.**
> `<PLUGIN_DIR>` = absolute path to this plugin directory (the parent folder containing `commands/`, `skills/`, `agents/`).

### Step 3: Perform LLM-First Analysis

Use the LLM as the primary SEO analyst:

1. Synthesize evidence from page content, metadata, and optional script outputs.
2. Produce findings with explicit proof:
   - `Finding`
   - `Evidence` (specific element, metric, or snippet)
   - `Impact` (why it matters for ranking/indexing/UX)
   - `Fix` (clear implementation step)
3. Prioritize by impact and implementation effort.
4. Separate confirmed issues, likely issues, and unknowns (missing data).

Always read and apply `skills/seo-audit/references/llm-audit-rubric.md` to keep scoring, severity, confidence, and output structure consistent across audit types.

### Step 4: Run Baseline Verification Scripts (When execution is available)

For full/page audits, run baseline checks to avoid hypothesis-only reporting. Do not replace LLM reasoning with script-only scoring.

```bash
# --- Technical SEO checks (skills/seo-technical/scripts/) ---
# Check robots.txt and AI crawler management
python3 <PLUGIN_DIR>/skills/seo-technical/scripts/robots_checker.py <url>

# Get Core Web Vitals from PageSpeed Insights (free API, no key needed)
python3 <PLUGIN_DIR>/skills/seo-technical/scripts/pagespeed.py <url> --strategy mobile

# Check security headers (HSTS, CSP, X-Frame-Options, etc.)
python3 <PLUGIN_DIR>/skills/seo-technical/scripts/security_headers.py <url>

# Trace redirect chains, detect loops and mixed HTTP/HTTPS
python3 <PLUGIN_DIR>/skills/seo-technical/scripts/redirect_checker.py <url>

# Check hreflang implementation
python3 <PLUGIN_DIR>/skills/seo-technical/scripts/hreflang_checker.py <url>

# --- Page-level checks (skills/seo-page/scripts/) ---
# Analyze readability from fetched HTML (Flesch-Kincaid, grade level, sentence stats)
python3 <PLUGIN_DIR>/skills/seo-page/scripts/readability.py /tmp/page.html --json

# Extract article content and perform keyword research for LLM-driven optimization
python3 <PLUGIN_DIR>/skills/seo-page/scripts/article_seo.py <url> --keyword "<optional_target_keyword>" --json

# --- Link health checks (skills/seo-links/scripts/) ---
# Detect broken links on a page (404s, timeouts, connection errors)
python3 <PLUGIN_DIR>/skills/seo-links/scripts/broken_links.py <url> --workers 5

# Analyze internal link structure, find orphan pages
python3 <PLUGIN_DIR>/skills/seo-links/scripts/internal_links.py <url> --depth 1 --max-pages 20

# --- Shared scripts (scripts/) ---
# Check llms.txt for AI search readiness
python3 <PLUGIN_DIR>/scripts/llms_txt_checker.py <url>

# Validate Open Graph and Twitter Card meta tags
python3 <PLUGIN_DIR>/scripts/social_meta.py <url>

# --- GitHub repository SEO (skills/seo-github/scripts/) ---
python3 <PLUGIN_DIR>/skills/seo-github/scripts/github_repo_audit.py --repo <owner/repo> --provider auto --json
python3 <PLUGIN_DIR>/skills/seo-github/scripts/github_readme_lint.py README.md --json
python3 <PLUGIN_DIR>/skills/seo-github/scripts/github_community_health.py --repo <owner/repo> --provider auto --json
python3 <PLUGIN_DIR>/skills/seo-github/scripts/github_search_benchmark.py --repo <owner/repo> --query "<llm_or_web_query>" --provider auto --json
python3 <PLUGIN_DIR>/skills/seo-github/scripts/github_competitor_research.py --repo <owner/repo> --query "<llm_or_web_query>" --provider auto --top-n 6 --json
python3 <PLUGIN_DIR>/skills/seo-github/scripts/github_traffic_archiver.py --repo <owner/repo> --provider auto --archive-dir .github-seo-data --json
python3 <PLUGIN_DIR>/skills/seo-github/scripts/github_seo_report.py --repo <owner/repo> --provider auto --markdown GITHUB-SEO-REPORT.md --action-plan GITHUB-ACTION-PLAN.md --json
```

If a check fails due network, DNS, permissions, or API rate limits:
- Report it explicitly as an **environment limitation**, not a confirmed site issue.
- Keep confidence as `Hypothesis` for impacted categories.
- Continue with available evidence instead of stopping the audit.
- Do not enter repeated fallback loops. Retry a failed source at most once, then finalize the audit.

**Visual analysis** (requires Playwright — use `conda activate pentest` if available):
```bash
python3 <PLUGIN_DIR>/scripts/capture_screenshot.py <url> --all
python3 <PLUGIN_DIR>/scripts/analyze_visual.py <url> --json
```

**HTML Report Generator** — generates a self-contained interactive HTML dashboard:
```bash
python3 <PLUGIN_DIR>/skills/seo-audit/scripts/generate_report.py <url>
python3 <PLUGIN_DIR>/skills/seo-audit/scripts/generate_report.py <url> --output custom-report.html
```

### PHASE CHECKPOINT
Present evidence collection results to user. Confirm which areas have confirmed data vs hypotheses before proceeding to analysis.

---

## Phase 3: Analysis & Delegation — Execution

### Step 5: Delegate to Specialist Agents

For comprehensive audits, read the relevant agent file from `agents/` to adopt the specialist role:

| Agent | File | Focus Area |
|-------|------|------------|
| Technical SEO | agents/seo-technical.md | Crawlability, indexability, security, URLs, mobile, CWV, JS rendering |
| Content Quality | agents/seo-content.md | E-E-A-T assessment, content metrics, AI content detection |
| Performance | agents/seo-performance.md | Core Web Vitals (LCP, INP, CLS), optimization recommendations |
| Schema Markup | agents/seo-schema.md | Detection, validation, generation of JSON-LD structured data |
| Sitemap | agents/seo-sitemap.md | XML sitemap validation, generation, quality gates |
| Visual Analysis | agents/seo-visual.md | Screenshots, above-the-fold, responsiveness, layout |
| Verifier (global) | agents/seo-verifier.md | Deduplicate findings, suppress contradictions, validate evidence |

### Step 6: Apply Quality Gates

Reference the quality standards in skill reference folders:

- **Content minimums**: Read quality-gates.md for word counts, unique content %, title/meta requirements
- **Schema validation**: Read schema-types.md for active/deprecated/restricted types
- **Core Web Vitals**: Read cwv-thresholds.md for current metric thresholds
- **E-E-A-T framework**: Read eeat-framework.md for scoring criteria
- **Google reference**: Read google-seo-reference.md for quick reference
- **LLM report rubric**: Read llm-audit-rubric.md for mandatory evidence format, confidence labels, and output contract

### Step 6.5: Verify Findings (All Workflows)

Before writing final reports, run verification:

```bash
python3 <PLUGIN_DIR>/skills/seo-github/scripts/finding_verifier.py --findings-json <raw_findings.json> --json
```

Use verified output for final report tables, not raw findings.

---

## Phase 4: Scoring & Reporting — Execution

### Step 7: Score and Report

Use numeric scores as guidance, not as a replacement for evidence quality and judgment.

#### Default Scoring Weights (Full Audit)

| Category | Weight |
|----------|--------|
| Technical SEO | 25% |
| Content Quality | 20% |
| On-Page SEO | 15% |
| Schema / Structured Data | 15% |
| Performance (CWV) | 10% |
| Image Optimization | 10% |
| AI Search Readiness (GEO) | 5% |

#### Score Interpretation
| Score | Rating |
|-------|--------|
| 90-100 | Excellent |
| 70-89 | Good |
| 50-69 | Needs Improvement |
| 30-49 | Poor |
| 0-29 | Critical |

### Step 8: Mandatory Deliverables

For `seo audit`, `seo page`, and generic `perform seo analysis on <url>` flows:

1. Create `FULL-AUDIT-REPORT.md` in the current working directory at the start of the audit, then update it as evidence is collected.
2. Create `ACTION-PLAN.md` in the current working directory at the start of the audit, then update it with prioritized fixes.
3. If HTML dashboard was generated, include its exact saved path.
4. In the final response, explicitly list generated artifacts and paths.
5. If technical checks are blocked by environment limits, still write both markdown files and include an "Environment Limitations" section.

---

## Industry Detection

When running `seo plan`, detect the business type and load the matching template:

| Industry | Template File |
|----------|---------------|
| SaaS / Software | skills/seo-plan/assets/saas.md |
| Local Service Business | skills/seo-plan/assets/local-service.md |
| E-commerce / Retail | skills/seo-plan/assets/ecommerce.md |
| Publisher / Media | skills/seo-plan/assets/publisher.md |
| Agency / Consultancy | skills/seo-plan/assets/agency.md |
| Other / Generic | skills/seo-plan/assets/generic.md |

**Detection signals:**
- SaaS: pricing page, feature pages, /docs, /api, trial/demo CTAs
- Local: address, phone, Google Business Profile, service area pages
- E-commerce: product pages, cart, checkout, /collections, /categories
- Publisher: article dates, author pages, /news, high content volume
- Agency: case studies, /work, /portfolio, team pages, service offerings

---

## Schema Templates

Pre-built JSON-LD templates are available in `skills/seo-schema/assets/templates.json` for:
- **Common**: BlogPosting, Article, Organization, LocalBusiness, BreadcrumbList, WebSite (with SearchAction)
- **Video**: VideoObject, BroadcastEvent, Clip, SeekToAction
- **E-commerce**: ProductGroup (variants), OfferShippingDetails, Certification
- **Other**: SoftwareSourceCode, ProfilePage (E-E-A-T author pages)

---

## Validation Scripts

Two validation scripts are available for CI/CD integration:

### Pre-commit SEO Check
```bash
bash <PLUGIN_DIR>/scripts/pre_commit_seo_check.sh
```

### Schema Validator
```bash
python3 <PLUGIN_DIR>/skills/seo-schema/scripts/validate_schema.py <file_path>
```

---

## Output Format

All sub-skill reports should use consistent severity levels:
- **Critical** — Directly impacts rankings or indexing (fix immediately)
- **Warning** — Optimization opportunity (fix within 1 month)
- **Pass** — Meets or exceeds standards
- **Info** — Not applicable or informational only

Structure reports as:
1. Summary table with element, value, and severity
2. Detailed findings grouped by category
3. Actionable recommendations ordered by impact

---

## Completion

Present the user with:
1. List of all generated artifacts and their file paths
2. Overall SEO Health Score with rating
3. Top 5 critical issues requiring immediate action
4. Top 5 quick wins for fast improvement
5. Recommended next steps

---

## Dependencies

### Optional Script Dependencies
- Python 3.8+
- `requests` (for network analysis scripts)
- `beautifulsoup4` (for HTML parsing scripts)
- Playwright (for `capture_screenshot.py` and `analyze_visual.py`)
  ```bash
  pip install playwright && playwright install chromium
  ```

### Install Script Dependencies
```bash
pip install requests beautifulsoup4
```
