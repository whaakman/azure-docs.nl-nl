---
title: Schaalbaarheid van Azure Service Fabric NET apps | Microsoft Docs
description: Meer informatie over het schalen van services in Azure Service Fabric NET.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 10/26/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: c58961011231f74b09abdf313b8624ddc8ad52c1
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52892339"
---
# <a name="scaling-service-fabric-mesh-applications"></a>Service Fabric-NET-toepassingen schalen

Een van de belangrijkste voordelen van het implementeren van toepassingen naar Service Fabric Mesh is de mogelijkheid voor u om services eenvoudig in of uit te schalen. Dit is handig voor het afhandelen van wisselende belastingen van uw services of het verbeteren van de beschikbaarheid. U kunt handmatig schalen uw in- of -services of setup automatisch schalen beleid.

## <a name="manual-scaling-instances"></a>Handmatige vergroten/verkleinen exemplaren

In de implementatiesjabloon voor de toepassingsresource heeft elke service een eigenschap *replicaCount* die kan worden gebruikt om in te stellen hoe vaak die service wordt geïmplementeerd. Een toepassing kan bestaan uit meerdere services, waarbij elke service een unieke waarde voor *replicaCount* heeft, die samen worden geïmplementeerd en beheerd. Om het aantal service-replica's te schalen, wijzigt u de waarde *replicaCount* voor elke service die u wilt schalen in de implementatiesjabloon of het parametersbestand. Vervolgens voert u een upgrade van de toepassing uit.

Zie voor voorbeelden van de services-exemplaren handmatig te schalen, [handmatig schalen van uw services in- of uitgeschaald](service-fabric-mesh-tutorial-template-scale-services.md).

## <a name="autoscaling-service-instances"></a>Service-exemplaren automatisch schalen
Automatisch schalen is een extra functie van Service Fabric dynamisch te schalen het nummer van uw service-instanties (horizontaal schalen). Automatisch schalen biedt grote flexibiliteit en kunt inrichten of verwijdering van service-exemplaren op basis van gebruik van CPU of geheugen.  Automatisch schalen, kunt u het juiste aantal exemplaren van de service voor uw workload uitgevoerd en kosten te optimaliseren.

Een functie voor automatisch schalen van beleid is gedefinieerd per service in het bestand van de resource. Elk beleid voor vergroten/verkleinen bestaat uit twee onderdelen:

- Een trigger vergroten/verkleinen, waarin wordt beschreven bij het schalen van de service wordt uitgevoerd. Er zijn drie factoren die bepalen wanneer de service wordt geschaald. *Drempelwaarde voor het laden van lagere* is een waarde die bepaalt wanneer de service worden ingeschaald. Als de gemiddelde belasting van alle exemplaren van de partities lager is dan deze waarde is, wordt de service worden geschaald. *Hoogste belastingsdrempelwaarde* is een waarde die bepaalt wanneer de service worden uitgebreid. Als de gemiddelde belasting van alle exemplaren van de partitie hoger dan deze waarde is, zal klikt u vervolgens de service worden uitgebreid. *Interval voor vergroten/verkleinen* bepaalt hoe vaak (in seconden), de trigger wordt gecontroleerd. Zodra de trigger is gecontroleerd, indien nodig schalen is het mechanisme wordt toegepast. Als schalen niet vereist is, klikt u vervolgens geen actie ondernomen. In beide gevallen wordt trigger niet gecontroleerd opnieuw voordat het interval voor vergroten/verkleinen is verstreken.

- Een vergroten/verkleinen mechanisme, waarin wordt beschreven hoe schalen wordt uitgevoerd wanneer deze wordt geactiveerd. *Verhoging schalen* bepaalt hoeveel exemplaren worden toegevoegd of verwijderd wanneer het mechanisme wordt geactiveerd. *Maximum aantal exemplaren* bepaalt de bovenste limiet voor het schalen. Als het aantal exemplaren van deze limiet bereikt, wordt klikt u vervolgens de service niet worden uitgebreid, ongeacht de belasting. *Minimum aantal exemplaren* bepaalt de lagere limiet voor het schalen. Als het aantal exemplaren van de partitie die deze limiet bereikt, zal klikt u vervolgens service niet worden geschaald, ongeacht de belasting.

Lees voor meer informatie over het instellen van een beleid voor automatisch schalen voor uw service, [voor automatisch schalen services](service-fabric-mesh-howto-auto-scale-services.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het toepassingsmodel [Service Fabric-resources](service-fabric-mesh-service-fabric-resources.md)
