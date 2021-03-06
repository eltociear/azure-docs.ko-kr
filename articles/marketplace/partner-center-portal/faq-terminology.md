---
title: 파트너 센터의 상용 마켓플레이스 분석에 대한 자주 묻는 질문 및 용어
description: 상용 마켓플레이스 분석에 대해 자주 묻는 질문을 해결하는 방법을 알아보세요. 분석 용어에 대한 데이터 사전을 포함합니다.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: b7367e58de818c20723c02a6763b1bf1e3b18f24
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81251829"
---
# <a name="frequently-asked-questions-and-terminology-for-commercial-marketplace-analytics"></a>상용 마켓플레이스 분석을 위해 자주 묻는 질문 및 용어

이 문서에서는 파트너 센터의 분석 메시지에 대한 자주 묻는 질문을 다루며 분석 용어 사전도 제공합니다.

## <a name="frequently-asked-questions"></a>질문과 대답

이 섹션에서는 파트너 센터에서 **사용 가능한 아직 사용할 수 없는** 메시지에 대해 자주 묻는 질문에 대한 답변을 제공합니다.

**파트너 센터에서 내 분석 데이터를 볼 수 없습니다. 이 페이지에 액세스하면 아래 메시지가 표시됩니다. 왜 그럴까요?**

![아직 오퍼에 대한 데이터가 없습니다.](./media/analytics-faq-no-data.png)

이 메시지가 있는 이유:

- 마켓플레이스에서 게시된 오퍼에 대한 인수는 현재 존재하지 않습니다. 즉, 제품이 마켓플레이스에 게재되어 제품 표시 페이지에서 고객의 조회수를 얻고 있지만 고객이 아직 구매 및 배포를 위한 조치를 취하지 않았음을 의미할 수 있습니다.
- 오퍼의 게시가 진행 중일 수 있으며 아직 게재되지 않았습니다. 라이브 오퍼만 고객이 획득할 수 있습니다. 오퍼의 상태를 확인하려면 [분석 대시보드의](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)개요를 참조하십시오. 자세한 내용은 [상용 마켓플레이스 분석의 요약 대시보드를](./summary-dashboard.md)참조하십시오.
- 귀하의 제안은 목록 전용 제안이며 마켓플레이스의 고객이 구입할 수 없는 **연락처 나(Contact Me)로**나열될 수 있습니다. 이러한 오퍼는 잠재 고객을 생성하고 사용자와 공유되지만 구매가 수 없으므로 이러한 오퍼에 대한 주문은 생성되지 않습니다. 쿠폰 목록 유형을 확인하려면 설정 페이지로 이동하세요.

**분석 데이터가 있다는 것을 알고 있지만 아래 메시지가 나타납니다.**

![지정된 날짜 범위에 대한 데이터가 없습니다.](./media/analytics-faq-data-range.png)

이 메시지를 받는 경우 분석 데이터가 있지만 선택한 날짜에 대한 데이터가 없다는 의미입니다. 2010년 이후의 데이터를 보려면 다른 날짜 범위 또는 사용자 지정 날짜 범위를 선택합니다. 자세한 내용은 [상용 마켓플레이스 분석에서 요약 대시보드의](./summary-dashboard.md)날짜 범위 섹션을 참조하십시오.

## <a name="dictionary-of-data-terms"></a>데이터 용어 사전

