---
name: Equals
description: Use when building live analyses, dashboards, and reports from connected data sources. Reach for Equals when you need to query databases, data warehouses, or SaaS tools; transform raw data into analysis-ready tables; build interactive dashboards; or automate report distribution to stakeholders.
metadata:
    mintlify-proj: equals
    version: "1.0"
---

# Product summary

Equals is a spreadsheet-based BI platform that combines live data querying with spreadsheet analysis and automated reporting. Connect to SQL databases, data warehouses (Snowflake, BigQuery, Redshift), and SaaS tools (Salesforce, HubSpot, Stripe, QuickBooks) to query live data directly into workbooks. Build analyses using spreadsheet formulas, pivot tables, and charts, then transform them into dashboards that auto-refresh and auto-distribute to Slack or email on custom schedules. The Equals Warehouse (powered by Snowflake) automatically syncs multiple datasources, enabling cross-source joins and transformations via Views. Key files and concepts: **Workspaces** (company accounts containing workbooks, datasources, queries, views), **Workbooks** (analysis files with connected sheets for querying and regular sheets for analysis), **Datasources** (connections to databases, warehouses, and SaaS apps), **Views** (SQL-based transformations synced to your schema), **Dashboards** (live reports built from workbook objects). Primary docs: https://docs.equals.com

# When to use

Use Equals when you need to:
- **Query live data** from databases, warehouses, or SaaS tools without leaving a spreadsheet
- **Build analyses** that automatically refresh when underlying data changes
- **Create dashboards** from spreadsheet analyses and distribute them to stakeholders on a schedule
- **Join data** across multiple datasources (requires Equals Warehouse support)
- **Transform raw data** into analysis-ready tables using SQL Views
- **Automate reporting** by scheduling queries and dashboard sends to email or Slack
- **Collaborate** on analyses with workspace members with granular permission controls
- **Pull custom data** from APIs using import scripts (JavaScript or Python)

Reach for Equals when users ask for "live dashboards," "automated reports," "cross-database analysis," or "spreadsheet-based BI."

# Quick reference

## Core workflow
| Task | Steps |
|------|-------|
| **Connect a datasource** | Workspace > Datasources > New datasource > Follow setup guide (or use specific guide for your provider) |
| **Query data** | Workbook > Connect to data > Select datasource > Use query builder or SQL editor > Run query |
| **Schedule a query** | Open Connections panel > Select query > Click Schedule > Choose frequency (hourly/daily/weekly) |
| **Create a View** | Workspace > Views > New View > Select Equals Warehouse datasource > Write SQL > Preview > Save |
| **Build a dashboard** | Workbook > Dashboard tab > Add charts/tables/cells/text > Publish to set distribution schedule |
| **Share a workbook** | Workbook > Share > Set permission (Read only / Can edit / Dashboard only) > Share with users or workspace |

## Datasource types
| Type | Synced to Warehouse | Join capability | Setup complexity |
|------|-------------------|-----------------|------------------|
| **Equals Warehouse** (HubSpot, Salesforce, Stripe, Xero, etc.) | Yes | Yes | Low (guided setup) |
| **SQL databases** (Postgres, MySQL, etc.) | No | No | Medium (connection details + IP allowlist) |
| **Data warehouses** (Snowflake, BigQuery, Redshift) | No | No | Medium (connection details + IP allowlist) |
| **Import scripts** (custom APIs) | No | No | High (JavaScript/Python code) |

## Permission levels
| Role | Workbook access | Datasource access | Workspace seat |
|------|-----------------|-------------------|-----------------|
| **Editor** | Can edit shared workbooks | Can query shared datasources | Yes (paid) |
| **Viewer** | Read-only on shared workbooks | Can query shared datasources | Yes (paid) |
| **Guest** | View specific workbooks only | None | No (external) |

## Key keyboard shortcuts
- `Cmd/Ctrl + K` — Open command bar (access all commands and AI Assist)
- `Cmd/Ctrl + J` — Chat with AI Assist
- `Cmd/Ctrl + Z` — Undo
- `Cmd/Ctrl + Y` — Redo
- `F4` — Toggle cell locks
- `Ctrl + Space` — Select column
- `Shift + Space` — Select row

# Decision guidance

## When to use Query Builder vs SQL Editor
| Scenario | Use Query Builder | Use SQL Editor |
|----------|-------------------|----------------|
| Simple filters, sorts, joins | ✓ | |
| No SQL knowledge | ✓ | |
| Complex transformations, CTEs, window functions | | ✓ |
| Need to convert later | ✓ (can switch to SQL) | |

## When to use Views vs Calculated Columns
| Scenario | Use Views | Use Calculated Columns |
|----------|-----------|------------------------|
| Reuse across multiple workbooks | ✓ | |
| Join multiple datasources | ✓ | |
| Transform queried data in one workbook | | ✓ |
| Share with team as single source of truth | ✓ | |
| Extract dates, categorize, simple math | | ✓ |

## When to use Saved Queries vs regular queries
| Scenario | Use Saved Queries | Use Regular Queries |
|----------|-------------------|-------------------|
| Query used in multiple workbooks | ✓ | |
| Need to update query in one place | ✓ | |
| One-off analysis | | ✓ |
| Share query with team | ✓ | |

## When to use Import Scripts vs native datasources
| Scenario | Use Import Scripts | Use Native Datasource |
|----------|-------------------|----------------------|
| API not natively supported | ✓ | |
| Need custom data transformation | ✓ | |
| Simple connection available | | ✓ |
| Requires authentication token | ✓ (use secret groups) | |

