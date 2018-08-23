---
title: Azure Active Directory v2.0 bereiken, machtigingen en toestemming | Microsoft Docs
description: Een beschrijving van de autorisatie in de Azure AD v2.0-eindpunt, met inbegrip van bereiken, machtigingen en toestemming.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2018
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: 6d3847f547646ae7c62f98b4cee716af5c6ba5e9
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42054942"
---
# <a name="scopes-permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Bereiken, machtigingen en toestemming in de Azure Active Directory v2.0-eindpunt
Apps die met Azure Active Directory (Azure AD integreren) gaat u als volgt een autorisatiemodel waarmee gebruikers controle over hoe een app toegang heeft tot hun gegevens. Het v2.0-implementatie van de autorisatiemodel is bijgewerkt en verandert hoe een app moet communiceren met Azure AD. In dit artikel bevat informatie over de basisconcepten van dit autorisatiemodel, met inbegrip van bereiken, machtigingen en toestemming.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

## <a name="scopes-and-permissions"></a>Scopes en -machtigingen
Azure AD worden de [OAuth 2.0](active-directory-v2-protocols.md) (authorization protocol). OAuth 2.0 is een methode waarmee een app van derden toegang web gehoste bronnen namens een gebruiker tot. Een web-hosted-resource die kan worden geïntegreerd met Azure AD heeft een resource-id of *URI toepassings-ID*. Bijvoorbeeld, van Microsoft web hosting resources onder andere:

* De Office 365 Unified Mail API: `https://outlook.office.com`
* De Azure AD Graph-API: `https://graph.windows.net`
* Microsoft Graph: `https://graph.microsoft.com`

Hetzelfde geldt voor alle externe resources die zijn geïntegreerd met Azure AD. Een van deze resources kunnen ook een reeks machtigingen die kunnen worden gebruikt om de functionaliteit van die resource onderverdelen in kleinere chunks definiëren. Een voorbeeld: [Microsoft Graph](https://graph.microsoft.io) bevat de gedefinieerde machtigingen voor het uitvoeren van de volgende taken, onder andere:

* Lezen van een gebruiker agenda
* Schrijven naar de agenda van een gebruiker
* E-mail met een gebruiker als afzender verzenden

De resource heeft met het definiëren van deze typen machtigingen, uiterst gedetailleerde controle over de gegevens en hoe de gegevens zichtbaar wordt gemaakt. Een app van derden kunt u deze machtigingen aanvragen van een appgebruiker. De app-gebruiker moet de machtigingen goedkeuren voordat de app namens de gebruiker kan fungeren. Door logische groepen te verdelen van de resource-functionaliteit in kleinere machtigingensets, kunnen apps van derden worden gebouwd om aan te vragen van alleen de specifieke machtigingen die ze nodig hebben om uit te voeren hun functie. App-gebruikers kunnen weet precies hoe een app gebruikt de gegevens en ze kunnen worden meer vertrouwen dat de app niet wordt gedragen met kwade bedoelingen.

In Azure AD OAuth, deze typen machtigingen worden genoemd *scopes*. Ze ook worden aangeduid als *oAuth2Permissions*. Een scope wordt weergegeven in Azure AD als een string-waarde. U doorgaat met de Microsoft Graph-voorbeeld, is de waarde voor het bereik voor elke machtiging:

* Lezen van de agenda van een gebruiker met behulp van `Calendars.Read`
* Schrijven naar de agenda van een gebruiker met behulp van `Calendars.ReadWrite`
* E-mail verzenden als een gebruiker met behulp van `Mail.Send`

Een app kunt u deze machtigingen aanvragen door te geven van de bereiken in aanvragen naar het v2.0-eindpunt.

## <a name="openid-connect-scopes"></a>OpenID Connect-scopes
Het v2.0-implementatie van OpenID Connect is een aantal goed gedefinieerde bereiken die niet van toepassing op een specifieke resource: `openid`, `email`, `profile`, en `offline_access`.

### <a name="openid"></a>openid
Als een app aanmelden met behulp van uitvoert [OpenID Connect](active-directory-v2-protocols.md), deze moet aanvragen de `openid` bereik. De `openid` bereik ziet op de pagina werken account toestemming als de machtiging 'Aanmelden' en op de persoonlijke toestemming pagina van een Microsoft-account als de machtiging 'Uw profiel bekijken en verbinding maken met apps en services met uw Microsoft-account'. Met deze machtiging kan een app een unieke id voor de gebruiker kan ontvangen in de vorm van de `sub` claim. Het geeft ook de apptoegang tot het eindpunt van de gebruikersgegevens. De `openid` bereik kan worden gebruikt bij het token v2.0-eindpunt aan te schaffen ID-tokens, die kunnen worden gebruikt voor het beveiligen van HTTP-aanroepen tussen de verschillende onderdelen van een app.

### <a name="email"></a>e-mailen
De `email` bereik kan worden gebruikt met de `openid` bereik en voor eventuele andere. Geeft de apptoegang tot de primaire e-mailadres van de gebruiker in de vorm van de `email` claim. De `email` claim is opgenomen in een token alleen als een e-mailadres is gekoppeld aan het gebruikersaccount dat niet altijd het geval is is. Als deze gebruikt de `email` bereik, uw app moet worden voorbereid voor het afhandelen van een aanvraag waarin de `email` claim bestaat niet in het token.

### <a name="profile"></a>profiel
De `profile` bereik kan worden gebruikt met de `openid` bereik en voor eventuele andere. Deze geeft de apptoegang tot een aanzienlijke hoeveelheid informatie over de gebruiker. Deze toegang heeft tot informatie bevat, maar is niet beperkt tot, van de gebruiker de voornaam, achternaam, gewenste gebruikersnaam en object-ID. Zie voor een volledige lijst van het profiel claims die beschikbaar zijn in de parameter id_tokens voor een specifieke gebruiker, de [v2.0 tokens verwijzing](v2-id-and-access-tokens.md).

### <a name="offlineaccess"></a>offline_access
De [ `offline_access` bereik](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) biedt uw apptoegang tot resources namens de gebruiker voor een langere periode. Op de pagina werken account toestemming weergegeven dit bereik als de machtiging 'Toegang tot uw gegevens op elk gewenst moment'. Op de persoonlijke Microsoft-account instemmingspagina, wordt deze weergegeven als de machtiging 'Toegang tot je gegevens op elk gewenst moment'. Wanneer een gebruiker keurt de `offline_access` bereik, uw app kan vernieuwingstokens ontvangen van het token v2.0-eindpunt. Vernieuwen van tokens worden lange levensduur hebben. Uw app kan aan het nieuwe toegangstokens oudere zijn verlopen.

Als uw app geen heeft aangevraagd de `offline_access` bereik, het ontvangt geen vernieuwingstokens. Dit betekent dat wanneer u een autorisatiecode in inwisselt de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols.md), ontvangt u alleen een toegangstoken van de `/token` eindpunt. Het toegangstoken is ongeldig voor een korte periode. Het toegangstoken is verlopen meestal binnen een uur. Op dat punt, uw app nodig heeft om te leiden van de gebruiker terug naar de `/authorize` eindpunt naar een nieuwe autorisatiecode ophalen. Tijdens deze omleiding, afhankelijk van het type app, moet de gebruiker mogelijk hun referenties opnieuw invoeren of opnieuw instemmen met machtigingen.

