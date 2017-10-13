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
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: c247a7abd3d1682f8ccd1940e506ef54f9409263
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-ibiza-portal"></a>Een gebeurtenisbron voor uw Time Series Insights-omgeving maken met de Ibiza-portal

Een Time Series Insights-gebeurtenisbron is afgeleid van een gebeurtenis-broker, zoals Azure Event Hubs. Time Series Insights maakt rechtstreeks verbinding met gebeurtenisbronnen en neemt de gegevensstroom op zonder dat gebruikers ook maar één coderegel hoeven te schrijven. Op dit moment biedt Time Series Insights ondersteuning voor Azure Event Hubs en Azure IoT Hubs. In de toekomst worden er meer gebeurtenisbronnen toegevoegd.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Stappen voor het toevoegen van een gebeurtenisbron aan uw omgeving

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

## <a name="next-steps"></a>Volgende stappen

* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron
* Uw omgeving bekijken in de [Time Series Insights-portal](https://insights.timeseries.azure.com)
