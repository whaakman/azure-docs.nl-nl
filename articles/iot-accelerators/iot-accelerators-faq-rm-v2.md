---
title: Externe bewaking oplossingsverbetering FAQ - Azure | Microsoft Docs
description: Veelgestelde vragen over oplossingsverbetering voor externe controle
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 746d21c7ff4d5b939eea7690193ac07425b4001c
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53602627"
---
# <a name="frequently-asked-questions-for-remote-monitoring-solution-accelerator"></a>Veelgestelde vragen over oplossingsverbetering voor externe controle

Zie ook de algemene [Veelgestelde vragen over](iot-accelerators-faq.md).

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Hoeveel kost het de nieuwe oplossing voor externe controle inrichten?

De nieuwe oplossingsverbetering biedt twee opties voor implementatie:

* Een *basic* optie die is ontworpen voor ontwikkelaars die willen voor lagere kosten voor ontwikkeling of klanten wilt bouwen, een demo's of concepttest.
* Een *standard* optie die is ontworpen voor ondernemingen willen implementeren van een infrastructuur gereed is voor productie.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Hoe kan ik dat ik en houd de kosten laag terwijl ik mijn oplossing ontwikkelen?

Naast het leveren van twee gedifferentieerde implementaties, heeft de nieuwe oplossing voor externe controle een instelling als u wilt in- of uitschakelen van de gesimuleerde apparaten op aanvraag. Uitschakelen van de simulatie van vermindert de gegevens die zijn opgenomen in de oplossing en dus de totale kosten.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Wat is het verschil tussen de basic en standard-implementatie-opties? Hoe bepaal ik tussen de twee implementatieopties?

Elke Implementatieoptie reageert op verschillende behoeften. De eenvoudige implementatie is ontworpen om aan de slag en ontwikkel PoC en kleine testfases. Het biedt een gestroomlijnde architectuur met de minimaal benodigde resources en lagere kosten. De standaardimplementatie is ontworpen om te ontwikkelen en aanpassen van een oplossing gereed is voor productie, en biedt een implementatie met de elementen die nodig zijn om te profiteren van die. Voor de betrouwbaarheid en schaal, microservices voor toepassingen worden gebouwd als Docker-containers en geïmplementeerd met behulp van een orchestrator (Kubernetes standaard). De orchestrator is verantwoordelijk voor de implementatie, schaling en beheer van de toepassing. U moet een optie op basis van uw huidige behoeften. U kunt een, de andere of een combinatie van beide, afhankelijk van de projectfase van uw.

### <a name="how-do-i-configure-a-dynamic-map-on-the-dashboard"></a>Hoe configureer ik een dynamische kaart op het dashboard?

Zie voor meer informatie, [Upgrade kaart sleutel om te zien van apparaten op een dynamische kaart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map).

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Waar vind ik informatie over de vorige versie van de oplossing voor externe controle

De vorige versie van de oplossingsverbetering voor externe controle was bekend als de IoT Suite vooraf geconfigureerde oplossing voor externe controle. U vindt de gearchiveerde documentatie op [ https://docs.microsoft.com/previous-versions/azure/iot-suite/ ](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de IoT-oplossingsversnellers bekijken:

* [Verken de mogelijkheden van de oplossingsverbetering voor externe controle](quickstart-remote-monitoring-deploy.md)
* [Een overzicht van de oplossingsversneller voor Predictief onderhoud](iot-accelerators-predictive-overview.md)
* [Verbonden Factory oplossingsversnellers implementeren](quickstart-connected-factory-deploy.md)
* [Fundamentele IoT-beveiliging](/azure/iot-fundamentals/iot-security-ground-up)
