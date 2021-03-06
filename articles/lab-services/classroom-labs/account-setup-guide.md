---
title: Azure 랩 서비스에 대한 랩 계정 설정 가이드 가속화
description: 이 가이드는 관리자가 학교 내에서 사용할 랩 계정을 신속하게 설정할 수 있도록 도와줍니다.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/18/2020
ms.author: spelluru
ms.openlocfilehash: 8fcc46487e7f7c2d075639f10a30cae9950ff31b
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879584"
---
# <a name="lab-account-setup-guide"></a>랩 계정 설정 가이드

첫 번째 단계로 관리자는 Azure 구독 내에서 랩 계정을 설정해야 합니다. 랩 계정은 강의실 랩의 컨테이너이며 설정하는 데 몇 분밖에 걸리지 않습니다.

## <a name="understand-your-schools-lab-account-requirements"></a>학교의 랩 계정 요구 사항 이해

학교의 필요에 따라 랩 계정을 구성하는 방법을 이해하려면 다음 질문을 고려해야 합니다.

### <a name="do-i-have-access-to-an-azure-subscription"></a>Azure 구독에 액세스할 수 있습니까?

랩 계정을 만들려면 학교에 대해 구성된 Azure 구독에 액세스해야 합니다. 학교에 하나 이상의 구독이 있을 수 있습니다. 구독을 사용하여 랩 계정을 포함한 모든 Azure 리소스 및 서비스에 대한 청구 및 보안을 관리합니다.

### <a name="how-many-lab-accounts-need-to-be-created"></a>몇 개의 랩 계정을 만들어야 합니까?

빠르게 시작하려면 단일 랩 계정을 만든 다음 나중에 필요에 따라 추가 랩 계정을 만듭니다. 예를 들어 결국 부서당 하나의 랩 계정이 있을 수 있습니다.

### <a name="who-should-be-owners-and-contributors-of-the-lab-account"></a>랩 계정의 소유자및 기여자는 누구여야 합니까?

관리자는 일반적으로 랩 계정의 소유자 및 참여자입니다. 랩 계정에 포함된 모든 랩에 적용되는 정책을 관리할 책임이 있습니다. 랩 계정을 만드는 사람은 자동으로 소유자입니다. 일반적으로 구독과 연결된 Azure Active Directory(Azure AD) 테넌트에서 추가 소유자 및 참가자를 추가할 수 있습니다. 이 기능은 랩 계정 수준에서 소유자 또는 기여자 역할을 할당하여 랩 계정을 관리하는 데 유용할 수 있습니다.

### <a name="who-will-be-allowed-to-create-and-manage-labs"></a>랩을 만들고 관리할 수 있는 사람은 누구입니까?

관리자와 교수진이 랩을 만들고 관리하도록 선택할 수 있습니다. 이러한 사용자(일반적으로 구독과 연결된 Azure AD 테넌트에서) 랩 계정 내의 랩 작성자 역할에 할당됩니다.

### <a name="do-you-want-to-give-lab-creators-the-ability-to-save-images-that-can-be-shared-across-labs"></a>랩 작성자에게 랩 간에 공유할 수 있는 이미지를 저장할 수 있는 기능을 제공하시겠습니까?

공유 이미지 갤러리는 이미지를 저장하고 공유하는 데 사용할 수 있는 리포지토리입니다. 동일한 이미지가 필요한 여러 클래스가 있는 경우 랩 작성자는 이미지를 한 번 만들어 랩 간에 공유할 수 있습니다. 그러나 시작하려면 나중에 언제든지 추가할 수 있으므로 공유 이미지 갤러리가 반드시 필요하지는 않습니다.

이 질문에 "예"라고 대답한 경우 랩 계정에 공유 이미지 갤러리를 만들거나 첨부해야 합니다. "잘 모르겠다"고 대답하면 이 결정을 나중에 연기할 수 있습니다.

### <a name="which-images-in-azure-marketplace-will-your-classroom-labs-use"></a>Azure 마켓플레이스에서 강의실 랩에서 어떤 이미지를 사용할 것인가?

Azure Marketplace는 랩 작성자가 랩을 만드는 데 이미지를 사용할 수 있도록 활성화할 수 있는 수백 개의 이미지를 제공합니다. 일부 이미지에는 랩에 이미 필요한 모든 것이 포함될 수 있습니다. 다른 경우에는 이미지를 시작점으로 사용할 수 있으며 랩 작성자는 추가 응용 프로그램이나 도구를 설치하여 이미지를 사용자 지정할 수 있습니다.

사용해야 할 이미지를 모르는 경우 나중에 다시 설정하여 사용할 수 있습니다. 또한 사용할 수 있는 이미지를 확인하는 가장 좋은 방법은 먼저 랩 계정을 만드는 것입니다. 이렇게 하면 액세스 권한이 제공되므로 사용 가능한 이미지 목록과 해당 내용을 검토할 수 있습니다.
  
