---
title: Azure Dev Spaces로 CI/CD 사용
services: azure-dev-spaces
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: conceptual
manager: gwallace
description: Azure 개발자 공간을 사용하여 Azure DevOps를 사용하여 지속적인 통합/연속 배포를 설정하는 방법에 대해 알아봅니다.
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, 컨테이너
ms.openlocfilehash: f2eb9449518b32ab74f2dbbca6b5489aed325db7
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685628"
---
# <a name="use-cicd-with-azure-dev-spaces"></a>Azure Dev Spaces로 CI/CD 사용

이 문서에서는 CI/CD(연속 통합/연속 배포)-Dev Spaces가 설정된 AKS(Azure Kubernetes Service)를 설정하는 과정을 안내합니다. CI/CD-AKS를 사용하면 커밋된 코드를 소스 리포지토리에 푸시할 때마다 앱 업데이트가 자동으로 배포될 수 있습니다. CI/CD를 Dev Spaces 지원 클러스터와 함께 사용하면 팀이 작업하는 애플리케이션의 기준선을 최신 상태로 유지할 수 있으므로 유용합니다.

![CI/CD 다이어그램의 예제](../media/common/ci-cd-simple.png)

이 문서에서는 Azure DevOps를 기준으로 설명하지만 Jenkins, TeamCity 등의 CI/CD 시스템에도 동일한 개념이 적용됩니다.

