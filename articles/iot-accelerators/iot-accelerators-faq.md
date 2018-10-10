---
title: Azure IoT-oplossingsversnellers Veelgestelde vragen over | Microsoft Docs
description: Veelgestelde vragen over IoT-oplossingsversnellers
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 154d82ab5e309631df0bfa40df7fe14902327f46
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48884272"
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>Veelgestelde vragen over IoT-oplossingsversnellers

Zie ook de [verbonden Factory-specifieke Veelgestelde vragen over](iot-accelerators-faq-cf.md) en de [Veelgestelde vragen over externe bewaking-specifieke](iot-accelerators-faq-rm-v2.md) .

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Waar vind ik de broncode voor de oplossingsversnellers?

De broncode wordt opgeslagen in de volgende GitHub-opslagplaatsen:

* [Oplossingsverbetering voor externe bewaking (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Oplossingsverbetering voor externe bewaking (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Oplossingsverbetering voor predictief onderhoud](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Verbonden Factory-oplossingsversnellers](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Welke SDK's kan ik gebruiken voor het ontwikkelen van apparaatclients voor de oplossingsversnellers?

Vindt u koppelingen naar de andere taal (C, .NET, Java, Node.js, Python) IoT device SDK's in de [Microsoft Azure IoT SDK's](https://github.com/Azure/azure-iot-sdks) GitHub-opslagplaatsen.

Als u van het apparaat DevKit gebruikmaakt, vindt u bronnen en voorbeelden uit de [IoT DevKit SDK](https://github.com/Microsoft/devkit-sdk) GitHub-opslagplaats.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Is de nieuwe architectuur met microservices beschikbaar voor alle drie de oplossingsversnellers?

Op dit moment alleen de oplossing voor externe controle maakt gebruik van de architectuur met microservices omdat hierin de meest uiteenlopende scenario.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Welke voordelen biedt de nieuwe open source op basis van microservices-architectuur in de nieuwe update?

Architectuur van de afgelopen twee jaar aanzienlijk zich heeft ontwikkeld. Microservices hebben naar voren gekomen als een geweldige patroon om de schaal en flexibiliteit, zonder dat dit ten koste gaat ontwikkelingssnelheid bereiken. Dit architectuurpatroon wordt gebruikt in verschillende Microsoft-services die intern met geweldige betrouwbaarheid en schaalbaarheid resultaten. We worden deze in de praktijk leren, zodat onze klanten van deze profiteren uitgeoefend.

### <a name="where-can-i-find-information-about-the-previous-version-of-the-remote-monitoring-solution"></a>Waar vind ik informatie over de vorige versie van de oplossing voor externe controle

De vorige versie van de oplossingsverbetering voor externe controle was bekend als de IoT Suite vooraf geconfigureerde oplossing voor externe controle. U vindt de gearchiveerde documentatie op [ https://docs.microsoft.com/previous-versions/azure/iot-suite/ ](https://docs.microsoft.com/previous-versions/azure/iot-suite/).

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>Nieuwe solution accelerator is beschikbaar in dezelfde geografische regio als de bestaande oplossing?

Ja, de nieuwe externe controle is beschikbaar in dezelfde geografische regio's.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsuitecom"></a>Wat is het verschil tussen het verwijderen van een resourcegroep in Azure portal en te klikken op verwijderen bij een oplossingsversnellers op azureiotsuite.com?

* Als u de oplossingsversneller in verwijdert [azureiotsuite.com](https://www.azureiotsolutions.com/), verwijdert u alle resources die zijn ingericht bij het maken van de solution accelerator. Als u aanvullende resources toegevoegd aan de resourcegroep, worden deze resources worden ook verwijderd.
* Als u verwijdert de resourcegroep in de [Azure-portal](https://portal.azure.com), verwijdert u alleen de resources in die resourcegroep. U moet ook de Azure Active Directory-toepassing die is gekoppeld aan de solution accelerator verwijderen.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Kan ik dan blijven om mijn bestaande investeringen in Azure IoT-oplossingsversnellers?

Ja. Een oplossing die vandaag de dag blijft werken in uw Azure-abonnement en de broncode blijft beschikbaar is in GitHub.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van de IoT Hub kan ik inrichten in een abonnement?

Standaard kunt u inrichten [10 IoT-hubs per abonnement](../azure-subscription-service-limits.md#iot-hub-limits). U kunt maken een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om deze limiet te verhogen. Als gevolg hiervan, kunt aangezien elke oplossing accelerator levert een nieuwe IoT-Hub, u alleen inrichten tot 10 oplossingsversnellers in een bepaald abonnement.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van Azure Cosmos DB kan ik inrichten in een abonnement?

Vijftig. U kunt maken een [Azure-ondersteuningsticket](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om deze limiet te verhogen, maar standaard kunt u alleen 50 Cosmos DB-exemplaren per abonnement inrichten.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel gratis Bing Kaarten-API's kan ik inrichten met een abonnement?

Twee. U kunt slechts twee interne transacties niveau 1 Bing-kaarten voor Enterprise-abonnementen maken in een Azure-abonnement. De oplossing voor externe controle is standaard ingericht met het interne transacties Level-1-plan. Als gevolg hiervan kunt u alleen maximaal twee externe controle-oplossingen in een abonnement zonder wijzigingen inrichten.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Kan ik een oplossingsversnellers maken als ik Microsoft Azure voor DreamSpark heb?

> [!NOTE]
> Microsoft Azure voor DreamSpark is nu bekend als Microsoft Imagine voor studenten.

U kunt geen op dit moment een oplossingsversnellers met maken een [Microsoft Azure voor DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) account. U kunt echter maken een [gratis proefaccount voor Azure](https://azure.microsoft.com/free/) maken in een paar minuten waarmee u kunt een oplossingsversnellers.

### <a name="how-do-i-delete-an-aad-tenant"></a>Hoe verwijder ik een AAD-tenant?

Zie het blogbericht van Eric Golpe van [overzicht van het verwijderen van een Azure AD-Tenant](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de IoT-oplossingsversnellers bekijken:

* [Verken de mogelijkheden van de oplossingsverbetering voor externe controle](quickstart-remote-monitoring-deploy.md)
* [Een overzicht van de oplossingsversneller voor Predictief onderhoud](iot-accelerators-predictive-overview.md)
* [Verbonden Factory oplossingsversnellers implementeren](quickstart-connected-factory-deploy.md)
* [Fundamentele IoT-beveiliging](/azure/iot-fundamentals/iot-security-ground-up)
