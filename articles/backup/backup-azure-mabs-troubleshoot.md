---
title: Azure Backup Server 문제 해결
description: Azure Backup Server 설치, 등록 및 애플리케이션 워크로드의 백업 및 복원 문제를 해결합니다.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 07/05/2019
ms.openlocfilehash: 7a1cac63ba6497b8580c83fe2b666b020701283a
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81688044"
---
# <a name="troubleshoot-azure-backup-server"></a>Azure Backup Server 문제 해결

다음 표에 나열된 정보를 참조하여 Azure Backup Server 사용 중 발생하는 오류를 해결합니다.

## <a name="basic-troubleshooting"></a>기본 문제 해결

Microsoft Azure 백업 서버(MABS) 문제 해결을 시작하기 전에 아래 유효성 검사를 수행하는 것이 좋습니다.

- [Microsoft Azure 복구 서비스(MARS) 에이전트가 최신 상태인지 확인](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [MARS 에이전트와 Azure 간에 네트워크 연결이 있는지 확인](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup)
- Microsoft Azure Recovery Services가 실행 중인지 확인(서비스 콘솔에서) 필요한 경우 작업을 다시 시작하고 다시 시도하십시오.
- [스크래치 폴더 위치에서 5~10% 볼륨 여유 공간을 사용할 수 있는지 확인](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#whats-the-minimum-size-requirement-for-the-cache-folder)
- 등록에 실패한 경우 Azure Backup Server를 설치하려는 서버가 다른 자격 증명 모음에 아직 등록되지 않았는지 확인합니다.
- 푸시 설치에 실패할 경우 이미 DPM 에이전트가 있는지 확인합니다. 있는 경우 해당 에이전트를 제거하고 다시 설치
- [다른 프로세스 또는 바이러스 백신 소프트웨어가 Azure Backup을 방해하는지 확인](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-slow-backup-performance-issue#cause-another-process-or-antivirus-software-interfering-with-azure-backup)<br>
- SQL 에이전트 서비스가 실행 중이고 MABS 서버에서 자동으로 설정되어 있는지 확인합니다.<br>

## <a name="invalid-vault-credentials-provided"></a>잘못된 자격 증명 모음이 제공되었습니다.

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| 자격 증명 모음에 등록 | 잘못된 자격 증명 모음이 제공되었습니다. 파일이 손상되었거나 복구 서비스와 연결된 최신 자격 증명이 없습니다. | 권장 작업: <br> <ul><li> 자격 증명 모음에서 최신 자격 증명 파일을 다운로드하고 다시 시도합니다. <br>또는</li> <li> 위의 작업이 효과가 없으면 자격 증명을 다른 로컬 디렉터리에 다운로드하거나 새 자격 증명 모음을 만듭니다. <br>또는</li> <li> [이 문서에](https://docs.microsoft.com/azure/backup/backup-azure-mars-troubleshoot#invalid-vault-credentials-provided)설명된 대로 날짜 및 시간 설정을 업데이트해 보십시오. <br>또는</li> <li> \windows\temp에 있는 파일 수가 65,000개를 초과하는지 확인합니다. 오래된 파일을 다른 위치에 이동하거나 임시 폴더의 항목을 삭제합니다. <br>또는</li> <li> 인증서의 상태를 확인합니다. <br> a. 제어판에서 **컴퓨터 인증서 관리**를 엽니다. <br> b. **개인** 노드 및 해당 하위 노드 **인증서를 확장합니다.**<br> 다.  인증서를 제거 **윈도우 Azure 도구**. <br> d. Azure Backup 클라이언트에서 등록을 다시 시도합니다. <br> 또는 </li> <li> 그룹 정책이 구현되어 있는지 확인합니다. </li></ul> |

## <a name="replica-is-inconsistent"></a>복제본이 불일치

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| Backup | 복제본이 불일치 | 보호 그룹 마법사의 자동 일관성 검사 옵션이 켜져 있는지 확인합니다. 복제본 불일치 및 관련 제안의 원인에 대한 자세한 내용은 [복제본이 일치하지 않는](https://docs.microsoft.com/previous-versions/system-center/data-protection-manager-2006/cc161593(v=technet.10))문서를 참조하십시오.<br> <ol><li> 시스템 상태/BMR 백업의 경우 보호된 서버에 Windows Server 백업이 설치되어 있는지 확인합니다.</li><li> DPM/Microsoft Azure Backup Server의 DPM 스토리지 풀에서 공간 관련 문제를 확인하고 필요에 따라 스토리지를 할당합니다.</li><li> 보호된 서버에서 볼륨 섀도 복사본 서비스의 상태를 확인합니다. 사용할 수 없는 상태이면 수동으로 시작하도록 설정합니다. 서버에서 서비스를 시작합니다. 그런 다음, DPM/Microsoft Azure Backup Server 콘솔로 다시 돌아가 일관성 검사 작업과 동기화를 시작합니다.</li></ol>|

## <a name="online-recovery-point-creation-failed"></a>온라인 복구 지점 생성 실패

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| Backup | 온라인 복구 지점 생성 실패 | **오류 메시지**: Windows Azure 백업 에이전트가 선택한 볼륨의 스냅숏을 만들 수 없습니다. <br> **해결:** 복제본 및 복구 지점 볼륨의 공간을 늘려보십시오.<br> <br> **오류 메시지**: Windows Azure 백업 에이전트가 OBEngine 서비스에 연결할 수 없습니다. <br> **해결 방법**: 컴퓨터에서 실행 중인 서비스 목록에 OBEngine이 있는지 확인합니다. OBEngine 서비스가 실행 중이 아닌 경우 “net start OBEngine” 명령을 사용하여 OBEngine 서비스를 시작합니다. <br> <br> **오류 메시지**: 이 서버의 암호화 암호가 설정되지 않았습니다. 암호화에 사용할 암호를 구성하세요. <br> **해결:** 암호화 암호를 구성해 보십시오. 실패하면 다음 단계를 수행합니다. <br> <ol><li>스크래치 위치가 있는지 확인합니다. 레지스트리 **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config**에 언급된 위치에 **ScratchLocation** 이름이 있어야 합니다.</li><li> 스크래치 위치가 존재하는 경우 이전 암호를 사용하여 다시 등록합니다. *암호화에 사용할 암호를 구성할 때마다 안전한 위치에 보관합니다.*</li><ol>|

## <a name="the-original-and-external-dpm-servers-must-be-registered-to-the-same-vault"></a>원본 및 외부 DPM 서버는 동일한 자격 증명 모음에 등록해야 합니다.

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| 복원 | **오류 코드**: CBPServer등록볼트돈트매치현재/볼트 자격 증명 오류: 100110 <br/> <br/>**오류 메시지**: 원본 및 외부 DPM 서버를 동일한 자격 증명 모음에 등록해야 합니다. | **원인**: 이 문제는 외부 DPM 복구 옵션을 사용하여 원래 서버에서 대체 서버로 파일을 복원하려고 할 때 발생하며 복구 중인 서버와 데이터가 백업된 원래 서버가 동일한 복구 서비스 자격 증명 모음과 연결되지 않은 경우 발생합니다.<br/> <br/>**해결 해결** 이 문제를 해결하려면 원본 서버와 대체 서버가 모두 동일한 자격 증명 모음에 등록되어 있는지 확인합니다.|

## <a name="online-recovery-point-creation-jobs-for-vmware-vm-fail"></a>VMware VM에 대한 온라인 복구 지점 생성 작업 실패

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| Backup | VMware VM에 대한 온라인 복구 지점 생성 작업이 실패합니다. DPM이 ChangeTracking 정보를 가져오는 동안 VMware에서 오류가 발생했습니다. 오류 코드 - 파일FaultFault (ID 33621) |  <ol><li> 영향을 받는 VM에 대해 VMWare에서 CTK를 다시 설정합니다.</li> <li>독립 디스크가 VMware에 준비되어 있지 않은지 확인합니다.</li> <li>영향을 받는 VM에 대한 보호를 중지하고 **새로 고침** 단추를 통해 다시 보호합니다. </li><li>영향을 받는 VM에 대해 CC를 실행합니다.</li></ol>|

## <a name="the-agent-operation-failed-because-of-a-communication-error-with-the-dpm-agent-coordinator-service-on-the-server"></a>서버에서 DPM 에이전트 코디네이터 서비스와 통신 오류로 인해 에이전트 작업에 실패했습니다.

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| 보호된 서버에 에이전트 푸시 | \<ServerName>에서 DPM 에이전트 코디네이터 서비스와 통신 오류로 인해 에이전트 작업에 실패했습니다. | **제품에 표시된 권장 작업이 효과가 없으면 다음 단계를 수행합니다.** <ul><li> 신뢰할 수 없는 도메인에서 컴퓨터를 연결하는 경우 [다음 단계를](https://docs.microsoft.com/system-center/dpm/back-up-machines-in-workgroups-and-untrusted-domains?view=sc-dpm-2019)따르십시오. <br> 또는 </li><li> 신뢰할 수 있는 도메인에서 컴퓨터를 연결하는 경우 [이 블로그에](https://techcommunity.microsoft.com/t5/system-center-blog/data-protection-manager-agent-network-troubleshooting/ba-p/344726)설명된 단계를 사용하여 문제를 해결합니다. <br>또는</li><li> 문제 해결 단계로 바이러스 백신을 사용하지 않도록 설정해 봅니다. 문제가 해결되면 [이 문서](https://docs.microsoft.com/system-center/dpm/run-antivirus-server?view=sc-dpm-2019)에 제안된 대로 바이러스 백신 설정을 수정합니다.</li></ul> |

## <a name="setup-could-not-update-registry-metadata"></a>설치 프로그램이 레지스트리 메타데이터를 업데이트할 수 없습니다.

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
|-----------|---------------|------------|
|설치 | 설치 프로그램이 레지스트리 메타데이터를 업데이트할 수 없습니다. 이 업데이트 오류로 인해 스토리지가 지나치게 사용될 수 있습니다. 이 업데이트를 방지하려면 ReFS 트리밍 레지스트리 항목을 참조하십시오. | 레지스트리 키 를 조정 **하는 SYSTEM\현재 제어설정\제어\파일 시스템\RefsEnableInline.** Dword 값을 1로 설정합니다. |
|설치 | 설치 프로그램이 레지스트리 메타데이터를 업데이트할 수 없습니다. 이 업데이트 오류로 인해 스토리지가 지나치게 사용될 수 있습니다. 이 문제를 방지하려면 Volume SnapOptimization 레지스트리 항목을 업데이트합니다. | 레지스트리 키 **소프트웨어\Microsoft 데이터 보호 관리자\구성\VolSnap최적화\쓰기 ID빈** 문자열 값을 만듭니다. |

## <a name="registration-and-agent-related-issues"></a>등록 및 에이전트 관련 문제

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| 보호된 서버에 에이전트 푸시 | 서버에 대해 지정된 자격 증명이 잘못되었습니다. | **제품에 표시된 권장 작업이 작동하지 않으면 다음 단계를 수행하십시오.** <br> [이 문서에](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-2019)지정된 대로 프로덕션 서버에 보호 에이전트를 수동으로 설치하십시오.|
| Azure Backup Agent가 Azure Backup 서비스에 연결할 수 없습니다(ID: 100050). | Azure Backup Agent가 Azure Backup 서비스에 연결할 수 없습니다. | **제품에 표시된 권장 작업이 작동하지 않으면 다음 단계를 수행하십시오.** <br>1. 상승 된 프롬프트에서 다음 명령을 실행 : **psexec -i -s "c:\\프로그램 파일\인터넷 익스플로러\iexplore.exe**. 그러면 Internet Explorer 창이 열립니다. <br/> 2. **도구** > **인터넷 옵션** > **연결** > **LAN 설정으로**이동합니다. <br/> 3. 프록시 서버를 사용하도록 설정을 변경합니다. 그런 다음 프록시 서버 세부 정보를 제공합니다.<br/> 4. 컴퓨터가 인터넷 액세스가 제한된 경우 컴퓨터 또는 프록시의 방화벽 설정에서 이러한 [URL](install-mars-agent.md#verify-internet-access) 및 [IP 주소를](install-mars-agent.md#verify-internet-access)허용해야 합니다.|
| Azure Backup Agent 설치 실패 | Microsoft Azure Recovery Services 설치에 실패했습니다. 시스템에 Microsoft Azure Recovery Services 설치로 인한 모든 변경 사항은 롤백되었습니다. (ID: 4024) | Azure 에이전트를 수동으로 설치합니다.

## <a name="configuring-protection-group"></a>보호 그룹 구성

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| 보호 그룹 구성 | DPM은 보호된 컴퓨터(보호된 컴퓨터 이름)에 애플리케이션 구성 요소를 열거할 수 없습니다. | 관련 데이터 원본/구성 요소 수준의 보호 그룹 구성 UI 화면에서 **새로 고침**을 선택합니다. |
| 보호 그룹 구성 | 보호를 구성할 수 없음 | 보호된 서버가 SQL Server인 경우 [이 문서](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh757977(v=sc.12))에 설명된 대로 보호된 컴퓨터에 대한 시스템 계정(NTAuthority\System)에 sysadmin 역할 권한이 제공되었는지 확인합니다.
| 보호 그룹 구성 | 이 보호 그룹에 대한 스토리지 풀에 여유 공간이 부족합니다. | 스토리지 풀에 추가된 디스크는 [파티션을 포함하지 않아야 합니다](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12)). 디스크에 있는 기존 볼륨을 모두 삭제합니다. 그런 다음 스토리지 풀에 추가합니다.|
| 정책 변경 |백업 정책을 수정할 수 없습니다. 오류: 내부 서비스 오류 [0x29834]로 인해 현재 작업이 실패했습니다. 잠시 후 작업을 다시 시도하세요. 문제가 지속되면 Microsoft 지원에 문의하세요. | **원인:**<br/>이 오류는 보안 설정이 활성화된 경우, 위에서 지정된 최소값 미만으로 보존 범위를 줄이려고 하는 경우 및 지원되지 않는 버전을 사용하는 경우 등 세 가지 조건에서 발생합니다. (지원되지 않는 버전은 Microsoft Azure Backup Server 버전 2.0.9052 및 Azure Backup Server 업데이트 1 미만입니다.) <br/>**권장 조치:**<br/> 정책 관련 업데이트를 계속하려면 보존 기간을 지정된 최소 보존 기간보다 길게 설정합니다. (최소 보존 기간은 일별 백업의 경우 7일, 주별 백업의 경우 4주, 월별 백업의 경우 3개월, 연도별 백업의 경우 1년입니다.) <br><br>그 외에도 권장되는 방법은 백업 에이전트 및 Azure Backup Server를 업데이트하여 모든 보안 업데이트를 적용하는 것입니다. |

## <a name="backup"></a>Backup

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| Backup | 작업을 실행하는 동안 예기치 않은 오류가 발생했습니다. 디바이스가 준비되지 않았습니다. | **제품에 표시된 권장 작업이 효과가 없으면 다음 단계를 수행합니다.** <br> <ul><li>섀도 복사본 스토리지 공간을 보호 그룹의 항목에서 무제한으로 설정하고 일관성 검사를 실행합니다.<br></li> 또는 <li>기존 보호 그룹을 삭제하고 여러 개의 새 그룹을 만들어 봅니다. 새 보호 그룹 각각에는 개별 항목이 있어야 합니다.</li></ul> |
| Backup | 시스템 상태만 백업하는 경우 보호된 컴퓨터에 시스템 상태 백업을 저장하기에 충분한 여유 공간이 있는지 확인합니다. | <ol><li>보호된 컴퓨터에 Windows Server 백업이 설치되어 있는지 확인합니다.</li><li>보호된 컴퓨터에 시스템 상태를 저장할 공간이 충분히 있는지 확인합니다. 이를 간단히 확인하려면 보호된 컴퓨터로 이동하고 Windows Server 백업을 연 다음, 선택 사항을 클릭하고 BMR을 선택합니다. 그러면 UI에 얼마나 많은 공간이 필요한지 표시됩니다. **열기 WSB** > **로컬 백업** > **일정** > **선택 백업 구성** > **전체 서버** (크기가 표시 됩니다). 검증에 이 크기를 사용합니다.</li></ol>
| Backup | BMR에 대한 백업 실패 | BMR 크기가 크면 일부 애플리케이션 파일을 OS 드라이브로 이동한 후 다시 시도합니다. |
| Backup | 새 Microsoft Azure 백업 서버에서 VM웨어 VM을 다시 보호하는 옵션은 추가할 수 있는 옵션이 표시되지 않습니다. | VMware 속성이 더 이상 사용되지 않는 이전 Microsoft Azure Backup Server 인스턴스에 지정되어 있습니다. 이 문제를 해결하려면:<br><ol><li>VCenter(SC-VMM에 해당)에서 **요약** 탭, **사용자 지정 특성**으로 이동합니다.</li>  <li>**DPMServer** 값에서 이전 Microsoft Azure Backup Server 이름을 삭제합니다.</li>  <li>새 Microsoft Azure Backup Server로 돌아가서 PG를 수정합니다.  **새로 고침** 단추를 선택하면 VM을 보호에 추가할 수 있도록 확인란이 표시됩니다.</li></ol> |
| Backup | 파일/공유 폴더에 액세스하는 동안 오류가 발생했습니다. | 이 문서에서 제안한 대로 바이러스 백신 설정을 수정해 [보십시오DPM 서버에서 바이러스 백신 소프트웨어 실행.](https://docs.microsoft.com/system-center/dpm/run-antivirus-server?view=sc-dpm-2019)|

## <a name="change-passphrase"></a>암호 변경

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| 암호 변경 |입력한 보안 PIN이 잘못되었습니다. 이 작업을 완료하려면 올바른 보안 PIN을 입력합니다. |**원인:**<br/> 이 오류는 중요한 작업(예: 암호 변경)을 수행하는 동안 잘못되거나 만료된 보안 PIN을 입력하는 경우 발생합니다. <br/>**권장 조치:**<br/> 작업을 완료하려면 유효한 보안 PIN을 입력해야 합니다. PIN을 얻으려면 Azure Portal에 로그인한 다음, Recovery Services 자격 증명 모음으로 이동합니다. 그런 다음 **설정** > **속성으로** > **이동하여 보안 PIN을 생성합니다.** 이 PIN을 사용하여 암호를 변경합니다. |
| 암호 변경 |작업이 실패했습니다. ID: 120002 |**원인:**<br/>이 오류는 보안 설정이 활성화된 경우, 또는 지원되지 않는 버전을 사용하는 중에 암호를 변경하려고 하는 경우 발생합니다.<br/>**권장 조치:**<br/> 암호를 변경하려면 먼저 백업 에이전트를 최소 버전인 2.0.9052로 업데이트해야 합니다. 또한 Azure Backup Server도 최소 업데이트 1로 업데이트한 다음, 유효한 보안 PIN을 입력해야 합니다. PIN을 얻으려면 Azure Portal에 로그인한 다음, Recovery Services 자격 증명 모음으로 이동합니다. 그런 다음 **설정** > **속성으로** > **이동하여 보안 PIN을 생성합니다.** 이 PIN을 사용하여 암호를 변경합니다. |

## <a name="configure-email-notifications"></a>전자 메일 알림 구성

| 작업(Operation) | 오류 세부 정보 | 해결 방법 |
| --- | --- | --- |
| Office 365 계정을 사용하여 메일 알림 설정 |오류 ID: 2013| **원인:**<br> Office 365 계정을 사용하려고 합니다. <br>**권장 조치:**<ol><li> 가장 먼저 확인해야 할 것은 DPM 서버에 대해 "수신 커넥터에서 익명 릴레이 허용"이 Exchange에서 설정되어 있다는 것입니다. 이를 구성하는 방법에 대한 자세한 내용은 [수신 커넥터에서 익명 릴레이 허용](https://docs.microsoft.com/exchange/mail-flow/connectors/allow-anonymous-relay?view=exchserver-2019)을 참조하십시오.</li> <li> 내부 SMTP 릴레이를 사용할 수 없고 Office 365 서버를 사용하여 설정해야 할 경우 IIS가 릴레이가 되도록 설정할 수 있습니다. [IIS를 사용하여 SMTP를 O365로 릴레이하도록](https://docs.microsoft.com/exchange/mail-flow/test-smtp-with-telnet?view=exchserver-2019)DPM 서버를 구성합니다.<br><br>  도메인\사용자가 아닌\@사용자 domain.com *not* 형식을 사용해야 합니다.<br><br><li>DPM에서 로컬 서버 이름을 SMTP 서버(포트 587)로 사용하도록 지정합니다. 그런 다음, 메일을 가져올 사용자 메일을 가리킵니다.<li> DPM SMTP 설정 페이지의 사용자 이름과 암호는 DPM이 사용 설정되어 있는 도메인 계정용이어야 합니다. </li><br> SMTP 서버 주소를 변경할 경우 새 설정을 변경하고 설정 상자를 닫은 후 다시 열어 새 값이 적용되었는지 확인하세요.  단순히 변경 및 테스트한다고 해서 새 설정이 적용되는 것은 아니므로, 이 방법으로 테스트하는 것이 좋습니다.<br><br>이 과정 중 언제든지 DPM 콘솔을 닫고 다음 레지스트리 키를 편집하여 이러한 설정을 지울 수 있습니다. **HKLM\SOFTWARE\Microsoft\Microsoft Data Protection Manager\Notification\ <br/> SMTPPassword 및 SMTPUserName 키를 삭제합니다.** 다시 시작할 때 UI에 다시 추가할 수 있습니다.

## <a name="common-issues"></a>일반적인 문제

이 섹션에서는 Azure Backup Server를 사용하는 동안 발생할 수 있는 일반적인 오류를 다룹니다.

### <a name="cbpsourcesnapshotfailedreplicamissingorinvalid"></a>CBPSourceSnapshotFailedReplicaMissingOrInvalid

오류 메시지 | 권장 조치 |
-- | --
디스크 백업 복제본이 잘못되었거나 누락되어 백업에 실패했습니다. | 이 문제를 해결하려면 아래 단계를 확인하고 작업을 다시 시도하십시오. <br/> 1. 디스크 복구 지점 만들기<br/> 2. 데이터 소스에서 일관성 검사 실행 <br/> 3. 데이터 원본 보호 중지 한 다음이 데이터 원본에 대 한 보호를 다시 구성

### <a name="cbpsourcesnapshotfailedreplicametadatainvalid"></a>CBPSourceSnapshotFailedReplicaMetadataInvalid

오류 메시지 | 권장 조치 |
-- | --
복제본의 메타데이터가 잘못되어 원본 볼륨 스냅숏이 실패했습니다. | 이 데이터 원본의 디스크 복구 지점을 만들고 온라인 백업을 다시 시도합니다.

### <a name="cbpsourcesnapshotfailedreplicainconsistent"></a>CBPSourceSnapshotFailedReplicaInconsistent

오류 메시지 | 권장 조치 |
-- | --
일관되지 않은 데이터 원본 복제본으로 인해 원본 볼륨 스냅숏이 실패했습니다. | 이 데이터 원본에서 일관성 검사를 실행하고 다시 시도하십시오.

### <a name="cbpsourcesnapshotfailedreplicacloningissue"></a>CBPSourceSnapshotFailedReplicaCloningIssue

오류 메시지 | 권장 조치 |
-- | --
디스크 백업 복제본을 복제할 수 없어 백업이 실패했습니다.| 온라인 백업 중에 이전의 모든 디스크 백업 복제파일(.vhdx)이 마운트 해제되지 않고 디스크 백업이 진행되지 않았는지 확인합니다.
