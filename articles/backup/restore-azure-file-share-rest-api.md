---
title: REST API를 사용 하 고 Azure 파일 공유 복원
description: REST API를 사용하여 Azure Backup에서 만든 복원 지점에서 Azure 파일 공유 또는 특정 파일을 복원하는 방법에 대해 알아봅니다.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 1c3160491ef92c62745af1468556e7d5c30437fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252507"
---
# <a name="restore-azure-file-shares-using-rest-api"></a>REST API를 사용하여 Azure 파일 공유 복원

이 문서에서는 REST API를 사용하여 [Azure Backup에서](https://docs.microsoft.com/azure/backup/backup-overview) 만든 복원 지점에서 전체 파일 공유 또는 특정 파일을 복원하는 방법을 설명합니다.

이 문서의 끝으로 REST API를 사용 하 여 다음 작업을 수행 하는 방법을 알아봅니다.

* 백업된 Azure 파일 공유에 대한 복원 지점을 봅니다.
* 전체 Azure 파일 공유를 복원합니다.
* 개별 파일 또는 폴더를 복원합니다.

## <a name="prerequisites"></a>사전 요구 사항

복원하려는 백업된 파일 공유가 이미 있다고 가정합니다. 그렇지 않은 경우 [REST API를 사용하여 Azure 백업 파일 공유를](backup-azure-file-share-rest-api.md) 확인하여 파일을 만드는 방법을 알아봅니다.

이 문서에서는 다음 리소스를 사용합니다.

* **복구 서비스 볼트**: *azurefilesvault*
* **리소스 그룹**: *azurefiles*
* **저장 계정**: *afsaccount*
* **파일 공유**: *azurefiles*

## <a name="fetch-containername-and-protecteditemname"></a>컨테이너 이름 및 보호된항목 이름 가져오기

대부분의 복원 관련 API 호출의 경우 {containerName} 및 {protectedItemName} URI 매개 변수에 대한 값을 전달해야 합니다. [GET backupprotectableitems](https://docs.microsoft.com/rest/api/backup/protecteditems/get) 작업의 응답 본문에 ID 특성을 사용하여 이러한 매개 변수에 대한 값을 검색합니다. 이 예제에서는 보호하려는 파일 공유의 ID는 다음과 같은 것입니다.

`"/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/storagecontainer;storage;azurefiles;afsaccount/protectableItems/azurefileshare;azurefiles`

따라서 값은 다음과 같이 변환됩니다.

* {컨테이너 이름} - *저장소 컨테이너;저장소;azurefiles;afsaccount*
* {보호된ItemName} - *azurefileshare;azurefiles*

## <a name="fetch-recovery-points-for-backed-up-azure-file-share"></a>백업된 Azure 파일 공유에 대한 복구 지점 가져오기

백업된 파일 공유 또는 파일을 복원하려면 먼저 복구 지점을 선택하여 복원 작업을 수행합니다. 백업된 항목의 사용 가능한 복구 지점은 [복구 지점 목록](https://docs.microsoft.com/rest/api/site-recovery/recoverypoints/listbyreplicationprotecteditems) REST API 호출을 사용하여 나열할 수 있습니다. 모든 관련 값을 가진 GET 작업입니다.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2019-05-13&$filter={$filter}
```

URI 값을 다음과 같이 설정합니다.

* {패브릭 이름}: *Azure*
* {볼트 이름}: *azurefilesvault*
* {컨테이너 이름}: *저장소 컨테이너;저장소;azurefiles;afsaccount*
* {보호된ItemName}: *azurefileshare;azurefiles*
* {리소스 그룹 이름}: *azurefiles*

GET URI에는 필요한 모든 매개 변수가 있습니다. 추가 요청 본문이 필요하지 않습니다.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints?api-version=2019-05-13
```

### <a name="example-response"></a>예제 응답

GET URI가 제출되면 200개의 응답이 반환됩니다.

```http
HTTP/1.1" 200 None
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Transfer-Encoding': 'chunked'
'Content-Type': 'application/json'
'Content-Encoding': 'gzip'
'Expires': '-1'
'Vary': 'Accept-Encoding'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': 'd68d7951-7d97-4c49-9a2d-7fbaab55233a'
'x-ms-client-request-id': '4edb5a58-47ea-11ea-a27a-0a580af41908, 4edb5a58-47ea-11ea-a27a-0a580af41908'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'Server': 'Microsoft-IIS/10.0'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-reads': '11998'
'x-ms-correlation-request-id': 'd68d7951-7d97-4c49-9a2d-7fbaab55233a'
'x-ms-routing-request-id': 'WESTEUROPE:20200205T073708Z:d68d7951-7d97-4c49-9a2d-7fbaab55233a'
'Date': 'Wed, 05 Feb 2020 07:37:08 GMT'
{
“value”:[
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints/932881138555802864",
    "location": null,
    "name": "932881138555802864",
    "properties": {
      "fileShareSnapshotUri": "https://afsaccount.file.core.windows.net/azurefiles?sharesnapshot=2020-02-04T08:01:35.0000000Z",
      "objectType": "AzureFileShareRecoveryPoint",
      "recoveryPointSizeInGb": 1,
      "recoveryPointTime": "2020-02-04T08:01:35+00:00",
      "recoveryPointType": "FileSystemConsistent"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints"
  },
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints/932878582606969225",
    "location": null,
    "name": "932878582606969225",
    "properties": {
      "fileShareSnapshotUri": "https://afsaccount.file.core.windows.net/azurefiles?sharesnapshot=2020-02-03T08:05:30.0000000Z",
      "objectType": "AzureFileShareRecoveryPoint",
      "recoveryPointSizeInGb": 1,
      "recoveryPointTime": "2020-02-03T08:05:30+00:00",
      "recoveryPointType": "FileSystemConsistent"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints"
  },
  {
    "eTag": null,
    "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/recoveryPoints/932890167574511261",
    "location": null,
    "name": "932890167574511261",
    "properties": {
      "fileShareSnapshotUri": "https://afsaccount.file.core.windows.net/azurefiles?sharesnapshot=2020-02-02T08:03:50.0000000Z",
      "objectType": "AzureFileShareRecoveryPoint",
      "recoveryPointSizeInGb": 1,
      "recoveryPointTime": "2020-02-02T08:03:50+00:00",
      "recoveryPointType": "FileSystemConsistent"
    },
    "resourceGroup": "azurefiles",
    "tags": null,
    "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints"
  },
```

복구 지점은 위의 응답에서 {name} 필드로 식별됩니다.

## <a name="full-share-recovery-using-rest-api"></a>REST API를 사용한 전체 공유 복구

이 복원 옵션을 사용하여 원본 또는 대체 위치에서 전체 파일 공유를 복원합니다.
복원 트리거링은 POST 요청이며 REST API [를 복원하는 트리거를](https://docs.microsoft.com/rest/api/backup/restores/trigger) 사용하여 이 작업을 수행할 수 있습니다.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

{컨테이너Name} 및 {protectedItemName} 값은 [여기에](#fetch-containername-and-protecteditemname) 설정되어 있으며 recoveryPointID는 위에서 언급한 복구 지점의 {name} 필드입니다.

```http
POST https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare%3Bazurefiles/recoveryPoints/932886657837421071/restore?api-version=2019-05-13'
```

### <a name="create-request-body"></a>요청 본문 만들기

Azure 파일 공유에 대한 복원을 트리거하려면 요청 본문 구성 요소는 다음과 같습니다.

이름 |  Type   |   Description
--- | ---- | ----
속성 | AzureFileShare복원요청 | 복원요청리소스 속성

요청 본문 및 기타 세부 정보의 전체 정의 목록은 [REST API 문서 복원 트리거를](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body)참조하십시오.

### <a name="restore-to-original-location"></a>원래 위치로 복원

#### <a name="request-body-example"></a>요청 본문 예제

다음 요청 본문에서 Azure 파일 공유 복원을 트리거하는 데 필요한 속성을 정의합니다.

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "recoveryType":"OriginalLocation",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afsaccount",
      "copyOptions":"Overwrite",
      "restoreRequestType":"FullShareRestore"
}
}
```

### <a name="restore-to-alternate-location"></a>대체 위치에 복원

대체 위치 복구를 위해 다음 매개 변수를 지정합니다.

* **targetResourceId:** 백업된 콘텐츠가 복원되는 저장소 계정입니다. 대상 스토리지 계정은 자격 증명 모음과 동일한 위치에 있어야 합니다.
* **이름**: 백업된 콘텐츠가 복원되는 대상 저장소 계정 내의 파일 공유입니다.
* **targetFolderPath**: 데이터가 복원되는 파일 공유 아래의 폴더입니다.

#### <a name="request-body-example"></a>요청 본문 예제

다음 요청 본문에서는 *afsaccount* 저장소 계정의 *azurefiles* 파일 공유를 *afaccount1* 저장소 계정의 *azurefiles1* 파일 공유로 복원합니다.

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "recoveryType":"AlternateLocation",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afsaccount",
      "copyOptions":"Overwrite",
      "restoreRequestType":"FullShareRestore",
      "restoreFileSpecs":[
         {
            "targetFolderPath":"restoredata"
}
],
      "targetDetails":{
         "name":"azurefiles1",
         "targetResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afaccount1"
}
}
}
```

### <a name="response"></a>응답

복원 작업의 트리거링은 [비동기 작업입니다.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) 이 작업은 별도로 추적해야 하는 다른 작업을 만듭니다.
다른 작업이 생성될 때 202(허용됨)와 해당 작업이 완료되면 200(확인)의 두 개의 응답을 반환합니다.

#### <a name="response-example"></a>응답 예제

복원을 트리거하기 위해 *POST* URI를 제출하면 초기 응답은 위치 헤더 또는 Azure-비동기 헤더가 있는 202(허용됨)입니다.

```http
HTTP/1.1" 202
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Expires': '-1'
'Location': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/operationResults/68ccfbc1-a64f-4b29-b955-314b5790cfa9?api-version=2019-05-13'
'Retry-After': '60'
'Azure-AsyncOperation': 'https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare;azurefiles/operationsStatus/68ccfbc1-a64f-4b29-b955-314b5790cfa9?api-version=2019-05-13'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '2426777d-c5ec-44b6-a324-384f8947460c'
'x-ms-client-request-id': '3c743096-47eb-11ea-ae90-0a580af41908, 3c743096-47eb-11ea-ae90-0a580af41908'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-writes': '1198'
'x-ms-correlation-request-id': '2426777d-c5ec-44b6-a324-384f8947460c'
'x-ms-routing-request-id': 'WESTEUROPE:20200205T074347Z:2426777d-c5ec-44b6-a324-384f8947460c'
'Date': 'Wed, 05 Feb 2020 07:43:47 GMT'
```

그런 다음 GET 명령을 사용하여 위치 헤더 또는 Azure-AsyncOperation 헤더를 사용하여 결과 작업을 추적합니다.

```http
GET https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupOperations/68ccfbc1-a64f-4b29-b955-314b5790cfa9?api-version=2016-12-01
```

작업이 완료되면 응답 본문에서 결과 복원 작업의 ID를 사용하여 200(정상)을 반환합니다.

```http
HTTP/1.1" 200
'Cache-Control': 'no-cache'
'Pragma': 'no-cache'
'Transfer-Encoding': 'chunked'
'Content-Type': 'application/json'
'Content-Encoding': 'gzip'
'Expires': '-1'
'Vary': 'Accept-Encoding'
'X-Content-Type-Options': 'nosniff'
'x-ms-request-id': '41ee89b2-3be4-40d8-8ff6-f5592c2571e3'
'x-ms-client-request-id': '3c743096-47eb-11ea-ae90-0a580af41908, 3c743096-47eb-11ea-ae90-0a580af41908'
'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
'Server': 'Microsoft-IIS/10.0'
'X-Powered-By': 'ASP.NET'
'x-ms-ratelimit-remaining-subscription-reads': '11998'
'x-ms-correlation-request-id': '41ee89b2-3be4-40d8-8ff6-f5592c2571e3'
'x-ms-routing-request-id': 'WESTEUROPE:20200205T074348Z:41ee89b2-3be4-40d8-8ff6-f5592c2571e3'
'Date': 'Wed, 05 Feb 2020 07:43:47 GMT'
{
  "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/a7e97e42-4e54-4d4b-b449-26fcf946f42c",
  "location": null,
  "name": "a7e97e42-4e54-4d4b-b449-26fcf946f42c",
  "properties": {
    "actionsInfo": [
      "Cancellable"
    ],
    "activityId": "3c743096-47eb-11ea-ae90-0a580af41908",
    "backupManagementType": "AzureStorage",
    "duration": "0:00:01.863098",
    "endTime": null,
    "entityFriendlyName": "azurefiles",
    "errorDetails": null,
    "extendedInfo": {
      "dynamicErrorMessage": null,
      "propertyBag": {},
      "tasksList": []
    },
    "jobType": "AzureStorageJob",
    "operation": "Restore",
    "startTime": "2020-02-05T07:43:47.144961+00:00",
    "status": "InProgress",
    "storageAccountName": "afsaccount",
    "storageAccountVersion": "Storage"
  },
  "resourceGroup": "azurefiles",
  "tags": null,
  "type": "Microsoft.RecoveryServices/vaults/backupJobs"
}
```

대체 위치 복구의 경우 응답 본문에는 다음과 같이 표시됩니다.

```http
{
  "id": "/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupJobs/7e0ee41e-6e31-4728-a25c-98ff6b777641",
  "location": null,
  "name": "7e0ee41e-6e31-4728-a25c-98ff6b777641",
  "properties": {
    "actionsInfo": [
      "Cancellable"
    ],
    "activityId": "6077be6e-483a-11ea-a915-0a580af4ad72",
    "backupManagementType": "AzureStorage",
    "duration": "0:00:02.171965",
    "endTime": null,
    "entityFriendlyName": "azurefiles",
    "errorDetails": null,
    "extendedInfo": {
      "dynamicErrorMessage": null,
      "propertyBag": {
        "Data Transferred (in MB)": "0",
        "Job Type": "Recover to an alternate file share",
        "Number Of Failed Files": "0",
        "Number Of Restored Files": "0",
        "Number Of Skipped Files": "0",
        "RestoreDestination": "afaccount1/azurefiles1/restoredata",
        "Source File Share Name": "azurefiles",
        "Source Storage Account Name": "afsaccount",
        "Target File Share Name": "azurefiles1",
        "Target Storage Account Name": "afaccount1"
      },
      "tasksList": []
    },
    "jobType": "AzureStorageJob",
    "operation": "Restore",
    "startTime": "2020-02-05T17:10:18.106532+00:00",
    "status": "InProgress",
    "storageAccountName": "afsaccount",
    "storageAccountVersion": "ClassicCompute"
  },
  "resourceGroup": "azurefiles",
  "tags": null,
  "type": "Microsoft.RecoveryServices/vaults/backupJobs"
}
```

백업 작업은 장기 실행 작업이므로 [REST API를 사용한 모니터링 작업 문서](https://docs.microsoft.com/azure/backup/backup-azure-arm-userestapi-managejobs#tracking-the-job)에 설명된 대로 추적해야 합니다.

## <a name="item-level-recovery-using-rest-api"></a>REST API를 사용한 항목 수준 복구

이 복원 옵션을 사용하여 원본 또는 대체 위치에 있는 개별 파일 또는 폴더를 복원할 수 있습니다.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2019-05-13
```

{컨테이너Name} 및 {protectedItemName} 값은 [여기에](#fetch-containername-and-protecteditemname) 설정되어 있으며 recoveryPointID는 위에서 언급한 복구 지점의 {name} 필드입니다.

```http
POST https://management.azure.com/Subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault/backupFabrics/Azure/protectionContainers/StorageContainer;storage;azurefiles;afsaccount/protectedItems/AzureFileShare%3Bazurefiles/recoveryPoints/932886657837421071/restore?api-version=2019-05-13'
```

### <a name="create-request-body"></a>요청 본문 만들기

Azure 파일 공유에 대한 복원을 트리거하려면 요청 본문 구성 요소는 다음과 같습니다.

이름 |  Type   |   Description
--- | ---- | ----
속성 | AzureFileShare복원요청 | 복원요청리소스 속성

요청 본문 및 기타 세부 정보의 전체 정의 목록은 [REST API 문서 복원 트리거를](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body)참조하십시오.

### <a name="restore-to-original-location"></a>원래 위치로 복원

다음 요청 본문에는 *afsaccount* 저장소 계정의 *azurefiles* 파일 공유에 *Restoretest.txt* 파일을 복원하는 것입니다.

요청 본문 만들기

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "copyOptions":"Overwrite",
      "recoveryType":"OriginalLocation",
      "restoreFileSpecs":[
         {
            "fileSpecType":"File",
            "path":"RestoreTest.txt",
            "targetFolderPath":null
}
],
      "restoreRequestType":"ItemLevelRestore",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/azurefiles/providers/Microsoft.storage/storageAccounts/afsaccount",
      "targetDetails":null
}
}
```

### <a name="restore-to-alternate-location"></a>대체 위치에 복원

다음 요청 본문은 *afsaccount* 저장소 계정의 *azurefiles* 파일 공유에 있는 *Restoretest.txt* 파일을 *afaccount1* 저장소 계정의 *azurefiles1* 파일 공유의 *restoredata* 폴더로 복원하는 것입니다.

요청 본문 만들기

```json
{
   "properties":{
      "objectType":"AzureFileShareRestoreRequest",
      "recoveryType":"AlternateLocation",
      "sourceResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afsaccount",
      "copyOptions":"Overwrite",
      "restoreRequestType":"ItemLevelRestore",
      "restoreFileSpecs":[
         {
            "path":"Restore/RestoreTest.txt",
            "fileSpecType":"File",
            "targetFolderPath":"restoredata"
}
],
      "targetDetails":{
         "name":"azurefiles1",
         "targetResourceId":"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/AzureFiles/providers/Microsoft.Storage/storageAccounts/afaccount1"
}
}
}
```

[전체 공유 복원에](#full-share-recovery-using-rest-api)대해 위에서 설명한 것과 동일한 방식으로 응답을 처리해야 합니다.

## <a name="next-steps"></a>다음 단계

* [나머지 API를 사용하여 Azure 파일 공유 백업을 관리하는](manage-azure-file-share-rest-api.md)방법에 대해 알아봅니다.
