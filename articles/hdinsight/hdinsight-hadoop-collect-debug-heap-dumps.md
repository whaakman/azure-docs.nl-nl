---
title: Fouten opsporen en analyseren van Hadoop-services met heapdumps - Azure
description: Automatisch heapdumps voor Hadoop-services te verzamelen en plaats binnen de Azure Blob storage-account voor foutopsporing en analyse.
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/25/2017
ms.author: jasonh
ROBOTS: NOINDEX
ms.openlocfilehash: 35f7843ebf49e79d9045c72493bb38b218234288
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43099764"
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Heapdumps verzamelen in Blob-opslag voor foutopsporing en analyseren van Hadoop-services
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heapdumps bevatten een momentopname van het geheugen van de toepassing, met inbegrip van de waarden van variabelen op het moment dat de dump is gemaakt. Ze zijn daarom nuttig voor het oplossen van problemen die tijdens de uitvoering optreden. Heapdumps worden automatisch verzameld voor Hadoop-services en in de Azure Blob storage-account van een gebruiker in HDInsightHeapDumps geplaatst /.

De verzameling van heapdumps voor verschillende services moet zijn ingeschakeld voor services op afzonderlijke clusters. De standaardwaarde voor deze functie is om te worden uitgeschakeld voor een cluster. Deze heapdumps kunnen oplopen, zodat u aangeraden wordt voor het bewaken van de Blob storage-account waar ze worden opgeslagen nadat de verzameling is ingeschakeld.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. De informatie in dit artikel is alleen van toepassing op HDInsight op basis van Windows. Zie voor informatie over HDInsight op basis van Linux, [heapdumps voor Hadoop op Linux gebaseerde HDInsight-services inschakelen](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>In aanmerking komende services voor heapdumps
Heapdumps voor de volgende services, kunt u inschakelen:

* **hcatalog** -tempelton
* **hive** -hiveserver2, metastore, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resourcemanager, nodemanager, timelineserver
* **hdfs** -datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Configuratie-elementen die heapdumps inschakelen
Als u wilt inschakelen op heapdumps voor een service, moet u de juiste configuratie-elementen in de sectie voor die service, die is opgegeven door instellen **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

De waarde van **service_name** kan een van de services die hier worden vermeld: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, of namenode.

## <a name="enable-using-azure-powershell"></a>Inschakelen met behulp van Azure PowerShell
Bijvoorbeeld, als u wilt heapdumps inschakelen met behulp van Azure PowerShell voor jobhistoryserver, kunt u het volgende script:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Inschakelen met behulp van .NET SDK
Bijvoorbeeld, als u wilt heapdumps inschakelen met behulp van de Azure HDInsight .NET SDK voor jobhistoryserver, kunt u de volgende code:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
