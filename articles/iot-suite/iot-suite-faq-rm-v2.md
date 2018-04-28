---
title: Externe controle Veelgestelde vragen over Azure IoT-Suite | Microsoft Docs
description: Veelgestelde vragen over IoT Suite vooraf geconfigureerde oplossing voor externe controle
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: b0d6dbb567f66537c599d7ac3e4337988b3374c0
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="frequently-asked-questions-for-iot-suite-remote-monitoring-preconfigured-solution"></a>Veelgestelde vragen over IoT Suite vooraf geconfigureerde oplossing voor externe controle

Zie ook de algemene [Veelgestelde vragen over](iot-suite-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Hoeveel kost het inrichten van de nieuwe oplossing voor externe controle

De nieuwe vooraf geconfigureerde oplossing biedt twee opties voor implementatie:

* Een *basic* optie ontworpen voor ontwikkelaars die op zoek naar de lagere kosten voor ontwikkeling of klanten die willen maken van een demo noch een bewijs van het concept.
* Een *standaard* optie ontworpen voor ondernemingen willen implementeren van een infrastructuur gereed voor productie.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Hoe zorgt dat ik kostenbeperkend mijn tijdens het ontwikkelen van Mijn-oplossing?

Naast het bieden van twee gedifferentieerde implementaties, heeft de nieuwe oplossing voor externe controle een instelling of de gesimuleerde apparaten op aanvraag uit te schakelen. Het uitschakelen van de simulatie, vermindert de gegevens in de oplossing en dus de totale kosten ingenomen.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Wat is het verschil tussen de basis en standaard implementatie-opties? Hoe bepaal ik tussen de twee implementatie-opties?

Elke Implementatieoptie reageert op verschillende behoeften. De eenvoudige implementatie is ontworpen om aan de slag en implementatiemodel en kleine testfases ontwikkelen. Het biedt een gestroomlijnde architectuur met de minimaal benodigde resources en lagere kosten. De standaardimplementatie is ontworpen om te bouwen en aanpassen van een oplossing gereed is voor productie en biedt een implementatie met de vereiste elementen mee die. Betrouwbaarheid en schaal application microservices als Docker-containers zijn gebouwd en geïmplementeerd met behulp van een orchestrator (Kubernetes standaard). De orchestrator is verantwoordelijk voor de implementatie, schaalbaarheid en beheer van de toepassing. Kies een optie op basis van uw huidige behoeften. U kunt een, de andere of een combinatie van beide, afhankelijk van uw projectfase gebruiken.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Hoe kan ik een dynamische kaart configureren op het dashboard?

Zie voor meer informatie [Upgrade kaart sleutel apparaten wilt bekijken op een dynamische kaart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel gratis Bing Kaarten-API's kan ik inrichten met een abonnement?

Twee. U kunt slechts twee interne transacties niveau 1 Bing-kaarten voor Enterprise plannen in een Azure-abonnement maken. De oplossing voor externe controle is standaard ingericht met het interne transacties niveau 1-plan. Als gevolg hiervan kunt u met een abonnement waaraan geen wijzigingen zijn aangebracht maximaal twee externe bewakingsoplossingen inrichten.

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Ontdek de mogelijkheden van de vooraf geconfigureerde oplossing voor externe controle](iot-suite-remote-monitoring-explore.md)
* [Overzicht van voorspeld onderhoud vooraf geconfigureerde oplossing](iot-suite-predictive-overview.md)
* [Overzicht van de verbonden factory vooraf geconfigureerde oplossing](iot-suite-connected-factory-overview.md)
* [Beveiliging van een compleet nieuwe IoT](securing-iot-ground-up.md)