# Workflow

## Typical analysis workflow

1. **Set up workspace** — Create workspace, enable auto-join if using company domain, configure default timezone and location
2. **Connect datasources** — Go to Datasources, add each data source (SaaS tools, databases, warehouses), share with team members who need access
3. **Create Views (optional)** — If joining multiple datasources or creating reusable transformations, create Views in the Views section using SQL
4. **Create workbook** — New workbook > Create connected sheet > Select datasource > Write query (builder or SQL) > Run to pull data
5. **Build analysis** — Add formulas, pivot tables, charts to analyze the data; use calculated columns to transform queried data
6. **Schedule queries** — Open Connections panel > Select query > Schedule to refresh hourly/daily/weekly so analysis stays current
7. **Build dashboard** — Go to Dashboard tab > Add charts, tables, cells, text, AI summaries > Arrange and resize
8. **Distribute reports** — Click Publish > Add destination (email or Slack) > Set schedule (hourly/daily/weekly/monthly) > Save
9. **Share with team** — Click Share > Set permission level > Share with individuals or entire workspace

## Typical View creation workflow

1. **Navigate to Views** — Workspace home > Views section
2. **Create new View** — Click New View > Select Equals Warehouse datasource
3. **Write SQL** — Use Table Browser to explore schema > Write SELECT query > Reference other Views if needed
4. **Preview** — Click Preview to validate output (required before saving)
5. **Save and rebuild** — Click Save and Rebuild > Wait for sync to Equals Warehouse
6. **Query in workbooks** — Open any workbook > Connect to data > Select View > Use in analysis

## Typical dashboard distribution workflow

1. **Prepare workbook** — Ensure all queries are scheduled to run before dashboard send time
2. **Build dashboard** — Add all charts, tables, cells, and text to Dashboard tab
3. **Set up distribution** — Click Publish > Add destination (email or Slack)
4. **Configure schedule** — Choose frequency (hourly/daily/weekly/monthly) and time
5. **Test send** — Perform test send to verify appearance before saving
6. **Save destination** — Click Save > Dashboard will auto-send on schedule

# Common gotchas

- **Equals Warehouse datasources only** — Views can only be created from datasources that sync to Equals Warehouse (HubSpot, Salesforce, Stripe, etc.). SQL databases and warehouses cannot be used to create Views or joined with other datasources.
- **IP allowlist required** — When connecting SQL databases or warehouses, you must allowlist Equals' IP (`54.68.61.53`) in your database security settings. Without this, connections will fail silently.
- **Calculated columns are read-only when shared** — If you create calculated columns in a saved query, they appear as result-only fields (purple) in workbooks. Users cannot edit them.
- **Timezone matters for scheduling** — Query and dashboard send schedules use the workspace's default timezone (set in Workspace Settings). If you change timezone, existing schedules may run at unexpected times.
- **Datasource access controls query access** — Users can only run queries from datasources they have access to. Sharing a workbook does not grant datasource access; you must share the datasource separately.
- **Views require preview before save** — You cannot save a View without clicking Preview at least once. The preview validates the SQL and ensures the output is correct.
- **Single dashboard per workbook** — Each workbook has exactly one dashboard. You cannot create multiple dashboards in a single workbook.
- **Auto-expand requires adjacent cells** — Auto-expand only works if your analysis (table, chart, pivot) is directly adjacent to queried data. Gaps or non-contiguous ranges break auto-expand.
- **Spill range errors block array formulas** — If an array formula returns `#SPILL!`, check that no cells in the spill range are blocked or contain data.
- **Saved query changes propagate everywhere** — Publishing changes to a saved query updates it in all workbooks that use it. There is no "draft" mode.

# Verification checklist

Before submitting work with Equals:

- [ ] **Datasources are shared** — Verify all users who need to run queries have access to the underlying datasources (not just the workbook)
- [ ] **Queries are scheduled** — Confirm all queries that feed dashboards are scheduled to run before the dashboard send time
- [ ] **Views are synced** — If using Views, check that they appear in the Views list and have synced to the Equals Warehouse schema
- [ ] **Dashboard objects are live** — Verify charts, tables, and cells on the dashboard are linked to workbook objects (not static images)
- [ ] **Permissions are correct** — Test sharing by viewing the workbook as a different user; confirm they can see data and run queries if intended
- [ ] **Timezone is set** — Confirm workspace timezone matches your team's location (Settings > Workspace)
- [ ] **Distribution schedule is tested** — Perform a test send of any scheduled dashboard distribution before enabling production schedule
- [ ] **Formulas reference correct ranges** — Check that formulas reference the correct cells and will expand/contract with auto-expand enabled
- [ ] **No broken references** — Verify that deleted sheets, columns, or cells have not broken formulas or dashboard objects

# Resources

For comprehensive page-by-page navigation of all Equals documentation, see the [llms.txt file](https://docs.equals.com/llms.txt).

**Critical documentation pages:**
- [Getting Started Overview](https://docs.equals.com/docs/getting-started) — Core concepts (workspaces, datasources, workbooks, dashboards, views)
- [Data Syncing Overview](https://docs.equals.com/docs/data-syncing-overview) — Datasource types, Equals Warehouse, connection setup
- [Create and Edit Dashboards](https://docs.equals.com/docs/create-and-edit-dashboards) — Building dashboards with charts, tables, cells, AI summaries

---

> For additional documentation and navigation, see: https://docs.equals.com/llms.txt