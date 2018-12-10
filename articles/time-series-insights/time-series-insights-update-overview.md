---
title: Overzicht van Azure Time Series Insights | Microsoft Docs
description: Overzicht van Azure Time Series Insights
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: anshan
ms.workload: big-data
ms.topic: overview
ms.date: 12/03/2018
ms.openlocfilehash: 83513ab9f7bee5c51fc909e5f4246105b6f145ca
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52876128"
---
# <a name="azure-time-series-insights-overview"></a>Overzicht van Azure Time Series Insights

Azure Time Series Insights (TSI) is een end-to-end Platform-as-a-Service voor het opnemen, verwerken, opslaan en opvragen van gegevens op IoT-schaal die maximaal gecontextualiseerd en als tijdreeks geoptimaliseerd zijn. Dat maakt Azure TSI ideaal voor ad-hoc gegevensverkenning en voor operationele analyse. TSI is een uniek uitbreidbare, aangepaste service die voldoet aan de algemene vereisten voor industriële IoT-implementaties.

## <a name="what-is-iot-data"></a>Wat zijn IoT-gegevens?

IoT-gegevens zijn 'industriële' gegevens die beschikbaar zijn in organisaties die met veel assets werken. IoT-gegevens zijn vaak in hoge mate ongestructureerd omdat ze worden verzonden vanuit een breed scala van assets die metingen met de nodige ruis vastleggen, zoals temperatuur, beweging en vochtigheid. Deze gegevensstromen worden bovendien vaak gekenmerkt door aanzienlijke hiaten, beschadigde berichten en verkeerde metingen. Gegevens van die stromen moeten worden opgeschoond voordat er een analyse kan worden uitgevoerd. IoT-gegevens zijn vaak alleen zinvol binnen de context van aanvullende gegevens die afkomstig zijn van interne (zoals CRM of ERP) of externe gegevensbronnen (bijvoorbeeld het weer of de locatie).

Daardoor wordt alleen een te verwaarlozen fractie van de gegevens gebruikt voor operationele en zakelijke doeleinden. Deze gegevens bieden consistente, uitgebreide, actuele en juiste informatie voor rapportage en analyse. Om verzamelde IoT-gegevens te kunnen omzetten in bruikbare inzichten zijn daarom enkele belangrijke functionaliteiten vereist:

* Gegevensverwerking voor het opschonen, filteren, interpoleren, transformeren en voorbereiden van gegevens voor analyse
* Structuur om te navigeren door de gegevens en deze te begrijpen (om de gegevens te normaliseren en contextualiseren)
* Voordelige opslag voor lange/oneindige bewaarperioden voor decennia aan verwerkte (afgeleide) gegevens, alsmede onbewerkte gegevens

Een typische IoT-gegevensstroom kan als volgt worden weergegeven:

  ![IoT-gegevensstroom][1]

## <a name="azure-time-series-insights-for-industrial-iot"></a>Azure Time Series Insights voor industriële IoT

Het huidige IoT-landschap is gevarieerd. Het omvat klanten uit sectoren als productie, automotive, olie en gas, energie, slimme gebouwen en consulting. Tot de scenario's behoren ad-hoc gegevensverkenning waarbij de vorm van de gegevens onbekend is, evenals operationele analyse via geschematiseerde (expliciet gemodelleerde) gegevens ter ondersteuning van de operationele efficiëntie. Deze scenario's bestaan doorgaans naast elkaar en bieden ondersteuning voor verschillende soorten toepassingen. Platformfunctionaliteiten, zoals meerlaagse opslag (warm en koud), de mogelijkheid om decennia aan tijdreeksgegevens op te slaan en om expliciet query's te modelleren en optimaliseren voor operationele intelligentie op assetbasis, worden steeds belangrijker voor het succes van industriële IoT-ondernemingen en hun digitale revolutie.

Azure TSI is een uitgebreide oplossing voor end-to-end Platform-as-a-Service voor zowel IoT-gegevensverkenning als operationele inzichten. Time Series Insights biedt een volledig beheerde cloudservice voor het analyseren van IoT-tijdreeksgegevens.

Klanten kunnen onbewerkte gegevens opslaan in een geheugenarchief zonder schema en kunnen interactieve ad-hoc query's uitvoeren door middel van een gedistribueerde query-engine en API. Ze kunnen ook gebruikmaken van onze uitgebreide gebruikerservaring voor het visualiseren van miljarden gebeurtenissen in een paar seconden tijd. Meer informatie over onze [mogelijkheden voor gegevensverkenning](./time-series-insights-overview.md).

TSI biedt ook mogelijkheden voor operationele inzichten die momenteel in openbare preview zijn. Samen met interactieve gegevensverkenning en operationele intelligentie stelt Time Series Insights klanten in staat meer waarde af te leiden uit gegevens die verzameld zijn van IoT-apparaten. Meer in het bijzonder biedt Openbare preview de volgende hoofdfunctionaliteiten:

