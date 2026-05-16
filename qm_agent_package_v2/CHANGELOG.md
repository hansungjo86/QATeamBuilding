# Changelog

이 패키지의 모든 주요 변경 사항은 본 파일에 기록됩니다.
형식은 [Keep a Changelog](https://keepachangelog.com/)를 준용하고, 버전은 [Semantic Versioning](https://semver.org/)을 따릅니다.

---

## [v2.1.2] — 2026-05-16

주간보고 작성 사이클 회고(2026-05-16)에서 도출된 **3건의 예방 조치(PA)**를 qm-exec-reporter A섹션에 반영. 만성 패턴 P-01(사전 가정 미검증) + P-02(자율성 과잉) 차단이 목표.

### Added

- **qm-exec-reporter.md "운영 모드 결정" 섹션 신설 (Step 0 직전, PA-2026Q2-003)**
  - dry-run 모드: Step 1~5 실행하되 Step 6 업로드 직전 멈춤. 외부 변경 없이 페이로드 시연 후 사용자 승인 시 live 전환
  - live 모드: Step 1~6 실제 실행
  - 첫 자동 실행, 워크플로우 변경 직후, 30일 이상 미실행 시 dry-run 자동 권고
  - 베타테스트 정책(`feedback_qm_versioning.md`) 부합

- **qm-exec-reporter.md Step 0.5 사전 가정 검증 (PA-2026Q2-001)**
  - 5종 가정(A1~A5)을 Step 1 진입 전 read-only로 확인
    - DX 메인 페이지 존재 + 차수 추출 정규식 매칭
    - 부모 폴더 ID 유효성
    - Google Sheets ID 접근성
    - MCP 서버 응답 정상
    - 직전 주차 페이지 존재 + 뎁스 패턴 추출 가능성
  - 가정 위반 시 사용자에게 알리고 본 작업 진입 차단
  - 회고 NC-2026Q2-001/003/004(Mojibake / 폴더 인식 실패 / 차수 위치 오추측) 재발 예방

- **qm-exec-reporter.md Step 4-A 뎁스 패턴 추출 + Step 4-C 자가 검증 (PA-2026Q2-002)**
  - 신규 본문 작성 전 직전 주차 페이지의 뎁스 패턴을 별도 구조(`depth_pattern.json` 형태)로 추출
  - 신규 작성은 추출된 `max_depth` 이상 + 섹션 구조·그룹화 강제 일치
  - 깊이를 줄이는 변경은 사용자 명시 동의 없이 금지 (NC-2026Q2-002 평탄화 사고 재발 방지)
  - 작성 후 자가 검증 + 1회 자동 재시도

### Changed

- **qm-exec-reporter.md Step 4 → Step 4-A/B/C로 분할** — "이전 페이지 뎁스 구조 보존" 룰 한 줄에서 끝나던 것을, 추출 → 작성 → 검증 3단계로 명확화

### Why

2026-05-16 주간보고 회고에서 5건의 NC가 식별되었고 그 중 NC-001/002/003/004의 만성 원인이 두 가지 패턴(P-01 사전 가정 미검증, P-02 자율성 과잉)으로 수렴. v2.1.0/v2.1.1에서 본문 룰 추가로 시정조치(CA)는 완료했으나, **룰만으로는 다음 자동 실행에서 재발 위험 잔존**. 본 patch는 룰을 절차(read-only 검증 단계 + 패턴 추출 + 자가 검증)로 격상해 사고 재발을 구조적으로 차단.

### Not Changed

- 다른 에이전트(qm-doc-governor / qm-process-auditor / qm-capa-tracker / qm-kpi-tracker / qm-team-v1) 변경 없음
- ID 체계 / qm-state 디렉터리 / 모델 정책 변경 없음
- 신규 에이전트 추가 없음 (v2.2.0 작업과 분리)

### Action Items (다음 세션 시작 시)

- 첫 자동 실행 시 **dry-run 모드로 1회 검증** → 사용자 승인 후 live 전환 (PA-003 운영 검증)
- 회고에서 발급한 NC-2026Q2-001~005 + CA-2026Q2-001~005를 `qm-state/qm-report-log.json`에 백필 (다음 자동 실행 시점에 함께 처리)

---

## [v2.1.1] — 2026-05-10

전제 정정 patch — 패키지 본문에 잔존하던 **"MMORPG 라이브 운영"** 표현을 사용자 회사의 실제 환경(**개발 단계, 출시 전**)에 맞게 모두 정정. v2.1.0에서 README 목적은 이미 정정됐지만 다른 파일 본문은 미반영 상태였음.

### Changed

- **PROJECT_INSTRUCTIONS.md**: 운영 컨텍스트 전제를 "라이브 운영 환경" → "개발 환경(출시 전)"으로 정정. MMORPG 특수성 표현 갱신 (마일스톤 빌드 검증·시스템 통합 위험·기획 변경 빈도 중심). CAPA 예시도 빌드 결함 수정 맥락으로 정정. "현재 운영 중인" → "현재 사용 중인" 표현 통일.
- **agents/qm-capa-tracker.md, skills/qm-CAPA/SKILL.md**: CAPA 예시의 "라이브 핫픽스 적용" → "빌드 결함 긴급 수정 (마일스톤 직전)". "검증 없이 라이브" → "검증 없이 마일스톤 통과". 5 Why 예시 시작점을 "라이브 핫픽스 발생" → "빌드 직전 보스 몬스터 데이터 오류 발견". Fishbone 환경 카테고리에 "외주 자산 변경" 추가.
- **agents/qm-exec-reporter.md, skills/qm-mgmt-review/SKILL.md**: Management Review 입력 항목의 "라이브 환경 변화" → "빌드 환경 변화, 외주 자산 변경, 도구 변경". 경영진 보고서 템플릿 "라이브 안정성/품질" → "빌드 안정성/품질". 청자별 관심 영역에 "마일스톤 진행" 추가.
- **agents/qm-process-auditor.md**: Major 부적합 예시 "검증 없이 라이브 배포" → "검증 없이 마일스톤 통과".
- **skills/qm-KPI/SKILL.md, agents/qm-kpi-tracker.md**: 핫픽스 KPI 정의 보존, 적용 시점만 "(라이브 출시 후 적용)"으로 명시 — 개발 단계엔 미적용 KPI임을 명확화.

### Why

v2.1.0에서 README 목적은 "현재 개발 중인 MMORPG 게임의 품질 관리"로 정정됐지만, 패키지 다른 파일들의 본문에는 여전히 "라이브 운영" 가정이 남아 있어 사용자(개발 단계 QA 팀장)에게 잘못된 사고 프레임을 강요. 본 patch로 패키지 전체가 일관된 전제(개발 단계, 출시 전) 위에서 동작.

### Not Changed

- KPI 정의 자체는 보존 — 라이브 출시 후 같은 KPI를 그대로 재사용 가능
- 에이전트 구조 / 모델 정책 / 워크플로우 / ID 체계 / qm-state 등은 변경 없음
- 신규 에이전트 추가 (개발 단계 전용 KPI / 빌드 게이트 / 리스크 등록부 등)는 v2.2.0 이후 작업으로 분리

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
