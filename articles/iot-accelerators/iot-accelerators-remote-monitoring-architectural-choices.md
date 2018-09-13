---
title: Externe bewaking oplossing architectuurkeuzes - Azure | Microsoft Docs
description: In dit artikel worden de architecturale en technische mogelijkheden die in externe controle beschreven
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/12/2018
ms.topic: conceptual
ms.openlocfilehash: 709886e77819adca961a44f65fe6402dd7d20d53
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716295"
---
# <a name="remote-monitoring-architectural-choices"></a>Architectuurkeuzes voor externe bewaking

Azure IoT Remote Monitoring solution accelerator is een open-source, MIT-licentie, oplossingsverbetering introductie van algemene IoT-scenario's zoals connectiviteit van apparaten, beheer van apparaten en stream verwerking, zodat klanten kunnen hun ontwikkeling versnellen het proces.  De oplossing voor externe controle volgt de aanbevolen Azure-IoT-referentiearchitectuur gepubliceerd [hier](https://aka.ms/iotrefarchitecture).  

In dit artikel worden de architecturale en technische mogelijkheden die zijn gemaakt in elk van de subsystemen voor de oplossing voor externe controle beschreven, en alternatieven beschouwd.  Het is belangrijk te weten dat de technische keuzen gemaakt in de oplossing voor externe controle niet de enige manier zijn om het implementeren van een externe controle IoT-oplossing.  De technische implementatie is een basislijn voor het bouwen van een geslaagde toepassing en past de vaardigheden, ervaring en verticale toepassingsbehoeften voor implementatie van een klant de oplossing moet worden gewijzigd.

## <a name="architectural-choices"></a>Architectuurkeuzes

### <a name="microservices-serverless-and-cloud-native"></a>Microservices, serverloos, en cloud-native

De architectuur die wordt aangeraden voor IoT-toepassingen cloud-native, microservices zijn, en op basis van serverloze.  De verschillende subsystemen weer van een IoT-toepassing moeten worden gebouwd als afzonderlijke services onafhankelijk implementeerbare en kunnen onafhankelijk worden geschaald.  Deze kenmerken grotere schaal, meer flexibiliteit bij het bijwerken van afzonderlijke subsystemen inschakelen en bieden de flexibiliteit om te kiezen van de juiste technologie op basis van per subsysteem.  Microservices kunnen worden geïmplementeerd met meerdere technologieën. Bijvoorbeeld, met behulp van containertechnologie, zoals Docker met serverloze technologie zoals Azure Functions of die als host fungeert voor microservices in PaaS-services zoals Azure App Services.

## <a name="core-subsystem-technology-choices"></a>Technologieopties Core-subsysteem

In deze sectie worden de technologiekeuzes gaan gemaakt in de oplossing voor externe controle voor elk van de subsystemen core.

![Core-Diagram](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png) 

### <a name="cloud-gateway"></a>Cloudgateway
De Azure IoT Hub wordt gebruikt als de cloudgateway oplossing voor externe controle.  De IoT-Hub biedt beveiligde, bidirectionele communicatie met apparaten. U kunt meer informatie over IoT-Hub [hier](https://azure.microsoft.com/services/iot-hub/). Voor de connectiviteit van IoT-apparaten, worden de .NET Core- en Java IoT-Hub SDK's gebruikt.  De SDK's bieden dan geen wrappers rondom de IoT Hub REST-API en scenario's, zoals nieuwe pogingen.

### <a name="stream-processing"></a>Streamverwerking
Voor stream verwerken van de oplossing voor externe controle maakt gebruik van Azure Stream Analytics voor de regelverwerking van complexe.  Voor klanten die willen eenvoudiger regels, hebben we ook een aangepaste microservice met ondersteuning voor de verwerking van eenvoudige regels, hoewel deze configuratie geen deel uit van de kant van de implementatiefase. De referentiearchitectuur wordt aanbevolen voor gebruik van Azure Functions voor eenvoudige regelverwerking en Azure Stream Analytics (ASA) voor de regelverwerking van complexe.  

### <a name="storage"></a>Storage
Voor opslag gebruikt de oplossingsverbetering voor externe controle zowel Azure Time Series Insights en Azure Cosmos DB. Azure Time Series Insights slaat de berichten van uw verbonden apparaten via IoT Hub. De oplossingsversnellers maakt gebruik van Azure Cosmos DB voor alle andere opslag zoals koude opslag, definities van regels, waarschuwingen en configuratie-instellingen. Azure Cosmos DB is de aanbevolen instellingen voor de opslag voor algemeen gebruik warm-oplossing voor IoT-toepassingen al oplossingen zoals Azure Time Series Insights en Azure Data Lake geschikt voor veel van use cases zijn. Met Azure Time Series Insights krijgt u meer inzicht in uw time series-sensorgegevens door het ontdekken van trends en afwijkingen, zodat u kunt oorzaak-gevolganalyses uitvoeren en kostbare stilstand te vermijden. 

> [!NOTE]
> Azure Time Series Insights voor de oplossingsverbetering voor externe controle is momenteel in preview en is alleen beschikbaar in [regio's selecteren](https://azure.microsoft.com/global-infrastructure/services/). Als u de oplossingsverbetering voor externe bewaking buiten deze regio's implementeert, Cosmos DB de is de standaardoptie voor opslag.

### <a name="business-integration"></a>Integratie van Business
Integratie van de bedrijven in de oplossing voor externe controle is beperkt tot het genereren van waarschuwingen die in de warme opslag worden geplaatst. Zakelijke integraties kunnen verder worden uitgevoerd door de oplossing te integreren met Azure Logic Apps.

### <a name="user-interface"></a>Gebruikersinterface
De web UI is gebouwd met JavaScript reageren.  React biedt een veelgebruikte branche web UI-framework en is vergelijkbaar met andere populaire frameworks zoals Angular.  

### <a name="runtime-and-orchestration"></a>Runtime- en indeling
De runtime voor de toepassing voor het subsysteem voor implementatie in de oplossing voor externe controle hebt gekozen, is Docker-containers met Kubernetes als de orchestrator voor horizontaal schalen.  Deze architectuur kunt voor de definitie van de afzonderlijke schaal per subsysteem DevOps-kosten in virtuele machines en containers up-to-date te houden vanuit het beveiligingsoogpunt echter worden in rekening gebracht.  Alternatieven voor Docker en Kubernetes bevatten die als host fungeert voor microservices in PaaS-services (bijvoorbeeld Azure App Service) of met behulp van Service Fabric, DCOS, Swarm, enz. Als een orchestrator.

## <a name="next-steps"></a>Volgende stappen
* Implementeren van uw oplossing voor externe controle [hier](https://www.azureiotsolutions.com/).
* Verken code in GitHub [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) en [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* Meer informatie over de IoT-referentiearchitectuur [hier](https://aka.ms/iotrefarchitecture).
