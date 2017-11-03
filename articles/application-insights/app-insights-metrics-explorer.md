---
title: Verkennen van metrische gegevens in Azure Application Insights | Microsoft Docs
description: Grafieken op metrische explorer interpreteren en metrische explorer blades aanpassen.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: mbullwin
ms.openlocfilehash: 01b45323b74b54da157f4e9f1af783759c121be1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="exploring-metrics-in-application-insights"></a>Verkennen van metrische gegevens in Application Insights
Metrische gegevens in [Application Insights] [ start] zijn gemeten waarden en het aantal gebeurtenissen die worden verzonden in de telemetrie van uw toepassing. Ze helpen u bij het detecteren van prestatieproblemen en bekijk hoe trends in hoe uw toepassing wordt gebruikt. Er is een breed scala aan standard metrische gegevens en u kunt ook uw eigen aangepaste metrische gegevens en gebeurtenissen maken.

Aantal metrische gegevens en gebeurtenissen worden weergegeven in de grafieken van de geaggregeerde waarden zoals optellen, gemiddelden of aantallen.

Hier volgt een voorbeeld reeks grafieken:

![](./media/app-insights-metrics-explorer/01-overview.png)

U vindt metrische gegevens grafieken overal in de Application Insights-portal. In de meeste gevallen kan worden aangepast en u kunt meer grafieken toevoegen aan de blade. Op de blade overzicht doorklikken voor meer gedetailleerde grafieken (die titels zoals 'Servers') of klik op **Metrics Explorer** om een nieuwe blade waarin u aangepaste grafieken kunt maken te openen.

## <a name="time-range"></a>Tijdsbereik
U kunt het tijdbereik wordt gedekt door de grafieken of rasters op een blade wijzigen.

![Open de overzichtsblade van uw toepassing in de Azure portal](./media/app-insights-metrics-explorer/03-range.png)

Als u binnen enkele gegevens die nog niet zijn geregistreerd verwacht, klikt u op vernieuwen. Grafieken vernieuwd met een interval, maar de intervallen zijn langer van grotere tijdsbereik. Het kan even duren voordat gegevens afkomstig zijn via de pipeline voor analyse naar een diagram.

Als u wilt inzoomen deel uitmaakt van een grafiek, sleept u eroverheen:

![Sleep over een deel van een diagram.](./media/app-insights-metrics-explorer/12-drag.png)

Klik op de knop Zoomen ongedaan maken om te herstellen.

## <a name="granularity-and-point-values"></a>Samenvattingen en punt waarden
Beweeg de muis over de grafiek om de waarden van de metrische gegevens op dat moment weer te geven.

![Beweeg de muisaanwijzer over een grafiek](./media/app-insights-metrics-explorer/02-focus.png)

De waarde van de metrische gegevens op een bepaald tijdstip wordt tijdens de vorige steekproefinterval geaggregeerd.

De controle-interval of 'granulatie' wordt weergegeven boven aan de blade.

![De koptekst van een blade.](./media/app-insights-metrics-explorer/11-grain.png)

U kunt de granulatie in de blade van tijd bereik aanpassen:

![De koptekst van een blade.](./media/app-insights-metrics-explorer/grain.png)

De beschikbare granulariteit, is afhankelijk van het tijdsbereik dat u selecteert. De expliciete granulaties zijn alternatieven voor de 'automatisch' granulatie voor de periode.


## <a name="editing-charts-and-grids"></a>Grafieken en rasters bewerken
Een nieuwe grafiek toevoegen aan de blade:

![Kies in Metrics Explorer grafiek toevoegen](./media/app-insights-metrics-explorer/04-add.png)

Selecteer **bewerken** in een bestaande of nieuwe grafiek bewerken wat er wordt weergegeven:

![Selecteer een of meer waarden](./media/app-insights-metrics-explorer/08-select.png)

Hoewel er beperkingen van de combinaties die samen kunnen worden weergegeven, kunt u meer dan een waarde in een grafiek weergeven. Als u een waarde kiest, zijn sommige van de andere uitgeschakeld.

Als u gecodeerde [aangepaste metrische gegevens] [ track] in uw app (aanroepen TrackMetric en TrackEvent) ze worden hier vermeld.

## <a name="segment-your-data"></a>Segmenteren van uw gegevens
U kunt bijvoorbeeld een metriek door eigenschap - splitsen als u wilt vergelijken paginaweergaven op clients met verschillende besturingssystemen worden uitgevoerd.

Selecteer een grafiek of het raster, switch voor de groepering en kies een eigenschap moet worden gegroepeerd:

