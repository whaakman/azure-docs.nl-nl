---
title: Azure HDInsight voor Visual Studio code
description: Meer informatie over het gebruik van de Spark-& Hive-Hulpprogram Ma's (Azure HDInsight) voor Visual Studio code voor het maken en verzenden van query's en scripts.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: da5cdd36e70166d274d50fcb093c0889cf534172
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489017"
---
# <a name="use-spark--hive-tools-for-visual-studio-code"></a>Gebruik de Spark-& Hive-Hulpprogram Ma's voor Visual Studio code

Meer informatie over het gebruik van Spark-& Hive-Hulpprogram Ma's voor Visual Studio code voor het maken en verzenden van Apache Hive batch-taken, interactieve Hive-query's en PySpark-scripts voor Apache Spark. Eerst wordt beschreven hoe u de Spark-& Hive-hulpprogram ma's in Visual Studio code installeert. vervolgens wordt uitgelegd hoe u taken kunt verzenden naar Hive en Spark.  

Spark & Hive-Hulpprogram Ma's kunnen worden geïnstalleerd op platforms die worden ondersteund door Visual Studio code, waaronder Windows, Linux en macOS. Hieronder vindt u de vereisten voor verschillende platforms.


## <a name="prerequisites"></a>Vereisten

De volgende items zijn vereist voor het volt ooien van de stappen in dit artikel:

- An HDInsight cluster. Zie aan de [slag met HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md)om een cluster te maken. Of een Spark/Hive-cluster dat livy-eind punt ondersteunt.
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono is alleen vereist voor Linux en macOS.
- [Stel PySpark Interactive environment in voor Visual Studio code](set-up-pyspark-interactive-environment.md).
- Een lokale map met de naam **HDexample**.  In dit artikel wordt gebruikgemaakt van **C:\HD\HDexample**.

## <a name="install-spark--hive-tools"></a>& Hive-Hulpprogram Ma's voor Spark installeren

Nadat u de vereisten hebt voltooid, kunt u Spark-& Hive-Hulpprogram Ma's voor Visual Studio code installeren.  Voer de volgende stappen uit om Spark & Hive-Hulpprogram Ma's te installeren:

1. Open Visual Studio Code.

2. Ga in de menu balk naar**extensies** **weer geven** > .

3. Voer in het zoekvak **Spark &-Hive**in.

