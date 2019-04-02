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
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: celested
ms.reviewer: hirsin, jesakowi, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f6ccc2a355b22c2235253b78a1efa3912234027a
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793455"
---
# <a name="permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Machtigingen en toestemming in de Azure Active Directory v2.0-eindpunt

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Toepassingen die zijn geïntegreerd met Microsoft identity-platform gaat u als volgt een autorisatiemodel waarmee gebruikers en beheerders controle over hoe gegevens kunnen worden geopend. De implementatie van de autorisatiemodel is bijgewerkt op het v2.0-eindpunt en verandert hoe een app moet communiceren met de Microsoft identity-platform. In dit artikel bevat informatie over de basisconcepten van dit autorisatiemodel, met inbegrip van bereiken, machtigingen en toestemming.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle scenario's en onderdelen. Om te bepalen of het v2.0-eindpunt moet worden gebruikt, lees meer over [v2.0 beperkingen](active-directory-v2-limitations.md).

## <a name="scopes-and-permissions"></a>Scopes en -machtigingen

De Microsoft identity platform implementeert de [OAuth 2.0](active-directory-v2-protocols.md) (authorization protocol). OAuth 2.0 is een methode waarmee een app van derden toegang web gehoste bronnen namens een gebruiker tot. Een web-hosted-resource die kan worden geïntegreerd met het Microsoft identity-platform is een resource-id of *URI toepassings-ID*. Bijvoorbeeld, van Microsoft web hosting resources onder andere:

* Microsoft Graph: `https://graph.microsoft.com`
* Office 365 Mail API: `https://outlook.office.com`
* Azure AD Graph: `https://graph.windows.net`

> [!NOTE]
> Het is raadzaam dat u Microsoft Graph in plaats van Azure AD Graph, Office 365-e-Mail-API, enzovoort gebruiken.

