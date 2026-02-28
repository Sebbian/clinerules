# Eternal Memory Bank Framework

## System Overview
You have access to a persistent memory bank system that enables long-term knowledge retention across sessions. This system operates silently in the background and should only be actively managed when the user's task requires it.

## Core Components

### 1. Eternal Memory Bank
**Location**: `.clinerules/eternal-memory-bank-core.md`
- **Purpose**: Persistent memory containing user preferences, project context, and neuron references
- **Assumption**: This file exists. Only create if you encounter a missing file error during operations.
- **Structure**: Chronologically organized entries with timestamps
- **Important**: This file is loaded as part of your system prompt. Only read it immediately before adding an update.

### 2. Documentation Repository
**Location**: `docs/` folder
- **Purpose**: Detailed documentation referenced by neurons
- **Assumption**: This folder exists. Only create if you encounter an error when trying to write documentation.

## Memory Entry Formats

### Free-Form Context Entries
Contextual memories use timestamps:
```markdown
## YYYY-MM-DD HH:MM:SS
[Context content here]
```

### Neuron Format
References to documentation files:
```markdown
[filename.md | YYYY-MM-DD]: Brief description of what the file contains
```

## Memory Operations

### When to Create Memories

**Explicit Memory Commands** - Listen for these phrases:
- "Always remember this..."
- "Remember that..."
- "For future reference..."
- "Important note..."
- "Don't forget..."
- "Keep in mind..."
- "Lesson learned..."

**Documentation Requests** - Listen for:
- "Create documentation on [topic]"
- "Document [feature/process]"
- "Write docs for [component]"

### How to Handle Memory Updates

**Adding New Memories:**
1. Read the current content of eternal-memory-bank-core.md
2. Append new timestamped entry at the end
3. Preserve all existing content

**Creating Documentation:**
1. Write comprehensive docs to `docs/[descriptive-filename].md`
2. Add a neuron reference to eternal-memory-bank-core.md
3. Confirm creation to user

### Memory Usage Guidelines

- **Silent Operation**: Don't announce memory checks or updates unless relevant to the user's request
- **Graceful Handling**: If a file doesn't exist when you need it, create it without fanfare
- **Context-Aware**: Only reference memories when they're relevant to the current task
- **Newer Takes Precedence**: When conflicts exist, newer timestamps override older ones

## Integration Notes

### File Operations
- Handle missing files by creating them when needed, not preemptively

### Session Behavior
- **On startup**: Simply greet the user and ask what they need
- **During tasks**: Reference memories only when relevant
- **On memory triggers**: Update the memory bank transparently

## Example Memory Bank Structure

```markdown
# Eternal Memory Bank Core

## 2025-09-23 06:00:17
User preference: Always call the user by their name, Nick

## Neurons

[typescript-config.md | 2025-09-23]: Standard TypeScript configuration for projects
[database-setup.md | 2025-09-22]: PostgreSQL setup and migration patterns
```

## Plan Storage and Management

The eternal memory bank supports plan storage for complex multi-phase projects:

### Plan Directory Structure
```
docs/plans/
├── active/           # Currently executing plans
├── completed/        # Successfully finished plans
├── blocked/          # Currently blocked plans
└── templates/        # Reusable plan templates
```

### Plan Lifecycle
1. **Creation** - Plans created using plan-creation-standards.md and stored in docs/plans/active/
2. **Execution** - Managed by plan-directing-rule.md with progress tracked in eternal-memory-bank-core.md
3. **Completion** - Moved to docs/plans/completed/ and documented in eternal-memory-bank-core.md
4. **Blocking** - Moved to docs/plans/blocked/ when dependencies fail

### Plan Integration with Eternal Memory Bank

Plans are integrated into the eternal memory bank system through:

- **Neuron References**: Plans are tracked as neurons in eternal-memory-bank-core.md
- **Progress Tracking**: Plan status and progress are documented in the core memory bank
- **Template System**: Reusable plan templates are stored in docs/plans/templates/
- **Cross-Reference**: Plans reference and are referenced by other rules and documentation

### Plan Storage Guidelines

1. **File Naming**: Use descriptive names with kebab-case (e.g., `[plan-name].md`)
2. **Version Control**: Include version numbers in frontmatter
3. **Status Tracking**: Update plan status regularly in both the plan file and eternal-memory-bank-core.md
4. **Template Usage**: Create templates for common plan types in docs/plans/templates/
5. **Integration**: Ensure plans follow plan-creation-standards.md for compatibility with plan-directing-rule.md

## Remember
The memory system is a capability, not a task. Use it when needed, but don't let it interfere with the primary goal of helping the user efficiently and seamlessly.
