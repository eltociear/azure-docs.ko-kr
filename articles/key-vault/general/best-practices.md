---
title: 키 볼트 - Azure 키 볼트를 사용하는 모범 사례 | 마이크로 소프트 문서
description: 이 문서에서는 Key Vault를 사용하는 몇 가지 모범 사례에 대해 설명합니다.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-key-vault
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 03/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 923fb90f7f0e8eefec650515ed2a3b9b75d2ae77
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617912"
---
# <a name="best-practices-to-use-key-vault"></a>키 볼트를 사용하는 모범 사례

## <a name="control-access-to-your-vault"></a>볼트에 대한 액세스 제어

Azure Key Vault는 암호화 키와 비밀(예: 인증서, 연결 문자열 및 암호)을 보호하는 클라우드 서비스입니다. 이 데이터는 민감하고 업무상 중요하기 때문에 권한이 부여된 애플리케이션과 사용자만 허용하여 Key Vault에 대한 액세스를 보호해야 합니다. 이 [문서)는](secure-your-key-vault.md)키 볼트 액세스 모델에 대한 개요를 제공합니다. 여기서는 인증 및 권한 부여에 대해 설명하고 Key Vault 액세스의 보안을 유지하는 방법을 설명합니다.

볼트에 대한 액세스를 제어하는 동안 제안은 다음과 같습니다.
1. 구독, 리소스 그룹 및 RBAC(키 볼트)에 대한 액세스 잠금
2. 모든 자격 증명 모음에 대한 액세스 정책 만들기
3. 최소 권한 액세스 보안 주체를 사용하여 액세스 권한 부여
4. 방화벽 및 [VNET 서비스 끝점](overview-vnet-service-endpoints.md)켜기)

## <a name="use-separate-key-vault"></a>별도의 키 볼트 사용

환경당 응용 프로그램(개발, 사전 프로덕션 및 프로덕션)당 볼트를 사용하는 것이 좋습니다. 이렇게 하면 환경 간에 비밀을 공유하지 않고 위반 시 위협을 줄일 수 있습니다.

## <a name="backup"></a>Backup

Vault 내의 객체를 업데이트/삭제/생성할 때 [볼트를](https://blogs.technet.microsoft.com/kv/2018/07/20/announcing-backup-and-restore-of-keys-secrets-and-certificates/) 정기적으로 백업해야 합니다.

## <a name="turn-on-logging"></a>로깅 켜기

[로깅 켜기)](logging.md) 볼트에 대한. 또한 경고를 설정합니다.

## <a name="turn-on-recovery-options"></a>복구 옵션 켜기

1. 소프트 [삭제를](overview-soft-delete.md)켭니다.
2. 소프트 삭제가 켜져 있더라도 비밀 / 볼트의 강제 삭제를 방지하려면 제거 보호를 켭니다.
