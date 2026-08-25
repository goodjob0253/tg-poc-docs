---
id: ART-REQ-REFUND
type: requirement
title: 환불 API 오류 처리 요구사항
description: 중복 환불 요청을 HTTP 409로 응답하고 기존 처리 결과를 보존한다.
version: "1.4"
status: WORKING
system: Order Platform
owner: 주문서비스팀
tags:
  - order
  - refund
  - api
resources:
  - resources/refund-api.yaml
---

# 환불 API 오류 처리 요구사항

## REQ-REFUND-409 중복 환불 요청 충돌 응답

동일한 멱등키로 이미 처리 중이거나 완료된 환불 요청이 다시 접수되면 HTTP 409 Conflict로 응답한다.

### 업무 규칙

- 중복 요청에서는 새로운 환불 거래를 생성하지 않는다.
- 응답 본문은 오류 코드 `REFUND_ALREADY_PROCESSED`를 포함한다.
- 최초 환불 요청의 기존 HTTP 200 계약은 유지한다.
- 호출 시스템은 HTTP 409 수신 후 재시도하지 않고 기존 환불 상태를 조회한다.

### 수용 조건

| ID | 조건 | 기대 결과 |
| --- | --- | --- |
| AC-REFUND-409-01 | 완료된 환불과 동일한 멱등키로 재요청 | HTTP 409와 `REFUND_ALREADY_PROCESSED` 반환 |
| AC-REFUND-409-02 | 처리 중인 환불과 동일한 멱등키로 재요청 | 새 거래 없이 HTTP 409 반환 |
| AC-REFUND-409-03 | 최초 멱등키로 환불 요청 | 기존 HTTP 200 정상 처리 유지 |

### 변경 이력

- 2026-08-25: Rev.16 Golden Path에서 주문·결제·정산 연계 추적 경로를 재검증했다.
