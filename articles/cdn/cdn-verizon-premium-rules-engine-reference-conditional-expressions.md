---
title: Azure CDN - 버라이존 프리미엄 규칙 엔진에 대한 조건식
description: Verizon Premium 규칙 엔진 일치 조건 및 기능의 Azure CDN에 대한 참조 설명서입니다.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: allensu
ms.openlocfilehash: 25b3c1a011e39adea651a6daa1d4ab9aa3a9d186
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253512"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-conditional-expressions"></a>버라이존 프리미엄 규칙 엔진 조건식에서 Azure CDN

이 문서에서는 CDN(Azure 콘텐츠 배달 네트워크) [규칙 엔진에](cdn-verizon-premium-rules-engine.md)대한 조건식에 대한 자세한 설명을 나열합니다.

규칙의 첫 번째 부분은 조건식입니다.

조건식 | Description
-----------------------|-------------
IF | IF 식은 항상 규칙의 첫 번째 문의 일부입니다. 다른 모든 조건식과 마찬가지로 이 IF 문은 일치와 관련이 있어야 합니다. 추가 조건식정의식이 정의되지 않은 경우 이 일치는 피처 집합이 요청에 적용되기 전에 충족해야 하는 기준을 결정합니다.
AND IF | AND IF 식은 IF, AND IF 형식의 조건식 뒤에만 추가할 수 있습니다. 초기 IF 문에 대해 충족되어야 하는 다른 조건이 있음을 나타냅니다.
ELSE IF| ELSE IF 식은 이 ELSE IF 문과 관련된 기능 집합이 수행되기 전에 충족되어야 하는 대체 조건을 지정합니다. ELSE IF 문이 있으면 이전 문의 끝을 나타냅니다. ELSE IF 문 다음에 올 수 있는 유일한 조건식은 다른 ELSE IF 문입니다. 즉, ELSE IF 문은 충족해야 하는 단일 추가 조건을 지정하는 데에만 사용할 수 있습니다.

**예:** ![CDN 일치 조건](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > 후속 규칙은 이전 규칙에서 지정된 동작을 재정의할 수 있습니다.
   > 예: catch-all 규칙은 토큰 기반 인증을 통해 모든 요청을 보호합니다. 특정 요청 형식에 대한 예외를 만들기 위해 다른 규칙을 바로 아래에 만들 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [Azure CDN 개요](cdn-overview.md)
- [규칙 엔진 참조](cdn-verizon-premium-rules-engine-reference.md)
- [규칙 엔진 일치 조건](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [규칙 엔진 기능](cdn-verizon-premium-rules-engine-reference-features.md)
- [규칙 엔진을 사용하여 기본 HTTP 동작 재정의](cdn-verizon-premium-rules-engine.md)