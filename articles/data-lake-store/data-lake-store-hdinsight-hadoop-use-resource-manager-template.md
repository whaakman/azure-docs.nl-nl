---
title: Azure-sjablonen gebruiken om te maken van HDInsight met Azure Data Lake Storage Gen1 | Microsoft Docs
description: Azure Resource Manager-sjablonen gebruiken om te maken en gebruiken van HDInsight-clusters met Azure Data Lake Storage Gen1
services: data-lake-store,hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 8ef8152f-2121-461e-956c-51c55144919d
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 5b98513625a6179585601320d45996396fca7207
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54035900"
---
# <a name="create-an-hdinsight-cluster-with-azure-data-lake-storage-gen1-using-azure-resource-manager-template"></a>Een HDInsight-cluster met Azure Data Lake Storage Gen1 met behulp van Azure Resource Manager-sjabloon maken
> [!div class="op_single_selector"]
> * [Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Gebruik van PowerShell (standaardopslag)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Met behulp van PowerShell (voor extra opslag)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Met behulp van Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Informatie over het gebruik van Azure PowerShell een HDInsight-cluster configureren met Azure Data Lake Storage Gen1 **als extra opslag**.

Voor ondersteunde clustertypen, kan Data Lake Storage Gen1 worden gebruikt als een standaardopslag of een extra opslagaccount. Wanneer Data Lake Storage Gen1 wordt gebruikt als extra opslag, het standaardopslagaccount voor de clusters wordt Azure Storage Blobs (WASB) en de cluster-gerelateerde bestanden (zoals Logboeken, enzovoort) nog steeds worden geschreven naar de standaardopslag, terwijl de gegevens die u wilt proces kan worden opgeslagen in een Data Lake Storage Gen1-account. Met behulp van Data Lake Storage Gen1 als een extra opslagaccount heeft geen invloed op prestaties of de mogelijkheid om te lezen/schrijven naar de opslag van het cluster.

## <a name="using-data-lake-storage-gen1-for-hdinsight-cluster-storage"></a>Met behulp van Data Lake Storage Gen1 voor opslag voor HDInsight-cluster

Hier volgen enkele belangrijke overwegingen voor het gebruik van HDInsight met Data Lake Storage Gen1:

* Optie voor het maken van HDInsight-clusters met toegang tot Data Lake Storage Gen1 als standaardopslag beschikbaar voor HDInsight versie 3.5 en 3.6 is.

* Optie voor het maken van HDInsight-clusters met toegang tot Data Lake Storage Gen1 als extra opslag beschikbaar voor HDInsight versie 3.2, 3.4, 3.5 en 3.6 is.

In dit artikel wordt er een Hadoop-cluster met Data Lake Storage Gen1 ingericht als extra opslag. Zie voor instructies over het maken van een Hadoop-cluster met Data Lake Storage Gen1 als standaardopslag [maken van een HDInsight-cluster met Data Lake Storage Gen1 met behulp van Azure Portal](data-lake-store-hdinsight-hadoop-use-portal.md).

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).
* **Azure Active Directory Service-Principal**. Stappen in deze zelfstudie vindt instructies voor het maken van een service-principal in Azure AD. U moet echter een Azure AD-beheerder om te kunnen maken van een service-principal. Als u een Azure AD-beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    **Als u niet een Azure AD-beheerder**, u niet mogelijk om uit te voeren van de stappen die nodig zijn om een serviceprincipal te maken. In dat geval moet de Azure AD-beheerder eerst een service-principal maken voordat u een HDInsight-cluster met Data Lake Storage Gen1 kunt maken. Ook de service-principal moet worden gemaakt met een certificaat, zoals beschreven op [een service-principal maken met certificaat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-certificate-from-certificate-authority).

## <a name="create-an-hdinsight-cluster-with-data-lake-storage-gen1"></a>Een HDInsight-cluster maken met Data Lake Storage Gen1
De Resource Manager-sjabloon en de vereisten voor het gebruik van de sjabloon, zijn beschikbaar op GitHub op [implementeert u een HDInsight Linux-cluster met de nieuwe Data Lake Storage Gen1](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage). Volg de instructies op deze koppeling om u te maken van een HDInsight-cluster met Data Lake Storage Gen1 als de extra opslagruimte.

De instructies op de koppeling hierboven genoemde is PowerShell vereist. Voordat u met deze instructies begint, zorg er dan voor dat u zich aanmeldt bij uw Azure-account. Vanaf het bureaublad een nieuw Azure PowerShell-venster openen en voer de volgende codefragmenten. Wanneer u hierom wordt gevraagd om aan te melden, zorg er dan voor dat u zich aanmeldt als een van de beheerders/eigenaar van het abonnement:

```
# Log in to your Azure account
Connect-AzureRmAccount

# List all the subscriptions associated to your account
Get-AzureRmSubscription

# Select a subscription
Set-AzureRmContext -SubscriptionId <subscription ID>
```

De sjabloon implementeert deze resourcetypen:

* [Microsoft.DataLakeStore/accounts](/azure/templates/microsoft.datalakestore/accounts)
* [Microsoft.Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts)
* [Microsoft.HDInsight/clusters](/azure/templates/microsoft.hdinsight/clusters)

