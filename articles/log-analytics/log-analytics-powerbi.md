---
title: Log Analytics-gegevens exporteren naar Power BI | Microsoft Docs
description: "Power BI is een cloudgebaseerde business analytics-service van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets van gegevens biedt.  Log Analytics kunt continu gegevens exporteren uit de OMS-opslagplaats in Power BI zodat u van de vorm van visualisaties en hulpmiddelen voor analyse gebruikmaken kunt.  Dit artikel wordt beschreven hoe u query's in logboekanalyse die automatisch worden geëxporteerd naar Power BI met regelmatige tussenpozen configureert."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 271747e25f319c76195ec643025d24c6b7cdc9c5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="export-log-analytics-data-to-power-bi"></a>Log Analytics-gegevens exporteren naar Power BI

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [nieuwe logboekanalyse querytaal](log-analytics-log-search-upgrade.md), en vervolgens dit proces voor het exporteren van logboekanalyse gegevens met Power BI niet meer werken.  Eventuele bestaande schema's die u hebt gemaakt voordat u de upgrade wordt uitgeschakeld. Ook ziet niet langer u de mogelijkheid de mogelijkheden voor het exporteren van Power BI in instellingen onder Preview-functies inschakelen als deze functie volledig in de bijgewerkte werkruimten is vrijgegeven. 
>
> Na de upgrade, wordt Azure Log Analytics hetzelfde platform als Application Insights en u hetzelfde proces gebruiken om te exporteren Log Analytics-query's naar Power BI als [het proces voor het exporteren van Application Insights-query's naar Power BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  U kunt exporteren van de query met de console Analytics, zoals beschreven in dit artikel of kunt u de **Power BI** knop aan de bovenkant van het scherm in de portal logboek zoeken.
>
> Gebruikers moeten toegang tot de bron van de werkruimte in Azure om het gebruik van de Power BI export capabilitiy in bijgewerkte werkruimten. Zonder toegang tot ziet gebruikers een foutbericht weergegeven bij het importeren van de query voor Power BI desktop dat ze geen toegang.



[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) is een service in de cloud business analytics van Microsoft die uitgebreide visualisaties en rapporten voor analyse van verschillende sets van gegevens biedt.  Log Analytics kunt automatisch gegevens exporteren uit de OMS-opslagplaats in Power BI zodat u van de vorm van visualisaties en hulpmiddelen voor analyse gebruikmaken kunt.

Wanneer u Power BI met logboekanalyse configureert, maakt u log-query's die hun resultaten naar de bijbehorende gegevenssets in Power BI exporteren.  De query en uitvoer blijft automatisch moet worden uitgevoerd volgens een planning die u definieert voor de gegevensset up-to-date te houden met de meest recente gegevens verzameld door logboekanalyse.

