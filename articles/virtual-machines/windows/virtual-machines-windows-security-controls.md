---
title: Azure Windows Virtual Machines에 대 한 보안 제어
description: Azure Windows Virtual Machines 평가를 위한 보안 컨트롤의 검사 목록
ms.service: virtual-machines
ms.subservice: security
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: ac1ed9ac25d65d0391175fc6d43b48048da74926
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82101589"
---
# <a name="security-controls-for-windows-virtual-machines"></a>Windows Virtual Machines에 대 한 보안 제어

이 문서에서는 Windows Virtual Machines에 기본 제공 되는 보안 컨트롤을 설명 합니다.

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

## <a name="network"></a>네트워크

| 보안 제어 | 예/아니요 | 메모 |
|---|---|--|
| 서비스 엔드포인트 지원| 예 | |
| VNet 삽입 지원| 예 | |
| 네트워크 격리 및 방화벽 지원| 예 |  |
| 강제 터널링 지원| 예 | [Azure Resource Manager 배포 모델을 사용 하 여 강제 터널링 구성을](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm)참조 하세요. |

## <a name="monitoring--logging"></a>& 로깅 모니터링

| 보안 제어 | 예/아니요 | 메모|
|---|---|--|
| Azure 모니터링 지원 (Log analytics, App insights 등)| 예 | [Azure에서 Windows 가상 머신을 모니터링 하 고 업데이트](tutorial-monitoring.md)합니다. |
| 제어 및 관리 평면 로깅 및 감사| 예 |  |
| 데이터 평면 로깅 및 감사 | 예 |  |

## <a name="identity"></a>ID

| 보안 제어 | 예/아니요 | 메모|
|---|---|--|
| 인증| 예 |  |
| 권한 부여| 예 |  |

## <a name="data-protection"></a>데이터 보호

| 보안 제어 | 예/아니요 | 메모 |
|---|---|--|
| 미사용 서버 쪽 암호화: Microsoft 관리 키 | 예 | [WINDOWS VM에서 가상 디스크 암호화](/azure/virtual-machines/windows/disk-encryption-overview)를 참조 하세요. |
| 전송 중 암호화 (예: Express 경로 암호화, VNet 암호화 및 VNet-VNet 암호화)| 예 | Azure Virtual Machines는 [express](/azure/expressroute) 경로 및 VNet 암호화를 지원 합니다. [Vm의 전송 중 암호화를](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)참조 하세요. |
| 미사용 서버 쪽 암호화: 고객 관리 키 (BYOK) | 예 | 고객 관리 키는 지원 되는 Azure 암호화 시나리오입니다. [Azure 암호화 개요](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)를 참조 하세요.|
| 열 수준 암호화 (Azure Data Services)| 해당 없음 | |
| API 호출 암호화| 예 | HTTPS 및 TLS를 통해 |



## <a name="configuration-management"></a>구성 관리

| 보안 제어 | 예/아니요 | 메모|
|---|---|--|
| 구성 관리 지원 (구성의 버전 관리 등)| 예 |  | 

## <a name="next-steps"></a>다음 단계

- [Azure 서비스에서 기본 제공 되는 보안 컨트롤](../../security/fundamentals/security-controls.md)에 대해 자세히 알아보세요.