### <a name="do-the-labs-virtual-machines-need-to-have-access-to-other-azure-or-on-premises-resources"></a>랩의 가상 시스템이 다른 Azure 또는 온-프레미스 리소스에 액세스해야 합니까?

랩 계정을 설정할 때 가상 네트워크를 사용하여 피어피어할 수도 있습니다. 필요한지 여부를 결정하려면 다음 질문을 고려하십시오.

- **라이선싱 서버에 대한 액세스를 제공해야 합니까?**
  
   Azure Marketplace 이미지를 사용하려는 경우 운영 체제 라이선스 비용이 랩 서비스의 가격 책정에 번들로 제공됩니다. 따라서 운영 체제 자체에 대 한 라이센스를 제공할 필요가 없습니다. 그러나 설치된 추가 소프트웨어 및 응용 프로그램의 경우 적절한 라이센스를 제공해야 합니다.

- **랩 VM이 파일 공유 또는 데이터베이스와 같은 다른 온-프레미스 리소스에 액세스해야 합니까?**

   일반적으로 사이트 간 가상 네트워크 게이트웨이를 사용하여 온-프레미스 리소스에 대한 액세스를 제공하는 가상 네트워크를 만듭니다. 가상 네트워크가 구성되지 않은 경우 이를 위해 추가 시간을 투자해야 합니다.

- **랩 VM이 가상 네트워크 내에 있는 다른 Azure 리소스에 액세스해야 합니까?**

   가상 네트워크 내에서 *보호되지 않는* Azure 리소스에 액세스해야 하는 경우 피어링을 수행하지 않고 공용 인터넷을 통해 이러한 리소스에 액세스할 수 있습니다.

하나 이상의 질문에 "예"라고 대답한 경우 랩 계정을 가상 네트워크로 피어피해야 합니다. "잘 모르겠다"고 대답하면 이 결정을 나중에 연기할 수 있습니다. 랩 계정을 만든 후 언제든지 가상 네트워크를 피어로 이동하도록 선택할 수 있습니다.

## <a name="set-up-your-lab-account"></a>랩 계정 설정

랩 계정의 요구 사항을 이해하면 설정할 준비가 된 것입니다.

1. **랩 계정을 만듭니다.** 지침에 대 [한 랩 계정을 만드는](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#create-a-lab-account) 방법에 대 한 자습서를 참조 하십시오.

   랩 계정을 만들 때 관련된 Azure 리소스에 익숙해지는 것이 유용할 수 있습니다. 자세한 내용은 다음 문서를 참조하세요.

   - [구독](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#subscription)
   - [리소스 그룹](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#resource-group)
   - [랩 계정](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#lab-account)
   - [교실 실습실](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#classroom-lab)
   - [지역 및 위치 선택](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations)
   - [리소스에 대한 이름 지정 지침](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#naming)

2. **랩 작성자 역할에 사용자를 추가합니다.** 지침은 랩 [작성자 역할에 사용자 추가를](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role)참조하세요.

   또한 랩 계정 및 랩을 관리할 사용자에게 할당할 수 있는 다양한 역할에 대한 자세한 내용은 [ID 관리 가이드를](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#manage-identity)참조하십시오.

3. **피어 가상 네트워크에 연결합니다.** 지침은 [랩의 네트워크를 피어 가상 네트워크와](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network)연결참조하십시오.

   [랩 VM 주소 범위 구성에](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-lab-accounts#specify-an-address-range-for-vms-in-the-lab)대한 지침을 참조해야 할 수도 있습니다.

4. **이미지를 활성화하고 검토합니다.** 지침은 랩 [작성자용 Azure 마켓플레이스 이미지 사용(설정)을](https://docs.microsoft.com/azure/lab-services/classroom-labs/specify-marketplace-images)참조하십시오.

   각 Azure Marketplace 이미지의 내용을 검토하려면 이미지 이름을 선택합니다. 예를 들어 다음 스크린샷은 우분투 데이터 과학 VM 이미지에 대한 세부 정보를 보여 주며 다음과 같은 내용을 보여 주며 다음과 같은 내용을 보여 주며, 다음 의 스크린샷은 다음과 같은 내용을 보여줍니다.

   ![검토 Azure 마켓플레이스 이미지의 스크린샷](../media/setup-guide/review-marketplace-images.png)

   랩 계정에 공유 이미지 갤러리가 연결되어 있고 랩 작성자가 사용자 지정 이미지를 공유할 수 있도록 하려면 다음 스크린샷과 유사한 단계를 완료합니다.

   ![공유 이미지 갤러리에서 사용자 지정 이미지 사용 의 스크린샷](../media/setup-guide/enable-sig-custom-images.png)

## <a name="next-steps"></a>다음 단계

- [랩 계정 관리](how-to-manage-lab-accounts.md)

- [교실 랩 설정 가이드](setup-guide.md)
