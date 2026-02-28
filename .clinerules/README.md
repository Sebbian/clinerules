# clinerules

A collection of Cline rules and configurations for AI-assisted development.

## About

This directory contains the `.clinerules` used across projects in this workspace. These rule files are automatically loaded by Cline when placed in a `.clinerules/` folder at the project root.

## Structure

```
vs/
├── clinerules/            # This directory (clone/sync to .clinerules/ in projects)
│   ├── *.md               # Individual rule files
│   └── memory-bank/       # Dynamic memory storage
├── adt/                   # Project using these rules
└── docs/                  # Additional documentation
```

## Active Rules

These rules are active for all projects in this workspace:

| Rule | Purpose |
|------|---------|
| `general-development-rules.md` | Core development principles, Git conventions, communication language |
| `testing-strategy.md` | Test generation, coverage guidelines, TDD workflow |
| `code-review.md` | Code review checklist, severity classification, feedback approach |
| `create-documentation.md` | Documentation approach, README generation, inline comments |
| `cline-architecture.md` | Cline extension architecture, state management, task execution |
| `cline-continuous-improvement-protocol.md` | Learning capture, knowledge consolidation, reflection process |
| `mcp-development-protocol.md` | MCP server development workflow, testing requirements |
| `mcp_env_configuration.md` | Environment variable management in MCP servers |
| `context7-auto-usage.md` | Automatic Context7 MCP invocation for library docs |
| `sequential-thinking.md` | Guide for using sequentialthinking MCP tool |
| `new-task-automation.md` | Task handoff strategy, context window management |
| `self-improving-cline.md` | Reflection on interactions, rule improvement suggestions |
| `eternal-memory-bank.md` | Persistent memory system framework |
| `eternal-memory-bank-core.md` | Core memory bank initialization |
| `helm-chart-developer.md` | Helm chart development protocol, security requirements |
| `uv-python-usage-guide.md` | UV Python package manager guide |
| `version-increment.md` | Intelligent versioning workflow, changelog management |
| `workflow-rules.md` | Standardized Cline workflow file structure |
| `mermaid-plans.md` | Mermaid diagram usage in plan mode |
| `claude-code-subagents.md` | Claude CLI subagent spawning patterns |
| `writing-effective-clinerules.md` | Guidelines for creating effective rules |
| `plan-directing-rule.md` | Cline Directing rules for complex multi-phase plans |
| `plan-creation-standards.md` | Plan creation standards for compatibility with directing rules |
| `exact-file-editing-workflow.md` | Exact file editing workflow for precise changes |
| `baby-steps.md` | Baby Steps methodology for incremental implementation |

## For New Projects

To use these rules in a new project:

**Option 1: Copy**
```bash
cp -r clinerules/.clinerules /path/to/new/project/
```

**Option 2: Symlink (recommended for shared updates)**
```bash
ln -s /path/to/this/clinerules /path/to/new/project/.clinerules
```

## Contributing

To contribute improvements to these rules:

1. Read [writing-effective-clinerules.md](writing-effective-clinerules.md)
2. Create a PR with your changes
3. Follow the rule structure and conventions documented

## License

MIT