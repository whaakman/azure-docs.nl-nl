---
title: Een IoT Hub gebeurtenisbron toevoegen aan uw omgeving Azure Time Series Insights | Microsoft Docs
description: Deze zelfstudie wordt beschreven hoe u een gebeurtenisbron die is verbonden met een IoT-Hub aan uw omgeving Time Series Insights toevoegen
keywords: 
services: time-series-insights
documentationcenter: 
author: sandshadow
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/19/2017
ms.author: edett
ms.openlocfilehash: 3b98728ced81256d05b1bed2c92fc66c5ca61b98
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-iot-hub-event-source"></a>Het toevoegen van een gebeurtenisbron IoT-Hub

Deze zelfstudie wordt aangegeven hoe de Azure portal gebruiken voor het toevoegen van een gebeurtenisbron die aan uw omgeving Time Series Insights uit een IoT-Hub kan lezen.

## <a name="prerequisites"></a>Vereisten

U hebt een IoT-Hub gemaakt en worden gebeurtenissen schrijven naar het. Zie voor meer informatie over IoT Hubs <https://azure.microsoft.com/services/iot-hub/>

> [Consumergroepen] Elke keer reeks Insights gebeurtenisbron moet beschikken over een eigen speciale klantengroep die niet wordt gedeeld met andere consumenten. Als meerdere lezers gebeurtenissen van de dezelfde consumergroep gebruiken, zijn alle lezers waarschijnlijk fouten. Zie voor meer informatie de [Ontwikkelaarshandleiding voor IoT Hub](../iot-hub/iot-hub-devguide.md).

## <a name="choose-an-import-option"></a>Kies een optie importeren

De instellingen voor de gebeurtenisbron kunnen handmatig worden ingevoerd of een IoT-hub kan worden geselecteerd uit de IoT-hubs die voor u beschikbaar zijn.
In de **optie importeren** selector, kies een van de volgende opties:

* Geef de IoT Hub-instellingen handmatig
* Gebruik IoT Hub uit de beschikbare abonnementen

### <a name="select-an-available-iot-hub"></a>Selecteer een beschikbare IoT-Hub

De volgende tabel beschrijft elke optie in het tabblad nieuwe gebeurtenisbron met de beschrijving bij het selecteren van een IoT-Hub beschikbaar als een gebeurtenisbron:

| DE NAAM VAN EIGENSCHAP | BESCHRIJVING |
| --- | --- |
| De naam van de gebeurtenis-bron | De naam van de gebeurtenisbron. Deze naam moet uniek zijn binnen uw omgeving Time Series Insights.
| Bron | Kies **IoT Hub** voor het maken van een gebeurtenisbron IoT Hub.
| Abonnements-Id | Selecteer het abonnement waarin deze iothub is gemaakt.
| De naam van de IoT-hub | Selecteer de naam van de IoT-Hub.
| Naam voor het IoT-hub | Selecteer het beleid voor gedeelde toegang, die kan worden gevonden op het tabblad IoT Hub-instellingen. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **service verbinding** machtigingen.
| IoT hub klantengroep | De Consumergroep gebeurtenissen moeten worden gelezen uit de IoT-Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron.

### <a name="provide-iot-hub-settings-manually"></a>Geef de IoT Hub-instellingen handmatig

De volgende tabel beschrijft elke eigenschap in het tabblad nieuwe gebeurtenisbron met de beschrijving bij het invoeren van de instellingen handmatig:

| DE NAAM VAN EIGENSCHAP | BESCHRIJVING |
| --- | --- |
| De naam van de gebeurtenis-bron | De naam van de gebeurtenisbron. Deze naam moet uniek zijn binnen uw omgeving Time Series Insights.
| Bron | Kies **IoT Hub** voor het maken van een gebeurtenisbron IoT Hub.
| Abonnements-Id | Het abonnement waarin deze iothub is gemaakt.
| Resourcegroep | Het abonnement waarin deze iothub is gemaakt.
| De naam van de IoT-hub | De naam van uw IoT-Hub. Wanneer u uw IoT-hub gemaakt, u dit ook een specifieke naam gegeven
| Naam voor het IoT-hub | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad IoT Hub-instellingen. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **service verbinding** machtigingen.
| IoT hub beleidssleutel | De toegang tot de gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
| IoT hub klantengroep | De Consumergroep gebeurtenissen moeten worden gelezen uit de IoT-Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron.

## <a name="next-steps"></a>Volgende stappen

1. Een toegangsbeleid gegevens toevoegen aan uw omgeving [definiëren data access-beleid](time-series-insights-data-access.md)
1. Toegang tot uw omgeving in de [Time Series Insights-Portal](https://insights.timeseries.azure.com)