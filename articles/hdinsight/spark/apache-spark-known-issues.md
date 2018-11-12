---
title: Problemen oplossen met Apache Spark-cluster in Azure HDInsight
description: Meer informatie over problemen met Apache Spark-clusters in Azure HDInsight en hoe u die verhelpen.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: c364f9d06d29a601dfb9598bb568e7a6218d0a6f
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51013799"
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>Bekende problemen voor Apache Spark-cluster in HDInsight

Dit document houdt van alle bekende problemen voor de openbare preview van HDInsight Spark.  

## <a name="livy-leaks-interactive-session"></a>Interactieve sessie wordt gelekt Livy
Wanneer Livy opnieuw wordt opgestart (van Ambari of vanwege hoofdknooppunt 0 virtuele machine opnieuw opstarten) met een interactieve sessie nog steeds actief, wordt de sessie van een interactieve taak gelekt. Als gevolg hiervan nieuwe taken kunnen blijven steken bij de status goedgekeurd.

**Risicobeperking:**

Gebruik de volgende procedure om het probleem te omzeilen:

1. SSH naar het hoofdknooppunt. Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Voer de volgende opdracht te vinden van de toepassing-id's van de interactieve taken via Livy is gestart. 
   
        yarn application –list
   
    De namen van de taak standaard worden Livy als de taken zijn gestart met een interactieve sessie Livy met geen expliciete namen opgegeven. Voor de Livy-sessie gestart met Jupyter-notebook, begint de naam van de taak met remotesparkmagics_ *. 
3. Voer de volgende opdracht om deze taken af te sluiten. 
   
        yarn application –kill <Application ID>

Nieuwe taken gestart. 

## <a name="spark-history-server-not-started"></a>Spark geschiedenis Server is niet gestart
Spark geschiedenis-Server wordt niet automatisch gestart nadat een cluster is gemaakt.  

**Risicobeperking:** 

Handmatig starten van de server van de geschiedenis van Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Machtiging probleem in de logboekmap Spark
hdiuser haalt de volgende fout bij het spark-submit indienen van een taak met:

```
java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (Permission denied)
```
En er zijn geen stuurprogramma logboekvermelding wordt geschreven. 

**Risicobeperking:**

1. Hdiuser toevoegen aan de Hadoop-groep. 
2. Geef 777 machtigingen op /var/log/spark na het maken van het cluster. 
3. De locatie van het spark-logboek met Ambari moet een map met 777 machtigingen bijwerken.  
4. Voer spark-submit als sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Spark-Phoenix-connector wordt niet ondersteund

HDInsight Spark-clusters bieden geen ondersteuning voor de Spark-Phoenix-connector.

**Risicobeperking:**

In plaats daarvan moet u de Spark-HBase-connector gebruiken. Zie voor instructies [over het gebruik van Spark-HBase-connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-to-jupyter-notebooks"></a>Problemen met de Jupyter-notebooks
Hieronder vindt u enkele bekende problemen met betrekking tot de Jupyter-notebooks.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>-Notebooks met niet-ASCII-tekens in bestandsnamen
Gebruik geen niet-ASCII-tekens in Jupyter notebook bestandsnamen. Als u probeert te uploaden van een bestand via de UI Jupyter, een niet-ASCII-bestandsnaam heeft, mislukt het zonder een foutmelding krijgt. Jupyter kiest u het bestand te uploaden, maar heeft een fout weergegeven ofwel genereren.

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fout tijdens het laden van notitieblokken van grotere
U ziet mogelijk een fout **`Error loading notebook`** bij het verdelen van notitieblokken die groter zijn.  

**Risicobeperking:**

Als u deze fout optreedt, betekent niet dat uw gegevens is beschadigd of verloren gaan.  Uw laptops zijn nog steeds op schijf in `/var/lib/jupyter`, en kunt u SSH in het cluster toegang tot deze. Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

Nadat u verbinding hebt gemaakt met het cluster via SSH, kunt u uw notitieblokken kopiëren van het cluster naar uw lokale computer (met SCP of WinSCP) als een back-up om te voorkomen dat het verlies van gegevens van alle belangrijke gegevens in het notitieblok. Vervolgens kunt u SSH-tunnel naar het hoofdknooppunt op poort 8001 voor toegang tot Jupyter zonder tussenkomst van de gateway.  U kunt daar, schakelt u de uitvoer van uw laptop en sla opnieuw op om de grootte van het notitieblok te minimaliseren.

Om te voorkomen dat deze fout in de toekomst, moet u enkele aanbevolen procedures te volgen:

* Het is belangrijk dat u de grootte van de notebook klein te houden. Elke uitvoer van uw Spark-taken die worden verzonden naar Jupyter worden persistent gemaakt in het notitieblok.  Het is een best practice met Jupyter in het algemeen om te voorkomen dat wordt uitgevoerd `.collect()` op grote RDD of dataframes; in plaats daarvan, als u bekijken van de inhoud van een RDD wilt, kunt u actief `.take()` of `.sample()` zodat de uitvoer niet te groot.
* Ook als u een notitieblok opslaat, uitvoer Schakel alle cellen Verklein de grootte.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Eerste keer opstarten notebook duurt langer dan verwacht
Eerste instructie van de code in Jupyter-notebook met behulp van Magic-pakket voor Spark kan meer dan een minuut duren.  

**Uitleg:**

Dit gebeurt omdat wanneer de eerste codecel wordt uitgevoerd. Op de achtergrond Hiermee initieert sessieconfiguratie en Spark, SQL en Hive-contexten worden ingesteld. Nadat deze contexten zijn ingesteld, wordt de eerste instructie is uitgevoerd en dit de indruk dat geeft duurde de instructie het lang om te voltooien.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter notebook time-out bij het maken van de sessie
Wanneer de Spark-cluster heeft onvoldoende resources, wordt de Spark- en PySpark-kernels in de Jupyter-notebook time-out bij het maken van de sessie. 

**Oplossingen:** 

1. Sommige resources in uw Spark-cluster door vrij:
   
   * Het stoppen van andere Spark-laptops door te gaan naar het menu sluiten en stoppen of afsluiten in de notebook-Verkenner te klikken.
   * Het stoppen van andere toepassingen Spark van YARN.
2. Start opnieuw op de laptop die waarmee u probeert is om opnieuw te starten. Onvoldoende bronnen moeten beschikbaar zijn voor u om nu een sessie te maken.

## <a name="see-also"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