Zie voor meer informatie over het vernieuwen van tokens gebruiken de [protocolnaslaginformatie voor v2.0](active-directory-v2-protocols.md).

## <a name="accessing-v10-resources"></a>Toegang tot bronnen v1.0
v2.0-toepassingen kunnen aanvragen van tokens en toestemming voor v1.0 toepassingen (zoals de Power BI-API `https://analysis.windows.net/powerbi/api` of Sharepoint-API `https://{tenant}.sharepoint.com`).  Om dit te doen, kunt u verwijzen naar de app-URI en scope-tekenreeks in de `scope` parameter.  Bijvoorbeeld, `scope=https://analysis.windows.net/powerbi/api/Dataset.Read.All` zou aanvragen de Power BI `View all Datasets` machtiging voor uw toepassing. 

Voor het aanvragen van meerdere machtigingen toevoegen de volledige URI met een spatie of `+`, bijvoorbeeld `scope=https://analysis.windows.net/powerbi/api/Dataset.Read.All+https://analysis.windows.net/powerbi/api/Report.Read.All`.  Deze beide vraagt de `View all Datasets` en `View all Reports` machtigingen.  Houd er rekening mee dat net als bij alle Azure AD-bereiken en machtigingen, toepassingen alleen een aanvraag voor één resource tegelijkertijd indienen kunnen - zodat de aanvraag `scope=https://analysis.windows.net/powerbi/api/Dataset.Read.All+https://api.skypeforbusiness.com/Conversations.Initiate`, die zowel de Power BI-aanvragen `View all Datasets` machtigingen en de Skype voor bedrijven `Initiate conversations` machtiging geweigerd vanwege waarin machtiging wordt gevraagd op twee verschillende bronnen.  

### <a name="v10-resources-and-tenancy"></a>V1.0 resources en tenants
De v1.0 en v2.0 Azure AD-protocollen maken gebruik van een `{tenant}` parameter ingesloten in de URI (`https://login.microsoftonline.com/{tenant}/oauth2/`).  Wanneer het v2.0-eindpunt wordt gebruikt voor toegang tot een organisatie-resource v1.0 de `common` en `consumers` tenants kunnen niet worden gebruikt, aangezien deze resources alleen toegankelijk zijn met de organisatie (Azure AD zijn) accounts.  Dus bij het openen van deze resources, alleen de GUID van de tenant of `organizations` kan worden gebruikt als de `{tenant}` parameter.  

