---
description: Guides Cline on how to spawn and manage Cline CLI subagents
author: https://github.com/nickbaumann98
version: 2.0
category: "Cline Core"
tags: ["subagents", "cline cli", "multi-agent", "parallel execution"]
globs: ["*"]
---
# Cline CLI Subagent Rules

## Basic Spawning
```bash
# Simple execution
cline task "Your prompt here"

# With JSON output for parsing
cline --json task "Build feature"

# With verbose output for long tasks
cline --verbose task "Complex task"

# Run in specific directory
cline --cwd ./project task "Analyze codebase"
```

## Session Management
```bash
# List task history to get task ID
cline history

# Resume an existing task by ID
cline --taskId abc123

# Or use the task command with resume
cline task --taskId abc123
```

## Essential Options
```bash
# Run in plan mode
cline --plan task "Design architecture"

# Run in act mode
cline --act task "Implement feature"

# Auto-approve all actions (yolo mode)
cline --yolo task "Build complete app"

# Set timeout in seconds
cline --timeout 300 task "Long running task"

# Use specific model
cline --model claude-3-sonnet task "Task with specific model"

# Enable extended thinking
cline --thinking 2048 task "Complex reasoning task"

# Reasoning effort levels
cline --reasoning-effort high task "Difficult problem"

# Maximum consecutive mistakes before halting (yolo mode)
cline --yolo --max-consecutive-mistakes 3 task "Risky operation"

# Double-check completion for verification
cline --double-check-completion task "Critical task"
```

## Autonomous Subagents
```bash
# Fully autonomous - auto-approves all actions
cline --yolo task "Build complete app"

# Autonomous with timeout
cline --yolo --timeout 600 task "Large refactoring"

# Autonomous with model selection
cline --yolo --model claude-3-opus task "Complex implementation"

# Combine with reasoning for difficult tasks
cline --yolo --thinking 4096 --reasoning-effort xhigh task "Architect system"
```

## Monitoring & Error Handling
```bash
# Check success with JSON output
if RESULT=$(cline --json task "$prompt"); then
    echo "Success!"
    echo "$RESULT" | jq '.'
else
    echo "Failed with exit code $?"
fi

# Timeout for long tasks
timeout 300 cline --yolo task "$prompt" || echo "Timed out"

# Verbose mode for debugging
cline --verbose task "Debug this issue"
```

## Advanced Pattern: Parallel Agentic Workflows

This pattern, inspired by "Midjourney for Code," involves spawning multiple agents in isolated environments to generate diverse solutions to a single problem. This is useful for prototyping, A/B testing features, or exploring different implementations.

### Core Principle: Keep It Simple

The Cline CLI is well-designed for autonomous operation. Don't over-engineer with complex monitoring or JSON parsing unless you specifically need it.

### Pattern 1: Simple Parallel Variations

The most effective pattern for generating multiple variations:

```bash
#!/bin/bash
# spawn_variations.sh - Dead simple parallel execution

# Define your variations
for i in {1..5}; do
  mkdir -p "variant-$i"
  (
    cd "variant-$i" && \
    cline --yolo task "Build [YOUR TASK] - Variation $i: [specific approach/style]"
  ) &
done

wait
echo "✅ All variations complete. Check variant-*/ directories"
```

That's it. The Cline CLI handles progress display, error reporting, and retries automatically.

### Pattern 2: Parallel Features on Existing Codebase

For working on an existing project with git:

```bash
#!/bin/bash
# spawn_features.sh - Parallel feature development

FEATURE="implement dark mode"
BASE_BRANCH=$(git branch --show-current)

for approach in "css-variables" "theme-provider" "tailwind-classes"; do
  git checkout -b "feature-$approach" "$BASE_BRANCH"
  
  (
    cline --yolo task "Add $FEATURE using $approach approach"
  ) &
done

wait
echo "✅ Features ready on branches: feature-*"
```

### Pattern 3: Quick A/B Testing

For comparing different implementations:

```bash
#!/bin/bash
# ab_test.sh - Compare two approaches

mkdir -p option-a option-b

# Option A
(cd option-a && cline --yolo task "Build a todo app using vanilla JavaScript") &

# Option B  
(cd option-b && cline --yolo task "Build a todo app using React") &

wait
echo "✅ Both options ready for comparison"
```

### Pattern 4: Model Comparison

Compare different models on the same task:

