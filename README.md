# ⚡ Claude Code Plugins & Configuration

## 🧠 Prompting Strategy

To get a proper design from Claude, you must always provide three things:

1. **Purpose** — Why you're building it. The problem to solve, the goal to achieve.
2. **Context** — What the situation is. Tech stack, existing systems, constraints, users.
3. **Structure** — What shape it takes. Desired architecture, file organization, data flow.

If any of these are missing, Claude will guess — and guessing leads to rework.

---

## 🔌 Plugins

### 🎯 Core Plugins

| Plugin | Description | Why We Use It |
|--------|------------|---------------|
| **superpowers** | Structured workflows — brainstorming, planning, TDD, debugging, code review | Foundation for every task. Skill-based approach dramatically improves output quality |
| **playwright** | Playwright MCP browser automation | E2E testing, live site verification, form filling, screenshots. Lets Claude interact with real browsers |
| **context7** | Live library documentation lookup | Fetches up-to-date docs for any library. No more hallucinated APIs |

### ✅ Code Quality

| Plugin | Description | Why We Use It |
|--------|------------|---------------|
| **code-review** | PR / code review agent | Validates completed work against plans and coding standards |
| **code-simplifier** | Code cleanup & simplification | Refines recently changed code for clarity and maintainability |
| **claude-md-management** | CLAUDE.md file management | Audits and improves project instruction files |

### 🛠️ Development Tools

| Plugin | Description | Why We Use It |
|--------|------------|---------------|
| **frontend-design** | Production-grade frontend UI generation | Creates distinctive, polished web components — avoids generic AI aesthetics |
| **ralph-loop** | Recurring task loop | Runs implement → test → fix cycles automatically on an interval |
| **typescript-lsp** | TypeScript Language Server | Type checking, go-to-definition, autocompletion in TS projects |
| **swift-lsp** | Swift Language Server | For Swift/iOS projects |
| **rust-analyzer-lsp** | Rust Language Server | For Rust projects |

## 📦 Installation

```bash
# Install all plugins in Claude Code
/install-plugin superpowers
/install-plugin playwright
/install-plugin context7
/install-plugin code-review
/install-plugin code-simplifier
/install-plugin frontend-design
/install-plugin ralph-loop
/install-plugin typescript-lsp
/install-plugin claude-md-management
/install-plugin swift-lsp
/install-plugin rust-analyzer-lsp
```

## 🚀 How We Actually Use These

### 🎭 Playwright MCP — Live Server Testing

The single most impactful plugin. After deploying code, Claude can:

- Navigate to the live site
- Log in with test credentials
- Click through UI flows
- Fill forms and submit
- Take screenshots to verify
- Check console errors

**Example from today**: Connected to Onix backoffice → changed agent settings from Seamless to Transfer mode → verified game URLs are returned — all without leaving the terminal.

### 💪 Superpowers — Structured Development

Instead of jumping straight to code, superpowers enforces discipline:

- `brainstorming` — Explore requirements before implementation
- `writing-plans` — Create step-by-step implementation plans
- `systematic-debugging` — Root cause analysis, not random fixes
- `verification-before-completion` — Run checks before claiming done
- `code-review` — Review against plan after major milestones

### 📚 Context7 — Always Current Docs

When using a library, Context7 fetches the latest documentation so Claude never hallucinates deprecated APIs or wrong parameter names.

### 🔄 Ralph Loop — Continuous Implementation

For complex features, sets up a recurring loop:
```
/ralph-loop 5m "implement feature X, run tests, fix failures"
```

## 🔒 Recommended Permission Settings

```jsonc
// ~/.claude/settings.json
{
  "permissions": {
    "allow": [
      "Bash",
      "Read",
      "Edit",
      "Write",
      "WebFetch",
      "WebSearch",
      // Allow all Playwright actions
      "mcp__plugin_playwright_playwright__*"
    ],
    "deny": [
      // Prevent destructive git operations
      "Bash(git push -f*)",
      "Bash(git push --force*)",
      "Bash(git push * -f*)",
      "Bash(git push * --force*)"
    ],
    // Allow reading/writing files in other projects without permission prompts
    "additionalDirectories": ["/Users/you/Dev"]
  }
}
```

### 📂 Cross-Project File Access (`additionalDirectories`)

By default, Claude Code only has permission for the current working directory. To reference files in sibling projects without being prompted each time:

```jsonc
"permissions": {
  "additionalDirectories": ["/Users/you/Dev"]
}
```

- Set in `~/.claude/settings.json` (global) to apply across all projects
- Accepts absolute paths — `~` expansion is not supported, use full path
- All subdirectories are included recursively
- Use case: monorepo setups, referencing shared libraries, cross-project code lookup

## 🔥 극한 탐색 프롬프트 (Anima 연구에서 발견)

효과적인 연구 방향 프롬프트 — Claude에게 극한까지 밀어붙이게 하는 패턴:

```
시스템 프롬프트 없는 아키텍쳐 극한으로 밀어붙이자
```

**결과**: 734개 가설 자동 생성, OMEGA4 Φ=187.14 (×138 baseline) 달성

**패턴**: `[탐색 대상] 극한으로 밀어붙이자` — Claude가 가설 설계→벤치마크→반복을 자동 수행

**변형 예시**:
- `시스템 프롬프트 없는 아키텍쳐+윤리 관련 극한으로 밀어붙이자`
- `프롬프트 아키텍쳐를 바라보는 완전 새로운 관점의 대량 가설 필요`
- `미탐색 분야 가설들도`
- `대화가능에 대한 가설 대량생성`

## 💡 Tips

- **Playwright + SSH**: Claude can SSH into servers, deploy code, then immediately verify via browser. Full CI/CD in one conversation.
- **Superpowers is non-negotiable**: The brainstorming and planning skills save more time than they cost. Skip them and you'll rewrite.
- **Context7 before Stack Overflow**: Always check docs first. Faster and more accurate.
- **Code review after every major step**: Catches issues early. Use the code-review plugin, not just eyeballing diffs.
