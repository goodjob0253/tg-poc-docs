---
id: ART-DES-REFUND-API
title: 환불 API 상세 설계
type: design
version: "1.0"
status: WORKING
system: Order Platform
owner: 주문서비스팀
---

# 환불 API 상세 설계

## DES-REFUND-409 중복 환불 검증

환불 서비스는 환불 명령을 실행하기 전에 기존 환불 상태를 조회한다.

### 처리 흐름

1. 주문 식별자로 기존 환불 상태를 조회한다.
2. 상태가 `COMPLETED` 또는 `PROCESSING`인지 확인한다.
3. 중복 환불이면 새로운 환불 거래를 생성하지 않는다.
4. HTTP 409와 `REFUND_ALREADY_PROCESSED` 오류 코드를 반환한다.
5. 그 외 상태이면 정상 환불 처리를 계속한다.

### 추적 관계

- 요구사항: `REQ-REFUND-409`
- API: `POST /orders/{orderId}/refund`
- API 명세: `deliverables/resources/refund-api.yaml`

### 테스트 고려사항

- 최초 환불 요청은 정상 처리되어야 한다.
- 환불 완료 후 재요청은 HTTP 409를 반환해야 한다.
- 환불 처리 중 재요청은 HTTP 409를 반환해야 한다.
- 중복 요청으로 추가 환불 거래가 생성되지 않아야 한다.
