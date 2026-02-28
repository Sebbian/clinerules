# Plan Directing Rule for Cline

**Version:** 1.0  
**Created:** February 11, 2026  
**Purpose:** Enable Cline to act as a director for complex multi-phase plans with dependency management, parallel execution, and context optimization.

## Overview

This rule defines how Cline should handle plan directing, including task pickup, dependency validation, automatic decomposition, and progress tracking. When this rule is active, Cline acts as the director who proactively directs plan implementation by issuing commands to the user for execution.

## Plan Directing vs. Orchestration

**Plan Directing** is the process where Cline (the director) analyzes user-created plans and proactively issues `cline task "pick up..."` commands to the user at appropriate timing based on dependencies, rather than requiring the user to manually send commands to Cline. This eliminates ambiguity about when to start plan execution.

**Key Differences:**
- **Orchestration**: User manually picks up tasks using `cline task "pick up..."` commands
- **Directing**: Cline (director) analyzes plans and issues commands to user for execution
- **Directing** requires explicit user appointment of Cline as director

## User Appointment of Cline as Director

The user MUST explicitly appoint Cline as the director for plan implementation. This appointment is **not automatic** and requires a clear user request.

#### Appointment Command Format
The user appoints Cline as director by using one of these explicit commands:

```
"cline, please direct the implementation of [plan-name]"
"cline, please direct the implementation of the plan in [file-path]"
"cline, please act as director for [plan-name]"
```

#### Appointment Process
1. **User Request** - User explicitly requests Cline to direct plan implementation
2. **Plan Validation** - Cline validates the plan exists and is complete
3. **Director Activation** - Cline accepts the appointment and activates director mode
4. **Execution Commencement** - Cline begins issuing commands to the user for execution

#### Example Appointment Flow
```
User: "cline, please direct the implementation of [plan-name]"
Cline: "Appointing Cline as director for [plan-name]
Validating plan completeness...
Plan validated. Starting plan execution.
Phase 1: Foundation Setup
Command: cline task "pick up Phase 1 from [plan-name]""
```

## Rule Activation

This rule is activated when:
- User explicitly requests Cline to direct plan implementation using appointment commands
- User mentions plan execution or directing with explicit appointment
- Context contains plan-related files or discussions with explicit director appointment
- A complete plan is created and saved to eternal memory bank with explicit user appointment

## Core Capabilities

### 1. Plan Recognition and Parsing

Cline MUST recognize and parse plans that follow the standardized format:

```markdown
# [Plan Name] - Orchestration Format
**Version:** 1.0
**Status:** [Planning | In Progress | Completed | Blocked]
```

**Required Plan Sections:**
- Plan Metadata (dependencies, resources, status)
- Phase Structure (with status indicators)
- Task Structure (with prerequisites and validation criteria)

**Plan File Locations:**
- `docs/plans/active/` - Currently executing plans
- `docs/plans/completed/` - Finished plans
- `docs/plans/blocked/` - Blocked plans
- `docs/plans/templates/` - Reusable templates

### 2. Automatic Plan Execution Trigger

Cline MUST automatically initiate plan directing when:
- A complete plan is created and saved to the eternal memory bank
- The plan includes all required sections (phases, tasks, dependencies, Cline commands)
- The plan is marked as ready for execution
- User has explicitly appointed Cline as director

**Automatic Directing Process:**
1. **Validate Completeness** - Check all required sections are present
2. **Issue First Command** - Automatically issue the first phase command to the user
3. **Wait for Completion** - Monitor for task completion
4. **Issue Next Command** - Automatically issue the next phase command when ready
5. **Continue Sequentially** - Proceed through all phases automatically

**Command Format:**
```
"Starting plan execution: [plan-name]

Phase 1: [Phase Name]
Command: cline task "pick up Phase 1 from [plan-name]""
```

### 3. Task Pickup System

#### Command Recognition
Cline MUST recognize and process these command formats:
```bash
cline task "pick up [Phase X, Task Y] from [Plan Name]"
cline task "pick up [Task Name] from [Plan Name]"
cline task "pick up [Phase Name] from [Plan Name]"
```

#### Task Pickup Process
1. **Parse Command** - Extract phase, task, and plan name
2. **Validate Dependencies** - Check if prerequisites are met
3. **Load Context** - Load relevant plan sections and dependencies
4. **Initialize Agent** - Start with specific task context
5. **Execute Task** - Run through task steps using Baby Steps™
6. **Update Status** - Mark completion and update plan status
7. **Report Results** - Return results to main context

