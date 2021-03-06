---
title: Azure VM을 사용 하 고 백업 오류 문제 해결
description: 이 문서에서는 Azure 가상 시스템의 백업 및 복원으로 인해 발생한 오류를 해결하는 방법을 알아봅니다.
ms.reviewer: srinathv
ms.topic: troubleshooting
ms.date: 08/30/2019
ms.openlocfilehash: 019c27b1f7e8560c86252aaf2ed1fb79df2439fa
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677352"
---
# <a name="troubleshooting-backup-failures-on-azure-virtual-machines"></a>Azure 가상 머신에서 백업 오류 문제 해결

아래 나열된 정보와 함께 Azure Backup을 사용하는 동안 발생한 오류를 해결할 수 있습니다.

## <a name="backup"></a>Backup

이 섹션에서는 Azure 가상 시스템의 백업 작업 실패를 다룹니다.

### <a name="basic-troubleshooting"></a>기본 문제 해결

* VM 에이전트(WA 에이전트)가 [최신 버전인지](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#install-the-vm-agent)확인합니다.
* Windows 또는 Linux VM OS 버전이 지원되는지 확인하려면 [IaaS VM 백업 지원 매트릭스를](https://docs.microsoft.com/azure/backup/backup-support-matrix-iaas)참조하십시오.
* 다른 백업 서비스가 실행되고 있지 않은지 확인합니다.
  * 스냅숏 확장 문제가 없는지 확인하려면 [확장을 제거하여 강제로 다시 로드한 다음 백업을 다시 시도합니다.](https://docs.microsoft.com/azure/backup/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout)
* VM에 인터넷 연결이 있는지 확인합니다.
  * 다른 백업 서비스가 실행되고 있지 않은지 확인합니다.
* 에서 Windows **Azure 게스트 에이전트** 서비스가 **실행 중인지**확인합니다. `Services.msc` Windows **Azure 게스트 에이전트** 서비스가 없는 경우 [복구 서비스 자격 증명 모음에서 Azure VM 백업에서](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#install-the-vm-agent)설치합니다.
* **이벤트 로그에는** 다른 백업 제품(예: Windows Server 백업)의 백업 실패가 표시될 수 있으며 Azure 백업으로 인한 것이 아닙니다. 다음 단계를 사용하여 Azure Backup에서 문제가 있는지 확인합니다.
  * 이벤트 소스 또는 메시지에 **백업** 항목에 오류가 있는 경우 Azure IaaS VM 백업 백업이 성공했는지 여부와 원하는 스냅숏 유형으로 복원 지점을 만들었는지 확인합니다.
  * Azure Backup이 작동하는 경우 다른 백업 솔루션에서 문제가 발생할 수 있습니다.
  * 다음은 Azure 백업이 잘 작동했지만 "Windows 서버 백업"이 실패한 이벤트 뷰어 오류 517의 예입니다.<br>
    ![Windows 서버 백업 실패](media/backup-azure-vms-troubleshoot/windows-server-backup-failing.png)
  * Azure 백업에 실패 하는 경우 다음 이 문서에서 일반적인 VM 백업 오류 섹션에서 해당 오류 코드를 찾습니다.

## <a name="common-issues"></a>일반적인 문제

다음은 Azure 가상 시스템의 백업 실패와 관련된 일반적인 문제입니다.

## <a name="copyingvhdsfrombackupvaulttakinglongtime---copying-backed-up-data-from-vault-timed-out"></a>복사VHDsFromBackUpVaultTaking오랜 시간 - 시간 만료 된 볼트에서 백업 된 데이터를 복사

오류 코드: 복사VHDsFrom백백볼트복용긴시간 <br/>
오류 메시지: 시간 만료된 볼트에서 백업된 데이터를 복사

일시적인 저장소 오류 또는 백업 서비스가 시간 지정 기간 내에 데이터를 볼트로 전송하기 위한 저장소 계정 IOPS가 부족하여 발생할 수 있습니다. 이러한 [모범 사례를](backup-azure-vms-introduction.md#best-practices) 사용하여 VM 백업을 구성하고 백업 작업을 다시 시도합니다.

## <a name="usererrorvmnotindesirablestate---vm-is-not-in-a-state-that-allows-backups"></a>UserErrorVmNotIn바람직한상태 - VM이 백업을 허용하는 상태가 아닙니다.

오류 코드: 사용자 오류VmNotNotNotNot바람직한 상태 <br/>
오류 메시지: VM이 백업을 허용하는 상태가 아닙니다.<br/>

VM이 실패한 상태에 있기 때문에 백업 작업이 실패했습니다. 백업이 성공하려면 VM 상태가 실행 중, 중지됨 또는 중지됨(할당 취소)이어야 합니다.

* VM이 **실행 중**에서 **종료** 상태로 전환되고 있으면 상태가 변경될 때까지 기다립니다. 그런 다음, 백업 작업을 트리거합니다.
* VM이 Linux 에이전트이고 Security-Enhanced Linux 커널 모듈을 사용하는 경우 보안 정책에서 Azure Linux 에이전트 경로 **/var/lib/waagent**를 제외하여 백업 확장이 설치되도록 합니다.

## <a name="usererrorfsfreezefailed---failed-to-freeze-one-or-more-mount-points-of-the-vm-to-take-a-file-system-consistent-snapshot"></a>UserErrorFsFreezeFailed 실패 - 파일 시스템 일관된 스냅숏을 찍기 위해 VM의 하나 이상의 마운트 지점을 고정하지 못했습니다.

오류 코드: 사용자 오류 FsFreeze실패 <br/>
오류 메시지: 파일 시스템 일관된 스냅숏을 찍기 위해 VM의 하나 이상의 마운트 지점을 고정하지 못했습니다.

* **umount** 명령을 사용하여 파일 시스템 상태가 정리되지 않은 장치의 마운트를 해제합니다.
* **fsck** 명령을 사용하여 이러한 장치에서 파일 시스템 일관성 검사를 실행합니다.
* 장치를 다시 마운트하고 백업 작업을 다시 시도합니다.</ol>

## <a name="extensionsnapshotfailedcom--extensioninstallationfailedcom--extensioninstallationfailedmdtc---extension-installationoperation-failed-due-to-a-com-error"></a>확장스냅샷실패COM / 확장설치실패콤 / 확장설치실패MDTC - COM+ 오류로 인해 확장 설치/작동실패

오류 코드: 확장스냅샷 실패COM <br/>
오류 메시지: COM+ 오류로 인해 스냅숏 작업이 실패했습니다.

오류 코드: 확장설치실패COM  <br/>
오류 메시지: COM+ 오류로 인해 확장 설치/작업이 실패했습니다.

오류 코드: 확장설치실패MDTC <br/>
오류 메시지: "COM+가 Microsoft 분산 트랜잭션 코디네이터와 대화할 수 없습니다. <br/>

Windows 서비스 **COM+ 시스템** 응용 프로그램에 문제가 있어 백업 작업이 실패했습니다.  이 문제를 해결하려면 다음 단계를 따릅니다.

* Windows 서비스 **COM+ 시스템 응용 프로그램을** 시작/다시 시작해 보십시오(높은 명령 프롬프트에서 - 순 시작 **COMSysApp).**
* **분산 트랜잭션 코디네이터** 서비스가 **네트워크 서비스** 계정으로 실행되고 있는지 확인합니다. 그렇지 않은 경우 네트워크 **서비스** 계정으로 실행하도록 변경하고 **COM+ 시스템 응용 프로그램을**다시 시작합니다.
* 서비스를 다시 시작할 수 없는 경우 다음 단계에 따라 **분산 트랜잭션 코디네이터** 서비스를 다시 설치하십시오.
  * MSDTC 서비스를 중지합니다.
  * 명령 프롬프트(cmd)를 엽니다.
  * 실행 명령 "msdtc -제거"
  * 실행 명령 "msdtc -설치"
  * MSDTC 서비스를 시작합니다.
* **COM+ 시스템 애플리케이션** Windows 서비스를 시작합니다. **COM+ 시스템 애플리케이션**이 시작되면 Azure Portal에서 백업 작업을 트리거합니다.</ol>

## <a name="extensionfailedvsswriterinbadstate---snapshot-operation-failed-because-vss-writers-were-in-a-bad-state"></a>확장실패VssWriterInBadState - VSS 기록기가 잘못된 상태이기 때문에 스냅샷 작업이 실패했습니다.

오류 코드: 확장 실패한 vssWriterInBadState <br/>
오류 메시지: VSS 기록기가 잘못된 상태이기 때문에 스냅숏 작업이 실패했습니다.

잘못된 상태의 VSS 기록기를 다시 시작합니다. 관리자 권한의 명령 프롬프트에서 ```vssadmin list writers```를 실행합니다. 출력에는 모든 VSS 기록기와 해당 상태가 포함됩니다. **[1] 안정** 상태가 아닌 모든 VSS 기록기는 관리자 권한 명령 프롬프트에서 다음 명령을 실행하여 다시 시작합니다.

* ```net stop serviceName```
* ```net start serviceName```

## <a name="extensionconfigparsingfailure--failure-in-parsing-the-config-for-the-backup-extension"></a>확장ConfigParsing실패 - 백업 확장에 대한 구성을 구문 분석하는 데 실패

오류 코드: 확장구성파싱실패<br/>
오류 메시지: 백업 확장에 대한 구성을 구문 분석하는 데 실패합니다.

이 오류는 **MachineKeys** 디렉터리: **%systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**에 대한 권한 변경으로 인해 발생합니다.
다음 명령을 실행하고 **MachineKeys** 디렉터리에서 사용 권한이 기본**icacls %systemdrive%\programdata\microsoft\crypto\rsa\machinekeys**권한인지 확인합니다.

기본 권한은 다음과 같습니다.

* 모두: (R, W)
* 빌트인\관리자: (F)

**MachineKeys** 디렉터리에 대해 기본값과 다른 권한이 표시되면 아래 단계에 따라 권한을 수정하고, 인증서를 삭제한 후 백업을 트리거합니다.

1. **MachineKeys** 디렉터리에 대한 권한을 수정합니다. 디렉터리에서 탐색기 보안 속성 및 고급 보안 설정을 사용하여 권한을 기본값으로 다시 설정합니다. 디렉터리에서 기본값을 제외한 모든 사용자 개체를 제거하고 **Everyone** 사용 권한이 다음과 같은 특수한 액세스 권한을 갖는지 확인합니다.

   * 폴더 나열/데이터 읽기
   * 특성 읽기
   * 확장된 특성 읽기
   * 파일 만들기/데이터 쓰기
   * 폴더 만들기/데이터 추가
   * 특성 쓰기
   * 확장된 특성 쓰기
   * 읽기 권한
2. 발급 된 모든 인증서를 삭제 합니다 어디 **클래식** 배포 모델 또는 **Windows Azure CRP 인증서 생성기:**

   * [로컬 머신 콘솔에서 인증서를 엽니다](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in).
   * **개인** > **인증서에서** **발급된** 모든 인증서는 클래식 배포 모델 또는 **Windows Azure CRP 인증서 생성기입니다.**
3. VM 백업 작업을 트리거합니다.

## <a name="extensionstuckindeletionstate---extension-state-is-not-supportive-to-backup-operation"></a>확장스턱인Deletion상태 - 확장 상태는 백업 작업을 지원하지 않습니다.

오류 코드: 확장스턱인델리션상태 <br/>
오류 메시지: 확장 상태가 백업 작업을 지원하지 않습니다.

백업 확장의 일관되지 않은 상태로 인해 백업 작업이 실패했습니다. 이 문제를 해결하려면 다음 단계를 따릅니다.

* 게스트 에이전트가 설치되어 있고 응답하는지 확인합니다.
* Azure 포털에서 가상 **컴퓨터** > **모든 설정** > **확장으로** 이동
* 백업 확장 VmSnapshot 또는 VmSnapshotLinux를 선택하고 **제거**를 클릭합니다.
* 백업 확장을 삭제한 후 백업 작업을 다시 시도합니다.
* 후속 백업 작업은 새 확장을 원하는 상태로 설치할 것입니다.

## <a name="extensionfailedsnapshotlimitreachederror---snapshot-operation-failed-as-snapshot-limit-is-exceeded-for-some-of-the-disks-attached"></a>확장FailedSnapshotLimitReachedError - 연결된 일부 디스크에 대해 스냅숏 제한이 초과되면 스냅숏 작업이 실패했습니다.

오류 코드: 확장 실패 스냅샷LimitReached오류 <br/>
오류 메시지: 연결된 일부 디스크에 대해 스냅숏 제한이 초과되면 스냅숏 작업이 실패했습니다.

연결된 일부 디스크에 대해 스냅숏 제한이 초과되어 스냅숏 작업이 실패했습니다. 아래 문제 해결 단계를 완료한 다음 작업을 다시 시도합니다.

* 필요하지 않은 디스크 Blob-스냅숏을 삭제합니다. Disk Blob을 삭제하지 않도록 주의해야 하며 스냅숏 Blob만 삭제해야 합니다.
* VM 디스크 저장소-계정에서 소프트 삭제를 사용하도록 설정된 경우 기존 스냅숏이 언제든지 허용되는 최대값보다 적도록 소프트 삭제 보존을 구성합니다.
* 백업된 VM에서 Azure 사이트 복구를 사용하도록 설정하면 다음 단계를 수행합니다.

  * **isanysnapshotfailed의** 값이 /etc/azure/vmbackup.conf에서 false로 설정되어 있는지 확인합니다.
  * 백업 작업과 충돌하지 않도록 다른 시간에 Azure 사이트 복구를 예약합니다.

## <a name="extensionfailedtimeoutvmnetworkunresponsive---snapshot-operation-failed-due-to-inadequate-vm-resources"></a>확장실패TimeoutVNetwork응답 - 부적절한 VM 리소스로 인해 스냅샷 작업이 실패했습니다.

오류 코드: 확장 실패시간VMNetwork응답<br/>
오류 메시지: VM 리소스부족으로 인해 스냅숏 작업이 실패했습니다.

스냅숏 작업을 수행하는 동안 네트워크 호출이 지연되어 VM의 백업 작업이 실패했습니다. 이 문제를 해결하려면 1단계를 수행합니다. 문제가 지속되면 2 및 3단계를 시도합니다.

**1단계**: 호스트를 통한 스냅샷 만들기

상위(관리자) 명령 프롬프트에서 아래 명령을 실행합니다.

```text
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v SnapshotMethod /t REG_SZ /d firstHostThenGuest /f
REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgentPersistentKeys" /v CalculateSnapshotTimeFromHost /t REG_SZ /d True /f
```

이렇게 하면 Guest가 아닌 호스트를 통해 스냅샷이 만들어집니다. 백업 작업을 다시 시도합니다.

**2단계**: VM의 부하가 적은 시간(CPU/IOps 감소 등)으로 백업 일정을 변경해 보십시오.

**3 단계**: [VM 크기를 늘리고](https://azure.microsoft.com/blog/resize-virtual-machines/) 작업을 다시 시도하십시오.

## <a name="common-vm-backup-errors"></a>일반적인 VM 백업 오류

| 오류 세부 정보 | 해결 방법 |
| ------ | --- |
| **오류 코드**: 320001, ResourceNotFound <br/> **오류 메시지**: VM이 더 이상 존재하지 않기 때문에 작업을 수행할 수 없습니다. <br/> <br/> **오류 코드**: 400094, BCMV2VMNot <br/> **오류 메시지**: 가상 시스템이 존재하지 않습니다. <br/> <br/>  Azure 가상 머신을 찾을 수 없습니다.  |이 오류는 주 VM이 삭제되었지만 백업 정책이 백업을 수행하기 위해 여전히 VM을 검색할 때 발생합니다. 이 오류를 해결하려면 다음 단계를 수행합니다. <ol><li> 이름이 같고 리소스 그룹 이름, 클라우드 서비스 **이름으로**가상 컴퓨터를 다시 만듭니다.<br>**또는**</li><li> 백업 데이터를 삭제하거나 삭제하지 않고 가상 머신의 보호를 중지합니다. 자세한 내용은 [가상 머신 보호 중지](backup-azure-manage-vms.md#stop-protecting-a-vm)를 참조하세요.</li></ol>|
|**오류 코드**: 사용자 오류BCM프리미엄스토리지QuotaErrorError<br/> **오류 메시지**: 저장소 계정의 여유 공간이 부족하여 가상 시스템의 스냅숏을 복사할 수 없습니다. | VM 백업 스택 V1에 있는 프리미엄 VM의 경우 스토리지 계정에 스냅샷을 복사합니다. 이 단계는 스냅샷에서 작동하는 백업 관리 트래픽이 프리미엄 디스크를 사용하는 애플리케이션에서 사용 가능한 IOPS 수를 제한하지 않도록 하기 위한 것입니다. <br><br>총 스토리지 계정 공간의 50%, 17.5TB만을 할당하는 것이 좋습니다. 그런 다음, Azure Backup 서비스에서 스토리지 계정에 스냅샷을 복사하고 스토리지 계정의 복사된 위치에서 자격 증명 모음으로 데이터를 전송할 수 있습니다. |
| **오류 코드**: 380008, AzureVmOffline <br/> **오류 메시지**: 가상 시스템이 실행되지 않아 Microsoft 복구 서비스 확장을 설치하지 못했습니다. | VM 에이전트는 Azure Recovery Services 확장에 대한 필수 구성 요소입니다. Azure Virtual Machine 에이전트를 설치하고 등록 작업을 다시 시작합니다. <br> <ol> <li>VM 에이전트가 제대로 설치되었는지 확인합니다. <li>VM 구성의 플래그가 올바르게 설정되었는지 확인합니다.</ol> VM 에이전트 설치 및 VM 에이전트 설치의 유효성을 검사하는 방법에 대해 자세히 알아보세요. |
| **오류 코드**: 확장스냅샷비트로커오류 <br/> **오류 메시지**: VSS(볼륨 섀도우 복사 서비스) 작업 오류로 스냅숏 작업이 실패했습니다.이 **드라이브는 BitLocker 드라이브 암호화에 의해 잠겨 있습니다. 제어판에서 이 드라이브의 잠금을 해제해야 합니다.** |VM에 있는 모든 드라이브의 BitLocker를 끄고 VSS 문제가 해결되었는지 확인합니다. |
| **오류 코드**: VmNotIn바람직한 상태 <br/> **오류 메시지**: VM이 백업을 허용하는 상태가 아닙니다. |<ul><li>VM이 **실행 중**에서 **종료** 상태로 전환되고 있으면 상태가 변경될 때까지 기다립니다. 그런 다음, 백업 작업을 트리거합니다. <li> VM이 Linux 에이전트이고 Security-Enhanced Linux 커널 모듈을 사용하는 경우 보안 정책에서 Azure Linux 에이전트 경로 **/var/lib/waagent**를 제외하여 백업 확장이 설치되도록 합니다.  |
| VM 에이전트가 가상 머신에 없습니다. <br>모든 필수 구성 요소 및 VM 에이전트를 설치합니다. 그런 다음, 작업을 다시 시작합니다. |[VM 에이전트 설치 및 VM 에이전트 설치의 유효성을 검사하는 방법](#vm-agent)에 대해 자세히 알아보세요. |
| **오류 코드**: 확장스냅샷실패NoSecureNetwork <br/> **오류 메시지**: 보안 네트워크 통신 채널을 만들지 못해 스냅숏 작업이 실패했습니다. | <ol><li> 관리자 권한 모드에서 **regedit.exe**를 실행하여 레지스트리 편집기를 엽니다. <li> 시스템에 있는 모든 버전의 .NET Framework를 파악합니다. 이러한 버전은 레지스트리 키 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft**의 계층 구조 아래에 있습니다. <li> 레지스트리 키에 있는 각 .NET Framework에 대해 다음 키를 추가합니다. <br> **SchUseStrongCrypto"=dword:000000001**. </ol>|
| **오류 코드**: 확장VCRedist설치실패 <br/> **오류 메시지**: Visual Studio 2012에 대해 Visual C++ 재배포 가능 을 설치하지 못해 스냅숏 작업이 실패했습니다. | C:\패키지\플러그인\Microsoft.Azure.RecoveryServices.VMSnapshot\에이전트버전으로 이동하여 vcredist2013_x64 설치합니다.<br/>서비스 설치를 허용하는 레지스트리 키 값이 올바른 값으로 설정되어 있는지 확인합니다. 즉, **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\서비스\Msiserver에서** **시작** 값을 **4가**아닌 **3으로** 설정합니다. <br><br>설치하는 데 여전히 문제가 발생할 경우 관리자 권한 명령 프롬프트에서 **MSIEXEC /UNREGISTER**를 실행한 후 **MSIEXEC /REGISTER**를 실행하여 설치 서비스를 다시 시작합니다.  |
| **오류 코드**: 사용자 오류 요청디허용ByPolicy <BR> **오류 메시지**: 스냅숏 작업을 방해하는 VM에서 잘못된 정책이 구성됩니다. | [환경 내에서 태그를 제어하는](https://docs.microsoft.com/azure/governance/policy/tutorials/govern-tags)Azure 정책이 있는 경우 거부 [효과에서](https://docs.microsoft.com/azure/governance/policy/concepts/effects#deny) [수정 효과로](https://docs.microsoft.com/azure/governance/policy/concepts/effects#modify)정책을 변경하거나 [Azure Backup에 필요한 명명 스키마에](https://docs.microsoft.com/azure/backup/backup-during-vm-creation#azure-backup-resource-group-for-virtual-machines)따라 리소스 그룹을 수동으로 만드는 것이 좋습니다.

## <a name="jobs"></a>작업

| 오류 세부 정보 | 해결 방법 |
| --- | --- |
| 취소는 이 작업 유형에 지원되지 않습니다. <br>작업이 완료될 때까지 기다립니다. |None |
| 작업이 취소 가능한 상태에 있지 않습니다. <br>작업이 완료될 때까지 기다립니다. <br>**또는**<br> 선택한 작업이 취소 가능한 상태에 있지 않습니다. <br>작업이 완료될 때까지 기다립니다. |작업이 거의 완료되는 것입니다. 작업이 완료될 때까지 기다립니다.|
| 진행 중이 아니므로 백업에서 작업을 취소할 수 없습니다. <br>취소는 진행 중인 작업에서만 지원됩니다. 진행 중인 작업을 취소하려고 합니다. |이 오류는 일시적인 상태로 인해 발생합니다. 잠시 기다렸다가 취소 작업을 다시 시도합니다. |
| 백업에서 작업을 취소하지 못했습니다. <br>작업이 완료될 때까지 기다립니다. |None |

## <a name="restore"></a>복원

| 오류 세부 정보 | 해결 방법 |
| --- | --- |
| 클라우드 내부 오류로 인해 복원이 실패했습니다. |<ol><li>복원하려는 클라우드 서비스가 DNS 설정을 사용하여 구성되었습니다. 다음을 확인할 수 있습니다. <br>**$deployment = Get-AzureDeploy -ServiceName "ServiceName" -슬롯 "프로덕션" Get-AzureDns -DnsSettings $deployment. DnsSettings**.<br>**주소**가 구성된 경우 DNS 설정이 구성되었습니다.<br> <li>복원하려는 클라우드 서비스가 **ReservedIP**를 사용하여 구성되고, 클라우드 서비스의 기존 VM이 중단된 상태에 있습니다. 다음 PowerShell cmdlet: **$deployment = Get-AzureDeployment -ServiceName "servicename" -Slot "Production" $dep.ReservedIPName**을 사용하여 클라우드 서비스가 IP를 예약했는지 확인합니다. <br><li>동일한 클라우드 서비스에 다음과 같이 특수한 네트워크 구성을 사용하여 가상 머신을 복원하려고 시도하고 있습니다. <ul><li>부하 분산 장치 구성의 가상 머신, 내부 및 외부<li>여러 개의 예약된 IP를 사용하는 가상 머신 <li>여러 NIC가 있는 가상 머신 </ul><li>특수한 네트워크 구성을 가진 VM의 경우 [복원 고려 사항](backup-azure-arm-restore-vms.md#restore-vms-with-special-configurations)을 참조하거나 UI에서 새 클라우드 서비스를 선택하세요</ol> |
| 선택된 DNS 이름이 이미 사용 중입니다. <br>다른 DNS 이름을 지정하고 다시 시도합니다. |이 DNS 이름은 클라우드 서비스 이름을 가리킵니다. 일반적으로 **.cloudapp.net**으로 끝납니다. 이 이름은 고유해야 합니다. 이 오류가 발생하는 경우 복원하는 동안 다른 VM 이름을 선택해야 합니다. <br><br>  이 오류는 Azure 포털의 사용자에게만 표시됩니다. PowerShell 통한 복원 작업은 디스크만 복원하고 VM을 만들지 않기 때문에 성공합니다. 디스크 복원 작업 후 사용자가 명시적으로 VM를 만들 경우 오류가 발생합니다. |
| 지정된 가상 네트워크 구성이 올바르지 않습니다. <br>다른 가상 네트워크 구성을 지정하고 다시 시도합니다. |None |
| 지정된 클라우드 서비스에서 복원 중인 가상 머신의 구성과 일치하지 않는 예약된 IP를 사용하고 있습니다. <br>예약된 IP를 사용하지 않는 다른 클라우드 서비스를 지정합니다. 또는 복원할 다른 복구 지점을 선택합니다. |None |
| 클라우드 서비스가 입력 엔드포인트 수의 한계에 도달했습니다. <br>다른 클라우드 서비스를 지정하거나 기존 엔드포인트를 사용하여 작업을 다시 시도합니다. |None |
| Recovery Services 자격 증명 모음 및 대상 스토리지 계정이 서로 다른 두 지역에 있습니다. <br>복원 작업에서 지정된 스토리지 계정이 Recovery Services 자격 증명 모음과 동일한 Azure 지역에 있는지 확인합니다. |None |
| 복원 작업에 지정된 스토리지 계정이 지원되지 않습니다. <br>로컬 중복 또는 지역 중복 복제 설정이 있는 기본 또는 표준 스토리지 계정만 지원됩니다. 지원되는 스토리지 계정을 선택합니다. |None |
| 복원 작업에 지정된 스토리지 계정의 유형이 온라인 상태에 있지 않습니다. <br>복원 작업에 지정된 스토리지 계정이 온라인 상태에 있는지 확인합니다. |이 오류는 Azure Storage의 일시적인 오류 또는 가동 중지로 인해 발생할 수 있습니다. 다른 스토리지 계정을 선택하세요. |
| 리소스 그룹 할당량에 도달했습니다. <br>Azure Portal의 일부 리소스 그룹을 삭제하거나 Azure 지원에 문의하여 제한을 늘립니다. |None |
| 선택한 서브넷이 존재하지 않습니다. <br>존재하는 서브넷을 선택합니다. |None |
| Backup 서비스에 구독의 리소스에 액세스할 수 있는 권한이 없습니다. |이 오류를 해결하려면 먼저 [백업된 디스크 복원](backup-azure-arm-restore-vms.md#restore-disks)의 단계를 사용하여 디스크를 복원합니다. 그런 다음, [복원된 디스크에서 VM 만들기](backup-azure-vms-automation.md#restore-an-azure-vm)의 PowerShell 단계를 사용합니다. |

## <a name="backup-or-restore-takes-time"></a>백업 또는 복원에 시간이 걸림

백업에 12시간 이상 걸리거나 복원에 6시간이 이상 걸리는 경우 [모범 사례](backup-azure-vms-introduction.md#best-practices)및 성능 고려 [사항을 검토합니다.](backup-azure-vms-introduction.md#backup-performance)

## <a name="vm-agent"></a>VM 에이전트

### <a name="set-up-the-vm-agent"></a>VM 에이전트 설정

일반적으로, Azure 갤러리에서 만든 VM에는 VM 에이전트가 이미 있습니다. 그러나 온-프레미스 데이터 센터에서 마이그레이션되는 가상 머신에는 VM 에이전트가 설치되어 있지 않습니다. 이러한 VM의 경우 VM 에이전트를 명시적으로 설치해야 합니다.

#### <a name="windows-vms"></a>Windows VM

* [에이전트 MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)를 다운로드하여 설치합니다. 설치를 완료하려면 관리자 권한이 필요합니다.
* 클래식 배포 모델을 사용하여 생성된 가상 머신의 경우 [VM 속성을 업데이트](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/install-vm-agent-offline#use-the-provisionguestagent-property-for-classic-vms)하여 에이전트가 설치되었다고 표시합니다. Azure Resource Manager 가상 머신의 경우 이 단계가 필요하지 않습니다.

#### <a name="linux-vms"></a>Linux VM

* 배포 리포지토리에서 최신 버전의 에이전트를 설치합니다. 패키지 이름에 대한 자세한 내용은 [Linux 에이전트 리포지토리](https://github.com/Azure/WALinuxAgent)를 참조하십시오.
* 클래식 배포 모델을 사용하여 만든 VM의 경우 [VM 속성을 업데이트하고](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/install-vm-agent-offline#use-the-provisionguestagent-property-for-classic-vms) 에이전트가 설치되어 있는지 확인합니다. Resource Manager 가상 머신의 경우 이 단계가 필요하지 않습니다.

### <a name="update-the-vm-agent"></a>VM 에이전트 업데이트

#### <a name="windows-vms"></a>Windows VM

* VM 에이전트를 업데이트하려면 [VM 에이전트 이진 파일](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)을 다시 설치합니다. 에이전트를 업데이트하기 전에 VM 에이전트 업데이트 동안 백업 작업이 발생하지 않는지 확인합니다.

#### <a name="linux-vms"></a>Linux VM

* Linux VM 에이전트를 업데이트하려면 [Linux VM 에이전트 업데이트 문서의](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)지침을 따르십시오.

    > [!NOTE]
    > 항상 배포 리포지토리를 사용하여 에이전트를 업데이트합니다.

    GitHub에서 에이전트 코드를 다운로드하지 마세요. 최신 에이전트를 배포할 수 없는 경우 배포 지원에 문의하여 최신 에이전트를 획득하기 위한 지침을 얻으세요. GitHub 리포지토리에서 최신 [Microsoft Azure Linux 에이전트](https://github.com/Azure/WALinuxAgent/releases) 정보를 확인할 수 있습니다.

### <a name="validate-vm-agent-installation"></a>VM 에이전트 설치의 유효성 검사

Windows VM에서 VM 에이전트 버전을 확인합니다.

1. Azure Virtual Machine에 로그인하고 **C:\WindowsAzure\Packages** 폴더로 이동합니다. **WaAppAgent.exe** 파일을 찾아야 합니다.
2. 해당 파일을 마우스 오른쪽 단추로 클릭하고 **속성**으로 이동합니다. 그런 다음 **세부 정보** 탭을 선택합니다. **제품 버전** 필드는 2.6.1198.718 이상이어야 합니다.

## <a name="troubleshoot-vm-snapshot-issues"></a>VM 스냅샷 문제 해결

VM 백업은 기본 스토리지에 대한 스냅샷 명령 실행을 사용합니다. 스토리지에 액세스할 수 없거나 스냅샷 작업 실행이 지연되는 경우 백업 작업이 실패할 수 있습니다. 다음 조건으로 인해 스냅샷 작업 오류가 발생할 수 있습니다.

* **SQL Server 백업이 구성된 VM은 스냅숏 작업 지연을 일으킬 수 있습니다.** 기본적으로 VM 백업은 Windows VM에서 VSS 전체 백업을 만듭니다. SQL Server 백업이 구성된 SQL Server를 실행하는 VM에서는 스냅샷 지연이 발생할 수 있습니다. 스냅샷 지연으로 인해 백업이 실패하는 경우 다음 레지스트리 키를 설정합니다.

   ```text
   [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
   "USEVSSCOPYBACKUP"="TRUE"
   ```

* **VM이 RDP에서 종료되므로 VM 상태가 잘못 보고됩니다**. 원격 데스크톱을 사용하여 가상 머신을 종료한 경우 포털의 VM 상태가 올바른지 확인합니다. 상태가 올바르지 않은 경우 포털 VM 대시보드에서 **종료** 옵션을 사용하여 VM을 종료합니다.
* **두 개 이상의 VM이 동일한 클라우드 서비스를 공유하는 경우 여러 백업 정책에 VM을 분산합니다.** 5개 이상의 VM 백업이 동시에 시작되지 않도록 백업 시간을 엇갈려 지정합니다. 정책의 시작 시간을 1시간 이상 간격으로 지정합니다.
* **VM이 높은 CPU 또는 메모리에서 실행됩니다**. 가상 머신이 높은 메모리 또는 CPU 사용량(90% 초과)에서 실행 중인 경우 스냅샷 작업이 큐 대기되고 지연됩니다. 결국 그것은 밖으로 시간. 이 문제가 발생하면 주문형 백업을 시도하십시오.

## <a name="networking"></a>네트워킹

IaaS VM 백업이 작동하려면 게스트 내에 DHCP를 사용하도록 설정되어야 합니다. 고정 프라이빗 IP가 필요한 경우 Azure Portal 또는 PowerShell을 통해 구성합니다. VM 내 DHCP 옵션이 활성화되었는지 확인합니다.
PowerShell을 통해 고정 IP를 설정하는 방법에 대한 자세한 정보를 가져옵니다.

* [기존 VM에 고정 내부 IP를 추가하는 방법](https://docs.microsoft.com/powershell/module/az.network/set-aznetworkinterfaceipconfig?view=azps-3.5.0#description)
* [네트워크 인터페이스에 할당된 개인 IP 주소의 할당 방법 변경](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface)
