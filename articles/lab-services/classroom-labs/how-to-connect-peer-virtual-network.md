---
title: Azure 랩 서비스의 피어 네트워크에 연결 | 마이크로 소프트 문서
description: 랩 네트워크를 다른 네트워크와 피어로 연결하는 방법을 알아봅니다. 예를 들어 온-프레미스 학교/대학 네트워크를 Azure의 Lab의 가상 네트워크와 연결합니다.
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
ms.date: 03/31/2020
ms.author: spelluru
ms.openlocfilehash: 6ed0d743f9b9cdc136b8f52f4d9d02583fc63eb9
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81870189"
---
# <a name="connect-your-labs-network-with-a-peer-virtual-network-in-azure-lab-services"></a>Azure Lab 서비스의 피어 가상 네트워크와 랩 네트워크 연결

이 문서에서는 랩 네트워크를 다른 네트워크로 피어링하는 것에 대한 정보를 제공합니다.

## <a name="overview"></a>개요

가상 네트워크 피어링을 사용하면 Azure 가상 네트워크를 원활하게 연결할 수 있습니다. 가상 네트워크가 피어링되면 연결성을 위해 하나로 표시됩니다. 피어있는 가상 네트워크의 가상 시스템 간의 트래픽은 개인 IP 주소만을 통해 동일한 가상 네트워크의 가상 시스템 간에 트래픽이 라우팅되는 것과 마찬가지로 Microsoft 백본 인프라를 통해 라우팅됩니다. 자세한 내용은 [가상 네트워크 피어링](../../virtual-network/virtual-network-peering-overview.md)을 참조하세요.

다음 을 포함한 일부 시나리오에서는 랩의 네트워크를 피어 가상 네트워크와 연결해야 할 수 있습니다.

- 랩의 가상 시스템에는 온-프레미스 라이센스 서버에 연결하여 라이센스를 획득하는 소프트웨어가 있습니다.
- 랩의 가상 머신은 대학의 네트워크 공유에 있는 데이터 집합(또는 기타 파일)에 액세스해야 합니다.

특정 온-프레미스 네트워크는 [ExpressRoute](../../expressroute/expressroute-introduction.md) 또는 가상 [네트워크 게이트웨이를](../../vpn-gateway/vpn-gateway-about-vpngateways.md)통해 Azure 가상 네트워크에 연결됩니다. 이러한 서비스는 Azure Lab 서비스 외부에서 설정해야 합니다. ExpressRoute를 사용하여 온-프레미스 네트워크를 Azure에 연결하는 방법에 대한 자세한 내용은 [ExpressRoute 개요를](../../expressroute/expressroute-introduction.md)참조하십시오. 가상 네트워크 게이트웨이를 사용하여 온-프레미스 연결의 경우 게이트웨이, 지정된 가상 네트워크 및 랩 계정이 모두 동일한 지역에 있어야 합니다.

> [!NOTE]
> 랩 계정으로 피어볼 Azure 가상 네트워크를 만들 때는 가상 네트워크의 영역이 교실 랩을 만드는 위치에 미치는 영향을 이해하는 것이 중요합니다.  자세한 내용은 [지역\위치에](https://docs.microsoft.com/azure/lab-services/classroom-labs/administrator-guide#regionslocations)대한 관리자 가이드 섹션을 참조하십시오.

## <a name="configure-at-the-time-of-lab-account-creation"></a>랩 계정 생성 시 구성

새 [랩 계정 생성](tutorial-setup-lab-account.md)중에 **고급** 탭의 피어 가상 네트워크 드롭다운 목록에 표시되는 기존 **가상 네트워크를** 선택할 수 있습니다.  이 목록에는 랩 계정과 동일한 지역에있는 가상 네트워크만 표시됩니다. 선택한 가상 네트워크가 랩 계정으로 생성된 랩에 연결(피어) 됩니다.  이 변경 이후에 만들어진 랩의 모든 가상 컴퓨터는 피어있는 가상 네트워크의 리소스에 액세스할 수 있습니다.

![피어로 VNet 선택](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer.png)

### <a name="address-range"></a>주소 범위

랩에 대한 가상 컴퓨터에 대한 **주소 범위를** 제공하는 옵션도 있습니다.  **주소 범위** 속성은 랩에 대해 피어 **가상 네트워크를** 사용하도록 설정된 경우에만 적용됩니다.  주소 범위가 제공되면 랩 계정 아래의 모든 가상 컴퓨터가 해당 주소 범위에 만들어집니다. 주소 범위는 CIDR 표기형(예: 10.20.0.0/20)이어야 하며 기존 주소 범위와 겹치지 않아야 합니다.  주소 범위를 제공할 때는 생성될 *랩* 수를 생각하고 이를 수용할 수 있는 주소 범위를 제공하는 것이 중요합니다. 랩 서비스는 랩당 최대 512개의 가상 컴퓨터를 가정합니다.  예를 들어 '/23'이 있는 IP 범위는 하나의 랩만 만들 수 있습니다.  '/21'이 있는 범위는 4개의 실험실을 만들 수 있습니다.

주소 **범위를** 지정하지 않으면 랩 서비스는 가상 네트워크로 피어볼 가상 네트워크를 만들 때 Azure에서 제공하는 기본 주소 범위를 사용합니다.  범위는 종종 10.x.0.0/16과 같습니다.  이로 인해 ip 범위가 겹칠 수 있으므로 랩 설정에서 범위를 지정하고 주소지정하거나 피어인 가상 네트워크의 주소 범위를 확인해야 합니다.

## <a name="configure-after-the-lab-is-created"></a>랩을 만든 후 구성

랩 계정 생성 시 피어 네트워크를 설정하지 않은 경우 **랩 계정** 페이지의 **랩 구성** 탭에서 동일한 속성을 사용할 수 있습니다. 이 설정에 대한 변경 사항은 변경 후 생성된 랩에만 적용됩니다. 이미지에서 볼 수 있듯이 랩 계정의 랩에 대해 **피어 가상 네트워크를** 사용하거나 사용하지 않도록 설정할 수 있습니다.

![랩을 만든 후 VNet 피어링 사용 또는 비활성화](../media/how-to-connect-peer-virtual-network/select-vnet-to-peer-existing-lab.png)

**피어 가상** 네트워크 필드에 대한 가상 네트워크를 선택하면 랩 위치 옵션을 선택할 수 있도록 **랩 작성자가** 비활성화됩니다. 이는 랩 계정의 랩이 피어 가상 네트워크의 리소스와 연결하려면 랩 계정과 동일한 지역에 있어야 하기 때문입니다.

> [!IMPORTANT]
> 피어있는 가상 네트워크 설정은 기존 랩이 아니라 변경 이후에 만들어진 랩에만 적용됩니다.

## <a name="next-steps"></a>다음 단계

다음 문서를 참조하세요.

- [랩 작성자가 랩 위치를 선택하도록 허용](allow-lab-creator-pick-lab-location.md)
- [랩에 공유 이미지 갤러리 첨부](how-to-attach-detach-shared-image-gallery.md)
- [사용자를 랩 소유자로 추가](how-to-add-user-lab-owner.md)
- [랩의 방화벽 설정 보기](how-to-configure-firewall-settings.md)
- [랩에 대한 다른 설정 구성](how-to-configure-lab-accounts.md)
