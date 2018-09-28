---
title: De schaal van uw Azure Time Series Insights-omgeving plannen | Microsoft Docs
description: In dit artikel wordt beschreven hoe u Volg de aanbevolen procedures bij het plannen van een Azure Time Series Insights-omgeving, met inbegrip van opslagcapaciteit, Gegevensretentie, opnamecapaciteit, controle en herstel na noodgevallen voor bedrijven (BCDR).
services: time-series-insights
ms.service: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: fa178efadf001b70501b132ede67686ae5c06363
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47422555"
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Uw Azure Time Series Insights-omgeving plannen

In dit artikel wordt beschreven hoe u uw Azure Time Series Insights-omgeving op basis van de snelheid van de verwachte inkomend verkeer en de vereisten voor het bewaren van gegevens plannen.

## <a name="video"></a>Video: 

### <a name="in-this-video-we-cover-time-series-insights-data-retention-and-how-to-plan-for-itbr"></a>In deze video behandelen we bewaren van Time Series Insights-gegevens en het plannen voor deze.</br>

> [!VIDEO https://www.youtube.com/embed/03x6zKDQ6DU]

## <a name="best-practices"></a>Aanbevolen procedures

Als u wilt aan de slag met Time Series Insights, is het beste als u hoeveel gegevens u verwacht weet te pushen per minuut en hoe lang u nodig hebt voor het opslaan van uw gegevens.  

Zie voor meer informatie over de capaciteit en retentie voor beide Time Series Insights-SKU's [Time Series Insights prijzen](https://azure.microsoft.com/pricing/details/time-series-insights/).

Houd rekening met de volgende kenmerken naar beste plan de omgeving voor het succes op lange termijn: 
- Opslagcapaciteit
- Bewaarperiode van gegevens
- Opnamecapaciteit 
- Uw gebeurtenissen vormgeven
- Ervoor te zorgen dat er referentiegegevens op locatie

## <a name="understand-storage-capacity"></a>Opslagcapaciteit begrijpen
Time Series Insights houdt standaard gegevens op basis van de hoeveelheid opslag die u hebt ingericht (eenheden keer hoeveelheid opslag per eenheid) en ingress.

## <a name="understand-data-retention"></a>Inzicht in gegevensretentie
U kunt uw Time Series Insights-omgeving configureren **gegevensretentietijd** instelling, waardoor tot 400 dagen retentie.  Time Series Insights heeft twee modi, die is geoptimaliseerd voor ervoor te zorgen dat uw omgeving heeft de meest recente gegevens (op standaard), en een andere die is geoptimaliseerd voor ervoor te zorgen dat de bewaarperiode limieten zijn bereikt, waar binnenkomende is onderbroken als de totale opslagcapaciteit van de omgeving is bereikt.  U kunt aanpassen en en schakelen tussen de twee modi op de configuratiepagina van de omgeving in Azure portal.

U kunt maximaal 400 dagen bewaren van gegevens in uw Time Series Insights-omgeving configureren.

## <a name="configure-data-retention"></a>Gegevensretentie configureren

1. In de [Azure-portal](https://portal.azure.com), selecteer uw Time Series Insights-omgeving.

2. Op de **Time Series Insights-omgeving pagina**onder de **instellingen** kop, selecteer **configureren**. 

3. In de **bewaartijd van gegevens (in dagen)** vak, voer een waarde van 1 tot en met 400.

   ![Bewaren configureren](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Opnamecapaciteit begrijpen

Het gebied zich kunt richten op voor de planning is opnamecapaciteit, die afgeleid van de toewijzing per minuut is. 

Een gegevenspakket ingressed met een pakketgrootte van 32 KB wordt beschouwd als 32-gebeurtenissen van een beperking perspectief, elke grootte van 1 KB. De toegestane maximale gebeurtenisgrootte is 32 KB; gegevenspakketten die groter zijn dan 32 KB worden afgebroken.

De volgende tabel geeft een overzicht van de opnamecapaciteit voor elke SKU:

|SKU  |Aantal gebeurtenissen Per maand Per eenheid  |Het formaat van gebeurtenissen Per maand, Per eenheid  |Aantal gebeurtenissen Per minuut Per eenheid  | Grootte Per minuut Per eenheid   |
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

Zie voor meer informatie over hoe u om te voorkomen dat de beperking en latentie [verminderen latentie en beperking](time-series-insights-environment-mitigate-latency.md).

## <a name="shaping-your-events"></a>Uw gebeurtenissen vormgeven
Het is belangrijk om te controleren of de manier waarop u gebeurtenissen verzenden naar TSI biedt ondersteuning voor de grootte van de omgeving die u inricht (daarentegen, kunt u toewijzen de grootte van de omgeving op hoeveel gebeurtenissen TSI leest en de grootte van elke gebeurtenis).  Het is ook belangrijk om na te denken over de kenmerken die u wilt mogelijk segmenteren en filteren op bij het opvragen van uw gegevens.  Met dat gegeven in gedachte, wordt aangeraden voor het controleren van de JSON vormgeven sectie van onze [verzenden van gebeurtenissen documentatie](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-send-events).  Het is aan de onderkant van de pagina.  

## <a name="ensuring-you-have-reference-data-in-place"></a>Ervoor te zorgen dat er referentiegegevens op locatie
Een Referentiegegevensset is een verzameling van items die de ervaring van de gebeurtenissen uit uw gebeurtenisbron. De engine voor Time Series Insights inkomende lid wordt van elke gebeurtenis uit uw gebeurtenisbron aan de desbetreffende gegevensrij in uw referentiegegevensset. Deze uitgebreide gebeurtenis is vervolgens beschikbaar voor query’s. Deze koppeling is gebaseerd op de primaire sleutel of meer kolommen gedefinieerd in uw referentiegegevensset.

Let op: referentiegegevens is niet met terugwerkende kracht gekoppeld. Dit betekent dat alleen de gegevens van de huidige en toekomstige inkomend is afgestemd en toegevoegd aan de verwijzing datum is ingesteld, zodra deze is geconfigureerd en geüpload.  Als u wilt verzenden van grote hoeveelheden historische gegevens van TSI en niet uploaden of referentiegegevens eerst maken in de TSI mogelijk hebt u uw werk opnieuw toepassen (hint, niet veel plezier met).  

Ga voor meer informatie over het maken en beheren van uw referentiegegevens in TSI uploaden naar onze [gegevensset referentiedocumentatie](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-add-reference-data-set).

## <a name="business-disaster-recovery"></a>Noodherstel voor bedrijven
Als een Azure-service biedt Time Series Insights hoge beschikbaarheid (HA) met behulp van redundantie op het niveau van de Azure-regio, zonder extra werk vereist voor de oplossing. De Microsoft Azure-platform bevat ook functies voor informatie over het bouwen van oplossingen met mogelijkheden voor disaster recovery (DR) of regio-overschrijdende beschikbaarheid. Als u wilt voor globale, interregionale hoge beschikbaarheid voor apparaten of gebruikers profiteren van deze functies Azure herstel na Noodgevallen. Het artikel [technische richtlijnen voor Azure Business Continuity](../resiliency/resiliency-technical-guidance.md) beschrijving van de ingebouwde functies in Azure voor bedrijfscontinuïteit en herstel na Noodgevallen. De [herstel na noodgevallen en hoge beschikbaarheid voor Azure-toepassingen](https://docs.microsoft.com/azure/architecture/resiliency/index) document bevat architectuurrichtlijnen voor strategieën voor Azure-toepassingen om HA en DR te realiseren.

Azure Time Series Insights heeft geen ingebouwde noodherstel (BCDR).
Standaard hebben zowel Azure IoT Hub en Event Hubs herstel, zijn geïntegreerd.

Ga voor meer informatie over het beleid voor IoT-Hub BCDR [hier](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr).  

Ga voor meer informatie over het beleid van de Event hub BCDR [hier](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).

Klanten die behoefte BCDR hebben kunnen echter nog steeds een strategie voor herstel met behulp van de volgende methode implementeren.
Door het maken van een tweede Time Series Insights-omgeving in een back-Azure-regio maken en te verzenden-gebeurtenissen op deze secundaire omgeving van de primaire gegevensbron, gebruik te maken van een tweede speciale klantengroep en van de bron van die gebeurtenis BCDR richtlijnen.  

1.  In de tweede regio omgeving te maken.  Meer informatie over het maken van een Time Series Insights-omgeving [hier](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
2.  Maken van een tweede speciale klantengroep voor uw gebeurtenisbron en de bron van die gebeurtenis verbinden met de nieuwe omgeving.  Zorg ervoor dat de tweede, toegewezen consumergroep aanwijzen.  U meer informatie over dit door een [IoT Hub-documentatie](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) of [Event hub-documentatie](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access).
3.  Als uw primaire regio uitvallen tijdens een incident na noodgevallen, zijn overgestapt van bewerkingen aan de back-up Time Series Insights-omgeving.  

Het is **belangrijk te weten** tijdens een failover-scenario kunnen er een vertraging optreden voordat TSI verwerken van berichten opnieuw kunt starten: kan dit leiden tot een piek in de verwerking van berichten. Kijk eens voor meer informatie [dit document](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency)

## <a name="next-steps"></a>Volgende stappen
- [Een Event Hub-gebeurtenisbron toevoegen](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Een IoT Hub-gebeurtenisbron toevoegen](time-series-insights-how-to-add-an-event-source-iothub.md)
