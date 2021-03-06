---
title: 아파치 스리프트 프레임 워크에 & JDBC / ODBC 문제 - Azure HDInsight
description: Azure HDInsight에서 JDBC/ODBC 및 아파치 중고품 소프트웨어 프레임워크를 사용하여 대용량 데이터 세트를 다운로드할 수 없습니다.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 23693dcae2f361b88440ec88ca39fd8ed229d85a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894268"
---
# <a name="unable-to-download-large-data-sets-using-jdbcodbc-and-apache-thrift-software-framework-in-hdinsight"></a>HDInsight에서 JDBC/ODBC 및 아파치 중고품 소프트웨어 프레임워크를 사용하여 대용량 데이터 세트를 다운로드할 수 없음

이 문서에서는 Azure HDInsight 클러스터에서 아파치 스파크 구성 요소를 사용할 때 문제에 대한 문제 해결 단계 및 가능한 해결 에 대해 설명합니다.

## <a name="issue"></a>문제

Azure HDInsight에서 JDBC/ODBC 및 아파치 중고품 소프트웨어 프레임워크를 사용하여 대용량 데이터 세트를 다운로드하려고 할 때 다음과 유사한 오류 메시지가 나타납니다.

```
org.apache.spark.SparkException: Kryo serialization failed:
Buffer overflow. Available: 0, required: 36518. To avoid this, increase spark.kryoserializer.buffer.max value.
```

## <a name="cause"></a>원인

이 예외는 허용 된 것 보다 더 많은 버퍼 공간을 사용 하려고 하는 직렬화 프로세스에 의해 발생 합니다. Spark 2.0.0에서 클래스는 `org.apache.spark.serializer.KryoSerializer` 아파치 스리프트 소프트웨어 프레임워크를 통해 데이터에 액세스할 때 개체를 직렬화하는 데 사용됩니다. 네트워크를 통해 전송되거나 직렬화된 형식으로 캐시되는 데이터에 대해 다른 클래스가 사용됩니다.

## <a name="resolution"></a>해결 방법

`Kryoserializer` 버퍼 값을 늘립니다. 명명된 `spark.kryoserializer.buffer.max` 키를 추가하고 `2048` 아래의 spark2 `Custom spark2-thrift-sparkconf`구성에 설정합니다.

## <a name="next-steps"></a>다음 단계

문제가 표시되지 않거나 문제를 해결할 수 없는 경우 다음 채널 중 하나를 방문하여 추가 지원을 받으세요.

* Azure 커뮤니티 지원을 통해 Azure 전문가의 답변을 얻을 [수 있습니다.](https://azure.microsoft.com/support/community/)

* 연결 [@AzureSupport](https://twitter.com/azuresupport) - Azure 커뮤니티를 올바른 리소스(답변, 지원 및 전문가)에 연결하여 고객 경험을 개선하기 위한 공식 Microsoft Azure 계정과 연결합니다.

* 추가 도움이 필요한 경우 [Azure 포털](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)에서 지원 요청을 제출할 수 있습니다. 메뉴 모음에서 **지원을** 선택하거나 도움말 + 지원 허브를 **엽니다.** 자세한 내용은 [Azure 지원 요청을 만드는 방법을](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)검토하십시오. 구독 관리 및 청구 지원에 대한 액세스는 Microsoft Azure 구독에 포함되며 기술 지원은 [Azure 지원 계획](https://azure.microsoft.com/support/plans/)중 하나를 통해 제공됩니다.
