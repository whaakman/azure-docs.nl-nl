---
title: Kernels voor Jupyter-notebook in Spark-clusters in Azure HDInsight | Microsoft Docs
description: Meer informatie over de PySpark PySpark3 en Spark kernels voor Jupyter-notebook met Spark op Azure HDInsight-clusters.
keywords: jupyter-notebook in spark, jupyter spark
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0719e503-ee6d-41ac-b37e-3d77db8b121b
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: nitinme
ms.openlocfilehash: b561352d702d1c5a415ebebc253869b8a56d68d8
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="kernels-for-jupyter-notebook-on-spark-clusters-in-azure-hdinsight"></a>Kernels voor Jupyter-notebook in Spark-clusters in Azure HDInsight 

HDInsight Spark-clusters bieden kernels die u met de Jupyter-notebook in Spark gebruiken kunt voor het testen van uw toepassingen. Een kernel is een programma dat wordt uitgevoerd en interpreteert uw code. De drie kernels zijn:

- **PySpark** - voor toepassingen die zijn geschreven in Python2
- **PySpark3** - voor toepassingen die zijn geschreven in Python3
- **Spark** - voor toepassingen die zijn geschreven in Scala

In dit artikel leert u hoe u deze kernels en de voordelen van het gebruik ervan.

## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="create-a-jupyter-notebook-on-spark-hdinsight"></a>Maken van een Jupyter-notebook in Spark HDInsight

