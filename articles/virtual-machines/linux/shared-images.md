---
title: Azure CLI를 사용하여 공유 이미지 갤러리 만들기
description: 이 문서에서는 Azure CLI를 사용하여 Azure에서 VM의 공유 이미지를 만드는 방법을 알아봅니다.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: article
ms.workload: infrastructure
ms.date: 05/06/2019
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: c75d33f8310cfd143bf201cdac861954d07baf0b
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758411"
---
# <a name="create-a-shared-image-gallery-with-the-azure-cli"></a>Azure CLI를 사용하여 공유 이미지 갤러리 만들기

[공유 이미지 갤러리](shared-image-galleries.md)는 조직 내 사용자 지정 이미지 공유를 간소화합니다. 사용자 지정 이미지는 Marketplace 이미지와 같지만 직접 만듭니다. 애플리케이션 사전 로드, 애플리케이션 구성 및 기타 OS 구성과 같은 부트스트랩 구성에 사용자 지정 이미지를 사용할 수 있습니다. 

공유 이미지 갤러리를 사용하면 조직, 지역, AAD 테넌트의 다른 사용자와 사용자 지정 VM 이미지를 공유할 수 있습니다. 공유할 이미지, 이미지를 제공할 지역, 이미지를 공유할 사람을 선택하세요. 여러 갤러리를 만들어서 공유 이미지 논리적으로 그룹화할 수 있습니다. 

갤러리는 완전한 RBAC(역할 기반 액세스 제어)를 제공하는 최상위 리소스입니다. 이미지 버전을 관리할 수 있으며, 각 이미지 버전을 여러 Azure 지역에 복제하도록 선택할 수 있습니다. 갤러리는 관리되는 이미지에서만 작동합니다.

공유 이미지 갤러리 기능에는 여러 가지 리소스가 있습니다. 이 문서에서는 이러한 리소스를 사용하거나 작성하게 됩니다.

| 리소스 | Description|
|----------|------------|
| **관리되는 이미지** | 이는 단독으로 사용하거나 이미지 갤러리에 **이미지 버전**을 만드는 데 사용할 수 있는 기본 이미지입니다. 관리되는 이미지는 일반화된 VM에서 생성됩니다. 관리되는 이미지는 여러 VM을 만드는 데 사용할 수 있는 특수한 유형의 VHD로, 이제 공유 이미지 버전을 만드는 데 사용할 수 있습니다. |
| **이미지 갤러리** | Azure Marketplace와 마찬가지로 **이미지 갤러리**는 이미지를 관리하고 공유하는 데 사용되는 리포지토리이지만 액세스할 수 있는 사람을 제어할 수 있습니다. |
| **이미지 정의** | 이미지는 갤러리 내에 정의되고, 내부적으로 사용하기 위해 충족해야 할 요구 사항과 이미지에 대한 정보를 전달합니다. 여기에는 이미지가 Windows인지, Linux인지 여부, 릴리스 정보, 최소 및 최대 메모리 요구 사항이 포함됩니다. 이미지의 형식 정의입니다. |
| **이미지 버전** | **이미지 버전**은 갤러리를 사용하는 경우 VM을 만들 때 사용합니다. 사용 환경에 필요한 만큼 여러 버전의 이미지를 가질 수 있습니다. 관리되는 이미지와 마찬가지로 **이미지 버전**을 사용하여 VM을 만들 때는 이미지 버전을 사용하여 VM의 새 디스크를 만듭니다. 이미지 버전은 여러 번 사용할 수 있습니다. |



[!INCLUDE [virtual-machines-common-shared-images-cli](../../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-vm"></a>VM 만들기

az vm create를 사용하여 최신 이미지 버전에서 [VM을 만듭니다.](/cli/azure/vm#az-vm-create)

```azurecli-interactive 
az vm create\
   --resource-group myGalleryRG \
   --name myVM \
   --image "/subscriptions/subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition" \
   --generate-ssh-keys
```

`--image` 매개 변수에 대한 이미지 버전 ID를 사용하여 특정 버전을 사용할 수도 있습니다. 예를 들어 이미지 버전 *1.0.0* `--image "/subscriptions/<subscription ID where the gallery is located>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0"`을 사용하려면 다음을 입력합니다.

[!INCLUDE [virtual-machines-common-gallery-list-cli](../../../includes/virtual-machines-common-gallery-list-cli.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-cli](../../../includes/virtual-machines-common-shared-images-update-delete-cli.md)]

## <a name="next-steps"></a>다음 단계
[Azure 이미지 빌더(미리 보기)는](image-builder-overview.md) 이미지 버전 생성을 자동화하는 데 도움이 될 수 있으며, [기존 이미지 버전에서 새 이미지 버전을](image-builder-gallery-update-image-version.md)업데이트하고 만드는 데 사용할 수도 있습니다. 

템플릿을 사용하여 공유 이미지 갤러리 리소스를 만들 수도 있습니다. 다음의 몇 가지 Azure 빠른 시작 템플릿을 사용할 수 있습니다. 

- [Shared Image Gallery 만들기](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [공유 이미지 갤러리에서 이미지 정의 만들기](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [공유 이미지 갤러리에서 이미지 버전 만들기](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [이미지 버전에서 VM 만들기](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

공유 이미지 갤러리에 대한 자세한 내용은 [개요](shared-image-galleries.md)를 참조하세요. 문제 발생 시에는 [공유 이미지 갤러리 문제 해결](troubleshooting-shared-images.md)을 참조하세요.
