---
title: Weergaven voor het analyseren van gegevens in Azure Log Analytics maken | Microsoft Docs
description: Met behulp van ontwerper in Log Analytics, kunt u aangepaste weergaven die worden weergegeven in de Azure-portal en bevatten een verscheidenheid aan visualisaties van zoekgegevens in de Log Analytics-werkruimte maken. In dit artikel bevat een overzicht van de Weergaveontwerper en biedt procedures voor het maken en bewerken van aangepaste weergaven.
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
ms.component: na
ms.openlocfilehash: 77c7d9a549b10a56faaf6226062d0e6212ce19c0
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42057584"
---
# <a name="create-custom-views-by-using-view-designer-in-log-analytics"></a>Aangepaste weergaven maken met behulp van de Weergaveontwerper in Log Analytics
Met behulp van de Weergaveontwerper in [Azure Log Analytics](log-analytics-overview.md), kunt u tal van aangepaste weergaven maken in Azure portal kunt u visualiseren van gegevens in uw Log Analytics-werkruimte. In dit artikel geeft een overzicht van de Weergaveontwerper en procedures voor het maken en bewerken van aangepaste weergaven.

Zie voor meer informatie over Designer bekijken:

* [Tegel verwijzing](log-analytics-view-designer-tiles.md): biedt een Naslaggids voor de instellingen voor elk van de beschikbare tegels in uw aangepaste weergaven.
* [Naslaginformatie voor visualisatieonderdelen](log-analytics-view-designer-parts.md): biedt een Naslaggids voor de instellingen voor de visualisatie-onderdelen die beschikbaar in uw aangepaste weergaven zijn.


## <a name="concepts"></a>Concepten
Weergaven worden weergegeven op de **overzicht** pagina van uw Log Analytics-werkruimte in de Azure-portal. De tegels in elke aangepaste weergave op alfabetische volgorde worden weergegeven, en de tegels voor de oplossingen die zijn geïnstalleerd dezelfde werkruimte.

![Overzichtspagina](media/log-analytics-view-designer/overview-page.png)

De weergaven die u met de Weergaveontwerper maakt bevatten de elementen die worden beschreven in de volgende tabel:

| Onderdeel | Beschrijving |
|:--- |:--- |
| Tegels | Worden weergegeven in uw Log Analytics-werkruimte **overzicht** pagina. Elke tegel geeft een visueel overzicht van de aangepaste weergave vertegenwoordigt. Elk tegeltype biedt een andere visualisatie van uw records. U selecteert een tegel om een aangepaste weergave weer te geven. |
| Aangepaste weergave | Weergegeven wanneer u een tegel selecteert. Elke weergave bevat een of meer onderdelen van de visualisatie. |
| Visualisatie delen | Een visualisatie van gegevens in de Log Analytics-werkruimte op basis van een of meer presenteren [zoekopdrachten](log-analytics-log-searches.md). De meeste onderdelen zijn een koptekst, waarmee u een visualisatie op hoog niveau, en een lijst waarin de belangrijkste resultaten worden weergegeven. Elk onderdeeltype biedt een andere visualisatie van de records in de Log Analytics-werkruimte. U selecteren elementen in het gedeelte om uit te voeren van een logboekzoekopdracht die gedetailleerde records bevat. |


## <a name="work-with-an-existing-view"></a>Werken met een bestaande weergave
Weergaven die zijn gemaakt met de Weergaveontwerper weergegeven de volgende opties:

![Menu overzicht](media/log-analytics-view-designer/overview-menu.png)

De opties worden in de volgende tabel beschreven:

| Optie | Beschrijving |
|:--|:--|
| Vernieuwen   | Hiermee vernieuwt u de weergave met de meest recente gegevens. | 
| Analyse | Hiermee opent u de [portal Advanced Analytics](log-analytics-log-search-portals.md) voor het analyseren van gegevens met Logboeken-query's. |
| Bewerken       | Hiermee opent de weergave in de Designer bekijken om te bewerken van de inhoud en configuratie.  |
| Kloon      | Hiermee maakt u een nieuwe weergave en wordt deze geopend in de ontwerper. De naam van de nieuwe weergave is hetzelfde als de oorspronkelijke naam, maar met *kopie* hieraan toegevoegd. |
| Datumbereik | De datum en tijd bereikfilter instellen voor de gegevens die zijn opgenomen in de weergave. |
| +          | Definieer een aangepaste filter dat gedefinieerd voor de weergave. |


