---
title: Azure HDInsight Tools - Visual Studio Code gebruiken voor Hive, LLAP of pySpark | Microsoft Docs
description: Informatie over het gebruik van Azure HDInsight Tools voor Visual Studio Code voor het maken, indienen van query's en scripts.
Keywords: VScode, Azure HDInsight-hulpprogramma's, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, interactieve Hive, interactieve Query
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
ms.openlocfilehash: 9c1d0e0520df30306c1647cf1f3ec86c8a4fd8f5
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="use-azure-hdinsight-tool-for-visual-studio-code"></a>Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code

Informatie over het gebruik van Azure HDInsight Tools voor Visual Studio Code (VSCode) voor het maken, indienen van Hive-taken voor batch, interactieve Hive-query's en scripts pySpark. Azure HDInsight Tools kan worden geïnstalleerd op de platformen die worden ondersteund door VSCode, met inbegrip van Windows, Linux en Mac OS. U vindt de vereisten voor verschillende platforms.


## <a name="prerequisites"></a>Vereisten

De volgende items zijn vereist voor het voltooien van dit artikel:

- Een HDInsight-cluster.  Zie het maken van een cluster [aan de slag met HDInsight]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono is alleen vereist voor Linux en Mac OS.

## <a name="install-the-hdinsight-tools"></a>De HDInsight-hulpprogramma's installeren
   
Nadat u de vereiste onderdelen hebt geïnstalleerd, kunt u de Azure HDInsight Tools voor VSCode installeren. 

**Installeren Azure HDInsight-hulpprogramma 's**

