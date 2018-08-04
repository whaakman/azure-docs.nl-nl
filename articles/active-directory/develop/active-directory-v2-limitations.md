---
title: Azure Active Directory v2.0-eindpunt voorwaarden en beperkingen | Microsoft Docs
description: Een lijst met voorwaarden en beperkingen voor het Azure AD v2.0-eindpunt.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: celested
ms.reviewer: hirsin, dastrock
ms.custom: aaddev
ms.openlocfilehash: 4111174e7ce000a978a7a5280a2561897500bdb6
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505691"
---
# <a name="should-i-use-the-v20-endpoint"></a>Moet ik het v2.0-eindpunt gebruiken?

Wanneer u toepassingen die zijn geïntegreerd met Azure Active Directory (Azure AD) bouwt, moet u bepalen of het v2.0-eindpunt en de verificatieprotocollen voldoen aan uw behoeften. Oorspronkelijke van Azure AD-eindpunt nog steeds volledig wordt ondersteund en is in sommige opzichten meer uitgebreide functionaliteit dan versie 2.0. Echter, het v2.0-eindpunt [kennismaakt met aanzienlijke voordelen](active-directory-v2-compare.md) voor ontwikkelaars.

Dit is een vereenvoudigde aanbeveling voor ontwikkelaars op dit moment:

* Als in uw toepassing, u persoonlijke Microsoft-accounts ondersteunen moet, gebruikt u het v2.0-eindpunt. Maar voordat u dit doet, moet u dat u begrijpt dat de beperkingen die in dit artikel worden besproken.
* Als uw toepassing alleen moet ondersteuning voor Microsoft werk en schoolaccounts, hoeft u het v2.0-eindpunt. In plaats daarvan verwijzen naar de [ontwikkelaarsgids van Azure AD](azure-ad-developers-guide.md).

Het v2.0-eindpunt wordt aangepast om te voorkomen van de beperkingen die hier worden vermeld, zodat u alleen ooit moet het v2.0-eindpunt gebruiken. In de tussentijd Gebruik dit artikel om te bepalen of het v2.0-eindpunt bij u past. We blijven om bij te werken in dit artikel om de huidige status van het v2.0-eindpunt weer te geven. Controleer terug Evalueer uw vereisten op basis van v2.0-mogelijkheden.

Hebt u een bestaande Azure AD-app die het v2.0-eindpunt niet worden gebruikt, is er niet nodig om u te maken. In de toekomst, bieden we een manier waarop u uw bestaande Azure AD-toepassingen gebruiken met het v2.0-eindpunt.

## <a name="restrictions-on-app-types"></a>Beperkingen voor app-typen

