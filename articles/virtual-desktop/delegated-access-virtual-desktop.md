---
title: Windows 가상 데스크톱에서 위임된 액세스 - Azure
description: 예제를 포함하여 Windows 가상 데스크톱 배포에서 관리 기능을 위임하는 방법
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 91451ff3024a9a5019b3982b0e4471e2c4d80c74
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683911"
---
# <a name="delegated-access-in-windows-virtual-desktop"></a>Windows Virtual Desktop에서 위임된 액세스

Windows Virtual Desktop에는 특정 사용자가 역할을 할당하여 가질 수 있는 액세스 양을 정의할 수 있는 위임된 액세스 모델이 있습니다. 역할 할당에는 보안 주체, 역할 정의 및 범위의 세 가지 구성 요소가 있습니다. Windows 가상 데스크톱 위임된 액세스 모델은 Azure RBAC 모델을 기반으로 합니다. 특정 역할 할당 및 해당 구성 요소에 대한 자세한 내용은 [Azure 역할 기반 액세스 제어 개요를](../role-based-access-control/built-in-roles.md)참조하십시오.

Windows 가상 데스크톱 위임된 액세스는 역할 할당의 각 요소에 대해 다음 값을 지원합니다.

* 보안 주체
    * 사용자
    * 서비스 사용자
* 역할 정의
    * 기본 제공 역할
* 범위
    * 테넌트 그룹
    * 테넌트
    * 호스트 풀
    * 앱 그룹

## <a name="built-in-roles"></a>기본 제공 역할

Windows Virtual Desktop의 위임된 액세스에는 사용자 및 서비스 주체에게 할당할 수 있는 몇 가지 기본 제공 역할 정의가 있습니다.

* RDS 소유자는 리소스에 대한 액세스를 포함하여 모든 것을 관리할 수 있습니다.
* RDS 기고자는 모든 것을 관리할 수 있지만 리소스에 액세스할 수는 없습니다.
* RDS 리더는 모든 것을 볼 수 있지만 변경할 수는 없습니다.
* RDS 운영자는 진단 활동을 볼 수 있습니다.

## <a name="powershell-cmdlets-for-role-assignments"></a>역할 할당을 위한 PowerShell cmdlet

다음 cmdlet을 실행하여 역할 할당을 생성, 보기 및 제거할 수 있습니다.

* **Get-RdsRoleAssignment** 역할 할당 목록을 표시 합니다.
* **새 RdsRole할당은** 새 역할 할당을 만듭니다.
* **RdsRole할당 제거는** 역할 할당을 삭제합니다.

### <a name="accepted-parameters"></a>허용된 매개 변수

다음 매개 변수를 사용 하 고 기본 세 cmdlet을 수정할 수 있습니다.

* **AadTenantId**: 서비스 주체가 구성원인 Azure Active Directory 테넌트 ID를 지정합니다.
* **AppGroupName**: 원격 데스크톱 앱 그룹의 이름입니다.
* **진단**: 진단 범위를 나타냅니다. **(인프라** 또는 **테넌트** 매개 변수와 페어링해야 합니다.)
* **HostPoolName**: 원격 데스크톱 호스트 풀의 이름입니다.
* **인프라**: 인프라 범위를 나타냅니다.
* **RoleDefinitionName**: 사용자, 그룹 또는 앱에 할당된 원격 데스크톱 서비스 역할 기반 액세스 제어 역할의 이름입니다. (예를 들어, 원격 데스크톱 서비스 소유자, 원격 데스크톱 서비스 리더 등)
* **서버 원칙 이름**: Azure Active Directory 응용 프로그램의 이름입니다.
* **SignInName**: 사용자의 이메일 주소 또는 사용자 주체 이름입니다.
* **테넌트 이름**: 원격 데스크톱 테넌트의 이름입니다.

## <a name="next-steps"></a>다음 단계

각 역할이 사용할 수 있는 PowerShell cmdlet의 전체 목록은 [PowerShell 참조를](/powershell/windows-virtual-desktop/overview)참조하십시오.

Windows 가상 데스크톱 환경을 설정하는 방법에 대한 지침은 [Windows 가상 데스크톱 환경을](environment-setup.md)참조하십시오.