#### Example Command Processing
```bash
cline task "pick up Phase 3, Task 2 from [plan-name]"
```

**Cline Response:**
```
Starting Phase 3, Task 2: "Refactor StrategyPanelWidget"

Prerequisites Check:
✅ Phase 1: Foundation Setup (Completed)
✅ Phase 2: Layout Manager Implementation (Completed)
✅ Task 3.1: Refactor TitleBarWidget (Completed)
✅ Task 3.2: Refactor StrategyPanelWidget (Ready)

Loading Context:
- Plan: [plan-name]
- Phase: 3 (Widget Internal Layout Refactoring)
- Task: 3.2 (Refactor StrategyPanelWidget)
- Dependencies: Task 3.1 complete
- Estimated Duration: 1 day

Initializing Agent with Task Context...
```

### 4. Dependency Validation System

#### Sequential Dependencies
Cline MUST validate that all sequential dependencies are complete before starting a task:

```markdown
**Dependencies:** 
- Phase 1 → Phase 2 → Phase 3 (Sequential chain)
- Task 2.1 → Task 2.2 (Task-level sequential)
```

**Validation Process:**
1. Check phase-level dependencies
2. Check task-level dependencies
3. Verify prerequisite completion status
4. Block execution if dependencies incomplete
5. Suggest alternative available tasks

#### Parallel Dependencies
Cline MUST identify tasks that can run concurrently:

```markdown
**Parallel With:**
- Phase 4, Phase 5 (Phase-level parallel)
- Task 3.1, Task 3.2 (Task-level parallel)
```

**Parallel Execution Rules:**
1. Validate no shared resource conflicts
2. Ensure independent execution paths
3. Coordinate progress updates
4. Handle completion synchronization

#### Prerequisites Validation
Cline MUST check specific prerequisites before task execution:

```markdown
**Prerequisites:**
- Phase 1 complete
- Task 1.3 validated
- [Specific requirement]
```

**Prerequisites Check Process:**
1. Parse prerequisite list
2. Verify each prerequisite status
3. Report missing prerequisites
4. Suggest completion order

### 4. Automatic Plan Decomposition

#### Context Overflow Detection
Cline MUST monitor context usage and trigger decomposition when:
- Context usage exceeds 45% of available tokens
- Task complexity is marked as "Critical"
- Estimated duration exceeds 3 days
- Dependencies become too complex

#### Decomposition Process
1. **Analyze Task** - Identify sub-components that can be separated
2. **Create Sub-Tasks** - Break down into smaller, implementable steps
3. **Define Dependencies** - Establish clear prerequisite relationships
4. **Update Plan** - Mark original task as incomplete and add sub-tasks
5. **Notify User** - Inform about plan decomposition

#### Example Decomposition
```markdown
Original Task 3.2: "Implement widget layout manager" (Blocked - Too Complex)

Decomposed into:
- Task 3.2.1: "Create base widget layout interface" (New)
- Task 3.2.2: "Implement widget layout manager class" (New)  
- Task 3.2.3: "Add widget layout validation system" (New)
- Task 3.2: "Implement widget layout manager" (Incomplete - Requires sub-tasks)
```

### 5. Progress Tracking and Status Management

#### Status Indicators
Cline MUST use these status indicators:

**Phase Status:**
- **⬜ Not Started** - Task not yet begun
- **⏳ In Progress** - Currently being worked on
- **✅ Completed** - Task finished successfully
- **🔴 Blocked** - Cannot proceed due to dependencies or issues

**Task Status:**
- **[ ] Not Started** - Task not yet begun
- **[→] In Progress** - Currently being worked on
- **[x] Completed** - Task finished successfully
- **[!] Blocked** - Cannot proceed due to dependencies or issues

#### Progress Updates
Cline MUST update progress after each task completion:

```markdown
**Overall Progress:** 15/45 tasks completed (33%)
**Phase Progress:** Phase 2: 8/12 tasks completed (67%)
**Next Action:** Task 2.9 - Implement widget layout manager
```

#### Completion Validation
Before marking a task as complete, Cline MUST:
1. **Check Validation Criteria** - Verify all criteria are met
2. **Test Deliverables** - Validate outputs if applicable
3. **Update Dependencies** - Mark dependent tasks as unblocked
4. **Update Progress** - Increment overall progress counters
5. **Generate Report** - Create completion summary

