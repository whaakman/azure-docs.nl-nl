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
ms.date: 11/01/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 1547b5c3a5c629b85ff5fa9de6b39b25531d9ec9
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>COMPUTE omgevingen wordt ondersteund door Azure Data Factory
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [gekoppelde services gevonden in V2 Compute](../compute-linked-services.md).

Dit artikel wordt uitgelegd verschillende berekeningsomgevingen waarmee u kunt gegevens verwerken of transformatie. Het bevat ook informatie over verschillende configuraties (op aanvraag versus bring uw eigen) die door Data Factory worden ondersteund bij het configureren van de gekoppelde services koppelt deze compute-omgevingen aan een Azure data factory.

De volgende tabel bevat een lijst van compute-omgevingen wordt ondersteund door de Data Factory en de activiteiten die kunnen worden uitgevoerd op deze. 

| Compute-omgeving                      | activities                               |
| ---------------------------------------- | ---------------------------------------- |
| [HDInsight-cluster op aanvraag](#azure-hdinsight-on-demand-linked-service) of [uw eigen HDInsight-cluster](#azure-hdinsight-linked-service) | [DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop-Streaming](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](#azure-batch-linked-service) | [DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning-activiteiten: batchuitvoering en resources bijwerken](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL datawarehouse](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Opgeslagen procedure](data-factory-stored-proc-activity.md) |

## <a name="supported-hdinsight-versions-in-azure-data-factory"></a>Ondersteunde versies van HDInsight in Azure Data Factory
Azure HDInsight biedt ondersteuning voor meerdere versies van Hadoop-cluster die op elk gewenst moment kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de distributie Hortonworks Data Platform HDP () en een reeks onderdelen die deel uitmaken van dit distributiepunt. Microsoft houdt bijwerken van de lijst met ondersteunde versies van HDInsight nieuwste onderdelen voor Hadoop-ecosysteem en oplossingen te geven. Zie voor gedetailleerde informatie [ondersteunde versies van HDInsight](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

> [!IMPORTANT]
> De Linux gebaseerde HDInsight versie 3.3 buiten gebruik werd gesteld op 31 juli 2017. Data Factory v1 On-Demand HDInsight gekoppelde Services klanten gekregen tot en met 15 December 2017 om te testen en bijwerken naar een nieuwere versie van HDInsight. De HDInsight op basis van Windows wordt buiten gebruik worden gesteld op 31 juli 2018.
>
> 

**Wat gebeurt er na de datum buiten gebruik stellen** 

Na 15 December 2017:

- U wordt niet langer Linux gebaseerde HDInsight versie 3.3 (of eerdere versies) maken met behulp van gekoppelde On-Demand HDInsight-Service in Azure Data Factory-v1-clusters. 

- Als de [besturingssysteemtype en/of de eigenschap Version](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) zijn niet expliciet is opgegeven in de bestaande Azure Data Factory v1 On-Demand HDInsight gekoppelde Service JSON definities, de standaardwaarde wordt gewijzigd van **versie 3.1, besturingssysteemtype = = Windows** naar **versie 3.6, besturingssysteemtype = Linux =**.

Na 31 juli 2018:

- Niet meer mogelijk te maken van een willekeurige versie van Windows gebaseerde HDInsight-clusters met gekoppelde On-Demand HDInsight-Service in Azure Data Factory v1. 

 **Aanbevolen acties** 

- Update de [besturingssysteemtype en/of de eigenschap Version](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-compute-linked-services#azure-hdinsight-on-demand-linked-service) van de betrokken Azure Data Factory v1 gekoppelde On-Demand HDInsight-Service-definities voor de nieuwere op Linux gebaseerde HDInsight versies (HDInsight 3.6) om te controleren of u de meest recente Hadoop kunnen gebruiken ecosysteem onderdelen en oplossingen. 
- Voordat 15 December 2017 testen Azure Data Factory V1 Hive, Pig, MapReduce en Hadoop streaming van activiteiten die verwijzen naar de betrokken gekoppelde Service te controleren of ze compatibel zijn met de nieuwe *besturingssysteemtype* en/of  *Versie* standaardwaarde (versie 3.6, besturingssysteemtype = = Linux) of de expliciete HDInsight-versie en het besturingssysteemtype u naar bijwerkt. Raadpleeg voor meer informatie over de compatibiliteit van de [migreren vanaf een cluster met HDInsight op basis van Windows naar een cluster op basis van Linux](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-migrate-from-windows-to-linux) en [wat zijn de Hadoop-onderdelen en versies die beschikbaar met HDInsight?](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-component-versioning#hortonworks-release-notes-associated-with-hdinsight-versions) documentatie-webpagina's. 
- Expliciet ingesteld het besturingssysteemtype bij Windows voordat 15 December 2017 als u wilt doorgaan met het gebruik van Azure Data Factory v1On-Demand HDInsight gekoppelde Service maken op basis van Windows HDInsight-clusters. We raden echter nog steeds aan migreren naar Linux gebaseerde HDInsight-clusters voordat 31 juli 2018. 
- Werk de DotNet aangepaste activiteit JSON-definitie voor het gebruik van een gekoppelde Azure Batch-Service in plaats daarvan als u gekoppelde On-Demand HDInsight-Service gebruikt voor het uitvoeren van Azure Data Factory v1DotNet aangepaste activiteit. Meer informatie over de [aangepaste activiteiten gebruiken in een pijplijn Azure DataFactory](https://docs.microsoft.com/en-us/azure/data-factory/v1/data-factory-use-custom-activities) documentatie webpagina. 

>[!Note]
>Voor klanten die gebruikmaken van uw bestaande Bring Your Own Cluster (BYOC) gekoppelde HDInsight-Service in Azure Data Factory-v1- of personen die van BYOC gebruikmaakt en On-Demand HDInsightLinked Service in Azure Data Factory-v2 is de meest recente versie beleid ofAzure HDInsight ondersteuning clusters al wordt afgedwongen, is daarom geen actie vereist. 
>
> 


## <a name="on-demand-compute-environment"></a>Op aanvraag compute-omgeving
In dit type configuratie, wordt de computeromgeving volledig beheerd door de Azure Data Factory-service. Deze is automatisch gemaakt door de Data Factory-service voordat een taak wordt verzonden om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt een gekoppelde service voor de omgeving compute op aanvraag maken, configureren en gedetailleerde instellingen voor het uitvoeren van taak Clusterbeheer en acties uitvoeren van de bootstrap.

> [!NOTE]
> De configuratie op aanvraag wordt momenteel alleen ondersteund voor Azure HDInsight-clusters.
>
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight On-Demand gekoppelde Service
De Azure Data Factory-service kan automatisch maken van een Windows, Linux-gebaseerde bellen op HDInsight-cluster om gegevens te verwerken. Het cluster wordt gemaakt in dezelfde regio bevinden als het storage-account (de eigenschap linkedServiceName in de JSON) die zijn gekoppeld aan het cluster.

Let op het volgende **belangrijke** punten met betrekking tot HDInsight op aanvraag gekoppelde service:

* Er geen het HDInsight-cluster op aanvraag gemaakt in uw Azure-abonnement. de Azure Data Factory-service beheert het HDInsight-cluster op aanvraag namens jou.
* De logboeken voor taken die worden uitgevoerd op een HDInsight-cluster op aanvraag worden gekopieerd naar het opslagaccount die is gekoppeld aan het HDInsight-cluster. U kunt deze logboeken openen vanaf de Azure-portal in de **Details uitvoering van activiteit** blade. Zie [pijplijnen bewaken en beheren](data-factory-monitor-manage-pipelines.md) artikel voor meer informatie.
* Worden in rekening gebracht alleen voor de tijd wanneer het HDInsight-cluster actief is en de taken die worden uitgevoerd.

> [!IMPORTANT]
> Het duurt meestal **20 minuten** of meer voor het inrichten van een Azure HDInsight-cluster op aanvraag.
>
> 

### <a name="example"></a>Voorbeeld
De volgende JSON definieert een service op aanvraag een gekoppelde HDInsight op basis van Linux. De Data Factory-service maakt automatisch een **op basis van Linux** HDInsight-cluster bij het verwerken van een gegevenssegment. 

```json
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

Voor het gebruik van een HDInsight op basis van Windows-cluster instellen **besturingssysteemtype** naar **windows** of de eigenschap niet gebruiken als de standaardwaarde is: windows.  

> [!IMPORTANT]
> Het HDInsight-cluster maakt een **standaardcontainer** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight verwijdert deze container niet wanneer het cluster wordt verwijderd. Dit gedrag is standaard. Met een gekoppelde on-demand HDInsight-service wordt er steeds een HDInsight-cluster gemaakt wanneer er een segment moet worden verwerkt, tenzij er een bestaand livecluster is (**timeToLive**). Het cluster wordt verwijderd wanneer het verwerken is voltooid. 
>
> Naarmate er meer segmenten worden verwerkt, verschijnen er meer containers in uw Azure-blobopslag. Als u deze niet nodig hebt voor het oplossen van problemen met taken, kunt u ze verwijderen om de opslagkosten te verlagen. De namen van deze containers volgen een patroon: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Gebruik hulpprogramma's zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) om containers in uw Azure-blobopslag te verwijderen.
>
> 

### <a name="properties"></a>Eigenschappen
| Eigenschap                     | Beschrijving                              | Vereist |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | De eigenschap type moet worden ingesteld op **HDInsightOnDemand**. | Ja      |
| De clustergrootte                  | Het aantal worker/gegevens knooppunten in het cluster. Het HDInsight-cluster wordt gemaakt met 2 hoofdknooppunten samen met het aantal worker-knooppunten die u voor deze eigenschap opgeeft. De knooppunten zijn met een grootte van Standard_D3 met 4 kernen, zodat een cluster met 4 worker-knooppunten 24 kernen duurt (4\*4 = 16 cores voor worker-knooppunten plus 2\*4 = 8 cores voor hoofdknooppunten). Zie [maken Linux gebaseerde Hadoop-clusters in HDInsight](../../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie over de Standard_D3-laag. | Ja      |
| TimeToLive                   | De toegestane niet-actieve tijd voor het HDInsight-cluster op aanvraag. Hiermee geeft u op hoelang het HDInsight-cluster op aanvraag na voltooiing van een activiteit die wordt uitgevoerd als er geen actieve taken in het cluster actief blijft.<br/><br/>Bijvoorbeeld, als het uitvoeren van een activiteit 6 minuten duurt en timetolive is ingesteld op 5 minuten, blijft het cluster actief gedurende vijf minuten nadat de 6 minuten van de verwerking van de activiteit is uitgevoerd. Als een andere activiteit die wordt uitgevoerd met het venster 6 minuten wordt uitgevoerd, wordt het verwerkt door hetzelfde cluster.<br/><br/>Maken van een HDInsight-cluster op aanvraag is een dure bewerking (kan even duren) in dat geval gebruik deze instelling als die nodig zijn voor de prestaties van een gegevensfactory verbeteren door een HDInsight-cluster op aanvraag opnieuw te gebruiken.<br/><br/>Als u timetolive-waarde op 0 instelt, wordt het cluster wordt verwijderd zodra de uitvoering van de activiteit is voltooid. Dat, als u een hoge waarde instelt, kan het cluster niet-actieve onnodig wat resulteert in hoge kosten blijven. Het is daarom belangrijk dat u de juiste waarde op basis van uw behoeften.<br/><br/>Als de waarde van de eigenschap timetolive op de juiste wijze is ingesteld, kunnen meerdere pijplijnen het exemplaar van het HDInsight-cluster op aanvraag delen. | Ja      |
| Versie                      | Versie van het HDInsight-cluster. De standaardwaarde is 3.1 voor Windows-cluster en 3.2 voor Linux-cluster. | Nee       |
| linkedServiceName            | Gekoppelde Azure Storage-service moet worden gebruikt door het cluster op aanvraag voor het opslaan en verwerken van gegevens. Het HDInsight-cluster wordt gemaakt in dezelfde regio bevinden als dit Azure Storage-account.<p>Op dit moment kunt maken u een HDInsight-cluster op aanvraag die gebruikmaakt van een Azure Data Lake Store als de opslag niet. Als u wilt voor het opslaan van de resultaatgegevens van HDInsight worden verwerkt in een Azure Data Lake Store, gebruikt u een Kopieeractiviteit in de gegevens uit de Azure Blob Storage kopiëren naar Azure Data Lake Store. </p> | Ja      |
| additionalLinkedServiceNames | Hiermee geeft u extra opslagaccounts voor het HDInsight gekoppelde service, zodat de Data Factory-service namens jou registreren kunt. Deze opslagaccounts moeten zich in dezelfde regio bevinden als het HDInsight-cluster wordt gemaakt in dezelfde regio als het opslagaccount dat is opgegeven door linkedServiceName. | Nee       |
| besturingssysteemtype                       | Type besturingssysteem. Toegestane waarden zijn: (standaard) voor Windows en Linux | Nee       |
| hcatalogLinkedServiceName    | De naam van de Azure SQL gekoppelde service die verwijzen naar de HCatalog-database. Het HDInsight-cluster op aanvraag wordt gemaakt met behulp van de Azure SQL database als de metastore. | Nee       |

#### <a name="additionallinkedservicenames-json-example"></a>additionalLinkedServiceNames JSON-voorbeeld

```json
"additionalLinkedServiceNames": [
    "otherLinkedServiceName1",
    "otherLinkedServiceName2"
  ]
```

### <a name="advanced-properties"></a>Geavanceerde eigenschappen
U kunt ook de volgende eigenschappen voor de gedetailleerde configuratie van het HDInsight-cluster op aanvraag opgeven.

| Eigenschap               | Beschrijving                              | Vereist |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Hiermee geeft u de configuratieparameters core (zoals in core site.xml) voor het HDInsight-cluster moet worden gemaakt. | Nee       |
| hBaseConfiguration     | Hiermee geeft u de HBase-configuratieparameters (hbase-site.xml) voor het HDInsight-cluster. | Nee       |
| hdfsConfiguration      | Hiermee geeft u de HDFS-configuratieparameters (hdfs-site.xml) voor het HDInsight-cluster. | Nee       |
| hiveConfiguration      | Hiermee geeft u de hive-configuratieparameters (hive-site.xml) voor het HDInsight-cluster. | Nee       |
| mapReduceConfiguration | Hiermee geeft u de configuratieparameters van MapReduce (mapred-site.xml) voor het HDInsight-cluster. | Nee       |
| oozieConfiguration     | Hiermee geeft u de Oozie-configuratieparameters (oozie-site.xml) voor het HDInsight-cluster. | Nee       |
| stormConfiguration     | Hiermee geeft u de Storm-configuratieparameters (storm-site.xml) voor het HDInsight-cluster. | Nee       |
| yarnConfiguration      | Hiermee geeft u de Yarn-configuratieparameters (yarn-site.xml) voor het HDInsight-cluster. | Nee       |

#### <a name="example--on-demand-hdinsight-cluster-configuration-with-advanced-properties"></a>Voorbeeld: configuratie van de On-demand HDInsight-cluster met geavanceerde eigenschappen

```json
{
  "name": " HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
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
U kunt de grootte van de kop, gegevens en zookeeper-knooppunten met de volgende eigenschappen opgeven: 

| Eigenschap          | Beschrijving                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Hiermee geeft u de grootte van het hoofdknooppunt. De standaardwaarde is: Standard_D3. Zie de **knooppuntgrootten geven** sectie voor meer informatie. | Nee       |
| dataNodeSize      | Geeft de grootte van het gegevensknooppunt. De standaardwaarde is: Standard_D3. | Nee       |
| zookeeperNodeSize | Geeft de grootte van het knooppunt dierenverzorger. De standaardwaarde is: Standard_D3. | Nee       |

#### <a name="specifying-node-sizes"></a>Knooppuntgrootten opgeven
Zie de [grootten van virtuele Machines](../../virtual-machines/linux/sizes.md) artikel voor string-waarden die u wilt opgeven voor de eigenschappen die worden vermeld in de vorige sectie. De waarden moeten voldoen aan de **CMDLETs & API's** waarnaar wordt verwezen in het artikel. Zoals u in het artikel ziet, op het gegevensknooppunt met een grootte van de grote (standaard) 7 GB geheugen heeft, wat mogelijk niet goed voor uw scenario. 

Als u maken van de hoofdknooppunten D4 grootte en worker-knooppunten wilt, geeft u **Standard_D4** als de waarde voor headNodeSize en dataNodeSize-eigenschappen. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Als u een onjuiste waarde voor deze eigenschappen opgeeft, wordt het volgende **fout:** cluster maken is mislukt. Uitzondering: Kan het cluster niet maken. Bewerking is mislukt met code 400. Cluster heeft status: 'Fout'. Bericht: 'PreClusterCreationValidationFailure'. Wanneer u deze fout ontvangt, controleert u of u de **CMDLET & API's** naam uit de tabel in de [grootten van virtuele Machines](../../virtual-machines/linux/sizes.md) artikel.  

> [!NOTE]
> Azure Data Factory biedt momenteel geen ondersteuning voor HDInsight-clusters met behulp van Azure Data Lake Store als primaire archief. Azure Storage gebruiken als primaire winkel voor HDInsight-clusters. 
>
> 


## <a name="bring-your-own-compute-environment"></a>Breng uw eigen compute-omgeving
Gebruikers kunnen met dit type configuratie, een al bestaande computeromgeving registreren als een gekoppelde service in de Data Factory. Door de gebruiker de computeromgeving wordt beheerd en de Data Factory-service gebruikt deze om de activiteiten worden uitgevoerd.

Dit type configuratie wordt ondersteund voor de volgende compute-omgevingen:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB, Azure SQL DW, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight gekoppelde Service
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
| type              | De eigenschap type moet worden ingesteld op **HDInsight**. | Ja      |
| clusterUri        | De URI van het HDInsight-cluster.        | Ja      |
| gebruikersnaam          | Geef de naam van de gebruiker moet worden gebruikt voor het verbinding maken met een bestaand HDInsight-cluster. | Ja      |
| wachtwoord          | Wachtwoord voor het gebruikersaccount opgeven.   | Ja      |
| linkedServiceName | Naam van de gekoppelde Azure Storage-service die naar de Azure blob storage gebruikt door het HDInsight-cluster verwijst. <p>Op dit moment kunt opgeven u niet dat een Azure Data Lake Store gekoppelde service voor deze eigenschap. Als het HDInsight-cluster toegang tot de Data Lake Store heeft, kunt u toegang tot gegevens in Azure Data Lake Store van Hive/Pig-scripts. </p> | Ja      |

## <a name="azure-batch-linked-service"></a>Gekoppelde Service van Azure Batch
U kunt een gekoppelde Azure-Batch-service voor het registreren van een Batch-pool van virtuele machines (VM's) maken met een data factory. U kunt aangepaste .NET-activiteiten met behulp van Azure Batch of Azure HDInsight kunt uitvoeren.

Zie de volgende onderwerpen als u niet bekend met Azure Batch-service bent:

* [Basisbeginselen van Azure Batch](../../batch/batch-technical-overview.md) voor een overzicht van de Azure Batch-service.
* [Nieuwe AzureBatchAccount](https://msdn.microsoft.com/library/mt125880.aspx) cmdlet voor het maken van een Azure Batch-account (of) [Azure-portal](../../batch/batch-account-create-portal.md) naar de Azure Batch-account maken met Azure-portal. Zie [met behulp van PowerShell voor het beheren van Azure Batch-Account](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) onderwerp voor gedetailleerde instructies over het gebruik van de cmdlet.
* [Nieuwe-AzureBatchPool](https://msdn.microsoft.com/library/mt125936.aspx) cmdlet om een Azure Batch-pool te maken.

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

Append '**.\< regionaam\>**' op de naam van uw batch-account voor de **accountName** eigenschap. Voorbeeld:

```json
"accountName": "mybatchaccount.eastus"
```

Er is een andere optie voor het eindpunt batchUri zoals weergegeven in het volgende voorbeeld:

```json
"accountName": "adfteam",
"batchUri": "https://eastus.batch.azure.com",
```

### <a name="properties"></a>Eigenschappen
| Eigenschap          | Beschrijving                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| type              | De eigenschap type moet worden ingesteld op **AzureBatch**. | Ja      |
| Accountnaam       | Naam van de Azure Batch-account.         | Ja      |
| accessKey         | De toegangssleutel voor de Azure Batch-account.  | Ja      |
| Groepsnaam          | Naam van de groep van virtuele machines.    | Ja      |
| linkedServiceName | Naam van de Azure Storage gekoppelde service die is gekoppeld aan deze gekoppelde Azure-Batch-service. Deze gekoppelde service wordt gebruikt voor tijdelijke bestanden die zijn vereist voor het uitvoeren van de activiteit en opslaan van logboekbestanden voor het uitvoeren van de activiteit. | Ja      |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde Service
U maakt een Azure Machine Learning gekoppelde service voor het registreren van een Machine Learning-batch score-eindpunt met een data factory.

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
| Type       | De eigenschap type moet worden ingesteld op: **AzureML**. | Ja      |
| mlEndpoint | De URL voor batchscores.                   | Ja      |
| apiKey     | Het gepubliceerde werkruimtemodel API.     | Ja      |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde Service
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute een Azure data factory-service. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

De volgende tabel bevat beschrijvingen van de algemene eigenschappen die in de JSON-definitie. U kunt verder tussen service-principal en verificatie van gebruikersreferenties.

| Eigenschap                 | Beschrijving                              | Vereist                                 |
| ------------------------ | ---------------------------------------- | ---------------------------------------- |
| **type**                 | De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. | Ja                                      |
| **Accountnaam**          | Azure Data Lake Analytics-accountnaam.  | Ja                                      |
| **dataLakeAnalyticsUri** | Azure Data Lake Analytics-URI.           | Nee                                       |
| **abonnements-id**       | Azure-abonnement-id                    | Nee (als niet wordt opgegeven, abonnement van de gegevensfactory wordt gebruikt). |
| **resourceGroupName**    | Naam van een Azure-resourcegroep                | Nee (als niet wordt opgegeven, brongroep van de gegevensfactory wordt gebruikt). |

### <a name="service-principal-authentication-recommended"></a>Verificatie van de service principal (aanbevolen)
Registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) voor het gebruik van verificatie van de service-principal en wordt de toegang verlenen tot Data Lake Store. Zie voor gedetailleerde stappen [authentication Service-naar-serviceconnector](../../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:
* Toepassings-id
* Sleutel van toepassing 
* Tenant-id

Verificatie van de service-principal gebruiken door te geven van de volgende eigenschappen:

| Eigenschap                | Beschrijving                              | Vereist |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Geef de toepassing client-ID.     | Ja      |
| **servicePrincipalKey** | De sleutel van de toepassing opgeven.           | Ja      |
| **tenant**              | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja      |

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

### <a name="user-credential-authentication"></a>Verificatie van gebruikersreferenties
U kunt ook referentie gebruikersverificatie voor Data Lake Analytics gebruiken door te geven van de volgende eigenschappen:

| Eigenschap          | Beschrijving                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| **autorisatie** | Klik op de **autoriseren** in de Data Factory-Editor en voer uw referenties op waarmee de automatisch gegenereerde autorisatie-URL worden toegewezen aan deze eigenschap. | Ja      |
| **sessie-id**     | OAuth-sessie-ID van de OAuth-autorisatie-sessie. Elke sessie-ID is uniek en kan slechts één keer worden gebruikt. Deze instelling wordt automatisch gegenereerd wanneer u de Data Factory-Editor. | Ja      |

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
De autorisatiecode die u hebt gegenereerd met de **autoriseren** knop verloopt na enige tijd opnieuw. Zie de volgende tabel voor de vervaldatum tijdstippen voor verschillende soorten gebruikersaccounts. Mogelijk ziet u de volgende fout weergegeven wanneer de verificatie **-token verloopt**: referentie-bewerkingsfout: invalid_grant - AADSTS70002: fout bij het valideren van referenties. AADSTS70008: De toegewezen toegangsmachtiging is verlopen of ingetrokken. Traceer-ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 correlatie-ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 tijdstempel: 2015-12-15 21:09:31Z

| Gebruikerstype                                | Verloopt na                            |
| :--------------------------------------- | :--------------------------------------- |
| Gebruikersaccounts die niet worden beheerd door Azure Active Directory (@hotmail.com, @live.com, enz.) | 12 uur                                 |
| Gebruikersaccounts die worden beheerd door Azure Active Directory (AAD) | het is 14 dagen na het laatste segment worden uitgevoerd. <br/><br/>negentig dagen weergegeven, als een segment op basis van de gekoppelde service op basis van OAuth ten minste eenmaal in de 14 dagen wordt uitgevoerd. |

Autoriseren om deze fout voorkomen/oplossen, met behulp van de **autoriseren** knop wanneer de **-token verloopt** en de gekoppelde service implementeren. U kunt ook de waarden voor genereren **sessionId** en **autorisatie** eigenschappen programmatisch met code als volgt:

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

Zie [AzureDataLakeStoreLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), en [AuthorizationSessionGetResponse klasse](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) onderwerpen voor meer informatie over de Data Factory-klassen die in de code wordt gebruikt. Voeg een verwijzing naar: Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll voor de klasse WindowsFormsWebAuthenticationDialog. 

## <a name="azure-sql-linked-service"></a>Azure SQL gekoppelde Service
Een Azure SQL gekoppelde service maakt en gebruikt u deze met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie [Azure SQL-Connector](data-factory-azure-sql-connector.md#linked-service-properties) voor meer informatie over deze gekoppelde service.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL gekoppelde Service van datawarehouse
Een Azure SQL Data Warehouse gekoppelde service maakt en gebruikt u deze met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie [Azure SQL Data Warehouse Connector](data-factory-azure-sql-data-warehouse-connector.md#linked-service-properties) voor meer informatie over deze gekoppelde service.

## <a name="sql-server-linked-service"></a>Gekoppelde Service van SQL Server
U maakt een gekoppelde SQL-Server-service en deze gebruikt met de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie [SQL Server-connector](data-factory-sqlserver-connector.md#linked-service-properties) voor meer informatie over deze gekoppelde service.