## <a name="prerequisites"></a>사전 요구 사항
* [Azure Dev Spaces가 설정된 AKS(Azure Kubernetes Service) 클러스터](../get-started-netcore.md)
* [Azure Dev Spaces CLI 설치](upgrade-tools.md)
* [프로젝트가 있는 Azure DevOps 조직](https://docs.microsoft.com/azure/devops/user-guide/sign-up-invite-teammates?view=vsts)
* [ACR(Azure Container Registry)](../../container-registry/container-registry-get-started-azure-cli.md)
    * Azure Container Registry [관리자 계정](../../container-registry/container-registry-authentication.md#admin-account) 세부 정보
* [AKS 클러스터가 Azure Container Registry에서 끌어오도록 허가](../../aks/cluster-container-registry-integration.md)

## <a name="download-sample-code"></a>샘플 코드 다운로드
이제 샘플 코드 GitHub 리포지토리의 포크를 만들어 보겠습니다. https://github.com/Azure/dev-spaces로 이동하여 **포크**를 선택합니다. 포크 프로세스가 완료되면 리포지토리의 포크된 버전을 로컬로 **복제**합니다. 기본적으로 _master_ 분기가 체크 아웃되지만, _azds_updates_ 분기에서도 마찬가지로 포크 동안 전송되어야 하는 시간 절감 변경 내용을 몇 가지 포함했습니다. _azds_updates_ 분기는 Dev Spaces 자습서 섹션에서 수동으로 수행하도록 요구되는 업데이트와 CI/CD 시스템 배포를 간소화하기 위해 미리 만든 일부 YAML 및 JSON 파일도 포함되어 있습니다. `git checkout -b azds_updates origin/azds_updates`와 같은 명령을 사용하여 로컬 리포지토리의 _azds_updates_ 분기를 체크 아웃할 수 있습니다.

## <a name="dev-spaces-setup"></a>Dev Spaces 설치
`azds space select` 명령을 사용하여 _dev_라는 새 공간을 만듭니다. _dev_ 공간은 CI/CD 파이프라인에서 코드 변경 내용을 푸시하는 데 사용합니다. 또한 이 공간은 _dev_를 기준으로 하는 _자식 공간_을 만드는 데도 사용됩니다.

```cmd
azds space select -n dev
```

상위 개발 공간을 선택하라는 메시지가 표시되면 _ \<없음을\>_ 선택합니다.

개발 공간을 만든 후 호스트 접미사를 확인해야 합니다. `azds show-context` 명령을 사용하여 Azure 개발자 공간 인그레스 컨트롤러의 호스트 접미사를 표시합니다.

```cmd
$ azds show-context
Name   ResourceGroup    DevSpace  HostSuffix
-----  ---------------  --------  ----------------------
MyAKS  MyResourceGroup  dev       fedcba098.eus.azds.io
```

위의 예에서 호스트 접미사는 _fedcba098.eus.azds.io._ 이 값은 릴리스 정의를 만들 때 나중에 사용됩니다.

_dev_ 공간은 항상 리포지토리의 최신 상태인 기준선을 포함하므로, 개발자는 더 큰 앱의 컨텍스트 내에서 격리된 변경 내용을 테스트하기 위해 _dev_에서 _자식 공간_을 만들 수 있습니다. 이 개념은 Dev Spaces 자습서에서 자세히 설명하고 있습니다.

## <a name="creating-the-build-definition"></a>빌드 정의 만들기
웹 브라우저에서 Azure DevOps 팀 프로젝트를 열고 _파이프라인_ 섹션으로 이동합니다. 먼저 Azure DevOps 사이트의 오른쪽 위에 있는 프로필 사진을 클릭하고, 미리 보기 기능 창을 연 후, _새 YAML 파이프라인 생성 환경_을 사용하지 않도록 설정합니다.

![미리 보기 기능 창 열기](../media/common/preview-feature-open.png)

사용하지 않도록 설정할 옵션은 다음과 같습니다.

![새 YAML 파이프라인 생성 환경 옵션](../media/common/yaml-pipeline-preview-feature.png)

> [!Note]
> Azure DevOps _새 YAML 파이프라인 생성 환경_ 미리 보기 기능은 현재, 미리 정의된 빌드 파이프라인 만들기와 충돌합니다. 현재는 미리 정의된 빌드 파이프라인을 배포하려면 이 옵션을 사용하지 않도록 설정해야 합니다.

_azds_updates_ 분기에 *mywebapi* 및 *webfrontend*에 필요한 빌드 단계를 정의하는 간단한 [Azure 파이프라인 YAML](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema?view=vsts&tabs=schema)을 포함했습니다.

선택한 언어에 따라, 파이프라인 YAML은 `samples/dotnetcore/getting-started/azure-pipelines.dotnetcore.yml`과 유사한 경로에서 체크 아웃되었습니다.

이 파일에서 파이프라인을 만들려면
1. DevOps 프로젝트 메인 페이지에서 파이프라인 > 빌드로 이동합니다.
1. **새** 빌드 파이프라인을 만드는 옵션을 선택합니다.
1. **GitHub를** 소스로 선택하고 필요한 경우 GitHub 계정으로 권한을 부여한 다음 _개발 공간_ 샘플 응용 프로그램 리포지토리의 분기 버전에서 _azds_updates_ 분기를 선택합니다.
1. **구성을 코드**또는 **YAML로**선택합니다.
1. 이제 빌드 파이프라인의 구성 페이지가 표시됩니다. 위에서 언급 한 대로 **...button을** 사용 하 여 **YAML 파일 경로에** 대 한 언어 별 경로 이동 합니다. `samples/dotnetcore/getting-started/azure-pipelines.dotnet.yml`)을 입력합니다.
1. 변수 탭으로 **이동합니다.**
1. 수동으로 _dockerId_를 변수로 입력합니다. 이 값은 [Azure Container Registry 관리자 계정](../../container-registry/container-registry-authentication.md#admin-account)의 사용자 이름입니다. (필수 구성 요소 문서에 설명됨)
1. 수동으로 _dockerPassword_를 변수로 입력합니다. 이 값은 [Azure Container Registry 관리자 계정](../../container-registry/container-registry-authentication.md#admin-account)의 암호입니다. 보안상의 이유로 _dockerPassword_는 비밀로 지정해야 합니다(잠금 아이콘 선택).
1. **& 큐 저장을**선택합니다.

이제 GitHub 포크의 _azds_updates_ 분기에 푸시된 모든 업데이트에 대해 *mywebapi* 및 *webfrontend*를 자동으로 빌드하는 CI 솔루션이 구현되었습니다. Azure 포털로 이동, Azure 컨테이너 레지스트리 선택 및 **저장소** 탭을 탐색하여 Docker 이미지가 푸시되었는지 확인할 수 있습니다. 이미지가 빌드되고 컨테이너 레지스트리에 표시되는 데 몇 분 정도 걸릴 수 있습니다.

![Azure Container Registry 리포지토리](../media/common/ci-cd-images-verify.png)

## <a name="creating-the-release-definition"></a>릴리스 정의 만들기

1. DevOps 프로젝트 주 페이지에서 파이프라인 > 릴리스로 이동합니다.
1. 릴리스 정의를 아직 포함하지 않는 새로운 DevOps 프로젝트에서 작업할 경우 계속하기 전에 먼지 빈 릴리스 정의를 만들어야 합니다. 가져오기 옵션은 기존 릴리스 정의가 있을 때까지 UI에 표시되지 않습니다.
1. 왼쪽에서 **+ 새** 단추를 클릭한 다음 **파이프라인 가져오기를 클릭합니다.**
1. **찾아보기를** 클릭하고 프로젝트에서 선택합니다. `samples/release.json`
1. **확인**을 클릭합니다. 릴리스 정의 편집 페이지에 파이프라인 창이 로드된 것을 확인할 수 있습니다. 또한 계속 구성해야 하는 클러스터 관련 세부 정보를 나타내는 몇 개의 빨간색 경고 아이콘도 표시됩니다.
1. 파이프라인 창 왼쪽에서 **아티팩트 추가** 풍선을 클릭합니다.
1. **소스** 드롭다운에서 이전에 만든 빌드 파이프라인을 선택합니다.
1. 기본 **버전의**경우 **태그가 있는 빌드 파이프라인 기본 분기에서 최신을 선택합니다.**
1. 태그를 비워 **둡니다.**
1. **소스 별칭**을 `drop`으로 설정합니다. **Source 별칭** 값은 미리 정의된 릴리스 태스크에서 사용되므로 설정해야 합니다.
1. **추가**를 클릭합니다.
1. 이제 아래와 같이 새로 만든 `drop` 아티팩트 소스의 전구 모양 아이콘을 클릭합니다.

    ![릴리스 아티팩트 연속 배포 설정](../media/common/release-artifact-cd-setup.png)
1. 연속 **배포 트리거를 사용하도록 설정합니다.**
1. **파이프라인** 옆의 **작업** 탭 위로 마우스를 가져가고 _개발자를_ 클릭하여 _개발_ 단계 작업을 편집합니다.
1. **Azure 리소스 관리자가** **연결 유형에서** 선택되었는지 확인합니다. 세 가지 드롭다운 컨트롤이 빨간색으로 ![강조 표시됩니다.](../media/common/release-setup-tasks.png)
1. Azure 개발자 공간에서 사용 하는 Azure 구독을 선택 합니다. 권한 **부여를**클릭해야 할 수도 있습니다.
1. Azure 개발자 공간에서 사용 하려는 리소스 그룹 및 클러스터를 선택 합니다.
1. 에이전트 **작업을**클릭합니다.
1. **에이전트 풀에서** **호스팅 우분투 1604를** 선택합니다.
1. 상단의 **작업** 선택기 위로 마우스를 _가져가고 prod_ 를 클릭하여 _prod_ 스테이지 작업을 편집합니다.
1. **Azure 리소스 관리자가** **연결 유형에서** 선택되었는지 확인합니다. Azure 개발자 공간에서 사용 하는 Azure 구독, 리소스 그룹 및 클러스터를 선택 합니다.
1. 에이전트 **작업을**클릭합니다.
1. **에이전트 풀에서** **호스팅 우분투 1604를** 선택합니다.
1. 변수 **탭을** 클릭하여 릴리스의 변수를 업데이트합니다.
1. **데브스페이스호스트서픽스의** 값을 **UPDATE_ME** 호스트 접미사로 업데이트합니다. 명령을 더 일찍 실행하면 호스트 `azds show-context` 접미사가 표시됩니다.
1. 오른쪽 위에서 **저장**을 클릭하고 **확인**을 클릭합니다.
1. 저장 옆에 있는 **+ 릴리스**를 클릭하고 **릴리스 만들기**를 클릭합니다.
1. **아티팩트에서**빌드 파이프라인의 최신 빌드가 선택되었는지 확인합니다.
1. **만들기**를 클릭합니다.

이제 자동화된 릴리스 프로세스가 시작되고 *mywebapi* 및 *webfrontend* 차트를 _dev_ 최상위 수준 공간의 Kubernetes 클러스터에 배포합니다. Azure DevOps 웹 포털에서 릴리스 진행 상황을 모니터링할 수 있습니다.

1. 파이프라인 에서 **릴리스** 섹션으로 **이동합니다.**
1. 샘플 응용 프로그램의 릴리스 파이프라인을 클릭합니다.
1. 최신 릴리스의 이름을 클릭합니다.
1. **단계** **아래의 개발** 상자 위로 마우스를 **가져가고 로그를 클릭합니다.**

릴리스는 모든 작업이 완료되면 수행됩니다.

> [!TIP]
> 릴리스가 *업그레이드 실패: 조건을 기다리는 동안 시간 초과되었습니다.* 와 같은 오류 메시지를 나타내며 실패하는 경우 [Kubernetes 대시보드를 사용하여](../../aks/kubernetes-dashboard.md) 클러스터의 pod를 검사하세요. 포드가 *이미지 "azdsexample.azurecr.io/mywebapi:122": rpc 오류 : rpc 오류 : 코드 = 알 수없는 desc = 데몬의\/오류 응답 : https : /azdsexample.azurecr.io/v2/mywebapi/manifests/122 : 인증이 필요하므로*클러스터가 Azure 컨테이너 레지스트리에서 가져올 권한이 없기 때문일 수 있습니다. [AKS 클러스터가 Azure Container Registry에서 끌어오도록 허가](../../aks/cluster-container-registry-integration.md)를 완료했는지 확인합니다.

이제 Dev Spaces 샘플 앱의 GitHub 포크에 대해 완전 자동화 CI/CD 파이프라인이 구현되었습니다. 코드를 커밋하고 푸시할 때마다 빌드 파이프라인은 *mywebapi* 및 *webfrontend* 이미지를 빌드한 후 사용자 지정 ACR 인스턴스로 푸시합니다. 그러면 릴리스 파이프라인은 각 앱의 Helm 차트를 Dev Spaces 지원 클러스터의 _dev_ 공간으로 배포합니다.

## <a name="accessing-your-_dev_-services"></a>_dev_ 서비스에 액세스
배포 후에 *webfrontend*의 _dev_ 버전은 `http://dev.webfrontend.fedcba098.eus.azds.io`와 같은 공용 URL을 통해 액세스할 수 있습니다. 다음 명령을 실행하여 이 `azds list-uri` URL을 찾을 수 있습니다. 

```cmd
$ azds list-uris

Uri                                           Status
--------------------------------------------  ---------
http://dev.webfrontend.fedcba098.eus.azds.io  Available
```

## <a name="deploying-to-production"></a>프로덕션에 배포

이 자습서에서 만든 CI/CD 시스템을 사용하여 수동으로 특정 릴리스를 _prod_로 승격합니다.
1. 파이프라인 에서 **릴리스** 섹션으로 **이동합니다.**
1. 샘플 응용 프로그램의 릴리스 파이프라인을 클릭합니다.
1. 최신 릴리스의 이름을 클릭합니다.
1. **단계** 아래의 **prod** 상자 위로 마우스를 **가져가면 배포를**클릭합니다.
    ![프로덕션으로 승격](../media/common/prod-promote.png)
1. **스테이지** 아래에 다시 **prod** 상자 위로 마우스를 **가져가서 로그를 클릭합니다.**

릴리스는 모든 작업이 완료되면 수행됩니다.

CI/CD 파이프라인의 _prod_ 단계는 개발자 공간 인그레스 컨트롤러 대신 로드 밸러버를 사용하여 _prod_ 서비스에 대한 액세스를 제공합니다. _prod_ 단계에서 배포된 서비스는 DNS 이름 대신 IP 주소로 액세스할 수 있습니다. 프로덕션 환경에서는 자체 DNS 구성에 따라 서비스를 호스트할 고유한 Ingress 컨트롤러를 만들 도록 선택할 수 있습니다.

웹 프런트 엔드 서비스의 IP를 확인하려면 **웹 프런트 엔드 공용 IP** 인쇄 단계를 클릭하여 로그 출력을 확장합니다. 로그 출력에 표시된 IP를 사용하여 **웹 프런트 엔드** 응용 프로그램에 액세스합니다.

```cmd
...
2019-02-25T22:53:02.3237187Z webfrontend can be accessed at http://52.170.231.44
2019-02-25T22:53:02.3320366Z ##[section]Finishing: Print webfrontend public IP
...
```

## <a name="dev-spaces-instrumentation-in-production"></a>프로덕션 환경의 Dev Spaces 계측
Dev Spaces 계측은 애플리케이션의 정상 작동 중에는 개입하지 _않도록_ 디자인되었지만 Dev Spaces가 설정되지 않은 Kubernetes 네임스페이스에서 프로덕션 워크로드를 실행하는 것이 좋습니다. 이러한 유형의 Kubernetes 네임스페이스를 사용할 경우 `kubectl` CLI를 사용하여 프로덕션 네임스페이스를 만들거나 첫 번째 Helm 배포 동안 CI/CD 시스템이 네임스페이스를 만들도록 허용해야 합니다. 공간을 _선택_하거나 Dev Spaces 도구를 사용하여 만들면 해당 네임스페이스에 Dev Spaces 계측이 추가됩니다.

다음은 단일 Kubernetes 클러스터에서 기능 개발, 'dev' 환경 _및_ 프로덕션 환경을 모두 지원하는 네임스페이스 구조 예제입니다.

![네임스페이스 구조 예제](../media/common/cicd-namespaces.png)

> [!Tip]
> `prod` 공간을 이미 만들었으며 단순히 Dev Spaces 계측에서 제외하려는 경우(삭제하지 않음), 다음 Dev Spaces CLI 명령을 사용하여 이와 같이 할 수 있습니다.
>
> `azds space remove -n prod --no-delete`
>
> Dev Spaces 계측 없이 다시 만들 수 있도록 이 작업을 수행한 후에 `prod` 네임스페이스에서 모든 pod를 삭제해야 할 수 있습니다.

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [Azure Dev Spaces을 사용하는 팀 개발에 대해 알아보기](../team-development-netcore.md)