---
title: 빠른 시작 Apache Spark 풀 만들기(미리 보기)
description: 이 가이드의 단계에 따라 Azure Synapse Analytics 작업 영역에 사용할 새 Apache Spark 풀을 만듭니다.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 1457549fef3a78356c8b1af6be620fdf30ddab46
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81420627"
---
# <a name="quickstart-create-a-new-apache-spark-pool-preview"></a>빠른 시작: 새 Apache Spark 풀 만들기(미리 보기)

Synapse Analytics는 데이터를 수집, 변환, 모델링, 분석 및 제공하는 데 도움이 되는 다양한 분석 엔진을 제공합니다. Apache Spark 풀은 오픈 소스 빅 데이터 컴퓨팅 기능을 제공합니다. Synapse 작업 영역에서 Apache Spark 풀을 만든 후에는 데이터를 로드, 모델링, 처리 및 제공하여 인사이트를 얻을 수 있습니다.

이 빠른 시작에서는 Azure Portal을 사용하여 Synapse 작업 영역에 Apache Spark 풀을 만드는 방법을 배웁니다.

> [!IMPORTANT]
> Spark 인스턴스 요금은 인스턴스 사용 여부에 관계없이 분 단위 비례 배분 방식으로 청구됩니다. Spark 인스턴스를 사용한 후 꼭 인스턴스를 종료하거나 짧은 시간 제한을 설정해야 합니다. 자세한 내용은 이 문서의 **리소스 정리** 섹션을 참조하세요.

Azure 구독이 없는 경우 [시작하기 전에 체험 계정을 만듭니다](https:/azure.microsoft.com/free/).

## <a name="prerequisites"></a>사전 요구 사항

- Azure 구독 - [체험 구독 만들기](https:/azure.microsoft.com/free/)
- [Synapse Analytics 작업 영역](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인

[Azure 포털](https:/portal.azure.com/)

## <a name="create-new-apache-spark-pool"></a>새 Apache Spark 풀 만들기

1. Apache Spark 풀을 만들려는 Synapse 작업 영역에서 **새 Apache Spark 풀**을 클릭합니다.
![새 Apache Spark 풀을 만드는 명령 주위에 빨간색 상자가 있는 Synapse 작업 영역 개요](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-01.png)
2. **기본 사항** 탭에서 다음 정보를 입력합니다.

    |설정 | 제안 값 | Description |
    | :------ | :-------------- | :---------- |
    | **Apache Spark 풀 이름** | 유효한 풀 이름 | Apache Spark 풀이 사용할 이름입니다. |
    | **노드 크기** | 작음(4 vCPU/32GB) | 이 빠른 시작에서는 비용을 줄이기 위해 이 값을 가장 작은 크기로 설정합니다. |
    | **Autoscale** | 사용 | 이 기본 설정을 유지합니다. |
    | **노드 수** | 3-40 | 이 기본 설정을 유지합니다. |
    ||||

    ![Apache Spark 풀 만들기 흐름 - 기본 탭](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-02.png)
    > [!IMPORTANT]
    > Apache Spark 풀이 사용할 수 있는 이름과 관련된 제한 사항이 있습니다. 이름은 문자 또는 숫자만 포함해야 하고, 15자 이하여야 하고, 문자로 시작하고 예약어를 포함하지 않아야 하고, 작업 영역에서 고유해야 합니다.

3. **다음: 추가 설정**을 클릭하고 기본 설정을 검토합니다. 기본 설정을 수정하지 마세요.
![Apache Spark 풀 만들기 흐름 - 추가 설정 탭](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03.png)

4. **다음: 태그**를 클릭합니다. 태그를 추가하지 마세요.
![Apache Spark 풀 만들기 흐름 - 추가 설정 탭](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03-tags.png)

5. **검토 + 만들기**를 클릭합니다.

6. 이전에 입력한 내용에 따라 세부 정보가 올바른지 확인하고 **만들기**를 클릭합니다.
![Apache Spark 풀 만들기 흐름 - 설정 검토 탭](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-05.png)

7. 이제 리소스 프로비저닝 흐름이 시작되고, 완료되면 표시됩니다. ![Apache Spark 풀 만들기 흐름 - 리소스 프로비저닝](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-06.png)

8. 프로비전이 완료된 후 작업 영역으로 돌아가면 새로 만든 Apache Spark 풀에 대한 새 항목이 표시됩니다.
 ![Apache Spark 풀 만들기 흐름 - 리소스 프로비저닝](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-07.png)

9. 지금은 실행 중인 리소스가 없으므로 Spark 요금이 부과되지 않습니다. 여기서는 만들려는 Spark 인스턴스에 대한 메타데이터를 만들었습니다.

## <a name="clean-up-resources"></a>리소스 정리

작업 영역에서 Apache Spark 풀을 삭제하려면 다음 단계를 수행합니다.
> [!WARNING]
> Apache Spark 풀을 삭제하면 작업 영역에서 분석 엔진이 제거됩니다. 그러면 더 이상 풀에 연결할 수 없으며, 이 Apache Spark 풀을 사용하는 모든 쿼리, 파이프라인 및 Notebook은 더 이상 작동하지 않습니다.

Apache Spark 풀을 삭제하려면 다음을 수행합니다.

1. 작업 영역에서 Apache Spark 풀 블레이드로 이동합니다.
2. 삭제할 Apache 풀을 선택합니다(여기서는 **contosospark**).
3. **삭제**를 누릅니다.
 ![최근에 만든 풀이 선택된 Apache Spark 풀 목록](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-08.png)
4. 삭제를 확인하고 **삭제** 단추를 누릅니다.
 ![선택한 Apache Spark 풀을 삭제하는 확인 대화 상자](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-10.png)
5. 프로세스가 성공적으로 완료되면 Apache Spark 풀이 작업 영역 리소스에 더 이상 나열되지 않습니다.

SQL 풀이 만들어지면 작업 영역에서 데이터를 로드하고, 스트림을 처리하고, 레이크에서 데이터를 읽는 등에 사용할 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [빠른 시작: 웹 도구를 사용하여 Synapse Studio에서 Apache Spark 풀 만들기](spark/apache-spark-notebook-create-spark-use-sql.md)를 참조하세요.
- [빠른 시작: Azure Portal을 사용하여 Synapse SQL 풀 만들기](quickstart-create-sql-pool.md)를 참조하세요.
