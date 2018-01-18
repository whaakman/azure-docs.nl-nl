---
title: Overzicht van vooraf geconfigureerde Azure IoT Suite-oplossingen | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde Azure IoT Suite-oplossingen en hun architectuur, met links naar aanvullende resources.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 17/01/2018
ms.author: dobett
ms.openlocfilehash: 0ac6226b1e5773b1212b70172e2c13a0a5c925a8
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="what-is-azure-iot-suite"></a>Wat is Azure IoT Suite?

Azure IoT Suite is een verzameling van *vooraf geconfigureerde oplossingen* met de volgende eigenschappen:

* Ze implementeren binnen een paar minuten
* Ze helpen u snel aan de slag te gaan
* U kunt ze aanpassen aan uw specifieke vereisten

De vooraf geconfigureerde oplossingen van *IoT Suite* zijn ontworpen volgens dezelfde beginselen en doelstellingen.

De volgende video biedt een overzicht van de vooraf geconfigureerde oplossing voor externe controle:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Meet-the-new-Remote-Monitoring-accelerator-for-Azure-IoT/Player]

## <a name="preconfigured-solutions-overview"></a>Overzicht van vooraf geconfigureerde oplossingen

Een vooraf geconfigureerde oplossing is een open-source implementatie van algemene IoT-oplossingspatronen die u in Azure kunt implementeren met behulp van uw abonnement. Elke vooraf geconfigureerde oplossing combineert aangepaste code en Azure-services om een specifiek IoT-scenario of IoT-scenario's te implementeren. U kunt elk scenario aanpassen aan uw specifieke vereisten. Deze scenario's omvatten:

* Gegevens in een uitgebreide dashboard visualiseren voor diepe inzichten en oplossingsstatus.
* Regels en alarmen configureren via live IoT-apparaattelemetrie.
* Apparaatbeheertaken plannen, zoals updates voor software en configuratie.
* Uw eigen aangepaste fysieke of gesimuleerde apparaten inrichten.
* Problemen oplossen binnen uw IoT-apparaatgroepen.

Elke vooraf geconfigureerde oplossing is een volledige totaalimplementatie die gesimuleerde of fysieke apparaten kan gebruiken om telemetrie te genereren. U kunt de vooraf geconfigureerde oplossingen als oplossingaccelerators gebruiken voor het volgende:

* Een beginpunt voor uw eigen IoT-oplossingen bieden.
* Meer informatie krijgen over algemene patronen bij het ontwerpen en ontwikkelen van IoT-oplossingen.

Er zijn momenteel drie vooraf geconfigureerde oplossingen beschikbaar:

* [Externe bewaking](iot-suite-remote-monitoring-explore.md)
* [Predictief onderhoud](iot-suite-predictive-overview.md)
* [Verbonden factory](iot-suite-connected-factory-overview.md)

De volgende tabel toont u hoe de oplossingen aan specifieke IoT-functies zijn toegewezen:

| Oplossing | Gegevensopname | Apparaat-id | Apparaatbeheer | Randbewerking | Opdracht en controle | Regels en acties | Predictive analytics |
| ------------------------------------------------------------ | -- | -- | -- | -- | -- | -- | -- |
| [Externe bewaking](iot-suite-remote-monitoring-explore.md)  |Ja |Ja |Ja |-   |Ja |Ja |-   |
| [Predictief onderhoud](iot-suite-predictive-overview.md)   |Ja |Ja |-   |-   |Ja |Ja |Ja |
| [Verbonden factory](iot-suite-connected-factory-overview.md) |Ja |- |- |Ja |Ja |Ja |-   |

* *Gegevensopname*: instroom van gegevens op de gewenste schaal in de cloud.
* *Apparaat-id*: beheer unieke apparaat-id's en regel de toegang van apparaten tot de oplossing.
* *Apparaatbeheer*: beheer metagegevens van apparaten en voer bewerkingen uit, zoals het opnieuw opstarten van apparaten en firmware-upgrades.
* *Opdracht en controle*: verzend berichten naar een apparaat vanuit de cloud om het apparaat een actie te laten uitvoeren.
* *Regels en acties*: de back-end van de oplossing gebruikt regels om te reageren op specifieke apparaat-naar-cloudgegevens.
* *Predictive analytics*: de back-end van de oplossing analyseert apparaat-naar-cloudgegevens om te voorspellen wanneer bepaalde acties moeten plaatsvinden. Zo kan de telemetrie van vliegtuigmotoren worden geanalyseerd om te bepalen wanneer motoronderhoud is vereist.

