---
title: Gebruik scriptacties Solr installeren op Hadoop-cluster - Azure
description: Informatie over het aanpassen van HDInsight-cluster met Solr met behulp van scriptacties.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 6a3f9928fa685c36a0495ba5d423c3be7bff2bea
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51009158"
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a>Solr installeren en gebruiken op Windows gebaseerde HDInsight-clusters

Meer informatie over het aanpassen van HDInsight op basis van een Windows-cluster met Solr met Script Action en hoe u Solr gebruiken om te zoeken naar gegevens.

> [!IMPORTANT]
> De stappen in dit document werken alleen met HDInsight op basis van een Windows-clusters. HDInsight is alleen beschikbaar voor Windows voor versies lager dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over het gebruik van Solr met een cluster op basis van Linux [installeren en gebruiken Solr op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).


U Solr kunt installeren op elk type cluster (Hadoop, Storm, HBase, Spark) op Azure HDInsight met behulp van *scriptactie*. Een voorbeeld van een script Solr installeren op een HDInsight-cluster is beschikbaar via een alleen-lezen Azure storage-blob op [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

Het voorbeeldscript werkt alleen met HDInsight-clusterversie 3.1. Zie voor meer informatie over HDInsight-clusterversies [HDInsight-clusterversies](hdinsight-component-versioning.md).

Het voorbeeld van een script gebruikt in dit onderwerp wordt een Solr op basis van een Windows-cluster gemaakt met een specifieke configuratie. Als u configureren van het cluster Solr met verschillende verzamelingen, shards, schema's, replica's, enzovoort wilt, moet u het script en de binaire bestanden Solr dienovereenkomstig wijzigen.

**Gerelateerde artikelen**

* [Solr installeren en gebruiken op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster met Script Action aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van scriptacties.
* [Script Action-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-solr"></a>Wat is Solr?
<a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is een platform voor enterprise search waarmee krachtige zoeken in volledige tekst van gegevens. Hoewel Hadoop kunt opslaan en beheren van grote hoeveelheden gegevens, biedt Apache Solr de zoekfuncties voor het snel de gegevens worden opgehaald.

## <a name="install-solr-using-portal"></a>Solr met behulp van portal installeren
1. Beginnen met het maken van een cluster met behulp van de **aangepast maken** optie, zoals beschreven op [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md).
2. Op de **scriptacties** pagina van de wizard, klikt u op **scriptactie toevoegen** voor meer informatie over de scriptactie, zoals hieronder wordt weergegeven:

    ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "scriptactie gebruiken voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie. Bijvoorbeeld, <b>Solr installeren</b>.</td></tr>
        <tr><td>Script-URI</td>
            <td>Geef de Uniform Resource Identifier (URI) naar het script dat wordt aangeroepen voor het aanpassen van het cluster. Bijvoorbeeld: <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></td></tr>
        <tr><td>Soort knooppunt</td>
            <td>Geef op de knooppunten waarop de aanpassing-script wordt uitgevoerd. U kunt ervoor kiezen <b>alle knooppunten</b>, <b>hoofdknooppunten alleen</b>, of <b>Worker-knooppunten</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters op, indien vereist door het script. Solr installeren met het script is niet vereist voor alle parameters, zodat u kunt dit leeg laten.</td></tr>
    </table>

    U kunt meer dan één scriptactie voor het installeren van meerdere onderdelen op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het maken van het cluster.

## <a name="use-solr"></a>Solr gebruiken
U moet beginnen met Solr indexeren met enkele gegevensbestanden. U kunt vervolgens Solr zoekquery's uitvoeren op de geïndexeerde gegevens. Voer de volgende stappen uit voor het gebruik van Solr in een HDInsight-cluster:

1. **Remote Desktop Protocol (RDP) op afstand gebruiken in het HDInsight-cluster met Solr geïnstalleerd**. Vanuit de Azure-portal, moet u extern bureaublad inschakelen voor het cluster dat u hebt gemaakt met Solr geïnstalleerd en vervolgens extern in het cluster. Zie voor instructies [verbinding maken met HDInsight-clusters met behulp van RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Index Solr door gegevensbestanden te uploaden**. Wanneer u Solr indexeren, plaatst u documenten in die u wilt zoeken. Om te indexeren Solr, in het cluster extern bureaublad op afstand gebruiken, gaat u naar het bureaublad, opent u de Hadoop-opdrachtregel en navigeer naar **C:\apps\dist\solr-4.7.2\example\exampledocs**. Voer de volgende opdracht uit:

        java -jar post.jar solr.xml monitor.xml

    In de console ziet u de volgende uitvoer:

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    Het hulpprogramma post.jar Solr met twee voorbeelddocumenten indexeert **solr.xml** en **monitor.xml**. Het hulpprogramma post.jar en de voorbeelddocumenten zijn beschikbaar met Solr installatie.
3. **Het dashboard Solr gebruiken voor het zoeken in de geïndexeerde documenten**. Open Internet Explorer in het RDP-sessie met het HDInsight-cluster, en start het dashboard Solr op **http://headnodehost:8983/solr/#/**. In het linkerdeelvenster van de **Core Selector** vervolgkeuzelijst, selecteer **verzameling1**, en klik in dat op **Query**. Geef de volgende waarden om te selecteren en alle documenten in Solr retourneren, bijvoorbeeld:

   * In de **q** tekst Voer  **\*:**\*. Alle documenten die zijn geïndexeerd wordt in Solr geretourneerd. Als u zoeken naar een specifieke tekenreeks in de documenten wilt, kunt u deze tekenreeks hier invoeren.
   * In de **wt** tekst Selecteer indeling van de uitvoer. De standaardwaarde is **json**. Klik op **uitvoeren van query's**.

     ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "een query uitvoeren op Solr dashboard")

     De uitvoer geeft als resultaat van de twee documenten die we voor indexering Solr gebruikt. De uitvoer lijkt op het volgende:

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
4. **Aanbevolen: Back-up van de geïndexeerde gegevens uit Solr naar Azure Blob-opslag die is gekoppeld aan het HDInsight-cluster**. Als een goede gewoonte, moet u back-up van de geïndexeerde gegevens van de clusterknooppunten Solr naar Azure Blob-opslag. Voer de volgende stappen uit om dit te doen:

   1. De RDP-sessie, opent u Internet Explorer en verwijzen naar de volgende URL:

           http://localhost:8983/solr/replication?command=backup

       Hier ziet u een reactie als volgt:
            
      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
          <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
            <str name="status">OK</str>
          </response>
      ```
      
   2. Navigeer in de externe sessie naar {SOLR_HOME}\{verzameling} \data. Voor het cluster is gemaakt via het voorbeeld van een script, moet dit `C:\apps\dist\solr-4.7.2\example\solr\collection1\data`. Op deze locatie ziet u een map met momentopnamen die zijn gemaakt met een naam die lijkt op **momentopname.* tijdstempel ***.
   
   3. ZIP-de map met momentopnamen en dit uploaden naar Azure Blob-opslag. Navigeer naar de locatie van de map met momentopnamen met behulp van de volgende opdracht uit vanaf de opdrachtregel Hadoop:

      ```
      hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data
      ```

   Deze opdracht wordt de momentopname naar /example/data/onder de container in het standaard opslagaccount dat is gekoppeld aan het cluster.

## <a name="install-solr-using-aure-powershell"></a>Met behulp van Aure PowerShell Solr installeren
Zie [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Het voorbeeld ziet u hoe u Spark met behulp van Azure PowerShell installeren. U moet het script te gebruiken aanpassen [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="install-solr-using-net-sdk"></a>Solr met .NET SDK installeren
Zie [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Het voorbeeld ziet u hoe u Spark met behulp van de .NET SDK installeren. U moet het script te gebruiken aanpassen [ https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).

## <a name="see-also"></a>Zie ook
* [Solr installeren en gebruiken op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-solr-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster met Script Action aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van scriptacties.
* [Script Action-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).
* [Installeren en gebruiken van Spark op HDInsight-clusters][hdinsight-install-spark]: Script Action-voorbeeld over het installeren van Spark.
* [Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install.md): Script Action-voorbeeld over het installeren van Giraph.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
