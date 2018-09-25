---
title: Azure Active Directory-belangrijke wijzigingen referentie | Microsoft Docs
description: Wijzigingen in de Azure AD-protocollen die mogelijk van invloed op uw toepassing.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 14217d03cdb56c5c641ab8f8c3fc0748e8e815a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987753"
---
# <a name="whats-new-for-authentication"></a>Wat is er nieuw voor verificatie? 

|
>Blijf op de hoogte over wanneer u terug naar deze pagina voor updates door toe te voegen deze [URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) op uw RSS-feed lezer.

De verificatiesysteem wordt gewijzigd en voegt regelmatig om de beveiliging verbeteren en het verbeteren van naleving van standaarden. Als u wilt bijhouden met de meest recente ontwikkelingen, vindt in dit artikel u informatie over:

- De nieuwste functies
- Bekende problemen
- Protocol wijzigingen
- Afgeschafte functies

> [!TIP] 
> Deze pagina wordt regelmatig bijgewerkt, dus bezoeken. Tenzij anders vermeld, worden deze wijzigingen alleen plaats voor zojuist geregistreerde toepassingen.  

## <a name="upcoming-changes"></a>Toekomstige wijzigingen

### <a name="authorization-codes-can-no-longer-be-reused"></a>Autorisatiecodes niet meer opnieuw kunnen worden gebruikt

**Ingangsdatum**: 10 oktober 2018 **eindpunten beïnvloed**: zowel v1.0 en v2.0 **Protocol beïnvloed**: [flow-Code](v2-oauth2-auth-code-flow.md)

Vanaf 10 oktober 2018, meer Azure AD accepteert eerder gebruikt verificatie codes voor nieuwe apps. Alle Apps die zijn gemaakt vóór 10 oktober 2018 wordt nog steeds mogelijk om verificatie codes opnieuw te gebruiken. Deze wijziging in de beveiliging zorgt ervoor dat Azure AD in overeenstemming met de OAuth-specificatie brengen en worden afgedwongen op de v1- en v2-eindpunten.

Als uw app wordt gebruikgemaakt van autorisatiecodes om op te halen van tokens voor meerdere bronnen, raden wij u gebruik van de code om op te halen van een vernieuwingstoken en gebruikt vervolgens die vernieuwingstoken om te verkrijgen van aanvullende tokens voor andere resources. Autorisatiecodes kunnen slechts eenmaal worden gebruikt, maar vernieuwen van tokens kunnen meerdere keren worden gebruikt in meerdere resources. Een nieuwe app waarmee wordt geprobeerd om een verificatiecode op te geven tijdens de OAuth-codestroom opnieuw te gebruiken krijgt een foutmelding invalid_grant zijn.

Zie voor meer informatie over het vernieuwen van tokens [vernieuwen van de toegangstokens](v1-protocols-oauth-code.md#refreshing-the-access-tokens).

## <a name="may-2018"></a>Mei 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>ID-tokens kunnen niet worden gebruikt voor de stroom OBO

**Datum**: 1 mei 2018 **eindpunten beïnvloed**: zowel v1.0 en v2.0 **protocollen beïnvloed**: impliciete stroom en [OBO stroom](v1-oauth2-on-behalf-of-flow.md)

Na 1 mei 2018 kan niet id_tokens worden gebruikt als de verklaring in een stroom OBO voor nieuwe toepassingen.  Toegangstokens moeten in plaats daarvan worden gebruikt voor het beveiligen van API's, zelfs tussen een client en de middelste laag van dezelfde toepassing.  Apps die zijn geregistreerd voor 1 mei 2018 blijven werken en kunnen voor het uitwisselen van id_tokens voor een toegangstoken - maar dit wordt niet beschouwd als een best practice.

Om te kunnen werken om deze wijziging, kunt u het volgende doen:

1. Maak een Web-API voor de middelste laag-toepassing, met een of meer scopes.  Hierdoor kunnen wilt korrelig beheer en beveiliging.
1. In het manifest van uw app, de [Azure Portal](https://portal.azure.com), of de [Portal voor App-registratie](https://apps.dev.microsoft.com), zorg ervoor dat de app om uit te geven van tokens voor toegang via de impliciete stroom is toegestaan. Dit wordt geregeld via de `oauth2AllowImplicitFlow` sleutel.
1. Wanneer u de clienttoepassing aanvragen voor een id_token via `response_type=id_token`, ook vragen een toegangstoken (`response_type=token`) voor de Web-API die eerder is gemaakt.  Dus bij het gebruik van het v2.0-eindpunt de `scope` parameter moet er ongeveer als volgt `api://GUID/SCOPE`.  Op het eindpunt v1.0 de `resource` parameter moet de URI van de web-API-app.
1. Dit toegangstoken aan de middelste laag in plaats van het id_token doorgeven.  