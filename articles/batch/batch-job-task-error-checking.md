---
title: 작업 및 태스크 오류 확인
description: 작업 및 작업을 확인 하 고 문제를 해결 하는 오류
author: mscurrell
ms.topic: article
ms.date: 03/10/2019
ms.author: markscu
ms.openlocfilehash: 0c58bdf50f3e69b2b7d18f750f94fecdb512af85
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82116029"
---
# <a name="job-and-task-error-checking"></a>작업 및 태스크 오류 검사

작업 및 태스크를 추가할 때 발생할 수 있는 다양 한 오류가 있습니다. API, CLI 또는 UI에 의해 오류가 즉시 반환 되므로 이러한 작업에 대 한 오류를 검색 하는 작업은 간단 합니다.  그러나 나중에 작업 및 작업을 예약 하 고 실행할 때 발생할 수 있는 오류가 있습니다.

이 문서에서는 작업 및 작업을 제출한 후에 발생할 수 있는 오류에 대해 설명 합니다. 검사 하 고 처리 해야 하는 오류를 나열 하 고 설명 합니다.

## <a name="jobs"></a>작업

작업은 실행 되는 명령줄을 실제로 지정 하는 태스크 인 하나 이상의 태스크를 그룹화 한 것입니다.

작업을 추가 하는 경우 작업이 실패할 수 있는 방법에 영향을 줄 수 있는 다음 매개 변수를 지정할 수 있습니다.

