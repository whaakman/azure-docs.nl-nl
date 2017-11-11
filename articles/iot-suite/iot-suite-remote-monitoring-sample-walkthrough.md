---
title: Architectuur van de oplossing voor externe controle - Azure | Microsoft Docs
description: Een overzicht van de architectuur van de vooraf geconfigureerde oplossing voor externe controle.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: e19ba9c88e4fbe4f065c45ce7029247436f7155c
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="remote-monitoring-preconfigured-solution-architecture"></a>Externe controle vooraf geconfigureerde oplossingsarchitectuur

IoT Suite remote monitoring [vooraf geconfigureerde oplossing](iot-suite-what-are-preconfigured-solutions.md) implementeert u een oplossing voor de end-to-end-controle voor meerdere machines op externe locaties. De oplossing combineert belangrijke Azure-services voor een algemene implementatie van het bedrijfsscenario. U kunt de oplossing gebruiken als een beginpunt voor uw eigen implementatie en [aanpassen](iot-suite-remote-monitoring-customize.md) om te voldoen aan uw eigen specifieke zakelijke vereisten.

In dit artikel wordt stapsgewijs een aantal belangrijke elementen van externe controle beschreven, zodat u beter begrijpt hoe dit werkt. Deze kennis helpt u bij:

* Het oplossen van problemen met de oplossing.
* Het plannen van een aanpassing van de oplossing zodat deze voldoet aan uw eigen specifieke vereisten.
* Het ontwerpen van uw eigen IoT-oplossing die gebruikmaakt van Azure-services.

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing voor externe controle heen worden weergegeven op de [IoT-architectuur](iot-suite-what-is-azure-iot.md):

