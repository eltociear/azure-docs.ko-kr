---
title: Azure CLI를 사용하여 Azure HDInsight 클러스터 관리
description: Azure CLI를 사용하여 Azure HDInsight 클러스터를 관리하는 방법을 알아봅니다. 클러스터 유형에는 아파치 하두프, 스파크, HBase, 스톰, 카프카, 대화형 쿼리 및 ML 서비스가 포함됩니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 02/26/2020
ms.openlocfilehash: 2c6495454e5ba2449d4b3c74a096681f74610813
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272774"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Azure CLI를 사용하여 Azure HDInsight 클러스터 관리

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

[Azure CLI를](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) 사용하여 Azure HDInsight 클러스터를 관리하는 방법을 알아봅니다. Azure CLI(명령줄 인터페이스)는 Azure 리소스를 관리하기 위한 Microsoft의 플랫폼 간 명령줄 환경입니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="prerequisites"></a>사전 요구 사항

* Azure CLI. Azure CLI를 설치하지 않은 경우 [Azure CLI 설치](https://docs.microsoft.com/cli/azure/install-azure-cli)에 나온 단계를 참조하세요.

* HDInsight의 Apache Hadoop 클러스터. [리눅스에서 HDInsight로 시작하기를](hadoop/apache-hadoop-linux-tutorial-get-started.md)참조하십시오.

## <a name="connect-to-azure"></a>Azure에 연결

Azure 구독에 로그인합니다. Azure Cloud Shell을 사용하려는 경우 코드 블록의 오른쪽 위 모서리에서 **시도를** 선택합니다. 그렇지 않은 경우 아래 명령을 입력합니다.

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>클러스터 나열

[az hdinsight 목록을](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) 사용하여 클러스터를 나열합니다. 리소스 그룹의 이름으로 바꿉니다 `RESOURCE_GROUP_NAME` 아래 명령을 편집한 다음 명령을 입력합니다.

```azurecli-interactive
# List all clusters in the current subscription
az hdinsight list

# List only cluster name and its resource group
az hdinsight list --query "[].{Cluster:name, ResourceGroup:resourceGroup}" --output table

# List all cluster for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME

# List all cluster names for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME --query "[].{clusterName:name}" --output table
```

## <a name="show-cluster"></a>클러스터 표시

[az hdinsight 쇼를](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) 사용하여 지정된 클러스터에 대한 정보를 표시합니다. 을 대체하고 `RESOURCE_GROUP_NAME` `CLUSTER_NAME` 관련 정보로 다음 명령을 입력하여 아래 명령을 편집합니다.

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>클러스터 삭제

[az hdinsight 삭제를](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) 사용하여 지정된 클러스터를 삭제합니다. 을 대체하고 `RESOURCE_GROUP_NAME` `CLUSTER_NAME` 관련 정보로 다음 명령을 입력하여 아래 명령을 편집합니다.

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

또한 클러스터를 포함하는 리소스 그룹을 삭제하여 클러스터를 삭제할 수도 있습니다. 이렇게 하면 기본 저장소 계정을 포함하여 그룹의 모든 리소스가 삭제됩니다.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>클러스터 크기 조정

[az hdinsight 크기를](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) 조정하여 지정된 HDInsight 클러스터의 크기를 지정된 크기로 조정합니다. 을 대체하고 `RESOURCE_GROUP_NAME` `CLUSTER_NAME` 관련 정보로 아래 명령을 편집합니다. 클러스터에 원하는 작업자 노드 수로 바꿉습니다. `WORKERNODE_COUNT` 클러스터 확장에 대한 자세한 내용은 [HDInsight 클러스터 확장을](./hdinsight-scaling-best-practices.md)참조하십시오. 다음 명령을 입력합니다.

```azurecli-interactive
az hdinsight resize --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --workernode-count WORKERNODE_COUNT
```

## <a name="next-steps"></a>다음 단계

이 문서에서는 다른 HDInsight 클러스터 관리 작업을 수행하는 방법을 배웠습니다. 자세히 알아보려면 다음 아티클을 참조하세요.

* [Azure Portal을 사용하여 HDInsight의 Apache Hadoop 클러스터 관리](hdinsight-administer-use-portal-linux.md)
* [Azure PowerShell을 사용하여 HDInsight 클러스터 관리](hdinsight-administer-use-powershell.md)
* [Azure HDInsight 시작](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure CLI 시작](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)