- [작업 제약 조건](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)
  - 선택적 `maxWallClockTime` 으로 속성을 지정 하 여 작업을 활성화 하거나 실행할 수 있는 최대 시간을 설정할 수 있습니다. 이를 초과 하면 작업의 [Executioninfo](https://docs.microsoft.com/rest/api/batchservice/job/get#cloudjob) 에 설정 `terminateReason` 된 속성을 사용 하 여 작업이 종료 됩니다.
- [작업 준비 태스크](https://docs.microsoft.com/rest/api/batchservice/job/add#jobpreparationtask)
  - 지정 된 경우 작업 준비 태스크는 노드의 작업에 대해 작업을 처음 실행할 때 실행 됩니다. 작업 준비 태스크가 실패할 수 있으며,이로 인해 태스크가 실행 되지 않고 작업이 완료 되지 않습니다.
- [작업 해제 태스크](https://docs.microsoft.com/rest/api/batchservice/job/add#jobreleasetask)
  - 작업 준비 태스크가 구성 된 경우에만 작업 해제 태스크를 지정할 수 있습니다. 작업이 종료 되 면 작업 준비 태스크가 실행 된 각 풀 노드에서 작업 릴리스 태스크가 실행 됩니다. 작업 해제 태스크가 실패할 수 있지만 작업은 계속 `completed` 상태로 전환 됩니다.

### <a name="job-properties"></a>작업 속성

다음 작업 속성에 오류가 있는지 확인 해야 합니다.

- '[Executioninfo](https://docs.microsoft.com/rest/api/batchservice/job/get#jobexecutioninformation)':
  - `terminateReason` 속성에는 작업 제약 조건에 지정 된 `maxWallClockTime`가 초과 되어 작업이 종료 되었음을 나타내는 값이 있을 수 있습니다. 작업 `onTaskFailure` 속성이 적절 하 게 설정 된 경우 태스크가 실패 했음을 나타내도록 설정할 수도 있습니다.
  - [SchedulingError](https://docs.microsoft.com/rest/api/batchservice/job/get#jobschedulingerror) 속성은 예약 오류가 발생 한 경우에 설정 됩니다.
 
### <a name="job-preparation-tasks"></a>작업 준비 태스크

작업에 대해 작업 준비 태스크가 지정 된 경우 해당 작업의 인스턴스가 한 노드에서 처음으로 실행 될 때 해당 작업의 인스턴스가 실행 됩니다. 작업에 대해 구성 된 작업 준비 태스크는 풀의 노드 수까지 여러 작업 준비 태스크 인스턴스가 실행 되는 작업 템플릿으로 간주할 수 있습니다.

오류가 있는지 확인 하려면 작업 준비 태스크 인스턴스를 확인 해야 합니다.
- 작업 준비 태스크를 실행 하면 작업 준비 태스크를 트리거한 태스크가의 `preparing` [상태로](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate) 이동 합니다. 작업 준비 태스크가 실패 하면 트리거 작업이 `active` 상태로 되돌아가고 실행 되지 않습니다.  
- 실행 된 작업 준비 태스크의 모든 인스턴스는 [목록 준비 및 릴리스 작업 상태](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus) API를 사용 하 여 작업에서 가져올 수 있습니다. 모든 작업과 마찬가지로 `failureInfo`, `exitCode`, 등의 속성을 사용 하 여 [실행 정보](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) 를 사용할 수 `result`있습니다.
- 작업 준비 태스크가 실패 하는 경우 트리거 작업 태스크가 실행 되지 않으며 작업이 완료 되지 않고 중지 됩니다. 작업을 예약할 수 있는 다른 작업이 없는 경우 풀이 사용 되지 않을 수 있습니다.

### <a name="job-release-tasks"></a>작업 릴리스 작업

작업에 대해 작업 해제 태스크가 지정 된 경우 작업을 종료 하면 작업 준비 태스크가 실행 된 각 풀 노드에서 작업 해제 태스크의 인스턴스가 실행 됩니다.  작업 릴리스 태스크 인스턴스를 확인 하 여 오류가 있는지 확인 해야 합니다.
- 실행 중인 작업 릴리스 작업의 모든 인스턴스는 API [목록 준비 및 릴리스 작업 상태](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus)를 사용 하 여 작업에서 가져올 수 있습니다. 모든 작업과 마찬가지로 `failureInfo`, `exitCode`, 등의 속성을 사용 하 여 [실행 정보](https://docs.microsoft.com/rest/api/batchservice/job/listpreparationandreleasetaskstatus#jobpreparationandreleasetaskexecutioninformation) 를 사용할 수 `result`있습니다.
- 하나 이상의 작업 릴리스 태스크가 실패 한 경우에도 작업이 종료 되 고 `completed` 상태로 전환 됩니다.

## <a name="tasks"></a>작업

작업 태스크는 여러 가지 이유로 실패할 수 있습니다.

- 작업 명령줄이 실패 하 고 0이 아닌 종료 코드를 반환 합니다.
- 작업에 `resourceFiles` 대해가 지정 되었지만 하나 이상의 파일이 다운로드 되지 않았음을 나타내는 오류가 있습니다.
- 태스크에 `outputFiles` 대해가 지정 되었지만 하나 이상의 파일이 업로드 되지 않았음을 나타내는 오류가 있습니다.
- 작업 `maxWallClockTime` [제약 조건의](https://docs.microsoft.com/rest/api/batchservice/task/add#taskconstraints)속성에 의해 지정 된 작업에 대해 경과 된 시간을 초과 했습니다.

모든 경우에서 오류 및 오류에 대 한 정보를 확인 하려면 다음 속성을 확인 해야 합니다.
- 작업 [Executioninfo](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutioninformation) 속성에는 오류에 대 한 정보를 제공 하는 여러 속성이 포함 되어 있습니다. [결과](https://docs.microsoft.com/rest/api/batchservice/task/get#taskexecutionresult) 는 작업이 어떤 이유로 든 실패 하 `exitCode` 고 `failureInfo` 오류에 대 한 자세한 정보를 제공 하는지 여부를 나타냅니다.
- 작업은 성공 또는 실패 여부와 `completed` 관계 없이 항상 [상태로](https://docs.microsoft.com/rest/api/batchservice/task/get#taskstate)전환 됩니다.

작업에 대 한 태스크 실패의 영향과 작업 종속성을 고려해 야 합니다.  태스크가 종속성 및 작업에 대 한 작업을 구성 하는 데 [Exitconditions](https://docs.microsoft.com/rest/api/batchservice/task/add#exitconditions) 속성을 지정할 수 있습니다.
- 종속성의 경우 [DependencyAction](https://docs.microsoft.com/rest/api/batchservice/task/add#dependencyaction) 는 실패 한 태스크에 종속 된 태스크가 차단 되는지 아니면 실행 되는지를 제어 합니다.
- 작업의 경우 [JobAction](https://docs.microsoft.com/rest/api/batchservice/task/add#jobaction) 은 실패 한 태스크에서 작업을 사용 하지 않도록 설정 하거나, 종료 하거나, 그대로 유지 하도록 할지 여부를 제어 합니다.

### <a name="task-command-line-failures"></a>작업 명령줄 오류

작업 명령줄이 실행 되 면 출력은 및 `stderr.txt` `stdout.txt`에 기록 됩니다. 또한 응용 프로그램에서 응용 프로그램 관련 로그 파일에 쓸 수 있습니다.

태스크가 실행 된 풀 노드가 여전히 있는 경우 로그 파일을 가져와 볼 수 있습니다. 예를 들어 Azure Portal는 태스크 또는 풀 노드에 대 한 로그 파일을 나열 하 고 볼 수 있습니다. 여러 Api를 사용 하 여 작업 파일을 나열 하 고 가져올 수 있습니다 (예: [작업에서 가져오기](https://docs.microsoft.com/rest/api/batchservice/file/getfromtask)).

풀 및 풀 노드는 자주 삭제 되 고 노드를 계속 추가 하 고 삭제 하는 것으로 인해 로그 파일을 유지 하는 것이 좋습니다. [태스크 출력 파일](https://docs.microsoft.com/azure/batch/batch-task-output-files) 은 로그 파일을 Azure Storage에 저장 하는 편리한 방법입니다.

### <a name="output-file-failures"></a>출력 파일 오류
모든 파일 업로드에서 Batch는 두 개의 로그 파일을 `fileuploadout.txt` 및 `fileuploaderr.txt` 컴퓨팅 노드에 씁니다. 이러한 로그 파일을 검사하여 특정 오류에 대해 자세히 확인할 수 있습니다. 예를 들어 작업 자체를 실행할 수 없기 때문에 파일 업로드가 시도 되지 않은 경우에는 이러한 로그 파일이 존재 하지 않습니다.  

## <a name="next-steps"></a>다음 단계

응용 프로그램에서 포괄적인 오류 검사를 구현 하는지 확인 합니다. 문제를 즉시 검색 하 고 진단 하는 것이 중요할 수 있습니다.
