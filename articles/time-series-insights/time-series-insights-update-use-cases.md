---
title: Gebruiksvoorbeelden voor Azure Time Series Insights (preview) | Microsoft Docs
description: Gebruiksvoorbeelden voor inzicht in Azure Time Series Insights (preview)
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: 67be21ae7f0cb997563f17130b9d5ecb7d359b31
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873866"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Gebruiksvoorbeelden voor Azure Time Series Insights (Preview)

Dit artikel bevat een overzicht van enkele algemene scenario's voor Azure Time Series Insights (TSI). De aanbevelingen in dit artikel fungeren als een beginpunt tijdens het ontwikkelen van toepassingen en oplossingen met TSI.

Na het lezen van dit artikel, zal het mogelijk om de volgende vragen te beantwoorden:

* Wat zijn de algemene scenario's voor Azure TSI?
* Wat zijn de voordelen van het gebruik van Azure TSI voor gegevensverkenning en visuele anomaliedetectie?
* Wat zijn de voordelen van het gebruik van Azure TSI voor operationele analyses en efficiëntie van processen?
* Wat zijn de voordelen van het gebruik van Azure TSI voor geavanceerde analyses?

Dit document bevat een overzicht van de toepassingen die de Azure TSI Private Preview-versie is ontworpen voor.

## <a name="introduction"></a>Inleiding

Azure TSI is een end-to-end Platform-As-A-Service voor het opnemen, verwerken, opslaan en maximaal contextualized, time series geoptimaliseerd IoT-schaal gegevens op te vragen. Hierdoor is de Azure TSI ideaal voor ad-hocgegevensverkenning, evenals operationele analyse. TSI is een unieke uitbreidbare, aangepaste, voorziet dat voldoet aan de algemene van industriële IoT-implementaties behoeften.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Gegevensverkenning en visuele afwijkingsdetectie

Verken en analyseer direct miljarden gebeurtenissen om afwijkingen te vinden en verborgen trends in uw gegevens te ontdekken. Azure TSI voorziet in bijna realtime prestaties voor uw IoT- en DevOps workloads voor analyse.

![Data explorer][1]

Van alle voordelen van Azure TSI, de meeste klanten gaat ermee akkoord dat de tijd-tot-inzicht tot de sterkste behoort. TSI vereist geen gegevensvoorbereiding vooraf en werkt snel, u verbinding maken met miljarden gebeurtenissen in uw Azure IoT Hub of Event Hub in minuten.  Eenmaal verbinding hebben, kunt u visualiseren en analyseren van miljarden gebeurtenissen die moeten worden afwijkingen te vinden en verborgen trends te ontdekken in uw gegevens.  Omdat TSI intuïtief en eenvoudig te gebruiken, moet u beginnen met het interactie met uw gegevens zonder één regel code te schrijven. U hoeft ook geen nieuwe taal te leren: Time Series Insights biedt nauwkeurige query's op basis van tekst voor gevorderde gebruikers die SQL kennen, terwijl nieuwe gebruikers gegevens kunnen verkennen door er op te selecteren en te klikken.

We zien klanten profiteren van deze snelheid voor het vaststellen van problemen met betrekking tot asset snel bij het uitvoeren van DevOps om te gaan naar de hoofdoorzaak van een bug in een IoT-oplossing, en bij het identificeren van de gebieden te onderzoeken voor data science-initiatieven.  

Er zijn drie primaire manieren om te communiceren met gegevens die zijn opgeslagen in de TSI:

1. De eerste en eenvoudigste manier om aan de slag met is onze visualisatie, de explorer, waarmee u al uw IoT-gegevens op één plek snel te visualiseren. Het biedt hulpprogramma's zoals heatmap, waarmee u visueel herkennen afwijkingen in uw gegevens eenvoudig. Het biedt ook de perspectiefweergave, zodat u maximaal vier weergaven uit een of meer TSI-omgevingen in één dashboard vergelijken, zodat u een weergave van uw tijdseriegegevens op al uw locaties. Meer informatie over de [TSI-Verkenner](./time-series-insights-update-explorer.md). Als u van plan bent om uw omgeving TSI-update, wilt lezen [TSI planning bijwerken](./time-series-insights-update-plan.md).

