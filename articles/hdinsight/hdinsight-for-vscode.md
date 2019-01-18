---
title: Azure HDInsight-Tools - Visual Studio Code gebruiken voor Hive, LLAP of PySpark | Microsoft Docs
description: Informatie over het gebruik van de Azure HDInsight Tools voor Visual Studio Code maken en verzenden van query's en scripts.
Keywords: VS Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/15/2018
ms.openlocfilehash: cd9f2df7987f033b4d4c234109cf712b8291d1b7
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382849"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Azure HDInsight-hulpprogramma's voor Visual Studio Code gebruiken

Informatie over het gebruik van de Azure HDInsight Tools voor Visual Studio Code (VS-Code) maken en verzenden van Apache Hive-batchtaken, interactieve Hive-query's en PySpark-scripts voor Apache Spark. Eerst wordt wordt beschreven hoe u de HDInsight-hulpprogramma's in VS Code installeren en vervolgens behandelen we hoe u Hive-en Spark-taken verzenden. 

De hulpprogramma's voor Azure HDInsight kan worden geïnstalleerd op de platforms die worden ondersteund door Visual Studio Code, waaronder Windows, Linux en macOS. Hieronder vindt u de vereisten voor verschillende platforms.


## <a name="prerequisites"></a>Vereisten

De volgende items zijn vereist voor de stappen in dit artikel:

