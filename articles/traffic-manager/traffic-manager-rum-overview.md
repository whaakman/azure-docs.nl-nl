---
title: Echte gebruiker metingen in Azure Traffic Manager | Microsoft Docs
description: Inleiding tot de werkelijke gebruiker metingen in Traffic Manager
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: a7e8ae605b6234341d9ab8b790f4c54d8627f29f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Traffic Manager echte gebruiker metingen overzicht

>[!NOTE]
>De functie echte gebruiker metingen in Traffic Manager kan is openbare preview-versie en geen hetzelfde niveau van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Traffic Manager-updates](https://azure.microsoft.com/updates/?product=traffic-manager) pagina.

Bij het instellen van een Traffic Manager-profiel gebruiken, de routeringsmethode voor prestaties, de service wordt bekeken waarbij de DNS-queryaanvragen afkomstig zijn uit en maakt Routeringsbeslissingen om te verwijzen die aanvragers de Azure-regio waarmee ze de laagste latentie. Dit wordt bereikt door het gebruik van het netwerk latentie intelligence die Traffic Manager voor verschillende eindgebruiker netwerken onderhoudt.

Echte metingen van de gebruiker kunt u voor het meten van netwerk latentie metingen naar Azure-regio's vanuit de clienttoepassingen die uw eindgebruikers gebruiken, en hebben Traffic Manager rekening houden die informatie ook bij het Routeringsbeslissingen. U gebruikt de echte metingen van de gebruiker, kunt u de nauwkeurigheid van de routering voor aanvragen die afkomstig zijn van deze netwerken waar uw eindgebruikers zich bevinden verhogen. 

## <a name="how-real-user-measurements-work"></a>De werking van echte metingen van de gebruiker

Echte gebruiker metingen werken omdat uw client toepassingen meting latentie hoeft te Azure-regio's, zoals deze wordt weergegeven van de eindgebruiker netwerken waarin ze worden gebruikt. Een voorbeeld: als u een webpagina die wordt geopend door gebruikers op verschillende locaties (bijvoorbeeld in de Noord-Amerika regio's), kunt u gebruikmaken van de kracht van echte metingen van de gebruiker wanneer u routeringsmethode voor prestaties aan om ze het beste Azure-regio in die de servertoepassing wordt gehost.

Deze begint met het insluiten van een Azure opgegeven JavaScript (met een unieke sleutel in het) in uw webpagina's. Zodra u dat hebt gedaan, wanneer een gebruiker die webpagina bezoekt, vraagt de JavaScript Traffic Manager voor informatie over Azure-regio's die deze dient te waarderen. De service retourneert een set van eindpunten aan het script, die vervolgens meting deze regio's achter elkaar door het downloaden van een installatiekopie van één pixel gehost in de Azure-regio's en zoek ook de latentie tussen het moment dat de aanvraag is verzonden en het tijdstip waarop de eerste byte is ontvangen . Deze metingen zijn wordt gerapporteerd, terug naar de Traffic Manager-service.

Dit gebeurt vaak na verloop van tijd en veel netwerken voorloopspaties aan Traffic Manager, nauwkeuriger informatie ophalen over de kenmerken van de latentie van de netwerken waarin uw eindgebruikers worden geplaatst. Deze informatie wordt gestart moet worden opgenomen in de Routeringsbeslissingen gemaakt door Traffic Manager ophalen. Als gevolg hiervan leidt dit tot grotere nauwkeurigheid in deze besluiten op basis van de werkelijke gebruiker metingen verzonden.

Wanneer u een echte gebruiker metingen gebruikt, kunt u wordt gefactureerd op basis van het aantal metingen aan Traffic Manager verzonden. Voor meer informatie over de prijzen, gaat u naar de [Traffic Manager pagina met prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="next-steps"></a>Volgende stappen
- Informatie over het gebruik [echte gebruiker metingen met webpagina's](traffic-manager-create-rum-web-pages.md)
- Meer informatie over [de werking van Traffic Manager](traffic-manager-overview.md)
- Meer informatie over [Mobile Center](https://docs.microsoft.com/mobile-center/)
- Meer informatie over de [verkeersroutering methoden](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