## <a name="upload-sample-data-to-data-lake-storage-gen1"></a>Voorbeeldgegevens uploaden naar Data Lake Storage Gen1
De Resource Manager-sjabloon maakt u een nieuw account voor Data Lake Storage Gen1 en associeert deze met het HDInsight-cluster. Nu moet u enkele voorbeeldgegevens uploaden naar Data Lake Storage Gen1. U moet deze gegevens later in de zelfstudie taken uit te voeren vanuit een HDInsight-cluster die toegang hebben tot gegevens in het Data Lake Storage Gen1-account. Zie voor instructies over het uploaden van gegevens, [een bestand uploaden naar uw Data Lake Storage Gen1 account](data-lake-store-get-started-portal.md#uploaddata). Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken.

## <a name="set-relevant-acls-on-the-sample-data"></a>Relevante ACL's op de voorbeeldgegevens instellen
Als u wilt controleren of de voorbeeldgegevens die u uploadt, is toegankelijk vanuit het HDInsight-cluster, moet u ervoor zorgen dat de Azure AD-toepassing die wordt gebruikt voor het tot stand brengen identiteit tussen het HDInsight-cluster en de Data Lake Storage Gen1 heeft toegang tot het bestand of de map die u wilt de toegang. U doet dit door de volgende stappen uitvoeren.

1. Zoek de naam van de Azure AD-toepassing die is gekoppeld aan het HDInsight-cluster en de Gen1 van Data Lake Storage-account. Eén manier om te zoeken naar de naam is het openen van de blade van de HDInsight-cluster die u hebt gemaakt met behulp van de Resource Manager-sjabloon, klikt u op de **Cluster AAD-identiteit** tabblad en zoek de waarde van **weergavenaam van Service-Principal**.
2. Nu, bieden toegang tot deze Azure AD-toepassing op het bestand/de map die u wilt openen via het HDInsight-cluster. Zie voor meer informatie over het instellen van de juiste ACL's op het bestand/map in Data Lake Storage Gen1 [beveiligen van gegevens in Data Lake Storage Gen1](data-lake-store-secure-data.md#filepermissions).

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-data-lake-storage-gen1"></a>Testtaken uitvoeren op het HDInsight-cluster te gebruiken van Data Lake Storage Gen1
Nadat u een HDInsight-cluster hebt geconfigureerd, kunt u testtaken kunt uitvoeren op het cluster om te testen of Data Lake Storage Gen1 toegang hebt tot het HDInsight-cluster. Om dit te doen, wordt een voorbeeld-Hive-taak die u maakt een tabel met behulp van de voorbeeldgegevens die u eerder hebt geüpload naar uw Data Lake Storage Gen1-account uitgevoerd.

In deze sectie wordt u SSH in een HDInsight Linux-cluster en voer de een voorbeeld-Hive-query. Als u van een Windows-client gebruikmaakt, wordt u aangeraden **PuTTY**, die kan worden gedownload vanaf [ http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Zie voor meer informatie over het gebruik van PuTTY [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Eenmaal verbinding hebben, start u de CLI Hive met behulp van de volgende opdracht uit:

   ```
   hive
   ```
2. De volgende instructies om een nieuwe tabel met de naam te maken met behulp van de CLI, voer **voertuigen** met behulp van de voorbeeldgegevens in Data Lake Storage Gen1:

   ```
   DROP TABLE vehicles;
   CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestoragegen1>.azuredatalakestore.net:443/';
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


## <a name="access-data-lake-storage-gen1-using-hdfs-commands"></a>Toegang tot Data Lake Storage Gen1 met HDFS-opdrachten
Nadat u het HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 hebt geconfigureerd, kunt u de HDFS-shell-opdrachten kunt gebruiken voor toegang tot de store.

In deze sectie wordt u SSH in een HDInsight Linux-cluster en de HDFS-opdrachten. Als u van een Windows-client gebruikmaakt, wordt u aangeraden **PuTTY**, die kan worden gedownload vanaf [ http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html ](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Zie voor meer informatie over het gebruik van PuTTY [SSH gebruiken met Linux gebaseerde Hadoop op HDInsight vanaf Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Eenmaal verbinding hebben, gebruikt u de volgende opdracht uit voor HDFS-bestandssysteem om de bestanden in de Gen1 van Data Lake Storage-account weer te geven.

```
hdfs dfs -ls adl://<Data Lake Storage Gen1 account name>.azuredatalakestore.net:443/
```

Dit moet het bestand dat u eerder hebt geüpload naar Data Lake Storage Gen1 weergegeven.

```
15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
Found 1 items
-rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestoragegen1.azuredatalakestore.net:443/mynewfolder
```

U kunt ook de `hdfs dfs -put` opdracht voor sommige bestanden uploaden naar Data Lake Storage Gen1 en gebruik vervolgens `hdfs dfs -ls` om te controleren of de bestanden zijn geüpload.


## <a name="next-steps"></a>Volgende stappen
* [Gegevens kopiëren van Azure Storage-Blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-wasb-distcp.md)
* [Data Lake Storage Gen1 met Azure HDInsight-clusters gebruiken](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
