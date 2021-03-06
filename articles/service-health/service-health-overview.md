---
title: Service Health 개요 | Microsoft Docs
description: Azure 앱이 현재 및 향후 Azure 서비스 문제 및 유지 관리에 의해 어떤 영향을 받는지에 대한 개인 설정된 정보입니다.
ms.topic: conceptual
ms.date: 05/10/2019
ms.openlocfilehash: 2d98a909a45c9dd00b3174f495a15cd18ced11f9
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82146909"
---
# <a name="service-health-overview"></a>Service Health 개요

서비스 상태에서는 사용하는 영역에 있는 Azure 서비스의 상태를 추적하는 사용자 지정 가능한 대시보드를 제공합니다. 이 대시보드에서 지속적인 서비스 문제, 계획된 향후 유지 관리 또는 관련 상태 공지와 같은 활성 이벤트를 추적할 수 있습니다. 이벤트가 비활성화되면, 최대 90일 동안 상태 기록에 배치됩니다. 마지막으로 Service Health 대시보드를 사용하여 서비스 문제로 인해 영향이 발생할 경우 사전에 알리는 서비스 상태 경고를 만들고 관리할 수 있습니다.

## <a name="service-health-events"></a>Service Health 이벤트

Service Health는 리소스에 영향을 줄 수 있는 네 가지 유형의 상태 이벤트를 추적 합니다.

1. **서비스 문제** - 즉시 사용자에게 영향을 주는 Azure 서비스의 문제입니다. 
2. **계획된 유지 관리** - 나중에 서비스의 가용성에 영향을 줄 수 있는 예정된 유지 관리입니다.  
3. **상태 자문** - 주의가 필요한 Azure 서비스의 변경 내용입니다. Azure 기능을 사용 하지 않거나 업그레이드 요구 사항 (예: 지원 되는 PHP 프레임 워크로 업그레이드)을 예로 들 수 있습니다.
4. **보안 권고 (미리 보기)** -Azure 서비스의 가용성에 영향을 줄 수 있는 보안 관련 알림입니다.

> [!NOTE]
> Service Health 이벤트를 보려면 사용자에 게 구독에 대 한 [읽기 권한자 역할을 부여](../role-based-access-control/role-assignments-portal.md) 해야 합니다.

## <a name="get-started-with-service-health"></a>Service Health 시작

Service Health 대시보드를 시작하려면 포털 대시보드에서 Service Health 타일을 선택합니다. 이전에 타일을 제거했거나 사용자 지정 대시보드를 사용 중인 경우 “추가 서비스”(대시보드의 왼쪽 아래)에서 Service Health 서비스를 검색합니다.

![Service Health 시작](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>서비스에 영향을 주는 현재 문제 확인

**서비스 문제** 보기에는 리소스에 영향을 주고 있는 Azure 서비스의 진행 중인 문제가 표시됩니다. 문제가 시작된 시점 및 영향을 받는 서비스 및 지역을 파악할 수 있습니다. 또한 최신 업데이트를 읽어 문제를 해결하기 위해 Azure에서 수행 중인 내용을 파악할 수 있습니다. 

![서비스 문제 관리](./media/service-health-overview/azure-service-health-overview-2.png)

**잠재적 영향** 탭을 선택하여 소유한 리소스 중에서 문제의 영향을 받을 수 있는 특정 리소스 목록을 확인할 수 있습니다. 이러한 리소스의 CSV 목록을 다운로드하여 팀과 공유할 수 있습니다.

![서비스 문제 관리 - 영향](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="see-emerging-issues-which-may-impact-your-services"></a>서비스에 영향을 줄 수 있는 새로운 문제를 참조 하세요.

대상 통신이 영향을 받는 고객에 게 전송 되기 전에 광범위 한 서비스 문제가 [Azure 상태 페이지](https://status.azure.com) 에 게시 될 수 있는 경우가 있습니다. Azure Service Health에서 사용자에 게 영향을 줄 수 있는 문제에 대 한 포괄적인 보기를 제공 하도록 하기 위해 활성 Azure 상태 페이지 문제는 *새로운 문제로*Service Health에 표시 됩니다. Azure 상태 페이지에서 이벤트가 활성화 되 면 새로운 문제 배너가 Service Health에 표시 됩니다. 문제에 대 한 전체 정보를 보려면 배너를 클릭 합니다.

![새로운 서비스 문제](./media/service-health-overview/azure-service-health-emerging-issue.png)

## <a name="get-links-and-downloadable-explanations"></a>링크 및 다운로드할 수 있는 설명 가져오기 

문제 관리 시스템에서 사용할 문제의 링크를 가져올 수 있습니다. PDF 및 경우에 따라 CSV 파일을 다운로드 하 여 Azure Portal에 대 한 액세스 권한이 없는 사용자와 공유할 수 있습니다.   

![서비스 문제 관리 - 문제 관리](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Microsoft에서 지원 받기

문제가 해결된 후에도 리소스가 잘못된 상태로 남아 있는 경우 지원에 문의하세요.  페이지 오른쪽에 있는 지원 링크를 사용하세요.  

## <a name="pin-a-personalized-health-map-to-your-dashboard"></a>대시보드에 개인 설정된 상태 지도 고정

Service Health를 필터링하여 업무상 중요한 구독, 지역 및 리소스 종류를 표시합니다. 필터를 저장하고 개인 설정된 상태 세계 지도를 포털 대시보드에 고정합니다. 

![개인 설정된 상태 맵 필터링](./media/service-health-overview/azure-service-health-overview-6a.png)

![개인 설정된 상태 맵 핀 고정](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Service Health 경고 구성

Service Health는 Azure Monitor와 통합되어 업무상 중요한 리소스가 영향을 받는 경우 이일, 문자 메시지 및 웹후크 알림을 통해 경고합니다. 적절한 Service Health 이벤트에 대한 활동 로그 경고를 설정합니다. 작업 그룹을 사용하여 조직 내의 해당하는 사람들에게 해당 경고를 라우팅합니다. 자세한 내용은 [Service Health에 대한 경고 구성](../azure-monitor/platform/alerts-activity-log-service-notifications.md)을 참조하세요.

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="next-steps"></a>다음 단계

상태 문제 알림을 받도록 경고를 설정합니다. 자세한 내용은 [Azure Service Health 경고 설정 모범 사례](https://www.youtube.com/watch?v=k5d5ca8K6tc&list=PLLasX02E8BPBBSqygdRvlTnHfp1POwE8K&index=6&t=0s)를 참조 하세요. 
