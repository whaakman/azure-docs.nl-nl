---
title: Bewaken en beheren van gegevenspijplijnen - Azure | Microsoft Docs
description: Informatie over het gebruik van de bewaking en beheer-app bewaken en beheren van Azure data factory's en pijplijnen.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: f3f07bc4-6dc3-4d4d-ac22-0be62189d578
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 0678e9bf6ea9e4161fc291729f1480ac7082796a
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-monitoring-and-management-app"></a>Bewaken en beheren van Azure Data Factory-pijplijnen met behulp van de app voor bewaking en beheer
> [!div class="op_single_selector"]
> * [Met behulp van Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Met bewaking en beheer-app](data-factory-monitor-manage-app.md)
>
>

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [bewaken en beheren van de Data Factory-pijplijnen in versie 2](../monitor-visually.md).

Dit artikel wordt beschreven hoe u met de app voor bewaking en beheer bewaken, beheren en fouten opsporen in uw Data Factory-pijplijnen. Het bevat ook informatie over het maken van waarschuwingen om u te waarschuwen op fouten. U kunt aan de slag met behulp van de toepassing door de volgende video bekijken:

> [!NOTE]
> De gebruikersinterface wordt weergegeven in de video kan niet exact overeen met wat u ziet in de portal. Het is enigszins oudere maar concepten hetzelfde blijven. 

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-Data-Factory-Monitoring-and-Managing-Big-Data-Piplines/player]
>

## <a name="launch-the-monitoring-and-management-app"></a>Start de bewaking en beheer-app
Start de app bewaken en beheren, klikt u op de **Monitor & beheren** tegel op de **Data Factory** blade van uw gegevensfactory.

![Bewaking van de tegel op de startpagina van de Data Factory](./media/data-factory-monitor-manage-app/MonitoringAppTile.png)

Hier ziet u de bewaking en beheer-app in een apart venster geopend.  

![App voor controle en beheer](./media/data-factory-monitor-manage-app/AppLaunched.png)

> [!NOTE]
> Als u ziet dat de webbrowser is vastgelopen bij 'Autoriseren...', schakelt u de **blokkeren van cookies van derden en sitegegevens** selectievakje-- of houd deze is geselecteerd, maakt u een uitzondering voor **login.microsoftonline.com**, en probeer de app opnieuw te openen.


In de lijst Windows activiteit in het middelste deelvenster ziet u een venster van de activiteit voor elke uitvoering van een activiteit. Als u de activiteit die is gepland hebt voor de per uur vijf uur uitgevoerd, ziet u bijvoorbeeld vijf activiteit windows die zijn gekoppeld aan vijf gegevenssegmenten. Als er geen activiteit windows in de lijst onderaan, het volgende doen:
 
- Update de **begintijd** en **eindtijd** filters bovenaan overeenkomstig de begin- en eindtijden van uw pijplijn, en klik vervolgens op de **toepassen** knop.  
- De lijst met Windows-activiteit is niet automatisch vernieuwd. Klik op de **vernieuwen** op de werkbalk in de **Activiteitsvensters** lijst.  

