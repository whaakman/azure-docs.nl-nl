---
title: Zeppelin-notebooks gebruiken met Apache Spark-cluster in Azure HDInsight | Microsoft Docs
description: Stapsgewijze instructies voor het gebruik van Zeppelin-notebooks met Apache Spark-clusters op Azure HDInsight.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 19e363c9802d712937b4c1e9db550346e60a01c0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Zeppelin-notebooks met Apache Spark-cluster in Azure HDInsight gebruiken

HDInsight Spark-clusters bevatten Zeppelin-notebooks die u gebruiken kunt om uit te voeren Spark taken. In dit artikel leert u hoe u de notebook Zeppelin gebruikt op een HDInsight-cluster.

> [!NOTE]
> Zeppelin-notebooks zijn alleen beschikbaar voor Spark 1.6.3 in HDInsight 3.5 en Spark 2.1.0 in HDInsight 3.6.
>

**Vereisten:**

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Een laptop Zeppelin starten
1. Klik in de blade Spark-cluster op **Cluster-Dashboard**, en klik vervolgens op **Zeppelin-Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.
   
   > [!NOTE]
   > U mogelijk ook de Zeppelin-Notebook voor uw cluster bereiken door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. Maak een nieuwe notebook. Klik in het headerdeelvenster **Notebook**, en klik vervolgens op **nieuwe notitie maken**.
   
    ![Maak een nieuwe Zeppelin-notebook](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "een nieuwe Zeppelin-notebook maken")
   
    Voer een naam voor de notebook en klik vervolgens op **opmerking maken**.
