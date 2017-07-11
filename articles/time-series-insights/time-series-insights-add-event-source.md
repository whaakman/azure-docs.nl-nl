---
title: Een gebeurtenisbron toevoegen aan uw Azure Time Series Insights-omgeving | Microsoft Docs
description: In deze zelfstudie koppelt u een gebeurtenisbron aan uw Time Series Insights-omgeving
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: ffa2eaf3680e68ac14aabf49b6308caeb173fd43
ms.contentlocale: nl-nl
ms.lasthandoff: 07/01/2017

---

<a id="create-an-event-source-for-your-time-series-insights-environment-using-the-ibiza-portal" class="xliff"></a>

# Een gebeurtenisbron voor uw Time Series Insights-omgeving maken met de Ibiza-portal

Een Time Series Insights-gebeurtenisbron is afgeleid van een gebeurtenis-broker, zoals Azure Event Hubs. Time Series Insights maakt rechtstreeks verbinding met gebeurtenisbronnen en neemt de gegevensstroom op zonder dat gebruikers ook maar één coderegel hoeven te schrijven. Op dit moment biedt Time Series Insights ondersteuning voor Azure Event Hubs en Azure IoT Hubs. In de toekomst worden er meer gebeurtenisbronnen toegevoegd.

<a id="steps-to-add-an-event-source-to-your-environment" class="xliff"></a>

## Stappen voor het toevoegen van een gebeurtenisbron aan uw omgeving

1.  Meld u aan bij de [Ibiza-portal](https://portal.azure.com).
2.  Klik op Alle resources in het menu aan de linkerkant van de Ibiza-portal.
3.  Selecteer uw Time Series Insights-omgeving.

  ![Een Time Series Insights-gebeurtenisbron maken](media/add-event-source/getstarted-create-event-source-1.png)

4.  Selecteer Gebeurtenisbronnen en klik op + Toevoegen.

  ![Een Time Series Insights-gebeurtenisbron maken - Details](media/add-event-source/getstarted-create-event-source-2.png)

5.  Geef de naam op van de gebeurtenisbron. Deze naam is gekoppeld aan alle gebeurtenissen afkomstig uit deze gebeurtenisbron en komt tijdens het uitvoeren van query's beschikbaar.
6.  Selecteer een Event Hub in de lijst met Event Hub-bronnen in het huidige abonnement. Kies anders de importoptie Event Hub-instellingen handmatig opgeven om een Event Hub op te geven in een ander abonnement. Gebeurtenissen moeten worden gepubliceerd in de JSON-indeling.
7.  Selecteer het beleid met leesmachtigingen voor Event Hub.
8.  Specificeer de Event Hub-consumergroep.

  > [!IMPORTANT]
  > Deze consumergroep mag niet worden gebruikt door een andere service (zoals een Stream Analytics-taak of een andere Time Series Insights-omgeving). Als de consumergroep wordt gebruikt door andere services, wordt de leesbewerking negatief beïnvloed voor deze omgeving en de andere services. Als u $Default als consumergroep selecteert, kan dit leiden tot mogelijk hergebruik door andere lezers.

9.  Klik op Maken.

Wanneer de gebeurtenisbron is gemaakt, begint Time Series Insights automatisch met het streamen van gegevens naar uw omgeving.

<a id="next-steps" class="xliff"></a>

## Volgende stappen

* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron
* Uw omgeving bekijken in de [Time Series Insights-portal](https://insights.timeseries.azure.com)

