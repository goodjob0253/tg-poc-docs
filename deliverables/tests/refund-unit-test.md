---
id: ART-TEST-ORDER-REFUND
type: test
title: 주문 시스템 환불 단위테스트 케이스
description: 중복 환불의 HTTP 409 계약과 최초 요청 회귀를 검증한다.
version: "1.1"
status: WORKING
system: 주문 시스템
owner: 주문서비스팀
tags:
  - order
  - refund
  - unit-test
---

# 주문 시스템 단위테스트 케이스

## TEST-REFUND-409 중복 환불 충돌 응답 검증

중복 환불 판정과 정상 환불 회귀를 함께 검증한다.

### 추적 관계

- 요구사항: `REQ-REFUND-409`
- 설계: `DES-REFUND-409`

### 테스트 케이스

| 케이스 ID | 사전 조건 | 입력 | 기대 결과 |
| --- | --- | --- | --- |
| TC-REFUND-409-01 | 동일 멱등키 환불 완료 | 같은 주문·멱등키 재요청 | HTTP 409, `REFUND_ALREADY_PROCESSED`, 새 거래 없음 |
| TC-REFUND-409-02 | 동일 멱등키 환불 처리 중 | 같은 주문·멱등키 재요청 | HTTP 409, 새 거래 없음 |
| TC-REFUND-409-03 | 기존 환불 기록 없음 | 새 멱등키 요청 | HTTP 200, 환불 거래 1건 생성 |

### 통과 기준

- 세 테스트가 모두 통과한다.
- 테스트 실행 결과는 실제 commit SHA와 함께 TestEvidence로 등록한다.

### 실행 이력

- 2026-08-25: Rev.16 Golden Path 결정론적 데이터셋에서 전체 케이스 PASS를 확인했다.
