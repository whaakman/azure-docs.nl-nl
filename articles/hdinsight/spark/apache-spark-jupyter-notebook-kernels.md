---
title: Kernels voor Jupyter-notebook in Spark-clusters in Azure HDInsight
description: Meer informatie over de PySpark, PySpark3 en Spark kernels voor Jupyter-notitieblok met Spark-clusters in Azure HDInsight.
keywords: jupyter-notebook in spark, spark jupyter
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 02/22/2018
ms.author: hrasheed
ms.openlocfilehash: 937f6ffb9865419611c35b95ac84832bb2f1f3fe
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53791807"
---
# <a name="kernels-for-jupyter-notebook-on-apache-spark-clusters-in-azure-hdinsight"></a>Kernels voor Jupyter-notebook op Apache Spark-clusters in Azure HDInsight 

HDInsight Spark-clusters bieden kernels die u kunt gebruiken met de Jupyter-notebook op [Apache Spark](https://spark.apache.org/) voor het testen van uw toepassingen. Een kernel is een programma dat wordt uitgevoerd en interpreteert van gebruikers in uw code. De drie kernels zijn:

- **PySpark** : voor toepassingen die zijn geschreven in Python2.
- **PySpark3** : voor toepassingen die zijn geschreven in Python3.
- **Spark** : voor toepassingen die zijn geschreven in Scala.

In dit artikel leert u hoe u deze kernels en de voordelen van het gebruik ervan.

## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Een Jupyter-notebook in Spark HDInsight maken

1. Uit de [Azure-portal](https://portal.azure.com/), opent u het cluster.  Zie [clusters tonen en vermelden](../hdinsight-administer-use-portal-linux.md#showClusters) voor instructies. Het cluster wordt geopend in een nieuwe portalblade.

2. Uit de **snelkoppelingen** sectie, klikt u op **Clusterdashboards** openen de **Clusterdashboards** blade.  Als er geen **snelkoppelingen**, klikt u op **overzicht** in het menu links op de blade.

    ![Jupyter-notebook in Spark](./media/apache-spark-jupyter-notebook-kernels/hdinsight-jupyter-notebook-on-spark.png "Jupyter-notebook in Spark") 

3. Klik op **Jupyter-Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.
   
   > [!NOTE]  
   > U kunt ook contact opnemen de Jupyter-notebook in Spark-cluster door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`


3. Klik op **nieuw**, en klik dan ofwel **Pyspark**, **PySpark3**, of **Spark** om een notitieblok te maken. Gebruik de Spark-kernel voor Scala-toepassingen, de PySpark-kernel voor Python2-toepassingen en PySpark3 kernel voor Python3-toepassingen.
   
    ![Kernels voor Jupyter-notebook in Spark](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Kernels voor Jupyter-notebook in Spark") 

4. Een laptop wordt geopend met de kernel die u hebt geselecteerd.

## <a name="benefits-of-using-the-kernels"></a>Voordelen van het gebruik van de kernels

Hier volgen enkele voordelen van het gebruik van de nieuwe kernels op Jupyter-notebook in Spark HDInsight-clusters.

- **Vooraf ingestelde contexten**. Met **PySpark**, **PySpark3**, of de **Spark** kernels, hoeft u niet expliciet de Spark- of Hive-contexten instellen voordat u aan de slag gaat met uw toepassingen. Dit zijn standaard beschikbaar zijn. Deze context zijn:
   
   * **sc** - voor Spark-context
   * **sqlContext** - voor Hive-context
   
   U moet dus geen instructies als volgt om in te stellen de contexten worden uitgevoerd:
   
          sc = SparkContext('yarn-client')
          sqlContext = HiveContext(sc)
   
   U kunt de vooraf ingestelde contexten in plaats daarvan rechtstreeks in uw toepassing gebruiken.

- **Cel-magics**. De PySpark-kernel biedt enkele vooraf gedefinieerde "kunt", die zijn speciale opdrachten die u met aanroepen kunt `%%` (bijvoorbeeld `%%MAGIC` <args>). De magic-opdracht moet de eerste woord in een codecel en toestaan voor meerdere regels van inhoud. Het magische woord moet het eerste woord in de cel. Toevoegen van voordat de Magic-pakket, zelfs commentaar, veroorzaakt een fout.     Zie voor meer informatie over magics [hier](https://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    De volgende tabel geeft een lijst van de andere magics die beschikbaar zijn via de kernels.

   | Magic-pakket | Voorbeeld | Description |
   | --- | --- | --- |
   | Help |`%%help` |Genereert een tabel met alle beschikbare magics met voorbeeld en beschrijving |
   | informatie |`%%info` |Uitvoer sessie-informatie voor het eindpunt van de huidige Livy |
   | configureren |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Hiermee configureert u de parameters voor het maken van een sessie. De markering forceren (-f) is verplicht als een sessie al is gemaakt, die zorgt ervoor dat de sessie verwijderd en opnieuw gemaakt. Bekijk [van Livy POST /sessions aanvraagtekst](https://github.com/cloudera/livy#request-body) voor een lijst met geldige parameters op. Parameters moeten worden doorgegeven als een JSON-tekenreeks en moeten op de volgende regel na de Magic-pakket, zoals wordt weergegeven in de voorbeeldkolom. |
   | sql |`%%sql -o <variable name>`<br> `SHOW TABLES` |Voert een Hive-query op de sqlContext. Als de `-o` parameter is doorgegeven, het resultaat van de query worden bewaard de %% lokale Python-context als een [Pandas](https://pandas.pydata.org/) dataframe. |
   | lokaal |`%%local`<br>`a=1` |De code in de volgende regels wordt lokaal uitgevoerd. Code moet geldige Python2-code ook, ongeacht de kernel die u gebruikt. Dit het geval is, zelfs als u hebt geselecteerd **PySpark3** of **Spark** kernels tijdens het maken van het notitieblok, als u de `%%local` magic in een cel, die cel moet alleen geldige Python2 code hebben... |
   | logboeken |`%%logs` |Voert de logboeken voor de huidige Livy-sessie. |
   | delete |`%%delete -f -s <session number>` |Hiermee verwijdert u een specifieke sessie van de huidige Livy-eindpunt. U kunt de sessie die is gestart voor de kernel zelf niet verwijderen. |
   | opruimen |`%%cleanup -f` |Hiermee verwijdert u alle sessies voor de huidige Livy-eindpunt, met inbegrip van dit notitieblok sessie. De vlag force -f is verplicht. |

   > [!NOTE]  
   > Naast de magics die door de PySpark-kernel toegevoegd, kunt u ook gebruiken de [ingebouwde IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), waaronder `%%sh`. U kunt de `%%sh` magic-blok van code en scripts uitvoeren op het hoofdknooppunt van het cluster.

2. **Visualisatie automatisch**. De **Pyspark** kernel automatisch de uitvoer van Hive- en SQL-query's worden gevisualiseerd. U kunt kiezen tussen de verschillende typen visualisaties, met inbegrip van de tabel, Pie, regel, gebied, -balk.

## <a name="parameters-supported-with-the-sql-magic"></a>Parameters die worden ondersteund met de %% sql Magic-pakket
De `%%sql` Magic-pakket biedt ondersteuning voor verschillende parameters die u gebruiken kunt voor het beheren van het type van de uitvoer die u ontvangt wanneer u query's uitvoeren. De volgende tabel bevat de uitvoer.

| Parameter | Voorbeeld | Description |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Gebruik deze parameter om vast te leggen het resultaat van de query in de %% lokale Python-context, als een [Pandas](https://pandas.pydata.org/) dataframe. De naam van de variabele dataframe is de naam van de variabele die u opgeeft. |
| -q |`-q` |Gebruik deze optie uitschakelen visualisaties voor de cel. Als u niet wilt dat de inhoud van een cel met een automatisch te visualiseren en alleen wilt vastleggen als een dataframe en gebruik vervolgens `-q -o <VARIABLE>`. Als u uitschakelen visualisaties wilt zonder het vastleggen van de resultaten (bijvoorbeeld voor het uitvoeren van een SQL-query, zoals een `CREATE TABLE` instructie), gebruikt u `-q` zonder op te geven een `-o` argument. |
| -m |`-m <METHOD>` |Waar **methode** is **nemen** of **voorbeeld** (de standaardwaarde is **nemen**). Als u de methode **nemen**, de kernel kiest elementen vanaf de bovenkant van de resultatenset voor gegevens die worden opgegeven door MAXROWS (Zie verderop in deze tabel). Als u de methode **voorbeeld**, willekeurig voorbeelden elementen van de gegevensset op basis van de kernel `-r` parameter, die vervolgens worden beschreven in deze tabel. |
| -r |`-r <FRACTION>` |Hier **FRACTIE** is een getal met drijvende komma tussen 0,0 en 1,0 liggen. Als de voorbeeldmethode voor de SQL-query is `sample`, en vervolgens de kernel willekeurig voorbeelden van de opgegeven fractie van de elementen van de resultatenset voor u. Bijvoorbeeld, als u een SQL-query uitvoeren met de argumenten `-m sample -r 0.01`, en vervolgens 1% van de Resultatenrijen willekeurig steekproeven worden genomen. |
| -n |`-n <MAXROWS>` |**MAXROWS** is een geheel getal. De kernel beperkt het aantal rijen dat u uitvoer wilt **MAXROWS**. Als **MAXROWS** is een negatief getal zoals **-1**, en vervolgens het aantal rijen in de resultatenset niet beperkt is. |

**Voorbeeld:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

De instructie hiervoor doet het volgende:

* Hiermee selecteert u alle records uit **hivesampletable**.
* Omdat we - q, schakelt automatische-visualisatie.
* Omdat we `-m sample -r 0.1 -n 500` het willekeurig 10% van de rijen in de hivesampletable-voorbeelden en de limiet voor de grootte van het resultaat ingesteld op 500 rijen.
* Ten slotte, omdat we gebruikt `-o query2` Bovendien bespaart u de uitvoer in een dataframe met de naam **query2**.

## <a name="considerations-while-using-the-new-kernels"></a>Overwegingen bij het gebruik van de nieuwe kernels

Welke kernel die u gebruikt, verbruikt waardoor de laptops met de clusterresources.  Met deze kernels, omdat de contexten worden vooraf ingesteld, gewoon afsluiten van de notebooks de context niet beëindigen en kan daarom de clusterbronnen nog steeds worden gebruikt. Er is een goede gewoonte om te gebruiken de **sluiten en stoppen** optie uit van het notitieblok **bestand** menu wanneer u klaar met het notitieblok bent, beëindigt de context en vervolgens de notebook afgesloten.     

## <a name="show-me-some-examples"></a>Enkele voorbeelden weergeven

Wanneer u een Jupyter-notebook opent, ziet u twee mappen beschikbaar op het hoogste niveau.

* De **PySpark** heeft voorbeeldnotitieblokken die gebruikmaken van de nieuwe map **Python** kernel.
* De **Scala** heeft voorbeeldnotitieblokken die gebruikmaken van de nieuwe map **Spark** kernel.

U kunt openen de **00 - [lezen mij eerst] Spark Magic-Kernel-functies** notebook vanuit de **PySpark** of **Spark** map voor meer informatie over de andere magics die beschikbaar zijn. U kunt ook de andere beschikbare voorbeeldnotitieblokken onder de twee mappen gebruiken voor informatie over het bereiken van verschillende scenario's met Jupyter-notebooks met HDInsight Spark-clusters.

## <a name="where-are-the-notebooks-stored"></a>Waar worden de notebooks opgeslagen?

Als uw cluster gebruikmaakt van Azure Storage als het standaardaccount voor opslag, Jupyter-notebooks worden opgeslagen naar storage-account onder de **/HdiNotebooks** map.  Notitieblokken, bestanden en mappen die u vanuit Jupyter maakt zijn toegankelijk is vanaf de storage-account.  Bijvoorbeeld, als u een map maken met Jupyter **mijnmap** en een laptop **myfolder/mynotebook.ipynb**, u hebt toegang tot die laptop `/HdiNotebooks/myfolder/mynotebook.ipynb` binnen het opslagaccount.  Het omgekeerde geldt ook, dat wil zeggen, als u een notitieblok rechtstreeks uploaden naar uw opslagaccount op `/HdiNotebooks/mynotebook1.ipynb`, de laptop is ook van Jupyter zichtbaar.  Notitieblokken blijven in de storage-account, zelfs nadat het cluster wordt verwijderd.

> [!NOTE]  
> HDInsight-clusters met Azure Data Lake Storage als de standaardopslag geen notitieblokken opgeslagen in de bijbehorende opslag.

De manier waarop notitieblokken worden opgeslagen in het opslagaccount is compatibel met [Apache Hadoop HDFS](https://hadoop.apache.org/docs/r1.2.1/hdfs_design.html). Dus als u SSH in het cluster dat kunt u opdrachten voor het beheer zoals wordt weergegeven in het volgende codefragment:

    hdfs dfs -ls /HdiNotebooks                               # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                    # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter

Ongeacht of het cluster maakt gebruik van Azure Storage of Azure Data Lake Storage als het standaardaccount voor opslag, worden ook de notebooks opgeslagen op het hoofdknooppunt van het cluster op `/var/lib/jupyter`.

## <a name="supported-browser"></a>Ondersteunde browser

Jupyter-notebooks in HDInsight Spark-clusters worden alleen ondersteund op Google Chrome.

## <a name="feedback"></a>Feedback
De nieuwe kernels zijn in de fase in ontwikkeling en zullen vervallen na verloop van tijd. Dit kan ook betekenen dat de API's wijzigen kan omdat deze kernels vervallen. We zouden alle feedback die u hebt tijdens het gebruik van deze nieuwe kernels waarderen. Dit is handig in de definitieve versie van deze kernels vormgeven. U kunt feedback uw opmerkingen/onder de **opmerkingen** sectie onder aan dit artikel.

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
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op te sporen Apache Spark-toepassingen op afstand](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
