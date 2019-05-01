---
title: 'Quickstart: Azure Time Series Insights-verkenner | Microsoft Docs'
description: In deze quickstart wordt beschreven hoe u de verkenner van Azure Time Series Insights kunt gebruiken in uw webbrowser om grote hoeveelheden IoT-gegevens te visualiseren. In dit artikel worden de belangrijkste functies van de verkenner van Azure Time Series Insights behandeld via een rondleiding in een demo-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: be663520c9c1afd11ace57cd9dcb8ffb8219b86b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696962"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Snelstartgids: Azure Time Series Insights verkennen

Een explorer Quick Start wordt verstrekt aan de slag met Azure Time Series Insights in een gratis demo-omgeving. Via deze leert u hoe u uw webbrowser gebruiken voor het visualiseren van grote hoeveelheden IoT gegevens en rondleiding door de belangrijkste functies die momenteel in algemene beschikbaarheid.

Azure Time Series Insights is een volledig beheerde analyse-, opslag- en visualisatie-service die hoe vereenvoudigt te verkennen en analyseren van miljarden IoT-gebeurtenissen tegelijkertijd. Dit biedt u een algemeen overzicht van uw gegevens, helpt u snel uw IoT-oplossing te valideren en kostbare stilstand van essentiële apparaten te vermijden. U kunt via Azure Time Series Insights verborgen trends, anomalieën te herkennen, te detecteren en oorzaak-gevolganalyses bijna in realtime uitvoeren.

Voor meer flexibiliteit, Azure Time Series Insights kan worden toegevoegd aan een bestaande toepassing via de krachtige [REST-API's](./time-series-insights-update-tsq.md) en [client-SDK](./tutorial-create-tsi-sample-spa.md). De API's kunnen u om op te slaan, query time series-gegevens, en gebruiken van time series-gegevens in een clienttoepassing van uw keuze. U kunt ook de client-SDK gebruiken om u te UI-onderdelen toevoegen aan uw bestaande toepassing.

De Time Series Insights explorer een rondleiding van is functies die momenteel in het algemeen beschikbaar.

## <a name="prepare-the-demo-environment"></a>De demo-omgeving voorbereiden

1. Maak een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) als een nog niet is gemaakt.

1. Navigeer in uw browser naar de [demo voor algemene beschikbaarheid](https://insights.timeseries.azure.com/demo).

1. Als u hierom wordt gevraagd, moet u zich aanmelden bij de Time Series Insights-Verkenner met de referenties van uw Azure-account.

1. U ziet nu de eerste pagina van de korte rondleiding door Time Series Insights. Klik op **Next** om de rondleiding te starten.

   [![Klik op volgende](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox)

## <a name="explore-the-demo-environment"></a>De demo-omgeving verkennen

1. Het venster **Time selection** wordt weergegeven. Gebruik dit venster om het tijdsbestek te selecteren dat u wilt visualiseren.

   [![Deelvenster tijd selecteren](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Klik en sleep in de regio en klik vervolgens op de knop **Search**.

   [![Een tijdsbestek selecteren](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   U ziet in Time Series Insights een grafiek met een visualisatie voor het opgegeven tijdsbestek. U kunt verschillende bewerkingen uitvoeren in het lijndiagram, zoals filteren, vastmaken, sorteren en stapelen.

   Als u terug wilt naar het venster **Time selection**, klikt u op de pijl-omlaag, zoals wordt aangegeven:

   [![Grafiek](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Klik op **Add** in het venster **Terms** om een nieuwe zoekterm toe te voegen.

   [![Item toevoegen](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. In de grafiek kunt u een regio selecteren, hier met de rechtermuisknop op klikken en vervolgens **Explore Events** selecteren.

   [![Gebeurtenissen verkennen](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   U ziet een raster met de onbewerkte gegevens uit de regio die u bekijkt:

   [![Rasterweergave](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Selecteren en filteren van gegevens

1. Pas de termen aan om de waarden in de grafiek te wijzigen, en voeg een term toe om verschillende soorten waarden kruislings te correleren:

   [![Een term toevoegen](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. Typ een filterterm in de **reeks filteren...**  vak voor het filteren van geïmproviseerde reeks. Voor deze quickstart voeren we **Station5** in om de temperatuur- en drukwaarden voor dat station kruislings te correleren.

   [![Reeks filteren](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Als u klaar bent met de quickstart, kunt u experimenteren met de set voorbeeldgegevens om verschillende visualisaties te maken.

## <a name="next-steps"></a>Volgende stappen

U kunt nu uw eigen Time Series Insights-omgeving gaan maken:
> [!div class="nextstepaction"]
> [Uw Time Series Insights-omgeving plannen](time-series-insights-environment-planning.md)
