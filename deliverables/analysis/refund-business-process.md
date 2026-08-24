---
id: ART-ANALYSIS-REFUND
title: 환불 업무 프로세스 분석서
type: business-analysis
version: "1.0"
status: WORKING
system: Order Platform
owner: 주문서비스팀
description: 정상 및 중복 환불 요청의 업무 흐름과 영향 범위 분석
---

# 환불 업무 프로세스 분석서

## 분석 목적

환불 요청의 정상 처리와 중복 요청 차단 흐름을 구분하고, HTTP 409 계약이 주문·환불·호출 시스템에 미치는 영향을 정의한다.

## 현재 업무 흐름

1. 호출 시스템이 주문의 환불을 요청한다.
2. 주문 서비스가 주문과 기존 환불 상태를 조회한다.
3. 환불 이력이 없으면 환불 거래를 생성하고 정상 처리를 시작한다.
4. 환불 완료 또는 처리 중 이력이 있으면 새 거래를 생성하지 않는다.
5. 중복 요청에는 HTTP 409와 `REFUND_ALREADY_PROCESSED`를 반환한다.
6. 호출 시스템은 기존 환불 상태 조회 API로 현재 상태를 확인한다.

## 업무 규칙

- 최초 환불 요청은 기존 정상 처리 절차를 유지한다.
- 동일 주문의 완료 또는 처리 중 환불은 중복 요청으로 판정한다.
- 중복 요청은 데이터 변경 없이 종료한다.
- 중복 응답은 호출자가 재시도와 상태 조회를 구분할 수 있도록 일관된 오류 코드를 제공한다.

## 영향 범위

| 영역 | 영향 | 확인 사항 |
| --- | --- | --- |
| 주문 서비스 | 중복 상태 판정 추가 | 동시 요청에서도 단일 환불 거래만 유지 |
| 환불 API | HTTP 409 계약 추가 | 오류 코드와 응답 스키마 일치 |
| 호출 시스템 | 409 처리 분기 추가 | 실패 재시도 대신 기존 상태 조회 |
| 테스트 | 정상·완료·처리 중 시나리오 추가 | 최초 환불 회귀 포함 |

## 관련 산출물

- 요구사항: `deliverables/requirements/refund.md`
- 요구사항 검토: `deliverables/reviews/requirements-review.md`
- 상세 설계: `deliverables/design/refund-api.md`
- API 명세: `deliverables/resources/refund-api.yaml`

