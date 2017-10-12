---
title: Een Apache Spark-cluster maken in Azure HDInsight | Microsoft Docs
description: HDInsight Spark-snelstartgids over het maken van een Apache Spark-cluster in HDInsight.
keywords: spark-snelstartgids,interactieve spark,interactieve query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: maxluk
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/07/2017
ms.author: maxluk
ms.openlocfilehash: e64720efa168a84193dc3f351def0e58979f7f86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Een Apache Spark-cluster maken in Azure HDInsight

In dit artikel leert u hoe u een Apache Spark-cluster maakt in Azure HDInsight en vervolgens een Spark SQL-query uitvoert op een Hive-tabel. Zie [Overzicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md) voor informatie over Spark in HDInsight.

   ![Snelstartdiagram waarin de stappen worden beschreven voor het maken van een Apache Spark-cluster in Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark-snelstartgids met behulp van Apache Spark in HDInsight. Geïllustreerde stappen: maken van een cluster; interactieve Spark-query uitvoeren")

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Maak vandaag nog uw gratis Azure-account](https://azure.microsoft.com/free).

## <a name="create-hdinsight-spark-cluster"></a>HDInsight Spark-cluster maken

In deze sectie maakt u een HDInsight Spark-cluster met behulp van een [Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor andere methoden voor het maken van clusters.

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende waarden in:

    ![HDInsight Spark-cluster maken met behulp van een Azure Resource Manager-sjabloon](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Spark-cluster maken in HDInsight met behulp van een Azure Resource Manager-sjabloon")

    * **Abonnement**: selecteer uw Azure-abonnement voor dit cluster.
    * **Resourcegroep**: maak een nieuwe resourcegroep of selecteer een bestaande. Deze resourcegroep wordt gebruikt om Azure-resources voor uw projecten te beheren.
    * **Locatie**: selecteer een locatie voor de resourcegroep. De sjabloon gebruikt deze locatie voor het maken van het cluster en als standaardclusteropslag.
    * **Clusternaam**: geef een naam op voor het HDInsight-cluster dat u wilt maken.
    * **Spark-versie**: selecteer **2.0** als Spark-versie die u in het cluster wilt installeren.
    * **Clusteraanmeldgegevens**: de standaardaanmeldnaam is admin.
    * **SSH-aanmeldgegevens**.

   Noteer deze waarden.  U hebt ze later in de zelfstudie nodig.

3. Selecteer **Ik ga akkoord met de bovenstaande voorwaarden**, selecteer **Vastmaken aan dashboard** en klik op **Kopen**. U ziet een nieuwe tegel met de titel Implementatie indienen voor Sjabloonimplementatie. Het duurt ongeveer 20 minuten om het cluster te maken.

Als u een probleem ondervindt met het maken van HDInsight-clusters, beschikt u mogelijk niet over de juiste machtigingen om dit te doen. Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) voor meer informatie.

> [!NOTE]
> In dit artikel wordt een Spark-cluster gemaakt dat [Azure Storage-blobs als clusteropslag](hdinsight-hadoop-use-blob-storage.md) gebruikt. U kunt ook een Spark-cluster maken dat [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) als standaardopslag gebruikt. Zie [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (Een HDInsight-cluster maken met Data Lake Store) voor instructies.
>
>

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Spark SQL-instructies uitvoeren op een Hive-tabel

SQL (Structured Query Language) is de meest voorkomende en gebruikte taal voor het uitvoeren van query's en het definiëren van gegevens. De grondleggers van Spark wilden deze kennis gebruiken en de bekende taal voor het opvragen van gegevens beschikbaar maken voor een breder publiek van analisten die willen werken met gegevens die zich in HDFS (Hadoop Distributed File System) bevinden. Spark SQL biedt dit. Het fungeert als een uitbreiding van Apache Spark voor het verwerken van gestructureerde gegevens, met behulp van de bekende SQL-syntaxis.

Spark SQL ondersteunt zowel SQL als HiveQL als querytalen. De mogelijkheden omvatten binding in Python, Scala en Java. U kunt er query’s mee uitvoeren op gegevens die zijn opgeslagen op vele locaties, zoals externe databases, gestructureerde gegevensbestanden (voorbeeld: JSON) en Hive-tabellen.

### <a name="running-spark-sql-on-an-hdinsight-cluster"></a>Spark SQL uitvoeren op een HDInsight-cluster

Als u een Jupyter-notebook gebruikt dat is geconfigureerd voor uw HDInsight Spark-cluster, krijgt u een vooraf ingestelde `sqlContext` waarmee u Hive-query's kunt uitvoeren met behulp van Spark SQL. In deze sectie leert u hoe een Jupyter-notebook start en vervolgens een eenvoudige Spark SQL-query uitvoert op een bestaande Hive-tabel (**hivesampletable**) die beschikbaar is op alle HDInsight-clusters.

1. Open de [Azure Portal](https://portal.azure.com/).

2. Als u ervoor hebt gekozen om het cluster vast te maken aan het dashboard, klikt u vanuit het dashboard op de clustertegel om de clusterblade te starten.

    Als u het cluster niet hebt vastgemaakt aan het dashboard, klikt u in het linkerdeelvenster op **HDInsight clusters** en vervolgens op het cluster dat u hebt gemaakt.

3. Klik in **Snelkoppelingen** op **Clusterdashboards** en klik vervolgens op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   ![Jupyter-notebook openen om de interactieve Spark SQL-query uit te voeren](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Jupyter-notebook openen om de interactieve Spark SQL-query uit te voeren")

   > [!NOTE]
   > Mogelijk kunt u de Jupyter-notebook voor uw cluster ook openen door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Maak een notebook. Klik op **Nieuw** en klik vervolgens op **PySpark**.

   ![Jupyter-notebook maken om de interactieve Spark SQL-query uit te voeren](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Jupyter-notebook maken om de interactieve Spark SQL-query uit te voeren")

   Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled (Untitled.pynb).

4. Klik bovenaan op de naam van de notebook en wijzig deze desgewenst in een beschrijvende naam.

    ![Een naam opgeven voor de Jupyter-notebook waarop de interactieve Spark-query wordt uitgevoerd](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Een naam opgeven voor de Jupyter-notebook waarop de interactieve Spark-query wordt uitgevoerd")

5.  Plak de volgende code in een lege cel en druk op **Shift+Enter** om de code uit te voeren. In de onderstaande code `%%sql` ('sql magic' genoemd) wordt de Jupyter-notebook geïnstrueerd om de vooraf ingestelde `sqlContext` te gebruiken voor het uitvoeren van de Hive-query. De query haalt de bovenste 10 rijen op van een Hive-tabel (**hivesampletable**) die standaard beschikbaar is in alle HDInsight-clusters.

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    ![Hive-query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-query in HDInsight Spark")

    Voor meer informatie over de context van de '`%%sql` magic' en de vooraf ingestelde waarden raadpleegt u [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md) (Voor een HDInsight-cluster beschikbare Jupyter-kernels).

    > [!NOTE]
    > Telkens wanneer u in Jupyter een query uitvoert, toont de venstertitel van uw webbrowser de status **(Bezet)** en de notebooktitel. Ook ziet u een gevulde cirkel naast de **PySpark**-tekst in de rechterbovenhoek. Wanneer de taak is voltooid, verandert deze in een lege cirkel.
    >
    >
    
6. Het scherm moet worden vernieuwd om de query-uitvoer weer te geven.

    ![Uitvoer van Hive-query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Uitvoer van Hive-query in HDInsight Spark")

7. Sluit de notebook af om de clusterresources vrij te geven nadat u de toepassing hebt uitgevoerd. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**.

8. Als u van plan bent de volgende stappen op een later tijdstip uit te voeren, zorg er dan voor dat u het HDInsight-cluster dat u in dit artikel hebt gemaakt, verwijdert. 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a>Volgende stap 

In dit artikel hebt u geleerd hoe u een HDInsight Spark-cluster maakt en een eenvoudige Spark SQL-query uitvoert. Ga naar het volgende artikel voor meer informatie over het gebruik van een HDInsight Spark-cluster om interactieve query's uit te voeren op voorbeeldgegevens.

> [!div class="nextstepaction"]
>[Interactieve query's uitvoeren op een HDInsight Spark-cluster](hdinsight-apache-spark-load-data-run-query.md)



