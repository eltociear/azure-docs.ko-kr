---
title: Azure Automation에서 업데이트 관리, 변경 내용 추적 및 인벤토리 솔루션을 등록하는 방법
description: Azure Automation에 포함된 업데이트 관리, 변경 내용 추적 및 인벤토리 솔루션을 사용하여 Azure Virtual Machine을 등록하는 방법을 알아봅니다.
services: automation
ms.date: 4/11/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 57378005bd668fa9c0f2aea70c411bbf911130db
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457657"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>업데이트 관리, 변경 내용 추적 및 인벤토리 솔루션 등록

Azure Automation은 운영 체제 보안 업데이트를 관리하고, 변경 내용을 추적하며, 컴퓨터에 설치된 항목을 재고 자산으로 처리(인벤토리)하기 위한 솔루션을 제공합니다. 머신을 등록하는 방법에는 여러 가지가 있으며, [가상 머신](automation-onboard-solutions-from-vm.md), [여러 머신 검색](automation-onboard-solutions-from-browse.md), Automation 계정 또는 [Runbook](automation-onboard-solutions.md)에서 솔루션을 등록할 수 있습니다. 이 문서에서는 Automation 계정에서 이러한 솔루션을 등록하는 방법에 대해 설명합니다.

## <a name="sign-in-to-azure"></a>Azure에 로그인

에서 https://portal.azure.comAzure에 로그인합니다.

## <a name="enable-solutions"></a>솔루션 사용

자동화 계정으로 이동하여 **구성 관리**에서 **인벤토리** 또는 **변경 사항을 선택합니다.**

로그 분석 작업 영역 및 자동화 계정을 선택하고 **사용 설정을** 클릭하여 솔루션을 활성화합니다. 솔루션을 사용하도록 설정하는 데 최대 15분이 걸립니다.

