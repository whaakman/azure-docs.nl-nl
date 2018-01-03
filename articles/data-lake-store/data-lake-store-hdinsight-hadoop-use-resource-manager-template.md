---
title: Azure-sjablonen gebruiken voor het maken van HDInsight en Data Lake Store | Microsoft Docs
description: Gebruik van Azure Resource Manager-sjablonen maken en gebruiken van HDInsight-clusters met Azure Data Lake Store
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 8c5afc96cc8101345f00b5d435e9f393d22672de
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-resource-manager-template"></a>Een HDInsight-cluster maken met Data Lake Store met Azure Resource Manager-sjabloon
> [!div class="op_single_selector"]
> * [Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Met behulp van PowerShell (voor opslag van de standaard)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Met behulp van PowerShell (voor extra opslagruimte)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Resource Manager gebruiken](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Informatie over het gebruik van Azure PowerShell voor het configureren van een HDInsight-cluster met Azure Data Lake Store **als extra opslagruimte**.

Voor ondersteunde clustertypen, worden Data Lake Store gebruikt als een standaard-opslag- of aanvullende storage-account. Wanneer Data Lake Store als extra opslagruimte wordt gebruikt, is het standaardopslagaccount voor de clusters nog steeds Azure Storage-Blobs (WASB) en de cluster-gerelateerde bestanden (zoals Logboeken, enz.) worden nog steeds naar de standaard-opslag geschreven terwijl de gegevens die u wilt laten verwerken, kunnen worden opgeslagen in een Data Lake Store-account. Met behulp van Data Lake Store als extra storage-account heeft geen gevolgen voor prestaties of de mogelijkheid om te lezen/schrijven naar de opslag van het cluster.

## <a name="using-data-lake-store-for-hdinsight-cluster-storage"></a>Met behulp van Data Lake Store voor opslag van HDInsight-cluster

Hier volgen enkele belangrijke overwegingen voor het gebruik van HDInsight met Data Lake Store:

* De optie voor het maken van HDInsight-clusters met toegang tot Data Lake Store als standaard opslag beschikbaar voor HDInsight versie 3.5 en 3.6.

* De optie voor het maken van HDInsight-clusters met toegang tot Data Lake Store als extra opslag beschikbaar voor HDInsight versie 3.2, 3.4 3.5 en 3.6.

In dit artikel inrichten we een Hadoop-cluster met Data Lake Store als extra opslagruimte. Zie voor instructies over het maken van een Hadoop-cluster met Data Lake Store als standaard opslag [een HDInsight-cluster maken met Data Lake Store met Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).
* **Azure Active Directory Service-Principal**. De stappen in deze zelfstudie bevatten instructies voor het maken van een service-principal in Azure AD. U moet echter een Azure AD-beheerder om te kunnen maken van een service-principal. Als u een Azure AD-beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    **Als u niet een Azure AD-beheerder**, u kan niet worden de stappen die zijn vereist voor het maken van een service-principal uit te voeren. In dat geval moet uw Azure AD-beheerder eerst een service-principal maken voordat u een HDInsight-cluster met Data Lake Store maken kunt. Ook de service-principal moet worden gemaakt met een certificaat, zoals beschreven op [een service-principal maken met certificaat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-hdinsight-cluster-with-azure-data-lake-store"></a>Een HDInsight-cluster maken met Azure Data Lake Store
De Resource Manager-sjabloon en de vereisten voor het gebruik van de sjabloon zijn beschikbaar op GitHub op [een HDInsight Linux-cluster met nieuwe Data Lake Store implementeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage). Volg de instructies op deze koppeling te maken van een HDInsight-cluster met Azure Data Lake Store als extra opslagruimte.

De instructies in de bovenstaande koppeling vereisen PowerShell. Zorg voordat u met deze instructies begint, dat u aanmelden bij uw Azure-account. Een nieuw Azure PowerShell-venster openen vanaf het bureaublad en voer de volgende codefragmenten. Wanneer u wordt gevraagd om u aan te melden, zorg dan dat u zich aanmeldt als een van de beheerders/eigenaars van het abonnement:

```
# Log in to your Azure account
Login-AzureRmAccount

# List all the subscriptions associated to your account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

## <a name="upload-sample-data-to-the-azure-data-lake-store"></a>Voorbeeldgegevens uploadt naar de Azure Data Lake Store
De Resource Manager-sjabloon een nieuwe Data Lake Store-account maakt en koppelt u deze aan het HDInsight-cluster. Nu moet u voorbeeldgegevens uploadt naar Data Lake Store. U moet deze gegevens verderop in de zelfstudie taken uitvoeren vanuit een HDInsight-cluster die toegang tot gegevens in de Data Lake Store. Zie voor instructies over het uploaden van gegevens [een bestand uploaden naar uw Data Lake Store](data-lake-store-get-started-portal.md#uploaddata). Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken.

## <a name="set-relevant-acls-on-the-sample-data"></a>Stel relevante ACL's op de voorbeeldgegevens
Om te zorgen dat de voorbeeldgegevens die u uploadt, is toegankelijk vanaf het HDInsight-cluster, moet u ervoor zorgen dat de Azure AD-toepassing die wordt gebruikt voor het tot stand brengen identiteit tussen de HDInsight-cluster en de Data Lake Store toegang heeft tot het bestand of de map die u probeert te openen. U doet dit door de volgende stappen uitvoeren.

1. Zoek de naam van de Azure AD-toepassing die is gekoppeld aan het HDInsight-cluster en de Data Lake Store. Een manier om te zoeken naar de naam is de HDInsight-cluster-blade die u hebt gemaakt met behulp van de Resource Manager-sjabloon te openen, klikt u op de **Cluster AAD-identiteit** tabblad en zoekt u de waarde van **weergavenaam van Service-Principal**.
2. Nu bieden toegang tot deze Azure AD-toepassing op het bestand of de map die u wilt openen vanuit het HDInsight-cluster. Zie het instellen van de juiste ACL's op het bestand/map in Data Lake Store [beveiligen van gegevens in Data Lake Store](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Testtaken uitvoeren op het HDInsight-cluster te gebruiken van de Data Lake Store
Nadat u een HDInsight-cluster hebt geconfigureerd, kunt u testtaken uitvoeren op het cluster om te testen of de HDInsight-cluster toegang Data Lake Store tot. Om dit te doen, voeren we een voorbeeld Hive-taak die u maakt een tabel met de voorbeeldgegevens die u eerder hebt geüpload naar uw Data Lake Store.

In deze sectie wordt u SSH in een cluster in HDInsight Linux en voer de een Hive-voorbeeldquery. Als u een Windows-client gebruikt, wordt u aangeraden **PuTTY**, die kan worden gedownload vanaf [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Zie voor meer informatie over het gebruik van PuTTY [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Eenmaal zijn verbonden, start u de CLI Hive met behulp van de volgende opdracht:

   ```
   hive
   ```
2. De volgende instructies om een nieuwe tabel met de naam te maken met behulp van de CLI Voer **voertuigen** met behulp van de voorbeeldgegevens in de Data Lake Store:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
   SELECT * FROM vehicles LIMIT 10;
   ```

   Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

   ```
   1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
   1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
   1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
   1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
   1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
   1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
   1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
   1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
   1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
   1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1
   ```


## <a name="access-data-lake-store-using-hdfs-commands"></a>Toegang tot Data Lake Store met HDFS-opdrachten
Wanneer u het HDInsight-cluster voor het gebruik van Data Lake Store hebt geconfigureerd, kunt u de HDFS-shell-opdrachten voor toegang tot de store.

In deze sectie wordt u SSH in een HDInsight Linux-cluster en voer de HDFS-opdrachten. Als u een Windows-client gebruikt, wordt u aangeraden **PuTTY**, die kan worden gedownload vanaf [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Zie voor meer informatie over het gebruik van PuTTY [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Eenmaal zijn verbonden, gebruikt u de volgende opdracht uit HDFS-bestandssysteem voor het weergeven van de bestanden in de Data Lake Store.

```
hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/
```

Dit moet het bestand dat u eerder hebt geüpload naar de Data Lake Store weergeven.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder
```

U kunt ook de `hdfs dfs -put` opdracht voor sommige bestanden uploaden naar Data Lake Store en vervolgens gebruik `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.


## <a name="next-steps"></a>Volgende stappen
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Store](data-lake-store-copy-data-wasb-distcp.md)
* [Gebruik Data Lake Store met Azure HDInsight-clusters](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
