---
title: Een Service Fabric-cluster schalen in Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u snel een Service Fabric-cluster kunt schalen in Azure.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 010/01/2018
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: abc60d11a2e6b1f830822e93a8fef40be1181f07
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54426026"
---
# <a name="tutorial-scale-a-service-fabric-cluster-in-azure"></a>Zelfstudie: Een Service Fabric-cluster schalen in Azure

Deze zelfstudie is deel twee van een serie. Hier ziet u hoe u een bestaand cluster kunt in- en uitschalen. Aan het einde van deze zelfstudie weet u hoe u een cluster kunt schalen en eventuele resterende resources kunt opschonen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het aantal clusterknooppunten lezen
> * Clusterknooppunten toevoegen (uitschalen)
> * Clusterknooppunten verwijderen (inschalen)

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een veilig [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) maken in Azure met behulp van een sjabloon
> * Een cluster in- of uitschalen
> * [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)
> * [Een cluster verwijderen](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Installeer de [Azure Powershell-module, versie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) of de [Azure CLI](/cli/azure/install-azure-cli).
* Maak een veilig [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) in Azure
* Als u een Windows-cluster implementeert, richt u een Windows-ontwikkelomgeving in. Installeer [Visual Studio 2017](https://www.visualstudio.com) en de workloads voor **Azure-ontwikkeling**, **ASP.NET-ontwikkeling en webontwikkeling** en **.NET Core platformoverschrijdende ontwikkeling**.  Richt vervolgens een [.NET-ontwikkelomgeving in](service-fabric-get-started.md).
* Als u een Linux-cluster implementeert, richt u een Java-ontwikkelomgeving in voor [Linux](service-fabric-get-started-linux.md) of [Mac OS](service-fabric-get-started-mac.md).  Installeer de [Service Fabric CLI](service-fabric-cli.md).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij uw Azure-account en selecteer uw abonnement voordat u Azure-opdrachten gaat uitvoeren.

```powershell
Connect-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Om dit deel van de zelfstudie te voltooien, moet u verbinding maken met het Service Fabric-cluster en de virtuele-machineschaalset (die als host fungeert voor het cluster). De virtuele-machineschaalset is de Azure-resource die als host fungeert voor Service Fabric op Azure.

Wanneer u verbinding maakt met een cluster, kunt u hieruit informatie opvragen. U kunt het cluster gebruiken om te weten te komen welke knooppunten het cluster herkent. Om verbinding te maken met het cluster wordt in de volgende code hetzelfde certificaat gebruikt dat ook in het eerste deel van deze serie is gebruikt. Let erop dat u de variabelen `$endpoint` en `$thumbprint` instelt op uw waarden.

```powershell
$endpoint = "<mycluster>.southcentralus.cloudapp.azure.com:19000"
$thumbprint = "63EB5BA4BC2A3BADC42CA6F93D6F45E5AD98A1E4"

Connect-ServiceFabricCluster -ConnectionEndpoint $endpoint `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint $thumbprint `
          -FindType FindByThumbprint -FindValue $thumbprint `
          -StoreLocation CurrentUser -StoreName My

Get-ServiceFabricClusterHealth
```

```azurecli
sfctl cluster select --endpoint https://aztestcluster.southcentralus.cloudapp.azure.com:19080 \
--pem ./aztestcluster201709151446.pem --no-verify
```

Wanneer de verbinding is gemaakt, kunt u een opdracht gebruiken om de status van elk knooppunt in het cluster op te vragen. Voor **PowerShell** gebruikt u de opdracht `Get-ServiceFabricClusterHealth` en voor **sfctl** de opdracht `sfctl cluster select`.

## <a name="scale-out"></a>Uitschalen

Wanneer u uitschaalt, voegt u meer instanties van de virtuele machine toe aan de schaalset. Deze instanties worden de knooppunten die door Service Fabric worden gebruikt. Service Fabric weet wanneer er meer exemplaren aan de schaalset zijn toegevoegd (door uitschaling) en reageert daar automatisch op. Met de volgende code verkrijgt u een schaalset op naam en verhoogt u de **capaciteit** van de schaalset met 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Met deze code stelt u de capaciteit in op 6.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>Inschalen

Inschalen is hetzelfde als uitschalen; u gebruikt alleen een lagere waarde voor de **capaciteit**. Wanneer u de schaalset inschaalt, verwijdert u instanties van de virtuele machine uit de schaalset. Normaal gesproken merkt Service Fabric niets van wat is er gebeurd en lijkt het alsof er een knooppunt verloren is gegaan. Service Fabric rapporteert daarom een onjuiste status voor het cluster. Om deze onjuiste status te voorkomen, moet u aan Service Fabric doorgeven dat u verwacht dat het knooppunt verdwijnt.

### <a name="remove-the-service-fabric-node"></a>Het Service Fabric-knooppunt verwijderen

> [!NOTE]
> Dit gedeelte is alleen van toepassing op de *Bronzen* duurzaamheidslaag. Voor meer informatie over duurzaamheid raadpleegt u [Service Fabric cluster capacity planning][durability] (Capaciteitsplanning voor Service Fabric-clusters).

Als u de knooppunten van het cluster gelijkmatig verdeeld wilt houden in upgrade- en foutdomeinen en op die manier gelijkmatig gebruik wilt inschakelen, moet het meest recent gemaakte knooppunt eerst worden verwijderd. Met andere woorden - de knooppunten moeten op basis van 'last in, first out' worden verwijderd. Het meest recent gemaakte knooppunt heeft de hoogste `virtual machine scale set InstanceId`-eigenschapswaarde. Met de onderstaande codevoorbeelden wordt het meest recent gemaakte knooppunt geretourneerd.

```powershell
Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1
```

```azurecli
sfctl node list --query "sort_by(items[*], &name)[-1]"
```

Het Service Fabric-cluster moet weten dat dit knooppunt zal worden verwijderd. Er zijn drie stappen die u moet uitvoeren:

1. Schakel het knooppunt uit zodat het geen replica meer is voor gegevens.  
PowerShell: `Disable-ServiceFabricNode`  
sfctl: `sfctl node disable`

2. Stop het knooppunt zodat de Service Fabric-runtime netjes wordt afgesloten en uw app een beëindigingsaanvraag ontvangt.  
PowerShell: `Start-ServiceFabricNodeTransition -Stop`  
sfctl: `sfctl node transition --node-transition-type Stop`

2. Verwijder het knooppunt uit het cluster.  
PowerShell: `Remove-ServiceFabricNodeState`  
sfctl: `sfctl node remove-state`

Nadat deze drie stappen op het knooppunt zijn toegepast, kan het knooppunt worden verwijderd uit de schaalset. Als u naast de [bronzen][durability] duurzaamheidslaag nog een andere duurzaamheidslaag gebruikt, worden deze stappen voor u uitgevoerd wanneer de schaalsetinstantie wordt verwijderd.

In het volgende codeblok wordt het laatst gemaakte knooppunt opgehaald en wordt het knooppunt uitgeschakeld, gestopt en uit het cluster verwijderd.

```powershell
#### After you've connected.....
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300

    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }

    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

In onderstaande **sfctl**-code wordt de volgende opdracht gebruikt om de waarde van **knooppuntnaam** op te halen van het laatst gemaakte knooppunt: `sfctl node list --query "sort_by(items[*], &name)[-1].name"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Gebruik de volgende **sfctl**-query's om de status van elke stap te controleren
>
> **Deactiveringsstatus controleren**
> `sfctl node list --query "sort_by(items[*], &name)[-1].nodeDeactivationInfo"`
>
> **Stopstatus controleren**
> `sfctl node list --query "sort_by(items[*], &name)[-1].isStopped"`
>

### <a name="scale-in-the-scale-set"></a>De schaalset inschalen

Nu het Service Fabric-knooppunt is verwijderd uit het cluster, kan de virtuele-machineschaalset worden ingeschaald. In onderstaand voorbeeld wordt de capaciteit van de schaalset verminderd met 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Met deze code stelt u de capaciteit in op 5.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het aantal clusterknooppunten lezen
> * Clusterknooppunten toevoegen (uitschalen)
> * Clusterknooppunten verwijderen (inschalen)

Daarna gaat u verder met de volgende zelfstudie, waarin u leert hoe u een upgrade uitvoert van de runtime van een cluster.
> [!div class="nextstepaction"]
> [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
