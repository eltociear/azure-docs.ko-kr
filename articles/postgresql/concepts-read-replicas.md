---
title: 복제본 읽기 - PostgreSQL용 Azure 데이터베이스 - 단일 서버
description: 이 문서에서는 PostgreSQL - 단일 서버에 대한 Azure 데이터베이스의 읽기 복제본 기능에 대해 설명합니다.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/23/2020
ms.openlocfilehash: 545d04bdede76a6ce25c9e4665f39c01ff6caa73
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81531986"
---
# <a name="read-replicas-in-azure-database-for-postgresql---single-server"></a>PostgreSQL - 단일 서버에 대한 Azure 데이터베이스에서 복제본 읽기

읽기 복제본 기능을 사용하면 Azure Database for PostgreSQL 서버에서 읽기 전용 서버로 데이터를 복제할 수 있습니다. 마스터 서버에서 최대 5개의 복제본으로 복제할 수 있습니다. 복제본은 PostgreSQL 엔진 기본 복제 기술을 사용하여 비동기식으로 업데이트 됩니다.

복제본은 일반 Azure Database for PostgreSQL 서버와 비슷한 방식으로 관리하는 새로운 서버입니다. 읽기 복제본의 경우, vCore 및 스토리지에 프로비저닝된 컴퓨팅에 대한 비용이 GB/월 단위로 청구됩니다.

[복제본을 만들고 관리](howto-read-replicas-portal.md)하는 방법을 알아봅니다.

## <a name="when-to-use-a-read-replica"></a>읽기 복제본을 사용하는 경우
읽기 복제본 기능은 읽기 작업이 많은 워크로드의 성능과 규모를 향상시키는 데 도움이 됩니다. 읽기 워크로드를 복제본과 격리할 수 있고 쓰기 워크로드를 마스터로 보낼 수 있습니다.

일반적인 시나리오는 BI와 분석 워크로드가 보고를 위한 데이터 원본으로 읽기 복제본을 사용하도록 합니다.

복제본은 읽기 전용이기 때문에 마스터에 대한 쓰기 용량 부담을 직접적으로 줄이지는 못합니다. 이 기능은 쓰기 작업이 많은 워크로드를 대상으로 하지 않습니다.

읽기 복제본 기능은 PostgreSQL 동기식 복제를 사용합니다. 이 기능은 동기식 복제 시나리오를 위한 것이 아닙니다. 마스터와 복제본 간에는 측정 가능한 지연이 발생합니다. 복제본의 데이터는 결과적으로 마스터의 데이터와 일치하게 됩니다. 이러한 지연 시간을 수용할 수 있는 워크로드에 이 기능을 사용합니다.

## <a name="cross-region-replication"></a>지역 간 복제
마스터 서버와 다른 지역에서 읽기 복제본을 만들 수 있습니다. 지역 간 복제는 재해 복구 계획 또는 사용자에게 데이터를 더 가까이 가져오는 등의 시나리오에 유용할 수 있습니다.

