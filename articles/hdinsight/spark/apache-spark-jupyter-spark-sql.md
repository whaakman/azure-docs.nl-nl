---
title: Een Apache Spark-cluster maken in Azure HDInsight | Microsoft Docs
description: HDInsight Spark-snelstartgids over het maken van een Apache Spark-cluster in HDInsight.
keywords: spark-snelstartgids,interactieve spark,interactieve query,hdinsight spark,azure spark
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/01/2018
ms.author: jgao
ms.openlocfilehash: baad137a6f982df987faf95d7c7c595698e8e399
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a>Een Apache Spark-cluster maken in Azure HDInsight

Informatie over het maken van een Apache Spark-cluster in Azure HDInsight en het uitvoeren van Spark SQL-query's op basis van Hive-tabellen. Zie [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md) voor informatie over Spark in HDInsight.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Voordat u met deze zelfstudie begint, moet u een Azure-abonnement hebben. Zie [Create your free Azure account](https://azure.microsoft.com/free) (Maak uw gratis Azure-account).

## <a name="create-hdinsight-spark-cluster"></a>HDInsight Spark-cluster maken

U maakt een HDInsight Spark-cluster met behulp van een [Azure Resource Manager-sjabloon](../hdinsight-hadoop-create-linux-clusters-arm-templates.md). U vindt de sjabloon in [github](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/). Zie [HDInsight-clusters maken](../hdinsight-hadoop-provision-linux-clusters.md) voor andere methoden voor het maken van clusters.

