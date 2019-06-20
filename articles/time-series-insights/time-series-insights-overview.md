---
title: 'Overzicht: Wat is Azure Time Series Insights? | Microsoft Docs'
description: Inleiding tot Azure Time Series Insights, een nieuwe service voor de analyse van tijdreeksgegevens en IoT-oplossingen.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: overview
ms.date: 04/26/2019
ms.custom: seodec18
ms.openlocfilehash: ce18b0060fe0bd0f9d5a8308fe1358e300c04441
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165630"
---
# <a name="what-is-azure-time-series-insights"></a>Wat is Azure Time Series Insights?

Azure Time Series Insights is gebouwd voor het opslaan, visualiseren en query uitvoeren op grote hoeveelheden time series-gegevens, zoals die worden gegenereerd door de IoT-apparaten. Als u tijdreeksgegevens wilt opslaan, beheren, opvragen of visualiseren in de cloud, is Time Series Insights waarschijnlijk de juiste keuze voor u. 

![Stroomdiagram van Time Series Insights](media/overview/time-series-insights-flowchart.png)

Time Series Insights heeft vier belangrijke taken:

- Het volledig geïntegreerd met cloud-gateways, zoals Azure IoT Hub en Azure Event Hubs. Hierdoor kan er eenvoudig verbinding worden maakt met deze gebeurtenisbronnen en kan er JSON worden geparseerd uit berichten en structuren met gegevens in overzichtelijke rijen en kolommen. Metagegevens worden samengevoegd met telemetriegegevens en de gegevens worden geïndexeerd in een columnar store.
- Time Series Insights beheert de opslag van uw gegevens. Om ervoor te zorgen dat gegevens altijd eenvoudig toegankelijk is, wordt uw gegevens in het geheugen en SSD's opgeslagen voor maximaal 400 dagen. U kunt interactief miljarden gebeurtenissen in seconden op aanvraag opvragen.
- Time Series Insights biedt out-of-the-box-visualisatie via de Verkenner van Time Series Insights. 
- Time Series Insights biedt een queryservice, zowel in de Verkenner van Time Series Insights en via API's eenvoudig te integreren voor het insluiten van uw time series-gegevens in aangepaste toepassingen.

