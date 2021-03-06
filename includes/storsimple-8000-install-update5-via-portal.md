---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: d2df7388018c463ba58b57be46945915210b84b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67182401"
---
#### <a name="to-install-an-update-from-the-azure-portal"></a>Azure 포털에서 업데이트를 설치하려면

1. StorSimple 서비스 페이지에서 디바이스를 선택합니다.

    ![디바이스 선택](./media/storsimple-8000-install-update5-via-portal/update1.png)

2. 장치 **설정으로** > 이동**장치 업데이트.**

    ![디바이스 업데이트 클릭](./media/storsimple-8000-install-update5-via-portal/update2.png)

2. 새 업데이트를 사용할 수 있는 경우 알림이 나타납니다. 또는 **디바이스 업데이트** 블레이드에서 **업데이트 검색**을 클릭합니다. 사용 가능한 업데이트를 검색하는 작업이 만들어집니다. 작업이 성공적으로 완료되면 알림이 표시됩니다.

    ![디바이스 업데이트 클릭](./media/storsimple-8000-install-update5-via-portal/update3.png)

3. 디바이스에 업데이트를 적용하기 전에 릴리스 정보를 검토하는 것이 좋습니다. 업데이트를 적용하려면 **업데이트 설치**를 클릭합니다. **Confirm regular updates(정기 업데이트 확인)** 블레이드에서 업데이트를 적용하기 전에 완료할 필수 구성 요소를 검토합니다. 디바이스를 업데이트할 준비가 되었음을 나타내는 확인란을 선택한 후 **설치**를 클릭합니다.

    ![디바이스 업데이트 클릭](./media/storsimple-8000-install-update5-via-portal/update4.png)

6. 일련의 필수 조건 검사가 시작됩니다. 이들 검사는 다음과 같습니다.
   
   * **컨트롤러 상태 검사** 디바이스 컨트롤러가 정상이고 온라인에 있는지 모두 확인합니다.
   * **하드웨어 구성 요소 상태 검사** StorSimple 디바이스에서 모든 하드웨어 구성 요소가 정상인지 확인합니다.
   * **데이터 0 검사** 데이터 0이 디바이스에서 사용할 수 있는지 확인합니다. 이 인터페이스를 사용하지 않는 경우 다음을 사용하도록 설정하고 다시 시도해야 합니다.

     모든 검사가 성공적으로 완료된 경우에만 업데이트가 다운로드되어 설치됩니다. 검사가 진행 중인 경우 알림이 표시됩니다. 사전 검사가 실패하면 실패한 이유가 표시됩니다. 이러한 문제를 해결한 다음 작업을 다시 시도합니다. 혼자서 이러한 문제를 해결할 수 없는 경우 Microsoft 지원에 문의해야 할 수 있습니다.

7. 사전 검사를 성공적으로 완료한 후 업데이트 작업이 생성됩니다. 업데이트 작업이 성공적으로 만들어지면 알림이 표시됩니다.
   
    ![업데이트 작업 만들기](./media/storsimple-8000-install-update5-via-portal/update6.png)
   
    그런 다음 업데이트가 디바이스에 적용됩니다.

9. 업데이트를 완료하는 데 몇 시간이 걸립니다. 업데이트 작업을 선택하고 **세부 정보** 를 클릭하여 언제든지 작업의 세부 정보를 봅니다.

    ![업데이트 작업 만들기](./media/storsimple-8000-install-update5-via-portal/update8.png)

     **디바이스 설정 &gt; 작업**에서 업데이트 작업의 진행률을 모니터링할 수도 있습니다. **작업** 블레이드에서 업데이트 진행률을 볼 수 있습니다.

     ![업데이트 작업 만들기](./media/storsimple-8000-install-update5-via-portal/update7.png)

10. 작업이 완료되면 **디바이스 설정 &gt; 디바이스 업데이트**로 이동합니다. 이제 소프트웨어 버전이 업데이트됩니다.