1. Klik op de volgende afbeelding om de sjabloon in Azure Portal te openen.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Voer de volgende waarden in:

    ![HDInsight Spark-cluster maken met behulp van een Azure Resource Manager-sjabloon](./media/apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Spark-cluster maken in HDInsight met behulp van een Azure Resource Manager-sjabloon")

    * **Abonnement**: selecteer het Azure-abonnement dat u hebt gebruikt om dit cluster te maken.
    * **Resourcegroep**: maak een nieuwe resourcegroep of selecteer een bestaande. Deze resourcegroep wordt gebruikt om Azure-resources voor uw projecten te beheren.
    * **Locatie**: selecteer een locatie voor de resourcegroep. De sjabloon gebruikt deze locatie voor het maken van het cluster en als standaardclusteropslag.
    * **Clusternaam**: geef een naam op voor het HDInsight-cluster dat u wilt maken.
    * **Clusteraanmeldgegevens**: de standaardaanmeldnaam is admin. Kies een wachtwoord voor de clusteraanmelding.
    * **SSH-aanmeldgegevens**. Kies een wachtwoord voor de SSH-gebruiker.

3. Selecteer **Ik ga akkoord met de bovenstaande voorwaarden**, selecteer **Vastmaken aan dashboard** en klik op **Kopen**. U ziet een nieuwe tegel met de titel **Implementatie van sjabloonimplementatie**. Het duurt ongeveer 20 minuten om het cluster te maken.

Als u een probleem ondervindt met het maken van HDInsight-clusters, beschikt u mogelijk niet over de juiste machtigingen om dit te doen. Zie [Vereisten voor toegangsbeheer](../hdinsight-administer-use-portal-linux.md#create-clusters) voor meer informatie.

> [!NOTE]
> In dit artikel wordt een Spark-cluster gemaakt dat [Azure Storage-blobs als clusteropslag](../hdinsight-hadoop-use-blob-storage.md) gebruikt. U kunt ook een Spark-cluster maken dat [Azure Data Lake Store](../hdinsight-hadoop-use-data-lake-store.md) als standaardopslag gebruikt. Zie [Create an HDInsight cluster with Data Lake Store](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) (Een HDInsight-cluster maken met Data Lake Store) voor instructies.
>
>

## <a name="create-a-jupyter-notebook"></a>Een Jupyter-notebook maken

[Jupyter Notebook](http://jupyter.org) is een interactieve omgeving voor notebooks waarin verschillende programmeertalen worden ondersteund, en waar u met uw gegevens kunt werken, code met Markdown-tekst kunt combineren en eenvoudige visualisaties kunt uitvoeren. Apache Spark in HDInsight bevat tevens [Zeppelin Notebook](apache-spark-zeppelin-notebook.md). Jupyter Notebook wordt in deze zelfstudie gebruikt.

**Een Jupyter-notebook maken**

1. Open de [Azure Portal](https://portal.azure.com/).

2. Open de Apache Spark-cluster die u hebt gemaakt. Zie [Clusters tonen en vermelden](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters) voor instructies.

3. Klik in het portal op **Clusterdashboards** en vervolgens op **Jupyter-notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   ![Jupyter Notebook openen om de interactieve Spark SQL-query uit te voeren](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Jupyter Notebook openen om de interactieve Apache Spark SQL-query uit te voeren")

   > [!NOTE]
   > U kunt Jupyter Notebook voor uw cluster ook openen door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. Klik op **Nieuw** en vervolgens op **PySpark** om een notebook te maken. In Jupyter-notebooks in HDInsight-clusters worden drie kernels ondersteund: **PySpark**, **PySpark3** en **Spark**. In deze zelfstudie wordt de **PySpark**-kernel gebruikt. Zie [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](apache-spark-jupyter-notebook-kernels.md) (Jupyter-notebookkernels gebruiken met Apache Spark-clusters in HDInsight) voor meer informatie over de kernels en de voordelen van het gebruik van **PySpark**.

   ![Jupyter Notebook maken om de interactieve Spark SQL-query uit te voeren](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-spark-sql-query.png "Jupyter Notebook maken om de interactieve Apache Spark SQL-query uit te voeren")

   Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled (Untitled.pynb).

4. Klik bovenaan op de naam van de notebook en wijzig deze desgewenst in een beschrijvende naam.

    ![Een naam opgeven voor de Jupyter Notebook waarop de interactieve Spark-query wordt uitgevoerd](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Een naam opgeven voor de Jupyter Notebook waarop de interactieve Apache Spark-query wordt uitgevoerd")

## <a name="run-spark-sql-statements-on-a-hive-table"></a>Spark SQL-instructies uitvoeren op een Hive-tabel

SQL (Structured Query Language) is de meest voorkomende en gebruikte taal voor het uitvoeren van query's en het definiëren van gegevens. Spark SQL fungeert als een uitbreiding van Apache Spark voor het verwerken van gestructureerde gegevens, met behulp van de bekende SQL-syntaxis.

Spark SQL ondersteunt zowel SQL als HiveQL als querytalen. De mogelijkheden omvatten binding in Python, Scala en Java. U kunt er query’s mee uitvoeren op gegevens die zijn opgeslagen op vele locaties, zoals externe databases, gestructureerde gegevensbestanden (voorbeeld: JSON) en Hive-tabellen.

Zie [Interactieve query's uitvoeren op Apache Spark-clusters in HDInsight](apache-spark-load-data-run-query.md) voor een voorbeeld van het lezen van gegevens vanaf een CSV-bestand in plaats van een Hive-tabel.

**Apache Spark SQL uitvoeren**

1. Wanneer u de notebook voor het eerst start, voert de kernel enkele taken in de achtergrond uit. Wacht tot de kernel gereed is. Wanneer u een lege cirkel naast de naam van de kernel in de notebook ziet, is de kernel gereed. Gevulde cirkel geeft aan dat de kernel bezet is.

    ![Hive-query in HDInsight Spark](./media/apache-spark-jupyter-spark-sql/jupyter-spark-kernel-status.png "Hive-query in HDInsight Spark")

2. Wanneer de kernel gereed is, plakt u de volgende code in een lege cel en drukt u op **Shift+Enter** om de code uit te voeren. Met de opdracht worden de Hive-tabellen in het cluster weergegeven:

    ```PySpark
    %%sql
    SHOW TABLES
    ```
    Als u een Jupyter Notebook gebruikt dat is geconfigureerd voor uw HDInsight Spark-cluster, krijgt u een vooraf ingestelde `sqlContext` waarmee u Hive-query's kunt uitvoeren met behulp van Apache Spark SQL. `%%sql` instrueert Jupyter Notebook gebruik te maken van de vooraf ingestelde `sqlContext` om de Hive-query uit te voeren. De query haalt de bovenste tien rijen op uit een Hive-tabel (**hivesampletable**) die standaard worden meegeleverd met alle HDInsight-clusters. Het duurt ongeveer 30 seconden om de resultaten op te halen. De uitvoer ziet er als volgt uit: 

    ![Hive-query in HDInsight Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-query in HDInsight Spark")

    Voor meer informatie over de context van de '`%%sql` magic' en de vooraf ingestelde waarden raadpleegt u [Jupyter kernels available for an HDInsight cluster](apache-spark-jupyter-notebook-kernels.md) (Voor een HDInsight-cluster beschikbare Jupyter-kernels).

    Telkens wanneer u in Jupyter een query uitvoert, toont de venstertitel van uw webbrowser de status **(Bezet)** en de notebooktitel. Ook ziet u een gevulde cirkel naast de **PySpark**-tekst in de rechterbovenhoek.
    
2. Voer een andere query uit om de gegevens in `hivesampletable` te zien.

    ```PySpark
    %%sql
    SELECT * FROM hivesampletable LIMIT 10
    ```
    
    Het scherm wordt vernieuwd om de query-uitvoer weer te geven.

    ![Uitvoer van Hive-query in HDInsight Spark](./media/apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Uitvoer van Hive-query in HDInsight Spark")

2. Klik in het menu **Bestand** in de notebook op **Sluiten en stoppen**. Als de notebook wordt afgesloten, komen de clusterbronnen vrij.

3. Als u van plan bent de volgende stappen op een later tijdstip uit te voeren, zorg er dan voor dat u het HDInsight-cluster dat u in dit artikel hebt gemaakt, verwijdert. 

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Volgende stappen 

In dit artikel hebt u geleerd hoe u een HDInsight Spark-cluster maakt en een eenvoudige Spark SQL-query uitvoert. Ga naar het volgende artikel voor meer informatie over het gebruik van een HDInsight Spark-cluster om interactieve query's uit te voeren op voorbeeldgegevens.

> [!div class="nextstepaction"]
>[Interactieve query's uitvoeren op een HDInsight Spark-cluster](apache-spark-load-data-run-query.md)