```bash
#!/bin/bash
# model_comparison.sh - Compare model performance

TASK="Implement a REST API with authentication"

for model in "claude-3-opus" "claude-3-sonnet" "claude-3-haiku"; do
  mkdir -p "model-$model"
  (
    cd "model-$model" && \
    cline --yolo --model "$model" task "$TASK"
  ) &
done

wait
echo "✅ Model comparison complete. Check model-*/ directories"
```

### Pattern 5: Sequential Task Pipeline

For tasks that need to run in sequence:

```bash
#!/bin/bash
# pipeline.sh - Sequential task pipeline

# Task 1: Setup
cline --yolo task "Initialize project structure with TypeScript and ESLint"

# Task 2: Core implementation
cline --yolo task "Implement core business logic"

# Task 3: Testing
cline --yolo task "Write comprehensive tests"

# Task 4: Documentation
cline --yolo task "Generate API documentation"

echo "✅ Pipeline complete"
```

### When to Add Complexity

Only add monitoring/logging when you need:
- **Automated selection** - Parsing results to pick best variant
- **CI/CD integration** - Running in non-interactive environments  
- **Cost tracking** - Analyzing spend across many runs
- **Bulk operations** - Managing 10+ parallel agents

For interactive development, the simple patterns above are optimal.

### Monitoring (When Needed)

If you need to monitor progress:

```bash
# In another terminal
watch -n 2 'find . -name "*.ts" -o -name "*.js" | wc -l'

# Or check completion
ls -d variant-*/index.html 2>/dev/null | wc -l

# Check task history
cline history
```

### Cost Awareness

```bash
# Use faster/cheaper models for simple tasks
cline --yolo --model claude-3-haiku task "Simple refactoring"

# Use capable models for complex tasks
cline --yolo --model claude-3-opus task "Architect microservices"

# Timeout prevents runaway costs
cline --yolo --timeout 300 task "Bounded task"
```

## Context Preservation

### Fresh Context by Default

CLI subagents start with a **fresh context** - they do not inherit context from the parent Cline session. This is intentional and beneficial for:

- **Isolated experimentation** - No bias from previous decisions
- **Clean slate** - No context pollution from unrelated work
- **Parallel execution** - Each subagent operates independently

### Passing Context to CLI Subagents

When you need to share context with a CLI subagent, include it in the prompt:

```bash
# Without context (fresh start)
cline --yolo task "Implement user authentication"

# With context from current session
cline --yolo task "Implement user authentication for our Express.js API. We're using MongoDB, JWT tokens, and the User model is in models/User.js. Follow the error handling pattern in routes/auth.js."
```

### Context Preservation Pattern

For complex tasks requiring context handoff, combine with `new-task-automation.md`:

```bash
# Pattern: Spawn with embedded context
CONTEXT="Project: E-commerce API. Stack: Express.js, MongoDB, Redis. Current state: User auth complete, need product catalog. Key files: models/, routes/, middleware/auth.js"

cline --yolo task "Implement product catalog CRUD endpoints. $CONTEXT"
```

### Comparison with `new_task` Tool

| Feature | `new_task` Tool | CLI Subagents |
|---------|-----------------|---------------|
| **Context** | Full handoff with preserved state | Fresh context (explicit in prompt) |
| **Trigger** | Automatic at 45-50% context | Manual via `execute_command` |
| **Execution** | Sequential (linear) | Parallel (multiple processes) |
| **Use Case** | Continue same work | Parallel experiments, A/B testing |

**When to use which:**
- Use `new_task` (per `new-task-automation.md`) when you need to **continue** work with preserved context
- Use CLI subagents when you need **parallel execution** or **isolated experimentation**

## Integration with Cline VSCode Extension

The Cline CLI works seamlessly with the VSCode extension:

```bash
# Start a task in CLI, continue in VSCode
cline task "Start implementation"
# Note the task ID from output

# In VSCode, resume the task
# Use the task ID in the Cline extension
```

### Cross-Reference with Other Rules

- **`new-task-automation.md`**: Use for context preservation and sequential task handoff
- **`cline-continuous-improvement-protocol.md`**: Log learnings from subagent experiments
- **`eternal-memory-bank.md`**: Store insights from parallel experiments for future reference

## Configuration

```bash
# Show current configuration
cline config

# Authenticate a provider
cline auth

# Check for updates
cline update
```

## Key Tips
- **Keep it simple** - Basic bash patterns work best
- **Trust the tool** - Cline CLI handles errors, retries, and progress
- **Use `--yolo`** for fully autonomous operation
- **Set `--timeout`** to control maximum execution time
- **Use `--json`** for programmatic output parsing
- **Don't capture output you won't use** - Let the CLI display progress naturally
- **Combine with `--thinking`** for complex reasoning tasks