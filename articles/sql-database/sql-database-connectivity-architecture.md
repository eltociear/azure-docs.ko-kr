---
title: 연결 아키텍처
description: 이 문서에서는 azure 내부 또는 Azure 외부에서의 데이터베이스 연결에 대 한 Azure SQL 연결 아키텍처에 대해 설명 합니다.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: fasttrack-edit
titleSuffix: Azure SQL Database and SQL Data Warehouse
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
ms.date: 03/09/2020
ms.openlocfilehash: b4e7d827536245a22d168c7d9923c2e5b82830b0
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82111796"
---
# <a name="azure-sql-connectivity-architecture"></a>Azure SQL 연결 아키텍처
> [!NOTE]
> 이 문서는 Azure SQL 서버 및 Azure SQL 서버에서 생성된 SQL Database와 SQL Data Warehouse 데이터베이스에 적용됩니다. 간단히 하기 위해 SQL Database는 SQL Database와 SQL Data Warehouse를 참조할 때 사용됩니다.

> [!IMPORTANT]
> 이 문서는 **Azure SQL Database Managed Instance**에 적용되지 *않습니다*. [관리 되는 인스턴스의 연결 아키텍처](sql-database-managed-instance-connectivity-architecture.md)를 참조 하세요.

이 문서에서는 Azure SQL Database 또는 SQL Data Warehouse 네트워크 트래픽을 전달 하는 다양 한 구성 요소의 아키텍처에 대해 설명 합니다. 또한 다양 한 연결 정책 및 azure 외부에서 연결 하는 클라이언트와 Azure 내에서 연결 하는 클라이언트에 영향을 주는 방법을 설명 합니다. 

## <a name="connectivity-architecture"></a>연결 아키텍처

다음 다이어그램은 Azure SQL Database 연결 아키텍처의 대략적인 개요를 제공합니다.

![아키텍처 개요](./media/sql-database-connectivity-architecture/connectivity-overview.png)

다음 단계에서는 Azure SQL 데이터베이스에 연결하는 방법을 설명합니다.

- 클라이언트는 공용 IP 주소를 가지며 포트 1433에서 수신 대기하는 게이트웨이에 연결합니다.
- 효과적인 연결 정책에 따라 게이트웨이는 트래픽을 올바른 데이터베이스 클러스터로 리디렉션 또는 프록시합니다.
- 데이터베이스 내에서 클러스터 트래픽은 해당 Azure SQL 데이터베이스로 전달됩니다.

## <a name="connection-policy"></a>연결 정책

Azure SQL Database는 SQL Database 서버의 연결 정책 설정에 대한 다음 세 가지 옵션을 지원합니다.

- **리디렉션 (권장):** 클라이언트는 데이터베이스를 호스팅하는 노드에 직접 연결을 설정 하 여 대기 시간을 줄이고 처리량을 향상 시킵니다. 이 모드를 사용 하는 연결의 경우 클라이언트는 다음을 수행 해야 합니다.
   - 11000 11999 범위의 포트에서 지역의 모든 Azure SQL IP 주소에 대 한 클라이언트의 아웃 바운드 통신을 허용 합니다. SQL의 서비스 태그를 사용 하 여이 작업을 보다 쉽게 관리할 수 있습니다.  
   - 1433 포트에서 클라이언트의 Azure SQL Database 게이트웨이 IP 주소에 대 한 아웃 바운드 통신을 허용 합니다.

- **프록시:** 이 모드에서는 모든 연결이 Azure SQL Database 게이트웨이를 통해 프록시 되므로 대기 시간이 길어지고 처리량이 감소 합니다. 이 모드를 사용 하는 연결의 경우 클라이언트에서 1433 포트의 게이트웨이 IP 주소를 Azure SQL Database에 대 한 클라이언트의 아웃 바운드 통신을 허용 해야 합니다.

