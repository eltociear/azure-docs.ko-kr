---
title: 파트너 센터의 상용 마켓플레이스 분석의 고객 대시보드
description: 상용 마켓플레이스 분석에서 고객 대시보드를 사용하여 성장 추세를 비롯한 고객에 대한 정보에 액세스하는 방법을 알아봅니다.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: a8379ed883311d219bb6eeb56bd4424dfb470bc9
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81251642"
---
# <a name="customer-dashboard-in-commercial-marketplace-analytics"></a>상용 마켓플레이스 분석의 고객 대시보드

이 문서에서는 파트너 센터의 **고객 대시보드에** 대한 정보를 제공합니다. 이 대시보드에는 성장 추세를 비롯한 고객에 대한 정보가 그래픽 및 다운로드 가능한 형식으로 표시됩니다.

**고객 대시보드에**액세스하려면 상용 마켓플레이스에서 **[분석](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)** 대시보드를 엽니다.

>[!NOTE]
> 분석 용어에 대한 자세한 정의는 [상용 마켓플레이스 분석을 위한 자주 묻는 질문 및 용어를](./faq-terminology.md)참조하십시오.

## <a name="customer-dashboard"></a>고객 대시보드

**분석** 메뉴의 **고객 대시보드에는** 오퍼를 획득한 고객에 대한 데이터가 표시됩니다. 다음 항목의 그래픽 표현을 볼 수 있습니다.

