# qm-state — QM 에이전트 런타임 상태 디렉터리

이 디렉터리는 QM 에이전트들이 사용하는 **런타임 상태 파일**을 보관합니다.
git에 포함된 이 README는 디렉터리 용도와 파일 스키마를 안내하기 위한 것이며, 실제 상태 파일들(`*.json`)은 사용자 환경에 자동 생성되고 git에 커밋되지 않습니다(`.gitignore` 권장).

---

## 설치 위치

설치 시 이 디렉터리 자체는 다음 두 위치 중 하나에 생성합니다.

- **기본 권장**: `~/.claude/qm-state/` (Claude Code 사용자 환경, 모든 프로젝트가 공유)
- **대안**: 작업 디렉터리 기반 `<workspace>/qm-state/`

`qm-exec-reporter` 에이전트는 기본 권장 경로를 사용합니다. 변경 시 에이전트 본문의 경로도 함께 수정하세요.

---

## 파일 1: qm-session-state.json

**용도:** 주간 QA 업무보고 워크플로우(qm-exec-reporter 섹션 A)의 Step 1~6 진행 상태를 추적. 세션 중단 후 재실행 시 미완료 step부터 안전하게 재개하기 위함. tc-v2의 `team/state.json` 패턴 차용.

**스키마 (예시):**
```json
{
  "weekly_report": {
    "current_week": "2026-W18",
    "page_id": "345669653",
    "steps": {
      "step1_milestone": {
        "status": "done",
        "ts": "2026-05-04T10:15:00",
        "data": {"milestone_n": 6, "period_start": "2026-04-01", "period_end": "2026-06-30"}
      },
      "step2_data": {
        "status": "done",
        "ts": "2026-05-04T10:25:00",
        "data": {"jira_issues_count": 12, "tc_progress": "75%"}
      },
      "step3_folders": {
        "status": "done",
        "ts": "2026-05-04T10:30:00",
        "data": {"year_folder_id": "328401080", "month_folder_id": "345145455"}
      },
      "step4_draft": {"status": "in_progress", "ts": "2026-05-04T10:45:00"},
      "step5_self_heal": {"status": "pending"},
      "step6_uploaded": {"status": "pending"}
    }
  }
}
```

**상태 값:** `pending` / `in_progress` / `done` / `failed`

**갱신자:** `qm-exec-reporter` (각 Step 완료 직후)

**판단 로직:** `current_week`가 호출 시점 주차와 다르면 새 주차 시작 → 모든 step 초기화. 같으면 마지막 `done` 다음부터 재개.

---

## 파일 2: qm-report-log.json

**용도:** 주간/월간/분기 보고 이력 누적. ISO 9001 9.3 Management Review의 필수 입력인 "직전 검토의 액션 진행 상황"을 자동으로 채우기 위해 사용. 또한 NC/CA/PA ID 순번 발급 기준이 됨.

**스키마 (예시):**
```json
{
  "reports": [
    {
      "id": "WR-2026W18",
      "period": "2026-W18",
      "type": "weekly",
      "audience": "internal",
      "generated_at": "2026-05-04T14:30:00",
      "page_url": "https://dexar.atlassian.net/wiki/spaces/QA/pages/345669653",
      "milestone_n": 6,
      "issued_ids": ["NC-2026Q2-008", "CA-2026Q2-012"]
    },
    {
      "id": "MR-2026M04",
      "period": "2026-04",
      "type": "monthly",
      "audience": "executive",
      "generated_at": "2026-05-01T09:00:00",
      "action_items": [
        {
          "id": "AI-2026M04-001",
          "owner": "한성",
          "due": "2026-05-31",
          "status": "open",
          "source_nc": "NC-2026Q2-001",
          "description": "TC 자동 생성 시 데이터 영역 누락 방지 — 표준 갱신"
        }
      ]
    }
  ],
  "id_counters": {
    "2026Q2": {"NC": 12, "CA": 15, "PA": 4}
  }
}
```

**핵심 사용처:**
1. **qm-exec-reporter B 섹션 Step 1**: 가장 최근 monthly/quarterly 보고의 `action_items`를 자동으로 가져와 새 보고의 "직전 검토의 액션 진행 상황" 섹션 구성
2. **qm-capa-tracker, qm-process-auditor**: `id_counters[YYYYQQ]`에서 다음 NC/CA/PA 순번 조회 → 새 ID 발급 시 +1
3. **분기 전환 자동 감지**: 작성일이 새 분기에 진입했으면 새 키(`2026Q3` 등) 추가, `001`부터 시작

**갱신자:** `qm-exec-reporter` (보고 완료 직후), `qm-capa-tracker` / `qm-process-auditor` (ID 발급 시 `id_counters` 동기화)

---

## 충돌 방지

여러 에이전트가 동시에 같은 파일을 수정할 가능성이 낮지만(QM 작업은 단일 사용자 + 순차 실행), 안전을 위해:

- 갱신 전 항상 **읽기 → 수정 → 쓰기** 순서
- 쓰기 전 파일 존재·구조 검증
- JSON 파싱 실패 시 자동 복구 시도하지 말고 사용자에게 알릴 것

---

## .gitignore 권장 항목

이 디렉터리의 README는 git에 포함시키되, 상태 파일은 제외:

```
qm-state/qm-session-state.json
qm-state/qm-report-log.json
```

---

## 관련 문서

- `agents/qm-exec-reporter.md` — Step 0(상태 읽기) + 보고 로그 갱신 로직
- `agents/qm-capa-tracker.md` — NC/CA/PA ID 체계
- `agents/qm-process-auditor.md` — NC ID 체계
- `skills/qm-CAPA/SKILL.md`, `skills/qm-audit/SKILL.md` — ID 규칙 상세
