---
title: Azure Storage-oplossingen voor R Server op een HDInsight - Azure | Microsoft Docs
description: Meer informatie over de verschillende opties voor opslag beschikbaar voor gebruikers met R Server op HDInsight
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 863277294fc0462e9221edffab1dd4e2001d7493
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a>Azure Storage-oplossingen voor op HDInsight R Server

Microsoft R Server op HDInsight heeft een aantal opslagoplossingen voor het persistent maken van gegevens, code of objecten die bevatten de resultaten van de analyse. Deze omvatten de volgende opties:

- [Azure-blobopslag](https://azure.microsoft.com/services/storage/blobs/)
- [Azure Data Lake Storage](https://azure.microsoft.com/services/data-lake-store/)
- [Azure File storage](https://azure.microsoft.com/services/storage/files/)

U hebt ook de optie van de toegang tot meerdere Azure storage-accounts of containers met uw HDI-cluster. Azure File storage is een handige gegevens voor de opslagoptie voor gebruik op de edge-knooppunt dat kunt u het koppelen van een Azure Storage-bestandsshare, bijvoorbeeld het Linux-bestandssysteem. Maar Azure-bestandsshares kunnen worden gekoppeld en die wordt gebruikt door een systeem waarop een ondersteund besturingssysteem zoals Windows of Linux. 

Wanneer u een Hadoop-cluster in HDInsight maakt, geeft u ofwel een **Azure storage** account of een **Data Lake store**. Een specifieke storage-container uit dat account bevat het bestandssysteem voor het cluster dat u (bijvoorbeeld Hadoop Distributed File System maakt). Zie voor meer informatie en richtlijnen:

- [Azure storage gebruiken met HDInsight](../hdinsight-hadoop-use-blob-storage.md)
- [Gebruik Data Lake Store met Azure HDInsight-clusters](../hdinsight-hadoop-use-data-lake-store.md). 

Zie voor meer informatie over de Azure storage-oplossingen [Inleiding tot Microsoft Azure Storage](../../storage/common/storage-introduction.md). 

Zie voor instructies over het selecteren van de meest geschikte opslagoptie gebruiken voor uw scenario [beslissen wanneer u Azure Blobs, Azure-bestanden of Azure gegevensschijven gebruiken](../../storage/common/storage-decide-blobs-files-disks.md) 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a>Azure Blob storage-accounts gebruiken met R Server

Als u meer dan één opslagaccount hebt opgegeven bij het maken van uw R Server-cluster, heeft de volgende instructies wordt uitgelegd hoe u een secundaire account gebruiken voor toegang tot gegevens en bewerkingen op R Server worden weergegeven. De volgende storage-accounts en container: **storage1** en standaardcontainer aangeroepen **container1**, en **storage2**.

> [!WARNING]
> Voor de doeleinden van prestaties, wordt het HDInsight-cluster gemaakt in hetzelfde Datacenter als het primaire opslagaccount die u opgeeft. Met behulp van een opslagaccount in een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

1. Met behulp van een SSH-client verbinding maken met het edge-knooppunt van het cluster als remoteuser.  

  + In Azure portal > servicepagina HDI-cluster > overzicht, klikt u op **Secure Shell (SSH)**.
  + Selecteer in de hostnaam, het edge-knooppunt (het omvat *ed-ssh.azurehdinsight.net* in de naam).
  + Kopieer de hostnaam.
  + Open een SSH-client, zoals PutTY of SmartTY en voer de hostnaam.
  + Voer remoteuser voor de gebruikersnaam, gevolgd door het wachtwoord van het cluster.
  
2. Kopieer het bestand mycsv.csv naar de map/share. 

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

3. Ga naar R Studio of een andere R-console en R code schrijven voor het knooppunt naam ingesteld op **standaard** en de locatie van het bestand dat u wilt openen.  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(nameNode=myNameNode, consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

Alle mappen en bestanden verwijzingen verwijzen naar het opslagaccount wasb://container1@storage1.blob.core.windows.net. Dit is de **standaard opslagaccount** die is gekoppeld aan het HDInsight-cluster.

Stel nu dat u wilt laten verwerken van een bestand met de naam mySpecial.csv die zich in de /private map van **container2** in **storage2**.

In uw code R, wijst u de naam van knooppunt verwijzing naar de **storage2** storage-account.


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

Alle verwijzingen van de map- en wijs nu het opslagaccount wasb://container2@storage2.blob.core.windows.net. Dit is de **naam knooppunt** die u hebt opgegeven.

U hoeft te configureren/User/RevoShare/<SSH username> directory op **storage2** als volgt:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a>Gebruik een Azure Data Lake store met R Server

Voor het gebruik van Data Lake winkels met uw HDInsight-account, moet u uw cluster toegang geven tot elke Azure Data Lake store die u wilt gebruiken. Zie voor instructies over het gebruik van de Azure-portal voor het maken van een HDInsight-cluster met een Azure Data Lake Store-account als de standaard-opslag of als een extra store [een HDInsight-cluster maken met Data Lake Store met Azure-portal](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

U gebruikt het archief in uw R-script veel net zoals een secundaire Azure storage-account zoals beschreven in de vorige procedure.

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a>Toegang tot cluster toevoegen aan uw Azure Data Lake-winkels
U toegang tot een Data Lake store met behulp van een Service-Principal voor Azure Active Directory (Azure AD) dat is gekoppeld aan uw HDInsight-cluster.

Toevoegen van een Azure AD-Service-Principal:

1. Wanneer u uw HDInsight-cluster maakt, selecteert u **Cluster AAD-identiteit** van de **gegevensbron** tabblad.

2. In de **Cluster AAD-identiteit** dialoogvenster onder **AD Service-Principal selecteren**, selecteer **nieuw**.

Nadat u de Service-Principal een naam geven en een wachtwoord voor het maken, klikt u op **ADLS-toegang beheren** de Service-Principal koppelen aan uw Data Lake-stores.

Het is ook mogelijk toegang tot cluster toevoegen aan een of meer Data Lake winkels na het maken van de cluster. Open de Azure portal-vermelding voor een Data Lake store en Ga naar **Data Explorer > toegang > toevoegen**. 

### <a name="how-to-access-the-data-lake-store-from-r-server"></a>Toegang tot de Data Lake store van R Server

Zodra u toegang tot een Data Lake store krijgen hebt, kunt u het archief in R Server op HDInsight de manier waarop u zou een secundair Azure storage-account. Het enige verschil is dat het voorvoegsel **wasb: / /** wijzigingen in **adl: / /** als volgt:


    # Point to the ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of the week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define the data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


De volgende opdrachten worden gebruikt voor het configureren van het Data Lake storage-account met de map RevoShare en het CSV-voorbeeldbestand uit het vorige voorbeeld toevoegen:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a>Azure File storage gebruiken met R Server

Er is ook een handige gegevens voor de opslagoptie voor gebruik op de edge-knooppunt genoemd Azure Files ((https://azure.microsoft.com/services/storage/files/). Hiermee kunt u een Azure Storage-bestandsshare naar het bestandssysteem van Linux te koppelen. Deze optie is handig voor het opslaan van gegevensbestanden, R-scripts en resultaatobjecten die mogelijk ook later nodig zijn, vooral wanneer het zinvol zijn het systeemeigen bestandssysteem gebruiken op de edge-knooppunt in plaats van HDFS. 

Een belangrijk voordeel van Azure Files is dat de bestandsshares kunnen worden gekoppeld en die wordt gebruikt door een systeem waarop een ondersteund besturingssysteem zoals Windows of Linux. Het kan bijvoorbeeld worden gebruikt door een andere HDInsight-cluster dat u of iemand in uw team heeft met een virtuele machine in Azure, of zelfs door een on-premises systeem. Zie voor meer informatie:

- [Azure File Storage gebruiken met Linux](../../storage/files/storage-how-to-use-files-linux.md)
- [Azure File storage gebruiken in Windows](../../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Volgende stappen

Nu u inzicht in de Azure-opslag-opties, gebruiken de volgende koppelingen voor het detecteren van de manieren om gegevens wetenschappelijke taken uitgevoerd met op HDInsight R Server.

* [Overzicht van op HDInsight R Server](r-server-overview.md)
* [Aan de slag met R server op Hadoop](r-server-get-started.md)
* [Opties voor compute-context voor R Server op HDInsight](r-server-compute-contexts.md)

