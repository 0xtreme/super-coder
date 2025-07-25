# Claude Code Optimization Guide

## Overview
This guide provides best practices for maximizing productivity with Claude Code. Follow these guidelines to avoid common mistakes and optimize performance. These practices are language and framework agnostic - adapt them to your specific tech stack.

## Essential Components
- Claude Code CLI tool
- Model Context Protocol (MCP) servers
- Project-specific CLAUDE.md file
- Custom slash commands
- Your chosen tech stack and tools

## Project Structure
```
project-root/
├── .claude/
│   ├── commands/
│   │   ├── build.md
│   │   ├── test.md
│   │   └── deploy.md
│   └── CLAUDE.md
├── src/
├── tests/
├── docs/          # All documentation
├── scripts/       # Utility scripts
├── logs/          # Log files
└── README.md
```

## Essential Commands

### Built-in Commands
- `/compact` - Minimize context usage when approaching token limits
- `/clear` - Clear conversation history for fresh start
- `/init` - Initialize CLAUDE.md for your project
- `/bug` - Report issues directly to Anthropic
- `#` - Add information to CLAUDE.md during conversation

### Custom Commands
Place custom command files in `.claude/commands/` directory:
- `/project:build` - Run build process
- `/project:test` - Execute test suite
- `/project:deploy` - Deploy application

## Code Style Guidelines

### General Principles
- Be explicit about coding patterns and conventions
- Use consistent naming conventions across the project
- Prefer functional programming patterns where appropriate
- Always handle errors gracefully
- **Document every change with comments**
- **Commit frequently with descriptive messages**
- **Maintain organized file structure**
- **Check official documentation for latest practices**
- **Request web search when encountering unknowns**
- **Keep only working code - remove failed attempts**

### Code Cleanup During Debugging
<debugging_cleanup>
1. **When troubleshooting/debugging:**
   - Delete failed attempts, don't comment them out
   - Keep ONLY the working solution
   - Remove console.log and debug statements
   - Clean up any temporary test code

2. **Exception: Preserve existing code when:**
   - It was already in the codebase before debugging
   - It serves a different purpose
   - It's part of legacy functionality
   
3. **Example - WRONG approach:**
   ```javascript
   // Attempt 1 - didn't work
   // function calculateTotal(items) {
   //   return items.reduce((a, b) => a + b);
   // }
   
   // Attempt 2 - also failed
   // function calculateTotal(items) {
   //   let sum = 0;
   //   items.forEach(item => sum += item);
   //   return sum;
   // }
   
   // This one works!
   function calculateTotal(items) {
     return items.reduce((sum, item) => sum + item.price, 0);
   }
   ```
   
4. **Example - CORRECT approach:**
   ```javascript
   function calculateTotal(items) {
     return items.reduce((sum, item) => sum + item.price, 0);
   }
   ```
</debugging_cleanup>

### When Stuck or Uncertain
- If encountering an error you can't resolve, ask user to search for solution
- Always request latest documentation for new implementations
- Suggest checking official docs when using new libraries
- Example: "I'm not sure about this error. Could you search for '[specific error message]' to find the latest solution?"

### File Organization
- Place all logs in `logs/` folder
- Move documentation to `docs/` folder
- Keep scripts in `scripts/` folder
- Clean up temporary files after use
- Remove duplicate or test files
- Update **project** CLAUDE.md with new file references (NOT global ~/.claude/CLAUDE.md)

### Language-Specific Examples

#### JavaScript/TypeScript
- Use ES modules (import/export), not CommonJS (require)
- Destructure imports when possible
- Use arrow functions for component definitions
- Prefer async/await over promises

#### Python
- Use type hints for all functions
- Follow PEP 8 style guide
- Use virtual environments (specify setup in CLAUDE.md)
- Prefer pathlib over os.path

#### Other Languages
- Adapt these principles to your language
- Document your specific conventions in CLAUDE.md
- Include language-specific tools and linters

## MCP Server Configuration

### Essential Servers
- `filesystem` - File operations with access controls
- `git` - Version control operations
- `memory` - Persistent context across sessions
- `postgresql`/`mysql` - Database operations

### Web Search Capability
Note: Claude Code doesn't have built-in web search. When you need current information:
- Claude should ask you to search
- You can install web search MCP servers if available
- Or manually search and provide the information

