---
title: De Azure Data Explorer-connector voor Apache Spark gebruiken om gegevens te verplaatsen tussen Azure Data Explorer en Spark-clusters.
description: Dit onderwerp ziet u hoe u gegevens verplaatsen tussen Azure Data Explorer en Apache Spark-clusters.
author: orspod
ms.author: orspodek
ms.reviewer: michazag
ms.service: data-explorer
ms.topic: conceptual
ms.date: 4/29/2019
ms.openlocfilehash: 854e29b67b6e24c583a98b5851bf17551cfcbf61
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65441347"
---
# <a name="azure-data-explorer-connector-for-apache-spark-preview"></a>Azure Data Explorer-Connector voor Apache Spark (Preview)

[Apache Spark](https://spark.apache.org/) is een geïntegreerde analytics-engine voor grootschalige gegevensverwerking. Azure Data Explorer is een snelle, volledig beheerde data analytics-service voor realtime analyse van grote hoeveelheden gegevens. 

Azure Data Explorer-connector voor Spark implementeert gegevensbron en gegevens-sink voor het verplaatsen van gegevens in Azure Data Explorer en Spark-clusters naar hun mogelijkheden te gebruiken. Met Azure Data Explorer en Apache Spark, kunt u ontwikkelen voor snelle en schaalbare toepassingen die zijn gericht op scenario's, zoals machine learning-(ML), Extract-Transform-Load (ETL) en Log Analytics-gegevens. Schrijven naar Azure Data Explorer kan worden uitgevoerd in de batch- en streaming-modus.
Het lezen van het Azure Data Explorer biedt ondersteuning voor kolom verwijderen en predicaat pushdown, waardoor de hoeveelheid overgedragen gegevens door het filteren van gegevens in Azure Data Explorer.

Azure Data Explorer Spark-connector is een [open-SourceProject](https://github.com/Azure/azure-kusto-spark) die kunnen worden uitgevoerd op een Spark-cluster.

> [!NOTE]
> Hoewel sommige van de onderstaande voorbeelden naar verwijzen een [Azure Databricks](https://docs.azuredatabricks.net/) Spark-cluster, Azure Data Explorer Spark-connector neemt de directe afhankelijkheden op Databricks of elke andere Spark-distributie.

## <a name="prerequisites"></a>Vereisten

* [Een cluster van Azure Data Explorer en -database maken](/azure/data-explorer/create-cluster-database-portal) 
* Een Spark-cluster maken
* Installeer Azure Data Explorer-connector-bibliotheek en bibliotheken die worden vermeld [afhankelijkheden](https://github.com/Azure/azure-kusto-spark#dependencies) waaronder de volgende [Kusto Java SDK](/azure/kusto/api/java/kusto-java-client-library) bibliotheken:
    * [Kusto-gegevens-Client](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-data)
    * [Kusto Client opnemen](https://mvnrepository.com/artifact/com.microsoft.azure.kusto/kusto-ingest)
* Vooraf gemaakte-bibliotheken voor [Spark 2.4, Scala 2.11](https://github.com/Azure/azure-kusto-spark/releases)

## <a name="how-to-build-the-spark-connector"></a>Over het bouwen van de Spark-connector

Spark-Connector kunnen worden gebouwd van [bronnen](https://github.com/Azure/azure-kusto-spark) zoals hieronder aangegeven.

> [!NOTE]
> Deze stap is optioneel. Als u van vooraf gemaakte bibliotheken gebruikmaakt Ga naar [Spark-cluster setup](#spark-cluster-setup).

### <a name="build-prerequisites"></a>Vereisten bouwen

* Java 1.8 SDK is geïnstalleerd
* [Maven 3.x](https://maven.apache.org/download.cgi) geïnstalleerd
* Apache Spark-versie 2.4.0 of hoger

> [!TIP]
> 2.3.x versies worden ook ondersteund, maar mogelijk enkele wijzigingen in het pom.xml-afhankelijkheden.

Voor Scala/Java-toepassingen met behulp van Maven-project definities, Koppel uw App met de volgende artefact (meest recente versie kan verschillen):

```Maven
   <dependency>
     <groupId>com.microsoft.azure</groupId>
     <artifactId>spark-kusto-connector</artifactId>
     <version>1.0.0-Beta-02</version>
   </dependency>
```

### <a name="build-commands"></a>Bouwen-opdrachten

Jar bouwen en uitvoeren van alle tests:

```
mvn clean package
```

Alle tests uitvoeren voor het bouwen van de jar en jar installeren op uw lokale opslagplaats met Maven:

```
mvn clean install
```

Zie voor meer informatie, [connector gebruik](https://github.com/Azure/azure-kusto-spark#usage).

## <a name="spark-cluster-setup"></a>Spark-cluster instellen

> [!NOTE]
> Het verdient aanbeveling de meest recente versie van de Azure Data Explorer Spark-connector gebruiken bij het uitvoeren van de volgende stappen uit:

1. Stel de volgende Spark clusterinstellingen, op basis van Azure Databricks-cluster met behulp van Spark 2.4 en Scala 2.11: 

    ![Instellingen voor Databricks-cluster](media/spark-connector/databricks-cluster.png)

1. De Azure Data Explorer-connector-bibliotheek importeren:

    ![Azure Data Explorer-bibliotheek importeren](media/spark-connector/db-create-library.png)

1. Voeg extra afhankelijkheden:

    ![Afhankelijkheden toevoegen](media/spark-connector/db-dependencies.png)

    > [!TIP]
    > De juiste java-versie voor elke versie van Spark wordt gevonden [hier](https://github.com/Azure/azure-kusto-spark#dependencies).

1. Controleer of alle vereiste bibliotheken zijn geïnstalleerd:

    ![Controleer of de bibliotheken geïnstalleerd](media/spark-connector/db-libraries-view.png)

## <a name="authentication"></a>Verificatie

Azure Data Explorer Spark-connector kunt u verifiëren met Azure Active Directory (Azure AD) met behulp van een [Azure AD-toepassing](#azure-ad-application-authentication), [Azure AD-toegangstoken](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#direct-authentication-with-access-token), [apparaatverificatie ](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#device-authentication) (voor niet-productiescenario's), of [Azure Key Vault](https://github.com/Azure/azure-kusto-spark/blob/dev/docs/Authentication.md#key-vault). De gebruiker moet azure keyvault-pakket installeren en referenties van de aanvraag voor toegang tot de Key Vault-resource.

### <a name="azure-ad-application-authentication"></a>Azure AD-toepassing-verificatie

De meeste eenvoudig en algemene verificatiemethode. Deze methode wordt aanbevolen voor gebruik met Azure Data Explorer Spark-connector.

|Properties  |Description  |
|---------|---------|
|**KUSTO_AAD_CLIENT_ID**     |   ID Azure AD-toepassing (client).      |
|**KUSTO_AAD_AUTHORITY_ID**     |  Azure AD-verificatie-instantie. Azure AD-Directory (tenant)-ID.        |
|**KUSTO_AAD_CLIENT_PASSWORD**    |    Azure AD-Toepassingssleutel voor de client.     |

### <a name="azure-data-explorer-privileges"></a>Azure Data Explorer bevoegdheden

De volgende bevoegdheden moeten worden verleend op een Azure Data Explorer-Cluster:

* Voor het lezen van (gegevensbron), Azure AD-toepassing moet hebben *viewer* bevoegdheden op de doeldatabase of *admin* bevoegdheden voor de doeltabel.
* Voor het schrijven van (gegevenssink), Azure AD-toepassing moet hebben *ingestor* bevoegdheden op de doeldatabase. Het moet ook beschikken over *gebruiker* bevoegdheden op de doeldatabase om nieuwe tabellen te maken. Als de doeltabel al bestaat, *admin* bevoegdheden voor de doeltabel kunnen worden geconfigureerd.
 
Zie voor meer informatie over Azure Data Explorer-principalrollen [autorisatie op basis van rollen](/azure/kusto/management/access-control/role-based-authorization). Voor het beheren van beveiligingsrollen, Zie [rollen beveiligingsbeheer](/azure/kusto/management/security-roles).

## <a name="spark-sink-writing-to-azure-data-explorer"></a>Spark-sink: Schrijven naar Azure Data Explorer

1. Sink-parameters instellen:

     ```scala
    val KustoSparkTestAppId = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppId")
    val KustoSparkTestAppKey = dbutils.secrets.get(scope = "KustoDemos", key = "KustoSparkTestAppKey")
 
    val appId = KustoSparkTestAppId
    val appKey = KustoSparkTestAppKey
    val authorityId = "72f988bf-86f1-41af-91ab-2d7cd011db47"
    val cluster = "Sparktest.eastus2"
    val database = "TestDb"
    val table = "StringAndIntTable"
    ```

1. Spark DataFrame schrijven naar Azure Data Explorer cluster als batch:

    ```scala
    df.write
      .format("com.microsoft.kusto.spark.datasource")
      .option(KustoOptions.KUSTO_CLUSTER, cluster)
      .option(KustoOptions.KUSTO_DATABASE, database)
      .option(KustoOptions.KUSTO_TABLE, table)
      .option(KustoOptions.KUSTO_AAD_CLIENT_ID, appId)
      .option(KustoOptions.KUSTO_AAD_CLIENT_PASSWORD, appKey) 
      .option(KustoOptions.KUSTO_AAD_AUTHORITY_ID, authorityId)
      .save()
    ```

1. Streaming-gegevens schrijven:

    ```scala    
    import org.apache.spark.sql.streaming.Trigger
    import java.util.concurrent.TimeUnit
    
    // Set up a checkpoint and disable codeGen. Set up a checkpoint and disable codeGen as a workaround for an known issue 
    spark.conf.set("spark.sql.streaming.checkpointLocation", "/FileStore/temp/checkpoint")
    spark.conf.set("spark.sql.codegen.wholeStage","false")
    
    // Write to a Kusto table fro streaming source
    val kustoQ = csvDf
          .writeStream
          .format("com.microsoft.kusto.spark.datasink.KustoSinkProvider")
          .options(Map(
            KustoOptions.KUSTO_CLUSTER -> cluster,
            KustoOptions.KUSTO_TABLE -> table,
            KustoOptions.KUSTO_DATABASE -> database,
            KustoOptions.KUSTO_AAD_CLIENT_ID -> appId,
            KustoOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey,
            KustoOptions.KUSTO_AAD_AUTHORITY_ID -> authorityId))
          .trigger(Trigger.Once)
    
    kustoQ.start().awaitTermination(TimeUnit.MINUTES.toMillis(8))
    ```

## <a name="spark-source-reading-from-azure-data-explorer"></a>Spark-bron: Het lezen van het Azure Data Explorer

1. Bij het lezen van kleine hoeveelheden gegevens, definieert u de query:

    ```scala
    val conf: Map[String, String] = Map(
          KustoOptions.KUSTO_AAD_CLIENT_ID -> appId,
          KustoOptions.KUSTO_AAD_CLIENT_PASSWORD -> appKey,
          KustoOptions.KUSTO_QUERY -> s"$table | where (ColB % 1000 == 0) | distinct ColA"      
        )
    
    // Simplified syntax flavor
    import org.apache.spark.sql._
    import com.microsoft.kusto.spark.sql.extension.SparkExtension._
    import org.apache.spark.SparkConf
    
    val df = spark.read.kusto(cluster, database, "", conf)
    display(df)
    ```

1. Bij het lezen van grote hoeveelheden gegevens, kan de tijdelijke blob-opslag moet worden opgegeven. Geef storage-container SAS-sleutel of storage-accountnaam, accountsleutel en containernaam. Deze stap is alleen vereist voor de huidige preview-versie van de Spark-connector.

    ```scala
    // Use either container/account-key/account name, or container SaS
    val container = dbutils.secrets.get(scope = "KustoDemos", key = "blobContainer")
    val storageAccountKey = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountKey")
    val storageAccountName = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageAccountName")
    // val storageSas = dbutils.secrets.get(scope = "KustoDemos", key = "blobStorageSasUrl")
    ```

    In het bovenstaande voorbeeld geen er toegang tot de Key Vault met behulp van de connector-interface. We gebruiken ook een eenvoudigere methode van het gebruik van de Databricks-geheimen.

1. Lezen van Azure Data Explorer:

    ```scala
    val df2 = spark.read.kusto(cluster, database, "ReallyBigTable", conf3)
    
    val dfFiltered = df2
      .where(df2.col("ColA").startsWith("row-2"))
      .filter("ColB > 12")
      .filter("ColB <= 21")
      .select("ColA")
    
    display(dfFiltered)
    ```