| 특성 이름 | 보고서 | 정의|
|---|---|---|
| Azure 라이선스 유형 | 고객, 주문 | Azure 구매 고객이 체결하는 라이선싱 계약 유형입니다. 채널이라고도 함 |
| Azure 라이선스 유형: 클라우드 솔루션 공급자 | 고객, 주문 | 재판매인 역할을 하는 클라우드 솔루션 공급자를 통해 최종 고객이 Azure 및 Marketplace 제품을 조달받습니다.|
| Azure 라이선스 유형: 기업 | 고객, 주문 | Microsoft와 직접 체결하는 기업계약을 통해 최종 고객이 Azure 및 Marketplace 제품을 조달받습니다.|
| Azure 라이선스 유형: 기업(재판매인 있음)  | 고객, 주문 | 최종 고객은 Microsoft와의 엔터프라이즈 계약을 용이하게 하는 리셀러를 통해 Azure 및 Marketplace 오퍼를 조달합니다.|  |
| Azure 라이선스 유형: 종량제| 고객, 주문 | 최종 고객은 Microsoft와 직접 서명한 "Pay as You Go" 계약을 통해 Azure 및 마켓플레이스 오퍼를 조달합니다.||
| 클라우드 인스턴스 이름| 주문| VM이 배포된 Microsoft Cloud입니다.||
| 클라우드 인스턴스 이름: Azure 전역| 주문| 퍼블릭 글로벌 마이크로소프트 클라우드.|| |
| 클라우드 인스턴스 이름: Azure 정부 | 주문| 중국, 독일 또는 미국 중 하나에 대 한 정부 별 Microsoft 클라우드입니다.| |
| 고객 구/군/시| Customer| 고객이 제공한 도시 이름입니다. 도시는 고객의 Azure 구독에서 도시와 다를 수 있습니다.||
| 고객 통신 언어  | Customer| 고객이 통신용으로 선택한 기본 설정 언어입니다.||
| 고객 회사 이름 | 고객, 주문 | 고객이 제공한 회사 이름입니다. 이름은 고객의 Azure 구독에서 도시와 다를 수 있습니다.|  |
| 고객 국가 | 고객, 주문 | 고객이 제공한 국가 이름입니다. 국가 고객의 Azure 구독에서 국가 와 다를 수 있습니다.|  |
| 고객 이메일| Customer| 최종 고객이 제공한 전자 메일 주소입니다. 전자 메일은 고객의 Azure 구독의 전자 메일 주소와 다를 수 있습니다.||
| 고객 이름| Customer| 고객이 제공한 이름입니다. 이름은 고객의 Azure 구독에 제공된 이름과 다를 수 있습니다.| |
| 고객 ID | 고객, 주문 | 고객에게 할당된 고유 식별자입니다. 고객은 Azure 마켓플레이스 구독이 0개 이상일 수 있습니다.|  |
| 고객 우편 번호  | Customer| 고객이 제공한 우편 번호입니다. 코드는 고객의 Azure 구독에 제공된 우편 번호와 다를 수 있습니다.| |
| 고객 시/도| Customer| 고객이 제공한 상태(주소)입니다. 상태는 고객의 Azure 구독에 제공된 상태와 다를 수 있습니다.| |
| 취득일| Customer| 고객이 귀하가 게시한 모든 쿠폰을 구매한 첫 번째 날짜입니다.| |
| 취소일| Customer| 고객이 이전에 구매한 모든 오퍼의 마지막 을 취소한 마지막 날짜입니다.||
| 새로운 고객입니다.  | 주문| 이 가치는 처음으로 하나 이상의 오퍼를 획득한 신규 고객을 식별합니다(또는 그렇지 않음). "취득한 날짜"의 경우 같은 달 내에 값이 "예"가 됩니다. 고객이 보고된 달 이전에 쿠폰을 구매한 경우 값은 "아니요"가 됩니다. |
| 미리 보기 SKU입니다.| 주문| 이 값은 SKU에 "미리 보기"로 태그를 지정한 경우 알려드립니다. SKU에 따라 태그가 지정된 경우 값은 "예"가 되며 이 이미지를 배포하고 사용할 수 있는 권한이 지정된 Azure 구독만 사용할 수 있습니다. SKU가 "미리 보기"로 식별되지 않은 경우 값은 "아니요"가 됩니다.  |
| 프로모션 연락처 옵트인| Customer| 이 값을 통해 고객이 게시자의 프로모션 연락처를 사전에 선택했는지 확인할 수 있습니다. 현재는 고객에게 해당 옵션이 제공되지 않으므로 보드 전체에 “아니요”가 표시됩니다. 이 기능이 배포되면 그에 따라 업데이트를 시작할 예정입니다.|
| Marketplace 라이선스 유형| 주문| Marketplace 제품의 청구 방법입니다.||
| Marketplace 라이선스 유형: Azure를 통해 청구됨| 주문| Microsoft가 이 Marketplace 제품의 대리인으로 게시자 대신 고객에게 대금을 청구합니다. (종량제 신용 카드 또는 기업 송장)||
| Marketplace 라이선스 유형: BYOL(사용자 라이선스 필요) | 주문| VM을 배포하려면 고객이 제공하는 라이센스 키가 필요합니다. Microsoft는 마켓플레이스를 통해 이러한 방식으로 제품을 나열하는 것에 대해 고객에게 요금을 청구하지 않습니다.||
| Marketplace 라이선스 유형: 무료| 주문| 이 오퍼는 모든 사용자가 무료로 사용할 수 있도록 구성되어 있습니다. Microsoft는 고객에게 이 제품의 사용에 대해 요금을 청구하지 않습니다.||
| Marketplace 라이선스 유형: Microsoft - 재판매인  | 주문| Microsoft가 이 Marketplace 제품의 재판매인 역할을 합니다.|  |
| 마켓플레이스 구독 ID | 고객, 주문 | 고객이 마켓플레이스 오퍼를 구입하는 데 사용한 Azure 구독과 연결된 고유 식별자입니다. ID는 이전에 Azure 구독 GUID였습니다.||
| 제품 이름  | 주문| 마켓플레이스 오퍼링의 이름입니다.|| |
| 제품 유형  | 주문| Microsoft 마켓플레이스 오퍼링의 유형입니다.|||
| 제품 유형: 관리형 애플리케이션  | 순서 | Azure 앱 사용: 다음 조건이 필요한 경우 관리되는 앱 제품 유형: VM 또는 전체 IaaS 기반 솔루션을 사용하여 고객을 위한 구독 기반 솔루션을 배포합니다. 사용자 또는 고객은 파트너가 솔루션을 관리해야 합니다. |
| 오퍼 유형: Azure 응용 프로그램| 순서 | 솔루션에 간단한 VM 을 초과하는 추가 배포 및 구성 자동화가 필요한 경우 Azure Application 솔루션 제공 유형을 사용합니다.||
| 오퍼 유형: 컨설팅 서비스| 주문| Azure Marketplace의 컨설팅 서비스는 고객과 서비스를 연결하여 고객의 Azure 사용을 지원하고 확대하는 데 유용합니다.| |
| 오퍼 유형: 컨테이너 | 주문| 솔루션이 Kubernetes 기반 Azure Container Service로 프로비전된 Docker 컨테이너 이미지인 경우 컨테이너 제품 유형을 사용합니다.||
| 오퍼 유형: Dynamics 365 비즈니스 센트럴| 주문| 재무 및 운영을 위해 솔루션이 Dynamics 365와 통합된 경우 이 오퍼 유형을 사용하십시오.| |
| 오퍼 유형: 고객 참여를 위한 Dynamics 365 | 주문| 고객 참여를 위해 솔루션이 Dynamics 365와 통합된 경우 이 오퍼 유형을 사용합니다.||
| 오퍼 타입: IoT 에지 모듈 | 주문| Azure IoT Edge 모듈은 IoT Edge에서 관리하는 가장 작은 계산 단위이며 Microsoft 서비스(예: Azure Stream Analytics), 타사 서비스 또는 자체 솔루션별 코드를 포함할 수 있습니다. |
| 오퍼 유형: 전원 BI 응용 프로그램 | 주문| Power BI와 통합된 응용 프로그램을 배포할 때 Power BI 응용 프로그램 제공 유형을 사용합니다.|  |
| 오퍼 유형: SaaS 애플리케이션| 주문| SaaS 앱 제품 유형을 사용하면 고객이 SaaS 기반, 기술 솔루션을 구독으로 구매할 수 있습니다.||
| 오퍼 유형: 가상 머신 | 주문| 고객과 연결된 구독에 가상 어플라이언스를 배포할 경우 가상 머신 제품 형식을 사용합니다.||
| 오퍼 유형: 비주얼 스튜디오 마켓플레이스 확장  | 주문| Azure DevOps 확장 개발자가 이전에 사용할 수 있는 오퍼 유형입니다. 앞으로 Azure DevOps 확장 개발자는 고객에게 직접 확장을 판매할 수 있습니다. 확장 혜택은 유료 또는 평가판 포함으로 구성할 수 있습니다. |
| 주문 취소 날짜| 주문| Marketplace 주문이 취소된 날짜입니다.||
| 주문 ID| 주문| 마켓플레이스 서비스에 대한 고객 주문의 고유 식별자입니다. 가상 컴퓨터 사용 기반 오퍼는 주문과 연결되지 않습니다.| |
| 주문 구매 날짜| 주문| Marketplace 주문이 생성된 날짜입니다.|||
| 주문 상태| 주문| 데이터를 마지막으로 새로 고칠 때의 Marketplace 주문 상태입니다.|     |
| 주문 상태: 활성  | 주문| 고객이 주문을 구매했으며 주문을 취소하지 않았습니다.|         |
| 주문 상태: 취소됨 | 주문| 고객은 이전에 주문을 구매한 후 주문을 취소했습니다.||
| 공급자 이메일| Customer| Microsoft와 최종 고객 간의 관계에 관련된 공급자의 전자 메일 주소입니다. 고객이 리셀러를 통해 엔터프라이즈인 경우 이 리셀러가 됩니다. CSP(클라우드 솔루션 공급자)가 관련된 경우 CSP가 됩니다.|
| Provider Name| Customer| Microsoft와 최종 고객 간의 관계에 관련된 공급자의 이름입니다. 고객이 리셀러를 통해 엔터프라이즈인 경우 이 리셀러가 됩니다. CSP(클라우드 솔루션 공급자)가 관련된 경우 CSP가 됩니다.|
| SKU| 주문| 게시 중에 정의한 SKU 이름입니다. 한 제품에 여러 SKU가 포함될 수는 있지만 각 SKU는 제품 하나에만 연결할 수 있습니다.||
| 평가판 종료 날짜| 주문| 이 주문의 평가 기간이 종료되었거나 종료될 예정인 날짜입니다.||

