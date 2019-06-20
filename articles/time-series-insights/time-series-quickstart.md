---
title: 'Quickstart: Azure Time Series Insights-verkenner | Microsoft Docs'
description: In deze Quick Start laat zien hoe u aan de slag met de Azure Time Series Insights explorer in uw webbrowser grote hoeveelheden IoT-gegevens visualiseren. In dit artikel worden de belangrijkste functies van de verkenner van Azure Time Series Insights behandeld via een rondleiding in een demo-omgeving.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: 415ce28a7cab77c538a7dfb8f387900ff515dd0e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164550"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Snelstartgids: Azure Time Series Insights verkennen

Deze quickstart van Azure Time Series Insights explorer kunt u aan de slag met Time Series Insights in een gratis demo-omgeving. Via deze leert u hoe u uw webbrowser gebruiken voor het visualiseren van grote hoeveelheden IoT gegevens en rondleiding door de belangrijkste functies die nu algemeen beschikbaar.

Azure Time Series Insights is een volledig beheerde analyse-, opslag- en visualisatie-service die hoe vereenvoudigt te verkennen en analyseren van miljarden IoT-gebeurtenissen tegelijkertijd. Dit biedt u een algemeen overzicht van uw gegevens zodat u snel uw IoT-oplossing te valideren en kostbare stilstand van essentiële apparaten te vermijden. Azure Time Series Insights helpt u bij het detecteren van verborgen trends, anomalieën te herkennen en oorzaak-gevolganalyses bijna in realtime uitvoeren.

Voor meer flexibiliteit, kunt u Azure Time Series Insights toevoegen aan een bestaande toepassing via de krachtige [REST-API's](./time-series-insights-update-tsq.md) en [client-SDK](./tutorial-create-tsi-sample-spa.md). De API's kunt u opslaat, query, en gebruiken van time series-gegevens in een clienttoepassing van uw keuze. U kunt ook de client-SDK gebruiken UI-onderdelen toevoegen aan uw bestaande toepassing.

In deze snelstartgids van Time Series Insights explorer biedt een rondleiding van functies die nu algemeen beschikbaar.

## <a name="prepare-the-demo-environment"></a>De demo-omgeving voorbereiden

1. Maak een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) als u een al hebt gemaakt.

1. Ga in uw browser naar de [demo voor algemene beschikbaarheid](https://insights.timeseries.azure.com/demo).

1. Als u hierom wordt gevraagd, aanmelden bij de Verkenner van Time Series Insights met behulp van de referenties van uw Azure-account.

1. De Time Series Insights korte Tour langs de pagina wordt weergegeven. Selecteer **volgende** om te beginnen met de rondleiding.

   [![Klik op volgende](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox)

## <a name="explore-the-demo-environment"></a>De demo-omgeving verkennen

1. De **deelvenster tijd selecteren** wordt weergegeven. Gebruik dit venster om het tijdsbestek te selecteren dat u wilt visualiseren.

   [![Deelvenster tijd selecteren](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Selecteer een bepaalde periode en sleept u deze in de regio. Selecteer vervolgens **zoeken**.

   [![Een tijdsbestek selecteren](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   U ziet in Time Series Insights een grafiek met een visualisatie voor het opgegeven tijdsbestek. U kunt verschillende bewerkingen uitvoeren in het lijndiagram doen. U kunt bijvoorbeeld filteren, vastmaken, sorteren en stapelen.

   Om terug te keren naar de **deelvenster tijd selecteren**, selecteer de pijl-omlaag, zoals wordt weergegeven:

   [![Grafiek](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Selecteer **toevoegen** in de **deelvenster termen** om toe te voegen een nieuwe zoekterm.

   [![Item toevoegen](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. In de grafiek kunt u een regio selecteren, hier met de rechtermuisknop op klikken en vervolgens **Explore Events** selecteren.

   [![Gebeurtenissen verkennen](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   Een raster met de onbewerkte gegevens die worden weergegeven in de regio die u verkent.

   [![Rasterweergave](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Selecteren en filteren van gegevens

1. Bewerk uw voorwaarden om de waarden in de grafiek te wijzigen. Voeg een andere term om kruislings te correleren verschillende soorten waarden.

   [![Een term toevoegen](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. Typ een filterterm in de **reeks filteren** vak voor het filteren van geïmproviseerde reeks. Voor deze quickstart voeren we **Station5** in om de temperatuur- en drukwaarden voor dat station kruislings te correleren.

   [![Reeks filteren](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Als u klaar bent met de quickstart, kunt u experimenteren met de set voorbeeldgegevens om verschillende visualisaties te maken.

## <a name="next-steps"></a>Volgende stappen

Bent u klaar om uw eigen Time Series Insights-omgeving te maken:
> [!div class="nextstepaction"]
> [Uw Time Series Insights-omgeving plannen](time-series-insights-environment-planning.md)
