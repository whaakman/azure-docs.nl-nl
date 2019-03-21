---
title: Met behulp van Azure Classic CLI - Azure HDInsight Apache Hadoop-clusters beheren
description: Informatie over het gebruik van de klassieke Azure CLI voor het beheren van Apache Hadoop-clusters in Azure HDInsight.
services: hdinsight
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: tyfox
ms.openlocfilehash: 61be4088034d8cc557c67c5b6de4972766654a39
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58202381"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-using-the-azure-classic-cli"></a>Apache Hadoop-clusters in HDInsight met behulp van de klassieke Azure-CLI beheren
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Meer informatie over het gebruik van de [klassieke Azure-CLI](../cli-install-nodejs.md) voor het beheren van [Apache Hadoop](https://hadoop.apache.org/) -clusters in Azure HDInsight. De klassieke CLI is geïmplementeerd in Node.js. en kan worden gebruikt op elk platform dat ondersteuning biedt voor Node.js, zoals Windows, Mac en Linux.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Azure CLI voor klassieke** -Zie [installeren en configureren van de klassieke Azure-CLI](../cli-install-nodejs.md) voor installatie en configuratie-informatie.
* **Verbinding maken met Azure**, met de volgende opdracht:

    ```cli
    azure login
    ```
  
    Zie voor meer informatie over verificatie met een werk- of schoolaccount [verbinding maken met een Azure-abonnement vanuit de klassieke Azure-CLI](/cli/azure/authenticate-azure-cli).
* **Overschakelen naar de modus Azure Resource Manager** met de volgende opdracht:
  
    ```cli
    azure config mode arm
    ```

Als u Help-informatie, gebruikt de **-h** overschakelen.  Bijvoorbeeld:

```cli
azure hdinsight cluster create -h
```

## <a name="create-clusters-with-the-cli"></a>Clusters maken met de CLI
Zie [-clusters maken in HDInsight met behulp van de klassieke Azure-CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Clusterdetails tonen en vermelden
Gebruik de volgende opdrachten te kunnen aanbieden en clusterdetails weergeven:

```cli
azure hdinsight cluster list
azure hdinsight cluster show <Cluster Name>
```

![Opdrachtregel weergave van de lijst met clusters][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Clusters verwijderen
Gebruik de volgende opdracht om een cluster te verwijderen:

```cli
azure hdinsight cluster delete <Cluster Name>
```

U kunt ook een cluster verwijderen door de resourcegroep waarin het cluster te verwijderen. Let op: Hiermee worden alle resources in de groep met inbegrip van het standaardopslagaccount verwijderd.

```cli
azure group delete <Resource Group Name>
```

## <a name="scale-clusters"></a>Clusters schalen
De grootte van de Apache Hadoop-cluster wijzigen:

```cli
azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>
```


## <a name="enabledisable-http-access-for-a-cluster"></a>HTTP-toegang voor een cluster in-of uitschakelen

```cli
azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
azure hdinsight cluster disable-http-access [options] <Cluster Name>
```

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u verschillende HDInsight-cluster administratieve taken uit te voeren. Zie de volgende artikelen voor meer informatie:

* [Apache Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md)
* [HDInsight beheren met behulp van Azure PowerShell][hdinsight-admin-powershell]
* [Aan de slag met Azure HDInsight][hdinsight-get-started]
* [Het gebruik van de klassieke Azure-CLI][azure-command-line-tools]

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Clusters tonen en vermelden"
