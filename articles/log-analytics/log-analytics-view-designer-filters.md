---
title: Filters in weergaven van de Azure Log Analytics | Microsoft Docs
description: Een filter in een weergave logboekanalyse kan gebruikers de gegevens in de weergave met de waarde van een bepaalde eigenschap filteren zonder te wijzigen van de weergave zelf.  Dit artikel wordt beschreven hoe u een filter en toevoegen aan een aangepaste weergave.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2017
ms.author: bwren
ms.openlocfilehash: 5c2201292eb085dcc043e4257580c7971dbaffbd
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="filters-in-log-analytics-views"></a>Filters in weergaven Log Analytics
Een **filter** in een [logboekanalyse weergeven](log-analytics-view-designer.md) kunnen gebruikers de gegevens in de weergave filteren op de waarde van een bepaalde eigenschap zonder dat de weergave zelf worden gewijzigd.  U zou bijvoorbeeld kunnen gebruikers van uw weergave filteren op de weergave voor alleen de gegevens van een bepaalde computer of verzameling computers.  U kunt meerdere filters maken op een enkele weergave zodat gebruikers kunnen filteren op meerdere eigenschappen.  Dit artikel wordt beschreven hoe u een filter en toevoegen aan een aangepaste weergave.

## <a name="using-a-filter"></a>Met behulp van een filter
Klik op **Filter** om het filterdeelvenster voor een weergave te openen.  Hiermee kunt u een tijdsbereik en waarden voor alle filters die beschikbaar voor de weergave zijn te selecteren.  Wanneer u een filter selecteert, geeft een lijst met beschikbare waarden weer.  Selecteer een of meer waarden of typ de wachtwoorden in. De weergave wordt automatisch bijgewerkt om te filteren op de waarden die u opgeeft. 

Als er geen waarde voor een filter is ingeschakeld, wordt niet naar de weergave met het filter toegepast.  Als u alle waarden voor een filter verwijdert, wordt het filter niet langer toegepast.


![Voorbeeld van filter](media/log-analytics-view-designer/filters-example.png)


## <a name="creating-a-filter"></a>Een filter maken

Maken van een filter uit het **Filters** tabblad wanneer [bewerken van een weergave](log-analytics-view-designer.md).  Het filter is algemeen voor de weergave en geldt voor alle onderdelen in de weergave.  

![Instellingen voor filteren](media/log-analytics-view-designer/filters-settings.png)

De volgende tabel beschrijft de instellingen voor een filter.

| Instelling | Beschrijving |
|:---|:---|
| Veldnaam | Naam van het veld dat wordt gebruikt voor het filteren.  Dit moet overeenkomen met het veld samenvatten in **Query voor waarden**. |
| Query voor waarden | De query uit te voeren om het vullen van de vervolgkeuzelijst met filters voor de gebruiker.  Dit moet een gebruiken [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) of [distinct](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator) unieke waarden opgeven voor een bepaald veld en moet overeenkomen met de **veldnaam**.  U kunt [sorteren](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator) te sorteren van de waarden die worden weergegeven voor de gebruiker. |
| Label | Naam voor het veld dat wordt gebruikt in query's ondersteunen het filter en wordt ook weergegeven voor de gebruiker. |

### <a name="examples"></a>Voorbeelden

De volgende tabel staan enkele voorbeelden van algemene filters.  

| Veldnaam | Query voor waarden | Label |
|:--|:--|:--|
| Computer   | Hartslag &#124; afzonderlijke Computer &#124; sorteren op de Computer asc | Computers |
| EventLevelName | Gebeurtenis &#124; afzonderlijke EventLevelName | Ernst |
| Foutcode | Syslog &#124; afzonderlijke foutcode | Ernst |
| SvcChangeType | Configuratiewijziging &#124; afzonderlijke svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Wijzigen van query's weergeven

Voor een filter geen effect, moet u alle query's in de weergave te filteren op de geselecteerde waarden wijzigen.  Als u niet alle query's in de weergave wijzigt hebben geen waarden die van de gebruiker geen effect.

De syntaxis voor het gebruik van een filterwaarde in een query is: 

    where ${filter name}  

Bijvoorbeeld, als uw weergave een query de gebeurtenissen retourneert heeft en een filter Computers genoemd gebruikt, kunt u de volgende.

    Event | where ${Computers} | summarize count() by EventLevelName

Als u een ander filter aangeroepen ernst hebt toegevoegd, kunt u de volgende query beide filters te gebruiken.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [visualisatie delen](log-analytics-view-designer-parts.md) kunt u toevoegen aan de aangepaste weergave.