---
title: '자습서: TMWS(Trend Micro Web Security)와 Azure Active Directory SSO(Single Sign-On) 통합 | Microsoft Docs'
description: Azure Active Directory와 TMWS(Trend Micro Web Security) 간에 Single Sign-On을 구성하는 방법을 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 827285d3-8e65-43cd-8453-baeda32ef174
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a4c2cddbc9086c80922fcf9c5d96cd197ab4778
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425282"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-trend-micro-web-securitytmws"></a>자습서: TMWS(Trend Micro Web Security)와 Azure Active Directory SSO(Single Sign-On) 통합

이 자습서에서는 Azure AD(Azure Active Directory)와 TMWS(Trend Micro Web Security)를 통합하는 방법에 대해 알아봅니다. Azure AD와 TMWS(Trend Micro Web Security)를 통합하면 다음을 수행할 수 있습니다.

* Azure AD에서 TMWS(Trend Micro Web Security)에 액세스할 수 있는 사용자를 제어합니다.
* 사용자가 해당 Azure AD 계정으로 TMWS(Trend Micro Web Security)에 자동으로 로그인되도록 설정합니다.
* 단일 중앙 위치인 Azure Portal에서 계정을 관리합니다.

Azure AD와 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)를 참조하세요.

## <a name="prerequisites"></a>사전 요구 사항

시작하려면 다음 항목이 필요합니다.

