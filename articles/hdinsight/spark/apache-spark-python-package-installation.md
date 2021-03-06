---
title: Azure HDInsight에서 Jupyter를 가진 파이썬 패키지에 대한 스크립트 작업
description: 스크립트 작업을 사용하여 HDInsight Spark 클러스터와 함께 제공되는 Jupyter 노트북에서 외부 python 패키지를 사용하도록 구성하는 방법에 대한 단계별 지침입니다.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 659af8b85cb3736d663e79676b04af8041aeabfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129658"
---
# <a name="safely-manage-python-environment-on-azure-hdinsight-using-script-action"></a>스크립트 작업을 사용하여 Azure HDInsight에서 Python 환경을 안전하게 관리

> [!div class="op_single_selector"]
> * [셀 매직 사용](apache-spark-jupyter-notebook-use-external-packages.md)
> * [스크립트 작업 사용](apache-spark-python-package-installation.md)

HDInsight는 스파크 클러스터에 두 개의 내장 파이썬 설치, 아나콘다 파이썬 2.7 과 파이썬 3.5. 경우에 따라 고객은 외부 파이썬 패키지 또는 다른 파이썬 버전을 설치하는 것과 같이 파이썬 환경을 사용자 지정해야합니다. 이 문서에서는 HDInsight의 [아파치 스파크](./apache-spark-overview.md) 클러스터에 대한 파이썬 환경을 안전하게 관리하는 모범 사례를 보여 줍니다.

## <a name="prerequisites"></a>사전 요구 사항

HDInsight의 Apache Spark. 자세한 내용은 [Azure HDInsight에서 Apache Spark 클러스터 만들기](apache-spark-jupyter-spark-sql.md)를 참조하세요. HDInsight에 Spark 클러스터가 아직 없는 경우 클러스터를 만드는 동안 스크립트 작업을 실행할 수 있습니다. [사용자 지정 스크립트 작업을 사용하는 방법](../hdinsight-hadoop-customize-cluster-linux.md)에 대한 설명서를 참조하세요.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight 클러스터에서 사용하는 오픈 소스 소프트웨어 지원

