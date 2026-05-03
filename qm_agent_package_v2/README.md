에# QM Agent for MMORPG QA — 패키지 안내

**Version: v2.1.0** ([CHANGELOG](./CHANGELOG.md) 참조)

> **목적**: **현재 개발 중인 MMORPG 게임의 품질 관리(QM, Quality Management)**. 사용자의 [QA 팀 업무 프로세스](https://github.com/hansungjo86/QATeamBuilding/blob/main/QA_%ED%8C%80_%EC%97%85%EB%AC%B4_%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4.md)를 표준 기준으로, ISO 9001:2015를 표준으로 운영.
>
> **옵션 통합 (의존성 없음)**: 본 패키지는 **TC Team v2 없이도 독립 동작**합니다. 사용자가 [TC Team v2 멀티 에이전트 파이프라인](https://github.com/nobles92ts-ship-it/AI_GAME_QA_TestCase) 또는 다른 TC 자동 생성 도구를 함께 운영 중이라면, 그 산출물(TC 결과, 대시보드, Jira 흐름)을 KPI 측정·감사·보고의 입력으로 활용할 수 있습니다. 데이터 입력 형식만 맞으면 어떤 도구라도 무방합니다.
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
│   ├── qm-team-v1.md                  # 오케스트레이터 — 전체 QM 활동 조율
│   ├── qm-doc-governor.md             # 우선순위 1: 규약·표준 문서 거버넌스
│   ├── qm-kpi-tracker.md              # 우선순위 2 + 5: 품질 KPI 추적 + 효율성 측정
│   ├── qm-capa-tracker.md             # 우선순위 3: 회고 → 시정·예방 액션 추적
│   ├── qm-process-auditor.md          # 우선순위 4: QA 프로세스 준수 감사
│   └── qm-exec-reporter.md            # 우선순위 6: 주간 / 월간 / 분기 통합 리포터
└── skills/                            # ~/.claude/skills/ 에 복사
    ├── qm-doc-governance/SKILL.md
    ├── qm-KPI/SKILL.md
    ├── qm-CAPA/SKILL.md
    ├── qm-audit/SKILL.md
    └── qm-mgmt-review/SKILL.md
```

## 에이전트 / 모델 / 핵심 역할

| # | 에이전트 | 모델 | 핵심 역할 | 자연어 트리거 | 짝 스킬 | ISO 9001:2015 |
|---|---------|------|----------|--------------|---------|--------------|
| 1 | qm-team-v1 | sonnet | 오케스트레이터 — 사용자 요청을 해석해 하위 5개 에이전트에 위임 | "QM 보고서", "이번 분기 QM 활동" | (디스패처) | 전체 — QMS 운영 |
| 2 | qm-doc-governor | **opus** ⭐ | 표준 문서들의 용어/버전/상호참조/표준성 일관성 검증, 변경 영향 분석 | 문서 개정, "용어 통일", 신규 표준 도입 | qm-doc-governance | 7.5 Documented Information |
| 3 | qm-kpi-tracker | sonnet | Google Sheets TC 결과 + Jira 데이터 → PASS율, 회귀율, 자동화 ROI, 효율성 지표 계산 | "주간/월간 KPI", "TC 결과" | qm-KPI | 9.1.3 Performance Evaluation |
| 4 | qm-capa-tracker | sonnet | 회고록 분석 → 시정조치(CA) / 예방조치(PA) 분리 도출, 다음 회고에서 추적 | "회고", "액션 아이템", "시정조치" | qm-CAPA | 10.2 Nonconformity & Corrective Action |
| 5 | qm-process-auditor | **opus** ⭐ | 사용자 GitHub 표준 vs 실제 Jira/Confluence/Sheets 활동 비교 감사, 부적합 Finding 작성 | 분기 감사, 부적합 발견, 신규 프로세스 도입 후 | qm-audit | 9.2 Internal Audit |
| 6 | qm-exec-reporter | **opus** ⭐ | (A) 주간 QA 업무보고 Confluence 작성·업로드, (B) 월간 KPI/CAPA 종합, (C) 분기 경영진/외부 부서 보고 | "주간보고", "월간 리포트", "경영진 보고" | qm-mgmt-review | 9.3 Management Review |

⭐ = 외부 공개 / 표준 판정 / 부적합 판정 카테고리 — 항상 최신 Opus 모델 적용. 모델 업그레이드 시 작업 시작 전 사용자에게 안내. 자세한 정책은 각 에이전트 본문의 "모델 정책" 섹션 참조.

> Claude.ai Projects 환경에서는 슬래시 명령어(`/qm-kpi`, `/qm-retro`, `/qm-audit`, `/qm-doc`, `/qm-report`)로도 호출 가능. 자세한 내용은 슬래시 명령어 패키지의 `PROJECTS_ADDENDUM.md` 참조.

## 두 가지 사용처

이 패키지는 **두 곳에서 동시에 사용**되도록 설계되었습니다.

### A. Claude.ai Projects (가벼운 일상 보조용)

`PROJECT_INSTRUCTIONS.md`의 내용을 **본인의 Claude.ai > Projects > MMORPG QA > "사용자 지침(Custom Instructions)"** 영역에 붙여넣으세요.

→ 채팅 인터페이스에서 QM 작업을 자연스럽게 보조받을 수 있습니다.
→ 무거운 자동화·파이프라인은 아니지만, 빠른 검토·질문·드래프트 작성에 적합합니다.

### B. Claude Code (자동화·다단계 작업용)

`agents/`와 `skills/` 디렉터리를 본인의 `~/.claude/agents/`와 `~/.claude/skills/`에 복사하세요. (자세한 방법은 `INSTALL.md` 참고)

→ Claude Code 환경에서 동작합니다 (TC Team v2와 같은 인프라를 공유 가능, 그러나 독립 운영도 무방).
→ 자연어 명령으로 다단계 QM 워크플로우 실행이 가능합니다.

## TC 자동 생성 도구와의 관계 (옵션)

이 QM 에이전트는 **TC를 직접 생성하지 않습니다.** TC 작성은 별도 도구의 영역이며, 본 패키지는 그 결과물을 한 단계 위에서 분석·평가·보고합니다.

사용자가 TC 자동 생성 도구(예: TC Team v2)를 운영 중이라면 다음과 같이 통합할 수 있습니다.

```
[옵션] TC 자동 생성 도구 (예: TC Team v2)
└── 출력물: Google Sheets TC, 대시보드, TC 결과
                    │
                    ▼
QM 에이전트 (본 패키지) — 도구 종류와 무관하게 데이터 형식만 맞으면 동작
└── 입력물로 활용:
    ├── KPI 계산 (qm-kpi-tracker)
    ├── 프로세스 준수 감사 (qm-process-auditor)
    └── 경영진 보고 (qm-exec-reporter)
```

TC 자동 생성 도구가 없어도 본 패키지는 동작합니다. 사용자가 수동으로 데이터를 제공하거나, Jira/Confluence 같은 일반 도구의 데이터만으로도 KPI/감사/보고를 수행할 수 있습니다.

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
