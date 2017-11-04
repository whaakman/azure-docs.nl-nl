---
title: Gebruik R in HDInsight-clusters - Azure aanpassen | Microsoft Docs
description: Informatie over het installeren van R scriptactie met en gebruik R op HDInsight-clusters.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: be851270-afa5-4af0-a69e-2d343a4deeb7
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 666b51970bf04634708cbf65b8bca0c05412934b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>R installeren en gebruiken op HDInsight Hadoop-clusters

Informatie over het aanpassen van Windows op basis van HDInsight-cluster met R met behulp van de scriptactie en het gebruik van R op HDInsight-clusters. De [HDInsight aanbieding](https://azure.microsoft.com/pricing/details/hdinsight/) R Server als onderdeel van uw HDInsight-cluster bevat. Hierdoor kan de R-scripts met MapReduce en Spark gedistribueerde berekeningen uitgevoerd. Zie [Aan de slag met R Server in HDInsight](r-server/r-server-get-started.md) voor meer informatie. Zie voor meer informatie over het gebruik van R met een cluster op basis van Linux [installeert en gebruikt R op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md).

U kunt R installeren op elk type (Hadoop, Storm, HBase, Spark)-cluster in Azure HDInsight met behulp van *scriptactie*. Een voorbeeld van een script R installeren op een HDInsight-cluster is beschikbaar via een alleen-lezen Azure storage-blob op [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

**Verwante artikelen**

* [Installeren en gebruiken van R op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): algemene informatie over het maken van HDInsight-clusters
* [HDInsight-cluster via scriptactie aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van de scriptactie
* [Scriptactie-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md)

## <a name="what-is-r"></a>Wat is R?
De <a href="http://www.r-project.org/" target="_blank">R-Project voor statistische Computing</a> is een open source-taal en de omgeving voor statistische computing. R biedt honderden build in statistische functies en een eigen programmeertaal die aspecten van het functionele en objectgeoriënteerd programmeren combineert. Het bevat ook uitgebreide grafische mogelijkheden. R is de voorkeur programmeeromgeving voor de meeste professionele statistici en verzameld in een groot aantal velden.

R is compatibel met Azure Blob Storage (WASB) zodat er opgeslagen gegevens kunnen worden verwerkt met behulp van R op HDInsight.  

## <a name="install-r"></a>R installeren
Een [voorbeeldscript](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) R installeren op een HDInsight cluster is beschikbaar via een alleen-lezen blob in Azure Storage. Deze sectie bevat instructies over het gebruik van het voorbeeldscript tijdens het maken van het cluster met de Azure-Portal.

> [!NOTE]
> Het voorbeeldscript is geïntroduceerd in HDInsight-cluster versie 3.1. Zie voor meer informatie over de versies van HDInsight-cluster [versies van HDInsight-cluster](hdinsight-component-versioning.md).
>
>

1. Wanneer u een HDInsight-cluster vanuit de Portal maakt, klikt u op **optionele configuratie**, en klik vervolgens op **scriptacties**.
2. Op de **scriptacties** pagina, voer de volgende waarden:

    ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "scriptactie gebruiken voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie, bijvoorbeeld <b>R installeren</b>.</td></tr>
        <tr><td>Script-URI</td>
            <td>Geef de URI naar het script dat wordt opgeroepen voor het aanpassen van het cluster, bijvoorbeeld <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>Soort knooppunt</td>
            <td>Geef op de knooppunten waarop het script aanpassing wordt uitgevoerd. U kunt kiezen <b>alle knooppunten</b>, <b>hoofdknooppunten alleen</b>, of <b>Worker-knooppunten</b> alleen.
        <tr><td>Parameters</td>
            <td>Geef de parameters op, indien vereist door het script. Het script voor het installeren van R is echter geen parameters, vereist zodat u kunt dit leeg laten.</td></tr>
    </table>

    U kunt meer dan één scriptactie meerdere om onderdelen te installeren op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen crating van het cluster.

U kunt het script ook gebruiken voor het installeren van R in HDInsight met behulp van Azure PowerShell of de HDInsight .NET SDK. Instructies voor deze procedures vindt u verderop in dit artikel.

## <a name="run-r-scripts"></a>R-scripts uitvoeren
Deze sectie wordt beschreven hoe een R-script uitvoeren op het Hadoop-cluster met HDInsight.

1. **Extern bureaublad verbinding maken met het cluster**: van de Portal voor extern bureaublad inschakelen voor het cluster dat u hebt gemaakt met R is geïnstalleerd en maak verbinding met het cluster. Zie voor instructies [verbinding maken met HDInsight-clusters met RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).
2. **Open de console R**: het R-installatie plaatst een koppeling naar de R-console op het bureaublad van het hoofdknooppunt. Klik hierop om het R-console te openen.
3. **Het R-script uitvoeren**: het R-script rechtstreeks vanuit de R-console kan worden uitgevoerd door plakken, te selecteren en op ENTER te drukken. Hier volgt een eenvoudig voorbeeld-script dat de getallen van 1 tot 100 genereert en vervolgens wordt vermenigvuldigd met 2.

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

De eerste twee regels Roep de RHadoop-bibliotheken die zijn geïnstalleerd met R. De laatste regel worden de resultaten naar de console afgedrukt. De uitvoer ziet er als volgt:

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## <a name="install-r-using-aure-powershell"></a>R met Aure PowerShell installeren
Zie [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Het voorbeeld laat zien hoe Spark met Azure PowerShell installeren. U moet aanpassen van het script te gebruiken [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1).

## <a name="install-r-using-net-sdk"></a>R met .NET SDK installeren
Zie [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Het voorbeeld laat zien hoe Spark met de .NET SDK installeren. U moet aanpassen van het script te gebruiken [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11).

## <a name="see-also"></a>Zie ook
* [Installeren en gebruiken van R op HDinsight Hadoop-clusters (Linux)](hdinsight-hadoop-r-scripts-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-hadoop-provision-linux-clusters.md): algemene informatie over het maken van HDInsight-clusters
* [HDInsight-cluster via scriptactie aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van de scriptactie
* [Scriptactie-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md)
* [Installeren en gebruiken van Spark in HDInsight-clusters][hdinsight-install-spark]: scriptactie voorbeeld over het installeren van Spark
* [Giraph installeren op HDInsight-clusters](hdinsight-hadoop-giraph-install.md): scriptactie voorbeeld over het installeren van Giraph
* [Solr installeren op HDInsight-clusters](hdinsight-hadoop-solr-install-linux.md): scriptactie voorbeeld over het installeren van Solr.

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]:spark/apache-spark-jupyter-spark-sql.md
