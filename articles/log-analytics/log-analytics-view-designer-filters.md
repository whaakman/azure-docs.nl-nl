---
title: Filters in weergaven van de Azure Log Analytics | Microsoft Docs
description: Een filter in een weergave logboekanalyse kan gebruikers de gegevens in de weergave met de waarde van een bepaalde eigenschap filteren zonder te wijzigen van de weergave zelf.  Dit artikel wordt beschreven hoe u een filter en toevoegen aan een aangepaste weergave.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 21b54f60286e25c410b9d51de8be122c450080d3
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752772"
---
# <a name="filters-in-log-analytics-views"></a>Filters in weergaven Log Analytics
Een **filter** in een [logboekanalyse weergeven](log-analytics-view-designer.md) kunnen gebruikers de gegevens in de weergave filteren op de waarde van een bepaalde eigenschap zonder dat de weergave zelf worden gewijzigd.  U zou bijvoorbeeld kunnen gebruikers van uw weergave filteren op de weergave voor alleen de gegevens van een bepaalde computer of verzameling computers.  U kunt meerdere filters maken op een enkele weergave zodat gebruikers kunnen filteren op meerdere eigenschappen.  Dit artikel wordt beschreven hoe u een filter en toevoegen aan een aangepaste weergave.

## <a name="using-a-filter"></a>Met behulp van een filter
Klik op het tijdsbereik van de gegevens aan het begin van een weergave voor het openen van de vervolgkeuzelijst waar u het tijdsbereik van de gegevens voor de weergave kunt wijzigen.

![Voorbeeld van filter](media/log-analytics-view-designer/filters-example-time.png)

Klik op de **+** een filter met behulp van aangepaste filters die zijn gedefinieerd voor de weergave toe te voegen. Selecteer een waarde voor het filter ofwel uit de vervolgkeuzelijst of typ een waarde. Blijven filters toevoegen door te klikken op de **+**. 


![Voorbeeld van filter](media/log-analytics-view-designer/filters-example-custom.png)

Als u alle waarden voor een filter verwijdert, wordt het filter niet langer toegepast.


## <a name="creating-a-filter"></a>Een filter maken

Maken van een filter uit het **Filters** tabblad wanneer [bewerken van een weergave](log-analytics-view-designer.md).  Het filter is algemeen voor de weergave en geldt voor alle onderdelen in de weergave.  

![Filterinstellingen](media/log-analytics-view-designer/filters-settings.png)

De volgende tabel beschrijft de instellingen voor een filter.

| Instelling | Beschrijving |
|:---|:---|
| Veldnaam | Naam van het veld dat wordt gebruikt voor het filteren.  Dit moet overeenkomen met het veld samenvatten in **Query voor waarden**. |
| Query voor waarden | De query uit te voeren om het vullen van de vervolgkeuzelijst met filters voor de gebruiker.  Dit moet een gebruiken [samenvatten](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) of [distinct](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator) unieke waarden opgeven voor een bepaald veld en moet overeenkomen met de **veldnaam**.  U kunt [sorteren](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator) te sorteren van de waarden die worden weergegeven voor de gebruiker. |
| Tag | Naam voor het veld dat wordt gebruikt in query's ondersteunen het filter en wordt ook weergegeven voor de gebruiker. |

### <a name="examples"></a>Voorbeelden

De volgende tabel staan enkele voorbeelden van algemene filters.  

| Veldnaam | Query voor waarden | Tag |
|:--|:--|:--|
| Computer   | Heartbeat &#124; distinct Computer &#124; sorteren op een Computer asc | Computers |
| EventLevelName | Gebeurtenis &#124; distinct EventLevelName | Ernst |
| Foutcode | Syslog &#124; distinct foutcode | Ernst |
| SvcChangeType | Configuratiewijziging &#124; distinct svcChangeType | ChangeType |


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