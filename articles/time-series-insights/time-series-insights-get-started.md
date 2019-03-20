---
title: Een Azure Time Series Insights-omgeving maken | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Azure portal gebruiken om een nieuwe Time Series Insights-omgeving te maken.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.custom: seodec18
ms.openlocfilehash: b18ae44a140d77c24d8b5741bc09740767d1344f
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58075949"
---
# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Een nieuwe Time Series Insights-omgeving maken in Azure Portal
In dit artikel wordt beschreven hoe u een nieuwe Time Series Insights-omgeving maken met de Azure-portal.

Time Series Insights kunt u aan de slag te visualiseren en opvragen van gegevens die binnenkomen via Azure IoT Hubs en Event Hubs in minuten, zodat u grote volumes van de query van time series-gegevens in een paar seconden.  Het is ontworpen voor de schaal van internet of things (IoT) en terabytes aan gegevens kunt verwerken.

## <a name="steps-to-create-the-environment"></a>Stappen voor het maken van de omgeving
Volg deze stappen voor het maken van een omgeving:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer de **+ nieuw** knop.

3. Selecteer de **Internet of Things** categorie en selecteer **Time Series Insights**.

   ![De Time Series Insights-omgeving maken](media/time-series-insights-get-started/1-new-tsi.png)

4. Op de **Time Series Insights** weergeeft, schakelt **maken**.

5. Vul de vereiste parameters. De volgende tabel beschrijft elke parameter:
   
   ![De Time Series Insights-resourcegroep maken](media/time-series-insights-get-started/2-create-tsi.png)
   
   Instelling|Voorgestelde waarde|Description
   ---|---|---
   Naam van de omgeving | Een unieke naam | Deze naam vertegenwoordigt de omgeving in [time series explorer](https://insights.timeseries.azure.com)
   Abonnement | Uw abonnement | Als u meerdere abonnementen hebt, kiest u het abonnement dat bij voorkeur uw gebeurtenisbron bevat. Time Series Insights kan Azure IoT Hub- en Event Hub-resources die in hetzelfde abonnement aanwezig zijn, automatisch detecteren.
   Resourcegroep | Maak een nieuwe of bestaande gebruiken | Een resourcegroep is een verzameling Azure-resources die samen worden gebruikt. U kunt een bestaande resourcegroep, bijvoorbeeld de map die met uw Event Hub of IoT-Hub. Of u kunt een nieuwe maken als deze resource is niet gerelateerd aan de andere resources.
   Locatie | Dichtst bij uw gebeurtenisbron | Kies bij voorkeur de dezelfde datacenterlocatie waarin uw gebeurtenisgegevens bron in inspanningen om te voorkomen dat regio-overschrijdende en zoneoverschrijdende bandbreedtekosten toegevoegd en latentie toegevoegd bij het verplaatsen van gegevens buiten de regio.
   Prijscategorie | S1 | Kies de benodigde doorvoer. Selecteer voor de laagste kosten en starter capaciteit, S1.
   Capaciteit | 1 | Capaciteit is dat de vermenigvuldiger is van toepassing op de gegevens, capaciteit en kosten die gepaard gaan met de geselecteerde SKU.  U kunt de capaciteit van een omgeving wijzigen nadat deze is gemaakt. Voor de laagste kosten, selecteert u een capaciteit van 1. 
  
6. Controleer **vastmaken aan dashboard** meest eenvoudige toegang tot uw Time Series-omgeving in de toekomst.

   ![Time Series Insights vastmaken aan het dashboard](media/time-series-insights-get-started/3-pin-create.png)

7. Selecteer **maken** om te beginnen met het inrichtingsproces. Het kan enkele minuten duren.

8. Voor het controleren van het implementatieproces, selecteer de **meldingen** symbool (belpictogram).

   ![De meldingen bekijken](media/time-series-insights-get-started/4-notifications.png)

Wanneer de implementatie is gelukt, kunt u selecteren **naar de resource gaan** voor het configureren van andere eigenschappen, instellen van beveiliging met beleid voor gegevenstoegang, bronnen van gebeurtenissen en andere acties toevoegen.

## <a name="next-steps"></a>Volgende stappen
* [Gegevenstoegangsbeleid definiëren](time-series-insights-data-access.md) voor het beveiligen van uw omgeving.
* [Toevoegen van een Event Hub-gebeurtenisbron](time-series-insights-how-to-add-an-event-source-eventhub.md) aan uw Azure Time Series Insights-omgeving. 
* [Gebeurtenissen verzenden](time-series-insights-send-events.md) naar de gebeurtenisbron.
* Uw omgeving bekijken in [Verkenner van Time Series Insights](https://insights.timeseries.azure.com).
