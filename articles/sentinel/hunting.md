---
title: Azure 센티넬의 사냥 기능 | 마이크로 소프트 문서
description: 이 문서에서는 Azure Sentinel 사냥 기능을 사용하는 방법에 대해 설명합니다.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/10/2019
ms.author: yelevin
ms.openlocfilehash: 52af688917aa531d125f83844df29a988ed7cb7e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686627"
---
# <a name="hunt-for-threats-with-azure-sentinel"></a>Azure 센티넬로 위협 찾기

보안 위협을 사전에 찾고자 하는 조사자 인 경우 Azure Sentinel 강력한 검색 및 쿼리 도구를 사용하여 조직의 데이터 원본 에서 보안 위협을 검색합니다. 그러나 시스템과 보안 어플라이언스는 의미 있는 이벤트로 구문 분석및 필터링하기 어려울 수 있는 많은 양의 데이터를 생성합니다. 보안 분석가가 보안 앱에서 감지되지 않은 새로운 이상 징후를 사전에 파악할 수 있도록 Azure Sentinel의 기본 제공 검색 쿼리를 통해 네트워크에 이미 있는 데이터에서 문제를 찾기 위해 올바른 질문을 할 수 있습니다. 

예를 들어 기본 제공 쿼리 하나 인프라에서 실행 되는 가장 드문 프로세스에 대 한 데이터를 제공 합니다-실행 될 때마다 경고를 원하지 않을 것 이다, 그들은 완전히 무죄 수 있습니다., 하지만 경우에 쿼리를 살펴보고 비정상적인 아무것도 있는지 확인 하는 것이 좋습니다. 



Azure Sentinel 사냥을 사용하면 다음 기능을 활용할 수 있습니다.

- 기본 제공 쿼리: 시작 페이지를 시작하려면 시작 을 시작하고 테이블 및 쿼리 언어에 익숙해지도록 설계된 미리 로드된 쿼리 예제를 제공합니다. 이러한 기본 제공 검색 쿼리는 Microsoft 보안 연구원이 지속적으로 개발하여 새 쿼리를 추가하고 기존 쿼리를 미세 조정하여 새로운 검색을 찾고 새로운 공격의 시작을 위해 사냥을 시작할 위치를 파악할 수 있는 진입점을 제공합니다. 

- IntelliSense를 사용 하 고 강력한 쿼리 언어: 다음 단계로 사냥을 하는 데 필요한 유연성을 제공 하는 쿼리 언어 위에 내장 되어 있습니다.

- 나만의 북마크 만들기: 사냥 과정에서 일치항목이나 결과, 대시보드 또는 비정상적이거나 의심스러운 활동을 접할 수 있습니다. 나중에 다시 돌아올 수 있도록 해당 항목을 표시하려면 책갈피 기능을 사용합니다. 책갈피를 사용하면 나중에 항목을 저장할 수 있으므로 조사를 위해 인시던트를 만드는 데 사용할 수 있습니다. 책갈피에 대한 자세한 내용은 [찾기에서 책갈피 사용을](hunting.md)참조하십시오.
- 전자 필기장을 사용하여 조사를 자동화: 노트북은 조사 및 사냥단계를 거치도록 구축할 수 있는 단계별 플레이북과 같습니다.  전자 필기전자는 조직의 다른 사용자와 공유할 수 있는 재사용 가능한 플레이북의 모든 사냥 단계를 캡슐화합니다. 
- 저장된 데이터 쿼리: 쿼리할 수 있도록 테이블에서 데이터에 액세스할 수 있습니다. 예를 들어 프로세스 만들기, DNS 이벤트 및 기타 여러 이벤트 형식을 쿼리할 수 있습니다.

- 커뮤니티 링크: 더 큰 커뮤니티의 힘을 활용하여 추가 쿼리 및 데이터 원본을 찾습니다.
 
## <a name="get-started-hunting"></a>사냥 을 시작

1. Azure Sentinel 포털에서 **사냥**을 클릭합니다.
  ![푸른 파수꾼사냥 시작](media/tutorial-hunting/hunting-start.png)

2. **사냥** 페이지를 열면 모든 사냥 쿼리가 단일 테이블에 표시됩니다. 이 표에는 Microsoft 보안 분석가 팀이 작성한 모든 쿼리와 사용자가 만들거나 수정한 추가 쿼리가 나열되어 있습니다. 각 쿼리는 검색하는 내용과 검색되는 데이터의 종류에 대한 설명을 제공합니다. 이러한 템플릿은 오른쪽의 아이콘이 초기 액세스, 지속성 및 유출과 같은 위협 유형을 분류하는 다양한 전술로 그룹화됩니다. 필드를 사용하여 이러한 사냥 쿼리 템플릿을 필터링할 수 있습니다. 모든 쿼리를 즐겨찾기에 저장할 수 있습니다. 즐겨찾기에 쿼리를 저장하면 **사냥** 페이지에 액세스할 때마다 쿼리가 자동으로 실행됩니다. 고유한 사냥 쿼리를 만들거나 기존 사냥 쿼리 템플릿을 사용자 지정할 수 있습니다. 
 
2. 사냥 쿼리 세부 정보 페이지에서 **쿼리 실행을** 클릭하여 사냥 페이지를 벗어나지 않고 쿼리를 실행합니다.  일치 항목 수가 테이블 내에 표시됩니다. 사냥 쿼리 목록과 해당 일치 항목을 검토합니다. 킬 체인에서 매치가 연관된 스테이지를 확인하십시오.

