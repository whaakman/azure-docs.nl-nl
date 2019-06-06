---
title: De schaal van uw Azure Time Series Insights-omgeving plannen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Volg de aanbevolen procedures bij het plannen van een Azure Time Series Insights-omgeving. Gebieden die worden besproken omvatten opslagcapaciteit, Gegevensretentie, opnamecapaciteit, controle en zakelijke continuïteit en herstel na noodgevallen (BCDR).
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
ms.openlocfilehash: 9d9f9b7fe7aafa927f023e1d4e30e079a26f7fab
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/31/2019
ms.locfileid: "66431053"
---
# <a name="plan-your-azure-time-series-insights-ga-environment"></a>Uw Azure Time Series Insights GA-omgeving plannen

In dit artikel wordt beschreven hoe u uw Azure Time Series Insights-algemene beschikbaarheid (GA)-omgeving op basis van de snelheid van de verwachte inkomend verkeer en de vereisten voor het bewaren van gegevens plannen.

## <a name="video"></a>Video

**Bekijk deze video voor meer informatie over het bewaren van gegevens in Azure Time Series Insights en hoe u van plan bent voor het**:<br /><br />

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Aanbevolen procedures

Als u wilt aan de slag met Time Series Insights, is het beste als u hoeveel gegevens u verwacht weet te pushen met de minuut en hoe lang u nodig hebt voor het opslaan van uw gegevens.  

Zie voor meer informatie over de capaciteit en retentie voor beide Time Series Insights-SKU's [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights/).

Voor het beste van plan bent uw Time Series Insights-omgeving voor succes op lange termijn, houd rekening met de volgende kenmerken:

- <a href="#storage-capacity">Opslagcapaciteit</a>
- <a href="#data-retention">Bewaartermijn voor gegevens</a>
- <a href="#ingress-capacity">Opnamecapaciteit</a>
- <a href="#shape-your-events">Uw gebeurtenissen vormgeven</a>
- <a href="#ensure-that-you-have-reference-data">Ervoor te zorgen dat u hebt de referentiegegevens op locatie</a>

## <a name="storage-capacity"></a>Opslagcapaciteit

