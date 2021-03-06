---
title: 사용자 지정 정책에서 RESTful 기술 프로필 정의
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C의 사용자 지정 정책에서 RESTful 기술 프로필을 정의하는 방법을 설명합니다.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 03/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 410f413fc8450c0ee33c3ca95e860a3e8de34107
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80332602"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Azure Active Directory B2C 사용자 지정 정책에서 RESTful 기술 프로필 정의

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C(Azure AD B2C)는 사용자 고유의 RESTful 서비스를 통합하는 지원을 제공합니다. Azure AD B2C는 입력 클레임 컬렉션의 RESTful 서비스로 데이터를 보내고 출력 클레임 컬렉션에 데이터를 다시 수신합니다. 자세한 내용은 [Azure AD B2C 사용자 지정 정책에서 REST API 클레임 교환 통합을](custom-policy-rest-api-intro.md)참조하십시오.  

## <a name="protocol"></a>프로토콜

**Protocol** 요소의 **Name** 특성은 `Proprietary`로 설정해야 합니다. **handler** 특성은 Azure AD B2C에서 사용되는 프로토콜 처리기 어셈블리의 정규화된 이름을 포함해야 합니다. `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

다음 예제는 RESTful 기술 프로필을 보여 줍니다.

```XML
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="input-claims"></a>입력 클레임

**InputClaims** 요소는 REST API로 보낼 클레임 목록을 포함합니다. 클레임 이름을 REST API에서 정의된 이름에 매핑할 수도 있습니다. 다음 예제는 정책과 REST API 간의 매핑을 보여 줍니다. **givenName**은 REST API에 **firstName**으로 전송되는 반면, **surname**은 **lastName**으로 전송됩니다. **email** 클레임은 현재 상태대로 설정됩니다.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

**InputClaimsTransformations** 요소는 REST API로 보내기 전에 입력 클레임을 수정하거나 새 입력 클레임을 생성하는 데 사용되는 **InputClaimsTransformation** 요소 컬렉션을 포함할 수 있습니다.

## <a name="send-a-json-payload"></a>JSON 페이로드 보내기

REST API 기술 프로필을 사용하면 복잡한 JSON 페이로드를 엔드포인트로 보낼 수 있습니다.

복잡한 JSON 페이로드를 보내려면 다음을 수행하십시오.

1. [GenerateJson](json-transformations.md) 클레임 변환을 통해 JSON 페이로드를 빌드합니다.
1. REST API 기술 프로필에서:
    1. 클레임 변환에 대한 참조를 `GenerateJson` 사용하여 입력 클레임 변환을 추가합니다.
    1. 메타데이터 `SendClaimsIn` 옵션을 다음으로 설정합니다.`body`
    1. `ClaimUsedForRequestPayload` 메타데이터 옵션을 JSON 페이로드가 포함된 클레임의 이름으로 설정합니다.
    1. 입력 클레임에서 JSON 페이로드를 포함하는 입력 클레임에 대한 참조를 추가합니다.

다음 예제에서는 `TechnicalProfile` 타사 전자 메일 서비스(이 경우 SendGrid)를 사용하여 확인 전자 메일을 보냅니다.

```XML
<TechnicalProfile Id="SendGrid">
  <DisplayName>Use SendGrid's email API to send the code the the user</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://api.sendgrid.com/v3/mail/send</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="ClaimUsedForRequestPayload">sendGridReqBody</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_SendGridApiKey" />
  </CryptographicKeys>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="GenerateSendGridRequestBody" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="sendGridReqBody" />
  </InputClaims>
</TechnicalProfile>
```

## <a name="output-claims"></a>출력 클레임

**OutputClaims** 요소는 REST API에서 반환된 클레임 목록을 포함합니다. 정책에 정의된 클레임 이름을 REST API에서 정의된 이름에 매핑해야 할 수도 있습니다. `DefaultValue` 특성만 설정하면, REST API ID 공급자에서 반환되지 않은 클레임을 포함할 수도 있습니다.

**OutputClaimsTransformations** 요소는 출력 클레임을 수정하거나 새 출력 클레임을 생성하는 데 사용되는 **OutputClaimsTransformation** 요소 컬렉션을 포함할 수 있습니다.

