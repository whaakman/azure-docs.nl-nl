---
title: De HDFS-CLI gebruiken met Azure Data Lake Storage-Gen2 Preview
description: Inleiding tot HDFS CLI voor Data Lake Storage Gen2 Preview
services: storage
documentationcenter: ''
author: artemuwka
manager: jahogg
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: artek
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 75fb07120c78c45d422ee5017eac0afcf0e80859
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060815"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>De HDFS-CLI gebruiken met Data Lake Storage Gen2

Azure Data Lake Storage Gen2 Preview kunt u beheren en toegang tot gegevens, net zoals u zou met doen een [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Ongeacht of er een HDInsight-cluster aangesloten of uitvoeren van een Apache Spark-taak met Azure Databricks voor het uitvoeren van analyses op gegevens die zijn opgeslagen in Azure Data Lake Storage Gen2 kunt u opdrachtregelinterface (CLI) op te halen en de geladen gegevens bewerken. De rest van het artikel bevat een overzicht van de opties die u hebt tijdens [Azure Storage-team werkt over het toevoegen van ondersteuning voor Azure Storage Explorer en Azure-portal](https://azure.microsoft.com/roadmap/) -plezier!

## <a name="hdfs-cli-with-hdinsight"></a>HDFS CLI met HDInsight

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem toegankelijk zijn met behulp van de shell die rechtstreeks communiceert met de HDFS- en andere bestandssystemen die ondersteuning biedt voor Hadoop. Hieronder vindt u de veelgebruikte opdrachten en de koppelingen naar bronnen die nuttig.

>[!IMPORTANT]
>De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. Facturering is pro rato per minuut, dus u uw cluster altijd verwijderen moet wanneer deze niet langer in gebruik (meer informatie over hoe [verwijderen van een cluster](../../hdinsight/hdinsight-delete-cluster.md)). Gegevens die zijn opgeslagen in Azure Data Lake Storage Gen2 persistente, zelfs wanneer een HDInsight-cluster is verwijderd.

Voor een lijst van bestanden of mappen:

    hdfs dfs -ls <args>
Een map maken:

    hdfs dfs -mkdir [-p] <paths>
Verwijderen van een bestand of map:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Laten we nu eens HDInsight Hadoop-cluster op Linux als voorbeeld. Als u wilt gebruiken de HDFS CLI, moet u eerst tot stand brengen [externe toegang tot services](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Als u kiest [SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) de voorbeeldcode PowerShell zou er als volgt uitzien:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

De verbindingsreeks kan worden gevonden op de ' SSH + Cluster aanmelding ' sectie van de blade HDInsight-cluster in Azure-portal. SSH-referenties zijn opgegeven op het moment van het maken van het cluster.

Zie voor meer informatie over HDFS CLI de [officiële documentatie](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) en de [HDFS machtigingen handleiding](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>HDFS CLI met Azure Databricks

De Databricks biedt een gemakkelijk te gebruiken CLI gebouwd op de Databricks REST-API. De open source-project wordt gehost op [GitHub](https://github.com/databricks/databricks-cli). Hieronder vindt u de veelgebruikte opdrachten.

Voor een lijst van bestanden of mappen:

    dbfs ls [-l]
Een map maken:

    dbfs mkdirs
Een bestand te verwijderen:

    dbfs rm [-r]

Een andere manier van de interactie met Databricks zijn notitieblokken. Hoewel een primaire taal van een laptop heeft, kunt u talen elkaar door te geven van de taal magische opdracht % taal aan het begin van een cel. In het bijzonder kunt % servicel u shell code uitvoeren in uw laptop net zoals in het voorbeeld HDInsight eerder in dit artikel.

Voor een lijst van bestanden of mappen:

    %sh ls <args>
Een map maken:

    %sh mkdir [-p] <paths>
Verwijderen van een bestand of map:

    %sh rm [-skipTrash] URI [URI ...]

Na het starten van het Spark-cluster in Azure Databricks, maakt u een nieuwe notebook. Het voorbeeldscript notebook ziet er als volgt uit:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Zie voor meer informatie over Databricks CLI de [officiële documentatie](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Zie voor meer informatie over laptops de [notitieblokken](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) gedeelte van de documentatie.

## <a name="next-steps"></a>Volgende stappen

- [Een HDInsight-cluster maken met Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
- [Een Azure Data Lake Storage Gen2 geschikt account in Azure Databricks gebruiken](./quickstart-create-databricks-account.md) 