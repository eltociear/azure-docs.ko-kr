---
title: '행 추가: 모듈 참조'
titleSuffix: Azure Machine Learning
description: Azure 기계 학습에서 행 추가 모듈을 사용하여 두 데이터 집합을 통합하는 방법을 알아봅니다.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 02/22/2020
ms.openlocfilehash: cd9b5f8f182c4deab746d2c41e516a6ac23fb7aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477734"
---
# <a name="add-rows-module"></a>행 추가 모듈

이 문서에서는 Azure 기계 학습 디자이너(미리 보기)의 모듈에 대해 설명합니다.

이 모듈을 사용하여 두 데이터 집합을 연결합니다. 연결에서 두 번째 데이터 집합의 행이 첫 번째 데이터 집합의 끝에 추가됩니다.  
  
행 연결은 다음과 같은 시나리오에서 유용합니다.  
  
+ 일련의 평가 통계를 생성했으며 보다 쉬운 보고를 위해 이러한 통계를 테이블 하나로 결합하려는 경우  
  
+ 여러 데이터 집합에서 작업한 후 해당 데이터 집합을 결합하여 최종 데이터 집합을 만들려는 경우  

## <a name="how-to-use-add-rows"></a>행 추가 사용 방법  

두 데이터 집합에서 행을 연결하려면 행에 정확히 동일한 스키마가 있어야 합니다. 즉, 열수가 같고 열의 동일한 유형의 데이터가 표시됩니다.

1.  행 **추가** 모듈을 파이프라인으로 드래그하면 **데이터 변환**에서 찾을 수 있습니다.

2. 두 입력 포트에 데이터 집합을 연결합니다. 추가할 데이터 집합이 두 번째(오른쪽) 포트에 연결되어야 합니다. 
  
3.  파이프라인을 제출합니다. 출력 데이터 집합의 행 수는 두 입력 데이터 집합의 행 합계와 같아야 합니다.

    **행 추가** 모듈의 두 입력에 동일한 데이터 집합을 추가하면 데이터 집합이 중복됩니다. 

## <a name="next-steps"></a>다음 단계

Azure 기계 학습에 사용할 수 있는 [모듈 집합을](module-reference.md) 참조하십시오. 