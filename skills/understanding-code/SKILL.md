---
name: understanding-code
description: Use when asking questions about how existing code works, investigating complex features with multiple flows, or needing ground truth before making changes - prevents assumptions by dispatching parallel investigation subagents
---

# Understanding Code

## Overview

**Dispatch parallel subagents to investigate code, one per question. Get ground truth, not assumptions.**

When understanding complex features, don't guess - investigate. Each question gets its own subagent that reads actual files, checks DB schema, and maps logic flows.

## When to Use

```dot
digraph when {
    "Questions about existing implementation?" [shape=diamond];
    "Simple single-file lookup?" [shape=diamond];
    "Use Read tool directly" [shape=box];
    "understanding-code" [shape=box];

    "Questions about existing implementation?" -> "Simple single-file lookup?" [label="yes"];
    "Questions about existing implementation?" -> "Use Read tool directly" [label="no"];
    "Simple single-file lookup?" -> "Use Read tool directly" [label="yes"];
    "Simple single-file lookup?" -> "understanding-code" [label="no - multiple files/flows"];
}
```

**Use when:**
- "How does X feature work?"
- "What's the current implementation of Y?"
- "Are these flows differentiated?"
- "How is status/state tracked?"
- Multiple related questions about same feature
- Need ground truth before planning changes

**Don't use for:**
- Single file lookup (use Read directly)
- Known simple implementations
- Writing new code (use brainstorming first)

## The Process

```dot
digraph process {
    "Collect questions" [shape=box];
    "Dispatch parallel investigation subagents" [shape=box];
    "Each subagent: Read files, check DB, map flow" [shape=box];
    "Synthesize findings into ground truth" [shape=box];
    "Present IS-state to user" [shape=box];

    "Collect questions" -> "Dispatch parallel investigation subagents";
    "Dispatch parallel investigation subagents" -> "Each subagent: Read files, check DB, map flow";
    "Each subagent: Read files, check DB, map flow" -> "Synthesize findings into ground truth";
    "Synthesize findings into ground truth" -> "Present IS-state to user";
}
```

## Red Flags

**Never:**
- Assume code works as documented
- Skip checking actual DB schema
- Read one file and extrapolate
- Answer questions without reading code
- Trust variable names over actual behavior

**If you catch yourself:**
- "I think it works like..." → STOP, dispatch subagent to check
- "Based on the migration..." → STOP, check actual schema
- "It probably uses..." → STOP, read the actual code