- Een HDInsight-cluster. Zie voor het maken van een cluster, [aan de slag met HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).
- [Visual Studio Code](https://www.visualstudio.com/products/code-vs.aspx).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono is alleen vereist voor Linux en macOS.

## <a name="install-the-hdinsight-tools"></a>De HDInsight-hulpprogramma's installeren
   
Nadat u de vereiste onderdelen hebt geïnstalleerd, kunt u de Azure HDInsight-hulpprogramma's voor VS Code installeren. 

### <a name="to-install-azure-hdinsight-tools"></a>Azure HDInsight-hulpprogramma's installeren

1. Open Visual Studio Code.

2. Selecteer in het linkerdeelvenster **extensies**. Voer in het zoekvak **HDInsight**.

3. Naast **Azure HDInsight-hulpprogramma's**, selecteer **installeren**. Na enkele seconden, de **installeren** knop verandert **opnieuw laden**.

4. Selecteer **opnieuw laden** activeren de **Azure HDInsight-hulpprogramma's** extensie.

5. Selecteer **Reload Window** om te bevestigen. U kunt zien **Azure HDInsight-hulpprogramma's** in de **extensies** deelvenster.

   ![HDInsight voor het installeren van Python voor Visual Studio Code](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

## <a name="open-hdinsight-workspace"></a>HDInsight-werkruimte openen

Voordat u verbinding met Azure maken kunt, moet u een werkruimte maken in VS Code.

### <a name="to-open-a-workspace"></a>Om een werkruimte te openen

1. Op de **bestand** in het menu **map openen**. Vervolgens een bestaande map aanwijzen als uw werkmap of een nieuwe maken. De map wordt weergegeven in het linkerdeelvenster.

2. Selecteer in het linkerdeelvenster de **nieuw bestand** pictogram naast de werkmap.

   ![Nieuw bestand](./media/hdinsight-for-vscode/new-file.png)

3. Noem het nieuwe bestand met de .hql (Hive-query's) of de bestandsextensie .py (Spark-script). 

## <a name="connect-to-hdinsight-cluster"></a>Verbinding maken met HDInsight-Cluster

Voordat u scripts naar HDInsight-clusters van VS Code verzendt kunt, moet u verbinding maken met uw Azure-account, of koppelen van een cluster (met behulp van Ambari gebruikersnaam en wachtwoord of het domein gekoppeld account).

### <a name="to-connect-to-azure"></a>Verbinding maken met Azure

1. Maak een nieuwe werkmap en een nieuw scriptbestand als u ze nog niet hebt.

2. Met de rechtermuisknop op de script-editor, en selecteer vervolgens in het contextmenu **HDInsight: Aanmelding**. U kunt ook opgeven **Ctrl + Shift + P**, en voer vervolgens **HDInsight: Aanmelding**.

    ![HDInsight Tools voor Visual Studio Code-aanmelding](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

3. Als u wilt aanmelden, volg de instructies aanmelden in de **uitvoer** deelvenster.
    + Voor globale omgeving HDInsight sign in Azure wordt geactiveerd proces aanmelden.

        ![Meld u instructies voor azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Volg de instructies aanmelden voor andere omgevingen.

        ![Meld u instructies voor de andere omgeving](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

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
    - Configuratie van de

<h3 id="linkcluster">Om te koppelen van een cluster</h3>

U kunt een normale cluster koppelen met behulp van een [Apache Ambari](https://ambari.apache.org/) beheerd gebruikersnaam of het koppelen van een onderneming beveiliging Pack beveiligde Hadoop-cluster met behulp van een domein-gebruikersnaam (bijvoorbeeld: user1@contoso.com).
1. Open het opdrachtenpalet door te selecteren **CTRL + SHIFT + P**, en voer vervolgens **HDInsight: Koppeling van een Cluster**.

   ![de opdracht cluster koppelen](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Geef HDInsight cluster-URL-Username -> invoer > invoer wachtwoord -> Selecteer clustertype -> deze ziet geslaagd info als verificatie doorgegeven.
   
   ![dialoogvenster van de cluster koppelen](./media/hdinsight-for-vscode/link-cluster-process.png)

   > [!NOTE]  
   > De gekoppelde gebruikersnaam en wachtwoord worden gebruikt als het cluster zowel geregistreerd in Azure-abonnement en een cluster is gekoppeld. 
   
3. Ziet u een gekoppelde cluster met behulp van de opdracht **lijst Cluster**. U kunt nu een script voor dit cluster gekoppelde indienen.

   ![gekoppelde cluster](./media/hdinsight-for-vscode/linked-cluster.png)

4. U kunt een cluster ook ontkoppelen door invoeren **HDInsight: Ontkoppelen van een Cluster** van opdrachtenpalet.


### <a name="to-link-a-generic-apache-livy-endpoint"></a>Om te koppelen van een algemene Apache Livy-eindpunt

1. Open het opdrachtenpalet door te selecteren **CTRL + SHIFT + P**, en voer vervolgens **HDInsight: Koppeling van een Cluster**.
2. Selecteer **algemene Livy eindpunt**.
3. Voer het algemene Livy-eindpunt, bijvoorbeeld: http\:/ / 10.172.41.42:18080.
4. Selecteer **Basic** wanneer nodig autorisatie voor de algemene Livy eindpunt anders selecteert **geen**.
5. Invoer gebruikersnaam als select **Basic** in step4.
6. Invoer wachtwoord wanneer Selecteer **Basic** in step4.
7. De algemene livy-eindpunt is gekoppeld.

   ![gekoppelde algemeen livy-cluster](./media/hdinsight-for-vscode/link-cluster-process-generic-livy.png)

## <a name="list-hdinsight-clusters"></a>Lijst met HDInsight-clusters

Als u wilt testen van de verbinding, kunt u uw HDInsight-clusters vermelden:

### <a name="to-list-hdinsight-clusters-under-your-azure-subscription"></a>Voor HDInsight-clusters groeperen onder uw Azure-abonnement
1. Een werkruimte opent, en maak verbinding met Azure. Zie voor meer informatie, [Open HDInsight-werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-hdinsight-cluster).

2. Klik met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: Overzicht van Cluster** in het contextmenu. 

3. De HDInsight-clusters worden weergegeven in de **uitvoer** deelvenster.

    ![Stel een standaardconfiguratie voor cluster](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-a-default-cluster"></a>Een standaardcluster instellen
1. Een werkruimte opent en verbinding maken met Azure. Zie [Open HDInsight-werkruimte](#open-hdinsight-workspace) en [verbinding maken met Azure](#connect-to-hdinsight-cluster).

2. Klik met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: Instellen van de standaardcluster**. 

3. Selecteer een cluster als het standaardcluster voor het huidige scriptbestand. De hulpprogramma's voor het configuratiebestand voor het automatisch bijwerken **. VSCode\settings.json**. 

   ![Standaardconfiguratie voor cluster instellen](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>De Azure-omgeving instellen
1. Open het opdrachtenpalet door te selecteren **CTRL + SHIFT + P**.

2. Voer **HDInsight: Azure-omgeving instellen**.

3. Selecteer een omgeving, zoals 'Azure' of 'AzureChina' als de standaardwaarde voor de aanmelding.

4. In de tussentijd zorgen, het hulpprogramma al uw aanmelding standaardvermelding in opgeslagen **. VSCode\settings.json**. U bijwerken ook rechtstreeks in dit configuratie-item. 

   ![Aanmelden instellen-vermelding standaardconfiguratie](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Indienen van interactieve Hive-query's, Hive batchscripts

Met HDInsight Tools voor VS Code, kunt u interactieve Hive-query's, Hive batchscripts naar HDInsight-clusters kunt indienen.

1. Maak een nieuwe werkmap en een nieuw Hive-scriptbestand, als u deze nog niet hebt.

2. Verbinding maken met uw Azure-account of een koppeling-clusters.

3. Kopieer en plak de volgende code in het Hive-bestand en sla het bestand op.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```
4. Met de rechtermuisknop op de script-editor, selecteert u **HDInsight: Interactive hive** om de query verzenden, of gebruik snelkoppeling **Ctrl + Alt + I**. Selecteer **HDInsight: Hive-Batch** verzend het script of er snelkoppeling **Ctrl + Alt + H**. 

5. Als u een standaardcluster hebt opgegeven, selecteert u het cluster. Met de hulpprogramma’s kunt u ook een codeblok verzenden in plaats van het hele scriptbestand, met behulp van het contextmenu. Na enkele ogenblikken worden de queryresultaten weergegeven in een nieuw tabblad.

   ![Resultaten van interactieve Hive-query’s](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Deelvenster **RESULTATEN**: U kunt het hele resultaat op een lokaal pad opslaan als een CSV-, JSON- of Excel-bestand, of gewoon meerdere regels selecteren.

    - Deelvenster **BERICHTEN**: Wanneer u **Regelnummer** selecteert, gaat u naar de eerste regel van het actieve script.

## <a name="submit-interactive-pyspark-queries"></a>Interactieve PySpark-query's verzenden

### <a name="to-submit-interactive-pyspark-queries-to-hdinsight-spark-clusters"></a>Om in te dienen interactieve PySpark-query's voor HDInsight Spark-clusters.

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
4. Markeer dit script. Klik met de rechtermuisknop op de script-editor en selecteer **HDInsight: Interactieve PySpark**, of gebruikt u snelkoppeling **Ctrl + Alt + I**.

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

### <a name="to-disable-environment-check"></a>Aan de omgeving controle uitschakelen

Standaard zijn de hulpprogramma's voor HDInsight wordt omgeving controleren en de afhankelijke pakketten installeren wanneer de interactieve PySpark-query's verzenden. Als u wilt de controle van de omgeving uitschakelen, stelt de **hdinsight.disablePysparkEnvironmentValidation** naar **Ja** onder **GEBRUIKERSINSTELLINGEN**.

   ![Instellen van de controle van de omgeving van instellingen](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check.png)

U kunt ook op **uitschakelen validatie** wanneer het dialoogvenster wordt weergegeven.

   ![De controle van de omgeving van dialoogvenster instellen](./media/hdinsight-for-vscode/hdi-azure-hdinsight-environment-check-dialog.png)

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 wordt niet ondersteund met Spark2.2/2.3

PySpark3 wordt niet meer ondersteund met Spark 2.2 en Spark2.3 cluster, alleen 'PySpark' wordt ondersteund voor Python. Het is bekend probleem dat wordt verzonden met spark 2.2/2.3 mislukken met Python3.

   ![Bij fout bij het ophalen van de python3 indienen](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Volg de stappen voor het gebruik van Python2.x: 

1. Python 2.7 op de lokale computer installeren en toe te voegen aan het systeempad staat.

2. Opnieuw opstarten VSCode.

3. Schakel over naar de Python 2 door te klikken op de **Python XXX** op de status van de balk in en kies vervolgens het Python-doel.

   ![Selecteer python-versie](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)

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
4. Klik met de rechtermuisknop op de scripteditor en selecteer vervolgens **HDInsight: PySpark Batch**, of gebruikt u snelkoppeling **Ctrl + Alt + H**. 

5. Selecteer een cluster waarop om de PySpark-taak te verzenden. 

   ![Resultaat van de Python-taak verzenden](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nadat u een Python-taak verzenden, verzending van Logboeken worden weergegeven in de **uitvoer** venster in VS Code. De **Spark gebruikersinterface URL** en **Yarn-gebruikersinterface URL** ook worden weergegeven. U kunt de URL openen in een webbrowser om bij te houden van de status van de taak.

## <a name="apache-livy-configuration"></a>Configuratie van Apache Livy

[Apache Livy](https://livy.incubator.apache.org/) configuratie wordt ondersteund, kan worden ingesteld op de **. VSCode\settings.json** in werkmappen ruimte. Configuratie van livy ondersteunt momenteel alleen Python-script. Meer informatie, Zie [Livy Leesmij](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Het activeren van de configuratie van livy**
   
U kunt vinden op **bestand** in het menu **voorkeuren**, en kies **instellingen** op in het contextmenu. Klik op **WERKRUIMTE instellingen** tabblad kunt u beginnen met livy configuratie instellen.

U kunt ook een bestand verzenden, wordt u ziet dat de map .vscode automatisch toegevoegd aan de werkmap. U kunt de configuratie van de livy vinden door te klikken op **.vscode\settings.json**.

+ De projectinstellingen:

    ![Configuratie van Livy](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Voor instellingen **driverMomory** en **executorMomry**, stelt u de waarde met de eenheid op, bijvoorbeeld 1 g of 1024 m. 

+ De ondersteunde configuraties voor Livy:   

    **POST /batches**   
    Aanvraagtekst

    | naam | description | type | 
    | :- | :- | :- | 
    | bestand | Bestand met de toepassing uit te voeren | pad (vereist) | 
    | proxyUser | Gebruiker imiteren bij het uitvoeren van de taak | string | 
    | className | Toepassing Java/Spark-hoofdklasse | string |
    | argumenten | Opdrachtregelargumenten voor de toepassing | lijst met tekenreeksen | 
    | JAR-bestanden | JAR-bestanden moet worden gebruikt in deze sessie | Lijst met tekenreeksen | 
    | pyFiles | Python-bestanden moet worden gebruikt in deze sessie | Lijst met tekenreeksen |
    | bestanden weergeven | bestanden in deze sessie moet worden gebruikt | Lijst met tekenreeksen |
    | driverMemory | Hoeveelheid geheugen die moet worden gebruikt voor het stuurprogramma-proces | string |
    | driverCores | Aantal kernen moet worden gebruikt voor het stuurprogramma-proces | int |
    | executorMemory | Hoeveelheid geheugen per executorproces gebruiken | string |
    | executorCores | Aantal kernen moet worden gebruikt voor elke executor | int |
    | numExecutors | Aantal Executor voor deze sessie starten | int |
    | archiefbestanden | Archief moet worden gebruikt in deze sessie | Lijst met tekenreeksen |
    | wachtrij | De naam van de YARN-wachtrij waarnaar verzonden | string |
    | naam | De naam van deze sessie | string |
    | conf | Spark-configuratie-eigenschappen | Kaart van sleutel = val |

    Hoofdtekst van de reactie   
    Het gemaakte object van de Batch.

    | naam | description | type | 
    | :- | :- | :- | 
    | id | De sessie-id | int | 
    | appId | De toepassings-id van deze sessie |  Reeks |
    | appInfo | De gedetailleerde toepassingsinfo | Kaart van sleutel = val |
    | log | De regels van het logboek | lijst met tekenreeksen |
    | state |   De batch-status | string |

>[!NOTE]
>De configuratie van de toegewezen livy in het deelvenster Uitvoer wordt weergegeven wanneer het script indienen.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integreren met Azure HDInsight in Explorer

Azure HDInsight is toegevoegd aan het linkerdeelvenster. U kunt bladeren en het cluster rechtstreeks beheren.

1. Vouw de **AZURE HDINSIGHT**als niet-teken, wordt deze weergegeven **aanmelden bij Azure...**  koppeling.

    ![Meld u aan de installatiekopie van de koppeling](./media/hdinsight-for-vscode/hid-azure-hdinsight-sign-in.png)

2. Klik op **aanmelden bij Azure**, deze koppeling en code verschijnt rechts onderaan.

    ![Meld u instructies voor de andere omgeving](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin-code.png)

3. Klik op **kopiëren & openen** knop browser wordt geopend plak de code, klikt u op **doorgaan** knop, ziet u de aanwijzing over het aanmelden is.

4. Nadat u bent aangemeld, worden de beschikbare abonnementen en -clusters (Spark, Hadoop en HBase worden ondersteund) weergegeven in **AZURE HDINSIGHT**. 

   ![Abonnement op Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

5. Vouw het cluster om hive-database en tabel metagegevensschema weer te geven.

   ![Azure HDInsight-cluster](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)

## <a name="additional-features"></a>Aanvullende functies

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

* [Azure Toolkit voor IntelliJ voor foutopsporing van Apache Spark-toepassingen op afstand via VPN-verbinding gebruiken](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Azure Toolkit voor IntelliJ gebruiken voor foutopsporing van Apache Spark-toepassingen op afstand via SSH](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [HDInsight-hulpprogramma's voor IntelliJ met Hortonworks Sandbox gebruiken](hadoop/hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [HDInsight-hulpprogramma's in Azure Toolkit voor Eclipse gebruiken voor het maken van Apache Spark-toepassingen](spark/apache-spark-eclipse-tool-plugin.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Apache Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Interactieve PySpark-omgeving instellen voor Visual Studio Code](set-up-pyspark-interactive-environment.md)
* [Apache Zeppelin Apache Hive-query's uitvoeren in Azure HDInsight gebruiken ](./hdinsight-connect-hive-zeppelin.md)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met BI: Interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](spark/apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](spark/apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](spark/apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-running-applications"></a>Maken en uitvoeren van toepassingen
* [Een zelfstandige toepassing maken met behulp van Scala](spark/apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](spark/apache-spark-livy-rest-interface.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](spark/apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](spark/apache-spark-job-debugging.md)