4. Selecteer **Spark & Hive-Hulpprogram ma's** uit de zoek resultaten en selecteer vervolgens **installeren**.  

   ![De Spark-& Hive voor Visual Studio code python installeren](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. **Opnieuw laden** wanneer dit nodig is.


## <a name="open-work-folder"></a>Werkmap openen

Voer de volgende stappen uit om een werkmap te openen en maak een bestand in Visual Studio code:

1. Ga in de menu balk naar **bestand** > **openen map...** C:\HD\HDexample en selecteer vervolgens de knop **map selecteren** .   >  De map wordt aan de linkerkant weer gegeven in de weer gave van de **Verkenner** .

2. Selecteer in de **Verkenner** -weer gave de map **HDexample**en klik vervolgens op het pictogram **nieuw bestand** naast de werkmap.

   ![Nieuw bestand](./media/hdinsight-for-vscode/new-file.png)

3. Geef het nieuwe bestand `.hql` de naam (Hive-query's) of de `.py` bestands extensie (Spark-script).  In dit voor beeld wordt **HelloWorld. HQL**gebruikt.

## <a name="set-the-azure-environment"></a>De Azure-omgeving instellen

Voor de nationale Cloud gebruiker voert u de stappen uit om eerst de Azure-omgeving **in te stellen en vervolgens Azure te gebruiken:**  Aanmeld opdracht om u aan te melden bij Azure.
   
1. Klik op **File\Preferences\Settings**.
2. Zoeken **in Azure: Cloud**
3. Selecteer de nationale Cloud in de lijst.

   ![Configuratie van standaard aanmeldings vermelding instellen](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-azure-account"></a>Verbinding maken met Azure-account

Voordat u scripts kunt verzenden naar uw clusters vanuit Visual Studio code, moet u verbinding maken met uw Azure-account of een cluster koppelen (met behulp van Ambari gebruikers naam/wacht woord of een aan het domein gekoppelde account).  Voer de volgende stappen uit om verbinding te maken met Azure:

1. Ga in de menu balk naar  > **opdracht palet bekijken...** en voer **Azure in: Meld u**aan.

    ![Component Hulpprogramma's van Spark & voor Visual Studio code login](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Volg de instructies voor aanmelden om u aan te melden bij Azure. Nadat u verbinding hebt gemaakt, wordt de naam van uw Azure-account weer gegeven op de status balk onder aan het venster Visual Studio code.  
  

## <a name="link-a-cluster"></a>Een cluster koppelen

### <a name="link-azure-hdinsight"></a>Koppeling: Azure HDInsight

U kunt een normaal cluster koppelen met behulp van een [Apache Ambari](https://ambari.apache.org/) Managed username of een beveiligd Hadoop-cluster van het ondernemings beveiligings pakket koppelen met behulp `user1@contoso.com`van een domein gebruikers naam (zoals:).

1. Ga in de menu balk naar  > **opdracht palet bekijken...** en voer **Spark/Hive in: Koppel een cluster**.

   ![koppeling cluster opdracht](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Selecteer het gekoppelde cluster type **Azure HDInsight**.

3. Voer de URL van het HDInsight-cluster in.

4. Voer de Ambari-gebruikers naam in, de standaard instelling is **admin**.

5. Voer het Ambari-wacht woord in.

6. Selecteer een cluster type.

7. Stel de weergave naam van het cluster in (optioneel).

8. Bekijk de **uitvoer** weergave voor verificatie.

   > [!NOTE]  
   > De gekoppelde gebruikers naam en het bijbehorende wacht woord worden gebruikt als het cluster beide zijn aangemeld bij het Azure-abonnement en een cluster heeft gekoppeld.  


### <a name="link-generic-livy-endpoint"></a>Koppeling: Algemeen livy-eind punt

1. Ga in de menu balk naar  > **opdracht palet bekijken...** en voer **Spark/Hive in: Koppel een cluster**.

2. Selecteer het type van het **algemene livy-eind punt**voor het gekoppelde cluster.

3. Voer het algemene livy-eind punt in, bijvoorbeeld\:: http//10.172.41.42:18080.

4. Selecteer autorisatie type **Basic** of **none**.  Als **Basic**, dan:  
    &emsp;één. Voer de Ambari-gebruikers naam in, de standaard instelling is **admin**.  
    &emsp;b. Voer het Ambari-wacht woord in.

5. Bekijk de **uitvoer** weergave voor verificatie.

## <a name="list-clusters"></a>Clusters weer geven

1. Ga in de menu balk naar  > **opdracht palet bekijken...** en voer **Spark/Hive in: Lijst cluster**.

2. Selecteer het gewenste abonnement.

3. Bekijk de **uitvoer** weergave.  In de weer gave worden uw gekoppelde cluster (s) en alle cluster (s) weer gegeven onder uw Azure-abonnement.

    ![Een standaard cluster configuratie instellen](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Standaard cluster instellen

1. Open de map **HDexample** die u [eerder](#open-work-folder) hebt gemaakt als deze is gesloten.  

2. Selecteer het bestand **HelloWorld. HQL** dat u [eerder](#open-work-folder) hebt gemaakt en dat wordt geopend in de Script Editor.

3. Klik met de rechter muisknop op de script editor **en selecteer Spark/Hive: Het standaard cluster**instellen.  

4. [Maak verbinding](#connect-to-azure-account) met uw Azure-account of koppel een cluster als u dit nog niet hebt gedaan.

5. Selecteer een cluster als het standaard cluster voor het huidige script bestand. De hulpprogram ma's werken automatisch het configuratie bestand bij **. VSCode\settings.json**. 

   ![Standaard cluster configuratie instellen](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Interactieve Hive-query's, Hive batch scripts verzenden

Met Spark & Hive-Hulpprogram Ma's voor Visual Studio code kunt u interactieve Hive-query's en Hive-batch scripts verzenden naar uw clusters.

1. Open de map **HDexample** die u [eerder](#open-work-folder) hebt gemaakt als deze is gesloten.  

2. Selecteer het bestand **HelloWorld. HQL** dat u [eerder](#open-work-folder) hebt gemaakt en dat wordt geopend in de Script Editor.


3. Kopieer en plak de volgende code in het Hive-bestand en sla het bestand op.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. [Maak verbinding](#connect-to-azure-account) met uw Azure-account of koppel een cluster als u dit nog niet hebt gedaan.

5. Klik met de rechter muisknop op script editor **, selecteer Hive: Interactief** om de query te verzenden of gebruik de sneltoets **CTRL + ALT + I**.  Component **selecteren: Batch** om het script te verzenden of gebruik de sneltoets **CTRL + ALT + H**.  

6. Selecteer het cluster als u geen standaard cluster hebt opgegeven. Met de hulpprogramma’s kunt u ook een codeblok verzenden in plaats van het hele scriptbestand, met behulp van het contextmenu. Na enkele ogen blikken worden de query resultaten weer gegeven op een nieuw tabblad.

   ![Resultaten van interactieve Hive-query’s](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Deelvenster **RESULTATEN**: U kunt het hele resultaat op een lokaal pad opslaan als een CSV-, JSON- of Excel-bestand, of gewoon meerdere regels selecteren.

    - Deelvenster **BERICHTEN**: Wanneer u **Regelnummer** selecteert, gaat u naar de eerste regel van het actieve script.

## <a name="submit-interactive-pyspark-queries"></a>Interactieve PySpark-query's verzenden

U kunt interactieve PySpark-query's verzenden door de volgende stappen uit te voeren:

1. Open de map **HDexample** die u [eerder](#open-work-folder) hebt gemaakt als deze is gesloten.  

2. Maak een nieuw bestand **HelloWorld.py** door de [eerdere](#open-work-folder) stappen te volgen.

3. Kopieer en plak de volgende code in het script bestand:
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

4. [Maak verbinding](#connect-to-azure-account) met uw Azure-account of koppel een cluster als u dit nog niet hebt gedaan.

5. Kies alle code en klik met de rechter muisknop op de script editor **, selecteer Spark: PySpark Interactive** om de query te verzenden, of gebruik de sneltoets **CTRL + ALT + I**.

   ![context menu van pyspark Interactive](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Selecteer het cluster als u geen standaard cluster hebt opgegeven. Na enkele ogen blikken worden de **python-interactieve** resultaten weer gegeven op een nieuw tabblad. Met de hulpprogramma’s kunt u ook een codeblok verzenden in plaats van het hele scriptbestand, met behulp van het contextmenu. 

   ![interactief pyspark Interactive python-venster](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png) 

7. Voer **%% info**in en druk op **SHIFT + ENTER** om de taak gegevens weer te geven. (optioneel)

   ![taak gegevens weer geven](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. Het hulp programma ondersteunt ook de **Spark SQL** -query.

   ![Resultaat van interactieve weer gave Pyspark](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   De status van de verzen ding wordt weer gegeven aan de linkerkant van de onderste status balk wanneer query's worden uitgevoerd. Verzend geen andere query's wanneer de status **PySpark kernel (bezet)** is.  

   > [!NOTE] 
   >
   > Als de **uitbrei ding van python ingeschakeld is uitgeschakeld** in de instellingen (de standaard instelling is ingeschakeld), wordt het oude venster gebruikt voor de pyspark-interactie resultaten.
   >
   > ![pyspark Interactive python-uitbrei ding uitgeschakeld](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>Batch taak PySpark verzenden

1. Open de map **HDexample** die u [eerder](#open-work-folder) hebt gemaakt als deze is gesloten.  

2. Maak een nieuw bestand **BatchFile.py** door de [eerdere](#open-work-folder) stappen te volgen.

3. Kopieer en plak de volgende code in het script bestand:

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

4. [Maak verbinding](#connect-to-azure-account) met uw Azure-account of koppel een cluster als u dit nog niet hebt gedaan.

5. Klik met de rechter muisknop op de script editor en **Selecteer Spark: PySpark batch**, of gebruik de sneltoets **CTRL + ALT + H**. 

6. Selecteer een cluster waarnaar u uw PySpark-taak wilt verzenden. 

   ![Python-taak resultaat verzenden](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Nadat u een python-taak hebt verzonden, worden de inzendings logboeken weer gegeven in het **uitvoer** venster van Visual Studio code. De url's van de **Spark-gebruikers** interface en de **garen-URL** worden ook weer gegeven. U kunt de URL in een webbrowser openen om de taak status bij te houden.

## <a name="apache-livy-configuration"></a>Apache livy-configuratie

[Apache livy](https://livy.incubator.apache.org/) -configuratie wordt ondersteund en kan worden ingesteld op de **. VSCode\settings.json** in de map werk ruimte. Momenteel biedt livy-configuratie alleen ondersteuning voor python-script. Zie [LIVY README](https://github.com/cloudera/livy/blob/master/README.rst )(Engelstalig) voor meer informatie.

<a id="triggerlivyconf"></a>**Livy-configuratie activeren**

Methode 1  
1. Navigeer in de menu balk naar**instellingen**voor **Bestands** > **Voorkeuren** > .  
2. Voer**in het tekstvak **Zoek instellingen** HDInsight-taak sumission in: Livy conf**.  
3. Selecteer **bewerken in instellingen. json** voor het relevante Zoek resultaat.

Methode 2   
Een bestand verzenden, Let op: de map. vscode wordt automatisch toegevoegd aan de werkmap. U kunt de livy-configuratie vinden door te klikken op **. vscode\settings.json**.

+ De project instellingen:

    ![Livy-configuratie](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Voor instellingen **driverMomory** en **executorMomry**, stelt u de waarde met eenheid in, bijvoorbeeld 1g of 1024m. 

+ De ondersteunde livy-configuraties:   

    **/Batches plaatsen**   
    Hoofdtekst van de aanvraag

    | name | description | type | 
    | :- | :- | :- | 
    | file | Bestand met de toepassing die moet worden uitgevoerd | pad (vereist) | 
    | proxyUser | Gebruiker die moet worden geïmiteerd bij het uitvoeren van de taak | string | 
    | className | Hoofd klasse java/Spark-toepassing | string |
    | args | Opdracht regel argumenten voor de toepassing | lijst met teken reeksen | 
    | jars | potten die in deze sessie moeten worden gebruikt | Lijst met teken reeksen | 
    | pyFiles | Python-bestanden die moeten worden gebruikt in deze sessie | Lijst met teken reeksen |
    | files | bestanden die moeten worden gebruikt in deze sessie | Lijst met teken reeksen |
    | driverMemory | Hoeveelheid geheugen die moet worden gebruikt voor het stuur programma | string |
    | driverCores | Het aantal kernen dat moet worden gebruikt voor het stuur programma | int |
    | executorMemory | Hoeveelheid geheugen die per bewerkings proces moet worden gebruikt | string |
    | executorCores | Aantal kernen dat voor elke uitvoerder moet worden gebruikt | int |
    | numExecutors | Aantal actieve uitvoerendeers dat voor deze sessie wordt gestart | int |
    | archives | Archief archieven die moeten worden gebruikt in deze sessie | Lijst met teken reeksen |
    | queue | De naam van de wachtrij voor GARENs waarnaar deze is verzonden | string |
    | name | De naam van deze sessie | string |
    | belangen | Eigenschappen van Spark-configuratie | Toewijzing van sleutel = val |

    Hoofdtekst van de reactie   
    Het gemaakte batch-object.

    | name | description | type | 
    | :- | :- | :- | 
    | id | De sessie-id | int | 
    | appId | De toepassings-id van deze sessie |  Tekenreeks |
    | appInfo | Gedetailleerde informatie over de toepassing | Toewijzing van sleutel = val |
    | logboek | De logboek regels | lijst met teken reeksen |
    | toestand |   De batch status | string |

>[!NOTE]
>De toegewezen livy-configuratie wordt weer gegeven in het deel venster uitvoer bij het verzenden van een script.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integreren met Azure HDInsight vanuit Explorer

**Azure HDInsight** is toegevoegd aan de Verkenner-weer gave. U kunt uw cluster (s) rechtstreeks in **Azure HDInsight**doorzoeken en beheren.

1. [Maak verbinding](#connect-to-azure-account) met uw Azure-account of koppel een cluster als u dit nog niet hebt gedaan.

2. Ga in de menu balk naar **weer gave** > **Explorer**.

3. Vouw in het linkerdeel venster **Azure HDINSIGHT**uit.  De beschik bare abonnementen en clusters (Spark, Hadoop en HBase worden ondersteund) worden weer gegeven. 

   ![Azure HDInsight-abonnement](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Vouw het cluster uit om de meta gegevens database en het tabel schema van de Hive weer te geven.

   ![Azure HDInsight-cluster](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="preview-hive-table"></a>Voor beeld Hive-tabel
U kunt de Hive-tabel in uw cluster (s) rechtstreeks in **Azure HDInsight** Explorer bekijken.
1. [Maak verbinding](#connect-to-azure-account) met uw Azure-account als u dit nog niet hebt gedaan.

2. Klik op het pictogram van **Azure** in de meest linkse kolom.

3. Vouw in het linkerdeel venster **Azure HDINSIGHT**uit. De beschik bare abonnementen en clusters worden weer gegeven.

4. Vouw het cluster uit om de meta gegevens database en het tabel schema van de Hive weer te geven.

5. Klik met de rechter muisknop op de Hive-tabel, bijvoorbeeld hivesampletable. Selecteer **voor beeld**. 

   ![Component tabel van Spark & voor Visual Studio code-Preview](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. Het venster **preview-resultaten** wordt geopend.

   ![Het &-onderdeel Spark voor Visual Studio code-voorbeeld resultaten](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)
   
- Paneel **resultaten**

   U kunt het hele resultaat op een lokaal pad opslaan als een CSV-, JSON- of Excel-bestand, of gewoon meerdere regels selecteren.

- **Berichten** paneel
   1. Wanneer het aantal rijen in de tabel groter is dan 100 rijen, wordt in het bericht het volgende weer gegeven: **De eerste 100 rijen worden weer gegeven voor de Hive-tabel**.
   2. Wanneer het aantal rijen in de tabel kleiner is dan of gelijk is aan 100 rijen, wordt in het bericht het volgende weer gegeven: **er worden 60 rijen weer gegeven voor de Hive-tabel**.
   3. Wanneer er geen inhoud in de tabel staat, wordt in het bericht het volgende weer gegeven: de **rij 0 wordt weer gegeven voor de Hive-tabel**.

>[!NOTE]
>
>Installeer xclip in Linux om tabel gegevens te kunnen kopiëren.
>
>![Spark-& Hive voor Visual Studio code in Linux](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)
## <a name="additional-features"></a>Aanvullende functies

De Spark-& Hive voor Visual Studio code ondersteunt de volgende functies:

- **Automatisch aanvullen IntelliSense**. Suggesties worden pop-ups gemaakt voor tref woorden, methoden, variabelen, enzovoort. Verschillende pictogrammen vertegenwoordigen verschillende object typen.

    ![Spark-& Hive-Hulpprogram Ma's voor Visual Studio code IntelliSense-object typen](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Fout markering voor IntelliSense**. De taal service onderstreept de bewerkings fouten voor het Hive-script.     
- **Syntaxis punten**. De taal service gebruikt verschillende kleuren voor het onderscheiden van variabelen, tref woorden, gegevens typen, functies, enzovoort. 

    ![De belangrijkste aandachtspunten voor de Visual Studio-code voor het onderdeel Spark &](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Rol alleen lezer

Gebruikers met de   **rol** alleen cluster lezers kunnen geen taak meer verzenden naar het HDInsight-cluster of de Hive-Data Base weer geven. Neem contact op met de Cluster beheerder om uw rol bij te werken naar de [ **HDInsight** - **cluster** **operator** ](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) in de [Azure Portal](https://ms.portal.azure.com/). Als u Ambari-referenties kent, kunt u het cluster hand matig koppelen volgens de onderstaande instructie.

### <a name="browse-hdinsight-cluster"></a>Bladeren in HDInsight-cluster  

Wanneer u op de Azure HDInsight Explorer klikt om een HDInsight-cluster uit te vouwen, wordt u gevraagd het cluster te koppelen als u alleen een rol voor het cluster hebt. Volg de onderstaande stappen om een koppeling te maken met het cluster via Ambari-referenties. 

### <a name="submit-job-to-hdinsight-cluster"></a>Taak verzenden naar HDInsight-cluster

Bij het verzenden van een taak naar een HDInsight-cluster wordt u gevraagd het cluster te koppelen als u alleen een rol voor het cluster hebt. Volg de onderstaande stappen om een koppeling te maken met het cluster via Ambari-referenties. 

### <a name="link-to-cluster"></a>Koppelen aan cluster

1.  Voer de Ambari-gebruikers naam in 
2.  Voer Ambari-gebruikers wachtwoord in.

   ![Gebruikers naam van Spark &-Hive voor Visual Studio code](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Onderdeel Hulpprogramma's van Spark & voor Visual Studio code-wacht woord](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

> [!NOTE]
>
>U kunt Spark/Hive gebruiken: Lijst cluster om het gekoppelde cluster te controleren.
>
>![Koppeling naar het onderdeel Hulpprogramma's van Spark & voor Visual Studio code Reader](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="azure-data-lake-storage-gen2-adls-gen2"></a>Azure Data Lake Storage Gen2 (ADLS Gen2)

### <a name="browse-an-adls-gen2-account"></a>Bladeren in een ADLS Gen2-account

Wanneer u op de Azure HDInsight Explorer klikt om een ADLS Gen2 account uit te vouwen, wordt u gevraagd om de **toegangs sleutel** voor opslag in te voeren als uw Azure-account geen toegang heeft tot de Gen2-opslag. Het ADLS Gen2-account wordt automatisch uitgebreid wanneer de toegangs sleutel is gevalideerd. 

### <a name="submit-jobs-to-hdinsight-cluster-with-adls-gen2"></a>Taken verzenden naar een HDInsight-cluster met ADLS Gen2

Bij het verzenden van een taak naar een HDInsight-cluster met ADLS Gen2, wordt u gevraagd om de **toegangs sleutel** voor opslag in te voeren als uw Azure-account geen schrijf toegang heeft tot de Gen2-opslag.  De taak wordt verzonden zodra de toegangs sleutel is gevalideerd. 

![Spark-& Hive-Hulpprogram Ma's voor Visual Studio code AccessKey](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)   

> [!NOTE]
> 
>U kunt de toegangs sleutel voor het opslag account ophalen uit de Azure Portal. Zie [toegangs sleutels weer geven en kopiëren](https://docs.microsoft.com/azure/storage/common/storage-account-manage#access-keys)voor meer informatie.

## <a name="unlink-cluster"></a>Cluster ontkoppelen

1. Navigeer in de menu balk om  > het**opdracht palet te bekijken...** en voer **vervolgens Spark/Hive in: Een cluster**ontkoppelen.  

2. Selecteer een cluster om te ontkoppelen.  

3. Bekijk de **uitvoer** weergave voor verificatie.  

## <a name="sign-out"></a>Afmelden  

Ga in de menu balk naar  > **opdracht palet bekijken...** en voer **Azure in: Meld u af.**


## <a name="next-steps"></a>Volgende stappen
Voor een demonstratie video over het gebruik van Spark-& Hive voor Visual Studio code verwijzen wij u naar de [component spark & voor Visual Studio code](https://go.microsoft.com/fwlink/?linkid=858706)
