---
title: Apache Spark gestructureerde Streaming van Kafka aan Azure Cosmos DB - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Apache Spark gestructureerde Streaming voor het lezen van gegevens van Apache Kafka en vervolgens opslaan in Azure Cosmos DB. In dit voorbeeld kunt u gegevens met behulp van een Jupyter-notebook in Spark in HDInsight streamen.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/26/2018
ms.author: larryfr
ms.openlocfilehash: 7346a45cf04b50369cc7b853b985a8b0bc865493
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="use-spark-structured-streaming-with-kafka-and-azure-cosmos-db"></a>Gebruik van Spark gestructureerd streamen met Kafka en Azure Cosmos-DB

Informatie over het gebruik van gestructureerde Spark-Streaming voor gegevens lezen uit Apache Kafka in Azure HDInsight en de gegevens op te slaan in Azure Cosmos DB.

Azure Cosmos-database is een globaal gedistribueerd, meerdere model-database. Dit voorbeeld wordt een databasemodel SQL-API. Zie voor meer informatie de [Welkom bij Azure Cosmos DB](../cosmos-db/introduction.md) document.

Spark gestructureerd streaming is een stream-verwerkingsengine gebouwd op Spark SQL. Hiermee kunt u streaming berekeningen Express hetzelfde zijn als batch berekening op statische gegevens. Zie voor meer informatie over het gestructureerde Streaming de [gestructureerde Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) op Apache.org.

> [!IMPORTANT]
> In dit voorbeeld gebruikt 2.2 Spark in HDInsight 3.6.
>
> De stappen in dit document wordt een Azure-resourcegroep met zowel een Spark in HDInsight en een Kafka op HDInsight-cluster maken Deze clusters zijn beide zich binnen een virtueel netwerk van Azure, waardoor het Spark-cluster rechtstreeks communiceren met de Kafka-cluster.
>
> Wanneer u klaar bent met de stappen in dit document, vergeet niet de clusters om te voorkomen dat de overtollige kosten te verwijderen.

## <a name="create-the-clusters"></a>Clusters maken

Apache Kafka op HDInsight biedt geen toegang voor de beleggingsmakelaars Kafka via het openbare internet. Alles wat wordt gesproken naar Kafka moet zich in hetzelfde virtuele netwerk van Azure als de knooppunten in het cluster Kafka. In dit voorbeeld worden de Kafka- en Spark-clusters bevinden zich in een Azure-netwerk. Het volgende diagram toont hoe de communicatie tussen clusters stroomt:

![Diagram van Spark en Kafka clusters in een Azure-netwerk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> De service Kafka is beperkt tot communicatie binnen het virtuele netwerk. Andere services op het cluster, zoals SSH en Ambari, zijn toegankelijk via het internet. Zie voor meer informatie over de openbare poorten beschikbaar met HDInsight [poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md).

U kunt een Azure-netwerk Kafka, maken en handmatig Spark-clusters, is het ook eenvoudiger te gebruiken van een Azure Resource Manager-sjabloon. Gebruik de volgende stappen voor het implementeren van een Azure-netwerk Kafka, en Spark-clusters met uw Azure-abonnement.

1. Gebruik de volgende knop om te melden bij Azure en opent u de sjabloon in de Azure portal.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
    </a>

    De Azure Resource Manager-sjabloon bevindt zich in de GitHub-opslagplaats voor dit project ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Deze sjabloon maakt u de volgende bronnen:

    * Een Kafka op 3.6 HDInsight-cluster.

    * Een Spark in HDInsight 3.6 cluster.

    * Een virtueel netwerk in Azure die de HDInsight-clusters bevat.

        > [!NOTE]
        > Het virtuele netwerk gemaakt door de sjabloon maakt gebruik van de adresruimte van 10.0.0.0/16.

    * Een API van Azure Cosmos DB SQL-database.

    > [!IMPORTANT]
    > De gestructureerde streaming laptop gebruikt in dit voorbeeld vereist Spark in HDInsight 3.6. Als u een eerdere versie van Spark in HDInsight gebruikt, ontvangt u fouten bij gebruik van de notebook.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen op de **aangepaste implementatie** sectie:
   
    ![Aangepaste HDInsight-implementatie](./media/apache-kafka-spark-structured-streaming-cosmosdb/parameters.png)

    * **Abonnement**: selecteer uw Azure-abonnement.
   
    * **Resourcegroep**: Maak een groep of een bestaande set selecteren. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie geografisch dicht bij u.

    * **Accountnaam cosmos-DB**: deze waarde wordt gebruikt als de naam van de Cosmos-DB-account.

    * **Clusternaam baseren**: deze waarde wordt gebruikt als de basisnaam aan voor de Spark en Kafka-clusters. Bijvoorbeeld, voeren **myhdi** maakt een Spark-cluster met de naam __spark myhdi__ en een Kafka-cluster met de naam **kafka myhdi**.

    * **Cluster-versie**: de HDInsight-cluster versie.

        > [!IMPORTANT]
        > In dit voorbeeld wordt getest met HDInsight 3.6 en werkt mogelijk niet met andere clustertypen.

    * **Aanmeldingsnaam van gebruiker cluster**: de beheerdersgebruikersnaam voor Spark en Kafka-clusters.

    * **Aanmeldingswachtwoord cluster**: het beheerderswachtwoord voor de Spark en Kafka clusters.

    * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor Spark en Kafka-clusters.

    * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de Spark en Kafka clusters.

3. Lees de **voorwaarden en bepalingen**, en selecteer vervolgens **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.

4. Controleer ten slotte **vastmaken aan dashboard** en selecteer vervolgens **aankoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

> [!IMPORTANT]
> Het kan tot 45 minuten voor het maken van het clusters, het virtuele netwerk en de account van de Cosmos-DB duren.

## <a name="create-the-cosmos-db-database-and-collection"></a>De database van de Cosmos-database en verzameling maken

Het project in dit document gebruikt opslaat gegevens in Cosmos-database. Voordat u de code uitvoert, moet u eerst maken een _database_ en _verzameling_ in uw exemplaar van de Cosmos-DB. Bovendien moet u het eindpunt van het document ophalen en de _sleutel_ gebruikt voor verificatie aan de database van de Cosmos-aanvragen. 

Doen dit is het gebruik van de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest). Het volgende script maakt een database met naam `kafkadata` en een verzameling met de naam `kafkacollection`. Vervolgens wordt de primaire sleutel.

