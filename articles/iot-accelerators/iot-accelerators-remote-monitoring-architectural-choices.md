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
ms.openlocfilehash: 94641796fa77e03efc7158bc3aaf4bde9385c899
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2018
ms.locfileid: "51824265"
---
# <a name="remote-monitoring-architectural-choices"></a>Architectuurkeuzes voor externe bewaking

Azure IoT Remote Monitoring solution accelerator is een open-source, in licentie gegeven, MIT oplossingsverbetering. Om u te helpen u uw IoT-ontwikkelproces versnellen, algemene IoT-scenario's zoals u ziet:

- Connectiviteit van apparaten
- Apparaatbeheer
- Streamverwerking

De oplossing voor externe controle volgt de aanbevolen [Azure IoT-referentiearchitectuur](https://aka.ms/iotrefarchitecture).

In dit artikel beschrijft de gemaakte keuzes van architecturale en technische en de alternatieven beschouwd in elk van de subsystemen voor externe controle. De technische Microsoft gemaakt in de oplossing voor externe controle-opties zijn echter niet de enige manier om het implementeren van een externe controle IoT-oplossing. U moet de technische implementatie beschouwen als een basislijn voor het bouwen van een geslaagde toepassing en u moet deze te wijzigen:

- Aanpassen aan de beschikbare vaardigheden en ervaring in uw organisatie.
- Aan de behoeften van uw verticale toepassing.

## <a name="architectural-choices"></a>Architectuurkeuzes

De architectuur die worden aanbevolen voor een IoT-toepassing is cloud-native, microservices, en serverloze gebaseerd. U moet de verschillende subsystemen weer van een IoT-toepassing als afzonderlijke services die u kunt implementeren en schaal onafhankelijk van elkaar maken. Deze kenmerken grotere schaal, meer flexibiliteit bij het bijwerken van afzonderlijke subsystemen inschakelen en bieden de flexibiliteit om te kiezen van een juiste technologie voor elk subsysteem.

U kunt met behulp van meer dan één technologie microservices implementeren. Bijvoorbeeld, u kunt ervoor kiezen een van de volgende opties voor het implementeren van een microservice:

- Een containertechnologie, zoals Docker gebruiken met de serverloze technologie zoals Azure Functions.
- Host uw microservices in PaaS-services zoals Azure App Services.

## <a name="technology-choices"></a>Technologieopties

In deze sectie worden de technologiekeuzes gaan gemaakt in de oplossing voor externe controle voor elk van de subsystemen core.

![Core-Diagram](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png)

### <a name="cloud-gateway"></a>Cloudgateway

Azure IoT Hub wordt gebruikt als de cloudgateway oplossing voor externe controle. [IoT Hub](https://azure.microsoft.com/services/iot-hub/) biedt beveiligde, bidirectionele communicatie met apparaten.

Voor de connectiviteit van IoT-apparaten, kunt u het volgende gebruiken:

- De [IoT Hub apparaat-SDK's](../iot-hub/iot-hub-devguide-sdks.md#azure-iot-device-sdks) voor het implementeren van een systeemeigen clienttoepassing voor uw apparaat. De SDK's bieden dan geen wrappers rondom de IoT Hub REST-API en scenario's, zoals nieuwe pogingen.
- De integratie met Azure IoT Edge in de oplossingsversnellers implementeren en beheren van aangepaste modules in containers worden uitgevoerd op uw apparaten.

### <a name="stream-processing"></a>Streamverwerking

De oplossing voor externe controle wordt voor de verwerking, Azure Stream Analytics gebruikt voor de regelverwerking van complexe. Als u gebruiken eenvoudiger regels wilt, is er een aangepaste microservice met ondersteuning voor de verwerking van eenvoudige regel, hoewel deze configuratie geen deel uit van de out-of-the-box-implementatie. De referentiearchitectuur raadt Azure Functions voor eenvoudige regelverwerking en Azure Stream Analytics voor de regelverwerking van complexe.

### <a name="storage"></a>Storage

Voor opslag gebruikt de oplossingsverbetering voor externe controle zowel Azure Time Series Insights en Azure Cosmos DB. Azure Time Series Insights slaat de berichten van uw verbonden apparaten via IoT Hub. De oplossingsversnellers maakt gebruik van Azure Cosmos DB voor alle andere opslag zoals koude opslag, definities van regels, waarschuwingen en configuratie-instellingen.

Azure Cosmos DB is de aanbevolen instellingen voor de opslag voor algemeen gebruik warm-oplossing voor IoT-toepassingen al oplossingen zoals Azure Time Series Insights en Azure Data Lake geschikt voor veel van use cases zijn. Met Azure Time Series Insights krijgt u meer inzicht in uw time series-sensorgegevens door herkennen trends en afwijkingen. Deze functie kunt u oorzaak-gevolganalyses uitvoeren en kostbare stilstand te vermijden.

> [!NOTE]
> Time Series Insights is momenteel niet beschikbaar in de Azure China-cloud. Nieuwe Remote Monitoring solution accelerator implementaties in de cloud met Azure China Cosmos DB gebruiken voor alle opslag.

### <a name="business-integration"></a>Integratie van Business

Integratie van de bedrijven in de oplossing voor externe controle is beperkt tot het genereren van waarschuwingen die in de warme opslag worden geplaatst. Verbinding maken met de oplossing met Azure Logic Apps voor het implementeren van meer zakelijke integratiescenario's.

### <a name="user-interface"></a>Gebruikersinterface

De web UI is gebouwd met JavaScript reageren. React biedt een veelgebruikte branche web UI-framework en is vergelijkbaar met andere populaire frameworks zoals Angular.

### <a name="runtime-and-orchestration"></a>Runtime- en indeling

De oplossing voor externe controle maakt gebruik van Docker-containers uit te voeren van de subsystemen met Kubernetes als de orchestrator voor horizontaal schalen. Deze architectuur kunt afzonderlijke scale-definities voor elk subsysteem. Deze architectuur echter tot DevOps-kosten om te voorkomen dat de virtuele machines en containers up-to-date en beveiligd zijn.

Alternatieven voor Docker bevatten de microservices in PaaS-services zoals Azure App Service die als host fungeert. Alternatieven voor Kubernetes opnemen van orchestrators zoals Service Fabric, DC/OS of Swarm.

## <a name="next-steps"></a>Volgende stappen

* Implementeren van uw oplossing voor externe controle [hier](https://www.azureiotsolutions.com/).
* Verken code in GitHub [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) en [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* Meer informatie over de IoT-referentiearchitectuur [hier](https://aka.ms/iotrefarchitecture).
