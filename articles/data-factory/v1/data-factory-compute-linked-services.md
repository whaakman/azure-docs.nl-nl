---
title: COMPUTE-omgevingen die worden ondersteund door Azure Data Factory | Microsoft Docs
description: Meer informatie over de compute-omgevingen die u in Azure Data Factory-pijplijnen (zoals Azure HDInsight) voor het transformeren of verwerken gebruiken kunt.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 6877a7e8-1a58-4cfb-bbd3-252ac72e4145
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 0e0a249c53c90d3d8d03dcdb5fbb4f11f31c54df
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57545144"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>COMPUTE-omgevingen die worden ondersteund door Azure Data Factory
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [Compute gekoppelde services in](../compute-linked-services.md).

Dit artikel wordt uitgelegd van de compute-omgevingen die u kunt gebruiken om te verwerken en transformeren gegevens. Biedt ook meer informatie over verschillende configuraties (op aanvraag versus bring-your-own) die Data Factory ondersteunt bij het configureren van gekoppelde services die deze koppelen-omgevingen aan een Azure data factory COMPUTE.

De volgende tabel bevat een lijst met compute-omgevingen die worden ondersteund door Data Factory en de activiteiten die kunnen worden uitgevoerd op deze. 

| Compute-omgeving                      | Activiteiten                               |
| ---------------------------------------- | ---------------------------------------- |
| [On-demand Azure HDInsight-cluster](#azure-hdinsight-on-demand-linked-service) of [uw eigen HDInsight-cluster](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning-activiteiten: Batchuitvoering en resources bijwerken](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Opgeslagen procedureactiviteit](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>HDInsight-versies worden ondersteund in Data Factory
Azure HDInsight biedt ondersteuning voor meerdere versies van de Hadoop-cluster die u op elk gewenst moment kunt implementeren. Alle ondersteunde versies maakt een specifieke versie van de distributie van Hortonworks Data Platform (HDP) en een set onderdelen in de distributie. 

Microsoft werkt de lijst met ondersteunde versies van HDInsight met de nieuwste onderdelen van Hadoop-ecosysteem en oplossingen. Zie voor gedetailleerde informatie [ondersteund HDInsight-versies](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> Linux gebaseerde HDInsight versie 3.3 en met 31 juli 2017 buiten gebruik is gesteld. Data Factory versie 1 on-demand HDInsight gekoppelde services klanten tot 15 December 2017 gekregen, om te testen en een upgrade uitvoert naar een nieuwere versie van HDInsight. HDInsight op basis van Windows worden stopgezet op 31 juli 2018.
>
> 

### <a name="after-the-retirement-date"></a>Na de vervaldatum 

Na 15 December 2017:

- U kunt niet meer maken op basis van Linux HDInsight versie 3.3 (of eerdere versies) clusters met behulp van een on-demand HDInsight-service in Data Factory versie 1 gekoppeld. 
- Als de [ **osType** en **versie** eigenschappen](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) niet expliciet zijn opgegeven in de JSON-definitie voor een bestaande Data Factory versie 1 op aanvraag gekoppelde HDInsight-service , de standaardwaarde is gewijzigd van **versie 3.1, osType = Windows =** naar **versie =\<meest recente versie voor HDI-standaard\>(https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning), osType Linux =**.

Na 31 juli 2018:

- U kunt niet langer een willekeurige versie van Windows op basis van HDInsight-clusters maken met behulp van een gekoppelde HDInsight-service op aanvraag in Data Factory versie 1. 

### <a name="recommended-actions"></a>Aanbevolen acties 

- Om ervoor te zorgen dat u de nieuwste onderdelen van Hadoop-ecosysteem en oplossingen kunt gebruiken, werken de [ **osType** en **versie** eigenschappen](https://docs.microsoft.com/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) van betrokken Data Factory versie 1 op aanvraag Definities van de gekoppelde HDInsight-service naar nieuwere versies van HDInsight op basis van Linux (HDInsight 3.6). 
- Vóór 15 December 2017, test u Data Factory versie 1 Hive, Pig, MapReduce en Hadoop streaming activiteiten die verwijzen naar de betrokken gekoppelde service. Zorg ervoor dat ze compatibel met de nieuwe zijn **osType** en **versie** standaardwaarden (**versie 3.6 =**, **osType Linux =**) of de Typ dat u wilt naar bijwerken expliciete HDInsight-versie en besturingssysteem. 
  Zie voor meer informatie over de compatibiliteit, [migreren vanuit een HDInsight op basis van een Windows-cluster naar een cluster op basis van Linux](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) en [wat zijn de Hadoop-onderdelen en versies die beschikbaar met HDInsight?](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions). 
- Als u wilt doorgaan met behulp van een Data Factory versie 1-service op aanvraag HDInsight gekoppeld aan het maken van HDInsight op basis van een Windows-clusters, expliciet instellen **osType** naar **Windows** vóór 15 December 2017. Het is raadzaam om te migreren naar Linux gebaseerde HDInsight-clusters vóór 31 juli 2018. 
- Als u een gekoppelde HDInsight-service op aanvraag voor het uitvoeren van Data Factory versie 1 DotNet aangepaste activiteit, update de DotNet aangepaste activiteit JSON-definitie te gebruiken in plaats daarvan een Azure-Batch gekoppelde service. Zie voor meer informatie, [aangepaste activiteiten gebruiken in een Data Factory-pijplijn](https://docs.microsoft.com/azure/data-factory/v1/data-factory-use-custom-activities). 

> [!Note]
> Als u uw bestaande, bring-your-own cluster HDInsight gekoppeld apparaat in Data Factory versie 1 of een bring-your-own- en on-demand HDInsight gekoppelde service in Azure Data Factory, hoeft u geen actie is vereist. In deze scenario's, wordt al het ondersteuningsbeleid van meest recente versie van HDInsight-clusters afgedwongen. 
>
> 


## <a name="on-demand-compute-environment"></a>Op aanvraag compute-omgeving
In een op-verzoek-configuratie, wordt de compute-omgeving volledig beheerd door Data Factory. Data Factory maakt automatisch de compute-omgeving voordat een taak voor het verwerken van gegevens wordt verzonden. Wanneer de taak is voltooid, wordt in Data Factory de compute-omgeving verwijderd. 

U kunt een gekoppelde service voor een on-demand compute-omgeving maken. Gebruik van de gekoppelde service te configureren van de compute-omgeving, en voor het beheren van gebruik van gedetailleerde instellingen voor het uitvoeren van taak Clusterbeheer en acties uitvoeren van de bootstrap.

> [!NOTE]
> Op dit moment wordt de op-verzoek-configuratie alleen ondersteund voor HDInsight-clusters.
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight-gekoppelde service op aanvraag
Data Factory kan automatisch een Windows- of Linux gebaseerde HDInsight-cluster op aanvraag voor het verwerken van gegevens maken. Het cluster is gemaakt in dezelfde regio als het opslagaccount dat is gekoppeld aan het cluster. Gebruikt u de JSON- **linkedServiceName** eigenschap om het cluster te maken.

Let op het volgende *sleutel* punten over on-demand HDInsight gekoppelde service:

* De HDInsight-cluster op aanvraag wordt niet weergegeven in uw Azure-abonnement. De HDInsight-cluster op aanvraag namens u worden beheerd door de Data Factory-service.
* De logboeken voor taken die worden uitgevoerd op een on-demand HDInsight-cluster zijn gekopieerd naar het opslagaccount dat is gekoppeld aan het HDInsight-cluster. Voor toegang tot deze logboeken, in de Azure-portal, gaat u naar de **Details uitvoering van activiteit** deelvenster. Zie voor meer informatie, [bewaken en beheren van pijplijnen](data-factory-monitor-manage-pipelines.md).
* U betaalt alleen voor de tijd die het HDInsight-cluster actief is en de taken die worden uitgevoerd.

> [!IMPORTANT]
> Het duurt doorgaans *20 minuten* of meer voor het inrichten van een on-demand HDInsight-cluster.
>
> 

### <a name="example"></a>Voorbeeld
De volgende JSON definieert een service op aanvraag gekoppeld HDInsight op basis van Linux. Data Factory maakt automatisch een *op basis van Linux* HDInsight-cluster wanneer deze een gegevenssegment verwerkt. 

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
> Het HDInsight-cluster maakt een *standaardcontainer* in de Azure Blob-opslag die u in de JSON opgeeft **linkedServiceName** eigenschap. Standaard verwijdert HDInsight deze container niet wanneer het cluster wordt verwijderd. In een gekoppelde HDInsight-service op aanvraag, een HDInsight-cluster gemaakt wanneer er een segment moet worden verwerkt, tenzij er een bestaand livecluster is (**timeToLive**). Het cluster wordt verwijderd wanneer de verwerking is voltooid. 
>
> Als er meer segmenten worden verwerkt, ziet u veel containers in uw Blob storage. Als u niet de containers nodig hebt voor het oplossen van taken, is het raadzaam om de containers om de opslagkosten te verlagen te verwijderen. De namen van deze containers volgen een patroon: `adf<your Data Factory name>-<linked service name>-<date and time>`. U kunt een hulpprogramma zoals [Microsoft Opslagverkenner](https://storageexplorer.com/) om containers in Blob-opslag te verwijderen.
>
> 

### <a name="properties"></a>Properties
| Eigenschap                     | Description                              | Vereist |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | De eigenschap type ingesteld op **HDInsightOnDemand**. | Ja      |
| clusterSize                  | Het aantal worker- en gegevensknooppunten in het cluster. Het HDInsight-cluster is gemaakt met 2 hoofdknooppunten, naast het aantal worker-knooppunten die u voor deze eigenschap opgeeft. De knooppunten zijn van de grootte Standard_D3 met 4 kernen. Een cluster met 4-worker-knooppunten wordt 24 kernen (4\*4 = 16 cores voor worker-knooppunten, plus 2\*4 = 8 cores voor hoofdknooppunten). Zie voor meer informatie over de laag Standard_D3 [maken Linux gebaseerde Hadoop-clusters in HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). | Ja      |
| timeToLive                   | De toegestane niet-actieve tijd voor het HDInsight-cluster op aanvraag. Hiermee geeft u op hoelang het HDInsight-cluster op aanvraag blijft actief wanneer het uitvoeren van een activiteit is voltooid, als er geen andere actieve taken in het cluster.<br /><br />Bijvoorbeeld, als een activiteit uitvoert 6 minuten duurt en **timeToLive** is ingesteld op 5 minuten, de cluster blijft actief gedurende vijf minuten na de zes minuten van de verwerking van de activiteit die wordt uitgevoerd. Als een andere activiteit die wordt uitgevoerd in het venster 6 minuten wordt uitgevoerd, wordt verwerkt door hetzelfde cluster.<br /><br />Het maken van een on-demand HDInsight-cluster is een dure bewerking (het kan even duren). Gebruik deze instelling als die nodig zijn om prestaties te verbeteren van een data factory door opnieuw een on-demand HDInsight-cluster te gebruiken.<br /><br />Als u de **timeToLive** waarde die moet worden **0**, het cluster wordt verwijderd zodra de uitvoering van de activiteit is voltooid. Echter, als u een hoge waarde hebt ingesteld, het cluster kan Blijf op de niet-actieve, onnodig leidt tot hoge kosten. Het is belangrijk om in te stellen van de juiste waarde op basis van uw behoeften.<br /><br />Als de **timeToLive** waarde op de juiste wijze is ingesteld, wordt het exemplaar van de HDInsight-cluster op aanvraag door meerdere pijplijnen kunnen delen. | Ja      |
| versie                      | De versie van het HDInsight-cluster. Zie voor toegestane HDInsight-versies, [ondersteund HDInsight-versies](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#supported-hdinsight-versions). Als deze waarde niet is opgegeven, de [meest recente versie voor HDI-standaard](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning) wordt gebruikt. | Nee       |
| linkedServiceName            | De gekoppelde Azure Storage-service moet worden gebruikt door het cluster op aanvraag voor het opslaan en verwerken van gegevens. Het HDInsight-cluster is gemaakt in dezelfde regio als dit storage-account.<p>U kunt geen op dit moment een HDInsight-cluster op aanvraag die gebruikmaakt van Azure Data Lake Store als de opslag maken. Als u wilt voor het opslaan van de resulterende gegevens uit HDInsight verwerking in Data Lake Store, kunt u de Kopieeractiviteit gebruiken de gegevens van Blob-opslag kopiëren naar Data Lake Store. </p> | Ja      |
| additionalLinkedServiceNames | Hiermee geeft u een extra opslagaccounts voor de gekoppelde HDInsight-service. Data Factory registreert de storage-accounts uit uw naam. Deze opslagaccounts moeten zich in dezelfde regio bevinden als het HDInsight-cluster. Het HDInsight-cluster is gemaakt in dezelfde regio als het opslagaccount dat opgegeven door de **linkedServiceName** eigenschap. | Nee       |
| besturingssysteemtype                       | Het type besturingssysteem. Toegestane waarden zijn **Linux** en **Windows**. Als deze waarde niet is opgegeven, **Linux** wordt gebruikt.  <br /><br />Het is raadzaam met HDInsight op basis van Linux-clusters. De vervaldatum voor HDInsight in Windows is en met 31 juli 2018. | Nee       |
| hcatalogLinkedServiceName    | De naam van de gekoppelde Azure SQL-service die naar de HCatalog-database verwijst. De on-demand HDInsight-cluster is gemaakt met behulp van de SQL-database als de metastore. | Nee       |

#### <a name="example-linkedservicenames-json"></a>Voorbeeld: LinkedServiceNames JSON

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Geavanceerde eigenschappen
U kunt de volgende eigenschappen opgeven voor de gedetailleerde configuratie van de HDInsight-cluster op aanvraag:

| Eigenschap               | Description                              | Vereist |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Hiermee geeft u de configuratieparameters core (core-site.xml) voor het HDInsight-cluster moet worden gemaakt. | Nee       |
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
Als u wilt de grootte van de hoofd-, gegevens- en ZooKeeper-knooppunten opgeven, gebruikt u de volgende eigenschappen: 

| Eigenschap          | Description                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Hiermee stelt u de grootte van het hoofdknooppunt. De standaardwaarde is **Standard_D3**. Zie voor meer informatie, [knooppuntgrootten opgeven](#specify-node-sizes). | Nee       |
| dataNodeSize      | Hiermee stelt u de grootte van het gegevensknooppunt. De standaardwaarde is **Standard_D3**. | Nee       |
| zookeeperNodeSize | Hiermee stelt u de grootte van de ZooKeeper-knooppunt. De standaardwaarde is **Standard_D3**. | Nee       |

#### <a name="specify-node-sizes"></a>Knooppuntgrootten opgeven
Zie voor tekenreekswaarden die u voor de eigenschappen die worden beschreven in de voorgaande sectie opgeven moet [grootten van virtuele machines](../../virtual-machines/linux/sizes.md). De waarden moeten voldoen aan de cmdlets en API's waarnaar wordt verwezen in [grootten van virtuele machines](../../virtual-machines/linux/sizes.md). De grootte van de grote (standaard)-knooppunt is 7 GB geheugen. Dit is mogelijk niet voldoende is voor uw scenario. 

Als u de hoofdknooppunten D4-grootte en het worker-knooppunten maakt wilt, geeft u **Standard_D4** als de waarde voor de **headNodeSize** en **dataNodeSize** eigenschappen: 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Als u een onjuiste waarde voor deze eigenschappen instelt, ziet u mogelijk het volgende bericht:

  Kan geen cluster maken. Uitzondering: Kan niet worden voltooid het cluster niet maken. Bewerking is mislukt met code 400. Cluster heeft status: 'Fout'. Bericht: 'PreClusterCreationValidationFailure'. 
  
Als u dit bericht ziet, zorg ervoor dat u gebruikmaakt van de cmdlet en API-namen uit de tabel in [grootten van virtuele machines](../../virtual-machines/linux/sizes.md).  

> [!NOTE]
> Op dit moment ondersteunt Data Factory geen HDInsight-clusters met Data Lake Store als de primaire opslag gebruiken. Azure-opslag gebruiken als de primaire opslag voor HDInsight-clusters. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Bring-your-own-compute-omgeving
U kunt een bestaande compute-omgeving registreren als een gekoppelde service in Data Factory. U beheren de compute-omgeving. De Data Factory-service maakt gebruik van de compute-omgeving voor het uitvoeren van activiteiten.

Dit type configuratie wordt ondersteund voor de volgende omgevingen:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL Database, Azure SQL Data Warehouse, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight gekoppelde service
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

### <a name="properties"></a>Properties
| Eigenschap          | Description                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| type              | De eigenschap type ingesteld op **HDInsight**. | Ja      |
| clusterUri        | De URI van het HDInsight-cluster.        | Ja      |
| gebruikersnaam          | De naam van het gebruikersaccount dat u gebruikt om te verbinden met een bestaand HDInsight-cluster. | Ja      |
| wachtwoord          | Het wachtwoord voor het gebruikersaccount.   | Ja      |
| linkedServiceName | De naam van de gekoppelde storage-service die naar de Blob-opslag die wordt gebruikt door het HDInsight-cluster verwijst. <p>Momenteel kunt opgeven u niet dat een Data Lake Store gekoppelde service voor deze eigenschap. Als het HDInsight-cluster toegang tot Data Lake Store heeft, kunt u toegang tot gegevens in Data Lake Store via Hive of Pig-scripts. </p> | Ja      |

## <a name="azure-batch-linked-service"></a>Azure Batch gekoppelde service
U kunt een gekoppelde batchservice voor het registreren van een Batch-pool van virtuele machines (VM's) om een data factory te maken. U kunt Microsoft .NET aangepaste activiteiten uitvoeren met behulp van de Batch of de HDInsight.

Als u niet bekend bent met de Batch-service:

* Meer informatie over [basisbeginselen van Azure Batch](../../batch/batch-technical-overview.md).
* Meer informatie over de [New-AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet. Gebruik deze cmdlet voor het maken van een Batch-account. Of u kunt de Batch-account maken met behulp van de [Azure-portal](../../batch/batch-account-create-portal.md). Zie voor gedetailleerde informatie over het gebruik van de cmdlet [met behulp van PowerShell voor het beheren van een Batch-account](https://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx).
* Meer informatie over de [New-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet. Deze cmdlet gebruiken om een Batch-pool te maken.

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

Voor de **accountName** eigenschap toevoegen **.\< regionaam\>**  op de naam van uw batch-account. Bijvoorbeeld:

```json
"accountName": "mybatchaccount.eastus"
```

Een andere optie is voor de **batchUri** eindpunt. Bijvoorbeeld:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Properties
| Eigenschap          | Description                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| type              | De eigenschap type ingesteld op **AzureBatch**. | Ja      |
| accountName       | De naam van het Batch-account.         | Ja      |
| accessKey         | De toegangssleutel voor het Batch-account.  | Ja      |
| poolName          | De naam van de groep van virtuele machines.    | Ja      |
| linkedServiceName | De naam van de gekoppelde storage-service die is gekoppeld aan deze Batch gekoppelde service. Deze gekoppelde service wordt gebruikt voor tijdelijke bestanden die vereist om uit te voeren van de activiteit en zijn voor het opslaan van logboekbestanden voor het uitvoeren van activiteiten. | Ja      |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service
U kunt een Machine Learning gekoppelde service voor het registreren van een Machine Learning batch scoring-eindpunt om een data factory te maken.

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

### <a name="properties"></a>Properties
| Eigenschap   | Description                              | Vereist |
| ---------- | ---------------------------------------- | -------- |
| Type       | De eigenschap type ingesteld op **AzureML**. | Ja      |
| mlEndpoint | Het batch scoring-URL.                   | Ja      |
| ApiKey     | Het gepubliceerde werkruimtemodel API.     | Ja      |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde service
U kunt een Data Lake Analytics gekoppelde service om te koppelen van een Data Lake Analytics compute-service aan een Azure data factory maken. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

De volgende tabel bevat de algemene eigenschappen die worden gebruikt in de JSON-definitie:

| Eigenschap                 | Description                              | Vereist                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| type                 | De eigenschap type ingesteld op **AzureDataLakeAnalytics**. | Ja                                      |
| accountName          | De naam van de Data Lake Analytics-account.  | Ja                                      |
| dataLakeAnalyticsUri | De Data Lake Analytics-URI.           | Nee                                       |
| subscriptionId       | De Azure-abonnement-ID.                    | Nee<br /><br />(Indien niet opgegeven, wordt de data factory-abonnement gebruikt.) |
| resourceGroupName    | De naam van de Azure-resourcegroep.                | Nee<br /><br /> (Indien niet opgegeven, wordt de resourcegroep van de data factory gebruikt.) |

### <a name="authentication-options"></a>Verificatieopties
Voor uw Data Lake Analytics gekoppelde service, kunt u kiezen tussen verificatie met behulp van een service-principal of de referenties van een gebruiker.

#### <a name="service-principal-authentication-recommended"></a>Service-principal verificatie (aanbevolen)
Registreren voor het gebruik van service-principal verificatie, de Toepassingsentiteit van een in Azure Active Directory (Azure AD). Vervolgens Azure AD toegang verlenen tot Data Lake Store. Zie voor gedetailleerde stappen [Service-naar-serviceverificatie](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Toepassingssleutel 
* Tenant-id

Gebruik verificatie van service-principal door de volgende eigenschappen op te geven:

| Eigenschap                | Description                              | Vereist |
| :---------------------- | :--------------------------------------- | :------- |
| servicePrincipalId  | Van de toepassing client-ID.     | Ja      |
| servicePrincipalKey | Sleutel van de toepassing.           | Ja      |
| tenant              | De tenantgegevens van de (domain name of tenant-ID) waar uw toepassing zich bevindt. Als u deze informatie, de muisaanwijzer in de rechterbovenhoek van de Azure-portal. | Ja      |

**Voorbeeld: Service-principal verificatie**
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
Voor verificatie van de gebruiker referentie voor Data Lake Analytics, geeft u de volgende eigenschappen:

| Eigenschap          | Description                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| Autorisatie | In Data Factory-Editor selecteert u de **autoriseren** knop. Voer de referenties die de automatisch gegenereerde autorisatie-URL aan deze eigenschap wordt toegewezen. | Ja      |
| sessionId     | De OAuth-sessie-ID van de OAuth-autorisatie-sessie. Elke sessie-ID is uniek en kan slechts één keer worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u Data Factory-Editor gebruiken. | Ja      |

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

#### <a name="token-expiration"></a>Geldigheidsduur van het token
De autorisatiecode die u hebt gegenereerd door het selecteren van de **autoriseren** knop verloopt na een interval instellen. 

U ziet het volgende foutbericht weergegeven wanneer het verificatietoken is verlopen: 

  Referentie-bewerkingsfout: invalid_grant - AADSTS70002: Fout bij het valideren van referenties. AADSTS70008: De opgegeven toegang verlenen, is verlopen of ingetrokken. Traceer-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 15-12-2015 21:09:31Z

De volgende tabel ziet u accountwachtwoorden door gebruikersaccounttype: 

| Gebruikerstype                                | Verloopt na                            |
| :--------------------------------------- | :--------------------------------------- |
| Gebruikersaccounts die zijn *niet* beheerd door Azure AD (Hotmail, Live, enzovoort) | 12 uur.                                 |
| Gebruikersaccounts die *zijn* beheerd door Azure AD | 14 dagen na het laatste segment worden uitgevoerd. <br /><br />na 90 dagen als een segment dat gebaseerd op een gekoppelde OAuth-service wordt uitgevoerd ten minste eenmaal per 14 dagen. |

Autoriseren om te voorkomen of oplossen van deze fout, door het selecteren van de **autoriseren** knop wanneer het token is verlopen. De gekoppelde service vervolgens opnieuw. U kunt ook genereren waarden voor de **sessionId** en **autorisatie** eigenschappen programmatisch met behulp van de volgende code:

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

Zie voor meer informatie over de Data Factory-klassen die worden gebruikt in dit voorbeeld:
* [AzureDataLakeStoreLinkedService class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)
* [AzureDataLakeAnalyticsLinkedService class](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)
* [AuthorizationSessionGetResponse klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)

Voeg een verwijzing naar Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll voor de **WindowsFormsWebAuthenticationDialog** klasse. 

## <a name="azure-sql-linked-service"></a>Azure SQL gekoppelde service
U kunt maken van een gekoppelde SQL-service en deze gebruikt met de [opgeslagen-Procedureactiviteit](data-factory-stored-proc-activity.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. Zie voor meer informatie, [Azure SQL-connector](data-factory-azure-sql-connector.md#linked-service-properties).

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Data Warehouse gekoppelde service
U kunt een SQL Data Warehouse gekoppelde service maken en gebruiken met de [opgeslagen-Procedureactiviteit](data-factory-stored-proc-activity.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. Zie voor meer informatie, [Azure SQL Data Warehouse-connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties).

## <a name="sql-server-linked-service"></a>Gekoppelde SQL Server-service
U kunt een gekoppelde SQL Server-service maken en gebruiken met de [opgeslagen-Procedureactiviteit](data-factory-stored-proc-activity.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. Zie voor meer informatie, [SQL Server-connector](data-factory-sqlserver-connector.md#linked-service-properties).

