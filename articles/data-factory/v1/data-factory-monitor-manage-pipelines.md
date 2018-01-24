---
title: Bewaken en beheren van pijplijnen met behulp van de Azure-portal en PowerShell | Microsoft Docs
description: Informatie over het gebruik van de Azure-portal en Azure PowerShell om te controleren en beheren van de Azure data factory's en pijplijnen die u hebt gemaakt.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 9b0fdc59-5bbe-44d1-9ebc-8be14d44def9
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: d9e7b1d020a99e939ea01c43c7e5e935188b212e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>Bewaken en beheren van Azure Data Factory-pijplijnen met behulp van de Azure-portal en PowerShell
> [!div class="op_single_selector"]
> * [Met behulp van Azure portal/Azure PowerShell](data-factory-monitor-manage-pipelines.md)
> * [Met bewaking en beheer-app](data-factory-monitor-manage-app.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [bewaken en beheren van de Data Factory-pijplijnen in versie 2](../monitor-visually.md).

> [!IMPORTANT]
> De toepassing voor bewaking en beheer biedt een betere ondersteuning voor bewaking en het beheren van uw gegevenspijplijnen en het oplossen van problemen. Zie voor meer informatie over het gebruik van de toepassing [bewaken en beheren van Data Factory-pijplijnen met behulp van de app voor bewaking en beheer](data-factory-monitor-manage-app.md). 


In dit artikel wordt beschreven hoe bewaken, beheren en fouten opsporen in uw pijplijnen met behulp van Azure-portal en PowerShell. Het artikel bevat ook informatie over het maken van waarschuwingen en blijf op de hoogte over mislukte.

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

   ![Blade gegevensfactory](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

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
<td>Geen</td><td>-</td><td>Een segment wordt gebruikt voor te komen met een andere status, maar is teruggezet.</td>
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

## <a name="create-alerts"></a>Waarschuwingen maken
Azure logboeken gebruikersgebeurtenissen als een Azure-resource (bijvoorbeeld een gegevensfactory) wordt gemaakt, bijgewerkt of verwijderd. U kunt waarschuwingen maken van deze gebeurtenissen. Data Factory kunt u verschillende metrische gegevens vastleggen en waarschuwingen maken op de metrische gegevens. U wordt aangeraden dat u gebeurtenissen voor realtime-controle en metrische gegevens worden gebruikt voor historische doeleinden.

### <a name="alerts-on-events"></a>Waarschuwingen over gebeurtenissen
Gebeurtenissen van Azure bieden handig inzicht in wat in uw Azure-resources gebeurt er. Wanneer u van Azure Data Factory gebruikmaakt, gebeurtenissen worden gegenereerd wanneer:

* Een gegevensfactory is gemaakt, bijgewerkt of verwijderd.
* Verwerken van gegevens (als 'wordt uitgevoerd') is gestart of voltooid.
* Een HDInsight-cluster op aanvraag wordt gemaakt of verwijderd.

U kunt waarschuwingen van deze gebruikersgebeurtenissen maken en configureren voor het e-mailmeldingen verzenden naar de beheerder en coadministrators van het abonnement. Bovendien kunt u aanvullende e-mailadressen van gebruikers die toegang e-mailmeldingen ontvangen moeten wanneer de voorwaarden wordt voldaan. Deze functie is handig als u wilt Blijf op de hoogte op fouten en niet moet worden bewaakt continu uw gegevensfactory.

> [!NOTE]
> De portal wordt niet op dit moment kunnen waarschuwingen worden weergegeven op gebeurtenissen. Gebruik de [app voor bewaking en beheer](data-factory-monitor-manage-app.md) om alle waarschuwingen te zien.


#### <a name="specify-an-alert-definition"></a>Geef een definitie van een waarschuwing
Geef een definitie van een waarschuwing, moet u een JSON-bestand met een beschrijving van de bewerkingen die u worden gewaarschuwd wilt op maken. In het volgende voorbeeld wordt met de waarschuwing een e-mailmelding voor de bewerking RunFinished verzendt. Als u specifieke, een e-mailmelding wordt verzonden wanneer een uitvoering in de gegevensfactory is voltooid en de uitvoering is mislukt (Status = FailedExecution).

```JSON
{
    "contentVersion": "1.0.0.0",
     "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters": {},
    "resources":
    [
        {
            "name": "ADFAlertsSlice",
            "type": "microsoft.insights/alertrules",
            "apiVersion": "2014-04-01",
            "location": "East US",
            "properties":
            {
                "name": "ADFAlertsSlice",
                "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                "isEnabled": true,
                "condition":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                    "dataSource":
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                        "operationName": "RunFinished",
                        "status": "Failed",
                        "subStatus": "FailedExecution"   
                    }
                },
                "action":
                {
                    "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails": [ "<your alias>@contoso.com" ]
                }
            }
        }
    ]
}
```

U kunt verwijderen **subStatus** van de JSON-definitie als u niet wilt worden gewaarschuwd op een specifieke fout.

In dit voorbeeld is ingesteld van de waarschuwing voor alle data Factory in uw abonnement. Als u wilt dat de waarschuwing worden ingesteld voor een bepaalde gegevensfactory, kunt u data factory **resourceUri** in de **dataSource**:

```JSON
"resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"
```

De volgende tabel bevat de lijst met beschikbare bewerkingen en statussen (en substatus).

| Bewerkingsnaam | Status | Substatus |
| --- | --- | --- |
| RunStarted |Gestart |Starting |
| RunFinished |Kan niet / is voltooid |FailedResourceAllocation<br/><br/>Geslaagd<br/><br/>FailedExecution<br/><br/>TimedOut<br/><br/>< geannuleerd<br/><br/>FailedValidation<br/><br/>Afgebroken |
| OnDemandClusterCreateStarted |Gestart | |
| OnDemandClusterCreateSuccessful |Geslaagd | |
| OnDemandClusterDeleted |Geslaagd | |

Zie [waarschuwingsregel maken](https://msdn.microsoft.com/library/azure/dn510366.aspx) voor meer informatie over de JSON-elementen die worden gebruikt in het voorbeeld.

#### <a name="deploy-the-alert"></a>De waarschuwing implementeren
Voor het implementeren van de waarschuwing, gebruikt u de Azure PowerShell-cmdlet **New-AzureRmResourceGroupDeployment**, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  
```

Nadat de resource-group-implementatie is voltooid, ziet u de volgende berichten:

```
VERBOSE: 7:00:48 PM - Template is valid.
WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
Please update scripts to remove this parameter.
VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded

DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

> [!NOTE]
> U kunt de [waarschuwingsregel maken](https://msdn.microsoft.com/library/azure/dn510366.aspx) REST-API om een waarschuwingsregel te maken. De JSON-nettolading is vergelijkbaar met het JSON-voorbeeld.  


#### <a name="retrieve-the-list-of-azure-resource-group-deployments"></a>De lijst met Azure resourcegroepimplementaties ophalen
Gebruik de cmdlet voor het ophalen van de lijst met geïmplementeerde Azure resourcegroepimplementaties **Get-AzureRmResourceGroupDeployment**, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
Get-AzureRmResourceGroupDeployment -ResourceGroupName adf
```

```
DeploymentName    : ADFAlertFailedSlice
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 10/11/2014 2:01:00 AM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           :
```

#### <a name="troubleshoot-user-events"></a>Oplossen van gebruikersgebeurtenissen
1. U kunt zien dat alle gebeurtenissen die worden gegenereerd wanneer u op de **metrische gegevens en bewerkingen** tegel.

    ![Tegel metrische gegevens en bewerkingen](./media/data-factory-monitor-manage-pipelines/metrics-and-operations-tile.png)
2. Klik op de **gebeurtenissen** tegel om de gebeurtenissen te bekijken.

    ![Tegel gebeurtenissen](./media/data-factory-monitor-manage-pipelines/events-tile.png)
3. Op de **gebeurtenissen** blade ziet u informatie over gebeurtenissen en gefilterde gebeurtenissen.

    ![Blade gebeurtenissen](./media/data-factory-monitor-manage-pipelines/events-blade.png)
4. Klik op een **bewerking** in de lijst met bewerkingen die een fout veroorzaakt.

    ![Een bewerking selecteren](./media/data-factory-monitor-manage-pipelines/select-operation.png)
5. Klik op een **fout** gebeurtenis voor informatie over de fout.

    ![Fout van gebeurtenis](./media/data-factory-monitor-manage-pipelines/operation-error-event.png)

Zie [inzicht van Azure-cmdlets](https://msdn.microsoft.com/library/mt282452.aspx) ophalen of verwijderen van waarschuwingen voor PowerShell-cmdlets die u gebruiken kunt om toe te voegen. Hier volgen enkele voorbeelden van het gebruik van de **Get-AlertRule** cmdlet:

```powershell
get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
```

```
Properties :
Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
Condition   :
DataSource :
EventName             :
Category              :
Level                 :
OperationName         : RunFinished
ResourceGroupName     :
ResourceProviderName  :
ResourceId            :
Status                : Failed
SubStatus             : FailedExecution
Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
Condition      :
Description : One or more of the data slices for the Azure Data Factory has failed processing.
Status      : Enabled
Name:       : ADFAlertsSlice
Tags       :
$type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
Location   : West US
Name       : ADFAlertsSlice
```

```powershell
Get-AlertRule -res $resourceGroup
```
```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0

Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
Location   : West US
Name       : FailedExecutionRunsWest3
```

```powershell
Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
```

```
Properties : Microsoft.Azure.Management.Insights.Models.Rule
Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
Location   : West US
Name       : FailedExecutionRunsWest0
```

Voer de volgende opdrachten get-help om te zien, details en voorbeelden van de cmdlet Get-AlertRule.

```powershell
get-help Get-AlertRule -detailed
```

```powershell
get-help Get-AlertRule -examples
```


Als u de gebeurtenissen voor het genereren van waarschuwingen op de portalblade ziet, maar u geen e-mailmeldingen ontvangen, moet u controleren of het e-mailadres dat is opgegeven voor het ontvangen van e-mailberichten van externe afzenders is ingesteld. De e-mailwaarschuwingen is mogelijk geblokkeerd door uw e-mailinstellingen.

### <a name="alerts-on-metrics"></a>Waarschuwingen op metrische gegevens
U kunt verschillende metrische gegevens vastleggen en waarschuwingen maken op de metrische gegevens in Data Factory. U kunt bewaken en waarschuwingen maken op de volgende metrische gegevens voor de segmenten in de gegevensfactory:

* **Mislukte wordt uitgevoerd**
* **Geslaagde wordt uitgevoerd**

Deze metrische gegevens zijn nuttig en helpen u voor een overzicht van de algemene mislukte en geslaagde wordt uitgevoerd in de gegevensfactory. Metrische gegevens zijn verzonden, elke keer dat er wordt een segment uitvoeren. Aan het begin van het uur, zijn deze metrische gegevens geaggregeerd en gepusht naar uw opslagaccount. Instellen van een opslagaccount zodat metrische gegevens.

#### <a name="enable-metrics"></a>Metrische gegevens inschakelen
Klik op de volgende handelingen uit om metrische gegevens, de **Data Factory** blade:

**Bewaking** > **metriek** > **diagnostische instellingen** > **diagnostische gegevens**

![Diagnostische gegevens koppelen](./media/data-factory-monitor-manage-pipelines/diagnostics-link.png)

Op de **Diagnostics** blade, klikt u op **op**, selecteer het opslagaccount en klikt u op **opslaan**.

![Blade diagnostische gegevens](./media/data-factory-monitor-manage-pipelines/diagnostics-blade.png)

Het duurt maximaal één uur van de metrische gegevens worden weergegeven op de **bewaking** blade omdat metrische gegevens aggregatie per uur gebeurt.

### <a name="set-up-an-alert-on-metrics"></a>Instellen van een waarschuwing op metrische gegevens
Klik op de **Data Factory metrische gegevens** tegel:

![Data factory metrische gegevens tegel](./media/data-factory-monitor-manage-pipelines/data-factory-metrics-tile.png)

Op de **metriek** blade, klikt u op **+ waarschuwing toevoegen** op de werkbalk.
![Metrische blade gegevensfactory > Waarschuwing toevoegen](./media/data-factory-monitor-manage-pipelines/add-alert.png)

Op de **een waarschuwingsregel toevoegen** pagina en klik op de volgende stappen uitvoert **OK**.

* Voer een naam voor de waarschuwing (voorbeeld: 'is mislukt van de waarschuwing').
* Voer een beschrijving voor de waarschuwing (voorbeeld: 'een e-mailbericht verzenden wanneer een fout optreedt').
* Selecteer een waarde (vs "Wordt uitgevoerd is mislukt". 'Geslaagd wordt uitgevoerd').
* Geef een voorwaarde en de drempelwaarde.   
* Geef de periode.
* Geef op of een e-mailbericht moet worden verzonden naar eigenaren, bijdragers en lezers.

![Metrische blade gegevensfactory > waarschuwingsregel toevoegen](./media/data-factory-monitor-manage-pipelines/add-an-alert-rule.png)

Nadat de waarschuwingsregel is toegevoegd, de blade wordt gesloten en u de nieuwe waarschuwing zien op de **metriek** blade.

![Metrische blade gegevensfactory > nieuwe waarschuwing toegevoegd](./media/data-factory-monitor-manage-pipelines/failed-alert-in-metric-blade.png)

U ziet ook het aantal waarschuwingen in de **waarschuwing regels** tegel. Klik op de **waarschuwing regels** tegel.

![Metrische blade gegevensfactory - regels voor waarschuwingen](./media/data-factory-monitor-manage-pipelines/alert-rules-tile-rules.png)

Op de **regels waarschuwingen** blade ziet u een bestaande waarschuwingen. Klik op als u een waarschuwing **waarschuwing toevoegen** op de werkbalk.

![Regels voor waarschuwingen-blade](./media/data-factory-monitor-manage-pipelines/alert-rules-blade.png)

### <a name="alert-notifications"></a>Meldingen van waarschuwingen
Nadat de waarschuwingsregel overeenkomt met de voorwaarde, krijgt u een e-mailbericht met de tekst van de waarschuwing is geactiveerd. Nadat het probleem opgelost is en de meldingsvoorwaarde meer komt niet overeen, krijgt u een e-mailbericht met de tekst van de waarschuwing is opgelost.

Dit gedrag is anders dan gebeurtenissen waarbij een melding wordt verzonden op elke fout die een waarschuwingsregel in aanmerking komt voor.

### <a name="deploy-alerts-by-using-powershell"></a>Waarschuwingen implementeren met behulp van PowerShell
U kunt waarschuwingen voor de metrische gegevens op dezelfde manier als u zou voor gebeurtenissen doen implementeren.

**Definitie van waarschuwing**

```JSON
{
    "contentVersion" : "1.0.0.0",
    "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "parameters" : {},
    "resources" : [
    {
            "name" : "FailedRunsGreaterThan5",
            "type" : "microsoft.insights/alertrules",
            "apiVersion" : "2014-04-01",
            "location" : "East US",
            "properties" : {
                "name" : "FailedRunsGreaterThan5",
                "description" : "Failed Runs greater than 5",
                "isEnabled" : true,
                "condition" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                    "dataSource" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                        "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                        "metricName" : "FailedRuns"
                    },
                    "threshold" : 5.0,
                    "windowSize" : "PT3H",
                    "timeAggregation" : "Total"
                },
                "action" : {
                    "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                    "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                    "customEmails" : ["abhinav.gpt@live.com"]
                }
            }
        }
    ]
}
```

Vervang *subscriptionId*, *resourceGroupName*, en *dataFactoryName* in het voorbeeld met de juiste waarden.

*metricName* ondersteunt momenteel twee waarden:

* FailedRuns
* SuccessfulRuns

**De waarschuwing implementeren**

Voor het implementeren van de waarschuwing, gebruikt u de Azure PowerShell-cmdlet **New-AzureRmResourceGroupDeployment**, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json
```

Ziet het volgende bericht na een geslaagde implementatie:

```
VERBOSE: 12:52:47 PM - Template is valid.
VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded


DeploymentName    : FailedRunsGreaterThan5
ResourceGroupName : adf
ProvisioningState : Succeeded
Timestamp         : 7/27/2015 7:52:56 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
Outputs           
```

U kunt ook de **toevoegen AlertRule** cmdlet voor het implementeren van een waarschuwingsregel. Zie de [toevoegen AlertRule](https://msdn.microsoft.com/library/mt282468.aspx) onderwerp voor meer informatie en voorbeelden.  

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Een gegevensfactory verplaatsen naar een andere resourcegroep of abonnement
U kunt een gegevensfactory verplaatsen naar een andere resourcegroep of een ander abonnement met behulp van de **verplaatsen** opdracht balk knop op de startpagina van uw gegevensfactory.

![Verplaatsen van de gegevensfactory](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

U kunt ook alle gerelateerde resources (zoals waarschuwingen die gekoppeld aan de gegevensfactory zijn), samen met de data factory verplaatsen.

![Dialoogvenster resources verplaatsen](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