* Een schaalbare, voor prestaties en kosten geoptimaliseerd tijdreeksgegevensarchief dat een cloud-gebaseerde IoT-oplossing inschakelt om jaren aan tijdreeksgegevens in luttele seconden inzichtelijk te maken.
* Ondersteuning van semantische modellen voor het beschrijven van het domein en de metagegevens die gekoppeld zijn aan de afgeleide en niet-afgeleide signalen van assets en apparaten.
* Een verbeterde gebruikerservaring waarbij inzichten in gegevens op assetbasis worden gecombineerd met uitgebreide ad-hoc gegevensanalyses ter ondersteuning van zakelijke en operationele intelligentie
* Integratie met geavanceerde machine learning en analyseprogramma's, zoals Azure Databricks, Apache Spark, Azure Machine Learning, Jupyter-notebooks, Power BI, enzovoort, om klanten te helpen bij het verwerken van tijdreeksgegevens en de operationele efficiëntie te ondersteunen.

Operationele inzichten en gegevensverkenning worden samen geleverd met een eenvoudig prijsmodel op basis van betalen per gebruik voor gegevensverwerking, opslag en query's, waardoor klanten een veel beter schaalbaar model krijgen dat aansluit bij hun veranderende zakelijke behoeften.

Hieronder volgt een stroomdiagram van gegevens op hoog niveau dat de bijgewerkte functionaliteiten illustreert:

  ![Belangrijkste functionaliteiten][2]

Met de introductie van deze belangrijke IoT-functionaliteiten biedt Azure TSI de volgende belangrijke voordelen.

* Opslag met meerdere lagen voor tijdreeksgegevens op IoT-schaal

  * Met een algemene pijplijn voor gegevensverwerking voor het ophalen van gegevens hebben klanten de mogelijkheid gegevens op te slaan in een semi-dynamische opslag voor interactieve query's en/of in een koude opslag voor het opslaan van grote hoeveelheden gegevens en te profiteren van krachtige query's op assetbasis.

  * Dynamische routering voor opslaglagen komt binnenkort beschikbaar.

* Tijdreeksmodel voor het contextualiseren van onbewerkte telemetriegegevens en het afleiden van inzichten op assetbasis

  * Klanten kunnen onbewerkte telemetriegegevens contextualiseren met een beschrijvend tijdreeksmodel en uitgebreide operationele intelligentie afleiden met krachtige, geoptimaliseerde apparaat-query's op basis van prestaties en kosten.

* Naadloze integratie met andere data-oplossingen
  
  * Omdat gegevens in Azure Time Series Insights worden opgeslagen in open-source Apache Parquet-bestanden, kunnen klanten eenvoudig een integratie uitvoeren met andere data-oplossingen (eigen oplossingen of van derden) voor end-to-end scenario's met inbegrip van operationele intelligentie, geavanceerde machine learning, voorspellende analyse, enzovoort.

* Gegevens worden bijna in realtime verkend

  * De verkenner van de gebruikerservaring in Azure Time Series Insights biedt visualisatie voor alle gegevens die via de opnamepijplijn worden geleid. Kort nadat ze verbinding hebben gemaakt met een gebeurtenisbron, kunnen klanten gebeurtenisgegevens bekijken, verkennen en opvragen om te valideren of een apparaat op de verwachte manier gegevens uitzendt en om een IoT-asset te controleren op status, productiviteit en algehele effectiviteit.

* Hoofdoorzaakanalyse en anomaliedetectie

  * De verkenner van Azure Time Series Insights ondersteunt patronen en perspectiefweergaven voor het uitvoeren en opslaan van een hoofdoorzaakanalyse (root-cause analysis) die uit meerdere stappen bestaat. In combinatie met Azure Stream Analytics kunnen klanten Time Series Insights gebruiken om waarschuwingen en afwijkingen in realtime te detecteren.

* Aangepaste toepassingen bouwen op het Time Series Insights-platform

  * Azure Time Series Insights biedt ondersteuning voor JavaScript-SDK met uitgebreide besturingselementen en vereenvoudigde toegang tot query's zodat klanten aangepaste IoT-toepassingen kunnen maken op het Time Series Insights-platform die aansluiten bij de behoeften van individuele bedrijven.
  * Klanten kunnen query-API's van Time Series Insights ook rechtstreeks gebruiken om gegevens naar aangepaste IoT-toepassingen te sturen.

## <a name="next-steps"></a>Volgende stappen

U bent klaar om aan de slag te gaan met de Azure TSI (Preview):

> [!div class="nextstepaction"]
> [Lees de Quickstart-handleiding](./time-series-insights-update-quickstart.md)

<!-- Images -->
[1]: media/v2-update-overview/overview_one.png
[2]: media/v2-update-overview/overview_two.png