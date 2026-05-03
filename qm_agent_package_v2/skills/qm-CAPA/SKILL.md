---
name: qm-CAPA
description: 시정·예방 조치(CAPA) 관리 도메인 지식. ISO 9001:2015 기반 부적합 식별, CA/PA 분리 기준, 효과성 검증 방법. qm-capa-tracker 에이전트가 활용.
---

# QM CAPA 스킬

## CAPA 정의 (ISO 9001:2015)

> 출처: deGRANDSON Global - Correction vs Corrective Action vs Preventive Action (https://info.degrandson.com/blog/ccapa)
>
> *영문 원문 인용:*
> *"Correction: ISO 9001:2015 defines Correction as the Action to eliminate a detected nonconformity."*
> *"Corrective Action: ISO 9000:2015 defines Corrective Action as eliminating the cause of nonconformity and preventing its recurrence."*
> *"Preventive Action: ISO 9000:2015 defines Preventive Action as Action taken to eliminate the cause of a potential nonconformity."*
>
> *한국어 의역 (의미 손실 없음):*
> *시정(Correction): 발견된 부적합을 즉시 제거하는 행동*
> *시정 조치(CA): 부적합의 원인을 제거하고 재발을 방지하는 행동*
> *예방 조치(PA): 잠재적 부적합의 원인을 제거하는 행동*

## 3단계 구분 (반드시 명확히)

| 구분 | 시점 | 대상 | 사례 |
|------|------|------|------|
| **시정 (Correction)** | 사후 즉시 | 발생한 사건 자체 | 라이브 핫픽스 적용 |
| **시정 조치 (CA)** | 사후 시스템적 | 근본 원인 | TC 추가 + 코드 리뷰 강화 |
| **예방 조치 (PA)** | 사전 | 잠재 위험 | 차기 기능 사전 리스크 분석 |

> 주의: ISO 9001:2015에서는 PA를 "Risk-based thinking"으로 통합했으나, 실무에서 PA 개념을 유지하는 것은 무방합니다.

## 부적합 식별 키워드

회고록·감사 보고서에서 다음 패턴을 찾습니다.

| 부적합 유형 | 식별 키워드 |
|----------|---------|
| 프로세스 부적합 | "절차 누락", "단계 건너뜀", "프로세스 위반" |
| 품질 부적합 | "버그 발견", "회귀 발생", "TC 누락", "스펙 불일치" |
| 문서 부적합 | "기획서 모호", "문서 미갱신", "버전 충돌" |
| 자원 부적합 | "인력 부족", "도구 미작동", "권한 없음" |
| 커뮤니케이션 부적합 | "공유 지연", "전달 누락", "오해" |

## 부적합 심각도 분류

> 출처: 9000 Store - Nonconformity and Corrective Action (https://the9000store.com/iso-9001-2015-requirements/iso-9001-2015-improvement/nonconformity-corrective-action/)
> *원문 인용: "A major nonconformance is classified when there is an absence or a complete breakdown in your QMS."*
> *한국어 의역: Major 부적합은 QMS의 부재 또는 완전한 붕괴가 있을 때 분류된다.*

| 분류 | 정의 | MMORPG 환경 예시 |
|------|------|------------------|
| Major | QMS 부재 또는 완전한 붕괴 | 전체 TC 작성 누락, 회귀 테스트 미실시, 검증 없이 라이브 |
| Minor | 일부 인원의 절차 미준수 (시스템 위협 없음) | 1건의 TC 결과 기록 누락, 1건의 일감 상태 누락 |

## 근본 원인 분석 (RCA) 기법

### 5 Why 기법

문제 → "왜 발생했나?" 5번 반복하여 근본 원인 도출.

```
문제: 라이브 핫픽스 발생 (보스 몬스터 데이터 오류)

Why 1: 데이터가 잘못 들어갔다 → 왜?
Why 2: 데이터 변경 후 검증이 없었다 → 왜?
Why 3: 데이터 검증 TC가 없었다 → 왜?
Why 4: 자동 생성 시 데이터 영역이 누락됐다 → 왜?
Why 5: TC 작성 표준에 데이터 영역이 명시되지 않았다 ← 근본 원인

CA 후보: TC 작성 표준에 데이터 영역 명시 추가
```

### Fishbone (Ishikawa) 기법

원인을 6개 카테고리(인력·방법·설비·재료·측정·환경)로 분류해 시각화.

본 환경에서는:
- **인력 (People)**: 인원 부족, 교육 부재
- **방법 (Method)**: 프로세스 결함
- **도구 (Tools)**: TC Team v2 한계, BTS 설정
- **데이터 (Data)**: 기획서 품질, 입력 데이터
- **측정 (Measurement)**: KPI 측정 누락
- **환경 (Environment)**: 라이브 변화, 외부 의존성

## ID 체계 (분기 기반 추적)

모든 부적합·CA·PA에는 분기 단위 연속 추적 가능한 ID를 부여합니다. qm-process-auditor의 Finding ID와 동일한 NC- 접두사를 공유합니다.

| 종류 | 형식 | 예시 |
|------|------|------|
| 부적합 (Nonconformity) | `NC-YYYYQQ-NNN` | `NC-2026Q2-001` |
| 시정 조치 (Corrective Action) | `CA-YYYYQQ-NNN` | `CA-2026Q2-001` |
| 예방 조치 (Preventive Action) | `PA-YYYYQQ-NNN` | `PA-2026Q2-001` |

**생성 규칙:**
1. **YYYYQQ**: 작성일이 속한 연·분기 (1~3월=Q1, 4~6월=Q2, 7~9월=Q3, 10~12월=Q4)
2. **NNN**: 해당 분기 내 직전 보고 마지막 순번 +1, 3자리 zero-padded
3. 분기 전환 시 `001`부터 다시 시작
4. 같은 분기에서 NC/CA/PA는 **각자 독립 순번** (예: NC-2026Q2-001, CA-2026Q2-001 동시 존재 가능). 단, qm-process-auditor의 Finding NC와는 NC 순번을 공유.
5. 한 부적합에 다수 CA가 매핑되면 같은 NC ID를 참조 (예: `NC-2026Q2-001` ← `CA-2026Q2-005`, `CA-2026Q2-006`)

**ID 발급 시 사전 확인:** 직전 보고 로그(`~/.claude/qm-state/qm-report-log.json`)에서 해당 분기 마지막 순번 조회. 부재 시 사용자 확인 후 `001`부터.

## CA / PA 작성 형식

```
## 부적합 NC-YYYYQQ-NNN: [요약]

**유형**: [프로세스/품질/문서/자원/커뮤니케이션]
**심각도**: [Major/Minor]
**근본 원인 (가설)**: [RCA 결과]
**증거**: [구체적 데이터·로그·인용]

### 시정 조치 (CA) — 재발 방지
- [ ] CA-YYYYQQ-NNN: [구체적 행동]
  - 매핑 부적합: NC-YYYYQQ-NNN
  - 담당자 후보: [...]
  - 권고 기한: [...]
  - 효과 측정 방법: [...]

### 예방 조치 (PA) — 잠재 위험 제거 (선택)
- [ ] PA-YYYYQQ-NNN: [구체적 행동]
  - 매핑 부적합: NC-YYYYQQ-NNN
  - 담당자 후보: [...]
  - 권고 기한: [...]
```

## 효과성 검증 (Effectiveness Verification)

> 출처: ISO 9001 Checklist - 10.2 Corrective Action Explained (https://www.iso-9001-checklist.co.uk/10.2-corrective-action.htm)
> *영문 원문 인용: "All ISO Management System Standards require that the effectiveness of Corrective Action be checked."*
> *한국어 의역: 모든 ISO 경영시스템 표준은 시정 조치의 효과성을 검증할 것을 요구한다.*

CA 적용 후 다음 회고에서 검증:

```
## 효과성 검증: CA-YYYYQQ-NNN

**원래 부적합**: NC-YYYYQQ-NNN [요약]
**적용된 CA**: [...]
**검증 방법**: [측정 데이터, KPI, 재발 여부]

**판정 기준**:
- 효과 있음: 부적합 재발 없음 + KPI 개선
- 부분 효과: 일부 개선 + 일부 잔존
- 효과 없음: 부적합 재발 + KPI 개선 없음
- 데이터 부족: 검증 불가

**결과**: [판정]
**다음 행동**: [추가 조치 필요 여부]
```

## CAPA 운영 권고 (할루시네이션 방지 명시)

> 다음은 외부 표준이 아닌 본 가이드의 운영 권고안입니다. 팀 합의로 조정 가능.

- **회고당 핵심 CA는 3개 이내**, **PA는 2개 이내**로 제한
  - 너무 많은 CA는 실행되지 않아 그 자체가 부적합이 됨
- **모든 CA는 측정 가능한 효과 검증 방법을 가져야 함**
- **3회 이상 회고에 걸쳐 반복되는 부적합은 별도 관리** (만성 부적합)

## 만성 부적합 (Chronic Nonconformity) 처리

3회 이상 동일·유사 부적합이 반복되면:

```
## 만성 부적합 알림

**부적합 유형**: [반복되는 유형]
**관찰 회수**: N회 (회고 #1, #2, #3)
**기존 CA들**: [효과 없었던 CA 목록]

**권고**:
1. 5 Why를 다시 처음부터 적용 (이전 분석이 깊지 않았을 가능성)
2. 시스템·도구·정책 차원의 변경 검토
3. 외부 자문 또는 더 큰 변경 (예: 도구 교체) 검토

이는 일반적 CA로 해결되지 않는 시스템적 문제일 수 있습니다.
```