Microsoft Azure HDInsight 서비스는 Apache Hadoop으로 형성된 오픈 소스 기술의 에코시스템을 사용합니다. Microsoft Azure는 오픈 소스 기술에 대한 일반 수준의 지원을 제공합니다. 자세한 내용은 [Azure 지원 FAQ 웹 사이트를](https://azure.microsoft.com/support/faq/)참조하십시오. HDInsight 서비스는 기본 제공 구성 요소에 대해 추가 수준의 지원을 제공합니다.

HDInsight 서비스에서 사용할 수 있는 오픈 소스 구성 요소에는 두 가지 유형이 있습니다.

|구성 요소 |설명 |
|---|---|
|기본 제공|이러한 구성 요소는 HDInsight 클러스터에 사전 설치되며 클러스터의 핵심 기능을 제공합니다. 예를 들어 아파치 하두롭 YARN 리소스 관리자, 아파치 하이브 쿼리 언어(HiveQL) 및 Mahout 라이브러리는 이 범주에 속합니다. 클러스터 구성 요소의 전체 목록은 [HDInsight에서 제공하는 Apache Hadoop 클러스터 버전의 새로운 기능](../hdinsight-component-versioning.md)에 있습니다.|
|사용자 지정|클러스터의 사용자로서 커뮤니티에서 사용 가능하거나 사용자가 만든 구성 요소를 워크로드에 설치하거나 사용할 수 있습니다.|

> [!IMPORTANT]
> HDInsight 클러스터에 제공되는 구성 요소는 완벽히 지원됩니다. Microsoft 지원은 이러한 구성 요소와 관련된 문제를 격리하고 해결하도록 도와줍니다.
>
> 사용자 지정 구성 요소는 문제 해결에 도움이 되는 합리적인 지원을 받습니다. Microsoft 지원을 통해 문제를 해결할 수 있습니다. 또는 해당 기술에 대한 전문 지식이 있는 오픈 소스 기술에 대해 사용 가능한 채널에 참여하도록 요청할 수 있습니다. 예를 들어, 다음과 같은 많은 커뮤니티 사이트가 있습니다: [HDInsight에 대한 MSDN 포럼](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). 또한 아파치 프로젝트에는 [https://apache.org](https://apache.org)다음과 같은 프로젝트 사이트가 있습니다. [Hadoop](https://hadoop.apache.org/)

## <a name="understand-default-python-installation"></a>기본 파이썬 설치 이해

HDInsight 스파크 클러스터는 아나콘다 설치로 만들어집니다. 클러스터에는 아나콘다 파이썬 2.7과 파이썬 3.5의 두 개의 파이썬 설치가 있습니다. 아래 표는 스파크, 리비 및 주피터의 기본 파이썬 설정을 보여 주며 있습니다.

| |Python 2.7|Python 3.5|
|----|----|----|
|경로|/usr/bin/아나콘다/빈|/usr/bin/아나콘다/엔vs/py35/빈|
|Spark|기본값은 2.7로 설정됩니다.|해당 없음|
|Livy|기본값은 2.7로 설정됩니다.|해당 없음|
|Jupyter|파이스파크 커널|파이스파크3 커널|

## <a name="safely-install-external-python-packages"></a>외부 파이썬 패키지를 안전하게 설치

HDInsight 클러스터는 파이썬 2.7과 파이썬 3.5 모두 내장 된 파이썬 환경에 따라 달라집니다. 이러한 기본 기본 제공 환경에 사용자 지정 패키지를 직접 설치하면 예기치 않은 라이브러리 버전 변경이 발생하고 클러스터가 더 이상 중단될 수 있습니다. Spark 응용 프로그램에 대한 사용자 지정 외부 파이썬 패키지를 안전하게 설치하려면 아래 단계를 따르십시오.

1. 콘다를 사용하여 파이썬 가상 환경을 만듭니다. 가상 환경은 다른 환경을 손상시키지 않고 프로젝트에 격리된 공간을 제공합니다. 파이썬 가상 환경을 만들 때 사용할 파이썬 버전을 지정할 수 있습니다. 파이썬 2.7 과 3.5를 사용하려는 경우에도 가상 환경을 만들어야합니다. 이는 클러스터의 기본 환경이 끊어지지 않도록 하기 위한 것입니다. 아래 스크립트를 사용하여 모든 노드에 대해 클러스터에서 스크립트 작업을 실행하여 Python 가상 환경을 만듭니다.

    -   `--prefix`에서는 conda 가상 환경이 서식하는 경로를 지정합니다. 여기에 지정된 경로에 따라 추가로 변경해야 하는 몇 가지 구성이 있습니다. 이 예제에서는 클러스터에 py35라는 기존 가상 환경이 이미 있으므로 py35new를 사용합니다.
    -   `python=`가상 환경에 대한 파이썬 버전을 지정합니다. 이 예제에서는 클러스터가 하나에 내장된 것과 동일한 버전인 버전 3.5를 사용합니다. 다른 파이썬 버전을 사용하여 가상 환경을 만들 수도 있습니다.
    -   `anaconda`가상 환경에 아나콘다 패키지를 설치하기 위해 package_spec 아나콘다로 지정합니다.
    
    ```bash
    sudo /usr/bin/anaconda/bin/conda create --prefix /usr/bin/anaconda/envs/py35new python=3.5 anaconda --yes
    ```

2. 필요한 경우 생성된 가상 환경에 외부 파이썬 패키지를 설치합니다. 아래 스크립트를 사용하여 모든 노드에 대해 클러스터에서 스크립트 작업을 실행하여 외부 Python 패키지를 설치합니다. 가상 환경 폴더에 파일을 작성하려면 sudo 권한이 필요합니다.

    사용할 수 있는 패키지의 전체 목록은 [패키지 인덱스](https://pypi.python.org/pypi)를 검색할 수 있습니다. 다른 소스에서 사용 가능한 패키지 목록을 가져올 수도 있습니다. 예를 들어 [conda-forge](https://conda-forge.org/feedstocks/)를 통해 제공되는 패키지를 설치할 수 있습니다.

    최신 버전으로 라이브러리를 설치하려면 아래 명령을 사용합니다.

    - 콘다 채널 사용:

        -   `seaborn`은 설치하려는 패키지 이름입니다.
        -   `-n py35new`방금 생성되는 가상 환경 이름을 지정합니다. 가상 환경 만들기에 따라 해당 이름을 변경해야 합니다.

        ```bash
        sudo /usr/bin/anaconda/bin/conda install seaborn -n py35new --yes
        ```

    - 또는 파이파이 리포지토리를 사용하여 변경하고 `seaborn` `py35new` 그에 따라 다음을 수행하십시오.
        ```bash
        sudo /usr/bin/anaconda/env/py35new/bin/pip install seaborn
        ```

    특정 버전으로 라이브러리를 설치하려면 아래 명령을 사용합니다.

    - 콘다 채널 사용:

        -   `numpy=1.16.1`설치하려는 패키지 이름 및 버전입니다.
        -   `-n py35new`방금 생성되는 가상 환경 이름을 지정합니다. 가상 환경 만들기에 따라 해당 이름을 변경해야 합니다.

        ```bash
        sudo /usr/bin/anaconda/bin/conda install numpy=1.16.1 -n py35new --yes
        ```

    - 또는 파이파이 리포지토리를 사용하여 변경하고 `numpy==1.16.1` `py35new` 그에 따라 다음을 수행하십시오.

        ```bash
        sudo /usr/bin/anaconda/env/py35new/bin/pip install numpy==1.16.1
        ```

    가상 환경 이름을 모르는 경우 SSH를 클러스터의 헤드 노드로 실행하고 `/usr/bin/anaconda/bin/conda info -e` 모든 가상 환경을 표시할 수 있습니다.

3. 스파크 및 리비 구성을 변경하고 생성된 가상 환경을 가리킵니다.

    1. Ambari UI를 열고 Spark2 페이지, 구성 탭으로 이동합니다.

        ![암바리를 통해 스파크와 리비 구성 변경](./media/apache-spark-python-package-installation/ambari-spark-and-livy-config.png)

    2. 고급 livy2-env를 확장하고 아래 문을 아래에 추가합니다. 다른 접두사를 가진 가상 환경을 설치한 경우 그에 따라 경로를 변경합니다.

        ```
        export PYSPARK_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        export PYSPARK_DRIVER_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        ```

        ![암바리를 통해 리비 구성 변경](./media/apache-spark-python-package-installation/ambari-livy-config.png)

    3. 고급 spark2-env확장, 하단에 기존 내보내기 PYSPARK_PYTHON 문을 대체합니다. 다른 접두사를 가진 가상 환경을 설치한 경우 그에 따라 경로를 변경합니다.

        ```
        export PYSPARK_PYTHON=${PYSPARK_PYTHON:-/usr/bin/anaconda/envs/py35new/bin/python}
        ```

        ![암바리를 통한 스파크 구성 변경](./media/apache-spark-python-package-installation/ambari-spark-config.png)

    4. 변경 내용을 저장하고 영향을 받는 서비스를 다시 시작합니다. 이러한 변경 사항은 Spark2 서비스를 다시 시작해야 합니다. Ambari UI는 필요한 다시 시작 알림을 묻는 메시지가 표시됩니다, 모든 영향을 받는 서비스를 다시 시작하려면 다시 시작을 클릭합니다.

        ![암바리를 통한 스파크 구성 변경](./media/apache-spark-python-package-installation/ambari-restart-services.png)

4. Jupyter에서 새로 만든 가상 환경을 사용하려는 경우. 주피터 구성을 변경하고 주피터를 다시 시작해야 합니다. 아래 문으로 모든 헤더 노드에서 스크립트 작업을 실행하여 Jupyter가 새로 생성된 가상 환경을 가리킵니다. 가상 환경에 대해 지정한 접두사에 대한 경로를 수정해야 합니다. 이 스크립트 작업을 실행한 후 Ambari UI를 통해 Jupyter 서비스를 다시 시작하여 이 변경 을 사용할 수 있도록 합니다.

    ```bash
    sudo sed -i '/python3_executable_path/c\ \"python3_executable_path\" : \"/usr/bin/anaconda/envs/py35new/bin/python3\"' /home/spark/.sparkmagic/config.json
    ```

    아래 코드를 실행하여 Jupyter 노트북에서 파이썬 환경을 두 번 확인할 수 있습니다.

    ![주피터 노트북에서 파이썬 버전 확인](./media/apache-spark-python-package-installation/check-python-version-in-jupyter.png)

## <a name="known-issue"></a>알려진 문제

아나콘다 버전 4.7.11, 4.7.12 및 4.8.0에 알려진 버그가 있습니다. 스크립트 작업이 에 걸려 `"Collecting package metadata (repodata.json): ...working..."` 실패하는 `"Python script has been killed due to timeout after waiting 3600 secs"`경우. [이 스크립트를](https://gregorysfixes.blob.core.windows.net/public/fix-conda.sh) 다운로드하여 모든 노드에서 스크립트 작업으로 실행하여 문제를 해결할 수 있습니다.

Anaconda 버전을 확인하려면 클러스터 헤더 노드로 SSH를 `/usr/bin/anaconda/bin/conda --v`실행하고 실행할 수 있습니다.

## <a name="next-steps"></a>다음 단계

* [개요: Azure HDInsight에서 Apache Spark](apache-spark-overview.md)
* [BI와 Apache Spark: BI 도구와 함께 HDInsight의 Spark를 사용하여 대화형 데이터 분석 수행](apache-spark-use-bi-tools.md)
* [Azure HDInsight에서 Apache Spark 클러스터에 대한 리소스 관리](apache-spark-resource-manager.md)
* [HDInsight의 Apache Spark 클러스터에서 실행되는 작업 추적 및 디버그](apache-spark-job-debugging.md)
