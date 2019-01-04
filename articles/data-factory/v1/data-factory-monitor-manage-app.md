---
title: Bewaken en beheren van gegevenspijplijnen - Azure | Microsoft Docs
description: Meer informatie over het gebruik van de app bewaking en beheer om te controleren en beheren van Azure data factory's en pijplijnen.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 5b70edd4f65538b52c70881258bc500a34b04d80
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54025416"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a>Controleren en beheren van Azure Data Factory-pijplijnen met behulp van de app bewaking en beheer
> [!div class="op_single_selector"]
> * [Met behulp van Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Met behulp van bewaking en beheer van app](data-factory-monitor-manage-app.md)
>
>

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [controleren en beheren van Data Factory-pijplijnen in](../monitor-visually.md).

In dit artikel wordt beschreven hoe u de app bewaking en beheer gebruiken om te controleren, beheren en fouten opsporen in uw Data Factory-pijplijnen. U kunt aan de slag met behulp van de toepassing door de volgende video bekijken:

> [!NOTE]
> De gebruikersinterface wordt weergegeven in de video kan niet precies overeenkomen met wat u ziet in de portal. Het is enigszins oudere, maar concepten hetzelfde blijven. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a>Start de bewaking en beheer van app
U start de app controleren en beheren, klikt u op de **bewaking en beheer** tegel op de **Data Factory** blade voor uw data factory.

