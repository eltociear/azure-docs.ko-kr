---
title: CLI를 사용 하 여 Azure 가상 머신에 대 한 유지 관리 제어
description: 유지 관리 제어 및 CLI를 사용 하 여 Azure Vm에 유지 관리를 적용 하는 시기를 제어 하는 방법을 알아봅니다.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/20/2020
ms.author: cynthn
ms.openlocfilehash: 4843b4769e31748fd5f624005792c604db18f11e
ms.sourcegitcommit: 1ed0230c48656d0e5c72a502bfb4f53b8a774ef1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82137504"
---
# <a name="control-updates-with-maintenance-control-and-the-azure-cli"></a>유지 관리 제어 및 Azure CLI를 사용 하 여 업데이트 제어

유지 관리 제어를 통해 격리 된 Vm 및 Azure 전용 호스트에 업데이트를 적용 하는 시기를 결정할 수 있습니다. 이 항목에서는 유지 관리 제어를 위한 Azure CLI 옵션을 설명 합니다. 유지 관리 제어, 제한 사항 및 기타 관리 옵션을 사용 하는 이점에 대 한 자세한 내용은 [유지 관리 제어를 사용 하 여 플랫폼 업데이트 관리](maintenance-control.md)를 참조 하세요.

## <a name="create-a-maintenance-configuration"></a>유지 관리 구성 만들기

를 `az maintenance configuration create` 사용 하 여 유지 관리 구성을 만듭니다. 이 예제에서는 호스트로 범위가 지정 된 *Myconfig* 라는 유지 관리 구성을 만듭니다. 

```azurecli-interactive
az group create \
   --location eastus \
   --name myMaintenanceRG
az maintenance configuration create \
   -g myMaintenanceRG \
   --name myConfig \
   --maintenanceScope host\
   --location eastus
```

나중에 사용 하려면 출력에서 구성 ID를 복사 합니다.

를 `--maintenanceScope host` 사용 하면 호스트에 대 한 업데이트를 제어 하는 데 유지 관리 구성이 사용 됩니다.

이름이 같은 구성을 만들려고 하지만 다른 위치에 있는 경우 오류가 발생 합니다. 구성 이름은 구독에 대해 고유 해야 합니다.

을 사용 하 여 `az maintenance configuration list`사용 가능한 유지 관리 구성을 쿼리할 수 있습니다.

```azurecli-interactive
az maintenance configuration list --query "[].{Name:name, ID:id}" -o table 
```

## <a name="assign-the-configuration"></a>구성 할당

를 `az maintenance assignment create` 사용 하 여 격리 된 VM 또는 Azure 전용 호스트에 구성을 할당 합니다.

### <a name="isolated-vm"></a>격리 된 VM

구성의 ID를 사용 하 여 VM에 구성을 적용 합니다. 에 `--resource-type virtualMachines` vm `--resource-name`의 이름을 지정 하 고에서 `--resource-group`vm에 대 한 리소스 그룹을 지정 하 고에 대 한 `--location`vm의 위치를 제공 합니다. 

```azurecli-interactive
az maintenance assignment create \
   --resource-group myMaintenanceRG \
   --location eastus \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   --configuration-assignment-name myConfig \
   --maintenance-configuration-id "/subscriptions/1111abcd-1a11-1a2b-1a12-123456789abc/resourcegroups/myMaintenanceRG/providers/Microsoft.Maintenance/maintenanceConfigurations/myConfig"
```

### <a name="dedicated-host"></a>전용 호스트

전용 호스트에 구성을 적용 하려면 호스트 그룹의 이름 및 `--resource-type hosts` `--resource-parent-name` `--resource-parent-type hostGroups`를 포함 하 여을 포함 해야 합니다. 

