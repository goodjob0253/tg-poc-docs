---
id: ART-DES-PAYMENT-REFUND
type: design
title: 결제 시스템 환불 중복 응답 프로그램설계서
description: 주문 시스템의 중복 환불 응답을 판별하고 결제 취소 중복 생성을 방지한다.
version: "1.0"
status: WORKING
system: 결제 시스템
owner: 결제서비스팀
tags:
  - payment
  - refund
  - design
---

# 결제 시스템 프로그램설계서

## DES-PAYMENT-REFUND-409 중복 환불 응답 처리 설계

결제 연계 모듈은 주문 시스템의 HTTP 상태와 업무 오류 코드를 함께 확인해 중복 환불을 판별한다.

### 추적 관계

- 요구사항: `REQ-PAYMENT-REFUND-409`
- 요구사항: `REQ-REFUND-409`

### 처리 흐름

1. 주문 시스템에 환불을 요청한다.
2. HTTP 409와 `REFUND_ALREADY_PROCESSED`를 수신하면 결제 취소를 다시 실행하지 않는다.
3. 기존 결제 취소 상태를 조회한다.
4. 조회 결과를 표준 환불 상태로 변환해 반환한다.

### 예외 처리

- 오류 코드가 없거나 다른 코드이면 기존 처리로 간주하지 않는다.
- 상태 조회가 실패하면 자동 재처리하지 않고 확인 필요 상태로 남긴다.