- [고객 요약](#customer-summary)
- [지역별 고객](#customer-by-geography)
- [고객 동향](#customer-trends)
- [주문 및 사용량에 따라 고객](#customers-by-orders-and-usage)
- [SUS고객](#customers-by-skus)
- [고객 유형별 주문 및 사용량](#orders-and-usage-by-customer-type)
- [고객 세부 정보 표](#customer-details-table)
- [고객 페이지 필터](#customer-page-filters)

### <a name="customer-summary"></a>고객 요약

고객 요약 섹션에는 선택한 기간 동안 신규, 기존 및 이탈을 포함한 모든 고객수가 표시됩니다.

- 총 고객은 쿠폰을 구매하고 취소되지 않은 주문을 하나 이상 받은 모든 고객의 수로 정의됩니다.
- 전월과 비교한 성장률은 녹색 또는 하향 표시기의 숫자와 상향 표시기로 표시됩니다.
- 성장 추세는 막대 그래프로 표시되며 차트의 열 위로 마우스를 가져가서 매월 값을 표시합니다.

세 가지 **고객 유형이**있습니다: 신규, 기존 및 이탈.

- 신규 고객이 선택한 달 내에 처음으로 하나 이상의 오퍼를 획득했습니다.
- 기존 고객이 선택한 달 이전에 하나 이상의 오퍼를 획득했습니다.
- 이탈한 고객이 이전에 구매한 모든 오퍼를 취소했습니다.

### <a name="customer-by-geography"></a>지역별 고객

**지역별 고객** 차트는 선택한 기간 동안 획득한 모든 고객 및 고객의 수를 표시하며 고객 국가를 기반으로 매핑됩니다. 맵의 밝은 색상에서 어두운 색상은 고객 수의 낮은 값에서 높은 값을 나타냅니다. 표의 레코드를 클릭하여 국가를 확대합니다.

히트맵에는 고객 국가별 고객 수와 %가 표시됩니다. 맵을 이동하여 정확한 위치를 보고 특정 위치로 확대할 수 있습니다. 이 맵에는 해당 위치에 새로 추가된 고객뿐만 아니라 위치별로 고객의 %를 볼 수 있는 추가 그리드가 있습니다.

### <a name="customer-trends"></a>고객 동향

**고객 추세** 차트에는 월별 성장 추세와 함께 신규, 기존 및 이탈을 포함한 모든 고객의 수가 표시됩니다.

- 표선 차트는 전체 고객 증가율을 나타냅니다.
- 월 열은 신규, 기존 및 이탈한 고객이 누적한 고객 수를 나타냅니다.
- 변동된 고객 수는 Y축의 음수 방향에 표시됩니다.
- 특정 범례 항목을 선택하여 보다 자세한 뷰를 표시할 수 있습니다. 예를 들어 범례에서 새 고객을 선택하여 새 고객만 표시합니다.
- 차트 상단의 슬라이더를 사용하여 x축에서 오른쪽과 왼쪽으로 스크롤하고 특정 데이터 포인트에 초점을 맞추어 더 자세히 볼 수 있습니다.
- 차트의 열 위로 마우스를 가져가면 해당 월에 대한 세부 정보만 표시됩니다.

### <a name="customers-by-orders-and-usage"></a>주문 및 사용량에 따라 고객

**주문/사용 차트별 고객에는** "주문", "정규화 된 사용"및 "원시 사용량"의 세 가지 탭이 있습니다. **상위 고객 백분위수는** 주문 수에 따라 결정되는 x축을 따라 표시됩니다. y축은 고객의 주문 수를 표시합니다. 보조 y축(선 그래프)은 총 주문 수의 누적 백분율을 표시합니다. 표선 차트를 따라 점 위로 마우스를 가져가서 세부 정보를 표시할 수 있습니다.

예를 들어 정규화된 사용량은 아래 차트를 참조하십시오: 상위 30번째 백분위수 고객은 정규화된 사용량의 87%를 누적하여 기여하고 있습니다. 고객의 30번째 백분위수는 157만 시간의 사용에 불과합니다.

### <a name="customers-by-skus"></a>SUS고객

**SCO/사용 차트별 고객** 설명은 다음과 같습니다.

1. 리더 보드는 주문 수에 따라 순위가 매겨진 상위 50개 고객에 대한 세부 정보를 제공합니다. 고객을 선택한 후, 고객의 세부 사항은 이 리더 보드의 섹션 2, 3 및 4에 표시됩니다.
2. 게시자가 소유자 역할로 로그인하면 고객 프로필 세부 정보가 이 공간에 표시됩니다. 게시자가 기여자 역할로 로그인한 경우 이 섹션의 세부 정보를 사용할 수 없습니다.
3. SCO 도넛 차트의 주문에는 SCO에 대해 구매한 주문내역이 표시됩니다. 주문 수가 가장 많은 상위 5개 SCO가 표시되고 나머지 주문은 '모두 나머지'로 그룹화됩니다.
4. SCO 도넛 차트의 시트는 SCO에 주문한 시트의 내역을 표시합니다. 가장 높은 좌석을 가진 상위 5개의 SCO가 표시되고 나머지 주문은 모두 나머지 로 그룹화됩니다.

### <a name="orders-and-usage-by-customer-type"></a>고객 유형별 주문 및 사용량

**고객 유형별 주문/사용량** 차트에는 주문 수, 정규화된 사용량 및 신규 고객과 기존 고객 간에 분할된 원시 사용 시간이 표시됩니다. 차트에서 각각 주문, 정규화 및 원시 사용량을 선택합니다.

- 새 고객이 같은 달(y축) 내에 처음으로 오퍼 또는 보고된 사용량 중 하나 이상을 획득했습니다.
- 기존 고객이 이전에 사용자로부터 제안을 획득했거나 역월(y축)에 보고된 달 이전에 사용량을 보고한 것입니다.

### <a name="customer-details-table"></a>고객 세부 정보 표

**고객 세부 정보** 테이블에는 처음 쿠폰 중 하나를 획득한 날짜별로 정렬된 상위 1000명의 고객 의 번호가 매겨진 목록이 표시됩니다.

- 고객 의 개인 정보는 고객이 동의한 경우에만 사용할 수 있습니다. 사용 권한의 소유자 역할 수준으로 로그인한 경우에만 이 정보를 볼 수 있습니다. 사용자 역할 및 사용 권한에 대해 자세히 알아보세요.
- 그리드의 각 열은 정렬할 수 있습니다.
- 레코드 수가 1000개 미만이면 데이터를 TSV 파일로 추출할 수 있습니다.
- 레코드 번호가 1000개를 초과하면 내보낸 데이터는 다음 30일 동안 다운로드 페이지에 비동기적으로 배치됩니다.
- 필터를 테이블에 적용하여 관심 있는 데이터만 표시할 수 있습니다. 데이터는 회사 이름, 고객 ID, 마켓플레이스 구독 ID, Azure 라이선스 유형, 획득 날짜, 분실날짜, 고객 이메일, 고객 국가/주/도시/우편, 고객 언어 등으로 필터링할 수 있습니다.

### <a name="customer-page-filters"></a>고객 페이지 필터

**고객 페이지** 필터는 고객 페이지 수준에서 적용됩니다. 여러 필터를 선택하여 '상세 주문 데이터' 그리드/내보내기에서 보려는 기준과 보려는 데이터에 대한 차트를 렌더링할 수 있습니다. 필터는 주문 페이지의 오른쪽 상단 모서리에서 선택한 데이터 범위에 대해 추출된 데이터에 적용됩니다.

>[!NOTE]
> 고객 그리드, 페이지 필터 및 가능한 선택 항목의 각 필드에 대한 자세한 정의는 [상용 마켓플레이스 분석을 위한 자주 묻는 질문과 용어에](./faq-terminology.md)있습니다.

## <a name="next-steps"></a>다음 단계

- 파트너 센터 상용 마켓플레이스에서 사용할 수 있는 분석 보고서에 대한 개요는 [파트너 센터의 상용 마켓플레이스에 대한 애널리틱스를](./analytics.md)참조하세요.
- 오퍼의 마켓플레이스 활동을 요약하는 집계 데이터의 그래프, 추세 및 값은 [상용 마켓플레이스 분석의 요약 대시보드를](./summary-dashboard.md)참조하십시오.
- 그래픽 및 다운로드 가능한 형식으로 주문에 대한 자세한 내용은 [상용 마켓플레이스 분석의 주문 대시보드를](./orders-dashboard.md)참조하십시오.
- VM(가상 머신)은 사용량 및 유료 청구 메트릭을 제공하므로 [상용 마켓플레이스 분석에서 사용 대시보드를](./usage-dashboard.md)참조하십시오.
- 지난 30일 동안의 다운로드 요청 목록은 [상용 마켓플레이스 분석의 다운로드 대시보드를](./downloads-dashboard.md)참조하십시오.
- Azure 마켓플레이스 및 AppSource에서 제안에 대한 고객 피드백에 대한 통합보기를 보려면 [상용 마켓플레이스 분석에서 등급 및 리뷰 대시보드를](./ratings-reviews.md)참조하십시오.
- 상용 마켓플레이스 분석 및 포괄적인 데이터 용어 사전에 대한 자주 묻는 질문은 [상용 마켓플레이스 분석에 대한 자주 묻는 질문 및 용어를](./faq-terminology.md)참조하십시오.
