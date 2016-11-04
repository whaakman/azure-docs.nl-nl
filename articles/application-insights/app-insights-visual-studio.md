---
title: Met Application Insights werken in Visual Studio
description: Analyse van prestaties en diagnostische gegevens tijdens foutopsporing en algemeen gebruik.
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: douge

ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2016
ms.author: awills

---
# Met Application Insights werken in Visual Studio
In Visual Studio (2015 en hoger) kunt u de prestaties analyseren en problemen identificeren tijdens de foutopsporing en algemeen gebruik. Dit gebeurt aan de hand van telemetrie uit [Visual Studio Application Insights](app-insights-overview.md).

Als u [Application Insights nog niet hebt geïnstalleerd in uw app](app-insights-asp-net.md), doet u dat nu.

## <a name="run"></a> Fouten opsporen voor uw project
Start uw toepassing met F5 en probeer deze uit. Open verschillende pagina’s om telemetrie te genereren.

In Visual Studio ziet u het aantal gebeurtenissen dat is geregistreerd.

![Tijdens het opsporen van fouten wordt in Visual Studio de knop Application Insights weergegeven.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Klik op deze knop om diagnostische gegevens te doorzoeken. 

## Diagnostische gegevens doorzoeken
In het zoekvenster worden gebeurtenissen weergegeven die zijn geregistreerd. (Als u zich bij het instellen van Application Insights hebt aangemeld bij Azure, kunt u dezelfde gebeurtenissen ook in de portal doorzoeken.)

![Klik met de rechtermuisknop op het project en kies Application Insights > Zoeken.](./media/app-insights-visual-studio/34.png)

U kunt zoeken met vrije tekst gebruiken voor alle velden in de gebeurtenissen. Zoek bijvoorbeeld naar een deel van de URl van een pagina, naar de waarde van een eigenschap, zoals de clientlocatie, of naar specifieke woorden in een traceringslogboek.

Klik op een gebeurtenis om de gedetailleerde eigenschappen ervan weer te geven.

U kunt ook het tabblad Verwante items openen om mislukte aanvragen of uitzonderingen te diagnosticeren.

![](./media/app-insights-visual-studio/41.png)

## Diagnostics Hub
In de Diagnostics Hub (Visual Studio 2015 of hoger) ziet u de Application Insights-servertelemetrie op het moment dat deze wordt gegenereerd. Dit werkt zelfs als u ervoor hebt gekozen om alleen de SDK te installeren, zonder deze te koppelen aan een resource in de Azure Portal.

![Open het venster Diagnostische hulpprogramma's en bekijk de Application Insights-gebeurtenissen.](./media/app-insights-visual-studio/31.png)

## Uitzonderingen
Als u [uitzonderingencontrole hebt ingesteld](app-insights-asp-net-exceptions.md), worden de uitzonderingenrapporten weergegeven in het venster Zoeken. 

Klik op een uitzondering voor een stack-trace. Als de code van de app in Visual Studio is geopend, kunt u via de stack-trace doorklikken naar de relevante coderegel.

![Uitzondering voor stack-trace](./media/app-insights-visual-studio/17.png)

Bovendien ziet u, in de Code Lens-regel boven elke methode, het aantal uitzonderingen dat door Application Insights in de afgelopen 24 uur is geregistreerd.

![Uitzondering voor stack-trace](./media/app-insights-visual-studio/21.png)

## Lokale bewaking
(Vanaf Visual Studio 2015 Update 2) Als u de SDK nog niet hebt geconfigureerd voor het verzenden van telemetrie naar de Application Insights-portal (zodat er in ApplicationInsights.config geen instrumentatiesleutel staat), wordt in het venster met diagnostische gegevens telemetrie weergegeven van de meest recente foutopsporingssessie. 

Dit is handig als u al een eerdere versie van uw app hebt gepubliceerd. Zo voorkomt u dat de telemetrie van uw foutopsporingssessies in de Application Insights-portal wordt verward met de telemetrie over de gepubliceerde app.

Ook als u beschikt over [aangepaste telemetrie](app-insights-api-custom-events-metrics.md) waarin u fouten wilt opsporen voordat u deze naar de portal verzendt, komt deze functie van pas.

* *Aanvankelijk heb ik Application Insights volledig geconfigureerd voor het verzenden van telemetrie naar de portal. Maar nu wil ik de telemetrie alleen bekijken in Visual Studio.*
  
  * De instellingen van het venster Zoeken bevat een optie om lokale diagnostische gegevens te doorzoeken, zelfs als uw app telemetrie verzendt naar de portal.
  * Als u geen telemetrie meer naar de portal wilt verzenden, moet u de regel `<instrumentationkey>...` in ApplicationInsights.config uitcommentariëren. Als u weer telemetrie naar de portal wilt verzenden, verwijdert u het commentaarteken.

## Trends
Trends is een hulpprogramma waarmee u de werking van uw app gedurende een bepaalde periode kunt visualiseren. 

Kies **Telemetrietrends verkennen** op de Application Insights-werkbalkknop of in het Application Insights-zoekvenster. Kies een van de vijf algemene query's om te beginnen. U kunt verschillende gegevenssets analyseren op basis van telemetrietypen, tijdsbereik en andere eigenschappen. 

Als u wilt zoeken naar afwijkingen in uw gegevens, kiest u een van de afwijkingsopties onder de vervolgkeuzelijst Type weergave. Met de filteropties aan de onderkant van het venster kunt u eenvoudig specifieke subreeksen van uw telemetrie selecteren.

![Trends](./media/app-insights-visual-studio/51.png)

[Meer informatie over Trends](app-insights-visual-studio-trends.md).

## Volgende stappen
|  |  |
| --- | --- |
| **[Meer gegevens toevoegen](app-insights-asp-net-more.md)**<br/>Bewaak het gebruik, de beschikbaarheid, de afhankelijkheden en de uitzonderingen. Integreer bijgehouden informatie uit frameworks voor logboekregistratie. Schrijf aangepaste telemetrie. |![Visual Studio](./media/app-insights-asp-net/64.png) |
| **[Werken met de Application Insights-portal](app-insights-dashboards.md)**<br/>Dashboards, krachtige hulpprogramma's voor diagnose en analyse, waarschuwingen, een live afhankelijkheidskaart van uw toepassing en exportmogelijkheden voor telemetrie. |![Visual Studio](./media/app-insights-asp-net/62.png) |

<!--HONumber=Sep16_HO3-->


