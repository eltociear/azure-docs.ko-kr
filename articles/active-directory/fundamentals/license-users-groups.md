---
title: 라이선스 할당 또는 제거 - Azure Active Directory | Microsoft Docs
description: 사용자 또는 그룹에서 Azure Active Directory 라이선스를 할당하거나 제거하는 방법에 대한 지침입니다.
services: active-directory
author: msaburnley
manager: daveba
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: ajburnle
ms.reviewer: jeffsta
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 20ec1d1909a53a9de29e12be33957acfd1643698
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80128837"
---
# <a name="assign-or-remove-licenses-in-the-azure-active-directory-portal"></a>Azure Active Directory 포털에서 라이선스 할당 또는 제거

많은 Azure Active Directory(Azure AD) 서비스에는 해당 서비스에 대한 각 사용자 또는 그룹(및 관련 멤버)에 라이선스를 부여해야 합니다. 활성 라이선스를 가진 사용자만 해당 라이선스가 있는 Azure AD 서비스에 액세스하고 사용할 수 있습니다. 라이선스는 테넌트당 적용되며 다른 테넌트로 이전되지 않습니다. 

## <a name="available-license-plans"></a>사용 가능한 라이센스 플랜

Azure AD 서비스에 사용할 수 있는 몇 가지 라이선스 계획(다음을 포함)이 있습니다.

- Azure AD Free

- Azure AD Premium P1

- Azure AD Premium P2

