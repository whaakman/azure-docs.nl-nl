---
title: Hadoop Hive gebruiken met PowerShell in HDInsight - Azure | Microsoft Docs
description: PowerShell gebruiken voor het uitvoeren van Hive-query's in Hadoop op HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/27/2017
ms.author: larryfr
ms.openlocfilehash: 95bfab18a6a8a9ad9eb547179a3205ae4b186213
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="run-hive-queries-using-powershell"></a>Uitvoeren van Hive-query's met behulp van PowerShell
[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Dit document bevat een voorbeeld van het gebruik van Azure PowerShell in de modus Azure Resource Group Hive-query's uitvoeren in een Hadoop op HDInsight-cluster.

> [!NOTE]
> Dit document biedt geen een gedetailleerde beschrijving van wat de HiveQL-instructies die worden gebruikt in de voorbeelden doen. Zie voor informatie over de HiveQL die wordt gebruikt in dit voorbeeld, [Hive gebruiken met Hadoop op HDInsight](hdinsight-use-hive.md).

**Vereisten**

* **Een Azure HDInsight-cluster**: het maakt niet uit of het cluster Windows is of Linux-.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* **Een werkstation met Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a>Uitvoeren van Hive-query's met Azure PowerShell

Azure PowerShell biedt *cmdlets* waarmee u kunt op afstand Hive-query's uitvoeren op HDInsight. Intern, de REST-aanroepen naar voor het maken van de cmdlets [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) op het HDInsight-cluster.

De volgende cmdlets worden gebruikt bij het uitvoeren van Hive-query's in een externe HDInsight-cluster:

* **Add-AzureRmAccount**: Azure PowerShell verifieert met uw Azure-abonnement.
* **Nieuwe AzureRmHDInsightHiveJobDefinition**: maakt een *taak definitie* met behulp van de opgegeven HiveQL-instructies.
* **Start AzureRmHDInsightJob**: de taakdefinitie verzendt naar HDInsight en de taak wordt gestart. Een *taak* object wordt geretourneerd.
* **Wacht AzureRmHDInsightJob**: het taakobject gebruikt om te controleren van de status van de taak. Wacht totdat de taak is voltooid of de wachttijd is overschreden.
* **Get-AzureRmHDInsightJobOutput**: gebruikt voor het ophalen van de uitvoer van de taak.
* **Aanroepen AzureRmHDInsightHiveJob**: gebruikt voor het uitvoeren van HiveQL-instructies. Deze cmdlet-blokken de query is voltooid en vervolgens de resultaten geretourneerd.
* **Gebruik AzureRmHDInsightCluster**: Hiermee stelt u het huidige cluster moet worden gebruikt voor de **Invoke-AzureRmHDInsightHiveJob** opdracht.

De volgende stappen laten zien hoe u deze cmdlets gebruiken om een taak uitvoert in uw HDInsight-cluster:

1. De volgende code als met een editor opslaan **hivejob.ps1**.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Open een nieuw **Azure PowerShell** opdrachtprompt. Wijzig de mappen naar de locatie van de **hivejob.ps1** bestand en vervolgens voert u het script met de volgende opdracht:

        .\hivejob.ps1

    Wanneer het script wordt uitgevoerd, wordt u gevraagd de clusternaam en referenties van het HTTPS/Admin-account invoeren voor het cluster. U kunt ook gevraagd zich aanmelden bij uw Azure-abonnement.

3. Wanneer de taak is voltooid, wordt informatie vergelijkbaar met de volgende tekst:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Zoals eerder gezegd **Invoke-Hive** kan worden gebruikt voor een query uitvoeren en wacht tot het antwoord. Het volgende script gebruiken om te zien hoe Invoke-Hive werkt:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    De uitvoer ziet er de volgende tekst:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > Voor meer HiveQL-query's, kunt u Azure PowerShell **hier tekenreeksen** cmdlet of HiveQL scriptbestanden. Het volgende fragment toont hoe u de **Invoke-Hive** cmdlet een bestand HiveQL-script uit te voeren. Het scriptbestand HiveQL moet worden geüpload naar wasb: / /.
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > Voor meer informatie over **hier tekenreeksen**, Zie <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">met behulp van Windows PowerShell hier-tekenreeksen</a>.

## <a name="troubleshooting"></a>Problemen oplossen

Als er geen gegevens worden geretourneerd als de taak is voltooid, kunt u de foutenlogboeken weergeven. Als u wilt weergeven van informatie over de fout voor deze taak, Voeg het volgende toe aan het einde van de **hivejob.ps1** bestand, opslaan en vervolgens opnieuw uit te voeren.

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Deze cmdlet retourneert de gegevens die tijdens de verwerking van een taak naar STDERR worden geschreven.

## <a name="summary"></a>Samenvatting

Zoals u zien kunt, biedt Azure PowerShell een eenvoudige manier om Hive-query's uitvoeren in een HDInsight-cluster, de taakstatus te controleren en ophalen van de uitvoer.

## <a name="next-steps"></a>Volgende stappen

Voor algemene informatie over Hive in HDInsight:

* [Hive gebruiken met Hadoop in HDInsight](hdinsight-use-hive.md)

Voor informatie over andere manieren kunt u werken met Hadoop op HDInsight:

* [Pig gebruiken met Hadoop in HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met Hadoop op HDInsight](hdinsight-use-mapreduce.md)