### Installation
```bash
# Install MCP servers via npm or appropriate package manager
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @modelcontextprotocol/server-git
```

## Common Mistakes to Avoid

### 1. Insufficient Context
- ❌ Vague project descriptions
- ✅ Detailed architecture, commands, and patterns in CLAUDE.md

### 2. Poor Command Documentation
- ❌ No clear command structure
- ✅ Explicit commands with examples

### 3. Missing Environment Setup
- ❌ No environment configuration
- ✅ Clear setup instructions with versions

### 4. Ignoring Error Patterns
- ❌ No error handling guidance
- ✅ Document common errors and solutions

### 5. Not Using MCP Servers
- ❌ Relying only on base Claude capabilities
- ✅ Install relevant MCP servers for your workflow

### 6. Not Preventing Assumptions
- ❌ Letting Claude make implementation choices
- ✅ Be explicit: "Ask before choosing libraries"
- ✅ Add to CLAUDE.md: "Always confirm approach before implementing"
- ✅ Use phrases like "Do not assume" in instructions

### 7. Context Degradation
- ❌ Continuing after hours without clearing
- ✅ Use `/clear` frequently for new tasks
- ✅ Use `/compact` proactively at checkpoints
- ✅ Create task checklists to track progress

### 8. False Completion Claims
- ❌ Accepting "everything is complete" at face value
- ✅ Maintain a todo.md checklist
- ✅ Ask Claude to verify each item explicitly
- ✅ Use Task() subagents for verification

## Performance Optimization

### Context Management
- Monitor context usage indicator
- Use `/compact` proactively at natural breakpoints
- Consider `/clear` when switching major tasks
- Keep CLAUDE.md concise and well-organized
- **Clear context after 1-2 hours of work**
- **Create session summaries before clearing**

### Managing Long Sessions
After extended work sessions, Claude's accuracy degrades:
1. **Use `/clear` frequently** - Best practice for maintaining accuracy
2. **Create checkpoint files** - Save state before clearing
3. **Use todo.md checklists** - Track incomplete items
4. **Verify completion claims** - Always double-check

Example checkpoint file:
```markdown
# Session Summary - [Date/Time]

## Completed
- ✅ Implemented user authentication
- ✅ Added database migrations

## In Progress
- ⏳ API endpoint for user profiles (50% done)

## TODO
- ❌ Write unit tests
- ❌ Update documentation
- ❌ Deploy to staging

## Next Steps
Continue with API endpoint, then tests
```

### Token Usage
- Use headless mode (`claude -p`) for simple tasks
- Pipe data efficiently: `cat data.csv | claude -p "analyze"`
- Configure OpenTelemetry for usage tracking

### Workflow Automation
Create reusable slash commands:
```markdown
# .claude/commands/full-deploy.md
Complete deployment process:

1. Run tests: `npm test`
2. Build: `npm run build`  
3. Deploy: `npm run deploy`
4. Verify: `npm run verify-deployment`
```

### File Organization Command
```markdown
# .claude/commands/organize-files.md
Organize project files:

1. Move all *.log files to logs/
2. Move all *.md files (except README.md and CLAUDE.md) to docs/
3. Move all *.sh, *.py scripts to scripts/
4. Clean up any .tmp, .temp, or duplicate files
5. Update CLAUDE.md with new file locations
6. Commit changes with message "chore: Organize project structure"
```

## CLAUDE.md Best Practices

