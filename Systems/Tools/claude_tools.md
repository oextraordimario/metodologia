# Claude Code Tools

This document lists all the tools available to Claude Code and their descriptions.

## File Operations

### Read
Reads files from the local filesystem.
- Can read any file with absolute path
- Reads up to 2000 lines by default
- Supports optional line offset and limit for long files
- Lines longer than 2000 characters are truncated
- Can read images (PNG, JPG, etc.) visually
- Can read PDF files page by page
- Can read Jupyter notebooks (.ipynb) with outputs
- Returns results in cat -n format with line numbers

### Write
Writes content to a file, creating or overwriting it.
- Overwrites existing files
- Must use Read tool first before overwriting existing files
- Requires absolute paths (not relative)
- Should prefer editing over creating new files

### Edit
Performs exact string replacements in files.
- Must use Read tool at least once before editing
- Must preserve exact indentation from file
- Requires unique old_string or use replace_all parameter
- Use replace_all for renaming variables throughout a file
- Fails if old_string is not unique (unless replace_all is true)

### NotebookEdit
Edits Jupyter notebook cells (.ipynb files).
- Can replace, insert, or delete cells
- Cells are 0-indexed
- Supports both code and markdown cells
- Must specify absolute path

## Search & Discovery

### Glob
Fast file pattern matching tool.
- Works with any codebase size
- Supports glob patterns (e.g., "**/*.js", "src/**/*.ts")
- Returns matching file paths sorted by modification time
- Use for finding files by name patterns

### Grep
Powerful search tool built on ripgrep.
- Supports full regex syntax
- Filter files with glob or type parameter
- Output modes: content (lines), files_with_matches (paths), count (counts)
- Supports context lines (-A, -B, -C)
- Case insensitive option (-i)
- Multiline matching support
- Can limit results with head_limit and offset

## Command Execution

### Bash
Executes bash commands in a persistent shell session.
- For terminal operations (git, npm, docker, etc.)
- NOT for file operations - use specialized tools instead
- Supports optional timeout (up to 10 minutes)
- Can run in background with run_in_background parameter
- Quote file paths with spaces
- Use && to chain dependent commands
- Use parallel calls for independent commands

### BashOutput
Retrieves output from background bash shells.
- Takes bash_id to identify the shell
- Returns only new output since last check
- Supports optional regex filtering
- Use to monitor long-running processes

### KillShell
Terminates running background bash shells.
- Takes shell_id parameter
- Returns success or failure status
- Use when needing to stop long-running processes

## Web Operations

### WebSearch
Searches the web and returns current information.
- Provides up-to-date information beyond knowledge cutoff
- Supports domain filtering (allowed/blocked domains)
- Returns search results with links as markdown
- Must include "Sources:" section with URLs in responses
- Only available in the US

### WebFetch
Fetches and processes web content.
- Takes URL and prompt as input
- Converts HTML to markdown
- Processes content with AI model
- Read-only operation
- 15-minute cache for repeated access
- Handles redirects with notification

## Agent & Task Management

### Task
Launches specialized agents for complex, multi-step tasks.
- **general-purpose**: Research, code search, multi-step tasks
- **statusline-setup**: Configure status line settings
- **Explore**: Fast codebase exploration (quick/medium/very thorough)
- **Plan**: Software architect for implementation planning
- **claude-code-guide**: Help with Claude Code, Agent SDK, and Claude API

Features:
- Can run agents in background
- Supports resuming previous agents
- Launch multiple agents concurrently for performance
- Agents have access to conversation history
- Returns agent ID for later resumption

### AgentOutputTool
Retrieves output from async agent tasks.
- Takes agentId parameter
- Can block until results ready or return immediately
- Maximum wait time up to 300 seconds
- Use block=false to check progress without waiting

### EnterPlanMode
Transitions into plan mode for implementation planning.
- Use before non-trivial implementation tasks
- Allows exploration and design before coding
- Good for new features, architectural decisions, multi-file changes
- Requires user approval
- Skip for simple single-line fixes

### ExitPlanMode
Exits plan mode after planning is complete.
- Use after writing plan to plan file
- Signals readiness for user approval
- Only for tasks requiring implementation planning
- Not for research tasks

## Task Management

### TodoWrite
Creates and manages structured task lists.
- Track progress on complex tasks
- Task states: pending, in_progress, completed
- Must provide both content and activeForm for tasks
- Keep exactly ONE task in_progress at a time
- Update status in real-time
- Remove irrelevant tasks
- Only mark completed when fully done

## User Interaction

### AskUserQuestion
Asks users questions during execution.
- Gather preferences and requirements
- Clarify ambiguous instructions
- Get decisions on implementation choices
- Support for 1-4 questions at once
- Users can select "Other" for custom input
- Supports single or multi-select options
- Include header (max 12 chars) and descriptions

## Extensibility

### Skill
Executes specialized skills within conversation.
- Must invoke immediately when relevant
- Use skill name only (no arguments)
- Only use skills listed in available_skills
- Do not invoke if already running

### SlashCommand
Executes custom slash commands.
- Only for custom commands in Available Commands list
- Not for built-in CLI commands
- Execute sequentially when multiple requested
- Do not invoke if already running
- Commands expand to prompts from .claude/commands/

## Usage Best Practices

1. **Parallel Execution**: Call multiple independent tools in a single message for optimal performance
2. **Use Specialized Tools**: Prefer dedicated tools over bash commands (Read vs cat, Edit vs sed)
3. **Never Guess**: Don't use placeholders or guess parameters - wait for dependencies
4. **Read Before Edit**: Always read files before modifying them
5. **Plan Complex Tasks**: Use TodoWrite and EnterPlanMode for non-trivial work
6. **Search Efficiently**: Use Task with Explore agent for open-ended codebase exploration
7. **Complete Tasks**: Finish all work fully - no partial completions
8. **Minimize Changes**: Only make requested changes, avoid over-engineering
