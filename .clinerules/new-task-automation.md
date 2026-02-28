---
description: Workflow for starting new task when context window reaches 50%
author: https://github.com/cline
version: 1.0
category: "Cline Core"
tags: ["context-management", "new-task"]
globs: ["*"]
---
# You MUST use the `new_task` tool: Task Handoff Strategy Guide

**⚠️ CRITICAL INSTRUCTIONS - YOU MUST FOLLOW THESE GUIDELINES ⚠️**

This guide provides **MANDATORY** instructions for effectively breaking down complex tasks and implementing a smooth handoff process between tasks. You **MUST** follow these guidelines to ensure continuity, context preservation, and efficient task completion.

## ⚠️ CONTEXT WINDOW MONITORING - MANDATORY ACTION REQUIRED ⚠️

You **MUST** monitor the context window usage displayed in the environment details. 

**Proactive Handoff Threshold: 45%** - When usage exceeds 45%, you **MUST** proactively initiate a task handoff using the `new_task` tool.

**Hard Limit: 50%** - The 50% mark is the absolute maximum that must never be exceeded. If you reach 50%, you **MUST** immediately stop and initiate handoff.

Example of context window usage approaching the 45% threshold with a 200K context window:

\`\`\`text
# Context Window Usage
92,000 / 200,000 tokens (46%)
Model: anthropic/claude-3.7-sonnet (200K context window)
Input: 14% | Output: 32% | Total: 46%
\`\`\`

**IMPORTANT**: When you see context window usage at or above 45%, you MUST:
1. Complete your current logical step
2. Use the `ask_followup_question` tool to offer creating a new task
3. If approved, use the `new_task` tool with comprehensive handoff instructions

**Token Budget Guidelines:**
- **Input Limit**: Maximum 15% of total context window
- **Output Limit**: Maximum 35% of total context window  
- **Total Limit**: Absolute maximum 50% of total context window (hard stop)
- **Proactive Handoff**: Automatic task handoff at 45% total usage

## Context Management Principles

- **[priority=critical, scope=universal, trigger=session-start]** :: !!! Context Window Management REQUIRED !!! :: You MUST monitor context usage throughout the session. Before reaching the 45% context window threshold, apply selective loading, progressive context building, and context isolation to minimize context consumption. Load high-level architecture first, then add specific implementation details on-demand. The task handoff workflow is mandatory when these optimization efforts are insufficient.

- **[priority=high, scope=universal, trigger=context-threshold]** :: !!! Progressive Context Building REQUIRED !!! :: Start with minimal necessary context and progressively add detail only as needed. This prevents context overload and maintains focus. The relationship between context management and task handoff is sequential: (1) You MUST apply optimization strategies throughout the session; (2) You MUST monitor context usage continuously; (3) When usage exceeds 45% despite optimization, you MUST initiate the task handoff process.

- **[priority=high, scope=universal, trigger=development-workflow]** :: !!! Follow Core Development Principles !!! :: All task handoffs MUST maintain the core principles defined in `general-development-rules.md`, including progressive implementation ("start simple, then grow"), modular testing, and clear communication. The handoff context MUST reference these principles and ensure the next session continues following them.

- **[priority=high, scope=universal, trigger=plan-directing]** :: !!! Plan Directing Integration !!! :: When plans are involved, task handoffs MUST reference the plan-directing-rule.md and plan-creation-standards.md. The handoff context MUST include plan status, next phase information, and any plan-related dependencies that need to be considered in the next session.

## Integration with Plan Orchestration

When creating new tasks that involve complex multi-phase work, use the `new_task` tool with proper orchestration structure including:

- **Agent Commands:** Include specific Cline agent commands for each phase (e.g., `cline task "pick up Phase 1 from plan-name"`)
- **File Conflict Management:** Analyze and document file dependencies to prevent conflicts between phases
- **Parallel Execution Opportunities:** Identify tasks that can run concurrently within phases
- **Progress Tracking Format:** Use standardized progress tracking format for real-time status updates
- **Team Organization:** Define clear team assignments and execution order

**Example Integration:**
```xml
<new_task>
<context>
# Task Continuation: Implement User Authentication System

