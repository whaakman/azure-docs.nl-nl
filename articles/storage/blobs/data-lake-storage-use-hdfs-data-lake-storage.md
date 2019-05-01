---
title: De HDFS-CLI gebruiken met Azure Data Lake Storage Gen2
description: Inleiding tot de HDFS-CLI voor Data Lake Storage Gen2
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.reviewer: artek
ms.openlocfilehash: 24123278ff353860ff2af59f4fd77645dfc189e3
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64938846"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>De HDFS-CLI gebruiken met Data Lake Storage Gen2

U kunt toegang tot en beheer van de gegevens in uw storage-account met behulp van een opdrachtregelinterface, net zoals u zou met doen een [Hadoop Distributed File System (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html). Dit artikel bevat enkele voorbeelden die u helpen aan de slag.

HDInsight biedt toegang tot het Distributed File System dat lokaal wordt gekoppeld aan de rekenknooppunten. U kunt toegang tot dit bestandssysteem met behulp van de shell die rechtstreeks communiceert met de HDFS- en andere bestandssystemen die ondersteuning biedt voor Hadoop.

Zie voor meer informatie over de CLI HDFS, de [officiële documentatie](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) en de [handleiding voor HDFS-machtigingen](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

>[!NOTE]
>Als u met behulp van Azure Databricks in plaats van HDInsight, en u communiceren met uw gegevens wilt met behulp van een opdrachtregelinterface, kunt u de CLI Databricks kunt gebruiken om te communiceren met het Databricks-bestandssysteem. Zie [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).

## <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>De HDFS-CLI gebruiken met een HDInsight Hadoop-cluster op Linux

Eerst tot stand brengen [externe toegang tot services](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services). Als u kiest [SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) de voorbeeld-PowerShell-code zou er als volgt uitzien:

```powershell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
De verbindingsreeks kunt u vinden op de ' SSH- + Cluster aanmelding ' sectie van de HDInsight-cluster-blade in Azure portal. SSH-referenties zijn opgegeven op het moment dat het cluster wordt gemaakt.

>[!IMPORTANT]
>HDInsight-cluster facturering begint nadat een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. Zie voor informatie over het verwijderen van een cluster, onze [artikel van het onderwerp](../../hdinsight/hdinsight-delete-cluster.md). Gegevens die zijn opgeslagen in een opslagaccount met Data Lake Storage Gen2 ingeschakeld blijft echter bestaan, zelfs nadat u een HDInsight-cluster wordt verwijderd.

## <a name="create-a-file-system"></a>Een bestandssysteem maken

    hdfs dfs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<file-system-name>@<storage-account-name>.dfs.core.windows.net/

* Vervang de tijdelijke aanduiding `<file-system-name>` door de naam die u aan uw bestandssysteem wilt geven.

* Vervang de tijdelijke plaatsaanduiding `<storage-account-name>` door de naam van uw opslagaccount.

## <a name="get-a-list-of-files-or-directories"></a>Haal een lijst van bestanden of mappen

    hdfs dfs -ls <path>

Vervang de `<path>` tijdelijke aanduiding door de URI van het bestandssysteem of de map van het bestandssysteem.

Bijvoorbeeld: `hdfs dfs -ls abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name`

## <a name="create-a-directory"></a>Een map maken

    hdfs dfs -mkdir [-p] <path>

Vervang de `<path>` tijdelijke aanduiding door de naam van het bestandssysteem hoofdmap of een map in uw bestandssysteem.

Bijvoorbeeld: `hdfs dfs -mkdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/`

## <a name="delete-a-file-or-directory"></a>Een bestand of map verwijderen

    hdfs dfs -rm <path>

Vervang de `<path>` tijdelijke aanduiding door de URI van het bestand of map die u wilt verwijderen.

Bijvoorbeeld: `hdfs dfs -rmdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name/my-file-name`

## <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>De toegangsbeheerlijsten (ACL's) van bestanden en mappen weergeven

    hdfs dfs -getfacl [-R] <path>

Voorbeeld:

`hdfs dfs -getfacl -R /dir`

See [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl)

## <a name="set-acls-of-files-and-directories"></a>Stel de ACL's van bestanden en mappen

    hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]

Voorbeeld:

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

See [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl)

## <a name="change-the-owner-of-files"></a>De eigenaar van bestanden wijzigen

    hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>

Zie [eigenaar](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown)

## <a name="change-group-association-of-files"></a>De koppeling van bestanden wijzigen

    hdfs dfs -chgrp [-R] <group> <URI>

Zie [chgrp uitvoert](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp)

## <a name="change-the-permissions-of-files"></a>De machtigingen van bestanden wijzigen

    hdfs dfs -chmod [-R] <mode> <URI>

Zie [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod)

U ziet de volledige lijst van opdrachten op de [Apache Hadoop 2.4.1 bestand System Shell Guide](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) Website.

## <a name="next-steps"></a>Volgende stappen

* [Een Azure Data Lake Storage Gen2 geschikt account gebruiken in Azure Databricks](./data-lake-storage-quickstart-create-databricks-account.md)

* [Meer informatie over toegangsbeheerlijsten voor bestanden en mappen](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)
