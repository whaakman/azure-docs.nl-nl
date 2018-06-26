---
title: Maken van weergaven voor het analyseren van gegevens in Azure Log Analytics | Microsoft Docs
description: Ontwerp weergeven in Log Analytics gebruikt, kunt u aangepaste weergaven die worden weergegeven in de Azure portal en tal van gegevensvisualisaties in de werkruimte voor logboekanalyse bevatten. Dit artikel bevat een overzicht van Designer bekijken en geeft de procedures voor het maken en bewerken van aangepaste weergaven.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 91d4efcd7fabc2f284078d752ea68778a9bd8d86
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752036"
---
# <a name="create-custom-views-by-using-view-designer-in-log-analytics"></a>Aangepaste weergaven maken met behulp van de ontwerpfunctie voor weergaven in Log Analytics
Met behulp van de ontwerpfunctie voor weergaven in [Azure Log Analytics](log-analytics-overview.md), kunt u tal van aangepaste weergaven maken in de Azure-portal kunt u gegevens in de werkruimte voor logboekanalyse visualiseren. Dit artikel biedt een overzicht van Designer bekijken en procedures voor het maken en bewerken van aangepaste weergaven.

Zie voor meer informatie over Designer bekijken:

* [Tegel verwijzing](log-analytics-view-designer-tiles.md): biedt een Naslaggids voor de instellingen voor elk van de beschikbare tegels in aangepaste weergaven.
* [Visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md): biedt een Naslaggids voor de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.


## <a name="concepts"></a>Concepten
Weergaven worden weergegeven op de **overzicht** pagina van de werkruimte voor logboekanalyse in de Azure portal. De tegels in elke aangepaste weergave alfabetische volgorde worden weergegeven en de tegels voor de oplossingen die zijn geïnstalleerd dezelfde werkruimte.

![Overzichtspagina](media/log-analytics-view-designer/overview-page.png)

De weergaven die u met de ontwerpfunctie voor weergaven maakt bevatten de elementen die worden beschreven in de volgende tabel:

| Onderdeel | Beschrijving |
|:--- |:--- |
| Tegels | Worden weergegeven in de werkruimte voor logboekanalyse **overzicht** pagina. Elke tegel geeft een visuele samenvatting van de aangepaste weergave vertegenwoordigt. Elk tegeltype biedt een andere visualisatie van uw records. Selecteert u een tegel om een aangepaste weergave weer te geven. |
| Aangepaste weergave | Weergegeven wanneer u een tegel selecteert. Elke weergave bevat een of meer visualisatie delen. |
| Visualisatie delen | Een visualisatie van gegevens in de werkruimte voor logboekanalyse op basis van een of meer aanwezig [Meld zoekopdrachten](log-analytics-log-searches.md). De meeste onderdelen bevatten een kop, waarmee een visualisatie in op hoog niveau, en een lijst waarin de bovenste resultaten. Elk onderdeeltype biedt een andere visualisatie van de records in de werkruimte voor logboekanalyse. U selecteert de elementen in het onderdeel zoekopdracht logboek die voorziet in gedetailleerde records. |


## <a name="work-with-an-existing-view"></a>Werken met een bestaande weergave
Weergaven die zijn gemaakt met de ontwerpfunctie voor weergaven weergegeven de volgende opties:

![Overzicht van menu](media/log-analytics-view-designer/overview-menu.png)

De opties worden in de volgende tabel beschreven:

| Optie | Beschrijving |
|:--|:--|
| Vernieuwen   | Hiermee vernieuwt u de weergave met de meest recente gegevens. | 
| Analyse | Hiermee opent u de [Advanced Analytics-portal](log-analytics-log-search-portals.md#advanced-analytics-portal) voor het analyseren van gegevens met logboek zoekopdrachten. |
| Bewerken       | Hiermee opent u de weergave in de Designer bekijken om te bewerken van de inhoud en configuratie.  |
| Kloon      | Een nieuwe weergave maakt en geopend in de ontwerpfunctie voor weergaven. De naam van de nieuwe weergave is hetzelfde als de oorspronkelijke naam, maar met *kopie* hieraan toegevoegd. |
| Datumbereik | De datum en tijd bereikfilter instellen voor de gegevens die zijn opgenomen in de weergave. |
| +          | Een aangepast filter dat gedefinieerd voor de weergave definiëren. |


## <a name="create-a-new-view"></a>Een nieuwe weergave maken
U kunt een nieuwe weergave maken in Designer bekijken door te selecteren **ontwerper** in het menu van de werkruimte voor logboekanalyse.

![Designer-venster weergeven](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Werken met de Designer bekijken
U kunt ontwerper nieuwe weergaven maken of bestaande relaties te bewerken. 

Ontwerper bestaat uit drie deelvensters: 
* **Ontwerp**: de aangepaste weergave bevat die u maakt of bewerkt. 
* **Besturingselementen**: bevat de tegels en onderdelen die u toevoegt aan de **ontwerp** deelvenster. 
* **Eigenschappen**: geeft de eigenschappen van de tegels of de geselecteerde onderdelen.

![Designer weergeven](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>De tegel configureren
Een aangepaste weergave kan slechts één tegel hebben. Als u het huidige venster weergeven of Selecteer een alternatieve, selecteer de **tegel** tabblad de **besturingselement** deelvenster. De **eigenschappen** deelvenster ziet u de eigenschappen van het huidige venster. 

U kunt de tegel eigenschappen volgens de informatie in de [tegel verwijzing](log-analytics-view-designer-tiles.md) en klik vervolgens op **toepassen** de wijzigingen wilt opslaan.

### <a name="configure-the-visualization-parts"></a>De visualisatie-onderdelen configureren
Een weergave kan een onbeperkt aantal visualisatie onderdelen bevatten. Selecteer om onderdelen toe aan een weergave de **weergave** tabblad en selecteer vervolgens een visualisatie-onderdeel. De **eigenschappen** deelvenster worden de eigenschappen van het geselecteerde onderdeel weergegeven. 

U kunt de weergave-eigenschappen volgens de informatie in de [visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md) en klik vervolgens op **toepassen** de wijzigingen wilt opslaan.

Weergaven hebben slechts één rij visualisatie delen. U kunt de bestaande onderdelen rangschikken door ze naar een nieuwe locatie te slepen.

U kunt een visualisatie deel uit de weergave verwijderen door de **X** boven rechts van het onderdeel.


### <a name="menu-options"></a>Menuopties
De opties voor het werken met weergaven in de bewerkingsmodus zijn beschreven in de volgende tabel.

![Menu Bewerken](media/log-analytics-view-designer/edit-menu.png)

| Optie | Beschrijving |
|:--|:--|
| Opslaan        | De wijzigingen wilt opslaan en sluit u de weergave. |
| Annuleren      | Wijzigingen en sluit u de weergave. |
| Weergave verwijderen | Hiermee verwijdert u de weergave. |
| Exporteren      | De weergave exporteert een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) die u kunt importeren in een andere werkruimte. De naam van het bestand is de naam van de weergave en hieraan een *omsview* extensie. |
| Importeren      | Invoer de *omsview* -bestand dat u hebt geëxporteerd uit een andere werkruimte. Deze actie wordt de configuratie van de bestaande weergave overschreven. |
| Kloon       | Een nieuwe weergave maakt en geopend in de ontwerpfunctie voor weergaven. De naam van de nieuwe weergave is hetzelfde als de oorspronkelijke naam, maar met *kopie* hieraan toegevoegd. |

## <a name="next-steps"></a>Volgende stappen
* Voeg [tegels](log-analytics-view-designer-tiles.md) aan uw aangepaste weergave.
* Voeg [visualisatie delen](log-analytics-view-designer-parts.md) aan uw aangepaste weergave.