## <a name="create-a-new-view"></a>Een nieuwe weergave maken
U kunt een nieuwe weergave maken in de Designer bekijken door te selecteren **Weergaveontwerper** in het menu van uw Log Analytics-werkruimte.

![Designer tegel niet weergeven](media/log-analytics-view-designer/view-designer-tile.png)


## <a name="work-with-view-designer"></a>Werken met Weergaveontwerper
U kunt Weergaveontwerper nieuwe weergaven maken of bestaande relaties bewerken. 

Weergaveontwerper heeft drie deelvensters: 
* **Ontwerp**: de aangepaste weergave bevat die u maakt of bewerkt. 
* **Besturingselementen**: bevat de tegels en delen die u toevoegt aan de **ontwerp** deelvenster. 
* **Eigenschappen**: geeft de eigenschappen van de tegels of de geselecteerde onderdelen.

![Designer weergeven](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-the-view-tile"></a>Configureer de tegel weergeven
Een aangepaste weergave kan slechts één tegel hebben. Als u wilt de huidige tegel niet weergeven of Selecteer een alternatieve, selecteer de **tegel** tabblad de **besturingselement** deelvenster. De **eigenschappen** deelvenster geeft de eigenschappen van het huidige venster. 

U kunt de tegel eigenschappen op basis van de informatie in de [tegel verwijzing](log-analytics-view-designer-tiles.md) en klik vervolgens op **toepassen** de wijzigingen op te slaan.

### <a name="configure-the-visualization-parts"></a>De visualisatie-onderdelen configureren
Een weergave kan een onbeperkt aantal visualisatie onderdelen bevatten. Als u wilt delen naar een weergave toevoegen, selecteert u de **weergave** tabblad, en selecteer vervolgens een visualisatie-onderdeel. De **eigenschappen** deelvenster worden de eigenschappen van het geselecteerde onderdeel weergegeven. 

U kunt de weergave-eigenschappen op basis van de informatie in de [naslaginformatie voor Visualisatieonderdelen](log-analytics-view-designer-parts.md) en klik vervolgens op **toepassen** de wijzigingen op te slaan.

Weergaven hebben slechts één rij van de visualisatie delen. U kunt de bestaande onderdelen opnieuw rangschikken door ze naar een nieuwe locatie te slepen.

U kunt het onderdeel van een visualisatie verwijderen uit de weergave door te selecteren de **X** aan de bovenkant van het onderdeel.


### <a name="menu-options"></a>Menu-Opties
De opties voor het werken met weergaven in de bewerkingsmodus worden beschreven in de volgende tabel.

![Menu Bewerken](media/log-analytics-view-designer/edit-menu.png)

| Optie | Beschrijving |
|:--|:--|
| Opslaan        | Uw wijzigingen worden opgeslagen en sluit u de weergave. |
| Annuleren      | Wijzigingen en sluit u de weergave. |
| Weergave verwijderen | Hiermee verwijdert u de weergave. |
| Exporteren      | Hiermee exporteert u de weergave voor een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md) die u kunt importeren in een andere werkruimte. De naam van het bestand is de naam van de weergave en er een *omsview* extensie. |
| Importeren      | Invoer de *omsview* -bestand dat u hebt geëxporteerd uit een andere werkruimte. Deze actie wordt de configuratie van de bestaande weergave overschreven. |
| Kloon       | Hiermee maakt u een nieuwe weergave en wordt deze geopend in de ontwerper. De naam van de nieuwe weergave is hetzelfde als de oorspronkelijke naam, maar met *kopie* hieraan toegevoegd. |

## <a name="next-steps"></a>Volgende stappen
* Voeg [tegels](log-analytics-view-designer-tiles.md) aan uw aangepaste weergave.
* Voeg [visualisatie delen](log-analytics-view-designer-parts.md) aan uw aangepaste weergave.
