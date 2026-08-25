---
id: ART-TEST-PAYMENT-REFUND
type: test
title: 결제 시스템 환불 중복 응답 단위테스트 케이스
description: 중복 환불 응답 수용과 결제 취소 중복 방지를 검증한다.
version: "1.1"
status: WORKING
system: 결제 시스템
owner: 결제서비스팀
tags:
  - payment
  - refund
  - unit-test
---

# 결제 시스템 단위테스트 케이스

## TEST-PAYMENT-REFUND-409 결제 취소 중복 방지 검증

주문 시스템의 중복 환불 응답이 결제 시스템에서 안전하게 처리되는지 검증한다.

### 추적 관계

- 요구사항: `REQ-PAYMENT-REFUND-409`
- 설계: `DES-PAYMENT-REFUND-409`

### 테스트 케이스

| 케이스 ID | 입력 | 기대 결과 |
| --- | --- | --- |
| TC-PAYMENT-409-01 | HTTP 409와 `REFUND_ALREADY_PROCESSED` | 추가 취소 없음, 기존 취소 상태 조회 |
| TC-PAYMENT-409-02 | HTTP 409와 미등록 오류 코드 | 확인 필요 오류, 자동 수용 없음 |
| TC-PAYMENT-409-03 | HTTP 200 | 기존 정상 처리 유지 |

### 통과 기준

- 세 테스트가 모두 통과한다.
- 추가 결제 취소 호출이 0건임을 TestEvidence에 포함한다.

### 실행 이력

- 2026-08-25: Rev.16 Golden Path 결정론적 데이터셋에서 전체 케이스 PASS를 확인했다.
