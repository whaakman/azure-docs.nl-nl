---
title: Met Azure Application Insights werken in Visual Studio | Microsoft Docs
description: Analyse van web-app-prestaties en diagnostische gegevens tijdens foutopsporing en algemeen gebruik.
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/17/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 7fe1b572104416fec62261c60228fa966d197dd9
ms.lasthandoff: 03/21/2017


---
# <a name="working-with-azure-application-insights-in-visual-studio"></a>Met Azure Application Insights werken in Visual Studio
In Visual Studio (2015 en hoger) kunt u de prestaties analyseren en problemen in uw ASP.NET web-app identificeren tijdens de foutopsporing en algemeen gebruik. Dit gebeurt aan de hand van telemetrie uit [Azure Application Insights](app-insights-overview.md).

Als u uw ASP.NET-web-app hebt gemaakt met Visual Studio 2017 of hoger, beschikt deze al over de Application Insights-SDK. Anders [voegt u Application Insights aan uw app toe](app-insights-asp-net.md) als u dit nog niet hebt gedaan.

Als uw app wilt bewaken wanneer deze in live productie is, bekijkt u normaal gesproken de Application Insights-telemetrie in de [Azure-portal](https://portal.azure.com), waar u waarschuwingen kunt instellen en krachtige bewakingsprogramma's kunt toepassen. Voor foutopsporing kunt u echter ook de telemetrie in Visual Studio zoeken en analyseren. U kunt Visual Studio gebruiken om telemetrie te analyseren die afkomstig is van de productiesite, maar ook uit foutopsporingsruns op uw ontwikkelmachine. In het laatste geval kunt u foutopsporingsruns al analyseren nog voordat u de SDK hebt geconfigureerd om telemetrie naar de Azure-portal te verzenden. 

## <a name="run"></a> Fouten opsporen in uw project
Voer uw web-app in de lokale foutopsporingsmodus door op F5 te drukken. Open verschillende pagina's om telemetrie te genereren.

In Visual Studio ziet u een aantal gebeurtenissen die door de Application Insights-module in het project zijn geregistreerd.

![Tijdens het opsporen van fouten wordt in Visual Studio de knop Application Insights weergegeven.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Klik op deze knop om uw telemetrie te doorzoeken. 

## <a name="application-insights-search"></a>Application Insights-zoekopdracht
In het zoekvenster van Application Insights worden geregistreerde gebeurtenissen weergegeven. (Als u zich bij het instellen van Application Insights hebt aangemeld bij Azure, kunt u dezelfde gebeurtenissen ook in de Azure-portal doorzoeken.)

![Klik met de rechtermuisknop op het project en kies Application Insights > Zoeken.](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> Nadat u filters hebt in- of uitgeschakeld, klikt u op de knop Zoeken aan het einde van het veld Tekst zoeken.
>

U kunt zoeken met vrije tekst gebruiken voor alle velden in de gebeurtenissen. Zoek bijvoorbeeld naar een deel van de URl van een pagina, naar de waarde van een eigenschap, zoals de clientlocatie, of naar specifieke woorden in een traceringslogboek.

Klik op een gebeurtenis om de gedetailleerde eigenschappen ervan weer te geven.

Voor aanvragen voor uw web-app, kunt u verder klikken naar de code.

![Klik onder Aanvraagdetails door naar de code](./media/app-insights-visual-studio/31.png)

U kunt ook de Verwante items openen om mislukte aanvragen of uitzonderingen te diagnosticeren.

![Blader onder Aanvraagdetails naar beneden, naar gerelateerde items](./media/app-insights-visual-studio/41.png)

## <a name="exceptions-and-failed-requests"></a>Uitzonderingen en mislukte aanvragen
Uitzonderingsrapporten worden weergegeven in het venster Zoeken. (In enkele oudere typen ASP.NET-toepassingen moet u [uitzonderingencontrole instellen](app-insights-asp-net-exceptions.md) om te zien welke uitzonderingen door het framework worden afgehandeld.)

Klik op een uitzondering voor een stack-trace. Als de code van de app in Visual Studio is geopend, kunt u via de stack-trace doorklikken naar de relevante coderegel.

![Uitzondering voor stack-trace](./media/app-insights-visual-studio/17.png)

## <a name="request-and-exception-summaries-in-the-code"></a>Samenvattingen van aanvragen en uitzonderingen in de code
In de Code Lens-regel boven elke handlermethode ziet u het aantal aanvragen en uitzonderingen dat door Application Insights de afgelopen 24 uur is geregistreerd.

![Uitzondering voor stack-trace](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> Code Lens toont alleen Application Insights-gegevens als u [uw app hebt geconfigureerd om telemetrie te verzenden naar de Application Insights-portal](app-insights-asp-net.md).
>

[Meer informatie over Application Insights in Code Lens](app-insights-visual-studio-codelens.md)

## <a name="trends"></a>Trends
Trends is een hulpprogramma waarmee u de werking van uw app gedurende een bepaalde periode kunt visualiseren. 

Kies **Telemetrietrends verkennen** op de Application Insights-werkbalkknop of in het Application Insights-zoekvenster. Kies een van de vijf algemene query's om te beginnen. U kunt verschillende gegevenssets analyseren op basis van telemetrietypen, tijdsbereik en andere eigenschappen. 

Als u wilt zoeken naar afwijkingen in uw gegevens, kiest u een van de afwijkingsopties onder de vervolgkeuzelijst Type weergave. Met de filteropties aan de onderkant van het venster kunt u eenvoudig specifieke subreeksen van uw telemetrie selecteren.

![Trends](./media/app-insights-visual-studio/51.png)

[Meer informatie over Trends](app-insights-visual-studio-trends.md).

## <a name="local-monitoring"></a>Lokale bewaking
(Vanaf Visual Studio 2015 Update 2) Als u de SDK nog niet hebt geconfigureerd voor het verzenden van telemetrie naar de Application Insights-portal (zodat er in ApplicationInsights.config geen instrumentatiesleutel staat), wordt in het venster met diagnostische gegevens telemetrie weergegeven van de meest recente foutopsporingssessie. 

Dit is handig als u al een eerdere versie van uw app hebt gepubliceerd. Zo voorkomt u dat de telemetrie van uw foutopsporingssessies in de Application Insights-portal wordt verward met de telemetrie over de gepubliceerde app.

Ook als u beschikt over [aangepaste telemetrie](app-insights-api-custom-events-metrics.md) waarin u fouten wilt opsporen voordat u deze naar de portal verzendt, komt deze functie van pas.

* *Aanvankelijk heb ik Application Insights volledig geconfigureerd om telemetrie naar de portal te verzenden. Maar nu wil ik de telemetrie alleen bekijken in Visual Studio.*
  
  * De instellingen van het venster Zoeken bevat een optie om lokale diagnostische gegevens te doorzoeken, zelfs als uw app telemetrie verzendt naar de portal.
  * Als u geen telemetrie meer naar de portal wilt verzenden, moet u de regel `<instrumentationkey>...` in ApplicationInsights.config uitcommentariëren. Als u weer telemetrie naar de portal wilt verzenden, verwijdert u het commentaarteken.


## <a name="whats-next"></a>Volgende stappen
|  |  |
| --- | --- |
| **[Meer gegevens toevoegen](app-insights-asp-net-more.md)**<br/>Bewaak het gebruik, de beschikbaarheid, de afhankelijkheden en de uitzonderingen. Integreer bijgehouden informatie uit frameworks voor logboekregistratie. Schrijf aangepaste telemetrie. |![Visual Studio](./media/app-insights-visual-studio/64.png) |
| **[Werken met de Application Insights-portal](app-insights-dashboards.md)**<br/>Dashboards, krachtige hulpprogramma's voor diagnose en analyse, waarschuwingen, een live afhankelijkheidskaart van uw toepassing en exportmogelijkheden voor telemetrie. |![Visual Studio](./media/app-insights-visual-studio/62.png) |


