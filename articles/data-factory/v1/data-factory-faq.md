---
title: Azure Data Factory - Veelgestelde vragen
description: Veelgestelde vragen over Azure Data Factory.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 2806a65c55f019da5842f29c6ef99dec63ae1bd0
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Azure Data Factory - Veelgestelde vragen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Veelgestelde vragen over - Data Factory versie 2](../frequently-asked-questions.md).

## <a name="general-questions"></a>Algemene vragen
### <a name="what-is-azure-data-factory"></a>Wat is Azure Data Factory?
Data Factory is een cloud-gebaseerde gegevensintegratie service die **automatiseert de verplaatsing en transformatie van gegevens**. Net als een factory die de apparatuur te grondstoffen te transformeren in eindproducten wordt, ingedeeld Data Factory bestaande services die onbewerkte gegevens verzamelen en transformeren in kant-en-klare informatie.

Data Factory kunt u gegevensgestuurde werkstromen maken voor het verplaatsen van gegevens tussen zowel lokale en cloud gegevensarchieven als proces/transformatie-gegevens met behulp van compute services zoals Azure HDInsight en Azure Data Lake Analytics. Nadat u een pijplijn die de actie uitgevoerd die u nodig hebt gemaakt, kunt u plannen om periodiek wordt uitgevoerd (elk uur, dagelijks, wekelijks, enzovoort).   

