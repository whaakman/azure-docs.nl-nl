---
title: Azure HDInsight Tools - Visual Studio Code gebruiken voor Hive, LLAP of pySpark | Microsoft Docs
description: Informatie over het gebruik van Azure HDInsight Tools voor Visual Studio Code maken en verzenden van query's en scripts.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: 
author: jejiang
manager: 
editor: jgao
tags: azure-portal
ms.assetid: 
ms.service: HDInsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 89e83dc02f32f6f2a781cf2e35040b29cc3d3c06
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Gebruik van Azure HDInsight Tools voor Visual Studio Code

Informatie over het gebruik van Azure HDInsight Tools voor Visual Studio Code (VS-Code) maken en Hive-taken voor batch, interactieve Hive-query's en scripts pySpark verzenden. Azure HDInsight Tools kan worden geïnstalleerd op de platforms die worden ondersteund door VS-Code. Het gaat hierbij om Windows, Linux en Mac OS. U vindt de vereisten voor verschillende platforms.


## <a name="prerequisites"></a>Vereisten

De volgende items zijn vereist voor de stappen in dit artikel:

- Een HDInsight-cluster.  Zie het maken van een cluster [aan de slag met HDInsight]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono is alleen vereist voor Linux en Mac OS.

## <a name="install-the-hdinsight-tools"></a>De HDInsight-hulpprogramma's installeren
   
Nadat u de vereiste onderdelen hebt geïnstalleerd, kunt u de Azure HDInsight Tools voor VS-Code installeren. 

**Installeren Azure HDInsight-hulpprogramma 's**

1. Open Visual Studio Code.

2. Selecteer in het linkerdeelvenster **extensies**. Voer in het zoekvak **HDInsight**.

3. Naast **Azure HDInsight tools**, selecteer **installeren**. Na enkele seconden, de **installeren** knop verandert **opnieuw laden**.

4. Selecteer **opnieuw laden** voor het activeren van de **Azure HDInsight tools** extensie.

