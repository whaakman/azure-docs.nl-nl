---
title: Gegevens verkennen met behulp van de Azure Time Series Insights-Verkenner | Microsoft Docs
description: In dit artikel wordt beschreven hoe u gebruik van Azure Time Series Insights explorer in uw webbrowser om snel een globale weergave van uw big data te bekijken en valideren van uw IoT-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: a7905213d67fd1cb97a8e50c938427b7ad775924
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165777"
---
# <a name="azure-time-series-insights-explorer"></a>Azure Time Series Insights explorer

In dit artikel beschrijft de functies en opties in het algemeen beschikbaar voor de Azure Time Series Insights [explorer web-app](https://insights.timeseries.azure.com/). De Verkenner van Time Series Insights ziet u de krachtige visualisatie gegevensmogelijkheden die worden geboden door de service en kan worden geopend in uw eigen omgeving.

Azure Time Series Insights is een volledig beheerde service voor analyse, opslag en visualisatie die het eenvoudig maakt om miljarden IoT-gebeurtenissen tegelijkertijd te verkennen en te analyseren. Dit biedt u een algemeen overzicht van uw gegevens, waarmee u snel uw IoT-oplossing te valideren en kostbare stilstand van essentiële apparaten te vermijden. U kunt verborgen trends, anomalieën te herkennen, te detecteren en oorzaak-gevolganalyses bijna in realtime uitvoeren. De Verkenner van Time Series Insights is momenteel in openbare preview.

> [!TIP]
> Lees voor een rondleiding door de demo-omgeving, de [Azure Time Series Insights-snelstartgids](time-series-quickstart.md).

## <a name="video"></a>Video

### <a name="learn-about-querying-data-by-using-the-time-series-insights-explorer-br"></a>Meer informatie over het opvragen van gegevens met behulp van de Verkenner van Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/SHFPZvrR71s]

>[!NOTE]
>Zie de bovenstaande video <a href="https://www.youtube.com/watch?v=6ehNf6AJkFo">"Aan de slag met Time Series Insights met behulp van een Azure IoT-oplossingsversnellers."</a>

## <a name="prerequisites"></a>Vereisten

Voordat u de Verkenner van Time Series Insights gebruiken kunt, moet u het volgende doen:

- Maak een Time Series Insights-omgeving. Zie voor meer informatie, [aan de slag met Time Series Insights](./time-series-insights-get-started.md).
- [Toegang bieden](time-series-insights-data-access.md) aan uw account in de omgeving.
- Voeg een [IoT-hub](time-series-insights-how-to-add-an-event-source-iothub.md) of [gebeurtenishub](time-series-insights-how-to-add-an-event-source-eventhub.md) gebeurtenisbron toe.

## <a name="explore-and-query-data"></a>Verken en gegevens op te vragen

U kunt binnen enkele minuten verbinding te maken met uw gebeurtenisbron aan uw Time Series Insights-omgeving kunt verkennen en query uitvoeren op uw time series-gegevens.

1. Als u wilt gaan, opent u de [Verkenner van Time Series Insights](https://insights.timeseries.azure.com/) in uw webbrowser. Selecteer een omgeving aan de linkerkant van het venster. Alle omgevingen die u toegang tot hebt worden weergegeven in alfabetische volgorde.

1. Nadat u een omgeving hebt geselecteerd, gebruiken de **van** en **naar** configuraties aan de bovenkant of klik en sleep gedurende de periode die u wilt. Selecteer het Vergrootglas in de rechterbovenhoek, of met de rechtermuisknop op de geselecteerde periode en selecteer **zoeken**.

1. Ook kunt u vernieuwen beschikbaarheid automatisch elke minuut door de **automatisch op** knop. De **automatisch op** knop is alleen van toepassing op de van beschikbaarheidsgrafiek, niet de inhoud van de belangrijkste visualisatie.

1. Het pictogram van de Azure-cloud gaat u naar uw omgeving in Azure portal.

   [![Time Series Insights-omgeving](media/time-series-insights-explorer/explorer1.png)](media/time-series-insights-explorer/explorer1.png#lightbox)

1. Vervolgens ziet u een diagram met een telling van alle gebeurtenissen tijdens de geselecteerde periode. U hebt hier een aantal besturingselementen:

    - **Deelvenster van Editor voorwaarden**: De term-ruimte is waar u uw omgeving een query. Aan de linkerkant van het scherm wordt vastgesteld:
      - **Meting**: Deze vervolgkeuzelijst bevat alle numerieke kolommen (**verdubbelt**).
      - **Splitsen op basis van**: Deze vervolgkeuzelijst bevat categorische kolommen (**tekenreeksen**).
      - U kunt inschakelen stapsgewijze interpolatie, minimale en maximale weergeven en naast de y-as van het Configuratiescherm aanpassen **meting**. U kunt aanpassen of gegevens die worden weergegeven een aantal, gemiddelde of som van de gegevens is.
      - U kunt maximaal vijf voorwaarden om weer te geven op de dezelfde x-as toevoegen. Gebruik de **kopiëren naar beneden** klikken om een extra term toevoegen, of selecteer **toevoegen** om toe te voegen een nieuwe term.

        [![Deelvenster termen-Editor](media/time-series-insights-explorer/explorer2.png)](media/time-series-insights-explorer/explorer2.png#lightbox)

      - **Predikaat**: Gebruik het predicaat snel uw om gebeurtenissen te filteren met behulp van de set met operanden die worden vermeld in de volgende tabel. Als u een zoekopdracht uitvoeren door te selecteren of klikken, het predicaat automatisch updates op basis van die zoekopdracht. Ondersteunde Operandtypen zijn onder andere:

         |Bewerking  |Ondersteunde typen  |Opmerkingen  |
         |---------|---------|---------|
         |`<`, `>`, `<=`, `>=`     |  Double, DateTime, TimeSpan       |         |
         |`=`, `!=`, `<>`     | String, Bool, Double, DateTime, TimeSpan NULL        |         |
         |IN     | String, Bool, Double, DateTime, TimeSpan NULL        |  Alle operands moeten zijn van hetzelfde type of NULL-constante.        |
         |HAS     | String        |  Alleen constante letterlijke tekenreeksen zijn toegestaan op aan de rechterkant. Lege tekenreeks en NULL zijn niet toegestaan.       |

      - **Voorbeelden van query 's**

         [![Voorbeelden van query 's](media/time-series-insights-explorer/explorer9.png)](media/time-series-insights-explorer/explorer9.png#lightbox)

1. U kunt de **intervalgrootte** schuifregelaar om in te zoomen en afmelden bij intervallen via de dezelfde periode. De schuifregelaar biedt meer controle over verkeer tussen grote segmenten van de tijd waarin smooth trends op segmenten zo klein is als de milliseconde nauwkeurig, waarmee u kunt om te zien met hoge resolutie, gedetailleerde delen van uw gegevens. Startpunt van de schuifregelaar-standaard is ingesteld als de meest optimale weergave van de gegevens van uw selectie op resolutie, de snelheid van de query en de granulatie in balans brengen.

1. De **tijd kwast** hulpprogramma kunt u eenvoudig navigeren vanuit een timespan naar een andere.

1. Gebruik de **opslaan** opdracht voor het opslaan van uw huidige query en deze delen met andere gebruikers van de omgeving. Bij het gebruik **Open**, ziet u alle opgeslagen query's en eventuele gedeelde query's van andere gebruikers in een omgeving die u toegang tot hebt.

   [![Query 's](media/time-series-insights-explorer/explorer3.png)](media/time-series-insights-explorer/explorer3.png#lightbox)

## <a name="visualize-data"></a>Gegevens visualiseren

1. Gebruik de **perspectiefweergave** hulpprogramma voor een gelijktijdige weergave van maximaal vier unieke query's. De **perspectiefweergave** knop is in de rechterbovenhoek van de grafiek.

   [![Perspectief weergeven](media/time-series-insights-explorer/explorer4.png)](media/time-series-insights-explorer/explorer4.png#lightbox)

1. Weergeven in een grafiek visueel verkennen van uw gegevens en gebruik de **grafiek** hulpprogramma's:

    - **Selecteer** of **klikt u op** een bepaalde periode of een enkele gegevensreeks.
    - U kunt binnen een selectie timespan zoomen of gebeurtenissen verkennen.
    - Binnen een gegevensreeks kunt u de reeks splitsen op een andere kolom, de reeks toevoegen als een nieuwe term, alleen de geselecteerde reeks weergeven, uitsluiten van de geselecteerde reeks, reeks pingen of gebeurtenissen van de geselecteerde reeks verkennen.
    - U kunt in het gebied van het filter aan de linkerkant van de grafiek, zien alle weergegeven gegevensreeksen en de volgorde wijzigen door de naam of waarde. Ook kunt u alle gegevens of een reeks vastgemaakte of losgemaakte weergeven. U kunt één gegevensreeks selecteren en de reeks splitsen op een andere kolom, de reeks toevoegen als een nieuwe term, alleen de geselecteerde reeks weergeven, uitsluiten van de geselecteerde reeks, reeks vastmaken of gebeurtenissen van de geselecteerde reeks verkennen.
    - Wanneer u tegelijkertijd meerdere gebruiksrechtovereenkomsten bekijkt, kunt u stack, niet meer stapelen, Zie aanvullende gegevens over een reeks en de dezelfde y-as aan alle voorwaarden gebruiken. Gebruik de knoppen in de rechterbovenhoek van de grafiek.

    [![Grafiek-hulpprogramma](media/time-series-insights-explorer/explorer5.png)](media/time-series-insights-explorer/explorer5.png#lightbox)

1. Gebruik de **heatmap** om snel unieke of afwijkende gegevensreeks in een bepaalde query. Slechts één zoekterm kan worden gevisualiseerd als een heatmap.

    [![Heatmap](media/time-series-insights-explorer/explorer6.png)](media/time-series-insights-explorer/explorer6.png#lightbox)

1. Wanneer u gebeurtenissen onderzoeken door te selecteren of met de rechtermuisknop op de **gebeurtenissen** deelvenster beschikbaar wordt gesteld. Hier ziet u alle van de ruwe gebeurtenissen en de gebeurtenissen exporteren als JSON- of CSV-bestanden. Time Series Insights alle onbewerkte gegevens worden opgeslagen.

    [![gebeurtenissen](media/time-series-insights-explorer/explorer7.png)](media/time-series-insights-explorer/explorer7.png#lightbox)

1. Selecteer de **statistieken** tabblad nadat u gebeurtenissen als beschikbaar patronen en kolomstatistieken wilt onderzoeken.

    - **Patronen**: Deze functie proactief de meest statistisch significant patronen in een regio voor de geselecteerde gegevens. U hebt geen om te kijken naar duizenden gebeurtenissen om te begrijpen welke patronen vereisen dat de meeste tijd en energie. U kunt gaan met Time Series Insights, rechtstreeks in deze statistisch significant patronen om door te gaan met het uitvoeren van een analyse. Deze functie is ook handig voor het onderzoeken van daarmee van historische gegevens.
    - **Kolomstatistieken**: Kolomstatistieken bieden grafieken en tabellen die gegevens uit een kolom van de geselecteerde gegevensreeks in de geselecteerde periode opdelen.

      [![STATS](media/time-series-insights-explorer/explorer8.png)](media/time-series-insights-explorer/explorer8.png#lightbox)

U hebt nu gezien de verschillende functies en opties die beschikbaar binnen de Time Series Insights explorer web-app zijn.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [problemen vaststellen en oplossen](time-series-insights-diagnose-and-solve-problems.md) in uw Time Series Insights-omgeving.
- Uitvoeren van de begeleide [Azure Time Series Insights-snelstartgids](time-series-quickstart.md) tour.
