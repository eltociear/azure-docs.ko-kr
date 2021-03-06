---
title: Azure 청구서 보기 및 다운로드
description: Azure 청구서를 보고 다운로드하는 방법을 설명합니다.
keywords: 청구서, 청구서 다운로드, azure 청구서, azure 사용 현황
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 03/27/2020
ms.author: banders
ms.openlocfilehash: 16534343a831f0802a60a9214f567742153360e0
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80479001"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Microsoft Azure 청구서 보기 및 다운로드

[Azure Portal](https://portal.azure.com/)에서 청구서를 다운로드하거나 이메일로 전송할 수 있습니다. 기업계약이 체결된 Azure 고객(EA 고객)의 경우 조직의 청구서를 다운로드할 수 없습니다. 대신, 등록에 대한 청구서를 받도록 설정된 사람에게 청구서가 전송됩니다.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="when-invoices-are-generated"></a>청구서가 생성되는 경우

청구서는 청구 계정 유형에 따라 생성됩니다. MOSP(Microsoft 온라인 서비스 프로그램), MCA(Microsoft 고객 계약) 및 MPA(Microsoft 파트너 계약) 청구 계정에 대한 청구서가 생성됩니다. 또한 EA(기업계약) 청구 계정에 대한 청구서도 생성됩니다. 그러나 EA 청구 계정에 대한 청구서는 Azure Portal에 표시되지 않습니다.

청구 계정에 대해 자세히 알아보고 청구 계정 유형을 확인하려면 [Azure Portal에서 청구 계정 보기](../manage/view-all-accounts.md)를 참조하세요.

## <a name="invoices-for-mosp-billing-accounts"></a>MOSP 청구 계정의 청구서

MOSP 청구 계정은 Azure 웹 사이트를 통해 Azure에 가입할 때 생성됩니다. 예를 들어 [Azure 체험 계정](https://azure.microsoft.com/offers/ms-azr-0044p/)에 가입하는 경우 [종량제 요금을 사용하는 계정](https://azure.microsoft.com/offers/ms-azr-0003p/) 또는 [Visual studio 구독자](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) 자격입니다.

Azure 웹 사이트를 통해 [종량제 요금을 사용한 계정](https://azure.microsoft.com/offers/ms-azr-0003p/)이나 [Azure 체험 계정](https://azure.microsoft.com/offers/ms-azr-0044p/)에 가입한 특정 지역의 고객은 MCA에 대한 청구 계정도 있을 수 있습니다.

청구 계정 유형을 잘 모르는 경우 이 문서의 지침을 수행하기 전에 [청구 계정 유형 확인](../manage/view-all-accounts.md#check-the-type-of-your-account)을 참조하세요. 

MOSP 청구 계정에는 다음과 같은 청구서가 있을 수 있습니다.

**Azure 서비스 요금** - 구독에서 사용하는 Azure 리소스를 포함하는 각 Azure 구독에 대한 청구서가 생성됩니다. 청구서에는 청구 기간에 해당하는 요금이 포함됩니다. 청구 기간은 구독이 생성된 날짜로 결정됩니다.

예를 들어 John은 3월 5일에 *Azure sub 01*을 만들고 3월 10일에 *Azure sub 02*를 만들었습니다. *Azure sub 01* 청구서에는 매월 5일부터 익월 4일까지의 요금이 청구됩니다. *Azure sub 02* 청구서에는 매월 10일부터 익월 9일까지의 요금이 청구됩니다. 모든 Azure 구독에 대한 청구서는 일반적으로 계정이 만들어진 날에 생성되지만 최대 2일이 더 소요될 수 있습니다. 이 예에서 John이 2월 2일에 계정을 만들었다면 *Azure sub 01* 및 *Azure sub 02*에 대한 청구서는 일반적으로 매월 2일에 생성되지만 최대 2일이 더 소요될 수 있습니다.

**Azure Marketplace, 예약 및 스폿 VM** - 구독을 사용하여 구매한 예약, 마켓플레이스 제품 및 스폿 VM에 대한 청구서가 생성됩니다. 청구서에는 이전 달의 관련 요금이 표시됩니다. 예를 들어 John이 3월 1일에 예약을 구매하고 3월 30일에 다른 예약을 구매했습니다. 4월에는 두 예약에 대한 단일 청구서가 생성됩니다. Azure Marketplace, 예약 및 스폿 VM에 대한 청구서는 항상 9일쯤에 생성됩니다.

사용자가 신용 카드로 Azure에 대한 요금을 지불하고 예약을 구매하는 경우 Azure는 즉시 청구서를 생성합니다. 하지만 청구서로 요금이 청구되는 경우 다음 달 청구서에 예약 대금이 청구됩니다.

**Azure 지원 계획** - 매달 지원 계획 구독에 대한 청구서가 생성됩니다. 첫 번째 청구서는 구매일 또는 구매일로부터 최대 2일 후에 생성됩니다. 후속 지원 계획 청구서는 일반적으로 계정이 만들어진 날에 생성되지만 최대 2일 후에 생성될 수 있습니다.

## <a name="download-your-mosp-azure-subscription-invoice"></a>MOSP Azure 구독 청구서 다운로드

청구서는 MOSP 청구 계정에 속한 구독에 대해서만 생성됩니다. [MOSP 계정에 대한 액세스 권한을 확인하세요](../manage/view-all-accounts.md#check-the-type-of-your-account). 

청구서를 다운로드하려면 구독에 대한 계정 관리자 역할이 있어야 합니다. 소유자, 기여자 또는 읽기 권한자 역할을 가진 사용자는 계정 관리자로부터 권한을 부여받은 경우 해당 청구서를 다운로드할 수 있습니다. 자세한 내용은 [사용자가 청구서를 다운로드하도록 허용](../manage/manage-billing-access.md#opt-in)을 참조하세요.

1. [Azure Portal의 구독 페이지](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)에서 구독을 선택합니다.
1. 청구 섹션에서 **청구서**를 선택합니다.  
    ![구독에 대한 청구서 옵션을 선택하는 사용자를 보여주는 스크린샷](./media/download-azure-invoice/select-subscription-invoice.png)
1. **다운로드**를 선택하여 PDF 버전의 청구서를 다운로드한 다음, 청구서 섹션에서 **다운로드**를 선택합니다.  
    [![청구 기간, 다운로드 옵션 및 각 청구 기간별 총 요금을 보여주는 스크린샷](./media/download-azure-invoice/download-invoice-subscription.png)](./media/download-azure-invoice/download-invoice-subscription-zoomed.png#lightbox)
1. 또한 사용량 세부 정보 섹션에서 **다운로드**를 선택하여 일일 사용량 분석 및 요금을 다운로드할 수 있습니다. CSV 파일을 준비하는 데 몇 분 정도 걸릴 수 있습니다.  
    ![청구서 및 사용량 다운로드 옵션을 보여주는 스크린샷](./media/download-azure-invoice/usage-and-invoice-subscription.png)

청구서에 대한 자세한 내용은 [Microsoft Azure 청구서 이해](../understand/review-individual-bill.md)를 참조하세요. 비용 관리에 대한 도움말은 [Azure 청구 및 비용 관리를 사용하여 예상치 못한 비용 방지](../manage/getting-started.md)를 참조하세요.

## <a name="download-your-mosp-support-plan-invoice"></a>MOSP 지원 계획 청구서 다운로드

청구서는 MOSP 청구 계정에 속한 지원 계획 구독에 대해서만 생성됩니다. [MOSP 계정에 대한 액세스 권한을 확인하세요](../manage/view-all-accounts.md#check-the-type-of-your-account). 

청구서를 다운로드하려면 지원 계획 구독에 대한 계정 관리자 역할이 있어야 합니다.

1.  [Azure Portal](https://portal.azure.com)에 로그인합니다.
1.  **Cost Management + 청구**를 검색합니다.  
    ![비용 관리 + 청구에 대한 포털 내 검색을 보여주는 스크린샷](./media/download-azure-invoice/search-cmb.png)
1.  왼쪽에서 **청구서**를 선택합니다.
1.  지원 계획 구독을 선택한 후 **다운로드**를 선택합니다.  
    [![청구 프로필 목록을 보여주는 스크린샷](./media/download-azure-invoice/cmb-invoices.png)](./media/download-azure-invoice/cmb-invoices-zoomed-in.png#lightbox)
1.  **다운로드**를 선택하여 PDF 버전의 청구서를 다운로드합니다.  
    ![청구 기간, 다운로드 옵션 및 각 청구 기간별 총 요금을 보여주는 스크린샷](./media/download-azure-invoice/download-invoice-support-plan.png)

## <a name="allow-others-to-download-an-mosp-subscription-invoice"></a>다른 사용자가 MOSP 구독 청구서를 다운로드할 수 있도록 허용

PDF 버전의 청구서에는 계정 관리자에 대한 개인 정보가 포함되므로 이 구독의 계정 관리자는 다른 사용자에게 청구서를 다운로드할 수 있는 권한을 부여해야 합니다. 권한이 부여되면 다음 사용자와 적절한 역할이 있는 사용자가 구독 청구서를 다운로드할 수 있습니다.

- 사용자
- 그룹
- 소유자 역할이 부여된 서비스 사용자
- 참가자
- 판독기
- 사용자 액세스 관리자
- 청구 읽기 권한자
- 공동 관리자
- 서비스 관리자

청구서를 다운로드하려면 다음을 수행합니다.

1.  구독의 계정 관리자 권한으로 [Azure Portal](https://portal.azure.com)에 로그인합니다.
1.  **Cost Management + 청구**를 검색합니다.  
1.  왼쪽에서 **청구서**를 선택합니다.
1.  Azure 구독을 선택한 다음, **청구서에 액세스**를 선택합니다.  
    [![청구서에 액세스 선택을 보여주는 스크린샷](./media/download-azure-invoice/cmb-select-access-to-invoice.png)](./media/download-azure-invoice/cmb-select-access-to-invoice-zoomed-in.png#lightbox)
1.  페이지 위쪽에서 **켜기**, **저장**을 차례로 선택합니다.  
    ![청구서에 액세스 켜기를 선택하는 모습을 보여주는 스크린샷](./media/download-azure-invoice/cmb-access-to-invoice.png)

## <a name="get-mosp-subscription-invoice-in-email"></a>MOSP 구독 청구서를 이메일로 받기

이메일로 청구서를 받도록 옵트인하려면 구독 또는 지원 계획에 대한 계정 관리자 역할이 있어야 합니다. 이메일 청구서는 구독 및 지원 계획에만 지원되며 예약 또는 Azure Marketplace 구매에는 지원되지 않습니다. 옵트인한 후에는 이메일로 청구서를 받을 또 다른 수신자를 추가할 수 있습니다.

1.  [Azure Portal](https://portal.azure.com)에 로그인합니다.
1.  **Cost Management + 청구**를 검색합니다.  
1.  왼쪽에서 **청구서**를 선택합니다.
1.  Azure 구독 또는 지원 계획 구독을 선택한 다음, **이메일 청구서**를 선택합니다.  
    [![청구 프로필 목록을 보여주는 스크린샷](./media/download-azure-invoice/cmb-email-invoice.png)](./media/download-azure-invoice/cmb-email-invoice-zoomed-in.png#lightbox)
1. **옵트인**을 선택하고 조건에 동의합니다.  
    ![옵트인 흐름 2단계를 보여주는 스크린샷](./media/download-azure-invoice/invoice-article-step02.png)
1. 청구서는 기본 통신 이메일 주소로 전송됩니다. [청구 계정의 연락처 정보](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade/Properties)에서 이메일 주소를 업데이트할 수 있습니다.  
    ![옵트인 흐름 3단계를 보여주는 스크린샷](./media/download-azure-invoice/invoice-article-step03-verify-email.png)

## <a name="share-your-mosp-invoices-with-others-by-email"></a>이메일을 통해 다른 사용자와 MOSP 청구서 공유

구독 및 지원 계획에 대한 청구서를 매달 회계 팀과 공유하거나 다른 이메일 주소 중 하나로 보낼 수 있습니다.

1. [구독 및 지원 계획의 청구서를 이메일로 받기](#get-mosp-subscription-invoice-in-email)의 단계를 수행하고 **받는 사람 구성**을 선택합니다.  
    ![받는 사람 구성을 선택하는 사용자를 보여주는 스크린샷](./media/download-azure-invoice/invoice-article-step03.png)
1. 이메일 주소를 입력한 다음, **받는 사람 추가**를 선택합니다. 이메일 주소를 여러 개 추가할 수 있습니다.  
    ![수신자를 추가하는 사용자를 보여주는 스크린샷](./media/download-azure-invoice/invoice-article-step04.png)
1. 이메일 주소를 모두 추가한 후 화면 아래쪽에서 **완료**를 선택합니다.

## <a name="invoices-for-mca-and-mpa-billing-accounts"></a>MCA 및 MPA 청구 계정의 청구서

귀하의 조직이 Microsoft 담당자와 협력하여 MCA에 서명하는 경우 MCA 청구 계정이 생성됩니다. Azure 웹 사이트를 통해 [종량제 요금을 사용한 계정](https://azure.microsoft.com/offers/ms-azr-0003p/)이나 [Azure 체험 계정](https://azure.microsoft.com/offers/ms-azr-0044p/)에 가입한 특정 지역의 일부 고객은 MCA에 대한 청구 계정도 있을 수 있습니다. 자세한 내용은 [MCA 청구 계정 시작](../understand/mca-overview.md)을 참조하세요.

MPA 청구 계정은 CSP(클라우드 솔루션 공급자) 파트너가 자체 고객을 새로운 상거래 환경에서 관리할 수 있도록 생성되었습니다. Azure Portal에서 청구 계정을 관리하려면 파트너에게 [Azure 플랜](https://docs.microsoft.com/partner-center/purchase-azure-plan)을 사용하는 고객이 하나 이상 있어야 합니다. 자세한 내용은 [MPA 청구 계정 시작](../understand/mpa-overview.md)을 참조하세요.

월초에 사용자 계정에서 각 청구 프로필의 월별 청구서가 생성됩니다. 청구서에는 이전 달의 모든 Azure 구독 및 기타 구매건의 해당 요금이 포함됩니다. 예를 들어 John은 3월 5일에 *Azure sub 01*을 만들고 3월 10일에 *Azure sub 02*를 만들었습니다. 3월 28일에 *Billing profile 01*을 사용하여 *Azure support 01* 구독을 구매했습니다. John은 4월 초부터 Azure 구독과 지원 계획에 대한 요금이 포함된 단일 청구서를 받게 됩니다.

##  <a name="download-an-mca-or-mpa-billing-profile-invoice"></a>MCA 또는 MPA 청구 프로필 청구서 다운로드

Azure Portal에서 청구서를 다운로드하려면 청구 프로필 소유자, 기여자, 읽기 권한자 또는 송장 관리자여야 합니다. 청구 계정에 대한 소유자, 기여자 또는 읽기 권한자 역할이 있는 사용자는 계정의 모든 청구 프로필에 대한 청구서를 다운로드할 수 있습니다.

1.  [Azure Portal](https://portal.azure.com)에 로그인합니다.
1.  **Cost Management + 청구**를 검색합니다.  
1. 왼쪽에서 **청구서**를 선택합니다.
    [![MCA 청구 계정의 청구서 페이지를 보여주는 스크린샷](./media/download-azure-invoice/mca-billing-profile-invoices.png)](./media/download-azure-invoice/mca-billing-profile-invoices-zoomed.png#lightbox)
1. 청구서 표에서 다운로드하려는 청구서를 선택합니다.
1. 페이지 위쪽에서 **청구서 PDF 다운로드**를 선택합니다.  
    ![청구서 PDF 다운로드를 보여주는 스크린샷](./media/download-azure-invoice/mca-billing-profile-download-invoice.png)
1. 또한 **Azure 사용량 다운로드**를 선택하여 일일 사용량 분석 및 예상 요금이 포함된 CSV 파일을 다운로드할 수 있습니다. CSV 파일을 준비하는 데 몇 분 정도 걸릴 수 있습니다.

## <a name="get-your-billing-profiles-invoice-in-email"></a>청구 프로필의 청구서를 이메일로 받기

이메일 청구서 기본 설정을 업데이트하려면 청구 프로필 또는 청구 계정에 대한 소유자 또는 기여자 역할이 있어야 합니다. 옵트인하면 청구 프로필에 대한 소유자, 기여자, 읽기 권한자 및 송장 관리자 역할이 있는 모든 사용자가 이메일로 청구서를 받게 됩니다. 

1.  [Azure Portal](https://portal.azure.com)에 로그인합니다.
1.  **Cost Management + 청구**를 검색합니다.  
1.  왼쪽에서 **청구서**를 선택한 다음, 페이지 위쪽에서 **이메일 청구서**를 선택합니다.  
    [![MCA 청구 계정의 청구서 페이지를 보여주는 스크린샷](./media/download-azure-invoice/mca-billing-profile-select-email-invoice.png)](./media/download-azure-invoice/mca-billing-profile-select-email-invoice-zoomed.png#lightbox)
1.  여러 청구 프로필이 있는 경우 청구 프로필을 선택한 다음, **옵트인**을 선택합니다.  
    ![MCA 청구 계정의 청구서 페이지를 보여주는 스크린샷](./media/download-azure-invoice/mca-billing-profile-email-invoice.png)
1.  **업데이트**를 선택합니다.

## <a name="give-others-access-to-mca-or-mpa-invoices"></a>다른 사용자에게 MCA 또는 MPA 청구서에 대한 액세스 권한 부여

MCA 또는 MPA 청구 프로필에 대한 송장 관리자 역할을 할당하여 다른 사용자에게 청구서를 보고, 다운로드하고, 요금을 지불할 수 있는 액세스 권한을 부여할 수 있습니다. 청구서를 이메일로 받기 위해 옵트인한 경우 사용자도 이메일로 청구서를 받게 됩니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
1. **Cost Management + 청구**를 검색합니다.  
1. 왼쪽에서 **청구 프로필**을 선택합니다. 청구 프로필 목록에서 송장 관리자 역할을 할당하려는 청구 프로필을 선택합니다.  
   ![청구 프로필 목록을 보여주는 스크린샷](./media/download-azure-invoice/mca-select-profile-zoomed-in.png)
1. 왼쪽에서 **Access Control(IAM)** 을 선택한 다음, 페이지 맨 위에서 **추가**를 선택합니다.  
    ![액세스 제어 페이지를 보여주는 스크린샷](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png)
1. 역할 드롭다운 목록에서 **송장 관리자**를 선택합니다. 액세스 권한을 부여할 사용자의 이메일 주소를 입력합니다. **저장**을 선택하여 역할을 할당합니다.  
   ![사용자를 송장 관리자로 추가하는 것을 보여주는 스크린샷](./media/download-azure-invoice/mca-added-invoice-manager.png)

## <a name="why-you-might-not-see-an-invoice"></a><a name="noinvoice"></a> 청구서가 보이지 않는 이유

몇 가지 이유로 인해 청구서가 보이지 않을 수 있습니다.

- Azure를 구독한 지 30일 이내입니다. 
- Azure는 청구 기간이 지나고 며칠 후에 요금을 청구합니다. 따라서 청구서가 아직 생성되지 않았을 수 있습니다.
- 청구서를 볼 수 있는 권한이 없습니다.
    MCA 또는 MPA 청구 계정이 있는 경우 청구서를 보려면 청구 프로필에 대한 소유자, 기여자, 읽기 권한자 또는 송장 관리자 역할이 있거나 청구 계정에 대한 소유자, 기여자 또는 읽기 권한자 역할이 있어야 합니다. 다른 구독의 경우 계정 관리자가 아니면 청구서가 보이지 않을 수 있습니다. 청구 정보에 액세스하는 방법에 대한 자세한 내용은 [역할을 사용하여 Azure 청구에 대한 액세스 관리](../manage/manage-billing-access.md)를 참조하세요.
- MOSP 청구 계정이 있고 월별 크레딧 금액이 포함된 Azure 체험 계정 또는 구독에 가입한 경우 월별 크레딧 금액을 초과하는 경우에만 청구서를 받게 됩니다. MCA 또는 MPA에 대한 청구 계정이 있는 경우 항상 청구서를 받게 됩니다.

## <a name="need-help-contact-us"></a>도움 필요 시 문의하세요.

질문이 있거나 도움이 필요한 경우 [지원 요청을 만드세요](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>다음 단계

청구서 및 요금에 대한 자세한 내용은 다음을 참조하세요.

- [Microsoft Azure 사용량 및 요금 보기 및 다운로드](download-azure-daily-usage.md)
- [Microsoft Azure 요금 청구 방식 이해](review-individual-bill.md)
- [Azure 청구서의 용어 이해](understand-invoice.md)

MCA가 있는 경우 다음을 참조하세요.

- [청구 프로필 청구서의 요금 이해](review-customer-agreement-bill.md)
- [청구 프로필 청구서의 용어 이해](mca-understand-your-invoice.md)
- [청구 프로필에 대한 Azure 사용량 및 요금 파일 이해](mca-understand-your-usage.md)


