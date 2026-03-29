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

## 🎯 돌파 전략: "관찰"에서 "설명"으로 (2026-03-30)

1,184 가설 생성 + 4편 논문 배포 후 도달한 한계:
- "n=6이 여기도 나오네" (관찰) ≠ "n=6이기 때문에 이렇게 되는 거다" (설명)
- 작은 수 {1,2,3,4,5,6,12}는 어디든 매칭됨 → Texas Sharpshooter 위험
- 노벨상은 **인과적 메커니즘**에 주어짐

### 돌파 프롬프트 1: 역방향 유도 (Reverse Derivation)

기존 (실패 패턴):
```
"SLE에서 κ=6이다. 6은 완전수다. 연결!"
```

돌파 프롬프트:
```
[현상 X]를 first principles에서 유도해라.
유도의 각 단계에서 어떤 수가 나타나는지 추적해라.
그 수가 나타나는 이유가 "우연히 6이어서"인지,
아니면 "σ(n)=2n을 만족하는 수여야 해서"인지 구분해라.
반례: 같은 유도를 n=6 대신 n=7, n=8, n=28로 반복해서
어디서 깨지는지 정확히 보여라.
```

### 돌파 프롬프트 2: 반사실적 파괴 (Counterfactual Destruction)

기존 (실패 패턴):
```
"유전코드가 (4,3)이고 τ(6)=4, n/φ=3이다. 일치!"
```

돌파 프롬프트:
```
[발견 Y]에서 "n=6이 완전수"라는 사실을 제거해라.
즉, σ(6)=12라는 조건 없이도 [발견 Y]가 성립하는가?
만약 성립하면: 완전수와 무관한 우연이다.
만약 깨지면: 정확히 어떤 단계에서 σ(n)=2n이 필요한지 보여라.
이것이 "관찰"을 "설명"으로 바꾸는 증명이다.
```

### 돌파 프롬프트 3: 구성적 필연성 (Constructive Necessity)

기존 (실패 패턴):
```
"Kissing k(2)=6=n. 일치!"
```

돌파 프롬프트:
```
[물리/수학 문제 Z]를 변분 원리로 풀어라.
최적화의 해가 완전수의 산술적 성질(σ=2n, 약수 합 = 자기 자신의 2배)을
필연적으로 요구함을 보여라.
핵심: "답이 6이다"가 아니라 "답이 완전수여야 한다"를 증명해라.
그러면 가장 작은 완전수가 6이므로 자연스럽게 6이 선택된다.
```

### 가장 현실적인 돌파 후보: P-CODON (유전코드)

이미 증명된 것:
- n/φ(n) ∈ Z ⟺ n=6 (완전수 중) — σ(n)=2n이 증명의 핵심 단계에서 사용됨

아직 안 된 것 + 돌파 프롬프트:
```
정보 채널에서 오류 정정 능력을 최대화하는 코드 (b,L)을 유도해라.
제약 조건: b^L > 20 (아미노산), L은 정수, 단일 염기 치환에 대한 robustness 최대화.
이 최적화 문제의 해가 b=τ(n), L=n/φ(n)이 되려면
n이 어떤 성질을 가져야 하는지 역으로 유도해라.
최적화의 KKT 조건에서 σ(n)=2n이 나타남을 보여라.
```

### 돌파 성공 판정 기준

| 수준 | 의미 | 예시 |
|------|------|------|
| Level 0 | 숫자 일치 | "κ=6이고 6은 완전수" |
| Level 1 | 반례 실패 | "n=7,8,28에서는 안 됨" |
| Level 2 | 조건 추적 | "증명의 3단계에서 σ(n)=2n이 필요" |
| **Level 3** | **구성적 필연** | **"최적화하면 완전수가 나옴을 증명"** |
| Level 4 | 새 예측 | "이 원리로 미발견 현상 예측 + 검증" |

현재: 대부분 Level 0~1. **Level 3 달성이 노벨급 돌파.**

### 실패한 시도 기록 (교훈)

- **P1 보편성 지수**: n=6으로 분해 시도 → REFUTED (n=8,12가 더 잘 매칭). 교훈: 대조군 필수.
- **σ-chain 의식 벤치**: 소규모에서 INCONCLUSIVE. 교훈: 스케일 부족은 반증도 검증도 아님.

## 💡 Tips

- **Playwright + SSH**: Claude can SSH into servers, deploy code, then immediately verify via browser. Full CI/CD in one conversation.
- **Superpowers is non-negotiable**: The brainstorming and planning skills save more time than they cost. Skip them and you'll rewrite.
- **Context7 before Stack Overflow**: Always check docs first. Faster and more accurate.
- **Code review after every major step**: Catches issues early. Use the code-review plugin, not just eyeballing diffs.