- **기본값:** 이는 연결 정책을 `Proxy` 또는 `Redirect`로 명시적으로 변경 하지 않는 한 생성 후 모든 서버에 적용 되는 연결 정책입니다. 기본 정책은 Azure 내부`Redirect` 에서 시작 되는 모든 클라이언트 연결 (예: Azure 가상 머신) 및 `Proxy`외부에서 발생 하는 모든 클라이언트 연결 (예: 로컬 워크스테이션의 연결)에 대 한 것입니다.

 가장 낮은 대기 시간 `Redirect` 및 최고 처리량을 `Proxy` 위해 연결 정책에 대 한 연결 정책을 권장 합니다. 그러나 위에서 설명한 대로 네트워크 트래픽을 허용 하기 위한 추가 요구 사항을 충족 해야 합니다. 클라이언트가 Azure Virtual Machine 인 경우 [서비스 태그](../virtual-network/security-overview.md#service-tags)를 포함 하는 Nsg (네트워크 보안 그룹)를 사용 하 여이를 수행할 수 있습니다. 클라이언트가 온-프레미스의 워크스테이션에서 연결 하는 경우 네트워크 관리자와 협력 하 여 회사 방화벽을 통해 네트워크 트래픽을 허용 해야 할 수 있습니다.

## <a name="connectivity-from-within-azure"></a>Azure 내부에서 연결

Azure 내부에서 연결하는 경우 연결에는 기본적으로 `Redirect` 연결 정책이 있습니다. `Redirect` 정책의 경우 TCP 세션이 Azure SQL 데이터베이스로 설정된 후에 대상 가상 IP가 Azure SQL Database 게이트웨이에서 클러스터의 대상 가상 IP로 변경되어 클라이언트 세션이 적절한 데이터베이스 클러스터로 리디렉션됩니다. 그런 다음, 모든 후속 패킷은 Azure SQL Database 게이트웨이를 우회하고 클러스터로 직접 흐릅니다. 아래 다이어그램은 이 트래픽 흐름을 보여줍니다.

![아키텍처 개요](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Azure 외부에서 연결

Azure 외부에서 연결하는 경우 연결에는 기본적으로 `Proxy` 연결 정책이 있습니다. `Proxy` 정책의 경우 TCP 세션이 Azure SQL Database 게이트웨이를 통해 설정되고 모든 후속 패킷이 게이트웨이를 통합니다. 아래 다이어그램은 이 트래픽 흐름을 보여줍니다.

![아키텍처 개요](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

> [!IMPORTANT]
> 또한 [DAC와의 연결](https://docs.microsoft.com/sql/database-engine/configure-windows/diagnostic-connection-for-database-administrators?view=sql-server-2017#connecting-with-dac) 을 설정 하기 위해 TCP 포트 1434 및 14000-14999를 엽니다.


## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Database 게이트웨이 IP 주소

다음 표에서는 지역별 게이트웨이의 IP 주소를 나열 합니다. Azure SQL Database에 연결 하려면 네트워크 트래픽이 해당 지역에 대 한 **모든** 게이트웨이에서 & 하도록 허용 해야 합니다.

특정 지역의 새 게이트웨이로 트래픽을 마이그레이션하는 방법에 대 한 세부 정보는 다음 문서에 나와 있습니다. [최신 게이트웨이로의 트래픽 마이그레이션 Azure SQL Database](sql-database-gateway-migration.md)


| 지역 이름          | 게이트웨이 IP 주소 |
| --- | --- |
| 오스트레일리아 중부    | 20.36.105.0 |
| 오스트레일리아 Central2   | 20.36.113.0 |
| 오스트레일리아 동부       | 13.75.149.87, 40.79.161.1 |
| 오스트레일리아 동남부 | 191.239.192.109, 13.73.109.251 |
| 브라질 남부         | 104.41.11.5, 191.233.200.14 |
| 캐나다 중부       | 40.85.224.249      |
| 캐나다 동부          | 40.86.226.166      |
| 미국 중부           | 13.67.215.62, 52.182.137.15, 23.99.160.139, 104.208.16.96, 104.208.21.1 | 
| 중국 동부           | 139.219.130.35     |
| 중국 동부 2         | 40.73.82.1         |
| 중국 북부          | 139.219.15.17      |
| 중국 북부 2        | 40.73.50.0         |
| 동아시아            | 191.234.2.139, 52.175.33.150, 13.75.32.4 |
| 미국 동부              | 40.121.158.30, 40.79.153.12, 191.238.6.43, 40.78.225.32 |
| 미국 동부 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0, 191.239.224.107, 104.208.150.3 | 
| 프랑스 중부       | 40.79.137.0, 40.79.129.1 |
| 독일 중부      | 51.4.144.100       |
| 독일 북동부   | 51.5.144.179       |
| 인도 중부        | 104.211.96.159     |
| 인도 남부          | 104.211.224.146    |
| 인도 서부           | 104.211.160.80     |
| 일본 동부           | 13.78.61.196, 40.79.184.8, 13.78.106.224, 191.237.240.43, 40.79.192.5 | 
| 일본 서부           | 104.214.148.156, 40.74.100.192, 191.238.68.11, 40.74.97.10 | 
| 한국 중부        | 52.231.32.42       |
| 한국 남부          | 52.231.200.86      |
| 미국 중북부     | 23.96.178.199, 23.98.55.75, 52.162.104.33 |
| 북유럽         | 40.113.93.91, 191.235.193.75, 52.138.224.1 | 
| 노르웨이 동부          | 51.120.96.0        |
| 노르웨이 서 부          | 51.120.216.0       |
| 남아프리카 북부   | 102.133.152.0      |
| 남아프리카 공화국 서 부    | 102.133.24.0       |
| 미국 중남부     | 13.66.62.124, 23.98.162.75, 104.214.16.32   | 
| 동남아시아      | 104.43.15.0, 23.100.117.95, 40.78.232.3   | 
| 아랍에미리트 중부          | 20.37.72.64        |
| 아랍에미리트 북부            | 65.52.248.0        |
| 영국 남부             | 51.140.184.11      |
| 영국 서부              | 51.141.8.11        |
| 미국 중서부      | 13.78.145.25       |
| 서유럽          | 40.68.37.158, 191.237.232.75, 104.40.168.105  |
| 미국 서부              | 104.42.238.205, 23.99.34.75, 13.86.216.196   |
| 미국 서부 2            | 13.66.226.202      |
|                      |                    |



## <a name="next-steps"></a>다음 단계

- Azure SQL Database 서버에 대한 Azure SQL Database 연결 정책을 변경하는 방법에 대해서는 [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy)를 참조합니다.
- ADO.NET 4.5 이상 버전을 사용하는 클라이언트의 Azure SQL Database 연결 동작에 대한 자세한 정보는 [ADO.NET 4.5에 대한 1433 이외 포트](sql-database-develop-direct-route-ports-adonet-v12.md)를 참조하세요.
- 일반 애플리케이션 개발 개요 정보는 [SQL Database 애플리케이션 개발 개요](sql-database-develop-overview.md)를 참조하세요.