* Azure AD 구독 구독이 없는 경우 [체험 계정](https://azure.microsoft.com/free/)을 얻을 수 있습니다.
* TMWS(Trend Micro Web Security) SSO(Single Sign-On)를 사용하도록 설정된 구독

## <a name="scenario-description"></a>시나리오 설명

이 자습서에서는 테스트 환경에서 Azure AD SSO를 구성하고 테스트합니다.

* TMWS(Trend Micro Web Security)에서 **SP** 시작 SSO를 지원합니다.
* TMWS(Trend Micro Web Security)가 구성되면 세션 제어를 적용하여 조직의 중요한 데이터의 반출 및 반입을 실시간으로 보호할 수 있습니다. 세션 제어는 조건부 액세스에서 확장됩니다. [Microsoft Cloud App Security를 사용하여 세션 제어를 적용하는 방법을 알아봅니다](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-trend-micro-web-securitytmws-from-the-gallery"></a>갤러리에서 TMWS(Trend Micro Web Security) 추가

TMWS(Trend Micro Web Security)가 Azure AD에 통합되도록 구성하려면 갤러리의 TMWS(Trend Micro Web Security)를 관리형 SaaS 앱 목록에 추가해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 회사 또는 학교 계정, 개인 Microsoft 계정으로 로그인합니다.
1. 왼쪽 탐색 창에서 **Azure Active Directory** 서비스를 선택합니다.
1. **엔터프라이즈 애플리케이션**으로 이동한 다음, **모든 애플리케이션**을 선택합니다.
1. 새 애플리케이션을 추가하려면 **새 애플리케이션**을 선택합니다.
1. **갤러리에서 추가** 섹션의 검색 상자에서 **TMWS(Trend Micro Web Security)** 를 입력합니다.
1. 결과 패널에서 **TMWS(Trend Micro Web Security)** 를 선택한 다음, 앱을 추가합니다. 앱이 테넌트에 추가될 때까지 잠시 동안 기다려 주세요.

## <a name="configure-and-test-azure-ad-single-sign-on-for-trend-micro-web-securitytmws"></a>TMWS(Trend Micro Web Security)에 대한 Azure AD Single Sign-On 구성 및 테스트

**B.Simon**이라는 테스트 사용자를 사용하여 TMWS(Trend Micro Web Security)에서 Azure AD SSO를 구성하고 테스트합니다. SSO가 작동하려면 Azure AD 사용자와 TMWS(Trend Micro Web Security)의 관련 사용자 간에 연결 관계를 설정해야 합니다.

TMWS(Trend Micro Web Security)에서 Azure AD SSO를 구성하고 테스트하려면 다음 구성 요소를 완료합니다.

1. **[Azure AD SSO 구성](#configure-azure-ad-sso)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
    1. **[Azure AD 테스트 사용자 만들기](#create-an-azure-ad-test-user)** - B.Simon을 사용하여 Azure AD Single Sign-On을 테스트합니다.
    1. **[Azure AD 테스트 사용자 할당](#assign-the-azure-ad-test-user)** - B. Simon이 Azure AD Single Sign-On을 사용할 수 있도록 합니다.
    1. **[Azure AD에서 사용자 및 그룹 동기화 설정 구성](#configure-user-and-group-synchronization-settings-in-azure-ad)** - Azure AD에서 사용자 및 그룹 동기화 설정을 구성합니다.
1. **[TMWS(Trend Micro Web Security) SSO 구성](#configure-trend-micro-web-security-sso)** - 애플리케이션 쪽에서 Single Sign-On 설정을 구성합니다.
1. **[SSO 테스트](#test-sso)** - 구성이 작동하는지 여부를 확인합니다.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 구성

Azure Portal에서 Azure AD SSO를 사용하도록 설정하려면 다음 단계를 수행합니다.

1. [Azure Portal](https://portal.azure.com/)의 **TMWS(Trend Micro Web Security)** 애플리케이션 통합 페이지에서 **관리** 섹션을 찾고, **Single Sign-On**을 선택합니다.
1. **Single Sign-On 방법 선택** 페이지에서 **SAML**을 선택합니다.
1. **SAML로 Single Sign-On 설정** 페이지에서 **기본 SAML 구성**에 대한 편집(연필 모양) 아이콘을 클릭하여 설정을 편집합니다.

   ![기본 SAML 구성 편집](common/edit-urls.png)

1. **기본 SAML 구성** 섹션에서 다음 필드에 대한 값을 입력합니다.

    a. **식별자(엔터티 ID)** 텍스트 상자에서 `https://auth.iws-hybrid.trendmicro.com/([0-9a-f]{16})` 패턴을 사용하는 URL을 입력합니다.

    b. **회신 URL** 텍스트 상자에서 `https://auth.iws-hybrid.trendmicro.com/simplesaml/module.php/saml/sp/saml2-acs.php/ics-sp` URL을 입력합니다.

    > [!NOTE]
    > 식별자 값은 실제 값이 아닙니다. 실제 식별자로 이 값을 업데이트하세요. 식별자 값을 얻으려면 [TMWS(Trend Micro Web Security) 클라이언트 지원 팀](https://success.trendmicro.com/contact-support-north-america)에 문의하세요. Azure Portal의 **기본 SAML 구성** 섹션에 표시된 패턴을 참조할 수도 있습니다.

1. TMWS(Trend Micro Web Security) 애플리케이션에는 사용자 지정 특성 매핑을 SAML 토큰 특성 구성에 추가해야 하는 특정 형식의 SAML 어설션이 필요합니다. 다음 스크린샷에서는 기본 특성의 목록을 보여 줍니다.

    ![이미지](common/default-attributes.png)

1. 위에서 언급한 특성 외에도 TMWS(Trend Micro Web Security) 애플리케이션에는 아래에 표시된 SAML 응답에서 다시 전달되어야 하는 몇 가지 특성이 추가로 필요합니다. 이러한 특성도 미리 채워져 있지만 요구 사항에 따라 검토할 수 있습니다.
    
    | 속성 | 원본 특성|
    | --------------- | --------- |
    | sAMAccountName | user.onpremisessamaccountname |
    | uPN | user.userprincipalname |

1. **SAML로 Single Sign-On 설정** 페이지의 **SAML 서명 인증서** 섹션에서 **인증서(Base64)** 를 찾은 후 **다운로드**를 선택하여 인증서를 다운로드하고 컴퓨터에 저장합니다.

    ![인증서 다운로드 링크](common/certificatebase64.png)

1. **TMWS(Trend Micro Web Security) 설정** 섹션에서 요구 사항에 따라 적절한 URL을 복사합니다.

    ![구성 URL 복사](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기

이 섹션에서는 Azure Portal에서 B.Simon이라는 테스트 사용자를 만듭니다.

1. Azure Portal의 왼쪽 창에서 **Azure Active Directory**, **사용자**, **모든 사용자**를 차례로 선택합니다.
1. 화면 위쪽에서 **새 사용자**를 선택합니다.
1. **사용자** 속성에서 다음 단계를 수행합니다.
   1. **이름** 필드에 `B.Simon`을 입력합니다.  
   1. **사용자 이름** 필드에서 username@companydomain.extension을 입력합니다. `B.Simon@contoso.com`)을 입력합니다.
   1. **암호 표시** 확인란을 선택한 다음, **암호** 상자에 표시된 값을 적어둡니다.
   1. **만들기**를 클릭합니다.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 B.Simon에게 TMWS(Trend Micro Web Security)에 대한 액세스 권한을 부여합니다.

1. Azure Portal에서 **엔터프라이즈 애플리케이션**을 선택한 다음, **모든 애플리케이션**을 선택합니다.
1. 애플리케이션 목록에서 **TMWS(Trend Micro Web Security)** 를 선택합니다.
1. 앱의 개요 페이지에서 **관리** 섹션을 찾고 **사용자 및 그룹**을 선택합니다.

   !["사용자 및 그룹" 링크](common/users-groups-blade.png)

1. **사용자 추가**를 선택한 다음, **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 추가 링크](common/add-assign-user.png)

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **B.Simon**을 선택한 다음, 화면 아래쪽에서 **선택** 단추를 클릭합니다.
1. SAML 어설션에 역할 값이 필요한 경우 **역할 선택** 대화 상자의 목록에서 사용자에 대한 적절한 역할을 선택한 다음, 화면의 아래쪽에 있는 **선택** 단추를 클릭합니다.
1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.

### <a name="configure-user-and-group-synchronization-settings-in-azure-ad"></a>Azure AD에서 사용자 및 그룹 동기화 설정 구성

1. 왼쪽 탐색 영역에서 **Azure Active Directory**를 클릭합니다.

1. **관리** 아래에서 **앱 등록**을 클릭한 다음, **모든 애플리케이션** 영역 아래에서 새 엔터프라이즈 애플리케이션을 클릭합니다.

1. **관리** 아래에서 **인증서 및 비밀**을 선택합니다.

1. 표시되는 [클라이언트 암호] 영역에서 **새 클라이언트 암호**를 클릭합니다.

1. 표시되는 [클라이언트 암호 추가] 화면에서 필요에 따라 설명을 추가하고, 이 클라이언트 암호의 만료 기간을 선택한 다음, **추가**를 클릭합니다. 새로 추가된 클라이언트 암호는 [클라이언트 암호] 영역 아래에 표시됩니다.

1. 값을 기록합니다. 이 정보는 나중에 TMWS에 입력합니다.

1. **관리** 아래에서 **API 권한**을 클릭합니다. 

1. 표시되는 [API 권한] 화면에서 **권한 추가**를 클릭합니다.

1. 표시되는 [API 권한 요청] 화면의 Microsoft API 탭에서 **Microsoft Graph**, **애플리케이션 권한**을 차례로 클릭합니다.

1. 다음 권한을 찾아서 추가합니다. 

    * Group.Read.All
    * User.Read.All

1. **권한 추가**를 클릭합니다. 설정이 성공적으로 저장되었는지 확인하는 메시지가 표시됩니다. 새로 추가된 권한이 [API 권한] 화면에 표시됩니다.

1. [동의 허용] 영역 아래에서 **<관리자 계정>에 대한 관리자 동의 허용(기본 디렉터리)** 을 클릭한 다음, **예**를 클릭합니다. 요청된 권한에 대한 관리자 동의가 성공적으로 부여되었는지 확인하는 메시지가 표시됩니다.

1. **개요**를 클릭합니다. 

1. 표시되는 오른쪽 창에서 애플리케이션(클라이언트) ID와 디렉터리(테넌트) ID를 기록합니다. 이 정보는 나중에 TMWS에 입력합니다. 또한 Azure **Active Directory > 관리** 아래에서 **사용자 지정 도메인 이름**을 클릭하고, 오른쪽 창에서 도메인 이름을 기록할 수 있습니다.

## <a name="configure-trend-micro-web-security-sso"></a>Trend Micro Web Security SSO 구성

**TMWS(Trend Micro Web Security)** 쪽에서 Single Sign-On을 구성하려면 Azure Portal에서 다운로드한 **인증서(Base64)** 와 적절히 복사한 URL을 [TMWS(Trend Micro Web Security) 지원 팀](https://success.trendmicro.com/contact-support-north-america)으로 보내야 합니다. 이렇게 설정하면 SAML SSO 연결이 양쪽에서 제대로 설정됩니다.

## <a name="test-sso"></a>SSO 테스트 

성공적으로 Azure AD 서비스가 구성되고 Azure AD가 사용자 인증 방법으로 지정되었으면 TMWS 프록시 서버에 로그온하여 설정을 확인할 수 있습니다. Azure AD 로그온에서 계정이 확인되면 인터넷을 방문할 수 있습니다.

> [!NOTE]
> TMWS는 Azure AD 포털의 개요 > Single Sign-On > SAML로 Single Sign-On 설정 > 새 엔터프라이즈 애플리케이션 테스트 아래에서 Single Sign-On 테스트를 지원하지 않습니다.

1. 브라우저에서 모든 쿠키를 지운 다음, 브라우저를 다시 시작합니다. 

1. 브라우저에서 TMWS 프록시 서버를 가리킵니다. 자세한 내용은 [PAC 파일을 사용한 트래픽 전달](https://docs.trendmicro.com/en-us/enterprise/trend-micro-web-security-online-help/administration_001/pac-files/traffic-forwarding-u.aspx#GUID-A4A83827-7A29-4596-B866-01ACCEDCC36B)을 참조하세요.

1. 인터넷 웹 사이트를 방문합니다. TMWS에서 TMWS 종속 포털로 안내합니다.

1. Active Directory 계정(형식: domain\sAMAccountName 또는 sAMAccountName@domain), 이메일 주소 또는 UPN을 지정한 다음, **로그온**을 클릭합니다. TMWS에서 사용자를 Azure AD 로그온으로 보냅니다.

1. Azure AD 로그온에서 AD 계정 자격 증명을 입력합니다. TMWS에 성공적으로 로그온됩니다.

## <a name="additional-resources"></a>추가 리소스

- [Azure Active Directory와 SaaS 앱을 통합하는 방법에 대한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory의 조건부 액세스란?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD에서 TMWS(Trend Micro Web Security) 사용해 보기](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security의 세션 제어란?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [고급 표시 유형 및 제어를 사용하여 TMWS(Trend Micro Web Security)를 보호하는 방법](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

