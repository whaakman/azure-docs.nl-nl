---
title: COMPUTE omgevingen wordt ondersteund door Azure Data Factory | Microsoft Docs
description: Meer informatie over de compute-omgevingen die u in Azure Data Factory-pijplijnen (zoals Azure HDInsight) voor het transformeren of proces gebruiken kunt.
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shengc
ms.openlocfilehash: f242a8a15334818d83651cf0af55e8ec39bce212
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>COMPUTE omgevingen wordt ondersteund door Azure Data Factory
Dit artikel wordt uitgelegd verschillende berekeningsomgevingen waarmee u kunt gegevens verwerken of transformatie. Het bevat ook informatie over verschillende configuraties (op aanvraag versus bring uw eigen) die door Data Factory worden ondersteund bij het configureren van de gekoppelde services koppelt deze compute-omgevingen aan een Azure data factory.

De volgende tabel bevat een lijst van compute-omgevingen wordt ondersteund door de Data Factory en de activiteiten die kunnen worden uitgevoerd op deze. 

| Compute-omgeving                      | activities                               |
| ---------------------------------------- | ---------------------------------------- |
| [HDInsight-cluster op aanvraag](#azure-hdinsight-on-demand-linked-service) of [uw eigen HDInsight-cluster](#azure-hdinsight-linked-service) | [Hive](transform-data-using-hadoop-hive.md), [Pig](transform-data-using-hadoop-pig.md), [Spark](transform-data-using-spark.md), [MapReduce](transform-data-using-hadoop-map-reduce.md), [Hadoop Streaming](transform-data-using-hadoop-streaming.md) |
| [Azure Batch](#azure-batch-linked-service) | [Aangepaste](transform-data-using-dotnet-custom-activity.md) |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning-activiteiten: batchuitvoering en resources bijwerken](transform-data-using-machine-learning.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md) |
| [Azure SQL](#azure-sql-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Opgeslagen procedure](transform-data-using-stored-procedure.md) |

>  

## <a name="on-demand-compute-environment"></a>Op aanvraag compute-omgeving
In dit type configuratie, wordt de computeromgeving volledig beheerd door de Azure Data Factory-service. Deze is automatisch gemaakt door de Data Factory-service voordat een taak wordt verzonden om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt een gekoppelde service voor de omgeving compute op aanvraag maken, configureren en gedetailleerde instellingen voor het uitvoeren van taak Clusterbeheer en acties uitvoeren van de bootstrap.

> [!NOTE]
> De configuratie op aanvraag wordt momenteel alleen ondersteund voor Azure HDInsight-clusters.
>
> 

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight on demand-gekoppelde service
De Azure Data Factory-service kan automatisch maken van een HDInsight-cluster op aanvraag om gegevens te verwerken. Het cluster wordt gemaakt in dezelfde regio bevinden als het storage-account (de eigenschap linkedServiceName in de JSON) die zijn gekoppeld aan het cluster. Het opslagaccount moet een algemeen standaard Azure-opslagaccount. 

Let op het volgende **belangrijke** punten met betrekking tot HDInsight op aanvraag gekoppelde service:

* Het HDInsight-cluster op aanvraag wordt gemaakt onder uw Azure-abonnement. U bent het cluster in uw Azure-portal zien wanneer het cluster actief is en wordt uitgevoerd. 
* De logboeken voor taken die worden uitgevoerd op een HDInsight-cluster op aanvraag worden gekopieerd naar het opslagaccount die is gekoppeld aan het HDInsight-cluster. Het clusterUserName, clusterPassword, clusterSshUserName, clusterSshPassword gedefinieerd in de definitie van de gekoppelde service worden gebruikt voor aanmelding bij het cluster voor diepgaande probleemoplossing tijdens de levenscyclus van het cluster. 
* Worden in rekening gebracht alleen voor de tijd wanneer het HDInsight-cluster actief is en de taken die worden uitgevoerd.

> [!IMPORTANT]
> Het duurt meestal **20 minuten** of meer voor het inrichten van een Azure HDInsight-cluster op aanvraag.
>
> 

### <a name="example"></a>Voorbeeld
De volgende JSON definieert een service op aanvraag een gekoppelde HDInsight op basis van Linux. De Data Factory-service maakt automatisch een **op basis van Linux** HDInsight-cluster voor het verwerken van de activiteit vereist. 

```json
{
  "name": "HDInsightOnDemandLinkedService",
  "properties": {
    "type": "HDInsightOnDemand",
    "typeProperties": {
      "clusterType": "hadoop",
      "clusterSize": 1,
      "timeToLive": "00:15:00",
      "hostSubscriptionId": "<subscription ID>",
      "servicePrincipalId": "<service principal ID>",
      "servicePrincipalKey": {
        "value": "<service principal key>",
        "type": "SecureString"
      },
      "tenant": "<tenent id>",
      "clusterResourceGroup": "<resource group name>",
      "version": "3.6",
      "osType": "Linux",
      "linkedServiceName": {
        "referenceName": "AzureStorageLinkedService",
        "type": "LinkedServiceReference"
      }
    },
    "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
    }
  }
}
```

> [!IMPORTANT]
> Het HDInsight-cluster maakt een **standaardcontainer** in de blobopslag die u hebt opgegeven in de JSON (**linkedServiceName**). HDInsight verwijdert deze container niet wanneer het cluster wordt verwijderd. Dit gedrag is standaard. Met een gekoppelde on-demand HDInsight-service wordt er steeds een HDInsight-cluster gemaakt wanneer er een segment moet worden verwerkt, tenzij er een bestaand livecluster is (**timeToLive**). Het cluster wordt verwijderd wanneer het verwerken is voltooid. 
>
> Als u meer activiteit wordt uitgevoerd, ziet u veel containers in uw Azure-blobopslag. Als u deze niet nodig hebt voor het oplossen van problemen met taken, kunt u ze verwijderen om de opslagkosten te verlagen. De namen van deze containers volgen een patroon: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Gebruik hulpprogramma's zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) om containers in uw Azure-blobopslag te verwijderen.
>
> 

### <a name="properties"></a>Eigenschappen
| Eigenschap                     | Beschrijving                              | Vereist |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | De eigenschap type moet worden ingesteld op **HDInsightOnDemand**. | Ja      |
| De clustergrootte                  | Het aantal worker/gegevens knooppunten in het cluster. Het HDInsight-cluster wordt gemaakt met 2 hoofdknooppunten samen met het aantal worker-knooppunten die u voor deze eigenschap opgeeft. De knooppunten zijn met een grootte van Standard_D3 met 4 kernen, zodat een cluster met 4 worker-knooppunten 24 kernen duurt (4\*4 = 16 cores voor worker-knooppunten plus 2\*4 = 8 cores voor hoofdknooppunten). Zie [clusters in HDInsight met Hadoop, Spark en Kafka instellen](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie. | Ja      |
| linkedServiceName            | Gekoppelde Azure Storage-service moet worden gebruikt door het cluster op aanvraag voor het opslaan en verwerken van gegevens. Het HDInsight-cluster wordt gemaakt in dezelfde regio bevinden als dit Azure Storage-account. Voor Azure HDInsight geldt een beperking voor het totale aantal kernen dat u kunt gebruiken in elke Azure-regio die wordt ondersteund. Zorg ervoor dat er voldoende quota core in die Azure-regio om te voldoen aan de vereiste de clustergrootte. Raadpleeg voor meer informatie, [clusters in HDInsight met Hadoop, Spark en Kafka instellen](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)<p>Op dit moment kunt maken u een HDInsight-cluster op aanvraag die gebruikmaakt van een Azure Data Lake Store als de opslag niet. Als u wilt voor het opslaan van de resultaatgegevens van HDInsight worden verwerkt in een Azure Data Lake Store, gebruikt u een Kopieeractiviteit in de gegevens uit de Azure Blob Storage kopiëren naar Azure Data Lake Store. </p> | Ja      |
| clusterResourceGroup         | Het HDInsight-cluster wordt gemaakt in deze resourcegroep. | Ja      |
| TimeToLive                   | De toegestane niet-actieve tijd voor het HDInsight-cluster op aanvraag. Hiermee geeft u op hoelang het HDInsight-cluster op aanvraag na voltooiing van een activiteit die wordt uitgevoerd als er geen actieve taken in het cluster actief blijft. De minimaal toegestane waarde is 5 minuten (00: 05:00).<br/><br/>Bijvoorbeeld, als het uitvoeren van een activiteit 6 minuten duurt en timetolive is ingesteld op 5 minuten, blijft het cluster actief gedurende vijf minuten nadat de 6 minuten van de verwerking van de activiteit is uitgevoerd. Als een andere activiteit die wordt uitgevoerd met het venster 6 minuten wordt uitgevoerd, wordt het verwerkt door hetzelfde cluster.<br/><br/>Maken van een HDInsight-cluster op aanvraag is een dure bewerking (kan even duren) in dat geval gebruik deze instelling als die nodig zijn voor de prestaties van een gegevensfactory verbeteren door een HDInsight-cluster op aanvraag opnieuw te gebruiken.<br/><br/>Als u timetolive-waarde op 0 instelt, wordt het cluster wordt verwijderd zodra de uitvoering van de activiteit is voltooid. Dat, als u een hoge waarde instelt, het cluster niet actief is voor u zich aanmelden voor het probleem oplossen kan blijven doel, maar dit kan leiden tot hoge kosten. Het is daarom belangrijk dat u de juiste waarde op basis van uw behoeften.<br/><br/>Als de waarde van de eigenschap timetolive op de juiste wijze is ingesteld, kunnen meerdere pijplijnen het exemplaar van het HDInsight-cluster op aanvraag delen. | Ja      |
| clusterType                  | Het type van het HDInsight-cluster moet worden gemaakt. Toegestane waarden zijn 'hadoop' en 'spark'. Als niet wordt opgegeven, wordt de standaardwaarde hadoop. | Nee       |
| versie                      | Versie van het HDInsight-cluster. Als niet wordt opgegeven, wordt deze met behulp van de huidige versie van de HDInsight-gedefinieerde standaard. | Nee       |
| hostSubscriptionId           | De Azure-abonnement-ID die wordt gebruikt voor het maken van HDInsight-cluster. Als niet wordt opgegeven, wordt de abonnements-ID van de context van uw Azure-aanmelding. | Nee       |
| clusterNamePrefix           | Het voorvoegsel van de naam in de HDI-cluster, een tijdstempel wordt automatisch toegevoegd aan het einde van de naam van het cluster| Nee       |
| sparkVersion                 | De versie van spark als het clustertype is 'Spark' | Nee       |
| additionalLinkedServiceNames | Hiermee geeft u extra opslagaccounts voor het HDInsight gekoppelde service, zodat de Data Factory-service namens jou registreren kunt. Deze opslagaccounts moeten zich in dezelfde regio bevinden als het HDInsight-cluster wordt gemaakt in dezelfde regio als het opslagaccount dat is opgegeven door linkedServiceName. | Nee       |
| osType                       | Type besturingssysteem. Toegestane waarden zijn: Linux- en Windows (voor HDInsight 3.3). De standaardwaarde is Linux. | Nee       |
| hcatalogLinkedServiceName    | De naam van de Azure SQL gekoppelde service die verwijzen naar de HCatalog-database. Het HDInsight-cluster op aanvraag wordt gemaakt met behulp van de Azure SQL database als de metastore. | Nee       |
| connectVia                   | De Runtime-integratie moeten worden gebruikt voor het verzenden van de activiteiten bij deze gekoppelde HDInsight-service. Voor een gekoppelde HDInsight-service op aanvraag ondersteunt het alleen Azure-integratie Runtime. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee       |
| clusterUserName                   | De gebruikersnaam voor toegang tot het cluster. | Nee       |
| clusterPassword                   | Het wachtwoord in het type van een beveiligde tekenreeks toegang tot het cluster. | Nee       |
| clusterSshUserName         | De gebruikersnaam voor SSH is extern verbinding maken met het clusterknooppunt (voor Linux). | Nee       |
| clusterSshPassword         | Het wachtwoord in het type van een beveiligde tekenreeks te SSH verbinden op afstand van clusterknooppunt (voor Linux). | Nee       |


> [!IMPORTANT]
> HDInsight ondersteunt meerdere versies van Hadoop-cluster dat kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de distributie Hortonworks Data Platform HDP () en een reeks onderdelen die deel uitmaken van dit distributiepunt. De lijst met ondersteunde versies van HDInsight houdt wordt bijgewerkt met de nieuwste onderdelen voor Hadoop-ecosysteem en oplossingen. Zorg ervoor dat u altijd verwijzen naar de meest recente informatie van [ondersteund HDInsight-versie en het Type besturingssysteem](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) om te controleren of u gebruikmaakt van ondersteunde versie van HDInsight. 
>
> 

#### <a name="additionallinkedservicenames-json-example"></a>additionalLinkedServiceNames JSON-voorbeeld

```json
"additionalLinkedServiceNames": [{
    "referenceName": "MyStorageLinkedService2",
    "type": "LinkedServiceReference"          
}]
```

### <a name="service-principal-authentication"></a>Verificatie van service-principal

De gekoppelde On-Demand HDInsight-service vereist een service principal verificatie HDInsight-clusters maken namens jou. Service-principal-verificatie gebruiken, het registreren van een Toepassingsentiteit in Azure Active Directory (Azure AD) en het geven de **Inzender** rol van het abonnement of de resourcegroep waarin het HDInsight-cluster is gemaakt. Zie voor gedetailleerde stappen [portal gebruik maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Noteer de volgende waarden die u gebruikt voor het definiëren van de gekoppelde service:

- Toepassings-id
- Sleutel van toepassing 
- Tenant-id

Verificatie van de service-principal gebruiken door te geven van de volgende eigenschappen:

| Eigenschap                | Beschrijving                              | Vereist |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Geef de toepassing client-ID.     | Ja      |
| **servicePrincipalKey** | De sleutel van de toepassing opgeven.           | Ja      |
| **tenant**              | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja      |

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
          "hostSubscriptionId": "<subscription ID>",
          "servicePrincipalId": "<service principal ID>",
          "servicePrincipalKey": {
            "value": "<service principal key>",
            "type": "SecureString"
          },
          "tenant": "<tenent id>",
          "clusterResourceGroup": "<resource group name>",
          "version": "3.6",
          "osType": "Linux",
          "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
            },
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
            "additionalLinkedServiceNames": [{
                "referenceName": "MyStorageLinkedService2",
                "type": "LinkedServiceReference"          
            }]
        }
    },
      "connectVia": {
      "referenceName": "<name of Integration Runtime>",
      "type": "IntegrationRuntimeReference"
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
Zie de [grootten van virtuele Machines](../virtual-machines/linux/sizes.md) artikel voor string-waarden die u wilt opgeven voor de eigenschappen die worden vermeld in de vorige sectie. De waarden moeten voldoen aan de **CMDLETs & API's** waarnaar wordt verwezen in het artikel. Zoals u in het artikel ziet, op het gegevensknooppunt met een grootte van de grote (standaard) 7 GB geheugen heeft, wat mogelijk niet goed voor uw scenario. 

Als u maken van de hoofdknooppunten D4 grootte en worker-knooppunten wilt, geeft u **Standard_D4** als de waarde voor headNodeSize en dataNodeSize-eigenschappen. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Als u een onjuiste waarde voor deze eigenschappen opgeeft, wordt het volgende **fout:** cluster maken is mislukt. Uitzondering: Kan het cluster niet maken. Bewerking is mislukt met code 400. Cluster heeft status: 'Fout'. Bericht: 'PreClusterCreationValidationFailure'. Wanneer u deze fout ontvangt, controleert u of u de **CMDLET & API's** naam uit de tabel in de [grootten van virtuele Machines](../virtual-machines/linux/sizes.md) artikel.        

## <a name="bring-your-own-compute-environment"></a>Breng uw eigen compute-omgeving
Gebruikers kunnen met dit type configuratie, een al bestaande computeromgeving registreren als een gekoppelde service in de Data Factory. Door de gebruiker de computeromgeving wordt beheerd en de Data Factory-service gebruikt deze om de activiteiten worden uitgevoerd.

Dit type configuratie wordt ondersteund voor de volgende compute-omgevingen:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB, Azure SQL DW, SQL Server

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
        "userName": "username",
        "password": {
            "value": "passwordvalue",
            "type": "SecureString"
          },
        "linkedServiceName": {
              "referenceName": "AzureStorageLinkedService",
              "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
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
| connectVia        | De Runtime-integratie moeten worden gebruikt voor het verzenden van de activiteiten van deze gekoppelde service. U kunt Azure integratie Runtime of Self-hosted integratie-Runtime gebruiken. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee       |

> [!IMPORTANT]
> HDInsight ondersteunt meerdere versies van Hadoop-cluster dat kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de distributie Hortonworks Data Platform HDP () en een reeks onderdelen die deel uitmaken van dit distributiepunt. De lijst met ondersteunde versies van HDInsight houdt wordt bijgewerkt met de nieuwste onderdelen voor Hadoop-ecosysteem en oplossingen. Zorg ervoor dat u altijd verwijzen naar de meest recente informatie van [ondersteund HDInsight-versie en het Type besturingssysteem](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) om te controleren of u gebruikmaakt van ondersteunde versie van HDInsight. 
>

## <a name="azure-batch-linked-service"></a>Azure Batch gekoppeld-service

U kunt een gekoppelde Azure-Batch-service voor het registreren van een Batch-pool van virtuele machines (VM's) maken met een data factory. U kunt aangepaste activiteit met behulp van Azure Batch uitvoeren.

Zie de volgende onderwerpen als u niet bekend met Azure Batch-service bent:

* [Basisbeginselen van Azure Batch](../batch/batch-technical-overview.md) voor een overzicht van de Azure Batch-service.
* [Nieuwe AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1) cmdlet voor het maken van een Azure Batch-account (of) [Azure-portal](../batch/batch-account-create-portal.md) naar de Azure Batch-account maken met Azure-portal. Zie [met behulp van PowerShell voor het beheren van Azure Batch-Account](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) onderwerp voor gedetailleerde instructies over het gebruik van de cmdlet.
* [Nieuwe-AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) cmdlet om een Azure Batch-pool te maken.

### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureBatchLinkedService",
    "properties": {
      "type": "AzureBatch",
      "typeProperties": {
        "accountName": "batchaccount",
        "accessKey": {
          "type": "SecureString",
          "value": "access key"
        },
        "batchUri": "https://batchaccount.region.batch.azure.com",
        "poolName": "poolname",
        "linkedServiceName": {
          "referenceName": "StorageLinkedService",
          "type": "LinkedServiceReference"
        }
      },
      "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
  }
```


### <a name="properties"></a>Eigenschappen
| Eigenschap          | Beschrijving                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| type              | De eigenschap type moet worden ingesteld op **AzureBatch**. | Ja      |
| accountName       | Naam van de Azure Batch-account.         | Ja      |
| accessKey         | De toegangssleutel voor de Azure Batch-account.  | Ja      |
| batchUri          | URL naar uw Azure Batch-account in de indeling van https://*batchaccountname.region*. batch.azure.com. | Ja      |
| poolName          | Naam van de groep van virtuele machines.    | Ja      |
| linkedServiceName | Naam van de Azure Storage gekoppelde service die is gekoppeld aan deze gekoppelde Azure-Batch-service. Deze gekoppelde service wordt gebruikt voor tijdelijke bestanden die zijn vereist voor het uitvoeren van de activiteit. | Ja      |
| connectVia        | De Runtime-integratie moeten worden gebruikt voor het verzenden van de activiteiten van deze gekoppelde service. U kunt Azure integratie Runtime of Self-hosted integratie-Runtime gebruiken. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee       |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service
U maakt een Azure Machine Learning gekoppelde service voor het registreren van een Machine Learning-batch score-eindpunt met een data factory.

### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
      "type": "AzureML",
      "typeProperties": {
        "mlEndpoint": "https://[batch scoring endpoint]/jobs",
        "apiKey": {
            "type": "SecureString",
            "value": "access key"
        }
     },
     "connectVia": {
        "referenceName": "<name of Integration Runtime>",
        "type": "IntegrationRuntimeReference"
      }
    }
}
```

### <a name="properties"></a>Eigenschappen
| Eigenschap               | Beschrijving                              | Vereist                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| Type                   | De eigenschap type moet worden ingesteld op: **AzureML**. | Ja                                      |
| mlEndpoint             | De URL voor batchscores.                   | Ja                                      |
| apiKey                 | Het gepubliceerde werkruimtemodel API.     | Ja                                      |
| updateResourceEndpoint | De bron-URL bijwerken voor een Azure ML Web Service-eindpunt dat is gebruikt voor het bijwerken van de voorspellende webservice met getrainde model-bestand | Nee                                       |
| servicePrincipalId     | Geef de toepassing client-ID.     | Vereist als updateResourceEndpoint wordt opgegeven |
| servicePrincipalKey    | De sleutel van de toepassing opgeven.           | Vereist als updateResourceEndpoint wordt opgegeven |
| tenant                 | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Vereist als updateResourceEndpoint wordt opgegeven |
| connectVia             | De Runtime-integratie moeten worden gebruikt voor het verzenden van de activiteiten van deze gekoppelde service. U kunt Azure integratie Runtime of Self-hosted integratie-Runtime gebruiken. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee                                       |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde service
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute een Azure data factory-service. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

### <a name="example"></a>Voorbeeld

```json
{
    "name": "AzureDataLakeAnalyticsLinkedService",
    "properties": {
        "type": "AzureDataLakeAnalytics",
        "typeProperties": {
            "accountName": "adftestaccount",
            "dataLakeAnalyticsUri": "azuredatalakeanalytics URI",
            "servicePrincipalId": "service principal id",
            "servicePrincipalKey": {
                "value": "service principal key",
                "type": "SecureString"
            },
            "tenant": "tenant ID",
            "subscriptionId": "<optional, subscription id of ADLA>",
            "resourceGroupName": "<optional, resource group name of ADLA>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="properties"></a>Eigenschappen

| Eigenschap             | Beschrijving                              | Vereist                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| type                 | De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. | Ja                                      |
| accountName          | Azure Data Lake Analytics-accountnaam.  | Ja                                      |
| dataLakeAnalyticsUri | Azure Data Lake Analytics-URI.           | Nee                                       |
| subscriptionId       | Azure-abonnement-id                    | Nee (als niet wordt opgegeven, abonnement van de gegevensfactory wordt gebruikt). |
| resourceGroupName    | Naam van een Azure-resourcegroep                | Nee (als niet wordt opgegeven, brongroep van de gegevensfactory wordt gebruikt). |
| servicePrincipalId   | Geef de toepassing client-ID.     | Ja                                      |
| servicePrincipalKey  | De sleutel van de toepassing opgeven.           | Ja                                      |
| tenant               | De tenant-gegevens (domain name of tenant-ID) opgeven onder uw toepassing zich bevindt. U kunt deze ophalen door de muis in de rechterbovenhoek van de Azure portal. | Ja                                      |
| connectVia           | De Runtime-integratie moeten worden gebruikt voor het verzenden van de activiteiten van deze gekoppelde service. U kunt Azure integratie Runtime of Self-hosted integratie-Runtime gebruiken. Als niet wordt opgegeven, wordt de standaardwaarde Azure integratie Runtime. | Nee                                       |



## <a name="azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service
Een Azure SQL gekoppelde service maakt en gebruikt u deze met de [activiteit opgeslagen Procedure](transform-data-using-stored-procedure.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie [Azure SQL-Connector](connector-azure-sql-database.md#linked-service-properties) voor meer informatie over deze gekoppelde service.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Data Warehouse gekoppelde service
Een Azure SQL Data Warehouse gekoppelde service maakt en gebruikt u deze met de [activiteit opgeslagen Procedure](transform-data-using-stored-procedure.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie [Azure SQL Data Warehouse Connector](connector-azure-sql-data-warehouse.md#linked-service-properties) voor meer informatie over deze gekoppelde service.

## <a name="sql-server-linked-service"></a>SQL Server gekoppeld-service
U maakt een gekoppelde SQL-Server-service en deze gebruikt met de [activiteit opgeslagen Procedure](transform-data-using-stored-procedure.md) aan te roepen, een opgeslagen procedure uit een Data Factory-pijplijn. Zie [SQL Server-connector](connector-sql-server.md#linked-service-properties) voor meer informatie over deze gekoppelde service.

## <a name="azure-data-factory---naming-rules"></a>Azure Data Factory - naamgevingsregels
De volgende tabel bevat de naamgevingsregels voor Data Factory-artefacten.

| Naam                             | Uniekheid van de naam                          | De validatie wordt gecontroleerd                        |
| :------------------------------- | :--------------------------------------- | :--------------------------------------- |
| Data Factory                     | Uniek zijn voor Microsoft Azure. Namen zijn niet hoofdlettergevoelig, dat wil zeggen, `MyDF` en `mydf` verwijzen naar de dezelfde gegevensfactory. | <ul><li>Elke gegevensfactory is gekoppeld aan één Azure-abonnement.</li><li>Object-namen moeten beginnen met een letter of cijfer en mag alleen letters, cijfers en het streepje (-) bevatten.</li><li>Elk streepje (-) moet direct worden voorafgegaan en gevolgd door een letter of cijfer. Twee opeenvolgende streepjes zijn in containernamen niet toegestaan.</li><li>Naam mag 3-63 tekens lang zijn.</li></ul> |
| Gekoppelde services/tabellen/pijplijnen | Unieke met in een gegevensfactory. De namen zijn niet hoofdlettergevoelig. | <ul><li>Maximum aantal tekens in een tabelnaam: 260.</li><li>Objectnamen moeten beginnen met een letter, cijfer of een onderstrepingsteken (_).</li><li>Volgende tekens zijn niet toegestaan: '. ', '+ ','?', '/', ' < ', ' > "," * ","%","&",": ","\\"</li></ul> |
| Resourcegroep                   | Uniek zijn voor Microsoft Azure. De namen zijn niet hoofdlettergevoelig. | <ul><li>Maximum aantal tekens: 1000.</li><li>Naam mag letters, cijfers en de volgende tekens bevatten: '-', ' _ ',', 'en'. '</li></ul> |

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van de activiteiten voor gegevenstransformatie ondersteund door Azure Data Factory [transformeer gegevens](transform-data.md).
