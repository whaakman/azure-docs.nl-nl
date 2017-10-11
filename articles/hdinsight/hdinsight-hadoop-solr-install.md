---
title: Installeert met een scriptactie Solr op Hadoop-cluster - Azure | Microsoft Docs
description: Informatie over het aanpassen van HDInsight-cluster met Solr met behulp van de scriptactie.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 6efb7ea26c3cdf7748fff4b02b5810c85cc41e1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a>Installeren en gebruiken van Solr op Windows gebaseerde HDInsight-clusters

Informatie over het aanpassen van HDInsight op basis van Windows-cluster met Solr met behulp van de scriptactie en Solr gebruiken om te zoeken van gegevens.

> [!IMPORTANT]
> De stappen in dit document wordt alleen werken met HDInsight op basis van Windows-clusters. HDInsight is alleen beschikbaar in Windows voor versies lager is dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over het gebruik van Solr met een cluster op basis van Linux [installeert en gebruikt Solr op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).


U kunt Solr installeren op elk type (Hadoop, Storm, HBase, Spark)-cluster in Azure HDInsight met behulp van *scriptactie*. Een voorbeeld van een script Solr installeren op een HDInsight-cluster is beschikbaar via een alleen-lezen Azure storage-blob op [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

Het voorbeeldscript werkt alleen met HDInsight-cluster versie 3.1. Zie voor meer informatie over de versies van HDInsight-cluster, [versies van HDInsight-cluster](hdinsight-component-versioning.md).

Het voorbeeldscript gebruikt in dit onderwerp maakt een Solr op basis van Windows-cluster met een specifieke configuratie. Als u wilt configureren van het cluster Solr met verschillende verzamelingen, shards, schema's, replica's, enz., moet u het script en de binaire bestanden Solr dienovereenkomstig wijzigen.

**Verwante artikelen**

* [Installeren en gebruiken van Solr op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster via scriptactie aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van de scriptactie.
* [Scriptactie-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-solr"></a>Wat is Solr?
<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is een platform voor het zoeken van enterprise waarmee krachtige zoekopdracht in volledige tekst van gegevens. Hadoop kunt opslaan en beheren van de enorme hoeveelheden gegevens, biedt Apache Solr de zoekmogelijkheden snel gegevens ophalen.

## <a name="install-solr-using-portal"></a>Solr met portal installeren
1. Beginnen met het maken van een cluster met behulp van de **aangepast maken** optie, zoals beschreven op [maken Hadoop-clusters in HDInsight](hdinsight-provision-clusters.md).
2. Op de **scriptacties** pagina van de wizard, klikt u op **scriptactie toevoegen** om details te verstrekken over de scriptactie, zoals hieronder wordt weergegeven:

    ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "scriptactie gebruiken voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie. Bijvoorbeeld: <b>installeren Solr</b>.</td></tr>
        <tr><td>Script-URI</td>
            <td>Geef de URI Uniform Resource Identifier () naar het script dat wordt opgeroepen voor het aanpassen van het cluster. Bijvoorbeeld: <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Soort knooppunt</td>
            <td>Geef op de knooppunten waarop het script aanpassing wordt uitgevoerd. U kunt kiezen <b>alle knooppunten</b>, <b>hoofdknooppunten alleen</b>, of <b>Worker-knooppunten</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters op, indien vereist door het script. Het script voor het installeren van Solr vereist geen parameters, zodat u kunt dit leeg laten.</td></tr>
    </table>

    U kunt meer dan één scriptactie meerdere om onderdelen te installeren op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het maken van het cluster.

## <a name="use-solr"></a>Solr gebruiken
U moet beginnen met het indexeren Solr met enkele gegevensbestanden. U kunt vervolgens Solr zoekquery's uitvoeren op de geïndexeerde gegevens. Voer de volgende stappen voor het gebruik van Solr in een HDInsight-cluster:

1. **Remote Desktop Protocol (RDP) naar extern gebruik in het HDInsight-cluster met Solr geïnstalleerd**. Vanuit de Azure-portal, moet u extern bureaublad inschakelen voor het cluster dat u hebt gemaakt met Solr geïnstalleerd en vervolgens extern in het cluster. Zie voor instructies [verbinding maken met HDInsight-clusters met RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Index Solr door gegevensbestanden uploaden**. Wanneer u de index Solr, plaatst u documenten in die u zoeken wilt op. Om te indexeren Solr, RDP gebruikt om externe bij het cluster, gaat u naar het bureaublad, open de Hadoop-opdrachtregel en navigeer naar **C:\apps\dist\solr-4.7.2\example\exampledocs**. Voer de volgende opdracht uit:

        java -jar post.jar solr.xml monitor.xml

    Op de console ziet u de volgende uitvoer:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Het hulpprogramma post.jar Solr met twee voorbeelddocumenten indexeert **solr.xml** en **monitor.xml**. Het hulpprogramma post.jar en de voorbeelddocumenten zijn beschikbaar met Solr-installatie.
3. **Het dashboard Solr gebruiken om te zoeken binnen de geïndexeerde documenten**. In de RDP-sessie met de HDInsight-cluster, opent u Internet Explorer en start het dashboard Solr op **solr-http://headnodehost:8983 / #/**. In het linkerdeelvenster van de **Core Selector** vervolgkeuzelijst, selecteer **collection1**, en binnen die, op **Query**. Als u bijvoorbeeld om te selecteren en retourneren van alle documenten in Solr, bieden de volgende waarden:

   * In de **q** tekst Voer  **\*:**\*. Hiermee wordt de documenten die zijn geïndexeerd in Solr retourneren. Als u zoeken naar een specifieke tekenreeks binnen de documenten wilt, kunt u deze tekenreeks hier invoeren.
   * In de **wt** tekst Selecteer indeling van de uitvoer. Standaard is **json**. Klik op **-Query uitvoeren**.

     ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "een query uitvoeren op Solr dashboard")

     De uitvoer retourneert de twee documenten die we voor indexeren Solr gebruikt. De uitvoer lijkt op het volgende:

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }
4. **Aanbevolen: Back-up van de geïndexeerde gegevens van Solr naar Azure Blob-opslag die is gekoppeld aan het HDInsight-cluster**. Als een goede gewoonte back u-up de geïndexeerde gegevens van de clusterknooppunten Solr naar Azure Blob-opslag. Voer de volgende stappen uit om dit te doen:

   1. Open Internet Explorer in de RDP-sessie en wijst u de volgende URL:

           http://localhost:8983/solr/replication?command=backup

       U ziet een antwoord als volgt:

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. Navigeer in de externe sessie naar {SOLR_HOME}\{verzameling} \data. Voor het cluster is gemaakt via het voorbeeldscript, moet dit **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**. Op deze locatie ziet u een momentopnamemap is gemaakt met een naam die lijkt op  **momentopname.* tijdstempel***.
   3. De snapshotmap ZIP en dit uploaden naar Azure Blob-opslag. Navigeer naar de locatie van de momentopnamemap met de volgende opdracht vanaf de opdrachtregel Hadoop:

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       Met deze opdracht wordt de momentopname gekopieerd naar /example/data/onder de container binnen het standaard opslagaccount die is gekoppeld aan het cluster.

## <a name="install-solr-using-aure-powershell"></a>Solr met Aure PowerShell installeren
Zie [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Het voorbeeld laat zien hoe Spark met Azure PowerShell installeren. U moet aanpassen van het script te gebruiken [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Solr met .NET SDK installeren
Zie [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Het voorbeeld laat zien hoe Spark met de .NET SDK installeren. U moet aanpassen van het script te gebruiken [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="see-also"></a>Zie ook
* [Installeren en gebruiken van Solr op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster via scriptactie aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van de scriptactie.
* [Scriptactie-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).
* [Installeren en gebruiken van Spark in HDInsight-clusters][hdinsight-install-spark]: scriptactie voorbeeld over het installeren van Spark.
* [R installeren op HDInsight-clusters][hdinsight-install-r]: scriptactie voorbeeld over het installeren van R.
* [Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install.md): scriptactie voorbeeld over het installeren van Giraph.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