Als een toepassing probeert te krijgen tot een organisatie v1.0-resource met een onjuiste tenant, een die vergelijkbaar is met de volgende fout geretourneerd. 

`AADSTS90124: Resource 'https://analysis.windows.net/powerbi/api' (Microsoft.Azure.AnalysisServices) is not supported over the /common or /consumers endpoints. Please use the /organizations or tenant-specific endpoint.`


## <a name="requesting-individual-user-consent"></a>Aanvragen van toestemming van de afzonderlijke gebruiker
In een [OpenID Connect of OAuth 2.0](active-directory-v2-protocols.md) autorisatie-aanvraag, een app kunt aanvragen de machtigingen die nodig zijn met behulp van de `scope` queryparameter. Bijvoorbeeld, wanneer een gebruiker zich aanmeldt bij een app, de app verzendt een aanvraag, zoals in het volgende voorbeeld (met regeleinden toegevoegd voor een betere leesbaarheid):

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendars.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

De `scope` parameter is een door spaties gescheiden lijst met scopes die van de app aanvragen. Elk bereik wordt aangegeven door de waarde voor het bereik van de resource-id (de URI toepassings-ID) toe te voegen. In het voorbeeld aanvraag moet de app toestemming voor het lezen van de gebruiker agenda en e-mail verzenden als de gebruiker.

Nadat de gebruiker de referenties invoert, wordt het v2.0-eindpunt voor een overeenkomende record van gecontroleerd *toestemming van de gebruiker*. Als de gebruiker heeft niet ingestemd met een van de aangevraagde machtigingen in het verleden, het v2.0-eindpunt wordt u gevraagd de gebruiker om de vereiste machtigingen te verlenen.

![Toestemming voor Work-account](./media/v2-permissions-and-consent/work_account_consent.png)

Wanneer de gebruiker de machtiging goedkeurt, wordt de toestemming zodat de gebruiker beschikt niet over opnieuw accepteren op latere accountaanmelding geregistreerd.

## <a name="requesting-consent-for-an-entire-tenant"></a>Aanvragen van toestemming voor een hele tenant
Vaak, wanneer een organisatie een licentie of abonnement voor een toepassing koopt, wil de organisatie de toepassing voor de werknemers volledig heeft ingericht. Een beheerder kan toestemming voor de toepassing namens een werknemer verlenen als onderdeel van dit proces. Als de beheerder toestemming voor de gehele tenant verleent, weergegeven niet van de organisatie werknemers een instemmingspagina voor de toepassing.

Uw app kan het eindpunt beheerder toestemming gebruiken voor het aanvragen van toestemming voor alle gebruikers in een tenant.

## <a name="admin-restricted-scopes"></a>Beheer de toegang is beperkt bereiken
Sommige machtigingen hoge bevoegdheden in het Microsoft-ecosysteem kunnen worden ingesteld op *administrator beperkt*. Voorbeelden van dit soort bereiken zijn de volgende machtigingen:

* Een organisatie-mapgegevens lezen met behulp van `Directory.Read`
* Gegevens schrijven naar de map van een organisatie met behulp van `Directory.ReadWrite`
* Beveiligingsgroepen in de adreslijst van een organisatie met behulp van lezen `Groups.Read.All`

Hoewel een consument-gebruiker kan een App toegang tot dit soort gegevens verlenen, worden organisatie-gebruikers worden uitgesloten van het verlenen van toegang tot de dezelfde set van gevoelige bedrijfsgegevens. Als uw toepassing worden aangevraagd toegang op een van deze machtigingen van een organisatie-gebruiker, ontvangt de gebruiker een foutmelding krijgen dat ze niet gemachtigd zijn om in te stemmen op uw app machtigingen.

Als uw app nodig heeft voor toegang tot beperkte beheerder bereiken voor bedrijven, moet u aanvragen ze rechtstreeks vanuit de bedrijfsbeheerder van een, ook met behulp van het eindpunt beheerder toestemming, hieronder wordt beschreven.

Wanneer een beheerder deze machtigingen via het eindpunt beheerder toestemming verleent, wordt toestemming verleend voor alle gebruikers in de tenant.

