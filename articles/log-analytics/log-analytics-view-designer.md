---
title: Maken van weergaven voor het analyseren van gegevens in OMS Log Analytics | Microsoft Docs
description: Ontwerper in Log Analytics kunt u aangepaste weergaven maken die worden weergegeven in de OMS- en Azure-portal en bevatten verschillende visualisaties van gegevens in de OMS-opslagplaats. Dit artikel bevat een overzicht van Designer bekijken en procedures voor het maken en bewerken van aangepaste weergaven.
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
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: e3c463d749dc4179df58286b9bb75584880a6bc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-view-designer-to-create-custom-views-in-log-analytics"></a>Weergave Designer gebruiken voor het maken van aangepaste weergaven in Log Analytics
De ontwerper in [logboekanalyse](log-analytics-overview.md) kunt u aangepaste weergaven maken in de OMS-console die verschillende visualisaties van gegevens in de OMS-opslagplaats bevatten. Dit artikel bevat een overzicht van Designer bekijken en procedures voor het maken en bewerken van aangepaste weergaven.

Er zijn andere artikelen die beschikbaar zijn voor weergave Designer:

* [Tegel verwijzing](log-analytics-view-designer-tiles.md) -verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.
* [Visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md) -verwijzing van de instellingen voor elk van de tegels beschikbaar voor gebruik in aangepaste weergaven.

>[!NOTE]
> Als uw werkruimte is bijgewerkt naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md), en vervolgens de query's in alle weergaven moeten worden geschreven in de [nieuwe querytaal](https://go.microsoft.com/fwlink/?linkid=856078).  De weergaven die zijn gemaakt voordat de werkruimte werd bijgewerkt worden doorloopt geconverteerd.

## <a name="concepts"></a>Concepten
Weergaven maken met de ontwerpfunctie voor weergaven bevatten de elementen in de volgende tabel.

| Onderdeel | Beschrijving |
|:--- |:--- |
| Tegel |Op het hoofddashboard Log Analytics overzicht weergegeven.  Bevat een samenvatting van de informatie in de aangepaste weergave visual.  Verschillende typen van de tegel bieden verschillende visualisaties van records in de OMS-opslagplaats.  Klik op de tegel om de aangepaste weergave te openen. |
| Aangepaste weergave |Wanneer de gebruiker klikt op de tegel weergegeven.  Bevat een of meer visualisatie delen. |
| Visualisatie delen |Visualisatie van gegevens in de OMS-opslagplaats op basis van een of meer [Meld zoekopdrachten](log-analytics-log-searches.md).  De meeste onderdelen bevat een Header die een hoog niveau visualisatie bevat en een lijst van de bovenste resultaten.  Verschillende typen bieden verschillende visualisaties van records in de OMS-opslagplaats.  Klik op elementen in het gedeelte logboek zoekopdracht bieden gedetailleerde records. |

