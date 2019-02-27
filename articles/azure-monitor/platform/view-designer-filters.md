---
title: Filters in weergaven van Azure Monitor | Microsoft Docs
description: Een filter in de weergave van een Azure Monitor kan gebruikers de gegevens in de weergave filteren op de waarde van een bepaalde eigenschap zonder te wijzigen van de weergave zelf.  In dit artikel wordt beschreven hoe u een filter gebruiken en toevoegen aan een aangepaste weergave.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 31a902302ba806889854330c6517d9f5745f1c0c
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56888335"
---
# <a name="filters-in-azure-monitor-views"></a>Filters in Azure Monitor-weergaven
Een **filter** in een [Azure Monitor weergeven](view-designer.md) kunnen gebruikers de gegevens in de weergave filteren op de waarde van een bepaalde eigenschap zonder te wijzigen van de weergave zelf.  Bijvoorbeeld, kunnen gebruikers van de weergave voor het filteren van de weergave voor alleen de gegevens van een bepaalde computer of verzameling computers.  U kunt meerdere filters in één weergave die u wilt toestaan dat gebruikers om te filteren op meerdere eigenschappen maken.  In dit artikel wordt beschreven hoe u een filter gebruiken en toevoegen aan een aangepaste weergave.

## <a name="using-a-filter"></a>Met behulp van een filter
Klik op het datumbereik voor de tijd aan de bovenkant van een weergave te openen van de vervolgkeuzelijst het datumbereik voor de tijd voor de weergave te wijzigen.

![Voorbeeld van filter](media/view-designer-filters/filters-example-time.png)

Klik op de **+** een filter met behulp van aangepaste filters die zijn gedefinieerd voor de weergave toe te voegen. Selecteer een waarde voor het filter ofwel uit de vervolgkeuzelijst of typ een waarde in. Doorgaan met het toevoegen van filters door te klikken op de **+**. 


![Voorbeeld van filter](media/view-designer-filters/filters-example-custom.png)

Als u alle waarden voor een filter verwijdert, wordt met het filter niet langer toegepast.


## <a name="creating-a-filter"></a>Een filter maken

Maken van een filter van de **Filters** tabblad wanneer [bewerken van een weergave](view-designer.md).  Het filter voor de weergave van een globale is en is van toepassing op alle onderdelen in de weergave.  

![Filterinstellingen](media/view-designer-filters/filters-settings.png)

De volgende tabel beschrijft de instellingen voor een filter.

| Instelling | Description |
|:---|:---|
| Veldnaam | De naam van het veld dat wordt gebruikt om te filteren.  Dit veld moet overeenkomen met het veld samenvatten in **Query voor waarden**. |
| Query voor waarden | De query uit te voeren om het vullen van de vervolgkeuzelijst met filters voor de gebruiker.  Deze query moet een gebruiken [samenvatten](/azure/kusto/query/summarizeoperator) of [afzonderlijke](/azure/kusto/query/distinctoperator) unieke waarden opgeven voor een bepaald veld, zodat het overeenkomt met de **veldnaam**.  U kunt [sorteren](/azure/kusto/query/sortoperator) om te sorteren van de waarden die worden weergegeven aan de gebruiker. |
| Label | Naam voor het veld dat wordt gebruikt in query's het filter ondersteunt en wordt ook weergegeven aan de gebruiker. |

### <a name="examples"></a>Voorbeelden

De volgende tabel bevat enkele voorbeelden van algemene filters.  

| Veldnaam | Query voor waarden | Label |
|:--|:--|:--|
| Computer   | Heartbeat &#124; distinct Computer &#124; sorteren op Computer asc | Computers |
| EventLevelName | Event &#124; distinct EventLevelName | Severity |
| Foutcode | Syslog &#124; distinct SeverityLevel | Severity |
| SvcChangeType | ConfigurationChange &#124; afzonderlijke svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Wijzigen van query's weergeven

Voor een filter van invloed zijn, moet u alle query's in de weergave om te filteren op basis van de geselecteerde waarden wijzigen.  Als er query's in de weergave niet te wijzigen, klikt u vervolgens heeft alle waarden die van de gebruiker geen effect.

De syntaxis voor het gebruik van een filterwaarde in een query is: 

    where ${filter name}  

Bijvoorbeeld, als uw weergave heeft een query die Hiermee worden gebeurtenissen geretourneerd en gebruikt een filter met de naam _Computers_, kunt u de volgende query uit.

    Event | where ${Computers} | summarize count() by EventLevelName

Als u een ander filter met de naam van de ernst toegevoegd, kunt u de volgende query uit beide filters te gebruiken.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [visualisatie delen](view-designer-parts.md) kunt u toevoegen aan uw aangepaste weergave.
