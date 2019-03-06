---
title: Pijplijnen bewaken en beheren met behulp van de Azure-portal en PowerShell | Microsoft Docs
description: Informatie over het gebruik van Azure portal en Azure PowerShell om te controleren en beheren van de Azure data factory's en pijplijnen die u hebt gemaakt.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 64fae56bfc95b62bd60444d49100689845f64278
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57445140"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>Controleren en beheren van Azure Data Factory-pijplijnen met behulp van de Azure-portal en PowerShell
> [!div class="op_single_selector"]
> * [Met behulp van Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Met behulp van bewaking en beheer van app](data-factory-monitor-manage-app.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [controleren en beheren van Data Factory-pijplijnen in](../monitor-visually.md).

In dit artikel wordt beschreven hoe u bewaken, beheren en fouten opsporen in uw pijplijnen met behulp van Azure portal en PowerShell.

> [!IMPORTANT]
> De toepassing bewaking en beheer biedt een betere ondersteuning voor bewaking en beheer van uw gegevenspijplijnen en oplossen van problemen. Zie voor meer informatie over het gebruik van de toepassing [controleren en beheren van Data Factory-pijplijnen met behulp van de app bewaking en beheer](data-factory-monitor-manage-app.md). 

> [!IMPORTANT]
> Azure Data Factory versie 1 nu maakt gebruik van de nieuwe [Azure Monitor-waarschuwingen infrastructuur](../../monitoring-and-diagnostics/monitor-alerts-unified-usage.md). De oude waarschuwingen infrastructuur is afgeschaft. Als gevolg hiervan uw bestaande waarschuwingen geconfigureerd voor versie 1 data factory's niet meer werken. Uw bestaande waarschuwingen voor v1 data factory's worden niet automatisch gemigreerd. U moet deze waarschuwingen op de nieuwe waarschuwingen infrastructuur opnieuw maken. Meld u aan bij de Azure portal en selecteer **Monitor** nieuwe waarschuwingen over metrische gegevens (zoals mislukte uitvoeringen of geslaagde uitvoeringen) voor uw versie 1 data factory's maken.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="understand-pipelines-and-activity-states"></a>Pijplijnen en status van activiteiten
Met behulp van de Azure-portal, kunt u het volgende doen:

* Uw data factory als een diagram weergeven.
* Activiteiten bekijken in een pijplijn.
* Gegevenssets voor invoer en uitvoer weergeven.

Deze sectie beschrijft ook hoe een segment van de gegevensset van een status status verandert in een andere.   

### <a name="navigate-to-your-data-factory"></a>Navigeer naar uw data factory
1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Data Factory's** in het menu aan de linkerkant. Als u dit niet ziet, klikt u op **meer services >**, en klik vervolgens op **Data Factory's** onder de **INTELLIGENCE en analyse** categorie.

   ![Door alles bladeren > Data Factory's](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. Op de **Data Factory's** blade, selecteert u de data factory waarin u geïnteresseerd bent.

    ![Data factory selecteren](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   U ziet de startpagina van de data factory.

   ![Blade Data factory](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Diagramweergave van uw data factory
De **Diagram** weergave van een data factory biedt een enkel glazen bewaken en beheren van de data factory en bijbehorende assets. Om te zien de **Diagram** weergeven van uw data factory, klikt u op **Diagram** op de startpagina voor de data factory.

![Diagramweergave](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

U kunt inzoomen, uitzoomen, zoomen als u wilt aanpassen, zoomen naar 100%, de indeling van het diagram vergrendelen en automatische positie gebruiken voor pijplijnen en gegevenssets. U ziet ook de gegevens van de afkomst (dat wil zeggen, items upstream- en downstreamitems van geselecteerde items weergeven).

### <a name="activities-inside-a-pipeline"></a>Activiteiten binnen een pijplijn
1. Met de rechtermuisknop op de pijplijn en klik vervolgens op **pijplijn openen** om te zien van alle activiteiten in de pijplijn, samen met de invoer- en uitvoergegevenssets voor de activiteiten. Deze functie is handig als uw pijplijn vindt u meer dan één activiteit en u wilt weten over de operationele afkomst van één pijplijn.

    ![Menu Pijplijn openen](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. In het volgende voorbeeld ziet u een kopieeractiviteit in de pijplijn met een invoer en uitvoer. 

    ![Activiteiten binnen een pijplijn](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. U kunt navigeren naar de startpagina van de data factory door te klikken op de **Data factory** koppeling in het koppelingenmenu in de linkerbovenhoek.

    ![Ga terug naar data factory](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a>De status van elke activiteit binnen een pijplijn weergeven
U kunt de huidige status van een activiteit weergeven door de status van een van de gegevenssets die worden geproduceerd door de activiteit weer te geven.

Door te dubbelklikken op de **OutputBlobTable** in de **Diagram**, ziet u alle segmenten die worden geproduceerd door verschillende activiteiten bij uitvoering binnen een pijplijn. Kunt u zien dat de copy-activiteit is voor de afgelopen acht uur is uitgevoerd en die worden geproduceerd segmenten in de **gereed** staat.  

![Status van de pijplijn](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

De segmenten van de gegevensset in de data factory kunnen een van de volgende statussen hebben:

<table>
<tr>
    <th align="left">Status</th><th align="left">Substatus</th><th align="left">Description</th>
</tr>
<tr>
    <td rowspan="8">Wachten</td><td>ScheduleTime</td><td>De tijd is niet afkomstig zijn van het segment uit te voeren.</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>De upstream-afhankelijkheden zijn niet gereed.</td>
</tr>
<tr>
<td>ComputeResources</td><td>De compute-resources zijn niet beschikbaar.</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>Alle activiteitsinstanties zijn bezig met het uitvoeren van andere segmenten.</td>
</tr>
<tr>
<td>ActivityResume</td><td>De activiteit is onderbroken en segmenten kan niet worden uitgevoerd totdat de activiteit is hervat.</td>
</tr>
<tr>
<td>Opnieuw proberen</td><td>Uitvoering van activiteit wordt opnieuw geprobeerd.</td>
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
<td>Het segment wordt verwerkt.</td>
</tr>
<tr>
<td rowspan="4">Mislukt</td><td>TimedOut</td><td>De activiteit is uitgevoerd duurde langer dan is toegestaan door de activiteit.</td>
</tr>
<tr>
<td>Geannuleerd</td><td>Het segment is geannuleerd door gebruikersactie.</td>
</tr>
<tr>
<td>Validatie</td><td>De validatie is mislukt.</td>
</tr>
<tr>
<td>-</td><td>Het segment de status kan niet worden gegenereerd en/of gevalideerd.</td>
</tr>
<td>Gereed</td><td>-</td><td>Het segment is gereed voor gebruik.</td>
</tr>
<tr>
<td>Overgeslagen</td><td>Geen</td><td>Het segment wordt niet verwerkt.</td>
</tr>
<tr>
<td>Geen</td><td>-</td><td>Een segment gebruikt een andere status had, maar is ingesteld.</td>
</tr>
</table>



U kunt de details van een segment weergeven door te klikken op de vermelding van een segment in de **onlangs bijgewerkt segmenten** blade.

![Details gegevenssegment](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Als het segment is meerdere keren uitgevoerd, ziet u meerdere rijen in de **uitvoeringen van activiteit** lijst. U vindt meer informatie over een activiteit die wordt uitgevoerd door te klikken op de vermelding uitvoeren in de **uitvoeringen van activiteit** lijst. De lijst bevat de logboekbestanden, samen met een foutbericht weergegeven als er een. Deze functie is handig voor het weergeven en logboeken foutopsporing zonder te hoeven verlaten van uw gegevensfactory.

![Details uitvoering van activiteit](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Als het segment de status niet de **gereed** staat, ziet u de upstreamsegmenten die nog niet klaar bent en blokkeert de huidige status wordt uitgevoerd de **upstreamsegmenten die niet gereed** lijst. Deze functie is handig wanneer het segment de status **wachten** staat en u wilt weten over de upstream-afhankelijkheden die voor het segment ondernemen.

![Upstreamsegmenten die niet gereed zijn](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Gegevensset staat diagram
Nadat u een data factory implementeert en de pijplijnen een ongeldige actieve periode hebben, segmenten de gegevensset overgang naar de andere van de ene staat. Hier volgt het de segmentstatus op dit moment in het volgende diagram van de status:

![Status diagram](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

De gegevensset staat overgang stroom in data factory is het volgende: Wacht In-voortgang/In uitvoering (valideren) -> -> gereed/is mislukt.

Het segment wordt gestart in een **wachten** staat, wachten op voorwaarden waaraan moet worden voldaan voordat deze wordt uitgevoerd. Vervolgens wordt de activiteit wordt uitgevoerd en het segment krijgt een **In uitvoering** staat. De activiteit is uitgevoerd, kan slagen of mislukken. Het segment is gemarkeerd als **gereed** of **mislukt**, op basis van het resultaat van de uitvoering.

Kunt u het segment om terug te gaan van de status opnieuw instellen de **gereed** of **mislukt** status naar de **wachten** staat. U kunt ook het segment de status markeren **overslaan**, waardoor de activiteiten van het uitvoeren van en het segment niet verwerken.

## <a name="pause-and-resume-pipelines"></a>Onderbreken en hervatten van pijplijnen
U kunt uw pijplijnen beheren met behulp van Azure PowerShell. U kunt bijvoorbeeld onderbreken en hervatten van pijplijnen door het uitvoeren van Azure PowerShell-cmdlets. 

> [!NOTE] 
> De diagramweergave biedt geen ondersteuning voor het onderbreken en hervatten van pijplijnen. Als u gebruiken van een gebruikersinterface wilt, gebruikt u de toepassing bewaking en beheer. Zie voor meer informatie over het gebruik van de toepassing [controleren en beheren van Data Factory-pijplijnen met behulp van de app bewaking en beheer](data-factory-monitor-manage-app.md) artikel. 

U kunt onderbreken of onderbreken pijplijnen met behulp van de **stand-by-AzDataFactoryPipeline** PowerShell-cmdlet. Deze cmdlet is handig als u niet uitvoeren van uw pijplijnen wilt totdat een probleem is opgelost. 

```powershell
Suspend-AzDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Bijvoorbeeld:

```powershell
Suspend-AzDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Nadat het probleem is opgelost met de pijplijn, kunt u de onderbroken pijplijn kunt hervatten door de volgende PowerShell-opdracht uit:

```powershell
Resume-AzDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Bijvoorbeeld:

```powershell
Resume-AzDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Fouten opsporen in pijplijnen
Azure Data Factory biedt uitgebreide mogelijkheden voor u fouten opsporen en oplossen van pijplijnen met behulp van Azure portal en Azure PowerShell.

> [!NOTE] 
> Het is veel eenvoudiger daarvoor fouten met behulp van de App bewaking en beheer. Zie voor meer informatie over het gebruik van de toepassing [controleren en beheren van Data Factory-pijplijnen met behulp van de app bewaking en beheer](data-factory-monitor-manage-app.md) artikel. 

### <a name="find-errors-in-a-pipeline"></a>Fouten in een pijplijn te zoeken
Als de uitvoering van activiteit in een pijplijn mislukt, wordt de status van de gegevensset die wordt geproduceerd door de pijplijn is een foutstatus vanwege de fout. U kunt fouten opsporen en oplossen van fouten in Azure Data Factory met behulp van de volgende methoden.

#### <a name="use-the-azure-portal-to-debug-an-error"></a>De Azure portal gebruiken voor het fouten opsporen in een fout
1. Op de **tabel** blade, klikt u op het segment probleem waarvoor de **Status** ingesteld op **mislukt**.

   ![Blade tabel met probleem segment](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. Op de **gegevenssegment** blade, klikt u op het uitvoeren van de activiteit die is mislukt.

   ![Gegevenssegment met een fout](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. Op de **details uitvoering van activiteit** blade kunt u de bestanden die gekoppeld aan de verwerking voor HDInsight zijn downloaden. Klik op **downloaden** voor Status/stderr voor het downloaden van het foutenlogboekbestand met informatie over de fout.

   ![Blade met details met de fout de uitvoering van activiteiten](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a>PowerShell gebruiken voor het fouten opsporen in een fout
1. Start **PowerShell**.
2. Voer de **Get-AzDataFactorySlice** opdracht om te zien van de segmenten en hun status. Er is een segment met de status van **mislukt**.        

    ```powershell   
    Get-AzDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Bijvoorbeeld:

    ```powershell   
    Get-AzDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   Vervang **StartDateTime** met begintijd van uw pijplijn. 
3. Voer nu de **Get-AzDataFactoryRun** cmdlet voor meer informatie over de activiteit is uitgevoerd voor het segment.

    ```powershell   
    Get-AzDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Bijvoorbeeld:

    ```powershell   
    Get-AzDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    De waarde van StartDateTime is de begintijd voor het segment fout/probleem die u in de vorige stap hebt genoteerd. De datum / tijd moet tussen dubbele aanhalingstekens worden geplaatst.
4. Hier ziet u uitvoer met de details over de fout die vergelijkbaar is met het volgende:

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
5. U kunt uitvoeren de **opslaan AzDataFactoryLog** cmdlet met de id-waarde die u in de uitvoer zien en de logboekbestanden gedownload met behulp van de **- DownloadLogsoption** voor de cmdlet.

    ```powershell
    Save-AzDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Fouten in een pijplijn opnieuw uitvoeren

> [!IMPORTANT]
> Het is eenvoudiger oplossen van fouten en mislukte segmenten opnieuw met behulp van de App bewaking en beheer. Zie voor meer informatie over het gebruik van de toepassing [controleren en beheren van Data Factory-pijplijnen met behulp van de app bewaking en beheer](data-factory-monitor-manage-app.md). 

### <a name="use-the-azure-portal"></a>Azure Portal gebruiken
Nadat u problemen oplossen en opsporen van fouten in een pijplijn, kunt u fouten opnieuw uitvoeren door te klikken en te navigeren naar het segment de status fout de **uitvoeren** knop op de opdrachtbalk.

![Een mislukte segment opnieuw uitvoeren](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

In geval het segment heeft validatie is mislukt vanwege een fout bij het beleid (bijvoorbeeld als gegevens niet beschikbaar zijn), kunt u de fout corrigeren en validatie opnieuw uit door te klikken op de **valideren** knop op de opdrachtbalk.

![Corrigeer de fouten en valideren](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Azure PowerShell gebruiken
U kunt fouten opnieuw uitvoeren met behulp van de **Set AzDataFactorySliceStatus** cmdlet. Zie de [Set AzDataFactorySliceStatus](https://docs.microsoft.com/powershell/module/az.datafactory/set-azdatafactoryslicestatus) onderwerp voor de syntaxis en andere details over de cmdlet.

**Voorbeeld:**

Het volgende voorbeeld wordt de status van alle segmenten voor de tabel 'DAWikiAggregatedData' om te wachten in de Azure data factory 'WikiADF'.

De 'UpdateType' is ingesteld op 'UpstreamInPipeline', wat betekent dat de status van elk segment voor de tabel en alle afhankelijke (upstream) tabellen zijn ingesteld op 'Wachten'. De mogelijke waarde voor deze parameter is 'Afzonderlijk'.

```powershell
Set-AzDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```
## <a name="create-alerts-in-the-azure-portal"></a>Waarschuwingen maken in Azure portal

1.  Meld u aan bij de Azure portal en selecteer **Monitor -> waarschuwingen** om de pagina waarschuwingen te openen.

    ![Open de pagina met waarschuwingen.](media/data-factory-monitor-manage-pipelines/v1alerts-image1.png)

2.  Selecteer **+ nieuwe waarschuwingsregel** om een nieuwe waarschuwing te maken.

    ![Een nieuwe waarschuwing maken](media/data-factory-monitor-manage-pipelines/v1alerts-image2.png)

3.  Definieer de **waarschuwen voorwaarde**. (Zorg ervoor dat u selecteert **Data Factory's** in de **filteren op resourcetype** veld.) U kunt ook waarden opgeven voor **dimensies**.

    ![De waarschuwingsvoorwaarde - doel selecteren definiëren](media/data-factory-monitor-manage-pipelines/v1alerts-image3.png)

    ![De waarschuwing voorwaarde definiëren: waarschuwingscriteria toevoegen](media/data-factory-monitor-manage-pipelines/v1alerts-image4.png)

    ![De waarschuwing voorwaarde definiëren: waarschuwingslogica toevoegen](media/data-factory-monitor-manage-pipelines/v1alerts-image5.png)

4.  Definieer de **Waarschuwingsdetails**.

    ![De Waarschuwingsdetails definiëren](media/data-factory-monitor-manage-pipelines/v1alerts-image6.png)

5.  Definieer de **actiegroep**.

    ![De actiegroep definiëren: een nieuwe actiegroep maken](media/data-factory-monitor-manage-pipelines/v1alerts-image7.png)

    ![Definieer de actiegroep - eigenschappen instellen](media/data-factory-monitor-manage-pipelines/v1alerts-image8.png)

    ![Definieer de actiegroep - nieuwe actiegroep gemaakt](media/data-factory-monitor-manage-pipelines/v1alerts-image9.png)

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Een data factory verplaatsen naar een andere resourcegroep of abonnement
U kunt een data factory verplaatsen naar een andere resourcegroep of een ander abonnement met behulp van de **verplaatsen** opdracht balk knop op de startpagina van uw data factory.

![Data factory verplaatsen](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

U kunt ook alle gerelateerde resources (zoals berichten die gekoppeld aan de data factory zijn), samen met de data factory verplaatsen.

![In het dialoogvenster voor resources verplaatsen](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
