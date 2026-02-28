---
description: Automatically use Context7 MCP for library/API documentation, code generation, setup or configuration steps
author: Cline
version: 1.0
category: "Cline Core"
tags: ["context7", "auto-usage", "documentation", "code-generation"]
globs: ["*"]
---

# Context7 Auto-Usage Rule

## Objective
This rule ensures that Context7 MCP is automatically invoked when the user needs library/API documentation, code generation, setup, or configuration steps without requiring explicit requests.

## Trigger Conditions
Context7 MCP should be automatically used when ANY of the following conditions are met:

1. **Library/API Documentation Requests**:
   - Questions about specific library functions, methods, or classes
   - Requests for API documentation or examples
   - Questions about library usage patterns or best practices

2. **Code Generation Tasks**:
   - Requests to generate code using specific libraries or frameworks
   - Implementation of library-specific functionality
   - Setup of library configurations or integrations

3. **Setup and Configuration Steps**:
   - Installation and setup of libraries/frameworks
   - Configuration of library-specific settings
   - Integration of libraries with existing projects

4. **Version-Specific Requests**:
   - Questions about specific library versions
   - Migration between library versions
   - Version compatibility issues

## Implementation Guidelines

### Automatic Context7 Invocation
- **MUST** automatically prepend "use context7" to any prompt that matches the trigger conditions
- **MUST** use the `resolve-library-id` tool first to identify the appropriate library
- **MUST** then use the `query-docs` tool to retrieve relevant documentation

### Library Identification
- **MUST** analyze the user's request to identify relevant libraries
- **MUST** use library names mentioned in the request
- **MUST** infer libraries based on the context when not explicitly mentioned

### Query Formulation
- **MUST** create specific, well-formed queries for Context7
- **MUST** include relevant details from the user's request
- **MUST** maintain the original intent and context of the user's request

## Examples

### Example 1: Library Documentation
**User Request**: "How do I use the useEffect hook in React?"

**Automatic Action**:
- Identify library: React
- Use Context7 to retrieve useEffect documentation
- Provide comprehensive answer with up-to-date examples

### Example 2: Code Generation
**User Request**: "Create a Next.js API route that connects to MongoDB"

**Automatic Action**:
- Identify libraries: Next.js, MongoDB
- Use Context7 to retrieve relevant documentation
- Generate code with proper integration patterns

### Example 3: Setup and Configuration
**User Request**: "How do I set up authentication with Firebase in a React app?"

**Automatic Action**:
- Identify libraries: Firebase, React
- Use Context7 to retrieve authentication setup documentation
- Provide step-by-step configuration guide

## Exceptions
- **MUST NOT** use Context7 for general programming questions unrelated to specific libraries
- **MUST NOT** use Context7 when the user explicitly requests not to use it
- **MUST NOT** use Context7 for non-technical questions or discussions

## Priority and Fallback
- **MUST** prioritize Context7 for library-specific requests
- **MUST** fall back to general knowledge when Context7 is unavailable
- **MUST** inform the user when Context7 is being used automatically

## User Communication
- **SHOULD** inform the user that Context7 is being used automatically
- **SHOULD** provide attribution for information retrieved from Context7
- **SHOULD** allow users to opt-out of automatic Context7 usage if desired