```azurecli
#!/bin/bash

# Replace 'myresourcegroup' with the name of your resource group
resourceGroupName='myresourcegroup'
# Replace 'mycosmosaccount' with the name of your Cosmos DB account name
name='mycosmosaccount'

# WARNING: If you change the databaseName or collectionName
#          then you must update the values in the Jupyter notebook
databaseName='kafkadata'
collectionName='kafkacollection'

# Create the database
az cosmosdb database create --name $name --db-name $databaseName --resource-group $resourceGroupName
# Create the collection
az cosmosdb collection create --collection-name $collectionName --name $name --db-name $databaseName --resource-group $resourceGroupName

# Get the endpoint
az cosmosdb show --name $name --resource-group $resourceGroupName --query documentEndpoint

# Get the primary key
az cosmosdb list-keys --name $name --resource-group $resourceGroupName --query primaryMasterKey
```

Het document eindpunt en de primaire-sleutelgegevens is vergelijkbaar met de volgende tekst:

```text
# endpoint
"https://mycosmosaccount.documents.azure.com:443/"
# key
"YqPXw3RP7TsJoBF5imkYR0QNA02IrreNAlkrUMkL8EW94YHs41bktBhIgWq4pqj6HCGYijQKMRkCTsSaKUO2pw=="
```

> [!IMPORTANT]
> Sla het eindpunt en sleutelwaarden, als u ze in de Jupyter-Notebooks.

## <a name="get-the-kafka-brokers"></a>De Kafka beleggingsmakelaars ophalen

De code in dit voorbeeld verbindt met Kafka broker hosts in het cluster Kafka. Om de adressen van de twee Kafka broker hosts vinden, gebruikt u het volgende PowerShell- of Bash-voorbeeld:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds `
    -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

> [!NOTE]
> In het voorbeeld Bash verwacht `$CLUSTERNAME` aan de naam van het cluster Kafka bevatten.
>
> In dit voorbeeld wordt de [jq](https://stedolan.github.io/jq/) hulpprogramma voor het parseren van gegevens buiten het JSON-document.

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Voer desgevraagd het wachtwoord voor de cluster-serviceaccount voor aanmelding (admin)

De uitvoer lijkt op het volgende:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Deze gegevens niet opslaan omdat het wordt gebruikt in de volgende secties van dit document.

## <a name="get-the-notebooks"></a>De laptops ophalen

De code voor het voorbeeld in dit document beschreven is beschikbaar op [ https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb ](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb).

## <a name="upload-the-notebooks"></a>De laptops uploaden

Gebruik de volgende stappen voor het uploaden van de laptops van het project met uw Spark in HDInsight-cluster:

1. In uw webbrowser verbinding maken met de Jupyter-notebook in Spark-cluster. Vervang in de volgende URL `CLUSTERNAME` met de naam van uw __Spark__ cluster:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Wanneer u wordt gevraagd, typt u de cluster-aanmelding (admin) en het wachtwoord die worden gebruikt wanneer u het cluster hebt gemaakt.

2. Gebruik vanaf boven aan de rechterkant van de pagina, de __uploaden__ knop voor het uploaden van de __Stream-taxi-gegevens-naar-kafka.ipynb__ bestand aan het cluster. Selecteer __Open__ het uploaden wordt gestart.

3. Zoeken naar de __Stream-taxi-gegevens-naar-kafka.ipynb__ vermelding in de lijst van notitieblokken en selecteer __uploaden__ knop ernaast.

4. Herhaal stap 1-3 laden van de __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ notebook.

## <a name="load-taxi-data-into-kafka"></a>Taxi gegevens laden in Kafka

Zodra de bestanden zijn geüpload, selecteert u de __Stream-taxi-gegevens-naar-kafka.ipynb__ vermelding de notebook te openen. Volg de stappen in de notebook gegevens laadt in Kafka.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>Gegevens over het installatieproces taxi met gestructureerde Spark-Streaming

Selecteer in de startpagina Jupyter-Notebook de __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ vermelding. Volg de stappen in de notebook met stroomgegevens van Kafka en naar Azure Cosmos DB met gestructureerde Spark-Streaming.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u gestructureerde Spark-Streaming, Zie de volgende documenten voor meer informatie over het werken met Spark, Kafka en Azure Cosmos DB:

* [Het gebruik van Spark-streaming (DStream) met Kafka](hdinsight-apache-spark-with-kafka.md).
* [Beginnen met Jupyter-Notebook en Spark in HDInsight](spark/apache-spark-jupyter-spark-sql.md)
* [Welkom bij Azure Cosmos DB](../cosmos-db/introduction.md)
