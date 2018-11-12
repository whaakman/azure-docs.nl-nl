---
title: Giraph installeren en gebruiken op Hadoop-clusters in HDInsight - Azure
description: Meer informatie over het aanpassen van HDInsight-cluster met Giraph en hoe u Giraph gebruikt.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2016
ms.author: hrasheed
ROBOTS: NOINDEX
ms.openlocfilehash: 6138cc337c35924405fa3f6489e7e40bfc5779c9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007005"
---
# <a name="install-and-use-giraph-on-windows-based-hdinsight-clusters"></a>Giraph installeren en gebruiken op Windows gebaseerde HDInsight-clusters

Meer informatie over het aanpassen van Windows op basis van HDInsight-cluster met Giraph met Script Action en hoe u Giraph gebruikt om grootschalige grafieken te verwerken. Zie voor meer informatie over het gebruik van Giraph met een cluster op basis van Linux [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).

> [!IMPORTANT]
> De stappen in dit document werken alleen met HDInsight op basis van een Windows-clusters. HDInsight is alleen beschikbaar voor Windows voor versies lager dan HDInsight 3.4. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor meer informatie over hoe u Giraph installeren op een Linux gebaseerde HDInsight-cluster [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md).


U Giraph kunt installeren op elk type cluster (Hadoop, Storm, HBase, Spark) op Azure HDInsight met behulp van *scriptactie*. Een voorbeeld van een script Giraph installeren op een HDInsight-cluster is beschikbaar via een alleen-lezen Azure storage-blob op [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1). Het voorbeeldscript werkt alleen met HDInsight-clusterversie 3.1. Zie voor meer informatie over HDInsight-clusterversies [HDInsight-clusterversies](hdinsight-component-versioning.md).

**Gerelateerde artikelen**

