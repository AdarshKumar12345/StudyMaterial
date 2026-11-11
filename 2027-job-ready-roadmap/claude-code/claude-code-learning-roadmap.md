# Claude Code – Complete Learning Roadmap

**Goal:** Learn Claude Code as an AI coding agent: understand how it works, guide it with project context, control its tools safely, verify its changes, and automate repeatable development workflows.

> Claude Code features, model names, flags, and command behavior can change. Check the current official documentation before relying on a specific option.

---

## 1. Claude Code Fundamentals

### Setup and workflow
- [ ] What is Claude Code?
- [ ] Claude Code vs chat-based AI assistants
- [ ] Install and authenticate Claude Code
- [ ] Use Claude Code in the terminal
- [ ] Understand the interactive CLI workflow
- [ ] Start Claude Code in the correct project directory
- [ ] Understand sessions and conversation history
- [ ] Use Claude Code with VS Code / supported IDE integrations
- [ ] Understand the difference between Claude Code and the Claude API

### Agent architecture
- [ ] What is an AI coding agent?
- [ ] Understand the agent loop: observe → decide → use tool → inspect result → repeat
- [ ] Model reasoning vs tool execution
- [ ] Built-in tools: read, search, edit, write, and terminal commands
- [ ] How tool results become part of the next model context
- [ ] Tool errors and recovery
- [ ] Human-in-the-loop approval
- [ ] Limits of autonomous coding agents

### Models and task selection
- [ ] Understand model quality, speed, and cost trade-offs
- [ ] Choose a stronger reasoning model for complex architecture or debugging
- [ ] Choose a faster model for small edits, explanations, and routine tasks
- [ ] Use the currently available model selector and model aliases
- [ ] Understand that model availability depends on plan, settings, and current product support
- [ ] Know when to split a large task into smaller tasks

## 2. Tools, Permissions, and Safety

### Tool usage
- [ ] Understand file-reading and code-search tools
- [ ] Understand file-editing and file-creation tools
- [ ] Understand terminal / Bash tool usage
- [ ] Understand tool arguments and tool output
- [ ] Learn how Claude chooses tools for a task
- [ ] Understand tool failures and retries

### Permissions
- [ ] Understand permission prompts
- [ ] Review file edits before accepting them
- [ ] Review terminal commands before running them
- [ ] Understand permission modes available in the current version
- [ ] Configure allow / ask / deny rules
- [ ] Use `/permissions` to inspect permission settings
- [ ] Understand project and user settings
- [ ] Avoid broad permission grants without a clear reason
- [ ] Understand sandboxing and its limits
- [ ] Know why skipping all permission checks is risky

### Practical tasks
- [ ] Ask Claude to inspect a repository without editing it
- [ ] Ask Claude to explain a command before approving it
- [ ] Configure a narrow allowlist for safe development commands
- [ ] Deny access to sensitive files and destructive commands where possible

## 3. Context Engineering

### CLAUDE.md
- [ ] What is `CLAUDE.md`?
- [ ] User-level instructions
- [ ] Project-level instructions
- [ ] Nested / folder-level instructions
- [ ] How instruction files are discovered and loaded
- [ ] Import other instruction files where supported
- [ ] Keep instructions concise and actionable
- [ ] Add project architecture and folder structure
- [ ] Add install, build, lint, and test commands
- [ ] Document coding style and naming conventions
- [ ] Document important constraints and things to avoid
- [ ] Keep personal preferences separate from team conventions
- [ ] Review and update instructions as the codebase changes
- [ ] Use `/init` to create a starting project instruction file, then review it

### Rules and scoped instructions
- [ ] Understand `.claude/rules/`
- [ ] Use path-scoped rules for specific folders or file types
- [ ] Separate always-relevant facts from task-specific procedures
- [ ] Avoid putting long runbooks into `CLAUDE.md`
- [ ] Know when a skill is better than a permanent instruction

### Context quality
- [ ] Give Claude the goal, constraints, and acceptance criteria
- [ ] Provide relevant files, error messages, and reproduction steps
- [ ] Avoid dumping unrelated code into the conversation
- [ ] Ask Claude to inspect the codebase before proposing changes
- [ ] Keep source-of-truth documentation clear
- [ ] Understand context-window limits and token usage

## 4. Plan Mode and Task Planning

