```
SESSION REFERENCE
│
├── BEFORE YOU CODE
│   ├── Ambiguous task?         → Clarify requirements FIRST, not mid-task
│   ├── Task > 30 min?          → Write a plan, get approval, then execute
│   ├── Unfamiliar codebase?    → Read CLAUDE.md / README before touching anything
│   └── New large repo?         → codebase-memory-mcp first (see CODEBASE section)
│
├── MONITORING
│   └── Always run              → ccusage --live (track burn rate throughout session)
│
├── SHELL COMMANDS
│   └── git, cargo, docker,     → rtk [wraps every command, 60-90% output reduction]
│       npm, make, etc.            rtk git log / rtk cargo build / rtk docker ps
│
├── CODE SEARCH  (pick by question type)
│   ├── "Where is file X?"      → fd (fast filename search)
│   │                              fd auth.ts / fd --ext py "models"
│   ├── "Where is this string?" → rg (ripgrep — text/regex pattern search)
│   │                              rg "createUser" / rg -t ts "export default"
│   ├── "What calls this        → ast-grep (AST-aware, language-specific)
│   │    function / class?"        sg --pattern 'console.log($A)' / sg -l python
│   └── "What does this         → serena-slim ← BEST for large codebases
│         symbol mean / where       Semantic: types, references, call graphs,
│         is it used?"              cross-file jumps. Use when rg gives too many
│                                   false positives or you need type context.
│
├── CODEBASE UNDERSTANDING  (ordered by situation)
│   ├── First time in large     → 1. codebase-memory-mcp  (build knowledge graph,
│   │   repo (>50 files)             persists across sessions, 120x fewer tokens)
│   │                              2. Then use serena-slim for deep symbol queries
│   ├── Targeted question in    → Skip knowledge graph, go straight to rg + serena
│   │   known codebase
│   ├── Already read files,     → GrapeRoot (tracks visited files, prevents
│   │   context getting fat         re-reading already-seen code)
│   └── One-shot full-repo      → Repomix (pack + compress ~70%, good for
│       analysis / handoff          "explain this whole codebase" prompts)
│
├── EDITING STRATEGY  (pick by change size)
│   ├── Small targeted change   → str_replace_editor (surgical, no rewrite risk)
│   │   (1-20 lines, known loc)
│   ├── Refactor / restructure  → Read file first → str_replace for each chunk
│   │   (multiple locations)        Avoid full rewrites; preserves git history
│   └── New file from scratch   → Full write. No str_replace needed.
│       Rule of thumb: if you can describe the exact old string, use str_replace.
│       If you're rewriting >40% of a file, full overwrite is cleaner.
│
├── EXTERNAL KNOWLEDGE
│   ├── Library / framework     → Context7 MCP (on-demand docs, ~100 tokens)
│   │   docs                       "use context7" in prompt, specify library + version
│   ├── JSON / YAML config      → jq / yq  (parse, don't cat + eyeball)
│   │                              cat config.yaml | yq '.services.api.port'
│   └── Runtime error /         → Search error message BEFORE re-reading source.
│       stack trace                 rg the error string, check GH issues via web.
│                                   Re-reading source is the most common token trap.
│
├── TESTING
│   ├── Running tests           → rtk [test runner] to suppress verbose output
│   ├── Test fails              → Read failure message fully before touching code
│   ├── Flaky test?             → Run 3x before debugging. Flakiness ≠ your bug.
│   └── After any edit          → Run affected tests immediately, not at the end
│
├── PR / DIFF WORKFLOWS
│   ├── Reviewing a diff        → rtk git diff main..branch (not full file reads)
│   ├── Writing commit msg      → rtk git diff --staged → summarise what changed
│   └── Before opening PR       → rg for leftover TODOs / console.logs / debug flags
│
├── MCP SELECTION  (match tool to task, not habit)
│   ├── Docs lookup             → Context7 MCP
│   ├── Large codebase nav      → codebase-memory-mcp
│   ├── File visit tracking     → GrapeRoot
│   ├── Repo packing            → Repomix
│   └── MCP overhead rule:
│       < 3 active MCP servers  → Native Claude MCP Tool Search
│       3+ active MCP servers   → Bifrost Code Mode (50%+ token reduction)
│
└── LONG / COMPLEX SESSIONS
    ├── Before starting         → Decompose task into checkpoints. Commit after each.
    ├── Context swelling?       → /compact  (summarise) or /rewind (drop recent turns)
    ├── Mid-task spiral?        → STOP. Re-read the original requirement.
    │                              Most spirals come from drifting, not hard problems.
    ├── Hit a wall (>3 attempts → Abandon and restart with a clean description of
    │   same approach fails)?      what you learned. Sunk cost kills sessions.
    └── Skills > CLAUDE.md      → Load workflows on-demand. Don't always-on load
                                   heavy context you might not need this session.



```
