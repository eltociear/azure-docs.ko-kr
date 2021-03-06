---
title: Visual Studio Code-Azure IoT Edge를 사용 하 여 규모에 모듈 배포
description: Visual Studio Code 용 IoT 확장을 사용 하 여 IoT Edge 장치 그룹에 대 한 자동 배포를 만듭니다.
keywords: ''
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 1/8/2020
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 92540c57179ae0198f78b588681167fe48097362
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134365"
---
# <a name="deploy-iot-edge-modules-at-scale-using-visual-studio-code"></a>Visual Studio Code를 사용 하 여 규모에 IoT Edge 모듈 배포

Visual Studio Code를 사용 하 여 여러 장치에 대 한 지속적인 배포를 한 번에 관리 하는 **IoT Edge 자동 배포** 를 만들 수 있습니다. IoT Edge에 대 한 자동 배포는 IoT Hub [자동 장치 관리](/azure/iot-hub/iot-hub-automatic-device-management) 기능의 일부입니다. 배포는 여러 장치에 여러 모듈을 배포 하는 데 사용할 수 있는 동적 프로세스입니다. 모듈의 상태와 상태를 추적 하 고 필요한 경우 변경할 수도 있습니다.

자세한 내용은 [단일 장치에 대 한 자동 배포의 이해 또는 규모에 IoT Edge](module-deployment-monitoring.md)을 참조 하세요.

이 문서에서는 Visual Studio Code 및 IoT 확장을 설정 합니다. 그런 다음 IoT Edge 장치 집합에 모듈을 배포 하는 방법을 알아봅니다.

## <a name="prerequisites"></a>필수 구성 요소

