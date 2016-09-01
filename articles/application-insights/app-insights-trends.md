<properties
    pageTitle="Trends analyseren in Visual Studio | Microsoft Azure"
    description="In uw Application Insights-telemetrie in Visual Studio kunt u trends analyseren, visualiseren en verkennen."
    services="application-insights"
    documentationCenter=".net"
    authors="numberbycolors"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/14/2016"
    ms.author="daviste"/>

# Trends analyseren in Visual Studio

Met het hulpprogramma Trends van Application Insights kunt u visualiseren hoe belangrijke telemetriegebeurtenissen van uw toepassingen na verloop van tijd veranderen, zodat u snel problemen en afwijkingen kunt identificeren. Met koppelingen naar gedetailleerdere diagnostische gegevens kan Trends u helpen de prestaties van uw app te verbeteren, de oorzaken van uitzonderingen te achterhalen en inzichten op basis van aangepaste gebeurtenissen te bieden.

![Voorbeeld van Trends-venster](./media/app-insights-trends/app-insights-trends-hero-750.png)

> [AZURE.NOTE] Application Insights Trends is beschikbaar in Visual Studio 2015 Update 3 en hoger, of met de [extensie Developer Analytics Tools](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a) versie 5.209 en hoger.

## Application Insights Trends openen

U kunt het venster Application Insights Trends op een van de volgende manieren openen:

* Kies op de Application Insights-werkbalk de knop **Explore Telemetry Trends**.
* Kies **Application Insights > Explore Telemetry Trends** in het snelmenu van het project.
* Kies in de menubalk van Visual Studio **View > Other Windows > Application Insights Trends**.

Mogelijk wordt u gevraagd om een resource te selecteren. Klik in dat geval op **Select a resource**, meld u aan bij een Azure-abonnement en kies een Application Insights-resource waarvoor u telemetrietrends wilt analyseren.

## Een trendanalyse kiezen

![Menu met algemene typen trendanalysen](./media/app-insights-trends/app-insights-trends-1-750.png)

U gaat aan de slag door een van de vijf algemene trendanalysen te kiezen. Met elk daarvan worden gegevens van de afgelopen 24 uur geanalyseerd:

* **Onderzoek prestatieproblemen met uw serveraanvragen**: aanvragen naar uw service, gegroepeerd op reactietijden
* **Analyseer fouten in uw serveraanvragen**: aanvragen naar uw service, gegroepeerd op HTTP-responscode
* **Bestudeer de uitzonderingen in uw toepassing**: uitzonderingen van uw service, gegroepeerd op uitzonderingstype
* **Controleer de prestaties van de afhankelijkheden van uw toepassing**: services die door uw service worden aangeroepen, gegroepeerd op reactietijden
* **Inspecteer uw aangepaste gebeurtenissen**: aangepaste gebeurtenissen die u hebt ingesteld voor uw service, gegroepeerd op gebeurtenistype

Deze vooraf gemaakte analysen zijn later beschikbaar via de knop **View common types of telemetry analysis** in de linkerbovenhoek van het venster Trends.

## Trends in uw toepassing visualiseren

Application Insights Trends maakt een tijdreeksvisualisatie op basis van de telemetrie van uw toepassing. Elke gevisualiseerde tijdreeks toont één type telemetrie, gegroepeerd op één eigenschap van die telemetrie en gedurende een bepaalde periode.

Zo wilt u misschien serveraanvragen weergeven, gegroepeerd op het land van waaruit ze afkomstig zijn, in de afgelopen 24 uur. In dit voorbeeld stelt elke bel in de visualisatie het aantal serveraanvragen voor een bepaald land gedurende één uur voor.

Gebruik de besturingselementen boven in het venster om aan te passen welke typen telemetrie u wilt weergeven. Kies eerst de telemetrietypen waarin u bent geïnteresseerd:

* **Telemetry Type**: serveraanvragen, uitzonderingen, afhankelijkheden of aangepaste gebeurtenissen
* **Time Range**: een willekeurige periode van de afgelopen 30 minuten tot de laatste 3 dagen
* **Group By**: uitzonderingstype, probleem-id, land/regio en meer

Klik vervolgens op **Analyze Telemetry** om de query uit te voeren.

U kunt als volgt tussen de bellen in de visualisatie navigeren:

* Klik om een bel te selecteren, waarmee de filters onder in het venster worden bijgewerkt en de gebeurtenissen tijdens een bepaalde periode worden samengevat.
* Dubbelklik op een bel om naar de zoekfunctie te gaan en alle afzonderlijke telemetriegebeurtenissen te zien die in die periode zijn opgetreden.
* Druk op de toets **Ctrl** en klik vervolgens op een bel om de selectie ervan in de visualisatie op te heffen.

