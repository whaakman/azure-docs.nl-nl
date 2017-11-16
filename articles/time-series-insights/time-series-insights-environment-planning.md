---
title: Plannen van de schaal van uw omgeving Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe Volg de aanbevolen procedures bij het plannen van een Azure Time Series Insights-omgeving, inclusief opslagcapaciteit, Gegevensretentie, inkomend capaciteit en bewaking.
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 5fb158ba162dd199f419f9568de08a7a18c833dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Uw omgeving Azure Time Series Insights plannen

Dit artikel wordt beschreven hoe u van plan bent uw Azure Time Series Insights-omgeving op basis van de snelheid van de verwachte inkomend en uw vereisten voor het bewaren van gegevens.

## <a name="best-practices"></a>Aanbevolen procedures

Als u wilt beginnen met de Time Series inzichten, is het raadzaam als u hoeveel gegevens u verwacht weet te pushen per minuut ook hoe lang u moet uw gegevens opslaat.  

Zie voor meer informatie over de capaciteit en de bewaarperiode voor beide Time Series Insights SKU's [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights/).

Houd rekening met de volgende kenmerken aan best plan de omgeving voor succes op lange termijn: 
- Opslagcapaciteit
- Bewaarperiode van gegevens
- Inkomend capaciteit 

## <a name="understand-storage-capacity"></a>Opslagcapaciteit begrijpen
Standaard behoudt Time Series Insights gegevens op basis van de hoeveelheid opslagruimte die u hebt ingericht (eenheden keer hoeveelheid opslag per eenheid) en uitgangsclaims.

## <a name="understand-data-retention"></a>Bewaren van gegevens begrijpen
U kunt de Time Series Insights-omgeving configureren **gegevensretentietijd** instelling, het inschakelen van maximaal 400 dagen bewaren.  Time Series Insights heeft twee modi, een dat is geoptimaliseerd voor uw omgeving zodat de meest recente gegevens (op standaard), en een andere dat is geoptimaliseerd voor het garanderen van retentie limieten zijn bereikt, waarbij inkomend is onderbroken als de totale capaciteit van de omgeving is bereikt.  U kunt bewaren en schakelen tussen de twee modi in de configuratiepagina van de omgeving in de Azure portal kunt aanpassen.

U kunt maximaal 400 dagen bewaren van gegevens in uw omgeving Time Series Insights configureren.

## <a name="configure-data-retention"></a>Bewaren van gegevens configureren

1. In de [Azure-portal](https://portal.azure.com), selecteer uw Time Series Insights-omgeving.

2. Op de **Time Series Insights omgeving pagina**onder de **instellingen** kop, selecteer **configureren**. 

3. In de **bewaartijd van gegevens (in dagen)** Voer een waarde van 1 tot en met 400.

   ![Bewaartermijn configureren](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Inkomend capaciteit begrijpen

Het gebied te concentreren op voor de planning is inkomend capaciteit, die afgeleid van de toewijzing voor de per minuut is. 

Vanuit een bandbreedteregeling perspectief wordt een ingressed gegevenspakket met een pakketgrootte 32 kB wordt behandeld als 32-gebeurtenissen, elke grootte van 1 KB. De grootte van de maximum toegestane gebeurtenis is 32 KB; gegevenspakketten die groter zijn dan 32 KB worden afgekapt.

De volgende tabel geeft een overzicht van de capaciteit van de inkomende gegevens voor elke SKU:

|SKU  |Aantal gebeurtenissen Per maand Per eenheid  |Het formaat van gebeurtenissen Per maand, Per eenheid  |Aantal gebeurtenissen Per minuut Per eenheid  | Grootte Per minuut Per eenheid   |
|---------|---------|---------|---------|---------|
|S1     |   30 miljoen     |  30 GB     |  700    |  700 KB   |
|S2     |   300 miljoen    |   300 GB   | 7,000   | 7000 KB  |

U kunt de capaciteit van een S1 of S2 SKU op 10 eenheden in een omgeving met één verhogen. U kunt in een omgeving S1 naar een S2, of vanuit een S2-omgeving naar een S1 niet migreren. 

Capaciteit van de inkomende gegevens, moet u eerst de totale ingress per maand op basis van een gewenste bepalen. Vervolgens bepalen wat uw per minuut moet zijn, aangezien dit waar bandbreedtebeperking en latentie een rol spelen.

Als u een piek in uw gegevens inkomend duurt minder dan 24 uur hebt, kunnen Time Series Insights 'bijwerken' een inkomend verhouding van 2 x de hierboven vermelde tarieven. 

Bijvoorbeeld, als er een enkel S1 SKU en inkomende gegevens met een frequentie van 700 gebeurtenissen per minuut en piek voor minder dan 1 uur met een snelheid van gebeurtenissen 1400 of minder, zou er geen merkbare latentie aan uw omgeving. Als u gebeurtenissen per minuut gedurende meer dan één uur 1400 overschrijdt, zou u echter waarschijnlijk latentie voor gegevens die gevisualiseerde en beschikbaar voor de query in uw omgeving optreden. 

U weet mogelijk niet van tevoren hoeveel gegevens die u verwacht te pushen. In dit geval kunt u gegevens telemetrie voor vinden [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) en [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) in uw Azure-portal. Deze telemetrie kunt u bepalen het inrichten van uw omgeving. Gebruik de **metrische gegevens** pagina in de Azure-portal voor de respectieve gebeurtenisbron om de telemetrie weer te geven. Als u de bron metrische gegevens van uw gebeurtenis weet, kunt u efficiënter plannen en inrichten van uw omgeving Time Series Insights.

## <a name="calculate-ingress-requirements"></a>Vereisten voor inkomende gegevens berekenen

- Controleer de capaciteit van de inkomende gegevens hoger is dan de gemiddelde per minuut en dat uw omgeving groot genoeg is is voor het afhandelen van de verwachte inkomend equivalent zijn aan 2 x de capaciteit voor minder dan 1 uur.

- Als inkomend pieken optreden die laatste langer dan 1 uur, de frequentie piek gebruiken als de gemiddelde en inrichten van een omgeving met de capaciteit voor het afhandelen van de piek-snelheid.
 
## <a name="mitigate-throttling-and-latency"></a>Bandbreedtebeperking en latentie beperken

Zie voor meer informatie over het voorkomen van bandbreedtebeperking en latentie [verhelpen latentie en beperking](time-series-insights-environment-mitigate-latency.md). 

## <a name="next-steps"></a>Volgende stappen
- [Het toevoegen van een gebeurtenisbron Event Hub](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Het toevoegen van een gebeurtenisbron IoT-Hub](time-series-insights-how-to-add-an-event-source-iothub.md)