![Overzicht van de Designer](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Ontwerper toevoegen aan uw werkruimte
Hoewel Designer bekijken in de preview, moet u deze toevoegen aan uw werkruimte door te selecteren **Voorbeeldfuncties** in de **instellingen** sectie van de OMS-portal.

![Voorbeeld inschakelen](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Maken en bewerken van weergaven
### <a name="create-a-new-view"></a>Een nieuwe weergave maken
Open een nieuwe weergave in de **ontwerper** door te klikken op de tegel Designer bekijken in de belangrijkste OMS-dashboard.

![Designer-venster weergeven](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Een bestaande weergave bewerken
Een bestaande weergave in de ontwerpfunctie voor weergave bewerken, opent u de weergave door te klikken op de tegel in het belangrijkste OMS-dashboard.  Klik vervolgens op de **bewerken** te openen van de weergave in de ontwerpfunctie voor weergave.

![Een weergave bewerken](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klonen van een bestaande weergave
Wanneer u een weergave klonen, wordt een nieuwe weergave gemaakt en geopend in de ontwerpfunctie voor weergaven.  De nieuwe weergave heeft dezelfde naam als het origineel met "kopiëren" toegevoegd aan het einde van het.  Als u wilt klonen van een weergave, de bestaande weergave te openen door te klikken op de tegel in het belangrijkste OMS-dashboard.  Klik vervolgens op de **kloon** te openen van de weergave in de ontwerpfunctie voor weergave.

![Klonen van een weergave](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Een bestaande weergave verwijderen
Als u wilt verwijderen van een bestaande weergave, de weergave te openen door te klikken op de tegel in het belangrijkste OMS-dashboard.  Klik vervolgens op de **bewerken** klikken om de weergave in de ontwerpfunctie voor weergaven openen en klik op **weergave verwijderen**.

![Een weergave verwijderen](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Exporteren van een bestaande weergave
U kunt een weergave exporteren naar een JSON-bestand dat u kunt importeren in een andere werkruimte of een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-authoring-templates.md).  Als u wilt exporteren van een bestaande weergave, de weergave te openen door te klikken op de tegel in het belangrijkste OMS-dashboard.  Klik vervolgens op de **exporteren** om te maken van een bestand in de downloadmap de browser.  De naam van het bestand moet de naam van de weergave met de extensie *omsview*.

![Een weergave exporteren](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Importeren van een bestaande weergave
U kunt importeren een *omsview* -bestand dat u hebt geëxporteerd uit een andere beheergroep.  Voor het importeren van een bestaande weergave, moet u eerst een nieuwe weergave maken.  Klik vervolgens op de **importeren** en selecteer de *omsview* bestand.  De configuratie in het bestand wordt gekopieerd naar de bestaande weergave.

![Een weergave exporteren](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Werken met de Designer bekijken
De ontwerper bestaat uit drie deelvensters.  De **ontwerp** deelvenster vertegenwoordigt de aangepaste weergave.  Wanneer u toevoegt, tegels en delen van de **besturingselement** deelvenster naar de **ontwerp** deelvenster zijn toegevoegd aan de weergave.  De **eigenschappen** deelvenster worden de eigenschappen voor de tegel of het geselecteerde onderdeel weergegeven.

![Designer weergeven](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Tegel configureren
Een aangepaste weergave kan slechts één tegel hebben.  Selecteer de **tegel** tabblad de **besturingselement** deelvenster op het huidige venster weergeven of Selecteer een alternatieve.  De **eigenschappen** deelvenster worden de eigenschappen voor het huidige venster weergegeven.  Configureer de eigenschappen van de tegel volgens de gedetailleerde informatie in de [tegel verwijzing](log-analytics-view-designer-tiles.md) en klik op **toepassen** wijzigingen op te slaan.

### <a name="configure-visualization-parts"></a>Visualisatie onderdelen configureren
Een weergave kan een onbeperkt aantal visualisatie onderdelen bevatten.  Selecteer de **weergave** tabblad en vervolgens een visualisatie onderdeel toevoegen aan de weergave.  De **eigenschappen** deelvenster worden de eigenschappen voor het geselecteerde onderdeel weergegeven.  Configureren van de weergave-eigenschappen volgens de gedetailleerde informatie in de [visualisatie onderdeelverwijzing](log-analytics-view-designer-parts.md) en klik op **toepassen** wijzigingen op te slaan.

### <a name="delete-a-visualization-part"></a>U kunt een visualisatie deel verwijderen
U kunt een visualisatie deel verwijderen uit de weergave door te klikken op de **X** knop in de rechterbovenhoek van het onderdeel.

### <a name="rearrange-visualization-parts"></a>Visualisatie onderdelen rangschikken
Weergaven hebben slechts één rij met visualisatie delen.  Bestaande onderdelen in een weergave rangschikken door te klikken en slepen naar een nieuwe locatie.

## <a name="next-steps"></a>Volgende stappen
* Voeg [tegels](log-analytics-view-designer-tiles.md) aan uw aangepaste weergave.
* Voeg [visualisatie delen](log-analytics-view-designer-parts.md) aan uw aangepaste weergave.
