---
title: PowerShell-Azure DevTest Labs에서 랩에 외부 사용자 추가
description: 이 문서에서는 Azure DevTest Labs에서 랩에 외부 사용자를 추가 하는 Azure PowerShell 스크립트를 제공 합니다.
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/22/2020
ms.author: spelluru
ms.openlocfilehash: 40103294e2f610e3ff5879c650f835d4c3e4c207
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82101725"
---
# <a name="use-powershell-to-add-an-external-user-to-a-lab-in-azure-devtest-labs"></a>PowerShell을 사용하여 Azure DevTest Labs의 랩에 외부 사용자 추가

이 샘플 PowerShell 스크립트는 Azure DevTest Labs의 랩에 외부 사용자를 추가합니다. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="prerequisites"></a>사전 요구 사항
* **랩**. 스크립트를 사용하려면 기존 랩이 있어야 합니다. 

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell[main](../../../powershell_scripts/devtest-lab/add-external-user-to-lab/add-external-user-to-custom-lab.ps1 "Add external user to a lab")]

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용합니다. 

| 명령 | 메모 |
|---|---|
| [AzADUser](/powershell/module/az.resources/get-azaduser) | Azure Active Directory에서 사용자 개체를 다시 시도합니다. |
| [New-AzRoleAssignment](/powershell/module/az.resources/new-azroleassignment) | 지정된 범위에서 지정된 보안 주체에 지정된 역할을 할당합니다. |

## <a name="next-steps"></a>다음 단계

Azure PowerShell에 대한 자세한 내용은 [Azure PowerShell 설명서](https://docs.microsoft.com/powershell/)를 참조하세요.

추가 Azure Lab Services PowerShell 스크립트 샘플은 [Azure Lab Services PowerShell 샘플](../samples-powershell.md)에서 확인할 수 있습니다.
