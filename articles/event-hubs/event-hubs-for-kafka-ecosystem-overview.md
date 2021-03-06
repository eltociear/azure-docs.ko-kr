---
title: Apache Kafka 앱에서 이벤트 허브 사용 - Azure Event Hubs | Microsoft Docs
description: 이 문서에서는 Azure Event Hubs에서 지원하는 Apache Kafka에 정보를 제공합니다.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: article
ms.custom: seodec18
ms.date: 02/12/2020
ms.author: shvija
ms.openlocfilehash: 91e2d70bab8c1be4b3e5b400ce21122eccb1e9eb
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811367"
---
# <a name="use-azure-event-hubs-from-apache-kafka-applications"></a>Apache Kafka 애플리케이션에서 Azure Event Hubs 사용
Event Hubs는 기존 Kafka 기반 애플리케이션이 사용자 고유의 Kafka 클러스터 실행에 대한 대안으로 사용할 수 있는 Kafka 엔드포인트을 제공합니다. Event Hubs는 [Apache Kafka 프로토콜 1.0 이상](https://kafka.apache.org/documentation/)을 지원하고 MirrorMaker를 포함하여 기존 Kafka 애플리케이션과 작동합니다.  

> [!VIDEO https://www.youtube.com/embed/UE1WgB96_fc]

## <a name="what-does-event-hubs-for-kafka-provide"></a>Kafka용 Event Hubs는 무엇을 제공하나요?

Kafka용 Event Hubs 기능은 Kafka 버전 1.0과 호환가능한 이진이며 나중에 Kafka 토픽에서 읽기 및 쓰기를 위한 Azure Event Hubs에 프로토콜 헤드를 제공합니다. 코드 변경 없이 최소한의 구성 변경으로 애플리케이션에서 Kafka 엔드포인트를 사용하여 시작할 수 있습니다. Kafka 클러스터를 가리키는 대신 이벤트 허브에서 노출된 Kafka 엔드포인트를 가리키도록 구성에서 연결 문자열을 업데이트합니다. 그런 다음, Kafka 프로토콜을 사용하는 애플리케이션에서 Event Hubs로 이벤트 스트리밍을 시작할 수 있습니다. 또한 이 통합은 현재 미리 보기로 제공되는 [Kafka Connect](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/connect) 같은 프레임워크를 지원합니다. 

개념적으로 Kafka 및 이벤트 허브는 거의 동일합니다: 스트리밍 데이터를 위해 구축된 분할된 로그입니다. 다음 표에서 Kafka 및 Event Hubs 간 개념을 매핑합니다.

### <a name="kafka-and-event-hub-conceptual-mapping"></a>Kafka 및 Event Hubs의 개념적 매핑

| Kafka 개념 | Event Hubs 클러스터|
| --- | --- |
| 클러스터 | 네임스페이스 |
| 항목 | 이벤트 허브 |
| 파티션 | 파티션|
| 소비자 그룹 | 소비자 그룹 |
| Offset | Offset|

### <a name="key-differences-between-kafka-and-event-hubs"></a>Kafka 및 Event Hubs의 주요 차이점

[Apache Kafka](https://kafka.apache.org/)는 선택한 어디서든 실행할 수 있는 소프트웨어인 반면 Event Hubs는 Azure Blob Storage와 유사한 클라우드 서비스입니다. 관리할 네트워크나 서버가 없으며 구성할 브로커가 없습니다. 항목이 포함된 FQDN인 네임스페이스를 만든 다음, 해당 네임스페이스 내에 항목 또는 Event Hubs를 만듭니다. Event Hubs 및 네임스페이스에 대한 자세한 내용은 [Event Hubs 기능](event-hubs-features.md#namespace)을 참조하세요. 클라우드 서비스로서 Event Hubs는 하나의 안정적인 가상 IP 주소를 엔드포인트로 사용하므로 클라이언트는 클러스터 내의 브로커 나 컴퓨터에 대해 알 필요가 없습니다. 

Event Hubs에서 확장은 구매한 처리량 단위 수에 의해 제어되며, 각 처리량 단위는 초당 1MB 또는 초당 이벤트 1000개를 수신할 수 있습니다. 기본적으로 Event Hubs는 [자동 확장](event-hubs-auto-inflate.md) 기능으로 한계에 도달할 경우 처리량 단위를 확장합니다. 이 기능은 Kafka용 Event Hubs 기능과 함께 작동합니다. 

### <a name="security-and-authentication"></a>보안 및 인증
Kafka의 이벤트 허브에서 이벤트를 게시하거나 사용할 때마다 클라이언트가 이벤트 허브 리소스에 액세스하려고 합니다. 권한 있는 엔터티를 사용하여 리소스에 액세스하도록 합니다. 클라이언트와 함께 Apache Kafka 프로토콜을 사용하는 경우 SASL 메커니즘을 사용하여 인증 및 암호화에 대한 구성을 설정할 수 있습니다. Kafka에 이벤트 허브를 사용하는 경우 TLS 암호화가 필요합니다(이벤트 허브로 전송되는 모든 데이터는 TLS 암호화). 구성 파일에서 SASL_SSL 옵션을 지정할 수 있습니다. 

Azure Event Hubs는 보안 리소스에 대한 액세스 권한을 부여하기 위한 여러 옵션을 제공합니다. 

- OAuth
- 공유 액세스 서명(SAS)

#### <a name="oauth"></a>OAuth
이벤트 허브는 **OAuth** 2.0 호환 중앙 인증 서버를 제공하는 Azure Active Directory(Azure AD)와 통합됩니다. Azure AD를 사용하면 RBAC(역할 기반 액세스 제어)를 사용하여 클라이언트 ID에 미세한 권한을 부여할 수 있습니다. 프로토콜에 대한 **SASL_SSL** 메커니즘에 대한 **OAUTHBEARER를** 지정하여 Kafka 클라이언트와 함께 이 기능을 사용할 수 있습니다. 액세스 범위 지정에 대한 RBAC 역할 및 수준에 대한 자세한 내용은 [Azure AD를 사용하여 액세스 권한 부여를](authorize-access-azure-active-directory.md)참조하세요.

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=OAUTHBEARER
sasl.jaas.config=org.apache.kafka.common.security.oauthbearer.OAuthBearerLoginModule required;
sasl.login.callback.handler.class=CustomAuthenticateCallbackHandler;
```

#### <a name="shared-access-signature-sas"></a>공유 액세스 서명(SAS)
또한 이벤트 허브는 Kafka 리소스에 대한 이벤트 허브에 대한 위임된 액세스를 위한 **공유 액세스 서명(SAS)을** 제공합니다. OAuth 2.0 토큰 기반 메커니즘을 사용하여 액세스 권한을 부여하면 SAS에 비해 뛰어난 보안과 사용 편의성을 제공합니다. 기본 제공 역할은 사용자가 유지 관리하고 관리해야 하는 ACL 기반 권한 부여의 필요성을 제거할 수도 있습니다. 프로토콜에 대한 **SASL_SSL** 메커니즘에 대한 **PLAIN을** 지정하여 Kafka 클라이언트와 함께 이 기능을 사용할 수 있습니다. 

```xml
bootstrap.servers=NAMESPACENAME.servicebus.windows.net:9093
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

#### <a name="samples"></a>샘플 
이벤트 허브를 만들고 SAS 또는 OAuth를 사용하여 액세스하는 단계별 지침이 있는 **자습서의** 경우 [빠른 시작: Kafka 프로토콜을 사용하여 이벤트 허브를 사용한 데이터 스트리밍을](event-hubs-quickstart-kafka-enabled-event-hubs.md)참조하십시오.

카프카에 대한 이벤트 허브와 OAuth를 사용하는 방법을 보여 주는 자세한 **샘플은** [GitHub의 샘플을](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/oauth)참조하십시오.

## <a name="other-event-hubs-features-available-for-kafka"></a>Kafka에 사용할 수 있는 기타 Event Hubs 기능

Kafka용 Event Hubs 기능을 사용하면 한 프로토콜로 읽고 다른 프로토콜로 쓸 수 있으므로 현재 Kafka 생산자는 Kafka를 통해 게시를 계속할 수 있으며 Azure Stream Analytics 또는 Azure Functions 같은 Event Hubs를 사용하여 판독기를 추가할 수도 있습니다. 또한 [캡처](event-hubs-capture-overview.md) 및 [지역 재해 복구](event-hubs-geo-dr.md) 같은 Event Hubs 기능은 Kafka용 Event Hubs 기능과 함께 작동합니다.

## <a name="features-that-are-not-yet-supported"></a>아직 지원되지 않는 기능 

다음은 아직 지원되지 않는 Kafka 기능 목록입니다.

*   Idempotent 생산자
*   트랜잭션
*   압축
*   크기 기반 보존
*   로그 압축
*   파티션을 기존 항목에 추가
*   HTTP Kafka API 지원
*   Kafka 스트림

## <a name="next-steps"></a>다음 단계
이 문서는 Kafka용 Event Hubs에 대한 소개를 제공합니다. 자세한 내용은 [Azure 이벤트 허브에 대한 아파치 카프카 개발자 가이드를](apache-kafka-developer-guide.md)참조하십시오.


