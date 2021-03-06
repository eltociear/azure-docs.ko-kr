---
title: 파트너 센터 - Azure 마켓플레이스를 사용하여 Azure IoT Edge 모듈 오퍼 생성
description: 파트너 센터를 사용하여 Azure 마켓플레이스에서 IoT Edge 모듈 을 만드는 방법에 대해 알아봅니다.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: cca54e4e456fe766b190f64657cd1aca1d9520e0
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869139"
---
# <a name="create-an-iot-edge-module-offer"></a>IoT Edge 모듈 제품 만들기

> [!IMPORTANT]
> 클라우드 파트너 포털에서 파트너 센터로 IoT Edge 모듈 의 관리를 이전하고 있습니다. 오퍼가 마이그레이션될 때까지 [IoT Edge 모듈의](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts) 지침에 따라 클라우드 파트너 포털에 대한 게시 개요를 참조하여 오퍼를 관리하십시오.

이 문서에서는 Azure Marketplace용 IoT(사물 인터넷) Edge 모듈 오퍼를 만들고 게시하는 방법에 대해 설명합니다.

IoT Edge 모듈 오퍼를 만들려면 먼저 파트너 센터에 상용 마켓플레이스 계정이 있어야 합니다. 아직 만들지 않은 경우 파트너 [센터에서 상업용 마켓플레이스 계정 만들기를](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account)참조하세요.

## <a name="create-a-new-offer"></a>새 제안 만들기

1. 파트너 센터에 로그인합니다.
2. 왼쪽 탐색 메뉴에서 **상업용 마켓플레이스** > **개요를**선택합니다.

    ![왼쪽 탐색 메뉴를 보여 줍니다.](./media/cs-menu-overview.png)

3. 선택 **+ 새로운 제공** > **IoT 에지 모듈.** **새 오퍼** 대화 상자가 나타납니다.

> [!IMPORTANT]
> 오퍼가 게시된 후 파트너 센터에서 편집한 편집은 제안을 다시 게시한 후에만 상점에 표시됩니다. 변경한 후 항상 다시 게시해야 합니다.

### <a name="offer-id-and-alias"></a>제공 ID 및 별칭

**오퍼 ID를**입력합니다. 이 식별자는 계정의 각 오퍼에 대한 고유 식별자입니다.

- 이 ID는 마켓플레이스 오퍼의 웹 주소및 Azure Resource Manager 템플릿(해당하는 경우)의 고객에게 표시됩니다.
- 소문자와 숫자만 사용할 수 있습니다. 하이픈과 밑줄이 포함될 수 있지만 공백은 포함되지 않으며 50자로 제한됩니다. 예를 들어 **테스트-offer-1을**입력하면 제안 웹 `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`주소가 됩니다.
- 만들기를 선택한 후에는 쿠폰 ID를 변경할 수 없습니다.

**오퍼 별칭을**입력합니다. 파트너 센터에서 쿠폰을 참조하는 데 사용되는 이름입니다.

- 이 이름은 마켓플레이스에서 사용되지 않으며 고객에게 표시되는 오퍼 이름 및 기타 값과 다릅니다.
- 는 **을**선택한 후에는 변경할 수 없습니다.

이 두 값을 입력한 후 다음 페이지로 계속하기 전에 **만들기를** 선택합니다.

## <a name="offer-overview"></a>제품 개요

**제안 개요** 페이지에는 이 제안을 게시하는 데 필요한 단계(완료및 예정)와 각 단계를 완료하는 데 걸리는 시간이 시각적으로 표시됩니다.

이 페이지에는 선택한 항목을 기반으로 이 오퍼에서 작업을 수행하는 링크가 포함되어 있습니다. 예를 들어:

