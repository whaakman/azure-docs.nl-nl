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
ms.date: 11/27/2018
ms.openlocfilehash: 9d9fab9f0a515cacdf2a1425c4da06c9e3d4c364
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856133"
---
# <a name="azure-time-series-insights-preview-use-cases"></a>Gebruiksvoorbeelden voor Azure Time Series Insights (preview)

Dit artikel bevat een overzicht van enkele algemene scenario's voor Azure Time Series Insights (TSI). De aanbevelingen in dit artikel fungeren als een beginpunt tijdens het ontwikkelen van toepassingen en oplossingen met TSI.

Na het lezen van dit artikel, zal het mogelijk om de volgende vragen te beantwoorden:

* Wat zijn de algemene scenario's voor Azure TSI?
* Wat zijn de voordelen van het gebruik van Azure TSI voor gegevensverkenning en visuele anomaliedetectie?
* Wat zijn de voordelen van het gebruik van Azure TSI voor operationele analyses en efficiëntie van processen?
* Wat zijn de voordelen van het gebruik van Azure TSI voor geavanceerde analyses?

Dit document bevat een overzicht van de toepassingen die de Azure Time Series Insights Private Preview is ontworpen voor.

## <a name="data-exploration-and-visual-anomaly-detection"></a>Gegevensverkenning en visuele anomaliedetectie

Direct verkennen en analyseren van miljarden gebeurtenissen die moeten worden afwijkingen te vinden en verborgen trends in uw gegevens te detecteren. Azure TSI voorziet in bijna realtime prestaties voor uw IoT- en DevOps workloads voor analyse.

![Data explorer][1]

Van alle voordelen van Azure TSI, de meeste klanten gaat ermee akkoord dat de tijd-tot-inzicht tot de sterkste behoort. TSI vereist geen gegevensvoorbereiding vooraf en werkt snel, u verbinding maken met miljarden gebeurtenissen in uw Azure IoT Hub of Event Hub in minuten.  Eenmaal verbinding hebben, kunt u visualiseren en analyseren van miljarden gebeurtenissen die moeten worden afwijkingen te vinden en verborgen trends te ontdekken in uw gegevens.  Omdat TSI intuïtief en eenvoudig te gebruiken, moet u beginnen met het interactie met uw gegevens zonder één regel code te schrijven. U hoeft ook geen nieuwe taal te leren: Time Series Insights biedt nauwkeurige query's op basis van tekst voor gevorderde gebruikers die SQL kennen, terwijl nieuwe gebruikers gegevens kunnen verkennen door er op te selecteren en te klikken.

We zien klanten profiteren van deze snelheid voor het vaststellen van problemen met betrekking tot asset snel bij het uitvoeren van DevOps om te gaan naar de hoofdoorzaak van een bug in een IoT-oplossing, en bij het identificeren van de gebieden te onderzoeken voor data science-initiatieven.  

Er zijn drie primaire manieren om te communiceren met gegevens die zijn opgeslagen in de TSI:

1. De eerste en eenvoudigste manier om aan de slag met is onze visualisatie, de Explorer, waarmee u al uw IoT-gegevens op één plek snel te visualiseren. Het biedt hulpprogramma's zoals heatmap, waarmee u visueel herkennen afwijkingen in uw gegevens eenvoudig, evenals de perspectiefweergave, zodat u kunt maximaal vier weergaven van een of meer TSI-omgevingen in één dashboard, zodat u een weergave van uw time series-gegevens over vergelijken al uw locaties. Meer informatie over de [TSI-Verkenner](./time-series-insights-update-explorer.md). Als u van plan bent om uw omgeving TSI-update, wilt lezen [TSI planning bijwerken](./time-series-insights-update-plan.md).