> [AZURE.TIP] Met de functies Trends en Search kunt u de telemetriegebeurtenissen aanwijzen die problemen met uw service veroorzaken. Als uw klanten bijvoorbeeld melden dat een app een bepaalde middag minder snel werkt, kunt u het proces starten om het probleem met Trends te analyseren. Analyseer de aanvragen die de afgelopen uren bij uw service zijn binnengekomen, gegroepeerd op reactietijd. Kijk of er een ongebruikelijk groot cluster van trage aanvragen is. Dubbelklik vervolgens op die bel om naar de functie Search te gaan, die op deze aanvraaggebeurtenissen is gefilterd. Vanuit de zoekfunctie kunt u de inhoud van deze aanvragen verkennen en de betreffende code zoeken, zodat u het probleem kunt oplossen.

## Op specifieke trends filteren

Ontdek meer specifieke trends met de filterbesturingselementen onder in het venster. Als u een filter wilt toepassen, klikt u op de naam ervan. U kunt snel tussen verschillende filters schakelen om trends te ontdekken die mogelijk zijn verborgen in een bepaalde dimensie van uw telemetrie. Als u een filter toepast in één dimensie, bijvoorbeeld de dimensie Exception Type, kunt u op filters in andere dimensies blijven klikken, ook al zijn ze lichter gekleurd. Klik nogmaals op een filter als u de toepassing ervan wilt opheffen. Druk op de toets **Ctrl** en klik vervolgens om meerdere filters in dezelfde dimensie te selecteren.

![Trendfilters](./media/app-insights-trends/TrendsFiltering-750.png)

Hoe past u meerdere filters toe?

1. Pas het eerste filter toe.
2. Klik op de knop **Apply selected filters and query again** voor de naam van de dimensie van het eerste filter. Hiermee wordt de telemetriequery opnieuw uitgevoerd, maar nu specifiek voor gebeurtenissen die voldoen aan het eerste filter.
3. Pas een tweede filter toe.
4. Herhaal dit proces om trends in specifieke subreeksen van de telemetrie te vinden. U kunt bijvoorbeeld serveraanvragen opvragen met de naam 'Home GET/Index' die afkomstig zijn uit Duitsland en statuscode 500 hebben gekregen.

Als u de toepassing van een van deze filters wilt opheffen, klikt u op de knop **Remove selected filters and query again** voor de dimensie.

![Meerdere filters](./media/app-insights-trends/TrendsFiltering2-750.png)

## Afwijkingen vinden

Het hulpprogramma Trends kan bellen markeren van gebeurtenissen die afwijken ten opzichte van de andere bellen in dezelfde tijdreeks. Kies in de vervolgkeuzelijst **View Type** de optie **Counts in time bucket (highlight anomalies)** of **Percentages in time bucket (highlight anomalies)**. Rode bellen zijn afwijkingen.

Afwijkingen worden gedefinieerd als bellen met aantallen/percentages die 2,1 keer zo groot zijn als de standaardafwijking van de aantallen/percentages die zijn opgetreden in de afgelopen twee perioden (bijvoorbeeld 48 uur als u de afgelopen 24 uur bekijkt).

![Gekleurde punten geven afwijkingen aan](./media/app-insights-trends/TrendsAnomalies-750.png)

> [AZURE.TIP] Het proces van het markeren van afwijkingen kan met name nuttig zijn bij het vinden van uitschieters in tijdreeksen van kleine bellen die anders misschien niet opvallen.  

## <a name="next"></a>Volgende stappen


[Werken met Application Insights in Visual Studio](app-insights-visual-studio.md): u kunt telemetrie zoeken, gegevens bekijken in CodeLens en Application Insights configureren, alles vanuit Visual Studio.

[Meer gegevens toevoegen](app-insights-asp-net-more.md): u kunt gebruik, beschikbaarheid, afhankelijkheden en uitzonderingen bewaken. Integreer bijgehouden informatie uit frameworks voor logboekregistratie. Schrijf aangepaste telemetrie.

[Werken met de Application Insights-portal](app-insights-dashboards.md): dashboards, krachtige hulpprogramma's voor diagnose en analyse, waarschuwingen, een live afhankelijkheidskaart van uw toepassing en exportmogelijkheden voor telemetrie.



<!--HONumber=ago16_HO4-->


