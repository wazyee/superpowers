# Investigation Subagent Prompt Template

Use this template when dispatching investigation subagents during brainstorming to understand current implementation before proposing ideas.

**Context:** Investigation subagents are dispatched BEFORE brainstorming to get ground truth. Each subagent investigates a specific area or question.

## Initial Investigation (Broad Overview)

```
Task tool (general-purpose):
  description: "Investigate: [feature area] current implementation"
  prompt: |
    **ULTRATHINK before starting investigation.**

    You are investigating the current implementation to provide ground truth for brainstorming.

    ## Feature Area

    [FEATURE_AREA]

    ## Your Job

    Get a complete picture of what exists. Don't assume - verify.

    ### 1. Database Schema (ACTUAL, not migrations)

    Query the actual database:
    ```bash
    # PostgreSQL
    php artisan tinker --execute="DB::select(\"SELECT column_name, data_type, is_nullable FROM information_schema.columns WHERE table_name = 'TABLE_NAME' ORDER BY ordinal_position\")"

    # Check enum values
    php artisan tinker --execute="DB::select(\"SELECT enum_range(NULL::enum_name)\")"
    ```

    ### 2. Models

    For each relevant model, check:
    - \`$fillable\` / \`$guarded\`
    - \`$casts\` (especially enums)
    - Relationships
    - Scopes
    - Accessors/Mutators

    ### 3. Controllers

    Find entry points:
    - Routes that handle this feature
    - Request validation rules
    - What jobs/services are dispatched

    ### 4. Jobs & Services

    - Queue configuration
    - State transitions
    - Error handling
    - External API calls

    ### 5. Frontend

    - TypeScript types/enums
    - Components that display this data
    - Forms that modify this data
    - State management

    ## Output Format

    ### Current Implementation Summary

    [2-3 paragraph overview of how it works NOW]

    ### Database Schema

    | Table | Column | Type | Notes |
    |-------|--------|------|-------|
    | ... | ... | ... | ... |

    ### Enums/Status Values

    | Enum | Values | Used In |
    |------|--------|---------|
    | ... | ... | ... |

    ### Key Files

    | Purpose | File | Lines |
    |---------|------|-------|
    | ... | ... | ... |

    ### Data Flow

    [Step-by-step flow from user action to completion]

    ### Potential Issues

    [Any inconsistencies, dead code, enum mismatches, etc.]

    ### Areas Needing Deeper Investigation

    **Flag specific areas that are unclear or complex:**
    1. [Area 1] - Why it needs investigation
    2. [Area 2] - Why it needs investigation
    3. ...

    Be thorough. Read every relevant file. Query actual database. Don't assume.
```

## Deep Investigation (Specific Question)

Use this when the initial investigation flags areas needing deeper understanding:

```
Task tool (general-purpose):
  description: "Investigate: [specific question]"
  prompt: |
    **ULTRATHINK before starting investigation.**

    You are investigating a specific question about the codebase.

    ## Question

    [THE_QUESTION]

    ## Context

    [What we already know from initial investigation]

    ## Your Job

    1. Find ALL related files (controllers, jobs, services, models, migrations)
    2. Check actual DB schema - don't trust migrations
    3. Read the actual code - don't assume
    4. Map the logic flow step-by-step
    5. Note any inconsistencies between code, schema, and expected behavior

    ## Output Format

    ### Files Involved

    [List with file:line references]

    ### DB Schema (actual)

    [Column names and types from database query]

    ### Logic Flow

    [Step-by-step flow with file:line references]

    1. [file.php:123] - What happens
    2. [file.php:456] - What happens next
    3. ...

    ### Status/State Tracking

    [What columns/caches track state, what values mean what]

    ### Issues Found

    [Any inconsistencies, dead code, or problems]

    ### Answer to Question

    [Direct answer based on investigation]

    Be thorough. Read every file. Don't assume.
```

## Multiple Questions (Parallel Investigation)

When multiple distinct questions need answers, dispatch one subagent per question in parallel:

```
Task tool (general-purpose) x N:
  description: "Investigate: [question 1]"
  description: "Investigate: [question 2]"
  description: "Investigate: [question N]"
  ...
```

Each subagent uses the Deep Investigation template above.

## What to Check by Layer

| Layer | What to Check | How |
|-------|---------------|-----|
| Database | Schema, constraints, indexes | \`information_schema\` query |
| Migrations | What SHOULD exist | Read migration files |
| Models | Fillable, casts, relationships | Read model files |
| Controllers | Entry points, validation | Read + check routes |
| Jobs | Queue logic, state transitions | Read job files |
| Services | Business logic | Read service files |
| Frontend Types | TypeScript enums/interfaces | Read types files |
| Frontend Components | UI state handling | Read component files |

## Red Flags to Report

Always flag these in your output:

- **Enum mismatch**: DB enum differs from code enum
- **Dead code**: Values defined but never used
- **Missing status**: Code sets value not in enum
- **Unscoped queries**: \`::find()\` without organization filter
- **Cache/DB mismatch**: Cache TTL vs job timeout
- **Frontend/Backend mismatch**: TypeScript types missing values