매개 변수 `--resource-id` 는 호스트의 ID입니다. [Az vm host get instance-view](/cli/azure/vm/host#az-vm-host-get-instance-view) 를 사용 하 여 전용 호스트의 ID를 가져올 수 있습니다.

```azurecli-interactive
az maintenance assignment create \
   -g myDHResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --configuration-assignment-name myConfig \
   --maintenance-configuration-id "/subscriptions/1111abcd-1a11-1a2b-1a12-123456789abc/resourcegroups/myDhResourceGroup/providers/Microsoft.Maintenance/maintenanceConfigurations/myConfig" \
   -l eastus \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups 
```

## <a name="check-configuration"></a>구성 확인

구성이 올바르게 적용 되었는지 확인 하거나를 사용 하 여 `az maintenance assignment list`현재 적용 되는 구성을 확인할 수 있습니다.

### <a name="isolated-vm"></a>격리 된 VM

```azurecli-interactive
az maintenance assignment list \
   --provider-name Microsoft.Compute \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --query "[].{resource:resourceGroup, configName:name}" \
   --output table
```

### <a name="dedicated-host"></a>전용 호스트 

```azurecli-interactive
az maintenance assignment list \
   --resource-group myDHResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups 
   --query "[].{ResourceGroup:resourceGroup,configName:name}" \
   -o table
```


## <a name="check-for-pending-updates"></a>보류 중인 업데이트 확인

보류 `az maintenance update list` 중인 업데이트가 있는지 확인 하려면를 사용 합니다. 업데이트--구독이 VM을 포함 하는 구독의 ID가 되도록 합니다.

업데이트가 없는 경우 명령은 라는 텍스트를 포함 하는 오류 메시지를 반환 합니다 `Resource not found...StatusCode: 404`.

업데이트가 있는 경우 보류 중인 업데이트가 여러 개 있더라도 하나만 반환 됩니다. 이 업데이트에 대 한 데이터는 개체에서 반환 됩니다.

```text
[
  {
    "impactDurationInSec": 9,
    "impactType": "Freeze",
    "maintenanceScope": "Host",
    "notBefore": "2020-03-03T07:23:04.905538+00:00",
    "resourceId": "/subscriptions/9120c5ff-e78e-4bd0-b29f-75c19cadd078/resourcegroups/DemoRG/providers/Microsoft.Compute/hostGroups/demoHostGroup/hosts/myHost",
    "status": "Pending"
  }
]
  ```

### <a name="isolated-vm"></a>격리 된 VM

격리 된 VM에 대 한 보류 중인 업데이트를 확인 합니다. 이 예제에서 출력은 가독성을 위해 테이블 형식으로 지정 됩니다.

```azurecli-interactive
az maintenance update list \
   -g myMaintenanceRg \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   -o table
```

### <a name="dedicated-host"></a>전용 호스트

전용 호스트의 보류 중인 업데이트를 확인 합니다. 이 예제에서 출력은 가독성을 위해 테이블 형식으로 지정 됩니다. 리소스의 값을 고유한 값으로 바꿉니다.

```azurecli-interactive
az maintenance update list \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myHostResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parentname myHostGroup \
   --resource-parent-type hostGroups \
   -o table
```

## <a name="apply-updates"></a>업데이트 적용

보류 `az maintenance apply update` 중인 업데이트를 적용 하는 데 사용 합니다. 성공할 경우이 명령은 업데이트의 세부 정보를 포함 하는 JSON을 반환 합니다.

### <a name="isolated-vm"></a>격리 된 VM

격리 된 VM에 업데이트를 적용 하는 요청을 만듭니다.

```azurecli-interactive
az maintenance applyupdate create \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute
```


### <a name="dedicated-host"></a>전용 호스트

전용 호스트에 업데이트를 적용 합니다.

```azurecli-interactive
az maintenance applyupdate create \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   --resource-group myHostResourceGroup \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \
   --resource-parent-type hostGroups
```

## <a name="check-the-status-of-applying-updates"></a>업데이트 적용 상태 확인 

을 사용 하 여 `az maintenance applyupdate get`업데이트의 진행률을 확인할 수 있습니다. 

업데이트 이름으로 `default` 를 사용 하 여 마지막 업데이트에 대 한 결과를 보거나를 실행할 `myUpdateName` `az maintenance applyupdate create`때 반환 된 업데이트의 이름으로 바꿀 수 있습니다.

```text
Status         : Completed
ResourceId     : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso
LastUpdateTime : 1/1/2020 12:00:00 AM
Id             : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso/providers/Microsoft.Maintenance/applyUpdates/default
Name           : default
Type           : Microsoft.Maintenance/applyUpdates
```
LastUpdateTime은 자동 유지 관리 기간을 사용 하지 않는 경우 사용자 또는 플랫폼에 의해 시작 된 업데이트가 완료 된 시간입니다. 유지 관리 제어를 통해 업데이트를 적용 한 적이 없는 경우 기본값을 표시 합니다.

### <a name="isolated-vm"></a>격리 된 VM

```azurecli-interactive
az maintenance applyupdate get \
   --resource-group myMaintenanceRG \
   --resource-name myVM \
   --resource-type virtualMachines \
   --provider-name Microsoft.Compute \
   --apply-update-name default 
```

### <a name="dedicated-host"></a>전용 호스트

```azurecli-interactive
az maintenance applyupdate get \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \ 
   --resource-group myMaintenanceRG \
   --resource-name myHost \
   --resource-type hosts \
   --provider-name Microsoft.Compute \
   --resource-parent-name myHostGroup \ 
   --resource-parent-type hostGroups \
   --apply-update-name myUpdateName \
   --query "{LastUpdate:lastUpdateTime, Name:name, ResourceGroup:resourceGroup, Status:status}" \
   --output table
```


## <a name="delete-a-maintenance-configuration"></a>유지 관리 구성 삭제

유지 `az maintenance configuration delete` 관리 구성을 삭제 하려면를 사용 합니다. 구성을 삭제 하면 연결 된 리소스에서 유지 관리 컨트롤이 제거 됩니다.

```azurecli-interactive
az maintenance configuration delete \
   --subscription 1111abcd-1a11-1a2b-1a12-123456789abc \
   -g myResourceGroup \
   --name myConfig
```

## <a name="next-steps"></a>다음 단계
자세히 알아보려면 [유지 관리 및 업데이트](maintenance-and-updates.md)를 참조 하세요.
