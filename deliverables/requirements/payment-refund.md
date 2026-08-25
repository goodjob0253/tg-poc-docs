---
id: ART-REQ-PAYMENT-REFUND
type: requirement
title: 결제 시스템 환불 중복 응답 처리 요구사항
description: 주문 시스템의 중복 환불 응답을 수용하고 결제 취소를 중복 수행하지 않는다.
version: "1.0"
status: WORKING
system: 결제 시스템
owner: 결제서비스팀
tags:
  - payment
  - refund
  - api
---

# 결제 시스템 요구사항 정의서

## REQ-PAYMENT-REFUND-409 중복 환불 응답 수용

주문 시스템이 중복 환불 요청에 HTTP 409를 반환하면 결제 시스템은 이를 재처리 대상이 아닌 기존 처리 결과로 취급한다.

### 업무 규칙

- HTTP 409와 `REFUND_ALREADY_PROCESSED` 조합만 기존 처리 결과로 인정한다.
- 동일 결제 건에 대한 취소 거래를 추가 생성하지 않는다.
- 기존 결제 취소 상태를 조회해 호출자에게 반환한다.
- 그 밖의 HTTP 409 응답은 자동 수용하지 않고 오류로 처리한다.

### 수용 조건

| ID | 조건 | 기대 결과 |
| --- | --- | --- |
| AC-PAYMENT-409-01 | 주문 시스템이 중복 환불 오류 반환 | 추가 결제 취소 없이 기존 상태 조회 |
| AC-PAYMENT-409-02 | 다른 오류 코드의 HTTP 409 반환 | 일반 오류 처리 및 확인 필요 기록 |