3. 쿼리 세부 정보 창에서 기본 쿼리를 빠르게 검토하거나 **쿼리 결과 보기를** 클릭하여 로그 애널리틱스에서 쿼리를 엽니다. 하단에서 쿼리에 대한 일치 항목을 검토합니다.

4.    행을 클릭하고 조사할 행을 추가하려면 **책갈피 추가를** 선택합니다. 

5. 그런 다음 기본 **사냥** 페이지로 **돌아가서 북마크** 탭을 클릭하여 모든 의심스러운 활동을 확인합니다. 

6. 책갈피를 선택한 다음 **조사를** 클릭하여 조사 환경을 엽니다. 책갈피를 필터링할 수 있습니다. 예를 들어 캠페인을 조사하는 경우 캠페인에 대한 태그를 만든 다음 캠페인을 기반으로 모든 책갈피를 필터링할 수 있습니다.

1. 가능한 공격에 대한 높은 가치의 통찰력을 제공하는 사냥 쿼리를 발견한 후 쿼리를 기반으로 사용자 지정 검색 규칙을 만들고 이러한 인사이트를 보안 인시던트 응답자에게 경고로 표시할 수도 있습니다.

 

## <a name="query-language"></a>쿼리 언어 

Azure Sentinel에서의 사냥은 Kusto 쿼리 언어를 기반으로 합니다. 쿼리 언어 및 지원되는 연산자에 대한 자세한 내용은 [쿼리 언어 참조](/azure/azure-monitor/log-query/get-started-queries)를 참조하십시오.

## <a name="public-hunting-query-github-repository"></a>공개 사냥 쿼리 GitHub 리포지토리

[사냥 쿼리 리포지토리를](https://github.com/Azure/Orion)확인하십시오. 고객이 공유하는 예제 쿼리를 기여하고 사용합니다.

 

## <a name="sample-query"></a>샘플 쿼리

일반적인 쿼리는 테이블 이름으로 시작하며 그 뒤에 로 구분된 일련의 \|연산자가 있습니다.

위의 예에서 테이블 이름 SecurityEvent로 시작 하 고 필요에 따라 파이프 된 요소를 추가 합니다.

1. 이전 7일 간의 레코드만 검토할 시간 필터를 정의합니다.

2. 이벤트 ID 4688만 표시하도록 쿼리에 필터를 추가합니다.

3. cscript.exe의 인스턴스만 포함하도록 CommandLine의 쿼리에 필터를 추가합니다.

4. 탐색하려는 열만 투영하고 결과를 1000으로 제한하고 쿼리 **실행을**클릭합니다.
5. 녹색 삼각형을 클릭하고 쿼리를 실행합니다. 쿼리를 테스트하고 실행하여 비정상적인 동작을 찾을 수 있습니다.

## <a name="useful-operators"></a>유용한 연산자

쿼리 언어는 강력하며 사용 가능한 연산자가 많으며 일부 유용한 연산자가 여기에 나열되어 있습니다.

**여기서** - 조건어를 충족하는 행의 하위 집합으로 테이블을 필터링합니다.

**요약** 요약 - 입력 테이블의 내용을 집계하는 테이블을 생성합니다.

**join** - 두 테이블의 행을 병합하여 각 테이블에서 지정된 열의 값을 일치시켜 새 테이블을 형성합니다.

**count** - 입력 레코드 집합의 레코드 수를 반환합니다.

**top** - 지정된 열로 정렬된 첫 번째 N 레코드를 반환합니다.

**제한** - 지정된 수의 행으로 되돌아갑니다.

**project** - 포함, 이름 변경 또는 삭제할 열을 선택하고 계산된 새 열을 삽입할 열을 선택합니다.

**확장** - 계산된 열을 만들고 결과 집합에 보도록 합니다.

**makeset** - Expr이 그룹에서 취하는 고유 값 집합의 동적(JSON) 배열반환

**찾기** - 테이블 집합에서 조건어와 일치하는 행을 찾습니다.

## <a name="save-a-query"></a>쿼리 저장

쿼리를 만들거나 수정하여 고유한 쿼리로 저장하거나 동일한 테넌트에 있는 사용자와 공유할 수 있습니다.

   ![쿼리 저장](./media/tutorial-hunting/save-query.png)

새 사냥 쿼리 만들기:

1. **새 쿼리를** 클릭하고 **저장을**선택합니다.
2. 모든 빈 필드를 입력하고 **저장을**선택합니다.

   ![새 쿼리](./media/tutorial-hunting/new-query.png)

기존 사냥 쿼리를 복제하고 수정합니다.

1. 수정할 테이블에서 사냥 쿼리를 선택합니다.
2. 수정하려는 쿼리 줄에서 타원(...)을 선택하고 **복제 쿼리를**선택합니다.

   ![복제 쿼리](./media/tutorial-hunting/clone-query.png)
 

3. 쿼리를 수정하고 **만들기를**선택합니다.

   ![사용자 지정 쿼리](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>다음 단계
이 문서에서는 Azure Sentinel을 사용하여 사냥 조사를 실행하는 방법을 배웠습니다. Azure Sentinel에 대한 자세한 내용은 다음 문서를 참조하세요.


- [노트북을 사용하여 자동화된 사냥 캠페인 실행](notebooks.md)
- [북마크를 사용하여 사냥하는 동안 흥미로운 정보를 저장합니다.](bookmarks.md)
