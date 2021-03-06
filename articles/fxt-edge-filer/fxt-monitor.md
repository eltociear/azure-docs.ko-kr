---
title: Azure FXT Edge Filer 모니터링
description: Azure FXT Edge 파일러 하이브리드 저장소 캐시의 하드웨어 상태를 모니터링하는 방법
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: conceptual
ms.date: 06/20/2019
ms.author: rohogue
ms.openlocfilehash: 3f422339af2040ad81c585c0e193e6cb3667b135
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72254876"
---
# <a name="monitor-azure-fxt-edge-filer-hardware-status"></a>Azure FXT 에지 파일러 하드웨어 상태 모니터링

Azure FXT Edge Filer 하이브리드 저장소 캐시 시스템에는 섀시에 여러 상태 표시등이 내장되어 관리자가 하드웨어의 작동 방식을 이해하는 데 도움이 됩니다.

## <a name="system-health-status"></a>시스템 상태

더 높은 수준에서 캐시 작업을 모니터링하려면 [제어판 대시보드 가이드에](https://azure.github.io/Avere/legacy/dashboard/4_7/html/ops_dashboard_index.html) 설명된 대로 소프트웨어 제어판의 **대시보드** 페이지를 사용합니다.

## <a name="hardware-status-leds"></a>하드웨어 상태 LED

이 섹션에서는 Azure FXT Edge Filer 하드웨어에 내장된 다양한 상태 표시등을 설명합니다.

### <a name="hard-drive-status-leds"></a>하드 드라이브 상태 LED

![하드 드라이브 전면, 수평, 콜아웃 레이블 2(왼쪽 위 모서리), 1(왼쪽 아래 모서리), 3(오른쪽) 그림](media/fxt-monitor/fxt-drive-callouts.png)

각 드라이브 캐리어에는 활동 표시기(1)와 상태 표시기(2)의 두 가지 상태 LED가 있습니다. 

* 드라이브가 사용 중일 때 활동 LED(1)가 켜집니다.  
* 상태 LED(2)는 아래 표의 코드를 사용하여 드라이브의 상태를 나타냅니다.

| 드라이브 상태 LED 상태              | 의미  |
|-------------------------------------|----------------------------------------------------------|
| 초당 녹색으로 두 번 깜박임      | 드라이브 *식별 또는* <br> 제거를 위한 드라이브 준비  |
| 꺼기(점등 되지 않은 경우)                         | 시스템이 시작이 완료되지 *않았거나* <br>드라이브를 제거할 준비가 되었습니다. |
| 녹색, 황색 및 꺼미로 깜박임       | 드라이브 실패 예측   |
| 초당 4회 황색으로 깜박임 | 드라이브 에 실패   |
| 단색 녹색                         | 드라이브가 온라인 상태입니다. |

드라이브의 오른쪽(3)에는 드라이브용량 및 기타 정보가 표시됩니다.

드라이브 번호는 드라이브 사이의 공간에 인쇄됩니다. Azure FXT Edge Filer에서 드라이브 0은 왼쪽 위 드라이브이며 드라이브 1은 바로 아래에 있습니다. 번호 매기기는 해당 패턴에서 계속됩니다. 

![드라이브 번호 및 용량 레이블을 보여 주는 FXT 섀시에 있는 한 하드 드라이브 베이의 사진](media/fxt-drives-photo.png)

## <a name="left-control-panel"></a>왼쪽 제어판

좌측 전면 제어판에는 다양한 상태 LED 표시기(1)와 대형 조명 시스템 상태 표시기(2)가 있습니다. 

![왼쪽 상태 패널, 왼쪽에 1개의 레이블 상태 표시기, 오른쪽에 큰 시스템 상태 표시등 표시등 2개](media/fxt-monitor/fxt-control-panel-left.jpg)

### <a name="control-panel-status-indicators"></a>제어판 상태 표시기 

왼쪽의 상태 표시시기에 해당 시스템에 오류가 있는 경우 단색 황색 표시등이 표시됩니다. 아래 표는 오류에 대한 가능한 원인과 해결 에 대한 해결 에 대해 설명합니다. 

이러한 해결 솔루션을 시도한 후에도 오류가 계속 발생하면 [지원팀에 문의하여](fxt-support-ticket.md) 도움을 요청하십시오. 

| 아이콘 | 설명 | 오류 조건 | 가능한 해결 방법 |
|----------------|---------------|--------------------|----------------------|
| ![드라이브 아이콘](media/fxt-monitor/fxt-hd-icon.jpg) | 드라이브 상태 | 드라이브 오류 | 시스템 이벤트 로그를 확인하여 드라이브에 오류가 있는지 확인하거나 <br>적절한 온라인 진단 테스트를 실행합니다. 시스템을 다시 시작하고 임베디드 진단(ePSA)을 실행하거나 <br>드라이브가 RAID 어레이에서 구성된 경우 시스템을 다시 시작하고 호스트 어댑터 구성 유틸리티 프로그램을 입력합니다. |
|![온도 아이콘](media/fxt-monitor/fxt-temp-icon.jpg) | 온도 상태 | 열 오류 - 예를 들어 팬이 고장나거나 주변 온도가 범위를 벗어났습니다. | 다음 주소 지정 가능한 조건을 확인합니다. <br>냉각 팬이 없거나 실패했습니다. <br>시스템의 덮개, 공기 덮개, 메모리 모듈 블랭크 또는 백 필러 브래킷이 제거됩니다. <br>주변 온도가 너무 높습니다. <br>외부 공기 흐름이 막혀 |
|![전기 아이콘](media/fxt-monitor/fxt-electric-icon.jpg) | 전기 상태 | 전기 오류 - 예를 들어, 범위를 벗어난 전압, PSU 고장 또는 고장난 전압 레귤레이터 |  시스템 이벤트 로그 또는 시스템 메시지에 특정 문제가 있는지 확인합니다. PSU 문제가 있는 경우 PSU 상태 LED를 확인하고 필요한 경우 PSU를 다시 장착하십시오. | 
|![메모리 아이콘](media/fxt-monitor/fxt-memory-icon.jpg) | 메모리 상태 | 메모리 오류 | 시스템 이벤트 로그 또는 시스템 메시지에서 실패한 메모리의 위치를 확인합니다. 메모리 모듈을 다시 장착합니다. |
|![PCIe 아이콘](media/fxt-monitor/fxt-pcie-icon.jpg) | PCIe 상태 | PCIe 카드 오류 | 시스템을 다시 시작하십시오. 업데이트 PCIe 카드 드라이버; 카드를 다시 설치하십시오. |


### <a name="system-health-status-indicator"></a>시스템 상태 표시기

왼쪽 제어판 오른쪽에 있는 큰 조명 버튼은 전체 시스템 상태를 나타내며 시스템 ID 모드에서 장치 로케이터 라이트로도 사용됩니다.

시스템 상태 및 ID 버튼을 눌러 시스템 ID 모드와 시스템 상태 모드 간에 전환합니다.

|시스템 상태 상태 | 조건 |
|-------------------------------------------|-----------------------------------------------|
| 단색 파란색 | 정상 작동: 시스템이 켜져 있고 정상적으로 작동하며 시스템 ID 모드가 활성화되어 있지 않습니다. <br/>시스템 ID 모드로 전환하려면 시스템 상태 및 ID 버튼을 누릅니다. |
| 파란색 깜박임 | 시스템 ID 모드가 활성화되어 있습니다. 시스템 상태 모드로 전환하려면 시스템 상태 및 시스템 ID 버튼을 누릅니다. |
| 단색 황색 | 시스템이 페일 세이프 모드에 있습니다. 문제가 지속되면 [Microsoft 고객 서비스 및 지원 팀에 문의하십시오.](fxt-support-ticket.md) |
| 황색 깜박임 | 시스템 오류. 시스템 이벤트 로그에 특정 오류 메시지가 있는지 확인합니다. 시스템 구성 요소를 모니터링하는 시스템 펌웨어 및 에이전트에서 생성한 이벤트 및 오류 메시지에 대한 자세한 내용은 qrl.dell.com 오류 코드 조회 페이지를 참조하십시오. |


