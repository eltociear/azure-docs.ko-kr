---
title: Azure Marketplace에 대 한 사용자 VM 이미지 만들기
description: 사용자 VM 이미지를 만드는 데 필요한 단계와 참조를 나열합니다.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: dsindona
ms.openlocfilehash: 9d82d50769925480d461c122096c3919d7e8940d
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82146577"
---
# <a name="create-a-user-vm-image"></a>사용자 VM 이미지 만들기

> [!IMPORTANT]
> 2020 년 4 월 13 일부 터 파트너 센터에 대 한 Azure Virtual Machine 제품의 이동 관리를 시작 합니다. 마이그레이션 후 파트너 센터에서 제품을 만들고 관리 합니다. [Azure Virtual Machine 기술 자산 만들기](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-vm-create-offer) 의 지침에 따라 마이그레이션된 제안을 관리 합니다.

이 문서에서는 일반화된 VHD에서 관리되지 않는 이미지를 만드는 데 필요한 두 가지 일반 단계를 설명합니다.  이미지 캡처 및 이미지 일반화의 각 단계를 안내하는 참조가 제공됩니다.


## <a name="capture-the-vm-image"></a>VM 이미지 캡처

액세스 방법에 해당하는 VM 캡처에 대한 다음 문서의 지침을 사용합니다.

-  PowerShell: [Azure VM에서 관리되지 않는 VM 이미지를 만드는 방법](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [가상 머신 또는 VHD의 이미지를 만드는 방법](../../../virtual-machines/linux/capture-image.md)
-  API: [Virtual Machines - 캡처](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)


## <a name="generalize-the-vm-image"></a>VM 이미지 일반화

이전에 일반화된 VHD에서 사용자 이미지를 생성했으므로 이를 일반화해야 합니다.  다시 한 번 액세스 메커니즘에 해당하는 다음 문서를 선택합니다.  (디스크를 캡처할 때 이미 디스크를 일반화했을 수 있습니다.)

-  PowerShell: [VM 일반화](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [2단계: VM 이미지 만들기](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [Virtual Machines - 일반화](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="next-steps"></a>다음 단계

다음으로, [인증서를 만들고](cpp-create-key-vault-cert.md) 새 Azure Key Vault에 저장합니다.  이 인증서는 VM에 대한 보안 WinRM 연결을 설정하는 데 필요합니다.
