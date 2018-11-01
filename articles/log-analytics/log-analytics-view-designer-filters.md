---
title: Filters in de Azure Log Analytics | Microsoft Docs
description: Een filter in een Log Analytics kan gebruikers de gegevens in de weergave filteren op de waarde van een bepaalde eigenschap zonder te wijzigen van de weergave zelf.  In dit artikel wordt beschreven hoe u een filter gebruiken en toevoegen aan een aangepaste weergave.
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
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 87cfcefd91652194adc0a5ca4580a8d00ba4d234
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412743"
---
# <a name="filters-in-log-analytics-views"></a>Filters in Log Analytics-weergaven
Een **filter** in een [Log Analytics weergeven](log-analytics-view-designer.md) kunnen gebruikers de gegevens in de weergave filteren op de waarde van een bepaalde eigenschap zonder te wijzigen van de weergave zelf.  Bijvoorbeeld, kunnen gebruikers van de weergave voor het filteren van de weergave voor alleen de gegevens van een bepaalde computer of verzameling computers.  U kunt meerdere filters in één weergave die u wilt toestaan dat gebruikers om te filteren op meerdere eigenschappen maken.  In dit artikel wordt beschreven hoe u een filter gebruiken en toevoegen aan een aangepaste weergave.

## <a name="using-a-filter"></a>Met behulp van een filter
Klik op het bereik van de tijd aan de bovenkant van een weergave te openen van de vervolgkeuzelijst voor bereik van de tijd voor de weergave te wijzigen.

![Voorbeeld van filter](media/log-analytics-view-designer-filters/filters-example-time.png)

Klik op de **+** een filter met behulp van aangepaste filters die zijn gedefinieerd voor de weergave toe te voegen. Selecteer een waarde voor het filter ofwel uit de vervolgkeuzelijst of typ een waarde in. Doorgaan met het toevoegen van filters door te klikken op de **+**. 


![Voorbeeld van filter](media/log-analytics-view-designer-filters/filters-example-custom.png)

Als u alle waarden voor een filter verwijdert, wordt met het filter niet langer toegepast.


## <a name="creating-a-filter"></a>Een filter maken

Maken van een filter van de **Filters** tabblad wanneer [bewerken van een weergave](log-analytics-view-designer.md).  Het filter voor de weergave van een globale is en is van toepassing op alle onderdelen in de weergave.  

![Filterinstellingen](media/log-analytics-view-designer-filters/filters-settings.png)

De volgende tabel beschrijft de instellingen voor een filter.

| Instelling | Beschrijving |
|:---|:---|
| Veldnaam | De naam van het veld dat wordt gebruikt om te filteren.  Dit moet overeenkomen met het veld samenvatten in **Query voor waarden**. |
| Query voor waarden | De query uit te voeren om het vullen van de vervolgkeuzelijst met filters voor de gebruiker.  Dit moet een gebruiken [samenvatten](/azure/kusto/query/summarizeoperator) of [afzonderlijke](/azure/kusto/query/distinctoperator) unieke waarden opgeven voor een bepaald veld, zodat het overeenkomt met de **veldnaam**.  U kunt [sorteren](/azure/kusto/query/sortoperator) om te sorteren van de waarden die worden weergegeven aan de gebruiker. |
| Label | Naam voor het veld dat wordt gebruikt in query's het filter ondersteunt en wordt ook weergegeven aan de gebruiker. |

### <a name="examples"></a>Voorbeelden

De volgende tabel bevat enkele voorbeelden van algemene filters.  

| Veldnaam | Query voor waarden | Label |
|:--|:--|:--|
| Computer   | Heartbeat &#124; distinct Computer &#124; sorteren op Computer asc | Computers |
| EventLevelName | Gebeurtenis &#124; afzonderlijke EventLevelName | Severity |
| Foutcode | Syslog &#124; afzonderlijke foutcode | Severity |
| SvcChangeType | ConfigurationChange &#124; afzonderlijke svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Wijzigen van query's weergeven

Voor een filter van invloed zijn, moet u alle query's in de weergave om te filteren op basis van de geselecteerde waarden wijzigen.  Als er query's in de weergave niet te wijzigen hebben geen waarden die de gebruiker selecteert geen effect.

De syntaxis voor het gebruik van een filterwaarde in een query is: 

    where ${filter name}  

Bijvoorbeeld, als uw weergave een query de gebeurtenissen retourneert heeft en een filter met de naam van de Computers gebruikt, kunt u de volgende.

    Event | where ${Computers} | summarize count() by EventLevelName

Als u een ander filter met de naam van de ernst toegevoegd, kunt u de volgende query uit beide filters te gebruiken.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [visualisatie delen](log-analytics-view-designer-parts.md) kunt u toevoegen aan uw aangepaste weergave.