Hetzelfde geldt voor alle resources van derden die zijn geïntegreerd met het Microsoft identity-platform. Een van deze resources kunnen ook een reeks machtigingen die kunnen worden gebruikt om de functionaliteit van die resource onderverdelen in kleinere chunks definiëren. Een voorbeeld: [Microsoft Graph](https://graph.microsoft.com) bevat de gedefinieerde machtigingen voor het uitvoeren van de volgende taken, onder andere:

* Lezen van een gebruiker agenda
* Schrijven naar de agenda van een gebruiker
* E-mail met een gebruiker als afzender verzenden

De resource heeft met het definiëren van deze typen machtigingen, uiterst gedetailleerde controle over de gegevens en hoe de functionaliteit van de API wordt weergegeven. Een app van derden kan deze machtigingen aanvragen van gebruikers en beheerders die de aanvraag voor de app moet goedkeuren kunnen krijgen tot gegevens of handelen namens een gebruiker. Door logische groepen te verdelen van de resource-functionaliteit in kleinere machtigingensets, kunnen apps van derden worden gebouwd om aan te vragen van alleen de specifieke machtigingen die ze nodig hebben om uit te voeren hun functie. Gebruikers en beheerders weet precies welke gegevens de app toegang heeft tot, en deze kunnen mogelijk meer vertrouwen dat deze is niet aan de hand met kwade bedoelingen. Ontwikkelaars moeten altijd ontmoeten door het concept van minimale bevoegdheden, alleen de machtigingen die ze nodig hebben voor hun toepassingen goed te laten wordt gevraagd.

In de OAuth 2.0, deze typen machtigingen genoemd *scopes*. Worden ze ook vaak gewoon aangeduid als *machtigingen*. Een machtiging wordt weergegeven in het Microsoft identity-platform als een string-waarde. U doorgaat met de Microsoft Graph-voorbeeld, is de tekenreekswaarde voor elke machtiging:

* Lezen van de agenda van een gebruiker met behulp van `Calendars.Read`
* Schrijven naar de agenda van een gebruiker met behulp van `Calendars.ReadWrite`
* E-mail verzenden als een gebruiker met behulp van `Mail.Send`

Een app-aanvragen meestal deze machtigingen door de bereiken op te geven in aanvragen voor het v2.0 eindpunt voor autorisatie. Bepaalde machtigingen hoge bevoegdheden kunnen echter alleen worden verleend via goedgekeurd door een beheerder en over het algemeen aangevraagd/verleend met behulp de [administrator toestemming eindpunt](v2-permissions-and-consent.md#admin-restricted-permissions). Lees verder voor meer informatie.

## <a name="permission-types"></a>Machtigingstypen

Microsoft identity-platform ondersteunt twee typen machtigingen: **overgedragen machtigingen** en **Toepassingsmachtigingen**.

* **Gedelegeerde machtigingen** worden gebruikt door apps waarvoor een aangemelde gebruiker aanwezig zijn. Voor deze apps of de gebruiker of beheerder hiermee akkoord gaat met de machtigingen die de app-aanvragen en de app is overgedragen machtiging om te fungeren als de gebruiker is aangemeld bij het maken van aanroepen naar de doelresource. Sommige gedelegeerde machtigingen kunnen worden gegeven door gebruikers zonder beheerdersrechten, maar sommige machtigingen hogere bevoegdheden vereisen [administrator toestemming](v2-permissions-and-consent.md#admin-restricted-permissions). Als u wilt weten welke beheerder rollen toestemming voor gedelegeerde machtigingen geven kunnen, Zie [rol beheerdersmachtigingen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md).

* **Toepassingsmachtigingen** worden gebruikt door apps die worden uitgevoerd zonder een aangemelde gebruiker aanwezig zijn, bijvoorbeeld: apps die worden uitgevoerd als Achtergrondservices of daemons.  Machtigingen van de toepassing mag alleen bestaan uit [ingestemd door een beheerder](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

_Effectieve machtigingen_ zijn de machtigingen die uw app heeft bij aanvragen voor de doelresource. Het is belangrijk om te begrijpen van het verschil tussen de gedelegeerde machtigingen van de toepassing die uw app is verleend en de effectieve machtigingen bij het maken van aanroepen naar de doelresource.

- Voor gedelegeerde machtigingen, het _effectieve machtigingen_ van uw app is het laagst mogelijke snijpunt van de gedelegeerde machtigingen die de app heeft gekregen (via toestemming) en de bevoegdheden van de momenteel aangemelde gebruiker. Uw app kan nooit meer machtigingen hebben dan de aangemelde gebruiker. De machtigingen van de aangemelde gebruiker kunnen in organisaties worden bepaald door beleid of door lidmaatschap in een of meer beheerdersrollen. Als u wilt weten welke beheerder rollen toestemming voor gedelegeerde machtigingen geven kunnen, Zie [rol beheerdersmachtigingen in Azure AD](../users-groups-roles/directory-assign-admin-roles.md).
  Bijvoorbeeld, wordt ervan uitgegaan dat uw app heeft gekregen de _User.ReadWrite.All_ overgedragen machtiging. Deze machtiging verleent uw app in feite machtigingen om het profiel van elke gebruiker in een organisatie te lezen en bij te werken. Als de aangemelde gebruiker een globale beheerder is, kan uw app het profiel van elke gebruiker in de organisatie bijwerken. Als de aangemelde gebruiker echter geen beheerdersrol heeft, zal uw app alleen het profiel van de aangemelde gebruiker kunnen bijwerken. De app kan geen profielen van andere gebruikers in de organisatie bijwerken omdat de gebruiker namens welke de app machtigingen heeft om te handelen niet over deze rechten beschikt.
  
- Voor de machtigingen van de toepassing, de _effectieve machtigingen_ van uw app is het niveau van bevoegdheden impliciet door de machtiging Volledig. Bijvoorbeeld, een app met de _User.ReadWrite.All_ machtiging van de toepassing kan het profiel van elke gebruiker in de organisatie bijwerken. 

## <a name="openid-connect-scopes"></a>OpenID Connect-scopes

Het v2.0-implementatie van OpenID Connect is een aantal goed gedefinieerde bereiken die niet van toepassing op een specifieke resource: `openid`, `email`, `profile`, en `offline_access`. De `address` en `phone` OpenID Connect bereiken worden niet ondersteund.

### <a name="openid"></a>openid

Als een app aanmelden met behulp van uitvoert [OpenID Connect](active-directory-v2-protocols.md), deze moet aanvragen de `openid` bereik. De `openid` bereik ziet op de pagina werken account toestemming als de machtiging 'Aanmelden' en op de persoonlijke toestemming pagina van een Microsoft-account als de machtiging 'Uw profiel bekijken en verbinding maken met apps en services met uw Microsoft-account'. Met deze machtiging kan een app een unieke id voor de gebruiker kan ontvangen in de vorm van de `sub` claim. Het geeft ook de apptoegang tot het eindpunt van de gebruikersgegevens. De `openid` bereik kan worden gebruikt bij het token v2.0-eindpunt aan te schaffen ID-tokens, die kunnen worden gebruikt voor het beveiligen van HTTP-aanroepen tussen de verschillende onderdelen van een app.

### <a name="email"></a>e-mail

De `email` bereik kan worden gebruikt met de `openid` bereik en voor eventuele andere. Geeft de apptoegang tot de primaire e-mailadres van de gebruiker in de vorm van de `email` claim. De `email` claim is opgenomen in een token alleen als een e-mailadres is gekoppeld aan het gebruikersaccount dat niet altijd het geval is is. Als deze gebruikt de `email` bereik, uw app moet worden voorbereid voor het afhandelen van een aanvraag waarin de `email` claim bestaat niet in het token.

### <a name="profile"></a>profiel

De `profile` bereik kan worden gebruikt met de `openid` bereik en voor eventuele andere. Deze geeft de apptoegang tot een aanzienlijke hoeveelheid informatie over de gebruiker. Deze toegang heeft tot informatie bevat, maar is niet beperkt tot, van de gebruiker de voornaam, achternaam, gewenste gebruikersnaam en object-ID. Zie voor een volledige lijst van het profiel claims die beschikbaar zijn in de parameter id_tokens voor een specifieke gebruiker, de [ `id_tokens` verwijzing](id-tokens.md).

### <a name="offlineaccess"></a>offline_access

De [ `offline_access` bereik](https://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) biedt uw apptoegang tot resources namens de gebruiker voor een langere periode. Op de pagina toestemming weergegeven dit bereik als de machtiging 'Gewoon toegang tot de gegevens die u toegang hebt gegeven'. Wanneer een gebruiker keurt de `offline_access` bereik, uw app kan vernieuwingstokens ontvangen van het token v2.0-eindpunt. Vernieuwen van tokens worden lange levensduur hebben. Uw app kan aan het nieuwe toegangstokens oudere zijn verlopen.

Als uw app niet expliciet aanvragen de `offline_access` bereik, het ontvangt geen vernieuwingstokens. Dit betekent dat wanneer u een autorisatiecode in inwisselt de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols.md), ontvangt u alleen een toegangstoken van de `/token` eindpunt. Het toegangstoken is ongeldig voor een korte periode. Het toegangstoken is verlopen meestal binnen een uur. Op dat punt, uw app nodig heeft om te leiden van de gebruiker terug naar de `/authorize` eindpunt naar een nieuwe autorisatiecode ophalen. Tijdens deze omleiding, afhankelijk van het type app, moet de gebruiker mogelijk hun referenties opnieuw invoeren of opnieuw instemmen met machtigingen.  Houd er rekening mee dat hoewel de `offline_access` bereik wordt automatisch aangevraagd door de server, de client moet het nog steeds aanvragen om te kunnen ontvangen van het vernieuwen van tokens. 

Zie voor meer informatie over het vernieuwen van tokens gebruiken de [protocolnaslaginformatie voor v2.0](active-directory-v2-protocols.md).

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

De `scope` parameter is een door spaties gescheiden lijst van gedelegeerde machtigingen die de app is aangevraagd. Elke machtiging die wordt aangegeven door de waarde van de machtiging van de resource-id (de URI toepassings-ID) toe te voegen. In het voorbeeld aanvraag moet de app toestemming voor het lezen van de gebruiker agenda en e-mail verzenden als de gebruiker.

Nadat de gebruiker de referenties invoert, wordt het v2.0-eindpunt voor een overeenkomende record van gecontroleerd *toestemming van de gebruiker*. Als de gebruiker heeft niet ingestemd met een van de aangevraagde machtigingen in het verleden, noch heeft een beheerder ingestemd met deze machtigingen namens de hele organisatie, het v2.0-eindpunt wordt de gebruiker gevraagd om de vereiste machtigingen te verlenen.

> [!NOTE]
> Op dit moment de `offline_access` ('gewoon toegang tot de gegevens die u toegang hebt gegeven") en `user.read` ('aanmelden en uw profiel te lezen') machtigingen automatisch zijn opgenomen in de eerste toestemming voor een toepassing.  Deze machtigingen zijn algemeen vereist zijn voor de functionaliteit van de juiste app - `offline_access` de apptoegang tot het vernieuwen van tokens, kritieke biedt voor native en web-apps, terwijl `user.read` biedt toegang tot de `sub` claim, zodat de client of de app correct identificeren van de gebruiker gedurende de tijd en toegang elementaire gebruikersgegevens.  

![Toestemming voor Work-account](./media/v2-permissions-and-consent/work_account_consent.png)

Wanneer de gebruiker de machtiging-aanvraag goedkeurt, toestemming is geregistreerd en de gebruiker beschikt niet over accepteren opnieuw op de volgende aanmeldingen voor de toepassing.

## <a name="requesting-consent-for-an-entire-tenant"></a>Aanvragen van toestemming voor een hele tenant

Vaak, wanneer een organisatie een licentie of abonnement voor een toepassing koopt, wil de organisatie proactief instellen van de toepassing voor gebruik door alle leden van de organisatie. Een beheerder kan toestemming voor de toepassing namens een gebruiker in de tenant verlenen als onderdeel van dit proces. Als de beheerder toestemming voor de gehele tenant verleent, weergegeven niet van de organisatie gebruikers een pagina toestemming voor de toepassing.

Uw app kan het eindpunt beheerder toestemming gebruiken voor het aanvragen van toestemming voor gedelegeerde machtigingen voor alle gebruikers in een tenant.

Toepassingen moeten ook het eindpunt beheerder toestemming gebruiken om aan te vragen van de machtigingen van de toepassing.

## <a name="admin-restricted-permissions"></a>Beheerder beperkte machtigingen

Sommige machtigingen hoge bevoegdheden in het Microsoft-ecosysteem kunnen worden ingesteld op *administrator beperkt*. Voorbeelden van dit soort machtigingen omvatten het volgende:

* Het volledige profiel van alle gebruikers lezen met behulp van `User.Read.All`
* Gegevens schrijven naar de map van een organisatie met behulp van `Directory.ReadWrite.All`
* Alle groepen in de adreslijst van een organisatie met behulp van lezen `Groups.Read.All`

Hoewel een consument-gebruiker kan een App toegang tot dit soort gegevens verlenen, worden organisatie-gebruikers worden uitgesloten van het verlenen van toegang tot de dezelfde set van gevoelige bedrijfsgegevens. Als uw toepassing worden aangevraagd toegang op een van deze machtigingen van een organisatie-gebruiker, ontvangt de gebruiker een foutmelding krijgen dat ze niet gemachtigd zijn om in te stemmen op uw app machtigingen.

Als uw app nodig heeft voor toegang tot beperkte beheerder bereiken voor bedrijven, moet u aanvragen ze rechtstreeks vanuit de bedrijfsbeheerder van een, ook met behulp van het eindpunt beheerder toestemming, hieronder wordt beschreven.

Als de toepassing aanvraagt hoge bevoegdheid overgedragen machtigingen en een beheerder deze machtigingen via het eindpunt beheerder toestemming verleent, wordt toestemming verleend voor alle gebruikers in de tenant.

Als de toepassing machtigingen van de toepassing aanvraagt en een beheerder verleent dat deze machtigingen via de beheerder toestemming eindpunt, wordt deze machtiging niet gedaan namens een specifieke gebruiker. In plaats daarvan de clienttoepassing krijgt de machtigingen *rechtstreeks*. Deze typen machtigingen zijn doorgaans alleen gebruikt door de daemon-services en andere niet-interactieve toepassingen die worden uitgevoerd op de achtergrond.

## <a name="using-the-admin-consent-endpoint"></a>Met behulp van het eindpunt beheerder toestemming

Wanneer de beheerder van een bedrijf maakt gebruik van uw toepassing en wordt omgeleid naar het geautoriseerde eindpunt, wordt de Microsoft identity-platform detecteren van de gebruikersrol en vraag deze als ze zouden graag om in te stemmen namens de gehele tenant voor de machtigingen die u hebt aangevraagd. Er is echter ook een exclusieve beheerdersverbinding toestemming-eindpunt die u gebruiken kunt als u proactief aanvragen wilt dat een beheerder machtiging namens de gehele tenant verleent. Met behulp van dit eindpunt is ook nodig voor het aanvragen van de machtigingen van de toepassing (dit kan niet worden aangevraagd met behulp van het geautoriseerde eindpunt).

Als u deze stappen hebt uitgevoerd, kan uw app machtigingen voor alle gebruikers in een tenant, met inbegrip van de beheerder beperkte bereiken kunt aanvragen. Dit is een bewerking met hoge bevoegdheden en moet alleen worden uitgevoerd als die nodig zijn voor uw scenario.

Zie voor een codevoorbeeld van waarmee de stappen de [administrator beperkt scopes voorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in de portal voor app-registratie van aanvragen

De toestemming van een beheerder een bereikparameter niet accepteert, zodat de machtigingen worden aangevraagd moet statisch zijn gedefinieerd in de registratie van de toepassing. In het algemeen is het aanbevolen procedure om ervoor te zorgen dat de machtigingen die statisch zijn gedefinieerd voor een bepaalde toepassing een hoofdverzameling van de machtigingen dat deze wordt aangevraagd dynamisch/incrementeel zijn.

De lijst met machtigingen die zijn statisch aangevraagd voor een toepassing configureren: 
1. Ga naar uw toepassing in de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of [maken van een app](quickstart-v2-register-an-app.md) als u dat nog niet gedaan hebt.
2. Zoek de **machtigingen voor Microsoft Graph** sectie en voeg vervolgens de machtigingen die uw app nodig heeft.
3. **Sla** de app-registratie.

### <a name="recommended-sign-the-user-into-your-app"></a>Aanbevolen: Meld u aan de gebruiker in uw app

Wanneer u een toepassing die gebruikmaakt van het eindpunt beheerder toestemming bouwt, moet de app normaal gesproken een pagina of weergave waarin de beheerder van de app-machtigingen kunt goedkeuren. Deze pagina kan deel uitmaken van de app Meld u aan flow, onderdeel van de app-instellingen, of een specifieke stroom met 'verbinding maken'. In veel gevallen is het handig voor de app weer te geven 'connect' weergave alleen nadat een gebruiker is aangemeld met een werk- of school Microsoft-account.

Wanneer u de gebruiker zich in uw app, kunt u de organisatie die de beheerder behoort voordat ze de benodigde machtigingen goedkeuren kunt identificeren. Hoewel niet strikt noodzakelijk is, kunt u bij het maken van een meer intuïtieve ervaring voor gebruikers van uw organisatie. Volg voor het tekenen van de gebruiker in onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

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
| `tenant` | Vereist | De directory-tenant die u wilt toestemming van aanvragen. Kan worden opgegeven in de beschrijvende naamindeling of GUID of algemeen waarnaar wordt verwezen met `common` zoals te zien is in het voorbeeld. |
| `client_id` | Vereist | De toepassing (client)-ID die de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) of [nieuwe portal voor App-registraties (preview)](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) is toegewezen aan uw app. |
| `redirect_uri` | Vereist |De omleidings-URI waar u het antwoord moet worden verzonden voor uw app om af te handelen. Het moet exact overeenkomen met een van de omleidings-URI's die u in de portal voor app-registratie hebt geregistreerd. |
| `state` | Aanbevolen | Een waarde die is opgenomen in de aanvraag die wordt ook in het token antwoord geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. Gebruik de status van de informatie over de status van de gebruiker in de app coderen voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave op. |

Op dit moment, vereist Azure AD een tenantbeheerder zijn om aan te melden bij de aanvraag te voltooien. De beheerder wordt gevraagd de machtigingen die u hebt aangevraagd voor uw app in de portal van de registratie van de app goedkeuren.

#### <a name="successful-response"></a>Geslaagde reactie

Als de beheerder heeft de machtigingen voor uw app goedgekeurd, is de geslaagde respons ziet eruit als volgt:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Description |
| --- | --- |
| `tenant` | De directory-tenant die uw toepassing, de machtigingen aangevraagd, in GUID-indeling. |
| `state` | Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. De status wordt gebruikt om informatie over de status van de gebruiker in de app coderen voordat de verificatieaanvraag heeft plaatsgevonden, zoals de pagina of de weergave die ze al had geopend. |
| `admin_consent` | Wordt ingesteld op `True`. |

#### <a name="error-response"></a>Foutbericht

Als de beheerder worden de machtigingen voor uw app niet goedgekeurd, wordt de mislukte reactie ziet eruit zoals deze:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Description |
| --- | --- |
| `error` | Een tekenreeks voor de foutcode die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| `error_description` | Een specifieke foutbericht dat een ontwikkelaar kan helpen de hoofdoorzaak van een fout identificeren. |

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

## <a name="the-default-scope"></a>Het bereik /.default

U kunt de `/.default` bereik om u te helpen uw apps van het eindpunt v1.0 migreren naar het v2.0-eindpunt. Dit is een ingebouwd bereik voor elke toepassing die naar een statische lijst met machtigingen die zijn geconfigureerd op de registratie van de toepassing verwijst. Een `scope` waarde van `https://graph.microsoft.com/.default` is functioneel hetzelfde als de eindpunten v1.0 `resource=https://graph.microsoft.com` -dat wil zeggen het aanvraagt een token met de scopes op Microsoft Graph dat de toepassing voor is geregistreerd in Azure portal.

Het bereik /.default kan worden gebruikt in een OAuth 2.0-stroom, maar is met name nodig in de [op namens-stroom](v2-oauth2-on-behalf-of-flow.md) en [clientreferenties die stroom](v2-oauth2-client-creds-grant-flow.md).  

> [!NOTE]
> Statische door clients kunnen niet worden gecombineerd (`/.default`) en dynamische toestemming in één aanvraag. Dus `scope=https://graph.microsoft.com/.default+mail.read` zal leiden tot een fout vanwege de combinatie van bereik typen.

### <a name="default-and-consent"></a>/.Default en toestemming

De `/.default` bereik activeert het eindpuntgedrag v1.0 voor `prompt=consent` ook. Het aanvraagt toestemming voor alle machtigingen die zijn geregistreerd door de toepassing, ongeacht de bron. Als u wordt opgenomen als onderdeel van de aanvraag de `/.default` bereik retourneert een token dat bestaat uit de bereiken voor de resource die specifiek is aangevraagd.

### <a name="default-when-the-user-has-already-given-consent"></a>/.Default wanneer de gebruiker heeft al ingestemd

Omdat `/.default` is identiek aan de `resource`-georiënteerde v1.0-eindpunt gedrag, het zorgt voor het gedrag van de toestemming van de v1.0-eindpunt. Dat wil zeggen `/.default` alleen een toestemmingsprompt wordt geactiveerd als er geen machtiging heeft verleend tussen de client en de resource door de gebruiker. Als een dergelijke toestemming bestaat, wordt klikt u vervolgens een token geretourneerd met alle scopes die zijn verleend door de gebruiker voor die bron. Echter, als u hebt geen machtiging heeft gekregen, of de `prompt=consent` parameter is opgegeven, een toestemmingsprompt worden weergegeven voor alle scopes die zijn geregistreerd door de clienttoepassing. 

#### <a name="example-1-the-user-or-tenant-admin-has-granted-permissions"></a>Voorbeeld 1: De gebruiker of een tenantbeheerder heeft machtigingen verleend

De gebruiker (of een tenantbeheerder) heeft de client de Microsoft Graph machtigingen verleend `mail.read` en `user.read`. Als de client een aanvraag doet voor `scope=https://graph.microsoft.com/.default`, en vervolgens zonder toestemming vragen worden weergegeven, ongeacht de inhoud van de clienttoepassingen geregistreerde machtigingen voor Microsoft Graph. Een token wordt geretourneerd met de scopes `mail.read` en `user.read`.

#### <a name="example-2-the-user-hasnt-granted-permissions-between-the-client-and-the-resource"></a>Voorbeeld 2: De gebruiker nog niet is gemachtigd tussen de client en de resource

Er is geen toestemming voor de gebruiker bestaat tussen de client en de Microsoft Graph. De client is geregistreerd voor de `user.read` en `contacts.read` machtigingen, evenals het bereik van Azure Key Vault `https://vault.azure.net/user_impersonation`. Wanneer de client vraagt een token voor `scope=https://graph.microsoft.com/.default`, de gebruiker ziet een toestemmingsscherm voor de `user.read`, `contacts.read`, en de Key Vault `user_impersonation` bereiken. Het token geretourneerd heeft zojuist de `user.read` en `contacts.read` scopes erin.

#### <a name="example-3-the-user-has-consented-and-the-client-requests-additional-scopes"></a>Voorbeeld 3: De gebruiker heeft ingestemd en de client vraagt een extra scopes

De gebruiker heeft al ingestemd met `mail.read` voor de client. De client is geregistreerd voor de `contacts.read` bereik in de registratie ervan. Wanneer de client dient een aanvraag voor een token met `scope=https://graph.microsoft.com/.default` en toestemming van de aanvragen via `prompt=consent`, de gebruiker ziet een toestemmingsscherm voor alleen en alle machtigingen die zijn geregistreerd door de toepassing. `contacts.read` in het instemmingsscherm aanwezig zijn, maar `mail.read` niet. Het token wordt geretourneerd is voor Microsoft Graph en bevat `mail.read` en `contacts.read`.

### <a name="using-the-default-scope-with-the-client"></a>Het bereik /.default gebruiken met de client

Een speciaal geval van de `/.default` bereik bestaat wanneer een client een aanvraag een eigen `/.default` bereik. Het volgende voorbeeld ziet u dit scenario.

```
// Line breaks are for legibility only.

GET https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
response_type=token            //code or a hybrid flow is also possible here
&client_id=9ada6f8a-6d83-41bc-b169-a306c21527a5
&scope=9ada6f8a-6d83-41bc-b169-a306c21527a5/.default
&redirect_uri=https%3A%2F%2Flocalhost
&state=1234
```

Dit resulteert in een toestemmingsscherm voor alle geregistreerde machtigingen (indien van toepassing op basis van de bovenstaande beschrijvingen van toestemming en `/.default`), geeft als resultaat een id_token, in plaats van een toegangstoken.  Dit gedrag voor bepaalde verouderde clients van verplaatsen van ADAL naar MSAL bestaat en mag niet worden gebruikt door nieuwe clients die zijn gericht op het v2.0-eindpunt.  

## <a name="troubleshooting-permissions-and-consent"></a>Oplossen van problemen met machtigingen en toestemming

Als u of gebruikers van uw toepassing onverwachte fouten tijdens het proces toestemming weergegeven wordt, ziet u in dit artikel voor stappen voor probleemoplossing: [Onverwachte fout bij het uitvoeren van toestemming voor een toepassing](../manage-apps/application-sign-in-unexpected-user-consent-error.md).
