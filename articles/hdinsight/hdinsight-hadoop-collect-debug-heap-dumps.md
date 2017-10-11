---
title: Fouten opsporen en analyseren van Hadoop-services met heap dumpbestanden - Azure | Microsoft Docs
description: Automatisch verzamelen heap dumpbestanden voor Hadoop-services en plaats in de Azure Blob storage-account voor foutopsporing en analyse.
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e4ec4ebb-fd32-4668-8382-f956581485c4
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6d1d4d47d279eb7a1f0bf1f587445683f0ace7a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Collect heap dumpen in Blob storage opsporen en analyseren van Hadoop-services
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Dumpbestanden voor heap bevatten een momentopname van de toepassing geheugen, met inbegrip van de waarden van variabelen op het moment dat de dump is gemaakt. Ze zijn daarom nuttig voor het oplossen van problemen die tijdens runtime optreden. Dumpbestanden voor heap kunnen automatisch worden verzameld voor Hadoop-services en in de Azure Blob storage-account van een gebruiker onder HDInsightHeapDumps geplaatst /.

De verzameling van heap dumpbestanden voor verschillende services moet zijn ingeschakeld voor services op afzonderlijke clusters. De standaardwaarde voor deze functie is te zijn uitgeschakeld voor een cluster. Deze dumpbestanden heap kunnen oplopen, zodat u aangeraden wordt te controleren van de Blob storage-account waarin ze worden opgeslagen na inschakeling van de verzameling.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. De informatie in dit artikel is alleen van toepassing op HDInsight op basis van Windows. Zie voor meer informatie op Linux gebaseerde HDInsight [inschakelen heap dumpbestanden voor Hadoop-services op Linux gebaseerde HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>In aanmerking komende services voor heap dumpbestanden
U kunt de heap dumpbestanden voor de volgende services inschakelen:

* **hcatalog** -tempelton
* **hive** -hiveserver2, metastore, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Configuratie-elementen waarmee heap dumpbestanden
Als u wilt inschakelen heap dumpbestanden voor een service, moet u de juiste configuratie-elementen in de sectie voor de service, die wordt opgegeven met instellen **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

De waarde van **service_name** kan een van de services hier vermeld: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, of namenode.

## <a name="enable-using-azure-powershell"></a>Met Azure PowerShell inschakelen
Bijvoorbeeld, als u wilt inschakelen heap dumpbestanden via Azure PowerShell voor jobhistoryserver, kunt u het volgende script:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Schakel met .NET SDK
Bijvoorbeeld, als u wilt heap dumpbestanden inschakelen met behulp van de Azure HDInsight .NET SDK voor jobhistoryserver, kunt u de volgende code:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