Als u een toepassing voor externe klanten te gebruiken of voor intern verbruik bouwt, kunt u Time Series Insights gebruiken als een back-end. U kunt deze index, archief en statistische time series-gegevens. Voor het bouwen van een aangepaste visualisatie en gebruikerservaring bovenaan, gebruikt u de [Client SDK](tutorial-explore-js-client-lib.md). Time Series Insights is ook voorzien van verschillende [Query-API's](how-to-shape-query-json.md) aangepast zodat deze scenario's.

Tijdreeksgegevens tonen aan hoe een asset of proces verandert in de loop der tijd. Time series-gegevens wordt geïndexeerd door tijdstempels en -tijd is de meest zinvolle as langs waarop dergelijke gegevens zijn onderverdeeld. Time series-gegevens worden meestal binnenkomt in opeenvolgende volgorde, dus meestal wordt dit beschouwd als een invoegen in plaats van een update voor uw database.

Het kan lastig zijn om te slaan, indexeren, query, analyseren en visualiseren van time series-gegevens in grote volumes.
Azure Time Series Insights legt vast en slaat u elke nieuwe gebeurtenis als een rij, en de wijziging efficiënt na verloop van tijd wordt gemeten. Als gevolg hiervan kunt u zoeken achterwaartse om te verwerven in het verleden om u te helpen bij het voorspellen van toekomstige wijzigen.

## <a name="video"></a>Video

### <a name="learn-more-about-azure-time-series-insights-the-cloud-based-iot-analytics-platformbr"></a>Meer informatie over Azure Time Series Insights, het cloud-gebaseerde IoT analytics-platform.</br>

[![VIDEO](https://img.youtube.com/vi/GaARrFfjoss/0.jpg)](https://www.youtube.com/watch?v=GaARrFfjoss)

## <a name="primary-scenarios"></a>Primaire scenario's

- Time series-gegevens in een schaalbare manier om Store. 

   De basis van Time Series Insights is een database die speciaal is ontworpen voor het werken met tijdreeksgegevens. Omdat het schaalbare en volledig beheerde, verwerkt de Time Series Insights het werk van het opslaan en beheren van gebeurtenissen.

- Verken de gegevens in bijna realtime. 

   Time Series Insights biedt een Verkenner die alle gegevens worden gevisualiseerd die gegevensstromen in een omgeving. Kort nadat u verbinding met een bron van gebeurtenis maakt, kunt u bekijken, verkennen en gebeurtenisgegevens binnen de Time Series Insights. De gegevens helpen u om te valideren of een apparaat gegevens verzendt zoals verwacht en het controleren van een IoT-asset voor status, productiviteit en totale efficiëntie. 

- Uitvoeren van de hoofdoorzaak wordt onderzocht en afwijkingen.

   Time Series Insights biedt hulpmiddelen, zoals patronen en Perspectiefweergaven voor het uitvoeren en opslaan met meerdere stappen hoofdoorzaak wordt onderzocht. Time Series Insights zijn ook werkt met waarschuwingen van services, zoals Azure Stream Analytics, zodat u kunt waarschuwingen weergeven en gedetecteerde afwijkingen in bijna realtime in de Verkenner van Time Series Insights. 

- Krijg een algemeen overzicht van time series-gegevens die vanaf verschillende locaties voor meerdere asset of site vergelijking streams.

   U kunt meerdere bronnen van gebeurtenissen verbinden met een Time Series Insights-omgeving. Op deze manier kunt u gegevens die de vanaf meerdere, afzonderlijke locaties samen in bijna realtime gegevensstromen weergeven. Gebruikers kunnen profiteren van deze zichtbaarheid gegevens kunnen delen met leidinggevenden. Ze kunnen beter samenwerken met de domein-experts die hun ervaring om u te helpen bij het oplossen van problemen met, toepassen van aanbevolen procedures en geleerde lessen delen kunnen toepassen.

- Bouw een toepassing op Time Series Insights. 

   Time Series Insights beschrijft REST Query API's die u gebruiken kunt om toepassingen die gebruikmaken van time series-gegevens te bouwen.

## <a name="capabilities"></a>Functionaliteit

- **Snel aan de slag**: Azure Time Series Insights vereist geen voorbereiding van gegevens, zodat u snel verbinding met miljoenen gebeurtenissen in uw IoT-hub of event hub kunt maken. Nadat u verbinding hebt gemaakt, kunt u visualiseren en communiceren met sensorgegevens om snel te valideren uw IoT-oplossingen. U kunt met uw gegevens werken zonder code te schrijven en hoeft u voor meer informatie over een nieuwe taal. Time Series Insights een gedetailleerde queryruimte voor vrije tekst biedt voor ervaren gebruikers, en wijst en klik-mogelijkheid.

- **Bijna realtime inzichten**: Time Series Insights kan opnemen van miljoenen sensorgebeurtenissen per dag, met één minuut latentie. Time Series Insights biedt u meer inzicht in uw sensorgegevens. Deze gebruiken om trends en afwijkingen te herkennen, oorzaak-gevolganalyses uitvoeren en kostbare stilstand te vermijden. Cross-correlatie tussen realtime en historische gegevens kunt u verborgen trends in de gegevens te zoeken.

- **Aangepaste oplossingen bouwen**: Azure Time Series Insights-gegevens insluiten in uw bestaande toepassingen. U kunt ook nieuwe aangepaste oplossingen maken met de Time Series Insights REST API's. U kunt bovendien persoonlijke weergaven maken die u kunt delen met anderen om zo uw inzichten breder beschikbaar te maken.

- **Schaalbaarheid**: Time Series Insights kan zeer grote hoeveelheden IoT-gegevens opnemen. De service kan 1 miljoen tot 100 miljoen gebeurtenissen per dag worden opnemen, met een standaardbewaarperiode van 31 dagen. U kunt visualiseren en analyseren van live gegevensstromen bijna in realtime, samen met historische gegevens.

## <a name="get-started"></a>Aan de slag

Volg deze stappen om te beginnen.

1. Richt een Time Series Insights-omgeving in Azure portal.
1. Verbinding maken met een bron van gebeurtenis, zoals een IoT-hub of een event hub. 
1. Naslaginformatie over gegevens uploaden. Dit is niet een extra service.
1. Bekijk de gegevens na enkele minuten met de verkenner van Time Series Insights.

## <a name="time-series-insights-explorer"></a>Verkenner van Time Series Insights

In dit diagram toont een voorbeeld van timeseries insights-gegevens weergegeven in de Verkenner van Time Series Insights.

![Verkenner van Time Series Insights](media/time-series-insights-explorer/explorer4.png)

## <a name="next-steps"></a>Volgende stappen

- De algemene beschikbaarheid van Azure Time Series Insights verkennen [gratis demo-omgeving](./time-series-quickstart.md).
- Meer informatie over het [van plan bent uw Time Series Insights](time-series-insights-environment-planning.md) omgeving.
