---
title: Toegang tot Azure Active Directory tokens referentie | Microsoft Docs
description: Toegangstokens accepteren verzonden door de v1.0 en v2.0 eindpunten van Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: df02ab01-3490-419b-90f5-be7adaeadd58
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: c6399c2662a655f1c4ba50380a5ac4dde6ddda78
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221155"
---
# <a name="azure-active-directory-access-tokens"></a>Azure Active Directory-toegangstokens

Toegangstokens zodat clients kunnen veilig aanroepen van API's die zijn beveiligd door Azure. Azure Active Directory (Azure AD)-toegangstokens zijn [JWTs](https://tools.ietf.org/html/rfc7519), met Base64 gecodeerde JSON-objecten die zijn ondertekend door Azure. Clients moeten toegang tokens als ondoorzichtige tekenreeksen, als de inhoud van het token zijn bedoeld voor de resource alleen behandeld. Voor validatie en foutopsporing, ontwikkelaars kunnen worden gedecodeerd JWTs met behulp van een site, zoals [jwt.ms](https://jwt.ms). De client kan een toegangstoken ophalen uit een eindpunt (v1.0 of v2.0) met behulp van verschillende protocollen.

Wanneer u een toegangstoken aanvraagt, retourneert Azure AD ook een aantal metagegevens over het toegangstoken voor het gebruiken van uw app. Deze informatie omvat de verlooptijd van het toegangstoken en de bereiken waarvoor deze geldig is. Deze gegevens kan uw app uit te voeren intelligente cachebewerkingen van toegangstokens zonder te parseren van het toegangstoken zelf.

Als uw toepassing een resource (web-API) die clients de toegang tot aanvragen is kunnen, bieden toegangstokens nuttige informatie voor gebruik bij verificatie en autorisatie, zoals de gebruiker, client, uitgever, machtigingen en meer. 

Zie de volgende secties voor meer informatie over hoe een resource kunt valideren en de claims binnen een toegangstoken gebruiken.

> [!NOTE]
> Tijdens het testen van uw client-toepassing met een persoonlijk account (zoals hotmail.com of outlook.com), merkt u misschien dat het toegangstoken dat is ontvangen door de client een ondoorzichtige tekenreeks is. Dit is omdat de resource die wordt benaderd verouderde tickets van de beheerde Serviceaccounts (Microsoft-account) die zijn versleuteld en kunnen niet worden geïnterpreteerd door de client heeft aangevraagd.

## <a name="sample-tokens"></a>Voorbeeld van tokens

V1.0 en v2.0-tokens sterk lijken en veel van de dezelfde claims bevatten. Een voorbeeld van elk die hier beschikbaar.

### <a name="v10"></a>v1.0

   `eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiJlZjFkYTlkNC1mZjc3LTRjM2UtYTAwNS04NDBjM2Y4MzA3NDUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTUyMjIyOS8iLCJpYXQiOjE1MzcyMzMxMDYsIm5iZiI6MTUzNzIzMzEwNiwiZXhwIjoxNTM3MjM3MDA2LCJhY3IiOiIxIiwiYWlvIjoiQVhRQWkvOElBQUFBRm0rRS9RVEcrZ0ZuVnhMaldkdzhLKzYxQUdyU091TU1GNmViYU1qN1hPM0libUQzZkdtck95RCtOdlp5R24yVmFUL2tES1h3NE1JaHJnR1ZxNkJuOHdMWG9UMUxrSVorRnpRVmtKUFBMUU9WNEtjWHFTbENWUERTL0RpQ0RnRTIyMlRJbU12V05hRU1hVU9Uc0lHdlRRPT0iLCJhbXIiOlsid2lhIl0sImFwcGlkIjoiNzVkYmU3N2YtMTBhMy00ZTU5LTg1ZmQtOGMxMjc1NDRmMTdjIiwiYXBwaWRhY3IiOiIwIiwiZW1haWwiOiJBYmVMaUBtaWNyb3NvZnQuY29tIiwiZmFtaWx5X25hbWUiOiJMaW5jb2xuIiwiZ2l2ZW5fbmFtZSI6IkFiZSAoTVNGVCkiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMjIyNDcvIiwiaXBhZGRyIjoiMjIyLjIyMi4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJvaWQiOiIwMjIyM2I2Yi1hYTFkLTQyZDQtOWVjMC0xYjJiYjkxOTQ0MzgiLCJyaCI6IkkiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJsM19yb0lTUVUyMjJiVUxTOXlpMmswWHBxcE9pTXo1SDNaQUNvMUdlWEEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6ImFiZWxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJGVnNHeFlYSTMwLVR1aWt1dVVvRkFBIiwidmVyIjoiMS4wIn0=.D3H6pMUtQnoJAGq6AHd`

