---
title: De HDFS-CLI gebruiken met Azure Data Lake Storage Gen2
description: Inleiding tot de HDFS-CLI voor Data Lake Storage Gen2
services: storage
author: artemuwka
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: artek
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 649fe5ebadf69a90b4794fcaf4519ea5bcc0c4a2
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55874176"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>De HDFS-CLI gebruiken met Data Lake Storage Gen2

Azure Data Lake Storage Gen2 kunt u beheren en toegang tot gegevens, net zoals u zou met doen een [Hadoop Distributed File System (HDFS)](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Of u een HDInsight-cluster die is gekoppeld of een Apache Spark-taak uitvoeren met Azure Databricks analyses uitvoeren op gegevens die zijn opgeslagen in een Azure Storage-account hebt, kunt u opdrachtregelinterface (CLI) kunt gebruiken om te halen en de geladen gegevens te manipuleren.

## <a name="hdfs-cli-with-hdinsight"></a>HDFS-CLI met HDInsight

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. Dit bestandssysteem is toegankelijk via de shell die rechtstreeks communiceert met de HDFS- en andere bestandssystemen die ondersteuning biedt voor Hadoop. Hieronder vindt u de veelgebruikte opdrachten en de koppelingen naar nuttige informatiebronnen.

>[!IMPORTANT]
>HDInsight-cluster facturering begint nadat een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. Zie voor informatie over het verwijderen van een cluster, onze [artikel van het onderwerp](../../hdinsight/hdinsight-delete-cluster.md). Gegevens die zijn opgeslagen in een opslagaccount met Data Lake Storage Gen2 ingeschakeld blijft echter bestaan, zelfs nadat u een HDInsight-cluster wordt verwijderd.

### <a name="get-a-list-of-files-or-directories"></a>Haal een lijst van bestanden of mappen

    hdfs dfs -ls <args>

### <a name="create-a-directory"></a>Een map maken

    hdfs dfs -mkdir [-p] <paths>

### <a name="delete-a-file-or-a-directory"></a>Een bestand of een map verwijderen

    hdfs dfs -rm [-skipTrash] URI [URI ...]

### <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>De HDFS-CLI gebruiken met een HDInsight Hadoop-cluster op Linux

Eerst tot stand brengen [externe toegang tot services](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Als u kiest [SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) de voorbeeld-PowerShell-code zou er als volgt uitzien:

```PowerShell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
De verbindingsreeks kunt u vinden op de ' SSH- + Cluster aanmelding ' sectie van de HDInsight-cluster-blade in Azure portal. SSH-referenties zijn opgegeven op het moment dat het cluster wordt gemaakt.

Zie voor meer informatie over de CLI HDFS, de [officiële documentatie](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) en de [handleiding voor HDFS-machtigingen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html). Zie voor meer informatie over de ACL's in Databricks, de [geheimen CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#secrets-cli). 

## <a name="hdfs-cli-with-azure-databricks"></a>HDFS-CLI met Azure Databricks

De Databricks biedt een eenvoudig te gebruiken CLI gebaseerd op het Databricks-REST-API. De open source-project wordt gehost op [GitHub](https://github.com/databricks/databricks-cli). Hieronder vindt u de meest gebruikte opdrachten.

### <a name="get-a-list-of-files-or-directories"></a>Haal een lijst van bestanden of mappen

    dbfs ls [-l]

### <a name="create-a-directory"></a>Een map maken

    dbfs mkdirs

### <a name="delete-a-file"></a>Een bestand verwijderen

    dbfs rm [-r]

Een andere methode voor interactie met Databricks notebooks zijn. Hoewel een primaire taal van een laptop heeft, kunt u talen kunt combineren door de taal voor het % van magic-opdracht aan het begin van een cel op te geven. Specifiek, kunt % sh u voor de uitvoering van shell-code in het notitieblok lijkt veel op in het HDInsight-voorbeeld eerder in dit artikel.

### <a name="get-a-list-of-files-or-directories"></a>Haal een lijst van bestanden of mappen

    %sh ls <args>

### <a name="create-a-directory"></a>Een map maken

    %sh mkdir [-p] <paths>

### <a name="delete-a-file-or-a-directory"></a>Een bestand of een map verwijderen

    %sh rm [-skipTrash] URI [URI ...]

Na het starten van het Spark-cluster in Azure Databricks, maakt u een nieuwe notebook. Het voorbeeldscript notebook ziet er als volgt uit:

    #Execute basic HDFS commands invoking the shell. Display the hierarchy.
    %sh ls /
    #Create a sample directory.
    %sh mkdir /samplefolder
    #Get the ACL of the newly created directory.
    hdfs dfs -getfacl /samplefolder

Zie voor meer informatie over Databricks CLI de [officiële documentatie](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html). Zie voor meer informatie over laptops, de [notitieblokken](https://docs.azuredatabricks.net/user-guide/notebooks/index.html) gedeelte van de documentatie.

## <a name="set-file-and-directory-level-permissions"></a>Bestands- en machtigingen op databaseniveau instellen

U stelt en ophalen van toegangsmachtigingen op het niveau van bestanden en mappen. Hier volgt een aantal opdrachten aan de slag te gaan. 

Zie voor meer informatie over bestands- en machtigingen op het niveau voor het bestandssysteem van Azure Data Lake Gen2 [toegangsbeheer in Azure Data Lake Storage Gen2](storage-data-lake-storage-access-control.md).

### <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>De toegangsbeheerlijsten (ACL's) van bestanden en mappen weergeven

    hdfs dfs -getfacl [-R] <path>

Voorbeeld:

`hdfs dfs -getfacl -R /dir`

See [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl)

### <a name="set-acls-of-files-and-directories"></a>Stel de ACL's van bestanden en mappen

    hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]

Voorbeeld:

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

See [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl)

### <a name="change-the-owner-of-files"></a>De eigenaar van bestanden wijzigen

    hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>

Zie [eigenaar](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown)

### <a name="change-group-association-of-files"></a>De koppeling van bestanden wijzigen

    hdfs dfs -chgrp [-R] <group> <URI>

Zie [chgrp uitvoert](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp)

### <a name="change-the-permissions-of-files"></a>De machtigingen van bestanden wijzigen

    hdfs dfs -chmod [-R] <mode> <URI>

Zie [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod)

U ziet de volledige lijst van opdrachten op de [Apache Hadoop 2.4.1 bestand System Shell Guide](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) Website.

## <a name="next-steps"></a>Volgende stappen

[Een Azure Data Lake Storage Gen2 geschikt account gebruiken in Azure Databricks](./data-lake-storage-quickstart-create-databricks-account.md) 
