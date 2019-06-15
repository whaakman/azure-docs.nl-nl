---
title: Azure voordeur Service - back-end-statuscontrole | Microsoft Docs
description: In dit artikel helpt u begrijpen hoe Azure voordeur Service controleert de status van uw back-ends
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
ms.openlocfilehash: 59a3bac39437b91eeee3b005bd23476a34a308b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60736578"
---
# <a name="health-probes"></a>Statuscontroles

Om te bepalen van de status van elke back-end, verzendt elke omgeving voordeur periodiek een synthetische HTTP/HTTPS-aanvraag naar elk van de geconfigureerde back-ends. Front Door gebruikt de reacties op deze tests dan om te bepalen welke back-ends het beste zijn voor het routeren van echte aanvragen van klanten.


## <a name="supported-protocols"></a>Ondersteunde protocollen

Voordeur ondersteunt het verzenden van tests via HTTP of HTTPS-protocol. De tests worden via dezelfde TCP-poorten verzonden als de poorten die zijn geconfigureerd voor het routeren van aanvragen van klanten. Deze instelling kan niet worden overschreven.

## <a name="health-probe-responses"></a>Test statusverklaringsreacties

| Responses  | Description | 
| ------------- | ------------- |
| Status bepalen  |  Een 200 OK statuscode geeft aan dat de back-end in orde is. Alle andere wordt beschouwd als een fout. Als een geldige HTTP-antwoord niet voor een bepaalde reden (inclusief vanwege een netwerkfout) voor een test ontvangen wordt, wordt de test als mislukt beschouwd.|
| Latentie meten  | Latentie is de kloktijd gemeten vanaf het moment dat onmiddellijk voordat we de test-aanvraag verzenden naar het moment waarop we de laatste byte van het antwoord ontvangen. We gebruiken een nieuwe TCP-verbinding voor elke aanvraag, zodat deze meting niet op de back-ends met bestaande warm-verbindingen gericht is.  |

## <a name="how-front-door-determines-backend-health"></a>Hoe voordeur bepaalt voor back-endstatus

Azure voordeur-Service gebruikt hetzelfde proces drie stappen hieronder over alle algoritmen om status te bepalen.

1. Back-ends voor uitgeschakelde uitsluiten.

2. Back-ends die de gezondheid van tests fouten bevatten uitsluiten:
    * Deze selectie wordt gedaan door te kijken naar de laatste _n_ statusverklaringsreacties test. Als ten minste _x_ in orde is, de back-end wordt beschouwd als in orde zijn.

    * _n_ is geconfigureerd door de eigenschap SampleSize in load balancing-instellingen te wijzigen.

    * _x_ is geconfigureerd door de eigenschap SuccessfulSamplesRequired in load balancing-instellingen te wijzigen.

3. Buiten de set met goede back-ends via de back-endpool, voordeur Daarnaast meet en onderhoudt de latentie (round trip time) voor elke back-end.


## <a name="complete-health-probe-failure"></a>Fout voor status-test

Als statuscontroles voor elke back-end in een back-endpool mislukken, voordeur van mening is dat alle back-ends in orde en routeert verkeer in een round robin-distributie op alle.

Wanneer een back-end een goede status retourneert, hervat voordeur het normale algoritme voor taakverdeling.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).
