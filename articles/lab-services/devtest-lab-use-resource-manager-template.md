---
title: 가상 컴퓨터의 Azure 리소스 관리자 템플릿 보기 및 사용
description: 가상 머신에서 Azure Resource Manager 템플릿을 사용하여 다른 VM을 만드는 방법을 알아봅니다.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: 7064fdeec04f4dc5ae2c73c1a3896cf2d10dd01d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169110"
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Azure Resource Manager 템플릿을 사용하여 가상 머신 만들기 

[Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)을 통해 DevTest Labs에서 VM(가상 머신)을 만드는 경우 VM을 저장하기 전에 Azure Resource Manager 템플릿을 볼 수 있습니다. 그런 다음 템플릿을 기초로 사용해서 동일한 설정으로 더 많은 랩 VM을 만들 수 있습니다.

이 문서는 여러 VM과 단일 VM Resource Manager 템플릿을 설명하고 VM을 만들 때 템플릿을 보고 저장하는 방법을 보여 줍니다.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>다중 VM 및 단일 VM Resource Manager 템플릿
리소스 관리자 템플릿을 사용하여 DevTest Labs에서 VM을 만드는 방법에는 Microsoft.DevTestLab/labs/가상 머신 리소스를 프로비전하거나 Microsoft.Compute/가상 시스템 리소스를 프로비전하는 두 가지 방법이 있습니다. 각 방법은 서로 다른 시나리오에서 사용되며, 필요한 사용 권한도 다릅니다.

- Microsoft.DevTestLab/labs/virtualmachines 리소스 종류(템플릿의 “resource” 속성에 선언됨)를 사용하는 Resource Manager 템플릿은 개별 랩 VM을 프로비전할 수 있습니다. 그러면 각 VM이 DevTest Labs 가상 머신 목록에서 단일 항목으로 표시됩니다.

   ![DevTest Labs 가상 머신 목록에서 단일 항목으로 표시되는 VM 목록](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   이러한 유형의 리소스 관리자 템플릿은 Azure PowerShell 명령 **New-AzResourceGroupDeploy를** 통해 또는 Azure CLI 명령 **az 그룹 배포 create를**통해 프로비전할 수 있습니다. 관리자 권한이 필요하므로 DevTest Labs 사용자 역할이 할당된 사용자는 배포를 수행할 수 없습니다. 

- Microsoft.Compute/virtualmachines 리소스 종류를 사용하는 Resource Manager 템플릿은 여러 VM을 DevTest Labs 가상 머신 목록의 단일 환경으로 프로비전할 수 있습니다.

   ![DevTest Labs 가상 머신 목록에서 단일 항목으로 표시되는 VM 목록](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   동일한 환경의 VM은 함께 관리할 수 있으며, 동일한 수명 주기를 공유합니다. DevTest Labs 사용자 역할이 할당된 사용자는 이러한 템플릿을 사용하여 환경을 만들 수 있지만 관리자가 해당 방식으로 랩을 구성한 경우에만 가능합니다.

이 문서의 나머지 부분에서는 Microsoft.DevTestLab/labs/virtualmachines를 사용하는 Resource Manager 템플릿에 대해 설명합니다. 이러한 템플릿은 랩 관리자가 랩 VM 생성(예: 클레임 가능한 VM) 또는 골든 이미지 생성(예: 이미지 팩터리)을 자동화하는 데 사용됩니다.

[Azure Resource Manager 템플릿 작성에 대한 모범 사례](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices)에 안정적이고 사용하기 쉬운 Azure Resource Manager 템플릿을 만드는 데 도움이 되는 다양한 지침과 제안이 나와 있습니다.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>가상 머신의 Resource Manager 템플릿 보기 및 저장
1. [랩에서 첫 번째 VM 만들기](tutorial-create-custom-lab.md#add-a-vm-to-the-lab)의 단계에 따라 가상 머신 생성을 시작합니다.
1. 가상 머신에 대한 필요한 정보를 입력하고 이 VM에 사용하려는 아티팩트를 모두 추가합니다.
1. **고급 설정** 탭으로 Swtich. 
1. 설정 구성 창의 맨 아래에서 **ARM 템플릿 보기**를 선택합니다.
1. 나중에 다른 가상 머신을 만드는 데 사용할 Resource Manager 템플릿을 복사하고 저장합니다.

   ![나중에 사용하기 위해 저장할 Resource Manager 템플릿](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Resource Manager 템플릿을 저장한 후 먼저 템플릿의 매개 변수 섹션을 업데이트해야 템플릿을 사용할 수 있습니다. 실제 Resource Manager 템플릿 외부에서 매개 변수만 사용자 지정하는 parameter.json을 만들 수 있습니다. 

![JSON 파일을 사용하여 매개 변수 사용자 지정](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

이제 [VM을 만드는](devtest-lab-create-environment-from-arm.md) 데 Resource Manager 템플릿을 사용할 준비가 되었습니다.

## <a name="set-expiration-date"></a>만료 날짜 설정
교육, 데모 및 평가판과 같은 시나리오에서는 불필요한 비용이 발생하지 않도록 고정된 기간 이후에 가상 컴퓨터를 만들고 자동으로 삭제할 수 있습니다. VM에 대한 **만료Date** 속성을 지정하여 만료 날짜가 있는 랩 VM을 만들 수 있습니다. [GitHub 리포지토리에서](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-customimage-with-expiration)동일한 리소스 관리자 템플릿을 확인하십시오.



### <a name="next-steps"></a>다음 단계
* [Resource Manager 템플릿으로 다중 VM 환경을 만드는](devtest-lab-create-environment-from-arm.md) 방법을 알아봅니다.
* [Resource Manager 템플릿을 배포하여 VM 만들기](devtest-lab-create-environment-from-arm.md#automate-deployment-of-environments)
* [공개 DevTest Labs GitHub 리포지토리에서](https://github.com/Azure/azure-quickstart-templates)DevTest Labs 자동화에 대한 더 빠른 시작 리소스 관리자 템플릿을 살펴보십시오.
