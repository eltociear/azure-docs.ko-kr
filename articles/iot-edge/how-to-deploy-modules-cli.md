---
title: Azure CLI 명령줄에서 모듈 배포-Azure IoT Edge
description: Azure IoT 확장과 함께 Azure CLI를 사용 하 여 배포 매니페스트에 구성 된 대로 IoT Hub의 IoT Edge 모듈을 IoT Edge 장치로 푸시합니다.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/16/2019
ms.topic: conceptual
ms.reviewer: menchi
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: fbd0d65624852737c424128e9125b8370b870d4d
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133938"
---
# <a name="deploy-azure-iot-edge-modules-with-azure-cli"></a>Azure CLI를 사용하여 Azure IoT Edge 모듈 배포

비즈니스 논리를 사용하여 IoT Edge 모듈을 만들면 디바이스에 배포하여 에지에서 작동시킵니다. 데이터를 수집하고 처리하기 위해 다중 모듈을 사용한 경우 한 번에 모두 배포하고 여기에 연결된 회람 규칙을 선언할 수 있습니다.

[Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)는 IoT Edge 같은 Azure 리소스를 관리하기 위한 오픈 소스 교차 플랫폼 명령줄 도구입니다. 이 기능을 사용하면 Azure IoT Hub 리소스, 디바이스 프로비전 서비스 인스턴스 및 연결된 허브를 즉시 관리할 수 있습니다. 새로운 IoT 확장은 디바이스 관리 및 전체 IoT Edge 같은 기능으로 Azure CLI를 강화합니다.

이 아티클에서는 JSON 배포 매니페스트를 만든 다음, 해당 파일을 사용하여 IoT Edge 디바이스에 배포를 푸시하는 방법을 보여줍니다. 해당 공유 태그에 따라 다중 디바이스를 대상으로 지정하는 배포를 만드는 방법에 대한 정보는 [대규모 IoT Edge 모듈 배포 및 모니터링](how-to-deploy-cli-at-scale.md)을 참조하세요.

## <a name="prerequisites"></a>필수 구성 요소

* Azure 구독의 [IoT hub](../iot-hub/iot-hub-create-using-cli.md)
* IoT Edge 런타임이 설치된 [IoT Edge 디바이스](how-to-register-device.md#register-with-the-azure-cli)
* 사용자 환경의 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli). 최소한 Azure CLI 버전은 2.0.70 이상 이어야 합니다. `az --version` 명령을 사용하여 유효성을 검사합니다. 이 버전은 az extension 명령을 지원하며 Knack 명령 프레임워크를 도입했습니다.
* [Azure CLI에 대 한 IoT 확장](https://github.com/Azure/azure-iot-cli-extension)입니다.

## <a name="configure-a-deployment-manifest"></a>배포 매니페스트 구성

배포 매니페스트는 배포할 모듈, 모듈 간의 데이터 흐름 및 모듈 쌍의 desired 속성을 설명하는 JSON 문서입니다. 배포 매니페스트의 작동 방식 및 생성 방법에 대한 자세한 내용은 [IoT Edge 모듈을 사용, 구성 및 다시 사용하는 방법에 대한 이해](module-composition.md)를 참조하세요.

Azure CLI를 사용하여 모듈을 배포하려면 배포 매니페스트를 로컬에 .json 파일로 저장합니다. 명령을 실행하여 디바이스에 구성을 적용할 때 다음 섹션에서 파일 경로를 사용합니다.

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

## <a name="deploy-to-your-device"></a>새 디바이스에 배포

모듈 정보를 사용하여 구성한 배포 매니페스트를 적용하여 디바이스에 모듈을 배포합니다.

배포 매니페스트가 저장된 폴더로 디렉터리를 변경합니다. VS Code IoT Edge 템플릿 중 하나를 사용한 경우 `deployment.template.json` 파일 대신 솔루션 디렉터리의 **config** 폴더에 있는 `deployment.json` 파일을 사용합니다.

다음 명령을 사용하여 IoT Edge 디바이스에 구성을 적용합니다.

   ```azurecli
   az iot edge set-modules --device-id [device id] --hub-name [hub name] --content [file path]
   ```

장치 ID 매개 변수는 대/소문자를 구분 합니다. 콘텐츠 매개 변수는 저장한 배포 매니페스트 파일을 가리킵니다.

   ![az iot edge set-modules output](./media/how-to-deploy-cli/set-modules.png)

## <a name="view-modules-on-your-device"></a>디바이스에서 모듈 보기

디바이스에 모듈을 배포하면 다음 명령을 사용하여 모두 볼 수 있습니다.

IoT Edge 디바이스의 모듈 보기:

   ```azurecli
   az iot hub module-identity list --device-id [device id] --hub-name [hub name]
   ```

장치 ID 매개 변수는 대/소문자를 구분 합니다.

   ![az iot hub module-identity list output](./media/how-to-deploy-cli/list-modules.png)

## <a name="next-steps"></a>다음 단계

[대규모 IoT Edge 모듈을 배포 및 모니터링](how-to-deploy-at-scale.md)하는 방법 알아보기