* Azure 구독의 [IoT hub](../iot-hub/iot-hub-create-through-portal.md)
* IoT Edge 런타임이 설치된 [IoT Edge 디바이스](how-to-register-device.md#register-with-visual-studio-code)
* [Visual Studio Code](https://code.visualstudio.com/)
* Visual Studio Code 용 [Azure IoT 도구](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools#overview) 입니다.

## <a name="sign-in-to-access-your-iot-hub"></a>로그인하여 IoT Hub에 액세스

Visual Studio Code에 대 한 Azure IoT 확장을 사용 하 여 허브를 통해 작업을 수행할 수 있습니다. 이러한 작업을 수행 하려면 Azure 계정에 로그인 하 고 작업 중인 IoT hub를 선택 해야 합니다.

1. Visual Studio Code에서 **탐색기** 보기를 엽니다.

1. 탐색기 아래쪽에서 **Azure IoT Hub** 섹션을 확장 합니다.

1. **Azure IoT Hub** 섹션 헤더에서 **...** 를 클릭 합니다. 줄임표가 표시되지 않으면 헤더를 마우스로 가리킵니다.

1. **IoT Hub 선택**을 선택합니다.

1. Azure 계정에 로그인 하지 않은 경우 프롬프트에 따라 작업을 수행 합니다.

1. Azure 구독을 선택합니다.

1. IoT Hub를 선택합니다.

## <a name="configure-a-deployment-manifest"></a>배포 매니페스트 구성

배포 매니페스트는 배포할 모듈을 설명 하는 JSON 문서입니다. 또한 모듈 간의 데이터 흐름과 모듈 쌍의 desired 속성을 설명 합니다. 자세한 내용은 [모듈을 배포 하 고 IoT Edge에서 경로를 설정 하는 방법 알아보기](module-composition.md)를 참조 하세요.

Visual Studio Code를 사용하여 모듈을 배포하려면 배포 매니페스트를 로컬에 .JSON 파일로 저장합니다. 장치에 구성을 적용 하는 명령을 실행할 때 해당 위치를 제공 해야 합니다.

예를 들어 한 개의 모듈이 있는 기본 배포 매니페스트의 예제는 다음과 같습니다.

```json
{
  "content": {
    "modulesContent": {
      "$edgeAgent": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "runtime": {
            "type": "docker",
            "settings": {
              "minDockerVersion": "v1.25",
              "loggingOptions": "",
              "registryCredentials": {}
            }
          },
          "systemModules": {
            "edgeAgent": {
              "type": "docker",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
                "createOptions": "{}"
              }
            },
            "edgeHub": {
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
                "createOptions": "{\"HostConfig\":{\"PortBindings\":{\"5671/tcp\":[{\"HostPort\":\"5671\"}],\"8883/tcp\":[{\"HostPort\":\"8883\"}],\"443/tcp\":[{\"HostPort\":\"443\"}]}}}"
              }
            }
          },
          "modules": {
            "SimulatedTemperatureSensor": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
                "createOptions": "{}"
              }
            }
          }
        }
      },
      "$edgeHub": {
        "properties.desired": {
          "schemaVersion": "1.0",
          "routes": {
            "upstream": "FROM /messages/* INTO $upstream"
          },
          "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
          }
        }
      },
      "SimulatedTemperatureSensor": {
        "properties.desired": {
          "SendData": true,
          "SendInterval": 5
        }
      }
    }
  }
}
```

현재 구성할 수 있는 IoT Edge 장치를 확인 해야 하는 경우 **IoT Edge: 장치 정보 가져오기** 명령을 실행 합니다.

## <a name="identify-devices-with-target-conditions"></a>대상 조건이 있는 장치 식별

배포를 받을 IoT Edge 장치를 식별 하려면 대상 조건을 지정 해야 합니다. 지정 된 조건이 deviceId, tag 값 또는 보고 된 속성 값과 일치 하는 경우 대상 조건이 충족 됩니다.

장치 쌍의 태그를 구성 합니다. 태그를 포함 하는 장치 쌍의 예는 다음과 같습니다.

```json
"tags":{
  "location":{
    "building": "20",
    "floor": "2"
  },
  "roomtype": "conference",
  "environment": "prod"
}
```

배포에 대 한 대상 조건에와 `tag.location.building = '20'`같은 태그 값 중 하 나와 일치 하는 식이 포함 된 경우이 장치는 배포를 수신 합니다.

태그 또는 다른 값에 관계 없이 특정 장치를 대상으로 지정 하려면 대상 조건에 대해를 `deviceId` 지정 하면 됩니다.

다음은 몇 가지 추가 예입니다.

* deviceId ='linuxprod1'
* deviceId = ' linuxprod1 ' 또는 deviceId = ' linuxprod2 ' 또는 deviceId = ' linuxprod3 '
* tags.environment ='prod'
* tags. environment = ' prod ' 및 태그. location = ' westus2 '
* tags. environment = ' prod ' 또는 태그. location = ' westus2 '
* tags. operator = ' John ' 및 태그. environment = ' prod ' 및 no deviceId = ' linuxprod1 '

자세한 내용은 [대상 조건](module-deployment-monitoring.md#target-condition) 을 참조 하세요. 디바이스 쌍 및 태그에 대한 자세한 내용은 [IoT Hub의 디바이스 쌍 이해 및 사용](../iot-hub/iot-hub-devguide-device-twins.md)을 참조하세요.

### <a name="edit-the-device-twin"></a>장치 쌍 편집

Visual Studio Code에서 장치 쌍을 편집 하 여 태그를 구성할 수 있습니다. **보기** 메뉴에서 **명령 팔레트** 를 선택 하 고 **IoT Edge: 장치 쌍 편집** 명령을 실행 합니다. IoT Edge 장치를 선택 하면 장치 쌍이 표시 됩니다.

이 예제에서는 태그가 정의 되지 않았습니다. 현재 빈 섹션 `"tags": {}` 을 원하는 태그 정의로 바꿉니다.

```json
{
    "deviceId": "myEdgeDevice",
    "etag": "AAAAAAAAAAE=",
    "deviceEtag": "NTgwMDg5MDAz",
    "status": "enabled",
    "statusUpdateTime": "0001-01-01T00:00:00Z",
    "connectionState": "Disconnected",
    "lastActivityTime": "0001-01-01T00:00:00Z",
    "cloudToDeviceMessageCount": 0,
    "authenticationType": "sas",
    "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
    },
    "version": 2,
    "properties": {
        "desired": {
            "$metadata": {
                "$lastUpdated": "2019-12-29T00:58:49.9315265Z"
            },
            "$version": 1
        },
        "reported": {
            "$metadata": {
                "$lastUpdated": "2019-12-29T00:58:49.9315265Z"
            },
            "$version": 1
        }
    },
    "capabilities": {
        "iotEdge": true
    },
    "deviceScope": "ms-azure-iot-edge://myEdgeDevice-637131779299315265",
    "tags": {}
}
```

로컬 파일을 저장 한 후에는 **IoT Edge: 장치 쌍 업데이트** 명령을 실행 합니다.

## <a name="create-deployment-at-scale"></a>대규모로 배포 만들기

장치 쌍에서 배포 매니페스트 및 구성 된 태그를 구성 하 고 나면 배포할 준비가 된 것입니다.

1. **보기** 메뉴에서 **명령 팔레트** 를 선택 하 고 **Azure IoT Edge: 크기 조정 시 배포 만들기** 명령을 선택 합니다.

1. 사용하려는 배포 매니페스트 JSON 파일로 이동하고 **에지 배포 매니페스트 선택**을 클릭합니다.

1. **배포 ID**부터 시작 하 여 값을 입력 합니다.

   ![배포 ID 지정](./media/how-to-deploy-monitor-vscode/create-deployment-at-scale.png)

   다음 매개 변수에 대 한 값을 지정 합니다.

  | 매개 변수 | 설명 |
  | --- | --- |
  | 배포 ID | IoT hub에서 생성 될 배포의 이름입니다. 배포에 최대 128자의 소문자로 된 고유한 이름을 지정합니다. 공백과 잘못된 문자(`& ^ [ ] { } \ | " < > /`)는 사용하지 않도록 합니다. |
  | 대상 조건 | 대상 조건을 입력 하 여이 배포의 대상으로 지정할 장치를 결정 합니다.조건은 디바이스 쌍 태그 또는 보고되는 디바이스 쌍 속성을 기반으로 하며, 표현 형식이 일치해야 합니다.예를 `tags.environment='test' and properties.reported.devicemodel='4000x'`들면입니다. |
  | 우선 순위 |  양의 정수입니다. 둘 이상의 배포가 동일한 장치를 대상으로 하는 경우 우선 순위 값이 가장 높은 배포가 적용 됩니다. |

  우선 순위를 지정 하면 터미널은 다음 표시와 유사한 출력을 표시 합니다.

   ```cmd
   [Edge] Start deployment with deployment id [{specified-value}] and target condition [{specified-value}]
   [Edge] Deployment with deployment id [{specified-value}] succeeded.
   ```

## <a name="monitoring-and-modifying-deployments"></a>배포 모니터링 및 수정

[Azure Portal](how-to-monitor-iot-edge-deployments.md#monitor-a-deployment-in-the-azure-portal) 또는 [Azure CLI](how-to-monitor-iot-edge-deployments.md#monitor-a-deployment-with-azure-cli) 를 사용 하 여 배포를 모니터링, 수정 및 삭제할 수 있습니다. 둘 다 배포에 대 한 메트릭을 제공 합니다.

## <a name="next-steps"></a>다음 단계

[IoT Edge 장치에 모듈을 배포 하](module-deployment-monitoring.md)는 방법에 대해 자세히 알아보세요.
