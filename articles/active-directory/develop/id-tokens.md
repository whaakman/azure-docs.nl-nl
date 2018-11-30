---
title: Azure Active Directory-ID tokenverwijzing | Microsoft Docs
description: Informatie over het gebruik van id_tokens verzonden door de v1.0 en v2.0-eindpunten van Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: ab2c0f671eaf6147baad24b426c4a527f07e136f
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422402"
---
# <a name="id-tokens"></a>Id-tokens

`id_tokens` worden verzonden naar de clienttoepassing als onderdeel van een [OpenID Connect](v1-protocols-openid-connect-code.md) stroom. Ze kunnen worden verzonden, naast of in plaats van een toegangstoken en worden door de client gebruikt voor het verifiëren van de gebruiker. 

## <a name="using-the-idtoken"></a>Met behulp van de id_token

ID-Tokens moeten worden gebruikt om te valideren dat een gebruiker daadwerkelijk wie zij beweren is te zijn en krijg nuttige informatie over deze - mag niet worden gebruikt voor autorisatie in plaats van een [toegangstoken](access-tokens.md). De claims die het biedt kunnen worden gebruikt voor UX in uw toepassing, een database te versleutelen en bieden toegang tot de clienttoepassing. 

## <a name="claims-in-an-idtoken"></a>Claims in een id_token

