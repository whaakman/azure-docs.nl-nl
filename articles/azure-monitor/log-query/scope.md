---
title: Meld u querybereik in Azure Monitor Log Analytics | Microsoft Docs
description: Beschrijving van het bereik- en bereik voor een logboekquery in Azure Monitor Log Analytics.
services: log-analytics
author: bwren
manager: carmonm
ms.service: log-analytics
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: bwren
ms.openlocfilehash: a948b80f6524339f0908a2fb19c4a83d70b3b140
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67297237"
---
# <a name="log-query-scope-and-time-range-in-azure-monitor-log-analytics"></a>Meld u query bereik en tijdsbereik in Azure Monitor Log Analytics
Bij het uitvoeren van een [logboekquery](log-query-overview.md) in [Log Analytics in Azure portal](get-started-portal.md), de set gegevens die worden geëvalueerd door de query is afhankelijk van het bereik en het tijdsbereik dat u selecteert. Dit artikel beschrijft de bereik- en tijdbereik en hoe u elk afhankelijk van uw vereisten kunt instellen. Hierin wordt ook het gedrag van verschillende typen bereiken.


## <a name="query-scope"></a>Querybereik
De querybereik definieert de records die worden geëvalueerd door de query. Deze bevat gewoonlijk alle records in een enkele Log Analytics-werkruimte of Application Insights-toepassing. Log Analytics kunt u een bereik voor een bepaalde bewaakte Azure-resource instellen. Hiermee wordt een resource-eigenaar in te richten op hun gegevens, zelfs als die resource naar meerdere werkruimten schrijft.

Het bereik wordt altijd weergegeven bovenaan links van de Log Analytics-venster. Een pictogram geeft aan of het bereik een Log Analytics-werkruimte of een Application Insights-toepassing is. Er is geen pictogram geeft aan een andere Azure-resource.

![Scope](media/scope/scope.png)

Het bereik wordt bepaald door de methode met kunt u Log Analytics starten en in sommige gevallen kunt u het bereik wijzigen door erop te klikken. De volgende tabel bevat de verschillende typen bereik dat wordt gebruikt en andere details voor elk.

| Querybereik | Records in het bereik | Selecteren | Bereik wijzigen |
|:---|:---|:---|:---|
| Log Analytics-werkruimte | Alle records in de Log Analytics-werkruimte. | Selecteer **logboeken** uit de **Azure Monitor** menu of de **Log Analytics-werkruimten** menu.  | Kan bereik wijzigen met een andere resourcetype. |
| Application Insights-toepassing | Alle records in de Application Insights-toepassing. | Selecteer **Analytics** van **overzicht** pagina van Application Insights. | Kan alleen bereik wijzigen naar een andere Application Insights-toepassing. |
| Resourcegroep | Records die zijn gemaakt door alle resources in de resourcegroep. Kan gegevens opnemen uit meerdere Log Analytics-werkruimten. | Selecteer **logboeken** vanuit het menu resourcegroep. | Kan het bereik niet wijzigen.|
| Abonnement | Records die zijn gemaakt door alle resources in het abonnement. Kan gegevens opnemen uit meerdere Log Analytics-werkruimten. | Selecteer **logboeken** in het menu abonnement.   | Kan het bereik niet wijzigen. |
| Andere Azure-resources | Records die zijn gemaakt door de resource. Kan gegevens opnemen uit meerdere Log Analytics-werkruimten.  | Selecteer **logboeken** in het resourcemenu.<br>OF<br>Selecteer **logboeken** uit de **Azure Monitor** menu en selecteer vervolgens een nieuwe scope. | Kan alleen bereik wijzigen naar dezelfde resourcetype. |

### <a name="limitations-when-scoped-to-a-resource"></a>Beperkingen bij het binnen het bereik van een resource

Als het querybereik een Log Analytics-werkruimte of een Application Insights-toepassing is, zijn alle opties in de portal en alle queryopdrachten beschikbaar. Als echter bereik aan een resource, de volgende opties in de portal niet beschikbaar omdat ze gekoppeld aan een één werkruimte of de toepassing zijn:

- Opslaan
- Queryverkenner
- Nieuwe waarschuwingsregel

U kunt de volgende opdrachten in een query als binnen het bereik van een resource omdat het querybereik nog geen werkruimten met gegevens voor die resource of set resources bevat gebruiken:

- [app](app-expression.md)
- [workspace](workspace-expression.md)
 


## <a name="time-range"></a>Tijdsbereik
Het tijdsbereik Hiermee geeft u de set met records die worden geëvalueerd voor de query op basis van wanneer de record is gemaakt. Dit wordt gedefinieerd door de eigenschap van een standaard op elke record in de werkruimte of de toepassing zoals opgegeven in de volgende tabel.

| Locatie | Eigenschap |
|:---|:---|
| Log Analytics-werkruimte          | TimeGenerated |
| Application Insights-toepassing | timestamp     |

Stel het tijdsbereik op door deze te selecteren van de tijd kiezen aan de bovenkant van het venster Log Analytics.  U kunt een vooraf gedefinieerde periode selecteren of **aangepaste** om op te geven van een bepaalde periode.

![Tijdkiezer](media/scope/time-picker.png)

Als u een filter in de query die gebruikmaakt van de eigenschap (standaardtijd instellen) zoals weergegeven in de bovenstaande tabel, de tijdkiezer gewijzigd naar **in query instellen**, en de tijdkiezer is uitgeschakeld. In dit geval is het meest efficiënt is om het filter aan de bovenkant van de query zodat elke verdere verwerking is alleen nodig heeft om te werken met de gefilterde records.

![Gefilterde query](media/scope/query-filtered.png)

Als u de [werkruimte](workspace-expression.md) of [app](app-expression.md) opdracht gegevens ophalen uit een andere werkruimte of toepassing, de tijdkiezer kan zich anders gedragen. Als het bereik een Log Analytics-werkruimte is en u gebruikt **app**, of als het bereik een Application Insights-toepassing is en u gebruikt **werkruimte**, en vervolgens de Log Analytics mogelijk niet weten dat de eigenschap moet worden gebruikt in de filter moet het tijdfilter bepalen.

In het volgende voorbeeld wordt het bereik ingesteld op een Log Analytics-werkruimte.  De query gebruikt **werkruimte** gegevens ophalen uit een andere Log Analytics-werkruimte. De tijdkiezer wordt gewijzigd in **in query instellen** omdat deze een filter die gebruikmaakt van de verwachte ziet **TimeGenerated** eigenschap.

![Query's uitvoeren met de werkruimte](media/scope/query-workspace.png)

Als de query gebruikt **app** als u wilt echter gegevens ophaalt uit een toepassing Application Insights, Log Analytics niet wordt herkend door de **timestamp** eigenschap in het filter en de tijdkiezer ongewijzigd blijft. In dit geval worden beide filters toegepast. In het voorbeeld, alleen de records die zijn gemaakt in de afgelopen 24 uur zijn opgenomen in de query zelfs als deze zeven dagen in Hiermee geeft u de **waar** component.

![Query's uitvoeren met de app](media/scope/query-app.png)

## <a name="next-steps"></a>Volgende stappen

- Doorloop een [zelfstudie over het gebruik van Log Analytics in Azure portal](get-started-portal.md).
- Doorloop een [zelfstudie over het schrijven van query's](get-started-queries.md).