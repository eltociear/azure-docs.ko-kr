---
title: '자습서: Azure Active Directory를 사용 하 고 자동 사용자 프로비저닝에 대 한 Soloinsight-CloudGate SSO 구성 | 마이크로 소프트 문서'
description: Soloinsight-CloudGate SSO에 사용자 계정을 자동으로 프로비전 및 프로비저닝 해제하도록 Azure Active Directory를 구성하는 방법을 알아봅니다.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: 07558ceb-d406-40e7-90b8-1b40fdc829e7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/14/2019
ms.author: Zhchia
ms.openlocfilehash: 6ab90a6aea262d5c7067f9f41b9ddfc090b7371d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77063198"
---
# <a name="tutorial-configure-soloinsight-cloudgate-sso-for-automatic-user-provisioning"></a>자습서: 자동 사용자 프로비저닝을 위한 Soloinsight-CloudGate SSO 구성

이 자습서의 목적은 Soloinsight-CloudGate SSO 및 Azure Active Directory(Azure AD)에서 수행할 단계를 보여 주어 사용자 및/또는 그룹을 Soloinsight-CloudGate SSO에 자동으로 프로비저닝 및 프로비저닝 해제하도록 Azure AD를 구성하는 것입니다.

> [!NOTE]
> 이 자습서에서는 Azure AD 사용자 프로비저닝 서비스에 기반하여 구축된 커넥터에 대해 설명합니다. 이 서비스의 기능, 작동 방법 및 질문과 대답에 대한 중요한 내용은 [Azure Active Directory를 사용하여 SaaS 애플리케이션의 사용자를 자동으로 프로비저닝 및 프로비저닝 해제](../app-provisioning/user-provisioning.md)를 참조하세요.
>
> 이 커넥터는 현재 공개 미리 보기로 있습니다. 미리 보기 기능의 Microsoft Azure 일반 사용 약관에 대한 자세한 내용은 [Microsoft Azure 미리 보기에 대한 추가 사용 조건](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)을 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

이 자습서에 설명된 시나리오에서는 사용자에게 이미 다음 필수 구성 요소가 있다고 가정합니다.

