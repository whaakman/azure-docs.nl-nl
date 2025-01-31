---
title: Een Service Fabric-cluster verwijderen in Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u een door Azure gehost Service Fabric-cluster verwijdert, inclusief de bijbehorende resources. U kunt de resourcegroep met het cluster verwijderen of resources selectief verwijderen.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/26/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 0e5137a8183f378ee5960846e281222c6ecaaa47
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66158074"
---
# <a name="tutorial-remove-a-service-fabric-cluster-running-in-azure"></a>Zelfstudie: Een Service Fabric-cluster dat in Azure wordt uitgevoerd, verwijderen

Deze zelfstudie is deel vijf van een reeks en ziet u hoe u het verwijderen van een Service Fabric-cluster in Azure uitvoert. Als u een Service Fabric-cluster volledig wilt verwijderen, moet u dus ook alle resources waar het cluster gebruik van maakt, verwijderen. U hebt twee opties: verwijder de resourcegroep waarin het cluster zich bevindt (waarmee de clusterresource en alle andere resources in de resourcegroep worden verwijderd), of verwijder de clusterresource en de bijbehorende resources specifiek (maar niet de andere resources uit de resourcegroep).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een resourcegroep en de bijhorende resources verwijderen
> * Selectief resources uit een resourcegroep verwijderen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een beveiligd [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) maken in Azure met behulp van een sjabloon
> * [Een cluster bewaken](service-fabric-tutorial-monitor-cluster.md)
> * [Een cluster in- of uitschalen](service-fabric-tutorial-scale-cluster.md)
> * [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)
> * Een cluster verwijderen


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Installeer [Azure Powershell](https://docs.microsoft.com/powershell/azure//install-Az-ps) of [Azure CLI](/cli/azure/install-azure-cli).
* Een beveiligd [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) maken in Azure

## <a name="delete-the-resource-group-containing-the-service-fabric-cluster"></a>Verwijder de resourcegroep die het Service Fabric-cluster bevat
De eenvoudigste manier om het cluster en alle resources te verwijderen, is om de resourcegroep te verwijderen.

Aanmelden bij Azure en selecteer de abonnements-ID die u wilt verwijderen van het cluster.  U kunt uw abonnements-id vinden door u aan te melden bij [Azure Portal](https://portal.azure.com). Verwijder de resourcegroep en alle clusterbronnen met behulp van de [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) cmdlet of [az group delete](/cli/azure/group?view=azure-cli-latest) opdracht.

```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Remove-AzResourceGroup -Name $groupname -Force
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az group delete --name $ResourceGroupName
```

## <a name="selectively-delete-the-cluster-resource-and-the-associated-resources"></a>Clusterresources en de bijbehorende resources gericht verwijderen
Als uw resourcegroep alleen resources bevat die gerelateerd zijn aan het Service Fabric-cluster da tu wilt verwijderen, is het eenvoudiger om de volledige resourcegroep te verwijderen. Als u de resources uit uw resourcegroep selectief wilt verwijderen en u de resources die niet gerelateerd zijn aan het cluster, wilt behouden, volgt u deze stappen.

Lijst van resources in de resourcegroep:

```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId <guid>
$groupname = "sfclustertutorialgroup"
Get-AzResource -ResourceGroupName $groupname | ft
```

```azurecli
az login
az account set --subscription <guid>
ResourceGroupName="sfclustertutorialgroup"
az resource list --resource-group $ResourceGroupName
```

Voer voor elk van de resources die u wilt verwijderen het volgende script uit:

```powershell
Remove-AzResource -ResourceName "<name of the Resource>" -ResourceType "<Resource Type>" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "<Resource Type>" --resource-group $ResourceGroupName
```

Als u de clusterresource wilt verwijderen, voert u het volgende script uit:

```powershell
Remove-AzResource -ResourceName "<name of the Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName $groupname -Force
```

```azurecli
az resource delete --name "<name of the Resource>" --resource-type "Microsoft.ServiceFabric/clusters" --resource-group $ResourceGroupName
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een resourcegroep en de bijhorende resources verwijderen
> * Selectief resources uit een resourcegroep verwijderen

Nu u deze zelfstudie hebt voltooid, kunt de volgende zaken doen:
* Ontdekken hoe u een Service Fabric-cluster inspecteert en beheer met [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
* Ontdekken hoe u [het Windows-besturingssysteem patcht](service-fabric-patch-orchestration-application.md) voor de clusterknooppunten.
* Ontdekken hoe u gebeurtenissen aggregeert en verzamelt voor [Windows-clusters](service-fabric-diagnostics-event-aggregation-wad.md) en hoe u [Log Analytics instelt](service-fabric-diagnostics-oms-setup.md) voor het bewaken van clustergebeurtenissen.