다음 예제는 REST API에서 반환된 클레임을 보여 줍니다.

- **loyaltyNumber** 클레임 이름에 매핑된 **MembershipId** 클레임입니다.

기술 프로필은 ID 공급자에서 반환되지 않은 클레임도 반환합니다.

- 기본값이 `true`로 설정된 **loyaltyNumberIsNew** 클레임입니다.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>메타데이터

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| ServiceUrl | yes | REST API 엔드포인트의 URL입니다. |
| AuthenticationType | yes | RESTful 클레임 공급자가 수행하는 인증 형식입니다. 가능한 값은 `None`, `Basic`, `Bearer` 또는 `ClientCertificate`입니다. `None` 값은 REST API가 익명이 아님을 나타냅니다. `Basic` 값은 REST API가 HTTP 기본 인증으로 보호됨을 나타냅니다. Azure AD B2C를 포함하여 확인된 사용자만 API에 액세스할 수 있습니다. (권장) 값은 `ClientCertificate` REST API가 클라이언트 인증서 인증을 사용하여 액세스를 제한한다는 것을 나타냅니다. 적절한 인증서가 있는 서비스(예: Azure AD B2C)만 API에 액세스할 수 있습니다. 이 `Bearer` 값은 REST API가 클라이언트 OAuth2 Bearer 토큰을 사용하여 액세스를 제한한다는 것을 나타냅니다. |
| 허용안전하지 않은아우트인프로덕션| 예| 프로덕션 환경에서 `AuthenticationType` 설정할 `none` 수 있는지 여부를`DeploymentMode` 나타냅니다(TrustFrameworkPolicy가 `Production`설정되어 있음). [TrustFrameworkPolicy](trustframeworkpolicy.md) 가능한 값: true 또는 false(기본값). |
| SendClaimsIn | 예 | 입력 클레임이 RESTful 클레임 공급자에게 전송되는 방법을 지정합니다. 가능한 값은 `Body`(기본값), `Form`, `Header` 또는 `QueryString`입니다. `Body` 값은 JSON 형식의 요청 본문에 전송되는 입력 클레임입니다. `Form` 값은 앰퍼샌드 '&'로 구분된 키 값 형식의 요청 본문에 전송되는 입력 클레임입니다. `Header` 값은 요청 헤더에 전송되는 입력 클레임입니다. `QueryString` 값은 요청 쿼리 문자열에 전송되는 입력 클레임입니다. 각 동사에서 호출하는 HTTP 동사는 다음과 같습니다.<br /><ul><li>`Body`: 포스트</li><li>`Form`: 포스트</li><li>`Header`: GET</li><li>`QueryString`: GET</li></ul> |
| ClaimsFormat | 예 | 현재 사용되지 않는 무시할 수 있습니다. |
| 클레임사용된For요청페이로드| 예 | REST API로 보낼 페이로드를 포함하는 문자열 클레임의 이름입니다. |
| DebugMode | 예 | 디버그 모드에서 기술 프로필을 실행합니다. 가능한 값: `true` `false` " 또는 (기본값) . 디버그 모드에서 REST API는 자세한 정보를 반환할 수 있습니다. 오류 [반환 메시지](#returning-error-message) 섹션을 참조하십시오. |
| 클레임 해결내 처리 포함  | 예 | 입력 및 출력 클레임의 경우 [클레임 해결이](claim-resolver-overview.md) 기술 프로필에 포함되는지 여부를 지정합니다. 가능한 값: `true` `false`  " 또는 (기본값) . 기술 프로필에서 클레임 확인자를 사용하려면 이 것을 `true`로 설정합니다. |
| 리졸브JsonPathsInJson토큰  | 예 | 기술 프로파일이 JSON 경로를 해결하는지 여부를 나타냅니다. 가능한 값: `true` `false` " 또는 (기본값) . 이 메타데이터를 사용하여 중첩된 JSON 요소의 데이터를 읽습니다. [OutputClaim에서](technicalprofiles.md#outputclaims) `PartnerClaimType` 출력할 JSON 경로 요소로 설정합니다. 예를 들어: `firstName.localized` `data.0.to.0.email`또는 .|
| 사용 클레임아베어토큰| 예| 베어러 토큰을 포함하는 클레임의 이름입니다.|

