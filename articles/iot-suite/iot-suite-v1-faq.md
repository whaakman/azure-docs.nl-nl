---
title: Veelgestelde vragen over Azure IoT Suite | Microsoft Docs
description: Veelgestelde vragen over IoT Suite
services: ''
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
ms.date: 11/02/2017
ms.author: corywink
ms.openlocfilehash: 7e7c4affee64a945900c02b6375ba4df5d085183
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35757801"
---
# <a name="frequently-asked-questions-for-iot-suite"></a>Veelgestelde vragen over IoT Suite

Zie ook de specifieke verbonden factory [Veelgestelde vragen over](../iot-accelerators/iot-accelerators-faq-cf.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Waar vind ik de broncode voor de vooraf geconfigureerde oplossingen?

De broncode wordt opgeslagen in de volgende GitHub-opslagplaatsen:
* [Vooraf geconfigureerde oplossing voor externe controle][lnk-remote-monitoring-github]
* [Oplossing voor voorspeld onderhoud][lnk-predictive-maintenance-github]
* [Oplossing voor verbonden factory preconfigured](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-do-i-update-to-the-latest-version-of-the-remote-monitoring-preconfigured-solution-that-uses-the-iot-hub-device-management-features"></a>Hoe kan ik bijwerken naar de nieuwste versie van de vooraf geconfigureerde oplossing voor externe controle die gebruikmaakt van de functies voor IoT-Hub Apparaatbeheer?

* Als u een vooraf geconfigureerde oplossing van implementeert de https://www.azureiotsuite.com/ site implementeert altijd een nieuw exemplaar van de meest recente versie van de oplossing.
* Als u een vooraf geconfigureerde oplossing met behulp van de opdrachtregel implementeert, kunt u een bestaande implementatie bijwerken met nieuwe code. Zie [implementatie in de Cloud] [ lnk-cloud-deployment] in de GitHub [opslagplaats][lnk-remote-monitoring-github].

### <a name="how-can-i-add-support-for-a-new-device-method-to-the-remote-monitoring-preconfigured-solution"></a>Hoe kan ik ondersteuning voor een nieuwe apparaatmethode voor het toevoegen aan de vooraf geconfigureerde oplossing voor externe controle?

Zie de sectie [ondersteuning voor een nieuwe methode toevoegen aan de simulator] [ lnk-add-method] in de [een vooraf geconfigureerde oplossing aanpassen] [ lnk-customize] artikel.

### <a name="the-simulated-device-is-ignoring-my-desired-property-changes-why"></a>Het gesimuleerde apparaat negeert mijn wijzigingen gewenste eigenschap waarom?
In de vooraf geconfigureerde oplossing voor externe controle, het gesimuleerde apparaat gebruikt alleen code de **Desired.Config.TemperatureMeanValue** en **Desired.Config.TelemetryInterval** gewenste eigenschappen op de gerapporteerde eigenschappen bijwerken. Alle andere gewenste eigenschap wijzigingsaanvragen worden genegeerd.

### <a name="my-device-does-not-appear-in-the-list-of-devices-in-the-solution-dashboard-why"></a>Mijn apparaat wordt niet weergegeven in de lijst met apparaten op het dashboard van de oplossing, waarom?

De lijst met apparaten op het dashboard van de oplossing maakt gebruik van een query om terug te keren van de lijst met apparaten. Op dit moment kan niet een query meer dan 10K apparaten retourneren. Probeer de zoekcriteria voor de query meer beperkende maken.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Wat is het verschil tussen het verwijderen van een resourcegroep in Azure Portal en op verwijderen te klikken in een vooraf geconfigureerde oplossing op azureiotsuite.com?

* Als u de vooraf geconfigureerde oplossing in verwijdert [azureiotsuite.com][lnk-azureiotsuite], verwijdert u alle resources die zijn ingericht toen u de vooraf geconfigureerde oplossing hebt gemaakt. Als u aanvullende resources toegevoegd aan de resourcegroep, worden deze resources worden ook verwijderd. 
* Als u verwijdert de resourcegroep in de [Azure-portal][lnk-azure-portal], verwijdert u alleen de resources in die resourcegroep. U moet ook verwijderen van de Azure Active Directory-toepassing die is gekoppeld aan de vooraf geconfigureerde oplossing in de [Azure-portal][lnk-azure-portal].

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van de IoT Hub kan ik inrichten in een abonnement?

Standaard kunt u inrichten [10 IoT-hubs per abonnement][link-azuresublimits]. U kunt maken een [Azure-ondersteuningsticket] [ link-azuresupportticket] om deze limiet te verhogen. Omdat elke vooraf geconfigureerde oplossing voor een nieuwe IoT-Hub inricht, kunt u als gevolg hiervan alleen maximaal 10 vooraf geconfigureerde oplossingen in een bepaald abonnement inrichten. 

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Hoeveel exemplaren van Azure Cosmos DB kan ik inrichten in een abonnement?

Vijftig. U kunt maken een [Azure-ondersteuningsticket] [ link-azuresupportticket] om deze limiet te verhogen, maar standaard kunt u alleen 50 Cosmos DB-exemplaren per abonnement inrichten. 

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Hoeveel gratis Bing Kaarten-API's kan ik inrichten met een abonnement?

Twee. U kunt slechts twee interne transacties niveau 1 Bing-kaarten voor Enterprise-abonnementen maken in een Azure-abonnement. De oplossing voor externe controle is standaard ingericht met het interne transacties Level-1-plan. Als gevolg hiervan kunt u met een abonnement waaraan geen wijzigingen zijn aangebracht maximaal twee externe bewakingsoplossingen inrichten.

### <a name="i-have-a-remote-monitoring-solution-deployment-with-a-static-map-how-do-i-add-an-interactive-bing-map"></a>Ik heb een implementatie van een externe bewakingsoplossing met een statische kaart, hoe voeg ik een interactieve Bing-kaart toe?

1. Ophalen van uw Bing kaarten-API voor Enterprise-querysleutel in [Azure-portal][lnk-azure-portal]: 
   
   1. Navigeer naar de resourcegroep waar uw Bing kaarten-API voor Enterprise zich in de [Azure-portal][lnk-azure-portal].
   2. Klik op **alle instellingen**, klikt u vervolgens **Sleutelbeheer**. 
   3. U ziet twee sleutels: **hoofdsleutel** en **querysleutel**. Kopieer de waarde voor **querysleutel**.
      
      > [!NOTE]
      > U hebt geen Bing Kaarten-API voor Enterprise-account? Maken in de [Azure-portal] [ lnk-azure-portal] door te klikken op + Nieuw, te zoeken naar Bing kaarten-API voor Enterprise en volg de aanwijzingen voor het maken.
      > 
      > 
2. Haal de meest recente code uit de [Azure-IoT-Remote-Monitoring][lnk-remote-monitoring-github].
3. Uitvoeren van een lokale of implementatie van de opdrachtregel implementatie-informatie in de map /docs/ in de opslagplaats te volgen in de cloud. 
4. Nadat u een lokale implementatie of een cloudimplementatie hebt uitgevoerd, zoekt u in de hoofdmap naar het bestand *.user.config dat is gemaakt tijdens de implementatie. Open dit bestand in een teksteditor. 
5. Wijzig de volgende regel zodat die de waarde die u hebt gekopieerd uit uw **querysleutel**: 
   
   `<setting name="MapApiQueryKey" value="" />`

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>Kan ik een vooraf geconfigureerde oplossing maken als ik Microsoft Azure voor DreamSpark heb?

> [!NOTE]
> Microsoft Azure voor DreamSpark is nu bekend als Microsoft Imagine voor studenten.

Op dit moment niet kan u een vooraf geconfigureerde oplossing met een [Microsoft Azure voor DreamSpark](https://azure.microsoft.com/pricing/member-offers/imagine/) account. U kunt echter maken een [gratis proefaccount voor Azure](https://azure.microsoft.com/free/) binnen een paar minuten waarmee u kunt een vooraf geconfigureerde oplossing maken.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Kan ik een vooraf geconfigureerde oplossing maken als ik heb Cloud Solution Provider (CSP)-abonnement?

Op dit moment kunt u een vooraf geconfigureerde oplossing maken met een Cloud Solution Provider (CSP)-abonnement. U kunt echter maken een [gratis proefaccount voor Azure] [ lnk-30daytrial] binnen een paar minuten waarmee u kunt een vooraf geconfigureerde oplossing maken.

### <a name="how-do-i-delete-an-azure-ad-tenant"></a>Hoe verwijder ik een Azure AD-tenant?

Zie het blogbericht van Eric Golpe van [overzicht van het verwijderen van een Azure AD-Tenant][lnk-delete-aad-tennant].

### <a name="next-steps"></a>Volgende stappen

U kunt ook enkele van de andere functies en mogelijkheden van de vooraf geconfigureerde IoT Suite-oplossingen verkennen:

* [Overzicht van de voorspeld onderhoud vooraf geconfigureerde oplossing][lnk-predictive-overview]
* [Overzicht van de verbonden factory vooraf geconfigureerde oplossing](../iot-accelerators/iot-accelerators-connected-factory-overview.md)
* [Fundamentele IoT-beveiliging][lnk-security-groundup]

[lnk-predictive-overview]:../iot-accelerators/iot-accelerators-predictive-overview.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up

[link-azuresupportticket]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade 
[link-azuresublimits]: https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/#iot-hub-limits
[lnk-azure-portal]: https://portal.azure.com
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring 
[lnk-dreamspark]: https://www.dreamspark.com/Product/Product.aspx?productid=99 
[lnk-30daytrial]: https://azure.microsoft.com/free/
[lnk-delete-aad-tennant]: http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx
[lnk-cloud-deployment]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/cloud-deployment.md
[lnk-add-method]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md#add-support-for-a-new-method-to-the-simulator
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-remote-monitoring-github]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-predictive-maintenance-github]: https://github.com/Azure/azure-iot-predictive-maintenance
