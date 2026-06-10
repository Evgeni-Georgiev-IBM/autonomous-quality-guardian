# Jira Ticket Implementation Rule

## Automatic Ticket-Based Code Implementation

When a user mentions a Jira ticket key in Code or Advanced mode, automatically fetch the ticket information, analyze requirements, and implement the necessary code changes to resolve the issue.

## Workflow

### 1. Ticket Detection and Fetching

Detect Jira ticket keys using the pattern `[A-Z]{2,10}-\d+` and automatically fetch ticket details:

```xml
<use_mcp_tool>
<server_name>jira</server_name>
<tool_name>get_jira_issue</tool_name>
<arguments>
{
  "issueKey": "WDY-123"
}
</arguments>
</use_mcp_tool>
```

### 2. Requirements Analysis

Extract and analyze ticket information:
- **Summary**: Main objective
- **Description**: Detailed requirements, technical specifications, acceptance criteria
- **Type**: Bug fix, feature implementation, refactoring, etc.
- **Priority**: Determines implementation approach
- **Labels**: Technical tags for component identification

### 3. Codebase Discovery

Automatically discover relevant code:

#### A. Search for Related Code
```xml
<search_files>
<path>main/</path>
<regex>[keywords from ticket summary/description]</regex>
<file_pattern>*.java</file_pattern>
</search_files>
```

#### B. List Code Definitions
```xml
<list_code_definition_names>
<path>[identified directories]</path>
</list_code_definition_names>
```

#### C. Read Relevant Files
```xml
<read_file>
<args>
<file>
<path>[identified file 1]</path>
</file>
<file>
<path>[identified file 2]</path>
</file>
</args>
</read_file>
```

### 4. Implementation Strategy

Based on ticket type, choose implementation approach:

#### Bug Fix Strategy
1. Locate the bug source
2. Understand root cause
3. Implement minimal fix
4. Add/update tests to prevent regression
5. Verify fix doesn't break existing functionality

#### Feature Implementation Strategy
1. Identify integration points
2. Design implementation following existing patterns
3. Implement core functionality
4. Add comprehensive tests
5. Update documentation

#### Refactoring Strategy
1. Understand current implementation
2. Identify improvement opportunities
3. Refactor incrementally
4. Ensure tests pass at each step
5. Update related documentation

### 5. Code Implementation

Implement changes using appropriate tools:

#### For Targeted Changes (Preferred)
```xml
<apply_diff>
<path>path/to/file.java</path>
<diff>
<<<<<<< SEARCH
:start_line:10
-------
existing code to replace
=======
new code implementation
>>>>>>> REPLACE
</diff>
</apply_diff>
```

#### For New Files
```xml
<write_to_file>
<path>path/to/new/file.java</path>
<content>
complete file content
 style** exceptions (PMD rules)
- **Ant build coexistence** for CLI tools
- **Encryption keys** handling

### Incremental Implementation

For complex tickets:

1. **Break into phases** (from Plan mode)
2. **Implement one phase at a time**
3. **Test after each phase**
4. **Commit incrementally** with descriptive messages
5. **Update TODO list** to track progress

## User Commands

Support explicit implementation commands:

- `"implement WDY-123"` → Fetch ticket, analyze, implement code
- `"fix WDY-123"` → Fetch ticket, implement bug fix
- `"code WDY-123"` → Fetch ticket, implement changes
- `"start WDY-123"` → Fetch ticket, begin implementation
- `"continue WDY-123"` → Resume implementation from current state

## Multi-Step Implementation

For tickets requiring multiple changes:

1. **Create TODO list**:
   ```xml
   <update_todo_list>
   <todos>
   [ ] Phase 1: Analyze codebase
   [ ] Phase 2: Implement core changes
   [ ] Phase 3: Add tests
   [ ] Phase 4: Update documentation
   </todos>
   </update_todo_list>
   ```

2. **Execute step-by-step**, updating TODO after each completion

3. **Wait for user confirmation** between major phases

## Error Handling

- **Ticket not found**: Verify ticket key and access permissions
- **Insufficient requirements**: Ask user for clarification
- **Build failures**: Analyze errors, fix, and retry
- **Test failures**: Debug, fix implementation, rerun tests
- **Merge conflicts**: Resolve conflicts, maintain functionality

## Quality Checks

Before completion:

1. **Code style**: Follow PMD rules (pmd_ruleset.xml)
2. **Test coverage**: Ensure adequate test coverage
3. **Build verification**: All modules build successfully
4. **Documentation**: Code comments and docs updated
5. **No regressions**: Existing tests still pass

## Examples

### Example 1: Bug Fix Implementation
**User:** "fix WDY-6708"

**Action:**
1. Fetch WDY-6708 (Remove Runtime Base Deployment)
2. Search for "Runtime Base Deployment" in codebase
3. Identify affected files
4. Read relevant files
5. Implement changes:
   - Add watt property for enablement
   - Update UI to remove deployment type selection
   - Make Repository Base Deployment default
   - Update tests
6. Run tests
7. Present completion summary

### Example 2: Feature Implementation
**User:** "implement WDY-456"

**Action:**
1. Fetch ticket (new feature)
2. Analyze requirements
3. Search for integration points
4. Design implementation following existing patterns
5. Implement feature incrementally
6. Add comprehensive tests
7. Update documentation
8. Verify build and tests

### Example 3: Git Branch Context
**User:** "continue implementation"

**Action:**
1. Get current branch: `feature/WDY-123-oauth`
2. Extract ticket: WDY-123
3. Fetch ticket details
4. Check git log for completed work
5. Identify remaining tasks
6. Continue implementation from current state

### Example 4: Multi-Ticket Implementation
**User:** "implement WDY-123 and WDY-456"

**Action:**
1. Fetch both tickets
2. Analyze dependencies
3. Determine implementation order
4. Implement first ticket completely
5. Test and verify
6. Implement second ticket
7. Test integration
8. Present consolidated summary

## Integration with Other Modes

- **Ask mode**: Fetch ticket info for context
- **Plan mode**: Generate implementation plan
- **Code mode**: Execute implementation (this rule)
- **Advanced mode**: Use MCP tools for complex operations

## Ticket Status Updates

After successful implementation, offer to update Jira:

```markdown
Implementation complete! Would you like me to:
- Update ticket status to "In Review"?
- Add implementation notes to ticket?
- Link related commits?
```

## Best Practices

1. **Read before writing**: Always read files before modifying
2. **Minimal changes**: Make smallest change that solves the problem
3. **Test-driven**: Write/update tests alongside implementation
4. **Incremental commits**: Commit logical units of work
5. **Clear messages**: Use descriptive commit messages with ticket key
6. **Follow patterns**: Match existing code style and architecture
7. **Document changes**: Update comments and documentation
8. **Verify thoroughly**: Test, build, and validate before completion

## MCP Server Requirements

This rule requires the `jira` MCP server with:
- `get_jira_issue`: Fetch ticket details
- `search_jira_issues`: Find related tickets (optional)
- `update_jira_issue`: Update ticket status (optional)

## Key Principles

1. **Be thorough**: Understand requirements completely before coding
2. **Be precise**: Make exact changes needed, no more, no less
3. **Be safe**: Test thoroughly, avoid breaking existing functionality
4. **Be consistent**: Follow project conventions and patterns
5. **Be communicative**: Explain changes clearly in completion summary
6. **Be proactive**: Identify and address potential issues early
7. **Be collaborative**: Wait for user confirmation on major decisions