### 6. Error Handling and Recovery

#### Task Failure Detection and Response
When a task fails, hangs, or gets stuck, Cline MUST:

```markdown
**Status:** [!] Blocked
**Reason:** [Timeout | Loop | Failed | Context Overflow]
**Failed Task:** Task 2.3: "Implement strategy manager"
**Blocked Dependencies:** Task 2.4 (depends on Task 2.3)
```

**Primary Recovery Strategy - Automatic Decomposition:**
1. **Immediate Decomposition** - Automatically break failed task into smaller, atomic steps
2. **Dependency Preservation** - Maintain all pre-defined dependencies from planning phase
3. **Sequential Sub-Task Execution** - Execute sub-tasks in dependency order
4. **Iterative Decomposition** - If sub-tasks still fail, decompose further
5. **User Escalation** - Only escalate to user after multiple decomposition attempts fail

**Example Recovery Flow:**
```
Task 2.3: "Implement strategy manager" - Failed (too complex)

Director Command: "Decompose Task 2.3 into smaller steps"
System Response: Creates sub-tasks:
- 2.3.1: Create base strategy manager class
- 2.3.2: Implement core strategy lifecycle
- 2.3.3: Add strategy registry integration

Execution Order (following pre-defined dependencies):
- cline task "pick up Phase 2, Task 2.3.1 from [plan-name]" ✅
- cline task "pick up Phase 2, Task 2.3.2 from [plan-name]" ✅
- cline task "pick up Phase 2, Task 2.3.3 from [plan-name]" ✅

Task 2.3: [x] Completed (all sub-tasks done)
Task 2.4: [ ] Unblocked (dependency satisfied)
```

#### Dependency-Aware Execution
**Dependencies are pre-defined during planning and MUST be strictly followed:**

1. **Automatic Blocking** - Tasks dependent on failed tasks are automatically blocked
2. **Sequential Resolution** - Complete all sub-tasks of failed task before unblocking dependents
3. **No Dependency Analysis** - Director does NOT analyze dependencies - uses pre-computed dependency chain from planning
4. **Execution Order** - Follow dependency chain exactly as defined in plan

**Example Dependency Handling:**
```
Plan Dependencies:
- Task 2.3 → Task 2.4 (Task 2.4 depends on Task 2.3 completion)

Task 2.3 fails → Task 2.4 automatically blocked
Task 2.3 decomposed and completed → Task 2.4 automatically unblocked
Director proceeds: cline task "pick up Phase 2, Task 2.4 from [plan-name]"
```

#### Context Overflow Handling
When context limits are exceeded:

1. **Detect Overflow** - Monitor token usage during execution
2. **Immediate Decomposition** - Automatically break task into smaller steps
3. **Context Preservation** - Save current progress before decomposition
4. **Sequential Execution** - Execute sub-tasks within context limits
5. **Resume Strategy** - Continue from saved progress point

#### Task Timeout and Loop Detection
When tasks hang or loop:

1. **Timeout Detection** - Monitor execution time against expected duration
2. **Loop Detection** - Identify repetitive patterns without progress
3. **Automatic Termination** - Force task termination when timeout/loop detected
4. **Decomposition Strategy** - Break task into smaller steps with different approach
5. **Progress Preservation** - Save any partial progress achieved

#### User Escalation Protocol
**Only escalate to user when automatic decomposition fails:**

1. **Multiple Decomposition Attempts** - Try at least 3 levels of decomposition first
2. **Detailed Analysis** - Provide comprehensive failure analysis to user
3. **Clear Options** - Present specific manual intervention options
4. **User Decision Required** - Wait for explicit user direction before proceeding
5. **Manual Override** - Allow user to modify plan or provide alternative approach

**Escalation Example:**
```
Task 2.3.2.3: "Implement state transition methods" - Failed after 3 decomposition levels

Director: "Multiple decomposition attempts failed. Escalating to user for manual intervention."

User Input Required For:
- Architecture decisions
- External dependencies
- Complex integration issues
- Design clarifications

Awaiting user decision before proceeding...
```

### 7. Integration with Memory Bank

#### Plan Storage and Retrieval
Cline MUST manage plans in the eternal memory bank structure:

