---
title: Azure Lighthouse 시나리오의 테넌트, 역할 및 사용자
description: Azure Active Directory 테넌트, 사용자 및 역할의 개념과 Azure Lighthouse 시나리오에서 이러한 항목을 사용하는 방법을 알아봅니다.
ms.date: 04/03/2020
ms.topic: conceptual
ms.openlocfilehash: 7ed5af18efbb0f5b97dcab20093cc45e8bed1d03
ms.sourcegitcommit: f7fb9e7867798f46c80fe052b5ee73b9151b0e0b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82144929"
---
# <a name="tenants-roles-and-users-in-azure-lighthouse-scenarios"></a>Azure Lighthouse 시나리오의 테넌트, 역할 및 사용자

[Azure 위임 리소스관리](azure-delegated-resource-management.md)를 위해 고객을 온보딩하기 전에 Azure AD(Azure Active Directory) 테넌트, 사용자 및 역할이 작동하는 방식과 Azure Lighthouse 시나리오에서 이러한 항목을 사용할 수 있는 방법을 이해하는 것이 중요합니다.

*테넌트*는 Azure AD의 신뢰할 수 있는 전용 인스턴스입니다. 일반적으로 하나의 Azure 테넌트는 단일 조직을 나타냅니다. Azure 위임 리소스 관리를 통해 한 테넌트에서 다른 테넌트로 리소스를 논리적으로 프로젝션할 수 있습니다. 이렇게 하면 관리 테넌트의 사용자(예: 서비스 공급자에 속하는 사용자)는 고객 테넌트의 위임된 리소스에 액세스할 수 있고, [여러 테넌트가 있는 엔터프라이즈에서 관리 작업을 중앙에서 수행](enterprise.md)할 수 있습니다.

이러한 논리적 프로젝션을 수행하려면 고객 테넌트의 구독(또는 구독 내의 하나 이상의 리소스 그룹)을 Azure 위임 리소스관리를 위해 *온보딩*해야 합니다. 이 온보딩 프로세스는 [Azure Resource Manager 템플릿을 통해](../how-to/onboard-customer.md) 수행하거나 [Azure Marketplace에 퍼블릭 또는 프라이빗 제품을 게시](../how-to/publish-managed-services-offers.md)하여 수행할 수 있습니다.

선택하는 온보딩 방법에 관계없이 *권한 부여*를 정의해야 합니다. 각 권한 부여는 위임된 리소스에 대해 액세스 권한이 있는 관리 테넌트의 사용자 계정을 지정하고 이러한 각 사용자가 이러한 리소스에 대해 보유하게 되는 사용 권한을 설정하는 기본 제공 역할을 지정합니다.

## <a name="role-support-for-azure-delegated-resource-management"></a>Azure 위임 리소스 관리에 대한 역할 지원

권한 부여를 정의할 때 각 사용자 계정에는 [RBAC(역할 기반 액세스 제어) 기본 제공 역할](../../role-based-access-control/built-in-roles.md) 중 하나를 할당해야 합니다. 사용자 지정 역할 및 [클래식 구독 관리자 역할](../../role-based-access-control/classic-administrators.md)은 지원되지 않습니다.

현재 모든 [기본 제공 역할](../../role-based-access-control/built-in-roles.md)은 Azure 위임 리소스 관리에서 지원됩니다. 단, 다음과 같은 예외가 적용됩니다.

- [소유자](../../role-based-access-control/built-in-roles.md#owner) 역할은 지원되지 않습니다.
- [DataActions](../../role-based-access-control/role-definitions.md#dataactions) 권한이 있는 기본 제공 역할은 지원되지 않습니다.
- [사용자 액세스 관리자](../../role-based-access-control/built-in-roles.md#user-access-administrator) 기본 제공 역할은 지원되지만, [고객 테넌트에서 관리 ID에 역할을 할당](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant)하는 제한된 목적으로만 사용할 수 있습니다. 이 역할에서 일반적으로 부여하는 다른 사용 권한은 적용되지 않습니다. 이 역할이 있는 사용자를 정의하는 경우 이 사용자가 관리 ID에 할당할 수 있는 기본 제공 역할도 지정해야 합니다.

> [!NOTE]
> 적용 가능한 새 기본 제공 역할이 Azure에 추가 되 면 [Azure Resource Manager 템플릿을 사용 하 여 고객을 온 보 딩](../how-to/onboard-customer.md)할 때 할당 될 수 있습니다. [관리 서비스 제공을 게시할](../how-to/publish-managed-services-offers.md)때 Cloud 파트너 포털에서 새로 추가 된 역할을 사용할 수 있게 될 때까지 지연이 있을 수 있습니다.

## <a name="best-practices-for-defining-users-and-roles"></a>사용자 및 역할을 정의하기 위한 모범 사례

권한 부여를 만들 때 다음 모범 사례를 따르는 것이 좋습니다.

- 대부분의 경우 일련의 개별 사용자 계정이 아닌 Azure AD 사용자 그룹 또는 서비스 주체에 권한을 할당하는 것이 좋습니다. 이렇게 하면 액세스 요구 사항이 변경될 때 플랜을 업데이트한 후 다시 게시하지 않고도 개별 사용자에 대한 액세스 권한을 추가하거나 제거할 수 있습니다.
- 사용자가 작업을 완료하는 데 필요한 권한만 갖도록 하여 실수로 인한 오류 발생 가능성을 줄일 수 있게 최소 권한 원칙을 따라야 합니다. 자세한 내용은 [권장 보안 방법](../concepts/recommended-security-practices.md)을 참조하세요.
- 필요한 경우 나중에 [위임에 대한 액세스 권한을 제거](../how-to/remove-delegation.md)할 수 있도록 [관리 서비스 등록 할당 삭제 역할](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role)에 사용자를 포함합니다. 이 역할을 할당하지 않으면 고객 테넌트의 사용자만 위임된 리소스를 제거할 수 있습니다.
- [Azure Portal에서 내 고객 페이지를 볼 수 있어야](../how-to/view-manage-customers.md) 하는 모든 사용자에게 [읽기 권한자](../../role-based-access-control/built-in-roles.md#reader) 역할(또는 읽기 권한자 액세스 권한을 포함하는 다른 기본 제공 역할)이 있어야 합니다.

> [!IMPORTANT]
> Azure AD 그룹에 대 한 사용 권한을 추가 하려면 **그룹 종류가** **Office 365**이 아닌 **보안** 이어야 합니다. 이 옵션은 그룹을 만들 때 선택 됩니다. 자세한 내용은 [Create a basic group and add members using Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)(Azure Active Directory를 사용하여 기본 그룹 만들기 및 멤버 추가)를 참조하세요.

## <a name="next-steps"></a>다음 단계

- [Azure 위임 리소스 관리에 대한 권장 보안 방법](recommended-security-practices.md)을 알아봅니다.
- [Azure Resource Manager 템플릿을 사용](../how-to/onboard-customer.md)하거나 [개인 또는 공용 관리형 서비스 제품을 Azure Marketplace에 게시](../how-to/publish-managed-services-offers.md)하여 Azure 위임 리소스 관리에 고객을 온보딩합니다.