1. Open **Visual Studio Code**.
2. Klik op **extensies** in het linkerdeelvenster. Voer **HDInsight** in het zoekvak.
3. Klik op **installeren** naast **Azure HDInsight tools**. Na enkele seconden, de **installeren** knop wordt gewijzigd in **opnieuw laden**.
4. Klik op **opnieuw laden** voor het activeren van de **Azure HDInsight tools** extensie.
5. Klik op **opnieuw laden venster** om te bevestigen. U kunt zien **Azure HDInsight tools** in het deelvenster uitbreidingen.

   ![HDInsight Visual Studio Code Python installeren](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>Open de werkruimte HDInsight

U moet een werkruimte in VSCode maken voordat u verbinding met Azure maken kunt.

**Om een werkruimte te openen**

1. Van de **bestand** menu, klikt u op **map openen**, Geef een bestaande map of maak een nieuwe map als uw werkmap. De map wordt weergegeven in het linkerdeelvenster.
2. Klik in het linkerdeelvenster op het **nieuw bestand** pictogram naast de werkmap.

   ![Nieuw bestand](./media/hdinsight-for-vscode/new-file.png)
3. Geef het nieuwe bestand met de .hql (Hive-query's) of de bestandsextensie .py (Spark script). U ziet een **XXXX_hdi_settings.json** configuratiebestand wordt automatisch toegevoegd aan de werkmap.
4. Open **XXXX_hdi_settings.json** van **EXPLORER**, of met de rechtermuisknop op de script-editor om te selecteren **configuratie ingesteld**. U kunt aanmelding vermelding van de standaardcluster en taakparameters verzending, configureren, zoals weergegeven in het voorbeeld in het bestand. Ook kunt u de resterende parameters leeg laten.

## <a name="connect-to-azure"></a>Verbinding maken met Azure

Voordat u scripts naar HDInsight-clusters van VSCode indienen kunt, moet u verbinding met uw Azure-account.

**Verbinding maken met Azure**

1. Maak een nieuwe werkmap en een nieuwe scriptbestand als u niet hebt.
2. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: aanmelding** in het contextmenu. U kunt ook op drukken **CTRL + SHIFT + P** en in te voeren **HDInsight: aanmelding**.

    ![HDInsight Tools voor Visual Studio Code aanmelden](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)
3. Volg de instructies van de aanmelding in de **uitvoer** deelvenster aan te melden.

    **Azure:** ![HDInsight Tools voor Visual Studio Code aanmelding info](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Eenmaal zijn verbonden, wordt de naam van uw Azure-account op de statusbalk onder links van het venster VSCode weergegeven. 

    > [!NOTE]
    > Open de browser met privé-modus of incognito-modus vanwege een probleem met bekende Azure authentication. Als uw Azure-account twee factoren is ingeschakeld heeft, wordt het aanbevolen telefoon om verificatie te gebruiken in plaats van pincode.
  

4. Met de rechtermuisknop op het bewerken van het script voor het openen van het contextmenu. In het contextmenu kunt u de volgende taken uitvoeren:

    - afmelden
    - Lijst met clusters
    - Instellen van de standaardcluster
    - Interactieve Hive-query's verzenden
    - Indienen van Hive-script voor batch
    - Interactieve PySpark-query's verzenden
    - PySpark batchscript verzenden
    - Set-configuratie

## <a name="list-hdinsight-clusters"></a>Lijst met HDInsight-clusters

De om verbinding te testen, kunt u uw HDInsight-clusters weergeven:

**Voor een lijst met HDInsight-clusters onder uw Azure-abonnement**
1. Een werkruimte te openen en verbinding maken met Azure. Zie [Open HDInsight werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-azure).
2. Met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: lijst Cluster** in het contextmenu. 
3. Het Hive- en Spark-clusters worden weergegeven in de **uitvoer** deelvenster.

    ![verzameling standaard-clusterconfiguratie](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Instellen van de standaardcluster
1. Een werkruimte te openen en verbinding maken met Azure. Zie [Open HDInsight werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-azure).
2. Met de rechtermuisknop op de scripteditor en klik vervolgens op **HDInsight: Default Cluster instellen**. 
3. Selecteer een cluster als standaardcluster voor het huidige scriptbestand. De hulpprogramma's voor automatisch bijwerken van het configuratiebestand **XXXX_hdi_settings.json**. 

   ![verzameling standaard-clusterconfiguratie](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-azure-environment"></a>Azure-omgeving instellen 
1. Het palet opdracht openen door te drukken **CTRL + SHIFT + P**.
2. Voer **HDInsight: Azure-omgeving instellen**.
3. Selecteer een van de manieren van Azure en AzureChina als uw aanmelding standaardvermelding.
4. Ondertussen onze tool al opgeslagen wat u hebt geselecteerd aanmelding standaardvermelding in **XXXX_hdi_settings.json**. U bijwerken ook rechtstreeks in dit configuratiebestand. 

   ![aanmelding set-vermelding standaardconfiguratie](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Interactieve Hive-query's verzenden

HDInsight Tools voor VSCode kunt u interactieve Hive-query's om interactieve Query HDInsight-clusters te verzenden.

1. Maak een nieuwe werkmap en een nieuwe Hive-scriptbestand als u niet hebt.
2. Verbinding maken met uw Azure-account en configureer vervolgens het standaardcluster als u dit nog niet hebt gedaan.
3. Kopieer en plak de volgende code in uw Hive-bestand en vervolgens opslaan.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Met de rechtermuisknop op de scripteditor en klik vervolgens op **HDInsight: interactieve Hive** verzenden van de query. De hulpprogramma's kunnen u indienen van een blok van code in plaats van het hele scriptbestand dat u het contextmenu. Snel daarna is wordt het resultaat van de query weergegeven in een nieuw tabblad:

   ![interactieve Hive-resultaat](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - **RESULTATEN** Configuratiescherm: U kunt het hele resultaat opslaan als CSV, JSON, EXCEL naar het lokale pad of selecteer meerdere regels.
    - **BERICHTEN** Configuratiescherm: te klikken op **regel** getal, het gaat u naar de eerste regel van het script wordt uitgevoerd.

Vergelijken met het [met een Hive-batchtaak](#submit-hive-batch-scripts), de interactieve query veel minder tijd in beslag neemt.

## <a name="submit-hive-batch-scripts"></a>Indienen van Hive batchscripts

1. Maak een nieuwe werkmap en een nieuwe Hive-scriptbestand als u niet hebt.
2. Verbinding maken met uw Azure-account en configureer vervolgens het standaardcluster als u dit nog niet hebt gedaan.
3. Kopieer en plak de volgende code in uw Hive-bestand en vervolgens opslaan.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Met de rechtermuisknop op de scripteditor en klik vervolgens op **HDInsight: Batch Hive** verzenden van een Hive-taak. 
4. Selecteer een cluster waarop u verzenden wilt naar.  

    Na het verzenden van een Hive-taak, de verzending van geslaagd info en jobid wordt weergegeven in **uitvoer** Configuratiescherm. En er wordt geopend **WEBBROWSER** waarin de taak realtime logboeken en status weergegeven in.

   ![resultaat van Hive-taak verzenden](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

Vergelijken met het [interactieve Hive-query's verzenden](#submit-interactive-hive-queries), de batchverwerking veel langer duurt.

## <a name="submit-interactive-pyspark-queries"></a>Interactieve PySpark-query's verzenden
HDInsight Tools voor VSCode kunt u indienen interactieve PySpark-query's met Spark-clusters.
1. Maak een nieuwe werkmap en een nieuw scriptbestand met de extensie .py als u niet hebt.
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
4. Markeer deze scripts en klikt u op met de rechtermuisknop op de scripteditor **HDInsight: PySpark interactieve**.
5. Klik op de volgende **installeren** knop als u niet hebt geïnstalleerd **Python** de extensie in de VSCode.
    ![HDInsight Visual Studio Code Python installeren](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Instellen van python-omgeving in uw systeem als u deze niet installeren. 
   - Voor windows, downloaden en installeren [Python](https://www.python.org/downloads/). Controleer vervolgens of `Python` en `pip` in uw systeem pad.
   - De instructie voor Mac OS- en Linux, Zie [ingesteld Up PySpark interactieve omgeving voor Visual Studio Code](set-up-pyspark-interactive-environment.md).
7. Selecteer een cluster om uw query PySpark verzenden. Snel daarna is wordt het queryresultaat weergegeven in het tabblad rechts nieuw:

   ![resultaat van python-taak verzenden](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Onze hulpprogramma ondersteunt ook query de **SQL-component**.

   ![indienen python taak resultaat](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) de status van de verzending van aan de linkerkant van de balk onderaan status wordt weergegeven bij het uitvoeren van query's. U kunt andere query's niet verzenden als de status **PySpark-Kernel (bezet)**, anders de uitgevoerd is vastgelopen.
9. Onze clusters kunnen handhaven een sessie. Bijvoorbeeld: **een = 100**, deze sessie al in cluster behouden, nu u alleen uitvoeren **afdrukken een** aan cluster.
 

## <a name="submit-pyspark-batch-job"></a>Batchverwerking PySpark verzenden

1. Maak een nieuwe werkmap en een nieuw scriptbestand met de extensie .py als u niet hebt.
2. Verbinding maken met uw Azure-account als u dit nog niet hebt gedaan.
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
4. Met de rechtermuisknop op de scripteditor en klik vervolgens op **HDInsight: PySpark Batch**. 
5. Selecteer een cluster om het verzenden van de PySpark-taak. 

   ![resultaat van python-taak verzenden](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Na het verzenden van een taak python verzending van Logboeken wordt weergegeven in **uitvoer** venster in VSCode. De **Spark UI URL** en **Yarn UI URL** ook worden weergegeven. U kunt de URL in een webbrowser om bij te houden van de status van de taak openen.


## <a name="additional-features"></a>Aanvullende functies

Het HDInsight voor VSCode ondersteunt de volgende functies:

- **IntelliSense automatisch aanvullen**. Suggesties zijn verschijnt rond sleutelwoord, methode, variabelen, enzovoort. Verschillende pictogrammen verschillende typen objecten:

    ![HDInsight Tools voor Visual Studio Code IntelliSense objecttypen](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense fout markering**. De taalservice onderstrepen de bewerking fouten voor Hive-script.     
- **Syntaxis licht**. De taalservice gebruikt een andere kleur te onderscheiden van variabelen, trefwoorden, gegevenstype, functies, enzovoort. 

    ![HDInsight Tools voor Visual Studio Code syntaxis highlights](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Volgende stappen

### <a name="demo"></a>Demo
* HDInsight voor VScode: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen

* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark scala-toepassingen op afstand via VPN-verbinding](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-Toolkit voor IntelliJ gebruiken om op te sporen Spark-toepassingen op afstand via SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Gebruik van HDInsight Tools voor IntelliJ met Hortonworks Sandbox](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [Gebruik van HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse Spark-toepassingen maken](spark/apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [PySpark interactieve omgeving instellen voor Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](spark/apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Spark in HDInsight voor het bouwen van realtime streamingtoepassingen gebruiken](spark/apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a>Maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](spark/apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="managing-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](spark/apache-spark-job-debugging.md)



