---
title: COMPUTE omgevingen wordt ondersteund door Azure Data Factory | Microsoft Docs
description: Meer informatie over de compute-omgevingen die u in Azure Data Factory-pijplijnen (zoals Azure HDInsight) voor het transformeren of proces gebruiken kunt.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 410fb74d8f8ec6196bbd4cc19cc97704649b75c9
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>COMPUTE omgevingen wordt ondersteund door Azure Data Factory
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [Compute-gekoppelde services in versie 2](../compute-linked-services.md).

Dit artikel wordt uitgelegd van de compute-omgevingen die u kunt gegevens verwerken of transformatie. Bovendien wordt informatie over verschillende configuraties (op aanvraag versus bring your own) dat Data Factory ondersteunt bij het configureren van gekoppelde services die deze koppeling-omgevingen aan een Azure data factory COMPUTE.

De volgende tabel bevat een lijst van de compute-omgevingen die worden ondersteund door Data Factory en de activiteiten die kunnen worden uitgevoerd op deze. 

| Compute-omgeving                      | Activiteiten                               |
| ---------------------------------------- | ---------------------------------------- |
| [Azure HDInsight-cluster op aanvraag](#azure-hdinsight-on-demand-linked-service) of [uw eigen HDInsight-cluster](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning-activiteiten: batchuitvoering en resources bijwerken](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Opgeslagen procedureactiviteit](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>HDInsight-versies ondersteund in de Data Factory
Azure HDInsight biedt ondersteuning voor meerdere versies van Hadoop-cluster die u op elk gewenst moment kunt implementeren. Alle ondersteunde versies maakt een specifieke versie van de distributie Hortonworks Data Platform HDP () en een set onderdelen in de distributie. 

De lijst met ondersteunde versies van HDInsight door Microsoft wordt bijgewerkt met de nieuwste onderdelen voor Hadoop-ecosysteem en oplossingen. Zie voor gedetailleerde informatie [versies van HDInsight ondersteund](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> Linux gebaseerde HDInsight versie 3.3 buiten gebruik werd gesteld 31 juli 2017. Data Factory versie 1 op aanvraag HDInsight gekoppelde services klanten gekregen tot en met 15 December 2017 om te testen en bijwerken naar een nieuwere versie van HDInsight. HDInsight op basis van Windows wordt 31 juli 2018 ingetrokken.
>
> 

### <a name="after-the-retirement-date"></a>Na de vervaldatum 

Na 15 December 2017:

- U kunt niet langer maken Linux gebaseerde HDInsight versie 3.3 (of eerdere versies) gekoppelde service in de Data Factory versie 1 van clusters met behulp van een HDInsight op aanvraag. 
- Als de [ **besturingssysteemtype** en **versie** eigenschappen](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) niet expliciet zijn opgegeven in de JSON-definitie voor een bestaande Gegevensfactory versie 1 op aanvraag een gekoppelde HDInsight-service , de standaardwaarde is gewijzigd van **versie 3.1, besturingssysteemtype = Windows =** naar **versie =\<meest recente versie van de HDI standaard\>(https://docs.microsoft.com/azure/hdinsight/ hdinsight-component-Versioning#hadoop-Components-Available-with-different-hdinsight-Versions) besturingssysteemtype Linux =**.

Na 31 juli 2018:

- U kunt niet langer een willekeurige versie van Windows gebaseerde HDInsight-clusters maken met behulp van een gekoppelde HDInsight-service op aanvraag in de Data Factory versie 1. 

### <a name="recommended-actions"></a>Aanbevolen acties 

- Om ervoor te zorgen dat u de nieuwste onderdelen voor Hadoop-ecosysteem en oplossingen kunt gebruiken, werken de [ **besturingssysteemtype** en **versie** eigenschappen](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) van betrokken Data Factory versie 1 op aanvraag Definities van de gekoppelde HDInsight-service naar nieuwere versies van Linux gebaseerde HDInsight (HDInsight 3.6). 
- Vóór 15 December 2017 Data Factory versie 1 Hive, Pig, MapReduce en Hadoop streaming activiteiten die verwijzen naar de betrokken gekoppelde service te testen. Zorg ervoor dat ze compatibel met de nieuwe zijn **besturingssysteemtype** en **versie** standaardwaarden (**versie 3.6 =**, **besturingssysteemtype Linux =**) of de expliciete HDInsight-versie en OS typt u een upgrade naar. 
  Zie voor meer informatie over de compatibiliteit, [migreren vanaf een cluster met HDInsight op basis van Windows naar een cluster op basis van Linux](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) en [wat zijn de Hadoop-onderdelen en versies die beschikbaar met HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions). 
- Om door te gaan met behulp van een Data Factory-service voor een gekoppelde HDInsight versie 1 op aanvraag maken op basis van Windows HDInsight-clusters expliciet instellen **besturingssysteemtype** naar **Windows** vóór 15 December 2017. Het is raadzaam om te migreren naar Linux gebaseerde HDInsight-clusters voordat 31 juli 2018. 
- Als u van een gekoppelde HDInsight-service op aanvraag gebruikmaakt voor het uitvoeren van de Data Factory versie 1 DotNet aangepaste activiteit, update de DotNet aangepaste activiteit JSON-definitie te gebruiken in plaats daarvan een Azure-Batch gekoppelde service. Zie voor meer informatie [aangepaste activiteiten gebruiken in een Data Factory-pijplijn](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities). 

> [!Note]
> Als u uw bestaande, bring your own cluster HDInsight gekoppeld apparaat in de Data Factory versie 1 of een bring your own en on-demand gekoppelde HDInsight-service in Azure Data Factory versie 2, hoeft u geen actie is vereist. In deze scenario's, is het meest recente versie ondersteuningsbeleid van HDInsight-clusters al afgedwongen. 
>
> 


## <a name="on-demand-compute-environment"></a>Op aanvraag compute-omgeving
In een configuratie op aanvraag beheert Data Factory volledig de compute-omgeving. Data Factory maakt automatisch de compute-omgeving voordat u een taak voor het verwerken van gegevens wordt ingediend. Wanneer de taak is voltooid, verwijdert Data Factory de compute-omgeving. 

U kunt een gekoppelde service voor een omgeving op aanvraag compute maken. De gekoppelde service gebruiken om de compute-omgeving te configureren en beheren van gedetailleerde instellingen voor het uitvoeren van taak Clusterbeheer en acties uitvoeren van de bootstrap.

> [!NOTE]
> Op dit moment wordt wordt de configuratie op aanvraag alleen ondersteund voor HDInsight-clusters.
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight on demand-gekoppelde service
Data Factory kunt automatisch maken van een Windows- of Linux-op-demand HDInsight-cluster voor het verwerken van gegevens. Het cluster wordt gemaakt in dezelfde regio bevinden als het opslagaccount dat is gekoppeld aan het cluster. Gebruik de JSON **linkedServiceName** eigenschap om het cluster te maken.

Let op het volgende *sleutel* punten met betrekking tot HDInsight op aanvraag gekoppelde service:

* Het HDInsight-cluster op aanvraag wordt niet weergegeven in uw Azure-abonnement. De Data Factory-service beheert het HDInsight-cluster op aanvraag namens jou.
* De logboeken voor taken die worden uitgevoerd op een HDInsight-cluster op aanvraag worden gekopieerd naar het opslagaccount dat is gekoppeld aan het HDInsight-cluster. Voor toegang tot deze logboeken, in de Azure portal, gaat u naar de **Details uitvoering van activiteit** deelvenster. Zie voor meer informatie [bewaken en beheren van pijplijnen](data-factory-monitor-manage-pipelines.md).
* Worden in rekening gebracht alleen voor de tijd die het HDInsight-cluster actief is en de taken die worden uitgevoerd.

> [!IMPORTANT]
> Het duurt meestal *20 minuten* of meer voor het inrichten van een HDInsight-cluster op aanvraag.
>
> 

### <a name="example"></a>Voorbeeld
De volgende JSON definieert een service op aanvraag een gekoppelde HDInsight op basis van Linux. Data Factory maakt automatisch een *op basis van Linux* HDInsight-cluster bij de verwerking van een gegevenssegment. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.6",
            "osType": "Linux",
            "clusterSize": 1,
            "timeToLive": "00:05:00",            
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

> [!IMPORTANT]
> Het HDInsight-cluster maakt een *standaardcontainer* in de Azure-blobopslag die u in de JSON opgeeft **linkedServiceName** eigenschap. Standaard deze container op HDInsight niet verwijderd wanneer het cluster is verwijderd. In een gekoppelde HDInsight-service op aanvraag, een HDInsight-cluster wordt gemaakt telkens wanneer een segment worden verwerkt, moet tenzij er een bestaand livecluster (**timeToLive**). Het cluster wordt verwijderd zodra de verwerking is voltooid. 
>
> Als er meer segmenten worden verwerkt, ziet u veel containers in uw blobopslag. Als u de containers hoeft voor het oplossen van taken, kunt u mogelijk wilt verwijderen van de containers om de opslagkosten te verlagen. De namen van deze containers volgen een patroon: `adf<your Data Factory name>-<linked service name>-<date and time>`. U kunt een hulpprogramma zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) containers in blobopslag te verwijderen.
>
> 

### <a name="properties"></a>Eigenschappen
| Eigenschap                     | Beschrijving                              | Vereist |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | De eigenschap type ingesteld op **HDInsightOnDemand**. | Ja      |
| De clustergrootte                  | Het aantal worker en gegevens knooppunten in het cluster. Het HDInsight-cluster wordt gemaakt met 2 hoofdknooppunten naast het aantal worker-knooppunten die u voor deze eigenschap opgeeft. De knooppunten zijn met een grootte van Standard_D3 met 4 kernen. Een cluster met 4-worker-knooppunten duurt 24 kernen (4\*4 = 16 cores voor worker-knooppunten plus 2\*4 = 8 cores voor hoofdknooppunten). Zie voor meer informatie over de laag Standard_D3 [maken Linux gebaseerde Hadoop-clusters in HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). | Ja      |
| timeToLive                   | De toegestane niet-actieve tijd voor het HDInsight-cluster op aanvraag. Hiermee geeft u op hoelang het HDInsight-cluster op aanvraag blijft actief wanneer het uitvoeren van een activiteit is voltooid, als er geen actieve taken in het cluster.<br /><br />Bijvoorbeeld, als een activiteit uitgevoerd 6 minuten duurt en **timeToLive** is ingesteld op 5 minuten, de cluster blijft actief gedurende vijf minuten na het 6 minuten van de verwerking van de activiteit die wordt uitgevoerd. Als een andere activiteit die wordt uitgevoerd in het venster 6 minuten wordt uitgevoerd, wordt het verwerkt door hetzelfde cluster.<br /><br />Maken van een HDInsight-cluster op aanvraag is een dure bewerking (dit kan even duren). Gebruik deze instelling zo nodig voor het verbeteren van de prestaties van een gegevensfactory door een HDInsight-cluster op aanvraag opnieuw te gebruiken.<br /><br />Als u de **timeToLive** van waarde naar **0**, het cluster wordt verwijderd zodra de activiteit uitgevoerd is voltooid. Echter, als u een hoge waarde instelt, het cluster kan blijven inactief onnodig wat resulteert in hoge kosten. Het is belangrijk om in te stellen van de juiste waarde op basis van uw behoeften.<br /><br />Als de **timeToLive** waarde juist is ingesteld, meerdere pijplijnen het exemplaar van het HDInsight-cluster op aanvraag kunnen delen. | Ja      |
| versie                      | De versie van het HDInsight-cluster. Zie voor toegestane HDInsight versies [versies van HDInsight ondersteund](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). Als deze waarde niet wordt opgegeven, de [meest recente versie van de HDI standaard](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hadoop-components-available-with-different-hdinsight-versions) wordt gebruikt. | Nee       |
| linkedServiceName            | De gekoppelde Azure Storage-service moet worden gebruikt door het cluster op aanvraag voor het opslaan en verwerken van gegevens. Het HDInsight-cluster wordt gemaakt in dezelfde regio bevinden als dit opslagaccount.<p>Op dit moment kunt maken u een HDInsight-cluster op aanvraag die gebruikmaakt van Azure Data Lake Store als de opslag niet. Als u wilt voor het opslaan van de resultaatgegevens van HDInsight worden verwerkt in Data Lake Store, gebruikt u Kopieeractiviteit de gegevens uit Blob storage kopiëren naar Data Lake Store. </p> | Ja      |
| additionalLinkedServiceNames | Hiermee geeft u een extra opslagaccounts voor de gekoppelde HDInsight-service. Data Factory registreert de storage-accounts namens jou. Deze opslagaccounts moeten zich in dezelfde regio bevinden als het HDInsight-cluster. Het HDInsight-cluster is gemaakt in dezelfde regio als het opslagaccount dat opgegeven door de **linkedServiceName** eigenschap. | Nee       |
| osType                       | Het type besturingssysteem. Toegestane waarden zijn **Linux** en **Windows**. Als deze waarde niet wordt opgegeven, **Linux** wordt gebruikt.  <br /><br />Het is raadzaam met behulp van Linux gebaseerde HDInsight-clusters. De intrekkingsdatum voor HDInsight in Windows is 31 juli 2018. | Nee       |
| hcatalogLinkedServiceName    | De naam van de Azure SQL gekoppelde service die naar de HCatalog-database verwijst. Het HDInsight-cluster op aanvraag wordt gemaakt met behulp van de SQL-database als de metastore. | Nee       |

#### <a name="example-linkedservicenames-json"></a>Voorbeeld: LinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Geavanceerde eigenschappen
U kunt de volgende eigenschappen opgeven voor de gedetailleerde configuratie van het HDInsight-cluster op aanvraag:

| Eigenschap               | Beschrijving                              | Vereist |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Hiermee geeft u de configuratieparameters core (core site.xml) voor het HDInsight-cluster moet worden gemaakt. | Nee       |
| hBaseConfiguration     | Hiermee geeft u de HBase-configuratieparameters (hbase-site.xml) voor het HDInsight-cluster. | Nee       |
| hdfsConfiguration      | Hiermee geeft u de HDFS-configuratieparameters (hdfs-site.xml) voor het HDInsight-cluster. | Nee       |
| hiveConfiguration      | Hiermee geeft u de Hive-configuratieparameters (hive-site.xml) voor het HDInsight-cluster. | Nee       |
| mapReduceConfiguration | Hiermee geeft u de configuratieparameters van MapReduce (mapred-site.xml) voor het HDInsight-cluster. | Nee       |
| oozieConfiguration     | Hiermee geeft u de Oozie-configuratieparameters (oozie-site.xml) voor het HDInsight-cluster. | Nee       |
| stormConfiguration     | Hiermee geeft u de Storm-configuratieparameters (storm-site.xml) voor het HDInsight-cluster. | Nee       |
| yarnConfiguration      | Hiermee geeft u de YARN-configuratieparameters (yarn-site.xml) voor het HDInsight-cluster. | Nee       |

#### <a name="example-on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Voorbeeld: On-demand HDInsight-clusterconfiguratie met geavanceerde eigenschappen

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "version": "3.6",
      "osType": "Linux",
      "clusterSize": 16,
      "timeToLive": "01:30:00",
      "linkedServiceName": "adfods1",
      "coreConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "hiveConfiguration": {
        "templeton.mapper.memory.mb": "5000"
      },
      "mapReduceConfiguration": {
        "mapreduce.reduce.java.opts": "-Xmx4000m",
        "mapreduce.map.java.opts": "-Xmx4000m",
        "mapreduce.map.memory.mb": "5000",
        "mapreduce.reduce.memory.mb": "5000",
        "mapreduce.job.reduce.slowstart.completedmaps": "0.8"
      },
      "yarnConfiguration": {
        "yarn.app.mapreduce.am.resource.mb": "5000",
        "mapreduce.map.memory.mb": "5000"
      },
      "additionalLinkedServiceNames": [
        "datafeeds",
        "adobedatafeed"
      ]
    }
  }
}
```

### <a name="node-sizes"></a>Knooppuntgrootten
Als u wilt de grootte van head, gegevens en ZooKeeper-knooppunten opgeven, gebruikt u de volgende eigenschappen: 

| Eigenschap          | Beschrijving                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Hiermee stelt u de grootte van het hoofdknooppunt. De standaardwaarde is **Standard_D3**. Zie voor meer informatie [knooppuntgrootten opgeven](#specify-node-sizes). | Nee       |
| dataNodeSize      | Hiermee stelt u de grootte van het gegevensknooppunt. De standaardwaarde is **Standard_D3**. | Nee       |
| zookeeperNodeSize | Hiermee stelt u de grootte van het knooppunt ZooKeeper. De standaardwaarde is **Standard_D3**. | Nee       |

#### <a name="specify-node-sizes"></a>Knooppuntgrootten opgeven
Zie voor tekenreekswaarden die u voor de eigenschappen die worden beschreven in de vorige sectie opgeven moet [grootten van virtuele machines](../../virtual-machines/linux/sizes.md). De waarden moeten voldoen aan de cmdlets en API's waarnaar wordt verwezen in [grootten van virtuele machines](../../virtual-machines/linux/sizes.md). De gegevensgrootte van het knooppunt van grote (standaard) heeft 7 GB geheugen. Dit is mogelijk niet voldoende is voor uw scenario. 

Als u maken van de hoofdknooppunten D4 grootte en worker-knooppunten wilt, geeft u **Standard_D4** als de waarde voor de **headNodeSize** en **dataNodeSize** eigenschappen: 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Als u een onjuiste waarde voor deze eigenschappen instelt, ziet u mogelijk het volgende bericht:

  Cluster maken is mislukt. Uitzondering: Kan het cluster niet maken. Bewerking is mislukt met code 400. Cluster heeft status: 'Fout'. Bericht: 'PreClusterCreationValidationFailure'. 
  
Als u dit bericht ziet, controleert u of u de cmdlet en API-namen uit de tabel in [grootten van virtuele machines](../../virtual-machines/linux/sizes.md).  

> [!NOTE]
> Data Factory ondersteunt momenteel geen HDInsight-clusters die gebruikmaken van Data Lake Store als de primaire winkel. Azure Storage gebruiken als de primaire winkel voor HDInsight-clusters. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Bring your own compute-omgeving
U kunt een bestaande omgeving voor compute registreren als een gekoppelde service in de Data Factory. U kunt de compute-omgeving beheren. De Data Factory-service gebruikt de compute-omgeving activiteiten uitvoeren.

Dit type configuratie wordt ondersteund voor de volgende compute-omgevingen:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL Database, Azure SQL datawarehouse, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure gekoppelde HDInsight-service
U kunt een gekoppelde HDInsight-service voor het registreren van uw eigen HDInsight-cluster met Data Factory maken.

### <a name="example"></a>Voorbeeld

```json
{
  "name": "HDInsightLinkedService",
  "properties": {
    "type": "HDInsight",
    "typeProperties": {
      "clusterUri": " https://<hdinsightclustername>.azurehdinsight.net/",
      "userName": "admin",
      "password": "<password>",
      "linkedServiceName": "MyHDInsightStoragelinkedService"
    }
  }
}
```

### <a name="properties"></a>Eigenschappen
| Eigenschap          | Beschrijving                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| type              | De eigenschap type ingesteld op **HDInsight**. | Ja      |
| clusterUri        | De URI van het HDInsight-cluster.        | Ja      |
| gebruikersnaam          | De naam van het gebruikersaccount dat verbinding maakt met een bestaand HDInsight-cluster. | Ja      |
| wachtwoord          | Het wachtwoord voor het gebruikersaccount.   | Ja      |
| linkedServiceName | De naam van de gekoppelde storage-service die naar de blobopslag die wordt gebruikt door het HDInsight-cluster verwijst. <p>Op dit moment kunt opgeven u niet dat een Data Lake Store gekoppelde service voor deze eigenschap. Als het HDInsight-cluster toegang tot Data Lake Store heeft, kunt u toegang tot gegevens in Data Lake Store via Hive of Pig-scripts. </p> | Ja      |

## <a name="azure-batch-linked-service"></a>Azure Batch gekoppeld-service
U kunt een gekoppelde batchservice voor het registreren van een Batch-pool van virtuele machines (VM's) aan een gegevensfactory maken. U kunt Microsoft .NET aangepaste activiteiten uitvoeren met behulp van de Batch of de HDInsight.

Als u niet bekend met de Batch-service bent:

* Meer informatie over [basisbeginselen van Azure Batch](../../batch/batch-technical-overview.md).
* Meer informatie over de [nieuw AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet. Gebruik deze cmdlet voor het maken van een Batch-account. U kunt het Batch-account maken met behulp van de [Azure-portal](../../batch/batch-account-create-portal.md). Zie voor gedetailleerde informatie over het gebruik van de cmdlet [met behulp van PowerShell voor het beheren van een Batch-account](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx).
* Meer informatie over de [New-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet. Gebruik deze cmdlet voor het maken van een Batch-pool.

### <a name="example"></a>Voorbeeld

```json
{
  "name": "AzureBatchLinkedService",
  "properties": {
    "type": "AzureBatch",
    "typeProperties": {
      "accountName": "<Azure Batch account name>",
      "accessKey": "<Azure Batch account key>",
      "poolName": "<Azure Batch pool name>",
      "linkedServiceName": "<Specify associated storage linked service reference here>"
    }
  }
}
```

Voor de **accountName** -eigenschap toevoegen **.\< regionaam\>**  op de naam van uw batch-account. Bijvoorbeeld:

```json
"accountName": "mybatchaccount.eastus"
```

Een andere optie is om de **batchUri** eindpunt. Bijvoorbeeld:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Eigenschappen
| Eigenschap          | Beschrijving                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| type              | De eigenschap type ingesteld op **AzureBatch**. | Ja      |
| accountName       | De naam van het Batch-account.         | Ja      |
| accessKey         | De toegangssleutel voor het Batch-account.  | Ja      |
| poolName          | De naam van de groep van virtuele machines.    | Ja      |
| linkedServiceName | De naam van de gekoppelde storage-service die is gekoppeld aan deze Batch van de gekoppelde service. Deze gekoppelde service wordt gebruikt voor gefaseerde installatie van de bestanden die vereist zijn voor het uitvoeren van de activiteit en voor het opslaan van logboekbestanden voor het uitvoeren van activiteit. | Ja      |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service
U kunt een Machine Learning gekoppelde service voor het registreren van een Machine Learning-batch score-eindpunt op een gegevensfactory maken.

### <a name="example"></a>Voorbeeld

```json
{
  "name": "AzureMLLinkedService",
  "properties": {
    "type": "AzureML",
    "typeProperties": {
      "mlEndpoint": "https://[batch scoring endpoint]/jobs",
      "apiKey": "<apikey>"
    }
  }
}
```

### <a name="properties"></a>Eigenschappen
| Eigenschap   | Beschrijving                              | Vereist |
| ---------- | ---------------------------------------- | -------- |
| Type       | De eigenschap type ingesteld op **AzureML**. | Ja      |
| mlEndpoint | De URL voor batchscores.                   | Ja      |
| apiKey     | Het gepubliceerde werkruimtemodel API.     | Ja      |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde service
U kunt een Data Lake Analytics gekoppelde service om te koppelen van een Data Lake Analytics compute-service aan een Azure data factory maken. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

De volgende tabel beschrijft de algemene eigenschappen die worden gebruikt in de JSON-definitie:

| Eigenschap                 | Beschrijving                              | Vereist                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| type                 | De eigenschap type ingesteld op **AzureDataLakeAnalytics**. | Ja                                      |
| accountName          | De naam van de Data Lake Analytics-account.  | Ja                                      |
| dataLakeAnalyticsUri | De Data Lake Analytics-URI.           | Nee                                       |
| subscriptionId       | De Azure-abonnement-ID.                    | Nee<br /><br />(Als niet wordt opgegeven, wordt de data factory-abonnement gebruikt.) |
| resourceGroupName    | De naam van de Azure-resourcegroep.                | Nee<br /><br /> (Als niet wordt opgegeven, wordt de data factory-resourcegroep gebruikt.) |

### <a name="authentication-options"></a>Verificatieopties
Voor uw service Data Lake Analytics is gekoppeld, kunt u kiezen tussen verificatie met behulp van een service-principal of een gebruikersreferenties.

#### <a name="service-principal-authentication-recommended"></a>Verificatie van de service principal (aanbevolen)
Service principal om verificatie te gebruiken, registreert u een Toepassingsentiteit in Azure Active Directory (Azure AD). Verleen toegang van Azure AD naar Data Lake Store. Zie voor gedetailleerde stappen [authentication Service-naar-serviceconnector](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Sleutel van toepassing 
* Tenant-id

Verificatie van de service-principal gebruiken door te geven van de volgende eigenschappen:

| Eigenschap                | Beschrijving                              | Vereist |
| :---------------------- | :--------------------------------------- | :------- |
| servicePrincipalId  | De toepassing client-ID.     | Ja      |
| servicePrincipalKey | De sleutel van de toepassing.           | Ja      |
| tenant              | De gegevens van de tenant (domain name of tenant-ID) waar de toepassing zich bevindt. Als u deze informatie, de muisaanwijzer in de rechterbovenhoek van de Azure portal. | Ja      |

**Voorbeeld: Service-principal-verificatie**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="user-credential-authentication"></a>Verificatie van gebruikersreferenties
Voor verificatie van gebruikersreferenties voor Data Lake Analytics, geeft u de volgende eigenschappen:

| Eigenschap          | Beschrijving                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| Autorisatie | Selecteer in Data Factory-Editor de **autoriseren** knop. Voer de referenties waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja      |
| sessie-id     | De OAuth-sessie-ID van de OAuth-autorisatie-sessie. Elke sessie-ID is uniek en kan slechts één keer worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u Data Factory-Editor. | Ja      |

**Voorbeeld: Verificatie van gebruikersreferenties**
```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
            "authorization": "<authcode>",
            "sessionId": "<session ID>", 
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        }
    }
}
```

#### <a name="token-expiration"></a>Verlopen van het token
De autorisatiecode die u hebt gegenereerd door het selecteren van de **autoriseren** knop verloopt na een bepaalde periode. 

U ziet het volgende foutbericht weergegeven wanneer het verificatietoken is verlopen: 

  Referentie-bewerkingsfout: invalid_grant - AADSTS70002: fout bij het valideren van referenties. AADSTS70008: De toegewezen toegangsmachtiging is verlopen of ingetrokken. Traceer-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 2015-12-15 21:09:31Z

De volgende tabel ziet u accountwachtwoorden door gebruikersaccounttype: 

| Gebruikerstype                                | Verloopt na                            |
| :--------------------------------------- | :--------------------------------------- |
| Gebruikersaccounts die zijn *niet* beheerd door Azure AD (Hotmail, Live, enzovoort) | 12 uur.                                 |
| Gebruikersaccounts die *zijn* beheerd door Azure AD | het is 14 dagen na het laatste segment worden uitgevoerd. <br /><br />negentig dagen weergegeven, als een segment dat gebaseerd op een gekoppelde op basis van het OAuth-service wordt uitgevoerd ten minste eenmaal in de 14 dagen. |

Opnieuw autoriseren door te selecteren om te voorkomen of oplossen van deze fout, de **autoriseren** knop wanneer het token is verlopen. Implementeer vervolgens opnieuw de gekoppelde service. U kunt ook genereren waarden voor de **sessionId** en **autorisatie** eigenschappen programmatisch met behulp van de volgende code:

```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```

Zie voor meer informatie over de Data Factory-klassen die worden gebruikt in dit codevoorbeeld:
* [AzureDataLakeStoreLinkedService class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
* [AzureDataLakeAnalyticsLinkedService class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* [AuthorizationSessionGetResponse klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)

Voeg een verwijzing naar Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll voor de **WindowsFormsWebAuthenticationDialog** klasse. 

## <a name="azure-sql-linked-service"></a>Azure SQL gekoppelde service
U kunt een SQL gekoppelde service maken en deze gebruikt met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie voor meer informatie [Azure SQL-connector](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Data Warehouse gekoppelde service
U kunt een SQL Data Warehouse gekoppelde service maken en deze gebruikt met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie voor meer informatie [Azure SQL Data Warehouse connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties).

## <a name="sql-server-linked-service"></a>SQL Server gekoppeld-service
U kunt een SQL Server gekoppelde service maken en deze gebruikt met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie voor meer informatie [SQL Server-connector](data-factory-sqlserver-connector.md#linked-service-properties).