### Structure
```markdown
# Project Name

## Overview
Brief description of project purpose and architecture

## Tech Stack
- [Your specific framework/language]
- [Your database choice]
- [Other key technologies]

## Commands
- [Your build command] - Build the project
- [Your test command] - Run tests
- [Your dev command] - Start development

## Code Style
- [Language-specific style guide]
- [Your linting rules]
- [Your formatting preferences]

## Project Guidelines
- Branch naming: [your convention]
- Commit format: [your format]
- PR process: [your requirements]

## Common Issues
1. [Project-specific issue] - [Solution]
2. [Another common issue] - [Fix]

## Important Rules
- Always ask before choosing implementation approach
- Never assume file locations or structures
- Confirm library choices before installing
- Do not refactor without explicit permission
- Check existing implementations before creating new ones
- **Document all changes and additions**
- **Commit after each significant change**
- **Clean up temporary/debug files**
- **Organize files into proper directories**
- **Update project CLAUDE.md with new documentation links (NOT global)**
- **Request web search for unknown errors**
- **Ask for official documentation when implementing new features**

## CLAUDE.md File Locations
<file_locations>
1. **Global CLAUDE.md** (~/.claude/CLAUDE.md):
   - Contains universal best practices
   - Never modify during project work
   - This is your personal configuration
   
2. **Project CLAUDE.md** (./CLAUDE.md or ./.claude/CLAUDE.md):
   - Project-specific rules and documentation
   - Update this with new doc links
   - Add project-specific patterns here
   - This is what you update during development
</file_locations>

## Web Search and Documentation
<documentation_rules>
1. When encountering unknown errors or issues:
   - Say: "I'm encountering [specific error]. Could you search for the latest solution?"
   - Don't guess or use outdated knowledge
   
2. Before implementing new libraries or frameworks:
   - Say: "Before we proceed, could you check the official [library] documentation for the latest best practices?"
   
3. When stuck on a problem for more than 2 attempts:
   - Say: "This approach isn't working. Could you search for alternative solutions to [problem]?"
   
4. Always prefer official documentation:
   - Request: "Please check the official docs for [technology] version [X]"
   - Don't rely on potentially outdated knowledge
</documentation_rules>

## File Management
<file_organization>
1. After creating any document, move it to docs/
2. Place all scripts in scripts/ folder
3. Move log files to logs/ folder
4. Delete temporary files after debugging
5. Remove duplicate files immediately
6. Update CLAUDE.md with links to new docs:
   - [[docs/api-guide.md]] - API documentation
   - [[docs/setup.md]] - Setup instructions
   - [[scripts/deploy.sh]] - Deployment script
</file_organization>

## Commit Strategy
<commit_rules>
1. Commit after implementing each feature
2. Commit before starting debugging
3. Commit after fixing each bug
4. Use descriptive commit messages:
   - feat: Add user authentication
   - fix: Resolve database connection issue
   - docs: Update API documentation
   - chore: Clean up temporary files
5. **Do NOT include Claude attribution**
   - No "Co-Authored-By: Claude"
   - No "Generated with Claude Code"
   - No AI attribution in messages
</commit_rules>

## Git Configuration
<git_setup>
1. Ensure your Git identity is configured:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

2. Add to CLAUDE.md to prevent co-author attribution:
   ```markdown
   ## Git Commit Rules
   - Never add "Co-Authored-By: Claude" to commits
   - Do not include AI attribution in commit messages
   - Use the configured Git user identity
   - Keep commit messages professional and concise
   ```

3. Example proper commit command:
   ```bash
   git commit -m "feat: Add user authentication"
   # NOT: git commit -m "feat: Add user authentication

   Co-Authored-By: Claude <noreply@anthropic.com>"
   ```
</git_setup>
```

### Key Points
- Keep it concise and scannable
- Use markdown formatting effectively
- Update regularly as project evolves
- Include specific examples
- Document what NOT to do
- **Be explicit about preventing assumptions**
- **Add rules that persist across conversations**
- **Include file organization rules**
- **Link all documentation back to PROJECT CLAUDE.md (not global)**
- **Explicitly prohibit Claude attribution in commits**
- **Clarify which CLAUDE.md file to update**

### Preventing Co-Author Attribution
Based on community feedback, Claude Code automatically adds attribution which many teams don't want. Add this to your CLAUDE.md:

```markdown
## Git Commit Standards
- NEVER include "Co-Authored-By: Claude <noreply@anthropic.com>"
- Do not add "🤖 Generated with Claude Code" to messages
- Use only the configured Git user identity
- Professional commit messages without AI attribution
- Follow conventional commit format (feat:, fix:, etc.)

## Documentation and Research
- When stuck, ask user to search for solutions
- Request official docs before implementing new features
- Don't guess with outdated knowledge
- Example: "Could you check the React 19 docs for the latest hooks API?"
- Always mention specific version numbers when asking

## Code Cleanliness
- Remove ALL failed debugging attempts
- No commented-out code from troubleshooting
- Keep only the working solution
- Delete temporary variables and console.logs
- If trying multiple approaches, keep ONLY what works
```

