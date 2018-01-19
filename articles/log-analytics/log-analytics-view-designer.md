---
title: Maken van weergaven voor het analyseren van gegevens in Azure Log Analytics | Microsoft Docs
description: Ontwerper in Log Analytics kunt u aangepaste weergaven maken die worden weergegeven in de Azure-portal en verschillende visualisaties van gegevens in de werkruimte voor logboekanalyse bevatten. Dit artikel bevat een overzicht van Designer bekijken en procedures voor het maken en bewerken van aangepaste weergaven.
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2018
ms.author: bwren
ms.openlocfilehash: a84f40503c1b9778c496461ebbf6864f99bd1c4b
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Weergave Designer gebruiken voor het maken van aangepaste weergaven in Log Analytics
De ontwerper in [logboekanalyse](log-analytics-overview.md) kunt u aangepaste weergaven maken in de Azure portal die verschillende visualisaties van gegevens in de werkruimte voor logboekanalyse bevatten. Dit artikel bevat een overzicht van Designer bekijken en procedures voor het maken en bewerken van aangepaste weergaven.

Er zijn andere artikelen die beschikbaar zijn voor weergave Designer:

* [Tegel verwijzing](log-analytics-view-designer-tiles.md) -verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.
* [Visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md) -verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en vervolgens de query's in alle weergaven moeten worden geschreven in de [nieuwe querytaal](https://go.microsoft.com/fwlink/?linkid=856078).  De weergaven die zijn gemaakt voordat de werkruimte werd bijgewerkt worden doorloopt geconverteerd.

## <a name="concepts"></a>Concepten
Weergaven worden weergegeven op de **overzicht** pagina van de werkruimte voor logboekanalyse in de Azure portal.  De tegel voor elke aangepaste weergave wordt alfabetisch met de tegels weergegeven voor de oplossingen dezelfde werkruimte geïnstalleerd.

![Overzichtspagina](media/log-analytics-view-designer/overview-page.png)

Weergaven maken met de ontwerpfunctie voor weergaven bevatten de elementen in de volgende tabel.

| Onderdeel | Beschrijving |
|:--- |:--- |
| Tegel |Weergegeven op de overzichtspagina voor uw werkruimte voor logboekanalyse.  Bevat een samenvatting van de informatie in de aangepaste weergave visual.  Verschillende typen van de tegel bieden verschillende visualisaties van records.  Klik op de tegel om de aangepaste weergave te openen. |
| Aangepaste weergave |Wanneer de gebruiker klikt op de tegel weergegeven.  Bevat een of meer visualisatie delen. |
| Visualisatie delen |Visualisatie van gegevens in de werkruimte voor logboekanalyse op basis van een of meer [Meld zoekopdrachten](log-analytics-log-searches.md).  De meeste onderdelen bevat een Header die een hoog niveau visualisatie bevat en een lijst van de bovenste resultaten.  Verschillende typen bieden verschillende visualisaties van records in de werkruimte voor logboekanalyse.  Klik op elementen in het gedeelte logboek zoekopdracht bieden gedetailleerde records. |


## <a name="work-with-an-existing-view"></a>Werken met een bestaande weergave
Wanneer u een weergave die is gemaakt met de ontwerpfunctie voor weergaven opent, heeft dit een menu met de opties in de volgende tabel.

![Overzicht van menu](media/log-analytics-view-designer/overview-menu.png)


