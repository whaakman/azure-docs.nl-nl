---
title: 'Quickstart: Een cluster van Azure Data Explorer en -database maken met behulp van PowerShell'
description: Informatie over het maken van een cluster van Azure Data Explorer en -database met behulp van PowerShell
services: data-explorer
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 03/25/2019
ms.openlocfilehash: 86fbf5801e9ff1c8bd9dead8be14aeeea1b58a29
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472477"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>Een cluster van Azure Data Explorer en -database maken met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  

Azure Data Explorer is een snelle, volledig beheerde service voor gegevensanalyses waarmee grote hoeveelheden gegevens van toepassingen, websites, IoT-apparaten en dergelijke in real-time kunnen worden geanalyseerd. Als u Azure Data Explorer wilt gebruiken, maakt u eerst een cluster. Daarna maakt u een of meer databases in het cluster. De volgende stap is het opnemen (laden) van gegevens in een database, zodat u er query's op kunt uitvoeren. In deze snelstartgids maakt u een cluster en een database met behulp van Powershell. U kunt PowerShell-cmdlets en scripts uitvoeren in Windows, Linux, of in [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) met [Az.Kusto](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto) maken en configureren van Azure Data Explorer-clusters en databases.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de Azure CLI lokaal te installeren en te gebruiken, moet u voor deze quickstart versie 2.0.4 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om uw versie te controleren. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="configure-parameters"></a>Parameters configureren

De volgende stappen zijn niet vereist als u opdrachten in Azure Cloud Shell uitvoert. Als u de CLI lokaal uitvoert, voert u de stappen 1 en 2 om aan te melden bij Azure en in te stellen van uw huidige abonnement:

1. Voer de volgende opdracht uit om u aan te melden bij Azure:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

1. Stel het abonnement waar u uw cluster te maken:

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
1. Bij het uitvoeren van Azure CLI lokaal of in de Azure Cloud Shell, moet u de module Az.Kusto installeren op uw apparaat:
    
    ```azurepowershell-interactive
     Install-Module -Name Az.Kusto  
    ```

## <a name="create-the-azure-data-explorer-cluster"></a>Het Azure Data Explorer-cluster maken

1. Maak uw cluster met behulp van de volgende opdracht:

    ```azurepowershell-interactive
     New-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster -Location 'Central US' -Sku D13_v2 -Capacity 10
    ```

   |**Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**|
   |---|---|---|
   | Name | *mykustocluster* | De gewenste naam van uw cluster.|
   | Sku | *D13_v2* | De SKU die wordt gebruikt voor uw cluster. |
   | ResourceGroupName | *testrg* | Naam van de resourcegroep waar het cluster wordt gemaakt. |

    Er zijn aanvullende optionele parameters die u kunt gebruiken, zoals de capaciteit van het cluster.

1. Voer de volgende opdracht uit om te controleren of het cluster is gemaakt:

    ```azurepowershell-interactive
    Get-AzKustoCluster -Name mykustocluster --ResourceGroupName testrg
    ```

Als het resultaat `provisioningState` met waarde `Succeeded` bevat, is het maken van het cluster geslaagd.

## <a name="create-the-database-in-the-azure-data-explorer-cluster"></a>De database in het Azure Data Explorer-cluster maken

1. Maak uw database met behulp van de volgende opdracht:

    ```azurepowershell-interactive
    New-AzKustoDatabase -ResourceGroupName testrg -ClusterName mykustocluster -Name mykustodatabase -SoftDeletePeriod 3650:00:00:00 -HotCachePeriod 3650:00:00:00
    ```

   |**Instelling** | **Voorgestelde waarde** | **Beschrijving van veld**|
   |---|---|---|
   | ClusterName | *mykustocluster* | De naam van het cluster waar de database wordt gemaakt.|
   | Name | *mykustodatabase* | De naam van uw database.|
   | ResourceGroupName | *testrg* | Naam van de resourcegroep waar het cluster wordt gemaakt. |
   | SoftDeletePeriod | *3650:00:00:00* | Hoe lang gegevens worden opgeslagen om query's erop te kunnen uitvoeren. |
   | HotCachePeriod | *3650:00:00:00* | Hoe lang gegevens worden opgeslagen in de cache. |

1. Voer de volgende opdracht uit om de database te bekijken die u hebt gemaakt:

    ```azurepowershell-interactive
    Get-AzKustoDatabase -ClusterName mykustocluster --ResourceGroupName testrg -Name mykustodatabase
    ```

U hebt nu een cluster en een database.

## <a name="clean-up-resources"></a>Resources opschonen

* Als u onze andere snelstarts en zelfstudies wilt volgen, behoudt u de gemaakte resources.
* Als u resources wilt opschonen, moet u het cluster verwijderen. Wanneer u een cluster verwijdert, worden alle databases hierin ook verwijderd. Gebruik de volgende opdracht om uw cluster te verwijderen:

    ```azurepowershell-interactive
    Remove-AzKustoCluster -ResourceGroupName testrg -Name mykustocluster
    ```

## <a name="next-steps"></a>Volgende stappen

* [Aanvullende Az.Kusto opdrachten](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto)
* [Snelstart: Opname van gegevens met behulp van de Azure Data Explorer .NET Standard SDK (Preview)](net-standard-ingest-data.md)
