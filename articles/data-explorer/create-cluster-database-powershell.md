---
title: 'Quickstart: Een cluster van Azure Data Explorer en -database maken met behulp van PowerShell'
description: Informatie over het maken van een cluster van Azure Data Explorer en -database met behulp van PowerShell
services: data-explorer
author: oflipman
ms.author: oflipman
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: quickstart
ms.date: 03/17/2019
ms.openlocfilehash: 650bdc5cdf99645bc2be6c8e85737dacd10a6b27
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287514"
---
# <a name="create-an-azure-data-explorer-cluster-and-database-by-using-powershell"></a>Een cluster van Azure Data Explorer en -database maken met behulp van PowerShell

> [!div class="op_single_selector"]
> * [Portal](create-cluster-database-portal.md)
> * [CLI](create-cluster-database-cli.md)
> * [PowerShell](create-cluster-database-powershell.md)
> * [C#](create-cluster-database-csharp.md)
> * [Python](create-cluster-database-python.md)
>  


In deze Quick Start wordt beschreven hoe u een cluster van Azure Data Explorer en -database maken met behulp van PowerShell.

U kunt PowerShell-cmdlets en scripts uitvoeren in Windows, Linux, of in [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) maken en configureren [Azure Data Explorer](https://docs.microsoft.com/azure/kusto/ ).

De [ **Az.Kusto**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto ). Met Azure PowerShell en **Az.Kusto**, kunt u de volgende taken uitvoeren:

## <a name="prerequisites"></a>Vereisten

U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="configure-parameters"></a>Parameters configureren

De volgende stappen zijn niet vereist als u opdrachten in Azure Cloud Shell uitvoert. Als u de CLI lokaal uitvoert, volgt u deze stappen om u aan te melden bij Azure en uw huidige abonnement in te stellen:

1. Voer de volgende opdracht uit om u aan te melden bij Azure:

    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Stel het abonnement in waarin u uw cluster wilt maken.

    ```azurepowershell-interactive
     Set-AzContext -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ```
3. Az.Kusto-module installeren op uw apparaat:
    
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

2. Voer de volgende opdracht uit om te controleren of het cluster is gemaakt:

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

2. Voer de volgende opdracht uit om de database te bekijken die u hebt gemaakt:

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

U vindt meer Az.Kusto opdrachten [ **hier**](https://docs.microsoft.com/powershell/module/az.kusto/?view=azps-1.4.0#kusto )

> [!div class="nextstepaction"]
> [Snelstart: Opname van gegevens met behulp van de Azure Data Explorer .NET Standard SDK (Preview)](net-standard-ingest-data.md)