| Optie | Beschrijving |
|:--|:--|
| Vernieuwen   | Vernieuw de weergave met de meest recente gegevens. | 
| Analytische gegevens | Open de [Advanced Analytics-portal](log-analytics-log-search-portals.md#advanced-analytics-portal) voor het analyseren van gegevens met logboek zoekopdrachten. () log-Analytics-log-Search-portals.MD#Advanced-Analytics-Portal). |
| Filteren    | Stel een tijdfilter voor de gegevens die zijn opgenomen in de weergave. |
| Bewerken      | Open de weergave in de Designer bekijken om te bewerken van de inhoud en configuratie.   |
| Kloon     | Maak een nieuwe weergave en open het in de ontwerpfunctie voor weergave.  De nieuwe weergave heeft dezelfde naam als het origineel met "kopiëren" toegevoegd aan het einde van deze. |


## <a name="create-a-new-view"></a>Een nieuwe weergave maken
Maak een nieuwe weergave in de **ontwerper** door te klikken op de tegel Designer bekijken op de overzichtspagina van de werkruimte voor logboekanalyse in de Azure portal.

![Designer-venster weergeven](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="working-with-view-designer"></a>Werken met de Designer bekijken
U werkt met Designer bekijken of u een nieuwe weergave maakt of een bestaande bewerkt.  

De ontwerper bestaat uit drie deelvensters.  De **ontwerp** deelvenster de aangepaste weergave bevat die u maakt of bewerkt.  U toevoegen tegels en delen van de **besturingselement** deelvenster naar de **ontwerp** deelvenster.  De **eigenschappen** deelvenster worden de eigenschappen voor de tegel of het geselecteerde onderdeel weergegeven.

![Designer weergeven](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Tegel configureren
Een aangepaste weergave kan slechts één tegel hebben.  Selecteer de **tegel** tabblad de **besturingselement** deelvenster op het huidige venster weergeven of Selecteer een alternatieve.  De **eigenschappen** deelvenster worden de eigenschappen voor het huidige venster weergegeven.  Configureer de eigenschappen van de tegel volgens de gedetailleerde informatie in de [tegel verwijzing](log-analytics-view-designer-tiles.md) en klik op **toepassen** wijzigingen op te slaan.

### <a name="configure-visualization-parts"></a>Visualisatie onderdelen configureren
Een weergave kan een onbeperkt aantal visualisatie onderdelen bevatten.  Selecteer de **weergave** tabblad en vervolgens een visualisatie onderdeel toevoegen aan de weergave.  De **eigenschappen** deelvenster worden de eigenschappen voor het geselecteerde onderdeel weergegeven.  Configureren van de weergave-eigenschappen volgens de gedetailleerde informatie in de [visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md) en klik op **toepassen** wijzigingen op te slaan.

Weergaven hebben slechts één rij met visualisatie delen.  Bestaande onderdelen in een weergave rangschikken door te klikken en slepen naar een nieuwe locatie.

U kunt een visualisatie deel verwijderen uit de weergave door te klikken op de **X** knop in de rechterbovenhoek van het onderdeel.


### <a name="menu-options"></a>Menuopties
Wanneer u met een weergave in de bewerkingsmodus werkt, hebt u de opties in de volgende tabel.

![Menu Bewerken](media/log-analytics-view-designer/edit-menu.png)

| Optie | Beschrijving |
|:--|:--|
| Opslaan        | Sla uw wijzigingen op en sluit de weergave. |
| Annuleren      | Uw wijzigingen negeren en de weergave te sluiten. |
| Weergave verwijderen | De weergave verwijderen. |
| Exporteren      | Exporteren van de weergave een [Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) die u kunt importeren in een andere werkruimte.  De naam van het bestand moet de naam van de weergave met de extensie *omsview*. |
| Importeren      | Importeer een *omsview* -bestand dat u hebt geëxporteerd uit een andere werkruimte.  Hiermee wordt de configuratie van de bestaande weergave overschreven. |
| Kloon       | Maak een nieuwe weergave en open het in de ontwerpfunctie voor weergave.  De nieuwe weergave heeft dezelfde naam als het origineel met "kopiëren" toegevoegd aan het einde van deze. |
| Publiceren     | De weergave exporteren naar een JSON-bestand die kan worden ingevoegd in een [Mangagement oplossing](../operations-management-suite/operations-management-suite-solutions-resources-views.md).  De naam van het bestand moet de naam van de weergave met de extensie *json*. Een tweede bestand is gemaakt met de extensie *resjson* die waarden voor resources die zijn gedefinieerd in het JSON-bestand bevat.

## <a name="next-steps"></a>Volgende stappen
* Voeg [tegels](log-analytics-view-designer-tiles.md) aan uw aangepaste weergave.
* Voeg [visualisatie delen](log-analytics-view-designer-parts.md) aan uw aangepaste weergave.
