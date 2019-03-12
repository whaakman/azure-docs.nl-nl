---
title: Zeppelin-notebooks gebruiken met Apache Spark-cluster in Azure HDInsight
description: Stapsgewijze instructies voor het Zeppelin-notebooks gebruiken met Apache Spark-clusters in Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/04/2019
ms.openlocfilehash: 019232308ec5fa6d735e4499c3fb5f3ac2727e2d
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57766391"
---
# <a name="use-apache-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Apache Zeppelin-notebooks gebruiken met Apache Spark-cluster in Azure HDInsight

HDInsight Spark-clusters omvatten [Apache Zeppelin](https://zeppelin.apache.org/) laptops die u gebruiken kunt om uit te voeren [Apache Spark](https://spark.apache.org/) taken. In dit artikel leert u hoe u kunt de Zeppelin-notitieblok gebruiken op een HDInsight-cluster.

**Vereisten:**

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.
* Het URI-schema voor de primaire opslag van clusters. Dit zou zijn `wasb://` voor Azure Blob Storage, `abfs://` voor Azure Data Lake Storage Gen2 of `adl://` voor Azure Data Lake Storage Gen1. Als veilige overdracht is ingeschakeld voor Blob Storage of Data Lake Storage Gen2, de URI zou worden `wasbs://` of `abfss://`, respectievelijk.  Zie ook [veilige overdracht in Azure Storage vereisen](../../storage/common/storage-require-secure-transfer.md) voor meer informatie.

## <a name="launch-an-apache-zeppelin-notebook"></a>Een Apache Zeppelin-notitieblok starten

1. In het Spark-cluster **overzicht**, selecteer **Zeppelin-notitieblok** van **Clusterdashboards**. Voer de beheerdersreferenties voor het cluster.  

   > [!NOTE]  
   > U kunt ook contact opnemen de Zeppelin-Notebook voor uw cluster door het openen van de volgende URL in uw browser. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Maak een nieuwe notebook. Navigeer in het deelvenster koptekst naar **Notebook** > **nieuwe notitie maken**.

    ![Maak een nieuwe Zeppelin-notebook](./media/apache-spark-zeppelin-notebook/hdinsight-create-zeppelin-notebook.png "een nieuwe Zeppelin-notebook maken")

    Voer een naam voor de notebook en selecteer vervolgens **opmerking maken**.

3. Zorg ervoor dat de notebook-header geeft de status van een verbonden. Dit wordt aangeduid met een groene punt in de rechterbovenhoek.

    ![Zeppelin-notebook status](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-connected.png "Zeppelin notebook status")

4. Laad voorbeeldgegevens in een tijdelijke tabel. Wanneer u een Spark-cluster in HDInsight, het bestand met voorbeeldgegevens maakt `hvac.csv`, wordt gekopieerd naar het bijbehorende opslagaccount onder `\HdiSamples\SensorSampleData\hvac`.

    Plak het volgende codefragment in de lege alinea die wordt standaard in de nieuwe notebook gemaakt.

    ```scala
    %livy2.spark
    //The above magic instructs Zeppelin to use the Livy Scala interpreter

    // Create an RDD using the default Spark context, sc
    val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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
    ```

    Druk op **SHIFT + ENTER** of klik op de **afspelen** knop van de alinea om uit te voeren in het codefragment. De status van de rechterbenedenhoek van de alinea moet de voortgang van gereed is, in behandeling, die wordt uitgevoerd op voltooid. De uitvoer wordt weergegeven aan de onderkant van stejném paragraph. De schermafbeelding ziet er als volgt uit:

    ![Maak een tijdelijke tabel van onbewerkte gegevens](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-load-data.png "een tijdelijke tabel maken van onbewerkte gegevens")

    U kunt ook een titel aan elke alinea opgeven. Selecteer in de rechterhoek van het lid de **instellingen** pictogram (sprocket) en selecteer vervolgens **titel weergeven**.  

    > [!NOTE]  
    > % spark2 interpreter wordt niet ondersteund in Zeppelin-notebooks in alle HDInsight-versies en % sh interpreter worden niet ondersteund in HDInsight 4.0 en hoger.

5. U kunt nu Spark SQL-instructies uitvoeren op de `hvac` tabel. De volgende query in een nieuwe alinea te plakken. De query haalt de gebouw-ID en het verschil tussen het doel en de werkelijke temperaturen voor elke bouwen op een bepaalde datum. Druk op **SHIFT + ENTER**.

    ```sql
    %sql
    select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
    ```  

    De **% sql** instructie aan het begin wordt de notebook geïnstrueerd om het gebruik van de Livy Scala-interpreter.

6. Selecteer de **staafdiagram** pictogram om de weergave te wijzigen.  **instellingen voor**, die wordt weergegeven nadat u hebt geselecteerd **staafdiagram**, kunt u kiezen **sleutels**, en **waarden**.  De volgende schermafbeelding ziet u de uitvoer.

    ![Een Spark SQL-instructie met behulp van de notebook uitvoert](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-1.png "voert u een Spark SQL-instructie uit met behulp van de notebook")

7. U kunt ook Spark SQL-instructies voor het gebruik van variabelen in de query uitvoeren. Het volgende fragment ziet u hoe u een variabele, `Temp`, in de query met de mogelijke waarden die u wilt zoeken met. Wanneer u de query voor het eerst uitvoert, wordt een vervolgkeuzelijst wordt automatisch gevuld met de waarden die u hebt opgegeven voor de variabele.

    ```sql
    %sql  
    select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}"
    ```

    Dit fragment plakken in een nieuwe alinea en druk op **SHIFT + ENTER**. Selecteer vervolgens **65** uit de **Temp** vervolgkeuzelijst is. 

8. Selecteer de **staafdiagram** pictogram om de weergave te wijzigen.  Selecteer vervolgens **instellingen** en breng de volgende wijzigingen:

    * **Groepen:**  Voeg **targettemp**.  
    * **Waarden:** 1. Verwijder **datum**.  2. Add **temp_diff**.  3.  Wijzigen van de aggregator van **som** naar **AVG**.  

    De volgende schermafbeelding ziet u de uitvoer.

    ![Een Spark SQL-instructie met behulp van de notebook uitvoert](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-spark-query-2.png "voert u een Spark SQL-instructie uit met behulp van de notebook")

9. Start opnieuw op de Livy-interpreter om de toepassing af te sluiten. Om dit te doen, open interpreter instellingen door het selecteren van de aangemelde in de naam van de gebruiker in de rechterbovenhoek en selecteer vervolgens **Interpreter**.  

    ![Start interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-uitvoer")

10. Schuif naar **livy**, en selecteer vervolgens **opnieuw**.  Selecteer **OK** bij de prompt.

    ![Opnieuw opstarten van de intepreter Livy](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "opnieuw opstarten van de intepreter Zeppelin")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Hoe ik externe pakketten gebruiken met de notebook?
U kunt de Zeppelin-notitieblok in Apache Spark-cluster in HDInsight gebruiken van externe, door de community geleverde pakketten die niet opgenomen out-of-the-box in het cluster zijn configureren. U kunt zoeken naar de [Maven-opslagplaats](https://search.maven.org/) voor de volledige lijst met pakketten die beschikbaar zijn. U kunt ook een lijst met beschikbare pakketten ophalen uit andere bronnen. Bijvoorbeeld, een volledige lijst van door de community geleverde pakketten is beschikbaar op [Spark pakketten](https://spark-packages.org/).

In dit artikel ziet u hoe u de [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) pakket met de Jupyter-notebook.

1. Instellingen voor Open-interpreter. In de rechterbovenhoek van de aangemelde selecteren in de gebruikersnaam van de en selecteer vervolgens **Interpreter**.

    ![Start interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-uitvoer")

2. Schuif naar **livy**en selecteer vervolgens **bewerken**.

    ![Wijzigen van instellingen voor interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "interpreter instellingen wijzigen")

3. Voeg een nieuwe sleutel met de naam `livy.spark.jars.packages`, en stel de waarde in de indeling `group:id:version`. Als u wilt gebruiken de [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) pakket, moet u de waarde van de sleutel instellen `com.databricks:spark-csv_2.10:1.4.0`.

    ![Wijzigen van instellingen voor interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "interpreter instellingen wijzigen")

    Selecteer **opslaan** en start vervolgens opnieuw de Livy-interpreter.

4. Als u wilt weten hoe u om naar de waarde van de sleutel die hierboven zijn ingevoerd, ziet u hier hoe.
   
    a. Het pakket niet vinden in de Maven-opslagplaats. Voor deze zelfstudie gebruikt we [spark-csv](https://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Verzamel uit de opslagplaats en de waarden voor **groeps-id**, **ArtifactId**, en **versie**.
   
    ![Externe pakketten gebruiken met Jupyter-notebook](./media/apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "externe pakketten gebruiken met Jupyter-notebook")
   
    c. De drie waarden, gescheiden door een dubbele punt (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Waar worden de Zeppelin-notebooks opgeslagen?
Zeppelin-notebooks worden opgeslagen in de hoofdknooppunten van het cluster. Dus als u het cluster verwijdert, wordt de notebooks eveneens worden verwijderd. Als u uw notitieblokken voor later gebruik in andere clusters behouden wilt, moet u deze exporteren wanneer u klaar bent met het uitvoeren van de taken. Voor het exporteren van een laptop, selecteer de **exporteren** pictogram zoals wordt weergegeven in de onderstaande afbeelding.

![Downloaden van de notebook](./media/apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "downloaden van de notebook")

Dit wordt de notebook opgeslagen als JSON-bestand in uw downloadlocatie.

## <a name="livy-session-management"></a>Sessiebeheer Livy
Wanneer u het eerste lid van de code in uw Zeppelin-notitieblok uitvoert, wordt een nieuwe sessie van Livy gemaakt in uw HDInsight Spark-cluster. Deze sessie wordt gedeeld door alle Zeppelin-notitieblokken die u maakt. Als de Livy sessie is afgesloten om een bepaalde reden (cluster opnieuw opstarten, enzovoort), wordt het niet mogelijk om uit te voeren taken vanuit de Zeppelin-notitieblok.

In dat geval moet u de volgende stappen uitvoeren voordat u kunt beginnen met het uitvoeren van taken van een Zeppelin-notitieblok.  

1. Start opnieuw op de Livy-interpreter vanuit het Zeppelin-notitieblok. Om dit te doen, open interpreter instellingen door het selecteren van de aangemelde in de naam van de gebruiker in de rechterbovenhoek en selecteer vervolgens **Interpreter**.

    ![Start interpreter](./media/apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Hive-uitvoer")

2. Schuif naar **livy**en selecteer vervolgens **opnieuw**.

    ![Opnieuw opstarten van de intepreter Livy](./media/apache-spark-zeppelin-notebook/hdinsight-zeppelin-restart-interpreter.png "opnieuw opstarten van de intepreter Zeppelin")

3. Voer een codecel uit een bestaand Zeppelin-notitieblok. Hiermee maakt u een nieuwe Livy-sessie in het HDInsight-cluster.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met BI: Interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om te maken en verzenden van Apache Spark Scala-toepassingen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op te sporen Apache Spark-toepassingen op afstand](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../../storage/common/storage-create-storage-account.md 