## Completed Work
- Created basic Express.js server structure
- Implemented MongoDB connection and user schema
- Completed user registration endpoint with password hashing

## Current State
- Server runs successfully on port 3000
- MongoDB connection is established
- Registration endpoint (/api/users/register) is fully functional

## Next Steps
1. Implement login endpoint (/api/users/login)
2. Create authentication middleware
3. Add protected routes that require authentication
4. Implement password reset functionality

## Integration with Cline Orchestrator Rules
**Phase 1: Core Infrastructure**
```
cline task "pick up Phase 1 from user-auth-plan"
```
- **Agent:** Team 1 (Core Infrastructure)
- **Execution:** Sequential execution of all Phase 1 features
- **Dependencies:** None (Foundation phase)

**Phase 2: User Experience**
```
cline task "pick up Phase 2 from user-auth-plan"
```
- **Agent:** Team 2 (UX Enhancement)
- **Execution:** Sequential execution
- **Dependencies:** Phase 1 completion required

### File Conflict Management
**Critical File Dependencies:**
- `routes/auth.js`: Modified in all phases - **Sequential execution required**
- `models/User.js`: Modified only in Phase 1 - **Safe for early execution**
- `middleware/auth.js`: Modified in Phase 2 - **Phase 1 before Phase 2**

### Progress Tracking Format
```
EXECUTION STATUS:

Phase 1: Core Infrastructure
├── [ ] Feature 1: User Schema (0/4 tasks complete)
├── [ ] Feature 2: Registration Endpoint (0/6 tasks complete)
└── [ ] Agent 1: Core Infrastructure (0/10 tasks complete)

Phase 2: User Experience
├── [ ] Feature 3: Login Endpoint (0/5 tasks complete)
├── [ ] Feature 4: Authentication Middleware (0/5 tasks complete)
└── [ ] Agent 2: UX Enhancement (0/10 tasks complete)

OVERALL PROGRESS: 0/20 tasks complete (0%)
NEXT ACTIONS: Complete Phase 1 features
```
</context>
</new_task>
```

## Implementation Methodology

Each subtask identified during planning should be implemented incrementally:

- **Task Planning**: Identifies WHAT needs to be done (subtasks, dependencies, priorities)
- **Implementation**: Defines HOW each subtask is executed (incrementally, with validation)

**Workflow**:

Large Task → Plan Mode (break into subtasks) → Act Mode (implement each subtask)

Always apply incremental implementation principles when executing planned subtasks: one meaningful change at a time, validate after each step, document progress.

## Task Breakdown in Plan Mode - REQUIRED PROCESS

Plan Mode is specifically designed for analyzing complex tasks and breaking them into manageable subtasks. When in Plan Mode, you **MUST**:

### 1. Initial Task Analysis - REQUIRED

- **MUST** begin by thoroughly understanding the full scope of the user's request
- **MUST** identify all major components and dependencies of the task
- **MUST** consider potential challenges, edge cases, and prerequisites

### 2. Strategic Task Decomposition - REQUIRED

- **MUST** break the overall task into logical, discrete subtasks
- **MUST** prioritize subtasks based on dependencies (what must be completed first)
- **MUST** aim for subtasks that can be completed within a single session (15-30 minutes of work)
- **MUST** consider natural breaking points where context switching makes sense

### 3. Creating a Task Roadmap - REQUIRED

- **MUST** present a clear, numbered list of subtasks to the user
- **MUST** explain dependencies between subtasks
- **MUST** provide time estimates for each subtask when possible
- **MUST** use Mermaid diagrams to visualize task flow and dependencies when helpful

\`\`\`mermaid
graph TD
    A[Main Task] --> B[Subtask 1: Setup]
    A --> C[Subtask 2: Core Implementation]
    A --> D[Subtask 3: Testing]
    A --> E[Subtask 4: Documentation]
    B --> C
    C --> D
\`\`\`

### 4. Getting User Approval - REQUIRED

- **MUST** ask for user feedback on the proposed task breakdown
- **MUST** adjust the plan based on user priorities or additional requirements
- **MUST** confirm which subtask to begin with
- **MUST** request the user to toggle to Act Mode when ready to implement
- **MUST** ask for user feedback when context window size reaches 45% to finish step in task or Initiating the Handoff Process
- **MUST** ask for user feedback when context window size above 45% to pick up next step in task or Initiating the Handoff Process

## Task Implementation and Handoff Process - MANDATORY PROCEDURES

When implementing tasks in Act Mode, you **MUST** follow these guidelines for effective task handoff:

### 1. Focused Implementation - REQUIRED

- **MUST** focus on completing the current subtask fully
- **MUST** document progress clearly through comments and commit messages
- **MUST** create checkpoints at logical completion points

### 2. Recognizing Completion Points - CRITICAL

You **MUST** identify natural handoff points when:
- The current subtask is fully completed
- You've reached a logical stopping point in a larger subtask
- The implementation is taking longer than expected and can be continued later
- The task scope has expanded beyond the original plan
- **CRITICAL**: The context window usage exceeds 45% (e.g., 90,000+ tokens for a 200K context window) or reaches the 50% hard limit

### 3. Initiating the Handoff Process - MANDATORY ACTION

When you've reached a completion point, you **MUST**:

1. Summarize what has been accomplished so far
2. Clearly state what remains to be done
3. **MANDATORY**: Use the `ask_followup_question` tool to offer creating a new task:

\`\`\`xml
<ask_followup_question>
<question>I've completed [specific accomplishment]. Would you like me to create a new task to continue with [remaining work]?</question>
<options>["Yes, create a new task", "No, continue in this session", "Let me think about it"]</options>
</ask_followup_question>
\`\`\`

### 4. Creating a New Task with Context - REQUIRED ACTION

If the user agrees to create a new task, you **MUST** use the `new_task` tool with comprehensive handoff instructions:

\`\`\`xml
<new_task>
<context>
# Task Continuation: [Brief Task Title]

## Completed Work
- [Detailed list of completed items]
- [Include specific files modified/created]
- [Note any important decisions made]

## Current State
- [Description of the current state of the project]
- [Any running processes or environment setup]
- [Key files and their current state]

## Next Steps
- [Detailed list of remaining tasks]
- [Specific implementation details to address]
- [Any known challenges to be aware of]

## Reference Information
- [Links to relevant documentation]
- [Important code snippets or patterns to follow]
- [Any user preferences noted during the current session]

Please continue the implementation by [specific next action].
</context>
</new_task>
\`\`\`

### 5. Detailed Context Transfer - MANDATORY COMPONENTS

When creating a new task, you **MUST** always include:

#### Project Context - REQUIRED
- **MUST** include the overall goal and purpose of the project
- **MUST** include key architectural decisions and patterns
- **MUST** include technology stack and dependencies

#### Implementation Details - REQUIRED
- **MUST** list files created or modified in the current session
- **MUST** describe specific functions, classes, or components implemented
- **MUST** explain design patterns being followed
- **MUST** outline testing approach

#### Progress Tracking - REQUIRED
- **MUST** provide checklist of completed items
- **MUST** provide checklist of remaining items
- **MUST** note any blockers or challenges encountered

#### User Preferences - REQUIRED
- **MUST** note coding style preferences mentioned by the user
- **MUST** document specific approaches requested by the user
- **MUST** highlight priority areas identified by the user

## Best Practices for Effective Handoffs - MANDATORY GUIDELINES

### 1. Maintain Continuity - REQUIRED

- **MUST** use consistent terminology between tasks
- **MUST** reference previous decisions and their rationale
- **MUST** maintain the same architectural approach unless explicitly changing direction

### 2. Preserve Context - REQUIRED

- **MUST** include relevant code snippets in the handoff
- **MUST** summarize key discussions from the previous session
- **MUST** reference specific files and line numbers when applicable

### 3. Set Clear Next Actions - REQUIRED

- **MUST** begin the handoff with a clear, actionable next step
- **MUST** prioritize remaining tasks
- **MUST** highlight any decisions that need to be made

### 4. Document Assumptions - REQUIRED

- **MUST** clearly state any assumptions made during implementation
- **MUST** note areas where user input might be needed
- **MUST** identify potential alternative approaches

### 5. Optimize for Resumability - REQUIRED

- **MUST** structure the handoff so the next session can begin working immediately
- **MUST** include setup instructions if environment configuration is needed
- **MUST** provide a quick summary at the top for rapid context restoration

## Example Task Handoff

### Example #1 of an effective task handoff:

\`\`\`xml
<new_task>
<context>
# Task Continuation: Implement User Authentication System

## Completed Work
- Created basic Express.js server structure
- Implemented MongoDB connection and user schema
- Completed user registration endpoint with password hashing
- Added input validation using Joi
- Created initial test suite for registration endpoint

## Current State
- Server runs successfully on port 3000
- MongoDB connection is established
- Registration endpoint (/api/users/register) is fully functional
- Test suite passes for all registration scenarios

## Next Steps
1. Implement login endpoint (/api/users/login)
   - Use bcrypt to compare passwords
   - Generate JWT token upon successful login
   - Add proper error handling for invalid credentials
2. Create authentication middleware
   - Verify JWT tokens
   - Extract user information
   - Handle expired tokens
3. Add protected routes that require authentication
4. Implement password reset functionality

## Reference Information
- JWT secret should be stored in .env file
- Follow the existing error handling pattern in routes/users.js
- User schema is defined in models/User.js
- Test patterns are established in tests/auth.test.js

Please continue by implementing the login endpoint following the same patterns established in the registration endpoint.
</context>
</new_task>
\`\`\`

### Example #2 of an ineffective task handoff:

An ineffective handoff lacks critical details needed for the next session to continue work. For example, it might describe what was done without explaining the current project state, omit specific files that were modified, or fail to provide clear next steps. Without these details, the next session must spend valuable context window space rediscovering information that should have been preserved.

## When to Use Task Handoffs - MANDATORY TRIGGERS

You **MUST** initiate task handoffs in these scenarios:

1. **CRITICAL**: When context window usage exceeds 45% (e.g., 90,000+ tokens for a 200K context window) or reaches the 50% hard limit
2. **Long-running projects** that exceed a single session
3. **Complex implementations** with multiple distinct phases
4. **When context window limitations** are approaching
5. **When switching focus areas** within a larger project
6. **When different expertise** might be beneficial for different parts of the task

**⚠️ FINAL REMINDER - CRITICAL INSTRUCTION ⚠️**

You **MUST** monitor the context window usage in the environment details section. When it exceeds 45% (e.g., "92,000 / 200,000 tokens (46%)"), you **MUST** proactively initiate the task handoff process using the `ask_followup_question` tool followed by the `new_task` tool. You MUST use the `new_task` tool. The 50% mark is the absolute hard limit that must never be exceeded.

By strictly following these guidelines, you'll ensure smooth transitions between tasks, maintain project momentum, and provide the best possible experience for users working on complex, multi-session projects.

## User Interaction & Workflow Considerations

*   **Linear Flow:** Currently, using `new_task` creates a linear sequence. The old task ends, and the new one begins. The old task history remains accessible for backtracking.
*   **User Approval:** You always have control, approving the handoff and having the chance to modify the context Cline proposes to carry forward.
*   **Flexibility:** The core `new_task` tool is a flexible building block. Experiment with `.clinerules` to create workflows that best suit your needs, whether for strict context management, task decomposition, or other creative uses.