- [ ] What is Plan Mode?
- [ ] Use read-only exploration before implementation
- [ ] Ask Claude to inspect architecture and dependencies
- [ ] Ask it to identify files that need changes
- [ ] Ask it to identify risks and unknowns
- [ ] Review the proposed implementation plan
- [ ] Correct assumptions and refine the plan
- [ ] Define acceptance criteria and test cases
- [ ] Approve the plan before implementation
- [ ] Use planning for large or risky tasks
- [ ] Use direct execution for small, clear changes
- [ ] Verify that the implementation follows the approved plan

### Practice
- [ ] Ask Claude to plan a new API endpoint without editing files
- [ ] Review the plan for security, error handling, and tests
- [ ] Approve the plan and let Claude implement it
- [ ] Compare the final diff with the original plan

## 5. Context Management and Checkpoints

- [ ] Understand context-window usage
- [ ] Use `/context` to inspect context usage
- [ ] Understand automatic context compaction
- [ ] Use `/compact` when a long session needs a concise summary
- [ ] Use `/clear` when starting an unrelated task
- [ ] Use `/rewind` to return to an earlier checkpoint when supported
- [ ] Understand what checkpoints can and cannot restore
- [ ] Use Git commits before large or risky changes
- [ ] Use branches for isolated work
- [ ] Keep important decisions in project documentation
- [ ] Start a fresh session with a concise handoff when appropriate

> Checkpoints are not a replacement for Git. Use commits and branches for durable version control, and inspect the working tree before and after a rewind.

## 6. Verification Loops

- [ ] What is a verification loop?
- [ ] Define success criteria before implementation
- [ ] Ask Claude to run relevant unit tests
- [ ] Run integration and end-to-end tests
- [ ] Run linting and formatting
- [ ] Run type checks
- [ ] Run production builds
- [ ] Verify API behavior with requests or Postman
- [ ] Verify UI changes in a browser
- [ ] Provide screenshots or error output when useful
- [ ] Ask Claude to inspect test failures and fix the root cause
- [ ] Repeat verification after each meaningful fix
- [ ] Review the final diff yourself
- [ ] Require a summary of tests run and tests not run

### Example task instruction
```text
Implement the requested feature.

Before finishing:
1. Run the relevant tests.
2. Run linting and type checks.
3. Run the production build if applicable.
4. Fix failures caused by your changes.
5. Review the final diff for regressions.
6. Report exactly what passed and what remains unverified.
Do not claim success for checks you did not run.
```

## 7. Subagents and Parallel Work

### Subagent fundamentals
- [ ] What are subagents?
- [ ] Main agent vs subagent
- [ ] Separate context windows
- [ ] Tool restrictions for each subagent
- [ ] Model selection for subagents
- [ ] When to delegate vs keep work in the main session
- [ ] Understand subagent results and limitations
- [ ] Avoid multiple agents editing the same files simultaneously

### Useful specialist agents
- [ ] Codebase explorer / researcher
- [ ] Code reviewer
- [ ] Test writer
- [ ] Security auditor
- [ ] Performance reviewer
- [ ] Documentation writer

### Parallel development
- [ ] Delegate independent research tasks
- [ ] Split work by clearly separated files or modules
- [ ] Understand parallel sessions
- [ ] Use Git worktrees for isolated working directories
- [ ] Review and integrate agent changes
- [ ] Resolve merge conflicts
- [ ] Run the full test suite after integration

## 8. Hooks and Guardrails

### Hook fundamentals
- [ ] What are Claude Code hooks?
- [ ] Understand hook events and lifecycle
- [ ] Configure hooks in settings
- [ ] Understand matchers and hook commands
- [ ] Understand hook exit codes and blocking behavior
- [ ] Keep hook scripts reliable and fast
- [ ] Test hooks before using them in a team workflow

### Important use cases
- [ ] `PreToolUse`: inspect or block a tool action before it runs
- [ ] `PostToolUse`: format or lint after relevant edits
- [ ] `SessionStart`: load useful context or prepare the environment
- [ ] `Stop`: run deterministic completion checks
- [ ] `Notification`: notify when user input is needed
- [ ] `PreCompact` / `PostCompact`: preserve or restore important context around compaction where appropriate
- [ ] Block dangerous commands or edits to protected paths
- [ ] Run tests before a task is considered complete

### Important distinction
- [ ] Use `CLAUDE.md` for instructions and conventions
- [ ] Use hooks for deterministic actions and guardrails
- [ ] Use permissions for tool access control
- [ ] Do not rely on natural-language instructions alone for critical safety rules

## 9. MCP – Model Context Protocol

### Fundamentals
- [ ] What is MCP?
- [ ] MCP client, server, and tool concepts
- [ ] How Claude Code connects to MCP servers
- [ ] MCP tools vs built-in tools
- [ ] MCP resources and prompts
- [ ] Local vs remote MCP servers
- [ ] Understand authentication and environment variables
- [ ] Inspect connected servers and tools