```
docs/plans/
├── active/           # Currently being executed
├── completed/        # Successfully finished
├── blocked/          # Currently blocked
└── templates/        # Reusable plan templates
```

#### Progress Tracking Integration
Cline MUST update progress tracking in the eternal memory bank:

```markdown
## Active Plans
- [ ] [plan-name] (Phase X, Task Y - Status)
- [ ] [another-plan-name] (Phase X, Task Y - Status)

## Completed Plans
- [x] [completed-plan-name] (Completed - YYYY-MM-DD)
```

#### Plan Template Usage
Cline MUST utilize plan templates for common refactoring patterns:
- **Architecture Refactoring Template**
- **UI Component Migration Template**
- **Database Schema Migration Template**
- **API Integration Template**

### 8. Quality Gates and Validation

#### Completion Criteria
Tasks must pass these checks before completion:

- **Code Quality** - Follows established patterns and standards
- **Testing** - Includes appropriate tests if applicable
- **Documentation** - Updates relevant documentation
- **Integration** - Works with existing system components
- **Performance** - Meets performance requirements

#### Quality Gate Process
1. **Pre-Execution Check** - Verify task requirements
2. **During Execution** - Monitor adherence to standards
3. **Post-Execution Validation** - Verify completion criteria
4. **Integration Testing** - Test with existing components
5. **Performance Validation** - Ensure performance requirements met

### 9. Agent Coordination

#### Multiple Agent Support
Cline MUST coordinate multiple agents working on different plan sections:

1. **Task Assignment** - Assign non-conflicting tasks to different agents
2. **Progress Synchronization** - Coordinate progress updates
3. **Dependency Management** - Handle inter-agent dependencies
4. **Conflict Resolution** - Resolve resource conflicts
5. **Result Integration** - Combine results from multiple agents

#### Agent Communication Protocol
1. **Task Handoff** - Clear handoff between agents
2. **Progress Reporting** - Regular progress updates
3. **Issue Escalation** - Escalate blocked or failed tasks
4. **Result Validation** - Validate agent results
5. **Plan Updates** - Update plan status based on agent results

## Usage Examples

### Example 1: Starting a New Plan
**Cline Issues Command:**
```
Starting plan execution: [plan-name]

Phase 1: Foundation Setup
Command: cline task "pick up Phase 1, Task 1.1 from [plan-name]"
```

**User Executes Command:**
```bash
cline task "pick up Phase 1, Task 1.1 from [plan-name]"
```

**Cline Response:**
```
Starting Phase 1, Task 1.1: "Create core layout interfaces"

Prerequisites Check:
✅ Phase 1 prerequisites met
✅ Task 1.1 prerequisites met
✅ Dependencies available

Loading Context:
- Plan: [plan-name]
- Phase: 1 (Foundation Setup)
- Task: 1.1 (Create core layout interfaces)
- Dependencies: None
- Estimated Duration: 1 day

Initializing Agent with Task Context...
```

### Example 2: Handling Blocked Dependencies
```bash
cline task "pick up Phase 3, Task 3.2 from [plan-name]"
```

**Cline Response:**
```
Cannot execute Phase 3, Task 3.2: "Extract KAMA grid strategy"

Status: [!] Blocked

Blocked By:
- Phase 1: Foundation (Incomplete - Task 1.4 not started)
- Phase 2: Strategy Extraction (Not Started)

Next Available: After Phase 1 completion

Available Tasks:
- Phase 1, Task 1.1: "Create directory structure" (Ready)
- Phase 1, Task 1.2: "Implement event system" (Ready)
- Phase 1, Task 1.3: "Build settings hierarchy" (Ready)
```

### Example 3: Automatic Plan Decomposition
**During Execution:**
```
Task 2.3: "Implement strategy manager" - Context Usage: 45% (Approaching Limit)

⚠️ Context Overflow Detected

Initiating Automatic Decomposition:
- Original Task 2.3: "Implement strategy manager" (Too Complex)
- Creating Sub-Task 2.3.1: "Create strategy manager base class"
- Creating Sub-Task 2.3.2: "Implement strategy lifecycle management"
- Creating Sub-Task 2.3.3: "Add strategy registry integration"

Updating Plan Status:
- Task 2.3: [!] Incomplete (Requires decomposition)
- Task 2.3.1: [ ] Ready for execution
- Task 2.3.2: [ ] Ready for execution
- Task 2.3.3: [ ] Ready for execution
```