* Azure AD 테넌트
* [솔로인사이트-클라우드게이트 SSO 테넌트](https://www.soloinsight.com/)
* 관리자 권한이 있는 솔로인사이트-CloudGate SSO의 사용자 계정입니다.

## <a name="assigning-users-to-soloinsight-cloudgate-sso"></a>솔로인사이트-클라우드게이트 SSO에 사용자 할당

Azure Active Directory는 *할당이라는* 개념을 사용하여 선택한 앱에 대한 액세스 권한을 받아야 하는 사용자를 결정합니다. 자동 사용자 프로비저닝의 컨텍스트에서는 Azure AD의 응용 프로그램에 할당된 사용자 및/또는 그룹만 동기화됩니다.

자동 사용자 프로비저닝을 구성하고 활성화하기 전에 Azure AD의 사용자 및/또는 그룹이 Soloinsight-CloudGate SSO에 액세스해야 하는지 결정해야 합니다. 일단 결정, 당신은 여기에 지침에 따라 Soloinsight-CloudGate SSO에 이러한 사용자 및 / 또는 그룹을 할당 할 수 있습니다 :
* [엔터프라이즈 앱에 사용자 또는 그룹 할당](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-soloinsight-cloudgate-sso"></a>사용자를 솔로인사이트-클라우드게이트 SSO에 할당하기 위한 중요한 팁

* 단일 Azure AD 사용자가 자동 사용자 프로비저닝 구성을 테스트하기 위해 Soloinsight-CloudGate SSO에 할당되는 것이 좋습니다. 추가 사용자 및/또는 그룹은 나중에 할당할 수도 있습니다.

* 사용자를 Soloinsight-CloudGate SSO에 할당할 때 할당 대화 상자에서 유효한 응용 프로그램 별 역할(사용 가능한 경우)을 선택해야 합니다. **기본 액세스** 역할이 있는 사용자는 프로비저닝에서 제외됩니다.

## <a name="set-up-soloinsight-cloudgate-sso-for-provisioning"></a>프로비저닝을 위한 솔로인사이트-클라우드게이트 SSO 설정

1. [솔로인사이트-클라우드게이트 SSO 관리 콘솔에](https://soloinsight.sigateway.com/login)로그인하십시오. 시스템 설정 > **관리로**이동합니다.

    ![솔로인사이트-클라우드게이트 SSO 관리 콘솔](media/soloinsight-cloudgate-sso-provisioning-tutorial/admin.png)

2.  **일반**로 이동합니다.

    ![솔로인사이트-클라우드게이트 SSO, SCIM 추가](media/soloinsight-cloudgate-sso-provisioning-tutorial/config.png)

3.  **테넌트 URL** 및 **비밀 토큰을**얻으려면 페이지 끝으로 스크롤합니다. 비밀 **토큰을**복사합니다. 이 값은 Azure 포털에서 Soloinsight-CloudGate SSO 응용 프로그램의 프로비저닝 탭의 비밀 토큰 필드에 입력됩니다.

    ![솔로인사이트-클라우드게이트 SSO 토큰 생성](media/soloinsight-cloudgate-sso-provisioning-tutorial/token.png)

## <a name="add-soloinsight-cloudgate-sso-from-the-gallery"></a>갤러리에서 솔로인사이트-클라우드게이트 SSO 추가

Azure AD를 사용하여 자동 사용자 프로비전을 위해 Soloinsight-CloudGate SSO를 구성하기 전에 Azure AD 응용 프로그램 갤러리에서 관리되는 SaaS 응용 프로그램 목록에 Soloinsight-CloudGate SSO를 추가해야 합니다.

**Azure AD 응용 프로그램 갤러리에서 Soloinsight-CloudGate SSO를 추가하려면 다음 단계를 수행합니다.**

1. Azure **[포털에서](https://portal.azure.com)** 왼쪽 탐색 패널에서 **Azure Active Directory**를 선택합니다.

    ![Azure Active Directory 단추](common/select-azuread.png)

2. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

3. 새 응용 프로그램을 추가하려면 창 상단의 **새 응용 프로그램** 단추를 선택합니다.

    ![새 애플리케이션 단추](common/add-new-app.png)

4. 검색 상자에 **Soloinsight-CloudGate SSO를**입력하고 결과 패널에서 **Soloinsight-CloudGate SSO를** 선택한 다음 **추가** 단추를 클릭하여 응용 프로그램을 추가합니다.

    ![결과 목록의 Soloinsight-CloudGate SSO](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-soloinsight-cloudgate-sso"></a>솔로인사이트-클라우드게이트 SSO에 자동 사용자 프로비전 구성 

이 섹션에서는 Azure AD의 사용자 및/또는 그룹 할당을 기반으로 Soloinsight-CloudGate SSO에서 사용자 및/또는 그룹을 생성, 업데이트 및 비활성화하도록 Azure AD 프로비저닝 서비스를 구성하는 단계를 안내합니다.

> [!TIP]
> 솔로인사이트-CloudGate SSO 의 지침에 따라 솔로인사이트-CloudGate SSO 에 대한 SAML 기반 [단일 사인온을](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial)사용하도록 선택할 수도 있습니다. 단일 사인온은 자동 사용자 프로비저닝과 독립적으로 구성할 수 있지만 이 두 기능은 서로 보완합니다.

### <a name="to-configure-automatic-user-provisioning-for-soloinsight-cloudgate-sso-in-azure-ad"></a>Azure AD에서 Soloinsight-CloudGate SSO에 대한 자동 사용자 프로비저닝을 구성하려면 다음을 수행하십시오.

1. [Azure 포털에](https://portal.azure.com)로그인합니다. **엔터프라이즈 응용 프로그램을**선택한 다음 모든 응용 프로그램을 **선택합니다.**

    ![엔터프라이즈 애플리케이션 블레이드](common/enterprise-applications.png)

2. 애플리케이션 목록에서 **Soloinsight-CloudGate SSO**를 선택합니다.

    ![애플리케이션 목록의 Soloinsight-CloudGate SSO 링크](common/all-applications.png)

3. **프로비저닝** 탭을 선택합니다.

    ![프로비저닝 탭](common/provisioning.png)

4. **프로비저닝 모드를** **자동으로**설정합니다.

    ![프로비저닝 탭](common/provisioning-automatic.png)

5. 관리자 **자격 증명** 섹션에서 `https://sigateway.com/scim/v2/sync/serviceproviderconfig` **테넌트 URL에**입력합니다. **비밀** **토큰의** 앞에서 검색한 SCIM 인증 토큰 값을 입력합니다. Azure AD가 Soloinsight-CloudGate SSO에 연결할 수 있는지 확인하려면 **테스트 연결을** 클릭합니다. 연결이 실패하면 Soloinsight-CloudGate SSO 계정에 관리자 권한이 있는지 확인하고 다시 시도하십시오.

    ![테넌트 URL + 토큰](common/provisioning-testconnection-tenanturltoken.png)

6. **알림 메일** 필드에 프로비저닝 오류 알림을 받을 개인 또는 그룹의 메일 주소를 입력하고, **오류가 발생할 경우, 메일 알림 보내기** 확인란을 선택합니다.

    ![알림 이메일](common/provisioning-notification-email.png)

7. **저장**을 클릭합니다.

8. **매핑** 섹션에서 **Azure Active Directory 사용자 동기화를 솔로인사이트-CloudGate SSO로**선택합니다.

    ![솔로인사이트-클라우드게이트 SSO 사용자 매핑](media/soloinsight-cloudgate-sso-provisioning-tutorial/usermappings.png)

9. **속성 매핑** 섹션에서 Azure AD에서 Soloinsight-CloudGate SSO로 동기화된 사용자 특성을 검토합니다. **일치** 속성으로 선택한 특성은 업데이트 작업을 위해 Soloinsight-CloudGate SSO의 사용자 계정을 일치시키는 데 사용됩니다. **저장** 단추를 선택하여 변경 내용을 커밋합니다.

    ![솔로인사이트-클라우드게이트 SSO 사용자 속성](media/soloinsight-cloudgate-sso-provisioning-tutorial/userattributes.png)

10. **매핑** 섹션에서 **Azure Active Directory 그룹 동기화를 솔로인사이트-CloudGate SSO로**선택합니다.

    ![솔로인사이트-클라우드게이트 SSO 그룹 매핑](media/soloinsight-cloudgate-sso-provisioning-tutorial/groupmappings.png)

11. **속성 매핑** 섹션에서 Azure AD에서 Soloinsight-CloudGate SSO로 동기화된 그룹 특성을 검토합니다. **일치** 속성으로 선택한 특성은 업데이트 작업에 대한 Soloinsight-CloudGate SSO의 그룹을 일치시키는 데 사용됩니다. **저장** 단추를 선택하여 변경 내용을 커밋합니다.

    ![솔로인사이트-클라우드게이트 SSO 그룹 속성](media/soloinsight-cloudgate-sso-provisioning-tutorial/groupattributes.png)

12. 범위 지정 필터를 구성하려면 [범위 지정 필터 자습서](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)에서 제공하는 다음 지침을 참조합니다.

13. Soloinsight-CloudGate SSO에 대한 Azure AD 프로비저닝 서비스를 사용하려면 **설정** 섹션에서 **프로비저닝 상태를** **켜기로** 변경합니다.

    ![프로비전 상태 켜기로 전환](common/provisioning-toggle-on.png)

14. **설정** 섹션의 **범위에서** 원하는 값을 선택하여 Soloinsight-CloudGate SSO에 프로비전할 사용자 및/또는 그룹을 정의합니다.

    ![프로비전 범위](common/provisioning-scope.png)

15. 프로비전할 준비가 되면 **저장**을 클릭합니다.

    ![프로비전 구성 저장](common/provisioning-configuration-save.png)

이 작업은 **설정**의 **범위** 섹션에 정의된 모든 사용자 및/또는 그룹의 초기 동기화를 시작합니다. 초기 동기화는 Azure AD 프로비전 서비스가 실행되는 동안 약 40분마다 발생하는 후속 동기화보다 더 많은 시간이 걸립니다. 동기화 세부 **정보** 섹션을 사용하여 진행 상황을 모니터링하고 Soloinsight-CloudGate SSO에서 Azure AD 프로비저닝 서비스에서 수행하는 모든 작업을 설명하는 프로비저닝 활동 보고서에 대한 링크를 따를 수 있습니다.

Azure AD 프로비저닝 로그를 읽는 방법에 대한 자세한 내용은 [자동 사용자 계정 프로비저닝에 대한 보고](../app-provisioning/check-status-user-account-provisioning.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [엔터프라이즈 앱용 사용자 계정 프로비저닝 관리](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Azure Active Directory의 애플리케이션 액세스 및 Single Sign-On이란 무엇입니까?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>다음 단계

* [프로비저닝 작업에 대한 로그를 검토하고 보고서를 받아보는 방법을 알아봅니다](../app-provisioning/check-status-user-account-provisioning.md).

