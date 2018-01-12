---
title: Apache Spark gestructureerde streamen met Kafka - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Apache Spark-streaming (DStream) om gegevens van of naar Apache Kafka. In dit voorbeeld kunt u gegevens met behulp van een Jupyter-notebook in Spark in HDInsight streamen.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/07/2017
ms.author: larryfr
ms.openlocfilehash: 39734153a757d4665f2a15d3af244d2fab9ad9d7
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>Spark gestructureerd streamen met Kafka in HDInsight gebruiken

Informatie over het gebruik van gestructureerde Spark-Streaming gegevens lezen uit Apache Kafka in Azure HDInsight.

Spark gestructureerd streaming is een stream-verwerkingsengine gebouwd op Spark SQL. Hiermee kunt u streaming berekeningen Express hetzelfde zijn als batch berekening op statische gegevens. Zie voor meer informatie over het gestructureerde Streaming de [gestructureerde Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) op Apache.org.

> [!IMPORTANT]
> In dit voorbeeld gebruikt 2.1 Spark in HDInsight 3.6. Gestructureerde Streaming wordt beschouwd als __alpha__ op Spark 2.1.
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
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De Azure Resource Manager-sjabloon bevindt zich op **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    Deze sjabloon maakt u de volgende bronnen:

    * Een Kafka op 3.6 HDInsight-cluster.
    * Een Spark in HDInsight 3.6 cluster.
    * Een virtueel netwerk in Azure die de HDInsight-clusters bevat.

    > [!IMPORTANT]
    > De gestructureerde streaming laptop gebruikt in dit voorbeeld vereist Spark in HDInsight 3.6. Als u een eerdere versie van Spark in HDInsight gebruikt, ontvangt u fouten bij gebruik van de notebook.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen op de **aangepaste implementatie** sectie:
   
    ![Aangepaste HDInsight-implementatie](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Resourcegroep**: Maak een groep of een bestaande set selecteren. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie geografisch dicht bij u.

    * **Clusternaam baseren**: deze waarde wordt gebruikt als de basisnaam aan voor de Spark en Kafka-clusters. Bijvoorbeeld, voeren **hdi** maakt een Spark-cluster spark hdi__ met de naam en een Kafka-cluster met de naam **kafka hdi**.

    * **Aanmeldingsnaam van gebruiker cluster**: de beheerdersgebruikersnaam voor Spark en Kafka-clusters.

    * **Aanmeldingswachtwoord cluster**: het beheerderswachtwoord voor de Spark en Kafka clusters.

    * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor Spark en Kafka-clusters.

    * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de Spark en Kafka clusters.

3. Lees de **voorwaarden en bepalingen**, en selecteer vervolgens **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.

4. Controleer ten slotte **vastmaken aan dashboard** en selecteer vervolgens **aankoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

Zodra de resources zijn gemaakt, wordt een overzichtspagina weergegeven.

![Informatie over de bron voor het vnet en clusters](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Merk op dat de namen van de HDInsight-clusters **spark BASENAME** en **kafka BASENAME**, waarbij BASENAME is de naam die u hebt opgegeven in de sjabloon. U kunt deze namen in latere stappen gebruiken bij het verbinden met de clusters.

## <a name="get-the-kafka-brokers"></a>De Kafka beleggingsmakelaars ophalen

De code in dit voorbeeld verbindt met Kafka broker hosts in het cluster Kafka. Om de adressen van de twee Kafka broker hosts vinden, gebruikt u het volgende PowerShell- of Bash-voorbeeld:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
```

Voer desgevraagd het wachtwoord voor de cluster-serviceaccount voor aanmelding (admin)

> [!NOTE]
> In dit voorbeeld verwacht `$CLUSTERNAME` aan de naam van het cluster Kafka bevatten.
>
> In dit voorbeeld wordt de [jq](https://stedolan.github.io/jq/) hulpprogramma voor het parseren van gegevens buiten het JSON-document.

De uitvoer lijkt op het volgende:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Deze gegevens niet opslaan omdat het wordt gebruikt in de volgende secties van dit document.

## <a name="get-the-notebooks"></a>De laptops ophalen

De code voor het voorbeeld in dit document beschreven is beschikbaar op [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-the-notebooks"></a>De laptops uploaden

Gebruik de volgende stappen voor het uploaden van de laptops van het project met uw Spark in HDInsight-cluster:

1. In uw webbrowser verbinding maken met de Jupyter-notebook in Spark-cluster. Vervang in de volgende URL `CLUSTERNAME` met de naam van uw __Spark__ cluster:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Wanneer u wordt gevraagd, typt u de cluster-aanmelding (admin) en het wachtwoord die worden gebruikt wanneer u het cluster hebt gemaakt.

2. Gebruik vanaf boven aan de rechterkant van de pagina, de __uploaden__ knop voor het uploaden van de __stroom Tweets To_Kafka.ipynb__ bestand aan het cluster. Selecteer __Open__ het uploaden wordt gestart.

    ![Gebruik de knop uploaden te selecteren en een laptop uploaden](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Selecteer het bestand KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Zoeken naar de __stroom Tweets To_Kafka.ipynb__ vermelding in de lijst van notitieblokken en selecteer __uploaden__ knop ernaast.

    ![Als u wilt uploaden de notebook, gebruikt u de knop uploaden voor de vermelding KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. Herhaal stap 1-3 laden van de __Spark-gestructureerd-Streaming-van-Kafka.ipynb__ notebook.

## <a name="load-tweets-into-kafka"></a>Tweets in Kafka laden

Zodra de bestanden zijn geüpload, selecteert u de __stroom Tweets To_Kafka.ipynb__ vermelding de notebook te openen. Volg de stappen in de notebook tweets in Kafka laden.

## <a name="process-tweets-using-spark-structured-streaming"></a>Proces tweets met gestructureerde Spark-Streaming

Selecteer in de startpagina Jupyter-Notebook de __Spark-gestructureerd-Streaming-van-Kafka.ipynb__ vermelding. Volg de stappen in de notebook tweets laden vanaf Kafka met gestructureerde Spark-Streaming.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u gestructureerde Spark-Streaming, Zie de volgende documenten voor meer informatie over het werken met Spark en Kafka:

* [Het gebruik van Spark-streaming (DStream) met Kafka](hdinsight-apache-spark-with-kafka.md).
* [Beginnen met Jupyter-Notebook en Spark in HDInsight](spark/apache-spark-jupyter-spark-sql.md)