---
title: Azure Active Directory-belangrijke wijzigingen referentie | Microsoft Docs
description: Meer informatie over wijzigingen in de Azure AD-protocollen die mogelijk van invloed op uw toepassing.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3f4a04f1598b3ab0efd9ff95a707d3837bb37503
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56196022"
---
# <a name="whats-new-for-authentication"></a>Wat is er nieuw voor verificatie? 

>Blijf op de hoogte over updates voor deze pagina. Alleen toe te voegen [deze URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) op uw RSS-feed lezer.

De verificatiesysteem wordt gewijzigd en voegt voortdurend te verbeteren, beveiliging en naleving van standaarden. Om te blijven up-to-date zijn met de meest recente ontwikkelingen, vindt in dit artikel u informatie over de volgende details:

- Meest recente functies
- Bekende problemen
- Protocol wijzigingen
- Afgeschafte functies

> [!TIP] 
> Deze pagina wordt regelmatig bijgewerkt, dus vaak bezoekt. Tenzij anders vermeld, worden deze wijzigingen alleen plaats voor zojuist geregistreerde toepassingen.  

## <a name="upcoming-changes"></a>Toekomstige wijzigingen

Geen op dit moment is gepland. 

## <a name="october-2018"></a>Oktober 2018

### <a name="authorization-codes-can-no-longer-be-reused"></a>Autorisatiecodes niet meer opnieuw kunnen worden gebruikt

**Ingangsdatum**: 15 november 2018

**Eindpunten beïnvloed**: V1.0 zowel v2.0

**Protocol beïnvloed**: [De stroom](v2-oauth2-auth-code-flow.md)

Vanaf 15 November 2018, Azure AD wordt niet meer accepteren van eerder gebruikte verificatie codes voor apps. Deze wijziging in de beveiliging zorgt ervoor dat Azure AD in overeenstemming met de OAuth-specificatie brengen en worden afgedwongen op de v1- en v2-eindpunten.

Als uw app wordt gebruikgemaakt van autorisatiecodes om op te halen van tokens voor meerdere bronnen, raden wij u gebruik van de code om op te halen van een vernieuwingstoken en gebruikt vervolgens die vernieuwingstoken om te verkrijgen van aanvullende tokens voor andere resources. Autorisatiecodes kunnen slechts eenmaal worden gebruikt, maar vernieuwen van tokens kunnen meerdere keren worden gebruikt in meerdere resources. Een nieuwe app waarmee wordt geprobeerd om een verificatiecode op te geven tijdens de OAuth-codestroom opnieuw te gebruiken krijgt een foutmelding invalid_grant zijn.

Zie voor meer informatie over het vernieuwen van tokens [vernieuwen van de toegangstokens](v1-protocols-oauth-code.md#refreshing-the-access-tokens).  Als u ADAL of MSAL, dit voor u wordt verwerkt door de bibliotheek - vervangen door de tweede instantie van 'AcquireTokenByAuthorizationCodeAsync' met 'AcquireTokenSilentAsync'. 

## <a name="may-2018"></a>Mei 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>ID-tokens kunnen niet worden gebruikt voor de stroom OBO

**Datum**: Vanaf 1 mei 2018

**Eindpunten beïnvloed**: V1.0 zowel v2.0

**Protocollen beïnvloed**: Impliciete stroom en [OBO stroom](v1-oauth2-on-behalf-of-flow.md)

Na 1 mei 2018 kan niet id_tokens worden gebruikt als de verklaring in een stroom OBO voor nieuwe toepassingen. Toegangstokens moeten in plaats daarvan worden gebruikt voor het beveiligen van API's, zelfs tussen een client en de middelste laag van dezelfde toepassing. Apps die zijn geregistreerd voor 1 mei 2018 blijven werken en kunnen voor het uitwisselen van id_tokens voor een toegangstoken; Dit patroon wordt echter niet beschouwd als een best practice.

Als tijdelijke oplossing voor deze wijziging, kunt u het volgende doen:

1. Maak een Web-API voor uw toepassing, met een of meer scopes. Dit beginpunt expliciete kunt wilt korrelig beheer en beveiliging.
1. In het manifest van uw app, in de [Azure-portal](https://portal.azure.com) of de [portal voor app-registratie](https://apps.dev.microsoft.com), zorg ervoor dat de app om uit te geven van tokens voor toegang via de impliciete stroom is toegestaan. Dit wordt geregeld via de `oauth2AllowImplicitFlow` sleutel.
1. Wanneer u de clienttoepassing aanvragen voor een id_token via `response_type=id_token`, ook vragen een toegangstoken (`response_type=token`) voor de Web-API die eerder is gemaakt. Dus bij het gebruik van het v2.0-eindpunt de `scope` parameter moet er ongeveer als volgt `api://GUID/SCOPE`. Op het eindpunt v1.0 de `resource` parameter moet de URI van de web-API-app.
1. Dit toegangstoken aan de middelste laag in plaats van het id_token doorgeven.  