## Implementation Guidelines

### For Plan Creators
1. **Use Standard Format** - Follow the standardized plan structure
2. **Define Clear Dependencies** - Specify sequential vs parallel execution
3. **Estimate Context Usage** - Mark tasks that may require decomposition
4. **Include Validation Criteria** - Define clear completion requirements
5. **Update Progress Regularly** - Keep status indicators current

### For Plan Executors (Cline)
1. **Validate Dependencies** - Always check prerequisites before starting
2. **Monitor Context Usage** - Watch for overflow and decompose as needed
3. **Use Baby Steps™** - Execute tasks incrementally with validation
4. **Update Status** - Mark completion and update plan status
5. **Handle Errors Gracefully** - Block dependencies and create recovery plans

### For Users
1. **Create Plans** - Design comprehensive plans following the standardized format
2. **Appoint Director** - Explicitly request Cline to act as director for plan implementation
3. **Monitor Progress** - Check plan status regularly
4. **Handle Blockers** - Address blocked dependencies promptly
5. **Review Decompositions** - Approve plan splits when suggested
6. **Provide Feedback** - Report issues or improvements needed

## Error Handling

### Plan Not Found
When a plan cannot be found:
```
Error: Plan "[plan-name]" not found in eternal memory bank.

Available Plans:
- [List actual plans in memory bank]

Please check the plan name and ensure it exists in the eternal memory bank.
```

### Invalid Task Format
When task format is invalid:
```
Error: Invalid task format. Use one of these formats:

cline task "pick up Phase X, Task Y from [plan-name]"
cline task "pick up Task Name from [plan-name]"
cline task "pick up Phase Name from [plan-name]"

Example: cline task "pick up Phase 1, Task 1.1 from [plan-name]"
```

### Missing Dependencies
When dependencies are missing:
```
Error: Cannot execute task due to missing dependencies.

Task: Phase 3, Task 2
Missing Dependencies:
- Phase 2 (Incomplete)
- Task 2.3 (Not Started)

Please complete dependencies before proceeding, or use:
cline task "pick up available tasks from [plan-name]" to see available tasks.
```

### Context Overflow
When context limits are exceeded:
```
⚠️ Context Overflow Detected

Task: Phase 2, Task 2.3 - "Implement strategy manager"
Context Usage: 45% (Approaching Limit)

Initiating Automatic Decomposition:
- Creating Sub-Task 2.3.1: "Create strategy manager base class"
- Creating Sub-Task 2.3.2: "Implement strategy lifecycle management"
- Creating Sub-Task 2.3.3: "Add strategy registry integration"

Updated Plan Status:
- Task 2.3: [!] Incomplete (Requires decomposition)
- Task 2.3.1: [ ] Ready for execution
- Task 2.3.2: [ ] Ready for execution
- Task 2.3.3: [ ] Ready for execution

Continue with Sub-Task 2.3.1? (yes/no)
```

## Benefits of the Orchestration System

### Efficiency
- **Parallel Execution** - Multiple tasks can run concurrently
- **Context Optimization** - Tasks fit within model limits
- **Automatic Decomposition** - Complex tasks are broken down automatically
- **Dependency Management** - Prevents blocked execution paths

### Reliability
- **Progress Tracking** - Real-time status updates
- **Error Recovery** - Automatic handling of failures and blockers
- **Quality Gates** - Ensures tasks meet completion criteria
- **Validation** - Verifies dependencies before execution

### Scalability
- **Modular Design** - Plans can be easily extended or modified
- **Template System** - Reusable patterns for common tasks
- **Agent Coordination** - Multiple agents can work on different plan sections
- **Memory Management** - Efficient use of context and memory

## Future Enhancements

### Advanced Features
- **Predictive Analysis** - Predict task duration and resource needs
- **Risk Assessment** - Identify high-risk tasks and mitigation strategies
- **Resource Optimization** - Optimize team allocation and parallel execution
- **Integration Monitoring** - Track integration points between tasks

### User Interface
- **Visual Progress Tracking** - Gantt charts and dependency graphs
- **Real-time Notifications** - Alerts for task completion and blockers
- **Interactive Plan Editing** - Modify plans during execution
- **Progress Reporting** - Automated progress reports and summaries

This Plan Directing Rule enables Cline to efficiently manage complex refactoring plans while maintaining clear dependencies and enabling efficient parallel execution.
