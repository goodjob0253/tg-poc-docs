---
id: ART-REQ-REFUND
title: 환불 API 오류 처리 요구사항
type: requirement
version: "1.2"
status: WORKING
system: Order Platform
owner: 주문서비스팀
---
# 환불 API 오류 처리 요구사항

## REQ-REFUND-409 중복 환불 방지

이미 환불이 완료되었거나 환불 처리가 진행 중인 주문에 대해 다시 환불을 요청하면 시스템은 중복 처리를 방지해야 한다.

### 수용 기준

- 환불 완료 주문에 대한 재요청은 HTTP 409를 반환한다.
- 응답에는 `REFUND_ALREADY_PROCESSED` 오류 코드를 포함한다.
- 환불 처리 중인 주문에 대한 재요청도 동일한 정책을 적용한다.
- 중복 환불 요청은 새로운 환불 거래를 생성하지 않는다.
- HTTP 409 응답을 받은 호출자는 주문의 기존 환불 상태를 조회할 수 있어야 한다.
- 정상적인 최초 환불 요청의 기존 처리 방식은 변경하지 않는다.

### 변경관리 기준

- 본 요구사항의 변경 이력은 기본 브랜치에 머지된 commit을 기준으로 추적한다.

### 관련 산출물

- 상세 설계: `deliverables/design/refund-api.md`
- API 명세: `deliverables/resources/refund-api.yaml`
