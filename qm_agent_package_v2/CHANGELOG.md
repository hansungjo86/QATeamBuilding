# Changelog

이 패키지의 모든 주요 변경 사항은 본 파일에 기록됩니다.
형식은 [Keep a Changelog](https://keepachangelog.com/)를 준용하고, 버전은 [Semantic Versioning](https://semver.org/)을 따릅니다.

---

## [v2.1.0] — 2026-05-03

주간보고 자동화 + 핵심 에이전트 opus 승격 + tc-v2 reference 패턴 차용 (분기 ID 체계 + qm-state 런타임).

### Added

- **qm-exec-reporter — 주간 QA 업무보고 워크플로우 (A섹션)**
  - DX 메인 페이지(Confluence 41844931)에서 차수·일정 자동 추출
  - Jira + Google Sheets TC 시트 데이터 수집
  - Confluence 회의록 트리(`회의록 YYYY 년 / 회의록 MM월`) 자동 폴더 확인·생성
  - 자기 힐링(한글/인코딩 검증) 후 사용자 동의 받고 업로드
  - Step 0: 세션 상태 읽어 미완료 step부터 재개
- **분기 기반 ID 체계 (NC/CA/PA-YYYYQQ-NNN)**
  - qm-capa-tracker, qm-process-auditor, qm-CAPA/SKILL.md, qm-audit/SKILL.md 통일 적용
  - 두 에이전트가 NC 순번 공유 → 감사 Finding → CAPA 추적 직접 연결
- **qm-state/ 런타임 디렉터리** (신규)
  - `qm-session-state.json`: 주간 워크플로우 재개 안전망
  - `qm-report-log.json`: 보고 이력 누적 + ID 순번 발급 기준 + 직전 액션 자동 채움
  - `qm-state/README.md`: 디렉터리 용도·스키마·gitignore 권장 안내
- **README.md — 에이전트/모델/핵심 역할 통합 표** (디렉터리 트리 다음에 신규 섹션)
- **CHANGELOG.md** (본 파일)

### Changed

- **qm-exec-reporter** 모델 `sonnet → opus` (외부 공개물 + Confluence 직접 업로드 위험 → 신뢰성 우선)
- **qm-doc-governor** 모델 `sonnet → opus` (메타 표준 판정, 누적 학습 일관성 중요)
- **qm-process-auditor** 모델 `sonnet → opus` (감사 Finding은 외부 공유 + 판정 일관성 중요)
- 위 3개 에이전트에 "최신 모델 정책" 섹션 신규 추가 — 모델 업그레이드 시 작업 시작 전 사용자 안내 의무화
- **qm-team-v1.md** 위임 표에 모델·자연어 트리거 컬럼 추가, qm-exec-reporter 담당을 "주간 + 월간/분기 통합 리포터"로 갱신
- **PROJECT_INSTRUCTIONS.md** 우선순위 6에 주간보고 포함, 요청 패턴 표에 주간보고 행 추가
- **INSTALL.md** 자동 위임 예시에 "주간보고 → qm-exec-reporter" 추가
- **TC Team v2 의존성 제거 — 옵션 통합으로 명확화** (README/INSTALL/PROJECT_INSTRUCTIONS/qm-KPI/qm-audit). 본 패키지는 TC Team v2 없이도 독립 동작하며, 사용자 환경에 있다면 그 산출물을 입력으로 활용 가능. 데이터 형식만 맞으면 다른 TC 자동 생성 도구나 수동 TC도 무방.
- **목적 명확화**: 패키지 목적을 "현재 개발 중인 MMORPG 게임의 품질 관리"로 정정 (이전엔 "TC Team v2와 호환"이 목적처럼 표현되어 있었음)
- **ID 체계 양방향 참조 보강**: qm-capa-tracker.md에 "NC 순번은 qm-process-auditor와 공유" 명시 추가. qm-capa-tracker.md / qm-process-auditor.md에 "ID 발급 직후 id_counters 동기화 필수" 명시 추가 (누락 시 ID 중복 위험)

### Removed

- **`qm_agent_package_README.md`, `qm_agent_package_INSTALL.md`** — README.md/INSTALL.md의 구버전 사본. 한글 파일명 표기(`qm-팀-v1.md` 등)가 실제 영문 파일과 불일치, 최근 변경 누락 누적. README.md/INSTALL.md만 유지.

### Fixed

- `PROJECT_INSTRUCTIONS.md` L122 끝 `~` 오탈자 제거

### Related Commits

- `fdfefef` — QM 에이전트 패키지 v2 추가 (초기 push)
- `01ae2b0` — qm-doc-governor + qm-process-auditor opus 승격 + 패키지 문서 최신화
- `bb0cee7` — 중복된 한글 표기 README/INSTALL 사본 제거
- `3564154` — tc-v2 패턴 차용: 분기 ID 체계 + qm-state 런타임 디렉터리 도입

---

## [v2.0.0] — 2026-05-03 (초기 공개)

### Added

- 6개 에이전트: qm-team-v1, qm-doc-governor, qm-kpi-tracker, qm-capa-tracker, qm-process-auditor, qm-exec-reporter
- 5개 스킬: qm-doc-governance, qm-KPI, qm-CAPA, qm-audit, qm-mgmt-review
- 패키지 문서: README.md, INSTALL.md, PROJECT_INSTRUCTIONS.md
- ISO 9001:2015 기반 QM 활동 정의

### Related Commits

- `fdfefef` — QM 에이전트 패키지 v2 추가
