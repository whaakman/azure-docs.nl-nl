---
title: Typen toepassingen die kunnen worden gebruikt in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over de typen toepassingen die u in de Azure Active Directory B2C gebruiken kunt.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f4b45c743c0efa1c9df665018b28a8b4ffb76f73
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39238400"
---
# <a name="applications-types-that-can-be-used-in-active-directory-b2c"></a>Typen toepassingen die kunnen worden gebruikt in Active Directory B2C

Azure Active Directory (Azure AD) B2C ondersteunt verificatie voor een verscheidenheid aan moderne toepassingsarchitecturen. Ze zijn allemaal gebaseerd op de protocollen volgens de industrienorm [OAuth 2.0](active-directory-b2c-reference-protocols.md) of [OpenID Connect](active-directory-b2c-reference-protocols.md). Dit document wordt beschreven welke typen toepassingen die u kunt bouwen, onafhankelijk van de taal of het platform u liever. Ook helpt u inzicht in geavanceerde scenario's voordat u begint met het bouwen van toepassingen.

Elke toepassing die gebruikmaakt van Azure AD B2C moet zijn geregistreerd in uw [Azure AD B2C-tenant](active-directory-b2c-get-started.md) met behulp van de [Azure Portal](https://portal.azure.com/). Het registratieproces voor de toepassing verzamelt en waarden, zoals toegewezen:

* Een **toepassings-ID** die uniek voor uw toepassing.
* Een **omleidings-URI** die kunnen worden gebruikt om te leiden reacties terug naar uw toepassing.

Elk verzoek dat naar Azure AD B2C wordt verzonden, geeft een **beleid** op. Een beleid regelt het gedrag van Azure AD. U kunt deze eindpunten ook gebruiken om een uiterst aanpasbare verzameling gebruikerservaringen te maken. Algemene beleidsregels zijn beleidsregels voor registratie, aanmelding en het bewerken van profielen. Als u niet bekend bent met beleidsregels, lees dan over het [uitbreidbaar beleidsframework](active-directory-b2c-reference-policies.md) van Azure AD B2C voordat u verder gaat.

De interactie van elke toepassing volgt in grote lijnen hetzelfde patroon:

1. De toepassing zorgt ervoor dat de gebruiker naar het v2.0-eindpunt om uit te voeren een [beleid](active-directory-b2c-reference-policies.md).
2. De gebruiker voltooit het beleid volgens de beleidsdefinitie.
3. De toepassing ontvangt een beveiligingstoken van het v2.0-eindpunt.
4. De toepassing gebruikt het beveiligingstoken voor toegang tot beveiligde gegevens of een beveiligde bron.
5. De resource-server valideert het beveiligingstoken om te controleren of toegang kan worden verleend.
6. Het beveiligingstoken wordt regelmatig vernieuwd door de toepassing.

Deze stappen kunnen verschillen enigszins afhankelijk van het type van de toepassing die u bouwt.

## <a name="web-applications"></a>Webtoepassingen

Voor web-apps (inclusief .NET, PHP, Java, Ruby, Python en Node.js) die worden gehost op een server en toegankelijk is via een browser, Azure AD B2C ondersteunt [OpenID Connect](active-directory-b2c-reference-protocols.md) voor alle gebruikerservaringen. Hiertoe behoren aanmelden, registreren en profielbeheer. In de Azure AD B2C-implementatie van OpenID Connect initieert uw web-App deze gebruikerservaringen door verificatieaanvragen naar Azure AD. Het resultaat van de aanvraag is een `id_token`. Dit beveiligingstoken vertegenwoordigt de identiteit van de gebruiker. Het bevat ook informatie over de gebruiker in de vorm van claims:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Meer informatie over de typen tokens en claims die beschikbaar zijn voor een toepassing in de [naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md).

In een webtoepassing, bestaat de uitvoering van een [beleid](active-directory-b2c-reference-policies.md) neemt deze stappen op hoog niveau:

![Afbeelding van web-app-banen](./media/active-directory-b2c-apps/webapp.png)

Validatie van het `id_token` met behulp van een openbare ondertekeningssleutel die is ontvangen van Azure AD is voldoende om de identiteit van de gebruiker te controleren. Hiermee wordt ook een sessiecookie ingesteld waarmee de gebruiker bij volgende pagina-aanvragen kan worden geïdentificeerd.

Als u wilt zien van dit scenario werkt, probeert u een van de web application aanmelden codevoorbeelden in onze [aan de slag sectie](active-directory-b2c-overview.md).

Naast eenvoudige aanmelding vergemakkelijken, moet mogelijk ook een webservertoepassing voor toegang tot een back-end-webservice. In dit geval de web-App kunt uitvoeren een iets andere [OpenID Connect-stroom](active-directory-b2c-reference-oidc.md) en tokens verkrijgen met behulp van autorisatiecodes en vernieuwingstokens. Dit scenario wordt beschreven in de volgende sectie over [Web-API's](#web-apis).

## <a name="web-apis"></a>Web-API's

U kunt Azure AD B2C gebruiken om webservices, zoals van uw toepassing RESTful web-API te beveiligen. Web-API's kunnen OAuth 2.0 gebruiken om hun gegevens te beveiligen door verificatie van binnenkomende HTTP-aanvragen via tokens. De aanroeper van een web-API voegt een token toe in de autorisatie-header van een HTTP-aanvraag:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

De web-API kan het token vervolgens gebruiken om de identiteit van de API-aanroeper te verifiëren en om informatie over de aanroeper af te leiden uit de claims die in het token zijn gecodeerd. In de [Naslaginformatie over Azure AD B2C-tokens](active-directory-b2c-reference-tokens.md) vindt u meer informatie over de typen tokens en claims die beschikbaar zijn voor een app.

> [!NOTE]
> Op dit moment ondersteunt Azure AD B2C alleen web-API's die worden gebruikt door hun eigen bekende clients. De voltooide app omvat bijvoorbeeld een iOS-toepassing, een Android-toepassing en een back-end web-API. Deze architectuur wordt volledig ondersteund. Zodat een partner-client, zoals een ander iOS-toepassing, voor toegang tot de dezelfde web die API is momenteel niet ondersteund. Alle onderdelen van de volledige toepassing moet een enkele toepassing-id delen.
>
>

Een web-API kan tokens ontvangen van tal van clients, waaronder webtoepassingen, desktop en mobiele toepassingen, één pagina toepassingen, server-side-daemons en andere web-API's. Hier volgt een voorbeeld van de volledige stroom voor een webtoepassing die een web-API aanroept:

![Afbeelding van web-API van web-app-banen](./media/active-directory-b2c-apps/webapi.png)

Voor meer informatie over autorisatiecodes, vernieuwingstokens en de stappen voor het ophalen van tokens, leest u over het [OAuth 2.0-protocol](active-directory-b2c-reference-oauth-code.md).

Voor meer informatie over het beveiligen van een web-API met behulp van Azure AD B2C bekijkt u de web-API-zelfstudies in de sectie [Aan de slag](active-directory-b2c-overview.md).

## <a name="mobile-and-native-applications"></a>Mobiele en systeemeigen toepassingen

Toepassingen die zijn geïnstalleerd op apparaten, zoals toepassingen voor mobiele en desktop, is vaak nodig voor toegang tot back-endservices of web-API's namens gebruikers. U kunt aangepaste ervaringen voor identiteitsbeheer aan uw eigen toepassingen toevoegen en veilig back-endservices aanroepen met behulp van Azure AD B2C en de [OAuth 2.0-autorisatiecodestroom](active-directory-b2c-reference-oauth-code.md).  

In deze stroom voert de toepassing wordt uitgevoerd [beleid](active-directory-b2c-reference-policies.md) en ontvangt een `authorization_code` van Azure AD wanneer de gebruiker het beleid is voltooid. De `authorization_code` vertegenwoordigt van de toepassing toestemming om aan te roepen back-end-services namens de gebruiker die momenteel is aangemeld. De toepassing vervolgens kan uitwisselen de `authorization_code` op de achtergrond voor een `id_token` en een `refresh_token`.  De toepassingen kunnen gebruikmaken van de `id_token` om een back-end-web-API in HTTP-aanvragen te verifiëren. Het `refresh_token` kan ook worden gebruikt om nieuwe `id_token` te verkrijgen wanneer de oudere zijn verlopen.

> [!NOTE]
> Azure AD B2C ondersteunt momenteel alleen tokens die worden gebruikt voor toegang tot een toepassing uit eigen back-end-webservice. De volledige toepassing omvat bijvoorbeeld een iOS-toepassing, een Android-toepassing en een back-end web-API. Deze architectuur wordt volledig ondersteund. Uw iOS-toepassing voor toegang tot een web-API van de partner met behulp van OAuth 2.0-toegangstokens zodat wordt momenteel niet ondersteund. Alle onderdelen van de volledige toepassing moet een enkele toepassing-id delen.
>
>

![Afbeelding van banen voor systeemeigen web-app](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Huidige beperkingen

Azure AD B2C biedt momenteel geen ondersteuning voor de volgende typen apps, maar hier wordt aan gewerkt. 

### <a name="daemonsserver-side-applications"></a>Daemons/server-side '-toepassingen

Toepassingen die langlopende processen bevatten of die werken zonder de aanwezigheid van een gebruiker moeten ook een manier om toegang te krijgen tot beveiligde bronnen, zoals web-API's. Deze toepassingen kunnen verifiëren en tokens verkrijgen met behulp van de identiteit van de toepassing (in plaats van een gedelegeerde gebruikersidentiteit) en met behulp van de OAuth 2.0-client clientreferenties-stroom. Client referentie stroom is niet hetzelfde als op namens-stroom en op namens-stroom mogen niet worden gebruikt voor verificatie van de server-naar-server.

Hoewel client referentie stroom is momenteel niet ondersteund door Azure AD B2C, kunt u de client referentie-gegevensstroom met behulp van Azure AD instellen. Een Azure AD B2C-tenant deelt sommige functies met Azure AD-enterprise tenants.  De client referentie-gegevensstroom wordt ondersteund met behulp van de Azure AD-functionaliteit van de Azure AD B2C-tenant. 

Als u de client referentie stroom instelt, Zie [Azure Active Directory v2.0 en de OAuth 2.0-client referenties stroom](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-client-creds). Een geslaagde verificatie resulteert in de ontvangst van een token dat is geformatteerd zodat deze kan worden gebruikt door Azure AD zoals beschreven in [naslaginformatie over Azure AD-tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="web-api-chains-on-behalf-of-flow"></a>Web-API-ketens (namens-stroom)

Veel architecturen bevatten een web-API die een andere downstream web-API moet aanroepen, waarbij beide zijn beveiligd door Azure AD B2C. Dit scenario is gemeenschappelijk in systeemeigen clients met een web-API-back-end. Vervolgens wordt een onlineservice van Microsoft aangeroepen, zoals de Azure AD Graph API.

Dit scenario met web-API-keten kan worden ondersteund met behulp van de OAuth 2.0 JWT bearer-referentietoekenning, ook wel de namens-stroom genoemd.  De namens-stroom is momenteel echter niet geïmplementeerd in Azure AD B2C.