![Bewaking van de tegel op de Data Factory-startpagina](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

Hier ziet u de bewaking en beheer-app in een apart venster geopend.  

![App voor controle en beheer](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Als u ziet dat de webbrowser is vastgelopen bij "Autoriseren...", schakelt u de **blokkeren van cookies van derden en sitegegevens** selectievakje-- of houd deze is geselecteerd, maakt u een uitzondering voor **login.microsoftonline.com**, en Probeer het vervolgens naar de app opnieuw opent.


In de lijst de Windows-activiteit in het middelste deelvenster ziet u een activiteitsvenster voor elke uitvoering van een activiteit. Als u beschikt over de activiteit die is gepland om te worden elk uur gedurende vijf uur wordt uitgevoerd, ziet u bijvoorbeeld vijf activiteitsvensters die zijn gekoppeld aan vijf gegevenssegmenten. Als er geen activiteit windows in de lijst onderaan, het volgende doen:
 
- Update de **begintijd** en **eindtijd** filters aan de bovenkant overeenkomstig de begin- en eindtijden van uw pijplijn, en klik vervolgens op de **toepassen** knop.  
- De activiteit Windows-lijst wordt niet automatisch vernieuwd. Klik op de **vernieuwen** op de werkbalk in de **activiteit Windows** lijst.  

Als u een Data Factory-toepassing voor het testen van deze stappen niet hebt, gaat u de zelfstudie: [gegevens kopiëren van Blob Storage naar SQL Database met behulp van Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-the-monitoring-and-management-app"></a>Informatie over de bewaking en beheer van app
Aan de linkerkant ziet u drie tabbladen: **Resource Explorer**, **Monitoring-weergaven**, en **waarschuwingen**. Het eerste tabblad (**Resource Explorer**) is standaard geselecteerd.

### <a name="resource-explorer"></a>Resourceverkenner
U ziet het volgende:

* De Resource Explorer **structuurweergave** in het linkerdeelvenster.
* De **diagramweergave** boven in het middelste deelvenster.
* De **activiteit Windows** lijst onderaan in het middelste deelvenster.
* De **eigenschappen**, **Activiteitsvensterverkenner**, en **Script** tabbladen in het rechter deelvenster.

In Resource Explorer ziet u alle resources (pijplijnen, gegevenssets, gekoppelde services) in de data factory in een structuurweergave wordt weergegeven. Wanneer u een object selecteert in Resource Explorer:

* De gekoppelde Data Factory-entiteit is gemarkeerd in de diagramweergave.
* [Dat is gekoppeld activiteitsvensters](data-factory-scheduling-and-execution.md) zijn gemarkeerd in de lijst activiteit Windows aan de onderkant.  
* De eigenschappen van het geselecteerde object worden weergegeven in het venster Eigenschappen in het rechter deelvenster.
* De JSON-definitie van het geselecteerde object wordt weergegeven, indien van toepassing. Bijvoorbeeld: een gekoppelde service, een gegevensset of een pijplijn.

![Resourceverkenner](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Zie de [planning en uitvoering](data-factory-scheduling-and-execution.md) artikel voor gedetailleerde informatie over activiteitsvensters.

### <a name="diagram-view"></a>Diagramweergave
De diagramweergave van een data factory biedt een enkel glazen bewaken en beheren van een data factory en bijbehorende assets. Wanneer u een Data Factory-entiteit (gegevensset/pipeline) selecteert in de diagramweergave:

* De data factory-entiteit is geselecteerd in de structuurweergave wordt weergegeven.
* De bijbehorende activiteit windows zijn gemarkeerd in de lijst met Windows-activiteit.
* De eigenschappen van het geselecteerde object worden weergegeven in het venster Eigenschappen.

Wanneer de pijplijn is ingeschakeld (niet in de status onderbroken ingeschakeld), wordt deze weergegeven met een groene lijn:

![Pijplijn uitvoeren](./media/data-factory-monitor-manage-app/PipelineRunning.png)

U kunt onderbreken, hervatten of beëindigen van een pijplijn door te selecteren in de diagramweergave en het gebruik van de knoppen op de opdrachtbalk.

![Onderbreken/hervatten in de opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Er zijn drie opdrachtbalkknoppen voor de pijplijn in de diagramweergave. U kunt de tweede knop gebruiken om te onderbreken van de pijplijn. Onderbreken van de activiteiten die momenteel wordt uitgevoerd niet beëindigen en kunt deze gaat u verder met de voltooiing. De derde knop onderbreekt de pijplijn en beëindigt de bestaande activiteiten uitvoeren. De eerste knop hervat de pijplijn. Wanneer uw pijplijn wordt onderbroken, verandert de kleur van de pijplijn. Bijvoorbeeld een onderbroken pijplijn ziet er uit zoals in de volgende afbeelding: 

![Pijplijn onderbroken](./media/data-factory-monitor-manage-app/PipelinePaused.png)

U kunt meervoudige selectie twee of meer pijplijnen met behulp van de Ctrl-toets. U kunt de knoppen op de opdrachtbalk onderbreken/hervatten meerdere pijplijnen op een tijdstip.

U kunt ook met de rechtermuisknop op een pijplijn en selecteer opties om te onderbreken, hervatten of beëindigen van een pijplijn. 

![Contextmenu voor pijplijn](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Klik op de **pijplijn openen** optie om te zien van alle activiteiten in de pijplijn. 

![Menu Pijplijn openen](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

In de geopende pijplijnweergave ziet u alle activiteiten in de pijplijn. In dit voorbeeld is er slechts één activiteit: Activiteit kopiëren. 

![Geopende pijplijn](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

Als u wilt teruggaan naar de vorige weergave, klikt u op de naam van de data factory in het koppelingenmenu bovenaan.

In de pijplijnweergave, wanneer u een uitvoergegevensset of wanneer u de muisaanwijzer over de uitvoergegevensset, ziet u het Windows van de activiteit pop-upvenster voor deze gegevensset.

![Activiteit Windows pop-upvenster](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

U kunt klikken op een activiteitsvenster om details te bekijken voor het in de **eigenschappen** venster in het rechter deelvenster.

![Eigenschappen van activiteit](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

Ga in het rechter deelvenster naar de **Activiteitsvensterverkenner** tabblad om meer details te bekijken.

![Activiteitsvensterverkenner](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

U ziet ook **opgelost variabelen** voor elke poging uitgevoerd voor een activiteit in de **pogingen** sectie.

![Variabelen voor de opgelost](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Schakel over naar de **Script** tabblad om te bekijken van de JSON-script-definitie voor het geselecteerde object.   

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

Hier ziet u activiteitsvensters op drie locaties:

* De activiteit Windows-pop-upvenster in de diagramweergave (middelste deelvenster).
* De Activiteitsvensterverkenner in het rechter deelvenster.
* De activiteit Windows-lijst in het onderste deelvenster.

In de Windows van de activiteit pop-upvenster en Activiteitsvensterverkenner, kunt u naar de vorige week en de volgende week schuiven met behulp van de linker- en pijlen.

![Activiteit Explorer-venster links en rechts/pijlen](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Aan de onderkant van de diagramweergave ziet u deze knoppen: Inzoomen, uitzoomen, zoomen passend maken, Inzoomen tot 100%, indeling vergrendelen. De **indeling vergrendelen** knop voorkomt dat u per ongeluk het verplaatsen van tabellen en pijplijnen in de diagramweergave. Dit is standaard. U kunt deze uitschakelen en entiteiten navigeren in het diagram. Wanneer u deze functie uitschakelt, kunt u de knop laatste aan automatische positie gebruiken voor tabellen en pijplijnen. U kunt ook in- of uitzoomen met het muiswiel.

![Diagram weergeven zoomopdrachten](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>Lijst Activiteitsvensters
De activiteit Windows-lijst aan de onderkant van het middelste deelvenster geeft alle activiteitsvensters voor de gegevensset die u hebt geselecteerd in de Resource Explorer of de diagramweergave weer. Standaard is de lijst in aflopende volgorde, wat betekent dat u ziet dat de meest recente activiteitvenster aan de bovenkant.

![Lijst Activiteitsvensters](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Deze lijst niet automatisch wordt vernieuwd, zodat de vernieuwknop op de werkbalk gebruiken dit handmatig vernieuwen.  

Activiteit windows, kunnen zich in een van de volgende statussen:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Description</th>
</tr>
<tr>
    <td rowspan="8">Wachten</td><td>ScheduleTime</td><td>De tijd is niet afkomstig zijn van het activiteitvenster om uit te voeren.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>De upstream-afhankelijkheden zijn niet gereed.</td>
</tr>
<tr>
<td>ComputeResources</td><td>De compute-resources zijn niet beschikbaar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle activiteitsinstanties zijn bezig met het uitvoeren van andere activiteitsvensters.</td>
</tr>
<tr>
<td>ActivityResume</td><td>De activiteit is onderbroken en de activiteitsvensters kan niet worden uitgevoerd totdat deze wordt hervat.</td>
</tr>
<tr>
<td>Opnieuw proberen</td><td>Er wordt opnieuw geprobeerd de activiteit is uitgevoerd.</td>
</tr>
<tr>
<td>Validatie</td><td>Validatie is nog niet gestart.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Validatie is in afwachting opnieuw geprobeerd.</td>
</tr>
<tr>
<tr>
<td rowspan="2">Wordt uitgevoerd</td><td>Valideren</td><td>Validatie wordt uitgevoerd.</td>
</tr>
<td>-</td>
<td>Het activiteitvenster wordt verwerkt.</td>
</tr>
<tr>
<td rowspan="4">Mislukt</td><td>Time-out</td><td>De activiteit is uitgevoerd duurde langer dan is toegestaan door de activiteit.</td>
</tr>
<tr>
<td>Geannuleerd</td><td>Het venster van de activiteit is geannuleerd door gebruikersactie.</td>
</tr>
<tr>
<td>Validatie</td><td>De validatie is mislukt.</td>
</tr>
<tr>
<td>-</td><td>Het activiteitvenster kan niet worden gegenereerd of gevalideerd.</td>
</tr>
<td>Gereed</td><td>-</td><td>Het activiteitvenster is gereed voor gebruik.</td>
</tr>
<tr>
<td>Overgeslagen</td><td>-</td><td>Het venster van de activiteit is niet verwerkt.</td>
</tr>
<tr>
<td>Geen</td><td>-</td><td>Een activiteitsvenster met een andere status had, maar is opnieuw ingesteld.</td>
</tr>
</table>


Wanneer u een activiteitsvenster in de lijst klikt, ziet u details hierover deze in de **activiteit Windows Explorer** of de **eigenschappen** venster aan de rechterkant.

![Activiteitsvensterverkenner](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Activiteit windows vernieuwen
De details worden niet automatisch vernieuwd, Gebruik daarom de vernieuwknop (de tweede knop) op de opdrachtbalk aan de lijst activiteitsvensters handmatig vernieuwen.  

### <a name="properties-window"></a>Eigenschappenvenster
Het venster Eigenschappen is in het deelvenster uiterst rechts van de app bewaking en beheer.

![Eigenschappenvenster](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Eigenschappen van het item dat u hebt geselecteerd in de Resource Explorer (structuurweergave wordt weergegeven), de diagramweergave of de activiteit Windows-lijst wordt weergegeven.

### <a name="activity-window-explorer"></a>Activiteitsvensterverkenner
De **Activiteitsvensterverkenner** venster is in het deelvenster uiterst rechts van de app bewaking en beheer. Meer informatie over het venster van de activiteit die u hebt geselecteerd in het pop-upvenster activiteit Windows of de activiteit Windows-lijst wordt weergegeven.

![Activiteitsvensterverkenner](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

U kunt overschakelen naar een ander venster van de activiteit door erop te klikken in de kalenderweergave aan de bovenkant. U kunt ook de pijltjesknoppen pijl-links/rechts boven gebruiken om te zien activiteitsvensters uit de vorige week of de volgende week.

U kunt de knoppen op de werkbalk in het onderste deelvenster activiteitsvenster opnieuw of vernieuw de gegevens in het deelvenster.

### <a name="script"></a>Script
U kunt de **Script** tabblad om de JSON-definitie van de geselecteerde Data Factory-entiteiten (gekoppelde service, gegevensset of pijplijn) weer te geven.

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Systeemweergaven gebruiken
De app bewaking en beheer bevat vooraf gemaakte systeemweergaven (**recente activiteit windows**, **activiteitsvensters mislukt**, **activiteitsvensters In uitvoering**) waarmee u hebt om recente/mislukt/in uitvoering activiteitsvensters voor uw gegevensfactory weer te geven.

Schakel over naar de **weergaven** tabblad aan de linkerkant door erop te klikken.

![Weergaven tabblad controle](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Er zijn momenteel drie systeemweergaven die worden ondersteund. Selecteer een optie om te zien van recente activiteit windows, windows mislukte activiteit of activiteitsvensters wordt uitgevoerd in de lijst activiteit Windows (aan de onderkant van het middelste deelvenster).

Wanneer u selecteert de **recente activiteitsvensters** optie, ziet u alle recente activiteitsvensters in aflopende volgorde van de **tijd laatste poging**.

U kunt de **activiteitsvensters mislukt** weer te geven van alle mislukte activiteit windows in de lijst. Selecteer een mislukte activiteit-venster in de lijst voor informatie over het in de **eigenschappen** venster of de **Activiteitsvensterverkenner**. U kunt ook alle logboeken voor een mislukte activiteitvenster downloaden.

## <a name="sort-and-filter-activity-windows"></a>Sorteren en filteren activiteitsvensters
Wijzig de **begintijd** en **eindtijd** instellingen in de opdrachtbalk om te filteren activiteitsvensters. Nadat u de begintijd en eindtijd wijzigen, klikt u op de knop naast de eindtijd van de activiteit Windows-lijst te vernieuwen.

![Begin- en eindtijden](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> Alle tijden zijn op dit moment in de UTC-indeling in de app bewaking en beheer.
>
>

In de **activiteit Windows lijst**, klikt u op de naam van een kolom (bijvoorbeeld: Status).

![Menu kolom activiteit Windows-lijst](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

U kunt het volgende doen:

* De gegevens gesorteerd in oplopende volgorde.
* De gegevens gesorteerd in aflopende volgorde.
* Filteren op een of meer waarden (Ready, Waiting, enzovoort).

Wanneer u een filter voor een kolom opgeven, ziet u de filterknop is ingeschakeld voor die kolom, dat geeft aan dat de waarden in de kolom gefilterde waarden zijn.

![Filteren op een kolom van de activiteit Windows-lijst](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

U kunt de dezelfde pop-upvenster filters wissen. Schakel alle filters voor de activiteit Windows-lijst, klikt u op de knop filter wissen op de opdrachtbalk.

![Alle filters voor de activiteit Windows-lijst wissen](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Batch-acties uitvoeren
### <a name="rerun-selected-activity-windows"></a>Geselecteerde activiteit windows opnieuw uitvoeren
Selecteer een activiteitsvenster, klikt u op de pijl-omlaag voor de eerste knop op de opdrachtbalk en selecteer **opnieuw uitvoeren** / **opnieuw uitvoeren met upstream in pijplijn**. Wanneer u selecteert de **opnieuw uitvoeren met upstream in pijplijn** optie, alle upstream-activiteit windows ook opnieuw worden uitgevoerd.
    ![Opnieuw uitvoeren van een activiteitsvenster](./media/data-factory-monitor-manage-app/ReRunSlice.png)

U kunt ook meerdere activiteitsvensters in de lijst selecteren en ze opnieuw uitvoeren op hetzelfde moment. U kunt filteren op basis van de status activiteitsvensters (bijvoorbeeld: **Kan geen**)-- en voer de mislukte activiteit windows na het corrigeren van het probleem dat ervoor zorgt de activiteitsvensters dat mislukken. Zie de volgende sectie voor meer informatie over het filteren van activiteitsvensters in de lijst.  

### <a name="pauseresume-multiple-pipelines"></a>Meerdere pijplijnen onderbreken/hervatten
U kunt meervoudige selectie twee of meer pijplijnen met behulp van de Ctrl-toets. U kunt de knoppen op de opdrachtbalk (die zijn gemarkeerd in de rode rechthoek in de volgende afbeelding) ze onderbreken/hervatten.

![Onderbreken/hervatten in de opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
