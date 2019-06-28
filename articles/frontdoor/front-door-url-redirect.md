---
title: Azure Front deur Service - URL-omleiding | Microsoft Docs
description: In dit artikel helpt u begrijpen hoe Azure voordeur Service biedt ondersteuning voor URL-omleiding voor hun routes als geconfigureerd.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2019
ms.author: sharadag
ms.openlocfilehash: 3d77a16d24a1a843b39d97904a675518c43a525a
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332522"
---
# <a name="url-redirect"></a>URL-omleiding
U kunt Azure voordeur Service omleiden van verkeer. U kunt omleiden van verkeer op meerdere niveaus (protocol, de hostnaam, pad, query-tekenreeks) en alle functionaliteit voor afzonderlijke microservices kunnen worden geconfigureerd als de omleiding vindt plaats op basis van een pad. Dit vereenvoudigt de configuratie van toepassing, optimaliseert het gebruik van bronnen en biedt ondersteuning voor nieuwe omleiding-scenario's, met inbegrip van omleiding van globale en op basis van een pad.
</br>

![Azure voordeur URL-omleiding][1]

## <a name="redirection-types"></a>Type omleiding
Een omleidings-type stelt de antwoordstatuscode voor de clients leert wat het doel van de omleiding. De volgende typen omleiding worden ondersteund:

- **301 (definitief verplaatst)** : Geeft aan dat de doelresource een nieuwe, permanente URI is toegewezen en toekomstige verwijzingen naar deze bron zou moeten een van de ingesloten URI's te gebruiken. 301 statuscode gebruiken voor HTTP naar HTTPS-omleiding. 
- **302 (Found)** : Geeft aan dat de doelresource tijdelijk bevindt zich onder een andere URI. Omdat de omleiding kan soms worden gewijzigd, moet de client om door te gaan met de effectieve aanvraag-URI voor toekomstige aanvragen.
- **307 (tijdelijke omleidings)** : Geeft aan dat de doelresource tijdelijk bevindt zich onder een andere URI en de gebruikersagent mag niet de aanvraagmethode wijzigen als het een automatische omleiding naar deze URI wordt uitgevoerd. Omdat de omleiding na verloop van tijd veranderen kan, moet de client om door te gaan met behulp van de oorspronkelijke effectieve aanvraag-URI voor toekomstige aanvragen.
- **308 (permanente omleiding)** : Geeft aan dat de doelresource een nieuwe, permanente URI is toegewezen en toekomstige verwijzingen naar deze bron zou moeten een van de ingesloten URI's te gebruiken. Clients met koppeling bewerkingsmogelijkheden, zou als automatisch opnieuw koppelen verwijzingen naar de effectieve aanvraag-URI aan een of meer van de nieuwe referenties verzonden door de server, waar mogelijk.

## <a name="redirection-protocol"></a>Omleiding protocol
U kunt het protocol dat wordt gebruikt voor omleiding instellen. Hiermee wordt voor een van de meest voorkomende gebruiksvoorbeelden van omleidings-functie, die is het instellen van HTTP naar HTTPS-omleiding.

- **Alleen HTTPS**: Stel het protocol naar HTTPS alleen als u wilt omleiden van het verkeer van HTTP naar HTTPS. Azure voordeur-Service wordt aanbevolen dat u moet altijd ingesteld het omleiden naar HTTPS alleen.
- **Alleen HTTP**: Dit leidt de inkomende aanvraag naar HTTP. Deze waarde alleen als u wilt behouden van uw verkeer van HTTP, niet-versleutelde gebruiken.
- **Overeenkomst aanvraag**: Deze optie wordt het protocol dat wordt gebruikt door de inkomende aanvraag bewaard. Dus een HTTP-aanvraag HTTP blijft en een HTTPS-aanvraag blijft post-omleiding HTTPS.

## <a name="destination-host"></a>Doelhost
Als onderdeel van het configureren van een omleidings-routering, kunt u ook de hostnaam of het domein voor de aanvraag voor omleiding wijzigen. U kunt dit veld wijzigen van de hostnaam in de URL voor de omleiding of anders behouden de hostnaam van de inkomende aanvraag instellen. Ja, met behulp van dit veld kunt u omleiden alle aanvragen die worden verzonden op https://www.contoso.com/ * naar https://www.fabrikam.com/ *.

## <a name="destination-path"></a>Doelpad
Voor het geval waar u het padsegment van een URL vervangen als onderdeel van omleiding, kunt u dit veld met het nieuwe padwaarde instellen. U kunt anders de padwaarde behouden als onderdeel van omleiding. Dus met behulp van dit veld, u kunt alle aanvragen omleiden naar verzonden https://www.contoso.com/ * naar https://www.contoso.com/redirected-site.

## <a name="query-string-parameters"></a>Queryreeksparameters
U kunt ook de queryreeksparameters in de omgeleide URL vervangen. Als u wilt vervangen door een bestaande query-tekenreeks van de binnenkomende aanvraag-URL, stelt u dit veld aan 'Replace' en stel de juiste waarde. Anders kunt u de oorspronkelijke reeks queryreeksen behouden door in te stellen van het veld 'Preserve'. Een voorbeeld: met behulp van dit veld, u kunt omleiden al het verkeer verzonden naar https://www.contoso.com/foo/bar naar https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F. 

## <a name="destination-fragment"></a>Fragment van doel
Het fragment bestemming is het gedeelte van de URL na '#', gebruikt normaal gesproken door browsers op een specifieke sectie op een pagina. U kunt dit veld een fragment toevoegen aan de omleidings-URL instellen.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).

<!--Image references-->
[1]: ./media/front-door-url-redirect/front-door-url-redirect.png