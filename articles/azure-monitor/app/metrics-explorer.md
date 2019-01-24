---
title: Metrische gegevens in Azure Application Insights verkennen | Microsoft Docs
description: Over het interpreteren van grafieken op metric explorer en metric explorer blades aanpassen.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/22/2019
ms.author: mbullwin
ms.openlocfilehash: 5c659ca2f40d47450227d16963499a6b27c9e313
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54817114"
---
# <a name="exploring-metrics-in-application-insights"></a>Metrische gegevens in Application Insights verkennen
Metrische gegevens in [Application Insights] [ start] zijn gemeten waarden en aantallen gebeurtenissen die worden verzonden in telemetrie van uw toepassing. Ze helpen u prestatieproblemen detecteren en bekijk de trends in hoe uw toepassing wordt gebruikt. Er is een breed scala aan standaard metrische gegevens en u kunt ook uw eigen aangepaste metrische gegevens en gebeurtenissen maken.

> [!NOTE]
> Dit artikel beschrijft de ervaring in de klassieke metrics explorer die momenteel is afgeschaft en zal uiteindelijk buiten gebruik worden gesteld. We raden u aan te checken, wordt de nieuwe ervaring die wordt beschreven in [in dit artikel](../platform/metrics-charts.md).

Aantal metrische gegevens en gebeurtenissen worden weergegeven in de grafieken van de geaggregeerde waarden zoals totalen, gemiddelden of aantal.

Hier volgt een voorbeeld reeks grafieken:

![](./media/metrics-explorer/01-overview.png)

U vindt grafieken met metrische gegevens overal in de Application Insights-portal. In de meeste gevallen kan worden aangepast en u kunt meer grafieken toevoegen aan de blade. Klik op de blade overzicht door naar meer gedetailleerde grafieken (waarvoor titels, zoals 'Servers'), of klik op **Metrics Explorer** om een nieuwe blade waar u aangepaste grafieken kunt maken te openen.

## <a name="time-range"></a>Tijdsbereik
U kunt het tijdsbereik wordt gedekt door de diagrammen of rasters op een blade wijzigen.

![Open de overzichtsblade van uw toepassing in Azure portal](./media/metrics-explorer/03-range.png)

Als u bepaalde gegevens die nog niet is werden verwacht, klikt u op vernieuwen. Grafieken vernieuwd met intervallen, maar de intervallen zijn langer voor grotere tijdsbereik. Het kan even gegevens binnenkomen via de pijplijn analyse naar een diagram.

Als u wilt inzoomen op een deel van een diagram, sleept u erboven:

![Sleep in het gedeelte van een grafiek.](./media/metrics-explorer/12-drag.png)

Klik op de knop Zoomen ongedaan maken om te herstellen.

## <a name="granularity-and-point-values"></a>Waarden granulariteit en punt
De muisaanwijzer boven de grafiek om de waarden van de metrische gegevens op dat moment weer te geven.

![Beweeg de muisaanwijzer over een grafiek](./media/metrics-explorer/02-focus.png)

De waarde van de metrische gegevens op een bepaald tijdstip wordt geaggregeerd in het voorgaande steekproefinterval.

De controle-interval of 'granulariteit' wordt weergegeven aan de bovenkant van de blade.

![De koptekst van een blade.](./media/metrics-explorer/11-grain.png)

U kunt de granulatie in de blade van de bereik tijd aanpassen:

![De koptekst van een blade.](./media/metrics-explorer/grain.png)

De granulariteit die beschikbaar zijn, is afhankelijk van het tijdsbereik dat u selecteert. De expliciete granulaties zijn alternatieven voor de 'automatisch' granulariteit voor het tijdsbereik.


## <a name="editing-charts-and-grids"></a>Bewerken van grafieken en rasters
Een nieuwe grafiek toevoegen aan de blade:

![Kies in Metrics Explorer grafiek toevoegen](./media/metrics-explorer/04-add.png)

Selecteer **bewerken** op een bestaande of nieuwe grafiek bewerken wat er wordt weergegeven:

![Selecteer een of meer waarden](./media/metrics-explorer/08-select.png)

Hoewel er gelden beperkingen over de combinaties die samen kunnen worden weergegeven, kunt u meer dan één metrische gegevens weergeven in een grafiek. Als u een waarde kiezen, zijn sommige van de andere uitgeschakeld.

Als u gecodeerd [aangepaste metrische gegevens] [ track] in uw app (aanroepen naar TrackMetric en TrackEvent) ze worden hier vermeld.

## <a name="segment-your-data"></a>Uw gegevens segmenteren
U kunt bijvoorbeeld een metrische waarde door de eigenschap - splitsen als u wilt vergelijken van paginaweergaven op clients met verschillende besturingssystemen.

Selecteer een grafiek of het raster, Ga voor de groepering en kies een eigenschap te groeperen op:

![Selecteer groepering op set en selecteer vervolgens een eigenschap in Group By](./media/metrics-explorer/15-segment.png)