> [!NOTE]
> Als u een vooraf geconfigureerde oplossing wilt implementeren en meer wil weten over hoe u deze kunt aanpassen, gaat u naar [Microsoft Azure IoT Suite](https://www.azureiotsuite.com/).

## <a name="azure-services"></a>Azure-services

Wanneer u een vooraf geconfigureerde oplossing implementeert, wordt in het inrichtingsproces een aantal Azure-services geconfigureerd. De volgende tabel toont de services die in de vooraf geconfigureerde oplossingen worden gebruikt:

|                      | Externe bewaking  | Voorspellend onderhoud | Verbonden factory |
| -------------------- | ------------------ | ---------------------- | ----------------- |
| IoT Hub              | Ja                |                        | Ja               |
| Event Hubs           |                    | Ja                    |                   |
| Time Series Insights |                    |                        | Ja               |
| Containerservices   | Ja                |                        |                   |
| Stream Analytics     |                    | Ja                    |                   |
| Web Apps             | Ja                | Ja                    | Ja               |
| Cosmos DB            | Ja                | Ja                    |                    |
| Azure Storage         |                    | Ja                    | Ja               |

> [!NOTE]
> Zie dit [artikel](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/blob/master/README.md#basic-vs-standard-deployments) op GitHub voor meer informatie over de resources die zijn geïmplementeerd in de vooraf geconfigureerde oplossing voor externe controle.

* [Azure IoT Hub](../iot-hub/index.md). Deze service verzorgt de berichtgeving van het apparaat naar de cloud en van de cloud naar het apparaat, en fungeert als toegangspoort tot de cloud en de andere belangrijke IoT Suite-services. Door deze service kunt u op grote schaal berichten ontvangen van uw apparaten en opdrachten naar uw apparaten verzenden. Met deze service kunt u ook [uw apparaten beheren](../iot-hub/iot-hub-device-management-overview.md). U kunt bijvoorbeeld een of meer apparaten die zijn verbonden met de hub configureren, opnieuw opstarten of hierop de fabrieksinstellingen terugzetten.
* [Azure Event Hubs](../event-hubs/index.md). Deze service biedt gebeurtenisopname naar de cloud met grote volumes. Zie [Comparison of Azure IoT Hub and Azure Event Hubs](../iot-hub/iot-hub-compare-event-hubs.md) (Vergelijking van Azure IoT Hub en Azure Event Hubs).
* [Azure Time Series Insights](../time-series-insights/index.yml). De vooraf geconfigureerde oplossingen gebruiken deze service om de telemetriegegevens van uw apparaten te analyseren en weer te geven.
* [Azure Container Service](../container-service/index.yml). Deze service host en beheert de microservices in de vooraf geconfigureerde oplossingen.
* [Azure Cosmos DB](../cosmos-db/index.yml) en [Azure Storage](../storage/index.yml) voor gegevensopslag.
* [Azure Stream Analytics](../stream-analytics/index.md). De vooraf geconfigureerde oplossing voor predictief onderhoud gebruikt deze service om binnenkomende telemetriegegevens te verwerken, aggregatiebewerkingen uit te voeren en gebeurtenissen te detecteren. Deze vooraf geconfigureerde oplossing maakt ook gebruik van Stream Analytics voor het verwerken van informatieve berichten met gegevens, zoals metagegevens of reacties van apparaten op opdrachten.
* [Azure Web Apps](../app-service/index.yml) voor het hosten van aangepaste toepassingscode in de vooraf geconfigureerde oplossingen.

Zie [Microsoft Azure en het Internet of Things (IoT)](iot-suite-what-is-azure-iot.md) voor een overzicht van de architectuur van een typische IoT-oplossing.

## <a name="whats-new-in-preconfigured-solutions"></a>Wat is er vernieuwd voor vooraf geconfigureerde oplossingen?

Microsoft werkt de vooraf geconfigureerde oplossingen bij naar een nieuwe architectuur op basis van microservices. De volgende tabel toont de huidige status van de vooraf geconfigureerde oplossingen:

| Vooraf geconfigureerde oplossing | Architectuur  | Talen     |
| ---------------------- | ------------- | ------------- |
| Externe bewaking      | Microservices | Java en .NET |
| Voorspellend onderhoud | MVC           | .NET          |
| Verbonden factory      | MVC           | .NET          |

In de volgende gedeeltes wordt beschreven wat er is vernieuwd in de vooraf geconfigureerde oplossingen op basis van microservices:

### <a name="microservices"></a>Microservices

De nieuwe versie van de vooraf geconfigureerde oplossing voor externe bewaking maakt gebruik van een architectuur op basis van microservices. Deze vooraf geconfigureerde oplossing bestaat uit meerdere microservices, zoals *IoT Hub-beheer* en *Storage-beheer*. Er zijn Java- en .NET-versies van elke microservice beschikbaar om te downloaden, samen met de bijbehorende ontwikkelaarsdocumentatie. Zie [Architectuur voor externe bewaking](iot-suite-remote-monitoring-sample-walkthrough.md) voor meer informatie over de microservices.

Deze architectuur op basis van microservices is een beproefd patroon voor cloudoplossingen met de volgende eigenschappen:

* Het is schaalbaar.
* Het maakt uitbreidbaarheid mogelijk.
* Het is eenvoudig te begrijpen.
* Het stelt u in staat afzonderlijke services voor alternatieven te wisselen.

> [!TIP]
> Zie [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-toepassingsarchitectuur) en [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: een toepassingsrevolutie aangedreven door de cloud) voor meer informatie over microservicearchitectuur.

Wanneer u de nieuwe versie van externe bewaking implementeert, moet u een van de volgende implementatieopties selecteren:

* **Basic:** voordelige versie voor een demonstratie of het testen van een implementatie. Alle microservices worden geïmplementeerd op een enkele virtuele Azure-machine.
* **Standard:** uitgebreide infrastructuurimplementatie voor het ontwikkelen van een productie-implementatie. De Azure Container Service implementeert de microservices naar meerdere virtuele Azure-machines. Kubernetes deelt de Docker-containers in die de afzonderlijke microservices hosten.

### <a name="language-choices-java-and-net"></a>Computertalen: Java en .NET

Implementaties van elk van de microservices zijn in zowel Java als .NET beschikbaar. De Java-broncode is open-source, net als de .NET-code, en u kunt deze aan uw specifieke behoeften aanpassen:

* [.NET GitHub-opslagplaats voor externe bewaking](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java GitHub-opslagplaats voor externe bewaking](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

Als u andere taalimplementaties wilt zien, voegt u een aanvraag toe aan [Azure IoT user voice](https://feedback.azure.com/forums/321918-azure-iot).

### <a name="react-user-interface-framework"></a>Framework voor gebruikersinterface van React

De gebruikersinterface is gebouwd met behulp van de JavaScript-bibliotheek van [React](https://facebook.github.io/react/). De broncode is open-source en u kunt deze downloaden en aanpassen.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een overzicht van de vooraf geconfigureerde IoT Suite-oplossingen hebt, kunt u deze voorgestelde volgende stappen voor elk van de vooraf geconfigureerde oplossingen bekijken:

* [Verken het Resource Manager-implementatiemodel van de Azure IoT Suite-oplossing voor externe bewaking](iot-suite-remote-monitoring-explore.md).
* [Overzicht van de vooraf geconfigureerde oplossing voor predictief onderhoud](iot-suite-predictive-overview.md).
* [Aan de slag met de vooraf geconfigureerde oplossing voor verbonden factory's](iot-suite-connected-factory-overview.md).

Zie [Microsoft Azure IoT services: Reference Architecture](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf) (Microsoft Azure IoT-services: referentiearchitectuur) voor meer informatie over IoT-oplossingsarchitecturen.
