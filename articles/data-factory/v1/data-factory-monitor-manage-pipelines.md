---
title: Bewaken en beheren van pijplijnen met behulp van de Azure-portal en PowerShell | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal en Azure PowerShell om te controleren en beheren van de Azure data factory's en pijplijnen die u hebt gemaakt.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 94b3c1e812bdf3345d5fb1f7308fb7a55be8f922
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>Bewaken en beheren van Azure Data Factory-pijplijnen met behulp van de Azure-portal en PowerShell
> [!div class="op_single_selector"]
> * [Met behulp van Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Met bewaking en beheer-app](data-factory-monitor-manage-app.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [bewaken en beheren van de Data Factory-pijplijnen in versie 2](../monitor-visually.md).

> [!IMPORTANT]
> De toepassing voor bewaking en beheer biedt een betere ondersteuning voor bewaking en het beheren van uw gegevenspijplijnen en het oplossen van problemen. Zie voor meer informatie over het gebruik van de toepassing [bewaken en beheren van Data Factory-pijplijnen met behulp van de app voor bewaking en beheer](data-factory-monitor-manage-app.md). 


In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in uw pijplijnen met behulp van Azure-portal en PowerShell.

## <a name="understand-pipelines-and-activity-states"></a>Pijplijnen en activiteiten statussen begrijpen
U kunt met behulp van de Azure-portal:

* Uw data factory als een diagram weergeven.
* Activiteiten in een pijplijn weergeven.
* Invoer- en uitvoergegevenssets weergeven.

Deze sectie beschrijft ook hoe een gegevensset-segment van de ene staat status verandert in een andere.   

### <a name="navigate-to-your-data-factory"></a>Navigeer naar uw data factory
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **gegevensfactory** in het menu aan de linkerkant. Als u deze niet ziet, klikt u op **meer services >**, en klik vervolgens op **gegevensfactory** onder de **INTELLIGENCE en analyse** categorie.

   ![Door alle Bladeren > gegevensfactory's](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. Op de **gegevensfactory** blade, selecteert u de gegevensfactory die u geïnteresseerd bent in.

    ![Data factory selecteren](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   U ziet de startpagina van de gegevensfactory.

   ![Blade Data factory](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Diagramweergave van uw data factory
De **Diagram** weergave van een data factory biedt één glas bewaken en beheren van de gegevensfactory en de activa. Om te zien de **Diagram** van uw gegevensfactory weergeven, klikt u op **Diagram** op de startpagina voor de data factory.

![Diagramweergave](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

U kunt inzoomen, uitzoomen, zoomen als u wilt aanpassen, zoomen naar 100%, de indeling van het diagram vergrendelen en automatische positie gebruiken voor pijplijnen en gegevenssets. U ziet ook de gegevens van de afkomst (dat wil zeggen items upstream- en downstreamitems van geselecteerde items weergeven).

### <a name="activities-inside-a-pipeline"></a>Activiteiten binnen een pijplijn
1. Met de rechtermuisknop op de pijplijn en klik vervolgens op **pijplijn openen** voor een overzicht van alle activiteiten in de pijplijn, samen met invoer- en uitvoergegevenssets voor de activiteiten. Deze functie is handig als uw pijplijn meer dan één activiteit bevat en u wilt weten over de operationele afkomst van één pijplijn.

    ![Menu Pijplijn openen](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. In het volgende voorbeeld ziet u een kopieeractiviteit in de pijplijn met een invoer en uitvoer. 

    ![Activiteiten binnen een pijplijn](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. U kunt Ga terug naar de startpagina van de gegevensfactory door te klikken op de **gegevensfactory** koppeling in het koppelingenmenu op de linkerbovenhoek.

    ![Ga terug naar de data factory](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a>De status van elke activiteit in een pijplijn weergeven
U kunt de huidige status van een activiteit weergeven door de status van elk van de gegevenssets die worden geproduceerd door de activiteit weer te geven.

Door te dubbelklikken op de **OutputBlobTable** in de **Diagram**, ziet u alle segmenten die worden geproduceerd door andere activiteiten bij uitvoering binnen een pijplijn. U kunt zien dat met de kopieerbewerking is voor de afgelopen acht uur is uitgevoerd en die wordt geproduceerd segmenten van de **gereed** status.  

![Status van de pijplijn](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

De gegevensset segmenten in de gegevensfactory, kunnen een van de volgende statussen hebben:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Beschrijving</th>
</tr>
<tr>
    <td rowspan="8">Wachten</td><td>ScheduleTime</td><td>De tijd is niet geleverd voor het segment uit te voeren.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>De upstream-afhankelijkheden zijn niet gereed.</td>
</tr>
<tr>
<td>ComputeResources</td><td>De rekenresources zijn niet beschikbaar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle activiteitsinstanties zijn bezig met het uitvoeren van andere segmenten.</td>
</tr>
<tr>
<td>ActivityResume</td><td>De activiteit is onderbroken en segmenten kan niet worden uitgevoerd totdat de activiteit is hervat.</td>
</tr>
<tr>
<td>Opnieuw proberen</td><td>Er wordt opnieuw geprobeerd activiteit is uitgevoerd.</td>
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
<td>Het segment wordt verwerkt.</td>
</tr>
<tr>
<td rowspan="4">Mislukt</td><td>TimedOut</td><td>De activiteit is uitgevoerd duurde langer dan is toegestaan door de activiteit.</td>
</tr>
<tr>
<td>Geannuleerd</td><td>Het segment is geannuleerd door in te grijpen.</td>
</tr>
<tr>
<td>Validatie</td><td>De validatie is mislukt.</td>
</tr>
<tr>
<td>-</td><td>Het segment kan niet worden gegenereerd en/of gevalideerd.</td>
</tr>
<td>Gereed</td><td>-</td><td>Het segment is gereed voor gebruik.</td>
</tr>
<tr>
<td>Overgeslagen</td><td>Geen</td><td>Het segment wordt niet verwerkt.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Een segment wordt gebruikt voor te komen met een andere status, maar is teruggezet.</td>
</tr>
</table>



U kunt de details over een segment weergeven door te klikken op een segment vermelding in de **segmenten onlangs bijgewerkt** blade.

![Details van het segment](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Als het segment is meerdere keren uitgevoerd, ziet u meerdere rijen in de **activiteit wordt uitgevoerd** lijst. U kunt details zien over een activiteit die wordt uitgevoerd door te klikken op de vermelding uitvoeren in de **activiteit wordt uitgevoerd** lijst. De lijst bevat alle logboekbestanden, samen met een foutbericht als er een. Deze functie is handig om te bekijken en logboeken voor foutopsporing zonder te hoeven verlaten van uw gegevensfactory.

![Details uitvoering van activiteit](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Als het segment niet in de **gereed** staat, ziet u de upstreamsegmenten die nog niet klaar en blokkeren van de huidige status wordt uitgevoerd de **upstreamsegmenten die niet gereed** lijst. Deze functie is handig als uw segment **wachten** status en u wilt weten over de upstream-afhankelijkheden die het segment wacht op.

![Upstreamsegmenten die niet gereed zijn](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>DataSet staat diagram
Nadat u een gegevensfactory implementeert en pijplijnen een ongeldige actieve periode hebben, segmenten de gegevensset overgang naar een andere van de ene staat. De status van het segment volgt op dit moment is het volgende diagram van de status:

![Diagram van status](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

De stroom voor overgang status gegevensset in gegevensfactory is het volgende: wachten-In-voortgang/In uitvoering (Validating) > -> gereed/mislukt.

Het segment wordt gestart in een **wachten** staat, wacht voorwaarden moet worden voldaan voordat deze wordt uitgevoerd. Vervolgens wordt de activiteit wordt uitgevoerd en het segment probeert een **In uitvoering** status. De activiteit is uitgevoerd, kan slagen of mislukken. Het segment is gemarkeerd als **gereed** of **mislukt**, op basis van het resultaat van de uitvoering.

U kunt opnieuw instellen van het segment om terug te gaan van de **gereed** of **mislukt** status naar de **wachten op** status. U kunt ook het segment de status markeren **overslaan**, waarmee wordt voorkomen dat de activiteit wordt uitgevoerd en het segment niet verwerken.

## <a name="pause-and-resume-pipelines"></a>Onderbreken en hervatten pijplijnen
U kunt uw pijplijnen beheren met behulp van Azure PowerShell. U kunt bijvoorbeeld onderbreken en hervatten van pijplijnen door het uitvoeren van Azure PowerShell-cmdlets. 

> [!NOTE] 
> De diagramweergave biedt geen ondersteuning voor het onderbreken en hervatten pijplijnen. Als u gebruiken een gebruikersinterface wilt, moet u de controle- en het beheer van toepassing gebruiken. Zie voor meer informatie over het gebruik van de toepassing [bewaken en beheren van Data Factory-pijplijnen met behulp van de app voor bewaking en beheer](data-factory-monitor-manage-app.md) artikel. 

U kunt onderbreken/uitstellen pijplijnen met behulp van de **onderbreken AzureRmDataFactoryPipeline** PowerShell-cmdlet. Deze cmdlet is handig als u niet dat uw pijplijnen uitvoeren wilt totdat er een probleem is opgelost. 

```powershell
Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Bijvoorbeeld:

```powershell
Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Nadat het probleem is opgelost met de pijplijn, kunt u de onderbroken pijplijn hervatten door met de volgende PowerShell-opdracht:

```powershell
Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Bijvoorbeeld:

```powershell
Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Fouten opsporen in pijplijnen
Azure Data Factory biedt uitgebreide mogelijkheden voor u opsporen en oplossen van pijplijnen met behulp van de Azure portal en Azure PowerShell.

> [! Opmerking} is het veel eenvoudiger daarvoor fouten met de bewaking en beheer-App. Zie voor meer informatie over het gebruik van de toepassing [bewaken en beheren van Data Factory-pijplijnen met behulp van de app voor bewaking en beheer](data-factory-monitor-manage-app.md) artikel. 

### <a name="find-errors-in-a-pipeline"></a>Fouten gevonden in een pijplijn
Als de uitvoering van activiteit in een pijplijn uitvalt, wordt de status van de gegevensset die wordt geproduceerd door de pijplijn is in een foutstatus vanwege de fout. U kunt fouten opsporen en oplossen van fouten in Azure Data Factory met behulp van de volgende methoden.

#### <a name="use-the-azure-portal-to-debug-an-error"></a>De Azure portal gebruiken om op te sporen een fout opgetreden
1. Op de **tabel** blade, klikt u op het segment probleem met de **Status** ingesteld op **mislukt**.

   ![Blade tabel met probleem segment](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. Op de **gegevenssegment** blade, klikt u op het uitvoeren van de activiteit die is mislukt.

   ![Gegevenssegment met een fout](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. Op de **details uitvoering van activiteit** blade kunt u de bestanden die gekoppeld aan de verwerking van HDInsight zijn downloaden. Klik op **downloaden** voor Status/stderr voor het downloaden van het foutenlogboekbestand met informatie over de fout.

   ![Activiteit blade toewijzingdetails uitvoeren met de fout](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a>PowerShell gebruiken om fouten opsporen in een fout opgetreden
1. Start **PowerShell**.
2. Voer de **Get-AzureRmDataFactorySlice** opdracht om te zien van de segmenten en hun status. U ziet een segment met de status van **mislukt**.        

    ```powershell   
    Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Bijvoorbeeld:

    ```powershell   
    Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Vervang **StartDateTime** met begintijd van de pijplijn. 
3. Voer nu de **Get-AzureRmDataFactoryRun** cmdlet voor meer informatie over de activiteit is uitgevoerd voor het segment.

    ```powershell   
    Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Bijvoorbeeld:

    ```powershell   
    Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    De waarde van StartDateTime is de begintijd voor het segment fout/probleem die u uit de vorige stap hebt genoteerd. De datum-tijd moet tussen dubbele aanhalingstekens worden geplaatst.
4. Hier ziet u uitvoer met meer informatie over de fout die vergelijkbaar is met het volgende:

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. U kunt uitvoeren de **opslaan AzureRmDataFactoryLog** cmdlet uit met de id-waarde die u in de uitvoer zien en downloaden van de logboekbestanden met behulp van de **- DownloadLogsoption** voor de cmdlet.

    ```powershell
    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Fouten in een pijplijn opnieuw uitvoeren

> [!IMPORTANT]
> Het is eenvoudiger fouten oplossen en mislukte segmenten met behulp van de bewaking en beheer-App opnieuw. Zie voor meer informatie over het gebruik van de toepassing [bewaken en beheren van Data Factory-pijplijnen met behulp van de app voor bewaking en beheer](data-factory-monitor-manage-app.md). 

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken
Nadat u problemen oplossen en het opsporen van fouten in een pijplijn, kunt u fouten opnieuw uitvoeren door te navigeren naar het segment van de fout en te klikken op de **uitvoeren** knop op de opdrachtbalk.

![Een mislukte segment opnieuw uitvoeren](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

In geval het segment is mislukt vanwege een fout van beleid (bijvoorbeeld als gegevens niet beschikbaar is), kunt u de fout oplossen en validatie opnieuw uit door te klikken op de **valideren** knop op de opdrachtbalk.

![Corrigeer de fouten en valideren](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
U kunt fouten opnieuw uitvoeren met behulp van de **Set-AzureRmDataFactorySliceStatus** cmdlet. Zie de [Set-AzureRmDataFactorySliceStatus](https://msdn.microsoft.com/library/mt603522.aspx) onderwerp voor de syntaxis en andere details over de cmdlet.

**Voorbeeld:**

Het volgende voorbeeld wordt de status van alle segmenten voor de tabel 'DAWikiAggregatedData' om te wachten in de Azure-gegevensfactory 'WikiADF'.

'UpdateType' is ingesteld op 'UpstreamInPipeline', wat betekent dat de status van elk segment voor de tabel en alle afhankelijke (upstream) tabellen zijn ingesteld op 'Wachten'. De mogelijke waarde voor deze parameter is 'Afzonderlijk'.

```powershell
Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Een gegevensfactory verplaatsen naar een andere resourcegroep of abonnement
U kunt een gegevensfactory verplaatsen naar een andere resourcegroep of een ander abonnement met behulp van de **verplaatsen** opdracht balk knop op de startpagina van uw gegevensfactory.

![Verplaatsen van de gegevensfactory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

U kunt ook alle gerelateerde resources (zoals waarschuwingen die gekoppeld aan de gegevensfactory zijn), samen met de data factory verplaatsen.

![Dialoogvenster resources verplaatsen](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
