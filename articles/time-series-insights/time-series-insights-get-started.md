---
title: Een Azure Time Series Insights-omgeving maken | Microsoft Docs
description: In deze zelfstudie leert u hoe een Time Series-omgeving maakt, deze verbindt met een gebeurtenisbron en binnen enkele minuten klaar bent om uw gebeurtenisgegevens te analyseren.
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
ms.date: 10/18/2017
ms.author: omravi
ms.openlocfilehash: d8a3f79630868c83cd9fde6ea0c414e334a58e22
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Een nieuwe Time Series Insights-omgeving maken in Azure Portal

Een Time Series Insights-omgeving is een Azure-resource met inkomend verkeer en opslagcapaciteit. Klanten richten via Azure Portal omgevingen in met de vereiste capaciteit.

## <a name="steps-to-create-the-environment"></a>Stappen voor het maken van de omgeving

Volg deze stappen voor het maken van uw omgeving:

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).
2.  Klik op het plusteken (+) in de linkerbovenhoek.
3.  Zoek via het zoekvak naar 'Time Series Insights'.

  ![De Time Series Insights-omgeving maken](media/get-started/getstarted-create-environment1.png)

4.  Selecteer Time Series Insights en klik op Maken.

  ![De Time Series Insights-resourcegroep maken](media/get-started/getstarted-create-environment2.png)

5.  Geef de naam van de omgeving op. Deze naam vertegenwoordigt de omgeving in [time series explorer](https://insights.timeseries.azure.com).
6.  Selecteer een abonnement. Kies een abonnement dat uw gebeurtenisbron bevat. Time Series Insights kan Azure IoT Hub- en Event Hub-resources die in hetzelfde abonnement aanwezig zijn, automatisch detecteren.
7.  Selecteer of maak een resourcegroep. Een resourcegroep is een verzameling Azure-resources die samen worden gebruikt.
8.  Selecteer een hostinglocatie. Kies een locatie die uw gebeurtenisbron bevat om te voorkomen dat gegevens tussen datacenters worden verplaatst.
9.  Selecteer een prijscategorie.
10. Selecteer de capaciteit. U kunt de capaciteit van een omgeving wijzigen nadat deze is gemaakt.
11. Maak uw omgeving. U kunt uw omgeving voor het dashboard vastmaken aan het dashboard voor eenvoudige toegang wanneer u zich aanmeldt.

  ![Time Series Insights vastmaken aan het dashboard](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Volgende stappen

* [Gegevenstoegangsbeleid definiëren](time-series-insights-data-access.md) voor toegang tot uw omgeving in [Time Series Insights-portal](https://insights.timeseries.azure.com)
* [Een gebeurtenisbron maken](time-series-insights-add-event-source.md)
* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron
