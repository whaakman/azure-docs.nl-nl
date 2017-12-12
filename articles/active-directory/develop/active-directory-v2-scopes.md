---
title: Azure Active Directory v2.0 scopes, machtigingen en toestemming | Microsoft Docs
description: Een beschrijving van de autorisatie in de Azure AD v2.0-eindpunt, met inbegrip van bereiken, machtigingen en toestemming.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: 8f98cbf0-a71d-4e34-babf-e644ad9ff423
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: a93cfd710f89efbd4dab01b84ecdb12b4acb0033
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="scopes-permissions-and-consent-in-the-azure-active-directory-v20-endpoint"></a>Scopes, machtigingen en toestemming in de Azure Active Directory v2.0-eindpunt
Apps die zijn geïntegreerd met Azure Active Directory (Azure AD) voert u een autorisatie-model waarmee gebruikers controle over hoe een app toegang krijgen hun gegevens tot. Het v2.0-implementatie van het model van de autorisatie is bijgewerkt en verandert hoe een app moet communiceren met Azure AD. In dit artikel bevat informatie over de basisconcepten van deze autorisatie-model, inclusief scopes, machtigingen en toestemming.

> [!NOTE]
> Het v2.0-eindpunt biedt geen ondersteuning voor alle Azure Active Directory-scenario's en onderdelen. Meer informatie over om te bepalen of het v2.0-eindpunt moet worden gebruikt, [v2.0 beperkingen](active-directory-v2-limitations.md).
>
>

## <a name="scopes-and-permissions"></a>Scopes en -machtigingen
Azure AD-implementeert de [OAuth 2.0](active-directory-v2-protocols.md) (authorization protocol). OAuth 2.0 is een methode waarmee een app van derden toegang web gehoste bronnen namens een gebruiker tot. Een web gehost resource die kan worden geïntegreerd met Azure AD heeft een bron-id of *Application ID URI*. Bijvoorbeeld: Microsoft website wordt gehost resources onder andere:

* De Office 365 Unified API E-mail:`https://outlook.office.com`
* De Azure AD Graph-API:`https://graph.windows.net`
* Microsoft Graph:`https://graph.microsoft.com`