Als u een Data Factory-toepassing voor het testen van deze stappen met hebt, gaat u de zelfstudie: [gegevens kopiëren van Blob-opslag naar SQL Database met behulp van Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="understand-the-monitoring-and-management-app"></a>Overzicht van de bewaking en beheer-app
Er zijn drie tabbladen aan de linkerkant: **Resource Explorer**, **weergaven**, en **waarschuwingen**. Het eerste tabblad (**Resource Explorer**) is standaard geselecteerd.

### <a name="resource-explorer"></a>Resourceverkenner
U ziet het volgende:

* De Resource Explorer **structuurweergave** in het linkerdeelvenster.
* De **diagramweergave** boven in het middelste deelvenster.
* De **Activiteitsvensters** lijst onderaan in het middelste deelvenster.
* De **eigenschappen**, **activiteit venster Explorer**, en **Script** tabbladen in het rechterdeelvenster.

In Resource Explorer ziet u alle resources (pijplijnen, gegevenssets, gekoppelde services) in de gegevensfactory in een boomstructuur. Wanneer u een object selecteren in Resource Explorer:

* De gekoppelde Data Factory-entiteit is gemarkeerd in de diagramweergave.
* [Activiteit windows gekoppeld](data-factory-scheduling-and-execution.md) zijn gemarkeerd in de lijst met activiteiten Windows onderaan.  
* De eigenschappen van het geselecteerde object worden weergegeven in het venster Eigenschappen in het rechterdeelvenster.
* De JSON-definitie van het geselecteerde object wordt weergegeven, indien van toepassing. Bijvoorbeeld: een gekoppelde service, een dataset of een pijplijn.

![Resourceverkenner](./media/data-factory-monitor-manage-app/ResourceExplorer.png)

Zie de [planning en uitvoering](data-factory-scheduling-and-execution.md) artikel voor meer conceptuele informatie over windows activiteit.

### <a name="diagram-view"></a>Diagramweergave
De diagramweergave van een data factory biedt één glas bewaken en beheren van een gegevensfactory en de activa. Wanneer u een Data Factory-entiteit (gegevensset/pipeline) selecteert in de diagramweergave:

* De data factory-entiteit is in de structuurweergave geselecteerd.
* De bijbehorende activiteit windows zijn gemarkeerd in de lijst met Windows-activiteit.
* De eigenschappen van het geselecteerde object worden weergegeven in het venster Eigenschappen.

Wanneer de pijplijn (niet in een onderbroken status) is ingeschakeld, wordt deze weergegeven met een groene lijn:

![Pijplijn uitgevoerd](./media/data-factory-monitor-manage-app/PipelineRunning.png)

U kunt onderbreken, hervatten of een pipeline is beëindigd door te selecteren in de diagramweergave en met behulp van de knoppen op de opdrachtbalk.

![Onderbreken/hervatten op de opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)
 
Er zijn drie opdrachtbalkknoppen voor de pijplijn in de diagramweergave. De tweede knop kunt u de pijplijn te onderbreken. Onderbreken, de activiteiten die momenteel wordt uitgevoerd niet beëindigen en kan ze doorgaan voltooid. De derde knop pauzeert de pijplijn en beëindigt de bestaande activiteiten in uitvoering. De eerste knop hervat de pijplijn. Wanneer uw pijplijn wordt onderbroken, wordt de kleur van de pijplijn verandert. Bijvoorbeeld ziet een onderbroken pijplijn er in de volgende afbeelding: 

![Pijplijn onderbroken](./media/data-factory-monitor-manage-app/PipelinePaused.png)

Meervoudige selectie twee of meer pijplijnen kunt u met behulp van de Ctrl-toets. U kunt de knoppen op de opdrachtbalk onderbreken/hervatten meerdere pijplijnen tegelijk.

U kunt ook met de rechtermuisknop op een pijplijn en opties selecteren om te onderbreken, hervatten of een pipeline is beëindigd. 

![Contextmenu voor de pijplijn](./media/data-factory-monitor-manage-app/right-click-menu-for-pipeline.png)

Klik op de **pijplijn openen** optie voor een overzicht van alle activiteiten in de pijplijn. 

![Menu Pijplijn openen](./media/data-factory-monitor-manage-app/OpenPipelineMenu.png)

In de pijplijnweergave is geopend ziet u alle activiteiten in de pijplijn. In dit voorbeeld is er slechts één activiteit: Kopieeractiviteit. 

![Geopende pijplijn](./media/data-factory-monitor-manage-app/OpenedPipeline.png)

Als u wilt teruggaan naar de vorige weergave, klikt u op de naam van de gegevensfactory in het menu breadcrumb aan de bovenkant.

In de pipeline-weergave, wanneer u een uitvoergegevensset of wanneer u de muisaanwijzer over de uitvoergegevensset selecteert ziet u het Windows-activiteit pop-upvenster voor deze dataset.

![Activiteit Windows pop-upvenster](./media/data-factory-monitor-manage-app/ActivityWindowsPopup.png)

U kunt klikken op een venster van de activiteit voor details voor deze in de **eigenschappen** venster in het rechterdeelvenster.

![Eigenschappen van het venster activiteit](./media/data-factory-monitor-manage-app/ActivityWindowProperties.png)

In het rechterdeelvenster overschakelen naar de **activiteit venster Explorer** tabblad voor meer informatie.

![Activiteit venster Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer.png)

U ziet ook **opgelost variabelen** voor elke poging uitvoeren voor een activiteit in de **pogingen** sectie.

![Opgelost variabelen](./media/data-factory-monitor-manage-app/ResolvedVariables.PNG)

Overschakelen naar de **Script** tabblad voor een overzicht van de definitie van de JSON-script voor het geselecteerde object.   

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

Hier ziet u activiteitsvensters op drie locaties:

* De activiteit Windows-pop-upvenster in de diagramweergave (middelste deelvenster).
* De activiteit venster Explorer in het rechterdeelvenster.
* De lijst met Windows van activiteit in het onderste deelvenster.

In de Windows van de activiteit pop-upvenster en activiteit venster Explorer, kunt u naar de vorige week en de volgende week schuiven met de linker- en pijlen.

![Activiteit Explorer-venster horizontaal pijlen](./media/data-factory-monitor-manage-app/ActivityWindowExplorerLeftRightArrows.png)

Aan de onderkant van de diagramweergave ziet u deze knoppen: inzoomen, uitzoomen, zoomen passend maken, zoomen 100%, indeling vergrendelen. De **indeling vergrendelen** knop voorkomt u dat u per ongeluk tabellen en pijplijnen verplaatsen in de diagramweergave. Deze is standaard ingeschakeld. U kunt deze uitschakelen en entiteiten navigeren in het diagram. Wanneer u deze uitschakelen, kunt u de knop laatste plaatst automatisch tabellen en pijplijnen. U kunt ook in- of uitzoomen met behulp van het muiswiel.

![Diagram van weergave zoomopdrachten](./media/data-factory-monitor-manage-app/DiagramViewZoomCommands.png)

### <a name="activity-windows-list"></a>Lijst met Windows
De activiteit Windows lijst onderaan in het middelste deelvenster geeft alle activiteitsvensters voor de gegevensset die u hebt geselecteerd in de Resource Explorer of de diagramweergave. Standaard wordt de lijst is in aflopende volgorde, wat betekent dat u het venster van de meest recente activiteit boven ziet.

![Lijst met Windows](./media/data-factory-monitor-manage-app/ActivityWindowsList.png)

Deze lijst niet automatisch vernieuwd, zodat de knop Vernieuwen op de werkbalk gebruiken dit handmatig vernieuwen.  

Activiteit windows kunnen worden gebruikt in een van de volgende statussen:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Beschrijving</th>
</tr>
<tr>
    <td rowspan="8">Wachten</td><td>ScheduleTime</td><td>De tijd is niet afkomstig zijn van het activiteitvenster om uit te voeren.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>De upstream-afhankelijkheden zijn niet gereed.</td>
</tr>
<tr>
<td>ComputeResources</td><td>De rekenresources zijn niet beschikbaar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle activiteitsinstanties zijn bezig met andere windows activiteit.</td>
</tr>
<tr>
<td>ActivityResume</td><td>De activiteit is onderbroken en de windows activiteit kan niet worden uitgevoerd totdat deze hervat.</td>
</tr>
<tr>
<td>Opnieuw proberen</td><td>De uitvoering van de activiteit wordt opnieuw geprobeerd.</td>
</tr>
<tr>
<td>Validatie</td><td>Validatie is nog niet gestart.</td>
</tr>
<tr>
<td>ValidationRetry</td><td>Validatie wacht op opnieuw wordt uitgevoerd.</td>
</tr>
<tr>
<tr>
<td rowspan="2">Wordt uitgevoerd</td><td>Valideren</td><td>Validatie wordt uitgevoerd.</td>
</tr>
<td>-</td>
<td>Het activiteitvenster wordt verwerkt.</td>
</tr>
<tr>
<td rowspan="4">Mislukt</td><td>TimedOut</td><td>De activiteit is uitgevoerd duurde langer dan is toegestaan door de activiteit.</td>
</tr>
<tr>
<td>Geannuleerd</td><td>Het activiteitvenster is geannuleerd door in te grijpen.</td>
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
<td>Overgeslagen</td><td>-</td><td>Het activiteitvenster is niet verwerkt.</td>
</tr>
<tr>
<td>Geen</td><td>-</td><td>Het venster van een activiteit aan een andere status gebruikt, maar is opnieuw ingesteld.</td>
</tr>
</table>


Wanneer u een venster van de activiteit in de lijst klikt, ziet u details over deze in de **activiteit Windows Verkenner** of de **eigenschappen** venster aan de rechterkant.

![Activiteit venster Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-2.png)

### <a name="refresh-activity-windows"></a>Activiteit windows vernieuwen
De details worden niet automatisch vernieuwd, gebruikt u dus de knop Vernieuwen (de tweede knop) op de opdrachtbalk handmatig vernieuwen van de activiteit windows-lijst.  

### <a name="properties-window"></a>Eigenschappenvenster
Het venster Eigenschappen is in het deelvenster uiterst rechts van de app voor bewaking en beheer.

![Eigenschappenvenster](./media/data-factory-monitor-manage-app/PropertiesWindow.png)

Eigenschappen van het item dat u hebt geselecteerd in de Resource Explorer (structuurweergave), de diagramweergave of activiteit Windows lijst weergeven

### <a name="activity-window-explorer"></a>Activiteit venster Explorer
De **activiteit venster Explorer** venster is in het deelvenster uiterst rechts van de app voor bewaking en beheer. Meer informatie over het venster voor de activiteit die u hebt geselecteerd in het pop-upvenster activiteit Windows of de lijst met Windows activiteit wordt weergegeven.

![Activiteit venster Explorer](./media/data-factory-monitor-manage-app/ActivityWindowExplorer-3.png)

U kunt overschakelen naar een ander venster van de activiteit door erop te klikken in de kalenderweergave aan de bovenkant. U kunt ook de pijltjesknoppen pijl-links/rechts bovenin gebruiken om te zien van de activiteit windows uit de vorige week of de volgende week.

U kunt de knoppen op de werkbalk in het onderste deelvenster uitvoeren van het activiteitvenster of vernieuw de details in het deelvenster.

### <a name="script"></a>Script
U kunt de **Script** tabblad om de JSON-definitie van de geselecteerde entiteit Data Factory (gekoppelde service, gegevensset of pijplijn).

![Tabblad script](./media/data-factory-monitor-manage-app/ScriptTab.png)

## <a name="use-system-views"></a>Systeemweergaven gebruiken
De app voor bewaking en beheer bevat vooraf samengestelde systeemweergaven (**recente activiteit windows**, **activiteitsvensters is mislukt**, **windows activiteit In uitvoering**) waarmee u windows recente/mislukt/in uitvoering activiteit van uw gegevensfactory weergeven.

Overschakelen naar de **weergaven** tabblad aan de linkerkant door erop te klikken.

![Weergaven tabblad bewaking](./media/data-factory-monitor-manage-app/MonitoringViewsTab.png)

Er zijn momenteel drie systeemweergaven die worden ondersteund. Selecteer een optie voor een overzicht van recente activiteit windows, windows mislukte activiteit of activiteit in uitvoering windows in de lijst Activiteitsvensters (onderaan in het middelste deelvenster).

Wanneer u selecteert de **recente activiteit windows** optie, ziet u alle recente activiteit windows in aflopende volgorde van de **tijd laatste poging**.

U kunt de **activiteitsvensters is mislukt** weer te geven van alle mislukte activiteit windows in de lijst. Selecteer een mislukte activiteit-venster in de lijst voor informatie over het in de **eigenschappen** venster of de **activiteit venster Explorer**. U kunt ook alle logboeken voor een mislukte activiteitvenster downloaden.

## <a name="sort-and-filter-activity-windows"></a>Activiteitsvensters sorteren en filteren
Wijzig de **begintijd** en **eindtijd** instellingen in de opdrachtbalk filter activiteit windows. Nadat u de begintijd en eindtijd hebt gewijzigd, klikt u op de knop naast de eindtijd van activiteit Windows lijst met vernieuwen.

![Begin- en eindtijden](./media/data-factory-monitor-manage-app/StartAndEndTimes.png)

> [!NOTE]
> Alle tijden zijn momenteel in UTC-notatie in de app voor bewaking en beheer.
>
>

In de **Activiteitsvensters lijst**, klik op de naam van een kolom (bijvoorbeeld: Status).

![Menu kolom activiteit Windows-lijst](./media/data-factory-monitor-manage-app/ActivityWindowsListColumnMenu.png)

U kunt het volgende doen:

* Sorteren in oplopende volgorde.
* Sorteren in aflopende volgorde.
* Filteren op een of meer waarden (Ready, Waiting, enzovoort).

Als u een filter voor een kolom opgeven, ziet u de filterknop is ingeschakeld voor de kolom, waarmee wordt aangegeven dat de waarden in de kolom gefilterde waarden zijn.

![Filteren op een kolom van de activiteit Windows-lijst](./media/data-factory-monitor-manage-app/ActivityWindowsListFilterInColumn.png)

U kunt het hetzelfde pop-upvenster filters wissen. Schakel alle filters voor de activiteit Windows-lijst, klikt u op de knop filter wissen op de opdrachtbalk.

![Alle filters voor de activiteit Windows lijst wissen](./media/data-factory-monitor-manage-app/ClearAllFiltersActivityWindowsList.png)

## <a name="perform-batch-actions"></a>Batch-acties uitvoeren
### <a name="rerun-selected-activity-windows"></a>Geselecteerde activiteit windows opnieuw uitvoeren
Selecteer een activiteitvenster, klikt u op de pijl-omlaag voor de eerste knop en selecteer **opnieuw uitvoeren** / **opnieuw uitvoeren met upstream in pijplijn**. Wanneer u selecteert de **opnieuw uitvoeren met upstream in pijplijn** optie, evenals alle upstream-activiteit-vensters opnieuw worden uitgevoerd.
    ![Voer een venster van de activiteit](./media/data-factory-monitor-manage-app/ReRunSlice.png)

U kunt ook meerdere activiteitsvensters in de lijst selecteren en ze opnieuw uitgevoerd op hetzelfde moment. U kunt filteren op basis van de status activiteitsvensters (bijvoorbeeld: **mislukt**)-- en voer de mislukte activiteit windows na het corrigeren van het probleem dat ervoor zorgt de activiteit windows dat mislukken. Zie de volgende sectie voor meer informatie over het filteren van activiteit windows in de lijst.  

### <a name="pauseresume-multiple-pipelines"></a>Meerdere pijplijnen onderbreken/hervatten
Meervoudige selectie twee of meer pijplijnen kunt u met behulp van de Ctrl-toets. U kunt de knoppen op de opdrachtbalk (die zijn gemarkeerd in de rode rechthoek in de volgende afbeelding) te onderbreken/hervatten.

![Onderbreken/hervatten op de opdrachtbalk](./media/data-factory-monitor-manage-app/SuspendResumeOnCommandBar.png)

## <a name="create-alerts"></a>Waarschuwingen maken
De **waarschuwingen** pagina kunt u bij het maken van een waarschuwing en de bestaande waarschuwingen weergeven/bewerken/verwijderen. U kunt ook inschakelen/uitschakelen een waarschuwing. Klik op de pagina waarschuwingen overzicht van de **waarschuwingen** tabblad.

![Tabblad waarschuwingen](./media/data-factory-monitor-manage-app/AlertsTab.png)

### <a name="to-create-an-alert"></a>Een waarschuwing wilt maken
1. Klik op **waarschuwing toevoegen** een melding wilt toevoegen. U ziet de **Details** pagina.

    ![Waarschuwingen - pagina met Details maken](./media/data-factory-monitor-manage-app/CreateAlertDetailsPage.png)
2. Geef de **naam** en **beschrijving** voor de waarschuwing en klik op **volgende**. U ziet de **Filters** pagina.

    ![Waarschuwingen - pagina Filters maken](./media/data-factory-monitor-manage-app/CreateAlertFiltersPage.png)
3. Selecteer de **gebeurtenis**, **status**, en **substatus** (optioneel) dat u wilt maken van een Data Factory-service-waarschuwing voor, en klik op **volgende**. U ziet de **ontvangers** pagina.

    ![Waarschuwingen - ontvangers pagina maken](./media/data-factory-monitor-manage-app/CreateAlertRecipientsPage.png)
4. Selecteer de **e-abonnementsbeheerders** optie en/of geef een **extra beheerders-e**, en klik op **voltooien**. Hier ziet u de waarschuwing in de lijst.

    ![Lijst met waarschuwingen](./media/data-factory-monitor-manage-app/AlertsList.png)

Gebruik de knoppen die gekoppeld aan de waarschuwing te bewerken/verwijderen/inschakelen/uitschakelen een waarschuwing zijn in de lijst met waarschuwingen.

### <a name="eventstatussubstatus"></a>Status-gebeurtenis/substatus
De volgende tabel bevat de lijst met beschikbare gebeurtenissen en status (en substatus).

| De naam van gebeurtenis | Status | Substatus |
| --- | --- | --- |
| Activiteit die wordt uitgevoerd gestart |Gestart |Starting |
| Activiteit die wordt uitgevoerd is voltooid |Geslaagd |Geslaagd |
| Activiteit die wordt uitgevoerd is voltooid |Mislukt |Fout in de Resource-toewijzing<br/><br/>Mislukte uitvoering<br/><br/>Time-out opgetreden<br/><br/>De validatie is mislukt<br/><br/>Afgebroken |
| On-Demand HDI-Cluster maken gestart |Gestart |-|
| On-Demand HDI-Cluster is gemaakt |Geslaagd |-|
| On-Demand-HDI-Cluster is verwijderd |Geslaagd |-|

### <a name="to-edit-delete-or-disable-an-alert"></a>Als u wilt bewerken, verwijderen of uitschakelen van een waarschuwing

Gebruik de volgende knoppen (rood gemarkeerd) om te bewerken, verwijderen of uitschakelen van een waarschuwing.

![Waarschuwingen knoppen](./media/data-factory-monitor-manage-app/AlertButtons.png)
