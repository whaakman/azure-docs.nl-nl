---
title: Service to Service verificatie met Azure AD met behulp van OAuth 2.0 | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van HTTP-berichten voor het implementeren van service-to-service-verificatie met behulp van de OAuth 2.0 verlenen clientreferentiestroom.
services: active-directory
documentationcenter: .net
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: ryanwi
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d734db7fbedaf3e3f3cd71c31f9391a2237f5b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545266"
---
# <a name="service-to-service-calls-using-client-credentials-shared-secret-or-certificate"></a>Service-serviceaanroepen met behulp van clientreferenties (gedeeld geheim of certificaat)

[!INCLUDE [active-directory-develop-applies-v1](../../../includes/active-directory-develop-applies-v1.md)]

De OAuth 2.0-Client referenties stroom voor het verlenen van stelt een webservice (*vertrouwelijke client*) zijn eigen referenties gebruiken in plaats van een gebruiker imiteren om u te verifiëren bij het aanroepen van een andere webservice. In dit scenario wordt de client is doorgaans een middelste laag webservice, een daemon-service of website. Voor een hoger niveau van zekerheid kan Azure AD ook de aanroepende service moet een certificaat (in plaats van een gedeelde geheim genoemd) gebruiken als referentie.

## <a name="client-credentials-grant-flow-diagram"></a>Referenties voor client verlenen stroomdiagram
Het volgende diagram wordt uitgelegd hoe de stroom werkt in Azure Active Directory (Azure AD) voor het verlenen van de referenties van de client.

