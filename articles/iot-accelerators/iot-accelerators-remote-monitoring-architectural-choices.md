---
title: Externe controle oplossing bouwkundige keuzes - Azure | Microsoft Docs
description: In dit artikel beschrijft de architecturale en technische keuzen gemaakt in het externe controle
services: iot-suite
suite: iot-suite
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-suite
ms.date: 04/30/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 607b8aeb2f986eebddf8fe13b88e7f3bc7b4494b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="remote-monitoring-architectural-choices"></a>Externe controle bouwkundige keuzes

De Azure IoT Remote Monitoring (RM) is een open source, MIT-licentie, oplossingsverbetering die algemene IoT-scenario's zoals connectiviteit van apparaten, Apparaatbeheer en verwerken van de stroom is introduceert zodat klanten hun ontwikkeling versnellen kunnen.  RM volgt de aanbevolen Azure IoT-referentiearchitectuur gepubliceerd [hier](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/).  

Dit artikel worden de architecturale en technische mogelijkheden die zijn aangebracht in elk van de subsystemen voor RM beschreven en worden overwogen alternatieven besproken.  Het is belangrijk te weten dat de technische keuzen in RM gemaakt niet de enige manier zijn om een externe controle IoT-oplossing implementeren.  De technische implementatie is een basislijn voor een geslaagde toepassing bouwen en aanpassen aan de kwalificaties, ervaring en de behoeften van de verticale toepassing voor een klant-oplossing implementatie moet worden gewijzigd.

## <a name="architectural-choices"></a>Bouwkundige keuzes

### <a name="microservices-serverless-and-cloud-native"></a>Microservices zonder server, en in de cloud systeemeigen

De architectuur wordt aangeraden voor IoT-toepassingen cloud systeemeigen microservice zijn, en op basis van zonder server.  De verschillende subsystemen weer van een IoT-toepassing moeten worden samengesteld als discrete services die zijn onafhankelijk implementeerbare en kunnen afzonderlijk van elkaar schalen.  Deze kenmerken groter schaal, meer flexibiliteit bij het bijwerken van afzonderlijke subsystemen inschakelen en de flexibiliteit bieden om de juiste technologie op basis van de per-subsysteem kiezen.  Microservices kunnen worden geïmplementeerd met meerdere technologieën. Bijvoorbeeld, u container-technologie zoals Docker met zonder Server-technologie zoals Azure Functions of microservices in PaaS-services zoals Azure App Services host.

## <a name="core-subsystem-technology-choices"></a>Core-subsysteem technologie keuzes

In deze sectie beschrijft de technologie die in RM vermeld voor elk van de core subsystemen.

![Core-Diagram](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png) 

### <a name="cloud-gateway"></a>Cloudgateway
De Azure IoT Hub wordt gebruikt als de RM-Cloud-Gateway.  De IoT Hub biedt een veilige, bidirectionele communicatie met apparaten. U kunt meer informatie over IoT Hub [hier](https://azure.microsoft.com/services/iot-hub/). Voor connectiviteit van IoT-apparaten, worden de .NET Core en Java IoT Hub SDK's gebruikt.  De SDK's bieden wrappers voor de IoT Hub REST-API en ingang scenario's zoals het opnieuw 

### <a name="stream-processing"></a>Streamverwerking
Voor de stroom verwerken RM maakt gebruik van Azure Stream Analytics voor complexe regelverwerking.  Voor klanten die willen eenvoudiger regels, hebben we ook een aangepaste microservice met ondersteuning voor de verwerking van eenvoudige regels, hoewel deze configuratie geen deel uit van het buiten de implementatiefase. De referentiearchitectuur adviseert gebruik van Azure Functions voor eenvoudige regelverwerking en Azure Stream Analytics (ASA) voor complexe regelverwerking.  

### <a name="storage"></a>Storage
Voor opslag, Cosmos-database wordt gebruikt voor alle opslagbehoeften: koude opslag, warme opslag, opslag voor regels en alarmen. Er zijn momenteel bezig te verplaatsen naar Azure blob-opslag, zoals aanbevolen door de referentiearchitectuur.  Cosmos DB is de aanbevolen voor algemene doeleinden warme opslagoplossing voor IoT-toepassingen al oplossingen zoals Azure Time Series inzicht en Azure Data Lake geschikt voor veel gebruiksvoorbeelden zijn.

### <a name="business-integration"></a>Integratie van Business
Zakelijke integratie in RM is beperkt tot het genereren van waarschuwingen die in een warme opslag zijn geplaatst. Zakelijke integraties kunnen verder worden uitgevoerd door de oplossing integreren met Azure Logic Apps.

### <a name="user-interface"></a>De gebruikersinterface
Het web UI is gebouwd met JavaScript reageren.  Reageren een veelgebruikte bedrijfstak web UI-framework biedt en is vergelijkbaar met andere populaire frameworks, zoals Angular.  

### <a name="runtime-and-orchestration"></a>Runtime- en orkestratielaag
De Runtime voor de toepassing voor uitvoering in RM subsysteem gekozen is Docker-containers met Kubernetes (K8s) als de orchestrator voor horizontale schaal.  Deze architectuur kunnen voor afzonderlijke scale-definitie per subsysteem echter leidt ertoe dat DevOps-kosten in virtuele machines en containers up-to-date te houden vanuit beveiligingsoogpunt.  Alternatieven voor Docker & K8s bevatten die als host fungeert voor microservices in PaaS-services (bijvoorbeeld Azure App Service) of met behulp van Service Fabric, DCOS, Swarm, enz. Als een orchestrator.

## <a name="next-steps"></a>Volgende stappen
* Implementatie van uw oplossing RM [hier](https://www.azureiotsuite.com/).
* GitHub-code in verkennen [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) en [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* Meer informatie over de IoT Reference Architecture [hier](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/).