Dit token v1.0 in weergeven [JWT.ms](https://jwt.ms/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiJlZjFkYTlkNC1mZjc3LTRjM2UtYTAwNS04NDBjM2Y4MzA3NDUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTUyMjIyOS8iLCJpYXQiOjE1MzcyMzMxMDYsIm5iZiI6MTUzNzIzMzEwNiwiZXhwIjoxNTM3MjM3MDA2LCJhY3IiOiIxIiwiYWlvIjoiQVhRQWkvOElBQUFBRm0rRS9RVEcrZ0ZuVnhMaldkdzhLKzYxQUdyU091TU1GNmViYU1qN1hPM0libUQzZkdtck95RCtOdlp5R24yVmFUL2tES1h3NE1JaHJnR1ZxNkJuOHdMWG9UMUxrSVorRnpRVmtKUFBMUU9WNEtjWHFTbENWUERTL0RpQ0RnRTIyMlRJbU12V05hRU1hVU9Uc0lHdlRRPT0iLCJhbXIiOlsid2lhIl0sImFwcGlkIjoiNzVkYmU3N2YtMTBhMy00ZTU5LTg1ZmQtOGMxMjc1NDRmMTdjIiwiYXBwaWRhY3IiOiIwIiwiZW1haWwiOiJBYmVMaUBtaWNyb3NvZnQuY29tIiwiZmFtaWx5X25hbWUiOiJMaW5jb2xuIiwiZ2l2ZW5fbmFtZSI6IkFiZSAoTVNGVCkiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMjIyNDcvIiwiaXBhZGRyIjoiMjIyLjIyMi4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJvaWQiOiIwMjIyM2I2Yi1hYTFkLTQyZDQtOWVjMC0xYjJiYjkxOTQ0MzgiLCJyaCI6IkkiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJsM19yb0lTUVUyMjJiVUxTOXlpMmswWHBxcE9pTXo1SDNaQUNvMUdlWEEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6ImFiZWxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJGVnNHeFlYSTMwLVR1aWt1dVVvRkFBIiwidmVyIjoiMS4wIn0=.D3H6pMUtQnoJAGq6AHd).

### <a name="v20"></a>v2.0

   `eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiI2ZTc0MTcyYi1iZTU2LTQ4NDMtOWZmNC1lNjZhMzliYjEyZTMiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE1MzcyMzEwNDgsIm5iZiI6MTUzNzIzMTA0OCwiZXhwIjoxNTM3MjM0OTQ4LCJhaW8iOiJBWFFBaS84SUFBQUF0QWFaTG8zQ2hNaWY2S09udHRSQjdlQnE0L0RjY1F6amNKR3hQWXkvQzNqRGFOR3hYZDZ3TklJVkdSZ2hOUm53SjFsT2NBbk5aY2p2a295ckZ4Q3R0djMzMTQwUmlvT0ZKNGJDQ0dWdW9DYWcxdU9UVDIyMjIyZ0h3TFBZUS91Zjc5UVgrMEtJaWpkcm1wNjlSY3R6bVE9PSIsImF6cCI6IjZlNzQxNzJiLWJlNTYtNDg0My05ZmY0LWU2NmEzOWJiMTJlMyIsImF6cGFjciI6IjAiLCJuYW1lIjoiQWJlIExpbmNvbG4iLCJvaWQiOiI2OTAyMjJiZS1mZjFhLTRkNTYtYWJkMS03ZTRmN2QzOGU0NzQiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhYmVsaUBtaWNyb3NvZnQuY29tIiwicmgiOiJJIiwic2NwIjoiYWNjZXNzX2FzX3VzZXIiLCJzdWIiOiJIS1pwZmFIeVdhZGVPb3VZbGl0anJJLUtmZlRtMjIyWDVyclYzeERxZktRIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoiZnFpQnFYTFBqMGVRYTgyUy1JWUZBQSIsInZlciI6IjIuMCJ9.pj4N-w_3Us9DrBLfpCt`

Dit token v2.0 in weergeven [JWT.ms](https://jwt.ms/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiI2ZTc0MTcyYi1iZTU2LTQ4NDMtOWZmNC1lNjZhMzliYjEyZTMiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3L3YyLjAiLCJpYXQiOjE1MzcyMzEwNDgsIm5iZiI6MTUzNzIzMTA0OCwiZXhwIjoxNTM3MjM0OTQ4LCJhaW8iOiJBWFFBaS84SUFBQUF0QWFaTG8zQ2hNaWY2S09udHRSQjdlQnE0L0RjY1F6amNKR3hQWXkvQzNqRGFOR3hYZDZ3TklJVkdSZ2hOUm53SjFsT2NBbk5aY2p2a295ckZ4Q3R0djMzMTQwUmlvT0ZKNGJDQ0dWdW9DYWcxdU9UVDIyMjIyZ0h3TFBZUS91Zjc5UVgrMEtJaWpkcm1wNjlSY3R6bVE9PSIsImF6cCI6IjZlNzQxNzJiLWJlNTYtNDg0My05ZmY0LWU2NmEzOWJiMTJlMyIsImF6cGFjciI6IjAiLCJuYW1lIjoiQWJlIExpbmNvbG4iLCJvaWQiOiI2OTAyMjJiZS1mZjFhLTRkNTYtYWJkMS03ZTRmN2QzOGU0NzQiLCJwcmVmZXJyZWRfdXNlcm5hbWUiOiJhYmVsaUBtaWNyb3NvZnQuY29tIiwicmgiOiJJIiwic2NwIjoiYWNjZXNzX2FzX3VzZXIiLCJzdWIiOiJIS1pwZmFIeVdhZGVPb3VZbGl0anJJLUtmZlRtMjIyWDVyclYzeERxZktRIiwidGlkIjoiNzJmOTg4YmYtODZmMS00MWFmLTkxYWItMmQ3Y2QwMTFkYjQ3IiwidXRpIjoiZnFpQnFYTFBqMGVRYTgyUy1JWUZBQSIsInZlciI6IjIuMCJ9.pj4N-w_3Us9DrBLfpCt).

## <a name="claims-in-access-tokens"></a>Claims in toegangstokens

JWTs worden onderverdeeld in drie onderdelen: 

* **Koptekst** -vindt u informatie over hoe u [valideren van het token](#validating-tokens) , waaronder informatie over het type token en hoe het is ondertekend.
* **Nettolading** -bevat alle belangrijke gegevens over de gebruiker of de app die u probeert om aan te roepen uw service.
* **Handtekening** -grondstoffen gebruikt om te valideren van het token Is.

Elk wordt gescheiden door een punt (`.`) en afzonderlijk Base64-gecodeerd.

Claims zijn alleen aanwezig als een waarde bestaat om het te vullen. Uw app moet een afhankelijkheid dus niet uitvoeren op een claim aanwezig. Voorbeelden zijn onder meer `pwd_exp` (niet elke tenant vereist dat wachtwoorden verlopen) of `family_name` ([clientreferenties](v1-oauth2-client-creds-grant-flow.md) stromen namens toepassingen waarvoor namen zijn). Claims die worden gebruikt voor validatie van het toegangstoken wordt altijd aanwezig zijn.

> [!NOTE]
> Sommige claims worden gebruikt voor de Azure AD beveiligde tokens in het geval van hergebruik. Deze zijn gemarkeerd als niet wordt voor openbaar gebruik in de beschrijving als 'Ondoorzichtige'. Deze claims kunnen of kunnen niet worden weergegeven in een token, en nieuwe waarden kunnen worden toegevoegd zonder voorafgaande kennisgeving.

### <a name="header-claims"></a>Header-claims

|Claim | Indeling | Beschrijving |
|--------|--------|-------------|
| `typ` | Tekenreeks - altijd "JWT" | Geeft aan dat het token een JWT.|
| `nonce` | Reeks | Een unieke id gebruikt om te beveiligen tegen token opnieuw afspelen aanvallen. Je kunt deze waarde om te beveiligen tegen replays bronrecord. |
| `alg` | Reeks | Geeft aan dat de algoritme die is gebruikt voor het ondertekenen van het token, bijvoorbeeld "RS256" |
| `kid` | Reeks | Hiermee geeft u de vingerafdruk voor de openbare sleutel die wordt gebruikt voor het ondertekenen van dit token. Verzonden in v1.0 zowel versie 2.0-toegangstokens. |
| `x5t` | Reeks | (In gebruik en de waarde) dezelfde functies als `kid`. Dit is een verouderde claim verzonden alleen in v1.0 toegangstokens voor compatibiliteit van toepassing. |

### <a name="payload-claims"></a>De nettolading van claims

| Claim | Indeling | Beschrijving |
|-----|--------|-------------|
| `aud` | Tekenreeks, een URI van de App-ID | Hiermee geeft u de beoogde ontvanger van het token. In de toegangstokens te geven is de doelgroep van uw app toepassings-ID, toegewezen aan uw app in Azure portal. Uw app moet deze waarde te valideren en het token te negeren als de waarde komt niet overeen met. |
| `iss` | Tekenreeks, een STS-URI | Identificeert de beveiligingstokenservice (STS) die wordt gemaakt en retourneert het token en de Azure AD-tenant waarin de gebruiker is geverifieerd. Als het token is uitgegeven door het v2.0-eindpunt, de URI wordt beëindigd `/v2.0`. De GUID die wordt aangegeven dat de gebruiker een consument gebruiker vanuit een Microsoft-account is `9188040d-6c67-4c5b-b112-36a304b66dad`. Uw app moet de GUID-gedeelte van de claim gebruiken om het beperken van de set van tenants die kunnen zich aanmelden bij de app, indien van toepassing. |
| `iat` | int, een UNIX-timestamp | "Verleend aan" geeft aan wanneer de verificatie voor dit token is opgetreden. |
| `nbf` | int, een UNIX-timestamp | De claim 'nbf"(niet voor) geeft de tijd waarbinnen de JWT moet niet worden geaccepteerd voor verwerking. |
| `exp` | int, een UNIX-timestamp | De claim 'exp' (verlooptijd) identificeert de verlooptijd op of na die de JWT moet niet worden geaccepteerd voor verwerking. Het is belangrijk te weten dat een resource het token voordat deze tijd, afwijzen kan zoals wanneer een wijziging in de verificatie vereist is of intrekken van een token is gedetecteerd. |
| `acr` | Tekenreeks, een '0' of '1' | De claim "Authentication context class". Een waarde van '0' geeft aan dat de verificatie van de eindgebruiker niet voldoet aan de vereisten van ISO/IEC 29115. |
| `aio` | Ondoorzichtige tekenreeks | Een interne claim die worden gebruikt door Azure AD om gegevens te noteren voor hergebruik van token. Resources moeten deze claim niet gebruiken. |
| `amr` | JSON-matrix met tekenreeksen | Alleen aanwezig in v1.0 tokens. Hiermee geeft u op hoe de onderwerpnaam van het token is geverifieerd. Zie [de amr sectie claimen](#the-amr-claim) voor meer informatie. |
| `appid` | Tekenreeks, een GUID | Alleen aanwezig in v1.0 tokens. De toepassings-ID van de client met behulp van het token. De toepassing kan fungeren als zelf of namens een gebruiker. De toepassings-ID vertegenwoordigt doorgaans een toepassingsobject, maar het kan ook een service-principal-object vertegenwoordigen in Azure AD. |
| `appidacr` | "0", "1" of "2" | Alleen aanwezig in v1.0 tokens. Geeft aan hoe de client is geverifieerd. De waarde is voor een openbare client "0". Als de client-ID en het clientgeheim worden gebruikt, is de waarde "1". Als u een clientcertificaat is gebruikt voor verificatie, is de waarde "2". |
| `azp` | Tekenreeks, een GUID | Alleen aanwezig in v2.0-tokens. De toepassings-ID van de client met behulp van het token. De toepassing kan fungeren als zelf of namens een gebruiker. De toepassings-ID vertegenwoordigt doorgaans een toepassingsobject, maar het kan ook een service-principal-object vertegenwoordigen in Azure AD. |
| `azpacr` | "0", "1" of "2" | Alleen aanwezig in v2.0-tokens. Geeft aan hoe de client is geverifieerd. De waarde is voor een openbare client "0". Als de client-ID en het clientgeheim worden gebruikt, is de waarde "1". Als u een clientcertificaat is gebruikt voor verificatie, is de waarde "2". |
| `groups` | JSON-matrix van GUID 's | Object-id's die staan voor groepslidmaatschappen van de certificaathouder biedt. Deze waarden zijn unieke (Zie de Object-ID) en veilig kunnen worden gebruikt voor het beheren van toegang, zoals het afdwingen van machtiging voor toegang tot een resource. De groepen die zijn opgenomen in de claim van groepen op basis van per toepassing worden geconfigureerd via de `groupMembershipClaims` eigenschap van de [toepassingsmanifest](reference-app-manifest.md). Een null-waarde wordt alle groepen uitsluit, een waarde van 'Toewijzingsmodule' bevat alleen lidmaatschappen voor Active Directory-beveiligingsgroepen, en de waarde 'Alle' wordt opgenomen beveiligingsgroepen en distributielijsten van Office 365. <br><br>Zie de `hasgroups` claim hieronder voor meer informatie over het gebruik van de `groups` claim met de impliciete toekenning. <br>Voor andere stromen als het nummer van de gebruiker zich in groepen gaat via een limiet (150 voor SAML, 200 voor JWT), klikt u vervolgens een overschrijding claim toegevoegd aan de claim bronnen aan te wijzen aan de Graph-eindpunt met de lijst met groepen voor de gebruiker. |
| `hasgroups` | Booleaans | Indien aanwezig, altijd `true`, die aangeeft van de gebruiker is in ten minste één groep. Gebruikt in plaats van de `groups` claim voor JWTs in impliciete goedkeuring voor stromen als de volledige groepen claim gelden de URI-fragment buiten de URL-lengte-limieten (momenteel abonnement van 6 of meer groepen). Geeft aan dat de client de grafiek gebruiken moet om te bepalen van de gebruiker groepen (`https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects`). |
| `groups:src1` | JSON-object | Token aanvragen die niet beperkt de lengte zijn (Zie `hasgroups` hierboven), maar nog steeds te groot is voor het token, een koppeling naar de volledige lijst van de gebruiker worden opgenomen. Voor JWTs als een gedistribueerde claim, voor SAML als een nieuwe claim in plaats van de `groups` claim. <br><br>**Voorbeeldwaarde JWT**: <br> `"groups":"src1"` <br> `"_claim_sources`: `"src1" : { "endpoint" : "https://graph.windows.net/{tenantID}/users/{userID}/getMemberObjects" }`|
| `preferred_name` | Reeks | Alleen aanwezig in v2.0-tokens. De primaire gebruikersnaam die de gebruiker aangeeft. Het is mogelijk een e-mailadres, telefoonnummer of een algemene gebruikersnaam zonder een indeling die is opgegeven. De waarde ervan is veranderlijke en na verloop van tijd veranderen. Omdat dit veranderlijke, moet deze waarde niet worden gebruikt om autorisatie beslissingen te nemen. De `profile` bereik is vereist voor het ontvangen van deze claim. |
| `name` | Reeks | Biedt een leesbare waarde die aangeeft in het onderwerp van het token. De waarde kan niet worden gegarandeerd uniek te zijn, is het veranderlijke en is ontworpen om alleen worden gebruikt voor weer te geven. De `profile` bereik is vereist voor het ontvangen van deze claim. |
| `oid` | Tekenreeks, een GUID | De onveranderbare id voor een object in de Microsoft identity-platform, in dit geval een gebruikersaccount. Het kan ook worden gebruikt voor het uitvoeren van controles autorisatie veilig en als een sleutel in databasetabellen. Deze ID is uniek voor de gebruiker voor toepassingen: twee verschillende toepassingen die zich in dezelfde gebruiker ontvangt de dezelfde waarde in de `oid` claim. Dus `oid` kunnen worden gebruikt bij het maken van query's voor Microsoft online services, zoals de Microsoft Graph. De Microsoft Graph retourneert deze ID als de `id` eigenschap voor een bepaalde gebruikersaccount. Omdat de `oid` kunnen meerdere apps correleren van gebruikers, de `profile` bereik is vereist voor het ontvangen van deze claim. Houd er rekening mee dat als een enkele gebruiker in meerdere tenants bestaat, de gebruiker een ander object-ID in elke tenant bevat: deze worden beschouwd als andere accounts, zelfs als de gebruiker meldt zich aan bij elk account met dezelfde referenties. |
| `rh` | Ondoorzichtige tekenreeks | Een interne claim die door Azure gebruikt voor het valideren van tokens. Resources moeten deze claim niet gebruiken. |
| `scp` | Tekenreeks, een spatie gescheiden lijst met scopes | De set met scopes die worden weergegeven door uw toepassing waarvoor de clienttoepassing heeft aangevraagd (en ontvangen) toestemming geven. Uw app moet controleren of deze bereiken geldige die door uw app beschikbaar gesteld zijn en autorisatiebeslissingen te op basis van de waarde van deze bereiken nemen. Alleen opgenomen voor [gebruikerstokens](#user-and-application-tokens). |
| `roles`| Tekenreeks, een spatie gescheiden lijst met machtigingen | De set met machtigingen die worden weergegeven door uw toepassing die de aanvragende toepassing heeft zijn gemachtigd om aan te roepen. Dit wordt gebruikt tijdens de [-clientreferenties](v1-oauth2-client-creds-grant-flow.md) flow in plaats van de gebruiker bereiken en is alleen aanwezig in [toepassingen tokens](#user-and-application-tokens). |
| `sub` | Tekenreeks, een GUID | De principal waarover het token worden bevestigd met gegevens, zoals de gebruiker van een app. Deze waarde is onveranderbaar en kan niet worden toegewezen of opnieuw gebruikt. Deze controles uit te voeren autorisatie veilig, zoals wanneer het token wordt gebruikt voor toegang tot een bron kan worden gebruikt en kan worden gebruikt als een sleutel in databasetabellen. Omdat het onderwerp altijd is aanwezig zijn in de tokens dat problemen met Azure AD, wordt u aangeraden deze waarde in een autorisatiesysteem voor algemeen gebruik. Het onderwerp is echter een pairwise id - het is uniek is voor een bepaalde toepassing-ID. Als een enkele gebruiker zich in twee verschillende apps met behulp van de twee andere client-id's, krijgen die apps dus twee verschillende waarden voor de claim onderwerp. Dit kan wel of niet kan worden gewenst is afhankelijk van uw architectuur en privacy-vereisten. |
| `tid` | Tekenreeks, een GUID | Hiermee geeft u de Azure AD-tenant die door de gebruiker uit. De GUID is voor werk- en schoolaccounts accounts, de onveranderbare tenant-ID van de organisatie die de gebruiker behoort. De waarde voor persoonlijke accounts heeft `9188040d-6c67-4c5b-b112-36a304b66dad`. De `profile` bereik is vereist voor het ontvangen van deze claim. |
| `unique_name` | Reeks | Alleen aanwezig in v1.0 tokens. Biedt een mens leesbaar waarde die het onderwerp van het token identificeert. Deze waarde is niet noodzakelijkerwijs uniek zijn binnen een tenant en mag alleen worden gebruikt voor weer te geven. |
| `upn` | Reeks | De gebruikersnaam van de gebruiker. Mogelijk een telefoonnummer, e-mailadres of niet-opgemaakte tekenreeks. Moet alleen worden gebruikt voor het weer te geven en bieden gebruikersnaam hints in scenario's voor verificatie. |
| `uti` | Ondoorzichtige tekenreeks | Een interne claim die door Azure gebruikt voor het valideren van tokens. Resources moeten deze claim niet gebruiken. |
| `ver` | Tekenreeks, 1.0 of 2.0 | Geeft de versie van het toegangstoken. |

#### <a name="v10-basic-claims"></a>basic claims V1.0

De volgende claims worden opgenomen in v1.0 tokens indien van toepassing, maar worden niet standaard opgenomen in v2.0-tokens. Als u versie 2.0 en moet u een van deze claims worden gebruikt, deze aanvragen met behulp van [optionele claims](active-directory-optional-claims.md).

| Claim | Indeling | Beschrijving |
|-----|--------|-------------|
| `ipaddr`| Reeks | De gebruiker is geverifieerd vanaf het IP-adres. |
| `onprem_sid`| Tekenreeks, in [SID-indeling](https://docs.microsoft.com/windows/desktop/SecAuthZ/sid-components) | In gevallen waar de gebruiker een on-premises-verificatie heeft, biedt deze claim hun SID. Dit kan worden gebruikt voor autorisatie voor oudere toepassingen. |
| `pwd_exp`| int, een UNIX-timestamp | Geeft aan wanneer het wachtwoord van de gebruiker is verlopen. |
| `pwd_url`| Reeks | Een URL waar gebruikers hun wachtwoord opnieuw in te kunnen worden verzonden. |
| `in_corp`|booleaans | Signalen als de client is aangemeld vanuit het bedrijfsnetwerk bevinden. Als dat niet het geval is, is de claim niet opgenomen. |
| `nickname`| Reeks | Een andere naam voor de gebruiker te scheiden van de naam van eerste of laatste.|
| `family_name` | Reeks | Bevat de laatste naam, de achternaam of familienaam van de gebruiker gedefinieerd op het gebruikersobject. |
| `given_name` | Reeks | Biedt de eerste of de opgegeven naam van de gebruiker, zoals ingesteld op het gebruikersobject. |

#### <a name="the-amr-claim"></a>De `amr` claim

Microsoft-identiteiten kunnen verifiëren in tal van manieren, die mogelijk relevant zijn voor uw toepassing. De `amr` claim is een matrix met meerdere items zoals bevatten kan `["mfa", "rsa", "pwd"]`, voor een verificatie met een wachtwoord en de Authenticator-app gebruikt. 

| Waarde | Beschrijving |
|-----|-------------|
| `pwd` | Wachtwoordverificatie, Microsoft-wachtwoord van een gebruiker of het clientgeheim van een app. |
| `rsa` | Verificatie is op basis van de POC-fase van een RSA-sleutel, bijvoorbeeld met de [Microsoft Authenticator pp](https://aka.ms/AA2kvvu). Dit omvat als verificatie is uitgevoerd door een zelf-ondertekend JWT met een service die eigendom zijn X509 certificaat. |
| `otp` | Eenmalige wachtwoordcode met behulp van een e-mailbericht of een SMS-bericht. |
| `fed` | Een verklaring van federatieve verificatie (zoals JWT of SAML) is gebruikt. |
| `wia` | Geïntegreerde Windows-verificatie |
| `mfa` | Meervoudige verificatie is gebruikt. Als dat aanwezig is zijn de andere verificatiemethoden ook worden opgenomen. |
| `ngcmfa` | Gelijk aan `mfa`, die wordt gebruikt voor het inrichten van een bepaald type geavanceerde referentie. |
| `wiaormfa`| De gebruiker gebruikt Windows of een MFA-referentie om te verifiëren. |
| `none` | Er is geen verificatie uitgevoerd. |

## <a name="validating-tokens"></a>Valideren van tokens

Voor het valideren van een id_token of een access_token, moet het valideren van uw app in zowel de handtekening van het token en de claims. Om te valideren toegangstokens, moet de uitgever, de doelgroep en het ondertekenen tokens ook valideren door uw app. Deze moeten worden gevalideerd op basis van de waarden in de OpenID-discovery-document. Bijvoorbeeld, de tenant onafhankelijk versie van het document bevindt zich op [ https://login.microsoftonline.com/common/.well-known/openid-configuration ](https://login.microsoftonline.com/common/.well-known/openid-configuration). 

De Azure AD-middleware beschikt over ingebouwde mogelijkheden voor het valideren van tokens voor toegang en vindt u op onze [voorbeelden](https://docs.microsoft.com/azure/active-directory/active-directory-code-samples) te zoeken in de taal van uw keuze. Zie voor meer informatie over hoe u expliciet een JWT-token te valideren, de [handmatige JWT Validatievoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-webapi-manual-jwt-validation). 

We bieden bibliotheken en codevoorbeelden die laten zien hoe eenvoudig omgaan met validatie van tokens. De onderstaande informatie is opgegeven voor gebruikers die u wilt het onderliggende proces begrijpt. Er zijn ook verschillende externe open source-bibliotheken beschikbaar voor validatie van JWT - er is ten minste één optie voor bijna elk platform of elke taal beschikbaar. Zie voor meer informatie over Azure AD-verificatiebibliotheken en codevoorbeelden [v1.0-verificatiebibliotheken](active-directory-authentication-libraries.md) en [v2.0-verificatiebibliotheken](reference-v2-libraries.md).

### <a name="validating-the-signature"></a>Valideren van de handtekening

Een JWT bevat drie segmenten die worden gescheiden door het `.` teken. Het eerste segment wordt ook wel de **header**, het tweede als de **hoofdtekst**, en de derde als de **handtekening**. Het segment handtekening kan worden gebruikt om te valideren de echtheid van het token, zodat deze kan worden vertrouwd door uw app.

Tokens die zijn uitgegeven door Azure AD zijn ondertekend met behulp van de bedrijfstak standard asymmetrische coderingsalgoritmen, zoals RSA-256. De koptekst van het JWT bevat informatie over de sleutel en -versleuteling-methode gebruikt voor het ondertekenen van het token:

```json
{
  "typ": "JWT",
  "alg": "RS256",
  "x5t": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk",
  "kid": "iBjL1Rcqzhiy4fpxIxdZqohM2Yk"
}
```

De `alg` claim geeft aan dat de algoritme die is gebruikt voor het ondertekenen van de token, terwijl de `kid` claim geeft aan dat de bepaalde openbare sleutel die is gebruikt voor het ondertekenen van het token.

Azure AD kan een id_token met behulp van één van een bepaalde set van paren van openbare en persoonlijke sleutels zich op een willekeurig moment in-time. Azure AD draait het mogelijke set sleutels op periodieke basis, zodat uw app voor een correcte verwerking die automatisch moet worden geschreven. Een redelijke frequentie om te controleren op updates voor de openbare sleutels die worden gebruikt door Azure AD is elke 24 uur.

U kunt de ondertekening belangrijke gegevens die nodig zijn voor het valideren van de handtekening met behulp van de OpenID Connect-metagegevensdocument dat zich bevindt in verkrijgen:

```
https://login.microsoftonline.com/common/.well-known/openid-configuration
```

> [!TIP]
> Probeer deze URL in een browser.

Dit metagegevensdocument:

* Is een JSON-object bevat nuttige stukjes informatie, zoals de locatie van de verschillende eindpunten vereist voor het uitvoeren van de OpenID Connect-verificatie. 
* Bevat een `jwks_uri`, waardoor de locatie van de set met openbare sleutels die worden gebruikt voor het ondertekenen van tokens. Het JSON-document dat zich bevindt in de `jwks_uri` bevat alle gegevens van de openbare sleutel wordt gebruikt op een bepaald tijdstip. Uw app kan gebruiken de `kid` claim in de JWT-header te selecteren welke openbare sleutel in dit document is gebruikt voor het ondertekenen van een bepaalde token. Validatie van de handtekening met behulp van de juiste openbare sleutel en het opgegeven algoritme kan vervolgens uitvoeren.

> [!NOTE]
> Het eindpunt v1.0 retourneert zowel de `x5t` en `kid` claims. De `x5t` claim ontbreekt in v2.0-tokens. Het v2.0-eindpunt reageert met de `kid` claim. Voortaan, wordt u aangeraden de `kid` claim uw token te valideren.

Het valideren van de handtekening is buiten het bereik van dit document: Er zijn veel open-source-bibliotheken beschikbaar voor u doen indien nodig te helpen.

### <a name="claims-based-authorization"></a>Autorisatie op basis van claims

Deze stap worden bepaald door de bedrijfslogica van uw toepassing, hieronder enkele veelgebruikte autorisatiemethoden worden behandeld.

* Controleer de `scp` of `roles` claimen om te controleren of alle aanwezig bereiken overeenkomen met de door uw API beschikbaar gesteld en dat de client de gevraagde actie uit te voeren.
* Zorg ervoor dat de aanroepende client is toegestaan om aan te roepen uw API met de `appid` claim.
* Valideren van de status van de verificatie van de aanvragende client met behulp van `appidacr` -deze mag niet 0 zijn als openbare clients niet mogen uw API aan te roepen.
* Controleren op basis van een lijst met verleden `nonce` claims om te controleren of het token is niet opnieuw worden afgespeeld.
* Controleer of de `tid` komt overeen met een tenant die is toegestaan voor het aanroepen van uw API.
* Gebruik de `acr` claim om te controleren of de gebruiker MFA is uitgevoerd. Houd er rekening mee dat deze moet worden afgedwongen met behulp van [voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/conditional-access/overview).
* Als u hebt aangevraagd de `roles` of `groups` claims in het toegangstoken controleren of de gebruiker in de groep mag deze actie uit te voeren.
  * Voor tokens opgehaald met behulp van de impliciete stroom, waarschijnlijk moet u om op te vragen de [Graph](https://developer.microsoft.com/graph/) voor deze gegevens, zoals deze is vaak te groot voor in het token. 

## <a name="user-and-application-tokens"></a>Gebruikers en toepassingen-tokens

Uw toepassing tokens namens een gebruiker (de gebruikelijke stroom) of rechtstreeks vanuit een toepassing kan ontvangen (via de [clientreferenties die stroom](v1-oauth2-client-creds-grant-flow.md)). Deze app alleen-lezen-tokens erop wijzen dat deze aanroep afkomstig is van een toepassing heeft geen back-te maken van een gebruiker. Deze tokens worden afgehandeld grotendeels hetzelfde, met enkele verschillen:

* Alleen App-tokens heeft geen een `scp` claimen en heeft in plaats daarvan een `roles` claim. Dit is waar de machtiging van de toepassing (in plaats van gedelegeerde machtigingen) wordt geregistreerd. Zie voor meer informatie over machtigingen voor gedelegeerde en toepassingen, machtigingen en toestemming [v1.0](v1-permissions-and-consent.md) en [v2.0](v2-permissions-and-consent.md).
* Veel mensen-specifieke claims worden ontbreken, zoals `name`.

## <a name="token-revocation"></a>Intrekken van token

Vernieuwen van tokens kunnen worden ongeldig gemaakt of ingetrokken op elk gewenst moment, voor tal van redenen. Deze kunnen worden onderverdeeld in twee hoofdcategorieën: time-outs en intrekkingen.

### <a name="token-timeouts"></a>Token time-outs

* MaxInactiveTime: Als het vernieuwingstoken dat niet binnen de tijd die wordt bepaald door de MaxInactiveTime gebruikt is, Token vernieuwen wordt niet langer geldig. 
* MaxSessionAge: Als MaxAgeSessionMultiFactor of MaxAgeSessionSingleFactor zijn ingesteld op iets anders dan de standaardwaarde (tot en met ingetrokken), klikt u vervolgens herauthenticatie is vereist wanneer de tijd instellen in de MaxAgeSession * is verstreken. 
* Voorbeelden:
  * De tenant heeft een MaxInactiveTime van 5 dagen en de gebruiker voor een van de week op vakantie ging en dus AAD is niet zichtbaar voor een nieuwe tokenaanvraag van de gebruiker in de zeven dagen. De volgende keer dat de gebruiker een nieuw token aanvraagt ze, vindt hun Refresh Token is ingetrokken en moeten ze hun referenties opnieuw invoeren.
  * Gevoelige toepassingen heeft een MaxAgeSessionSingleFactor van 1 dag. Als een gebruiker zich aanmeldt op maandag en op dinsdag (nadat de 25 uur zijn verstreken), ze moeten verifiëren.

### <a name="revocation"></a>Intrekken

|   | Wachtwoord op basis van cookies | Wachtwoord op basis van token | Niet-wachtwoord op basis van cookies | Niet-wachtwoord op basis van token | Vertrouwelijke client-token| 
|---|-----------------------|----------------------|---------------------------|--------------------------|--------------------------|
| Wachtwoord verloopt | Blijft actief| Blijft actief | Blijft actief | Blijft actief | Blijft actief |
| Wachtwoord gewijzigd door gebruiker | Ingetrokken | Ingetrokken | Blijft actief | Blijft actief | Blijft actief |
| Gebruiker heeft SSPR | Ingetrokken | Ingetrokken | Blijft actief | Blijft actief | Blijft actief |
| Beheerderswachtwoord opnieuw instellen van wachtwoorden | Ingetrokken | Ingetrokken | Blijft actief | Blijft actief | Blijft actief |
| Gebruiker trekt u hun vernieuwingstokens [via PowerShell](https://docs.microsoft.com/powershell/module/azuread/revoke-azureadsignedinuserallrefreshtoken) | Ingetrokken | Ingetrokken |Ingetrokken | Ingetrokken |Ingetrokken | Ingetrokken |
| Beheerder alle vernieuwingstokens ingetrokken voor de tenant [via PowerShell](https://docs.microsoft.com/powershell/module/azuread/revoke-azureaduserallrefreshtoken) | Ingetrokken | Ingetrokken |Ingetrokken | Ingetrokken |Ingetrokken | Ingetrokken |
| [Eenmalige afmelding](v1-protocols-openid-connect-code.md#single-sign-out) op het web | Ingetrokken | Blijft actief | Ingetrokken | Blijft actief | Blijft actief |

> [!NOTE]
> Een aanmelding 'niet-wachtwoord op basis van' is een waarin de gebruiker een wachtwoord om dit probleem te niet opgeven. Bijvoorbeeld, met behulp van de face met Windows Hello, een FIDO-sleutel of een PINCODE. 
>
> Er bestaat een bekend probleem met het Windows primaire vernieuwen-Token. Als de PRT wordt verkregen via een wachtwoord, en vervolgens de gebruiker zich via Hallo aanmeldt, de herkomst van de PRT worden niet gewijzigd en zal worden ingetrokken als de gebruiker het wachtwoord wijzigt.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [ `id_tokens` in Azure AD](id-tokens.md).
* Meer informatie over de machtigingen en toestemming, via [v1.0](v1-permissions-and-consent.md) en [v2.0](v2-permissions-and-consent.md).
