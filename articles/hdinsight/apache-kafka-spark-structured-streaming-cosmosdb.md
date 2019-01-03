---
title: Apache Spark Structured Streaming van Apache Kafka met Azure Cosmos DB - Azure HDInsight
description: Leer hoe u Apache Spark Structured Streaming gebruiken om te lezen van gegevens uit Apache Kafka en vervolgens opslaan in Azure Cosmos DB. In dit voorbeeld gaat u met behulp van een Jupyter Notebook gegevens streamen van Spark naar HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: e964e00cd326d924a77a53348942f91ebbdbdea4
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53630157"
---
# <a name="use-apache-spark-structured-streaming-with-apache-kafka-and-azure-cosmos-db"></a>Apache Spark Structured Streaming met Apache Kafka en Azure Cosmos DB gebruiken

Meer informatie over het gebruik van [Apache Spark](https://spark.apache.org/) [Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) lezen van gegevens uit [Apache Kafka](https://kafka.apache.org/) op Azure HDInsight en vervolgens de gegevens in Azure Cosmos DB-archief.

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is een wereldwijd gedistribueerde, multi-model-database. In dit voorbeeld wordt een model van de database SQL-API. Zie voor meer informatie de [Welkom bij Azure Cosmos DB](../cosmos-db/introduction.md) document.

Spark Structured Streaming is een streamverwerkingsengine gebaseerd op Spark SQL. Hiermee kunt u streamingberekeningen op dezelfde manier weergeven als batchberekeningen van statische gegevens. Zie voor meer informatie over Structured Streaming de [Structured Streaming Programming Guide](https://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) op Apache.org.

> [!IMPORTANT]  
> In dit voorbeeld wordt Spark 2.2 in HDInsight 3.6 gebruikt.
>
> Met de stappen in dit document wordt een Azure-resourcegroep gemaakt die zowel een Spark in HDInsight- als een Kafka in HDInsight-cluster bevat. Deze clusters bevinden zich beide binnen een Azure Virtual Network, waardoor het Spark-cluster rechtstreeks kan communiceren met het Kafka-cluster.
>
> Nadat u de stappen in dit document hebt doorlopen, moet u niet vergeten de clusters te verwijderen om overtollige kosten te voorkomen.

## <a name="create-the-clusters"></a>De clusters maken

Apache Kafka in HDInsight biedt geen toegang tot de Kafka-brokers via het openbare internet. Alles wat met Kafka praat moet zich in dezelfde Azure virtual network als de knooppunten in het Kafka-cluster. In dit voorbeeld bevinden de Kafka- en Spark-clusters zich in een Azure-netwerk. Het volgende diagram toont hoe de communicatie tussen de clusters met stromen:

![Diagram van Spark- en Kafka-clusters in een Azure Virtual Network](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]  
> De Kafka-service blijft beperkt tot communicatie binnen het virtuele netwerk. Andere services in het cluster, zoals SSH en Ambari, zijn toegankelijk via internet. Zie [Poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md) voor meer informatie over de openbare poorten die beschikbaar zijn voor HDInsight.

Hoewel u een Azure-netwerk, Kafka, maken kunt en Spark-clusters handmatig, is het eenvoudiger te gebruiken van een Azure Resource Manager-sjabloon. Gebruik de volgende stappen uit op een Azure-netwerk, Kafka, implementeren en Spark-clusters naar uw Azure-abonnement.

1. Gebruik de volgende knop om u aan te melden bij Azure en de sjabloon in de Azure Portal te openen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://azuredeploy.net/deploybutton.png"/>
    </a>

    De Azure Resource Manager-sjabloon bevindt zich in de GitHub-opslagplaats voor dit project ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Met deze sjabloon maakt u de volgende bronnen:

    * Een Kafka in HDInsight 3.6-cluster.

    * Een Spark in HDInsight 3.6-cluster.

    * Een Azure Virtual Network dat de HDInsight-clusters bevat.

        > [!NOTE]  
        > Het virtuele netwerk gemaakt met de sjabloon maakt gebruik van de adresruimte 10.0.0.0/16.

    * Een Azure Cosmos DB SQL API-database.

    > [!IMPORTANT]  
    > Voor de Structured Streaming-notebook die in dit voorbeeld wordt gebruikt, is Spark in HDInsight 3.6 vereist. Als u een eerdere versie van Spark in HDInsight gebruikt, worden er fouten gegenereerd bij gebruik van de notebook.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen in de **aangepaste implementatie** sectie:
   
    ![Aangepaste HDInsight-implementatie](./media/apache-kafka-spark-structured-streaming-cosmosdb/parameters.png)

    * **Abonnement**: Selecteer uw Azure-abonnement.
   
    * **Resourcegroep**: Maak een groep of Selecteer een bestaande resourcegroep. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie geografisch dicht bij u.

    * **Cosmos DB-accountnaam**: Deze waarde wordt gebruikt als de naam van het Cosmos DB-account.

    * **Clusternaam baseren**: Deze waarde wordt gebruikt als basisnaam voor de Spark en Kafka-clusters. Bijvoorbeeld, voeren **myhdi** maakt u een Spark-cluster met de naam __spark-myhdi__ en een Kafka-cluster met de naam **kafka-myhdi**.

    * **Cluster-versie**: De versie van het HDInsight-cluster.

        > [!IMPORTANT]  
        > In dit voorbeeld is getest met HDInsight 3.6, en werkt mogelijk niet met andere clustertypen.

    * **Gebruikersnaam voor aanmelding cluster**: De beheerdersnaam voor de Spark en Kafka-clusters.

    * **Cluster-wachtwoord voor Clusteraanmelding**: Het beheerderswachtwoord voor de Spark en Kafka-clusters.

    * **SSH-gebruikersnaam**: De SSH-gebruiker maken voor de Spark en Kafka-clusters.

    * **SSH wachtwoord**: Het wachtwoord voor de SSH-gebruiker voor de Spark en Kafka-clusters.

3. Lees de **voorwaarden** en schakel vervolgens het selectievakje **Ik ga akkoord met de bovenstaande voorwaarden** in.

4. Selecteer ten slotte **aankoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

> [!IMPORTANT]  
> Het duurt maximaal 45 minuten om de clusters, virtueel netwerk en Cosmos DB-account te maken.

## <a name="create-the-cosmos-db-database-and-collection"></a>De Cosmos DB-database en verzameling maken

Het project dat wordt gebruikt in dit document worden gegevens opgeslagen in Cosmos DB. Voordat u de code uitvoert, moet u eerst maken een _database_ en _verzameling_ in uw Cosmos DB-exemplaar. U moet ook de document-eindpunt ophalen en de _sleutel_ gebruikt voor het verifiëren van aanvragen voor Cosmos DB. 

Een manier om te doen dit is het gebruik van de [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest). Het volgende script maakt u een database met de naam `kafkadata` en een verzameling met de naam `kafkacollection`. Vervolgens wordt de primaire sleutel.

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
> Het eindpunt en sleutelwaarden die zijn, opslaan als ze nodig in de Jupyter-Notebooks zijn wel.

## <a name="get-the-apache-kafka-brokers"></a>De Apache Kafka-brokers ophalen

De code in dit voorbeeld maakt verbinding met Kafka-broker hosts in het Kafka-cluster. Om de adressen van de twee hosts voor Kafka-broker, gebruik het volgende PowerShell- of Bash-voorbeeld:

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
> De Bash-voorbeeld wordt verwacht dat `$CLUSTERNAME` bevat de naam van het Kafka-cluster.
>
> In dit voorbeeld wordt de [jq](https://stedolan.github.io/jq/) hulpprogramma voor het parseren van gegevens uit de JSON-document.

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Voer desgevraagd het wachtwoord voor het account aanmeldingswachtwoord (beheerder)

De uitvoer lijkt op het volgende:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Bewaar deze gegevens, zoals het wordt gebruikt in de volgende secties van dit document.

## <a name="get-the-notebooks"></a>De notebooks ophalen

De code voor het voorbeeld in dit document is beschikbaar op [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb).

## <a name="upload-the-notebooks"></a>De notebooks uploaden

Gebruik de volgende stappen voor het uploaden van de laptops van het project naar uw Spark in HDInsight-cluster:

1. Maak vanuit uw webbrowser verbinding met de Jupyter-notebook in uw Spark-cluster. In de volgende URL vervangt u `CLUSTERNAME` door de naam van uw __Spark__-cluster:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Typ desgevraagd de cluster-aanmelding (beheerder) en het cluster-wachtwoord die u hebt gebruikt bij het maken van het cluster.

2. Gebruik in de rechterbovenhoek van de pagina, de __uploaden__ knop uploaden de __Stream-taxi-gegevens-naar-kafka.ipynb__ bestand aan het cluster. Selecteer __Openen__ om te beginnen met uploaden.

3. Zoek de __Stream-taxi-gegevens-naar-kafka.ipynb__ vermelding in de lijst met notebooks en selecteer __uploaden__ knop ernaast.

4. Herhaal stap 1-3 laden van de __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ notebook.

## <a name="load-taxi-data-into-kafka"></a>Gegevens van de laden over taxi's in Kafka

Zodra de bestanden zijn geüpload, selecteert u de __Stream-taxi-gegevens-naar-kafka.ipynb__ vermelding om het notitieblok te openen. Volg de stappen in het notitieblok om gegevens te laden in Kafka.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>De gegevens over taxi's verwerken met behulp van Spark Structured Streaming

Uit de [Jupyter-Notebook](https://jupyter.org/) startpagina, selecteer de __Stream-data-from-Kafka-to-Cosmos-DB.ipynb__ vermelding. Volg de stappen in het notitieblok om gegevens te streamen van Kafka en in Azure Cosmos DB met behulp van Spark Structured Streaming.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u Apache Spark Structured Streaming, Zie de volgende documenten voor meer informatie over het werken met Apache Spark, Apache Kafka en Azure Cosmos DB:

* [Het gebruik van Apache Spark streaming (DStream) met Apache Kafka](hdinsight-apache-spark-with-kafka.md).
* [Beginnen met Jupyter-Notebook en Apache Spark in HDInsight](spark/apache-spark-jupyter-spark-sql.md)
* [Welkom bij Azure Cosmos DB](../cosmos-db/introduction.md)