![Stroom voor het verlenen van OAuth 2.0-Client-referenties](./media/v1-oauth2-client-creds-grant-flow/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. De clienttoepassing wordt geverifieerd op het eindpunt van de Azure AD-token-uitgifte en vraagt een toegangstoken.
2. Het eindpunt van de Azure AD-token-uitgifte problemen met het toegangstoken.
3. Het toegangstoken wordt gebruikt voor verificatie met de beveiligde bron.
4. Gegevens van de beveiligde resource wordt geretourneerd naar de clienttoepassing.

## <a name="register-the-services-in-azure-ad"></a>De Services in Azure AD registreren
Zowel de aanroepende service en de ontvangende service registreren in Azure Active Directory (Azure AD). Zie voor gedetailleerde instructies [toepassingen integreren met Azure Active Directory](quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="request-an-access-token"></a>Aanvraag een toegangstoken
Om aan te vragen een toegangstoken, gebruikt u een HTTP POST naar de tenant-specifieke Azure AD-eindpunt.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## <a name="service-to-service-access-token-request"></a>Service-naar-service toegangstokenaanvraag
Er zijn twee mogelijke situaties, afhankelijk van of de clienttoepassing moet worden beveiligd door een gedeeld geheim, of een certificaat kiest.

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Eerste geval: Aanvraag voor een toegangstoken met een gedeeld geheim
Wanneer u een gedeeld geheim, bevat een tokenaanvraag voor de service-naar-service toegang tot de volgende parameters:

| Parameter |  | Description |
| --- | --- | --- |
| grant_type |Vereist |Hiermee geeft u het aangevraagde verlenen. In een stroom-clientreferenties de waarde moet **client_credentials**. |
| client_id |Vereist |Hiermee geeft u de Azure AD-client-id van de aanroepende webservice. In client-ID van de aanroepende toepassing, vinden de [Azure-portal](https://portal.azure.com), klikt u op **Azure Active Directory**, klikt u op **App-registraties**, klikt u op de toepassing. De client_id is de *toepassings-ID* |
| client_secret |Vereist |Geef een sleutel die is geregistreerd voor de aanroepende web service- of daemon-App in Azure AD. Klik op om een sleutel in Azure portal **Azure Active Directory**, klikt u op **App-registraties**, klik op de toepassing, **instellingen**, klikt u op **sleutels** , en voeg een sleutel.  URL-codering dit geheim bij het opgeven van deze. |
| resource |Vereist |Voer de URI van de App-ID van de ontvangende webservice. U kunt de URI van de App-ID vinden in Azure portal op **Azure Active Directory**, klikt u op **App-registraties**, klikt u op de servicetoepassing en klik vervolgens op **instellingen** en  **Eigenschappen van**. |

#### <a name="example"></a>Voorbeeld
De volgende HTTP POST-verzoeken om een [toegangstoken](access-tokens.md) voor de https://service.contoso.com/ webservice. De `client_id` identificeert de webservice die door het toegangstoken worden aangevraagd.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: Aanvraag voor een toegangstoken met een certificaat
Een service-naar-service toegangstokenaanvraag met een certificaat bevat de volgende parameters:

| Parameter |  | Description |
| --- | --- | --- |
| grant_type |Vereist |Hiermee geeft u het aangevraagde Responstype. In een stroom-clientreferenties de waarde moet **client_credentials**. |
| client_id |Vereist |Hiermee geeft u de Azure AD-client-id van de aanroepende webservice. In client-ID van de aanroepende toepassing, vinden de [Azure-portal](https://portal.azure.com), klikt u op **Azure Active Directory**, klikt u op **App-registraties**, klikt u op de toepassing. De client_id is de *toepassings-ID* |
| client_assertion_type |Vereist |De waarde moet liggen `urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Vereist | (Een JSON Web Token) een bewering die u wilt maken en te ondertekenen met het certificaat dat u geregistreerd als referenties voor uw toepassing. Meer informatie over [referenties van het certificaat](active-directory-certificate-credentials.md) voor informatie over het registreren van uw certificaat en de indeling van de verklaring.|
| resource | Vereist |Voer de URI van de App-ID van de ontvangende webservice. U kunt de URI van de App-ID vinden in Azure portal op **Azure Active Directory**, klikt u op **App-registraties**, klikt u op de servicetoepassing en klik vervolgens op **instellingen** en  **Eigenschappen van**. |

U ziet dat de parameters bijna hetzelfde als in het geval van de aanvraag van het gedeelde geheim zijn, behalve dat de waarde voor client_secret-parameter is vervangen door twee parameters: een client_assertion_type en client_assertion.

#### <a name="example"></a>Voorbeeld
De volgende HTTP-POST vraagt een toegangstoken voor de https://service.contoso.com/ webservice met een certificaat. De `client_id` identificeert de webservice die door het toegangstoken worden aangevraagd.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### <a name="service-to-service-access-token-response"></a>Service-naar-Service toegang Token antwoord

Een geslaagde reactie bevat een JSON OAuth 2.0-antwoord met de volgende parameters:

| Parameter | Description |
| --- | --- |
| access_token |Het aangevraagde toegangstoken. De aanroepende webservice kan dit token gebruiken om te verifiëren bij de ontvangende webservice. |
| token_type |Geeft aan dat de waarde van het token. Het enige type dat Azure AD ondersteunt **Bearer**. Zie voor meer informatie over bearer-tokens, de [OAuth 2.0 machtiging Framework: Bearer Token gebruik (RFC 6750)](https://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot de vervaltijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| not_before |De tijd van waaruit het toegangstoken gebruikt wordt. De datum wordt weergegeven als het aantal seconden vanaf 1970-01-01T0:0:0Z UTC tot geldigheidsduur voor het token.|
| resource |De App-ID-URI van de ontvangende webservice. |

#### <a name="example-of-response"></a>Voorbeeld van antwoord
Het volgende voorbeeld ziet een geslaagd antwoord op een verzoek om een toegangstoken uit aan een webservice.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## <a name="see-also"></a>Zie ook
* [OAuth 2.0 in Azure AD](v1-protocols-oauth-code.md)
* [Voorbeeld in C# van de aanroep van de service-to-service met een gedeeld geheim](https://github.com/Azure-Samples/active-directory-dotnet-daemon) en [voorbeeld in C# van de aanroep van de service-to-service met een certificaat](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