[PostgreSQL 영역에 대한 모든 Azure 데이터베이스에](https://azure.microsoft.com/global-infrastructure/services/?products=postgresql)마스터 서버를 가질 수 있습니다. 마스터 서버는 쌍을 이루는 리전 또는 범용 복제본 지역에 복제본을 가질 수 있습니다. 아래 그림은 마스터 지역에 따라 사용할 수 있는 복제본 영역을 보여 줍니다.

[![복제본 영역 읽기](media/concepts-read-replica/read-replica-regions.png)](media/concepts-read-replica/read-replica-regions.png#lightbox)

### <a name="universal-replica-regions"></a>범용 복제 영역
마스터 서버의 위치에 관계없이 언제든지 다음 리전에서 읽기 복제본을 만들 수 있습니다. 범용 복제 본리 영역은 다음과 같습니다.

오스트레일리아 동부, 오스트레일리아 남동부, 미국 중부, 동아시아, 미국 동부, 미국 동부, 동북2, 일본 동부, 일본 서부, 한국 중부, 한국 중부 미국, 북중부, 북유럽, 미국 중남부, 동남아시아, 영국 남부, 영국 서부, 서유럽, 서부 미국.

*미국 서부 2는 교차 지역 복제 위치로 일시적으로 사용할 수 없습니다.


### <a name="paired-regions"></a>쌍을 이루는 지역
범용 복제본 영역 외에도 마스터 서버의 Azure 쌍을 이루는 리전에서 읽기 복제본을 만들 수 있습니다. 리전 쌍을 모르는 경우 [Azure 쌍을 이루는 지역 문서에서](../best-practices-availability-paired-regions.md)자세히 알아볼 수 있습니다.

재해 복구 계획에 지역 간 복제본을 사용하는 경우 다른 지역 중 하나가 아닌 페어링된 리전에서 복제본을 만드는 것이 좋습니다. 쌍을 이루는 지역은 동시 업데이트를 피하고 물리적 격리 및 데이터 상지의 우선 순위를 지정합니다.  

고려해야 할 제한 사항이 있습니다. 

* 지역별 가용성: PostgreSQL용 Azure 데이터베이스는 미국 서부 2, 프랑스 중부, 아랍에미리트 북부 및 독일 중부에서 사용할 수 있습니다. 그러나 쌍을 이루는 영역은 사용할 수 없습니다.
    
* 단방향 쌍: 일부 Azure 영역은 한 방향으로만 페어링됩니다. 이 지역은 서인도, 브라질 남쪽을 포함합니다. 
   즉, 서인도의 마스터 서버는 인도 남부에서 복제본을 만들 수 있습니다. 그러나 인도 남부의 마스터 서버는 서인도에서 복제본을 만들 수 없습니다. 이는 서인도의 보조 지역이 인도 남부이지만 남인도의 보조 지역은 서인도가 아니기 때문입니다.


## <a name="create-a-replica"></a>복제본 만들기
복제본 만들기 워크플로를 시작하면 빈 Azure Database for PostgreSQL 서버가 만들어집니다. 새 서버는 마스터 서버에 있는 데이터로 채워집니다. 생성 시간은 마스터의 데이터 양과 지난 주 전체 백업 이후의 시간에 따라 달라집니다. 시간은 몇 분에서 몇 시간까지 걸릴 수 있습니다.

모든 복제본은 저장소 자동 성장에 사용할 수 [있습니다.](concepts-pricing-tiers.md#storage-auto-grow) 자동 증가 기능을 사용하면 복제본이 복제된 데이터를 계속 유지할 수 있으며 저장소 부족 오류로 인한 복제 중단을 방지할 수 있습니다.

읽기 복제본 기능은 PostgreSQL의 물리적 복제(논리 복제 아님)를 사용합니다. 복제 슬롯을 사용하는 스트리밍 복제가 기본 작동 모드입니다. 필요한 경우 따라잡기 위해 로그 전달이 사용됩니다.

[Azure Portal에서 읽기 복제본을 만드는 방법](howto-read-replicas-portal.md)을 알아봅니다.

## <a name="connect-to-a-replica"></a>복제본에 연결
복제본을 만들면 마스터 서버의 방화벽 규칙 또는 VNet 서비스 엔드포인트를 상속하지 않습니다. 이러한 규칙은 복제본에 대해 별도로 설정해야 합니다.

복제본은 마스터 서버에서 해당 관리자 계정을 상속합니다. 마스터 서버의 모든 사용자 계정은 읽기 복제본으로 복제됩니다. 마스터 서버에서 사용 가능한 사용자 계정을 사용해야만 읽기 복제본에 연결할 수 있습니다.

일반 Azure Database for PostgreSQL 서버에서처럼 같이 해당 호스트 이름 및 유효한 사용자 계정을 사용하여 복제본에 연결할 수 있습니다. 관리자 사용자 이름 **myadmin을**사용 하 여 **내 복제본이라는** 서버의 경우 psql을 사용하여 복제본에 연결할 수 있습니다.

```
psql -h myreplica.postgres.database.azure.com -U myadmin@myreplica -d postgres
```

프롬프트가 표시되면 사용자 계정의 암호를 입력합니다.

## <a name="monitor-replication"></a>복제 모니터링
PostgreSQL용 Azure 데이터베이스는 복제를 모니터링하기 위한 두 가지 메트릭을 제공합니다. 두 가지 메트릭은 **복제본 에서 최대 지연** 및 **복제본 지연입니다.** 이러한 메트릭을 보는 방법을 보려면 [읽기 복제본 방법 문서의](howto-read-replicas-portal.md) **복제본 모니터** 섹션을 참조하십시오.

**복제본에서 가장 지연되는 최대** 지연 메트릭은 마스터와 가장 뒤쳐진 복제본 사이의 바이트 지연을 보여 주며, 이 메트릭은 가장 지연되는 복제본입니다. 이 메트릭은 마스터 서버에서만 사용할 수 있습니다.

**복제 본래** 측정항목은 마지막으로 재생된 트랜잭션 이후의 시간을 표시합니다. 마스터 서버에서 트랜잭션이 발생하지 않으면 메트릭은 이 지연 시간을 반영합니다. 이 메트릭은 복제본 서버에서만 사용할 수 있습니다. 복제본 지연은 `pg_stat_wal_receiver` 뷰에서 계산됩니다.

```SQL
EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp());
```

복제본 지연 시간이 워크로드에 적합하지 않은 값에 도달하면 알리도록 경고를 설정하십시오. 

추가 인사이트를 얻으려면 마스터 서버를 직접 쿼리하여 모든 복제본의 복제 지연 시간(바이트)을 가져옵니다.

PostgreSQL 버전 10:

```SQL
select pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

PostgreSQL 버전 9.6 이전:

```SQL
select pg_xlog_location_diff(pg_current_xlog_location(), replay_location) 
AS total_log_delay_in_bytes from pg_stat_replication;
```

> [!NOTE]
> 마스터 서버 또는 읽기 복제본이 다시 시작되면 다시 시작하여 따라잡는 데 걸리는 시간이 복제본 지연 시간 메트릭에 반영됩니다.

## <a name="stop-replication"></a>복제 중지
마스터 및 복제본 간의 복제를 중지할 수 있습니다. 중지 작업을 수행하면 복제본이 다시 시작되고 복제 설정이 제거됩니다. 마스터 서버와 읽기 복제본 사이에서 복제가 중지된 후에 복제본은 독립 실행형 서버가 됩니다. 독립 실행형 서버의 데이터는 복제 중지 명령이 시작될 때 복제본에서 사용할 수 있던 데이터입니다. 독립 실행형 서버는 마스터 서버를 따라잡지 않습니다.

> [!IMPORTANT]
> 독립 실행형 서버를 다시 복제본으로 만들 수 없습니다.
> 읽기 복제본에서 복제를 중지하기 전에 복제본에 필요한 모든 데이터가 있는지 확인하십시오.

복제를 중지하면 복제본이 이전 마스터 및 다른 복제본에 대한 모든 링크를 잃게 됩니다.

[복제본에 대한 복제를 중지](howto-read-replicas-portal.md)하는 방법을 알아봅니다.

## <a name="failover"></a>장애 조치
마스터 서버와 복제본 서버 간에는 자동 장애 조치(failover)가 없습니다. 

복제는 비동기이므로 마스터와 복제본 사이에 지연이 있습니다. 지연의 양은 마스터 서버에서 실행되는 워크로드의 강도와 데이터 센터 간의 대기 시간 과 같은 여러 가지 요인에 의해 영향을 받을 수 있습니다. 대부분의 경우 복제본 지연 범위는 몇 초에서 몇 분 사이입니다. 각 복제본에 사용할 수 있는 메트릭 *복제 지연을*사용하여 실제 복제 지연을 추적할 수 있습니다. 이 메트릭은 마지막으로 재생된 트랜잭션 이후의 시간을 표시합니다. 기간 동안 복제본 지연을 관찰하여 평균 지연을 식별하는 것이 좋습니다. 복제본 지연에 대한 경고를 설정하여 예상 범위를 벗어나면 조치를 취할 수 있습니다.

> [!Tip]
> 복제본에 장애 조치(failover)하는 경우 마스터에서 복제본을 해제할 때의 지연은 손실되는 데이터의 양을 나타냅니다.

복제본에 장애 조치(failover)를 지정하기로 결정한 후에는 

1. 복제본에 대한 복제 중지<br/>
   이 단계는 복제본 서버가 쓰기를 수락할 수 있도록 하는 데 필요합니다. 이 프로세스의 일부로 복제본 서버가 다시 시작되고 마스터에서 해제됩니다. 복제 중지를 시작하면 백 엔드 프로세스를 완료하는 데 일반적으로 약 2분이 걸립니다. 이 작업의 의미를 이해하려면 이 문서의 [복제 중지](#stop-replication) 섹션을 참조하십시오.
    
2. 응용 프로그램을 (이전) 복제본으로 가리킵니다.<br/>
   각 서버에는 고유한 연결 문자열이 있습니다. 응용 프로그램을 업데이트하여 마스터 대신 (이전) 복제본을 가리킵니다.
    
응용 프로그램이 읽기 및 쓰기를 성공적으로 처리하면 장애 조치(failover)를 완료합니다. 응용 프로그램에서 경험하는 가동 중지 시간은 문제를 감지하고 위의 1단계와 2단계를 완료하는 시기에 따라 달라집니다.


## <a name="considerations"></a>고려 사항

이 섹션에서는 읽기 복제본 기능의 고려 사항에 대한 요약이 제공됩니다.

### <a name="prerequisites"></a>사전 요구 사항
읽기 복제본을 만들기 전에, 마스터 서버에서 `azure.replication_support` 매개 변수를 **REPLICA**로 설정해야 합니다. 이 매개 변수가 변경되면, 서버를 다시 시작해야 변경 사항이 적용됩니다. `azure.replication_support` 매개 변수는 범용 및 메모리 최적화 계층에만 적용됩니다.

### <a name="new-replicas"></a>새 복제본
읽기 복제본은 새로운 Azure Database for PostgreSQL 서버로 생성됩니다. 기존 서버를 복제본으로 만들 수 없습니다. 다른 읽기 복제본의 복제본은 만들 수 없습니다.

### <a name="replica-configuration"></a>복제본 구성
복제본은 마스터와 동일한 계산 및 저장소 설정을 사용하여 만들어집니다. 복제본을 만든 후에는 마스터 서버와는 별도로 컴퓨팅 생성, vCore, 스토리지 및 백업 보존 기간 등의 일부 설정을 변경할 수 있습니다. 가격 책정도 기본 계층에서 다른 계층으로 또는 다른 계층에서 기본 계층으로 변경하는 경우를 제외하고 독립적으로 변경할 수 있습니다.

> [!IMPORTANT]
> 마스터 설정을 새 값으로 업데이트하기 전에 복제본 구성을 같거나 더 큰 값으로 업데이트합니다. 이렇게 하면 복제본이 마스터에 대한 변경 내용을 유지할 수 있습니다.

PostgreSQL에서는 읽기 복제본의 `max_connections` 매개 변수 값이 마스터 값보다 크거나 같아야 합니다. 그렇지 않으면 해당 복제본이 시작되지 않습니다. Azure Database for PostgreSQL에서 `max_connections` 매개 변수 값은 SKU에 기반합니다. 자세한 내용은 [Azure Database for PostgreSQL의 제한](concepts-limits.md)을 참조하세요. 

위에서 설명한 서버 값을 업데이트하려고 하지만 제한을 준수하지 않으면 오류가 발생합니다.

방화벽 규칙, 가상 네트워크 규칙 및 매개 변수 설정은 복제본을 만들거나 나중에 복제본을 만들 때 마스터 서버에서 복제본으로 상속되지 않습니다.

### <a name="max_prepared_transactions"></a>max_prepared_transactions
[PostgreSQL은](https://www.postgresql.org/docs/current/runtime-config-resource.html#GUC-MAX-PREPARED-TRANSACTIONS) `max_prepared_transactions` 읽기 복제본의 매개 변수 값이 마스터 값보다 크거나 같아야 합니다. 그렇지 않으면 복제본이 시작되지 않습니다. 마스터에서 변경하려면 `max_prepared_transactions` 먼저 복제본에서 변경합니다.

### <a name="stopped-replicas"></a>중지된 복제본
마스터 서버와 읽기 복제본 간의 복제를 중지하면 복제본이 다시 시작되어 변경 내용이 적용됩니다. 중지된 복제본은 읽기와 쓰기를 모두 허용하는 독립 실행형 서버가 됩니다. 독립 실행형 서버를 다시 복제본으로 만들 수 없습니다.

### <a name="deleted-master-and-standalone-servers"></a>삭제된 마스터 및 독립 실행형 서버
마스터 서버를 삭제하면 이 서버의 모든 읽기 복제본이 독립 실행형 서버가 됩니다. 이 변경 내용을 반영하기 위해 복제본이 다시 시작됩니다.

## <a name="next-steps"></a>다음 단계
* [Azure Portal에서 읽기 복제본을 만들고 관리하는 방법](howto-read-replicas-portal.md)을 알아봅니다.
* [Azure CLI 및 REST API에서 읽기 복제본을 만들고 관리하는](howto-read-replicas-cli.md)방법에 대해 알아봅니다.
