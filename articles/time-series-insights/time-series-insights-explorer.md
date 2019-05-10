---
title: Verken gegevens met behulp van de Azure Time Series Insights-Verkenner | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van Azure Time Series Insights explorer in uw webbrowser om snel een globale weergave van uw big data te bekijken en valideren van uw IoT-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 0f22a0245d002b94d9fc0004214c37944350e262
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412966"
---
# <a name="azure-time-series-insights-explorer"></a>Azure Time Series Insights explorer

In dit artikel beschrijft de functies en opties in het algemeen beschikbaar voor de Azure Time Series Insights [explorer web-app](https://insights.timeseries.azure.com/). De Verkenner van Time Series Insights ziet u de krachtige visualisatie gegevensmogelijkheden die worden geboden door de service en kan worden geopend in uw eigen omgeving.

Azure Time Series Insights is een volledig beheerde service voor analyse, opslag en visualisatie die het eenvoudig maakt om miljarden IoT-gebeurtenissen tegelijkertijd te verkennen en te analyseren. Dit biedt u een algemeen overzicht van uw gegevens, waarmee u snel uw IoT-oplossing te valideren en kostbare stilstand van essentiële apparaten te vermijden. U kunt verborgen trends, anomalieën te herkennen, te detecteren en oorzaak-gevolganalyses bijna in realtime uitvoeren. De Verkenner van Time Series Insights is momenteel in openbare preview.

> [!TIP]
> Lees voor een rondleiding door de demo-omgeving, de [Azure Time Series Insights Quickstart](time-series-quickstart.md).

## <a name="video"></a>Video

### <a name="learn-about-querying-data-using-the-time-series-insights-explorer-br"></a>Meer informatie over het opvragen van gegevens met behulp van de Verkenner van Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>Zie de bovenstaande video <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">"Aan de slag met TSI met behulp van een Azure IoT-oplossingsversnellers"</a>.

## <a name="prerequisites"></a>Vereisten

Voordat u de Verkenner van Time Series Insights gebruiken kunt, moet u het volgende doen:

- Maak een Time Series Insights-omgeving. Zie voor meer informatie, [aan de slag met Time Series Insights](./time-series-insights-get-started.md).
- [Toegang bieden](time-series-insights-data-access.md) aan uw account in de omgeving.
- Voeg een [IoT-Hub](time-series-insights-how-to-add-an-event-source-iothub.md) of [Event Hub](time-series-insights-how-to-add-an-event-source-eventhub.md) gebeurtenisbron toe.

## <a name="explore-and-query-data"></a>Verken en gegevens op te vragen

U kunt binnen enkele minuten verbinding te maken met uw gebeurtenisbron aan uw Time Series Insights-omgeving kunt verkennen en query uitvoeren op uw time series-gegevens.

1. Als u wilt gaan, opent u de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com/) in uw webbrowser en selecteer een omgeving aan de linkerkant van het venster. Alle omgevingen die u toegang tot hebt worden weergegeven in alfabetische volgorde.

1. Wanneer u een omgeving selecteert, gebruiken de **FROM** en **aan** configuraties aan de bovenkant of klik en sleep via uw gewenste periode.  Klik op het Vergrootglas in de rechterbovenhoek, of klik met de rechtermuisknop op de geselecteerde periode en selecteer **zoeken**.  

1. U kunt ook vernieuwen beschikbaarheid automatisch elke minuut door de **automatisch op** knop. De **automatisch op** knop is alleen van toepassing op de van beschikbaarheidsgrafiek, niet de inhoud van de belangrijkste visualisatie.