각 라이선스 플랜 및 관련 라이선스 세부 정보에 대한 자세한 내용은 [어떤 라이선스가 필요한지](https://azure.microsoft.com/pricing/details/active-directory/)를 참조하십시오.

일부 Microsoft 서비스는 모든 위치에서 사용할 수 없습니다. 그룹에 라이선스를 할당하려면 먼저 모든 멤버에 대한 **사용 위치**를 지정해야 합니다. 이 값은 Azure AD의 **Azure Active Directory &gt; 사용자 &gt; 프로필 &gt; 설정** 영역에서 설정할 수 있습니다. 사용 위치를 지정하지 않은 모든 사용자는 Azure AD 조직의 위치를 상속합니다.

## <a name="view-license-plans-and-plan-details"></a>라이선스 계획 및 계획 세부 정보 보기

개별 라이선스를 포함하여 사용 가능한 서비스 계획을 보고, 보류 중인 만료 날짜를 확인하고, 사용 가능한 할당 수를 볼 수 있습니다.

### <a name="to-find-your-service-plan-and-plan-details"></a>서비스 계획 및 계획 세부 정보를 찾으려면

1. Azure AD 조직의 라이센스 관리자 계정을 사용하여 Azure [포털에](https://portal.azure.com/) 로그인합니다.

1. **Azure Active Directory**를 선택한 다음, **라이선스**를 선택합니다.

    ![구매한 서비스 수 및 할당된 라이선스가 있는 라이선스 페이지](media/license-users-groups/license-details-blade.png)

1. **구매한** 링크를 선택하여 **제품** 페이지를 보고 라이선스 플랜에 대한 **할당된**' **사용 가능'** 및 **만료 예정인** 숫자를 확인합니다.

    ![서비스 페이지, 서비스 라이센스 계획 및 관련 라이센스 정보](media/license-users-groups/license-products-blade-with-products.png)

1. 계획 이름을 선택하여 라이선스가 부여된 사용자 및 그룹을 확인합니다.

## <a name="assign-licenses-to-users-or-groups"></a>사용자 또는 그룹에 라이선스 할당

사용이 허가된 Azure AD 서비스를 사용해야 하는 모든 사용자에게 적절한 라이선스가 있는지 확인합니다. 사용자 또는 전체 그룹에 라이선스 권한을 추가할 수 있습니다.

### <a name="to-assign-a-license-to-a-user"></a>사용자에게 라이선스를 할당하려면

1. **제품** 페이지에서 사용자에게 할당할 라이선스 계획의 이름을 선택합니다.

    ![서비스 페이지, 강조 표시된 서비스 라이센스 계획](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. 라이센스 계획 개요 페이지에서 **에 대한 할당을**선택합니다.

    ![서비스 페이지, 강조 표시된 할당 옵션](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. **할당** 페이지에서 **사용자 및 그룹**을 선택한 다음, 라이선스를 할당하려는 사용자를 검색하여 선택합니다.

    ![검색 및 선택 옵션이 강조 표시된 라이선스 할당 페이지](media/license-users-groups/assign-license-blade-with-highlight.png)

1. **할당 옵션**을 선택하고, 적절한 라이선스 옵션이 설정되어 있는지 확인한 다음, **확인**을 선택합니다.

    ![라이센스 옵션 페이지( 라이센스 플랜에서 사용할 수 있는 모든 옵션)](media/license-users-groups/license-option-blade-assignments.png)

    **라이선스 할당** 페이지가 업데이트되어 사용자가 선택되고 할당이 구성되었음을 보여 줍니다.

    > [!NOTE]
    > 일부 Microsoft 서비스는 모든 위치에서 사용할 수 없습니다. 사용자에게 라이선스를 할당하려면 먼저 **사용 위치**를 지정해야 합니다. 이 값은 Azure AD의 **Azure Active Directory &gt; 사용자 &gt; 프로필 &gt; 설정** 영역에서 설정할 수 있습니다. 사용 위치를 지정하지 않은 모든 사용자는 Azure AD 조직의 위치를 상속합니다.

1. **을 선택합니다.**

    사용자가 사용이 허가된 사용자 목록에 추가되며, 포함된 Azure AD 서비스에 액세스할 수 있습니다.
    > [!NOTE]
    > 사용자의 라이선스 페이지에서 사용자에게 직접 라이선스를 할당할 수도 **있습니다.** 사용자가 그룹 구성원 자격을 통해 할당된 라이선스를 가지고 있고 동일한 라이선스를 사용자에게 직접 할당하려는 경우 1단계에서만 언급된 **제품** 페이지에서만 수행할 수 있습니다.

### <a name="to-assign-a-license-to-a-group"></a>그룹에 라이선스를 할당하려면

1. **제품** 페이지에서 사용자에게 할당할 라이선스 계획의 이름을 선택합니다.

    ![제품 블레이드, 강조 표시된 제품 라이센스 계획](media/license-users-groups/license-products-blade-with-product-highlight.png)

1. **Azure Active Directory Premium 요금제 2** 페이지에서 **할당**을 선택합니다.

    ![할당 옵션이 강조 표시된 제품 페이지](media/license-users-groups/license-products-blade-with-assign-option-highlight.png)

1. **할당** 페이지에서 **사용자 및 그룹**을 선택한 다음, 라이선스를 할당하려는 그룹을 검색하여 선택합니다.

    ![검색 및 선택 옵션이 강조 표시된 라이선스 할당 페이지](media/license-users-groups/assign-group-license-blade-with-highlight.png)

1. **할당 옵션**을 선택하고, 적절한 라이선스 옵션이 설정되어 있는지 확인한 다음, **확인**을 선택합니다.

    ![라이센스 옵션 페이지( 라이센스 플랜에서 사용할 수 있는 모든 옵션)](media/license-users-groups/license-option-blade-group-assignments.png)

    **라이선스 할당** 페이지가 업데이트되어 사용자가 선택되고 할당이 구성되었음을 보여 줍니다.

1. **을 선택합니다.**

    그룹이 사용이 허가된 그룹 목록에 추가되며, 모든 멤버가 포함된 Azure AD 서비스에 액세스할 수 있습니다.

## <a name="remove-a-license"></a>라이선스 제거

사용자의 Azure AD 사용자 페이지에서 라이선스를 제거하거나, 그룹 할당에 대한 그룹 개요 페이지에서 라이선스를 제거하거나, Azure AD **라이선스** 페이지에서 시작하여 라이선스에 대한 사용자 및 그룹을 볼 수 있습니다.

### <a name="to-remove-a-license-from-a-user"></a>사용자로부터 라이선스를 제거하려면

1. 서비스 계획에 대한 **사용이 허가된 사용자** 페이지에서 더 이상 라이선스가 없어야 하는 사용자를 선택합니다. 예를 들어 _Alain Charon_입니다.

1. **라이선스 제거**를 선택합니다.

    ![라이선스 제거 옵션이 강조 표시된 허가된 사용자 페이지](media/license-users-groups/license-products-user-blade-with-remove-option-highlight.png)

> [!IMPORTANT]
> 사용자가 그룹에서 상속하는 라이선스는 직접 제거할 수 없습니다. 대신, 라이선스를 상속하는 그룹에서 사용자를 제거해야 합니다.

### <a name="to-remove-a-license-from-a-group"></a>그룹에서 라이선스를 제거하려면

1. 라이선스 계획에 대한 **사용권 그룹** 페이지에서 더 이상 라이선스가 없어야 하는 그룹을 선택합니다.

1. **라이선스 제거**를 선택합니다.

    ![라이선스 제거 옵션이 강조 표시된 허가된 그룹 페이지](media/license-users-groups/license-products-group-blade-with-remove-option-highlight.png)
    
    > [!NOTE]
    > Azure AD에 동기화된 온-프레미스 사용자 계정이 동기화 범위를 벗어나거나 동기화가 제거되면 Azure AD에서 사용자가 일시 삭제됩니다. 이 경우 사용자에게 직접 또는 그룹 기반 라이선스를 통해 할당된 라이선스는 **삭제되지**않고 **일시 중단된** 것으로 표시됩니다.

## <a name="next-steps"></a>다음 단계

라이선스가 할당되면 다음 프로세스를 수행할 수 있습니다.

- [라이선스 할당 문제 식별 및 해결](../users-groups-roles/licensing-groups-resolve-problems.md)

- [라이선싱을 위해 그룹에 허가된 사용자 추가](../users-groups-roles/licensing-groups-migrate-users.md)

- [Azure Active Directory에서 라이선스 관리를 위해 그룹을 사용하는 경우 시나리오, 제한 사항 및 알려진 문제](../users-groups-roles/licensing-group-advanced.md)

- [프로필 정보 추가 또는 변경](active-directory-users-profile-azure-portal.md)
