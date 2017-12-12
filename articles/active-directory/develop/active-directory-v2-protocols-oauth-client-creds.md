---
title: Azure AD v2.0 gebruiken voor toegang tot beveiligde bronnen zonder gebruikersinteractie | Microsoft Docs
description: Webtoepassingen bouwen met behulp van de Azure AD-implementatie van het protocol OAuth 2.0-verificatie.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 9b7cfbd7-f89f-4e33-aff2-414edd584b07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 28616657c5aae4f6ada1ec592a2a6287e8607b6a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-v20-and-the-oauth-20-client-credentials-flow"></a>Azure Active Directory-v2.0 en de clientreferentiestroom van OAuth 2.0
U kunt de [OAuth 2.0-clientreferenties verlenen](http://tools.ietf.org/html/rfc6749#section-4.4), soms ook wel *tweearmige OAuth*, toegang krijgen tot bronnen web gehost met behulp van de identiteit van een toepassing. Dit type grant vaak wordt gebruikt voor de server naar server interacties dat moeten worden uitgevoerd op de achtergrond, zonder directe interactie met een gebruiker. Deze typen toepassingen vaak worden aangeduid als *daemons* of *-serviceaccounts*.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Meer informatie over om te bepalen of het v2.0-eindpunt moet worden gebruikt, [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

In de typische *OAuth driearmige*, een clienttoepassing, is gemachtigd voor toegang tot een bron namens een specifieke gebruiker. De machtiging is overgedragen van de gebruiker aan de toepassing, meestal tijdens de [toestemming](active-directory-v2-scopes.md) proces. In de clientreferentiestroom de machtigingen worden verleend rechtstreeks naar de toepassing zelf. Wanneer de app geeft die een token voor een resource, de bron wordt afgedwongen dat de app zelf autorisatie heeft voor het uitvoeren van een actie en niet dat heeft de gebruiker toestemming.

## <a name="protocol-diagram"></a>Protocol-diagram
De volledige clientreferentiestroom lijkt op het volgende diagram. Elk van de stappen verderop in dit artikel worden beschreven.

![Clientreferentiestroom](../../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Directe autorisatie ophalen
Een app ontvangt doorgaans direct autorisatie voor toegang tot een bron op twee manieren: via een toegangsbeheerlijst (ACL) op de bron of toepassing machtigingen worden toegewezen in Azure Active Directory (Azure AD). Deze twee methoden zijn de meest voorkomende in Azure AD en het is raadzaam deze voor clients en resources waarmee de client referentiestroom. Een bron kunt voor het autoriseren van de clients op andere manieren echter kiezen. Elke resource-server kan de methode die het meest zinvol voor de toepassing kiezen.

### <a name="access-control-lists"></a>Toegangsbeheerlijsten
Een resourceprovider mogelijk een autorisatie-controle op basis van een lijst met toepassings-id's die deze kent en een specifiek niveau van toegang tot verleent afdwingen. Wanneer de bron een token van het v2.0-eindpunt ontvangt, kan decoderen van het token en uitpakken van de client toepassings-ID van de `appid` en `iss` claims. Vergelijkt vervolgens de toepassing op basis van een ACL die wordt bijgehouden. De granulatie de ACL's en de methode kunnen aanzienlijk variëren tussen resources.

Een algemene gebruiksvoorbeeld is het gebruik van een ACL testen voor een webtoepassing of voor een Web-API. De Web-API kan alleen een subset van de volledige machtigingen verlenen aan een specifieke client. End-to-end-tests uitgevoerd op de API, een testclient die tokens van het v2.0-eindpunt verkrijgt en zendt deze naar de API te maken. De API controleert vervolgens de ACL voor de toepassings-ID van de testclient voor volledige toegang tot de volledige functionaliteit van de API. Als u dit soort ACL gebruikt, moet u niet alleen de aanroeper valideren `appid` waarde. Ook valideren dat de `iss` waarde van het token wordt vertrouwd.

Dit type verificatie is gebruikelijk daemons en serviceaccounts voor groepen die toegang moeten krijgen tot gegevens die eigendom zijn van de consumer-gebruikers die persoonlijke Microsoft-accounts hebben. Voor gegevens die eigendom zijn van organisaties, is het raadzaam dat u de benodigde machtiging via machtigingen voor een toepassing krijgt.

### <a name="application-permissions"></a>Toepassingsmachtigingen
U kunt in plaats van ACL's, API's gebruiken om een reeks Toepassingsmachtigingen weer te geven. Een toepassing toestemming te krijgen tot een toepassing door de beheerder van een organisatie en kan alleen worden gebruikt voor toegang tot gegevens die eigendom zijn van die organisatie en werknemers. Bijvoorbeeld: Microsoft Graph beschrijft de verschillende Toepassingsmachtigingen het volgende doen:

* E-mail in alle postvakken lezen
* E-mail in alle postvakken lezen en schrijven
* E-mail met elke willekeurige gebruiker als afzender verzenden
* Adreslijstgegevens lezen

Voor meer informatie over de machtigingen van een toepassing, gaat u naar [Microsoft Graph](https://graph.microsoft.io).

Voor het gebruik van machtigingen voor een toepassing in uw app, moet u in de volgende secties we bespreken stappen uitvoeren.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in de app-portal voor registratie van aanvragen
1. Ga naar uw toepassing in de [toepassing Registratieportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of [maken van een app](active-directory-v2-app-registration.md), als u dat nog niet gedaan hebt. U moet ten minste één Toepassingsgeheim gebruiken bij het maken van uw app.
2. Zoek de **Direct Toepassingsmachtigingen** sectie en voeg vervolgens de machtigingen die vereist dat uw app.
3. **Sla** de registratie van de app.

#### <a name="recommended-sign-the-user-in-to-your-app"></a>Aanbevolen: De gebruiker zich aanmeldt bij uw app
Wanneer u een toepassing die gebruikmaakt van machtigingen voor een toepassing maakt, is de app doorgaans, een pagina of weergave waarin de beheerder van de app-machtigingen goedkeurt. Deze pagina kan deel uitmaken van de app aanmelden stroom, onderdeel van de instellingen van de app, of een specifieke stroom van 'verbinding'. In veel gevallen is het zinvol voor de app weer te geven 'verbinding' weergave pas nadat een gebruiker is aangemeld met een werk- of school Microsoft-account.

Als u de gebruiker zich aanmeldt bij uw app, kunt u de organisatie die de gebruiker behoort, voordat u vraagt de gebruiker om goed te keuren van de toepassing worden machtigingen kunt identificeren. Hoewel niet strikt noodzakelijk is, kunt u u bij het maken van een intuïtieve ervaring voor uw gebruikers. De gebruiker in ondertekenen, volgt u onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>De machtigingen aanvragen bij een directory-beheerder
Wanneer u klaar bent om machtigingen van de beheerder van de organisatie, kunt u de gebruiker omgeleid naar het v2.0 *toestemming beheereindpunt*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the following request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Voorwaarde | Beschrijving |
| --- | --- | --- |
| Tenant |Vereist |De directory-tenant die u wilt machtiging van aanvragen. Dit kan zijn in de beschrijvende naam van de indeling of GUID. Als u niet welk tenant de gebruiker behoort en u laten aanmelden met een tenant weet wilt, gebruikt u `common`. |
| client_id |Vereist |De aanvraag-ID die de [Registratieportal toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| redirect_uri |Vereist |De omleidings-URI waar u het antwoord moet worden verzonden voor uw app om af te handelen. Moet exact overeenkomen met een van de omleidings-URI's die u in de portal hebt geregistreerd, behalve dat het moet een URL zijn gecodeerd en kunnen extra padsegmenten hebben. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks van inhoud die u wilt dat kan zijn. De status wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |

Azure AD wordt op dit punt wordt afgedwongen dat alleen een tenantbeheerder aanmelden kan bij de aanvraag niet voltooien. De beheerder wordt gevraagd om goed te keuren alle machtigingen van rechtstreekse toepassing die u hebt aangevraagd voor uw app in de app-portal voor wachtwoordregistratie.

##### <a name="successful-response"></a>Geslaagde reactie
Als de beheerder heeft de machtigingen voor uw toepassing goedgekeurd, is geslaagd antwoord ziet er als volgt:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschrijving |
| --- | --- | --- |
| Tenant |De directory-tenant die uw toepassing, de machtigingen die zij gevraagd in GUID-indeling. |
| state |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord wordt geretourneerd. Een tekenreeks van inhoud die u wilt dat kan zijn. De status wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |
| admin_consent |Ingesteld op **true**. |

##### <a name="error-response"></a>Foutbericht
Als de beheerder worden niet goedgekeurd voor de machtigingen voor uw toepassing, uitziet de mislukte reactie:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschrijving |
| --- | --- | --- |
| error |Een code-tekenreeks voor fout die u gebruiken kunt voor het classificeren van typen fouten, en dat u kunt gebruiken om te reageren op fouten. |
| error_description |Een specifiek foutbericht waarmee u kunt identificeren de hoofdoorzaak van een fout. |

Nadat u een geslaagde reactie van het eindpunt van app-inrichting ontvangen hebt, hebben de rechtstreekse toepassing worden machtigingen aangevraagde opgedaan met uw app. U kunt nu een token voor de resource die u wilt aanvragen.

## <a name="get-a-token"></a>Een token ophalen
Nadat u de benodigde machtiging hebt voor uw toepassing hebt verkregen, doorgaan met het ophalen van de toegangstokens voor API's. Voor een token met behulp van de client referenties ken stuurt u een POST-aanvraag naar de `/token` v2.0-eindpunt:

### <a name="first-case-access-token-request-with-a-shared-secret"></a>Het eerste aanvraagnummer: aanvraag voor toegang tot token met een gedeeld geheim

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameter | Voorwaarde | Beschrijving |
| --- | --- | --- |
| client_id |Vereist |De aanvraag-ID die de [Registratieportal toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| Bereik |Vereist |De doorgegeven waarde voor de `scope` parameter in deze aanvraag moet de resource identifier (URI toepassing-ID) van de resource die u wilt gebruiken, aangebracht met de `.default` achtervoegsel. De waarde voor de voorbeeld Microsoft Graph heeft `https://graph.microsoft.com/.default`. Deze waarde informeert het v2.0-eindpunt dat alle rechtstreekse toepassing machtigingen die u voor uw app hebt geconfigureerd, deze moet uitgeven van een token voor de regels die zijn gekoppeld aan de resource die u wilt gebruiken. |
| client_secret |Vereist |De Toepassingsgeheim die u voor uw app in de portal van de registratie van de app hebt gegenereerd. |
| grant_type |Vereist |Moet `client_credentials`. |

### <a name="second-case-access-token-request-with-a-certificate"></a>Tweede geval: aanvraag voor toegang tot token met een certificaat

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg&grant_type=client_credentials
```

| Parameter | Voorwaarde | Beschrijving |
| --- | --- | --- |
| client_id |Vereist |De aanvraag-ID die de [Registratieportal toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| Bereik |Vereist |De doorgegeven waarde voor de `scope` parameter in deze aanvraag moet de resource identifier (URI toepassing-ID) van de resource die u wilt gebruiken, aangebracht met de `.default` achtervoegsel. De waarde voor de voorbeeld Microsoft Graph heeft `https://graph.microsoft.com/.default`. Deze waarde informeert het v2.0-eindpunt dat alle rechtstreekse toepassing machtigingen die u voor uw app hebt geconfigureerd, deze moet uitgeven van een token voor de regels die zijn gekoppeld aan de resource die u wilt gebruiken. |
| client_assertion_type |Vereist |De waarde moet liggen`urn:ietf:params:oauth:client-assertion-type:jwt-bearer` |
| client_assertion |Vereist | Een bewering (een JSON Web Token) die u nodig hebt voor het maken en te ondertekenen met het certificaat u geregistreerd als referenties voor uw toepassing. Meer informatie over [referenties van het certificaat](active-directory-certificate-credentials.md) voor informatie over het registreren van uw certificaat en de indeling van de bevestiging.|
| grant_type |Vereist |Moet `client_credentials`. |

De parameters zijn bijna hetzelfde is in het geval van de aanvraag door een gedeeld geheim, behalve dat de parameter client_secret wordt vervangen door twee parameters: een client_assertion_type en client_assertion.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie ziet er als volgt:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameter | Beschrijving |
| --- | --- |
| access_token |Het aangevraagde toegangstoken. De app kunt dit token gebruiken om te verifiëren met de beveiligde bron, zoals een Web-API. |
| token_type |Geeft de waarde van het type token. Het enige type dat ondersteunt Azure AD is `bearer`. |
| expires_in |Hoe lang het toegangstoken is ongeldig (in seconden). |

### <a name="error-response"></a>Foutbericht
Een foutmelding ziet er als volgt:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een tekenreeks van de fout code die u gebruiken kunt voor het classificeren van typen fouten die optreden en om te reageren op fouten. |
| error_description |Een specifiek foutbericht die u kan helpen de hoofdoorzaak van een verificatiefout identificeren. |
| error_codes |Een lijst met foutcodes STS-specifieke die met diagnostische gegevens helpen kunnen. |
| tijdstempel |De tijd waarop de fout is opgetreden. |
| trace_id |Een unieke id voor de aanvraag die met diagnostische gegevens helpen kan. |
| correlation_id |Een unieke id voor de aanvraag die met diagnostische gegevens over de onderdelen helpen kan. |

## <a name="use-a-token"></a>Gebruik een token
Nu u een token hebt aangeschaft, gebruiken het token waarmee aanvragen op de resource. Wanneer het token is verlopen, herhaalt u de aanvraag voor de `/token` eindpunt te verkrijgen van een nieuw toegangstoken.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro tip: Try the following command! (Replace the token with your own.)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Codevoorbeeld
Zie voor een voorbeeld van een toepassing of implementeert de referenties van de client met behulp van de beheerder verleent toestemming eindpunt geven onze [v2.0-daemon-codevoorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
