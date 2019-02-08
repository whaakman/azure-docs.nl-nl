---
title: Azure HDInsight-Tools - Visual Studio Code gebruiken voor Hive, LLAP of PySpark | Microsoft Docs
description: Informatie over het gebruik van de Azure HDInsight Tools voor Visual Studio Code maken en verzenden van query's en scripts.
Keywords: Visual Studio Code,Azure HDInsight Tools,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,Interactive Hive,Interactive Query
services: HDInsight
documentationcenter: ''
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/04/2019
ms.openlocfilehash: 3301f3cdea1795db3b17bc2fa7da05bf362c744d
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895065"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Azure HDInsight-hulpprogramma's voor Visual Studio Code gebruiken

Informatie over het gebruik van Azure HDInsight Tools voor Visual Studio Code maken en verzenden van Apache Hive-batchtaken, interactieve Hive-query's en PySpark-scripts voor Apache Spark. Eerst wordt wordt beschreven hoe u de HDInsight-hulpprogramma's in Visual Studio Code installeren en vervolgens wordt begeleid bij het verzenden van taken voor het Hive- en Spark.  

Azure HDInsight-hulpprogramma's kan worden geïnstalleerd op platforms die worden ondersteund door Visual Studio Code, waaronder Windows, Linux en macOS. Hieronder vindt u de vereisten voor verschillende platforms.


## <a name="prerequisites"></a>Vereisten

De volgende items zijn vereist voor de stappen in dit artikel:

