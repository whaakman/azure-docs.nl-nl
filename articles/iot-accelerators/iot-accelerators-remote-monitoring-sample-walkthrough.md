---
title: Externe bewaking overzicht van de oplossingsversneller - Azure | Microsoft Docs
description: Een overzicht van de oplossingsverbetering voor externe controle.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: dfe584532efeab1dbc0d2928b7afb0a6695a21ee
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39184942"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Externe bewaking accelerator overzicht van de oplossing

Externe bewaking [oplossingsverbetering](../iot-accelerators/about-iot-accelerators.md) een end-to-end oplossing voor prestatiecontrole voor meerdere machines geïmplementeerd op verafgelegen locaties bevinden. De oplossing combineert belangrijke Azure-services voor een algemene implementatie van het bedrijfsscenario. U kunt de oplossing gebruiken als uitgangspunt voor uw eigen implementatie en [aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) te voldoen aan uw eigen specifieke zakelijke vereisten.

Dit artikel begeleidt u bij sommige van de belangrijkste elementen van de oplossing voor externe controle zodat u begrijpt hoe deze werkt. Deze kennis helpt u bij:

* Het oplossen van problemen met de oplossing.
* Het plannen van een aanpassing van de oplossing zodat deze voldoet aan uw eigen specifieke vereisten.
* Het ontwerpen van uw eigen IoT-oplossing die gebruikmaakt van Azure-services.

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de oplossing voor externe controle accelerator overlay bekijken op de [IoT-architectuur](../iot-fundamentals/iot-introduction.md):