![Logische architectuur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Waarom microservices?

Cloudarchitectuur heeft ontwikkeld sinds de eerste vooraf geconfigureerde oplossingen de release van Microsoft. [Microservices](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) beproefde uit veiligheidsoverwegingen als u de schaal en flexibiliteit zonder verlies van ontwikkelingssnelheid is ontstaan. Diverse Microsoft-services gebruiken dit patroon van een architectuur intern met betrouwbaar en schaalbaarheid resultaten. De bijgewerkte vooraf geconfigureerde oplossingen plaatsen deze geleerde lessen concreet, zodat u ook van deze profiteren kunt.

> [!TIP]
> Zie [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-toepassingsarchitectuur) en [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: een toepassingsrevolutie aangedreven door de cloud) voor meer informatie over microservicearchitectuur.

## <a name="device-connectivity"></a>Connectiviteit van apparaten

De oplossing omvat de volgende onderdelen in het apparaat verbinding deel van de logische architectuur:

### <a name="simulated-devices"></a>Gesimuleerde apparaten

De oplossing omvat een microservice waarmee u kunt een pool van de gesimuleerde apparaten voor het testen van de stroom end-to-end in de oplossing beheren. De gesimuleerde apparaten:

* Apparaat-naar-cloud telemetrie genereren.
* Reageren op cloud-naar-apparaat methodeaanroepen van IoT-Hub.

De microservice biedt een RESTful-eindpunt kunt maken, starten en stoppen simulaties. Elke simulatie bestaat uit een set van virtuele apparaten van verschillende typen, die telemetrie verzenden en reageren op aanroepen.

U kunt gesimuleerde apparaten vanuit het dashboard in de oplossingsportal inrichten.

### <a name="physical-devices"></a>Fysieke apparaten

U kunt fysieke apparaten verbinden met de oplossing. U kunt het gedrag van uw gesimuleerde apparaten met behulp van het apparaat met Azure IoT SDK's implementeren.

U kunt fysieke apparaten vanuit het dashboard in de oplossingsportal inrichten.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>IoT Hub en de microservice IoT-manager

De [IoT-hub](../iot-hub/index.md) opgenomen gegevens die worden verzonden vanaf de apparaten in de cloud en maakt het beschikbaar is voor de `telemetry-agent` microservice.

De IoT Hub in de oplossing doet ook het volgende:

* Onderhoudt een register-id's die de id's en verificatiesleutels van alle apparaten die verbinding mogen maken met de portal worden opgeslagen. U kunt apparaten in- en uitschakelen via het register van identiteiten.
* Roept namens de oplossingsportal methoden aan op uw apparaten.
* Onderhoudt apparaatdubbels voor alle geregistreerde apparaten. Een apparaatdubbel slaat de eigenschapswaarden op die door een apparaat worden gerapporteerd. Een apparaatdubbel slaat ook gewenste eigenschappen op die in de oplossingsportal zijn ingesteld, zodat het apparaat deze kan ophalen wanneer opnieuw verbinding wordt maakt.
* Plant taken voor het instellen van eigenschappen voor meerdere apparaten of voor het aanroepen van methoden op meerdere apparaten.

De oplossing omvat de `iot-manager` microservice voor het afhandelen van interactie met uw IoT-hub, zoals:

* Maken en beheren van IoT-apparaten.
* Horende apparaten beheren.
* Aanroepen van methoden op apparaten.
* Het beheren van IoT-referenties.

Deze service voert IoT Hub ook query's voor het ophalen van de apparaten die horen bij de gebruiker gedefinieerde groepen.

De microservice biedt een RESTful-eindpunt voor het beheren van apparaten en apparaten horende, methoden aanroepen en IoT Hub-query's uitvoeren.

## <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

De oplossing omvat de volgende onderdelen in de gegevensverwerking en analytics onderdeel van de logische architectuur:

### <a name="device-telemetry"></a>De apparaattelemetrie

De oplossing omvat twee microservices voor het afhandelen van apparaattelemetrie.

De [telemetrie-agent](https://github.com/Azure/telemetry-agent-dotnet) microservice:

* Telemetrie in Cosmos-database opgeslagen.
* Analyseert de telemetriestroom van apparaten.
* Genereert alarmen volgens de gedefinieerde regels.

De alarmen worden opgeslagen in een Cosmos-DB.

De `telemetry-agent` microservice maakt de oplossingsportal lezen van de telemetrie die is verzonden vanaf de apparaten. De oplossingsportal wordt ook gebruikt voor deze service:

* Bewaking regels, zoals de drempelwaarden die alarmen activeren definiëren
* De lijst van afgelopen alarmen opgehaald.

Gebruik de RESTful-eindpunt geleverd door deze microservice telemetrie, regels en alarmen beheren.

### <a name="storage"></a>Storage

De [opslagadapter](https://github.com/Azure/pcs-storage-adapter-dotnet) microservice is een adapter voor de belangrijkste storage-service gebruikt voor de vooraf geconfigureerde oplossing. Het biedt coolbarsimple-verzameling en sleutel / waarde-opslag.

De standaardimplementatie van de vooraf geconfigureerde oplossing maakt gebruik van Cosmos DB als de belangrijkste storage-service.

De database van de Cosmos-DB opslaat-gegevens in de vooraf geconfigureerde oplossing. De **opslagadapter** microservice fungeert als een adapter voor de andere microservices in de oplossing voor toegang tot opslagservices.

## <a name="presentation"></a>Presentatie

De oplossing omvat de volgende onderdelen in het gedeelte presentatie van de logische architectuur:

De [online gebruikersinterface is een toepassing reageren Javascript](https://github.com/Azure/pcs-remote-monitoring-webui). De toepassing:

* Alleen wordt gebruikt door Javascript reageren en volledig in de browser wordt uitgevoerd.
* Is opgemaakt met CSS.
* Communiceert met openbare gerichte microservices via AJAX-aanroepen.

De gebruikersinterface geeft alle functionaliteit van de vooraf geconfigureerde oplossing en communiceert met andere services, zoals:

* De [verificatie](https://github.com/Azure/pcs-auth-dotnet) microservice om gebruikersgegevens te beschermen.
* De [iothub-manager](https://github.com/Azure/iothub-manager-dotnet) microservice weergeven en beheren van de IoT-apparaten.

De [ui-config](https://github.com/Azure/pcs-config-dotnet) microservice kunt u de gebruikersinterface voor het opslaan en ophalen van configuratie-instellingen.

## <a name="next-steps"></a>Volgende stappen

Als u de bron-documentatie voor code- en developer verkennen wilt, beginnen met een de twee belangrijkste GitHub-opslagplaatsen:

* [Vooraf geconfigureerde oplossing voor externe controle met Azure IoT (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [Vooraf geconfigureerde oplossing voor externe controle met Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).
* [Vooraf geconfigureerde oplossing voor externe controle architectuur)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Zie voor meer informatie over de vooraf geconfigureerde oplossing voor externe controle [aanpassen van de vooraf geconfigureerde oplossing](iot-suite-remote-monitoring-customize.md).
