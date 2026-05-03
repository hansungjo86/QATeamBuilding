# 설치 가이드 (INSTALL.md)

> 본 패키지를 사용하는 두 가지 방법을 안내합니다.

---

## 방법 A. Claude.ai Projects (권장 — 일상 보조용)

가장 빠르게 시작할 수 있는 방법입니다. 별도 설치 없이 사용 가능합니다.

### Step 1. Claude.ai 접속

https://claude.ai 접속 → 로그인.

### Step 2. Projects > MMORPG QA 진입

좌측 메뉴 "Projects" → "MMORPG QA" 선택.

> 만약 "MMORPG QA" 프로젝트가 없다면 새로 생성하세요. 이 패키지는 그 프로젝트 안에서 사용하기 위해 설계되었습니다.

### Step 3. 프로젝트 지침 설정

프로젝트 화면 우측 또는 설정에서 **"사용자 지침(Custom Instructions)"** 영역을 찾으세요.

`PROJECT_INSTRUCTIONS.md` 파일을 열어, **`--- 시스템 지침 시작 ---`부터 `--- 시스템 지침 끝 ---` 사이의 내용 전체**를 복사하여 사용자 지침 영역에 붙여넣습니다.

> 주의: `## 사용 팁` 이하 부분은 본인 참고용이므로 시스템 지침에는 포함하지 마세요.

### Step 4. (선택) 프로젝트 지식 업로드

프로젝트의 **"프로젝트 지식(Project Knowledge)"** 영역에 다음 파일을 업로드하면 컨텍스트가 풍부해집니다.

권장 업로드:
- 사용자 GitHub의 `QA_팀_업무_프로세스.md` 파일
- TC Team v2의 `README.md`
- 본인의 KPI 정의서, 회고록 (있다면)
- 본 패키지의 `agents/*.md` 파일들 (Claude가 에이전트 정의를 참고함)

### Step 5. 새 대화 시작

프로젝트 안에서 새 대화를 시작하면 QM 에이전트로 동작합니다.

테스트 프롬프트:
```
이번 주 TC 결과 KPI 뽑아줘.
[데이터 첨부]
```

---

## 방법 B. Claude Code (자동화·다단계 작업용)

TC Team v2와 동일한 인프라에 설치합니다.

### 사전 요구 사항

- Claude Code 설치 완료 (https://claude.com/claude-code)
- Node.js v18+ (TC Team v2가 이미 설치되어 있다면 충족됨)
- `~/.claude/agents/`, `~/.claude/skills/` 디렉터리 존재

### Step 1. 패키지 다운로드 / 복사

본 패키지의 `agents/`, `skills/` 디렉터리를 본인 작업 환경으로 복사합니다.

### Step 2. 에이전트 설치

```bash
# Mac / Linux
cp agents/*.md ~/.claude/agents/

# Windows (PowerShell)
Copy-Item agents/*.md $HOME/.claude/agents/
```

설치된 에이전트 목록 확인:
```bash
ls ~/.claude/agents/qm-*
```

다음 6개 파일이 보여야 합니다.
```
qm-팀-v1.md
qm-doc-governor.md
qm-kpi-tracker.md
qm-capa-tracker.md
qm-process-auditor.md
qm-exec-reporter.md
```

### Step 3. Skills 설치

```bash
# Mac / Linux
cp -r skills/* ~/.claude/skills/

# Windows (PowerShell)
Copy-Item -Recurse skills/* $HOME/.claude/skills/
```

설치된 스킬 목록 확인:
```bash
ls ~/.claude/skills/qm-*
```

다음 5개 디렉터리가 보여야 합니다.
```
qm-문서거버넌스/
qm-KPI/
qm-CAPA/
qm-감사/
qm-경영검토/
```

### Step 4. (이미 TC Team v2 설치됨이라면) MCP 설정 확인

TC Team v2가 이미 설치되어 있다면 MCP 설정은 이미 완료되어 있습니다. 같은 Google Sheets / Atlassian MCP를 본 QM 에이전트도 활용합니다.

`~/.claude/.mcp.json` 파일이 존재하고 다음 서버가 등록되어 있는지 확인:
- `google-sheets`
- (선택) `atlassian` (Confluence·Jira 접근용)

### Step 5. 사용 시작

Claude Code를 실행하고 다음과 같이 호출:

```
@qm-팀-v1 이번 분기 QM 종합 보고서 만들어줘
```

또는 특정 에이전트를 명시적으로:

```
@qm-kpi-tracker 이번 주 KPI 분석해줘
```

---

## 사용 패턴

### 자연어 명령 (자동 위임)

각 에이전트의 `description` 필드에 따라 Claude Code가 자동으로 적절한 에이전트에 위임합니다.

예시:
```
이번 회고록 정리해서 액션 아이템 뽑아줘
→ qm-capa-tracker로 자동 위임 (description의 "회고록 정리"와 매칭)
```

```
규약 문서 일관성 검증해줘
→ qm-doc-governor로 자동 위임
```

### 명시적 호출 (자동 위임이 안 될 때)

자동 위임이 실패하면 명시적으로 호출하세요.

```
@qm-process-auditor 지난 스프린트 Jira 흐름 감사해줘
```

> 자동 위임의 한계는 본 패키지의 `README.md`에 설명되어 있습니다. 중요한 작업은 명시적 호출이 안전합니다.

---

## 트러블슈팅

### Q1. 에이전트가 자동으로 호출되지 않음

A. Claude Code의 자동 위임은 100% 신뢰성이 보장되지 않습니다. 명시적으로 `@qm-...` 형식으로 호출하세요.

### Q2. Skills 파일을 못 찾는다고 나옴

A. Skills 디렉터리가 정확히 `~/.claude/skills/qm-XXX/SKILL.md` 형식인지 확인하세요. (개별 파일이 아닌 디렉터리 안의 SKILL.md)

### Q3. Google Sheets / Jira에 접근 못 함 (Claude Code)

A. TC Team v2가 사용하는 MCP 설정이 정상인지 확인하세요. `~/.claude/.mcp.json`의 google-sheets 서버 설정을 점검합니다.

### Q4. 한국어 인코딩 문제 (Windows)

A. 에이전트 파일명에 한글이 포함되어 있어 일부 환경에서 문제가 될 수 있습니다. 필요 시 영문으로 변경 가능 (예: `qm-팀-v1.md` → `qm-team-v1.md`). 단, 변경 시 다른 에이전트에서의 참조도 함께 변경해야 합니다.

### Q5. Projects에서 첨부 파일이 너무 큼

A. 한 번에 모든 데이터를 첨부하지 말고, 작업별로 필요한 데이터만 첨부하세요. 예: KPI 분석 시에는 TC 결과만, 감사 시에는 Jira 이력만.

---

## 다음 단계

1. 1주일 PoC 운영
2. 각 에이전트의 출력 품질을 측정 (검증 시간, 정확도)
3. `description` 필드를 다듬어 자동 위임 정확도 향상
4. 본 가이드 자체를 `qm-doc-governor`로 검증 → 개선

---

## 출처

- Claude Code 공식 문서 - Subagents (https://code.claude.com/docs/en/sub-agents)
- TC Team v2 (https://github.com/nobles92ts-ship-it/AI_GAME_QA_TestCase) — 설치 패턴 참고
