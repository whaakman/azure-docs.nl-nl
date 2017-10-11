---
title: Azure AD Services Auth met OAuth2.0 | Microsoft Docs
description: In dit artikel wordt beschreven hoe het gebruik van HTTP-berichten voor het implementeren van services-verificatie met behulp van de OAuth2.0 grant clientreferentiestroom.
services: active-directory
documentationcenter: .net
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: a7f939d9-532d-4b6d-b6d3-95520207965d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: cc30a54cd56c0cb03a67f86e4552398baa764e58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# Service naar serviceaanroepen met clientreferenties (gedeelde geheim of certificaat)
De OAuth 2.0-Client referenties Grant stromen is toegestaan voor een webservice (*vertrouwelijke client*) zijn eigen referenties gebruiken in plaats van een gebruiker imiteren om te verifiëren bij het aanroepen van een andere webservice. In dit scenario wordt is de client meestal een middelste laag webservice, een daemon-service of de website. Voor een hoger niveau van zekerheid kan Azure AD ook de aanroepende service moet een certificaat (in plaats van een gedeeld geheim) gebruiken als referentie.

## Clientreferenties verlenen stroomdiagram
Het volgende diagram wordt uitgelegd hoe de stroom werkt in Azure Active Directory (Azure AD) voor het verlenen van referenties van de client.

![OAuth2.0 Grant Clientreferentiestroom](media/active-directory-protocols-oauth-service-to-service/active-directory-protocols-oauth-client-credentials-grant-flow.jpg)

1. De clienttoepassing wordt geverifieerd met het eindpunt van de uitgifte van tokens Azure AD en vraagt een toegangstoken.
2. Het eindpunt van de uitgifte van tokens Azure AD geeft het toegangstoken.
3. Het toegangstoken is gebruikt voor verificatie met de beveiligde bron.
4. Gegevens van de beveiligde bron wordt geretourneerd naar de webtoepassing.

## De Services in Azure AD registreren
Registreer de aanroepende service en de ontvangende service in Azure Active Directory (Azure AD). Zie voor gedetailleerde instructies [toepassingen integreren met Azure Active Directory](active-directory-integrating-applications.md).

## Aanvragen van een toegangstoken
Om aan te vragen een toegangstoken, gebruikt u een HTTP POST naar de tenant-specifieke Azure AD-eindpunt.

```
https://login.microsoftonline.com/<tenant id>/oauth2/token
```

## Service-naar-service toegang tokenaanvraag
Er zijn twee gevallen, afhankelijk van of de clienttoepassing kiest om te worden beveiligd door een gedeeld geheim of een certificaat.