![인벤토리 솔루션 등록](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

> [!NOTE]
> 솔루션을 사용하도록 설정할 때 특정 Azure 지역에서만 Log Analytics 작업 영역 및 Automation 계정을 연결할 수 있습니다.
>
> 지원되는 매핑 쌍 목록은 자동화 [계정 및 로그 분석 작업 영역에 대한 지역 매핑을](how-to/region-mappings.md)참조하십시오.

변경 내용 추적 및 인벤토리 솔루션은 가상 머신에서 [변경 내용 추적](automation-vm-change-tracking.md) 및 [인벤토리](automation-vm-inventory.md)의 기능을 제공합니다. 이 단계에서는 가상 머신에 솔루션을 사용할 수 있습니다.

변경 정보 추적 및 인벤토리 솔루션 온보딩 알림이 완료되면 **업데이트 관리에서** **업데이트 관리를** 선택합니다.

업데이트 관리 솔루션을 사용하면 Azure 및 하이브리드 VM에 대한 업데이트 및 패치를 관리할 수 있습니다. 사용 가능한 업데이트의 상태를 평가하고, 필요한 업데이트의 설치를 예약하고, 배포 결과를 검토하여 업데이트가 성공적으로 적용되었는지 확인할 수 있습니다.

사용 솔루션 페이지에서 선택한 Log Analytics 작업 영역은 이전 단계에서 사용된 작업 영역과 동일합니다. 업데이트 관리 솔루션을 온보로 **설정하려면 활성화를** 클릭합니다. 솔루션을 사용하도록 설정하는 데 최대 15분이 걸립니다.

![업데이트 솔루션 등록](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>범위 구성

각 솔루션은 작업 영역 내의 범위 구성을 사용하여 솔루션을 가져오는 컴퓨터를 대상으로 합니다. 범위 구성은 솔루션 범위를 특정 컴퓨터로 제한하는 데 사용되는 하나 이상의 저장된 검색 그룹입니다. 범위 구성에 액세스하려면 **관련 리소스**아래의 자동화 계정에서 **작업 영역을**선택합니다. 그런 다음 작업 **영역 데이터 원본**아래의 작업 영역에서 **범위 구성을 선택합니다.**

선택한 작업 영역에 업데이트 관리 또는 변경 내용 추적 솔루션이 아직 없는 경우 다음과 같은 범위 구성이 생성됩니다.

* **MicrosoftDefaultScopeConfig-ChangeTracking**

* **MicrosoftDefaultScopeConfig-Updates**

선택한 작업 영역에 솔루션이 이미 있는 경우에는 솔루션이 다시 배포되지 않고 범위 구성이 추가되지 않습니다.

## <a name="saved-searches"></a>저장된 검색

컴퓨터가 업데이트 관리 또는 변경 내용 추적 및 인벤토리 솔루션에 추가되면 작업 영역에서 두 가지 저장된 검색 중 하나에 추가됩니다. 이러한 저장된 검색은 이러한 솔루션을 대상으로 하는 컴퓨터가 포함된 쿼리입니다.

로그 분석 작업 영역으로 이동하여 **일반**에서 **저장된 검색을** 선택합니다. 다음 표에는 이러한 솔루션에서 사용하는 두 가지 저장된 검색이 나와 있습니다.

|속성     |범주  |Alias  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | 업데이트        | Updates__MicrosoftDefaultComputerGroup         |

저장된 검색 중 하나를 선택하여 그룹을 채우는 데 사용된 쿼리를 살펴봅니다. 다음 이미지에서는 쿼리 및 해당 결과를 보여 줍니다.

![저장된 검색](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Azure VM 등록

자동화 계정에서 **구성 관리에서** **인벤토리** 또는 **변경 추적을** 선택하거나 **업데이트 관리에서** **업데이트 관리를** 선택합니다.

**+ Azure VM 추가**를 클릭하고 목록에서 하나 이상의 VM을 선택합니다. 사용할 수 없는 가상 머신은 회색으로 표시되어 있으며 선택할 수 없습니다. Azure VM은 자동화 계정의 위치에 관계없이 모든 지역에 존재할 수 있습니다. **업데이트 관리 사용** 페이지에서 **사용**을 클릭합니다. 이 작업은 컴퓨터 그룹의 저장된 솔루션 검색에 선택한 VM을 추가합니다.

![Azure VM을 사용하도록 설정](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>비Azure 컴퓨터 등록

Azure에 없는 컴퓨터는 수동으로 추가해야 합니다. 자동화 계정에서 **구성 관리에서** **인벤토리** 또는 **변경 추적을** 선택하거나 **업데이트 관리에서** **업데이트 관리를** 선택합니다.

**비Azure 컴퓨터 추가**를 클릭합니다. 이 작업은 컴퓨터가 솔루션에 대한 보고를 시작할 수 있도록 [Windows용 Log Analytics 에이전트를 설치하고 구성하는 지침이](../azure-monitor/platform/log-analytics-agent.md) 포함된 새 브라우저 창을 엽니다. 시스템 센터 운영 관리자에서 현재 관리 중인 컴퓨터를 온보딩하는 경우 새 에이전트가 필요하지 않으며 작업 영역 정보가 기존 에이전트에 입력됩니다.

## <a name="onboard-machines-in-the-workspace"></a>작업 영역에서 컴퓨터 등록

이미 사용자의 작업 영역에 보고하는, 수동으로 설치된 머신을 Azure Automation에 추가해야 솔루션을 사용할 수 있습니다. 자동화 계정에서 **구성 관리에서** **인벤토리** 또는 **변경 추적을** 선택하거나 **업데이트 관리에서** **업데이트 관리를** 선택합니다.

**컴퓨터 관리**를 선택합니다. 이 작업은 **컴퓨터 관리** 페이지를 엽니다. 이 페이지에서는 선택한 컴퓨터 집합 또는 사용 가능한 모든 컴퓨터에서 솔루션을 사용하도록 설정하거나, 현재의 모든 컴퓨터 및 이후의 모든 컴퓨터에서 솔루션을 사용하도록 설정할 수 있습니다. 이전에 **사용 가능한 모든 향후 머신에서 사용** 옵션을 선택한 경우 **컴퓨터 관리** 단추가 회색으로 표시될 수 있습니다.

![저장된 검색](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>사용 가능한 모든 컴퓨터

사용 가능한 모든 컴퓨터에서 솔루션을 사용하도록 설정하려면, **사용 가능한 모든 컴퓨터에서 사용**을 선택합니다. 이 작업은 머신을 개별적으로 추가할 수 있는 컨트롤을 비활성화합니다. 이 작업은 작업 영역에 보고하는 컴퓨터의 모든 이름을 컴퓨터 그룹 저장된 검색 쿼리에 추가합니다. 이 작업을 선택하면 **컴퓨터 관리** 단추를 사용할 수 없게 됩니다.

### <a name="all-available-and-future-machines"></a>사용 가능한 모든 향후 컴퓨터

사용 가능한 모든 머신 및 향후 머신에서 솔루션을 사용하려면, **사용 가능한 모든 향후 머신에서 사용**을 선택합니다. 이 옵션은 작업 영역에서 저장된 검색 및 범위 구성을 삭제합니다. 이 작업은 작업 영역에 보고하는 모든 Azure 및 비Azure 머신에 대해 솔루션을 엽니다. 이 작업을 선택하면 남아 있는 범위 구성이 없으므로 **컴퓨터 관리** 단추가 영구적으로 비활성화됩니다.

초기 저장된 검색을 다시 추가하여 범위 구성을 다시 추가할 수 있습니다. 자세한 내용은 [저장된 검색을](#saved-searches)참조하십시오.

### <a name="selected-machines"></a>선택한 컴퓨터

하나 이상의 컴퓨터에 대한 솔루션을 사용하려면 **선택한 컴퓨터에서 활성화를** 선택하고 솔루션에 추가할 각 컴퓨터 옆에 **추가를** 클릭합니다. 이 작업은 선택한 컴퓨터의 이름을 솔루션에 대한 컴퓨터 그룹 저장된 검색 쿼리에 추가합니다.

## <a name="unlink-workspace"></a>작업 영역 연결 해제

다음 솔루션은 Log Analytics 작업 영역에 따라 다릅니다.

* [업데이트 관리](automation-update-management.md)
* [변경 내용 추적](automation-change-tracking.md)
* [근무 외 시간 동안 VM 시작/중지](automation-solution-vm-management.md)

자동화 계정을 Log Analytics 작업 영역과 더 이상 통합하지 않으려면 Azure 포털에서 직접 계정을 연결 해제할 수 있습니다.  계속하기 전에 앞에서 언급한 솔루션을 제거해야 합니다. 제거하지 않으면 이 프로세스를 계속 진행할 수 없습니다. 가져온 특정 솔루션에 대한 문서를 검토하여 제거하는 데 필요한 단계를 파악합니다.

이러한 솔루션을 제거하고 나면, 다음 단계에 따라 Automation 계정 연결을 해제할 수 있습니다.

> [!NOTE]
> 이전 버전의 Azure SQL 모니터링 솔루션을 포함한 일부 솔루션에서 자동화 자산을 만들었을 수 있으며, 작업 영역을 연결 해제하기 전에 제거해야 할 수도 있습니다.

1. Azure Portal에서 Automation 계정을 열고 Automation 계정 페이지의 왼쪽에서 **관련 리소스** 레이블이 지정된 섹션 아래의 **연결된 작업 영역**을 선택합니다.

2. 작업 영역 연결 해제 페이지에서 **작업 영역 연결 해제**를 클릭합니다.

   ![작업 영역 연결 해제 페이지](media/automation-onboard-solutions-from-automation-account/automation-unlink-workspace-blade.png).

   계속 진행할지 확인하는 메시지가 나타납니다.

3. Azure Automation이 Log Analytics 작업 영역에서 계정 연결을 해제하는 동안 메뉴의 **알림**에서 진행 상황을 추적할 수 있습니다.

업데이트 관리 솔루션을 사용한 경우 솔루션을 제거한 후 더 이상 필요하지 않은 다음 항목을 제거할 수도 있습니다.

* 업데이트 일정 - 각 업데이트 배포와 일치하는 이름이 있습니다.

* 솔루션을 위해 만든 하이브리드 작업자 그룹 - 각각 machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8)와 유사하게 이름이 지정됩니다.

작업 시간 외 VM 시작/중지 솔루션을 사용한 경우 솔루션을 제거한 후 더 이상 필요 없는 다음 항목을 선택적으로 제거할 수도 있습니다.

* VM runbook 시작 및 중지 일정
* VM runbook 시작 및 중지
* variables

또는 Log Analytics 작업 영역에서 자동화 계정에서 작업 영역의 연결을 해제할 수도 있습니다. 작업 영역에서 **관련 리소스**에서 **자동화 계정을** 선택합니다. 자동화 계정 페이지에서 **계정 연결 해제를 선택합니다.**

## <a name="clean-up-resources"></a>리소스 정리

업데이트 관리에서 VM을 제거하려면:

* Log Analytics 작업 영역에서, 범위 구성 `MicrosoftDefaultScopeConfig-Updates`에 대한 저장된 검색에서 VM을 제거합니다. 저장된 검색은 작업 영역의 **일반**에서 찾을 수 있습니다.
* [Windows용 로그 분석 에이전트](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) 또는 [Linux용 로그 분석 에이전트를](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)제거합니다.

## <a name="next-steps"></a>다음 단계

솔루션을 사용하는 방법을 알아보려면 다음 자습서로 계속 진행하세요.

* [자습서 - VM 업데이트 관리](automation-tutorial-update-management.md)

* [자습서 - VM에 설치된 소프트웨어 식별](automation-tutorial-installed-software.md)

* [자습서 - VM에 대한 변경 내용 문제 해결](automation-tutorial-troubleshoot-changes.md)
