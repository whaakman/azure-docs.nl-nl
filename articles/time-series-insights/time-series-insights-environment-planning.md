---
title: De schaal van uw Azure Time Series Insights-omgeving plannen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Volg de aanbevolen procedures bij het plannen van een Azure Time Series Insights-omgeving, met inbegrip van opslagcapaciteit, Gegevensretentie, opnamecapaciteit, controle en herstel na noodgevallen voor bedrijven (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: 1c6e8ef9effdb5cf31aee1603de28bed0d894cbb
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239096"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Uw Azure Time Series Insights GA-omgeving plannen

In dit artikel wordt beschreven hoe u uw Azure Time Series Insights-algemene beschikbaarheid (GA)-omgeving op basis van de snelheid van de verwachte inkomend verkeer en de vereisten voor het bewaren van gegevens plannen.

## <a name="video"></a>Video

### <a name="learn-more-about-data-retention-in-azuretime-series-insights-and-how-to-plan-for-itbr"></a>Meer informatie over het bewaren van gegevens in AzureTime Series Insights en hoe u van plan bent voor het.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Aanbevolen procedures

Als u wilt aan de slag met Time Series Insights, is het beste als u hoeveel gegevens u verwacht weet te pushen per minuut en hoe lang u nodig hebt voor het opslaan van uw gegevens.  

Zie voor meer informatie over de capaciteit en retentie voor beide Time Series Insights-SKU's [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights/).

Houd rekening met de volgende kenmerken naar beste plan de omgeving voor het succes op lange termijn:

- <a href="#understand-storage-capacity">Opslagcapaciteit</a>
- <a href="#understand-data-retention">Bewaartermijn voor gegevens</a>
- <a href="#understand-ingress-capacity">Opnamecapaciteit</a>
- <a href="#shape-your-events">Uw gebeurtenissen vormgeven</a>
- <a href="#ensure-you-have-reference-data">Ervoor te zorgen dat er referentiegegevens op locatie</a>

## <a name="understand-storage-capacity"></a>Opslagcapaciteit begrijpen

Standaard behoudt Azure Time Series Insights gegevens op basis van de hoeveelheid opslag die u hebt ingericht (eenheden keer hoeveelheid opslag per eenheid) en ingress.

## <a name="understand-data-retention"></a>Inzicht in gegevensretentie

U kunt uw Time Series Insights-omgeving configureren **gegevensretentietijd** instelling, waardoor tot 400 dagen retentie. Time Series Insights heeft twee modi, die is geoptimaliseerd voor ervoor te zorgen dat uw omgeving heeft de meest recente gegevens (op standaard), en een andere die is geoptimaliseerd voor ervoor te zorgen dat de bewaarperiode limieten zijn bereikt, waar binnenkomende is onderbroken als de totale opslagcapaciteit van de omgeving is bereikt.  U kunt aanpassen en en schakelen tussen de twee modi op de configuratiepagina van de omgeving in Azure portal.

U kunt maximaal 400 dagen bewaren van gegevens in uw Time Series Insights-omgeving configureren.

### <a name="configure-data-retention"></a>Gegevensretentie configureren

1. In de [Azure-portal](https://portal.azure.com), selecteer uw Time Series Insights-omgeving.

1. Op de **Time Series Insights-omgeving pagina**onder de **instellingen** kop, selecteer **configureren**.

1. In de **bewaartijd van gegevens (in dagen)** vak, voer een waarde van 1 tot en met 400.

   [![Bewaren configureren](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Meer informatie over het implementeren van een bewaarbeleid voor de juiste gegevens aan de hand [retentie configureren](./time-series-insights-how-to-configure-retention.md).

## <a name="understand-ingress-capacity"></a>Opnamecapaciteit begrijpen

Het gebied zich kunt richten op voor de planning is opnamecapaciteit, die afgeleid van de toewijzing per minuut is.

Een gegevenspakket ingressed met een pakketgrootte van 32 KB wordt beschouwd als 32-gebeurtenissen van een beperking perspectief, elke grootte van 1 KB. De toegestane maximale gebeurtenisgrootte is 32 KB; gegevenspakketten die groter zijn dan 32 KB worden afgebroken.

De volgende tabel geeft een overzicht van de opnamecapaciteit voor elke SKU:

|SKU  |Aantal gebeurtenissen / maand / eenheid  |Grootte van de gebeurtenissen / maand / eenheid  |Gebeurtenissen tellen / minuut / eenheid  | Grootte / minuut / eenheid   |
|---------|---------|---------|---------|---------|
|S1     |   30 miljoen     |  30 GB     |  720    |  720 KB   |
|S2     |   300 miljoen    |   300 GB   | 7,200   | 7200 KB  |

U kunt de capaciteit Verhoog van een S1 of S2 SKU naar 10 eenheden in één omgeving. U kunt vanuit een S1-omgeving naar een S2 of vanuit een S2-omgeving naar een S1 niet migreren.

Voor opnamecapaciteit, moet u eerst bepalen van de totale inkomend verkeer die u nodig op basis van per maand hebt. Vervolgens vaststellen wat uw per minuut nodig zijn, omdat dit is waar bandbreedtebeperking en latentie van een rol spelen.

Als u een piek in de binnenkomende gegevens langdurig is minder dan 24 uur hebt, kunt Time Series Insights 'bijwerken' tegen een tarief inkomend verkeer van 2 x de hierboven vermelde tarieven.

Bijvoorbeeld, als u een één S1 SKU en de inkomende gegevens tegen een tarief van 720 gebeurtenissen per minuut en piek voor minder dan 1 uur tegen een tarief van 1440 gebeurtenissen of minder hebt, wordt er geen merkbare latentie voor uw omgeving. Als u gebeurtenissen per minuut gedurende meer dan één uur 1440 overschrijdt, wordt u echter waarschijnlijk latentie op gegevens die zijn zinvoller visualiseren en beschikbaar voor query's in uw omgeving optreden.

U weet mogelijk niet van tevoren regelen hoeveel gegevens die u verwacht te pushen. In dit geval kunt u telemetrie van de gegevens voor [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) en [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) in uw Azure-portal. Deze telemetrische gegevens kunt u bepalen hoe u voor het inrichten van uw omgeving. Gebruik de **metrische gegevens** pagina in de Azure-portal voor de bron van de bijbehorende gebeurtenis om de telemetrie ervan weer te geven. Als u de metrische gegevens over bron van gebeurtenis begrijpt, kunt u effectiever plannen en uw Time Series Insights-omgeving inrichten.

### <a name="calculate-ingress-requirements"></a>Vereisten voor inkomend verkeer berekenen

- Controleer of uw opnamecapaciteit hoger is dan de gemiddelde snelheid van de per minuut en dat uw omgeving groot genoeg is is voor het verwerken van uw verwachte invoer die gelijkwaardig zijn aan 2 x de capaciteit voor minder dan 1 uur.

- Als inkomend pieken optreden die laatste langer dan 1 uur, de piek-tarief wordt gebruikt als uw gemiddelde en inrichten van een omgeving met de capaciteit voor het verwerken van de piek-tarief.

### <a name="mitigate-throttling-and-latency"></a>Bandbreedtebeperking en latentie te beperken

Voor informatie over hoe om te voorkomen dat de beperking en latentie, leest u over hoe u [verminderen latentie en beperking](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Vorm uw gebeurtenissen

Het is belangrijk om te controleren of de manier waarop u gebeurtenissen verzenden naar TSI biedt ondersteuning voor de grootte van de omgeving die u inricht (daarentegen, kunt u toewijzen de grootte van de omgeving op hoeveel gebeurtenissen TSI leest en de grootte van elke gebeurtenis). Het is ook belangrijk om na te denken over de kenmerken die u wilt mogelijk segmenteren en filteren op bij het opvragen van uw gegevens.

> [!TIP]
> Controleer de JSON-documentatie in vormgeven [verzenden van gebeurtenissen](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-you-have-reference-data"></a>Zorg ervoor dat u hebt referentiegegevens

Een **Referentiegegevensset** is een verzameling van items die de ervaring van de gebeurtenissen uit uw gebeurtenisbron. De engine voor Time Series Insights inkomende lid wordt van elke gebeurtenis uit uw gebeurtenisbron aan de desbetreffende gegevensrij in uw referentiegegevensset. Deze uitgebreide gebeurtenis is vervolgens beschikbaar voor query’s. Deze koppeling is gebaseerd op de primaire sleutel of meer kolommen gedefinieerd in uw referentiegegevensset.

Let op: referentiegegevens is niet met terugwerkende kracht gekoppeld. Dit betekent dat alleen de gegevens van de huidige en toekomstige inkomend is afgestemd en toegevoegd aan de verwijzing datum is ingesteld, zodra deze is geconfigureerd en geüpload.  Als u wilt verzenden van grote hoeveelheden historische gegevens van TSI en niet uploaden of referentiegegevens eerst maken in de TSI mogelijk hebt u uw werk opnieuw toepassen (hint, niet veel plezier met).  

Ga voor meer informatie over het maken en beheren van uw referentiegegevens in TSI uploaden naar onze [gegevensset referentiedocumentatie](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Volgende stappen

- Aan de slag met het maken van [een nieuwe Time Series Insights-omgeving in Azure portal](time-series-insights-get-started.md).

- Meer informatie over het [toevoegen van een Event Hub-gebeurtenisbron](time-series-insights-how-to-add-an-event-source-eventhub.md) voor Time Series Insights.

- Meer informatie over hoe u [configureren van een IoT Hub-gebeurtenisbron](time-series-insights-how-to-add-an-event-source-iothub.md).
