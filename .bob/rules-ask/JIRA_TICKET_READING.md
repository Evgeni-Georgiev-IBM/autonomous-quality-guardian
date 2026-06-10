# Jira Ticket Reading Rule

## Automatic Ticket Detection and Fetching

When a user mentions a Jira ticket key in their message, automatically fetch and display the ticket information using the configured Jira MCP server.

### Ticket Key Pattern Recognition

Detect Jira ticket keys in the following formats:
- `PROJECT-123` (standard format)
- `WDY-456` (example with specific project)
- `#PROJECT-789` (with hash prefix)
- Embedded in text: "I'm working on WDY-123 and need help"
- Multiple tickets: "Review WDY-123 and WDY-456"

### Ticket Key Extraction Rules

1. **Pattern**: `[A-Z]{2,10}-\d+`
   - Project key: 2-10 uppercase letters
   - Separator: hyphen (-)
   - Issue number: one or more digits

2. **Context-aware extraction**:
   - Extract from git branch names: `feature/WDY-123-description` → `WDY-123`
   - Extract from commit messages: `"Fix WDY-123: bug description"` → `WDY-123`
   - Extract from file paths: `docs/WDY-123-spec.md` → `WDY-123`

### Automatic Fetch Behavior

When a ticket key is detected:

1. **Use the Jira MCP server** to fetch the issue:
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

2. **Display the ticket information** in a structured format:
   ```
   📋 **Jira Ticket: WDY-123**
   
   **Summary:** [Issue summary]
   **Type:** [Story/Bug/Task/etc.]
   **Status:** [To Do/In Progress/Done/etc.]
   **Priority:** [High/Medium/Low]
   **Assignee:** [Username or Unassigned]
   **Reporter:** [Username]
   **Labels:** [label1, label2, ...]
   
   **Description:**
   [First 500 characters of description...]
   
   **Link:** https://[jira-instance]/browse/WDY-123
   ```

3. **Handle multiple tickets**: If multiple ticket keys are detected, fetch and display each one sequentially.

### Error Handling

- **Ticket not found (404)**: Inform user that ticket `WDY-123` does not exist or they don't have access
- **MCP server not configured**: Provide setup instructions for the Jira MCP server
- **Authentication error**: Inform user to check their Jira credentials in MCP settings
- **Network error**: Suggest retrying or checking connection

### Integration with Git Context

When analyzing git branches or commits, automatically extract and fetch related tickets:

```bash
# Get current branch
git rev-parse --abbrev-ref HEAD
# Output: feature/WDY-123-new-feature

# Extract ticket: WDY-123
# Automatically fetch and display ticket info
```

### User Commands

Support explicit commands for ticket reading:

- `"show WDY-123"` → Fetch and display ticket
- `"read ticket WDY-123"` → Fetch and display ticket
- `"what is WDY-123?"` → Fetch and display ticket
- `"get details for WDY-123"` → Fetch and display ticket

### When NOT to Auto-Fetch

Do not automatically fetch tickets in these cases:
- Ticket key appears in code comments or strings (unless explicitly requested)
- Ticket key is part of a URL that's already a link
- User explicitly says "don't fetch" or "ignore ticket"
- Ticket key appears in historical context (e.g., "we used to have WDY-123")

### MCP Server Configuration

This rule requires the `jira` MCP server to be configured with the following tools:
- `get_jira_issue`: Fetch a single issue by key
- `search_jira_issues`: Search for issues using JQL (optional, for advanced queries)

Verify MCP server is available before attempting to fetch tickets.

## Examples

### Example 1: Simple Mention
**User:** "I'm working on WDY-13745"

**Action:** Automatically detect `WDY-13745`, fetch using MCP, and display ticket details.

### Example 2: Git Branch Context
**User:** "What should I do next?"

**Action:** 
1. Check current git branch: `git rev-parse --abbrev-ref HEAD`
2. If branch is `feature/WDY-13745-implement-oauth`, extract `WDY-13745`
3. Fetch and display ticket to provide context for the question

### Example 3: Multiple Tickets
**User:** "Compare WDY-123 and WDY-456"

**Action:** Fetch both tickets and display them side-by-side for comparison.

### Example 4: Explicit Command
**User:** "show me WDY-13745"

**Action:** Fetch and display the ticket with full details.

## Key Principles

1. **Be proactive**: Automatically fetch tickets when mentioned to provide context
2. **Be efficient**: Cache ticket data during a conversation to avoid redundant fetches
3. **Be clear**: Always show the ticket key prominently when displaying information
4. **Be helpful**: Offer related actions after displaying a ticket (e.g., "Would you like to update this ticket?")
5. **Be respectful**: Don't fetch if the user indicates they don't want automatic fetching