![Logische architectuur](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Waarom microservices?

Cloudarchitectuur heeft zich ontwikkeld sinds de eerste oplossingsversnellers de release van Microsoft. [Microservices](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) hebben naar voren gekomen als een bewezen praktijken om schaal en flexibiliteit zonder dat dit ten koste gaat van de ontwikkelingssnelheid van de. Verschillende Microsoft-services gebruiken dit architectuurpatroon intern met geweldige betrouwbaarheid en schaalbaarheid resultaten. De bijgewerkte oplossingsversnellers plaats deze geleerde lessen in de praktijk, zodat u ook van deze profiteren kunt.

> [!TIP]
> Zie [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-toepassingsarchitectuur) en [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: een toepassingsrevolutie aangedreven door de cloud) voor meer informatie over microservicearchitectuur.

## <a name="device-connectivity"></a>Connectiviteit van apparaten

De oplossing omvat de volgende onderdelen in het gedeelte van de connectiviteit apparaat van de logische architectuur:

### <a name="simulated-devices"></a>Gesimuleerde apparaten

De oplossing bevat een microservice waarmee u voor het beheren van een groep gesimuleerde apparaten de end-to-end-stroom testen in de oplossing. De gesimuleerde apparaten:

* Apparaat-naar-cloud telemetrie genereren.
* Reageren op cloud-naar-apparaat methodeaanroepen van IoT-Hub.

De microservices biedt een RESTful-eindpunt kunt maken, starten en stoppen van simulaties. Elke simulatie bestaat uit een set virtuele apparaten van verschillende typen, die telemetrie verzenden en reageren op methodeaanroepen.

U kunt gesimuleerde apparaten vanuit het dashboard in de portal van de oplossing inrichten.

### <a name="physical-devices"></a>Fysieke apparaten

U kunt fysieke apparaten verbinden met de oplossing. U kunt het gedrag van uw gesimuleerde apparaten met behulp van de Azure IoT device SDK's kunt implementeren.

U kunt fysieke apparaten vanuit het dashboard in de portal van de oplossing inrichten.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>IoT Hub en de IoT-manager microservice

De [IoT-hub](../iot-hub/index.yml) neemt gegevens op die van de apparaten worden verzonden naar de cloud en maakt ze beschikbaar voor de `telemetry-agent` microservice.

De IoT Hub in de oplossing doet ook het volgende:

* Een identiteitsregister waarin de id's en verificatiesleutels van de apparaten die verbinding mogen maken met de portal. U kunt apparaten in- en uitschakelen via het register van identiteiten.
* Roept namens de oplossingsportal methoden aan op uw apparaten.
* Onderhoudt apparaatdubbels voor alle geregistreerde apparaten. Een apparaatdubbel slaat de eigenschapswaarden op die door een apparaat worden gerapporteerd. Een apparaatdubbel slaat ook gewenste eigenschappen op die in de oplossingsportal zijn ingesteld, zodat het apparaat deze kan ophalen wanneer opnieuw verbinding wordt maakt.
* Plant taken voor het instellen van eigenschappen voor meerdere apparaten of voor het aanroepen van methoden op meerdere apparaten.

De oplossing omvat de `iot-manager` microservice voor het afhandelen van interacties met uw IoT-hub, zoals:

* Het maken en beheren van IoT-apparaten.
* Dubbele apparaten beheren.
* Aanroepen van methoden op apparaten.
* Het beheren van IoT-referenties.

Deze service wordt ook uitgevoerd IoT Hub query's om op te halen die behoren tot de gebruiker gedefinieerde groepen apparaten.

De microservices biedt een RESTful-eindpunt voor het beheren van apparaten en apparaatdubbels, aanroepen van methoden en IoT Hub-query's uitvoeren.

## <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

De oplossing omvat de volgende onderdelen in de gegevensverwerking en analytics deel uit van de logische architectuur:

### <a name="device-telemetry"></a>Telemetrie van apparaten

De oplossing bevat twee microservices voor het afhandelen van telemetrie van apparaten.

De [telemetrie-agent](https://github.com/Azure/telemetry-agent-dotnet) microservice:

* Telemetrie in Azure Cosmos DB opgeslagen.
* Analyseert de telemetriestroom vanaf de apparaten.
* Genereert waarschuwingen op basis van gedefinieerde regels.

De waarschuwingen worden opgeslagen in Azure Cosmos DB.

De [telemetrie-agent](https://github.com/Azure/telemetry-agent-dotnet) microservice kunt de portal van de oplossing te lezen van de telemetrie die is verzonden vanaf de apparaten. De portal van de oplossing gebruikt ook deze service:

* Regels voor bewaking, zoals de drempelwaarden die waarschuwingen definiëren
* De lijst van eerdere waarschuwingen worden opgehaald.

Gebruik de RESTful-eindpunt geleverd door deze microservice voor het beheren van telemetrie, regels en alarmen.

### <a name="storage"></a>Storage

De [opslagadapter](https://github.com/Azure/pcs-storage-adapter-dotnet) microservice is een adapter voor de belangrijkste storage-service gebruikt voor de solution accelerator. Het biedt eenvoudige verzameling en -sleutel / waarde-opslag.

De standaardimplementatie van de solution accelerator maakt gebruik van Azure Cosmos DB als de belangrijkste storage-service.

De Azure Cosmos DB-database worden gegevens opgeslagen in de solution accelerator. De **opslagadapter** microservice fungeert als een adapter voor de andere microservices in de oplossing voor toegang tot opslag-services.

## <a name="presentation"></a>Presentatie

De oplossing omvat de volgende onderdelen in het gedeelte van de presentatie van de logische architectuur:

De [online gebruikersinterface is een toepassing reageren Javascript](https://github.com/Azure/pcs-remote-monitoring-webui). De toepassing:

* Maakt alleen gebruik Javascript reageren en volledig in de browser wordt uitgevoerd.
* Is opgemaakt met CSS.
* Communiceert met openbare gerichte microservices via AJAX-aanroepen.

De gebruikersinterface geeft alle functionaliteit van de solution accelerator en communiceert met andere services, zoals:

* De [verificatie](https://github.com/Azure/pcs-auth-dotnet) microservice om gebruikersgegevens te beschermen.
* De [iothub-manager](https://github.com/Azure/iothub-manager-dotnet) microservice weergeven en beheren van de IoT-apparaten.

De [ui-config](https://github.com/Azure/pcs-config-dotnet) microservice kunt u de gebruikersinterface voor het opslaan en ophalen van configuratie-instellingen.

## <a name="next-steps"></a>Volgende stappen

Als u de documentatie van de bron-code en ontwikkelaars verkennen wilt, begint u met een de twee belangrijkste GitHub-opslagplaatsen:

* [De oplossingsversneller voor externe controle met Azure IoT (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [De oplossingsversneller voor externe controle met Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Gedetailleerde oplossing architectuurdiagrammen:
* [De oplossingsversneller voor externe controle architectuur](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Zie voor meer informatie over de oplossingsverbetering voor externe controle [de oplossingsversnellers aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