Hetzelfde geldt voor alle resources van derden die zijn geïntegreerd met Azure AD. Een van deze resources kunt ook een reeks machtigingen die kunnen worden gebruikt voor de functionaliteit van die bron onderverdelen in kleinere reeksen definiëren. Als u bijvoorbeeld [Microsoft Graph](https://graph.microsoft.io) is gedefinieerd machtigingen voor de volgende taken uitvoeren, onder andere:

* Lezen van een gebruiker kalender
* Schrijven naar een gebruiker kalender
* E-mail met een gebruiker als afzender verzenden

De resource heeft met het definiëren van deze typen machtigingen, fijnmazig controle over de gegevens en hoe de gegevens wordt weergegeven. Een app van derden kunt u deze machtigingen aanvragen bij een app-gebruiker. De app-gebruiker moet de machtigingen goedkeuren voordat de app kan namens de gebruiker. Door de verdeling in segmenten van de resource-functionaliteit in kleinere machtigingensets, kunnen apps van derden worden gebouwd om aan te vragen van alleen de specifieke machtigingen die ze nodig hebben om uit te voeren hun werking. App-gebruikers kunnen weet precies wijze waarop een app gebruikmaakt van hun gegevens en ze kunnen worden meer zeker weet dat de app werkt anders met kwade bedoelingen.

In Azure AD en OAuth, deze typen machtigingen worden genoemd *scopes*. Ze soms ook worden aangeduid als *oAuth2Permissions*. Een scope wordt weergegeven in Azure AD als een string-waarde. U kunt doorgaan met het Microsoft Graph-voorbeeld, is de waarde scope voor elke machtiging:

* Lezen van een gebruiker kalender met`Calendars.Read`
* Schrijven naar de kalender van een gebruiker met behulp van`Calendars.ReadWrite`
* E-mail verzenden namens een gebruiker met door`Mail.Send`

Een app kunt u deze machtigingen aanvragen door op te geven van de bereiken in hun aanvragen aan het v2.0-eindpunt.

## <a name="openid-connect-scopes"></a>Scopes OpenID Connect
Het v2.0-implementatie van OpenID Connect, is enkele goed gedefinieerde bereiken die niet van toepassing op een specifieke bron: `openid`, `email`, `profile`, en `offline_access`.

### <a name="openid"></a>openid
Als een app aanmelden met behulp van uitvoert [OpenID Connect](active-directory-v2-protocols.md), verzoekt zij de `openid` bereik. De `openid` bereik ziet op de pagina werken account toestemming als de machtiging 'Aanmelden' en op persoonlijke Microsoft-account toestemming pagina als de machtiging 'Uw profiel weergeven en verbinding maken met apps en services met behulp van uw Microsoft-account'. Met deze machtiging een app een unieke id voor de gebruiker kan ontvangen in de vorm van de `sub` claim. Het biedt ook de apptoegang naar het eindpunt van gebruikersgegevens. De `openid` bereik kan worden gebruikt bij het v2.0-eindpunt voor het token ID-tokens kunnen worden gebruikt voor het beveiligen van HTTP-aanroepen tussen de verschillende onderdelen van een app te verkrijgen.

### <a name="email"></a>e-mailen
De `email` bereik kan worden gebruikt met de `openid` bereik en alle andere. Dit geeft de apptoegang tot de primaire e-mailadres van de gebruiker in de vorm van de `email` claim. De `email` claim is opgenomen in een token alleen als een e-mailadres gekoppeld aan het gebruikersaccount is, is het niet altijd het geval is. Als deze gebruikmaakt van de `email` bereik, uw app moet worden voorbereid voor het afhandelen van een aanvraag waarin de `email` claim bestaat niet in het token.

### <a name="profile"></a>profiel
De `profile` bereik kan worden gebruikt met de `openid` bereik en alle andere. Dit geeft de apptoegang tot een aanzienlijke hoeveelheid informatie over de gebruiker. De toegang tot gegevens bevat, maar is niet beperkt tot de opgegeven naam van de gebruiker, achternaam, voorkeur gebruikersnaam en object-ID. Zie voor een volledige lijst van het profiel claims die beschikbaar zijn in de parameter id_tokens voor een specifieke gebruiker, de [v2.0 tokens verwijzing](active-directory-v2-tokens.md).

### <a name="offlineaccess"></a>offline_access
De [ `offline_access` bereik](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) geeft van uw apptoegang tot bronnen namens de gebruiker voor een langere periode. Op de pagina werken account toestemming weergegeven dit bereik als de machtiging 'Toegang tot uw gegevens op elk gewenst moment'. Deze weergegeven op de persoonlijke Microsoft-account toestemming pagina als de machtiging 'Altijd toegang tot je gegevens'. Wanneer een gebruiker keurt de `offline_access` bereik, uw app vernieuwen van tokens van het token v2.0-eindpunt kunt ontvangen. Vernieuwen van tokens zijn lange levensduur hebben. Uw app kan nieuwe toegangstokens ophalen wanneer oudere zijn verlopen.

Als uw app vraagt niet om de `offline_access` bereik, het ontvangt geen vernieuwen van tokens. Dit betekent dat wanneer u een autorisatiecode in inwisselen de [OAuth 2.0-autorisatiecodestroom](active-directory-v2-protocols.md), ontvangt u een toegangstoken van de `/token` eindpunt. Het toegangstoken is geldig voor een korte periode. Het toegangstoken verloopt meestal binnen een uur. Back-AT dat punt, uw app moet de gebruiker wordt omgeleid naar de `/authorize` eindpunt moet een nieuwe autorisatie-code. Tijdens deze omleiding, afhankelijk van het type app, de gebruiker mogelijk hun referenties opnieuw invoeren of opnieuw instemming met machtigingen.

Zie voor meer informatie over het vernieuwen van tokens gebruiken de [v2.0 protocolnaslaginformatie](active-directory-v2-protocols.md).

## <a name="requesting-individual-user-consent"></a>Afzonderlijke gebruikers toestemming wordt gevraagd
In een [OpenID Connect of OAuth 2.0](active-directory-v2-protocols.md) autorisatie aanvraagt, een app kan aanvragen de machtigingen die nodig is met behulp van de `scope` queryparameter. Bijvoorbeeld, wanneer een gebruiker zich aanmeldt bij een app, de app stuurt een aanvraag, zoals in het volgende voorbeeld (met regeleinden toegevoegd voor de leesbaarheid):

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

De `scope` parameter is een door spaties gescheiden lijst van bereiken die de app aanvraagt. Elke scope wordt aangegeven door de bereikwaarde van de resource-id (de toepassing-ID-URI) toe te voegen. De app moet in het voorbeeld aanvraag machtiging voor het lezen van de gebruiker agenda en mail verzenden als de gebruiker.

Nadat de gebruiker hun referenties invoert, het v2.0-eindpunt controleert of een overeenkomende record van *toestemming van de gebruiker*. Als de gebruiker niet aan een van de aangevraagde machtigingen in het verleden heeft ingestemd, wordt de gebruiker om de vereiste machtigingen te verlenen gevraagd in het v2.0-eindpunt.

![Account toestemming werken](../../media/active-directory-v2-flows/work_account_consent.png)

Wanneer de gebruiker de machtiging goedkeurt, wordt de toestemming vastgelegd zodat de gebruiker om toestemming opnieuw op de volgende accountaanmeldingen niet heeft.

## <a name="requesting-consent-for-an-entire-tenant"></a>Toestemming voor een volledige-tenant aanvragen
Vaak als een organisatie een licentie of abonnement voor een toepassing koopt, wil de organisatie volledig inrichten van de toepassing voor de werknemers. Als onderdeel van dit proces kan een beheerder toestemming voor de toepassing te handelen namens een werknemer verlenen. Als de beheerder toestemming voor de hele tenant verleent, kan de werknemers van de organisatie een pagina toestemming voor de toepassing niet zien.

Uw app kunt om aan te vragen van toestemming voor alle gebruikers in een tenant, het eindpunt beheerder toestemming.

## <a name="admin-restricted-scopes"></a>Scopes administrator beperkt
Sommige hoge privileges machtigingen in het Microsoft-ecosysteem kunnen worden ingesteld op *administrator beperkt*. Voorbeelden van dergelijke bereiken zijn de volgende machtigingen:

* Directory-gegevens voor een organisatie worden gelezen door`Directory.Read`
* Gegevens schrijven naar de map van een organisatie met behulp van`Directory.ReadWrite`
* Beveiligingsgroepen in de map van een organisatie worden gelezen door`Groups.Read.All`

Hoewel een gebruiker consumenten mogelijk een App toegang tot dit soort gegevens verlenen, worden organisatie gebruikers beperkt in het verlenen van toegang tot dezelfde set van gevoelige bedrijfsgegevens. Als uw toepassing toegang naar een van deze machtigingen van een organisatie-gebruiker vraagt, ontvangt de gebruiker een foutbericht dat ze niet gemachtigd om toestemming aan uw app-machtigingen.

Als uw app toegang tot bereiken voor organisaties administrator beperkt vereist, moet u aanvragen ze rechtstreeks vanuit de bedrijfsbeheerder van een, ook met behulp van het eindpunt beheerder toestemming, hieronder wordt beschreven.

Wanneer een beheerder deze machtigingen via het eindpunt beheerder toestemming verleent, wordt toestemming verleend voor alle gebruikers in de tenant.

## <a name="using-the-admin-consent-endpoint"></a>Met behulp van het eindpunt beheerder toestemming
Als u deze stappen hebt uitgevoerd, kan uw app-machtigingen voor alle gebruikers in een tenant, met inbegrip van de administrator beperkt scopes verzamelen. Zie voor een voorbeeld van code die de stappen implementeert de [administrator beperkt scopes voorbeeld](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

### <a name="request-the-permissions-in-the-app-registration-portal"></a>De machtigingen in de app-portal voor registratie van aanvragen
1. Ga naar uw toepassing in de [toepassing Registratieportal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), of [maken van een app](active-directory-v2-app-registration.md) als u dat nog niet gedaan hebt.
2. Zoek de **Microsoft Graph machtigingen** sectie en voeg vervolgens de machtigingen die vereist dat uw app.
3. Zorg ervoor dat u **opslaan** de registratie van de app.

### <a name="recommended-sign-the-user-in-to-your-app"></a>Aanbevolen: De gebruiker zich aanmeldt bij uw app
Als u een toepassing die gebruikmaakt van het eindpunt beheerder toestemming bouwen, moet de app normaal gesproken een pagina of weergave waarin de beheerder van de app-machtigingen kan goedkeuren. Deze pagina kan deel uitmaken van de app aanmelden stroom deel uit van de instellingen van de app of een specifieke stroom van "connect". In veel gevallen is het zinvol voor de app weer te geven 'verbinding' weergave pas nadat een gebruiker is aangemeld met een werk- of school Microsoft-account.

Wanneer u de gebruiker zich aanmeldt bij uw app, kunt u de organisatie die de beheerder behoort voordat het verzoek om de benodigde machtigingen goedkeuren identificeren. Hoewel niet strikt noodzakelijk is, kunt u u bij het maken van een intuïtieve ervaring voor gebruikers van uw organisatie. De gebruiker in ondertekenen, volgt u onze [v2.0 protocol zelfstudies](active-directory-v2-protocols.md).

### <a name="request-the-permissions-from-a-directory-admin"></a>De machtigingen aanvragen bij een directory-beheerder
Wanneer u klaar bent om machtigingen van de beheerder van uw organisatie, kunt u de gebruiker omgeleid naar het v2.0 *toestemming beheereindpunt*.

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
| Tenant |Vereist |De directory-tenant die u wilt machtiging van aanvragen. Kan worden opgegeven in de beschrijvende naam van de indeling of GUID. |
| client_id |Vereist |De aanvraag-ID die de [Registratieportal toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) toegewezen aan uw app. |
| redirect_uri |Vereist |De omleidings-URI waar u het antwoord moet worden verzonden voor uw app om af te handelen. Deze moet exact overeenkomen met een van de omleidings-URI's die u hebt geregistreerd in de portal van de registratie van de app. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. Gebruik de status voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |

Azure AD vereist op dit moment een tenantbeheerder aan te melden bij de aanvraag niet voltooien. De beheerder wordt gevraagd om het goedkeuren van alle machtigingen die u hebt aangevraagd voor uw app in de app-portal voor wachtwoordregistratie.

#### <a name="successful-response"></a>Geslaagde reactie
Als de beheerder heeft de machtigingen voor uw app goedgekeurd, is geslaagd antwoord ziet er als volgt:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameter | Beschrijving |
| --- | --- | --- |
| Tenant |De directory-tenant die uw toepassing, de machtigingen aangevraagd, in GUID-indeling. |
| state |Een waarde die is opgenomen in de aanvraag die ook in het token antwoord worden geretourneerd. Een tekenreeks van alle inhoud die u wilt dat kan zijn. De status wordt gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat het verificatieverzoek opgetreden, zoals de pagina of de weergave op. |
| admin_consent |Worden ingesteld op **true**. |

#### <a name="error-response"></a>Foutbericht
Als de beheerder worden de machtigingen voor uw app niet goedgekeurd, wordt de mislukte reactie als volgt weergegeven:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameter | Beschrijving |
| --- | --- | --- |
| error |Een tekenreeks van de fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden en kan worden gebruikt om te reageren op fouten. |
| error_description |Een specifiek foutbericht waarmee een ontwikkelaar kan identificeren de hoofdoorzaak van een fout. |

Nadat u een geslaagde reactie van het eindpunt beheerder toestemming hebt ontvangen, hebben de machtigingen die zij gevraagd opgedaan met uw app. U kunt vervolgens een token voor de resource die u wilt aanvragen.

## <a name="using-permissions"></a>Met behulp van machtigingen
Nadat de gebruiker akkoord aan machtigingen voor uw app gaat, kan uw app toegangstokens met daarin uw app-machtiging voor toegang tot een resource in sommige capaciteit verkrijgen. Een toegangstoken kan alleen worden gebruikt voor één resource, maar elke machtiging die uw app is verleend voor die bron gecodeerd binnen het toegangstoken is. Als u wilt een toegangstoken aanschaft, kunt uw app een aanvraag aanbrengen in v2.0 eindpunt van het token, als volgt:

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

U kunt het resulterende toegangstoken in HTTP-aanvragen naar de resource. Deze geeft betrouwbaar u aan de resource dat uw app in de juiste machtigingen voor het uitvoeren van een specifieke taak heeft.  

Zie voor meer informatie over het OAuth 2.0-protocol en het ophalen van de toegangstokens de [v2.0 protocol eindpuntreferentie](active-directory-v2-protocols.md).
