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
ms.openlocfilehash: 256d530590fadc9e2aeb1ea1efb7a52608014978
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46988560"
---
# <a name="health-probes"></a>Statuscontroles

Om te bepalen van de status van elke back-end, verzendt elke omgeving voordeur periodiek een synthetische HTTP/HTTPS-aanvraag naar elk van de geconfigureerde back-ends. Antwoorden van deze tests voordeur vervolgens gebruikt om te bepalen van de 'aanbevolen' back-ends waarnaar het echte clientaanvragen te routeren.


## <a name="supported-protocols"></a>Ondersteunde protocollen

Voordeur ondersteunt het verzenden van tests via HTTP of HTTPS-protocol. Deze tests worden verzonden via dezelfde TCP-poorten geconfigureerd voor routering aanvragen van clients, en kunnen niet worden overschreven.

## <a name="health-probe-responses"></a>Test statusverklaringsreacties

| Antwoorden  | Beschrijving | 
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

- Meer informatie over het [maken van een voordeur](quickstart-create-front-door.md).
- Informatie over [de werking van de voordeur](front-door-routing-architecture.md).
