---
title: Grote hoeveelheden gegevens uploaden naar Data Lake Store met behulp van offline methoden | Microsoft Docs
description: "Het hulpprogramma AdlCopy gebruiken om gegevens te kopiëren van Azure Storage-blobs naar Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 45321f6a-179f-4ee4-b8aa-efa7745b8eb6
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/01/2017
ms.author: nitinme
ms.openlocfilehash: 744759968706e0a2c9fe8c1c153f44cc958e31b8
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="use-the-azure-importexport-service-for-offline-copy-of-data-to-data-lake-store"></a>Gebruik de Azure Import/Export-service voor offline kopiëren van gegevens naar Data Lake Store
In dit artikel leert u hoe u kunt kopiëren van grote gegevenssets (> 200 GB) in een Azure Data Lake Store met behulp van methoden van offline-exemplaar, zoals de [Azure Import/Export-service](../storage/common/storage-import-export-service.md). Het bestand dat wordt gebruikt als voorbeeld in dit artikel is bijzonder 339,420,860,416 bytes of ongeveer 319 GB op schijf. Laten we dit bestand 319GB.tsv aanroepen.

De Azure Import/Export-service helpt u om over te brengen grote hoeveelheden gegevens veiliger naar Azure Blob-opslag door back-upfunctie harde schijven naar een Azure-datacenter.

## <a name="prerequisites"></a>Vereisten
Voordat u begint, moet u het volgende hebben:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure storage-account**.
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="preparing-the-data"></a>Voorbereiden van de gegevens
Voordat u de Import/Export-service, verbreekt u het bestand kan worden overgedragen **in kopieën die kleiner zijn dan 200 GB zijn** in grootte. Het hulpprogramma voor importeren werkt niet met bestanden die groter zijn dan 200 GB. In deze zelfstudie wordt het bestand splitsen segmenten van 100 GB. U kunt dit doen met behulp van [Cygwin](https://cygwin.com/install.html). Cygwin biedt ondersteuning voor Linux-opdrachten. In dit geval gebruiken de volgende opdracht:

    split -b 100m 319GB.tsv

De splitsbewerking maakt bestanden met de volgende namen.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Bereid u voor schijven met gegevens
Volg de instructies in [via de Azure Import/Export-service](../storage/common/storage-import-export-service.md) (onder de **voorbereiden van uw schijven** sectie) voor het voorbereiden van de vaste schijven. Hier volgt de algemene volgorde:

1. Schaft u een harde schijf die voldoet aan de vereisten voor de Azure Import/Export-service moet worden gebruikt.
2. Identificeer waar de gegevens worden gekopieerd nadat deze is verzonden naar het Azure datacenter Azure storage-account.
3. Gebruik de [Azure-hulpprogramma voor importeren/exporteren](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), een opdrachtregelprogramma. Hier volgt een voorbeeld-codefragment die laat zien hoe het hulpprogramma te gebruiken.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Zie [via de Azure Import/Export-service](../storage/common/storage-import-export-service.md) voor meer voorbeelden codefragmenten.
4. De voorgaande opdracht maakt een journal-bestand op de opgegeven locatie. Dit logboek-bestand gebruiken voor het maken van een import-taak van de [Azure-portal](https://portal.azure.com).

## <a name="create-an-import-job"></a>Een importtaak maken
U kunt nu een import-taak maken met behulp van de instructies in [via de Azure Import/Export-service](../storage/common/storage-import-export-service.md) (onder de **de Import-taak maken** sectie). Voor deze taak importeren met andere informatie, bieden ook het journaalbestand dat is gemaakt tijdens het voorbereiden van de schijfstations.

## <a name="physically-ship-the-disks"></a>De schijven fysiek verzenden
U kunt nu de schijven voor een Azure-datacenter fysiek verzenden. Er, is via de gegevens gekopieerd naar de Azure Storage-blobs die u hebt opgegeven tijdens het maken van de import-taak. Ook bij het maken van de taak kunt als u hebt gekozen om de traceringsgegevens later, u nu gaat u terug naar uw import-taak en bijwerken het volgnummer.

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a>Gegevens kopiëren van Azure Storage-blobs naar Azure Data Lake Store
Nadat de toont de status van de import-taak voltooid, kunt u controleren of de gegevens zijn beschikbaar in de Azure Storage-blobs die u heeft opgegeven. Vervolgens kunt u een aantal methoden voor het verplaatsen van die gegevens van de blobs met Azure Data Lake Store. Zie voor alle beschikbare opties voor het uploaden van gegevens, [ophalen van gegevens in Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

In deze sectie bieden wij u met de JSON-definities die u gebruiken kunt voor het maken van een Azure Data Factory-pijplijn voor het kopiëren van gegevens. U kunt deze definities van de JSON van de [Azure-portal](../data-factory/v1/data-factory-copy-activity-tutorial-using-azure-portal.md), of [Visual Studio](../data-factory/v1/data-factory-copy-activity-tutorial-using-visual-studio.md), of [Azure PowerShell](../data-factory/v1/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Bron gekoppelde service (Azure Storage-blob)
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

### <a name="target-linked-service-azure-data-lake-store"></a>Doel van de gekoppelde service (Azure Data Lake Store)
````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Invoer gegevensset
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
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
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
Zie voor meer informatie [verplaatsen van gegevens van Azure Storage-blob naar Azure Data Lake Store met Azure Data Factory](../data-factory/connector-azure-data-lake-store.md).

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a>Reconstrueer de gegevensbestanden in Azure Data Lake Store
We met een bestand dat is 319 GB en heeft deze niet actief in kleinere bestanden zodat deze kan worden overgedragen met behulp van de Azure Import/Export-service is gestart. Nu dat de gegevens zich in Azure Data Lake Store, kunnen we het bestand naar de oorspronkelijke grootte opnieuw opgebouwd. U kunt de volgende cmldts van Azure PowerShell gebruiken om dit te doen.

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