Standaard Time Series Insights-termijngegevens bewaard op basis van de hoeveelheid opslag die u inricht (eenheden &#215; de hoeveelheid opslag per eenheid) en ingress.

## <a name="data-retention"></a>Bewaartijd van gegevens

U kunt wijzigen de **gegevensretentietijd** instellen in uw Time Series Insights-omgeving. U kunt maximaal 400 dagen retentie inschakelen. 

Time Series Insights beschikt over twee modi. Er wordt één modus geoptimaliseerd om ervoor te zorgen dat uw omgeving de meest recente gegevens heeft. In deze modus is ingeschakeld, wordt standaard. 

De andere modus optimaliseert om ervoor te zorgen dat de limieten voor het bewaren wordt voldaan. Inkomend verkeer is in de tweede modus onderbroken als de totale opslagcapaciteit van de omgeving is voldaan. 

U kunt aanpassen en en schakelen tussen de twee modi op de configuratiepagina van de omgeving in Azure portal.

U kunt maximaal 400 dagen bewaren van gegevens in uw Time Series Insights-omgeving configureren.

### <a name="configure-data-retention"></a>Gegevensretentie configureren

1. In de [Azure-portal](https://portal.azure.com), selecteer uw Time Series Insights-omgeving.

1. In de **Time Series Insights-omgeving** deelvenster onder **instellingen**, selecteer **configureren**.

1. In de **bewaartijd van gegevens (in dagen)** vak, voer een waarde tussen 1 en 400.

   [![Bewaren configureren](media/environment-mitigate-latency/configure-retention.png)](media/environment-mitigate-latency/configure-retention.png#lightbox)

> [!TIP]
> Zie voor meer informatie over het implementeren van een juiste beleid voor Gegevensretentie, [retentie configureren](./time-series-insights-how-to-configure-retention.md).

## <a name="ingress-capacity"></a>Opnamecapaciteit

Het tweede gedeelte zich kunt richten op voor het plannen van uw Time Series Insights-omgeving is opnamecapaciteit. Opnamecapaciteit is afgeleid van de toewijzing per minuut.

Vanuit een beperking perspectief, wordt een ingressed gegevenspakket dat de pakketgrootte van een van 32 KB is behandeld als 32 gebeurtenissen, elke 1 KB groot. De toegestane maximale gebeurtenisgrootte is 32 KB. Gegevenspakketten die groter zijn dan 32 KB worden afgebroken.

De volgende tabel geeft een overzicht van de capaciteit van de inkomende gegevens per eenheid voor elke Time Series Insights-SKU:

|SKU  |Aantal gebeurtenissen per maand  |Gebeurtenisgrootte per maand  |Aantal gebeurtenissen per minuut  |Gebeurtenisgrootte per minuut  |
|---------|---------|---------|---------|---------|
|S1     |   30 miljoen     |  30 GB     |  720    |  720 KB   |
|S2     |   300 miljoen    |   300 GB   | 7,200   | 7200 KB  |

U kunt de capaciteit Verhoog van een S1 of S2 SKU naar 10 eenheden in één omgeving. U migreren niet van een omgeving S1 naar S2. U migreren niet vanaf een S2-omgeving naar een S1.

Voor opnamecapaciteit, bepaalt u eerst de totaal volume inkomend verkeer die u nodig op basis van per maand hebt. Vervolgens kunt u bepalen wat uw per minuut nodig zijn. 

Bandbreedtebeperking en latentie kunt u een rol spelen in capaciteit per minuut. Hebt u een piek in de opname van gegevens die minder dan 24 uur duurt, kan Time Series Insights "vang van" tegen een tarief inkomend verkeer van twee keer de hier vermelde tarieven in de voorgaande tabel.

Bijvoorbeeld, als u een één S1 SKU u inkomende gegevens tegen een tarief van 720 gebeurtenissen per minuut, en de snelheid waarmee u gegevens voor minder dan een uur tegen een tarief van 1.440 gebeurtenissen of minder pieken, is er geen merkbare latentie in uw omgeving. Als u meer dan 1.440 gebeurtenissen per minuut gedurende meer dan een uur, zult u waarschijnlijk latentie in de gegevens die zinvoller visualiseren en beschikbaar voor query's in uw omgeving ervaren.

Weet u misschien niet van tevoren regelen hoeveel gegevens die u verwacht te pushen. In dit geval kunt u telemetrie van de gegevens voor [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) en [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) in uw abonnement op Azure portal. De telemetrie kunt u bepalen hoe u voor het inrichten van uw omgeving. Gebruik de **metrische gegevens** deelvenster in de Azure-portal voor de bron van de bijbehorende gebeurtenis om de telemetrie ervan weer te geven. Als u de metrische gegevens over bron van gebeurtenis begrijpt, kunt u effectiever plannen en uw Time Series Insights-omgeving inrichten.

### <a name="calculate-ingress-requirements"></a>Vereisten voor inkomend verkeer berekenen

Voor het berekenen van de vereisten van uw inkomende gegevens:

- Controleer of dat uw opnamecapaciteit hoger dan de gemiddelde snelheid van de per minuut is en dat uw omgeving groot genoeg is is voor het verwerken van uw verwachte invoer gelijk is aan twee keer uw capaciteit voor minder dan een uur.

- Als binnenkomende pieken die laatste langer dan 1 uur, als uw gemiddelde de piek-tarief optreden gebruiken. Richt een omgeving met de capaciteit voor het verwerken van de piek-tarief.

### <a name="mitigate-throttling-and-latency"></a>Bandbreedtebeperking en latentie te beperken

Zie voor meer informatie over hoe u om te voorkomen dat de beperking en latentie [verminderen latentie en beperking](time-series-insights-environment-mitigate-latency.md).

## <a name="shape-your-events"></a>Vorm uw gebeurtenissen

Het is belangrijk om ervoor te zorgen dat de manier waarop die u verzendt gebeurtenissen naar Time Series Insights biedt ondersteuning voor de grootte van de omgeving die u inricht. (Daarentegen, kunt u toewijzen de grootte van de omgeving op hoeveel gebeurtenissen Time Series Insights leest en de grootte van elke gebeurtenis.) Het is ook belangrijk om na te denken over de kenmerken die u kunt gebruiken om te segmenteren en filteren op wanneer u uw gegevens op te vragen.

> [!TIP]
> Controleer de JSON-documentatie in vormgeven [verzenden van gebeurtenissen](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).

## <a name="ensure-that-you-have-reference-data"></a>Zorg ervoor dat u referentiegegevens hebt

Een *verwijst naar gegevensset* is een verzameling van items die de ervaring van de gebeurtenissen uit uw gebeurtenisbron. De Time Series Insights-engine voor inkomende lid wordt van elke gebeurtenis uit uw gebeurtenisbron aan de desbetreffende gegevensrij in uw gegevensset verwijzing. De uitgebreide gebeurtenis is vervolgens beschikbaar voor query's. De koppeling is gebaseerd op de **primaire sleutel** kolommen die zijn gedefinieerd in uw gegevensset verwijzing.

> [!NOTE]
> Referentiegegevens wordt niet met terugwerkende kracht gekoppeld. Alleen de gegevens van de huidige en toekomstige inkomend verkeer wordt vergeleken en toegevoegd aan de gegevensset verwijzing nadat deze is geconfigureerd en geüpload. Als u van plan bent een grote hoeveelheid historische gegevens verzenden naar Time Series Insights en niet de eerste uploaden of maken van referentiegegevens in Time Series Insights, hebt u mogelijk opnieuw uitvoeren van uw werk (hint: niet veel plezier met).  

Zie voor meer informatie over het maken, uploaden en beheren van uw referentiegegevens in Time Series Insights, onze [gegevensset referentiedocumentatie](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

[!INCLUDE [business-disaster-recover](../../includes/time-series-insights-business-recovery.md)]

## <a name="next-steps"></a>Volgende stappen

- Aan de slag met het maken van [een nieuwe Time Series Insights-omgeving in Azure portal](time-series-insights-get-started.md).

- Meer informatie over het [toevoegen van een gebeurtenisbron voor Event Hubs](time-series-insights-how-to-add-an-event-source-eventhub.md) voor Time Series Insights.

- Meer informatie over hoe u [configureren van een IoT Hub-gebeurtenisbron](time-series-insights-how-to-add-an-event-source-iothub.md).