## <a name="cryptographic-keys"></a>암호화 키

인증 형식이 `None`으로 설정된 경우 **CryptographicKeys** 요소가 사용되지 않습니다.

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
</TechnicalProfile>
```

인증 형식이 `Basic`으로 설정된 경우 **CryptographicKeys** 요소에 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | yes | 인증에 사용되는 사용자 이름입니다. |
| BasicAuthenticationPassword | yes | 인증에 사용되는 암호입니다. |

다음 예제는 기본 인증을 사용하는 기술 프로필을 보여 줍니다.

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Basic</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
  </CryptographicKeys>
</TechnicalProfile>
```

인증 형식이 `ClientCertificate`으로 설정된 경우 **CryptographicKeys** 요소에 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| ClientCertificate | yes | 인증에 사용할 X509 인증서(RSA 키 집합)입니다. |

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ClientCertificate</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
  </CryptographicKeys>
</TechnicalProfile>
```

인증 형식이 `Bearer`으로 설정된 경우 **CryptographicKeys** 요소에 다음 특성이 포함됩니다.

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| 베어러 인증토큰 | 예 | OAuth 2.0 베어러 토큰. |

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_B2cRestClientAccessToken" />
  </CryptographicKeys>
</TechnicalProfile>
```

## <a name="returning-error-message"></a>오류 메시지 반환

REST API가 'CRM 시스템에서 사용자를 찾을 수 없습니다.'와 같은 오류 메시지를 반환해야 할 수 있습니다. 오류가 발생하면 REST API는 HTTP 4xx 오류 메시지(예: 400(잘못된 요청) 또는 409(충돌) 응답 상태 코드를 반환해야 합니다. 응답 본문에는 JSON에 서식이 지정된 오류 메시지가 포함되어 있습니다.

```JSON
{
  "version": "1.0.0",
  "status": 409,
  "code": "API12345",
  "requestId": "50f0bd91-2ff4-4b8f-828f-00f170519ddb",
  "userMessage": "Message for the user",
  "developerMessage": "Verbose description of problem and how to fix it.",
  "moreInfo": "https://restapi/error/API12345/moreinfo"
}
```

| 특성 | 필수 | 설명 |
| --------- | -------- | ----------- |
| 버전 | yes | REST API 버전입니다. 예: 1.0.1 |
| 상태 | yes | 409이어야 합니다. |
| 코드 | 예 | `DebugMode`를 사용으로 설정한 경우에 표시되는 RESTful 엔드포인트 공급자의 오류 코드입니다. |
| requestId | 예 | `DebugMode`를 사용으로 설정한 경우에 표시되는 RESTful 엔드포인트 공급자의 요청 식별자입니다. |
| userMessage | yes | 사용자에게 표시되는 오류 메시지입니다. |
| developerMessage | 예 | `DebugMode`를 사용으로 설정한 경우에 표시되는, 문제점 및 해결 방법에 대한 자세한 설명입니다. |
| moreInfo | 예 | `DebugMode`를 사용으로 설정한 경우에 표시되는, 추가 정보를 가리키는 URI입니다. |


다음 예제는 오류 메시지를 반환하는 C# 클래스를 보여 줍니다.

```csharp
public class ResponseContent
{
  public string version { get; set; }
  public int status { get; set; }
  public string code { get; set; }
  public string userMessage { get; set; }
  public string developerMessage { get; set; }
  public string requestId { get; set; }
  public string moreInfo { get; set; }
}
```

## <a name="next-steps"></a>다음 단계

RESTful 기술 프로필 사용의 예는 다음 문서를 참조하십시오.

- [Azure AD B2C 사용자 지정 정책에서 REST API 클레임 교환 통합](custom-policy-rest-api-intro.md)
- [연습: 사용자 입력의 유효성 검사로 Azure AD B2C 사용자 여정에서 REST API 클레임 교환 통합](custom-policy-rest-api-claims-validation.md)
- [연습: Azure Active Directory B2C의 사용자 지정 정책에 REST API 클레임 교환 추가](custom-policy-rest-api-claims-validation.md)
- [REST API 서비스 보안](secure-rest-api.md)

