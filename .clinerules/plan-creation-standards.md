# Plan Creation Standards for Cline Directing

**Version:** 1.0  
**Created:** 2026-02-12  
**Purpose:** Ensure all plans are compatible with Cline's Plan Directing Rule rules system

## Overview

This rule defines the mandatory structure and format for all plans created in the memory-bank to ensure seamless integration with Cline's Directing rules system.

## Mandatory Plan Structure

All plans must follow this standardized structure:

### 1. Plan Metadata Section (Required)

```markdown
## Plan Metadata

### Dependencies
- **Sequential:** [Phase 1 → Phase 2 → Phase 3 → Phase 4]
- **Parallel:** [Within Phase 1: Feature A, Feature B]
- **Prerequisites:** [List specific prerequisites]

### Resources
- **Team Size:** [Number of agents/teams]
- **Complexity:** [Low/Medium/High]
- **Risk Level:** [Low/Medium/High]

### Status Tracking
- **Overall Progress:** [0/XX tasks completed (XX%)]
- **Last Updated:** [YYYY-MM-DD]
- **Next Phase:** [Current phase name]
```

### 2. Feature Summary Table (Required)

```markdown
## Feature Summary

| # | Feature | Phase | Files Modified | Est. Tasks | Complexity |
|---|---------|-------|----------------|------------|------------|
| 1 | Feature Name | Phase 1 | `file1.py` | 4 | Low |
| 2 | Feature Name | Phase 2 | `file2.py` | 6 | Medium |
```

### 3. Task Breakdown by Feature (Required)

Each feature must have:
- **Purpose:** Clear description of what the feature accomplishes
- **Tasks:** Numbered list with complexity ratings
- **Dependencies:** Specific prerequisite tasks
- **Estimated Time:** Duration for the feature
- **Subagent/Team:** Which team handles this feature

### 4. Dependency Matrix (Required)

```markdown
## Dependency Matrix

### Critical Path Dependencies

```
Phase 1: [Phase Name] (Sequential)
├── Task 1.1 → Task 1.2 → Task 1.3 (Feature 1)
└── Task 1.4 → Task 1.5 → Task 1.6 (Feature 2)

Phase 2: [Phase Name] (Depends on Phase 1 completion)
├── Task 2.1 → Task 2.2 → Task 2.3 (Feature 3)
└── Task 2.4 → Task 2.5 → Task 2.6 (Feature 4)
```

### Parallel Execution Opportunities

**Within Phase 1:**
- Feature 1 and Feature 2 can run in parallel (after Task 1.1)

**Within Phase 2:**
- Feature 3 and Feature 4 can run in parallel
```

### 5. Team Organization (Required)

```markdown
## Team Organization

### Team 1: [Team Name]
**Complexity:** [Low to High]
**Tasks:** [List of task ranges]
**Dependencies:** [List dependencies]
**Execution Order:** [Sequential/Parallel description]
**Estimated Duration:** [Duration]
```

### 6. Execution Strategy (Required)

```markdown
## Execution Strategy

### Phase-Based Execution
1. **Phase 1:** [Description] (Estimated: XX minutes)
2. **Phase 2:** [Description] (Estimated: XX minutes)
3. **Phase 3:** [Description] (Estimated: XX minutes)
4. **Phase 4:** [Description] (Estimated: XX minutes)
```

### 7. Integration with Cline Plan Directing Rules (Required)

```markdown
## Integration with Cline Plan Directing Rules

### Agent Commands and Direction

**Phase 1: [Phase Name]**
```
cline task "pick up Phase 1 from [plan-name]"
```
- **Agent:** [Team name]
- **Execution:** [Sequential/Parallel description]
- **Features:** [List features]
- **Duration:** [Estimated duration]
- **Dependencies:** [List dependencies]

**Phase 2: [Phase Name]**
```
cline task "pick up Phase 2, Feature X and Feature Y from [plan-name]"
```
- **Agents:** [List parallel agents]
- **Execution:** [Execution strategy]
- **Features:** [List features]
- **Duration:** [Estimated duration]
- **Dependencies:** [List dependencies]
```

### File Conflict Management (Required)

```markdown
### File Conflict Management

**Critical File Dependencies:**
- `file1.py`: Modified in Phase 1 and 2 - **Phase 1 before Phase 2**
- `file2.py`: Modified only in Phase 3 - **Safe for Phase 3**
- `file3.py`: Modified in all phases - **Sequential execution required**
```

### 8. Progress Tracking Format (Required)