1. Van de [Azure-portal](https://portal.azure.com/), opent u het cluster.  Zie [lijst en geeft weer clusters](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters) voor instructies. Het cluster wordt geopend in een nieuwe portalblade.

2. Van de **snelle koppelingen** sectie, klikt u op **Cluster dashboards** openen de **Cluster dashboards** blade.  Als er geen **snelkoppelingen**, klikt u op **overzicht** in het menu links op de blade.

    ![Jupyter-notebook in Spark](./media/apache-spark-jupyter-notebook-kernels/hdinsight-jupyter-notebook-on-spark.png "Jupyter-notebook in Spark") 

3. Klik op **Jupyter-Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.
   
   > [!NOTE]
   > U mag de Jupyter-notebook in Spark-cluster ook bereiken door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 

3. Klik op **nieuw**, en klikt u ofwel **Pyspark**, **PySpark3**, of **Spark** voor het maken van een laptop. Gebruik de kernel Spark Scala-toepassingen, PySpark-kernel voor Python2 toepassingen en PySpark3 kernel voor Python3 toepassingen.
   
    ![Kernels voor Jupyter-notebook in Spark](./media/apache-spark-jupyter-notebook-kernels/kernel-jupyter-notebook-on-spark.png "Kernels voor Jupyter-notebook in Spark") 

4. Een laptop wordt geopend met de kernel dat u hebt geselecteerd.

## <a name="benefits-of-using-the-kernels"></a>Voordelen van het gebruik van de kernels

Hier volgen enkele voordelen van het gebruik van de nieuwe kernels op Jupyter-notebook op HDInsight Spark-clusters.

- **Voorinstelling contexten**. Met **PySpark**, **PySpark3**, of de **Spark** kernels, hoeft u niet expliciet de Spark- of Hive-contexten instellen voordat u begint te werken met uw toepassingen. Dit zijn standaard beschikbaar. Deze contexten zijn:
   
   * **sc** : voor het Spark-context
   * **sqlContext** - voor Hive-context
   
   U moet dus geen instructies als volgt om in te stellen de contexten uitvoeren:
   
          sc = SparkContext('yarn-client')
          sqlContext = HiveContext(sc)
   
   U kunt de vooraf ingestelde contexten in plaats daarvan direct gebruiken in uw toepassing.

- **Cel-magics**. De PySpark-kernel biedt een aantal vooraf gedefinieerde 'magics', die zijn speciale opdrachten die u met aanroepen kunt `%%` (bijvoorbeeld `%%MAGIC` <args>). Het magische opdracht moet het eerste woord in een codecel en toestaan voor meerdere regels van inhoud. Het magische woord moet het eerste woord in de cel. Toevoegen van voordat de magic, zelfs opmerkingen veroorzaakt een fout.     Zie voor meer informatie over magics [hier](http://ipython.readthedocs.org/en/stable/interactive/magics.html).
   
    De volgende tabel bevat de verschillende magics die beschikbaar is via de kernels.

   | Verwerkt Magic-pakket | Voorbeeld | Beschrijving |
   | --- | --- | --- |
   | help |`%%help` |Genereert een lijst met alle beschikbare magics met voorbeeld en beschrijving |
   | Info |`%%info` |Sessie-informatie voor het huidige Livy eindpunt uitvoer |
   | configureren |`%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} |Hiermee configureert u de parameters voor het maken van een sessie. De vlag (-f) is verplicht als een sessie al is gemaakt, die zorgt ervoor dat de sessie wordt verwijderd en opnieuw gemaakt. Bekijk [van Livy POST /sessions aanvraagtekst](https://github.com/cloudera/livy#request-body) voor een lijst met geldige parameters op. Parameters moeten worden doorgegeven als een JSON-tekenreeks en moeten op de volgende regel na de magic, zoals wordt weergegeven in de voorbeeldkolom. |
   | sql |`%%sql -o <variable name>`<br> `SHOW TABLES` |Een Hive-query op de sqlContext worden uitgevoerd. Als de `-o` parameter is doorgegeven, het resultaat van de query wordt bewaard de %% lokale Python context als een [Pandas](http://pandas.pydata.org/) dataframe. |
   | lokale |`%%local`<br>`a=1` |De code in de volgende regels wordt lokaal uitgevoerd. De sitecode moet geldige Python2 code zelfs ongeacht de kernel dat u gebruikt. Zo is, zelfs als u **PySpark3** of **Spark** kernels tijdens het maken van de notebook als u de `%%local` magische in een cel, die cel moet alleen code bevatten, geldige Python2... |
   | logboeken |`%%logs` |De logboeken levert voor de huidige sessie van Livy. |
   | verwijderen |`%%delete -f -s <session number>` |Hiermee verwijdert u een bepaalde sessie van het huidige Livy-eindpunt. Houd er rekening mee dat u de sessie die is gestart voor de kernel zichzelf niet verwijderen. |
   | Opruimen |`%%cleanup -f` |Hiermee verwijdert u alle sessies voor de huidige Livy-eindpunt, waaronder deze laptop-sessie. De vlag force -f is verplicht. |

   > [!NOTE]
   > Naast de magics die door de PySpark-kernel zijn toegevoegd, kunt u ook gebruiken de [ingebouwde IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), inclusief `%%sh`. U kunt de `%%sh` verwerkt Magic-pakket voor het uitvoeren van scripts en codeblok op het cluster headnode.
   >
   >
2. **Visualisatie automatisch**. De **Pyspark** kernel visualiseren automatisch de uitvoer van Hive en SQL-query's. U kunt kiezen tussen verschillende soorten visualisaties, met inbegrip van de tabel, cirkel, regel, gebied, balk.

## <a name="parameters-supported-with-the-sql-magic"></a>Parameters die worden ondersteund met de %% sql verwerkt Magic-pakket
De `%%sql` magic ondersteunt verschillende parameters die u gebruiken kunt om te bepalen van het type uitvoer dat wordt weergegeven wanneer u query's uitvoeren. De volgende tabel bevat de uitvoer.

| Parameter | Voorbeeld | Beschrijving |
| --- | --- | --- |
| -o |`-o <VARIABLE NAME>` |Gebruik deze parameter om het resultaat van de query in de %% lokale Python-context, als een [Pandas](http://pandas.pydata.org/) dataframe. De naam van de variabele dataframe is de naam van de variabele die u opgeeft. |
| -q |`-q` |Met deze optie uitschakelen visualisaties voor de cel. Als u niet wilt dat de inhoud van een cel met een automatisch te visualiseren en willen het als een dataframe vastleggen en gebruik vervolgens `-q -o <VARIABLE>`. Als u uitschakelen visualisaties wilt zonder het vastleggen van de resultaten (bijvoorbeeld voor het uitvoeren van een SQL-query, zoals een `CREATE TABLE` instructie), gebruik `-q` zonder op te geven een `-o` argument. |
| -m |`-m <METHOD>` |Waar **methode** is **nemen** of **voorbeeld** (standaardwaarde is **nemen**). Als de methode **nemen**, de kernel uitgelicht elementen vanaf de bovenkant van de resultatenset voor gegevens die worden opgegeven door MAXROWS (Zie verderop in deze tabel). Als de methode **voorbeeld**, willekeurig samples elementen van de gegevensset volgens de kernel `-r` parameter hieronder wordt beschreven in deze tabel. |
| -r |`-r <FRACTION>` |Hier **FRACTIE** is een getal met drijvende komma tussen 0,0 en 1,0 liggen. Als de voorbeeldmethode voor de SQL-query is `sample`, en vervolgens de kernel willekeurig voorbeelden voor het opgegeven gedeelte van de elementen van de resultatenset voor u. Bijvoorbeeld, als u een SQL-query uitvoeren met de argumenten `-m sample -r 0.01`, en vervolgens 1% van de Resultatenrijen willekeurig worden vastgelegd. |
| -n |`-n <MAXROWS>` |**MAXROWS** is een geheel getal. De kernel beperkt het aantal rijen dat u uitvoer wilt **MAXROWS**. Als **MAXROWS** is een negatief getal zoals **-1**, en vervolgens het aantal rijen in de resultatenset niet beperkt is. |

**Voorbeeld:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2
    SELECT * FROM hivesampletable

De instructie hiervoor doet het volgende:

* Hiermee selecteert u alle records uit **hivesampletable**.
* Omdat we - q, schakelt automatisch visualisatie.
* Omdat we `-m sample -r 0.1 -n 500` het willekeurig samples 10% van de rijen in de hivesampletable en beperkt de grootte van het resultaat ingesteld op 500 rijen.
* Ten slotte omdat we gebruikt `-o query2` Bovendien bespaart u de uitvoer in een dataframe aangeroepen **query2**.

## <a name="considerations-while-using-the-new-kernels"></a>Overwegingen bij het gebruik van de nieuwe kernels

De kernel dat u gebruikt, verbruikt zodat de laptops met de clusterbronnen.  Met deze kernels omdat de contexten vooraf, worden afgesloten gewoon de laptops de context niet beëindigen en daarom de clusterbronnen blijft in gebruik. Een goede gewoonte is met de **sluiten en stoppen** optie uit de notebook **bestand** menu wanneer u klaar bent met de laptop, waardoor de context is funest en vervolgens de notebook afgesloten.     

## <a name="show-me-some-examples"></a>Sommige voorbeelden weergeven

Als u een Jupyter-notebook opent, ziet u twee mappen die beschikbaar zijn op het hoogste niveau.

* De **PySpark** map bevat voorbeeldquery notitieblokken die gebruikmaken van de nieuwe **Python** kernel.
* De **Scala** map bevat voorbeeldquery notitieblokken die gebruikmaken van de nieuwe **Spark** kernel.

U kunt openen de **00 - [Lees mij eerst] Spark Magic Kernel functies** notebook uit de **PySpark** of **Spark** map voor meer informatie over de andere magics die beschikbaar is. U kunt ook de andere beschikbare voorbeeld-notebooks gebruiken onder de twee mappen voor informatie over het bereiken van verschillende scenario's met Jupyter-notebooks met HDInsight Spark-clusters.

## <a name="where-are-the-notebooks-stored"></a>Waar worden de laptops opgeslagen?

Jupyter-notebooks worden opgeslagen in het opslagaccount die is gekoppeld aan het cluster onder de **/HdiNotebooks** map.  Notitieblokken, bestanden en mappen die u vanuit Jupyter maakt zijn toegankelijk is vanaf de storage-account.  Bijvoorbeeld, als u een map maken met Jupyter **mijnmap** en een laptop **myfolder/mynotebook.ipynb**, u toegang hebt tot die laptop `/HdiNotebooks/myfolder/mynotebook.ipynb` binnen het storage-account.  Het omgekeerde geldt ook, dat wil zeggen, als u een laptop rechtstreeks uploaden naar uw opslagaccount op `/HdiNotebooks/mynotebook1.ipynb`, de laptop is ook van Jupyter zichtbaar.  Laptops blijven in de storage-account, zelfs nadat het cluster wordt verwijderd.

De manier waarop notitieblokken worden opgeslagen in het opslagaccount is compatibel met HDFS. Dus als u SSH in het cluster dat kunt u opdrachten voor het beheer zoals weergegeven in het volgende fragment:

    hdfs dfs -ls /HdiNotebooks                               # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                    # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Als er problemen bij toegang tot het opslagaccount voor het cluster zijn, de laptops worden ook opgeslagen op de headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Ondersteunde browser

Jupyter-notebooks op HDInsight Spark-clusters worden alleen ondersteund op Google Chrome.

## <a name="feedback"></a>Feedback
De nieuwe kernels zijn in de fase in ontwikkeling en zal vervallen gedurende een bepaalde periode. Dit kan ook betekenen dat API's wijzigen kan, omdat deze kernels vervallen. We zouden feedback die u hebt tijdens het gebruik van deze nieuwe kernels stellen. Dit is handig in de definitieve versie van deze kernels vormgeven. U kunt feedback uw opmerkingen/onder de **opmerkingen** sectie onder aan dit artikel.

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
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