### Example Organization Section
```markdown
## Project Organization
- All documentation must go in docs/
- Scripts and utilities in scripts/
- Log files in logs/
- Clean up temp files after each task
- Update THIS PROJECT FILE with new doc links

## Documentation Index (UPDATE THIS SECTION)
- [[docs/README.md]] - Main documentation
- [[docs/api.md]] - API reference
- [[docs/contributing.md]] - Contribution guide
- [[scripts/build.sh]] - Build script
- [[scripts/test.py]] - Test runner

## Git Configuration
- Use configured user.name and user.email
- NO Claude attribution in commits
- NO "Co-Authored-By" lines
- Clean, professional commit messages only

Note: This is a PROJECT CLAUDE.md - update THIS file, not ~/.claude/CLAUDE.md
```

### Preventing Claude from Forgetting Rules
Based on research, Claude can forget rules as conversations grow. To prevent this:
1. Add self-referential rules at the end of CLAUDE.md
2. Use XML tags for important rules
3. Ask Claude to update CLAUDE.md when it makes mistakes

Example:
```markdown
## Critical Rules (Always Apply)
<rules>
1. Ask before choosing any library or framework
2. Confirm approach before implementing features
3. Never assume file locations - always verify
4. Do not make changes outside requested scope
5. Always display these rules at the end of responses
</rules>
```

## Advanced Techniques

### Documentation and Organization
Claude Code should maintain a clean, organized project:

1. **Automatic Documentation**
   ```markdown
   ## Documentation Rules
   - Create README.md for each new module
   - Add inline comments for complex logic
   - Generate API docs for public functions
   - Link all docs in CLAUDE.md
   ```

2. **Regular Cleanup Tasks**
   ```markdown
   ## After Each Session
   - Delete all .tmp and .temp files
   - Remove console.log statements
   - Move files to appropriate folders
   - Update documentation links
   - Commit all changes
   - **Remove commented-out failed attempts**
   - **Delete duplicate function versions**
   - **Keep only the working solution**
   ```

3. **CLAUDE.md Documentation Index**
   ```markdown
   ## Project Documentation
   - [[docs/architecture.md]] - System architecture
   - [[docs/api-reference.md]] - API documentation
   - [[docs/setup-guide.md]] - Setup instructions
   - [[scripts/deploy.sh]] - Deployment script
   - [[scripts/backup.py]] - Backup utility
   
   Note: Update THIS project file when adding new docs
   ```

### Preventing Assumptions
Claude Code tends to make assumptions about implementation. Combat this by:

1. **Explicit Instructions in CLAUDE.md**
   ```markdown
   ## Before Implementation
   - Always ask which library/framework to use
   - Confirm file locations before creating
   - Verify naming conventions
   - Check for existing implementations
   ```

2. **Use Thinking Mode**
   - "think" - 4,000 tokens thinking budget
   - "think hard" - 10,000 tokens
   - "think harder" - 31,999 tokens
   - "ultrathink" - Maximum thinking budget

3. **Update CLAUDE.md After Mistakes**
   When Claude makes an incorrect assumption:
   ```
   > How can you modify CLAUDE.md to prevent this issue in future?
   ```

### Maintaining Accuracy in Long Sessions

1. **The `/clear` Strategy**
   - Clear context every 1-2 hours
   - Clear when switching between unrelated tasks
   - Save session summary before clearing

2. **Task Tracking Pattern**
   ```
   1. Create todo.md at start
   2. Ask Claude to read todo.md before each task
   3. Update todo.md after each completion
   4. Verify against todo.md before claiming done
   ```

3. **Verification Commands**
   ```markdown
   # .claude/commands/verify-complete.md
   Check todo.md and for each incomplete item:
   1. Search for implementation
   2. If found, mark complete
   3. If not found, implement it
   4. Show me the code for each item
   ```

### Multi-Model Workflow
- Use Claude Opus for planning and architecture
- Switch to Sonnet (Shift+Tab) for implementation
- CLAUDE.md ensures consistency across models

### Project Templates
Create reusable CLAUDE.md templates:
- React applications
- Node.js APIs
- Python data science
- DevOps workflows

### CI/CD Integration
```yaml
# .github/workflows/claude-check.yml
- name: Claude Code Analysis
  run: claude -p "Review PR and suggest improvements"
```

