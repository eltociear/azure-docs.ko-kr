---
title: Azure IoT Hub 모듈 ID & 모듈 트윈(포털 및 .NET)
description: 포털 및 .NET을 사용하여 모듈 ID를 만들고 모듈 쌍을 업데이트하는 방법을 알아봅니다.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: conceptual
ms.date: 08/20/2019
ms.custom: amqp
ms.openlocfilehash: a3258de2ed7269ab50e6feca3c421d55de5a9d91
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759773"
---
# <a name="get-started-with-iot-hub-module-identity-and-module-twin-using-the-portal-and-net-device"></a>포털 및 .NET 디바이스를 사용하여 IoT Hub 모듈 ID 및 모듈 쌍 시작

[!INCLUDE [iot-hub-selector-module-twin-getstarted](../../includes/iot-hub-selector-module-twin-getstarted.md)]

> [!NOTE]
> [모듈 ID 및 모듈 쌍](iot-hub-devguide-module-twins.md)은 Azure IoT Hub 디바이스 ID 및 디바이스 쌍과 비슷하지만 더 자세한 세분성을 제공합니다. Azure IoT Hub 장치 ID 및 장치 쌍은 백 엔드 응용 프로그램이 장치를 구성하고 장치의 조건에 대한 가시성을 제공할 수 있도록 하는 반면 모듈 ID 및 모듈 쌍은 장치의 개별 구성 요소에 대해 이러한 기능을 제공합니다. 운영 체제 기반 장치 또는 펌웨어 장치와 같은 여러 구성 요소가 있는 지원 장치에서 모듈 ID 및 모듈 트윈을 사용하면 각 구성 요소에 대해 격리된 구성 및 조건을 사용할 수 있습니다.
>

이 자습서에서는 다음에 대해 알아봅니다.

* 포털에서 모듈 ID를 만드는 방법

* .NET 장치 SDK를 사용하여 장치에서 모듈 쌍업데이트를 하는 방법.

> [!NOTE]
> 장치에서 실행되는 두 응용 프로그램과 솔루션 백 엔드를 빌드하는 데 사용할 수 있는 Azure IoT SDK에 대한 자세한 내용은 [Azure IoT SDK를](iot-hub-devguide-sdks.md)참조하십시오.
>

## <a name="prerequisites"></a>사전 요구 사항

* Visual Studio.

