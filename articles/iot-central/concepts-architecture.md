---
title: Architectuur concepten in Azure IoT centrale | Microsoft Docs
description: Dit artikel bevat belangrijke concepten over de architectuur van Azure IoT centrale
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.prod: microsoft-iot-central
manager: timlt
ms.openlocfilehash: c97db3eb2c0fe1a5ec3c743ca75c595ec127823e
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="azure-iot-central-architecture"></a>Architectuur van Azure IoT-centraal

Dit artikel bevat een overzicht van de Microsoft Azure IoT Central-architectuur.

![Architectuur van het hoogste niveau](media/concepts-architecture/architecture.png)

## <a name="devices"></a>Apparaten

Apparaten uitwisselen van gegevens met uw Azure IoT centrale toepassing. Een apparaat kunt doen:

- Metingen zoals telemetrie verzenden.
- Instellingen worden gesynchroniseerd met uw toepassing.

De gegevens die een apparaat met uw toepassing uitwisselen kan is in Azure IoT centrale opgegeven in de sjabloon van een apparaat. Zie voor meer informatie over apparaat sjablonen [metagegevens management](#metadata-management).

Zie voor meer informatie over hoe apparaten verbinding met uw Azure IoT centrale toepassing maken, [connectiviteit van apparaten](concepts-connectivity.md).

## <a name="cloud-gateway"></a>Cloudgateway

Azure IoT centraal gebruikt Azure IoT Hub als een cloudgateway waarmee de connectiviteit van apparaten. Maakt gebruik van IoT-Hub:

- Gegevensopname op grote schaal in de cloud.
- Beheer van apparaten.
- Beveilig de connectiviteit van apparaten.

Zie voor meer informatie over IoT Hub, [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/).

Zie voor meer informatie over de connectiviteit van apparaten in Azure IoT centrale [connectiviteit van apparaten](concepts-connectivity.md).

## <a name="data-stores"></a>Gegevensopslag

Azure IoT centrale opslaat toepassingsgegevens in de cloud. Toepassingsgegevens opgeslagen omvat:

- Apparaat-sjablonen.
- Apparaat-id's.
- Metagegevens van apparaten.
- De gebruiker en rolautorisatie gegevens.

Azure IoT centraal maakt gebruik van een reeks opslaan tijd voor de meetgegevens van uw apparaten worden verzonden. Tijd reeksgegevens van apparaten die worden gebruikt door de analytics-service.

## <a name="analytics"></a>Analyse

De analytics-service is verantwoordelijk voor het genereren van de aangepaste rapportagegegevens die de toepassing worden weergegeven. Een operator kan [aanpassen van de analytics](howto-create-analytics.md) weergegeven in de toepassing. De Analyseservice is gebouwd boven [Azure Time Series Insights](https://azure.microsoft.com/services/time-series-insights/) en verwerkt de meetgegevens van uw apparaten worden verzonden.

## <a name="rules-and-actions"></a>Regels en acties

[Regels en acties](howto-create-telemetry-rules.md) werken nauw samen om taken in de toepassing te automatiseren. Een opbouwfunctie kunt definiëren regels op basis van telemetriegegevens, zoals de temperatuur van meer dan een opgegeven drempelwaarde. Azure IoT centraal maakt gebruik van een stroom processor om te bepalen wanneer de regelvoorwaarden wordt voldaan. Als een voorwaarde wordt voldaan, wordt een actie gedefinieerd door de opbouwfunctie voor geactiveerd. Een actie kunt bijvoorbeeld een e-mail naar een technicus de temperatuur van een apparaat is te hoog verzenden.

## <a name="metadata-management"></a>Metagegevens management

Apparaat-sjablonen definiëren in een Azure IoT centrale toepassing het gedrag en de mogelijkheid van de typen van apparaat. Een apparaat koelkast sjabloon geeft bijvoorbeeld de telemetrie die een koelkast naar uw toepassing verzendt.

![Sjabloon-architectuur](media/concepts-architecture/template_architecture.png)

In een sjabloon voor apparaat:

- **Metingen** geeft de telemetrie van het apparaat verzendt naar de toepassing.
- **Instellingen** geeft de configuraties die een operator kunt instellen.
- **Eigenschappen** metagegevens die kan worden ingesteld door een operator opgeven.
- **Regels** gedrag in de toepassing op basis van gegevens die worden verzonden vanaf een apparaat te automatiseren.
- **Dashboards** aanpasbare weergaven van een apparaat in de toepassing zijn.

Een toepassing kan een of meer gesimuleerde en echte apparaten op basis van de sjabloon van elk apparaat hebben.

## <a name="rbac"></a>RBAC

Een [beheerder kan toegangsregels definiëren](howto-administer.md) voor een Azure IoT centrale toepassing met behulp van de vooraf gedefinieerde rollen. Een beheerder kunt u gebruikers toewijzen aan rollen om te bepalen welke gebieden van de toepassing voor de gebruiker toegang heeft.

## <a name="security"></a>Beveiliging

Beveiligingsfuncties in Azure IoT centrale:

- Gegevens worden versleuteld in rust en onderweg.
- Verificatie wordt verstrekt door Azure Active Directory of het Microsoft-Account. Tweeledige verificatie wordt ondersteund.
- Volledige tenantisolatie.
- Beveiliging op apparaten.

## <a name="ui-shell"></a>UI-shell

De UI-shell is een moderne responsieve HTML5 browser gebaseerde toepassing.

## <a name="next-steps"></a>Volgende stappen

Nu dat u over de architectuur van Azure IoT centrale hebt geleerd, de voorgestelde volgende stap is te leren over [connectiviteit van apparaten](concepts-connectivity.md) in Azure IoT centrale.