> [!NOTE]
> Wanneer u groepen gebruikt, geef de oppervlakte en staafdiagram typen een gestapelde weer te geven. Dit is geschikt waar de aggregatiemethode som is. Maar waar het samenvoegingstype gemiddelde is, kiest u de Line- of Grid weergeven-typen.
>
>

Als u gecodeerd [aangepaste metrische gegevens] [ track] in uw app en ze eigenschapswaarden bevatten, u kunt de eigenschap selecteren in de lijst.

Is de grafiek te klein voor de gesegmenteerde gegevens? De hoogte aanpassen:

![Stel de schuifregelaar](./media/metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Aggregatietypen
De legenda aan de zijkant standaard meestal de geaggregeerde waarden worden weergegeven gedurende de periode van de grafiek. Als u de muisaanwijzer op de grafiek, geeft deze de waarde op dat moment.

Elk gegevenspunt in de grafiek is een statistische functie van de gegevenswaarden in de voorgaande steekproefinterval of "granulariteit" ontvangen. De granulatie wordt weergegeven aan de bovenkant van de blade en is afhankelijk van de algehele tijdschaal van de grafiek.

Metrische gegevens kunnen worden geaggregeerd op verschillende manieren:

* **Aantal** is een aantal van de gebeurtenissen ontvangen in het controle-interval. Het wordt gebruikt voor gebeurtenissen, zoals aanvragen. Verschillen in de hoogte van de grafiek geeft variaties in de snelheid waarmee de gebeurtenissen plaatsvinden. Maar houd er rekening mee dat de numerieke waarde wordt gewijzigd wanneer u de controle-interval wijzigen.
* **Som** is de som van de waarden van alle gegevenspunten die worden ontvangen via het controle-interval of de periode van de grafiek.
* **Gemiddelde** de som deelt door het aantal gegevenspunten ontvangen over het interval.
* **De unieke** aantallen worden gebruikt voor het aantal gebruikers en accounts. Via het controle-interval, of de periode van de grafiek ziet de afbeelding u het aantal verschillende gebruikers in die tijd weergegeven.
* **%** -percentage versies van elke aggregatie worden alleen gebruikt met gesegmenteerde grafieken. Het totaal wordt altijd tot 100% en de grafiek toont de relatieve bijdrage van de verschillende onderdelen van een totaal.

    ![Percentage aggregatie](./media/metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a>Het aggregatietype wijzigen

![De grafiek bewerken en selecteer vervolgens aggregatie](./media/metrics-explorer/05-aggregation.png)

De standaardmethode voor elke waarde wordt weergegeven wanneer u een nieuwe grafiek maakt of wanneer alle metrische gegevens zijn uitgeschakeld:

![Schakel alle metrische gegevens om te zien van de standaardinstellingen](./media/metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>Pincode y-as 
Y-as waarden vanaf nul tot maximale waarden in het gegevensbereik, zodat een visuele representatie van quantum van de waarden worden standaard een grafiek weergegeven. Maar in sommige gevallen meer dan het quantum mogelijk interessant om te kleine wijzigingen in waarden visueel te controleren. De y-as variëren voor aanpassingen, zoals dit gebruik bewerkingsfunctie om de waarde voor y-as minimum of maximum op de gewenste plek vast te maken.
Klik op 'Instellingen voor geavanceerde' selectievakje in om de instellingen van de y-as-bereik

![Klik op Geavanceerde instellingen, selecteer Aangepast bereik en min-max-waarden opgeven](./media/metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Uw gegevens filteren
Om te zien alleen de metrische gegevens voor een geselecteerde set met eigenschapswaarden:

![Klik op Filter, vouwt u een eigenschap en enkele waarden controleren](./media/metrics-explorer/19-filter.png)

Als u niet alle waarden voor een bepaalde eigenschap selecteert, is hetzelfde als wanneer u ze allemaal: Er is geen filter voor die eigenschap.

U ziet de aantallen gebeurtenissen samen met de waarde van elke eigenschap. Wanneer u de waarden van één eigenschap selecteert, wordt het aantal naast andere eigenschapswaarden worden aangepast.

Filters toepassen op alle grafieken uit een blad. Als u wilt dat verschillende filters toegepast op verschillende grafieken, maken en opslaan van verschillende metrische gegevens blades. Als u wilt, kunt u diagrammen uit verschillende blades aan het dashboard kunt vastmaken, zodat u ze naast elkaar kunt zien.

### <a name="remove-bot-and-web-test-traffic"></a>Verwijderen van bot- en web-test-verkeer
Gebruik het filter **daadwerkelijk of synthetisch verkeer** en Controleer **echte**.

U kunt ook filteren op **bron van synthetisch verkeer**.

### <a name="to-add-properties-to-the-filter-list"></a>Eigenschappen toevoegen aan de lijst met filters
Wilt u de telemetrie voor een categorie van uw eigen keuze filteren? Bijvoorbeeld, misschien u delen van uw gebruikers in verschillende categorieën, en u wilt dat uw gegevens segmenteren op deze categorieën.

[Maak uw eigen eigenschap](../../azure-monitor/app/api-custom-events-metrics.md#properties). Stel deze een [telemetrische Initializer](../../azure-monitor/app/api-custom-events-metrics.md#defaults) zodat deze worden weergegeven in alle telemetrie - met inbegrip van de telemetrie die is verzonden door andere SDK-modules.

## <a name="edit-the-chart-type"></a>Type van de grafiek bewerken
U ziet dat u tussen rasters en grafieken schakelen kunt:

![Selecteer een raster of diagram en kies vervolgens een grafiektype](./media/metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>De blade met metrische gegevens opslaan
Als u andere grafieken hebt gemaakt, slaat u ze als een favoriet. U kunt kiezen of om deze te delen met andere teamleden, als u een organisatie-account.

![Kies aan uw Favorieten toevoegen](./media/metrics-explorer/21-favorite-save.png)

Om te zien van de blade opnieuw **Ga naar de overzichtsblade** en Favorieten openen:

![Kies in de blade overzicht Favorieten](./media/metrics-explorer/22-favorite-get.png)

Als u bij het opslaan van relatief tijdsbereik, wordt de blade met de meest recente metrische gegevens worden bijgewerkt. Als u een absoluut tijdsbereik kiest, wordt deze telkens dezelfde gegevens weergeven.

## <a name="reset-the-blade"></a>De blade opnieuw instellen
Als u een blade bewerken, maar vervolgens u wilt teruggaan naar de oorspronkelijke set opgeslagen, klikt u op opnieuw instellen.

![In de knoppen aan de bovenkant van Metric Explorer](./media/metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Live metrische-gegevensstroom

Voor een nog veel meer onmiddellijk een overzicht van uw telemetrie, opent u [Live Stream](live-stream.md). De meeste metrische gegevens over duren enkele minuten worden weergegeven, omdat het proces van aggregatie. Daarentegen, zijn live metrische gegevens geoptimaliseerd voor lage latentie. 

## <a name="set-alerts"></a>Waarschuwingen instellen
Om te worden geïnformeerd via e-mail van ongebruikelijke waarden van elke meetwaarde, moet u een waarschuwing toevoegen. U kunt toekennen aan het e-mailbericht verzenden naar de accountbeheerders, of specifieke e-mailadressen.

![Kies in Metrics Explorer waarschuwingsregels, waarschuwing toevoegen](./media/metrics-explorer/appinsights-413setMetricAlert.png)

[Meer informatie over waarschuwingen][alerts].


## <a name="continuous-export"></a>Continuous Export
Als u gegevens continu exporteren wilt, zodat u deze extern kan verwerken, kunt u overwegen [continue export](../../azure-monitor/app/export-telemetry.md).

### <a name="power-bi"></a>Power BI
Als u nog uitgebreider weergaven van uw gegevens wilt, kunt u [exporteren naar Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analyse
[Analytics](../../azure-monitor/app/analytics.md) is een veelzijdiger manier voor het analyseren van uw telemetrie met behulp van een krachtige querytaal. Gebruik deze optie als u wilt combineren, berekenen van resultaten van metrische gegevens of een grondig onderzoek van de recente prestaties van uw app uitvoeren. 

In een grafiek met metrische gegevens, kunt u het pictogram Analytics rechtstreeks naar de equivalente Analytics-query.

## <a name="troubleshooting"></a>Problemen oplossen
*Ik zie niet alle gegevens in de grafiek.*

* Filters toepassen op de grafieken op de blade. Zorg ervoor dat u een filter op dat niet van toepassing op alle gegevens op een andere is niet ingesteld terwijl u te op één grafiek focussen bent.

    Als u wilt andere filters instellen op de verschillende grafieken, ze in verschillende blades maken, deze opslaan als afzonderlijke Favorieten. Als u wilt, kunt u deze vastmaken aan het dashboard zodat u ze naast elkaar kunt zien.
* Als u een grafiek op een eigenschap die is niet gedefinieerd in de metriek groepeert, wordt er niets in de grafiek. Wist u 'groeperen op', of kies een eigenschap verschillende groepeerniveaus.
* Prestatiegegevens (CPU, IO-snelheid, enzovoort) is beschikbaar voor Java-web-services, Windows-bureaublad-apps, [IIS web-apps en services als u statusmonitor installeert](../../azure-monitor/app/monitor-performance-live-website-now.md), en [Azure Cloud Services](../../azure-monitor/app/app-insights-overview.md). Het is niet beschikbaar voor Azure websites.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
* [Bewaking met Application Insights wordt gebruikt](../../azure-monitor/app/usage-overview.md)
* [Met behulp van diagnostische gegevens doorzoeken](../../azure-monitor/app/diagnostic-search.md)

<!--Link references-->

[alerts]: ../../azure-monitor/app/alerts.md
[start]: ../../azure-monitor/app/app-insights-overview.md
[track]: ../../azure-monitor/app/api-custom-events-metrics.md
