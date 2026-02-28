---
description: Enforces exact file reading and copying workflow before using replace_in_file to prevent matching failures
author: Cline
version: 1.0
globs: ["**/*"]
tags: ["workflow", "file-editing", "best-practices"]
---

# Exact File Editing Workflow

## Objective

This rule ensures that `replace_in_file` operations always succeed by enforcing a strict workflow:
1. **Read first** - Always read the file before editing
2. **Copy exact lines** - Copy lines from output, never reconstruct from memory
3. **Preserve formatting** - Match exact whitespace, indentation, and line endings

## Critical Rule: Read Before Replace

**⚠️ MANDATORY - YOU MUST FOLLOW THIS WORKFLOW ⚠️**

Before using `replace_in_file`, you **MUST**:

### Step 1: Read the File

Always call `read_file` first to see the exact content:

```xml
<read_file>
<path>path/to/file.py</path>
</read_file>
```

### Step 2: Copy Exact Lines from Output

**NEVER** reconstruct code from memory. **ALWAYS** copy the exact lines from the tool output.

**❌ WRONG - Reconstructing from memory:**
```xml
<replace_in_file>
<absolutePath>C:/s/project/file.py</absolutePath>
<diff>
------- SEARCH
def my_function():
    x = 1  # Did you get the indentation right?
    return x
=======
def my_function():
    x = 2
    return x
+++++++ REPLACE
</diff>
</replace_in_file>
```

**✅ CORRECT - Copying exact lines from read_file output:**
```xml
<read_file>
<path>file.py</path>
</read_file>

# Then copy from output:
<replace_in_file>
<absolutePath>C:/s/project/file.py</absolutePath>
<diff>
------- SEARCH
def my_function():
    x = 1
    return x
=======
def my_function():
    x = 2
    return x
+++++++ REPLACE
</diff>
</replace_in_file>
```

### Step 3: Include Enough Context

Make your SEARCH block unique by including surrounding lines:

**❌ WRONG - Too little context (may match multiple places):**
```
------- SEARCH
    x = 1
=======
    x = 2
+++++++ REPLACE
```

**✅ CORRECT - Unique context:**
```
------- SEARCH
def my_function():
    """Calculate something."""
    x = 1
    return x
=======
def my_function():
    """Calculate something."""
    x = 2
    return x
+++++++ REPLACE
```

### Step 4: Preserve Exact Formatting

Match character-for-character:
- Spaces vs tabs (count them)
- Trailing whitespace
- Line endings (LF vs CRLF)
- Comments and docstrings

## Complete Workflow Example

**Task:** Change the timeout value in a function.

**Step 1 - Read the file:**
```xml
<read_file>
<path>core/settings.py</path>
</read_file>
```

**Step 2 - See the output:**
```python
# core/settings.py
class Settings:
    """Application settings."""
    
    def __init__(self):
        self.timeout = 30  # seconds
        self.retries = 3
```

**Step 3 - Copy exact lines for SEARCH:**
```xml
<replace_in_file>
<absolutePath>C:/s/project/core/settings.py</absolutePath>
<diff>
------- SEARCH
    def __init__(self):
        self.timeout = 30  # seconds
        self.retries = 3
=======
    def __init__(self):
        self.timeout = 60  # seconds
        self.retries = 3
+++++++ REPLACE
</diff>
</replace_in_file>
```

## Handling Common Issues

### Issue: File Changed Between Read and Replace

**Solution:** Re-read the file if replace fails:
1. Note the error
2. Read the file again to get current state
3. Retry with updated content

### Issue: Multiple Matches

**Solution:** Include more unique context:
- Add function/class names
- Include surrounding comments
- Add more lines above and below

### Issue: Whitespace Mismatch

**Solution:** 
- Use `show invisible characters` in your editor
- Copy-paste directly from output
- Check for tabs vs spaces

## When to Use write_to_file Instead

If you need to make extensive changes or the file is small, use `write_to_file`:

```xml
<write_to_file>
<absolutePath>C:/s/project/file.py</absolutePath>
<content># entire new content</content>
</write_to_file>
```

## Verification Checklist

Before calling `replace_in_file`, verify:

- [ ] I have read the file with `read_file`
- [ ] I copied the SEARCH content from the output (not from memory)
- [ ] I included enough context to make the match unique
- [ ] I preserved exact whitespace and formatting
- [ ] Each line in SEARCH is complete (not truncated)

## Search Files Best Practices

For `search_files`, follow similar principles:

1. **Start simple** - Use basic patterns first
2. **Escape special characters** - `\. \* \+ \(\) \[\]`
3. **Test patterns** - Verify they match what you expect

**❌ WRONG - Complex pattern with unescaped chars:**
```xml
<search_files>
<path>.</path>
<regex>def.*(.*):</regex>
</search_files>
```

**✅ CORRECT - Simple, escaped pattern:**
```xml
<search_files>
<path>.</path>
<regex>def \w+\(</regex>
</search_files>
```

## Summary

**The Golden Rule:**
> When in doubt, read the file first. Copy exact lines. Never guess.

This workflow prevents:
- Failed replacements due to whitespace differences
- Wrong matches due to insufficient context
- Errors from reconstructed code

Following this rule ensures reliable, predictable file editing.