1. De tweede manier is het gebruik van onze JavaScript-SDK snel krachtige grafieken en diagrammen insluiten in uw eigen web-App. Met slechts een paar regels code, kunt u krachtige query's om in te vullen lijndiagrammen, cirkeldiagrammen, staafdiagrammen, heatmaps beschikbaar zijn, aan gegevensrasters en meer maken. Al deze elementen bestaat out-of-the-box met behulp van de SDK. De SDK isoleert ook TSI-query's, zodat u kunt SQL-achtige predicaten query uitvoeren op de gegevens die u wilt weergeven in een dashboard maken. Voor oplossingen voor hybride presentatie-laag biedt TSI geparameteriseerde URL's die voorzien in naadloze verbindingspunten met de explorer deep dives in gegevens. Lees voor meer informatie over de JavaScript-SDK, de [TSI JS-clientbibliotheek](https://docs.microsoft.com/azure/time-series-insights/tutorial-explore-js-client-lib) en de [TSI client](https://github.com/Microsoft/tsiclient) documentatie. Voor meer informatie over geparameteriseerde URL's, raadpleegt u ons artikel over [geparametriseerde URL's](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-parameterized-urls).  

1. Ten slotte biedt TSI krachtige API's voor het opvragen van gegevens die zijn opgeslagen in TSI. TSI bevat tijdelijke operators zoals uit, eerste en laatste aggregaties en transformaties, zoals gemiddelde, min, max, delen door, order by- en DateHistogram en operators, zoals het filteren is, in en, of, groter is dan reguliere EXPRESSIE, enzovoort. Alle deze operators downstream toepassingen snel interessante trends en patronen in uw gegevens zoeken en kunnen worden gebruikt voor het vullen van zelfgemaakte visualisaties aan afwijkingen te vinden.  

Zie voor meer informatie over Azure-producten voor IoT, [maken van het Internet van uw dingen](https://www.microsoft.com/internet-of-things).

## <a name="operational-analysis-and-driving-process-efficiency"></a>Operationele analyses en aangedreven proces efficiëntie

Schakel de bewaking in de status, het gebruik en de prestaties van de apparatuur van op grote schaal, waardoor er een eenvoudige manier voor het meten van de operationele efficiëntie. Time Series Insights helpt bij het beheer van uiteenlopende en onvoorspelbare IoT-workloads zonder dat dit ten koste gaat van opname- en prestaties van query's.

![overview][2]

Streaming en continue verwerking van gegevens die afkomstig zijn van operationele processen met succes een gedaanteverwisseling elk bedrijf als in combinatie met de juiste technologie/oplossing. Deze oplossingen zijn vaak een combinatie van verschillende systemen voor het inschakelen van verkenning en analyse van gegevens die voortdurend speciaal in IoT-realm veranderen. Deze systemen samen licht van de scenario's, die delen van een algemeen patroon als het gaat om op te nemen, verwerken, opslaan, analyseren en visualiseren van de IoT-gegevens.

Onderdeel van de oplossing, systemen nodig om op te nemen van miljarden gebeurtenissen van apparaten en sensoren spanning diverse landinstellingen. Vervolgens worden deze systemen verwerken en analyseren van streaming gegevens realtime statistieken. De gegevens worden vervolgens naar warme en koude opslag voor in de buurt van real-time en batch-analyses gearchiveerd.

Vervolgens moeten deze systemen verwerken opruimen van de gegevens worden verzameld om in te schakelen en contextualization tijdens het opslaan van gegevens voor het inschakelen van downstream scenario's voor query's en analyses. Microsoft Azure biedt uitgebreide services die kunnen worden toegepast voor deze IoT-scenario's met inbegrip van Azure TSI, Azure IoT Hub, Azure Event Hubs, Azure Stream Analytics, Azure Functions, Azure Logic Apps, Azure Databricks, Azure Machine Learning en Microsoft Power BI.

Met de bovenstaande instellingen van de oplossing kunnen gegevens worden opgenomen via Azure IoT of Event Hubs als gegevensopname hoge doorvoer met lage latentie biedt. Gegevens die zijn opgenomen die moet worden verwerkt voor realtime inzicht kan Azure Stream Analytics, Azure Logic Apps en Azure Functions worden softwareproducten. Het resultaat voor realtime dashboarding vervolgens naar Power BI kan worden ingevoerd, evenals voor waarschuwingen en bewaking vergelijken met historische seeding kan worden geladen in Azure Time Series Insights. Gegevens die zijn opgenomen waarvoor gegevens verkennen in bijna realtime of ad-hoc uitvoeren van query's voor historische trends, rechtstreeks naar Azure Time Series Insights kan worden geladen. De gegevens geladen is gereed om te worden opgevraagd, samen met onbeperkte historische gegevens voor analyse van de operationele en analytics kunt u processen voor maximale efficiëntie te optimaliseren. Alle gegevens of alleen wijzigingen in gegevens die worden geladen kunnen meest recent worden gebruikt als verwijzingsgegevens als onderdeel van realtime analyses. Bovendien kunnen gegevens verder worden verfijnd en verwerkt door Azure Time Series Insights-gegevens verbinden met HDInsight voor toewijzen/verminderen, Hive, enz. taken. Ten slotte maken deze gegevens beschikbaar in Power BI en in elke gewenste Klanttoepassing via onze openbare surface-query's.

## <a name="advanced-analytics"></a>Geavanceerde analyse

Integreer met geavanceerde analyseservices zoals Azure Machine Learning en Azure Databricks. TSI ingresses onbewerkte gegevens van miljoenen apparaten en contextuele gegevens die naadloos kan worden gebruikt door een reeks Azure Analytics-services worden toegevoegd.

![analytics][3]

Geavanceerde analyses en machine learning gebruiken en verwerken van grote hoeveelheden gegevens op gegevens gebaseerde beslissingen te nemen en voorspellende analyses uitvoeren. In gevallen IoT leert algoritmen voor geavanceerde analyses van de gegevens die worden verzameld van miljoenen apparaten die kunnen worden verzonden gegevens meerdere keren per seconde. De gegevens die worden verzameld van IoT-apparaten is echter onbewerkte en beschikt niet over de contextuele informatie zoals de locatie van het apparaat, eenheid van de sensor lezen enzovoort. Deze gegevens kan niet rechtstreeks voor geavanceerde analyses worden gebruikt.

Azure TSI slaat een brug tussen IoT-gegevens en geavanceerde analyses op een eenvoudige en voordelige manier. TSI verzamelt onbewerkte telemetriegegevens van miljoenen apparaten, verrijkt gegevens met contextuele informatie en gegevens worden getransformeerd in 'parquet-indeling', dat kan eenvoudig worden geïntegreerd met een aantal Azure Advanced Analytics-Services zoals Azure Machine Learning, Azure DataBricks, en uw eigen toepassingen van derden. Geavanceerde analysemodellen kunnen continu leren van binnenkomende telemetrie van IoT-gegevens om nauwkeurigere voorspellingen te doen.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de [TSI-Verkenner](./time-series-insights-update-explorer.md).

* Als u van plan bent om uw omgeving, wilt lezen [TSI (preview) plannen](./time-series-insights-update-plan.md).

* Lees de [TSI client](https://github.com/Microsoft/tsiclient) documentatie.

<!-- Images -->
[1]: media/v2-update-use-cases/data-explorer.png
[2]: media/v2-update-use-cases/overview.png
[3]: media/v2-update-use-cases/advanced-analytics.png
