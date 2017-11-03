---
title: Een Event Hub gebeurtenisbron toevoegen aan uw omgeving Azure Time Series Insights | Microsoft Docs
description: Deze zelfstudie wordt beschreven hoe u een gebeurtenisbron die is verbonden met een Event Hub aan uw omgeving Time Series Insights toevoegen
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
ms.openlocfilehash: f6a993b3858cfb94dd9795f5e55f15fa6ec7dcb2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-add-an-event-hub-event-source"></a>Het toevoegen van een gebeurtenisbron Event Hub

Deze zelfstudie wordt aangegeven hoe de Azure portal gebruiken om toe te voegen een gebeurtenisbron die aan uw omgeving Time Series Insights uit een Event Hub kan lezen.

## <a name="prerequisites"></a>Vereisten

U hebt gemaakt van een Event Hub en worden gebeurtenissen schrijven naar het. Zie voor meer informatie over Event Hubs <https://azure.microsoft.com/services/event-hubs/>

> [Consumergroepen] Elke keer reeks Insights gebeurtenisbron moet beschikken over een eigen speciale klantengroep die niet wordt gedeeld met andere consumenten. Als meerdere lezers gebeurtenissen van de dezelfde consumergroep gebruiken, zijn alle lezers waarschijnlijk fouten. Houd er rekening mee dat er ook een limiet van 20 consumergroepen per Event Hub is. Zie voor meer informatie de [Event Hubs-programmeergids](../event-hubs/event-hubs-programming-guide.md).

## <a name="choose-an-import-option"></a>Kies een optie importeren

De instellingen voor de gebeurtenisbron kunnen handmatig worden ingevoerd of een event hub kan worden geselecteerd uit de event-hubs die voor u beschikbaar zijn.
In de **optie importeren** selector, kies een van de volgende opties:

* Geef de Event Hub-instellingen handmatig
* Gebruik Event Hub uit de beschikbare abonnementen

### <a name="select-an-available-event-hub"></a>Selecteer een beschikbare Event Hub

De volgende tabel beschrijft elke optie in het tabblad nieuwe gebeurtenisbron met de beschrijving bij het selecteren van een beschikbare Event Hub als een gebeurtenisbron:

| DE NAAM VAN EIGENSCHAP | BESCHRIJVING |
| --- | --- |
| De naam van de gebeurtenis-bron | De naam van de gebeurtenisbron. Deze naam moet uniek zijn binnen uw omgeving Time Series Insights.
| Bron | Kies **Event Hub** voor het maken van een gebeurtenisbron Event Hub.
| Abonnements-Id | Selecteer het abonnement waarin deze event hub is gemaakt.
| Service bus-naamruimte | Selecteer de Service Bus-naamruimte die de Event Hub bevat.
| Naam Event hub | Selecteer de naam van de Event Hub.
| Naam Event hub-beleid | Selecteer het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad Event Hub configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **lezen** machtigingen.
| Event hub klantengroep | De Consumergroep gebeurtenissen moeten worden gelezen van de Event Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron.

### <a name="provide-event-hub-settings-manually"></a>Geef de Event Hub-instellingen handmatig

De volgende tabel beschrijft elke eigenschap in het tabblad nieuwe gebeurtenisbron met de beschrijving bij het invoeren van de instellingen handmatig:

| DE NAAM VAN EIGENSCHAP | BESCHRIJVING |
| --- | --- |
| De naam van de gebeurtenis-bron | De naam van de gebeurtenisbron. Deze naam moet uniek zijn binnen uw omgeving Time Series Insights.
| Bron | Kies **Event Hub** voor het maken van een gebeurtenisbron Event Hub.
| Abonnements-Id | Het abonnement waarin deze event hub is gemaakt.
| Resourcegroep | Het abonnement waarin deze event hub is gemaakt.
| Service bus-naamruimte | Een Service Bus-naamruimte is een container voor een set berichtentiteiten. Wanneer u een nieuwe Event Hub hebt gemaakt, hebt u ook een Service Bus-naamruimte gemaakt.
| Naam Event hub | De naam van uw Event Hub. Wanneer u uw event hub hebt gemaakt, u deze ook een specifieke naam gegeven
| Naam Event hub-beleid | Het beleid voor gedeelde toegang, die kan worden gemaakt op het tabblad Event Hub configureren. Elk gedeeld toegangsbeleid heeft een naam, machtigingen die u instelt en toegangssleutels. Het beleid voor gedeelde toegang voor de gebeurtenisbron *moet* hebben **lezen** machtigingen.
| Event hub beleidssleutel | De toegang tot de gedeelde sleutel gebruikt voor het verifiëren van toegang tot de Service Bus-naamruimte. Voer de primaire of secundaire sleutel hier in.
| Event hub klantengroep | De Consumergroep gebeurtenissen moeten worden gelezen van de Event Hub. Het is raadzaam een speciale klantengroep gebruiken voor de gebeurtenisbron.

## <a name="next-steps"></a>Volgende stappen

1. Een toegangsbeleid gegevens toevoegen aan uw omgeving [definiëren data access-beleid](time-series-insights-data-access.md)
1. Toegang tot uw omgeving in de [Time Series Insights-Portal](https://insights.timeseries.azure.com)