De volgende typen apps zijn op dit moment niet ondersteund door het v2.0-eindpunt. Zie voor een beschrijving van de ondersteunde app-typen, [App-typen voor de Azure Active Directory v2.0-eindpunt](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Zelfstandige Web-API 's

U kunt het v2.0-eindpunt naar [bouwen van een Web-API die wordt beveiligd met OAuth 2.0](active-directory-v2-flows.md#web-apis). Deze Web-API kan echter tokens ontvangen alleen vanuit een toepassing met de dezelfde toepassings-ID. U geen toegang tot een Web-API van een client met een andere toepassings-ID. De client niet mogelijk om te vragen of te verkrijgen van machtigingen voor uw Web-API.

Als u wilt zien over het bouwen van een Web-API die tokens accepteert van een client met dezelfde toepassings-ID, raadpleegt u de voorbeelden v2.0-eindpunt Web-API in de [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

## <a name="restrictions-on-app-registrations"></a>Beperkingen voor app-registraties

Op dit moment voor elke app die u wilt integreren met het v2.0-eindpunt, moet u een app-registratie in de nieuwe [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Bestaande Azure AD of apps in Microsoft-account zijn niet compatibel met het v2.0-eindpunt. Apps die zijn geregistreerd in een portal dan de Portal voor Appregistratie zijn niet compatibel met het v2.0-eindpunt. In de toekomst, willen we bieden een manier om een bestaande toepassing gebruiken als een v2.0-app. Er is momenteel geen migratiepad voor een bestaande app om te werken met het v2.0-eindpunt.

Daarnaast kunnen app-registraties die u maakt in de [Portal voor Appregistratie](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) hebben het volgende voorbehoud:

* Twee appgeheimen zijn toegestaan per toepassings-ID.
* Een app-registratie geregistreerd door een gebruiker met een persoonlijk Microsoft-account kan worden bekeken en beheerd alleen door een enkele developer-account. Het kan niet worden gedeeld tussen meerdere ontwikkelaars. Als u delen van de registratie van uw app met meerdere ontwikkelaars wilt, kunt u de toepassing als u zich aanmeldt bij de registratieportal met een Azure AD-account maken.
* Er zijn enkele beperkingen op de indeling van de omleidings-URI die is toegestaan. Zie de volgende sectie voor meer informatie over omleidings-URI's.

## <a name="restrictions-on-redirect-uris"></a>Beperkingen van de omleidings-URI 's

Apps die zijn geregistreerd in de Portal voor Appregistratie zijn beperkt tot een beperkt aantal omleidings-URI-waarden. De omleidings-URI voor de web-apps en services met het schema beginnen moet `https`, en alle omleidings-URI-waarden moeten één DNS-domein delen. U kunt bijvoorbeeld een web-app met een van deze omleidings-URI's niet registreren:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Het registratiesysteem vergelijkt de volledige DNS-naam van de bestaande omleidings-URI op de DNS-naam van de omleidings-URI die u wilt toevoegen. De aanvraag voor het toevoegen van de DNS-naam mislukt als aan een van de volgende voorwaarden wordt voldaan:  

* De volledige DNS-naam van de nieuwe omleidings-URI komt niet overeen met de DNS-naam van de bestaande omleidings-URI.
* De volledige DNS-naam van de nieuwe omleidings-URI is niet een subdomein van de bestaande omleidings-URI.

Bijvoorbeeld, als de app heeft deze omleidings-URI:

`https://login.contoso.com`

Dan kunt u er als volgt aan toevoegen:

`https://login.contoso.com/new`

In dit geval komt de DNS-naam precies overeen. Of u kunt dit doen:

`https://new.login.contoso.com`

In dit geval verwijst u naar een DNS-subdomein van login.contoso.com. Als u een app hebt die login-east.contoso.com en login-west.contoso.com wilt als omleidings-URI's heeft, moet u de omleidings-URI's in deze volgorde toevoegen:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste omleidings-URI, contoso.com. Deze beperking wordt verwijderd in een toekomstige release.

Let ook op, u kunt maximaal 20 antwoord-URL's voor een bepaalde toepassing hebben.

Zie voor meer informatie over het registreren van een app in de Portal voor Appregistratie [over het registreren van een app met het v2.0-eindpunt](quickstart-v2-register-an-app.md).

## <a name="restrictions-on-libraries-and-sdks"></a>Beperkingen voor bibliotheken en SDK 's

Ondersteuning voor clientbibliotheek voor het v2.0-eindpunt is momenteel beperkt. Als u het v2.0-eindpunt in een productietoepassing gebruiken wilt, hebt u deze opties:

* Als u een webtoepassing bouwt, kunt u veilig Microsoft algemeen verkrijgbaar serverzijde middleware gebruiken om uit te voeren, aanmelden en token-validatie. Hieronder vallen de OWIN Open ID Connect-middleware voor ASP.NET en de Node.js Passport-invoegtoepassing. Zie voor voorbeelden van code die gebruikmaken van Microsoft-middleware, de [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.
* Als u een bureaublad of mobiele toepassing ontwikkelt, kunt u een van de Preview-versie van Microsoft Authentication Libraries (MSAL). Deze bibliotheken zijn in een productie-ondersteunde preview, dus is het veilig om ze in productie-Apps gebruiken. U kunt meer lezen over de voorwaarden van de Preview-versie en de beschikbare bibliotheken in [verificatie-bibliotheken verwijzing](active-directory-v2-libraries.md).
* Voor platforms die niet wordt gedekt door Microsoft-bibliotheken, kunt u integreren met het v2.0-eindpunt door rechtstreeks verzenden en ontvangen van protocolberichten in uw toepassingscode. De OpenID Connect en OAuth-protocollen van v2.0 [expliciet worden gedocumenteerd](active-directory-v2-protocols.md) om u te helpen bij het uitvoeren van een dergelijke integratie.
* Ten slotte kunt u Open ID Connect en OAuth voor open source-bibliotheken om te integreren met het v2.0-eindpunt. Het v2.0-protocol moet compatibel zijn met veel open-source-protocol bibliotheken zonder ingrijpende wijzigingen. De beschikbaarheid van dit soort bibliotheken varieert per taal en platform. De [Open ID Connect](http://openid.net/connect/) en [OAuth 2.0](http://oauth.net/2/) websites een lijst met populaire implementaties bijhouden. Zie voor meer informatie, [Azure Active Directory v2.0 en verificatie bibliotheken](active-directory-v2-libraries.md), en de lijst met open source-clientbibliotheken en voorbeelden die zijn getest met het v2.0-eindpunt.

## <a name="restrictions-on-protocols"></a>Beperkingen voor protocollen

Het v2.0-eindpunt biedt geen ondersteuning voor SAML of WS-Federation; het ondersteunt alleen Open ID Connect en OAuth 2.0. Niet alle functies en mogelijkheden van OAuth-protocollen zijn opgenomen in het v2.0-eindpunt.

De volgende protocol-functies en mogelijkheden zijn momenteel *niet beschikbaar* in het v2.0-eindpunt:

* Op dit moment de `email` claim wordt alleen geretourneerd als een optionele claim is geconfigureerd en bereik scope = e-mailbericht is opgegeven in de aanvraag. Dit gedrag wijzigen, zoals het v2.0-eindpunt is bijgewerkt om verder te voldoen aan de Open ID Connect en OAuth 2.0-standaarden.
* Het eindpunt van de OpenID verbinding maken met gebruikersgegevens is niet geïmplementeerd op het v2.0-eindpunt. Alle gebruikersprofielgegevens die u mogelijk wilt ontvangen op dit eindpunt is echter beschikbaar is via de Microsoft Graph `/me` eindpunt.
* Het v2.0-eindpunt biedt geen ondersteuning voor claims van verlenende rol of groep in het ID-tokens.
* De [clientreferenties van OAuth 2.0-Resource-eigenaar wachtwoord](https://tools.ietf.org/html/rfc6749#section-4.3) wordt niet ondersteund door het v2.0-eindpunt.

Het v2.0-eindpunt biedt bovendien geen ondersteuning voor een vorm van het SAML- of WS-Federation-protocol.

Lees voor meer informatie over het bereik van protocol-functionaliteit worden ondersteund in het v2.0-eindpunt, onze [OpenID Connect en OAuth 2.0-protocol verwijzing](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>Beperkingen voor werk-en schoolaccounts

Als u Active Directory Authentication Library (ADAL) in Windows-toepassingen hebt gebruikt, hebt u mogelijk gebruikmaken van Windows geïntegreerde verificatie, die gebruikmaakt van de Security Assertion Markup Language (SAML) verklaring toekenning geleid. Met deze machtiging federatieve gebruikers van Azure AD tenants kunnen op de achtergrond verifiëren met hun on-premises Active Directory-exemplaar zonder referenties in te voeren. De SAML-verklaring toekenning is momenteel niet ondersteund op het v2.0-eindpunt.