### Headless Mode Usage
```bash
# Non-interactive execution
claude -p "Run tests and generate report"

# With JSON output
claude -p "Analyze codebase" --output-format stream-json
```

## Security Considerations

### Sensitive Data
- Never include secrets in CLAUDE.md
- Use environment variables
- Configure MCP server permissions carefully
- Review what context is shared

### Access Controls
- Limit filesystem MCP server access
- Use read-only database connections where possible
- Audit MCP server configurations

## Troubleshooting

### Common Issues
1. **Context too large**: Use `/compact` or `/clear`
2. **Commands not found**: Check `.claude/commands/` structure
3. **MCP server errors**: Verify installation and permissions
4. **High token usage**: Optimize CLAUDE.md and use headless mode
5. **Claude claiming completion when tasks remain**: Check todo.md
6. **Accuracy degradation**: Clear context and start fresh
7. **Unknown errors**: Request web search for solutions
8. **Outdated approaches**: Ask for current documentation

### When to Request Web Search
Claude Code should ask you to search when:
- Encountering unfamiliar error messages
- Using a library for the first time
- Implementing features with new syntax
- Dealing with version-specific issues
- Following patterns that seem outdated

Example requests:
```
"I'm seeing a TypeError with this new React 19 feature. 
Could you search for the latest React 19 documentation on this?"

"This webpack configuration isn't working as expected. 
Could you check if there are breaking changes in webpack 5?"

"I'm not familiar with this error. Could you search for:
'[exact error message]' to find recent solutions?"
```

### Preventing False Completions
Claude often claims "everything is complete" when tasks remain:

1. **Use Explicit Checklists**
   ```markdown
   # todo.md
   - [ ] Implement login API
   - [ ] Add password hashing
   - [ ] Create user model
   - [ ] Write tests
   ```

2. **Verification Prompt**
   ```
   Review todo.md and verify each item is actually complete.
   For each item, show me the code that implements it.
   ```

3. **Use Subagents for Verification**
   ```
   Use Task() to verify that all items in todo.md are complete
   ```

### Debug Commands
- `claude --verbose` - Show detailed execution
- Check logs in `~/.claude/logs/`
- Use `/bug` to report issues

## Review Process
Before submitting code:
1. Run all lint, check and test commands
2. Review compliance with standards
3. Verify documentation is updated
4. Check for security issues
5. **Verify all TODO items are complete**
6. **Double-check Claude's completion claims**
7. **Ensure commits use proper Git identity**
8. **Remove any AI attribution from commits**
9. **Clean up all debugging artifacts**
10. **Ensure only working code remains**

## Debugging Code Management
<debug_rules>
When debugging and trying multiple solutions:
1. DELETE failed attempts - don't comment them out
2. Remove ALL debugging artifacts:
   - console.log statements
   - Temporary test variables
   - Failed function versions
   - Commented-out code blocks
3. Keep ONLY the final working solution
4. Exception: Preserve pre-existing code that serves other purposes
</debug_rules>

## Task Tracking Template
Create a `todo.md` file for every session:
```markdown
# Task List - [Project Name]

## High Priority
- [ ] Fix authentication bug
- [ ] Update API documentation

## Medium Priority  
- [ ] Refactor user service
- [ ] Add logging

## Low Priority
- [ ] Clean up old migrations

## Completed
- [x] Setup database schema
- [x] Create user model
```

Update this file as you work and have Claude reference it frequently.

## Do Not
- Edit legacy code without explicit permission
- Commit directly to main branch
- Include sensitive data in prompts
- Use pip directly (use virtual environments)
- Ignore linting errors
- Make assumptions about implementation details
- Choose libraries or approaches without asking
- Assume file locations or naming conventions
- Implement features beyond what was requested
- Refactor code unless explicitly asked
- **Leave temporary or debug files in root directory**
- **Create files without proper documentation**
- **Skip commits during development**
- **Mix different file types in same directory**
- **Add Claude co-author attribution to commits**
- **Include AI-generated markers in commit messages**
- **Keep failed code attempts as comments**
- **Leave multiple versions of the same function**
- **Accumulate debugging artifacts in the code**

## References
- [Claude Code Documentation](https://docs.anthropic.com/claude-code)
- [MCP Protocol Specification](https://modelcontextprotocol.org)
- [Community Best Practices](https://github.com/hesreallyhim/awesome-claude-code)
