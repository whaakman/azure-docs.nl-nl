---
title: De HDFS-CLI gebruiken met Azure Data Lake Storage Gen2 Preview
description: Inleiding tot de HDFS-CLI voor Gen2 Preview van Data Lake-opslag
services: storage
author: artemuwka
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: artek
ms.component: data-lake-storage-gen2
ms.openlocfilehash: ef28468c7aa420c145f5e6d79e2b079bd1d094f4
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39523679"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>De HDFS-CLI gebruiken met Data Lake Storage Gen2

Azure Data Lake Storage Gen2 Preview kunt u beheren en toegang tot gegevens, net zoals u zou met doen een [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Of hebt u een HDInsight-cluster die is gekoppeld of een Apache Spark-taak uitvoeren met Azure Databricks analyses uitvoeren op gegevens die zijn opgeslagen in Azure Data Lake Storage Gen2 kunt u opdrachtregelinterface (CLI) kunt gebruiken om te halen en de geladen gegevens te manipuleren. De rest van het artikel bevat een overzicht van de opties die u hebt tijdens de [Azure Storage-team is bezig met toevoegen van ondersteuning voor Azure Storage Explorer en Azure-portal](https://azure.microsoft.com/roadmap/).

## <a name="hdfs-cli-with-hdinsight"></a>HDFS-CLI met HDInsight

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de shell die rechtstreeks communiceert met de HDFS- en andere bestandssystemen die ondersteuning biedt voor Hadoop. Hieronder vindt u de veelgebruikte opdrachten en de koppelingen naar nuttige informatiebronnen.

>[!IMPORTANT]
>De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. Facturering is pro-rata per minuut berekend, dus u altijd uw cluster verwijderen moet wanneer deze niet meer gebruikt wordt (meer informatie over het [een cluster verwijderen](../../hdinsight/hdinsight-delete-cluster.md)). Gegevens die zijn opgeslagen in Azure Data Lake Storage Gen2 blijft echter bestaan, zelfs nadat u een HDInsight-cluster wordt verwijderd.

Voor een lijst van bestanden of mappen:

    hdfs dfs -ls <args>
Een map wilt maken:

    hdfs dfs -mkdir [-p] <paths>
Een bestand of map verwijderen:

    hdfs dfs -rm [-skipTrash] URI [URI ...]


Nu nemen we HDInsight Hadoop-cluster op Linux als voorbeeld. Als u wilt de HDFS-CLI gebruikt, moet u eerst tot stand brengen [externe toegang tot services](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Als u kiest [SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) de voorbeeld-PowerShell-code zou er als volgt uitzien:
```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```

De verbindingsreeks kunt u vinden op de ' SSH- + Cluster aanmelding ' sectie van de HDInsight-cluster-blade in Azure portal. SSH-referenties zijn opgegeven op het moment dat het cluster wordt gemaakt.

Zie voor meer informatie over de CLI HDFS, de [officiële documentatie](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) en de [handleiding voor HDFS-machtigingen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).

## <a name="hdfs-cli-with-azure-databricks"></a>HDFS-CLI met Azure Databricks

De Databricks biedt een eenvoudig te gebruiken CLI gebaseerd op het Databricks-REST-API. De open source-project wordt gehost op [GitHub](https://github.com/databricks/databricks-cli). Hieronder vindt u de meest gebruikte opdrachten.

Voor een lijst van bestanden of mappen:

    dbfs ls [-l]
Een map wilt maken:

    dbfs mkdirs
Een bestand verwijderen:

    dbfs rm [-r]

Een andere methode voor interactie met Databricks notebooks zijn. Hoewel een primaire taal van een laptop heeft, kunt u talen kunt combineren door de taal voor het % van magic-opdracht aan het begin van een cel op te geven. Specifiek, kunt % sh u voor de uitvoering van shell-code in het notitieblok lijkt veel op in het HDInsight-voorbeeld eerder in dit artikel.

Voor een lijst van bestanden of mappen:

    %sh ls <args>
Een map wilt maken:

    %sh mkdir [-p] <paths>
Een bestand of map verwijderen:

    %sh rm [-skipTrash] URI [URI ...]

Na het starten van het Spark-cluster in Azure Databricks, maakt u een nieuwe notebook. Het voorbeeldscript notebook ziet er als volgt uit:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder

Zie voor meer informatie over Databricks CLI de [officiële documentatie](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Zie voor meer informatie over laptops, de [notitieblokken](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) gedeelte van de documentatie.

## <a name="next-steps"></a>Volgende stappen

- [Een HDInsight-cluster maken met Azure Data Lake Storage Gen2](./quickstart-create-connect-hdi-cluster.md)
- [Een Azure Data Lake Storage Gen2 geschikt account gebruiken in Azure Databricks](./quickstart-create-databricks-account.md) 