---
title: 에서 관리 서비스 제공 Azure Marketplace
description: 관리 서비스를 통해 서비스 공급자는 Azure Marketplace의 고객에 게 리소스 관리 제안을 판매할 수 있습니다.
ms.date: 03/30/2020
ms.topic: conceptual
ms.openlocfilehash: 13e1825ae6eb50b1b376e3bd3de908a545fbe023
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82144907"
---
# <a name="managed-service-offers-in-azure-marketplace"></a>에서 관리 서비스 제공 Azure Marketplace

이 문서에서는 [Azure Marketplace](https://azuremarketplace.microsoft.com)의 **관리 서비스** 제안 유형을 설명 합니다. 관리 서비스 제공을 사용 하면 [Azure 위임 된 리소스 관리](azure-delegated-resource-management.md)를 사용 하 여 고객에 게 리소스 관리 서비스를 제공할 수 있습니다. 이러한 제품은 모든 잠재 고객 또는 하나 이상의 특정 고객에 게 제공 될 수 있습니다. 이러한 관리형 서비스와 관련된 비용은 고객에게 직접 청구하므로 Microsoft에서 부과하는 요금은 없습니다.

## <a name="understand-managed-service-offers"></a>관리 서비스 제안 이해

관리 서비스는 Azure 위임 된 리소스 관리를 위해 고객을 온 보 딩 하는 프로세스를 간소화 합니다. 고객이 Azure Marketplace에서 제품을 구매하는 경우 등록해야 하는 구독 및/또는 리소스 그룹을 지정할 수 있습니다.

그런 다음 조직의 사용자는 제품을 만들 때 정의한 액세스에 따라 조직의 테 넌 트 내에서 해당 리소스에 대해 작업을 수행할 수 있습니다. 이것은 Azure 위임 리소스 관리를 사용하여 고객 리소스에 액세스할 수 있는 Azure AD 사용자, 그룹 및 서비스 원칙을 지정하는 매니페스트와 액세스 수준을 정의하는 역할을 통해 수행됩니다. 일련의 개별 사용자 또는 애플리케이션 계정이 아닌 Azure AD 그룹에 권한을 할당하므로 액세스 요구 사항이 변경될 때 개별 사용자를 추가하거나 제거할 수 있습니다.

## <a name="public-and-private-offers"></a>퍼블릭 및 프라이빗 제품

각 관리형 서비스 제품에는 하나 이상의 플랜이 포함됩니다. 요금제는 개인 또는 공용 일 수 있습니다.

특정 고객으로 제품을 제한하려는 경우 프라이빗 플랜을 게시할 수 있습니다. 이렇게 하면 사용자가 제공하는 특정 구독 ID에 대해서만 플랜을 구매할 수 있습니다. 자세한 내용은 [프라이빗 제품](../../marketplace/private-offers.md)을 참조하세요.

퍼블릭 플랜을 사용하면 서비스를 새 고객에게 프로모션할 수 있습니다. 이러한 정보는 일반적으로 고객 테넌트에 대한 액세스를 제한해야 할 때 좀 더 적합합니다. 고객과의 관계를 설정하고 난 후, 조직에 추가 액세스 권한을 부여하려는 경우 해당 고객을 위해 새 프라이빗 플랜을 게시하거나 [Azure Resource Manager 템플릿을 사용하여 추가 액세스를 위해 온보딩](../how-to/onboard-customer.md)할 수 있습니다.

적절한 경우 퍼블릭 및 프라이빗 플랜을 모두 동일한 제품에 포함할 수 있습니다.

> [!IMPORTANT]
> 계획이 공개로 게시 되 면 비공개로 변경할 수 없습니다. 제품을 수락 하 고 리소스를 위임할 수 있는 고객을 제어 하려면 비공개 요금제를 사용 합니다. 공개 계획을 사용 하면 특정 고객 또는 특정 수의 고객에 대 한 가용성을 제한할 수 없습니다 (이렇게 선택 하는 경우 계획을 완전히 판매 하지 않을 수 있음). 제품을 게시할 때 **역할 정의가** [관리 되는 서비스 등록 할당 삭제 역할](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) 로 설정 된 **권한 부여** 를 포함 한 경우에만 고객이 제안을 수락한 후 [에 위임에 대 한 액세스 권한을 제거할](../how-to/remove-delegation.md) 수 있습니다. 또한 고객에 게 연락 하 여 [액세스 권한을 제거](../how-to/view-manage-service-providers.md#add-or-remove-service-provider-offers)하도록 요청할 수 있습니다.

## <a name="publish-managed-service-offers"></a>관리형 서비스 제품 게시

관리형 서비스 제품을 게시하는 방법을 알아보려면 [Azure Marketplace에 관리형 서비스 솔루션 게시](../how-to/publish-managed-services-offers.md)를 참조하세요.

## <a name="next-steps"></a>다음 단계

- [Azure 위임 리소스 관리](azure-delegated-resource-management.md) 및 [테넌트 간 관리 환경](cross-tenant-management-experience.md)에 대해 자세히 알아봅니다.
- Azure Marketplace에 [관리형 서비스 솔루션을 게시합니다](../how-to/publish-managed-services-offers.md).