### Using MCP
- [ ] Connect GitHub
- [ ] Connect issue trackers such as Jira or Linear
- [ ] Connect databases safely
- [ ] Connect observability tools such as Sentry
- [ ] Understand the permissions exposed by each server
- [ ] Limit access to the minimum required
- [ ] Avoid exposing production secrets or unrestricted database access

### Building an MCP server
- [ ] Understand the MCP specification and SDK
- [ ] Build a small custom MCP server
- [ ] Expose a read-only tool first
- [ ] Validate tool inputs
- [ ] Handle errors and timeouts
- [ ] Add authentication where needed
- [ ] Connect the server to Claude Code
- [ ] Test the tool and inspect its output

## 10. Skills, Slash Commands, and Plugins

### Skills
- [ ] What are Claude Code skills?
- [ ] Create `.claude/skills/<skill-name>/SKILL.md`
- [ ] Understand skill descriptions and invocation
- [ ] Add supporting files and scripts
- [ ] Create reusable workflows for code review, testing, and releases
- [ ] Understand when a skill should be invoked automatically or manually
- [ ] Keep skills focused and maintainable

### Commands and plugins
- [ ] Understand slash-command workflows
- [ ] Know that skills are the preferred modern format for reusable workflows; legacy command files may still be supported
- [ ] Understand plugin structure
- [ ] Package skills, agents, hooks, and related configuration as plugins where supported
- [ ] Install and manage plugins
- [ ] Review plugin permissions and source before installation
- [ ] Share team workflows consistently

### Practical skills to build
- [ ] `/review` – review a diff for bugs and maintainability
- [ ] `/test` – identify and run relevant tests
- [ ] `/debug` – investigate an error and prove the fix
- [ ] `/security-audit` – inspect common security risks
- [ ] `/release-notes` – summarize changes for a release

## 11. Headless Claude Code and CI/CD

- [ ] What is headless / non-interactive mode?
- [ ] Understand `claude -p` usage
- [ ] Provide prompts through scripts and pipelines
- [ ] Capture output and exit status
- [ ] Configure authentication securely in CI
- [ ] Use GitHub Actions
- [ ] Review pull requests with Claude Code
- [ ] Summarize changed files and potential risks
- [ ] Diagnose failing builds
- [ ] Generate release notes
- [ ] Control token and runtime budgets
- [ ] Restrict CI permissions and secrets
- [ ] Require human review before merge or deployment
- [ ] Save logs and artifacts for debugging

## 12. Debugging with Claude Code

- [ ] Provide a clear bug description
- [ ] Share stack traces and relevant logs
- [ ] Reproduce the issue before editing
- [ ] Ask Claude to trace the execution path
- [ ] Identify root cause rather than masking symptoms
- [ ] Inspect related code and recent changes
- [ ] Use debugger output and targeted logging
- [ ] Provide screenshots for UI bugs
- [ ] Create a regression test
- [ ] Apply the smallest safe fix
- [ ] Run tests and verify the original reproduction
- [ ] Review the diff for unintended changes

### Debugging prompt template
```text
Investigate this issue before changing code.

1. Reproduce the problem.
2. Trace the execution path.
3. Identify the root cause and explain the evidence.
4. Propose the smallest safe fix.
5. Add a regression test that fails before the fix.
6. Implement the fix.
7. Run the relevant tests and report the results.
Do not hide the error or weaken existing tests to make them pass.
```

## 13. Security and Cost Management

### Security
- [ ] Understand permission modes and approval prompts
- [ ] Use least-privilege tool permissions
- [ ] Keep API keys and secrets out of prompts and context
- [ ] Use environment variables or a secret manager
- [ ] Review untrusted repository files and prompt-injection risks
- [ ] Inspect commands before execution
- [ ] Review AI-generated code for injection vulnerabilities
- [ ] Check authentication and authorization logic
- [ ] Review dependency changes
- [ ] Run tests and security scanners
- [ ] Protect production data and credentials
- [ ] Use sandboxing where appropriate
- [ ] Require human review for sensitive operations

### Cost and context efficiency
- [ ] Understand token usage and context limits
- [ ] Use `/context` to inspect context consumption
- [ ] Use `/compact` to summarize long sessions
- [ ] Use `/clear` between unrelated tasks
- [ ] Select models according to task complexity and current availability
- [ ] Avoid loading irrelevant files and documentation
- [ ] Delegate independent work only when parallelism is useful
- [ ] Limit unnecessary MCP tools and large tool outputs
- [ ] Set reasonable limits for automated CI tasks
- [ ] Review usage and billing in the relevant account dashboard

