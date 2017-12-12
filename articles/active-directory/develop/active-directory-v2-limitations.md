---
title: Azure Active Directory v2.0-eindpunt en beperkingen | Microsoft Docs
description: Een lijst met voorwaarden en beperkingen voor het Azure AD v2.0-eindpunt.
services: active-directory
documentationcenter: 
author: dstrockis
manager: mtillman
editor: 
ms.assetid: a99289c0-e6ce-410c-94f6-c279387b4f66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: a81f505c189da31edb91d1b522d9f3140f821cb4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="should-i-use-the-v20-endpoint"></a>Moet ik het v2.0-eindpunt gebruiken?
Wanneer u toepassingen die zijn geïntegreerd met Azure Active Directory maakt, moet u bepalen of het v2.0-eindpunt en verificatieprotocollen aan uw behoeften voldoet. Oorspronkelijke Azure Active Directory-eindpunt wordt nog steeds volledig ondersteund en is in sommige opzichten meer uitgebreide functionaliteit dan versie 2.0. Echter, het v2.0-eindpunt [introduceert aanzienlijke voordelen](active-directory-v2-compare.md) voor ontwikkelaars.

Hier volgt de vereenvoudigde aanbeveling voor ontwikkelaars op dit moment:

* Als u een persoonlijke Microsoft-accounts in uw toepassing ondersteunen moet, gebruikt u het v2.0-eindpunt. Maar voordat u dit doet, moet u dat u begrijpt de beperkingen die in dit artikel besproken.
* Als uw toepassing alleen moet ondersteuning voor Microsoft werken en schoolaccounts, hoeft u het v2.0-eindpunt. In plaats daarvan verwijzen naar onze [Ontwikkelaarshandleiding voor Azure AD](active-directory-developers-guide.md).

Na verloop van tijd meegroeit het v2.0-eindpunt om te verwijderen van de beperkingen die hier worden vermeld, zodat u alleen ooit moet het v2.0-eindpunt te gebruiken. In de tussentijd is in dit artikel bedoeld om te bepalen of het v2.0-eindpunt geschikt voor u is. We blijven werken in dit artikel om de huidige status van het v2.0-eindpunt weer te geven. Controleer terug Herzie uw vereisten tegen v2.0-mogelijkheden.

Als er een bestaande Azure AD-app die het v2.0-eindpunt niet wordt gebruikt, hoeft niet te starten vanaf het begin. In de toekomst, bieden we een manier om uw bestaande Azure AD-toepassingen gebruiken met het v2.0-eindpunt.

## <a name="restrictions-on-app-types"></a>Beperkingen van app-typen
De volgende typen apps worden momenteel niet ondersteund door het v2.0-eindpunt. Zie voor een beschrijving van de ondersteunde app-typen [App-typen voor de Azure Active Directory v2.0-eindpunt](active-directory-v2-flows.md).

