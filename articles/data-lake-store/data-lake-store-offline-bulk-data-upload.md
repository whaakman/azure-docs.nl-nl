---
title: Uploaden van grote hoeveelheden gegevens naar Azure Data Lake Storage Gen1 met behulp van offline methoden | Microsoft Docs
description: Het hulpprogramma AdlCopy gebruiken om gegevens uit Azure Storage-blobs kopiëren naar Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 45321f6a-179f-4ee4-b8aa-efa7745b8eb6
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 6430bf524ac81af242bf7afb4c2c8196309806ab
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44391672"
---
# <a name="use-the-azure-importexport-service-for-offline-copy-of-data-to-azure-data-lake-storage-gen1"></a>Gebruik de Azure Import/Export-service voor het offline kopiëren van gegevens naar Azure Data Lake Storage Gen1
In dit artikel leert u hoe u wilt kopiëren van grote gegevenssets (> 200 GB) in Azure Data Lake Storage Gen1 met behulp van methoden voor offline-exemplaar, zoals de [Azure Import/Export-service](../storage/common/storage-import-export-service.md). Het bestand dat wordt gebruikt als voorbeeld in dit artikel is specifiek, 339,420,860,416 bytes of ongeveer 319 GB op schijf. Noemen we deze 319GB.tsv bestand.

De Azure Import/Export-service helpt u om over te brengen grote hoeveelheden gegevens veiliger naar Azure Blob storage door de verzending van harde schijven naar een Azure-datacenter.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u het volgende hebt:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure storage-account**.
* **Een account met Azure Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)

## <a name="preparing-the-data"></a>De gegevens voorbereiden

Voordat u de Import/Export-service, verbreekt u het bestand om te worden overgedragen **naar exemplaren die kleiner is dan 200 GB** in grootte. Het hulpprogramma voor importeren werkt niet met bestanden die groter zijn dan 200 GB. In deze zelfstudie wordt het bestand in segmenten van 100 GB elk splitsen. U kunt dit doen met behulp van [Cygwin](https://cygwin.com/install.html). Cygwin biedt ondersteuning voor Linux-opdrachten. In dit geval gebruikt u de volgende opdracht:

    split -b 100m 319GB.tsv

De splitsbewerking worden bestanden met de volgende namen gemaakt.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Bereid u voor schijven met gegevens
Volg de instructies in [met behulp van de Azure Import/Export-service](../storage/common/storage-import-export-service.md) (onder de **voorbereiden van uw schijven** sectie) voor het voorbereiden van uw harde schijven. Hier volgt de algemene volgorde:

1. Aanschaffen van een harde schijf die voldoet aan de vereisten voor de Azure Import/Export-service moet worden gebruikt.
2. Identificeer de Azure storage-account waar de gegevens worden gekopieerd nadat deze is verzonden naar de Azure-datacenter.
3. Gebruik de [Azure Import/Export-hulpprogramma](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), een opdrachtregelprogramma. Hier volgt een voorbeeld-codefragment dat laat zien hoe u het hulpprogramma te gebruiken.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Zie [met behulp van de Azure Import/Export-service](../storage/common/storage-import-export-service.md) voor meer voorbeelden codefragmenten.
4. De voorgaande opdracht maakt een logboekbestand op de opgegeven locatie. Dit logboekbestand gebruiken om te maken van een import-taak uit de [Azure-portal](https://portal.azure.com).

## <a name="create-an-import-job"></a>Een importtaak maken
U kunt nu een importtaak maken met behulp van de instructies in [met behulp van de Azure Import/Export-service](../storage/common/storage-import-export-service.md) (onder de **de Import-taak maken** sectie). Voor deze taak importeren met andere gegevens bieden ook de logboekbestand gemaakt tijdens het voorbereiden van de schijven.

## <a name="physically-ship-the-disks"></a>Kies voor fysieke verzending van de schijven
U kunt nu de schijven naar een Azure-datacenter fysiek verzenden. De gegevens is, gekopieerd naar de Azure Storage-blobs die u hebt opgegeven tijdens het maken van de import-taak. Ook tijdens het maken van de taak, kunt als u ervoor hebt gekozen voor de controle-informatie later, u nu gaat u terug naar uw import-taak en het traceringsnummer bijwerken.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-storage-gen1"></a>Gegevens kopiëren van Azure Storage-blobs naar Azure Data Lake Storage Gen1
Nadat de status van de import-taak ziet u dat deze voltooid, kunt u controleren of de gegevens zijn beschikbaar in de Azure Storage-blobs die u heeft opgegeven. U kunt vervolgens verschillende methoden gebruiken die gegevens van de blobs naar Azure Data Lake Storage Gen1 verplaatsen. Zie voor alle beschikbare opties voor het uploaden van gegevens, [ophalen van gegevens in Data Lake Storage Gen1](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-storage-gen1).

In deze sectie bieden wij u met de JSON-definities die u gebruiken kunt om te maken van een Azure Data Factory-pijplijn voor het kopiëren van gegevens. U kunt deze JSON-definities van de [Azure-portal](../data-factory/tutorial-copy-data-portal.md) of [Visual Studio](../data-factory/tutorial-copy-data-dot-net.md).

### <a name="source-linked-service-azure-storage-blob"></a>Gekoppelde bron-service (Azure Storage-blob)
````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-storage-gen1"></a>Doel van de gekoppelde service (Azure Data Lake Storage Gen1)
````
{
    "name": "AzureDataLakeStorageGen1LinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Storage Gen1 account with your access rights>",
            "dataLakeStoreUri": "https://<adlsg1_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Gegevensset voor invoer
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Gegevensset voor uitvoer
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStorageGen1LinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Pijplijn (kopieeractiviteit)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Zie voor meer informatie, [gegevens verplaatsen van Azure Storage-blob naar Azure Data Lake Storage Gen1 met Azure Data Factory](../data-factory/connector-azure-data-lake-store.md).

## <a name="reconstruct-the-data-files-in-azure-data-lake-storage-gen1"></a>De gegevensbestanden in Azure Data Lake Storage Gen1 reconstrueren
We zijn begonnen met een bestand dat 319 GB is, en deze naar beneden naar bestanden van de kleinere verbroken zodat deze kan worden overgedragen met behulp van de Azure Import/Export-service. Nu dat de gegevens zich in Azure Data Lake Storage Gen1, kunnen we het bestand naar de oorspronkelijke grootte reconstrueren. U kunt de volgende Azure PowerShell-cmdlets gebruiken om dit te doen.

```
# Login to our account
Connect-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext -SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adlsg1_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv"
````

## <a name="next-steps"></a>Volgende stappen
* [Gegevens beveiligen in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)
