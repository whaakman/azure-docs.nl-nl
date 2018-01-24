---
title: Data Factory - .NET API wijzigingenlogboek | Microsoft Docs
description: Beschrijft de grote wijzigingen, functie toevoegingen, oplossingen voor problemen enzovoort... in een specifieke versie van .NET-API voor de Azure Data Factory.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 8208271b-7f4c-4214-b665-d2ff503c4470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 55a08d22c622c89b918d1bfadd0ce34b77c3d408
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory - logbestand voor .NET API
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. 

In dit artikel bevat informatie over wijzigingen met SDK van Azure Data Factory in een specifieke versie. U vindt de nieuwste NuGet-pakket voor Azure Data Factory [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)

## <a name="version-4110"></a>Versie 4.11.0
Functie toevoegingen:

* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* De volgende typen van de gegevensset zijn toegevoegd:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* De volgende typen van de kopie-bron zijn toegevoegd:
  * [MongoDbSource](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>Versie 4.10.0
* De volgende optionele eigenschappen zijn toegevoegd aan TextFormat:
  * [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [TreatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* De volgende typen van de gegevensset zijn toegevoegd:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* De volgende typen van de kopie-bron zijn toegevoegd:
  * [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* Voeg [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) eigenschap AzureMLBatchExecutionActivity
  * Meerdere web service invoer voor een Azure Machine Learning-experiment doorgeven inschakelen

## <a name="version-491"></a>Versie 4.9.1
### <a name="bug-fix"></a>Fout-oplossing
* Afschaffen WebApi-verificatie voor [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Versie 4.9.0
### <a name="feature-additions"></a>Functie-toevoegingen
* Voeg [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) en [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) eigenschappen van CopyActivity. Zie [kopie gefaseerde](data-factory-copy-activity-performance.md#staged-copy) voor meer informatie over de functie.

### <a name="bug-fix"></a>Fout-oplossing
* Introduceert een overload van [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) methode waarbij een [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) exemplaar.
* Markeer [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) en [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) als optioneel in CopySink.

## <a name="version-480"></a>Versie 4.8.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende optionele eigenschappen zijn toegevoegd aan het activiteitstype kopiëren om in te schakelen afstemming van de prestaties van de kopie van:
  * [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Versie 4.7.0
### <a name="feature-additions"></a>Functie-toevoegingen
* Nieuwe StorageFormat type toegevoegd [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) type om bestanden te kopiëren in geoptimaliseerde rij kolomindeling (ORC).
* Voeg [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) en PolyBaseSettings eigenschappen aan SqlDWSink.
  * Maakt het gebruik van PolyBase om gegevens te kopiëren in SQL Data Warehouse.

## <a name="version-461"></a>Versie 4.6.1
### <a name="bug-fixes"></a>Oplossen van fouten
* HTTP-aanvraag voor het aanbieden van activiteit windows corrigeert.
  * Naam van de resourcegroep en de naam van de factory verwijdert uit de nettolading van de aanvraag.

## <a name="version-460"></a>Versie 4.6.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende eigenschappen zijn toegevoegd aan [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [ExpirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Gegevenssets](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* De volgende eigenschappen zijn toegevoegd aan [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Nieuw toegevoegd [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) type [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) type voor het definiëren van gegevenssets waarvan de gegevens zich in de JSON-indeling.

## <a name="version-450"></a>Versie 4.5.0
### <a name="feature-additions"></a>Functie-toevoegingen
* Toegevoegd [bewerkingen voor activiteitvenster](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
  * Toegevoegde methoden voor het ophalen van activiteit windows met filters op basis van de entiteittypen (dat wil zeggen, data Factory, gegevenssets, pijplijnen en activiteiten).
* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* De volgende typen van de gegevensset zijn toegevoegd:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* De volgende typen van de kopie-bron zijn toegevoegd:     
  * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Versie 4.4.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende gekoppelde-servicetype is toegevoegd als gegevensbronnen en de sinks voor de activiteiten kopiëren:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Zie [Azure Storage Linked Service SAS](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) voor conceptuele informatie en voorbeelden.

## <a name="version-430"></a>Versie 4.3.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende gekoppelde service typen haven zijn toegevoegd als gegevensbronnen voor de activiteiten kopiëren:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Zie [verplaatsen van gegevens uit HDFS gebruik Data Factory](data-factory-hdfs-connector.md) voor conceptuele informatie en voorbeelden.
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Zie [verplaatsen van gegevens van ODBC-gegevensarchieven met behulp van Azure Data Factory](data-factory-odbc-connector.md) voor conceptuele informatie en voorbeelden.

## <a name="version-420"></a>Versie 4.2.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende nieuw activiteitstype is toegevoegd: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Zie voor meer informatie over de activiteit [modellen van Azure ML bijwerken met behulp van de activiteit van de Resource Update](data-factory-azure-ml-batch-execution-activity.md).
* Een nieuwe optionele eigenschap [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) is toegevoegd aan de [AzureMLLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx).
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) en [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) eigenschappen zijn toegevoegd aan de [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) klasse.
* Configuratie van de time-outs voor het clientaanroepen naar de Data Factory-service toestaan.

## <a name="version-410"></a>Versie 4.1.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* De volgende activiteitstypen zijn toegevoegd:
  * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* De volgende typen van de gegevensset zijn toegevoegd:
  * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* De volgende bron- en sink-typen voor de Kopieeractiviteit zijn toegevoegd:
  * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>Versie 4.0.1
### <a name="breaking-changes"></a>Wijzigingen op te splitsen
De volgende klassen hebben gekregen. De namen van de nieuwe zijn de namen van de oorspronkelijke van klassen voordat 4.0.0 loslaat.

| Naam in 4.0.0 | Naam in 4.0.1 |
|:--- |:--- |
| AzureSqlDataWarehouseDataset |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| RelationalDataset |[RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>Versie 4.0.0
### <a name="breaking-changes"></a>Wijzigingen op te splitsen
* De volgende klassen/interfaces zijn gewijzigd.

| Oude naam | Nieuwe naam |
|:--- |:--- |
| ITableOperations |[IDatasetOperations](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.idatasetoperations.aspx) |
| Tabel |[Dataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.dataset.aspx) |
| TableProperties |[DatasetProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetproperties.aspx) |
| TableTypeProprerties |[DatasetTypeProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasettypeproperties.aspx) |
| TableCreateOrUpdateParameters |[DatasetCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateparameters.aspx) |
| TableCreateOrUpdateResponse |[DatasetCreateOrUpdateResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdateresponse.aspx) |
| TableGetResponse |[DatasetGetResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetgetresponse.aspx) |
| TableListResponse |[DatasetListResponse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetlistresponse.aspx) |
| CreateOrUpdateWithRawJsonContentParameters |[DatasetCreateOrUpdateWithRawJsonContentParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datasetcreateorupdatewithrawjsoncontentparameters.aspx) |

* De **lijst** methoden nu wisselbare resultaten geretourneerd. Als het antwoord een niet-lege bevat **NextLink** eigenschap, de clienttoepassing moet gaan met het ophalen van de volgende pagina totdat alle pagina's zijn geretourneerd.  Hier volgt een voorbeeld:

    ```csharp
    PipelineListResponse response = client.Pipelines.List("ResourceGroupName", "DataFactoryName");
    var pipelines = new List<Pipeline>(response.Pipelines);

    string nextLink = response.NextLink;
    while (!string.IsNullOrEmpty(nextLink))
    {
        PipelineListResponse nextResponse = client.Pipelines.ListNext(nextLink);
        pipelines.AddRange(nextResponse.Pipelines);

        nextLink = nextResponse.NextLink;
    }
    ```
* **Lijst** pijplijn API retourneert alleen de samenvatting van een pijplijn in plaats van de volledige details. Activiteiten in een pijplijn samenvatting bevatten bijvoorbeeld alleen naam en type.

### <a name="feature-additions"></a>Functie-toevoegingen
* De [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) klasse ondersteunt twee nieuwe eigenschappen **SliceIdentifierColumnName** en **SqlWriterCleanupScript**, ter ondersteuning van idempotent kopiëren naar Azure SQL-gegevens Het magazijn. Zie de [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) artikel voor meer informatie over deze eigenschappen.
* We bieden nu ondersteuning opgeslagen procedure gegevensbronnen voor Azure SQL Database en Azure SQL Data Warehouse uitgevoerd als onderdeel van de Kopieeractiviteit. De [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) en [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) klassen hebben de volgende eigenschappen: **SqlReaderStoredProcedureName** en **StoredProcedureParameters**. Zie de [Azure SQL Database](data-factory-azure-sql-connector.md#sqlsource) en [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) artikelen op Azure.com voor meer informatie over deze eigenschappen.  
