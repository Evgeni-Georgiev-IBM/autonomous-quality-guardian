# Jira Ticket Planning Rule

## Automatic Ticket-Based Plan Generation

When a user mentions a Jira ticket key in Plan mode, automatically fetch the ticket information and create a comprehensive implementation plan based on the ticket's requirements.

## Workflow

### 1. Ticket Detection and Fetching

Detect Jira ticket keys using the same pattern as Ask mode:
- **Pattern**: `[A-Z]{2,10}-\d+`
- **Examples**: `WDY-123`, `PROJECT-456`, `#TICKET-789`
- **Context-aware**: Extract from git branches, commit messages, file paths

When a ticket key is detected:

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

### 2. Analyze Ticket Information

Extract and analyze key information from the fetched ticket:
- **Summary**: Main objective of the ticket
- **Description**: Detailed requirements, acceptance criteria, technical notes
- **Type**: Bug, Story, Task, Epic, etc.
- **Priority**: Urgency and importance
- **Labels**: Technical tags, component identifiers
- **Status**: Current state (helps understand if this is new work or continuation)

### 3. Generate Implementation Plan

Based on the ticket information, automatically create a structured implementation plan with:

#### Plan Structure

```markdown
# Implementation Plan: [TICKET-KEY] - [Summary]

## 📋 Ticket Overview
- **Key**: [TICKET-KEY]
- **Summary**: [Brief summary]
- **Type**: [Bug/Story/Task/etc.]
- **Priority**: [High/Medium/Low]
- **Status**: [Current status]
- **Link**: [Jira URL]

## 🎯 Objectives
[Extract main goals from ticket description]

## 📝 Requirements Analysis
[Break down requirements from description into actionable items]

## 🏗️ Implementation Steps

### Phase 1: Analysis & Preparation
- [ ] Review existing codebase
- [ ] Identify affected components
- [ ] Review related tickets/dependencies
- [ ] Set up development environment

### Phase 2: Core Implementation
[Generate specific steps based on ticket description]
- [ ] [Step 1 from requirements]
- [ ] [Step 2 from requirements]
- [ ] [Step 3 from requirements]

### Phase 3: Testing & Validation
- [ ] Write unit tests
- [ ] Write integration tests
- [ ] Manual testing
- [ ] Code review preparation

### Phase 4: Documentation & Deployment
- [ ] Update technical documentation
- [ ] Update user documentation
- [ ] Create deployment plan
- [ ] Prepare release notes

## 🔍 Technical Considerations
[Extract technical details from description]

## ⚠️ Risks & Challenges
[Identify potential issues based on ticket type and description]

## 📦 Deliverables
[List expected outputs]

## ✅ Acceptance Criteria
[Extract from ticket description or generate based on requirements]

## 🔗 Related Resources
- Jira Ticket: [URL]
- Related Tickets: [If mentioned in description]
- Documentation: [If referenced]
```

### 4. Context-Aware Plan Generation

Adapt the plan based on:

#### Ticket Type
- **Bug**: Focus on root cause analysis, fix implementation, regression testing
- **Story**: Focus on feature implementation, user acceptance criteria, documentation
- **Task**: Focus on specific deliverables, completion criteria
- **Epic**: Break down into sub-tasks, create high-level roadmap

#### Priority
- **High/Critical**: Add urgency notes, identify quick wins, parallel work streams
- **Medium**: Standard implementation flow
- **Low**: Consider technical debt reduction, refactoring opportunities

#### Project Context
- Check current git branch for related work
- Scan codebase for related files/components
- Identify existing patterns to follow

### 5. Codebase Integration

After generating the plan, automatically:

1. **Scan for related files**:
   ```xml
   <search_files>
   <path>main/</path>
   <regex>[keywords from ticket]</regex>
   </search_files>
   ```

2. **List relevant code definitions**:
   ```xml
   <list_code_definition_names>
   <path>[identified directories]</path>
   </list_code_definition_names>
   ```

3. **Update plan with specific file references**

### 6. Interactive Planning

After presenting the initial plan, offer:
- "Would you like me to elaborate on any specific phase?"
- "Should I start with Phase 1?"
- "Do you want to modify any steps?"
- "Shall I create a TODO list from this plan?"

## User Commands

Support explicit planning commands:

- `"plan WDY-123"` → Fetch ticket and generate plan
- `"create plan for WDY-123"` → Fetch ticket and generate plan
- `"how to fix WDY-123?"` → Fetch ticket and generate plan
- `"implement WDY-123"` → Fetch ticket, generate plan, offer to start implementation

## Git Branch Integration

When user asks "what should I work on?" or "what's next?":

1. Get current branch: `git rev-parse --abbrev-ref HEAD`
2. Extract ticket key from branch name
3. Fetch ticket information
4. Generate implementation plan
5. Identify current progress based on completed files/commits

## Multi-Ticket Planning

When multiple tickets are mentioned:

1. Fetch all tickets
2. Analyze dependencies and relationships
3. Create a consolidated plan with:
   - Execution order
   - Parallel work opportunities
   - Shared components
   - Integration points

## Plan Refinement

After initial plan generation:

1. **Analyze codebase** to refine steps with specific file paths
2. **Check for existing tests** to identify test patterns
3. **Review build configuration** to understand deployment process
4. **Scan for similar implementations** to suggest patterns

## Error Handling

- **Ticket not found**: Ask user to verify ticket key or check access permissions
- **Insufficient description**: Generate generic plan and ask for clarification
- **MCP server unavailable**: Provide instructions to configure Jira MCP server
- **Multiple interpretations**: Present options and ask user to choose

## Examples

### Example 1: Bug Fix Planning
**User:** "plan WDY-6708"

**Action:**
1. Fetch WDY-6708 (Runtime Base Deployment removal)
2. Analyze description (deprecation, watt property, default change)
3. Generate plan with phases:
   - Code analysis (find Runtime Base Deployment usage)
   - Add watt property configuration
   - Update UI to remove deployment type selection
   - Update tests
   - Documentation updates
   - XPC communication

### Example 2: Git Branch Context
**User:** "what should I do next?"

**Action:**
1. Get branch: `feature/WDY-6708-remove-runtime-deployment`
2. Extract: `WDY-6708`
3. Fetch ticket
4. Check git log for completed work
5. Generate plan showing remaining steps

### Example 3: Multiple Tickets
**User:** "plan WDY-123 and WDY-456"

**Action:**
1. Fetch both tickets
2. Analyze relationship
3. Create consolidated plan with execution order

## Key Principles

1. **Be comprehensive**: Cover all aspects from analysis to deployment
2. **Be specific**: Reference actual files, classes, methods when possible
3. **Be practical**: Break down into actionable, testable steps
4. **Be adaptive**: Adjust plan based on ticket type, priority, and context
5. **Be proactive**: Identify potential issues and suggest solutions
6. **Be collaborative**: Offer to start implementation after plan approval

## MCP Server Requirements

This rule requires the `jira` MCP server with:
- `get_jira_issue`: Fetch ticket details
- `search_jira_issues`: Find related tickets (optional)

## Integration with Other Modes

- **Ask mode**: Fetch ticket info for context
- **Plan mode**: Generate implementation plan (this rule)
- **Code mode**: Execute the plan with actual code changes
- **Architect mode**: Design system-level changes for complex tickets