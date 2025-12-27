---
name: brainstorming
description: "You MUST use this before any creative work - creating features, building components, adding functionality, or modifying behavior. Explores user intent, requirements and design before implementation."
---

# Brainstorming Ideas Into Designs

## Overview

Help turn ideas into fully formed designs and specs through natural collaborative dialogue.

Start by understanding the current project context, then ask questions one at a time to refine the idea. Once you understand what you're building, present the design in small sections (200-300 words), checking after each section whether it looks right so far.

## The Process

**Understanding the idea:**
- Check out the current project state first (files, docs, recent commits)
- Ask questions one at a time to refine the idea
- Prefer multiple choice questions when possible, but open-ended is fine too
- Only one question per message - if a topic needs more exploration, break it into multiple questions
- Focus on understanding: purpose, constraints, success criteria

**Exploring approaches:**
- Propose 2-3 different approaches with trade-offs
- Present options conversationally with your recommendation and reasoning
- Lead with your recommended option and explain why

**Presenting the design:**
- Once you believe you understand what you're building, present the design
- Break it into sections of 200-300 words
- Ask after each section whether it looks right so far
- Cover: architecture, components, data flow, error handling, testing
- Be ready to go back and clarify if something doesn't make sense

## After the Design

**Documentation:**
- Write the validated design to `docs/plans/YYYY-MM-DD-<topic>-design.md`
- Use elements-of-style:writing-clearly-and-concisely skill if available
- Commit the design document to git

**Implementation (if continuing):**
- Ask: "Ready to set up for implementation?"
- Use superpowers:using-git-worktrees to create isolated workspace
- Use superpowers:writing-plans to create detailed implementation plan

## Key Principles

- **One question at a time** - Don't overwhelm with multiple questions
- **Multiple choice preferred** - Easier to answer than open-ended when possible
- **YAGNI ruthlessly** - Remove unnecessary features from all designs
- **Explore alternatives** - Always propose 2-3 approaches before settling
- **Incremental validation** - Present design in sections, validate each
- **Be flexible** - Go back and clarify when something doesn't make sense

## Investigation Workflow

**Before brainstorming ideas, get ground truth about current implementation.**

### Phase 1: Initial Investigation

Dispatch investigation subagent to get broad overview:

```
Task tool (general-purpose):
  description: "Investigate: [feature area] current implementation"
  prompt: [Use ./investigation-prompt.md template]
```

**Investigator checks:**
- Actual database schema (query `information_schema`, not just migrations)
- Model code (fillable, casts, relationships, enums)
- Controllers (entry points, validation, dispatched jobs)
- Jobs & Services (queue logic, state transitions)
- Frontend (TypeScript types, components, state)

**Investigator reports:**
- Current implementation summary
- Database schema tables
- Enum/status values in use
- Key files with line ranges
- Data flow step-by-step
- **Areas needing deeper investigation**

### Phase 2: Deep Investigation (If Needed)

If initial investigation flags unclear areas, dispatch specific investigation subagents in parallel.

### Phase 3: Brainstorming

**Now that you have ground truth, brainstorm solutions.**

Use investigation findings to:
- Identify real constraints (not assumed)
- Spot inconsistencies to fix
- Propose ideas that fit existing architecture
- Avoid solutions that conflict with actual implementation

**Template for initial investigation:**

Use prompt template: `./investigation-prompt.md`

