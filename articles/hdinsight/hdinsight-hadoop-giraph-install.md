---
title: Installeren en gebruiken van Giraph op Hadoop-clusters in HDInsight - Azure | Microsoft Docs
description: Informatie over het aanpassen van HDInsight-cluster met Giraph en het gebruik van Giraph.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 77a1d0e0-55de-4e61-98a0-060914fb7ca0
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: f0eb5c1f457380600463a370043f03e6d655a02c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a>Installeren en gebruiken van Giraph op Windows gebaseerde HDInsight-clusters

Informatie over het aanpassen van Windows gebaseerde HDInsight-cluster met Giraph met behulp van de scriptactie en Giraph gebruiken voor het verwerken van grootschalige grafieken. Zie voor meer informatie over het gebruik van Giraph met een cluster op basis van Linux [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]
> De stappen in dit document wordt alleen werken met HDInsight op basis van Windows-clusters. HDInsight is alleen beschikbaar in Windows voor versies lager is dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over het installeren van Giraph op een Linux gebaseerde HDInsight-cluster [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).


U kunt Giraph installeren op elk type (Hadoop, Storm, HBase, Spark)-cluster in Azure HDInsight met behulp van *scriptactie*. Een voorbeeld van een script Giraph installeren op een HDInsight-cluster is beschikbaar via een alleen-lezen Azure storage-blob op [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Het voorbeeldscript werkt alleen met HDInsight-cluster versie 3.1. Zie voor meer informatie over de versies van HDInsight-cluster, [versies van HDInsight-cluster](hdinsight-component-versioning.md).

**Verwante artikelen**

* [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster via scriptactie aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van de scriptactie.
* [Scriptactie-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Wat is Giraph?
<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> Hiermee kunt u grafiek verwerken met behulp van Hadoop en kan worden gebruikt met Azure HDInsight. Grafieken model relaties tussen de objecten, zoals de verbindingen tussen routers op een grote netwerk, zoals Internet of relaties tussen personen op sociale netwerken (ook wel aangeduid als een sociale grafiek). Grafiek verwerking kunt u tot reden van de relaties tussen de objecten in een grafiek, zoals:

* Identificeren van mogelijke vrienden op basis van uw huidige relaties.
* Identificeren van de kortste afstand tussen twee computers in een netwerk.
* Berekenen van de positie van de pagina van webpagina's.

## <a name="install-giraph-using-portal"></a>Giraph met portal installeren
1. Beginnen met het maken van een cluster met behulp van de **aangepast maken** optie, zoals beschreven op [maken Hadoop-clusters in HDInsight](hdinsight-provision-clusters.md).
2. Op de **scriptacties** pagina van de wizard, klikt u op **scriptactie toevoegen** om details te verstrekken over de scriptactie, zoals hieronder wordt weergegeven:

    ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "scriptactie gebruiken voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie. Bijvoorbeeld: <b>installeren Giraph</b>.</td></tr>
        <tr><td>Script-URI</td>
            <td>Geef de URI Uniform Resource Identifier () naar het script dat wordt opgeroepen voor het aanpassen van het cluster. Bijvoorbeeld: <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Soort knooppunt</td>
            <td>Geef op de knooppunten waarop het script aanpassing wordt uitgevoerd. U kunt kiezen <b>alle knooppunten</b>, <b>hoofdknooppunten alleen</b>, of <b>Worker-knooppunten</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters op, indien vereist door het script. Het script voor het installeren van Giraph vereist geen parameters, zodat u kunt dit leeg laten.</td></tr>
    </table>

    U kunt meer dan één scriptactie meerdere om onderdelen te installeren op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het maken van het cluster.

## <a name="use-giraph"></a>Giraph gebruiken
We gebruiken de SimpleShortestPathsComputation-voorbeeld ter illustratie van het basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementatie voor het vinden van het kortste pad tussen de objecten in een grafiek. Gebruik de volgende stappen om een taak uitvoert met behulp van het voorbeeld SimpleShortestPathsComputation te upload de voorbeeldgegevens en de jar voorbeeld en bekijk vervolgens de resultaten.

1. Een Voorbeeldgegevensbestand uploaden naar Azure Blob-opslag. Maak een nieuw bestand met de naam op uw lokale werkstation **tiny_graph.txt**. Bevatten moeten de volgende regels:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Upload het bestand tiny_graph.txt naar de primaire opslag voor uw HDInsight-cluster. Zie voor instructies over het uploaden van gegevens [gegevens voor Hadoop-taken in HDInsight uploaden](hdinsight-upload-data.md).

    Deze gegevens worden beschreven van een relatie tussen de objecten in een gerichte grafiek, via de indeling [bron\_-id, de bron\_waarde [[dest\_id], [rand\_waarde],...]]. Elke regel vertegenwoordigt een relatie tussen een **bron\_id** object en een of meer **dest\_id** objecten. De **rand\_waarde** (of gewicht) kunnen worden beschouwd als de sterkte of de afstand van de verbinding tussen **source_id** en **dest\_id**.

    Getekend, en met de waarde (gewicht) als de afstand tussen de objecten, de bovenstaande gegevens als volgt uitzien:

    ![tiny_graph.txt getekend als cirkels met verschillende afstand tussen regels](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. Voer in het voorbeeld SimpleShortestPathsComputation. De volgende Azure PowerShell-cmdlets gebruiken om het voorbeeld uitvoert met behulp van het bestand tiny_graph.txt als invoer.

    > [!IMPORTANT]
    > Azure PowerShell-ondersteuning voor het beheer van HDInsight-resources met behulp van Azure Service Manager is **afgeschaft** en per 1 januari 2017 verdwenen. In de stappen in dit document worden de nieuwe HDInsight-cmdlets gebruikt die met Azure Resource Manager werken.
    >
    > Volg de stappen in [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) als u de nieuwste versie van Azure PowerShell wilt installeren. Als u scripts hebt die moeten worden gewijzigd om ze te kunnen gebruiken met de nieuwe cmdlets die werken met Azure Resource Manager, raadpleegt u [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Migreren naar op Azure Resource Manager gebaseerde hulpprogramma’s voor HDInsight-clusters) voor meer informatie.

    ```powershell
    $clusterName = "clustername"
    # Giraph examples jar
    $jarFile = "wasb:///example/jars/giraph-examples.jar"
    # Arguments for this job
    $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                    "-ca", "mapred.job.tracker=headnodehost:9010",
                    "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                    "-vip", "wasb:///example/data/tiny_graph.txt",
                    "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                    "-op",  "wasb:///example/output/shortestpaths",
                    "-w", "2"
    # Create the definition
    $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
        -JarFile $jarFile
        -ClassName "org.apache.giraph.GiraphRunner"
        -Arguments $jobArguments

    # Run the job, write output to the Azure PowerShell window
    $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureHDInsightJob -Job $job
    Write-Host "STDERR"
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput
    ```

    Vervang in het bovenstaande voorbeeld **clustername** met de naam van uw HDInsight-cluster met Giraph geïnstalleerd.
3. Bekijk de resultaten. Zodra de taak is voltooid, de resultaten worden opgeslagen in twee uitvoerbestanden in de **wasb: / / / voorbeeld/out/shotestpaths** map. De bestanden worden genoemd **onderdeel-m-00001** en **onderdeel-m-00002**. Voer de volgende stappen uit om te downloaden en de uitvoer weergeven:

    ```powershell
    $subscriptionName = "<SubscriptionName>"       # Azure subscription name
    $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
    $containerName = "<ContainerName>"             # Blob storage container name

    # Select the current subscription
    Select-AzureSubscription $subscriptionName

    # Create the Storage account context object
    $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Download the job output to the workstation
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
    Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force
    ```

    Hiermee maakt u de **uitvoer-voorbeeld/shortestpaths** directorystructuur in de huidige map op uw werkstation en het downloaden van de twee uitvoerbestanden naar die locatie.

    Gebruik de **Cat** cmdlet om de inhoud van de bestanden weer te geven:

        Cat example/output/shortestpaths/part*

    De uitvoer ziet er ongeveer als volgt:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    De SimpleShortestPathComputation voorbeeld is harde code vastgelegd beginnen met object-ID 1 en de kortste route naar andere objecten. Zodat de uitvoer moet worden gelezen als `destination_id distance`, waarbij afstand waarde (of gewicht) van de randen afgelegd tussen object-ID 1 en de doel-ID.

    Dit visualiseren, kunt u de resultaten controleren door de kortste paden onderweg tussen ID 1 en alle andere objecten. Houd er rekening mee dat het kortste pad tussen 1-ID en -ID 4, 5 is. Dit is de totale afstand tussen <span style="color:orange">ID 1 en 3</span>, en vervolgens <span style="color:red">ID 3 en 4</span>.

    ![Tekenen van objecten als cirkels met de kortste paden tussen getekend](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Giraph met Aure PowerShell installeren
Zie [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Het voorbeeld laat zien hoe Spark met Azure PowerShell installeren. U moet aanpassen van het script te gebruiken [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>Giraph met .NET SDK installeren
Zie [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Het voorbeeld laat zien hoe Spark met de .NET SDK installeren. U moet aanpassen van het script te gebruiken [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="see-also"></a>Zie ook
* [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster via scriptactie aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van de scriptactie.
* [Scriptactie-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).
* [Installeren en gebruiken van Spark in HDInsight-clusters][hdinsight-install-spark]: scriptactie voorbeeld over het installeren van Spark.
* [R installeren op HDInsight-clusters][hdinsight-install-r]: scriptactie voorbeeld over het installeren van R.
* [Solr installeren op HDInsight-clusters](hdinsight-hadoop-solr-install.md): scriptactie voorbeeld over het installeren van Solr.

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
