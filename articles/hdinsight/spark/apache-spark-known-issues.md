---
title: Problemen oplossen met Apache Spark-cluster in Azure HDInsight | Microsoft Docs
description: Meer informatie over problemen met Apache Spark-clusters in Azure HDInsight en hoe deze te omzeilen.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: bb5557eb0672b9ad137bc5817e47bf4f89e1c34d
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>Bekende problemen van Apache Spark-cluster in HDInsight

Dit document houdt van alle bekende problemen voor de openbare preview van HDInsight Spark.  

## <a name="livy-leaks-interactive-session"></a>Livy lekt interactieve sessies
Wanneer Livy opnieuw wordt gestart (van Ambari of vanwege headnode 0 virtuele machine opnieuw opstarten) met een interactieve sessie nog steeds actief is, wordt er een sessie interactieve taak gelekt. Als gevolg hiervan worden nieuwe taken kunnen vastgelopen in de status goedgekeurd en kunnen niet worden gestart.

**Risicobeperking:**

Gebruik de volgende procedure het probleem oplossen:

1. SSH in headnode. Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

2. Voer de volgende opdracht om de toepassing-id's van de interactieve taken gestart via Livy vinden. 
   
        yarn application –list
   
    De standaard-taak namen Livy worden als de taken zijn gestart met een interactieve sessie Livy met geen expliciete namen opgegeven voor de Livy-sessie gestart met Jupyter-notebook, de taaknaam van de zal starten met remotesparkmagics_ *. 
3. Voer de volgende opdracht om deze taken af te sluiten. 
   
        yarn application –kill <Application ID>

Nieuwe taken worden gestart met. 

## <a name="spark-history-server-not-started"></a>Spark geschiedenis Server is niet gestart
Spark geschiedenis Server is niet automatisch gestart nadat een cluster is gemaakt.  

**Risicobeperking:** 

De geschiedenis-server handmatig starten vanaf Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Machtiging probleem in de logboekmap Spark
Wanneer hdiuser een taak met spark-submit verzendt, er is een fout java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (toestemming geweigerd) en het Stuurprogrammalogboek is niet geschreven. 

**Risicobeperking:**

1. Hdiuser toevoegen aan de Hadoop-groep. 
2. Geef een 777 machtigingen op /var/log/spark na het maken van het cluster. 
3. De locatie van het logboekbestand spark met Ambari moet een map met 777 machtigingen bijwerken.  
4. Voer spark-indienen als sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Spark Phoenix connector wordt niet ondersteund

De connector Spark Phoenix wordt momenteel niet ondersteund met een HDInsight Spark-cluster.

**Risicobeperking:**

In plaats daarvan moet u de Spark-HBase-connector gebruiken. Zie voor instructies [Spark-HBase-connector gebruiken](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-to-jupyter-notebooks"></a>Problemen met Jupyter-notebooks
Hier volgen enkele bekende problemen die betrekking hebben op Jupyter-notebooks.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Laptops met niet-ASCII-tekens in bestandsnamen
Jupyter-notebooks die kunnen worden gebruikt in HDInsight Spark-clusters mag geen niet-ASCII-tekens in bestandsnamen. Als u probeert een bestand via de UI Jupyter een niet-ASCII-bestandsnaam heeft te uploaden, zal mislukken achtergrond (dat wil zeggen, Jupyter kunt u het bestand uploadt, maar deze won't ofwel een zichtbaar fout genereert). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fout tijdens het laden van notitieblokken van grotere
U ziet mogelijk een fout  **`Error loading notebook`**  wanneer het laden van laptops die groter zijn.  

**Risicobeperking:**

Als u deze fout krijgt, betekent niet dat uw gegevens is beschadigd of verloren.  Uw laptops zich nog steeds op schijf in `/var/lib/jupyter`, en u kunt SSH in het cluster toegang toe hebben. Zie [SSH-sleutels gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

Wanneer u verbinding hebt gemaakt met het cluster via SSH, kunt u uw notitieblokken kopiëren van het cluster op uw lokale computer (via SCP of WinSCP) als een back-up van alle belangrijke gegevens in de notebook gegevensverlies te voorkomen. U kunt vervolgens SSH-tunnel in uw headnode op poort 8001 voor toegang tot Jupyter zonder tussenkomst van de gateway.  U kunt daar schakelt u de uitvoer van uw laptop en sla opnieuw op om de grootte van de notebook te minimaliseren.

Om te voorkomen dat deze fout in de toekomst, moet u enkele aanbevolen procedures volgen:

* Het is belangrijk klein te houden de notebook-grootte. Elke uitvoer van uw Spark-taken die worden verzonden naar Jupyter wordt bewaard in de notebook.  Het is een best practice met Jupyter in het algemeen om te voorkomen dat actief `.collect()` op grote RDD of dataframes; in plaats daarvan als u bekijken van een RDD inhoud wilt, kunt u overwegen uitgevoerd `.take()` of `.sample()` zodat de uitvoer te groot niet ophalen.
* Ook als u een laptop opslaat, uitvoer Wis alle cellen wilt verkleinen.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Laptop opstarten duurt langer dan verwacht
Eerste code-instructie in Jupyter-notebook met behulp van Spark magic kan meer dan een minuut duren.  

**Uitleg:**

Dit gebeurt omdat wanneer de eerste codecel wordt uitgevoerd. Op de achtergrond Hiermee initieert u sessieconfiguratie en Spark, SQL en Hive-contexten worden ingesteld. Nadat deze contexten zijn ingesteld, wordt de eerste instructie is uitgevoerd en dit de indruk dat geeft duurde de instructie lang om te voltooien.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter-notebook time-out bij het maken van de sessie
Wanneer het Spark-cluster heeft onvoldoende resources, wordt de Spark en Pyspark kernels in de Jupyter-notebook time-out bij het maken van de sessie. 

**Oplossingen:** 

1. Sommige resources in uw Spark-cluster door vrijmaken:
   
   * Andere laptops Spark gestopt door te gaan naar het menu sluit en stilstand of afsluiten in de Verkenner notebook te klikken.
   * Andere toepassingen Spark YARN wordt gestopt.
2. Start opnieuw op de notebook die u probeert te starten. Onvoldoende bronnen moeten beschikbaar zijn voor u nu een sessie te maken.

## <a name="see-also"></a>Zie ook
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
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

