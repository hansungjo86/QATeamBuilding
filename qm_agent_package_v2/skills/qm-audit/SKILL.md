---
name: qm-audit
description: 내부 감사(Internal Audit) 도메인 지식. ISO 9001:2015 기반 감사 절차, 부적합 분류, 증거 수집 방법. qm-process-auditor 에이전트가 활용.
---

# QM 내부 감사 스킬

## 내부 감사의 정의

> 출처: IT Governance Framework - ISO 9001 Surveillance Audit Checklist (https://www.itgov-docs.com/blogs/iso-9001/iso-9001-surveillance-audit-checklist)
>
> *원문 인용: "Internal audits are an essential component of the surveillance audit checklist. These audits are conducted by internal auditors within the organization to evaluate the effectiveness of the quality management system and identify any areas for improvement."*
>
> *한국어 의역: 내부 감사는 감사 체크리스트의 필수 요소로, 조직 내부 감사자가 QMS의 효과성을 평가하고 개선 영역을 식별하기 위해 수행한다.*

## ID 체계 (분기 기반 추적)

모든 Finding(부적합)에는 분기 단위 연속 추적 가능한 ID를 부여합니다. qm-CAPA 스킬과 동일한 NC- 접두사를 공유하여, 감사에서 발견된 부적합이 그대로 CAPA 추적으로 이어집니다.

| 종류 | 형식 | 예시 |
|------|------|------|
| 부적합 (Audit Finding) | `NC-YYYYQQ-NNN` | `NC-2026Q2-001` |

**생성 규칙:**
1. **YYYYQQ**: 감사 작성일이 속한 연·분기 (1~3월=Q1, 4~6월=Q2, 7~9월=Q3, 10~12월=Q4)
2. **NNN**: 해당 분기 내 직전 보고 마지막 순번 +1, 3자리 zero-padded
3. 분기 전환 시 `001`부터 다시 시작
4. **qm-CAPA 스킬과 NC 순번을 공유** (같은 분기에서 CAPA가 NC-2026Q2-005까지 발급했다면 감사는 NC-2026Q2-006부터)

**ID 발급 시 사전 확인:** 직전 보고 로그(`~/.claude/qm-state/qm-report-log.json`)에서 해당 분기 마지막 순번 조회. 부재 시 사용자 확인 후 `001`부터.

## 감사 5단계 절차

### Step 1. 감사 범위 정의 (Scope)

```
[감사 범위]
- 대상 기간: YYYY-MM-DD ~ YYYY-MM-DD
- 대상 활동: [Jira 흐름 / TC 작성 / 버그 처리 / 기획 변경 / 완료 처리 중 선택]
- 대상 일감: [구체적 ID 목록 또는 "전체"]
- 감사 목적: [정기 / 이슈 발생 후 / 신규 프로세스 적용 검증]
```

### Step 2. 표준 절차 추출 (Standard)

표준 문서에서 해당 활동의 정의된 절차를 **원문 그대로 인용**.

```
표준 절차: [활동명]
출처: [URL]
*원문 인용 (그대로):*
"[원문]"
```

추측이나 재해석 금지.

### Step 3. 실제 데이터 수집 (Evidence)

```
[필요 데이터]
- Jira 일감 ID + 상태 변경 이력 (시간 스탬프 포함)
- TC 시트 생성 일시 + 결과 기록 일시
- 버그 일감 할당 → 완료 시점
- 회고록 관련 언급
- 기타 [구체적 증거]
```

### Step 4. 비교 (표준 vs 실제)

각 단계를 표 형태로 비교:

```
| 단계 | 표준 | 실제 | 적합 여부 |
|------|------|------|---------|
| ... | ... | ... | ✅/⚠️/❌ |
```

판정:
- ✅ 적합: 표준대로 진행됨
- ⚠️ Minor 부적합: 일부 인원 절차 미준수 (전체 위협 없음)
- ❌ Major 부적합: QMS 부재·완전한 붕괴

### Step 5. 감사 보고서 작성

```
# 내부 감사 보고서 (초안)

**감사 대상**: [범위]
**감사 기간**: [기간]
**감사자**: qm-process-auditor + 인간 검증 필요

## 요약
- 감사 대상 활동 수: N
- 적합 (✅): N건
- Minor (⚠️): N건
- Major (❌): N건

## 발견 사항 (Findings)

### NC-YYYYQQ-NNN (Major/Minor)
- 부적합: [구체적 기술]
- 표준 출처: [URL + 원문 인용]
- 실제 증거: [데이터]
- 영향: [품질·고객 영향]
- 권고 시정 조치: [CA 권고 — qm-capa-tracker로 전달 시 동일 NC ID 참조]

## 적합 사항 (Conformities)
[잘 운영된 영역]

## 권고 다음 단계
- [ ] qm-capa-tracker로 전달하여 CA/PA 도출
- [ ] 다음 감사에서 동일 영역 재검증
```

## 사용자 환경의 감사 체크포인트

### A. Jira 일감 상태 흐름 감사

> 표준 출처: 사용자 GitHub - QA 팀 업무 프로세스 1절
> https://github.com/hansungjo86/QATeamBuilding/blob/main/QA_%ED%8C%80_%EC%97%85%EB%AC%B4_%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4.md

표준 흐름 (원문 인용):
```
[기획] 진행중 → 완료
[서버] 시작 전 → 진행중 → 테스트
[클라] 시작 전 → 진행중 → 테스트
[QA]  (자동 생성) → 진행중 → 완료
```

체크포인트:
- 각 일감이 정의된 순서로 변경되었는가
- 직군별 상태 변경 책임이 정확한가 (기획자/개발자/QA)
- [QA] 일감이 자동 생성되었는가 (수동 생성 시 부적합)

### B. TC 작성 절차 감사

> 표준 출처: 사용자 GitHub QA 프로세스 2-3절 + TC Team v2

체크포인트:
- TC Team v2 파이프라인이 실행되었는가
- 4가지 검토 항목이 점검되었는가
  - 기획서 주요 기능 빠짐없이 반영
  - 정상/부정/예외 비율 적절
  - 취소선 처리된 기능 제외
  - 추후 구현 항목 비고란 표기

### C. 버그 처리 흐름 감사

체크포인트:
- FAIL 발견 시 Jira 버그 일감이 생성되었는가
- 담당 개발자([클라] 또는 [서버])에 정확히 할당되었는가
- 수정 후 재테스트가 진행되었는가
- 회귀 테스트가 실시되었는가

### D. 기획 변경 처리 감사

표준 흐름:
1. 기획자가 Confluence 갱신
2. Jira 일감 코멘트에 변경 내용 기재
3. QA팀이 "기획 변경됐어" 명령 실행
4. TC 갱신 + 영향받는 결과 초기화
5. QA팀 재테스트

체크포인트:
- 기획 변경 시 즉시 QA팀에 공유되었는가
- TC 갱신 명령이 실행되었는가
- 영향받는 TC가 재테스트되었는가

### E. 완료 처리 감사

표준 (사용자 GitHub 6절):
- 모든 TC가 PASS 또는 N/A
- 버그 Jira 모두 "완료" 상태
- Google Sheets 결과 기록 완료
- Jira [QA] "완료" 상태 변경

체크포인트:
- FAIL 잔존 상태에서 완료 처리되지 않았는가 (Major 부적합)
- TC 결과가 비어 있는 채로 완료되지 않았는가 (Major 부적합)

## 감사 원칙

### 1. 증거 기반 (Evidence-based)

- 모든 부적합은 **구체적 증거** 동반
- 일감 ID, 시간 스탬프, 인용으로 뒷받침
- "느낌상 부적합" 금지

### 2. 시스템 vs 사람

- "OO이 잘못했다" ❌
- "프로세스가 미준수되었다" ✅
- ISO 9001은 비난(blame)이 아닌 시스템 개선 관점

### 3. 균형 잡힌 보고

- 부적합만 찾는 감사는 사기 저하
- 적합 사항도 명시하여 균형 유지

### 4. 감사자 독립성

> 출처: IT Governance Framework
> *원문 인용: "Verify that internal audits are being conducted by competent auditors who are independent of the areas being audited."*
> *한국어 의역: 내부 감사는 감사 대상 영역으로부터 독립된 유능한 감사자가 수행하는지 확인한다.*

본 환경에서는 2인 체제로 완전한 독립성 확보가 어려우므로:
- 가능한 한 LLM 에이전트의 1차 분석을 활용
- 인간 감사자(QA 팀장)는 검증·승인 역할

## 감사 빈도 권고 (할루시네이션 방지 명시)

> 다음은 외부 표준이 아닌 운영 권고안입니다.

- **분기 1회 정기 감사** + **이슈 발생 시 임시 감사**
- 신규 프로세스 도입 후 1개월 이내 1차 감사
- 이전 감사에서 Major 부적합 발견 시 60일 이내 추적 감사