```markdown
## Progress Tracking Format

```
EXECUTION STATUS:

Phase 1: [Phase Name]
├── [ ] Feature 1: [Feature Name] (0/4 tasks complete)
├── [ ] Feature 2: [Feature Name] (0/6 tasks complete)
└── [ ] Agent 1: [Team Name] (0/10 tasks complete)

Phase 2: [Phase Name]
├── [ ] Feature 3: [Feature Name] (0/5 tasks complete)
└── [ ] Agent 2: [Team Name] (0/5 tasks complete)

OVERALL PROGRESS: 0/XX tasks complete (XX%)
NEXT ACTIONS: [Next steps]
```
```

### 9. Quick Start Commands (Required)

```markdown
## Quick Start Commands

**To execute the entire plan:**
```
cline task "pick up Phase 1 from [plan-name]"
# Wait for completion, then:
cline task "pick up Phase 2 from [plan-name]"
# Wait for completion, then:
cline task "pick up Phase 3 from [plan-name]"
# Wait for completion, then:
cline task "pick up Phase 4 from [plan-name]"
```

**Expected Total Duration:** [Total estimated time]
**Expected Completion:** [What will be accomplished]
```

## Plan Validation Checklist

Before saving any plan, ensure it includes:

- [ ] **Plan Metadata** section with dependencies and resources
- [ ] **Feature Summary** table with all required columns
- [ ] **Task Breakdown** for each feature with complexity ratings
- [ ] **Dependency Matrix** showing critical path and parallel opportunities
- [ ] **Team Organization** with clear team assignments
- [ ] **Execution Strategy** with phase-based breakdown
- [ ] **Integration with Cline Directing Rules** section
- [ ] **File Conflict Management** analysis
- [ ] **Progress Tracking Format** for real-time status
- [ ] **Quick Start Commands** for easy execution
- [ ] **Agent Commands** for each phase with proper Directing syntax
- [ ] **File Dependencies** clearly mapped to prevent conflicts

## Plan Naming Conventions

- Use descriptive names with kebab-case
- Include version numbers in frontmatter: `version: 1.0`
- Use clear, descriptive titles: `PyQt6 New Features - Enhanced Implementation Plan for Cline Directing`

## Plan File Locations

- **Active Plans:** `docs/plans/active/`
- **Completed Plans:** `docs/plans/completed/`
- **Blocked Plans:** `docs/plans/blocked/`
- **Plan Templates:** `docs/plans/templates/`

## Plan Quality Standards

### Complexity Assessment
- **Low:** Import statements, basic configuration, simple setup
- **Medium:** Feature implementation with moderate logic, integration with existing components
- **High:** Complex dependency management, performance optimization, multi-component integration

### Dependency Management
- **Sequential Dependencies:** Must be clearly marked and justified
- **Parallel Opportunities:** Identify where safe parallel execution is possible
- **File Conflicts:** Must be analyzed and resolved through proper sequencing

### Agent Coordination
- **Clear Team Assignments:** Each team must have well-defined responsibilities
- **Execution Order:** Specify sequential vs parallel execution clearly
- **Handoff Points:** Define clear transition points between phases

## Plan Review Process

1. **Structure Validation:** Verify all required sections are present
2. **Dependency Analysis:** Check for logical dependency chains
3. **File Conflict Review:** Ensure no file modification conflicts
4. **Agent Command Validation:** Verify all Cline commands are properly formatted
5. **Complexity Assessment:** Validate complexity ratings are accurate
6. **Execution Feasibility:** Ensure the plan can be realistically executed

## Integration with Existing Rules

This rule works in conjunction with:
- **plan-directing-rule.md:** Defines how Cline Directing rules work
- **writing-effective-clinerules.md:** General guidelines for creating rules
- **eternal-memory-bank.md:** Overall memory bank structure and organization

## Examples

### Good Example
See for a complete example of a plan following these standards.

### Bad Example
Plans missing any of the required sections, or with unclear dependencies, or without proper agent commands.

## Enforcement

- All new plans must follow this format
- Existing plans should be updated to comply with these standards
- Plans not following this format will not be compatible with Cline Directing rules
- Use the validation checklist before saving any plan

## Related Documents

- [plan-directing-rule.md](./plan-directing-rule.md) - Cline Directing rules
- [eternal-memory-bank.md](../eternal-memory-bank.md) - Memory bank structure
- [writing-effective-clinerules.md](./writing-effective-clinerules.md) - Rule creation guidelines