### Het eerste aanvraagnummer: aanvraag voor toegang tot token met een gedeeld geheim
Wanneer u een gedeeld geheim, bevat een tokenaanvraag voor service-naar-service toegang tot de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |Vereist |Geeft het type voor aangevraagde verlenen. In een stroom Client referenties Grant de waarde moet **client_credentials**. |
| client_id |Vereist |Hiermee geeft u de Azure AD-client-id van de webservice aanroepen. In client-ID van de aanroepende toepassing, vinden de [Azure-portal](https://portal.azure.com), klikt u op **Active Directory**, directory overschakelen, klikt u op de toepassing. De client_id is de *toepassings-ID* |
| client_secret |Vereist |Geef een sleutel die is geregistreerd voor de aanroepende web service of -daemon toepassing in Azure AD. Klik op om een sleutel in de Azure portal **Active Directory**, directory overschakelen, klikt u op de toepassing, klikt u op **instellingen**, klikt u op **sleutels**, en een sleutel toevoegen.|
| Resource |Vereist |Voer de App ID URI van de ontvangende webservice. Klik op de App ID URI, informatie in de Azure portal **Active Directory**, directory overschakelen, klikt u op de servicetoepassing en klik vervolgens op **instellingen** en **eigenschappen** |

#### Voorbeeld
De volgende HTTP POST-aanvragen een toegangstoken voor de webservice https://service.contoso.com/. De `client_id` identificeert de webservice die het toegangstoken aanvragen.

```
POST /contoso.com/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&client_id=625bc9f6-3bf6-4b6d-94ba-e97cf07a22de&client_secret=qkDwDJlDfig2IpeuUZYKH1Wb8q1V0ju6sILxQQqhJ+s=&resource=https%3A%2F%2Fservice.contoso.com%2F
```

### Tweede geval: aanvraag voor toegang tot token met een certificaat
Een service-naar-service toegang tokenaanvraag met een certificaat bevat de volgende parameters:

| Parameter |  | Beschrijving |
| --- | --- | --- |
| grant_type |Vereist |Hiermee geeft u het aangevraagde antwoordtype. In een stroom Client referenties Grant de waarde moet **client_credentials**. |
| client_id |Vereist |Hiermee geeft u de Azure AD-client-id van de webservice aanroepen. In client-ID van de aanroepende toepassing, vinden de [Azure-portal](https://portal.azure.com), klikt u op **Active Directory**, directory overschakelen, klikt u op de toepassing. De client_id is de *toepassings-ID* |
| client_assertion_type |Vereist |De waarde moet liggen`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Vereist | Een bewering (een JSON Web Token) die u nodig hebt voor het maken en te ondertekenen met het certificaat u geregistreerd als referenties voor uw toepassing. Meer informatie over [referenties van het certificaat](active-directory-certificate-credentials.md) voor informatie over het registreren van uw certificaat en de indeling van de bevestiging.|
| Resource | Vereist |Voer de App ID URI van de ontvangende webservice. Klik op de App ID URI, informatie in de Azure portal **Active Directory**, klikt u op de map, klik op de toepassing en klik vervolgens op **configureren**. |

De parameters zijn bijna hetzelfde is in het geval van de aanvraag door een gedeeld geheim, behalve dat de parameter client_secret wordt vervangen door twee parameters: een client_assertion_type en client_assertion.

#### Voorbeeld
De volgende HTTP POST-aanvragen een toegangstoken voor de webservice https://service.contoso.com/ met een certificaat. De `client_id` identificeert de webservice die het toegangstoken aanvragen.

```
POST /<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

resource=https%3A%2F%contoso.onmicrosoft.com%2Ffc7664b4-cdd6-43e1-9365-c2e1c4e1b3bf&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

### Service-naar-Service toegang Token antwoord

Een geslaagde reactie bevat een JSON OAuth 2.0-antwoord met de volgende parameters:

| Parameter | Beschrijving |
| --- | --- |
| access_token |Het aangevraagde toegangstoken. De webservice aanroepen kunt dit token voor verificatie aan de ontvangende webservice gebruiken. |
| token_type |Geeft de waarde van het type token. Het enige type dat ondersteunt Azure AD is **Bearer**. Zie voor meer informatie over bearer-tokens, de [OAuth 2.0 autorisatie Framework: Bearer-Token gebruik (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt). |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |
| expires_on |De tijd wanneer het toegangstoken is verlopen. De datum die wordt weergegeven als het aantal seconden van 1970-01-01T0:0:0Z UTC totdat de verlooptijd. Deze waarde wordt gebruikt om te bepalen van de levensduur van tokens in de cache. |
| not_before |Het tijdstip van waaruit het toegangstoken gebruikt wordt. De datum die wordt weergegeven als het aantal seconden van 1970-01-01T0:0:0Z UTC totdat de tijd van geldigheid voor het token.|
| Resource |De App ID URI van de ontvangende webservice. |

#### Voorbeeld van antwoord
Het volgende voorbeeld toont een geslaagde reactie op een aanvraag voor een toegangstoken bij een webservice.

```
{
"access_token":"eyJhbGciOiJSUzI1NiIsIng1dCI6IjdkRC1nZWNOZ1gxWmY3R0xrT3ZwT0IyZGNWQSIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJodHRwczovL3NlcnZpY2UuY29udG9zby5jb20vIiwiaXNzIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvN2ZlODE0NDctZGE1Ny00Mzg1LWJlY2ItNmRlNTdmMjE0NzdlLyIsImlhdCI6MTM4ODQ0ODI2NywibmJmIjoxMzg4NDQ4MjY3LCJleHAiOjEzODg0NTIxNjcsInZlciI6IjEuMCIsInRpZCI6IjdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZSIsIm9pZCI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsInN1YiI6ImE5OTE5MTYyLTkyMTctNDlkYS1hZTIyLWYxMTM3YzI1Y2RlYSIsImlkcCI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzdmZTgxNDQ3LWRhNTctNDM4NS1iZWNiLTZkZTU3ZjIxNDc3ZS8iLCJhcHBpZCI6ImQxN2QxNWJjLWM1NzYtNDFlNS05MjdmLWRiNWYzMGRkNThmMSIsImFwcGlkYWNyIjoiMSJ9.aqtfJ7G37CpKV901Vm9sGiQhde0WMg6luYJR4wuNR2ffaQsVPPpKirM5rbc6o5CmW1OtmaAIdwDcL6i9ZT9ooIIicSRrjCYMYWHX08ip-tj-uWUihGztI02xKdWiycItpWiHxapQm0a8Ti1CWRjJghORC1B1-fah_yWx6Cjuf4QE8xJcu-ZHX0pVZNPX22PHYV5Km-vPTq2HtIqdboKyZy3Y4y3geOrRIFElZYoqjqSv5q9Jgtj5ERsNQIjefpyxW3EwPtFqMcDm4ebiAEpoEWRN4QYOMxnC9OUBeG9oLA0lTfmhgHLAtvJogJcYFzwngTsVo6HznsvPWy7UP3MINA",
"token_type":"Bearer",
"expires_in":"3599",
"expires_on":"1388452167",
"resource":"https://service.contoso.com/"
}
```

## Zie ook
* [OAuth 2.0 in Azure AD](active-directory-protocols-oauth-code.md)
* [Voorbeeld in C# van de aanroep van de service-service met een gedeeld geheim](https://github.com/Azure-Samples/active-directory-dotnet-daemon) en [voorbeeld in C# van de aanroep van de service-service met een certificaat](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential)