`id_tokens` voor een Microsoft-identiteit zijn [JWTs](https://tools.ietf.org/html/rfc7519), wat betekent dat ze bestaan uit een koptekst, nettolading en handtekening gedeelte. U kunt de header en -nettolading gebruiken om te controleren of de echtheid van het token, terwijl de nettolading de informatie over de gebruiker die is aangevraagd door de client bevat. Tenzij anders wordt vermeld, worden alle claims die hier worden vermeld in zowel v1.0 en v2.0-tokens weergegeven.

### <a name="v10"></a>v1.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q
```

Dit token v1.0 voorbeeld in weergeven [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyIsImtpZCI6IjdfWnVmMXR2a3dMeFlhSFMzcTZsVWpVWUlHdyJ9.eyJhdWQiOiJiMTRhNzUwNS05NmU5LTQ5MjctOTFlOC0wNjAxZDBmYzljYWEiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkvIiwiaWF0IjoxNTM2Mjc1MTI0LCJuYmYiOjE1MzYyNzUxMjQsImV4cCI6MTUzNjI3OTAyNCwiYWlvIjoiQVhRQWkvOElBQUFBcXhzdUIrUjREMnJGUXFPRVRPNFlkWGJMRDlrWjh4ZlhhZGVBTTBRMk5rTlQ1aXpmZzN1d2JXU1hodVNTajZVVDVoeTJENldxQXBCNWpLQTZaZ1o5ay9TVTI3dVY5Y2V0WGZMT3RwTnR0Z2s1RGNCdGsrTExzdHovSmcrZ1lSbXY5YlVVNFhscGhUYzZDODZKbWoxRkN3PT0iLCJhbXIiOlsicnNhIl0sImVtYWlsIjoiYWJlbGlAbWljcm9zb2Z0LmNvbSIsImZhbWlseV9uYW1lIjoiTGluY29sbiIsImdpdmVuX25hbWUiOiJBYmUiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMWRiNDcvIiwiaXBhZGRyIjoiMTMxLjEwNy4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJub25jZSI6IjEyMzUyMyIsIm9pZCI6IjA1ODMzYjZiLWFhMWQtNDJkNC05ZWMwLTFiMmJiOTE5NDQzOCIsInJoIjoiSSIsInN1YiI6IjVfSjlyU3NzOC1qdnRfSWN1NnVlUk5MOHhYYjhMRjRGc2dfS29vQzJSSlEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6IkFiZUxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJMeGVfNDZHcVRrT3BHU2ZUbG40RUFBIiwidmVyIjoiMS4wIn0=.UJQrCA6qn2bXq57qzGX_-D3HcPHqBMOKDPx4su1yKRLNErVD8xkxJLNLVRdASHqEcpyDctbdHccu6DPpkq5f0ibcaQFhejQNcABidJCTz0Bb2AbdUCTqAzdt9pdgQvMBnVH1xk3SCM6d4BbT4BkLLj10ZLasX7vRknaSjE_C5DI7Fg4WrZPwOhII1dB0HEZ_qpNaYXEiy-o94UJ94zCr07GgrqMsfYQqFR7kn-mn68AjvLcgwSfZvyR_yIK75S_K37vC3QryQ7cNoafDe9upql_6pB2ybMVlgWPs_DmbJ8g0om-sPlwyn74Cc1tW3ze-Xptw_2uVdPgWyqfuWAfq6Q).

### <a name="v20"></a>v2.0

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTE4ODA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjMzMzgwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0=.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw
```

Dit token v2.0 voorbeeld in weergeven [jwt.ms](https://jwt.ms/#id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IjFMVE16YWtpaGlSbGFfOHoyQkVKVlhlV01xbyJ9.eyJ2ZXIiOiIyLjAiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vOTE4ODA0MGQtNmM2Ny00YzViLWIxMTItMzZhMzA0YjY2ZGFkL3YyLjAiLCJzdWIiOiJBQUFBQUFBQUFBQUFBQUFBQUFBQUFJa3pxRlZyU2FTYUZIeTc4MmJidGFRIiwiYXVkIjoiNmNiMDQwMTgtYTNmNS00NmE3LWI5OTUtOTQwYzc4ZjVhZWYzIiwiZXhwIjoxNTM2MzYxNDExLCJpYXQiOjE1MzYyNzQ3MTEsIm5iZiI6MTUzNjI3NDcxMSwibmFtZSI6IkFiZSBMaW5jb2xuIiwicHJlZmVycmVkX3VzZXJuYW1lIjoiQWJlTGlAbWljcm9zb2Z0LmNvbSIsIm9pZCI6IjAwMDAwMDAwLTAwMDAtMDAwMC02NmYzLTMzMzJlY2E3ZWE4MSIsInRpZCI6IjMzMzgwNDBkLTZjNjctNGM1Yi1iMTEyLTM2YTMwNGI2NmRhZCIsIm5vbmNlIjoiMTIzNTIzIiwiYWlvIjoiRGYyVVZYTDFpeCFsTUNXTVNPSkJjRmF0emNHZnZGR2hqS3Y4cTVnMHg3MzJkUjVNQjVCaXN2R1FPN1lXQnlqZDhpUURMcSFlR2JJRGFreXA1bW5PcmNkcUhlWVNubHRlcFFtUnA2QUlaOGpZIn0=.1AFWW-Ck5nROwSlltm7GzZvDwUkqvhSQpm55TQsmVo9Y59cLhRXpvB8n-55HCr9Z6G_31_UbeUkoz612I2j_Sm9FFShSDDjoaLQr54CreGIJvjtmS3EkK9a7SJBbcpL1MpUtlfygow39tFjY7EVNW9plWUvRrTgVk7lYLprvfzw-CIqw3gHC-T7IK_m_xkr08INERBtaecwhTeN4chPC4W3jdmw_lIxzC48YoQ0dB1L9-ImX98Egypfrlbm0IBL5spFzL6JDZIRRJOu8vecJvj1mq-IUhGt0MacxX8jdxYLP-KUu2d9MbNKpCKJuZ7p8gwTL5B7NlUdh_dmSviPWrw).

### <a name="header-claims"></a>Header-claims

|Claim | Indeling | Beschrijving |
|-----|--------|-------------|
|`typ` | Tekenreeks - altijd "JWT" | Geeft aan dat het token een JWT.|
|`alg` | Reeks | Geeft aan dat de algoritme die is gebruikt voor het ondertekenen van het token. Voorbeeld: "RS256" |
|`kid` | Reeks | Vingerafdruk voor de openbare sleutel gebruikt voor het ondertekenen van dit token. Worden weergegeven in zowel v1.0 en v2.0 `id_tokens`. |
|`x5t` | Reeks | Hetzelfde (in gebruik en de waarde) als `kid`. Dit is echter een verouderde claim verzonden alleen in v1.0 `id_tokens` voor compatibiliteit van toepassing. |

### <a name="payload-claims"></a>De nettolading van claims

|Claim | Indeling | Beschrijving |
|-----|--------|-------------|
|`aud` |  Tekenreeks, een URI van de App-ID | Hiermee geeft u de beoogde ontvanger van het token. In `id_tokens`, de doelgroep is van uw app toepassings-ID, toegewezen aan uw app in Azure portal. Uw app moet deze waarde te valideren en het token te negeren als de waarde komt niet overeen met. |
|`iss` |  Tekenreeks, een STS-URI | Identificeert de beveiligingstokenservice (STS) die wordt gemaakt en retourneert het token en de Azure AD-tenant waarin de gebruiker is geverifieerd. Als het token is uitgegeven door het v2.0-eindpunt, de URI wordt beëindigd `/v2.0`.  De GUID die wordt aangegeven dat de gebruiker een consument gebruiker vanuit een Microsoft-account is `9188040d-6c67-4c5b-b112-36a304b66dad`. Uw app moet de GUID-gedeelte van de claim gebruiken om het beperken van de set van tenants die kunnen zich aanmelden bij de app, indien van toepassing. |
|`iat` |  int, een UNIX-timestamp | "Verleend aan" geeft aan wanneer de verificatie voor dit token is opgetreden.  |
|`idp`|Tekenreeks, meestal een STS-URI | Registreert de identiteitsprovider waarmee het onderwerp van het token is geverifieerd. Deze waarde is gelijk aan de waarde van de claim van verlener, tenzij het gebruikersaccount niet in dezelfde tenant als de verlener - gasten, bijvoorbeeld. Als de claim is niet aanwezig is, betekent dit dat de waarde van `iss` in plaats daarvan kan worden gebruikt.  Voor persoonlijke accounts worden gebruikt in een context orgnizational (bijvoorbeeld een persoonlijk account is uitgenodigd voor een Azure AD-tenant), de `idp` claim mogelijk 'live.com' of een STS URI met de tenant van Microsoft-account `9188040d-6c67-4c5b-b112-36a304b66dad`. |
|`nbf` |  int, een UNIX-timestamp | De claim 'nbf"(niet voor) geeft de tijd waarbinnen de JWT moet niet worden geaccepteerd voor verwerking.|
|`exp` |  int, een UNIX-timestamp | De claim 'exp' (verlooptijd) identificeert de verlooptijd op of na die de JWT mag niet worden geaccepteerd voor verwerking.  Het is belangrijk te weten dat een resource het token voordat deze tijd ook - weigeren kan als u bijvoorbeeld een wijziging in de verificatie is vereist of intrekken van een token is gedetecteerd. |
| `c_hash`| Reeks |De hash van de code is opgenomen in de ID-tokens, alleen wanneer de ID-token dat is uitgegeven met een OAuth 2.0-autorisatiecode. Het kan worden gebruikt om te valideren de echtheid van een autorisatiecode. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html). |
|`at_hash`| Reeks |Toegang tot de token-hash is opgenomen in de ID tokens alleen wanneer de ID-token dat is uitgegeven met een OAuth 2.0-toegangstoken. Het kan worden gebruikt om te valideren de echtheid van een toegangstoken. Zie voor meer informatie over het uitvoeren van deze validatie de [OpenID Connect-specificatie](https://openid.net/specs/openid-connect-core-1_0.html). |
|`aio` | Ondoorzichtige tekenreeks | Een interne claim die worden gebruikt door Azure AD om gegevens te noteren voor hergebruik van token. Moeten worden genegeerd.|
|`preferred_username` | Reeks | De primaire gebruikersnaam die de gebruiker aangeeft. Het is mogelijk een e-mailadres, telefoonnummer of een algemene gebruikersnaam zonder een indeling die is opgegeven. De waarde ervan is veranderlijke en na verloop van tijd veranderen. Omdat dit veranderlijke, moet deze waarde niet worden gebruikt om autorisatie beslissingen te nemen. De `profile` bereik is vereist voor het ontvangen van deze claim.|
|`email` | Reeks | De `email` claim bevindt zich standaard voor de gastaccounts waarvoor een e-mailadres.  Uw app kunt aanvragen tot de claim e-mailadres voor beheerde gebruikers (die uit dezelfde tenant als de resource) met behulp van de `email` [optionele claim](active-directory-optional-claims.md).  Op het v2.0-eindpunt kunt uw app ook vragen de `email` bereik OpenID Connect - hoeft u niet om aan te vragen de optionele claim en het bereik, om op te halen van de claim.  De e-claim ondersteunt alleen adresseerbare e-mail van de profielgegevens van de gebruiker. |
|`name` | Reeks | De `name` claim biedt een leesbare waarde die aangeeft in het onderwerp van het token. De waarde kan niet worden gegarandeerd uniek te zijn, is het veranderlijke en is ontworpen om alleen worden gebruikt voor weer te geven. De `profile` bereik is vereist voor het ontvangen van deze claim. |
|`nonce`| Reeks | De nonce overeenkomt met de parameter die is opgenomen in de oorspronkelijke / aanvraag om de id-provider te autoriseren. Als deze niet overeenkomt, moet uw toepassing het token weigeren. |
|`oid` | Tekenreeks, een GUID | De onveranderbare id voor een object in het Microsoft-identiteitssysteem, in dit geval een gebruikersaccount. Deze ID is uniek voor de gebruiker voor toepassingen: twee verschillende toepassingen die zich in dezelfde gebruiker ontvangt de dezelfde waarde in de `oid` claim. De Microsoft Graph retourneert deze ID als de `id` eigenschap voor een bepaalde gebruikersaccount. Omdat de `oid` kunnen meerdere apps correleren van gebruikers, de `profile` bereik is vereist voor het ontvangen van deze claim. Houd er rekening mee dat als een enkele gebruiker in meerdere tenants bestaat, de gebruiker een ander object-ID in elke tenant bevat: deze worden beschouwd als andere accounts, zelfs als de gebruiker meldt zich aan bij elk account met dezelfde referenties. |
|`rh` | Ondoorzichtige tekenreeks |Een interne claim die door Azure gebruikt voor het valideren van tokens. Moeten worden genegeerd. |
|`sub` | Tekenreeks, een GUID | De principal waarover het token worden bevestigd met gegevens, zoals de gebruiker van een app. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Het onderwerp is een pairwise id - het is uniek is voor een bepaalde toepassing-ID. Als een enkele gebruiker zich in twee verschillende apps met behulp van de twee andere client-id's, krijgen die apps dus twee verschillende waarden voor de claim onderwerp. Dit kan wel of niet kan worden gewenst is afhankelijk van uw architectuur en privacy-vereisten. |
|`tid` | Tekenreeks, een GUID | Een GUID die de Azure AD-tenant die door de gebruiker van vertegenwoordigt. De GUID is voor werk- en schoolaccounts accounts, de onveranderbare tenant-ID van de organisatie die de gebruiker behoort. De waarde voor persoonlijke accounts heeft `9188040d-6c67-4c5b-b112-36a304b66dad`. De `profile` bereik is vereist voor het ontvangen van deze claim. |
|`unique_name` | Reeks | Biedt een voor mensen leesbare waarde waarmee het onderwerp van het token wordt geïdentificeerd. Deze waarde is niet noodzakelijkerwijs uniek zijn binnen een tenant en mag alleen worden gebruikt voor weer te geven. Alleen uitgegeven met v1.0 `id_tokens`. |
|`uti` | Ondoorzichtige tekenreeks | Een interne claim die door Azure gebruikt voor het valideren van tokens. Moeten worden genegeerd. |
|`ver` | Tekenreeks, 1.0 of 2.0 | Geeft de versie van het id_token. |

## <a name="validating-an-idtoken"></a>Een id_token valideren

Valideren van een `id_token` is vergelijkbaar met de eerste stap van [valideren van een toegangstoken](access-tokens.md#validating-tokens) : de client moet valideren dat de juiste verlener terug het token heeft verzonden en die nog niet is geknoeid. Omdat `id_tokens` zijn altijd een JWT, veel bibliotheken bestaan voor het valideren van deze tokens - raden wij aan u een van deze in plaats van zelf doen. 

Voor het handmatig valideren van het token, Zie de details van de stappen in [valideren van een toegangstoken](access-tokens.md#validating-tokens). Nadat de handtekening van de tokens wilt valideren, moeten de volgende claims worden gevalideerd in de id_token (dit kunnen ook worden uitgevoerd door de validatie van tokens-bibliotheek):

* Tijdstempels: de `iat`, `nbf`, en `exp` tijdstempels moet alle valt vóór of na de huidige tijd, waar nodig. 
* Doelgroep: de `aud` claim moet overeenkomen met de app-ID voor uw toepassing.
* Nonce: de `nonce` claim in de nettolading moet overeenkomen met de nonce parameter doorgegeven aan het eindpunt voor autorisatie tijdens de eerste aanvraag.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [toegangstokens voor Azure AD](access-tokens.md)