- 제안이 초안인 경우 - [초안 오퍼 삭제](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- 오퍼가 실시간 인 경우 - [오퍼 판매 중지](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- 제안이 미리 보기인 경우 - [Go-live](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- 게시자 로그아웃을 완료하지 않은 경우 게시를 [취소합니다.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="offer-setup"></a>오퍼 설정

다음 단계에 따라 쿠폰을 설정합니다.

### <a name="connect-lead-management"></a>잠재 고객 관리 연결

파트너 센터를 통해 오퍼를 마켓플레이스에 게시할 때 선택적으로 CRM(고객 관계 관리) 시스템에 연결할 수 있습니다. 이렇게 하면 다른 사람이 귀사에 관심을 표명하거나 제품을 사용하는 즉시 고객 연락처 정보를 받을 수 있습니다.

1. 잠재 고객을 보내려는 잠재 고객 대상을 선택합니다. 파트너 센터는 다음 CRM 시스템을 지원합니다.

    - 고객 참여를 위한 [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM 시스템이 위에 나열되지 않은 경우 [Azure Table](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) 또는 [Https Endpoint를](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) 사용하여 고객 잠재 고객 데이터를 저장한 다음 CRM 시스템으로 데이터를 내보냅니다.

2. 파트너 센터에 게시할 때 잠재 고객 대상에 쿠폰을 연결합니다.
3. 잠재 고객 대상에 대한 연결이 제대로 구성되어 있는지 확인합니다. 파트너 센터에 게시한 후 연결의 유효성을 검사하고 테스트 리드를 보냅니다. 오퍼가 시작되기 전에 쿠폰을 미리 볼 때 미리 보기 환경에서 직접 쿠폰을 구입하여 잠재 고객 연결을 테스트할 수도 있습니다.
4. 잠재 고객을 잃지 않도록 잠재 고객 대상에 대한 연결이 업데이트된 상태로 유지되는지 확인합니다.

다음은 몇 가지 추가 잠재 고객 관리 리소스입니다.

- [잠재 고객 관리 개요](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [잠재 고객 관리 FAQ](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [일반적인 잠재 고객 구성 오류](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [잠재 고객 관리 개요](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF(팝업 차단이 꺼져 있는지 확인).

다음 섹션인 속성으로 계속하기 전에 **초안 저장을** 선택합니다.

### <a name="properties"></a>속성

이 페이지에서는 마켓플레이스에서 쿠폰을 그룹화하는 데 사용되는 카테고리와 쿠폰을 지원하는 법적 계약을 정의할 수 있습니다.

#### <a name="category"></a>범주

최소 1개 및 최대 5개 범주를 선택합니다. 이러한 카테고리는 적절한 마켓플레이스 검색 영역에 쿠폰을 배치하는 데 사용되며 쿠폰 세부정보 페이지에 표시됩니다. 쿠폰 설명에서 쿠폰이 이러한 범주를 지원하는 방법을 설명합니다. 찾아보기 페이지에서 모든 IoT Edge 모듈은 **사물 인터넷 > IoT Edge 모듈** 범주에 표시됩니다.

#### <a name="legal"></a>법적 정보

오퍼에 대한 이용 약관을 제공해야 합니다. 다음 두 가지 옵션을 사용할 수 있습니다.

- Microsoft 상용 마켓플레이스의 표준 계약을 사용합니다.
- 사용자 고유의 이용 약관을 제공합니다.

##### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>마이크로소프트 상업 시장에 대 한 표준 계약

우리는 상업 시장에서 거래를 용이하게하는 데 도움이 표준 계약 템플릿을 제공합니다. 고객이 한 번만 확인하고 수락하면 되는 표준 계약에 따라 솔루션을 제공하도록 선택할 수 있습니다. 사용자 지정 이용 약관을 만들고 싶지 않은 경우 이 옵션을 사용할 수 있습니다.

표준 계약에 대한 자세한 내용은 [Microsoft 상용 마켓플레이스의 표준 계약을](https://docs.microsoft.com/azure/marketplace/standard-contract)참조하십시오. [표준 계약](https://go.microsoft.com/fwlink/?linkid=2041178) PDF를 다운로드할 수도 있습니다(팝업 차단이 꺼져 있는지 확인).

표준 계약을 사용하려면 **Microsoft의 상용 마켓플레이스에 대한 표준 계약 사용** 확인란을 선택한 다음 **수락을**클릭합니다.

> [!NOTE]
> Microsoft 상용 마켓플레이스의 표준 계약을 사용하여 오퍼를 게시한 후에는 사용자 지정 이용 약관을 사용할 수 없습니다. 표준 계약에 따라 또는 자체 이용 약관에 따라 솔루션을 제공합니다.

![Microsoft의 상용 마켓플레이스 확인란에 대한 표준 계약을 사용하는 경우를 보여 줍니다.](./media/iot-edge-module-creation/iot-edge-module-standard-contract-checkbox.png)

##### <a name="your-own-terms-and-conditions"></a>사용자 고유의 이용 약관

사용자 고유의 사용자 지정 이용 약관을 제공하려면 **이용 약관** 상자에 입력하십시오. 이 상자에 텍스트 문자를 무제한으로 입력할 수 있습니다. 고객은 제품을 사용해 본 전에 이 약관에 동의해야 합니다.

다음 섹션인 제안 목록으로 이동하기 전에 **초안 저장을** 선택합니다.

## <a name="offer-listing"></a>오퍼 리스팅

여기에서 마켓플레이스에 표시되는 쿠폰 세부 정보를 정의합니다. 여기에는 오퍼 이름, 설명, 이미지 등이 포함됩니다. 이 제안을 구성하는 동안 Microsoft의 정책 페이지에 자세히 설명된 정책을 따라야 합니다.

> [!NOTE]
> 제안 설명이 "이 응용 프로그램은 [영어 이외의 언어]에서만 사용할 수 있습니다"라는 문구로 시작하는 경우 제안 세부 정보가 영어로 필요하지 않습니다. 또한 쿠폰 목록 세부 정보에 사용된 것과 다른 언어로 콘텐츠를 제공하는 유용한 링크를 제공하는 것도 괜찮습니다.

### <a name="name"></a>이름

여기에 입력한 이름은 쿠폰 의 제목으로 표시됩니다. 이 필드는 **오퍼를** 만들 때 쿠폰 별칭 상자에 입력한 텍스트로 미리 채워져 있습니다. 이 이름은 나중에 변경할 수 있습니다.

이름:

- 상표권(상표 또는 저작권 기호포함).
- 50자 를 초과할 수 없습니다.
- 이모티콘을 포함할 수 없습니다.

### <a name="search-results-summary"></a>검색 결과 요약

오퍼에 대한 간략한 설명을 제공합니다. 최대 100자까지 사용할 수 있으며 마켓플레이스 검색 결과에 사용됩니다.

### <a name="long-summary"></a>긴 요약

오퍼에 대한 자세한 설명을 제공하세요. 최대 256자까지 사용할 수 있으며 마켓플레이스 검색 결과에 사용됩니다.

### <a name="description"></a>설명

최대 3,000자까지 오퍼에 대한 더 긴 설명을 제공합니다. 이는 마켓플레이스 목록 개요의 고객에게 표시됩니다.

설명에 다음 중 하나 이상을 포함하십시오.

- 오디퍼가 제공하는 가치와 주요 이점
- 범주 또는 산업 협회 또는 둘 다
- 인앱 구매 기회
- 필요한 모든 공개

IoT Edge 모듈은 설명 하단에 최소 하드웨어 요구 사항 단락을 포함해야 합니다. 예를 들어:

*최소 하드웨어 요구 사항: Linux x64 및 arm32 OS, 1GB RAM, 500Mb 스토리지*

다음은 설명을 작성하기 위한 몇 가지 팁입니다.

- 설명의 처음 몇 문장에서 오퍼의 가치를 명확하게 설명합니다. 다음 항목을 포함합니다.
    - 제품에 대한 설명입니다.
    - 오퍼의 혜택을 누릴 수 있는 사용자 유형입니다.
    - 고객의 요구 또는 제안 주소 문제.
- 처음 몇 문장은 검색 결과에 표시될 수 있습니다.
- 제품을 판매하는 기능과 기능에 의존하지 마십시오. 대신 오퍼가 제공하는 가치에 초점을 맞춥니다.
- 산업별 어휘 또는 혜택 기반 표현을 사용해 보십시오.

오퍼 **설명을** 보다 매력적으로 만들려면 리치 텍스트 편집기를 사용하여 설명의 서식을 지정합니다. 리치 텍스트 편집기를 사용하면 숫자, 글머리 기호, 굵은 글꼴, 기울임꼴 및 들여쓰기를 추가하여 설명을 더 읽기 쉽게 만들 수 있습니다.

:::image type="content" source="media/text-editor2.png" alt-text="리치 텍스트 편집기입니다." border="false":::

- 콘텐츠의 형식을 변경하려면 이 스크린샷과 같이 텍스트 스타일을 서식 지정하고 선택할 텍스트를 강조 표시합니다.

     :::image type="content" source="media/text-editor3.png" alt-text="는 가재 텍스트 편집기에서 텍스트 스타일 컨트롤을 보여 줍니다." border="false":::

- 글머리 기호 또는 번호가 매겨진 목록을 텍스트에 추가하려면 이 스크린샷에 표시된 옵션을 사용합니다.
  
    :::image type="content" source="media/text-editor4.png" alt-text="전체 텍스트 편집기에서 글머리 기호 및 숫자 목록 컨트롤을 보여 줍니다." border="false":::

- 텍스트에 들여쓰기를 추가하거나 제거하려면 이 스크린샷에 표시된 옵션을 사용합니다.

    :::image type="content" source="media/text-editor5.png" alt-text="는 하위 텍스트 편집기에서 들여쓰기 컨트롤을 보여 줍니다." border="false":::

#### <a name="privacy-policy-url"></a>개인정보 처리방침 URL

조직의 개인 정보 보호 정책의 웹 주소를 입력합니다. 귀하는 귀하의 제안이 개인 정보 보호 법률 및 규정을 준수하는지 확인할 책임이 있습니다. 또한 웹사이트에 유효한 개인정보 보호정책을 게시할 책임이 있습니다.

#### <a name="useful-links"></a>유용한 링크

오퍼에 대한 추가 온라인 문서를 제공합니다. 최대 25개의 링크를 추가할 수 있습니다. 링크를 추가하려면 **+ 링크 추가를** 선택한 다음 다음 필드를 완료합니다.

- **제목** - 고객은 쿠폰 세부정보 페이지에 제목을 볼 수 있습니다.
- **링크(URL)** - 고객이 온라인 문서를 볼 수 있도록 링크를 입력합니다. 링크는 http:// 또는 https:// 시작해야 합니다.

 [Azure IoT 장치 카탈로그에서](https://catalog.azureiotsolutions.com/)호환되는 IoT Edge 장치에 하나 이상의 링크와 하나의 링크를 설명서에 추가해야 합니다.

### <a name="contact-information"></a>연락처 정보

**지원 담당자** 및 엔지니어링 연락처의 이름, 이메일 및 전화 번호를 제공해야 **합니다.** 이 정보는 고객에게 표시되지 않습니다. Microsoft에서 사용할 수 있으며 CSP(클라우드 솔루션 공급자) 파트너에게 제공될 수 있습니다.

- 지원 담당자(필수): 일반 지원 질문의 경우.
- 엔지니어링 담당자(필수): 기술 적 질문 및 인증 문제의 경우.
- CSP 프로그램 연락처(선택 사항): CSP 프로그램과 관련된 리셀러 질문의 경우.

지원 **연락처** 섹션에서 파트너가 글로벌 Azure, Azure 정부 또는 둘 다에서 서비스를 사용할 수 있는지 여부에 따라 오퍼에 대한 지원을 찾을 수 있는 **지원 웹 사이트의** 웹 주소를 제공합니다.

**CSP 프로그램 연락처** 섹션에서 CSP 파트너가 오퍼에 대한 마케팅 자료를 찾을 수 있는 링크(CSP**프로그램 마케팅 자료)를**제공합니다.

#### <a name="additional-marketplace-listing-resources"></a>추가 마켓플레이스 리스팅 리소스

오퍼 목록을 만드는 방법에 대해 자세히 알아보려면 [제안 목록 모범 사례를](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)참조하세요.

### <a name="marketplace-images"></a>마켓플레이스 이미지

오퍼와 함께 사용할 로고와 이미지를 제공합니다. 모든 이미지는 .png 형식이어야 합니다. 흐릿한 이미지는 거부됩니다.

>[!Note]
>파일을 업로드하는 데 문제가 있는 경우 로컬 네트워크가 파트너 https://upload.xboxlive.com 센터에서 사용하는 서비스를 차단하지 않는지 확인합니다.

#### <a name="store-logos"></a>스토어 로고

다음 네 픽셀 크기 각각에 쿠폰 로고의 .png 파일을 제공합니다.

- **소형(48 x 48)**
- **미디엄(90 x 90)**
- **라지 (216 x 216)**
- **와이드(255 x 115)**

네 개의 로고가 모두 필요하며 마켓플레이스 목록의 다른 위치에서 사용됩니다.

#### <a name="screenshots-optional"></a>스크린샷(선택 사항)

쿠폰의 작동 방식을 보여주는 스크린샷을 최대 5개까지 추가합니다. 각 픽셀의 크기와 .png 형식은 1280 x 720 픽셀이어야 합니다.

#### <a name="videos-optional"></a>동영상(선택 사항)

쿠폰을 보여주는 동영상을 최대 5개까지 추가할 수 있습니다. 동영상 의 이름, 웹 주소 및 1280 x 720 픽셀 크기로 비디오의 축소판 .png 이미지를 입력합니다.

#### <a name="offer-examples"></a>제안 예제

다음 예제에서는 오퍼 목록 필드가 오퍼의 여러 위치에 표시되는 방식을 보여 주며

이 스크린샷은 Azure 마켓플레이스의 **제안 목록** 페이지를 보여 주며 있습니다.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-offer-listing-page.png" alt-text="Azure 마켓플레이스의 제안 목록 페이지를 보여 줍니다.":::

이 스크린샷은 Azure 마켓플레이스의 검색 결과를 보여 주며 다음과 같은 결과를 보여 주며 다음과 같은 결과를 보여 주며 다음과 같은 결과를 보여 주며 다음과

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-search-results.png" alt-text="Azure 마켓플레이스의 검색 결과를 보여 줍니다.":::

이 스크린샷은 Azure 포털의 **제안 목록** 페이지를 보여 주며 있습니다.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-listing-page-azure-portal.png" alt-text="Azure 포털에서 제안 목록 페이지를 보여 줍니다.":::

이 스크린샷은 Azure 포털의 검색 결과를 보여 주며 있습니다.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-search-results.png" alt-text="Azure 포털에서 제안 목록 페이지를 보여 줍니다.":::

다음 섹션인 미리 보기로 진행하기 전에 **초안 저장을** 선택합니다.

## <a name="preview"></a>미리 보기

미리보기 **탭에서**더 넓은 마켓플레이스 오디언스에게 라이브로 게시하기 전에 쿠폰의 유효성을 검증하기 위해 제한된 **미리보기 잠재고객을** 선택할 수 있습니다.

> [!IMPORTANT]
> 미리 보기에서 쿠폰을 본 후 **라이브 로이동을** 선택하여 공개하도록 해야 합니다.

Azure 구독 ID GUID를 사용하여 미리 보기 대상을 지정하고 각 멤버쉽에 대한 선택적 설명을 지정합니다. 이러한 필드 중 어느 것도 고객이 볼 수 없습니다.

> [!NOTE]
> Azure 포털의 구독 페이지에서 Azure 구독 ID를 찾을 수 있습니다.

개별적으로(최대 10개) 또는 CSV 파일(최대 100개)을 업로드하여 하나 이상의 Azure 구독 ID를 추가합니다. 이러한 구독 아이디를 추가하면 쿠폰이 라이브로 게시되기 전에 미리 볼 수 있는 사람을 정의할 수 있습니다. 쿠폰이 이미 라이브 인 경우 미리 보기 대상을 정의하여 오퍼의 변경 또는 업데이트를 테스트할 수 있습니다.

> [!NOTE]
> 미리 보기 대상은 비공개 대상과 다릅니다. **미리 보기** 대상은 마켓플레이스에 게재되기 전에 모든 오퍼 플랜을 보고 확인할 수 있으며, 여기에는 **비공개** 잠재고객에게만 게시되는 플랜(가용성 탭에 설정됨)이 포함됩니다.

다음 섹션, 계획 개요로 진행하기 전에 **초안 저장을** 선택합니다.

### <a name="plan-overview"></a>계획 개요

이 탭을 사용하면 파트너 센터에서 동일한 오퍼 내에서 다양한 요금제 옵션을 제공할 수 있습니다. 이러한 계획은 이전에 SCO 또는 재고 유지 단위로 불렸습니다. 계획은 전역 클라우드, 정부 클라우드 및 계획에서 참조하는 이미지와 같이 사용할 수 있는 클라우드에 따라 다를 수 있습니다. 마켓플레이스에 쿠폰을 나열하려면 하나 이상의 요금제 설정을 해야 합니다.

계획을 만든 후 **계획 개요** 탭에 다음이 표시됩니다.

- 계획 이름
- 가격 책정 모델
- 클라우드 가용성(글로벌 또는 정부)
- 현재 게시 상태
- 사용 가능한 모든 작업

계획 개요에서 사용할 수 있는 작업은 계획의 현재 상태에 따라 다릅니다. 해당 기능은 아래와 같습니다.

- **초안 삭제**: 계획 상태가 초안인 경우.
- **판매 중지 계획**: 계획 상태가 실시간으로 게시된 경우.

#### <a name="create-new-plan"></a>새 계획 만들기

**새 계획 만들기를 선택합니다.** **새 계획** 대화 상자가 나타납니다.

계획 **ID** 상자에서 이 제안의 각 계획에 대해 고유한 계획 ID를 만듭니다. 이 ID는 제품 웹 주소의 고객에게 표시됩니다. 소문자와 숫자, 대시 또는 밑줄만 사용하고 최대 50자만 사용합니다.

계획 **이름** 상자에 이 계획의 이름을 입력합니다. 고객은 오퍼 내에서 선택할 플랜을 결정할 때 이 이름을 볼 수 있습니다. 이 오퍼의 각 계획에 대해 고유한 이름을 만듭니다. 예를 들어 Windows Server **2016** 및 Windows Server **2019**계획과 함께 **Windows** Server의 오퍼 이름을 사용할 수 있습니다.

> [!NOTE]
> 계획 ID **만들기를**선택한 후에는 변경할 수 없습니다.

**만들기**를 선택합니다.

### <a name="plan-setup"></a>계획 설정

이 탭을 사용하면 계획이 사용할 수 있는 클라우드를 구성할 수 있습니다. 이 탭의 답변은 다른 탭에 표시되는 필드에 영향을 줍니다.

#### <a name="cloud-availability"></a>클라우드 가용성

Azure IoT Hub를 사용하여 계획을 하나 이상의 클라우드에서 사용할 수 있어야 합니다.

마켓플레이스를 사용하는 모든 글로벌 Azure 지역의 고객이 계획을 사용할 수 있도록 **Azure Global** 옵션을 선택합니다. 자세한 내용은 [지리적 가용성 및 통화 지원을](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies)참조하십시오.

Azure [정부 클라우드](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) 옵션을 선택하여 솔루션을 여기에 표시합니다. 이 클라우드는 미국 연방, 주, 지방 또는 부족 정부 기관뿐만 아니라 서비스를 제공할 자격이 있는 파트너의 고객에 대한 제어된 액세스 권한이 있는 정부 커뮤니티 클라우드입니다. 퍼블리셔는 이 클라우드 커뮤니티에 대한 규정 준수 제어, 보안 조치 및 모범 사례를 담당합니다. Azure 정부는 물리적으로 격리된 데이터 센터 및 네트워크를 사용합니다(미국에만 있음). Azure 정부에 [게시하기](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners) 전에 결과가 다를 수 있으므로 해당 영역 내에서 솔루션을 테스트하고 확인합니다. 솔루션을 단계및 테스트하려면 Microsoft Azure [정부 평가판에서](https://azure.microsoft.com/global-infrastructure/government/request/)평가판 계정을 요청하십시오.

> [!NOTE]
> 계획이 게시되고 특정 클라우드에서 사용할 수 있게 되면 해당 클라우드를 제거할 수 없습니다.

#### <a name="azure-government-cloud-certifications"></a>Azure 정부 클라우드 인증

이 옵션은 Azure **정부 클라우드가** **클라우드 가용성**에서 선택된 경우에만 표시됩니다.

Azure 정부 서비스는 특정 정부 규정 및 요구 사항이 적용되는 데이터를 처리합니다. 예를 들어, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 및 CJIS. 이러한 프로그램에 대한 인증을 인식하려면 인증을 설명하는 링크를 최대 100개까지 제공할 수 있습니다. 이들은 직접 또는 자신의 웹 사이트에 프로그램에 목록에 대한 링크가 될 수 있습니다. 이러한 링크는 Azure 정부 고객에게만 표시됩니다.

## <a name="plan-listing"></a>계획 목록

이 탭에는 동일한 오퍼 내에서 각 요금제에 대한 특정 정보가 표시됩니다.

### <a name="plan-name"></a>계획 이름

이 이름은 계획을 만들 때 제공한 이름으로 미리 채워져 있습니다. 필요에 따라 이 이름을 변경할 수 있습니다. 최대 50자까지 가능합니다. 이 이름은 Azure Marketplace 및 Azure 포털에서 이 계획의 제목으로 나타납니다. 계획을 사용할 준비가 된 후 기본 모듈 이름으로 사용됩니다.

### <a name="plan-summary"></a>계획 요약

제안이 아닌 플랜에 대한 간략한 요약을 제공합니다. 이 요약은 Azure Marketplace 검색 결과에 표시되며 최대 100자까지 포함될 수 있습니다.

### <a name="plan-description"></a>계획 설명

이 계획이 어떻게 독특하게 만드는지, 그리고 오퍼 내의 계획 간의 차이점에 대해 설명합니다. 제안을 설명하지 말고 계획만 설명하십시오. 이 설명은 Azure 마켓플레이스 및 제안 목록 페이지의 Azure 포털에 표시됩니다. 계획 요약에서 제공한 내용과 동일한 콘텐츠일 수 있으며 최대 2,000자까지 포함할 수 있습니다.

이 필드를 완료한 후 **초안 저장을** 선택합니다.

#### <a name="plan-examples"></a>계획 예제

다음 예제에서는 계획 목록 필드가 다른 보기에 표시되는 방식을 보여 주습니다.

다음은 계획 세부 정보를 볼 때 Azure 마켓플레이스의 필드입니다.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-plan-details.png" alt-text="Azure Marketplace에서 계획 세부 정보를 볼 때 표시되는 필드를 보여 줍니다.":::

다음은 Azure 포털에 대한 계획 세부 정보입니다.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-plan-details.png" alt-text="Azure 포털에 대한 계획 세부 정보를 보여 줍니다.":::

## <a name="availability"></a>가용성

고객이 마켓플레이스에서 검색, 탐색 또는 구매할 수 없도록 게시된 쿠폰을 숨기려면 가용성 탭에서 **요금제 숨기기** 확인란을 선택합니다.

이 필드는 다음과 같은 경우에 일반적으로 사용됩니다.

- 이 오퍼는 다른 응용 프로그램을 참조할 때만 간접적으로 사용됩니다.
- 이 제안은 개별적으로 구매해서는 안됩니다.
- 이 계획은 초기 테스트에 사용되었으며 더 이상 관련이 없습니다.
- 이 플랜은 임시 또는 계절 적 제안에 사용되었으며 더 이상 제공되지 않아야합니다.

## <a name="technical-configuration"></a>기술 구성

**IoT Edge 모듈** 제공 유형은 IoT Edge 장치에서 실행되는 특정 유형의 컨테이너입니다. 기술 **구성** 탭에서 [Azure 컨테이너 레지스트리](https://azure.microsoft.com/services/container-registry/)내의 컨테이너 이미지 리포지토리에 대한 참조 정보와 고객이 모듈을 쉽게 사용할 수 있도록 하는 구성 설정을 제공합니다.

오퍼가 게시되면 IoT Edge 컨테이너 이미지가 특정 공용 컨테이너 레지스트리의 Azure Marketplace에 복사됩니다. 모듈을 사용하는 Azure 사용자의 모든 요청은 개인 컨테이너 레지스트리가 아닌 Azure Marketplace 공용 컨테이너 레지스트리에서 제공됩니다.

여러 플랫폼을 대상으로 지정하고 태그를 사용하여 여러 버전의 모듈 컨테이너 이미지를 제공할 수 있습니다. 태그 및 버전 관리에 대한 자세한 내용은 [IoT Edge 모듈 기술 자산 준비를](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-iot-edge-module-asset)참조하십시오.

### <a name="image-repository-details"></a>이미지 리포지토리 세부 정보

**이미지 리포지토리 세부 정보** 탭에서 다음 정보를 제공합니다.

**이미지 원본 선택:** **Azure 컨테이너 레지스트리** 옵션을 선택합니다.

**Azure 구독 ID**: 리소스 사용량이 보고되고 컨테이너 이미지가 포함된 Azure 컨테이너 레지스트리에 대한 서비스가 청구되는 구독 ID를 제공합니다. Azure 포털의 구독 페이지에서 이 ID를 찾을 수 [있습니다.](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)

**Azure 리소스 그룹 이름:** 컨테이너 이미지와 함께 Azure 컨테이너 레지스트리를 포함 하는 [리소스 그룹](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal) 이름을 제공 합니다. 리소스 그룹은 구독 ID(위)에서 액세스할 수 있어야 합니다. Azure 포털에서 리소스 [그룹](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) 페이지에서 이름을 찾을 수 있습니다.

**Azure 컨테이너 레지스트리 이름**: 컨테이너 이미지가 있는 [Azure 컨테이너 레지스트리의](https://docs.microsoft.com/azure/container-registry/container-registry-intro) 이름을 제공합니다. 컨테이너 레지스트리는 이전에 제공한 Azure 리소스 그룹에 있어야 합니다. 전체 로그인 서버 이름이 아닌 레지스트리 이름만 제공합니다. 이름에서 **azurecr.io** 생략해야 합니다. Azure 포털의 컨테이너 레지스트리 [페이지에서](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerRegistry%2Fregistries) 레지스트리 이름을 찾을 수 있습니다.

**Azure 컨테이너 레지스트리의 관리자 사용자 이름**: 컨테이너 이미지가 있는 Azure 컨테이너 레지스트리와 연결된 [관리자 사용자 이름을](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account) 제공합니다. 회사에서 레지스트리에 액세스할 수 있도록 하려면 사용자 이름과 암호가 필요합니다. 관리자 사용자 이름과 암호를 얻으려면 CLI(Azure 명령줄 인터페이스)를 사용하여 **관리자 지원** 속성을 **True로** 설정합니다. 선택적으로 관리 **사용자를** Azure 포털에서 **사용** 하도록 설정할 수 있습니다.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-admin-user.png" alt-text="컨테이너 레지스트리 업데이트 대화 상자를 보여 줍니다.":::

**Azure 컨테이너 레지스트리에 대한 암호**: Azure 컨테이너 레지스트리와 연결되고 컨테이너 이미지가 있는 관리자 사용자 이름에 대한 암호를 제공합니다. 회사에서 레지스트리에 액세스할 수 있도록 하려면 사용자 이름과 암호가 필요합니다. **컨테이너 레지스트리** > **액세스 키로** 이동하거나 표시 명령을 사용하여 Azure CLI를 사용하여 Azure 포털에서 암호를 얻을 수 [있습니다.](https://docs.microsoft.com/cli/azure/acr/credential?view=azure-cli-latest#az-acr-credential-show)

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-username-password.png" alt-text="Azure 포털의 액세스 키 화면을 보여 줍니다.":::

**Azure 컨테이너 레지스트리 내의 리포지토리 이름**입니다. 이미지가 있는 Azure 컨테이너 레지스트리 리포지토리의 이름을 제공합니다. 레지스트리에 이미지를 푸시할 때 리포지토리의 이름을 지정합니다. [저장소의](https://azure.microsoft.com/services/container-registry/) > 이름을 찾을 수 있습니다.컨테이너 레지스트리**리포지토리 페이지로**이동 하여 자세한 내용은 [Azure 포털의 컨테이너 레지스트리 리포지토리 보기를](https://docs.microsoft.com/azure/container-registry/container-registry-repositories)참조하십시오. 이름을 설정한 후에는 변경할 수 없습니다. 계정의 각 오퍼에 대해 고유한 이름을 사용합니다.

### <a name="image-tags-for-new-versions-of-your-offer"></a>새로운 버전의 오퍼에 대한 이미지 태그

고객은 업데이트를 게시할 때 Azure Marketplace에서 업데이트를 자동으로 받을 수 있어야 합니다. 업데이트하지 않으려면 특정 버전의 이미지를 유지할 수 있어야 합니다. 이미지를 업데이트할 때마다 새 이미지 태그를 추가하여 이 작업을 수행할 수 있습니다.

**이미지 태그**. 이 **필드에는** 지원되는 모든 플랫폼에서 이미지의 최신 버전을 가리키는 최신 태그가 포함되어야 합니다. 또한 버전 태그를 포함해야 합니다(예: xx가 숫자인 xx.xx.xx로 시작). 고객은 [매니페스트 태그를](https://github.com/estesp/manifest-tool) 사용하여 여러 플랫폼을 타겟팅해야 합니다. 매니페스트 태그에서 참조하는 모든 태그는 업로드할 수 있도록 추가되어야 합니다. 모든 매니페스트 태그(최신 태그 제외)는 X,Y, Z 가 정수인 X.Y- 또는 X.Y.Z-로 시작해야 합니다. 예를 들어 최신 태그가 1.0.1-linux-x64, 1.0.1-linux-arm32 및 1.0.1-windows-arm32를 가리키는 경우 이 6개의 태그를 이 필드에 추가해야 합니다. 태그 및 버전 관리에 대한 자세한 내용은 [IoT Edge 모듈 기술 자산 준비를 참조하십시오.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets)

### <a name="default-deployment-settings-optional"></a>기본 배포 설정(선택 사항)

IoT Edge 모듈을 배포하는 가장 일반적인 설정을 정의합니다. 이러한 기본 설정을 통해 IoT Edge 모듈을 즉시 실행하도록 하여 고객 배포를 최적화합니다.

**기본 경로**. IoT Edge Hub는 모듈, IoT 허브 및 장치 간의 통신을 관리합니다. 모듈과 IoT Hub 간에 데이터 입력 및 출력에 대한 경로를 설정할 수 있으므로 메시지를 처리하거나 추가 코드를 작성하기 위해 추가 서비스가 필요 없이 필요한 곳에 메시지를 보낼 수 있는 유연성을 제공합니다. 배관은 이름/값 쌍을 사용하여 생성됩니다. 각각 최대 512자까지 기본 경로 이름을 정의할 수 있습니다.

경로 값에 올바른 [경로 구문을](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) 사용해야 합니다(일반적으로 FROM/message/* INTO $upstream 정의). 즉, 모든 모듈에서 보낸 모든 메시지는 IoT Hub로 이동합니다. 모듈을 참조하려면 공백이나 특수 문자 없이 **오퍼 이름이**되는 기본 모듈 이름을 사용합니다. 아직 알려지지 않은 다른 모듈을 참조하려면 <FROM_MODULE_NAME> 규칙을 사용하여 고객에게 이 정보를 업데이트해야 한다는 것을 알수 있습니다. IoT Edge 경로에 대한 자세한 내용은 [경로 선언을](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes)참조하십시오.

예를 들어, 모듈 ContosoModule ContosoInput에서 입력을 수신 하는 경우 ContosoOutput에서 데이터를 출력, 다음 두 기본 경로 정의 하는 것이 합리적:

- 이름 #1: 토콘토소모듈
- 가치 #1: FROM /메시지/모듈/<FROM_MODULE_NAME>/출력/* INTO BrokeredEndpoint("/모듈/ContosoModule/입력/ContosoInput")
- 이름 #2: FromContosoModuleToCloud
- 값 #2: FROM /메시지/모듈/ContonsoModule/출력/contosoOutput into $upstream

**기본 모듈 트윈 원하는 속성**. 모듈 쌍은 원하는 속성을 포함하여 모듈 인스턴스에 대한 상태 정보를 저장하는 IoT Hub의 JSON 문서입니다. 원하는 속성은 보고된 속성과 함께 모듈 구성 또는 조건을 동기화하는 데 사용됩니다. 솔루션 백 엔드는 원하는 속성을 설정할 수 있으며 모듈은 이를 읽을 수 있습니다. 모듈은 원하는 속성에서 변경 알림을 받을 수도 있습니다. 원하는 속성은 최대 5개의 이름/값 쌍을 사용하여 만들어지며 각 기본값은 512자 미만이어야 합니다. 최대 5개의 이름/값 쌍둥이 원하는 속성을 정의할 수 있습니다. 두 개의 원하는 속성의 값은 4개의 레벨의 최대 중첩 계층 구조가 있는 배열없이 유효한 JSON이어야 합니다. 기본값에 필요한 매개 변수가 적합하지 않은 시나리오(예: 고객 서버의 IP 주소)에서는 매개 변수를 기본값으로 추가할 수 있습니다. 쌍 원하는 속성에 대한 자세한 내용은 [원하는 속성 정의 또는 업데이트](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties)를 참조하십시오.

예를 들어 모듈이 원하는 쌍 속성을 사용하여 동적으로 구성 가능한 새로 고침 빈도를 지원하는 경우 다음과 같은 기본 트윈 원하는 속성을 정의하는 것이 좋습니다.

- 이름 #1: 새로 고침
- 값 #1: 60

**기본 환경 변수**. 환경 변수는 구성 프로세스를 지원하는 모듈에 추가 정보를 제공합니다. 환경 변수는 이름/값 쌍을 사용하여 만들어집니다. 각 기본 환경 변수 이름과 값은 512자 미만이어야 하며 최대 5자까지 정의할 수 있습니다. 기본값에 필요한 매개 변수가 적합하지 않은 경우(예: 고객 서버의 IP 주소) 매개 변수를 기본값으로 추가할 수 있습니다.

예를 들어 모듈을 시작하기 전에 사용 약관에 동의해야 하는 경우 다음 환경 변수를 정의할 수 있습니다.

- 이름 #1: ACCEPT_EULA
- 값 #1: Y

**기본 컨테이너는 옵션을 만듭니다.** 컨테이너 생성 옵션은 IoT Edge 모듈 Docker 컨테이너를 직접 생성합니다. IoT Edge는 Docker 엔진 API 생성 컨테이너 옵션을 지원합니다. [목록 컨테이너의](https://docs.docker.com/engine/api/v1.30/#operation/ContainerList) 모든 옵션을 참조하십시오. 만들기 옵션 필드는 유효한 JSON, 이스케이프되지 않은 자 및 512자 미만이어야 합니다.

예를 들어 모듈에 포트 바인딩이 필요한 경우 다음 만들기 옵션을 정의합니다.

"호스트 콩피그":{"포트 바인딩":{"5012/tcp":[{"호스트 포트":"5012"}}}

## <a name="review-and-publish"></a>검토 및 게시

오퍼의 모든 필수 섹션을 완료한 후 제출하여 검토하고 게시할 수 있습니다.

포털의 오른쪽 상단 모서리에서 **검토 및 게시를**선택합니다.

검토 페이지에서 게시 상태를 볼 수 있습니다.

- 오퍼의 각 섹션에 대한 완료 상태를 참조하십시오. 오퍼의 모든 섹션이 완료로 표시될 때까지 게시할 수 없습니다.
    - **시작되지 않음** - 섹션이 시작되지 않았으며 완료해야 합니다.
    - **불완전** - 섹션에 수정해야 하거나 추가 정보를 제공해야 하는 오류가 있습니다. 지침은 이 문서의 위쪽 섹션을 참조하십시오.
    - **완료** - 섹션에 필요한 모든 데이터가 있으며 오류가 없습니다. 오퍼를 제출하려면 먼저 오퍼의 모든 섹션이 완료되어야 합니다.
- 제품 테스트가 올바르게 테스트되었는지 확인하기 위해 인증 팀에 테스트 지침을 제공합니다. 또한 제안을 이해하는 데 도움이 되는 추가 메모를 제공하십시오.

게시를 위한 제안을 제출하려면 **게시를**선택합니다.

제안의 미리 보기 버전을 검토하고 승인할 수 있는 시기를 알려주는 이메일이 전송됩니다. 오퍼를 공개(또는 비공개 오퍼의 경우 개인 대상에게 게시하려면 파트너 센터로 이동 **라이브)를**선택합니다.

## <a name="next-steps"></a>다음 단계

- [상용 시장에서 기존 오퍼 업데이트](https://docs.microsoft.com//azure/marketplace/partner-center-portal/update-existing-offer)