1. De tweede manier is het gebruik van onze JavaScript-SDK snel krachtige grafieken en diagrammen insluiten in uw eigen web-App. Met slechts een paar regels code, kunt u krachtige query's om in te vullen lijndiagrammen, cirkeldiagrammen, staafdiagrammen, heatmaps beschikbaar zijn, aan gegevensrasters en meer maken. Al deze elementen bestaat out-of-the-box met behulp van de SDK. De SDK isoleert ook TSI-query's, zodat u kunt SQL-achtige predicaten query uitvoeren op de gegevens die u wilt weergeven in een dashboard maken. Voor oplossingen voor hybride presentatie-laag biedt TSI geparameteriseerde URL's die voorzien in naadloze verbindingspunten met de explorer deep dives in gegevens. Lees voor meer informatie over de JavaScript-SDK, de [TSI JS-clientbibliotheek](https://docs.microsoft.com/azure/time-series-insights/tutorial-explore-js-client-lib) en de [TSI client](https://github.com/Microsoft/tsiclient) documentatie. Voor meer informatie over geparameteriseerde URL's, raadpleegt u ons artikel over [geparametriseerde URL's](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-parameterized-urls).  

1. Ten slotte biedt TSI krachtige API's voor het opvragen van gegevens die zijn opgeslagen in TSI. TSI bevat tijdelijke operators zoals uit, eerste en laatste aggregaties en transformaties, zoals gemiddelde, min, max, delen door, order by- en DateHistogram en operators, zoals het filteren is, in en, of, groter is dan reguliere EXPRESSIE, enzovoort. Alle deze operators downstream toepassingen snel interessante trends en patronen in uw gegevens zoeken en kunnen worden gebruikt voor het vullen van zelfgemaakte visualisaties aan afwijkingen te vinden.  

## <a name="operational-analysis-and-driving-process-efficiency"></a>Operationele analyse en efficiëntere processen

Schakel de bewaking in de status, het gebruik en de prestaties van de apparatuur van op grote schaal, waardoor er een eenvoudige manier voor het meten van de operationele efficiëntie. Time Series Insights helpt bij het beheer van uiteenlopende en onvoorspelbare IoT-workloads, zonder de opname- en queryprestaties te verminderen.

![overview][2]

Streaming en continue verwerking van gegevens die afkomstig zijn van operationele processen met succes een gedaanteverwisseling elk bedrijf als in combinatie met de juiste technologie/oplossing. Deze oplossingen zijn vaak een combinatie van verschillende systemen voor het inschakelen van verkenning en analyse van gegevens die voortdurend wordt gewijzigd in IoT-realm speciaal en delen van een algemeen patroon.

Deze patronen beginnen vaak met IoT ingeschakeld platforms die miljarden gebeurtenissen van apparaten en sensoren spanning diverse landinstellingen opnemen. Deze systemen verwerken en analyseren van streaming-gegevens als u wilt afleiden van realtime inzichten en acties, gegevens worden meestal gearchiveerd naar warme en koude opslag voor in de buurt van real-time en batch-analyses. Gegevens die worden verzameld, verloopt via reeks verwerking te reinigen en context kunnen worden geplaatst voor downstream scenario's voor query's en analyses. Microsoft Azure biedt uitgebreide services die kunnen worden toegepast op deze IoT-scenario's (Asset onderhoud, productie, enz.). Het gaat hierbij om Azure TSI, Azure IoT Hub, Azure Event Hubs, Azure Stream Analytics, Azure Functions, Azure Logic Apps, Azure Databricks, Azure Machine Learning en Microsoft Power BI.

Architectuur voor deze oplossing kan worden bereikt als volgt: opnemen van gegevens via Azure IoT Hub of Azure Event Hub voor de best mogelijke beveiliging, doorvoer en latentie. Gegevensverwerking en -berekeningen uitvoeren door funneling opgenomen gegevens via services zoals Azure Stream Analytics, Azure Logic Apps, Azure Functions, afhankelijk van de behoeften van de specifieke gegevens verwerken. Berekende signalen van de verwerkings-pipeline worden gepusht naar Azure TSI voor het opslaan en analytics. Azure Time Series Insights biedt in de buurt van real-time gegevens verkennen en inzichten op basis van een asset historische gegevens. Afhankelijk van de bedrijfsbehoeften kunnen MapReduce en Hive-taken worden uitgevoerd op gegevens die zijn opgeslagen in Time Series Insights door Time Series Insights verbinding te maken met HDInsight. Gegevens die zijn opgeslagen in Time Series Insights kunnen beschikbaar worden gemaakt naar Power BI en andere toepassingen van klanten via Time Series Insights openbare surface-query's voor uitvoerige zakelijke en operationele intelligentie scenario's.

## <a name="advanced-analytics"></a>Geavanceerde analyse

Integreer met geavanceerde analyseservices zoals Azure Machine Learning en Azure Databricks. Time Series Insights verzamelt onbewerkte gegevens van miljoenen apparaten en voegt daar contextuele gegevens aan toe die naadloos door een suite van Azure-analyseservices kunnen worden gebruikt.

![analytics][3]

Geavanceerde analyses en machine learning gebruiken en verwerken van grote hoeveelheden gegevens op gegevens gebaseerde beslissingen te nemen en voorspellende analyses uitvoeren. In gevallen IoT leert algoritmen voor geavanceerde analyses van de gegevens die worden verzameld van miljoenen apparaten die kunnen worden verzonden gegevens meerdere keren per seconde. De gegevens die worden verzameld van IoT-apparaten is echter onbewerkte en beschikt niet over de contextuele informatie zoals de locatie van het apparaat, eenheid van de sensor lezen enz., dus waardoor het moeilijk zijn de gegevens worden rechtstreeks voor geavanceerde analyses worden gebruikt.

Azure Time Series Insights een brug tussen IoT-gegevens en geavanceerde analyses op twee manieren voor eenvoudige en voordelige. Eerst update Time Series Insights verzamelt onbewerkte telemetriegegevens van miljoenen apparaten met behulp van IoT hub, verrijkt gegevens met contextuele informatie en gegevens worden getransformeerd in 'parquet-indeling', dat kan eenvoudig worden geïntegreerd met een aantal geavanceerde analyseservices zoals Als Azure Machine Learning, Azure Databricks en andere toepassingen van derden.  Time Series Insights kan fungeren als de bron-van-waarheid voor alle gegevens in een organisatie, dus het maken van een centrale opslagplaats voor downstream analytics workloads om te gebruiken.  Omdat Time Series Insights is een bijna realtime storage-service, geavanceerde analyses modellen kunnen continu leren van binnenkomende telemetrie van IoT-gegevens om nauwkeurigere voorspellingen te doen.

Ten tweede kan Time Series Insights worden ingevoerd de uitvoer van machine learning- en voorspellingsgegevens modellen om te visualiseren en opslaan van de resultaten ervan, en helpt organisaties om te optimaliseren en hun modellen aanpassen.  Sterker nog, kunt Time Series Insights u eenvoudig voor het visualiseren van streaming telemetrische gegevens op het vlak van hetzelfde als het getrainde model levert zodat data-scienceteams afwijkingen te vinden en patronen te identificeren.  

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de [TSI-Verkenner](./time-series-insights-update-explorer.md).

Als u van plan bent om uw omgeving, wilt lezen [TSI (Preview) plannen](./time-series-insights-update-plan.md).

Lees de [TSI client](https://github.com/Microsoft/tsiclient) documentatie.

<!-- Images -->
[1]: media/v2-update-use-cases/data-explorer.svg
[2]: media/v2-update-use-cases/overview.svg
[3]: media/v2-update-use-cases/advanced-analytics.svg
