---
id: ART-DES-REFUND-API
type: design
title: 환불 API 상세 설계
description: 멱등키로 중복 환불을 판별하고 일관된 HTTP 409 오류 계약을 제공한다.
version: "1.1"
status: WORKING
system: Order Platform
owner: 주문서비스팀
tags:
  - order
  - refund
  - design
resources:
  - resources/refund-api.yaml
---

# 환불 API 상세 설계

## DES-REFUND-409 중복 환불 검증 및 응답 설계

환불 서비스는 요청의 멱등키를 기준으로 기존 처리 기록을 조회하고, 중복 요청에는 기존 결과를 보존한 채 HTTP 409를 반환한다.

### 추적 관계

- 요구사항: `REQ-REFUND-409`

### 처리 흐름

1. 요청의 멱등키와 주문 식별자를 검증한다.
2. 멱등키에 대응하는 기존 환불 처리 기록을 조회한다.
3. 기존 기록이 처리 중이거나 완료 상태이면 HTTP 409와 `REFUND_ALREADY_PROCESSED`를 반환한다.
4. 기록이 없으면 환불 거래를 생성하고 기존 정상 처리 흐름을 수행한다.

### 인터페이스 계약

| 구분 | 값 |
| --- | --- |
| 요청 | `POST /refunds` |
| 정상 응답 | HTTP 200 |
| 중복 응답 | HTTP 409 |
| 오류 코드 | `REFUND_ALREADY_PROCESSED` |