## <a name="next-steps"></a>다음 단계

- 파트너 센터 상용 마켓플레이스에서 사용할 수 있는 분석 보고서에 대한 개요는 [파트너 센터의 상용 마켓플레이스에 대한 애널리틱스를](./analytics.md)참조하세요.
- 오퍼의 마켓플레이스 활동을 요약하는 집계 데이터의 그래프, 추세 및 값은 [상용 마켓플레이스 분석의 요약 대시보드를](./summary-dashboard.md)참조하십시오.
- 그래픽 및 다운로드 가능한 형식으로 주문에 대한 자세한 내용은 [상용 마켓플레이스 분석의 주문 대시보드를](./orders-dashboard.md)참조하십시오.
- VM(가상 머신)은 사용량 및 유료 청구 메트릭을 제공하므로 [상용 마켓플레이스 분석에서 사용 대시보드를](./usage-dashboard.md)참조하십시오.
- 성장 추세를 포함하여 고객에 대한 자세한 내용은 [상용 마켓플레이스 분석의 고객 대시보드를](./customer-dashboard.md)참조하십시오.
- 지난 30일 동안의 다운로드 요청 목록은 [상용 마켓플레이스 분석의 다운로드 대시보드를](./downloads-dashboard.md)참조하십시오.
- Azure 마켓플레이스 및 AppSource에서 제안에 대한 고객 피드백에 대한 통합보기를 보려면 [상용 마켓플레이스 분석에서 등급 및 리뷰 대시보드를](./ratings-reviews.md)참조하십시오.
