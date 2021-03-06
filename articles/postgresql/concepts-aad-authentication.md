---
title: 활성 디렉터리 인증 - PostgreSQL용 Azure 데이터베이스 - 단일 서버
description: PostgreSQL - 단일 서버에 대한 Azure 데이터베이스를 사용하여 인증을 위한 Azure Active Directory의 개념에 대해 알아봅니다.
author: lfittl
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: ec853657d6dd1f3b019d8a414cfa28edc1083b29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74769917"
---
# <a name="use-azure-active-directory-for-authenticating-with-postgresql"></a>PostgreSQL을 사용하여 인증하기 위해 Azure Active Directory사용

Microsoft Azure Active Directory(Azure AD) 인증은 Azure AD에 정의된 ID를 사용하여 PostgreSQL용 Azure 데이터베이스에 연결하는 메커니즘입니다.
Azure AD 인증을 사용하면 중앙 위치에서 데이터베이스 사용자 ID 및 기타 Microsoft 서비스를 관리할 수 있으므로 권한 관리가 간소화됩니다.

> [!IMPORTANT]
> PostgreSQL용 Azure 데이터베이스에 대한 Azure AD 인증은 현재 공개 미리 보기상태입니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다.
> 자세한 내용은 [Microsoft Azure Preview에 대한 추가 사용 약관](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

Azure AD 사용의 이점은 다음과 같습니다.

- 일관된 방식으로 Azure 서비스 전반에 걸친 사용자 인증
- 한 곳에서 암호 정책 및 암호 회전 관리
- Azure Active Directory에서 지원하는 여러 형태의 인증으로 암호를 저장할 필요가 없습니다.
- 고객이 외부(Azure AD) 그룹을 사용하여 데이터베이스 사용 권한을 관리할 수 있습니다.
- Azure AD 인증은 PostgreSQL 데이터베이스 역할을 사용하여 데이터베이스 수준에서 ID를 인증합니다.
- PostgreSQL용 Azure 데이터베이스에 연결하는 응용 프로그램에 대한 토큰 기반 인증 지원

Azure Active Directory 인증을 구성하고 사용하려면 다음 프로세스를 사용합니다.

1. 필요에 따라 Azure Active Directory를 만들고 사용자 ID로 채웁니다.
2. 선택적으로 현재 Azure 구독과 연결된 Active Directory를 연결하거나 변경합니다.
3. PostgreSQL 서버에 대한 Azure 데이터베이스에 대한 Azure AD 관리자를 만듭니다.
4. Azure AD ID에 매핑된 데이터베이스에서 데이터베이스 사용자를 만듭니다.
5. Azure AD ID에 대한 토큰을 검색하고 로그인하여 데이터베이스에 연결합니다.

> [!NOTE]
> Azure AD를 만들고 채우는 방법을 알아보려면 PostgreSQL에 대한 Azure 데이터베이스로 Azure AD를 [구성하고 PostgreSQL에 대한 Azure 데이터베이스에 대한 Azure AD로 로그인하고 등록하는 방법을 알아보려면 Azure AD에 대해 Azure AD를 구성하고 로그인합니다.](howto-configure-sign-in-aad-authentication.md)

## <a name="architecture"></a>Architecture

다음 상위 수준 다이어그램은 PostgreSQL용 Azure 데이터베이스에서 Azure AD 인증을 사용하여 인증이 작동하는 방식을 요약합니다. 화살표는 통신 경로 나타냅니다.

![인증 흐름][1]

## <a name="administrator-structure"></a>관리자 구조

Azure AD 인증을 사용하는 경우 PostgreSQL 서버에 대한 두 개의 관리자 계정이 있습니다. 원래 PostgreSQL 관리자와 Azure AD 관리자입니다. Azure AD 계정을 기반으로 하는 관리자만 사용자 데이터베이스에서 최초 Azure AD 포함 데이터베이스 사용자를 만들 수 있습니다. Azure AD 관리자 로그인은 Azure AD 사용자나 Azure AD 그룹이 될 수 있습니다. 관리자가 그룹 계정인 경우 모든 그룹 구성원이 사용할 수 있으므로 PostgreSQL 서버에 대해 여러 Azure AD 관리자를 사용할 수 있습니다. 그룹 계정을 관리자로 사용하면 PostgreSQL 서버의 사용자 또는 권한을 변경하지 않고 Azure AD에서 그룹 구성원을 중앙에서 추가 및 제거할 수 있으므로 관리 용이성이 향상됩니다. 한 번에 하나의 Azure AD 관리자(그룹 또는 사용자)를 구성할 수 있습니다.

![관리자 구조][2]

## <a name="permissions"></a>사용 권한

Azure AD로 인증할 수 있는 새 사용자를 만들려면 데이터베이스에 `azure_ad_admin` 역할이 있어야 합니다. 이 역할은 PostgreSQL 서버에 대한 특정 Azure 데이터베이스에 대한 Azure AD 관리자 계정을 구성하여 할당됩니다.

새 Azure AD 데이터베이스 사용자를 만들려면 Azure AD 관리자로 연결해야 합니다. 이것은 [PostgreSQL에 대한 Azure 데이터베이스에 대한 Azure AD를 사용하여 구성 및 로그인에서](howto-configure-sign-in-aad-authentication.md)설명합니다.

모든 Azure AD 인증은 PostgreSQL용 Azure 데이터베이스에 대해 Azure AD 관리자를 만든 경우에만 가능합니다. Azure Active Directory 관리자가 서버에서 제거된 경우 이전에 만든 기존 Azure Active Directory 사용자는 Azure Active Directory 자격 증명을 사용하여 데이터베이스에 더 이상 연결할 수 없습니다.

## <a name="connecting-using-azure-ad-identities"></a>Azure AD ID를 사용하여 연결

Azure Active Directory 인증에서는 Azure AD ID를 사용하여 데이터베이스에 연결하는 다음 방법을 지원합니다.

- Azure Active Directory 암호
- Azure Active Directory 통합
- Azure Active Directory MFA 지원을 통한 유니버설 인증
- Active Directory 응용 프로그램 인증서 또는 클라이언트 암호 사용

Active Directory에 대해 인증한 후에는 토큰을 검색합니다. 이 토큰은 로그인을 위한 암호입니다.

> [!NOTE]
> Active Directory 토큰으로 연결하는 방법에 대한 자세한 내용은 [PostgreSQL에 대한 Azure 데이터베이스에 대한 Azure AD 구성 및 로그인을](howto-configure-sign-in-aad-authentication.md)참조하십시오.

## <a name="additional-considerations"></a>기타 고려 사항

- 관리 효율성을 높일 수 있게 관리자 권한으로 전용 Azure AD 그룹을 프로비전하는 것이 좋습니다.
- PostgreSQL 서버에 대한 Azure 데이터베이스에 대해 언제든지 하나의 Azure AD 관리자(사용자 또는 그룹)만 구성할 수 있습니다.
- PostgreSQL에 대한 Azure AD 관리자만 Azure Active Directory 계정을 사용하여 PostgreSQL용 Azure 데이터베이스에 처음 연결할 수 있습니다. Active Directory 관리자가 이후의 Azure AD 데이터베이스 사용자를 구성할 수 있습니다.
- 사용자가 Azure AD에서 삭제되면 해당 사용자는 더 이상 Azure AD로 인증할 수 없으므로 해당 사용자에 대한 액세스 토큰을 더 이상 획득할 수 없습니다. 이 경우 일치하는 역할은 데이터베이스에 남아 있지만 해당 역할을 사용하여 서버에 연결할 수 없습니다.
> [!NOTE]
> 토큰이 만료될 때까지 삭제된 Azure AD 사용자와 로그인할 수 있습니다(토큰 발급 후 최대 60분).  또한 PostgreSQL에 대 한 Azure 데이터베이스에서 사용자를 제거 하는 경우이 액세스는 즉시 취소 됩니다.
- Azure AD 관리자가 서버에서 제거되면 서버는 더 이상 Azure AD 테넌트와 연결되지 않으므로 서버에 대해 모든 Azure AD 로그인이 비활성화됩니다. 동일한 테넌트에서 새 Azure AD 관리자를 추가하면 Azure AD 로그인이 다시 활성화됩니다.
- PostgreSQL용 Azure 데이터베이스는 사용자 이름을 사용하는 대신 사용자의 고유한 Azure AD 사용자 ID를 사용하여 PostgreSQL 역할에 대한 액세스 토큰을 Azure 데이터베이스에 일치시다. 즉, Azure AD 사용자가 Azure AD에서 삭제되고 동일한 이름으로 만든 새 사용자가 있는 경우 PostgreSQL용 Azure 데이터베이스는 다른 사용자로 간주합니다. 따라서 사용자가 Azure AD에서 삭제된 다음 이름이 같은 새 사용자가 추가되면 새 사용자는 기존 역할과 연결할 수 없습니다. 이를 허용하려면 PostgreSQL Azure AD 관리자용 Azure 데이터베이스가 취소한 다음 사용자에게 "azure_ad_user" 역할을 부여하여 Azure AD 사용자 ID를 새로 고치도록 해야 합니다.

## <a name="next-steps"></a>다음 단계

- Azure AD를 만들고 채우는 방법을 알아보려면 PostgreSQL에 대한 Azure 데이터베이스로 Azure AD를 [구성하고 PostgreSQL에 대한 Azure 데이터베이스에 대한 Azure AD로 로그인하고 등록하는 방법을 알아보려면 Azure AD에 대해 Azure AD를 구성하고 로그인합니다.](howto-configure-sign-in-aad-authentication.md)
- PostgreSQL에 대한 로그인, 사용자 및 데이터베이스 역할 Azure Database에 대한 개요는 [PostgreSQL - 단일 서버의 Azure 데이터베이스에서 사용자 만들기를](howto-create-users.md)참조하십시오.

<!--Image references-->

[1]: ./media/concepts-aad-authentication/authentication-flow.png
[2]: ./media/concepts-aad-authentication/admin-structure.png
