---
title: 함수 2.x에 대한 Azure 코스모스 DB 입력 바인딩
description: Azure 함수에서 Azure Cosmos DB 입력 바인딩을 사용하는 방법을 알아봅니다.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2020
ms.author: cshoe
ms.openlocfilehash: eabcf40e28927919215979ccc46fa029d19adbfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943416"
---
# <a name="azure-cosmos-db-input-binding-for-azure-functions-2x"></a>Azure 코스모스 DB Azure 함수에 대한 입력 바인딩 2.x

Azure Cosmos DB 입력 바인딩은 SQL API를 사용하여 하나 이상의 Azure Cosmos DB 문서를 검색하고, 함수의 입력 매개 변수에 전달합니다. 문서 ID 또는 쿼리 매개 변수는 함수를 호출하는 트리거를 기반으로 결정할 수 있습니다.

설정 및 구성 세부 정보에 대한 자세한 내용은 [개요를](./functions-bindings-cosmosdb-v2.md)참조하십시오.

> [!NOTE]
> 컬렉션이 [분할된](../cosmos-db/partition-data.md#logical-partitions)경우 조회 작업도 파티션 키 값을 지정해야 합니다.
>

<a id="example" name="example"></a>

# <a name="c"></a>[C #](#tab/csharp)

이 섹션에는 다음 예제가 포함되어 있습니다.

* [큐 트리거, JSON에서 ID 조회](#queue-trigger-look-up-id-from-json-c)
* [HTTP 트리거, 쿼리 문자열에서 ID 조회](#http-trigger-look-up-id-from-query-string-c)
* [HTTP 트리거, 경로 데이터에서 ID 조회](#http-trigger-look-up-id-from-route-data-c)
* [HTTP 트리거, 경로 데이터에서 ID 조회, SqlQuery 사용](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP 트리거, 여러 문서 가져오기, SqlQuery 사용](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP 트리거, 여러 문서 가져오기, DocumentClient 사용](#http-trigger-get-multiple-docs-using-documentclient-c)

예제에서는 간단한 `ToDoItem` 형식을 참조하세요.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string PartitionKey { get; set; }
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-look-up-id-from-json-c"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>큐 트리거, JSON에서 ID 조회 

다음 예제에서는 단일 문서를 검색하는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 함수는 JSON 개체가 있는 큐 메시지에 의해 트리거됩니다. 큐 트리거는 JSON을 조회할 ID `ToDoItemLookup`및 파티션 키 값을 포함하는 형식의 개체로 구문 분석합니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }

        public string ToDoItemPartitionKeyValue { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{ToDoItemId}",
                PartitionKey = "{ToDoItemPartitionKeyValue}")]ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId} Key={toDoItemLookup?.ToDoItemPartitionKeyValue}");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

<a id="http-trigger-look-up-id-from-query-string-c"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP 트리거, 쿼리 문자열에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 이 함수는 조회할 ID 및 파티션 키 값을 지정하기 위해 쿼리 문자열을 사용하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

>[!NOTE]
>HTTP 쿼리 문자열 매개 변수는 대/소문자를 구분합니다.
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{Query.id}",
                PartitionKey = "{Query.partitionKey}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-look-up-id-from-route-data-c"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP 트리거, 경로 데이터에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 이 함수는 경로 데이터를 사용하여 조회할 ID 및 파티션 키 값을 지정하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{partitionKey}/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                Id = "{id}",
                PartitionKey = "{partitionKey}")] ToDoItem toDoItem,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.LogInformation($"ToDo item not found");
            }
            else
            {
                log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP 트리거, 경로 데이터에서 ID 조회, SqlQuery 사용

다음 예제에서는 단일 문서를 검색하는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 함수는 조회할 ID를 지정하기 위해 경로 데이터를 사용하는 HTTP 요청에 의해 트리거됩니다. ID는 지정된 데이터베이스 및 컬렉션에서 `ToDoItem` 문서를 검색하는 데 사용됩니다.

이 예제에서는 `SqlQuery` 매개 변수에서 바인딩 식을 사용하는 방법을 보여줍니다. 표시된 대로 경로 데이터를 `SqlQuery` 매개 변수에 전달할 수는 있지만 현재 [쿼리 문자열 값을 전달할 수 없습니다](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).

> [!NOTE]
> ID로만 쿼리해야 하는 경우 [요청 단위를](../cosmos-db/request-units.md)덜 소비하므로 [이전 예제와](#http-trigger-look-up-id-from-query-string-c)같이 조회를 사용하는 것이 좋습니다. 점 읽기 연산(GET)은 ID별 쿼리보다 [효율적입니다.](../cosmos-db/optimize-cost-queries.md)
>

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

<a id="http-trigger-get-multiple-docs-using-sqlquery-c"></a>

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP 트리거, 여러 문서 가져오기, SqlQuery 사용

다음 예제에서는 문서 목록을 검색하는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 함수는 HTTP 요청에 의해 트리거됩니다. 쿼리는 `SqlQuery` 특성 속성에 지정됩니다.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using Microsoft.Extensions.Logging;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.LogInformation(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

<a id="http-trigger-get-multiple-docs-using-documentclient-c"></a>

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>HTTP 트리거, 여러 문서 가져오기, DocumentClient 사용

다음 예제에서는 문서 목록을 검색하는 [C# 함수](functions-dotnet-class-library.md)를 보여줍니다. 함수는 HTTP 요청에 의해 트리거됩니다. 코드를 Azure Cosmos DB 바인딩에 의해 제공된 `DocumentClient` 인스턴스를 사용하여 문서 목록을 읽습니다. `DocumentClient` 인스턴스는 쓰기 작업에 사용될 수도 있습니다.

> [!NOTE]
> [또한 IDocumentClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.idocumentclient?view=azure-dotnet) 인터페이스를 사용하여 테스트를 더 쉽게 수행할 수도 있습니다.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.LogInformation($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.LogInformation(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

# <a name="c-script"></a>[C# 스크립트](#tab/csharp-script)

이 섹션에는 다음 예제가 포함되어 있습니다.

* [큐 트리거, 문자열에서 ID 조회](#queue-trigger-look-up-id-from-string-c-script)
* [큐 트리거, 여러 문서 가져오기, SqlQuery 사용](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP 트리거, 쿼리 문자열에서 ID 조회](#http-trigger-look-up-id-from-query-string-c-script)
* [HTTP 트리거, 경로 데이터에서 ID 조회](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP 트리거, 여러 문서 가져오기, SqlQuery 사용](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP 트리거, 여러 문서 가져오기, DocumentClient 사용](#http-trigger-get-multiple-docs-using-documentclient-c-script)

HTTP 트리거 예제에서는 간단한 `ToDoItem` 형식을 참조합니다.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

<a id="queue-trigger-look-up-id-from-string-c-script"></a>

### <a name="queue-trigger-look-up-id-from-string"></a>큐 트리거, 문자열에서 ID 조회

다음 예에서는 *function.json* 파일의 Cosmos DB 입력 바인딩 및 바인딩을 사용하는 [C# 스크립트 함수](functions-reference-csharp.md)를 보여줍니다. 이 함수는 단일 문서를 읽고 문서의 텍스트 값을 업데이트합니다.

*function.json* 파일의 바인딩 데이터는 다음과 같습니다.

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
}
```
[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

C# 스크립트 코드는 다음과 같습니다.

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {
      inputDocument.text = "This has changed.";
    }
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>큐 트리거, 여러 문서 가져오기, SqlQuery 사용

다음 예제에서는 *function.json* 파일의 Azure Cosmos DB 입력 바인딩 및 해당 바인딩을 사용하는 [C# 스크립트 함수](functions-reference-csharp.md)를 보여 줍니다. 이 함수는 큐 트리거를 사용하여 쿼리 매개 변수를 사용자 지정하여 SQL 쿼리로 지정된 여러 문서를 검색합니다.

큐 트리거는 매개 변수 `departmentId`를 제공합니다. `{ "departmentId" : "Finance" }`의 큐 메시지는 재무 부서에 대한 모든 레코드를 반환합니다.

*function.json* 파일의 바인딩 데이터는 다음과 같습니다.

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

C# 스크립트 코드는 다음과 같습니다.

```csharp
    public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
    {
        foreach (var doc in documents)
        {
            // operate on each document
        }
    }

    public class QueuePayload
    {
        public string departmentId { get; set; }
    }
```

<a id="http-trigger-look-up-id-from-query-string-c-script"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP 트리거, 쿼리 문자열에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [C# 스크립트 함수](functions-reference-csharp.md)를 보여줍니다. 이 함수는 조회할 ID 및 파티션 키 값을 지정하기 위해 쿼리 문자열을 사용하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey" : "{Query.partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

C# 스크립트 코드는 다음과 같습니다.

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-look-up-id-from-route-data-c-script"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP 트리거, 경로 데이터에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [C# 스크립트 함수](functions-reference-csharp.md)를 보여줍니다. 이 함수는 경로 데이터를 사용하여 조회할 ID 및 파티션 키 값을 지정하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

C# 스크립트 코드는 다음과 같습니다.

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    if (toDoItem == null)
    {
         log.LogInformation($"ToDo item not found");
    }
    else
    {
        log.LogInformation($"Found ToDo item, Description={toDoItem.Description}");
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>

### <a name="http-trigger-get-multiple-docs-using-sqlquery"></a>HTTP 트리거, 여러 문서 가져오기, SqlQuery 사용

다음 예제에서는 문서 목록을 검색하는 [C# 스크립트 함수](functions-reference-csharp.md)를 보여줍니다. 함수는 HTTP 요청에 의해 트리거됩니다. 쿼리는 `SqlQuery` 특성 속성에 지정됩니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

C# 스크립트 코드는 다음과 같습니다.

```cs
using System.Net;
using Microsoft.Extensions.Logging;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.LogInformation(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a id="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>

### <a name="http-trigger-get-multiple-docs-using-documentclient"></a>HTTP 트리거, 여러 문서 가져오기, DocumentClient 사용

다음 예제에서는 문서 목록을 검색하는 [C# 스크립트 함수](functions-reference-csharp.md)를 보여줍니다. 함수는 HTTP 요청에 의해 트리거됩니다. 코드를 Azure Cosmos DB 바인딩에 의해 제공된 `DocumentClient` 인스턴스를 사용하여 문서 목록을 읽습니다. `DocumentClient` 인스턴스는 쓰기 작업에 사용될 수도 있습니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

C# 스크립트 코드는 다음과 같습니다.

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.LogInformation($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
    IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
        .Where(p => p.Description.Contains(searchterm))
        .AsDocumentQuery();

    while (query.HasMoreResults)
    {
        foreach (ToDoItem result in await query.ExecuteNextAsync())
        {
            log.LogInformation(result.Description);
        }
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

# <a name="javascript"></a>[자바 스크립트](#tab/javascript)

이 섹션에는 다양한 원본의 ID 값을 지정하여 단일 문서를 읽는 다음과 같은 예제가 포함됩니다.

* [큐 트리거, JSON에서 ID 조회](#queue-trigger-look-up-id-from-json-javascript)
* [HTTP 트리거, 쿼리 문자열에서 ID 조회](#http-trigger-look-up-id-from-query-string-javascript)
* [HTTP 트리거, 경로 데이터에서 ID 조회](#http-trigger-look-up-id-from-route-data-javascript)
* [큐 트리거, 여러 문서 가져오기, SqlQuery 사용](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

<a id="queue-trigger-look-up-id-from-json-javascript"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>큐 트리거, JSON에서 ID 조회

다음 예에서는 *function.json* 파일의 Cosmos DB 입력 바인딩 및 바인딩을 사용하는 [JavaScript 함수](functions-reference-node.md)를 보여줍니다. 이 함수는 단일 문서를 읽고 문서의 텍스트 값을 업데이트합니다.

*function.json* 파일의 바인딩 데이터는 다음과 같습니다.

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

JavaScript 코드는 다음과 같습니다.

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

<a id="http-trigger-look-up-id-from-query-string-javascript"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP 트리거, 쿼리 문자열에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [JavaScript 함수](functions-reference-node.md)를 보여줍니다. 이 함수는 조회할 ID 및 파티션 키 값을 지정하기 위해 쿼리 문자열을 사용하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey": "{Query.partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

JavaScript 코드는 다음과 같습니다.

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

<a id="http-trigger-look-up-id-from-route-data-javascript"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP 트리거, 경로 데이터에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [JavaScript 함수](functions-reference-node.md)를 보여줍니다. 이 함수는 경로 데이터를 사용하여 조회할 ID 및 파티션 키 값을 지정하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false
}
```

JavaScript 코드는 다음과 같습니다.

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>큐 트리거, 여러 문서 가져오기, SqlQuery 사용

다음 예제에서는 *function.json* 파일의 Azure Cosmos DB 입력 바인딩 및 해당 바인딩을 사용하는 [JavaScript 함수](functions-reference-node.md)를 보여 줍니다. 이 함수는 큐 트리거를 사용하여 쿼리 매개 변수를 사용자 지정하여 SQL 쿼리로 지정된 여러 문서를 검색합니다.

큐 트리거는 매개 변수 `departmentId`를 제공합니다. `{ "departmentId" : "Finance" }`의 큐 메시지는 재무 부서에 대한 모든 레코드를 반환합니다.

*function.json* 파일의 바인딩 데이터는 다음과 같습니다.

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

JavaScript 코드는 다음과 같습니다.

```javascript
    module.exports = function (context, input) {
        var documents = context.bindings.documents;
        for (var i = 0; i < documents.length; i++) {
            var document = documents[i];
            // operate on each document
        }
        context.done();
    };
```

# <a name="python"></a>[Python](#tab/python)

이 섹션에는 다양한 원본의 ID 값을 지정하여 단일 문서를 읽는 다음과 같은 예제가 포함됩니다.

* [큐 트리거, JSON에서 ID 조회](#queue-trigger-look-up-id-from-json-python)
* [HTTP 트리거, 쿼리 문자열에서 ID 조회](#http-trigger-look-up-id-from-query-string-python)
* [HTTP 트리거, 경로 데이터에서 ID 조회](#http-trigger-look-up-id-from-route-data-python)
* [큐 트리거, 여러 문서 가져오기, SqlQuery 사용](#queue-trigger-get-multiple-docs-using-sqlquery-python)

<a id="queue-trigger-look-up-id-from-json-python"></a>

### <a name="queue-trigger-look-up-id-from-json"></a>큐 트리거, JSON에서 ID 조회

다음 예제에서는 *function.json* 파일의 Cosmos DB 입력 바인딩 및 바인딩을 사용하는 [Python 함수](functions-reference-python.md)를 보여 줍니다. 이 함수는 단일 문서를 읽고 문서의 텍스트 값을 업데이트합니다.

*function.json* 파일의 바인딩 데이터는 다음과 같습니다.

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "in"
},
{
    "name": "$return",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
    "direction": "out"
}
```

[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

다음은 Python 코드입니다.

```python
import azure.functions as func


def main(queuemsg: func.QueueMessage, documents: func.DocumentList) -> func.Document:
    if documents:
        document = documents[0]
        document['text'] = 'This was updated!'
        return document
```

<a id="http-trigger-look-up-id-from-query-string-python"></a>

### <a name="http-trigger-look-up-id-from-query-string"></a>HTTP 트리거, 쿼리 문자열에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [Python 함수](functions-reference-python.md)를 보여 줍니다. 이 함수는 조회할 ID 및 파티션 키 값을 지정하기 위해 쿼리 문자열을 사용하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connectionStringSetting": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}",
      "PartitionKey": "{Query.partitionKeyValue}"
    }
  ],
  "disabled": true,
  "scriptFile": "__init__.py"
}
```

다음은 Python 코드입니다.

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])

    return 'OK'
```

<a id="http-trigger-look-up-id-from-route-data-python"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP 트리거, 경로 데이터에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 [Python 함수](functions-reference-python.md)를 보여 줍니다. 이 함수는 경로 데이터를 사용하여 조회할 ID 및 파티션 키 값을 지정하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 `ToDoItem` 값은 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

*function.json* 파일은 다음과 같습니다.

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{partitionKeyValue}/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "cosmosDB",
      "name": "todoitems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}",
      "PartitionKey": "{partitionKeyValue}"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

다음은 Python 코드입니다.

```python
import logging
import azure.functions as func


def main(req: func.HttpRequest, todoitems: func.DocumentList) -> str:
    if not todoitems:
        logging.warning("ToDo item not found")
    else:
        logging.info("Found ToDo item, Description=%s",
                     todoitems[0]['description'])
    return 'OK'
```

<a id="queue-trigger-get-multiple-docs-using-sqlquery-python"></a>

### <a name="queue-trigger-get-multiple-docs-using-sqlquery"></a>큐 트리거, 여러 문서 가져오기, SqlQuery 사용

다음 예제에서는 *function.json* 파일의 Azure Cosmos DB 입력 바인딩 및 해당 바인딩을 사용하는 [Python 함수](functions-reference-python.md)를 보여 줍니다. 이 함수는 큐 트리거를 사용하여 쿼리 매개 변수를 사용자 지정하여 SQL 쿼리로 지정된 여러 문서를 검색합니다.

큐 트리거는 매개 변수 `departmentId`를 제공합니다. `{ "departmentId" : "Finance" }`의 큐 메시지는 재무 부서에 대한 모든 레코드를 반환합니다.

*function.json* 파일의 바인딩 데이터는 다음과 같습니다.

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
}
```

[구성](#configuration) 섹션에서는 이러한 속성을 설명합니다.

다음은 Python 코드입니다.

```python
import azure.functions as func

def main(queuemsg: func.QueueMessage, documents: func.DocumentList):
    for document in documents:
        # operate on each document
```

# <a name="java"></a>[Java](#tab/java)

이 섹션에는 다음 예제가 포함되어 있습니다.

* [HTTP 트리거, 쿼리 문자열에서 ID 조회 - 문자열 매개 변수](#http-trigger-look-up-id-from-query-string---string-parameter-java)
* [HTTP 트리거, 쿼리 문자열에서 ID 조회 - POJO 매개 변수](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)
* [HTTP 트리거, 경로 데이터에서 ID 조회](#http-trigger-look-up-id-from-route-data-java)
* [HTTP 트리거, 경로 데이터에서 ID 조회, SqlQuery 사용](#http-trigger-look-up-id-from-route-data-using-sqlquery-java)
* [HTTP 트리거, SqlQuery를 사용하여 경로 데이터에서 여러 문서 가져오기](#http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java)

예제에서는 간단한 `ToDoItem` 형식을 참조하세요.

```java
public class ToDoItem {

  private String id;
  private String description;

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }

  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}
```

<a id="http-trigger-look-up-id-from-query-string---string-parameter-java"></a>

### <a name="http-trigger-look-up-id-from-query-string---string-parameter"></a>HTTP 트리거, 쿼리 문자열에서 ID 조회 - 문자열 매개 변수

다음 예제에서는 단일 문서를 검색하는 Java 함수를 보여줍니다. 이 함수는 조회할 ID 및 파티션 키 값을 지정하기 위해 쿼리 문자열을 사용하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 값은 String 형식으로 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용됩니다.

```java
public class DocByIdFromQueryString {

    @FunctionName("DocByIdFromQueryString")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

[Java 함수 런타임 라이브러리](/java/api/overview/azure/functions/runtime)에서 값이 Cosmos DB에서 제공되는 함수 매개 변수에 대한 `@CosmosDBInput` 주석을 사용합니다.  `Optional<T>`을 사용하여 원시 Java 형식, POJO 또는 null 허용 값으로 이 주석을 사용할 수 있습니다.

<a id="http-trigger-look-up-id-from-query-string---pojo-parameter-java"></a>

### <a name="http-trigger-look-up-id-from-query-string---pojo-parameter"></a>HTTP 트리거, 쿼리 문자열에서 ID 조회 - POJO 매개 변수

다음 예제에서는 단일 문서를 검색하는 Java 함수를 보여줍니다. 이 함수는 조회할 ID 및 파티션 키 값을 지정하기 위해 쿼리 문자열을 사용하는 HTTP 요청에 의해 트리거됩니다. 지정된 데이터베이스 및 컬렉션에서 문서를 검색하는 데 사용되는 해당 ID 및 파티션 키 값입니다. 그런 후 문서는 이전에 만든 ```ToDoItem``` POJO의 인스턴스로 변환되고 함수에 인수로 전달됩니다.

```java
public class DocByIdFromQueryStringPojo {

    @FunctionName("DocByIdFromQueryStringPojo")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{Query.id}",
              partitionKey = "{Query.partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Item from the database is " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

<a id="http-trigger-look-up-id-from-route-data-java"></a>

### <a name="http-trigger-look-up-id-from-route-data"></a>HTTP 트리거, 경로 데이터에서 ID 조회

다음 예제에서는 단일 문서를 검색하는 Java 함수를 보여줍니다. 이 함수는 경로 매개 변수를 사용하여 조회할 ID 및 파티션 키 값을 지정하는 HTTP 요청에 의해 트리거됩니다. 해당 ID 및 파티션 키 값은 지정된 데이터베이스 및 컬렉션에서 ```Optional<String>```문서를 검색하여 로 반환하는 데 사용됩니다.

```java
public class DocByIdFromRoute {

    @FunctionName("DocByIdFromRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems/{partitionKeyValue}/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              id = "{id}",
              partitionKey = "{partitionKeyValue}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            Optional<String> item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("String from the database is " + (item.isPresent() ? item.get() : null));

        // Convert and display
        if (!item.isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            // return JSON from Cosmos. Alternatively, we can parse the JSON string
            // and return an enriched JSON object.
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item.get())
                          .build();
        }
    }
}
 ```

 <a id="http-trigger-look-up-id-from-route-data-using-sqlquery-java"></a>

### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery"></a>HTTP 트리거, 경로 데이터에서 ID 조회, SqlQuery 사용

다음 예제에서는 단일 문서를 검색하는 Java 함수를 보여줍니다. 이 함수는 경로 매개 변수를 사용하여 조회할 ID를 지정하는 HTTP 요청에 의해 트리거됩니다. ID는 지정한 데이터베이스 및 컬렉션에서 문서를 검색한 후, 쿼리 조건에 따라 많은 문서가 반환될 수 있으므로 결과 세트를 ```ToDoItem[]```으로 변환하는 데 사용됩니다.

> [!NOTE]
> ID로만 쿼리해야 하는 경우 [요청 단위를](../cosmos-db/request-units.md)덜 소비하므로 [이전 예제와](#http-trigger-look-up-id-from-query-string---pojo-parameter-java)같이 조회를 사용하는 것이 좋습니다. 점 읽기 연산(GET)은 ID별 쿼리보다 [효율적입니다.](../cosmos-db/optimize-cost-queries.md)
>

```java
public class DocByIdFromRouteSqlQuery {

    @FunctionName("DocByIdFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET, HttpMethod.POST},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems2/{id}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where r.id = {id}",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] item,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Items from the database are " + item);

        // Convert and display
        if (item == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(item)
                          .build();
        }
    }
}
 ```

 <a id="http-trigger-get-multiple-docs-from-route-data-using-sqlquery-java"></a>

### <a name="http-trigger-get-multiple-docs-from-route-data-using-sqlquery"></a>HTTP 트리거, SqlQuery를 사용하여 경로 데이터에서 여러 문서 가져오기

다음 예제에서는 여러 문서를 검색하는 Java 함수를 보여 주며 있습니다. 이 함수는 라우트 매개 변수를 ```desc``` 사용하여 ```description``` 필드에서 검색할 문자열을 지정하는 HTTP 요청에 의해 트리거됩니다. 검색 용어는 지정한 데이터베이스 및 컬렉션에서 문서 컬렉션을 검색한 후 결과 세트를 ```ToDoItem[]```으로 변환하고 함수에 인수로 전달하는 데 사용됩니다.

```java
public class DocsFromRouteSqlQuery {

    @FunctionName("DocsFromRouteSqlQuery")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req",
              methods = {HttpMethod.GET},
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "todoitems3/{desc}")
            HttpRequestMessage<Optional<String>> request,
            @CosmosDBInput(name = "database",
              databaseName = "ToDoList",
              collectionName = "Items",
              sqlQuery = "select * from Items r where contains(r.description, {desc})",
              connectionStringSetting = "Cosmos_DB_Connection_String")
            ToDoItem[] items,
            final ExecutionContext context) {

        // Item list
        context.getLogger().info("Parameters are: " + request.getQueryParameters());
        context.getLogger().info("Number of items from the database is " + (items == null ? 0 : items.length));

        // Convert and display
        if (items == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("No documents found.")
                          .build();
        }
        else {
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(items)
                          .build();
        }
    }
}
 ```

 ---

## <a name="attributes-and-annotations"></a>특성 및 주석

# <a name="c"></a>[C #](#tab/csharp)

[C# 클래스 라이브러리](functions-dotnet-class-library.md)에서 [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) 특성을 사용합니다.

특성의 생성자는 데이터베이스 이름과 컬렉션 이름을 사용합니다. 이러한 설정 및 구성할 수 있는 다른 속성에 대한 자세한 내용은 [다음 구성 섹션](#configuration)을 참조하세요.

# <a name="c-script"></a>[C# 스크립트](#tab/csharp-script)

특성은 C# 스크립트에서 지원되지 않습니다.

# <a name="javascript"></a>[자바 스크립트](#tab/javascript)

속성은 자바 스크립트에서 지원되지 않습니다.

# <a name="python"></a>[Python](#tab/python)

특성은 파이썬에서 지원되지 않습니다.

# <a name="java"></a>[Java](#tab/java)

Java [함수 런타임 라이브러리에서](https://docs.microsoft.com/java/api/overview/azure/functions/runtime)Cosmos DB에 `@CosmosDBOutput` 쓰는 매개 변수에 대한 추가를 사용합니다. 추가 매개 변수 `OutputBinding<T>` `T` 형식은 네이티브 Java 형식 또는 POJO입니다.

---

## <a name="configuration"></a>Configuration

다음 표에서는 *function.json* 파일및 특성에서 설정한 바인딩 `CosmosDB` 구성 속성에 대해 설명합니다.

|function.json 속성 | 특성 속성 |설명|
|---------|---------|----------------------|
|**종류**     | 해당 없음 | `cosmosDB`로 설정해야 합니다.        |
|**direction**     | 해당 없음 | `in`로 설정해야 합니다.         |
|**(이름)**     | 해당 없음 | 함수에서 문서를 나타내는 바인딩 매개 변수의 이름입니다.  |
|**databaseName** |**Databasename** |문서를 포함하는 데이터베이스입니다.        |
|**컬렉션이름** |**CollectionName** | 문서를 포함하는 컬렉션의 이름입니다. |
|**id**    | **Id** | 검색할 문서의 ID입니다. 이 속성은 [바인딩 식](./functions-bindings-expressions-patterns.md)을 지원합니다. `id` **및 sqlQuery** 속성을 모두 설정 하지 마십시오. 둘 중 하나를 설정하지 않으면 전체 컬렉션이 검색됩니다. |
|**sql쿼리**  |**SqlQuery**  | 여러 문서를 검색하는 데 사용되는 Azure Cosmos DB SQL 쿼리입니다. 이 속성은 런타임 바인딩을 지원합니다(예: `SELECT * FROM c where c.departmentId = {departmentId}`). `id` 속성과 `sqlQuery` 속성을 모두 설정하지 마십시오. 둘 중 하나를 설정하지 않으면 전체 컬렉션이 검색됩니다.|
|**연결문자열 설정**     |**ConnectionStringSetting**|Azure Cosmos DB 연결 문자열을 포함하는 앱 설정의 이름입니다. |
|**파티션키**|**PartitionKey**|조회를 위한 파티션 키 값을 지정합니다. 바인딩 매개 변수가 포함될 수 있습니다. [분할된](../cosmos-db/partition-data.md#logical-partitions) 컬렉션의 조회에 필요합니다.|
|**기본 설정위치**| **기본 설정 위치**| (선택 사항) Azure Cosmos DB 서비스에서 지리적으로 복제된 데이터베이스 계정에 대한 기본 위치(영역)를 정의합니다. 값은 쉼표로 구분되어야 합니다. 예를 들어 "미국 동부, 미국 중남부, 북유럽". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>사용

# <a name="c"></a>[C #](#tab/csharp)

함수가 성공적으로 종료되면 명명된 입력 매개 변수를 통해 입력 문서에 대한 변경 내용이 자동으로 유지됩니다.

# <a name="c-script"></a>[C# 스크립트](#tab/csharp-script)

함수가 성공적으로 종료되면 명명된 입력 매개 변수를 통해 입력 문서에 대한 변경 내용이 자동으로 유지됩니다.

# <a name="javascript"></a>[자바 스크립트](#tab/javascript)

함수 종료 시 업데이트가 자동으로 이루어지지 않습니다. 대신 `context.bindings.<documentName>In` 및 `context.bindings.<documentName>Out`을 사용하여 업데이트합니다. JavaScript 예제를 참조하세요.

# <a name="python"></a>[Python](#tab/python)

데이터는 `DocumentList` 매개 변수를 통해 함수에 사용할 수 있습니다. 문서에 대한 변경 내용은 자동으로 유지되지 않습니다.

# <a name="java"></a>[Java](#tab/java)

Java [함수 런타임 라이브러리에서](https://docs.microsoft.com/java/api/overview/azure/functions/runtime) [@CosmosDBInput](https://docs.microsoft.com/java/api/com.microsoft.azure.functions.annotation.cosmosdbinput) 이 어노션은 Cosmos DB 데이터를 함수에 노출시다. `Optional<T>`을 사용하여 원시 Java 형식, POJO 또는 null 허용 값으로 이 주석을 사용할 수 있습니다.

---

## <a name="next-steps"></a>다음 단계

- [Azure Cosmos DB 문서를 만들거나 수정할 때 함수실행(트리거)](./functions-bindings-cosmosdb-v2-trigger.md)
- [Azure Cosmos DB 문서에 변경 내용 저장(출력 바인딩)](./functions-bindings-cosmosdb-v2-output.md)