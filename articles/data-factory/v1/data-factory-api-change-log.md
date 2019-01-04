---
title: Data Factory - .NET API-wijzigingenlogboek | Microsoft Docs
description: Beschrijving van belangrijke wijzigingen, aanvullingen functie, oplossingen voor problemen enzovoort... in een specifieke versie van .NET-API voor Azure Data Factory.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.assetid: 8208271b-7f4c-4214-b665-d2ff503c4470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 863f3500c84eeab1c3dac19141cd334fc6961694
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54015913"
---
# <a name="azure-data-factory---net-api-change-log"></a>Azure Data Factory - .NET API-wijzigingenlogboek
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. 

In dit artikel bevat informatie over wijzigingen voor Azure Data Factory SDK in een specifieke versie. U vindt de meest recente NuGet-pakket voor Azure Data Factory [hier](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories)

## <a name="version-4110"></a>Versie 4.11.0
Functie-toevoegingen:

* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [OnPremisesMongoDbLinkedService](https://msdn.microsoft.com/library/mt765129.aspx)
  * [AmazonRedshiftLinkedService](https://msdn.microsoft.com/library/mt765121.aspx)
  * [AwsAccessKeyLinkedService](https://msdn.microsoft.com/library/mt765144.aspx)
* De volgende gegevenssettypen zijn toegevoegd:
  * [MongoDbCollectionDataset](https://msdn.microsoft.com/library/mt765145.aspx)
  * [AmazonS3Dataset](https://msdn.microsoft.com/library/mt765112.aspx)
* De volgende typen van de kopie-gegevensbronnen zijn toegevoegd:
  * [MongoDbSource](https://msdn.microsoft.com/library/mt765123.aspx)

## <a name="version-4100"></a>Versie 4.10.0
* De volgende optionele eigenschappen zijn toegevoegd aan TextFormat:
  * [SkipLineCount](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.skiplinecount.aspx)
  * [FirstRowAsHeader](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.firstrowasheader.aspx)
  * [treatEmptyAsNull](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.textformat.treatemptyasnull.aspx)
* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [OnPremisesCassandraLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandralinkedservice.aspx)
  * [SalesforceLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.salesforcelinkedservice.aspx)
* De volgende gegevenssettypen zijn toegevoegd:
  * [OnPremisesCassandraTableDataset](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisescassandratabledataset.aspx)
* De volgende typen van de kopie-gegevensbronnen zijn toegevoegd:
  * [CassandraSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.cassandrasource.aspx)
* Voeg [WebServiceInputs](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azuremlbatchexecutionactivity.webserviceinputs.aspx) eigenschap AzureMLBatchExecutionActivity
  * Inschakelen geven meerdere web service-invoer voor een Azure Machine Learning-experiment

## <a name="version-491"></a>Versie 4.9.1
### <a name="bug-fix"></a>Opgelost probleem
* API-verificatie voor afschaffen [WebLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.weblinkedservice.authenticationtype.aspx).

## <a name="version-490"></a>Versie 4.9.0
### <a name="feature-additions"></a>Functie-toevoegingen
* Voeg [EnableStaging](https://msdn.microsoft.com/library/mt767916.aspx) en [StagingSettings](https://msdn.microsoft.com/library/mt767918.aspx) eigenschappen CopyActivity. Zie [gefaseerd kopiëren](data-factory-copy-activity-performance.md#staged-copy) voor meer informatie over de functie.

### <a name="bug-fix"></a>Opgelost probleem
* Introductie van een overbelasting van de [ActivityWindowOperationExtensions.List](https://msdn.microsoft.com/library/mt767915.aspx) methode, waarbij een [ActivityWindowsByActivityListParameters](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.activitywindowsbyactivitylistparameters.aspx) exemplaar.
* Mark [WriteBatchSize](https://msdn.microsoft.com/library/dn884293.aspx) en [WriteBatchTimeout](https://msdn.microsoft.com/library/dn884245.aspx) in CopySink als optioneel.

## <a name="version-480"></a>Versie 4.8.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende optionele eigenschappen zijn toegevoegd aan het type van de activiteit kopiëren om in te schakelen afstemmen van kopieerprestaties:
  * [ParallelCopies](https://msdn.microsoft.com/library/mt767910.aspx)
  * [CloudDataMovementUnits](https://msdn.microsoft.com/library/mt767912.aspx)

## <a name="version-470"></a>Versie 4.7.0
### <a name="feature-additions"></a>Functie-toevoegingen
* Nieuwe StorageFormat type toegevoegd [OrcFormat](https://msdn.microsoft.com/library/mt723391.aspx) te kopiëren van bestanden in de kolommen (ORC)-indeling geoptimaliseerde rij.
* Voeg [AllowPolyBase](https://msdn.microsoft.com/library/mt723396.aspx) en PolyBaseSettings eigenschappen aan SqlDWSink.
  * Hiermee kunt het gebruik van PolyBase om gegevens te kopiëren in SQL Data Warehouse.

## <a name="version-461"></a>Versie 4.6.1
### <a name="bug-fixes"></a>Oplossingen voor problemen
* HTTP-aanvraag voor activiteitenvensters corrigeert.
  * Hiermee verwijdert u de naam van de resourcegroep en de naam van de data factory op van de nettolading van de aanvraag.

## <a name="version-460"></a>Versie 4.6.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende eigenschappen zijn toegevoegd aan [PipelineProperties](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties_properties.aspx):
  * [PipelineMode](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.pipelinemode.aspx)
  * [expirationTime](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.expirationtime.aspx)
  * [Gegevenssets](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.pipelineproperties.datasets.aspx)
* De volgende eigenschappen zijn toegevoegd aan [PipelineRuntimeInfo](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.aspx):
  * [PipelineState](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.common.models.pipelineruntimeinfo.pipelinestate.aspx)
* Nieuw toegevoegd [StorageFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.storageformat.aspx) type [JsonFormat](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.jsonformat.aspx) type voor het definiëren van gegevenssets waarvan de gegevens in JSON-indeling wordt.

## <a name="version-450"></a>Versie 4.5.0
### <a name="feature-additions"></a>Functie-toevoegingen
* Toegevoegd [lijstbewerkingen voor activiteitsvenster](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.activitywindowoperationsextensions.aspx).
  * Toegevoegde methoden om op te halen activiteitsvensters met filters op basis van de Entiteitstypen (dat wil zeggen, data factory's, gegevenssets, pijplijnen en activiteiten).
* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [ODataLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odatalinkedservice.aspx), [WebLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.weblinkedservice.aspx)
* De volgende gegevenssettypen zijn toegevoegd:
  * [ODataResourceDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.odataresourcedataset.aspx), [WebTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.webtabledataset.aspx)
* De volgende typen van de kopie-gegevensbronnen zijn toegevoegd:     
  * [WebSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.websource.aspx)

## <a name="version-440"></a>Versie 4.4.0
### <a name="feature-additions"></a>Functie-toevoegingen
* Het volgende type gekoppelde service is toegevoegd als gegevensbronnen en sinks voor kopieeractiviteiten:
  * [AzureStorageSasLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.azurestoragesaslinkedservice.aspx). Zie [gekoppelde Service van Azure Storage SAS](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) voor conceptuele informatie en voorbeelden.

## <a name="version-430"></a>Versie 4.3.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende typen haven voor gekoppelde service is toegevoegd als gegevensbronnen voor kopieeractiviteiten:
  * [HdfsLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.hdfslinkedservice.aspx). Zie [verplaatsen van gegevens uit HDFS met behulp van Data Factory](data-factory-hdfs-connector.md) voor conceptuele informatie en voorbeelden.
  * [OnPremisesOdbcLinkedService](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.onpremisesodbclinkedservice.aspx). Zie [verplaatsen van gegevens van ODBC-gegevensopslag met Azure Data Factory](data-factory-odbc-connector.md) voor conceptuele informatie en voorbeelden.

## <a name="version-420"></a>Versie 4.2.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende nieuwe type van de activiteit is toegevoegd: [AzureMLUpdateResourceActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremlupdateresourceactivity.aspx). Zie voor meer informatie over de activiteit [bijwerken van Azure ML-modellen met behulp van de activiteit resources bijwerken](data-factory-azure-ml-batch-execution-activity.md).
* Een nieuwe optionele eigenschap [updateResourceEndpoint](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.updateresourceendpoint.aspx) is toegevoegd aan de [AzureMLLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuremllinkedservice.aspx).
* [LongRunningOperationInitialTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationinitialtimeout.aspx) en [LongRunningOperationRetryTimeout](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.longrunningoperationretrytimeout.aspx) eigenschappen zijn toegevoegd aan de [DataFactoryManagementClient](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.datafactorymanagementclient.aspx) klasse.
* Configuratie van de time-outs voor clientaanroepen naar de Data Factory-service toestaan.

## <a name="version-410"></a>Versie 4.1.0
### <a name="feature-additions"></a>Functie-toevoegingen
* De volgende typen van de gekoppelde service zijn toegevoegd:
  * [AzureDataLakeStoreLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
  * [AzureDataLakeAnalyticsLinkedService](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* De volgende activiteitstypen zijn toegevoegd:
  * [DataLakeAnalyticsUSQLActivity](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.datalakeanalyticsusqlactivity.aspx)
* De volgende gegevenssettypen zijn toegevoegd:
  * [AzureDataLakeStoreDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoredataset.aspx)
* De volgende typen voor de bron en sink voor de Kopieeractiviteit zijn toegevoegd:
  * [AzureDataLakeStoreSource](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresource.aspx)
  * [AzureDataLakeStoreSink](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestoresink.aspx)

## <a name="version-401"></a>Versie 4.0.1
### <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
De volgende klassen zijn, gewijzigd. De nieuwe namen zijn namen van de oorspronkelijke van klassen voordat 4.0.0 release.

| Naam in 4.0.0 | Naam in 4.0.1 |
|:--- |:--- |
| AzureSqlDataWarehouseDataset |[AzureSqlDataWarehouseTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqldatawarehousetabledataset.aspx) |
| AzureSqlDataset |[AzureSqlTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuresqltabledataset.aspx) |
| AzureDataset |[AzureTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuretabledataset.aspx) |
| OracleDataset |[OracleTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.oracletabledataset.aspx) |
| RelationalDataset |[RelationalTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.relationaltabledataset.aspx) |
| SqlServerDataset |[SqlServerTableDataset](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.sqlservertabledataset.aspx) |

## <a name="version-400"></a>Versie 4.0.0
### <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
* De volgende klassen/interfaces zijn, gewijzigd.

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

* De **lijst** methoden retourneren pagina's met zoekresultaten nu. Als het antwoord een niet-lege bevat **NextLink** moet de eigenschap, de clienttoepassing om door te gaan met het ophalen van de volgende pagina totdat alle pagina's worden geretourneerd.  Hier volgt een voorbeeld:

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
* **Lijst met** pijplijn API retourneert alleen de samenvatting van een pijplijn in plaats van de volledige details. Activiteiten in een samenvatting van de pijplijn bevat bijvoorbeeld alleen naam en type.

### <a name="feature-additions"></a>Functie-toevoegingen
* De [SqlDWSink](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsink.aspx) klasse biedt ondersteuning voor twee nieuwe eigenschappen, **SliceIdentifierColumnName** en **SqlWriterCleanupScript**, ter ondersteuning van idempotent kopiëren naar Azure SQL-gegevens Het magazijn. Zie de [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md) artikel voor meer informatie over deze eigenschappen.
* We bieden nu ondersteuning opgeslagen procedure uit te voeren op basis van Azure SQL Database en Azure SQL Data Warehouse bronnen als onderdeel van de Kopieeractiviteit. De [SqlSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqlsource.aspx) en [SqlDWSource](https://msdn.microsoft.com/library/azure/microsoft.azure.management.datafactories.models.sqldwsource.aspx) klassen hebben de volgende eigenschappen: **SqlReaderStoredProcedureName** en **StoredProcedureParameters**. Zie de [Azure SQL Database](data-factory-azure-sql-connector.md#sqlsource) en [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#sqldwsource) artikelen op Azure.com voor meer informatie over deze eigenschappen.  