## 14. Practical Projects

### Project 1 – Understand an existing codebase
- [ ] Ask Claude to map the project architecture
- [ ] Identify entry points, APIs, database models, and tests
- [ ] Create or refine `CLAUDE.md`
- [ ] Ask for a read-only implementation plan
- [ ] Make one small change and verify it

### Project 2 – Build a feature end to end
- [ ] Plan a feature before editing
- [ ] Implement backend and frontend changes
- [ ] Add tests and validation
- [ ] Use a code-review subagent
- [ ] Run lint, tests, and build
- [ ] Review the final diff

### Project 3 – Create a team coding workflow
- [ ] Create project instructions and scoped rules
- [ ] Add review and test skills
- [ ] Configure formatting and test hooks
- [ ] Add safe permission rules
- [ ] Document the workflow for teammates

### Project 4 – MCP integration
- [ ] Connect a development tool such as GitHub
- [ ] Read issue or pull-request information
- [ ] Build a small custom MCP server
- [ ] Restrict tool access and validate inputs

### Project 5 – CI/CD automation
- [ ] Run Claude Code non-interactively in GitHub Actions
- [ ] Summarize pull-request changes
- [ ] Run a controlled review workflow
- [ ] Capture output and exit codes
- [ ] Require human approval before sensitive actions

## 15. Recommended Learning Priority

| Priority | Topics |
|---|---|
| P0 – Core | Fundamentals, agent loop, tools, permissions, CLAUDE.md |
| P0 – Core | Plan Mode, context management, verification loops |
| P1 – Practical | Subagents, Git worktrees, hooks, debugging |
| P1 – Practical | Skills, MCP setup, project workflows |
| P2 – Advanced | Custom MCP servers, plugins, headless mode, CI/CD |
| P2 – Advanced | Security hardening, cost optimization, team-wide workflows |

## 16. Quick Revision

| Concept | Meaning |
|---|---|
| Agent loop | Repeated reasoning, tool use, observation, and next action |
| Tool | Capability Claude can invoke, such as reading files or running commands |
| Permission | Controls whether a tool action is allowed, denied, or requires approval |
| `CLAUDE.md` | Persistent project or user instructions |
| Plan Mode | Read-only exploration and planning before implementation |
| `/context` | Inspect context-window usage |
| `/compact` | Summarize the conversation to free context |
| `/clear` | Start a fresh conversation context |
| `/rewind` | Return to an earlier conversation / code checkpoint when supported |
| Subagent | Specialist agent with its own task context and tool access |
| Hook | Automation or guardrail triggered by a lifecycle event |
| MCP | Standard protocol for connecting external tools and data |
| Skill | Reusable workflow instructions and supporting resources |
| Plugin | Packaged extension that can distribute Claude Code capabilities |
| Headless mode | Run Claude Code non-interactively from scripts or CI |
| Verification loop | Run checks, inspect failures, fix, and verify again |

## Official Documentation and Resources

- Claude Code documentation: https://code.claude.com/docs
- Claude Code best practices: https://code.claude.com/docs/en/best-practices
- Memory / CLAUDE.md: https://code.claude.com/docs/en/memory
- Settings and permissions: https://code.claude.com/docs/en/permissions
- Hooks: https://code.claude.com/docs/en/hooks
- Subagents: https://code.claude.com/docs/en/sub-agents
- MCP: https://code.claude.com/docs/en/mcp
- Skills: https://code.claude.com/docs/en/skills
- Plugins: https://code.claude.com/docs/en/plugins
- GitHub Actions: https://code.claude.com/docs/en/github-actions
- Headless mode: https://code.claude.com/docs/en/headless
- Checkpointing: https://code.claude.com/docs/en/checkpointing

---

## Completion Checklist

You have a solid practical foundation when you can:

- [ ] Explain the agent loop and how tools are used.
- [ ] Configure user, project, and folder-level context appropriately.
- [ ] Plan a change before implementation.
- [ ] Manage context and recover from a wrong turn.
- [ ] Make Claude verify its own changes with real tests and builds.
- [ ] Delegate independent tasks to subagents safely.
- [ ] Use hooks and permissions as guardrails.
- [ ] Connect external tools through MCP.
- [ ] Create reusable skills for repeated workflows.
- [ ] Run controlled Claude Code tasks in CI/CD.
- [ ] Debug an issue and add a regression test.
- [ ] Review security, permissions, and usage before shipping.