3. Controleer ook of dat de notebook-header bevat een verbonden status. Dit wordt aangeduid met een groen punt in de rechterbovenhoek.
   
    ![Zeppelin-notebook status](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin-notebook status")
4. Laad voorbeeldgegevens in een tijdelijke tabel. Wanneer u een Spark-cluster in HDInsight, het voorbeeldgegevensbestand maakt **hvac.csv**, wordt gekopieerd naar de bijbehorende opslagaccount onder **\HdiSamples\SensorSampleData\hvac**.
   
    Plak het volgende codefragment in de lege alinea die standaard in de nieuwe laptop is gemaakt.
   
        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter
   
        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Druk op **SHIFT + ENTER** of klik op de **afspelen** knop voor de alinea om uit te voeren van het fragment. De status van de rechterbenedenhoek van de alinea moet de voortgang van READY, PENDING, die wordt uitgevoerd op voltooid. De uitvoer wordt weergegeven aan de onderkant van dezelfde paragraph. De schermafbeelding ziet er als volgt:
   
    ![Een tijdelijke tabel maken van ruwe gegevens](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "een tijdelijke tabel maken van ruwe gegevens")
   
    U kunt ook een titel op elke alinea opgeven. Klik in de rechterhoek op de **instellingen** pictogram en klik vervolgens op **titel weergeven**.
5. U kunt nu Spark SQL-instructies uitvoeren op de **hvac** tabel. Plak de volgende query in een nieuwe alinea. De query haalt de gebouw-ID en het verschil tussen het doel en de werkelijke temperaturen voor elke bouwen op een bepaalde datum. Druk op **SHIFT + ENTER**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    De **% sql** instructie aan het begin vertelt de notebook de interpreter Livy Scala gebruiken.
   
    De volgende schermafbeelding ziet u de uitvoer.
   
    ![Voer een Spark SQL-instructie met behulp van de notebook](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "een Spark SQL-instructie met behulp van de notebook uitvoert")
   
     Klik op de weergaveopties (gemarkeerd rechthoek) om te schakelen tussen de verschillende manieren voor dezelfde uitvoer. Klik op **instellingen** kiezen welke consitutes de sleutel en de waarden in de uitvoer. Maakt gebruik van de bovenstaande schermafbeelding **buildingID** als de sleutel en het gemiddelde van **temp_diff** als de waarde.
6. U kunt ook Spark SQL-instructies voor het gebruik van variabelen in de query uitvoeren. Het volgende fragment toont hoe een variabele definiëren **Temp**, in de query met de mogelijke waarden die u wilt zoeken met. Wanneer u de query voor het eerst uitvoert, wordt een vervolgkeuzelijst automatisch ingevuld met de waarden die u hebt opgegeven voor de variabele.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    In dit fragment plakken in een nieuwe alinea en druk op **SHIFT + ENTER**. De volgende schermafbeelding ziet u de uitvoer.
   
    ![Voer een Spark SQL-instructie met behulp van de notebook](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "een Spark SQL-instructie met behulp van de notebook uitvoert")
   
    U kunt voor de volgende query's, selecteert u een nieuwe waarde in de vervolgkeuzelijst en voer de query opnieuw uit. Klik op **instellingen** kiezen welke consitutes de sleutel en de waarden in de uitvoer. Maakt gebruik van de bovenstaande schermafbeelding **buildingID** als de sleutel, wordt het gemiddelde van **temp_diff** als de waarde en **targettemp** als de groep.
7. Start de interpreter Livy om af te sluiten van de toepassing opnieuw. Om dit te doen interpreter instellingen openen door te klikken op de geregistreerde gebruikersnaam in de rechterbovenhoek en klik vervolgens op **Interpreter**.
   
    ![Start interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-uitvoer")
8. Schuif naar Livy interpreter instellingen en klik vervolgens op **opnieuw**.
   
    ![Opnieuw opstarten van de intepreter Livy](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "opnieuw opstarten van de intepreter Zeppelin")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Hoe gebruik ik externe pakketten met de notebook
U kunt de notebook Zeppelin in Apache Spark-cluster in HDInsight (Linux) configureren voor het gebruik van externe, community bijgedragen pakketten die niet opgenomen out-of-the-box in het cluster zijn. U kunt zoeken in de [Maven opslagplaats](http://search.maven.org/) voor de volledige lijst met pakketten die beschikbaar zijn. U kunt ook een lijst met beschikbare pakketten opvragen uit andere bronnen. Bijvoorbeeld, een volledige lijst met pakketten community bijgedragen is beschikbaar op [Spark pakketten](http://spark-packages.org/).

In dit artikel ziet u hoe u de [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) pakket met de Jupyter-notebook.

1. Open interpreter instellingen. Klikt u op de geregistreerde gebruikersnaam in de rechterbovenhoek en klik vervolgens op **Interpreter**.
   
    ![Start interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-uitvoer")
2. Schuif naar Livy interpreter instellingen en klik vervolgens op **bewerken**.
   
    ![Wijzig de instellingen van de interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "interpreter instellingen wijzigen")
3. Voeg een nieuwe sleutel aangeroepen **livy.spark.jars.packages** en stel de waarde in de notatie `group:id:version`. Dus als u wilt gebruiken de [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) pakket, moet u de waarde van de sleutel instellen `com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Wijzig de instellingen van de interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "interpreter instellingen wijzigen")
   
    Klik op **opslaan** en start vervolgens opnieuw de interpreter Livy.
4. **Tip**: als u wilt weten hoe moet worden uitgevoerd op de waarde van de sleutel dat hierboven is opgegeven, wordt hier hoe.
   
    a. Het pakket niet vinden in de opslagplaats met Maven. Voor deze zelfstudie gebruikt we [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Verzamel de waarden voor uit de opslagplaats **GroupId**, **artefact-id**, en **versie**.
   
    ![Externe pakketten gebruiken met Jupyter-notebook](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "externe pakketten gebruiken met Jupyter-notebook")
   
    c. De drie waarden, gescheiden door een dubbele punt (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Waar worden de Zeppelin-notebooks opgeslagen?
Zeppelin-notebooks worden opgeslagen in de cluster-headnodes. Dus als u het cluster verwijdert, wordt de laptops ook verwijderd. Als u uw notitieblokken voor later gebruik op andere clusters behouden wilt, moet u deze exporteren wanneer u klaar bent met het uitvoeren van de taken. Als u wilt exporteren een laptop, klikt u op de **exporteren** pictogram zoals weergegeven in de onderstaande afbeelding.

![Downloaden van de notebook](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "downloaden van de notebook")

Hiermee wordt de notebook opgeslagen als een JSON-bestand op uw downloadlocatie.

## <a name="livy-session-management"></a>Sessiebeheer Livy
Wanneer u de eerste alinea in de code in uw laptop Zeppelin uitvoert, wordt een nieuwe sessie van Livy gemaakt in uw HDInsight Spark-cluster. Deze sessie worden verdeeld over alle Zeppelin-notebooks die u maakt. Als de Livy sessie is beëindigd voor een bepaalde reden (cluster opnieuw opstarten, enzovoort), is het niet mogelijk om uit te voeren taken van de notebook Zeppelin.

In dat geval moet u de volgende stappen uitvoeren voordat u taken uitvoert vanuit een Zeppelin-notebook kunt starten. 

1. De interpreter Livy van de notebook Zeppelin opnieuw opstarten Om dit te doen interpreter instellingen openen door te klikken op de geregistreerde gebruikersnaam in de rechterbovenhoek en klik vervolgens op **Interpreter**.
   
    ![Start interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-uitvoer")
2. Schuif naar Livy interpreter instellingen en klik vervolgens op **opnieuw**.
   
    ![Opnieuw opstarten van de intepreter Livy](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "opnieuw opstarten van de intepreter Zeppelin")
3. Een codecel uitvoeren vanaf een bestaande Zeppelin-notebook. Hiermee maakt u een nieuwe sessie van Livy in het HDInsight-cluster.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md 