1. Let op het pictogram van de Azure-cloud, gaat u naar uw omgeving in Azure portal.

   [![Time Series Insights-omgeving](media/time-series-insights-explorer/explorer1.png)](media/time-series-insights-explorer/explorer1.png#lightbox)

1. Vervolgens ziet u een diagram met een telling van alle gebeurtenissen tijdens de geselecteerde periode.  U hebt hier een aantal besturingselementen:

    **Deelvenster Editor voorwaarden**:  De term-ruimte is waar u uw omgeving een query.  Aan de linkerkant van het scherm wordt vastgesteld:
      - **Meting**:  Dit alle numerieke kolommen vervolgkeuzelijst wordt weergegeven (**verdubbelt**)
      - **Splitsen op basis van**: Deze vervolgkeuzelijst bevat categorische kolommen (**tekenreeksen**)
      - U kunt inschakelen stapsgewijze interpolatie, minimale en maximale weergeven en aanpassen van de y-as van het Configuratiescherm volgende om door te meten.  Bovendien kunt u aanpassen of gegevens die worden weergegeven een aantal, gemiddelde of som van de gegevens is.
      - U kunt maximaal vijf voorwaarden om weer te geven op de dezelfde x-as toevoegen.  Gebruik de **kopiëren naar beneden** om een extra term toevoegen of klik op de **toevoegen** om toe te voegen een nieuwe term.

        [![Deelvenster termen-Editor](media/time-series-insights-explorer/explorer2.png)](media/time-series-insights-explorer/explorer2.png#lightbox)

      - **Predikaat**:  Het predicaat kunt u snel uw om gebeurtenissen te filteren met behulp van de set met operanden hieronder vermeld. Als u een zoekopdracht door selecteren/te klikken uitvoeren, het predicaat wordt automatisch bijwerken op basis van die zoekopdracht. Ondersteunde Operandtypen zijn onder andere:

         |Bewerking  |Ondersteunde typen  |Opmerkingen  |
         |---------|---------|---------|
         |`<`, `>`, `<=`, `>=`     |  Double, DateTime, TimeSpan       |         |
         |`=`, `!=`, `<>`     | String, Bool, Double, DateTime, TimeSpan NULL        |         |
         |IN     | String, Bool, Double, DateTime, TimeSpan NULL        |  Alle operands moeten zijn van hetzelfde type of NULL-constante.        |
         |HAS     | String        |  Alleen constante letterlijke tekenreeksen zijn toegestaan op de rechterkant. Lege tekenreeks en NULL zijn niet toegestaan.       |

      - **Voorbeelden van query 's**

         [![Voorbeelden van query 's](media/time-series-insights-explorer/explorer9.png)](media/time-series-insights-explorer/explorer9.png#lightbox)

1. De **intervalgrootte** schuifregelaar kunt u om in te zoomen en afmelden bij intervallen via de dezelfde periode.  Dit biedt meer controle over verkeer tussen grote segmenten van de tijd die smooth trends op segmenten zo klein is als de milliseconde nauwkeurig, zodat u kunt om te zien met hoge resolutie, gedetailleerde delen van uw gegevens weergeven. Startpunt van de schuifregelaar-standaard is ingesteld als de meest optimale weergave van de gegevens van uw selectie. Netwerktaakverdeling resolutie, de snelheid van de query en de granulatie.

1. De **tijd kwast** hulpprogramma kunt u eenvoudig navigeren vanuit een tijdsduur naar een andere, intuïtieve UX front en center voor naadloze verplaatsing tussen tijdsbereik.

1. De **opslaan** opdracht kunt u uw huidige query opslaan en delen met andere gebruikers van de omgeving in te schakelen. Met behulp van **Open**, ziet u alle opgeslagen query's en eventuele gedeelde query's van andere gebruikers in een omgeving die u toegang tot hebt.

   [![Query 's](media/time-series-insights-explorer/explorer3.png)](media/time-series-insights-explorer/explorer3.png#lightbox)

## <a name="visualize-data"></a>Gegevens visualiseren

1. De **perspectiefweergave** hulpprogramma biedt een gelijktijdige weergave van maximaal vier unieke query's. U vindt de knop perspectief weergeven in de rechterbovenhoek van de grafiek.  

   [![Perspectief weergeven](media/time-series-insights-explorer/explorer4.png)](media/time-series-insights-explorer/explorer4.png#lightbox)

1. De **grafiek** kunt u uw gegevens visueel verkennen. Grafiek-hulpprogramma's zijn onder andere:

    - **Selecteer/Klik**, waarmee een selectie van een bepaalde periode of van een enkele gegevensreeks.  
    - Binnen een periode span selectie, kunt u inzoomen of gebeurtenissen verkennen.  
    - Binnen een gegevensreeks kunt u de reeks splitsen op een andere kolom, de reeks toevoegen als een nieuwe term, alleen de geselecteerde reeks weergeven, uitsluiten van de geselecteerde reeks, reeks pingen of gebeurtenissen van de geselecteerde reeks verkennen.
    - In het gebied van het filter aan de linkerkant van de grafiek, kunt u alle weergegeven gegevens uit de serie bekijken en de volgorde wijzigen door de naam of waarde, alle gegevensreeks of alle vastgemaakte of losgemaakte reeksen weergeven.  U kunt ook één gegevensreeks selecteren en de reeks splitsen op een andere kolom, de reeks toevoegen als een nieuwe term, alleen de geselecteerde reeks weergeven, uitsluiten van de geselecteerde reeks, reeks vastmaken of gebeurtenissen van de geselecteerde reeks verkennen.
    - Als u meerdere voorwaarden tegelijkertijd weergeeft, kunt u stack, niet meer stapelen, Zie aanvullende gegevens over een reeks en gebruiken van de dezelfde y-as voor alle voorwaarden met de knoppen in de rechterbovenhoek van de grafiek.

    [![Grafiek-hulpprogramma](media/time-series-insights-explorer/explorer5.png)](media/time-series-insights-explorer/explorer5.png#lightbox)

1. De **heatmap** unieke of afwijkende gegevensreeks in een bepaalde query om snel kan worden gebruikt. Slechts één zoekterm kan worden gevisualiseerd als een heatmap.

    [![Heatmap](media/time-series-insights-explorer/explorer6.png)](media/time-series-insights-explorer/explorer6.png#lightbox)

1. **Gebeurtenissen**:  Als u ervoor kiest gebeurtenissen bij het selecteren van verkennen of met de rechtermuisknop op hierboven, het deelvenster gebeurtenissen beschikbaar wordt gesteld.  Hier ziet u alle van de ruwe gebeurtenissen en de gebeurtenissen exporteren als JSON- of CSV-bestanden. Time Series Insights alle onbewerkte gegevens worden opgeslagen.

    [![gebeurtenissen](media/time-series-insights-explorer/explorer7.png)](media/time-series-insights-explorer/explorer7.png#lightbox)

1. Klik op de **statistieken** tabblad nadat u gebeurtenissen als patronen en kolomstatistieken beschikbaar wilt maken.  

    - **Patronen**: deze functie proactief de meest statistisch significant patronen in een regio voor de geselecteerde gegevens. Dit neemt u niet hoeft te kijken naar duizenden gebeurtenissen om te begrijpen welke patronen meest garandeert u tijd en energie. Time Series Insights verder kunt u gaat u rechtstreeks naar deze statistisch significant patronen om door te gaan met het uitvoeren van een analyse. Deze functie is ook handig voor het onderzoeken van daarmee van historische gegevens.

    - **Kolomstatistieken**:  Kolomstatistieken bieden voor grafieken en tabellen die gegevens uit een kolom van de geselecteerde gegevensreeks in de geselecteerde periode opdelen.  

      [![STATS](media/time-series-insights-explorer/explorer8.png)](media/time-series-insights-explorer/explorer8.png#lightbox)

U hebt nu gezien de verschillende functies en opties die beschikbaar zijn binnen de Time Series Insights explorer web-app.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [vaststellen en oplossen van problemen](time-series-insights-diagnose-and-solve-problems.md) in uw Time Series Insights-omgeving.

- Uitvoeren van de begeleide [Azure Time Series Insights Quickstart](time-series-quickstart.md) tour.