* 활성 Azure 계정. 계정이 없는 경우 단 몇 분 만에 [무료 계정을](https://azure.microsoft.com/pricing/free-trial/) 만들 수 있습니다.

## <a name="create-a-hub"></a>허브 만들기

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-hub"></a>허브에 새 장치 등록

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-a-module-identity-in-the-portal"></a>포털에서 모듈 ID 만들기

하나의 디바이스 ID 내에 최대 20개의 모듈 ID를 만들 수 있습니다. ID를 추가하려면 다음 단계를 따르십시오.

1. 이전 섹션에서 만든 장치의 경우 **모듈 ID 추가를** 선택하여 첫 번째 모듈 ID를 만듭니다.

1. *myFirstModule*의 이름을 입력합니다. 모듈 ID를 저장합니다.

    ![모듈 ID 추가](./media/iot-hub-portal-csharp-module-twin-getstarted/add-module-identity.png)

    새 모듈 ID가 화면 하단에 나타납니다. 모듈 ID 세부 정보를 보려면 선택합니다.

    ![모듈 ID 세부 정보 보기](./media/iot-hub-portal-csharp-module-twin-getstarted/module-identity-details.png)

연결 **문자열 저장 - 기본 키**. 다음 섹션에서 장치를 설정하여 모듈을 설정합니다.

## <a name="update-the-module-twin-using-net-device-sdk"></a>.NET 디바이스 SDK를 사용하여 모듈 쌍 업데이트

IoT Hub에서 모듈 ID를 성공적으로 만들었습니다. 시뮬레이트된 디바이스에서 클라우드와 통신해 보겠습니다. 모듈 ID가 만들어지면 모듈 쌍이 IoT Hub에서 암시적으로 생성됩니다. 이 섹션에서는 시뮬레이션된 디바이스에 모듈 쌍 reported 속성을 업데이트하는 .NET 콘솔 앱을 만듭니다.

### <a name="create-a-visual-studio-project"></a>Visual Studio 프로젝트 만들기

모듈 쌍보고 속성을 업데이트하는 앱을 만들려면 다음 단계를 따르십시오.

1. Visual Studio에서 **새 프로젝트 만들기를**선택한 다음 **콘솔 앱(.NET 프레임워크)을**선택하고 다음 을 **선택합니다.**

1. 새 프로젝트 구성에서 *UpdateModuleTwin보고된 속성을* **프로젝트 이름으로** **입력합니다.** **만들기** 를 선택하여 계속합니다.

    ![비주얼 스튜디오 프로젝트로 구성](./media/iot-hub-portal-csharp-module-twin-getstarted/configure-twins-project.png)

### <a name="install-the-latest-azure-iot-hub-net-device-sdk"></a>최신 Azure IoT Hub .NET 디바이스 SDK 설치

모듈 ID 및 모듈 쌍은 공개 미리 보기로 제공됩니다. IoT Hub 시험판 장치 SDK에서만 사용할 수 있습니다. 설치하려면 다음 단계를 따르십시오.

1. Visual Studio에서 오픈 **도구** > **NuGet 패키지 관리자는** > **솔루션에 대한 NuGet 패키지를 관리합니다.**

1. **찾아보기를**선택한 다음 **시험판 포함을**선택합니다. *Microsoft.Azure.Devices.Client*을 검색합니다. 최신 버전을 선택하고 설치합니다.

    ![Azure IoT Hub .NET 서비스 SDK 미리 보기 설치](./media/iot-hub-csharp-csharp-module-twin-getstarted/install-sdk.png)

    이제 모든 모듈 기능에 액세스할 수 있습니다.

### <a name="get-your-module-connection-string"></a>모듈 연결 문자열 가져오기

콘솔 앱의 모듈 연결 문자열이 필요합니다. 다음 단계를 수행하세요.

1. [Azure Portal](https://portal.azure.com/)에 로그인합니다.

1. IoT 허브로 이동하여 **IoT 장치를**선택합니다. **myFirstDevice를** 열면 **myFirstModule이** 성공적으로 만들어졌습니다.

1. **모듈 ID**에서 **myFirstModule을** 선택합니다. **모듈 ID 세부 정보에서**연결 **문자열(기본 키)을**복사합니다.

    ![Azure Portal 모듈 세부 정보](./media/iot-hub-portal-csharp-module-twin-getstarted/module-identity-details.png)

### <a name="create-updatemoduletwinreportedproperties-console-app"></a>UpdateModuleTwinReportedProperties 콘솔 앱 만들기

앱을 만들려면 다음 단계를 따르세요.

1. Program.cs 파일 `using` 의 맨 위에 **Program.cs** 다음 문을 추가합니다.

  ```csharp
  using Microsoft.Azure.Devices.Client;
  using Microsoft.Azure.Devices.Shared;
  using Newtonsoft.Json;
  ```

2. **Program** 클래스에 다음 필드를 추가합니다. 자리 표시자 값을 모듈 연결 문자열로 바꿉니다.

  ```csharp
  private const string ModuleConnectionString = "<Your module connection string>";
  private static ModuleClient Client = null;
  ```

3. **Program** 클래스에 **OnDesiredPropertyChanged** 메서드를 추가합니다.

  ```csharp
  private static async Task OnDesiredPropertyChanged(TwinCollection desiredProperties, object userContext)
      {
          Console.WriteLine("desired property change:");
          Console.WriteLine(JsonConvert.SerializeObject(desiredProperties));
          Console.WriteLine("Sending current time as reported property");
          TwinCollection reportedProperties = new TwinCollection
          {
              ["DateTimeLastDesiredPropertyChangeReceived"] = DateTime.Now
          };
  
          await Client.UpdateReportedPropertiesAsync(reportedProperties).ConfigureAwait(false);
      }
  ```

4. 마지막으로 **Main** 메서드를 다음 코드로 바꿉니다.

  ```csharp
  static void Main(string[] args)
  {
      Microsoft.Azure.Devices.Client.TransportType transport = Microsoft.Azure.Devices.Client.TransportType.Amqp;
  
      try
      {
          Client = ModuleClient.CreateFromConnectionString(ModuleConnectionString, transport);
          Client.SetConnectionStatusChangesHandler(ConnectionStatusChangeHandler);
          Client.SetDesiredPropertyUpdateCallbackAsync(OnDesiredPropertyChanged, null).Wait();
  
          Console.WriteLine("Retrieving twin");
          var twinTask = Client.GetTwinAsync();
          twinTask.Wait();
          var twin = twinTask.Result;
          Console.WriteLine(JsonConvert.SerializeObject(twin));
  
          Console.WriteLine("Sending app start time as reported property");
          TwinCollection reportedProperties = new TwinCollection();
          reportedProperties["DateTimeLastAppLaunch"] = DateTime.Now;
  
          Client.UpdateReportedPropertiesAsync(reportedProperties);
      }
      catch (AggregateException ex)
      {
          Console.WriteLine("Error in sample: {0}", ex);
      }
  
      Console.WriteLine("Waiting for Events.  Press enter to exit...");
      Console.ReadKey();
      Client.CloseAsync().Wait();
  }
  
  private static void ConnectionStatusChangeHandler(ConnectionStatus status, ConnectionStatusChangeReason reason)
  {
      Console.WriteLine($"Status {status} changed: {reason}");
  }
  ```
  
  **F5를**사용하여이 응용 프로그램을 빌드하고 실행할 수 있습니다.

이 코드 샘플에서는 AMQP 프로토콜을 사용하여 모듈 쌍을 검색하고 reported 속성을 업데이트하는 방법을 보여 줍니다. 공개 미리 보기에서는 모듈 쌍 작업에 대해서만 AMQP를 지원합니다.

## <a name="next-steps"></a>다음 단계

계속해서 IoT Hub을 시작하고 다른 IoT 시나리오를 탐색하려면 다음을 참조하세요.

* [.NET 백업 및 .NET 디바이스를 사용하여 IoT Hub 모듈 ID 및 모듈 쌍 시작](iot-hub-csharp-csharp-module-twin-getstarted.md)

* [IoT Edge 시작](../iot-edge/tutorial-simulate-device-linux.md)