* [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster met Script Action aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van scriptacties.
* [Script Action-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).

## <a name="what-is-giraph"></a>Wat is Giraph?
<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> Hiermee kunt u grafische verwerking met behulp van Hadoop en kan worden gebruikt met Azure HDInsight. Grafieken model relaties tussen objecten, zoals de verbindingen tussen routers in een groot netwerk zoals Internet, of de relaties tussen personen op sociale netwerken (soms ook wel een sociale grafiek genoemd). Grafische verwerking kunt u te beredeneren de relaties tussen de objecten in een grafiek, zoals:

* Identificeren van mogelijke vrienden op basis van uw huidige relaties.
* Hoe identificeert u de kortste route tussen twee computers in een netwerk.
* Berekening van de positie van de pagina van webpagina's.

## <a name="install-giraph-using-portal"></a>Giraph met behulp van portal installeren
1. Beginnen met het maken van een cluster met behulp van de **aangepast maken** optie, zoals beschreven op [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md).
2. Op de **scriptacties** pagina van de wizard, klikt u op **scriptactie toevoegen** voor meer informatie over de scriptactie, zoals hieronder wordt weergegeven:

    ![Scriptactie gebruiken voor het aanpassen van een cluster](./media/hdinsight-hadoop-giraph-install/hdi-script-action-giraph.png "scriptactie gebruiken voor het aanpassen van een cluster")

    <table border='1'>
        <tr><th>Eigenschap</th><th>Waarde</th></tr>
        <tr><td>Naam</td>
            <td>Geef een naam voor de scriptactie. Bijvoorbeeld, <b>Giraph installeren</b>.</td></tr>
        <tr><td>Script-URI</td>
            <td>Geef de Uniform Resource Identifier (URI) naar het script dat wordt aangeroepen voor het aanpassen van het cluster. Bijvoorbeeld: <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>Soort knooppunt</td>
            <td>Geef op de knooppunten waarop de aanpassing-script wordt uitgevoerd. U kunt ervoor kiezen <b>alle knooppunten</b>, <b>hoofdknooppunten alleen</b>, of <b>Worker-knooppunten</b>.
        <tr><td>Parameters</td>
            <td>Geef de parameters op, indien vereist door het script. Giraph installeren met het script is niet vereist voor alle parameters, zodat u kunt dit leeg laten.</td></tr>
    </table>

    U kunt meer dan één scriptactie voor het installeren van meerdere onderdelen op het cluster toevoegen. Nadat u de scripts hebt toegevoegd, klikt u op het vinkje om te beginnen met het maken van het cluster.

## <a name="use-giraph"></a>Giraph gebruiken
We gebruiken het SimpleShortestPathsComputation-voorbeeld ter illustratie van de basic <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> implementatie voor het zoeken naar het kortste pad tussen objecten in een grafiek. Gebruik de volgende stappen op de voorbeeldgegevens en de voorbeeld-jar uploaden, een taak uitvoeren met behulp van het voorbeeld SimpleShortestPathsComputation en bekijk vervolgens de resultaten.

1. Upload een bestand met voorbeeldgegevens naar Azure Blob-opslag. Maak een nieuw bestand met de naam op uw lokale werkstation **tiny_graph.txt**. Deze moet de volgende regels bevat:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Upload het bestand tiny_graph.txt naar de primaire opslag voor uw HDInsight-cluster. Zie voor instructies over het uploaden van gegevens, [gegevens uploaden voor Hadoop-taken in HDInsight](hdinsight-upload-data.md).

    Deze gegevens beschrijft de relatie tussen de objecten in een gerichte graaf met behulp van de indeling [bron\_-id, bron\_waarde [[dest\_id], [edge\_waarde],...]]. Elke regel vertegenwoordigt een relatie tussen een **bron\_id** object en een of meer **dest\_id** objecten. De **edge\_waarde** (of gewicht) kan worden beschouwd als de sterkte of de afstand van de verbinding tussen **source_id** en **dest\_id**.

    Getekend, en u de waarde (of gewicht) als de afstand tussen de objecten, de bovenstaande gegevens als volgt uitzien:

    ![tiny_graph.txt getekend als cirkels met verschillende afstand tussen regels](./media/hdinsight-hadoop-giraph-install/giraph-graph.png)
2. Voer in het voorbeeld SimpleShortestPathsComputation. Gebruik de volgende Azure PowerShell-cmdlets om uit te voeren in het voorbeeld met behulp van het bestand tiny_graph.txt als invoer.

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
3. Bekijk de resultaten. Nadat de taak is voltooid, de resultaten worden opgeslagen in twee uitvoerbestanden in de **wasb: / / / voorbeeld/out/shotestpaths** map. De bestanden worden genoemd **onderdeel-m-00001** en **onderdeel-m-00002**. Voer de volgende stappen uit om te downloaden en weergeven van de uitvoer:

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

    Hiermee maakt u de **uitvoer-voorbeeld/shortestpaths** mapstructuur, in de huidige map op uw werkstation en de uitvoerbestanden downloaden van de twee naar die locatie.

    Gebruik de **Cat** cmdlet om de inhoud van de bestanden weer te geven:

        Cat example/output/shortestpaths/part*

    De uitvoer ziet er ongeveer het volgende:

        0    1.0
        4    5.0
        2    2.0
        1    0.0
        3    1.0

    Het voorbeeld is moeilijk te beginnen met gecodeerd SimpleShortestPathComputation object-ID 1 en de kortste route op andere objecten. Zodat de uitvoer moet worden gelezen als `destination_id distance`, waarbij afstand is de waarde (of het gewicht) van de randen gereisd tussen object-ID 1 en de doel-ID.

    Dit visualiseren, kunt u de resultaten controleren door het kortste pad tussen 1-ID en alle andere objecten onderweg. Houd er rekening mee dat het kortste pad tussen 1-ID en -ID 4 5 is. Dit is de totale afstand tussen <span style="color:orange">ID 1 en 3</span>, en vervolgens <span style="color:red">ID 3 en 4</span>.

    ![Tekenen van objecten als cirkels met kortste paden tussen getekend](./media/hdinsight-hadoop-giraph-install/giraph-graph-out.png)

## <a name="install-giraph-using-aure-powershell"></a>Met behulp van Aure PowerShell Giraph installeren
Zie [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).  Het voorbeeld ziet u hoe u Spark met behulp van Azure PowerShell installeren. U moet het script te gebruiken aanpassen [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="install-giraph-using-net-sdk"></a>Giraph met .NET SDK installeren
Zie [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell). Het voorbeeld ziet u hoe u Spark met behulp van de .NET SDK installeren. U moet het script te gebruiken aanpassen [ https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 ](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1).

## <a name="see-also"></a>Zie ook
* [Giraph installeren op HDInsight Hadoop-clusters (Linux)](hdinsight-hadoop-giraph-install-linux.md)
* [Hadoop-clusters maken in HDInsight](hdinsight-provision-clusters.md): algemene informatie over het maken van HDInsight-clusters.
* [HDInsight-cluster met Script Action aanpassen][hdinsight-cluster-customize]: algemene informatie over het aanpassen van HDInsight-clusters met behulp van scriptacties.
* [Script Action-scripts ontwikkelen voor HDInsight](hdinsight-hadoop-script-actions.md).
* [Installeren en gebruiken van Spark op HDInsight-clusters][hdinsight-install-spark]: Script Action-voorbeeld over het installeren van Spark.
* [Solr installeren op HDInsight-clusters](hdinsight-hadoop-solr-install.md): Script Action-voorbeeld over het installeren van Solr.

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