![Log Analytics naar Power BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>Power BI-schema 's
Een *Power BI planning* bevat een logboek zoekopdracht die een set gegevens worden geëxporteerd vanuit de OMS-opslagplaats naar de bijbehorende gegevensset in Power BI en een planning die hoe vaak deze zoekopdracht wordt uitgevoerd definieert voor de gegevensset actueel te houden.

De velden in de gegevensset komt overeen met de eigenschappen van de records geretourneerd door het logboek zoeken.  Als de zoekopdracht records van verschillende typen retourneert vervolgens bevat de gegevensset alle eigenschappen van elk van de opgenomen recordtypen.  

> [!NOTE]
> Het is een best practice gebruik van een zoekquery logboek die als resultaat geeft onbewerkte gegevens in plaats van een consolidatie met opdrachten zoals uitvoert [meting](log-analytics-search-reference.md#measure).  U kunt een aggregatie en berekeningen in Power BI uitvoeren vanaf de onbewerkte gegevens.
>
>

## <a name="connecting-oms-workspace-to-power-bi"></a>OMS-werkruimte verbinding met Power BI
Voordat u van logboekanalyse naar Power BI exporteren kunt, moet u de OMS-werkruimte verbinden met uw Power BI-account met de volgende procedure.  

1. Klik in de OMS-console op de **instellingen** tegel.
2. Selecteer **Accounts**.
3. In de **werkruimte** sectie Klik **verbinding maken met Power BI-Account**.
4. Voer de referenties voor uw Power BI-account.

## <a name="create-a-power-bi-schedule"></a>Maak een planning van Power BI
Maak een Power BI-schema voor elke gegevensset met de volgende procedure.

1. Klik in de OMS-console op de **logboek zoeken** tegel.
2. Typ in een nieuwe query of Selecteer een opgeslagen zoekopdracht die de gegevens waarop u exporteren retourneert wilt naar **Power BI**.  
3. Klik op de **Power BI** knop aan de bovenkant van de pagina opent de **Power BI** dialoogvenster.
4. Geef de informatie in de volgende tabel en klik op **opslaan**.

| Eigenschap | Beschrijving |
|:--- |:--- |
| Naam |De naam om het schema te identificeren wanneer u de lijst met Power BI-schema's weergeven. |
| Opgeslagen zoekopdracht |De zoekopdracht logboek om uit te voeren.  Selecteer de huidige query of Selecteer een bestaande opgeslagen zoekopdracht in de vervolgkeuzelijst. |
| Planning |Hoe vaak de opgeslagen zoekopdracht uitvoeren en exporteren met de Power BI-gegevensset.  De waarde moet tussen 15 minuten en 24 uur. |
| De naam van de gegevensset |De naam van de gegevensset in Power BI.  Deze wordt gemaakt als deze niet bestaat en wordt bijgewerkt als het bestand bestaat. |

## <a name="viewing-and-removing-power-bi-schedules"></a>Weergeven en verwijderen van Power BI-schema 's
De lijst met bestaande Power BI-schema's met de volgende procedure weergeven.

1. Klik in de OMS-console op de **instellingen** tegel.
2. Selecteer **Power BI**.

Naast de details van de planning, worden het aantal keren dat met de planning wordt uitgevoerd in de afgelopen week en de status van de laatste synchronisatie weergegeven.  Als de synchronisatie fouten optreden, kunt u de koppeling om uit te voeren een zoekopdracht logboek voor records met gegevens over de fout kunt klikken.

U kunt een schema verwijderen door te klikken op de **X** in de **verwijderen kolom**.  U kunt een planning uitschakelen door het selecteren van **uit**.  Voor het wijzigen van een schema moet u deze verwijderen en maak deze opnieuw met de nieuwe instellingen.

![Power BI-schema 's](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>Voorbeeld-overzicht
De volgende sectie wordt een voorbeeld van een Power BI-schema maken en de bijbehorende gegevensset gebruiken voor het maken van een eenvoudig rapport uitgelegd.  In dit voorbeeld alle prestatiegegevens voor een set computers wordt geëxporteerd naar Power BI en vervolgens een lijngrafiek gemaakt om processorgebruik weer te geven.

### <a name="create-log-search"></a>Logboek zoekopdracht maken
Begin met het maken van een zoekopdracht logboek voor de gegevens die we willen verzenden naar de gegevensset.  In dit voorbeeld gebruiken we een query alle prestatiegegevens voor computers met een naam die begint retourneert met *srv*.  

![Power BI-schema 's](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>Zoeken met Power BI maken
We klikt u op de **Power BI** knop opent u het dialoogvenster Power BI en geef de vereiste gegevens.  We willen dat deze zoekopdracht uitvoeren één keer per uur en maken van een gegevensset met de naam *Contoso Perf*.  Aangezien we al de zoekopdracht openen die de gegevens die we willen maakt hebben, wordt de standaardwaarde van houden *huidige zoekquery gebruiken* voor **opgeslagen zoekopdrachten**.

![Zoeken met Power BI](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Controleer of zoeken met Power BI
Om te controleren of we de planning correct hebt gemaakt, wordt de lijst weergeven met Power BI zoekopdrachten op het **instellingen** -tegel in het dashboard OMS.  Er wacht een paar minuten en vernieuw deze weergave totdat deze rapporteert dat de synchronisatie is uitgevoerd.

![Zoeken met Power BI](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-the-dataset-in-power-bi"></a>Controleer of de gegevensset in Power BI
We Meld u aan bij onze account bij [powerbi.microsoft.com](http://powerbi.microsoft.com/) en schuif naar **gegevenssets** onderaan in het linkerdeelvenster.  U ziet dat de *Contoso Perf* gegevensset wordt vermeld die aangeeft dat de export is uitgevoerd.

![Power BI-gegevensset](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Maken op basis van gegevensset
U selecteert de **Contoso Perf** gegevensset en klik vervolgens op **resultaten** in de **velden** deelvenster op het recht om de velden die deel van deze gegevensset uitmaken te bekijken.  We uitvoeren de volgende acties voor het maken van een lijngrafiek processorgebruik voor elke computer weergegeven.

1. Selecteer de regel grafiek visualisatie.
2. Sleep **ObjectName** naar **rapport niveau filter** en Controleer **Processor**.
3. Sleep **CounterName** naar **rapport niveau filter** en Controleer **percentage processortijd**.
4. Sleep **tegenwaarde** naar **waarden**.
5. Sleep **Computer** naar **legenda**.
6. Sleep **TimeGenerated** naar **as**.

U ziet dat de resulterende lijngrafiek wordt weergegeven met de gegevens van onze gegevensset.

![Power BI-lijndiagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-the-report"></a>Het rapport opslaan
We het rapport door te klikken op de knop opslaan boven aan het scherm opslaan en valideren nu wordt vermeld in de sectie rapporten in het linkerdeelvenster.

![Power BI-rapporten](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) voor query's opbouwen die kunnen worden geëxporteerd naar Power BI.
* Meer informatie over [Power BI](http://powerbi.microsoft.com) visualisaties op basis van logboekanalyse uitvoer bouwen.