### <a name="standalone-web-apis"></a>Zelfstandige Web-API 's
U kunt het v2.0-eindpunt op [bouwen van een Web-API die is beveiligd met OAuth 2.0](active-directory-v2-flows.md#web-apis). Echter kan die Web-API tokens alleen ontvangen van een toepassing met de dezelfde toepassing-ID. U geen toegang tot een Web-API van een client met een andere toepassing. De client niet mogelijk aan te vragen of machtigingen voor uw Web-API te verkrijgen.

Als u wilt zien hoe u een Web-API die tokens accepteert van een client met dezelfde toepassings-ID, Zie het v2.0-eindpunt Web API-voorbeelden in onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.

## <a name="restrictions-on-app-registrations"></a>Beperkingen van app-registraties
Op dit moment voor elke app die u wilt integreren met het v2.0-eindpunt, moet u een app-registratie in het nieuwe [Portal voor registratie van Microsoft-toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList). Bestaande Azure AD of apps voor Microsoft-account zijn niet compatibel met het v2.0-eindpunt. Apps die zijn geregistreerd in een portal dan de Registratieportal van toepassing zijn niet compatibel met het v2.0-eindpunt. We zullen in de toekomst bieden een manier om een bestaande toepassing gebruiken als een v2.0-app. Op dit moment maar is er geen migratiepad voor een bestaande app werkt met het v2.0-eindpunt.

Bovendien app registraties die u maakt in de [Registratieportal toepassing](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) hebben het volgende voorbehoud:

* Twee appgeheimen toegestaan per aanvraag-ID.
* Een app-registratie geregistreerd door een gebruiker met een persoonlijk Microsoft-account kan worden bekeken en beheerd alleen door een enkele ontwikkelaarsaccount. Deze kan niet worden gedeeld tussen meerdere ontwikkelaars.  Als u delen van uw app-registratie in meerdere ontwikkelaars wilt, kunt u de toepassing wanneer u zich aanmeldt bij de registratieportal met een Azure AD-account maken.
* Er zijn enkele beperkingen van de indeling van de omleidings-URI die is toegestaan. Zie de volgende sectie voor meer informatie over omleidings-URI's.

## <a name="restrictions-on-redirect-uris"></a>Beperkingen voor omleidings-URI 's
Apps die zijn geregistreerd in de Portal voor Wachtwoordregistratie van toepassing zijn op dit moment is beperkt tot een beperkt aantal omleidings-URI-waarden. Omleidings-URI voor web-apps en services met het schema beginnen moet de `https`, en alle omleidings-URI-waarden moeten delen één DNS-domein. U kunt bijvoorbeeld een web-app met een van deze omleidings-URI's niet registreren:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Het registratiesysteem vergelijkt de volledige DNS-naam van de bestaande omleidings-URI op de DNS-naam van de omleidings-URI die u wilt toevoegen. De aanvraag voor het toevoegen van de DNS-naam mislukt als aan een van de volgende voorwaarden wordt voldaan:  

* De volledige DNS-naam van de nieuwe omleidings-URI komt niet overeen met de DNS-naam van de bestaande omleidings-URI.
* De volledige DNS-naam van de nieuwe omleidings-URI is niet een subdomein van de bestaande omleidings-URI.

Als bijvoorbeeld de app heeft deze omleidings-URI:

`https://login.contoso.com`

Dan kunt u er als volgt aan toevoegen:

`https://login.contoso.com/new`

In dit geval komt de DNS-naam precies overeen. Of u kunt dit doen:

`https://new.login.contoso.com`

In dit geval verwijst u naar een DNS-subdomein van login.contoso.com. Als u een app hebt die aanmelding east.contoso.com en aanmelding-west.contoso.com wilt als omleidings-URI's heeft, moet u toevoegen die omleidings-URI's in deze volgorde:

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

U kunt de laatste twee toevoegen, omdat ze subdomeinen van de eerste omleidings-URI, contoso.com. Deze beperking wordt verwijderd in een toekomstige release.

Vergeet, kunt u maximaal 20 antwoord-URL's voor een bepaalde toepassing hebben.

Zie voor informatie over het registreren van een app in de Portal van de registratie van de toepassing, [het registreren van een app met het v2.0-eindpunt](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services-and-apis"></a>Beperkingen voor de API's en services
Op dit moment is het v2.0-eindpunt ondersteunt aanmelden voor een app die is geregistreerd in de Portal van de registratie van de toepassing en die vallen in de lijst met [verificatie stroomt ondersteund](active-directory-v2-flows.md). Deze apps kunnen echter OAuth 2.0-toegangstokens voor een zeer beperkt aantal resources aanschaffen. Tokens alleen voor toegang tot de problemen met de v2.0-eindpunt:

* De app die het token aangevraagd. Een app kunt verkrijgen een toegangstoken voor zichzelf, als de logische app uit diverse verschillende onderdelen of lagen bestaat. Overzicht van dit scenario werkt, Bekijk onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) zelfstudies.
* De Outlook-e-Mail, agenda en contactpersonen REST-API's, die allemaal zich op https://outlook.office.com bevinden. Zie voor informatie over het schrijven van een app die toegang heeft tot deze API's, de [Office aan de slag](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) zelfstudies.
* Microsoft Graph API's. U kunt meer lezen over [Microsoft Graph](https://graph.microsoft.io) en de gegevens die voor u beschikbaar is.

Er zijn geen andere services worden ondersteund op dit moment. Meer Microsoft Online Services worden in de toekomst toegevoegd naast ondersteuning voor uw eigen op maat gemaakte Web-API's en services.

## <a name="restrictions-on-libraries-and-sdks"></a>Beperkingen voor bibliotheken en SDK 's
Ondersteuning voor het v2.0-eindpunt is momenteel beperkt. Als u het v2.0-eindpunt in een productietoepassing gebruiken wilt, hebt u deze opties:

* Als u een webtoepassing maakt, kunt u veilig Microsoft algemeen beschikbaar serverzijde middleware gebruiken voor aanmelden en token. Hierbij wordt de OWIN Open ID Connect middleware voor ASP.NET en de Node.js Passport-invoegtoepassing. Zie voor codevoorbeelden die gebruikmaken van Microsoft-middleware onze [aan de slag](active-directory-appmodel-v2-overview.md#getting-started) sectie.
* Als u een toepassing desktop of mobile bouwt, kunt u een van onze preview Microsoft Authentication Libraries (MSAL).  Deze bibliotheken zijn in een voorbeeld van productie ondersteund, dus veilig worden ze in productietoepassingen gebruiken. U kunt meer lezen over de voorwaarden van de Preview-versie en de beschikbare bibliotheken in onze [verificatie-bibliotheken verwijzing](active-directory-v2-libraries.md).
* Voor platforms die niet wordt gedekt door een Microsoft-bibliotheken, kunt u integreren met het v2.0-eindpunt door rechtstreeks verzenden en ontvangen van protocolberichten in uw toepassingscode. De v2.0 OpenID Connect en OAuth-protocollen [expliciet worden gedocumenteerd](active-directory-v2-protocols.md) om u te helpen bij het uitvoeren van een dergelijke integratie.
* Ten slotte kunt u open source Open ID Connect en OAuth-bibliotheken integreren met het v2.0-eindpunt. Het v2.0-protocol moet compatibel zijn met veel open-source-protocol bibliotheken zonder grote wijzigingen. De beschikbaarheid van dit soort bibliotheken varieert per taal en het platform. De [Open ID Connect](http://openid.net/connect/) en [OAuth 2.0](http://oauth.net/2/) websites een lijst met populaire implementaties bijhouden. Zie voor meer informatie [Azure Active Directory-verificatie en v2.0 bibliotheken](active-directory-v2-libraries.md), en de lijst met open source-clientbibliotheken en voorbeelden die zijn getest met het v2.0-eindpunt.

## <a name="restrictions-on-protocols"></a>Beperkingen voor protocollen
Het v2.0-eindpunt biedt geen ondersteuning voor SAML- of WS-Federation; ondersteunt alleen Open ID Connect en OAuth 2.0.  Niet alle functies en mogelijkheden van de OAuth-protocollen zijn opgenomen in het v2.0-eindpunt. Deze protocol functies en mogelijkheden zijn momenteel *niet beschikbaar* in het v2.0-eindpunt:

* ID-tokens die zijn uitgegeven door het v2.0-eindpunt bevatten geen een `email` claim voor de gebruiker, zelfs als u toestemming van de gebruiker om hun e-mailbericht weer te geven aan te schaffen.
* Het OpenID Connect gebruikersgegevens-eindpunt is niet geïmplementeerd op het v2.0-eindpunt. Alle gebruikersprofielgegevens die u mogelijk zou ontvangen op dit eindpunt is echter beschikbaar via Microsoft Graph `/me` eindpunt.
* Het v2.0-eindpunt biedt geen ondersteuning voor claims van verlenende functie of groep in de ID-tokens.
* De [OAuth 2.0 Resource eigenaar wachtwoord referenties Grant](https://tools.ietf.org/html/rfc6749#section-4.3) wordt niet ondersteund door het v2.0-eindpunt.

Het v2.0-eindpunt biedt bovendien geen ondersteuning voor elke vorm van het SAML of WS-Federation-protocol.

Lees voor meer informatie over het bereik van het protocol functionaliteit ondersteund in het v2.0-eindpunt, onze [OpenID Connect en OAuth 2.0-protocol verwijzing](active-directory-v2-protocols.md).

## <a name="restrictions-for-work-and-school-accounts"></a>Beperkingen voor werk-en schoolaccounts
Als u Active Directory Authentication Library (ADAL) in Windows-toepassingen hebt gebruikt, hebt u mogelijk gebruikmaken van geïntegreerde Windows-verificatie, waarbij de Security Assertion Markup Language (SAML) assertion grant ondernomen. Met deze machtiging federatieve gebruikers van Azure AD tenants kunnen achtergrond verifiëren met hun lokale Active Directory-exemplaar zonder referenties invoeren. De SAML-verklaring grant wordt momenteel niet ondersteund op het v2.0-eindpunt.