- Een HDInsight-cluster. Zie voor het maken van een cluster, [aan de slag met HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono is alleen vereist voor Linux en macOS.
- De [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) voor Visual Studio Code.
- [Interactieve PySpark-omgeving instellen voor Visual Studio Code](set-up-pyspark-interactive-environment.md).
- Een lokale map met de naam **HDexample**.  In dit artikel wordt gebruikgemaakt van **C:\HD\HDexample**.

## <a name="install-azure-hdinsight-tools"></a>Azure HDInsight-hulpprogramma's installeren

Nadat u de vereisten hebt voltooid, kunt u Azure HDInsight Tools voor Visual Studio Code installeren.  De volgende stappen voor het installeren van Azure HDInsight-hulpprogramma's:

1. Open Visual Studio Code.

2. In de menubalk, gaat u naar **weergave** > **extensies**.

3. Voer in het zoekvak **HDInsight**.

4. Selecteer **Azure HDInsight-hulpprogramma's** uit de zoekresultaten, en selecteer vervolgens **installeren**.  

   ![HDInsight voor het installeren van Python voor Visual Studio Code](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Selecteer **opnieuw laden** activeren de **Azure HDInsight-hulpprogramma's** extensie nadat deze is geïnstalleerd.


## <a name="open-hdinsight-work-folder"></a>HDInsight-werkmap openen

De volgende stappen uit om een werkmap te openen en maak een bestand in Visual Studio Code:

1. In de menubalk, gaat u naar **bestand** > **map openen...**   >  **C:\HD\HDexample**en selecteer vervolgens de **map selecteren** knop. De map wordt weergegeven in de **Explorer** weergave aan de linkerkant.

2. Uit de **Explorer** weergeven, selecteert u de map **HDexample**, en vervolgens de **nieuw bestand** pictogram naast de werkmap.

   ![Nieuw bestand](./media/hdinsight-for-vscode/new-file.png)

3. Noem het nieuwe bestand met de .hql (Hive-query's) of de bestandsextensie .py (Spark-script).  In dit voorbeeld wordt **HelloWorld.hql**.

## <a name="connect-to-hdinsight-cluster"></a>Verbinding maken met HDInsight-Cluster

Voordat u scripts naar HDInsight-clusters vanuit Visual Studio Code verzendt kunt, moet u verbinding maken met uw Azure-account, of koppelen van een cluster (met behulp van Ambari gebruikersnaam en wachtwoord of het domein gekoppeld account).  Voer de volgende stappen voor het verbinding maken met Azure:

1. Navigeer naar de in de menubalk van **weergave** > **Command Palette...** , en voer **HDInsight: Aanmelding**.

    ![HDInsight Tools voor Visual Studio Code-aanmelding](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Volg de instructies aanmelden in de **uitvoer** deelvenster.
    + Voor wereldwijde Azure-omgeving, **HDInsight: Aanmelding** opdracht wordt geactiveerd **aanmelden bij Azure** actie in het HDInsight-Verkenner en vice versa.

        ![Meld u instructies voor azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Volg de instructies aanmelden voor andere omgevingen.

        ![Meld u instructies voor de andere omgeving](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

 Nadat u verbonden bent, wordt de naam van uw Azure-account wordt weergegeven op de statusbalk in de linkerbenedenhoek van het venster Visual Studio Code.  
  

<h2 id="linkcluster">Koppeling maken: Azure HDInsight</h2>

U kunt een normale cluster koppelen met behulp van een [Apache Ambari](https://ambari.apache.org/) beheerd gebruikersnaam of het koppelen van een onderneming beveiliging Pack beveiligde Hadoop-cluster met behulp van een domein-gebruikersnaam (bijvoorbeeld: user1@contoso.com).

1. Navigeer naar de in de menubalk van **weergave** > **Command Palette...** , en voer **HDInsight: Koppeling van een Cluster**.

   ![de opdracht cluster koppelen](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Gekoppelde clustertype selecteren **Azure HDInsight**.

3. Voer de URL van de HDInsight-cluster.

4. Voer de naam van de Ambari-gebruiker, de standaardwaarde is **admin**.

5. Ambari-wachtwoord invoeren.

6. Clustertype selecteren.

7. Beoordeling **uitvoer** weergeven om te verifiëren.

   > [!NOTE]  
   > De gekoppelde gebruikersnaam en wachtwoord worden gebruikt als het cluster zowel geregistreerd in Azure-abonnement en een cluster is gekoppeld.  


## <a name="create-link-generic-livy-endpoint"></a>Koppeling maken: Algemene Livy-eindpunt

1. Navigeer naar de in de menubalk van **weergave** > **Command Palette...** , en voer **HDInsight: Koppeling van een Cluster**.

2. Gekoppelde clustertype selecteren **algemene Livy eindpunt**.

3. Voer het algemene Livy-eindpunt, bijvoorbeeld: http\:/ / 10.172.41.42:18080.

4. Selecteer autorisatietype **Basic** of **geen**.  Als **Basic**, klikt u vervolgens:  
    &emsp;a. Voer de naam van de Ambari-gebruiker, de standaardwaarde is **admin**.  
    &emsp;b. Ambari-wachtwoord invoeren.

5. Beoordeling **uitvoer** weergeven om te verifiëren.

## <a name="list-hdinsight-clusters"></a>Lijst met HDInsight-clusters

1. Navigeer naar de in de menubalk van **weergave** > **Command Palette...** , en voer **HDInsight: Overzicht van Cluster**.

2. Selecteer het gewenste abonnement.

3. Controleer de **uitvoer** weergeven.  De weergave wordt weergegeven het gekoppelde cluster (s) en alle clusters onder uw Azure-abonnement.

    ![Stel een standaardconfiguratie voor cluster](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Instellen van de standaardcluster

1. Open de map opnieuw **HDexample** gemaakt [eerdere](#open-hdinsight-work-folder) als gesloten.  

2. Selecteer het bestand **HelloWorld.hql** gemaakt [eerdere](#open-hdinsight-work-folder) en deze in de script-editor wordt geopend.

3. [Verbinding maken met](#connect-to-hdinsight-cluster) bij uw Azure-account als u dit nog nog niet hebt gedaan.

4. Met de rechtermuisknop op de script-editor, en selecteer **HDInsight: Instellen van de standaardcluster**.  

5. Selecteer een cluster als het standaardcluster voor het huidige scriptbestand. De hulpprogramma's voor het configuratiebestand voor het automatisch bijwerken **. VSCode\settings.json**. 

   ![Standaardconfiguratie voor cluster instellen](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)

## <a name="set-the-azure-environment"></a>De Azure-omgeving instellen

1. [Verbinding maken met](#connect-to-hdinsight-cluster) bij uw Azure-account als u dit nog nog niet hebt gedaan.

2. Navigeer naar de in de menubalk van **weergave** > **Command Palette...** , en voer **HDInsight: Azure-omgeving instellen**.

3. Selecteer een omgeving als de standaardwaarde voor de aanmelding.

4. In de tussentijd zorgen, het hulpprogramma al uw aanmelding standaardvermelding in opgeslagen **. VSCode\settings.json**. U kunt het ook rechtstreeks op dit configuratie-item bijwerken. 

   ![Aanmelden instellen-vermelding standaardconfiguratie](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Indienen van interactieve Hive-query's, Hive batchscripts

Met HDInsight Tools for Visual Studio Code, kunt u interactieve Hive-query's indienen en batchscripts Hive met HDInsight-clusters.

1. Open de map **HDexample** gemaakt [eerdere](#open-hdinsight-work-folder) als gesloten.  

2. Selecteer het bestand **HelloWorld.hql** gemaakt [eerdere](#open-hdinsight-work-folder) en deze in de script-editor wordt geopend.

3. [Verbinding maken met](#connect-to-hdinsight-cluster) bij uw Azure-account als u dit nog nog niet hebt gedaan.

4. Kopieer en plak de volgende code in het Hive-bestand en sla het bestand op.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

5. Met de rechtermuisknop op de script-editor, selecteert u **HDInsight: Interactive hive** om de query verzenden, of gebruik snelkoppeling **Ctrl + Alt + I**.  Selecteer **HDInsight: Hive-Batch** verzend het script of er snelkoppeling **Ctrl + Alt + H**.  

6. Als u een standaardcluster hebt opgegeven, selecteert u het cluster. Met de hulpprogramma’s kunt u ook een codeblok verzenden in plaats van het hele scriptbestand, met behulp van het contextmenu. Na enkele ogenblikken worden de queryresultaten weergegeven in een nieuw tabblad.

   ![Resultaten van interactieve Hive-query’s](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Deelvenster **RESULTATEN**: U kunt het hele resultaat op een lokaal pad opslaan als een CSV-, JSON- of Excel-bestand, of gewoon meerdere regels selecteren.

    - Deelvenster **BERICHTEN**: Wanneer u **Regelnummer** selecteert, gaat u naar de eerste regel van het actieve script.

## <a name="submit-interactive-pyspark-queries"></a>Interactieve PySpark-query's verzenden

1. Open de map **HDexample** gemaakt [eerdere](#open-hdinsight-work-folder) als gesloten.  

2. Maak een nieuw bestand **HelloWorld.py** volgende de [eerdere](#open-hdinsight-work-folder) stappen.

3. U ontvangt een Python-extensie aanbeveling dialoogvenster als u geen Python voor de vereiste hebt geïnstalleerd.  Installeren en opnieuw laden van Visual Studio Code om de installatie te voltooien.

    >![HDInsight voor het installeren van Python voor Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-vscode-install-python.png)

4. [Verbinding maken met](#connect-to-hdinsight-cluster) bij uw Azure-account als u dit nog nog niet hebt gedaan.

5. Kopieer en plak de volgende code in het scriptbestand:
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

6. Met de rechtermuisknop op de script-editor, selecteert u **HDInsight: Interactieve PySpark** om de query verzenden, of gebruik snelkoppeling **Ctrl + Alt + I**.  

7. Als u een standaardcluster hebt opgegeven, selecteert u het cluster. Met de hulpprogramma’s kunt u ook een codeblok verzenden in plaats van het hele scriptbestand, met behulp van het contextmenu. Na enkele ogenblikken worden de queryresultaten weergegeven in een nieuw tabblad.

   ![Resultaat van de Python-taak verzenden](./media/hdinsight-for-vscode/pyspark-interactive-result.png) 

8. Het hulpprogramma ondersteunt ook de **SQL-component** query.

   ![Python resultaat taak indienen](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png) de verzending van de status wordt weergegeven aan de linkerkant van de status van de onderste balk wanneer u query's uitvoert. Geen andere query's verzenden wanneer de status **PySpark-Kernel (bezet)**.  

>[!NOTE]  
>De clusters kunnen sessiegegevens onderhouden. De gedefinieerde variabele, de functie en de bijbehorende waarden worden bewaard in de sessie, zodat ze kunnen worden verwezen door meerdere serviceaanroepen voor hetzelfde cluster. 

### <a name="pyspark3-is-not-supported-with-spark2223"></a>PySpark3 wordt niet ondersteund met Spark2.2/2.3

PySpark3 wordt niet meer ondersteund met Spark 2.2 en Spark2.3 cluster, alleen 'PySpark' wordt ondersteund voor Python. Het is bekend probleem dat wordt verzonden met spark 2.2/2.3 mislukken met Python3.

   ![Bij fout bij het ophalen van de python3 indienen](./media/hdinsight-for-vscode/hdi-azure-hdinsight-py3-error.png)

Volg de stappen voor het gebruik van Python2.x: 

1. Python 2.7 op de lokale computer installeren en toe te voegen aan het systeempad staat.

2. Start Visual Studio Code opnieuw op.

3. Schakel over naar de Python 2 door te klikken op de **Python XXX** op de status van de balk in en kies vervolgens het Python-doel.

   ![Selecteer python-versie](./media/hdinsight-for-vscode/hdi-azure-hdinsight-select-python.png)


## <a name="submit-pyspark-batch-job"></a>PySpark batch-taak verzenden

1. Open de map **HDexample** gemaakt [eerdere](#open-hdinsight-work-folder) als gesloten.  

2. Maak een nieuw bestand **BatchFile.py** volgende de [eerdere](#open-hdinsight-work-folder) stappen.

3. [Verbinding maken met](#connect-to-hdinsight-cluster) bij uw Azure-account als u dit nog nog niet hebt gedaan.

4. Kopieer en plak de volgende code in het scriptbestand:

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

Nadat u een Python-taak verzenden, verzending van Logboeken worden weergegeven in de **uitvoer** venster in Visual Studio Code. De **Spark gebruikersinterface URL** en **Yarn-gebruikersinterface URL** ook worden weergegeven. U kunt de URL openen in een webbrowser om bij te houden van de status van de taak.

## <a name="apache-livy-configuration"></a>Configuratie van Apache Livy

[Apache Livy](https://livy.incubator.apache.org/) configuratie wordt ondersteund, kunnen worden ingesteld op de **. VSCode\settings.json** in de werkmap van de ruimte. Configuratie van livy ondersteunt momenteel alleen Python-script. Meer informatie, Zie [Livy Leesmij](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Het activeren van de configuratie van livy**

Methode 1  
1. In de menubalk, gaat u naar **bestand** > **voorkeuren** > **instellingen**.  
2. In de **zoekinstellingen** invoeren in het tekstvak **HDInsight taak Sumission: Livy-Conf**.  
3. Selecteer **bewerken in settings.json** voor de relevante zoekresultaten.

Methode 2   
Een bestand verzendt, ziet u dat de map .vscode wordt automatisch toegevoegd aan de werkmap. U kunt de configuratie van de livy vinden door te klikken op **.vscode\settings.json**.

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
    | appId | De toepassings-id van deze sessie |  String |
    | appInfo | De gedetailleerde toepassingsinfo | Kaart van sleutel = val |
    | log | De regels van het logboek | lijst met tekenreeksen |
    | state |   De batch-status | string |

>[!NOTE]
>De configuratie van de toegewezen livy in het deelvenster Uitvoer wordt weergegeven wanneer het script indienen.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integreren met Azure HDInsight in Explorer

**Azure HDInsight** is toegevoegd aan de weergave van de Verkenner. U kunt bladeren en beheert u clusters die rechtstreeks via **Azure HDInsight**.

1. [Verbinding maken met](#connect-to-hdinsight-cluster) bij uw Azure-account als u dit nog nog niet hebt gedaan.

2. In de menubalk, gaat u naar **weergave** > **Explorer**.

3. Vouw in het linkerdeelvenster **AZURE HDINSIGHT**.  De beschikbare abonnementen en -clusters (Spark, Hadoop en HBase worden ondersteund) wordt weergegeven. 

   ![Abonnement op Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Vouw het cluster om hive-database en tabel metagegevensschema weer te geven.

   ![Azure HDInsight-cluster](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="additional-features"></a>Aanvullende functies

HDInsight voor Visual Studio Code ondersteunt de volgende functies:

- **IntelliSense automatisch aanvullen**. Suggesties weergegeven voor het trefwoord, methoden en variabelen. Verschillende pictogrammen zijn de verschillende typen objecten.

    ![HDInsight Tools voor Visual Studio Code IntelliSense objecttypen](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **IntelliSense fout markering**. Language service onderstrepen de bewerken fouten voor de Hive-script.     
- **Syntaxis van de hoogtepunten**. Language service maakt gebruik van verschillende kleuren te onderscheiden van variabelen, trefwoorden, gegevenstype, functies, enzovoort. 

    ![HDInsight Tools voor Visual Studio Code syntaxis gemarkeerd](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)


## <a name="unlink-cluster"></a>Ontkoppelen van cluster

1. Navigeer naar de in de menubalk van **weergave** > **Command Palette...** , en voer vervolgens **HDInsight: Ontkoppelen van een Cluster**.  

2. Selecteer het cluster te ontkoppelen.  

3. Beoordeling **uitvoer** weergeven om te verifiëren.  


## <a name="logout"></a>Afmelden  

Navigeer naar de in de menubalk van **weergave** > **Command Palette...** , en voer vervolgens **HDInsight: Afmelden**.  Er is een pop-upvenster in de onderste rechter hoek met de mededeling **afmelden met succes!**.


## <a name="next-steps"></a>Volgende stappen
Zie voor een demonstratie video van het gebruik van HDInsight voor Visual Studio Code, [HDInsight voor Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706)