## <a name="using-the-admin-consent-endpoint"></a>Met behulp van het eindpunt beheerder toestemming
Als u deze stappen hebt uitgevoerd, kan uw app machtigingen voor alle gebruikers in een tenant, met inbegrip van de beheerder beperkte bereiken kunt verzamelen. Zie voor een codevoorbeeld van waarmee de stappen de [administrator beperkt scopes voorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in de portal voor app-registratie van aanvragen
1. Ga naar uw toepassing in de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of [maken van een app](quickstart-v2-register-an-app.md) als u dat nog niet gedaan hebt.
2. Zoek de **machtigingen voor Microsoft Graph** sectie en voeg vervolgens de machtigingen die uw app nodig heeft.
3. Zorg ervoor dat u **opslaan** de app-registratie.

### <a name="recommended-sign-the-user-in-to-your-app"></a>Aanbevolen: De gebruiker zich aanmeldt bij uw app
Wanneer u een toepassing die gebruikmaakt van het eindpunt beheerder toestemming bouwt, moet de app normaal gesproken een pagina of weergave waarin de beheerder van de app-machtigingen kunt goedkeuren. Deze pagina kan deel uitmaken van de app Meld u aan flow, onderdeel van de app-instellingen, of een specifieke stroom met 'verbinding maken'. In veel gevallen is het handig voor de app weer te geven 'connect' weergave alleen nadat een gebruiker is aangemeld met een werk- of school Microsoft-account.

Wanneer u de gebruiker zich aanmeldt bij uw app, kunt u de organisatie die de beheerder behoort voordat ze de benodigde machtigingen goedkeuren kunt identificeren. Hoewel niet strikt noodzakelijk is, kunt u bij het maken van een meer intuïtieve ervaring voor gebruikers van uw organisatie. Volg voor het tekenen van de gebruiker in onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>De machtigingen aanvragen van een directory-beheerder
Wanneer u klaar om aan te vragen van machtigingen van de beheerder van uw organisatie bent, kunt u de gebruiker omleiden naar het v2.0 *toestemming het beheereindpunt*.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameter | Voorwaarde | Beschrijving |
| --- | --- | --- |
| tenant |Vereist |De directory-tenant die u wilt toestemming van aanvragen. Kan worden opgegeven in de beschrijvende naamindeling of GUID of algemeen waarnaar wordt verwezen met 'algemene' zoals te zien is in het voorbeeld. |
| client_id |Vereist |De aanvraag-ID die de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| redirect_uri |Vereist |De omleidings-URI waar u het antwoord moet worden verzonden voor uw app om af te handelen. Het moet exact overeenkomen met een van de omleidings-URI's die u in de portal voor app-registratie hebt geregistreerd. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die wordt ook in het token antwoord geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. Gebruik de status van de informatie over de status van de gebruiker in de app coderen voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave op. |

Op dit moment, vereist Azure AD een tenantbeheerder zijn om aan te melden bij de aanvraag te voltooien. De beheerder wordt gevraagd de machtigingen die u hebt aangevraagd voor uw app in de portal van de registratie van de app goedkeuren.

#### <a name="successful-response"></a>Geslaagde reactie
Als de beheerder heeft de machtigingen voor uw app goedgekeurd, is de geslaagde respons ziet eruit als volgt:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschrijving |
| --- | --- | --- |
| tenant |De directory-tenant die uw toepassing, de machtigingen aangevraagd, in GUID-indeling. |
| state |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. De status wordt gebruikt om informatie over de status van de gebruiker in de app coderen voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |
| admin_consent |Wordt ingesteld op **waar**. |

#### <a name="error-response"></a>Foutbericht
Als de beheerder worden de machtigingen voor uw app niet goedgekeurd, wordt de mislukte reactie ziet eruit zoals deze:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschrijving |
| --- | --- | --- |
| error |Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een fout identificeren. |

Nadat u een geslaagde respons van het eindpunt beheerder toestemming hebt ontvangen, kan uw app in de machtigingen die zij gevraagd hebben opgedaan. Vervolgens kunt u een token voor de resource die u wilt dat vraagt.

## <a name="using-permissions"></a>Met behulp van machtigingen
Nadat de gebruiker toestemming voor machtigingen voor uw app heeft, kan uw app toegangstokens die staan voor uw app machtiging voor toegang tot een resource in sommige capaciteit aanschaffen. Een toegangstoken kan alleen worden gebruikt voor één resource, maar elke machtiging die uw app heeft gekregen voor die bron gecodeerd binnen het toegangstoken is. Als u wilt een toegangstoken verkrijgen, kunt uw app een aanvraag indienen bij het v2.0-tokeneindpunt als volgt:

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

U kunt het resulterende toegangstoken gebruiken in HTTP-aanvragen naar de resource. Deze geeft op betrouwbare wijze u aan de resource dat uw app de juiste machtigingen voor het uitvoeren van een specifieke taak heeft. 

Zie voor meer informatie over het OAuth 2.0-protocol en toegangstokens verkrijgen, de [protocolnaslaginformatie voor v2.0-eindpunt](active-directory-v2-protocols.md).
