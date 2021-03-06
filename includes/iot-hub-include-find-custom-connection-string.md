---
title: 포함 파일
description: 포함 파일
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 11/02/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 6f43bbcd83861f7d39de2aa89bbe035c2ff5b809
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70050468"
---
<!-- This tells how to create a custom shared access policy for your IoT hub and get the connection string for it-->

**서비스 연결** 및 **레지스트리 읽기** 권한을 부여하고 이 정책에 대한 연결 문자열을 얻는 공유 액세스 정책을 만들려면 다음 단계를 따르십시오.

1. Azure [포털에서](https://portal.azure.com) **리소스 그룹을**선택합니다. 허브가 있는 리소스 그룹을 선택한 다음 리소스 목록에서 허브를 선택합니다.

1. 허브의 왼쪽 창에서 공유 액세스 **정책을**선택합니다.

1. 정책 목록 위의 위쪽 메뉴에서 **추가를**선택합니다.

1. **공유 액세스 정책 추가에서** *serviceAndRegistryRead*와 같은 정책에 대한 설명 이름을 입력합니다. **사용 권한에서** **레지스트리 읽기** 및 서비스 **연결을**선택한 다음 **을 선택합니다.**

    ![새 공유 액세스 정책을 추가하는 방법 표시](./media/iot-hub-include-find-custom-connection-string/iot-hub-add-custom-policy.png)

1. 정책 목록에서 새 정책을 선택합니다.

1. **공유 액세스 키에서**연결 문자열의 복사 아이콘을 선택하고 **기본 키를** 선택하고 값을 저장합니다.

    ![연결 문자열을 검색하는 방법 표시](./media/iot-hub-include-find-custom-connection-string/iot-hub-get-connection-string.png)

IoT Hub 공유 액세스 정책 및 사용 권한에 대한 자세한 내용은 [액세스 제어 및 사용 권한을](../articles/iot-hub/iot-hub-devguide-security.md#access-control-and-permissions)참조하십시오.
