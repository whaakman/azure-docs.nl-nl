---
title: 'Overzicht: Azure Time Series Insights (preview) | Microsoft Docs'
description: Overzicht van Azure Time Series Insights (preview).
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: anshan
ms.workload: big-data
ms.topic: overview
ms.date: 04/26/2019
ms.custom: seodec18
ms.openlocfilehash: 6bcf0155be87a5b36c6b850fa889e23a340ac7ec
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237518"
---
# <a name="azure-time-series-insights-preview"></a>Azure Time Series Insights preview

Azure Time Series Insights Preview is een end-to-end platform-as-a-service (PaaS) aanbieding. Het wordt gebruikt voor het verzamelen, verwerken, opslaan, analyseren en maximaal contextualized, time series geoptimaliseerd IoT-schaal gegevens op te vragen. Time Series Insights is ideaal voor ad-hoc gegevens verkennen en operationele analyse. Time Series Insights is een uniek uitbreidbare en aangepaste service die voldoet aan de algemene vereisten voor industriële IoT-implementaties.

> [!TIP]
> Voor functies in het algemeen beschikbaar is (GA), leest de [overzicht van Azure Time Series Insights GA](time-series-insights-overview.md).

## <a name="video"></a>Video

### <a name="learn-more-about-azure-time-series-insights-preview-br"></a>Meer informatie over Azure Time Series Insights Preview. </br>

> [!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Time-Series-Insights-e2e-solution-for-industrial-IoT-analytics/player]

## <a name="define-iot-data"></a>IoT-gegevens definiëren

IoT-gegevens zijn alle industriële gegevens die beschikbaar is in de asset-intensieve organisaties. IoT-gegevens zijn vaak zeer ongestructureerd omdat deze vanuit assets worden verzonden die metingen verrichten met veel ruis. Deze metingen zijn temperatuur, beweging en vochtigheid. Deze gegevensstromen worden vaak gekenmerkt door aanzienlijke hiaten, beschadigde berichten en verkeerde metingen. Gegevens van die stromen moeten worden opgeschoond voordat er een analyse kan worden uitgevoerd.

IoT-gegevens zijn vaak alleen zinvol in de context van de invoer van aanvullende gegevens die afkomstig zijn van eigen bronnen, zoals CRM en ERP. Invoer kan ook afkomstig zijn van externe gegevensbronnen, zoals het weer en de locatie.

Daardoor wordt alleen een fractie van de gegevens gebruikt voor operationele en zakelijke doeleinden. Deze gegevens bieden consistente, uitgebreide, actuele en juiste informatie voor rapportage en analyse. Voor het omzetten van verzamelde IoT-gegevens in bruikbare inzichten zijn de volgende zaken nodig:

* Gegevensverwerking voor het opschonen, filteren, interpoleren, transformeren en voorbereiden van gegevens voor analyse.
* Een structuur om te navigeren door de gegevens en deze te begrijpen (om de gegevens te normaliseren en contextualiseren).
* Voordelige opslag voor lange/oneindige bewaarperioden voor decennia aan verwerkte (of afgeleide) gegevens, alsmede onbewerkte gegevens.