Zie voor meer informatie [overzicht & hoofdconcepten](data-factory-introduction.md).

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Waar vind ik prijsinformatie voor Azure Data Factory?
Zie [prijsinformatie voor Data Factory pagina] [ adf-pricing-details] voor de prijsinformatie voor de Azure Data Factory.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Hoe ga ik aan de slag met Azure Data Factory?
* Zie voor een overzicht van Azure Data Factory [Inleiding tot Azure Data Factory](data-factory-introduction.md).
* Voor een zelfstudie over het **gegevens kopiëren of verplaatsen** Kopieeractiviteit gebruikt, Zie [gegevens kopiëren van Azure Blob-opslag naar Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Voor een zelfstudie over het **gegevens transformeren** met HDInsight Hive-activiteit. Zie [gegevens verwerken door het Hive-script uitvoeren op Hadoop-cluster](data-factory-build-your-first-pipeline.md)

### <a name="what-is-the-data-factorys-region-availability"></a>Wat is de Data Factory beschikbaarheid in regio's?
Data Factory is beschikbaar in **VS-West** en **Noord-Europa**. De berekenings- en -services die worden gebruikt door gegevensfactory's kunnen zich in andere regio's. Zie [ondersteunde regio's](data-factory-introduction.md#supported-regions).

### <a name="what-are-the-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Wat zijn de limieten van het aantal data factory's / pijplijnen/activiteiten, gegevenssets?
Zie **Azure Data Factory limieten** sectie van de [Azure-abonnement en Service-limieten, quota's en beperkingen](../../azure-subscription-service-limits.md#data-factory-limits) artikel.

### <a name="what-is-the-authoringdeveloper-experience-with-azure-data-factory-service"></a>Wat is de ontwerpen of ontwikkelaar ervaring met Azure Data Factory-service?
U kunt de auteur/maken data Factory met een van de volgende hulpprogramma's / SDK's:

* **Azure-portal** blades in de Azure portal de Data Factory bieden uitgebreide gebruikersinterface u data factory's ad gekoppelde services maken. De **Data Factory-Editor**, die ook deel uitmaakt van de portal kunt u eenvoudig gekoppelde services, tabellen, gegevenssets en pijplijnen maken door op te geven van JSON-definities voor deze artefacten. Zie [bouwen van uw eerste gegevens pijplijn met Azure portal](data-factory-build-your-first-pipeline-using-editor.md) voor een voorbeeld van het gebruik van de portal/editor maken en implementeren van een gegevensfactory.
* **Visual Studio** u Visual Studio kunt gebruiken voor het maken van een Azure data factory. Zie [bouwen van uw eerste gegevens pijplijn met Visual Studio](data-factory-build-your-first-pipeline-using-vs.md) voor meer informatie.
* **Azure PowerShell** Zie [maken en te bewaken Azure Data Factory met Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md) voor een zelfstudie/overzicht voor het maken van een gegevensfactory met behulp van PowerShell. Zie [Data Factory Cmdlet Reference] [ adf-powershell-reference] inhoud op MSDN-bibliotheek voor een uitgebreide documentatie over Data Factory-cmdlets.
* **.NET class Library** programmatisch kunt u data factory maken met behulp van de Data Factory .NET SDK. Zie [maken, bewaken en beheren van data Factory met .NET SDK](data-factory-create-data-factories-programmatically.md) voor een overzicht van het maken van een gegevensfactory met .NET SDK. Zie [Data Factory Class Library Reference] [ msdn-class-library-reference] voor een uitgebreide documentatie over Data Factory .NET SDK.
* **REST-API** u kunt ook de REST-API die worden weergegeven door de Azure Data Factory-service maken en implementeren van de data Factory. Zie [Data Factory REST API Reference] [ msdn-rest-api-reference] voor een uitgebreide documentatie van de REST-API van Data Factory.
* **Azure Resource Manager-sjabloon** Zie [zelfstudie: uw eerste Azure-gegevensfactory bouwen met Azure Resource Manager-sjabloon](data-factory-build-your-first-pipeline-using-arm.md) voor meer informatie.

### <a name="can-i-rename-a-data-factory"></a>Kan ik de naam van een gegevensfactory wijzigen?
Nee. Net als andere Azure-resources, kan niet de naam van een Azure data factory worden gewijzigd.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-to-another"></a>Kan ik een gegevensfactory van één Azure-abonnement verplaatsen naar een andere?
Ja. Gebruik de **verplaatsen** knop op de blade gegevensfactory, zoals wordt weergegeven in het volgende diagram:

![Verplaatsen van de gegevensfactory](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-the-compute-environments-supported-by-data-factory"></a>Wat zijn de compute-omgevingen die door Data Factory worden ondersteund?
De volgende tabel bevat een lijst van compute-omgevingen wordt ondersteund door de Data Factory en de activiteiten die kunnen worden uitgevoerd op deze.

| Compute-omgeving | activities |
| --- | --- |
| [HDInsight-cluster op aanvraag](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) of [uw eigen HDInsight-cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](data-factory-compute-linked-services.md#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Machine Learning-activiteiten: batchuitvoering en resources bijwerken](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL Data Warehouse](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [SQL Server](data-factory-compute-linked-services.md#sql-server-linked-service) |[Opgeslagen procedure](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Hoe Azure Data Factory vergelijken met SQL Server Integration Services (SSIS)? 
Zie de [tegenover Azure Data Factory. SSIS](http://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) presentatie van een van onze MVP's (meest waardevolle Professionals): Reza Rad. Sommige van de meest recente wijzigingen in de Data Factory kan niet worden vermeld in de presentatie. We zijn voortdurend meer mogelijkheden toegevoegd aan Azure Data Factory. We zijn voortdurend meer mogelijkheden toegevoegd aan Azure Data Factory. We zullen deze updates opnemen in de vergelijking van gegevens integratietechnologieën van Microsoft enige tijd later dit jaar.   

## <a name="activities---faq"></a>Activiteiten - Veelgestelde vragen
### <a name="what-are-the-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Wat zijn de verschillende typen activiteiten die kunt u in een Data Factory-pijplijn?
* [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) om gegevens te verplaatsen.
* [Activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) naar proces/transformatie gegevens.

### <a name="when-does-an-activity-run"></a>Wanneer wordt een activiteit uitgevoerd?
De **beschikbaarheid** configuratie-instelling in de uitvoertabel van de gegevens wordt bepaald wanneer de activiteit wordt uitgevoerd. Als invoergegevenssets worden opgegeven, wordt de activiteit controleert of alle afhankelijkheden van de invoergegevens zijn voldaan (dat wil zeggen, **gereed** status) voordat het wordt gestart.

## <a name="copy-activity---faq"></a>Kopieeractiviteit - Veelgestelde vragen
### <a name="is-it-better-to-have-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Is het beter om een pijplijn met meerdere activiteiten of een afzonderlijke pijplijn voor elke activiteit?
Pijplijnen moeten gerelateerde activiteiten te bundelen. Als de gegevenssets die verbinding ze maken niet worden gebruikt door een andere activiteit buiten de pijplijn, kunt u de activiteiten in één pijplijn bewaren. Op deze manier zou niet hoeft te keten pijplijn actieve perioden zodat ze met elkaar worden uitgelijnd. Bovendien wordt de integriteit van gegevens in de tabellen die intern is aan de pijplijn beter behouden bij het bijwerken van de pijplijn. Pijplijn update in wezen stopt alle activiteiten in de pijplijn, worden deze verwijderd en opnieuw gemaakt. Vanuit het perspectief ontwerpen, mogelijk gemakkelijker om te zien van de stroom van gegevens binnen de gerelateerde activiteiten in een JSON-bestand voor de pijplijn.

### <a name="what-are-the-supported-data-stores"></a>Wat zijn de ondersteunde gegevensarchieven?
De kopieeractiviteit in Data Factory kopieert gegevens van een brongegevensarchief naar een sinkgegevensarchief. Data Factory ondersteunt de volgende gegevensarchieven. Gegevens vanuit elke willekeurige bron kunnen naar een sink worden geschreven. Klik op een gegevensarchief voor informatie over het kopiëren van gegevens naar en van dat archief.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> Gegevensarchieven met een * kunnen zich on-premises of op Azure IaaS bevinden. Hiervoor moet u [Data Management Gateway](data-factory-data-management-gateway.md) installeren op een on-premises/Azure IaaS-computer.

### <a name="what-are-the-supported-file-formats"></a>Wat zijn de ondersteunde bestandsindelingen?
[!INCLUDE [data-factory-file-format](../../../includes/data-factory-file-format.md)]

### <a name="where-is-the-copy-operation-performed"></a>Waar wordt de kopieerbewerking uitgevoerd?
Zie [wereldwijd beschikbare gegevensverplaatsing](data-factory-data-movement-activities.md#global) sectie voor meer informatie. Kortom, wanneer een on-premises gegevensopslag betrokken is, wordt de kopieerbewerking uitgevoerd door de Data Management Gateway in uw on-premises omgeving. En wanneer de verplaatsing van gegevens tussen twee cloud-winkels, de kopieerbewerking wordt uitgevoerd in die het dichtst bij de sink-locatie in de dezelfde geografische regio.

## <a name="hdinsight-activity---faq"></a>HDInsight-activiteit - Veelgestelde vragen
### <a name="what-regions-are-supported-by-hdinsight"></a>Welke regio's worden ondersteund door HDInsight?
Zie de sectie geografische beschikbaarheid in het volgende artikel: of [HDInsight prijsinformatie][hdinsight-supported-regions].

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>Welke regio wordt gebruikt door een HDInsight-cluster op aanvraag?
Het HDInsight-cluster op aanvraag wordt gemaakt in dezelfde regio waar de opslag die u hebt opgegeven om te worden gebruikt met het cluster bestaat.    

### <a name="how-to-associate-additional-storage-accounts-to-your-hdinsight-cluster"></a>Het koppelen van extra opslagaccounts met uw HDInsight-cluster?
Als u uw eigen HDInsight-Cluster (BYOC - Bring Your Own Cluster) gebruikt, raadpleegt u de volgende onderwerpen:

* [Met behulp van een HDInsight-Cluster met alternatieve Opslagaccounts en Metastores][hdinsight-alternate-storage]
* [Extra Storage-Accounts gebruiken met HDInsight Hive][hdinsight-alternate-storage-2]

Als u een cluster op aanvraag die is gemaakt door de Data Factory-service gebruikt, geeft u de extra opslagaccounts voor het HDInsight gekoppelde service, zodat de Data Factory-service namens jou registreren kunt. Gebruik in de JSON-definitie voor de gekoppelde service op aanvraag **additionalLinkedServiceNames** eigenschap alternatieve opslagaccounts opgeven, zoals wordt weergegeven in de volgende JSON-fragment:

```JSON
{
    "name": "MyHDInsightOnDemandLinkedService",
    "properties":
    {
        "type": "HDInsightOnDemandLinkedService",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ]
        }
    }
}
```
In het bovenstaande voorbeeld vertegenwoordigen otherLinkedServiceName1 en otherLinkedServiceName2 gekoppelde services waarvan definities referenties die het HDInsight-cluster nodig heeft bevatten voor toegang tot alternatieve opslagaccounts.

## <a name="slices---faq"></a>Segmenten - Veelgestelde vragen
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Waarom wordt mijn invoersegmenten één keer niet in de status gereed?
Een algemene fout niet tot stand **externe** eigenschap **waar** op invoergegevensset wanneer de invoergegevens zich buiten de gegevensfactory (niet geproduceerd door de data factory).

In het volgende voorbeeld, hoeft u alleen in te stellen **externe** op true op **dataset1**.  

**DataFactory1** Pipeline 1: dataset1 -> activiteit1 -> dataset2 -> activiteit2 -> dataset3 pijplijn 2: dataset3 -> activiteit3 -> dataset4

Als u een andere data factory met een pipeline waarmee dataset4 (die wordt geproduceerd door de pijplijn 2 in gegevensfactory 1) hebt, moet u dataset4 markeren als een externe gegevensset omdat de gegevensset wordt geproduceerd door een andere data factory (DataFactory1, niet DataFactory2).  

**DataFactory2**    
Pijplijn 1: dataset4 -> activity4 -> dataset5

Als de eigenschap external juist is ingesteld, moet u controleren of de invoergegevens in de opgegeven locatie in de definitie van de invoer-gegevensset bestaat.

### <a name="how-to-run-a-slice-at-another-time-than-midnight-when-the-slice-is-being-produced-daily"></a>Het uitvoeren van een segment op een later tijdstip dan middernacht wanneer het segment dagelijks wordt geproduceerd?
Gebruik de **offset** eigenschap om het tijdstip waarop u het segment wordt geproduceerd. Zie [gegevensset beschikbaarheid](data-factory-create-datasets.md#dataset-availability) sectie voor meer informatie over deze eigenschap. Hier volgt een voorbeeld van een snelle:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Dagelijkse segmenten beginnen bij **06: 00** in plaats van de standaard middernacht.     

### <a name="how-can-i-rerun-a-slice"></a>Hoe kan ik een segment opnieuw uitvoeren?
U kunt een segment opnieuw uitvoeren in een van de volgende manieren:

* Monitor and Manage App een activiteitvenster of het segment opnieuw wilt gebruiken. Zie [geselecteerde activiteit windows opnieuw uitvoeren](data-factory-monitor-manage-app.md#perform-batch-actions) voor instructies.   
* Klik op **uitvoeren** in de opdrachtbalk op de **GEGEVENSSEGMENT** blade voor het segment in de Azure portal.
* Voer **Set-AzureRmDataFactorySliceStatus** cmdlet met de Status ingesteld op **wachten** voor het segment.   

    ```PowerShell
    Set-AzureRmDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
Zie [Set-AzureRmDataFactorySliceStatus] [ set-azure-datafactory-slice-status] voor meer informatie over de cmdlet.

### <a name="how-long-did-it-take-to-process-a-slice"></a>Hoe lang was er nodig voor het verwerken van een segment?
Activiteit venster Explorer in App beheren en bewaken gebruiken om te weten hoe lang geduurd een gegevenssegment verwerken. Zie [activiteit venster Explorer](data-factory-monitor-manage-app.md#activity-window-explorer) voor meer informatie.

U kunt ook het volgende in de Azure portal doen:  

1. Klik op **gegevenssets** tegel op de **DATA FACTORY** blade van uw gegevensfactory.
2. Klik op de specifieke gegevensset in de **gegevenssets** blade.
3. Selecteer het segment dat u geïnteresseerd in van bent de **recente segmenten** lijst op de **tabel** blade.
4. Klik op de activiteit uitvoeren vanaf de **activiteiten bij uitvoering** lijst op de **GEGEVENSSEGMENT** blade.
5. Klik op **eigenschappen** tegel op de **DETAILS uitvoering van activiteit** blade.
6. U ziet de **duur** veld met een waarde. Deze waarde is de tijd die nodig is voor het verwerken van het segment.   

### <a name="how-to-stop-a-running-slice"></a>Hoe kan ik een actieve segment stoppen?
Als u stoppen met de pijplijn wilt wordt uitgevoerd, kunt u [onderbreken AzureRmDataFactoryPipeline](/powershell/module/azurerm.datafactories/suspend-azurermdatafactorypipeline) cmdlet. Op dit moment is stopt het uitstellen van de pijplijn niet de segment-uitvoeringen die uitgevoerd worden. Zodra u klaar bent met de uitvoeringen in voortgang, is geen extra segment verwerkt.

Als u zeker stoppen alle uitvoeringen onmiddellijk wilt, kan de enige manier worden verwijderd van de pijplijn en maak deze opnieuw. Als u verwijderen van de pijplijn wilt, hoeft u niet verwijderen van tabellen en gekoppelde services door de pijplijn gebruikt.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/azurerm.datafactories
[azure-portal]: http://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/set-azurermdatafactoryslicestatus

[adf-pricing-details]: http://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: http://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: http://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
