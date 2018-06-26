---
title: Plannen van de schaal van uw omgeving Azure Time Series Insights | Microsoft Docs
description: In dit artikel wordt beschreven hoe Volg de aanbevolen procedures bij het plannen van een Azure Time Series Insights-omgeving, inclusief opslagcapaciteit, Gegevensretentie, inkomend capaciteit, bewaking en herstel na noodgevallen voor bedrijven (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: jasonh
manager: jhubbard
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: f0f414e43231fc6d873d639902fd4f71e48f1002
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751166"
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
- Vormgeven van uw gebeurtenissen
- Hebt u referentiegegevens gezorgd

## <a name="understand-storage-capacity"></a>Opslagcapaciteit begrijpen
Standaard behoudt Time Series Insights gegevens op basis van de hoeveelheid opslagruimte die u hebt ingericht (eenheden keer hoeveelheid opslag per eenheid) en uitgangsclaims.

## <a name="understand-data-retention"></a>Inzicht in gegevensretentie
U kunt de Time Series Insights-omgeving configureren **gegevensretentietijd** instelling, het inschakelen van maximaal 400 dagen bewaren.  Time Series Insights heeft twee modi, een dat is geoptimaliseerd voor uw omgeving zodat de meest recente gegevens (op standaard), en een andere dat is geoptimaliseerd voor het garanderen van retentie limieten zijn bereikt, waarbij inkomend is onderbroken als de totale capaciteit van de omgeving is bereikt.  U kunt bewaren en schakelen tussen de twee modi in de configuratiepagina van de omgeving in de Azure portal kunt aanpassen.

U kunt maximaal 400 dagen bewaren van gegevens in uw omgeving Time Series Insights configureren.

## <a name="configure-data-retention"></a>Gegevensretentie configureren

1. In de [Azure-portal](https://portal.azure.com), selecteer uw Time Series Insights-omgeving.

2. Op de **Time Series Insights omgeving pagina**onder de **instellingen** kop, selecteer **configureren**. 

3. In de **bewaartijd van gegevens (in dagen)** Voer een waarde van 1 tot en met 400.

   ![Bewaren configureren](media/environment-mitigate-latency/configure-retention.png)

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

### <a name="calculate-ingress-requirements"></a>Vereisten voor inkomende gegevens berekenen

- Controleer de capaciteit van de inkomende gegevens hoger is dan de gemiddelde per minuut en dat uw omgeving groot genoeg is is voor het afhandelen van de verwachte inkomend equivalent zijn aan 2 x de capaciteit voor minder dan 1 uur.

- Als inkomend pieken optreden die laatste langer dan 1 uur, de frequentie piek gebruiken als de gemiddelde en inrichten van een omgeving met de capaciteit voor het afhandelen van de piek-snelheid.
 
### <a name="mitigate-throttling-and-latency"></a>Bandbreedtebeperking en latentie beperken

Zie voor meer informatie over het voorkomen van bandbreedtebeperking en latentie [verhelpen latentie en beperking](time-series-insights-environment-mitigate-latency.md). 

## <a name="shaping-your-events"></a>Vormgeven van uw gebeurtenissen
Het is belangrijk om te controleren of de manier waarop u gebeurtenissen verzenden naar TSI biedt ondersteuning voor de grootte van de omgeving die u inricht (als u daarentegen, kunt u toewijzen de grootte van de omgeving voor het aantal gebeurtenissen TSI leest en de grootte van elke gebeurtenis).  Het is ook belangrijk om na te denken over de kenmerken die u wilt misschien segmenteren en filteren op bij het opvragen van uw gegevens.  Met dat gegeven in gedachte, wij aanraden voor het controleren van de JSON vormgeven van sectie van onze *gebeurtenissen verzenden* documentatie [documentatie] (https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).  Het is aan de onderkant van de pagina.  

## <a name="ensuring-you-have-reference-data-in-place"></a>Hebt u referentiegegevens gezorgd
Een gegevensset verwijzing is een verzameling van items die verbeteren van de gebeurtenissen uit de gebeurtenisbron. Tijd reeks Insights inkomend engine koppelt elke gebeurtenis van de gebeurtenisbron-met de bijbehorende gegevensrij in uw gegevensset verwijzing. Deze uitgebreide gebeurtenis is vervolgens beschikbaar voor query’s. Deze koppeling is gebaseerd op de primaire sleutel kolommen gedefinieerd in uw gegevensset verwijzing.

Houd er rekening mee referentiegegevens met terugwerkende kracht niet is toegevoegd. Dit betekent dat alleen de gegevens van de huidige en toekomstige inkomend komt overeen met en gekoppeld aan de verwijzing naar datum is ingesteld, zodra deze is geconfigureerd en geüpload.  Als u van plan bent te veel van historische gegevens verzenden naar TSI en niet uploaden of referentiegegevens eerst in TSI maken en vervolgens u wellicht opnieuw doen van uw werk (hint, niet leuke).  

Voor meer informatie over het maken en beheren van uw referentiegegevens TSI uploaden, Ga naar onze *referentiegegevens* documentatie [documentatie](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="business-disaster-recovery"></a>Noodherstel voor bedrijven
Een Azure-service biedt Time Series Insights redundantie op het niveau van de Azure-regio, zonder extra werk vereist voor de oplossing met hoge beschikbaarheid (HA). Het Microsoft Azure-platform omvat ook functies waarmee u oplossingen bouwen met herstelfuncties van noodherstel (DR) of de regio-overschrijdende beschikbaarheid. Als u wilt bieden globale, hoge beschikbaarheid van de regio-overschrijdende voor apparaten of gebruikers profiteren van deze Azure DR-functies. Het artikel [Azure zakelijke continuïteit technische richtlijnen](../resiliency/resiliency-technical-guidance.md) beschrijving van de ingebouwde functies in Azure voor bedrijfscontinuïteit en Noodherstel. [Herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen] [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen] papier biedt architectuurrichtlijnen op strategieën voor het Azure-toepassingen om HA en Noodherstel te bereiken.

Time Series Insights heeft geen ingebouwde business noodherstel (BCDR).  Klanten die BCDR vereisen kunnen echter nog steeds een herstelstrategie implementeren. Een tweede keer reeks Insights-omgeving in een Azure-regio van de back-up maken en gebeurtenissen verzenden naar deze secundaire omgeving van de primaire gegevensbron voor gebruik van een tweede speciale klantengroep en die gebeurtenisbron BCDR-richtlijnen.  

1.  Omgeving maken in de tweede regio.  Meer informatie over het maken van een omgeving Time Series Insights [hier](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-get-started).
2.  Maak een tweede speciale klantengroep voor de gebeurtenisbron en die gebeurtenisbron verbinding met de nieuwe omgeving.  Zorg ervoor dat de tweede, speciale klantengroep aanwijzen.  U meer informatie over deze door een [IoT Hub-documentatie](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) of [Event hub-documentatie](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-data-access).
3.  Als uw primaire regio omlaag gaan tijdens een incident na noodgevallen, overschakelen via bewerkingen op de back-up Time Series Insights-omgeving.  

Ga voor meer informatie over IoT-Hub BCDR-beleid, [hier](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-ha-dr).  Ga voor meer informatie over het beleid van de Event hub BCDR [hier](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-geo-dr).  

## <a name="next-steps"></a>Volgende stappen
- [Het toevoegen van een gebeurtenisbron Event Hub](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Het toevoegen van een gebeurtenisbron IoT-Hub](time-series-insights-how-to-add-an-event-source-iothub.md)
