---
title: Azure HDInsight-Tools - Visual Studio-Code gebruiken voor Hive, LLAP of pySpark
description: Informatie over het gebruik van de Azure HDInsight Tools voor Visual Studio Code maken en verzenden van query's en scripts.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
author: jejiang
editor: jasonwhowell jgao
ms.service: HDInsight
ms.topic: conceptual
ms.date: 10/27/2017
ms.author: jejiang
ms.openlocfilehash: 7bf74155cba65d2b5abdc80103a46047aec1b5b7
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39592397"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Azure HDInsight-hulpprogramma's voor Visual Studio Code gebruiken

Informatie over het gebruik van de Azure HDInsight Tools voor Visual Studio Code (VS-Code) maken en verzenden van Hive-batchtaken, interactieve Hive-query's en pySpark-scripts. De hulpprogramma's voor Azure HDInsight kan worden geïnstalleerd op de platforms die worden ondersteund door VS Code. Zoals onder andere Windows, Linux en macOS. Hier vindt u de vereisten voor verschillende platforms.


## <a name="prerequisites"></a>Vereisten

De volgende items zijn vereist voor de stappen in dit artikel:

- Een HDInsight-cluster. Zie voor het maken van een cluster, [aan de slag met HDInsight]( hdinsight-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](http://www.mono-project.com/docs/getting-started/install/). Mono is alleen vereist voor Linux en macOS.

## <a name="install-the-hdinsight-tools"></a>De HDInsight-hulpprogramma's installeren
   
Nadat u de vereiste onderdelen hebt geïnstalleerd, kunt u de Azure HDInsight-hulpprogramma's voor VS Code installeren. 

**Installeer Azure HDInsight-hulpprogramma 's**

1. Open Visual Studio Code.

2. Selecteer in het linkerdeelvenster **extensies**. Voer in het zoekvak **HDInsight**.

3. Naast **hulpprogramma's voor Azure HDInsight**, selecteer **installeren**. Na enkele seconden, de **installeren** knop verandert **opnieuw laden**.

4. Selecteer **opnieuw laden** activeren de **hulpprogramma's voor Azure HDInsight** extensie.

5. Selecteer **Reload Window** om te bevestigen. U kunt zien **hulpprogramma's voor Azure HDInsight** in de **extensies** deelvenster.

   ![HDInsight voor het installeren van Python voor Visual Studio Code](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>HDInsight-werkruimte openen

Voordat u verbinding met Azure maken kunt, moet u een werkruimte maken in VS Code.

**Om een werkruimte te openen**

1. Op de **bestand** in het menu **map openen**. Vervolgens een bestaande map aanwijzen als uw werkmap of een nieuwe maken. De map wordt weergegeven in het linkerdeelvenster.

2. Selecteer in het linkerdeelvenster de **nieuw bestand** pictogram naast de werkmap.

   ![Nieuw bestand](./media/hdinsight-for-vscode/new-file.png)

3. Noem het nieuwe bestand met de .hql (Hive-query's) of de bestandsextensie .py (Spark-script). U ziet dat een **XXXX_hdi_settings.json** configuratiebestand wordt automatisch toegevoegd aan de werkmap.

4. Open **XXXX_hdi_settings.json** van **EXPLORER**, of met de rechtermuisknop op de script-editor om te selecteren **configuratie ingesteld**. U kunt aanmelding vermelding van de standaardcluster en taak indienparameters zoals weergegeven in het voorbeeld in het bestand configureren. Ook kunt u de resterende parameters leeg laten.

## <a name="connect-to-hdinsight-cluster"></a>Verbinding maken met HDInsight-Cluster

Voordat u scripts naar HDInsight-clusters van VS Code verzendt kunt, moet u verbinding maken met uw Azure-account, of koppelen van een cluster (met behulp van Ambari gebruikersnaam en wachtwoord of het domein gekoppeld account).

**Verbinding maken met Azure**

1. Maak een nieuwe werkmap en een nieuw scriptbestand als u ze nog niet hebt.

2. Met de rechtermuisknop op de script-editor, en selecteer vervolgens in het contextmenu **HDInsight: aanmelding**. U kunt ook opgeven **Ctrl + Shift + P**, en voer vervolgens **HDInsight: aanmelding**.

    ![Meld u HDInsight-hulpprogramma's voor Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Als u wilt aanmelden, volg de instructies aanmelden in de **uitvoer** deelvenster.

    **Azure:** ![HDInsight Tools voor Visual Studio Code-aanmelding info](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-Azurelogin-info.png)

    Nadat u verbonden bent, wordt de naam van uw Azure-account wordt weergegeven op de statusbalk linksonder in het venster VS Code. 

    > [!NOTE]
    > Vanwege een verificatieprobleem met bekende Azure-moet u een browser openen in de persoonlijke modus of incognito-modus. Als uw Azure-account twee factoren ingeschakeld heeft, wordt u aangeraden telefonische verificatie gebruiken in plaats van PINCODE-authenticatie.
  

4. Met de rechtermuisknop op de script-editor om te openen van het contextmenu. In het contextmenu, kunt u de volgende taken uitvoeren:

    - Afmelden
    - Clusters groeperen
    - Standaard-clusters instellen
    - Interactieve Hive-query's verzenden
    - Hive-batchscripts verzenden
    - Interactieve PySpark-query's verzenden
    - Verzenden van PySpark-scripts voor batch
    - Set-configuraties

<a id="linkcluster"></a>**Om te koppelen van een cluster**

U kunt een normale cluster koppelen met behulp van Ambari beheerd gebruikersnaam, ook een security hadoop-cluster koppelen met behulp van domeingebruikersnaam (bijvoorbeeld: user1@contoso.com).
1. Open het opdrachtenpalet door te selecteren **CTRL + SHIFT + P**, en voer vervolgens **HDInsight: koppeling van een cluster**.

   ![de opdracht cluster koppelen](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Geef HDInsight cluster-URL-Username -> invoer > invoer wachtwoord -> Selecteer clustertype -> deze ziet geslaagd info als verificatie doorgegeven.
   
   ![dialoogvenster van de cluster koppelen](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]
   > De gekoppelde gebruikersnaam en wachtwoord worden gebruikt als het cluster zowel geregistreerd in Azure-abonnement en een cluster is gekoppeld. 
   
3. Ziet u een gekoppelde cluster met behulp van de opdracht **lijst cluster**. U kunt nu een script voor dit cluster gekoppelde indienen.

   ![gekoppelde cluster](./media/hdinsight-for-vscode/linked-cluster.png)

4. U kunt een cluster ook ontkoppelen door invoeren **HDInsight: ontkoppelen van een cluster** van opdrachtenpalet.

## <a name="list-hdinsight-clusters"></a>Lijst met HDInsight-clusters

Als u wilt testen van de verbinding, kunt u uw HDInsight-clusters vermelden:

**Voor HDInsight-clusters groeperen onder uw Azure-abonnement**
1. Een werkruimte opent, en maak verbinding met Azure. Zie voor meer informatie, [Open HDInsight-werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-azure).

2. Met de rechtermuisknop op de script-editor, en selecteer vervolgens **HDInsight: lijst met Cluster** in het contextmenu. 

3. Het Hive- en Spark-clusters worden weergegeven in de **uitvoer** deelvenster.

    ![Stel een standaardconfiguratie voor cluster](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Een standaardcluster instellen
1. Een werkruimte opent en verbinding maken met Azure. Zie [Open HDInsight-werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-azure).

2. Met de rechtermuisknop op de script-editor, en selecteer vervolgens **HDInsight: standaard Cluster instellen**. 

3. Selecteer een cluster als het standaardcluster voor het huidige scriptbestand. De hulpprogramma's voor het configuratiebestand voor het automatisch bijwerken **XXXX_hdi_settings.json**. 

   ![Standaardconfiguratie voor cluster instellen](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>De Azure-omgeving instellen 
1. Open het opdrachtenpalet door te selecteren **CTRL + SHIFT + P**.

2. Voer **HDInsight: Azure-omgeving instellen**.

3. Selecteer één manier in Azure en AzureChina als de standaardwaarde voor de aanmelding.

4. In de tussentijd zorgen, het hulpprogramma al uw aanmelding standaardvermelding in opgeslagen **XXXX_hdi_settings.json**. U bijwerken ook rechtstreeks in dit configuratie-item. 

   ![Aanmelden instellen-vermelding standaardconfiguratie](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries"></a>Interactieve Hive-query's verzenden

Met HDInsight Tools voor VS Code, kunt u interactieve Hive-query's met HDInsight interactive query-clusters kunt indienen.

1. Maak een nieuwe werkmap en een nieuw Hive-scriptbestand, als u deze nog niet hebt.

2. Maak verbinding met uw Azure-account en configureer vervolgens het standaardcluster, als u dit nog niet hebt gedaan.

3. Kopieer en plak de volgende code in het Hive-bestand en sla het bestand op.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klik met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: Hive Interactive** om de query te verzenden. Met de hulpprogramma’s kunt u ook een codeblok verzenden in plaats van het hele scriptbestand, met behulp van het contextmenu. Kort hierna worden de queryresultaten weergegeven op een nieuw tabblad.

   ![Resultaten van interactieve Hive-query’s](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Paneel **RESULTATEN**: u kunt het hele resultaat op een lokaal pad opslaan als een CSV-, JSON- of Excel-bestand, of gewoon meerdere regels selecteren.

    - Paneel **BERICHTEN**: wanneer u **Regelnummer** selecteert, gaat u naar de eerste regel van het actieve script.

Het uitvoeren van de interactieve query kost veel minder tijd dan het [uitvoeren van een Hive-batchtaak](#submit-hive-batch-scripts).

## <a name="submit-hive-batch-scripts"></a>Hive-batchscripts verzenden

1. Maak een nieuwe werkmap en een nieuw Hive-scriptbestand, als u deze nog niet hebt.

2. Maak verbinding met uw Azure-account en configureer vervolgens het standaardcluster, als u dit nog niet hebt gedaan.

3. Kopieer en plak de volgende code in het Hive-bestand en sla het bestand op.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
3. Klik met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: Hive Batch** om een Hive-taak te verzenden. 

4. Selecteer het cluster waarnaar u de taak wilt verzenden.  

    Nadat u de Hive-taak hebt verzonden, worden de taak-id en informatie over het slagen van de verzending weergegeven in het paneel **UITVOER**. Met de Hive-taak wordt ook de **WEBBROWSER** geopend, waarin de realtimetaaklogboeken en de status worden weergegeven.

   ![Hive-taakresultaat verzenden](./media/hdinsight-for-vscode/submit-Hivejob-result.png)

[Het verzenden van interactieve Hive-query’s](#submit-interactive-hive-queries) gaat veel sneller dan het verzenden van een batchtaak.

## <a name="submit-interactive-pyspark-queries"></a>Interactieve PySpark-query's verzenden
HDInsight-hulpprogramma's voor VS Code kunt u interactieve PySpark-query's op Spark-clusters te verzenden.
1. Maak een nieuwe werkmap en een nieuw scriptbestand met de extensie .py als u deze nog niet hebt.

2. Verbinding maken met uw Azure-account als u dit nog nog niet hebt gedaan.

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
4. Markeer deze scripts. Klik met de rechtermuisknop op de script-editor en selecteer **HDInsight: interactieve PySpark**.

5. Als u dit nog niet hebt geïnstalleerd het **Python** extensie in de VS Code, selecteer de **installeren** knop zoals wordt weergegeven in de volgende afbeelding:

    ![HDInsight voor het installeren van Python voor Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

6. Installeer de Python-omgeving in uw systeem als u dat nog niet gedaan hebt. 
   - Voor Windows, download en installeer [Python](https://www.python.org/downloads/). Controleer vervolgens of `Python` en `pip` zijn in uw systeem pad.

   - Zie voor instructies voor macOS en Linux [interactieve PySpark-omgeving instellen voor Visual Studio Code](set-up-pyspark-interactive-environment.md).

7. Selecteer een cluster waarnaar u wilt de PySpark-query verzendt. Binnenkort daarna wordt het queryresultaat weergegeven in het nieuwe tabblad rechts:

   ![Resultaat van de Python-taak verzenden](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 
8. Het hulpprogramma ondersteunt ook de **SQL-component** query.

   ![Python resultaat taak indienen](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) de verzending van de status wordt weergegeven aan de linkerkant van de status van de onderste balk wanneer u query's uitvoert. Geen andere query's verzenden wanneer de status **PySpark-Kernel (bezet)**. 

>[!NOTE]
>De clusters kunnen sessiegegevens onderhouden. De gedefinieerde variabele, de functie en de bijbehorende waarden worden bewaard in de sessie, zodat ze kunnen worden verwezen door meerdere serviceaanroepen voor hetzelfde cluster. 
 

## <a name="submit-pyspark-batch-job"></a>PySpark batch-taak verzenden

1. Maak een nieuwe werkmap en een nieuw scriptbestand met de extensie .py als u deze nog niet hebt.

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
4. Met de rechtermuisknop op de script-editor, en selecteer vervolgens **HDInsight: PySpark Batch**. 

5. Selecteer een cluster waarop om de PySpark-taak te verzenden. 

   ![Resultaat van de Python-taak verzenden](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nadat u een Python-taak verzenden, verzending van Logboeken worden weergegeven in de **uitvoer** venster in VS Code. De **Spark gebruikersinterface URL** en **Yarn-gebruikersinterface URL** ook worden weergegeven. U kunt de URL openen in een webbrowser om bij te houden van de status van de taak.

>[!NOTE]
>PySpark3 wordt niet meer ondersteund in Livy 0.4 (dit is de HDI spark 2.2-cluster). Alleen 'PySpark' wordt ondersteund voor python. Het is bekend probleem dat met spark 2.2 verzendt met python3 mislukken.
   
## <a name="livy-configuration"></a>Configuratie van Livy
Livy-configuratie wordt ondersteund, deze kan worden ingesteld op de instellingen van het project in de werkmap van de ruimte. Meer informatie, Zie [Livy Leesmij](https://github.com/cloudera/livy/blob/master/README.rst ).

+ De projectinstellingen:

    ![Configuratie van Livy](./media/hdinsight-for-vscode/hdi-livyconfig.png)

+ De ondersteunde configuraties voor Livy:   

    **POST /batches**   
    Aanvraagtekst

    | naam | description | type | 
    | :- | :- | :- | 
    | bestand | Bestand met de toepassing uit te voeren | pad (vereist) | 
    | proxyUser | Gebruiker imiteren bij het uitvoeren van de taak | tekenreeks | 
    | Klassenaam | Toepassing Java/Spark-hoofdklasse | tekenreeks |
    | argumenten | Opdrachtregelargumenten voor de toepassing | lijst met tekenreeksen | 
    | JAR-bestanden | JAR-bestanden moet worden gebruikt in deze sessie | Lijst met tekenreeksen | 
    | pyFiles | Python-bestanden moet worden gebruikt in deze sessie | Lijst met tekenreeksen |
    | bestanden | bestanden in deze sessie moet worden gebruikt | Lijst met tekenreeksen |
    | driverMemory | Hoeveelheid geheugen die moet worden gebruikt voor het stuurprogramma-proces | tekenreeks |
    | driverCores | Aantal kernen moet worden gebruikt voor het stuurprogramma-proces | int |
    | executorMemory | Hoeveelheid geheugen per executorproces gebruiken | tekenreeks |
    | executorCores | Aantal kernen moet worden gebruikt voor elke executor | int |
    | numExecutors | Aantal Executor voor deze sessie starten | int |
    | archiefbestanden | Archief moet worden gebruikt in deze sessie | Lijst met tekenreeksen |
    | wachtrij | De naam van de YARN-wachtrij waarnaar verzonden | tekenreeks |
    | naam | De naam van deze sessie | tekenreeks |
    | conf | Spark-configuratie-eigenschappen | Kaart van sleutel = val |

    Antwoordtekst   
    Het gemaakte object van de Batch.

    | naam | description | type | 
    | :- | :- | :- | 
    | id | De sessie-id | int | 
    | toepassings-id | De toepassings-id van deze sessie |  Reeks |
    | appInfo | De gedetailleerde toepassingsinfo | Kaart van sleutel = val |
    | logboek | De regels van het logboek | lijst met tekenreeksen |
    | state |   De batch-status | tekenreeks |


## <a name="additional-features"></a>Extra functies

HDInsight voor Visual Studio Code ondersteunt de volgende functies:

- **IntelliSense automatisch aanvullen**. Suggesties weergegeven voor het trefwoord, methoden en variabelen. Verschillende pictogrammen zijn de verschillende typen objecten.

    ![HDInsight Tools voor Visual Studio Code IntelliSense objecttypen](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense fout markering**. Language service onderstrepen de bewerken fouten voor de Hive-script.     
- **Syntaxis van de hoogtepunten**. Language service maakt gebruik van verschillende kleuren te onderscheiden van variabelen, trefwoorden, gegevenstype, functies, enzovoort. 

    ![HDInsight Tools voor Visual Studio Code syntaxis gemarkeerd](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Volgende stappen

### <a name="demo"></a>Demo
* HDInsight voor VS Code: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen

* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via VPN-verbinding](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure-Toolkit voor IntelliJ fouten opsporen in Spark-toepassingen op afstand via SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het Spark-toepassingen maken](spark/apache-spark-eclipse-tool-plugin.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Interactieve PySpark-omgeving instellen voor Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](spark/apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](spark/apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](spark/apache-spark-job-debugging.md)