In de volgende afbeelding wordt een typische IoT-gegevensstroom weergegeven.

  ![IoT-gegevensstroom][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>Azure Time Series Insights voor industriële IoT

Het huidige IoT-landschap is gevarieerd. Klanten komen uit vele branches: productie, automotive, energie, nutsvoorzieningen, slimme gebouwen en advies. Scenario's zijn ad-hoc gegevensverkenning waar de vorm van de gegevens onbekend is. Het kan ook zijn dat er operationele analyses worden uitgevoerd voor geschematiseerde of expliciet gemodelleerde gegevens om de operationele efficiëntie te vergroten. Deze scenario's bestaan doorgaans naast elkaar en bieden ondersteuning voor verschillende soorten toepassingen. Platformmogelijkheden die essentieel zijn voor het succes van industriële IoT-ondernemingen en hun digitale revolutie:

- Meerlaagse opslag (semi-dynamisch en niet dynamisch).
- De mogelijkheid om tientallen jaren aan Time Series-gegevens op te slaan.
- De mogelijkheid om expliciet query's voor op assets gebaseerde operationele intelligence te modelleren en optimaliseren.

Time Series Insights is een uitgebreide, end-to-end PaaS voor IoT-gegevens verkennen en operational insights. Time Series Insights biedt een volledig beheerde cloudservice voor het analyseren van IoT-tijdreeksgegevens.

U kunt de onbewerkte gegevens opslaan in een schemaloos archief in het geheugen. U kunt vervolgens interactieve ad-hocquery's uitvoeren via een engine voor gedistribueerde query's en -API. Gebruik de uitgebreide gebruikerservaring om binnen enkele seconden miljarden gebeurtenissen te kunnen visualiseren. Meer informatie over onze [mogelijkheden voor gegevensverkenning](./time-series-insights-overview.md).

Time Series Insights biedt ook mogelijkheden voor operationele inzichten die momenteel in openbare preview zijn. Samen met interactieve gegevensverkenning en operationele intelligentie stelt Time Series Insights u in staat meer waardevolle informatie af te leiden uit gegevens die verzameld zijn van IoT-apparaten. De preview-versie biedt ondersteuning voor:

* Een schaalbare en prestaties en kosten geoptimaliseerd time series-gegevens worden opgeslagen. Met deze cloudgebaseerde IoT-oplossing kunnen jaren aan gegevens binnen enkele seconden worden geanalyseerd.
* Bism-ondersteuning die worden beschreven van het domein en de metagegevens die zijn gekoppeld aan de afgeleide en nonderived signalen van assets en apparaten.
* Een verbeterde gebruikerservaring en inzicht in gegevens op basis van een asset met uitgebreide, ad hoc gegevensanalyse combineert. Dit is ter ondersteuning van zakelijke en operationele intelligence.
* Integratie met hulpprogramma's voor geavanceerde machine learning en analyses. Deze hulpprogramma's omvatten Azure Databricks, Apache Spark, Azure Machine Learning, Jupyter-notebooks en Power BI. Met deze hulpprogramma's kunt u Time Series-gegevensuitdagingen aan en de operationele efficiëntie vergroten.

Operationele inzichten en gegevensverkenning worden samen geleverd met een eenvoudig prijsmodel op basis van betalen per gebruik voor gegevensverwerking, opslag en query's. Dit factureringsmodel sluit aan op uw veranderende zakelijke behoeften.

In dit gegevensstroomdiagram op hoog niveau ziet u de updates.

  ![Belangrijkste mogelijkheden][2]

Met de introductie van deze belangrijke IoT-functionaliteiten biedt Time Series Insights de volgende belangrijke voordelen.

| | |
| ---| ---|
| Meerlaagse opslag voor IoT-schaal time series-gegevens | Met een algemene pijplijn voor gegevensverwerking voor het opnemen van gegevens kunt u gegevens opslaan in semi-dynamische opslag voor interactieve query's. U kunt ook gegevens opslaan in niet-dynamische opslag, bijvoorbeeld als het om grote hoeveelheden gegevens gaat. Profiteer van op assets gebaseerde [query's](./time-series-insights-update-tsq.md) met hoge prestaties. |
| Time Series-Model onbewerkte telemetrie context kunnen worden geplaatst en leiden hieruit inzichten op basis van een asset | Contextualiseer onbewerkte telemetriegegevens met het beschrijvende [Time Series-model](./time-series-insights-update-tsm.md). Leid uitgebreide operationele intelligentie af met de uitstekend presterende en voordelige op apparaten gebaseerde query's. |
| Vloeiende en continue integratie met andere data-oplossingen | Gegevens in Time Series Insights worden [opgeslagen](./time-series-insights-update-storage-ingress.md) in open source Apache Parquet-bestanden. Deze integratie met andere gegevensoplossingen, of deze nu van uzelf of van een externe partij zijn, leent zich goed voor end-to-endscenario's. Dit zijn bijvoorbeeld scenario's met business intelligence, geavanceerde machine learning en predictive analytics. |
| Gegevens worden bijna in realtime verkend | De [verkenner van Azure Time Series Insights (preview)](./time-series-insights-update-explorer.md) biedt gebruikers visualisatie voor alle gegevens die door de opnamepijplijn worden geleid. Kort nadat u verbinding hebt gemaakt met een gebeurtenisbron, kunt u gebeurtenisgegevens bekijken, verkennen en opvragen. Op deze manier kunt u nagaan of een apparaat gegevens naar verwachting verzendt. U kunt IoT-apparaten ook controleren op status, productiviteit en algehele effectiviteit. |
| Hoofdoorzaakanalyse en anomaliedetectie | De [verkenner van Azure Time Series Insights (preview)](./time-series-insights-update-explorer.md) ondersteunt zowel patronen als perspectiefweergaven voor het uitvoeren en opslaan van een hoofdoorzaakanalyse die uit meerdere stappen bestaat. In combinatie met Azure Stream Analytics kunt u Time Series Insights gebruiken om waarschuwingen en afwijkingen vrijwel in realtime te detecteren. |
| Aangepaste toepassingen die zijn gebouwd op Time Series Insights-platform | Time Series Insights biedt ondersteuning voor de [JavaScript SDK](./tutorial-explore-js-client-lib.md). De SDK biedt uitgebreide besturingselementen en vereenvoudigde toegang tot query's. Gebruik de SDK voor het bouwen van aangepaste IoT-toepassingen op basis van Time Series Insights, die aansluiten op uw specifieke zakelijke behoeften. U kunt de [query-API's](./time-series-insights-update-tsq.md) van Time Series Insights ook rechtstreeks gebruiken om gegevens naar aangepaste IoT-toepassingen te sturen. |

## <a name="next-steps"></a>Volgende stappen

Aan de slag met Azure Time Series Insights Preview:

> [!div class="nextstepaction"]
> [Lees de Quickstart-handleiding](./time-series-insights-update-quickstart.md)

Informatie over use cases:

> [!div class="nextstepaction"]
> [Use cases voor Azure Time Series Insights (preview)](./time-series-insights-update-use-cases.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png
