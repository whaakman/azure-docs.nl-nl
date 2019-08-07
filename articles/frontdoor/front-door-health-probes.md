---
title: Azure front-deur-service-back-end-status controle | Microsoft Docs
description: Dit artikel helpt u te begrijpen hoe de status van uw back-endservers wordt bewaakt door de Azure front-deur service
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 289b05a2c50a2b4af50eb2114515a49bb653cf1a
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2019
ms.locfileid: "68742396"
---
# <a name="health-probes"></a>Tests

Om de status van elke back-end te bepalen, stuurt elke front-deur omgeving periodiek een synthetische HTTP/HTTPS-aanvraag naar elk van uw geconfigureerde back-ends. Front Door gebruikt de reacties op deze tests dan om te bepalen welke back-ends het beste zijn voor het routeren van echte aanvragen van klanten. Houd er rekening mee dat de status test een volume voor de back-end-aanvragen kan worden ingesteld op het moment dat er meer dan één aanvraag per seconde is, omdat de voor deur veel verschillende omgevingen wereld wijd heeft. 



## <a name="supported-protocols"></a>Ondersteunde protocollen

De voor deur ondersteunt het verzenden van tests over HTTP-of HTTPS-protocollen. De tests worden via dezelfde TCP-poorten verzonden als de poorten die zijn geconfigureerd voor het routeren van aanvragen van klanten. Deze instelling kan niet worden overschreven.

## <a name="health-probe-responses"></a>Antwoorden op status testen

| Responses  | Description | 
| ------------- | ------------- |
| Status bepalen  |  Een 200 OK-status code geeft aan dat de back-end in orde is. Alle andere zaken worden beschouwd als een fout. Als er om een of andere reden (inclusief netwerk fout) geen geldig HTTP-antwoord wordt ontvangen voor een test, wordt de test als een fout beschouwd.|
| Meet latentie  | Latentie is de klok tijd gemeten vanaf het moment onmiddellijk voordat de test aanvraag wordt verzonden naar het moment dat de laatste byte van het antwoord wordt ontvangen. We gebruiken een nieuwe TCP-verbinding voor elke aanvraag, zodat deze meting niet kan worden afgestemd op back-ends met bestaande warme verbindingen.  |

## <a name="how-front-door-determines-backend-health"></a>De status van de back-end voor de deur bepalen

De Azure front-deur service maakt gebruik van hetzelfde proces voor drie stappen hieronder in alle algoritmen om de status te bepalen.

1. Uitgeschakelde back-ends uitsluiten.

2. Back-end met status controles uitsluiten:
    * Deze selectie wordt uitgevoerd door de laatste _n_ antwoorden op de status test te bekijken. Als ten minste _x_ in orde is, wordt de back-end als gezond beschouwd.

    * _n_ wordt geconfigureerd door de eigenschap SampleSize in instellingen voor taak verdeling te wijzigen.

    * _x_ wordt geconfigureerd door de eigenschap SuccessfulSamplesRequired in instellingen voor taak verdeling te wijzigen.

3. Van de set met gezonde back-ends in de back-end-pool, worden ook de latentie (round-trip tijd) voor elke back-end gemeten.


## <a name="complete-health-probe-failure"></a>Het volt ooien van de status test is mislukt

Als de status controles voor elke back-end in een back-end-groep mislukken, worden alle back-ends in orde beschouwd en wordt het verkeer gerouteerd in een round robin distributie over alle.

Zodra een back-end een goede status krijgt, wordt het normale taakverdelings algoritme hervat met de voor deur.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).
