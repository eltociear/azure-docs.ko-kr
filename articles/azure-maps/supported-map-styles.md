---
title: 지원되는 맵 스타일 | 마이크로소프트 Azure 지도
description: 이 문서에서는 Microsoft Azure 지도에서 지원하는 다양한 맵 렌더링 스타일에 대해 알아봅니다.
author: philmea
ms.author: philmea
ms.date: 05/06/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 2eafe3c16a89723d55ec52fde785e9ec69e45e0c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334032"
---
# <a name="azure-maps-supported-map-styles"></a>Azure Maps에서 지원되는 지도 스타일
Azure Maps는 아래 설명된 대로 여러 가지 기본 제공 지도 스타일을 지원합니다.

## <a name="road"></a>도로
**도로** 지도는 도로, 자연적 특징 및 인공적 특징을 해당 특징에 대한 레이블과 함께 표시하는 표준 지도입니다.

![도로 지도 스타일](./media/supported-map-styles/road.png)

**적용 가능한 API:**
* [지도 이미지](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [맵 타일](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* 웹 SDK 맵 제어
* 안드로이드지도 제어

## <a name="blank-and-blank_accessible"></a>공백 및 blank_accessible

**빈** 및 **blank_accessible** 맵 스타일은 데이터를 시각화할 빈 캔버스를 제공합니다. **blank_accessible** 스타일은 기본 맵이 표시되지 않더라도 화면 판독기 업데이트에 맵의 위치 세부 정보를 계속 제공합니다.

> [!Note]
> 웹 SDK에서 맵 DIV 요소의 CSS `background-color` 스타일을 설정하여 맵의 배경색을 변경할 수 있습니다.

**적용 가능한 API:**
* 웹 SDK 맵 제어

## <a name="satellite"></a>위성 
**위성** 스타일은 위성 및 항공 이미지의 조합입니다.

![위성 타일 지도 스타일](./media/supported-map-styles/satellite.png)

**적용 가능한 API:**
* [위성 타일](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* 웹 SDK 맵 제어
* 안드로이드지도 제어

## <a name="satellite_road_labels"></a>satellite_road_labels
이 지도 스타일은 위성 및 항공 이미지 위에 겹쳐진 도로 및 레이블의 하이브리드입니다.

![satellite_road_labels 맵 스타일](./media/supported-map-styles/satellite-road-labels.png)

**적용 가능한 API:**
* 웹 SDK 맵 제어
* 안드로이드지도 제어

## <a name="grayscale_dark"></a>grayscale_dark
**그레이스케일 다크는** 도로 맵 스타일의 어두운 버전입니다.

![gray_scale 맵 스타일](./media/supported-map-styles/grayscale-dark.png)

**적용 가능한 API:**
* [지도 이미지](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [맵 타일](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* 웹 SDK 맵 제어 
* 안드로이드지도 제어


## <a name="grayscale_light"></a>grayscale_light
**그레이스케일 라이트는** 도로 맵 스타일의 라이트 버전입니다.

![그레이 스케일 라이트 맵 스타일](./media/supported-map-styles/grayscale-light.png)

**적용 가능한 API:**
* 웹 SDK 맵 제어
* 안드로이드지도 제어


## <a name="night"></a>야간
**야간**은 색이 지정된 도로 및 기호가 포함된 어두운 버전의 도로 지도 스타일입니다.

![야간 지도 스타일](./media/supported-map-styles/night.png)

**적용 가능한 API:**
* 웹 SDK 맵 제어
* 안드로이드지도 제어

## <a name="road_shaded_relief"></a>road_shaded_relief
**도로 음영 입체**는 지구의 등고선으로 채워진 Azure Maps 주요 스타일입니다.

![그늘진 릴리프 맵 스타일](./media/supported-map-styles/shaded-relief.png)

**적용 가능한 API:**
* [맵 타일](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* 웹 SDK 맵 제어
* 안드로이드지도 제어

## <a name="high_contrast_dark"></a>high_contrast_dark

**high_contrast_dark** 다른 스타일보다 대비가 높은 어두운 맵 스타일입니다.

![고대비 다크 맵 스타일](./media/supported-map-styles/high-contrast-dark.png)

**적용 가능한 API:**
* 웹 SDK 맵 제어

## <a name="next-steps"></a>다음 단계

Azure 지도에서 맵 스타일을 설정하는 방법에 대해 알아봅니다.

> [!div class="nextstepaction"]
> [맵 스타일 선택](https://docs.microsoft.com/azure/azure-maps/choose-map-style)