![Selecteer groepering op set en selecteer vervolgens een eigenschap in Group By](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> Wanneer u groepering gebruikt, geven de typen gebied en staafdiagram weergegeven in een gestapelde. Dit is geschikt waarbij de aggregatiemethode Sum. Maar wanneer het samenvoegingstype gemiddelde is, kiest u de regel of het raster weergave typen.
>
>

Als u gecodeerde [aangepaste metrische gegevens] [ track] in uw app en ze waarden van eigenschappen bevatten, u kunt de eigenschap in de lijst selecteren.

Is de grafiek te klein voor gesegmenteerde gegevens? De hoogte aanpassen:

![Stel de schuifregelaar](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Aggregatietypen
De legenda aan de kant standaard toont de cumulatieve waarde meestal gedurende de periode van de grafiek. Als u de muisaanwijzer op de grafiek, wordt de waarde op dat moment.

Elk gegevenspunt in de grafiek wordt een statistische functie van de gegevenswaarden in de voorgaande steekproefinterval of 'granulatie' ontvangen. De granulatie wordt weergegeven boven aan de blade en is afhankelijk van de algehele tijdschaal van de grafiek.

Metrische gegevens kunnen op verschillende manieren worden samengevoegd:

* **Aantal** is een aantal van de gebeurtenissen ontvangen in het controle-interval. Het wordt gebruikt voor gebeurtenissen zoals aanvragen. Verschillen in de hoogte van de grafiek geeft variaties in de snelheid waarmee de gebeurtenissen plaatsvinden. Maar houd er rekening mee dat de numerieke waarde verandert wanneer u het steekproefinterval wijzigt.
* **Som** voegt de waarden van alle gegevenspunten ontvangen via het controle-interval of de periode van de grafiek.
* **Gemiddelde** verdeelt de som van het aantal gegevenspunten ontvangen over het interval.
* **Unieke** tellingen voor aantallen gebruikers en -accounts worden gebruikt. Via het controle-interval of gedurende de periode van de grafiek ziet de afbeelding u de telling van andere gebruikers in die tijd weergegeven.
* **%**-percentage versies van elke aggregatie worden alleen gebruikt met gesegmenteerde grafieken. Het totale aantal altijd tot wel 100% wordt toegevoegd en de grafiek geeft de relatieve bijdrage van verschillende onderdelen van een totaal.

    ![Percentage aggregatie](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a>Het samenvoegingstype wijzigen

![De grafiek bewerken en selecteer vervolgens de samenvoeging](./media/app-insights-metrics-explorer/05-aggregation.png)

De standaardmethode voor elke waarde wordt weergegeven wanneer u een nieuwe grafiek maakt of wanneer alle metrische gegevens zijn uitgeschakeld:

![Schakel alle metrische gegevens voor een overzicht van de standaardinstellingen](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>Y-as pincode 
Standaard bevat een grafiek Y-as waarden vanaf nul tot de maximale waarden in het gegevensbereik geven een visuele representatie van quantum van de waarden. Maar in sommige gevallen meer dan het quantum mogelijk interessant visueel kleine wijzigingen in de waarden te controleren. De y-as liggen voor aanpassingen zoals dit gebruik bewerken functie om de waarde voor y-as minimum of maximum op de gewenste plaats vast te maken.
Klik op het selectievakje 'Geavanceerde instellingen' online zetten van het bereik voor y-as instellingen

![Klik op Geavanceerde instellingen aangepast bereik, en selecteer min max-waarden opgeven](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Filter uw gegevens
Om te zien alleen de metrische gegevens voor een geselecteerde set met eigenschapswaarden:

![Klik op Filter, vouw een eigenschap en sommige waarden controleren](./media/app-insights-metrics-explorer/19-filter.png)

Als u geen waarden voor een bepaalde eigenschap selecteert, is hetzelfde als wanneer u ze allemaal: Er is geen filter op die eigenschap.

U ziet het aantal gebeurtenissen naast elke eigenschapswaarde. Wanneer u de waarden van een eigenschap selecteert, wordt het aantal naast andere eigenschapswaarden worden aangepast.

Filters zijn van toepassing op de grafieken op een blade. Desgewenst kunt u verschillende filters zijn toegepast op verschillende grafieken maken en opslaan van andere metrische gegevens blades. Als u wilt, kunt u diagrammen van verschillende blades naar het dashboard vastmaken zodat u ze naast elkaar kan zien.

### <a name="remove-bot-and-web-test-traffic"></a>Verkeer van bot en web-test verwijderen
Gebruik het filter **daadwerkelijk of synthetisch verkeer** en Controleer **echte**.

U kunt ook filteren op **bron van synthetisch verkeer**.

### <a name="to-add-properties-to-the-filter-list"></a>Eigenschappen toevoegen aan de lijst met filters
Wilt u voor het filteren van telemetrie voor een categorie van uw eigen kiezen? Bijvoorbeeld, misschien verdeelt u van uw gebruikers in verschillende categorieën en u wilt dat uw gegevens te segmenteren in deze categorieën.

[Maak uw eigen eigenschap](app-insights-api-custom-events-metrics.md#properties). Stel deze een [telemetrie initialisatiefunctie](app-insights-api-custom-events-metrics.md#defaults) zodat deze worden weergegeven in alle telemetrie - waaronder de standaard telemetrie verzonden door de verschillende SDK-modules.

## <a name="edit-the-chart-type"></a>Het grafiektype bewerken
Merk op dat u tussen rasters en diagrammen kunt maken schakelen kunt:

![Selecteer een raster of de grafiek en een grafiektype kiezen](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>De blade metrische gegevens opslaan
Wanneer u sommige grafieken hebt gemaakt, kunt u ze als favoriet opslaan. U kunt kiezen of worden gedeeld met andere teamleden, als u een organisatie-account gebruiken.

![Kies favoriet](./media/app-insights-metrics-explorer/21-favorite-save.png)

Opnieuw, zodat de blade **gaat u naar de overzichtsblade** en Favorieten openen:

![Kies in de blade overzicht Favorieten](./media/app-insights-metrics-explorer/22-favorite-get.png)

Als u relatief tijdsbereik kiest wanneer u hebt opgeslagen, wordt de blade worden bijgewerkt met de meest recente metrische gegevens. Als u een absoluut tijdsbereik kiest, wordt deze telkens dezelfde gegevens weergeven.

## <a name="reset-the-blade"></a>De blade opnieuw instellen
Als u een blade bewerken, maar vervolgens u wilt teruggaan naar de oorspronkelijke set opgeslagen, klikt u op opnieuw instellen.

![In de knoppen aan de bovenkant van de metriek Explorer](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Stream Live metrische gegevens

Uw telemetrie veel meer direct wilt bekijken, opent u [Live Stream](app-insights-live-stream.md). De meeste metrische gegevens duren even wilt weergeven, door het proces van aggregatie. Daarentegen zijn live metrische gegevens geoptimaliseerd voor lage latentie. 

## <a name="set-alerts"></a>Waarschuwingen instellen
Ontvangen van e-mailadres van ongebruikelijke waarden van alle metrische gegevens, moet u een waarschuwing toevoegen. U kunt ofwel het e-mailbericht verzenden naar de accountbeheerders, of specifieke e-mailadressen.

![Kies in Metrics Explorer waarschuwingsregels, waarschuwing toevoegen](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Meer informatie over waarschuwingen][alerts].


## <a name="continuous-export"></a>Continuous Export
Als u gegevens continu exporteren wilt, zodat u deze extern kan verwerken, kunt u overwegen [continue export](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI
Als u nog meer weergaven van uw gegevens wilt, kunt u [exporteren naar Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analytische gegevens
[Analytics](app-insights-analytics.md) is een flexibeler manier om uw telemetrie met behulp van een krachtige querytaal analyseren. Te gebruiken als u wilt combineren de resultaten van de metrische gegevens berekenen, of een diepgaande exploratie van recente prestaties van uw app uitvoeren. 

Uit een metrische grafiek, kunt u het pictogram Analytics rechtstreeks naar de equivalente Analytics-query.

## <a name="troubleshooting"></a>Problemen oplossen
*Ik zie niet alle gegevens in de grafiek.*

* Filters zijn van toepassing op de grafieken op de blade. Zorg ervoor dat u een filter dat de gegevens op een andere uitsluit niet instellen terwijl u te op één grafiek focussen bent.

    Als u verschillende filters instellen voor verschillende grafieken, maakt u deze in verschillende blades wilt, slaat u ze als afzonderlijke Favorieten. Als u wilt, kunt u ze naar het dashboard vastmaken zodat u ze naast elkaar kan zien.
* Als u een grafiek met een eigenschap die is niet gedefinieerd in de metriek groepeert, wordt er niets op de grafiek. Wist u 'groeperen op' of kies een verschillende groepeerniveaus-eigenschap.
* Prestatiegegevens (CPU, i/o-snelheid, enzovoort) is beschikbaar voor Java-web-services, Windows desktop-apps, [IIS web-apps en services als u de statusmonitor installeren](app-insights-monitor-performance-live-website-now.md), en [Azure Cloud Services](app-insights-azure.md). Het is niet beschikbaar voor Azure websites.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
* [Gebruik met Application Insights controleren](app-insights-web-track-usage.md)
* [Met behulp van diagnostische gegevens doorzoeken](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
