---
title: COMPUTE-omgevingen die worden ondersteund door Azure Data Factory | Microsoft Docs
description: Meer informatie over de compute-omgevingen die u in Azure Data Factory-pijplijnen (zoals Azure HDInsight) voor het transformeren of verwerken gebruiken kunt.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: douglasl
ms.openlocfilehash: 5e620b03f5588369fc73a62f2019d857766596fd
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321939"
---
# <a name="compute-environments-supported-by-azure-data-factory"></a>COMPUTE-omgevingen die worden ondersteund door Azure Data Factory
In dit artikel vindt u verschillende rekenomgevingen die u kunt gebruiken voor gegevens verwerken en transformeren. Het biedt ook meer informatie over de verschillende configuraties (op aanvraag en voeg uw eigen) ondersteund door Data Factory bij het configureren van gekoppelde services koppelt deze compute-omgevingen aan een Azure data factory.

De volgende tabel bevat een lijst met compute-omgevingen wordt ondersteund door Data Factory en de activiteiten die kunnen worden uitgevoerd op deze. 

| Compute-omgeving                                          | activities                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [On-demand HDInsight-cluster](#azure-hdinsight-on-demand-linked-service) of [uw eigen HDInsight-cluster](#azure-hdinsight-linked-service) | [Hive](transform-data-using-hadoop-hive.md), [Pig](transform-data-using-hadoop-pig.md), [Spark](transform-data-using-spark.md), [MapReduce](transform-data-using-hadoop-map-reduce.md), [Hadoop Streaming](transform-data-using-hadoop-streaming.md) |
| [Azure Batch](#azure-batch-linked-service)                   | [Aangepaste](transform-data-using-dotnet-custom-activity.md)     |
| [Azure Machine Learning](#azure-machine-learning-linked-service) | [Machine Learning-activiteiten: Batchuitvoering en resources bijwerken](transform-data-using-machine-learning.md) |
| [Azure Data Lake Analytics](#azure-data-lake-analytics-linked-service) | [Data Lake Analytics U-SQL](transform-data-using-data-lake-analytics.md) |
| [Azure SQL](#azure-sql-database-linked-service), [Azure SQL Data Warehouse](#azure-sql-data-warehouse-linked-service), [SQL Server](#sql-server-linked-service) | [Opgeslagen procedure](transform-data-using-stored-procedure.md) |
| [Azure Databricks](#azure-databricks-linked-service)         | [Notebook](transform-data-databricks-notebook.md), [Jar](transform-data-databricks-jar.md), [Python](transform-data-databricks-python.md) |

>  

## <a name="on-demand-hdinsight-compute-environment"></a>On-demand HDInsight-compute-omgeving
In dit type configuratie kan wordt de computeromgeving volledig beheerd door de Azure Data Factory-service. Er wordt automatisch gemaakt door de Data Factory-service voordat een taak wordt verzonden om gegevens te verwerken en verwijderd wanneer de taak is voltooid. U kunt een gekoppelde service voor de on-demand compute-omgeving maken, configureren en gedetailleerde instellingen voor taakuitvoering, Clusterbeheer en acties uitvoeren van de bootstrap.

> [!NOTE]
> De configuratie op aanvraag wordt momenteel alleen ondersteund voor Azure HDInsight-clusters. Azure Databricks biedt ook ondersteuning voor on-demand-taken met taak-clusters, raadpleeg dan [Azure databricks gekoppelde service](#azure-databricks-linked-service) voor meer informatie.

## <a name="azure-hdinsight-on-demand-linked-service"></a>Azure HDInsight-gekoppelde service op aanvraag
De Azure Data Factory-service kan automatisch maken van een on-demand HDInsight-cluster om gegevens te verwerken. Het cluster is gemaakt in dezelfde regio als het opslagaccount (met de eigenschap linkedServiceName in de JSON) die zijn gekoppeld aan het cluster. Het opslagaccount moet een algemeen standaard Azure storage-account. 

Let op het volgende **belangrijk** punten over on-demand HDInsight gekoppelde service:

* De on-demand HDInsight-cluster wordt gemaakt onder uw Azure-abonnement. U staat op het cluster in uw Azure-portal zien wanneer het cluster actief is en wordt uitgevoerd. 
* De logboeken voor taken die worden uitgevoerd op een on-demand HDInsight-cluster zijn gekopieerd naar het opslagaccount dat is gekoppeld aan het HDInsight-cluster. Het clusterUserName, clusterPassword, clusterSshUserName, clusterSshPassword gedefinieerd in de definitie van de gekoppelde service worden gebruikt voor aanmelding bij het cluster voor het grondig oplossen van problemen tijdens de levenscyclus van het cluster. 
* U betaalt alleen voor de tijd waarop het HDInsight-cluster actief is en de taken die worden uitgevoerd.
* U kunt een **scriptactie** gekoppelde service met de Azure HDInsight op aanvraag.  

> [!IMPORTANT]
> Het duurt doorgaans **20 minuten** of meer voor het inrichten van een Azure HDInsight-cluster op aanvraag.

### <a name="example"></a>Voorbeeld
De volgende JSON definieert een service op aanvraag gekoppeld HDInsight op basis van Linux. De Data Factory-service maakt automatisch een **op basis van Linux** HDInsight-cluster voor het verwerken van de vereiste activiteit. 

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
> Als er meer activiteit wordt uitgevoerd, ziet u veel containers in uw Azure blob storage. Als u deze niet nodig hebt voor het oplossen van problemen met taken, kunt u ze verwijderen om de opslagkosten te verlagen. De namen van deze containers volgen een patroon: `adf**yourdatafactoryname**-**linkedservicename**-datetimestamp`. Gebruik hulpprogramma's zoals [Microsoft Opslagverkenner](http://storageexplorer.com/) om containers in uw Azure-blobopslag te verwijderen.
>
> 

### <a name="properties"></a>Properties
| Eigenschap                     | Description                              | Vereist |
| ---------------------------- | ---------------------------------------- | -------- |
| type                         | De eigenschap type moet worden ingesteld op **HDInsightOnDemand**. | Ja      |
| clusterSize                  | Het aantal worker/gegevensknooppunten in het cluster. Het HDInsight-cluster is gemaakt met 2 hoofdknooppunten samen met het aantal worker-knooppunten die u voor deze eigenschap opgeeft. De knooppunten zijn van de grootte Standard_D3 met 4 kernen, zodat een cluster met 4 worker-knooppunten 24 kernen wordt (4\*4 = 16 cores voor worker-knooppunten, plus 2\*4 = 8 cores voor hoofdknooppunten). Zie [clusters instellen in HDInsight met Hadoop, Spark, Kafka en meer](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md) voor meer informatie. | Ja      |
| linkedServiceName            | Gekoppelde Azure Storage-service moet worden gebruikt door het cluster op aanvraag voor het opslaan en verwerken van gegevens. Het HDInsight-cluster is gemaakt in dezelfde regio als dit Azure Storage-account. Voor Azure HDInsight geldt een beperking voor het totale aantal kernen dat u kunt gebruiken in elke Azure-regio die wordt ondersteund. Zorg ervoor dat u voldoende kerngeheugen hebt in deze Azure-regio om te voldoen aan de vereiste de clustergrootte. Raadpleeg voor meer informatie, [clusters instellen in HDInsight met Hadoop, Spark, Kafka en meer](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md)<p>U kunt geen op dit moment een on-demand HDInsight-cluster die gebruikmaakt van een Azure Data Lake Store als de opslag maken. Als u wilt voor het opslaan van de resulterende gegevens uit HDInsight verwerking in een Azure Data Lake Store, kunt u een Kopieeractiviteit gebruiken de gegevens van de Azure Blob-opslag kopiëren naar de Azure Data Lake Store. </p> | Ja      |
| clusterResourceGroup         | Het HDInsight-cluster is gemaakt in deze resourcegroep. | Ja      |
| timetolive                   | De toegestane niet-actieve tijd voor het HDInsight-cluster op aanvraag. Hiermee geeft u op hoelang het HDInsight-cluster op aanvraag na voltooiing van een activiteit die wordt uitgevoerd als er geen andere actieve taken in het cluster actief blijft. De minimaal toegestane waarde is 5 minuten (00: 05:00).<br/><br/>Bijvoorbeeld, als de uitvoering van een activiteit kan 6 minuten en timetolive is ingesteld op 5 minuten, blijft het cluster actief gedurende vijf minuten na het uitvoeren van de zes minuten van de verwerking van de activiteit. Als een andere activiteit die wordt uitgevoerd met het venster 6 minuten wordt uitgevoerd, wordt verwerkt door hetzelfde cluster.<br/><br/>Het maken van een on-demand HDInsight-cluster is een dure bewerking (kan even duren), dus gebruik deze instelling als die nodig zijn om prestaties te verbeteren van een data factory door opnieuw een on-demand HDInsight-cluster te gebruiken.<br/><br/>Als u timetolive waarde op 0 instelt, wordt het cluster wordt verwijderd zodra de uitvoering van de activiteit is voltooid. Dat, als u een hoge waarde hebt ingesteld, het cluster niet actief is voor u om aan te melden voor het oplossen van enkele problemen kan blijven, maar het doel kan leiden tot hoge kosten. Het is daarom belangrijk dat u de juiste waarde op basis van uw behoeften.<br/><br/>Als de waarde van de eigenschap timetolive op de juiste wijze is ingesteld, kan meerdere pijplijnen het exemplaar van de HDInsight-cluster op aanvraag kunnen delen. | Ja      |
| clusterType                  | Het type van de HDInsight-cluster te maken. Toegestane waarden zijn 'hadoop' en 'spark'. Indien niet opgegeven, is de standaardwaarde hadoop. Enterprise-beveiligingspakket ingeschakelde cluster op aanvraag, kan niet worden gemaakt in plaats daarvan gebruiken een [bestaande cluster / doen om uw eigen compute](#azure-hdinsight-linked-service). | Nee       |
| versie                      | De versie van het HDInsight-cluster. Indien niet opgegeven, wordt deze met behulp van de huidige versie van de HDInsight-gedefinieerde standaard. | Nee       |
| hostSubscriptionId           | De Azure-abonnement-ID die wordt gebruikt voor het maken van HDInsight-cluster. Als niet is opgegeven, wordt de abonnements-ID van de context van uw Azure-aanmelding. | Nee       |
| clusterNamePrefix           | Het voorvoegsel van HDI-clusternaam en een tijdstempel wordt automatisch toegevoegd aan het einde van de naam van het cluster| Nee       |
| sparkVersion                 | De versie van spark als het clustertype is "Spark" | Nee       |
| additionalLinkedServiceNames | Hiermee geeft u extra opslagaccounts voor het HDInsight gekoppelde service zodat de Data Factory-service op uw rekening registreren kan. Deze opslagaccounts moeten zich in dezelfde regio bevinden als het HDInsight-cluster wordt gemaakt in dezelfde regio bevinden als het opslagaccount dat is opgegeven door linkedServiceName. | Nee       |
| besturingssysteemtype                       | Het type besturingssysteem. Toegestane waarden zijn: Linux- en Windows (voor HDInsight 3.3 alleen). De standaardwaarde is Linux. | Nee       |
| hcatalogLinkedServiceName    | De naam van Azure SQL gekoppelde service die verwijzen naar de HCatalog-database. De on-demand HDInsight-cluster is gemaakt met behulp van de Azure SQL database als de metastore. | Nee       |
| connectVia                   | De Integratieruntime moet worden gebruikt om te verdelen van de activiteiten van deze gekoppelde HDInsight-service. Voor de gekoppelde HDInsight-service op aanvraag ondersteunt deze alleen Azure Integration Runtime. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee       |
| clusterUserName                   | De gebruikersnaam voor toegang tot het cluster. | Nee       |
| clusterPassword                   | Het wachtwoord in het type van een beveiligde tekenreeks toegang tot het cluster. | Nee       |
| clusterSshUserName         | De gebruikersnaam voor SSH is extern verbinding maken met de cluster-knooppunt (voor Linux). | Nee       |
| clusterSshPassword         | Het wachtwoord in het type van een beveiligde tekenreeks SSH op afstand verbinding maken met het clusterknooppunt (voor Linux). | Nee       |
| scriptActions | Een script opgeven voor [HDInsight-cluster aanpassingen](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux) tijdens het maken van on-demand-cluster. <br />Op dit moment opgeven slechts 1 scriptactie biedt ondersteuning voor Azure Data Factory-gebruikersinterface ontwerpgereedschap, maar u kunt krijgen via deze beperking in de JSON (meerdere scriptacties opgeven in de JSON). | Nee |


> [!IMPORTANT]
> HDInsight biedt ondersteuning voor meerdere versies van de Hadoop-cluster dat kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de distributie van Hortonworks Data Platform (HDP) en een set onderdelen die deel uitmaken van dit distributiepunt. De lijst met ondersteunde versies van HDInsight houdt wordt bijgewerkt om nieuwe onderdelen van Hadoop-ecosysteem en oplossingen te bieden. Zorg ervoor dat u altijd verwijzen naar de meest recente informatie van [ondersteund HDInsight-versie en Type besturingssysteem](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) om te controleren of u met behulp van ondersteunde versie van HDInsight. 
>
> [!IMPORTANT]
> Op dit moment HDInsight gekoppelde services biedt geen ondersteuning voor HBase, interactieve query's (Hive LLAP), Storm. 
>
> 

#### <a name="additionallinkedservicenames-json-example"></a>Voorbeeld van de JSON, additionallinkedservicenames

```json
"additionalLinkedServiceNames": [{
    "referenceName": "MyStorageLinkedService2",
    "type": "LinkedServiceReference"          
}]
```

### <a name="service-principal-authentication"></a>Verificatie van service-principal

De On-Demand HDInsight gekoppelde service is een service-principal verificatie om te maken van HDInsight-clusters uit uw naam vereist. Service-principal verificatie gebruiken, de Toepassingsentiteit van een registreren in Azure Active Directory (Azure AD) en het geven de **Inzender** rol van het abonnement of de resourcegroep waarin het HDInsight-cluster is gemaakt. Zie voor gedetailleerde stappen [portal gebruiken voor het maken van een Azure Active Directory-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Noteer de volgende waarden, die u gebruikt voor het definiëren van de gekoppelde service:

- Toepassings-id
- Toepassingssleutel 
- Tenant-id

Gebruik verificatie van service-principal door de volgende eigenschappen op te geven:

| Eigenschap                | Description                              | Vereist |
| :---------------------- | :--------------------------------------- | :------- |
| **servicePrincipalId**  | Opgeven van de toepassing client-ID.     | Ja      |
| **servicePrincipalKey** | Geef de sleutel van de toepassing.           | Ja      |
| **tenant**              | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja      |

### <a name="advanced-properties"></a>Geavanceerde eigenschappen

U kunt ook de volgende eigenschappen voor de gedetailleerde configuratie van de on-demand HDInsight-cluster opgeven.

| Eigenschap               | Description                              | Vereist |
| :--------------------- | :--------------------------------------- | :------- |
| coreConfiguration      | Hiermee geeft u de configuratieparameters core (zoals in de core-site.xml) voor het HDInsight-cluster moet worden gemaakt. | Nee       |
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
U kunt de grootte van de hoofd-, gegevens- en zookeeper-knooppunten met behulp van de volgende eigenschappen opgeven: 

| Eigenschap          | Description                              | Vereist |
| :---------------- | :--------------------------------------- | :------- |
| headNodeSize      | Hiermee geeft u de grootte van het hoofdknooppunt. De standaardwaarde is: Standard_D3. Zie de **op te geven knooppuntgrootten** sectie voor meer informatie. | Nee       |
| dataNodeSize      | Hiermee geeft u de grootte van het gegevensknooppunt. De standaardwaarde is: Standard_D3. | Nee       |
| zookeeperNodeSize | Hiermee geeft u de grootte van het knooppunt zoohouder. De standaardwaarde is: Standard_D3. | Nee       |

#### <a name="specifying-node-sizes"></a>Knooppuntgrootten op te geven
Zie de [grootten van virtuele Machines](../virtual-machines/linux/sizes.md) artikel voor tekenreekswaarden die u nodig hebt om op te geven voor de eigenschappen die worden vermeld in de vorige sectie. De waarden moeten voldoen aan de **-CMDLETs en API's** waarnaar wordt verwezen in het artikel. Zoals u in het artikel ziet, is het gegevensknooppunt van grote (standaard) 7 GB geheugen, wat mogelijk niet goed genoeg is voor uw scenario. 

Als u maken van de hoofdknooppunten D4 grootte en het worker-knooppunten wilt, geeft u **Standard_D4** als de waarde voor de eigenschappen headNodeSize en dataNodeSize. 

```json
"headNodeSize": "Standard_D4",    
"dataNodeSize": "Standard_D4",
```

Als u een onjuiste waarde voor deze eigenschappen opgeeft, ontvangt u mogelijk de volgende **fout:** Kan geen cluster maken. Uitzondering: Kan niet worden voltooid het cluster niet maken. Bewerking is mislukt met code 400. Cluster heeft status: 'Fout'. Bericht: 'PreClusterCreationValidationFailure'. Wanneer u dit foutbericht ontvangt, zorg ervoor dat u gebruikmaakt van de **CMDLET en API's** naam uit de tabel in de [grootten van virtuele Machines](../virtual-machines/linux/sizes.md) artikel.        

## <a name="bring-your-own-compute-environment"></a>Breng uw eigen compute-omgeving
Gebruikers kunnen met dit type configuratie kan een reeds bestaande computeromgeving registreren als een gekoppelde service in Data Factory. De computeromgeving wordt beheerd door de gebruiker en de Data Factory-service gebruikt voor het uitvoeren van de activiteiten.

Dit type configuratie wordt ondersteund voor de volgende omgevingen:

* Azure HDInsight
* Azure Batch
* Azure Machine Learning
* Azure Data Lake Analytics
* Azure SQL DB, Azure SQL DW, SQL Server

## <a name="azure-hdinsight-linked-service"></a>Azure HDInsight gekoppelde service
U kunt een Azure HDInsight gekoppelde service voor het registreren van uw eigen HDInsight-cluster met Data Factory maken.

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

### <a name="properties"></a>Properties
| Eigenschap          | Description                                                  | Vereist |
| ----------------- | ------------------------------------------------------------ | -------- |
| type              | De eigenschap type moet worden ingesteld op **HDInsight**.            | Ja      |
| clusterUri        | De URI van het HDInsight-cluster.                            | Ja      |
| gebruikersnaam          | Geef de naam van de gebruiker moet worden gebruikt om te verbinden met een bestaand HDInsight-cluster. | Ja      |
| wachtwoord          | Wachtwoord voor het gebruikersaccount opgeven.                       | Ja      |
| linkedServiceName | De naam van de gekoppelde Azure Storage-service die naar de Azure blob-opslag die wordt gebruikt door het HDInsight-cluster verwijst. <p>Momenteel kunt opgeven u niet dat een Azure Data Lake Store gekoppelde service voor deze eigenschap. Als het HDInsight-cluster toegang tot de Data Lake Store heeft, kan u toegang tot gegevens in de Azure Data Lake Store via Hive/Pig-scripts. </p> | Ja      |
| isEspEnabled      | Geef '*waar*' als het HDInsight-cluster is [Enterprise-beveiligingspakket](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-introduction) ingeschakeld. De standaardwaarde is '*false*'. | Nee       |
| connectVia        | De Integratieruntime moet worden gebruikt om te verdelen van de activiteiten van deze gekoppelde service. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. <br />Enterprise Security Package (ESP) ingeschakeld HDInsight-cluster gebruikt een zelf-hostende integratieruntime met peeren aan het cluster of het moet worden geïmplementeerd in hetzelfde Virtueelnetwerk als het ESP HDInsight-cluster. | Nee       |

> [!IMPORTANT]
> HDInsight biedt ondersteuning voor meerdere versies van de Hadoop-cluster dat kunnen worden geïmplementeerd. Elke versie keuze maakt een specifieke versie van de distributie van Hortonworks Data Platform (HDP) en een set onderdelen die deel uitmaken van dit distributiepunt. De lijst met ondersteunde versies van HDInsight houdt wordt bijgewerkt om nieuwe onderdelen van Hadoop-ecosysteem en oplossingen te bieden. Zorg ervoor dat u altijd verwijzen naar de meest recente informatie van [ondersteund HDInsight-versie en Type besturingssysteem](../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions) om te controleren of u met behulp van ondersteunde versie van HDInsight. 
>
> [!IMPORTANT]
> Op dit moment HDInsight gekoppelde services biedt geen ondersteuning voor HBase, interactieve query's (Hive LLAP), Storm. 
>
> 

## <a name="azure-batch-linked-service"></a>Azure Batch gekoppelde service

U kunt een gekoppelde Azure-Batch-service voor het registreren van een Batch-pool van virtuele machines (VM's) om een data factory te maken. U kunt aangepaste activiteit met behulp van Azure Batch kunt uitvoeren.

Zie de volgende onderwerpen als u niet bekend bent met Azure Batch-service:

* [Basisbeginselen van Azure Batch](../batch/batch-technical-overview.md) voor een overzicht van de Azure Batch-service.
* [Nieuwe AzureRmBatchAccount](/powershell/module/azurerm.batch/New-AzureRmBatchAccount?view=azurermps-4.3.1) cmdlet voor het maken van een Azure Batch-account (of) [Azure-portal](../batch/batch-account-create-portal.md) te maken van de Azure Batch-account met behulp van Azure portal. Zie [met behulp van PowerShell voor het beheren van Azure Batch-Account](http://blogs.technet.com/b/windowshpc/archive/2014/10/28/using-azure-powershell-to-manage-azure-batch-account.aspx) onderwerp voor gedetailleerde instructies over het gebruik van de cmdlet.
* [Nieuwe-AzureBatchPool](/powershell/module/azurerm.batch/New-AzureBatchPool?view=azurermps-4.3.1) cmdlet voor het maken van een Azure Batch-pool.

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


### <a name="properties"></a>Properties
| Eigenschap          | Description                              | Vereist |
| ----------------- | ---------------------------------------- | -------- |
| type              | De eigenschap type moet worden ingesteld op **AzureBatch**. | Ja      |
| accountName       | De naam van de Azure Batch-account.         | Ja      |
| accessKey         | De toegangssleutel voor de Azure Batch-account.  | Ja      |
| batchUri          | URL naar uw Azure Batch-account, in de indeling https://*batchaccountname.region*. batch.azure.com. | Ja      |
| poolName          | De naam van de pool van virtuele machines.    | Ja      |
| linkedServiceName | Naam van de Azure Storage gekoppelde service die is gekoppeld aan deze gekoppelde Azure-Batch-service. Deze gekoppelde service wordt gebruikt voor het Faseren van bestanden die nodig zijn om uit te voeren van de activiteit. | Ja      |
| connectVia        | De Integratieruntime moet worden gebruikt om te verdelen van de activiteiten van deze gekoppelde service. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee       |

## <a name="azure-machine-learning-linked-service"></a>Azure Machine Learning gekoppelde service
U maakt een Azure Machine Learning gekoppelde service voor het registreren van een Machine Learning batch scoring-eindpunt naar een data factory.

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

### <a name="properties"></a>Properties
| Eigenschap               | Description                              | Vereist                                 |
| ---------------------- | ---------------------------------------- | ---------------------------------------- |
| Type                   | De eigenschap type moet worden ingesteld op: **AzureML**. | Ja                                      |
| mlEndpoint             | Het batch scoring-URL.                   | Ja                                      |
| ApiKey                 | Het gepubliceerde werkruimtemodel API.     | Ja                                      |
| updateResourceEndpoint | De bron-URL bijwerken voor een Azure ML Web Service-eindpunt dat is gebruikt voor het bijwerken van de voorspellende webservice met getrainde model-bestand | Nee                                       |
| servicePrincipalId     | Opgeven van de toepassing client-ID.     | Vereist als updateResourceEndpoint wordt opgegeven |
| servicePrincipalKey    | Geef de sleutel van de toepassing.           | Vereist als updateResourceEndpoint wordt opgegeven |
| tenant                 | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Vereist als updateResourceEndpoint wordt opgegeven |
| connectVia             | De Integratieruntime moet worden gebruikt om te verdelen van de activiteiten van deze gekoppelde service. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee                                       |

## <a name="azure-data-lake-analytics-linked-service"></a>Azure Data Lake Analytics gekoppelde service
U maakt een **Azure Data Lake Analytics** gekoppelde service om te koppelen van een Azure Data Lake Analytics compute-service aan een Azure data factory. De Data Lake Analytics U-SQL-activiteit in de pijplijn verwijst naar deze gekoppelde service. 

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

### <a name="properties"></a>Properties

| Eigenschap             | Description                              | Vereist                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| type                 | De eigenschap type moet worden ingesteld op: **AzureDataLakeAnalytics**. | Ja                                      |
| accountName          | Azure Data Lake Analytics-accountnaam.  | Ja                                      |
| dataLakeAnalyticsUri | Azure Data Lake Analytics-URI.           | Nee                                       |
| subscriptionId       | Azure-abonnement-id                    | Nee                                       |
| resourceGroupName    | Naam van Azure-resourcegroep                | Nee                                       |
| servicePrincipalId   | Opgeven van de toepassing client-ID.     | Ja                                      |
| servicePrincipalKey  | Geef de sleutel van de toepassing.           | Ja                                      |
| tenant               | De tenantgegevens (domain name of tenant-ID) opgeven in uw toepassing zich bevindt. U kunt het ophalen van de muis in de rechterbovenhoek van de Azure-portal. | Ja                                      |
| connectVia           | De Integratieruntime moet worden gebruikt om te verdelen van de activiteiten van deze gekoppelde service. U kunt Azure Integration Runtime of zelfgehoste Cloudintegratieruntime gebruiken. Als niet is opgegeven, wordt de standaard Azure Integration Runtime. | Nee                                       |



## <a name="azure-databricks-linked-service"></a>Azure Databricks gekoppelde service
U kunt maken **Azure Databricks gekoppelde service** Databricks-werkruimte die u gebruiken wilt voor het uitvoeren van de Databricks-workloads(notebooks) registreren.

### <a name="example---using-new-job-cluster-in-databricks"></a>Voorbeeld: met behulp van de nieuwe taakcluster in Databricks

```json
{
    "name": "AzureDatabricks_LS",
    "properties": {
        "type": "AzureDatabricks",
        "typeProperties": {
            "domain": "https://eastus.azuredatabricks.net",
            "newClusterNodeType": "Standard_D3_v2",
            "newClusterNumOfWorker": "1:10",
            "newClusterVersion": "4.0.x-scala2.11",
            "accessToken": {
                "type": "SecureString",
                "value": "dapif33c9c721144c3a790b35000b57f7124f"
            }
        }
    }
}

```

### <a name="example---using-existing-interactive-cluster-in-databricks"></a>Voorbeeld: met behulp van bestaande interactieve cluster in Databricks

```json
{
    "name": " AzureDataBricksLinedService",
    "properties": {
      "type": " AzureDatabricks",
      "typeProperties": {
        "domain": "https://westeurope.azuredatabricks.net",
        "accessToken": {
            "type": "SecureString", 
            "value": "dapif33c9c72344c3a790b35000b57f7124f"
          },
        "existingClusterId": "{clusterId}"
        }
}

```

### <a name="properties"></a>Properties

| Eigenschap             | Description                              | Vereist                                 |
| -------------------- | ---------------------------------------- | ---------------------------------------- |
| naam                 | Naam van de gekoppelde Service               | Ja   |
| type                 | De eigenschap type moet worden ingesteld op: **AzureDatabricks**. | Ja                                      |
| domein               | Geef de Azure-regio dienovereenkomstig op basis van de regio van de Databricks-werkruimte. Voorbeeld: https://eastus.azuredatabricks.net | Ja                                 |
| accessToken          | Het toegangstoken is vereist voor Data Factory om te verifiëren met Azure Databricks. Toegangstoken moet worden gegenereerd op basis van de databricks-werkruimte. Meer gedetailleerde stappen voor het vinden van het toegangstoken vindt [hier](https://docs.azuredatabricks.net/api/latest/authentication.html#generate-token)  | Ja                                       |
| existingClusterId    | ID van een bestaand cluster alle taken uitvoeren op dit cluster. Dit moet een gemaakte interactieve-Cluster. Mogelijk moet u het cluster handmatig opnieuw starten als deze niet meer reageert. Databricks voorstellen uitgevoerde op nieuwe clusters voor een grotere betrouwbaarheid. U vindt de Cluster-ID van een interactieve-Cluster op Databricks-werkruimte ->-Clusters interactieve Clusternaam > -> configuratieserver -> Tags. [Meer informatie](https://docs.databricks.com/user-guide/clusters/tags.html) | Nee 
| newClusterVersion    | De Spark-versie van het cluster. Er wordt een taakcluster maken in databricks. | Nee  |
| newClusterNumOfWorker| Het aantal worker-knooppunten die dit cluster moet hebben. Een cluster heeft een Spark-stuurprogramma en num_workers Executor voor een totaal van num_workers + 1 knooppunten van Spark. Een tekenreeks opgemaakt Int32, zoals "1" betekent numOfWorker is 1 of '1:10 ' betekent dat automatisch schalen van 1 als min en 10 als max.  | Nee                |
| newClusterNodeType   | Dit veld codeert via één waarde, de beschikbare resources voor elk van de Spark-knooppunten in dit cluster. Bijvoorbeeld, de Spark-knooppunten kunnen worden ingericht en geoptimaliseerd voor geheugen of berekenen van intensieve workloads dit veld is vereist voor het nieuwe cluster                | Nee               |
| newClusterSparkConf  | een set van optionele, door de gebruiker opgegeven Spark configuratie sleutel / waarde-paren. Gebruikers kunnen ook doorgeven in een tekenreeks met extra JVM-opties voor het stuurprogramma en de Executor via spark.driver.extraJavaOptions en spark.executor.extraJavaOptions respectievelijk. | Nee  |


## <a name="azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service
U maakt een gekoppelde Azure SQL-service en deze gebruikt met de [opgeslagen-Procedureactiviteit](transform-data-using-stored-procedure.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. Zie [Azure SQL-Connector](connector-azure-sql-database.md#linked-service-properties) artikel voor meer informatie over deze gekoppelde service.

## <a name="azure-sql-data-warehouse-linked-service"></a>Azure SQL Data Warehouse gekoppelde service
U een Azure SQL Data Warehouse gekoppelde service maken en gebruiken met de [opgeslagen-Procedureactiviteit](transform-data-using-stored-procedure.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. Zie [Azure SQL Data Warehouse-Connector](connector-azure-sql-data-warehouse.md#linked-service-properties) artikel voor meer informatie over deze gekoppelde service.

## <a name="sql-server-linked-service"></a>Gekoppelde SQL Server-service
U maakt een gekoppelde SQL Server-service en deze gebruikt met de [opgeslagen-Procedureactiviteit](transform-data-using-stored-procedure.md) om aan te roepen een opgeslagen procedure uit een Data Factory-pijplijn. Zie [SQL Server-connector](connector-sql-server.md#linked-service-properties) artikel voor meer informatie over deze gekoppelde service.

## <a name="next-steps"></a>Volgende stappen
Zie voor een lijst van de activiteiten voor gegevenstransformatie ondersteund door Azure Data Factory, [gegevens transformeren](transform-data.md).
