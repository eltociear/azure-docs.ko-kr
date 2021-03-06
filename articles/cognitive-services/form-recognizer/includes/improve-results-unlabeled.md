---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: include
ms.date: 06/12/2019
ms.author: pafarley
ms.openlocfilehash: f0761847c3677b324ef16c5987eb9a1561dbcbe0
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75379297"
---
`"confidence"` 노드 아래의 각 키/값 결과에 대한 `"pageResults"` 값을 검사합니다. 텍스트 읽기 작업에 해당하는 `"readResults"` 노드의 신뢰도 점수도 확인해야 합니다. 읽기 결과의 신뢰도는 키/값 추출 결과의 신뢰도에 영향을 주지 않으므로 둘 다 확인해야 합니다.
* 읽기 작업에 대한 신뢰도 점수가 낮으면 입력 문서의 품질을 개선합니다([입력 요구 사항](../overview.md#input-requirements) 참조).
* 키/값 추출 작업의 신뢰도 점수가 낮으면 분석 중인 문서가 학습 집합에 사용된 문서와 동일한 유형인지 확인합니다. 학습 집합의 문서 모양이 변형된 경우 각 변형에 대해 하나의 모델을 학습하고 다른 폴더로 분할하는 것이 좋습니다.

대상으로 하는 신뢰도 점수는 사용 사례에 따라 달라지지만 일반적으로 80% 이상의 점수를 목표로 하는 것이 좋습니다. 의료 기록 또는 청구 명세서 읽기와 같은 보다 중요한 경우에는 100%의 점수가 권장됩니다.