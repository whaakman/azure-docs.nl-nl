---
title: Azure IoT Suite Veelgestelde vragen | Microsoft Docs
description: Veelgestelde vragen over IoT Suite
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 945b985e69c14bb513477fc740652f13236d7518
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Veelgestelde vragen over IoT Suite

Zie ook de verbonden factory-specifieke [Veelgestelde vragen over](iot-suite-faq-cf.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Waar vind ik de broncode voor de vooraf geconfigureerde oplossingen

De broncode is opgeslagen in de volgende GitHub-opslagplaatsen:

* [Externe controle vooraf geconfigureerde oplossing (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Externe controle vooraf geconfigureerde oplossing (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Oplossing voor voorspeld onderhoud vooraf geconfigureerde](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Verbonden factory vooraf geconfigureerde oplossing](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Hoeveel kost het inrichten van de nieuwe oplossing voor externe controle

De nieuwe vooraf geconfigureerde oplossing biedt twee opties voor implementatie:

* Een *basic* optie ontworpen voor ontwikkelaars die op zoek naar de lagere kosten voor ontwikkeling of klanten die willen maken van een demo noch een bewijs van het concept.
* Een *standaard* optie ontworpen voor ondernemingen willen implementeren van een infrastructuur gereed voor productie.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>Hoe zorgt dat ik kostenbeperkend mijn tijdens het ontwikkelen van Mijn-oplossing?

Naast het bieden van twee gedifferentieerde implementaties, heeft de nieuwe oplossing voor externe controle een instelling of de gesimuleerde apparaten op aanvraag uit te schakelen. Het uitschakelen van de simulatie, vermindert de gegevens in de oplossing en dus de totale kosten ingenomen.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-preconfigured-solutions"></a>De nieuwe microservices architectuur beschikbaar is voor de drie vooraf geconfigureerde oplossingen?

Alleen de oplossing voor externe controle wordt op dit moment wordt de architectuur microservices gebruikt als ze het breedste scenario omvatten.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Welke voordelen biedt de nieuwe open-source microservices gebaseerde architectuur in de nieuwe update?

Architectuur van de afgelopen twee jaar aanzienlijk zich heeft ontwikkeld. Micro services is als een geweldige patroon voor het bereiken van de schaal en flexibiliteit, zonder verlies van ontwikkelingssnelheid ontstaan. Deze architectuur patroon wordt gebruikt in verschillende Microsoft-services intern met betrouwbaar en schaalbaarheid resultaten. We wilt deze in de praktijk learning zodat onze klanten van deze profiteren opslaan.

### <a name="is-the-new-preconfigured-solution-available-in-the-same-geographic-region-as-the-existing-solution"></a>De nieuwe vooraf geconfigureerde oplossing is beschikbaar in dezelfde geografische regio als de bestaande oplossing?

Ja, de nieuwe externe controle is beschikbaar in dezelfde geografische regio's.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Wat is het verschil tussen de basis en standaard implementatie-opties? Hoe bepaal ik tussen de twee implementatie-opties?

Elke Implementatieoptie reageert op verschillende behoeften. De eenvoudige implementatie is ontworpen om aan de slag en implementatiemodel en kleine testfases ontwikkelen. Het biedt een gestroomlijnde architectuur met de minimaal benodigde resources en lagere kosten. De standaardimplementatie is ontworpen om te bouwen en aanpassen van een oplossing gereed is voor productie en biedt een implementatie met de vereiste elementen mee die. Betrouwbaarheid en schaal application microservices als Docker-containers zijn gebouwd en geïmplementeerd met behulp van een orchestrator (Kubernetes standaard). De orchestrator is verantwoordelijk voor de implementatie, schaalbaarheid en beheer van de toepassing. Kies een optie op basis van uw huidige behoeften. U kunt een, de andere of een combinatie van beide, afhankelijk van uw projectfase gebruiken.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-suite"></a>Kan ik mijn bestaande investeringen in Azure IoT Suite gebruikmaken doorgaan?

Ja. Een oplossing die tegenwoordig blijft werken in uw Azure-abonnement en de broncode blijft beschikbaar in GitHub.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Wat is het verschil tussen het verwijderen van een resourcegroep in Azure Portal en op verwijderen te klikken in een vooraf geconfigureerde oplossing op azureiotsuite.com?

* Als u de vooraf geconfigureerde oplossing in verwijdert [azureiotsuite.com](https://www.azureiotsuite.com/), verwijdert u alle resources die zijn ingericht toen u de vooraf geconfigureerde oplossing hebt gemaakt. Als u aanvullende bronnen toegevoegd aan de resourcegroep, wordt deze resources worden ook verwijderd.
* Als u de resourcegroep in verwijdert de [Azure-portal](https://portal.azure.com), verwijdert u alleen de resources in die resourcegroep. U moet ook de Azure Active Directory-toepassing die is gekoppeld aan de vooraf geconfigureerde oplossing verwijderen.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van de IoT Hub kan ik inrichten in een abonnement?

Standaard kunt u inrichten [10 IoT hubs per abonnement](../azure-subscription-service-limits.md#iot-hub-limits). Kunt u een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) deze limiet te verhogen. Omdat elke vooraf geconfigureerde oplossing een nieuwe IoT Hub inricht, kunt u als gevolg hiervan alleen maximaal 10 vooraf geconfigureerde oplossingen in een bepaald abonnement inrichten.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van Azure DB die Cosmos kan ik inrichten in een abonnement?

Vijftig. Kunt u een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) deze limiet te verhogen, maar standaard kunt u alleen 50 exemplaren van de Cosmos-database per abonnement inrichten.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel gratis Bing Kaarten-API's kan ik inrichten met een abonnement?

Twee. U kunt slechts twee interne transacties niveau 1 Bing-kaarten voor Enterprise plannen in een Azure-abonnement maken. De oplossing voor externe controle is standaard ingericht met het interne transacties niveau 1-plan. Als gevolg hiervan kunt u met een abonnement waaraan geen wijzigingen zijn aangebracht maximaal twee externe bewakingsoplossingen inrichten.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kan ik een vooraf geconfigureerde oplossing maken als ik Microsoft Azure voor DreamSpark heb?

> [!NOTE]
> Microsoft Azure voor DreamSpark is nu bekend als Microsoft Imagine voor studenten.

Op dit moment kunt u een vooraf geconfigureerde oplossing met geen maken een [Microsoft Azure voor DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) account. U kunt echter maken een [gratis proefaccount voor Azure](https://azure.microsoft.com/free/) binnen een paar minuten waarmee u een vooraf geconfigureerde oplossing maken.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Kan ik een vooraf geconfigureerde oplossing maken als ik Cloud Solution Provider (CSP) abonnement?

U kunt op dit moment een vooraf geconfigureerde oplossing maken met een abonnement Cloud Solution Provider (CSP). U kunt echter maken een [gratis proefaccount voor Azure](https://azure.microsoft.com/free/) binnen een paar minuten waarmee u een vooraf geconfigureerde oplossing maken.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hoe verwijder ik een AAD-tenant?

Zie het blogbericht van Eric Golpe van [overzicht van het verwijderen van een Azure AD-Tenant](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Overzicht van voorspeld onderhoud vooraf geconfigureerde oplossing](iot-suite-predictive-overview.md)
* [Overzicht van de verbonden factory vooraf geconfigureerde oplossing](iot-suite-connected-factory-overview.md)
* [Beveiliging van een compleet nieuwe IoT](securing-iot-ground-up.md)