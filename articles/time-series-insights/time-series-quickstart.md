---
title: Quick Start - inzicht van Azure Time Series explorer | Microsoft Docs
description: Deze snelstartgids wordt beschreven hoe u aan de slag met Azure Time Series Insights explorer in uw webbrowser voor het visualiseren van grote hoeveelheden gegevens van IoT. Rondleiding door de belangrijkste functies in een demo-omgeving.
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: v-mamcge
manager: jhubbard
editor: MarkMcGeeAtAquent, jasonwhowell, kfile, MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 11/15/2017
ms.openlocfilehash: b1f2881da21849c3ac09b008640fc9f72dc158dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Snelstartgids: Azure Time Series Insights verkennen
Deze snelstartgids wordt beschreven hoe u aan de slag met Azure Time Series Insights explorer in een omgeving met vrije demonstratie. U leert hoe u webbrowser grote volumes met IoT-gegevens visualiseren en rondleiding door de belangrijkste functies van de Time Series Insights explorer. 

Azure Time Series Insights is een volledig beheerde service voor analyse, opslag en visualisatie die het eenvoudig maakt om miljarden IoT-gebeurtenissen tegelijkertijd te verkennen en te analyseren. Dit biedt u een globale weergave van uw gegevens, zodat u snel uw IoT-oplossing te valideren en dure uitvaltijd voor essentiële apparaten voorkomen door ondersteuning bij het detecteren van verborgen trends herkennen afwijkingen en analyses van de hoofdoorzaak in bijna realtime uitvoeren.  Als u een toepassing die u reeksgegevens store of een query moet maakt, kunt u met de Time Series Insights REST API's kunt ontwikkelen.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Time Series Insights explorer in een omgeving met demo verkennen

1. Navigeer in uw browser naar [https://insights.timeseries.azure.com/demo](https://insights.timeseries.azure.com/demo). 

2. Als u wordt gevraagd, moet u zich aanmelden bij de Time Series Insights explorer met de referenties van uw Azure-account. 
 
3. De Time Series Insights rondleiding pagina wordt weergegeven. Klik op **volgende** om te beginnen met de rondleiding.

   ![Klik op volgende](media/quickstart/quickstart1.png)

4. De **tijd selectie Configuratiescherm** wordt weergegeven. Met dit deelvenster kunt u een tijdsbestek visualiseren.

   ![Tijd selectie Configuratiescherm](media/quickstart/quickstart2.png)

5. Klik en sleep in de regio en klik vervolgens op de **Search** knop.
 
   ![Een tijdsperiode selecteren](media/quickstart/quickstart3.png) 

   Time Series Insights geeft een grafiek visualisatie voor het opgegeven tijdsbestek weer. U kunt verschillende acties in de grafiek regel, zoals filteren, vastmaken, sorteer- en gestapeld doen. 

   Terugkeren naar de **tijd selectie Configuratiescherm**, klikt u op de pijl-omlaag, zoals wordt weergegeven:

   ![Grafiek](media/quickstart/quickstart4.png)

6. Klik op **toevoegen** in de **voorwaarden Configuratiescherm** toevoegen van een nieuwe zoekterm.

   ![Item toevoegen](media/quickstart/quickstart5.png)

7. In de grafiek kan een regio, met de rechtermuisknop op de regio, en selecteer **verkennen gebeurtenissen**.
 
   ![Gebeurtenissen verkennen](media/quickstart/quickstart6.png)

   Een raster met de onbewerkte gegevens wordt van de regio die u bladert weergegeven:

   ![Rasterweergave](media/quickstart/quickstart7.png)

8. Uw voorwaarden om te wijzigen van de waarden in de grafiek bewerken en een andere term om cross-correleer verschillende soorten waarden toevoegen:

   ![Een term toevoegen](media/quickstart/quickstart8.png)

9. Een filter term in de **reeks filteren... ** vak voor het filteren van ad-hoc reeks. Voer voor de snelstartgids **Station5** voor cross-correleer temperatuur en druk voor dat station.
 
   ![Reeks filteren](media/quickstart/quickstart9.png)

Nadat u de Quick Start hebt voltooid, kunt u experimenteren met de voorbeeldgegevens te maken van verschillende visualisaties. 

### <a name="next-steps"></a>Volgende stappen
U bent klaar om uw eigen omgeving Time Series inzichten te maken:
> [!div class="nextstepaction"]
> [Uw omgeving Time Series Insights plannen](time-series-insights-environment-planning.md)
