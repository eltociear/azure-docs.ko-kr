---
title: Azure 응용 프로그램 인사이트 에이전트 - 시작 | 마이크로 소프트 문서
description: 애플리케이션 인사이트 에이전트에 대한 빠른 시작 가이드입니다. 웹 사이트를 다시 배포하지 않고 웹 사이트 성능을 모니터링합니다. 온-프레미스, VM 또는 Azure에서 호스팅되는 ASP.NET 웹 앱과 함께 작동합니다.
ms.topic: conceptual
author: TimothyMothra
ms.author: tilee
ms.date: 04/23/2019
ms.openlocfilehash: 4cfa136585611e81a4060c5544d5dc464b32f12c
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537443"
---
# <a name="get-started-with-azure-monitor-application-insights-agent-for-on-premises-servers"></a>온-프레미스 서버에 대한 Azure 모니터 응용 프로그램 인사이트 에이전트 시작

이 문서에는 대부분의 환경에서 작동할 것으로 예상되는 빠른 시작 명령이 포함되어 있습니다.
지침은 업데이트를 배포하기 위해 PowerShell 갤러리에 따라 다릅니다.
이러한 명령은 PowerShell `-Proxy` 매개 변수를 지원합니다.

이러한 명령, 사용자 지정 지침 및 문제 해결에 대한 자세한 [내용은 자세한 지침을](status-monitor-v2-detailed-instructions.md)참조하십시오.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="download-and-install-via-powershell-gallery"></a>파워쉘 갤러리를 통한 다운로드 및 설치

### <a name="install-prerequisites"></a>필수 구성 요소 설치
PowerShell을 관리자로 실행합니다.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
PowerShell을 닫습니다.

### <a name="install-application-insights-agent"></a>애플리케이션 인사이트 에이전트 설치
PowerShell을 관리자로 실행합니다.
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>모니터링 사용
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download-and-install-manually-offline-option"></a>수동으로 다운로드 및 설치(오프라인 옵션)
### <a name="download-the-module"></a>모듈 다운로드
[수동으로 PowerShell 갤러리에서](https://www.powershellgallery.com/packages/Az.ApplicationMonitor)모듈의 최신 버전을 다운로드합니다.

### <a name="unzip-and-install-application-insights-agent"></a>응용 프로그램 인사이트 에이전트의 압축 을 풀고 설치
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.3.0-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>모니터링 사용
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```



## <a name="next-steps"></a>다음 단계

 원격 분석 보기:

- [메트릭을 탐색하여](../../azure-monitor/platform/metrics-charts.md) 성능 및 사용량을 모니터링합니다.
- [이벤트 및 로그를 검색하여](../../azure-monitor/app/diagnostic-search.md) 문제를 진단합니다.
- 고급 쿼리에 [애널리틱스를 사용합니다.](../../azure-monitor/app/analytics.md)
- [대시보드를 만듭니다.](../../azure-monitor/app/overview-dashboard.md)

 원격 분석 더 추가:

- [웹 테스트를 만들어](monitor-web-app-availability.md) 사이트가 라이브 상태로 유지되고 있는지 확인합니다.
- [웹 클라이언트 원격 분석을 추가하여](../../azure-monitor/app/javascript.md) 웹 페이지 코드의 예외를 확인하고 추적 호출을 사용하도록 설정합니다.
- 추적 및 로그 호출을 삽입할 수 있도록 [코드에 응용 프로그램 인사이트 SDK를 추가합니다.](../../azure-monitor/app/asp-net.md)

애플리케이션 인사이트 에이전트로 더 많은 작업을 수행합니다.

- 여기에 있는 명령에 대한 [자세한 지침을](status-monitor-v2-detailed-instructions.md) 검토하십시오.
- 가이드를 사용하여 애플리케이션 인사이트 에이전트 [문제를 해결하세요.](status-monitor-v2-troubleshoot.md)