5. Selecteer **opnieuw laden venster** om te bevestigen. U kunt zien **Azure HDInsight tools** in de **extensies** deelvenster.

   ![HDInsight Visual Studio Code Python installeren](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Open de werkruimte HDInsight

Een werkruimte in VS-Code maken voordat u verbinding met Azure maken kunt.

**Om een werkruimte te openen**

1. Op de **bestand** selecteert u **map openen**. Vervolgens een bestaande map aanwijzen als de map van uw werk of een nieuwe maken. De map wordt weergegeven in het linkerdeelvenster.

2. Selecteer in het linkerdeelvenster de **nieuw bestand** pictogram naast de werkmap.

   ![Nieuw bestand](./media/hdinsight-for-vscode/new-file.png)

3. Geef het nieuwe bestand met de .hql (Hive-query's) of de bestandsextensie .py (Spark script). U ziet dat een **XXXX_hdi_settings.json** configuratiebestand wordt automatisch toegevoegd aan de werkmap.

4. Open **XXXX_hdi_settings.json** van **EXPLORER**, of met de rechtermuisknop op de script-editor om te selecteren **configuratie ingesteld**. U kunt configureren aanmelding vermelding van de standaardcluster en taak verzending parameters zoals weergegeven in het voorbeeld in het bestand. Ook kunt u de resterende parameters leeg laten.

## <a name="connect-to-azure"></a>Verbinding maken met Azure

Voordat u scripts naar HDInsight-clusters van de Code van de VS indienen kunt, moet u verbinding met uw Azure-account.

**Verbinding maken met Azure**

1. Een nieuwe werkmap en een nieuwe scriptbestand maken als u ze nog niet hebt.

2. Met de rechtermuisknop op de scripteditor, en selecteer vervolgens in het contextmenu **HDInsight: aanmelding**. U kunt ook opgeven **Ctrl + Shift + P**, en voer vervolgens **HDInsight: aanmelding**.

    ![HDInsight Tools voor Visual Studio Code aanmelden](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Als u wilt aanmelden, volg de instructies aanmelden in de **uitvoer** deelvenster.

    **Azure:** ![HDInsight Tools voor Visual Studio Code aanmelding info](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Nadat u verbonden bent, wordt de naam van uw Azure-account wordt weergegeven op de statusbalk in de linkerbenedenhoek van het venster VS-Code. 

    > [!NOTE]
    > Vanwege een probleem met bekende Azure authentication moet u een browser openen in de privé-modus of incognito-modus. Als uw Azure-account twee factoren is ingeschakeld heeft, kunt u het beste met verificatie van de telefoon in plaats van PINCODE-authenticatie.
  

4. Met de rechtermuisknop op de script-editor om te openen van het contextmenu. In het contextmenu kunt u de volgende taken uitvoeren:

    - Afmelden
    - Lijst met clusters
    - Verzameling standaard-clusters
    - Interactieve Hive-query's verzenden
    - Indienen van Hive batchscripts
    - Interactieve PySpark-query's verzenden
    - PySpark batchscripts te verzenden
    - Set-configuraties

## <a name="list-hdinsight-clusters"></a>Lijst met HDInsight-clusters

De om verbinding te testen, kunt u uw HDInsight-clusters weergeven:

**Voor een lijst met HDInsight-clusters onder uw Azure-abonnement**
1. Een werkruimte te openen en vervolgens verbinding maken met Azure. Zie voor meer informatie [Open HDInsight werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-azure).

2. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: lijst Cluster** in het contextmenu. 

3. Het Hive- en Spark-clusters worden weergegeven in de **uitvoer** deelvenster.

    ![Stel een standaardconfiguratie van het cluster](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Een standaardcluster instellen
1. Een werkruimte te openen en verbinding maken met Azure. Zie [Open HDInsight werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-azure).

2. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: Default Cluster instellen**. 

3. Selecteer een cluster als het standaardcluster voor het huidige scriptbestand. De hulpprogramma's voor automatisch bijwerken van het configuratiebestand **XXXX_hdi_settings.json**. 

   ![verzameling standaard-clusterconfiguratie](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>De Azure-omgeving instellen 
1. Het palet opdracht openen door te selecteren **CTRL + SHIFT + P**.

2. Voer **HDInsight: Azure-omgeving instellen**.

3. Selecteer een van de manieren van Azure en AzureChina als uw aanmelding standaardvermelding.

4. Ondertussen het hulpprogramma al uw aanmelding standaardvermelding in opgeslagen **XXXX_hdi_settings.json**. U bijwerken ook rechtstreeks in dit configuratiebestand. 

   ![aanmelding set-vermelding standaardconfiguratie](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Interactieve Hive-query's verzenden

Met HDInsight Tools voor VS-Code, kunt u interactieve Hive-query's met HDInsight-clusters voor interactieve query indienen.

1. Maak een nieuwe werkmap en een nieuwe Hive-scriptbestand als u ze nog geen hebt.

2. Verbinding maken met uw Azure-account en configureer vervolgens het standaardcluster als u dat nog niet hebt gedaan.

3. Kopieer en plak de volgende code in uw Hive-bestand vervolgens opslaan.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: interactieve Hive** verzenden van de query. De hulpprogramma's kunnen u indienen van een blok van code in plaats van het hele scriptbestand dat u het contextmenu. Kort nadat de queryresultaten in een nieuw tabblad weergegeven.

   ![interactieve Hive-resultaat](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **RESULTATEN** Configuratiescherm: U kunt het hele resultaat opslaan als CSV, JSON of Excel-bestand naar een lokaal pad of selecteer meerdere regels.

    - **BERICHTEN** Configuratiescherm: wanneer u selecteert **regel** getal, het gaat u naar de eerste regel van het script wordt uitgevoerd.

Uitvoeren van de interactieve query kost veel minder tijd dan [met een Hive-batchtaak](#submit-hive-batch-scripts).

## <a name="submit-hive-batch-scripts"></a>Indienen van Hive batchscripts

1. Maak een nieuwe werkmap en een nieuwe Hive-scriptbestand als u ze nog geen hebt.

2. Verbinding maken met uw Azure-account en configureer vervolgens het standaardcluster als u dat nog niet hebt gedaan.

3. Kopieer en plak de volgende code in uw Hive-bestand vervolgens opslaan.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: Batch Hive** verzenden van een Hive-taak. 

4. Selecteer het cluster waarnaar u wilt verzenden.  

    Nadat u een Hive-taak hebt ingediend, de verzending van geslaagd info en jobid wordt weergegeven de **uitvoer** Configuratiescherm. Het Hive-taak ook geopend **WEBBROWSER**, waarin de real-time taaklogboeken en status.

   ![resultaat van Hive-taak verzenden](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

[Interactieve Hive-query's verzenden](#submit-interactive-hive-queries) kost veel minder tijd dan het verzenden van een batch-job.

## <a name="submit-interactive-pyspark-queries"></a>Interactieve PySpark-query's verzenden
HDInsight Tools voor VS-Code kunt u indienen interactieve PySpark-query's met Spark-clusters.
1. Maakt een nieuwe werkmap en een nieuw scriptbestand met de extensie .py als u ze nog niet hebt.

2. Verbinding maken met uw Azure-account als u dit nog niet hebt gedaan.

3. Kopieer en plak de volgende code in het scriptbestand:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```
4. Markeer deze scripts. Klik met de rechtermuisknop op de scripteditor en selecteer **HDInsight: PySpark interactieve**.

5. Als u nog niet hebt geïnstalleerd het **Python** de extensie in de Code van de VS, selecteer de **installeren** knop zoals weergegeven in de volgende afbeelding:

    ![HDInsight Visual Studio Code Python installeren](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Als u dat nog niet gedaan hebt, moet u de Python-omgeving installeren in uw systeem. 
   - Voor Windows, downloaden en installeren [Python](https://www.python.org/downloads/). Controleer vervolgens of `Python` en `pip` zijn in uw systeem pad.

   - Zie voor instructies voor Mac OS- en Linux [PySpark interactieve omgeving instellen voor Visual Studio Code](set-up-pyspark-interactive-environment.md).

7. Selecteer een cluster waarop u wilt uw query PySpark verzenden. Snel daarna is wordt het resultaat van de query weergegeven in het nieuwe tabblad rechts:

   ![Resultaat van Python-taak verzenden](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Het hulpprogramma ondersteunt ook de **SQL-component** query.

   ![Indienen Python taak resultaat](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) de verzending van status wordt weergegeven aan de linkerkant van de onderkant statusbalk wanneer u query's uitvoert. Geen andere query's verzenden wanneer de status **PySpark-Kernel (bezet)**. 

>[!NOTE]
>De clusters kunnen sessiegegevens onderhouden. De gedefinieerde variabele, de functie en de bijbehorende waarden worden opgeslagen in de sessie, zodat ze via meerdere service-aanroepen voor hetzelfde cluster kunnen worden verwezen. 
 

## <a name="submit-pyspark-batch-job"></a>Batchverwerking PySpark verzenden

1. Maakt een nieuwe werkmap en een nieuw scriptbestand met de extensie .py als u ze nog niet hebt.

2. Verbinding maken met uw Azure-account als u dat nog niet hebt gedaan.

3. Kopieer en plak de volgende code in het scriptbestand:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```
4. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: PySpark Batch**. 

5. Selecteer een cluster waarop u wilt verzenden van de PySpark-taak. 

   ![Resultaat van Python-taak verzenden](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nadat u een Python-taak hebt ingediend, verzending van Logboeken worden weergegeven in de **uitvoer** venster in VS-Code. De **Spark UI URL** en **Yarn UI URL** ook worden weergegeven. U kunt de URL in een webbrowser om bij te houden van de status van de taak openen.


## <a name="additional-features"></a>Aanvullende functies

HDInsight voor VS Code ondersteunt de volgende functies:

- **IntelliSense automatisch aanvullen**. Suggesties weergegeven voor het trefwoord, methoden en variabelen. Verschillende typen objecten de verschillende pictogrammen.

    ![HDInsight Tools voor Visual Studio Code IntelliSense objecttypen](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense fout markering**. De taalservice onderstrepen de bewerking fouten voor het Hive-script.     
- **Syntaxis licht**. De taalservice gebruikt verschillende kleuren te onderscheiden van variabelen, trefwoorden, gegevenstype, functies, enzovoort. 

    ![HDInsight Tools voor Visual Studio Code syntaxis highlights](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Volgende stappen

### <a name="demo"></a>Demo
* HDInsight voor VS-Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen

* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark-toepassingen op afstand via SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](spark/apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Interactieve Hive-Query-gegevens visualiseren met Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [PySpark interactieve omgeving instellen voor Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](spark/apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Spark in HDInsight voor het bouwen van realtime streamingtoepassingen gebruiken](spark/apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](spark/apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](spark/apache-spark-job-debugging.md)



