# QM Agent for MMORPG QA — 패키지 안내

> **목적**: 사용자가 운영 중인 [TC Team v2 멀티 에이전트 파이프라인](https://github.com/nobles92ts-ship-it/AI_GAME_QA_TestCase)과 호환되며, 사용자의 [QA 팀 업무 프로세스](https://github.com/hansungjo86/QATeamBuilding/blob/main/QA_%ED%8C%80_%EC%97%85%EB%AC%B4_%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4.md)를 표준 기준으로 사용하는 **MMORPG QA 프로젝트용 QM(Quality Management) 에이전트 패키지**.
>
> **운영 환경**: 2인 QA 파트 (팀장 = QM 역할 겸임 / 팀원 = LLM R&D)
> **기준 표준**: ISO 9001:2015 (Quality Management Systems)

---

## 이 패키지의 구성

```
qm_agent_package/
├── README.md                          # 본 문서
├── PROJECT_INSTRUCTIONS.md            # Claude.ai Projects > MMORPG QA에 붙여넣을 프로젝트 지침
├── INSTALL.md                         # Claude Code에 설치하는 방법
├── agents/                            # ~/.claude/agents/ 에 복사
│   ├── qm-팀-v1.md                    # 오케스트레이터 — 전체 QM 활동 조율
│   ├── qm-doc-governor.md             # 우선순위 1: 규약·표준 문서 거버넌스
│   ├── qm-kpi-tracker.md              # 우선순위 2 + 5: 품질 KPI 추적 + 효율성 측정
│   ├── qm-capa-tracker.md             # 우선순위 3: 회고 → 시정·예방 액션 추적
│   ├── qm-process-auditor.md          # 우선순위 4: QA 프로세스 준수 감사
│   └── qm-exec-reporter.md            # 우선순위 6: 외부 부서·경영진 보고
└── skills/                            # ~/.claude/skills/ 에 복사
    ├── qm-문서거버넌스/SKILL.md
    ├── qm-KPI/SKILL.md
    ├── qm-CAPA/SKILL.md
    ├── qm-감사/SKILL.md
    └── qm-경영검토/SKILL.md
```

## 두 가지 사용처

이 패키지는 **두 곳에서 동시에 사용**되도록 설계되었습니다.

### A. Claude.ai Projects (가벼운 일상 보조용)

`PROJECT_INSTRUCTIONS.md`의 내용을 **본인의 Claude.ai > Projects > MMORPG QA > "사용자 지침(Custom Instructions)"** 영역에 붙여넣으세요.

→ 채팅 인터페이스에서 QM 작업을 자연스럽게 보조받을 수 있습니다.
→ 무거운 자동화·파이프라인은 아니지만, 빠른 검토·질문·드래프트 작성에 적합합니다.

### B. Claude Code (자동화·다단계 작업용)

`agents/`와 `skills/` 디렉터리를 본인의 `~/.claude/agents/`와 `~/.claude/skills/`에 복사하세요. (자세한 방법은 `INSTALL.md` 참고)

→ TC Team v2와 동일한 인프라에서 동작합니다.
→ 자연어 명령으로 다단계 QM 워크플로우 실행이 가능합니다.

## TC Team v2와의 관계

이 QM 에이전트는 **TC Team v2를 대체하지 않고, 그 위에서 활용·감사·개선합니다**.

```
TC Team v2 (QA 실무 파이프라인)
└── 출력물: Google Sheets TC, 대시보드, TC 결과
                    │
                    ▼
QM 에이전트 (이 패키지)
└── 입력물로 활용:
    ├── KPI 계산 (qm-kpi-tracker)
    ├── 프로세스 준수 감사 (qm-process-auditor)
    └── 경영진 보고 (qm-exec-reporter)
```

QM 에이전트는 **TC 자동 생성 작업에 개입하지 않습니다.** TC Team v2가 만들어낸 결과물을 한 단계 위에서 분석·평가·보고합니다.

## 4가지 작성 지침 준수

본 패키지의 모든 문서는 다음 지침을 따릅니다.

1. **한글화**: 본문 한글, 영문 약어·명령어·파일명만 영문 유지
2. **출처 명시**: 외부 정보 인용 시 URL 명시 (각 문서 하단 출처 섹션)
3. **축약·의역 검증**: ISO 9001 등 영문 표준 인용 시 영문 원문 + 한국어 의역 표시
4. **할루시네이션 검증**: 외부 표준 출처가 없는 부분은 "권고안" 또는 "사용자 환경 추정"으로 명시

## 시작하기

1. `PROJECT_INSTRUCTIONS.md`를 먼저 읽고 Projects에 붙여넣기
2. 필요 시 `INSTALL.md`를 따라 Claude Code에 에이전트 설치
3. 본인의 환경(Jira/Confluence/Google Sheets/Drive 폴더 ID)을 각 에이전트의 frontmatter에 반영
4. 1주 PoC 운영 → 결과 평가 → 프롬프트 조정

## 출처

| # | 출처 | URL |
|---|------|-----|
| 1 | 사용자 GitHub - QA 팀 업무 프로세스 | https://github.com/hansungjo86/QATeamBuilding |
| 2 | TC Team v2 (TC 자동 생성 파이프라인) | https://github.com/nobles92ts-ship-it/AI_GAME_QA_TestCase |
| 3 | Babtec - QM vs QA 차이 (ISO 9000 기반) | https://www.babtec.com/blog/difference-quality-management-quality-assurance |
| 4 | ISO 공식 - Quality Management | https://www.iso.org/quality-management/ |
| 5 | Anthropic 공식 - Subagents in Claude Code | https://claude.com/blog/subagents-in-claude-code |
| 6 | Claude Code 공식 문서 | https://code.claude.com/docs/en/sub-agents |
