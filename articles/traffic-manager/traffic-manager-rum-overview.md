---
title: Real-User-metingen in Azure Traffic Manager
description: Inleiding tot Real-User-metingen in Traffic Manager
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: fd37ef739522955ae8227db39a41aecf199d65c3
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052816"
---
# <a name="traffic-manager-real-user-measurements-overview"></a>Overzicht van Traffic Manager Real User Measurements

Bij het instellen van een Traffic Manager-profiel gebruiken de routeringsmethode voor prestaties is de service gekeken van waar de DNS-query-aanvragen die afkomstig zijn van en routering beslissingen te nemen om te leiden die aanvragers naar de Azure-regio met de laagste latentie. Dit wordt bereikt door het gebruik van de netwerklatentie-informatie die wordt onderhouden door Traffic Manager voor verschillende eindgebruikers netwerken.

Real-User-metingen kunt u voor het meten van de replicatielatentie netwerk naar Azure-regio's, vanuit de clienttoepassingen die uw eindgebruikers gebruiken, en hebben Traffic Manager kunt u deze informatie ook bij het maken van Routeringsbeslissingen. Als u kiest de Real-User-metingen gebruiken, kunt u de nauwkeurigheid van de routering voor aanvragen die afkomstig zijn van deze netwerken waar uw gebruikers zich bevinden verhogen. 

## <a name="how-real-user-measurements-work"></a>De werking van Real User Measurements

Real User Measurements werken, doordat uw client-toepassingen meting latentie met aan Azure-regio's, zoals deze wordt weergegeven van de eindgebruiker-netwerken waarin ze worden gebruikt. Bijvoorbeeld, als u een webpagina die wordt gebruikt door gebruikers op verschillende locaties (bijvoorbeeld in de Noord-Amerikaanse regio's) hebt, kunt u Real User Measurements met routeringsmethode voor prestaties aan om ze naar de beste Azure-regio waarin uw server toepassing wordt gehost.

Deze begint met het insluiten van een Azure opgegeven JavaScript (met een unieke sleutel erin) in uw webpagina's. Wanneer dat is gebeurd, wanneer een gebruiker die webpagina bezoekt, vraagt de JavaScript Traffic Manager voor informatie over de Azure-regio's die het te meten. De service retourneert een set met eindpunten aan het script, die vervolgens meting deze regio's achter elkaar door te downloaden van een installatiekopie van één pixel die wordt gehost in de Azure-regio's en waarbij de latentie tussen het moment dat de aanvraag is verzonden en het tijdstip waarop de eerste byte is ontvangen . Deze metingen worden vervolgens terug naar de Traffic Manager-service gerapporteerd.

Dit gebeurt vaak na verloop van tijd, en in veel netwerken toonaangevende op Traffic Manager, nauwkeurigere informatie ophalen over de kenmerken van de latentie van de netwerken waarin uw gebruikers zich bevinden. Deze informatie wordt gestart om te worden opgenomen in de Routeringsbeslissingen van Traffic Manager aan. Als gevolg hiervan leidt het tot betere nauwkeurigheid van de gegevens in deze beslissingen te nemen op basis van de Real-User-metingen verzonden.

Als u Real User Measurements gebruikt, kunt u wordt gefactureerd op basis van het aantal metingen naar Traffic Manager verzonden. Voor meer informatie over de prijzen, gaat u naar de [Traffic Manager-pagina met prijzen](https://azure.microsoft.com/pricing/details/traffic-manager/).

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het gebruik van [Real User Measurements met webpagina's](traffic-manager-create-rum-web-pages.md)
- Informatie over [hoe Traffic Manager werkt](traffic-manager-overview.md)
- Meer informatie over [Mobile Center](https://docs.microsoft.com/mobile-center/)
- Meer informatie over de [routeringsmethoden voor verkeer](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over het [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)

