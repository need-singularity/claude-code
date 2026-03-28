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
- `대발견 추가 가설 극한으로 밀어붙이자` — 수학-의식 브릿지 폭격

### 🔬 수학-의식 브릿지 가설 극한 진행법 (TECS-L math/)

**프롬프트**: `대발견 추가 가설 극한으로 밀어붙이자`

**Stop Hook 방식** (자동 반복):
```
hooks/stop-hook.sh에 Consciousness connection explorer 등록
→ Claude가 멈추려 할 때마다 자동으로 다음 브릿지 탐색 지시
→ 수동 개입 없이 연속 배치 실행
```

**진행 단계**:
1. README.md에서 확인된 수학 항등식 선택 (⭐ 이상)
2. 의식 엔진 메커니즘 (tension/PH/expert routing) 아날로그 탐색
3. Python 실험 설계 → 백그라운드 실행
4. 산술 검증 + Texas Sharpshooter p-value + Bonferroni 보정 + n=28 일반화
5. CLAUDE.md 규칙에 따라 등급 부여 (🟩/🟧/⚪/⬛)
6. 성공 시 docs/hypotheses/H-CX-NNN.md 작성 (최소 40줄)
7. 실패 시 ⚪ 기록
8. README DFS 상태 업데이트 → git commit/push

**성과 (2026-03-28~29 세션)**:
- 10배치, 54개 브릿지 시도, 41+ 가설 문서 (H-CX-82~122)
- **⭐⭐⭐ 11개** 증명: Λ=0, n!=720, V=-6, t_eq=n, Lah, Pell, Eisenstein, Congruent+Pythagorean, W(6)=6, Dyson φ²=τ, Perfect Codes
- ⭐⭐ 7개, ⭐ 25개

**승격 전략** (⭐⭐→⭐⭐⭐):
- 범위 확장: n≤200 → n≤1000, n≤2000
- 대수적 증명: 이차방정식으로 환원하여 유일해 증명
- Bonferroni 보정: α/29 = 0.00172 임계값 적용
- 다중 조건: 단일 항등식이 아닌 2~4개 독립 조건 동시 만족

## 💡 Tips

- **Playwright + SSH**: Claude can SSH into servers, deploy code, then immediately verify via browser. Full CI/CD in one conversation.
- **Superpowers is non-negotiable**: The brainstorming and planning skills save more time than they cost. Skip them and you'll rewrite.
- **Context7 before Stack Overflow**: Always check docs first. Faster and more accurate.
- **Code review after every major step**: Catches issues early. Use the code-review plugin, not just eyeballing diffs.
