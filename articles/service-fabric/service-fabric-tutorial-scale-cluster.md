---
title: Een Azure Service Fabric-cluster schalen | Microsoft Docs
description: Informatie over het snel schalen Service Fabric-cluster.
services: service-fabric
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/24/2017
ms.author: adegeo
ms.openlocfilehash: d203580f676d55acbad4936160982a40592af1d0
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="scale-a-service-fabric-cluster"></a>Een Service Fabric-cluster schalen

Deze zelfstudie maakt deel uit van een reeks en ziet u hoe u het bestaande cluster uit- en schalen. Wanneer u hebt voltooid, hebt u wellicht schalen van uw cluster en het opschonen van eventuele resterende resources.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Het aantal clusters knooppunt lezen
> * Toevoegen van de clusterknooppunten (scale-out)
> * Verwijder de clusterknooppunten (schaal in)

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint:
- Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Installeer de [Azure Powershell-moduleversie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) of [Azure CLI 2.0](/cli/azure/install-azure-cli).
- Maken van een veilige [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) of [Linux-cluster](service-fabric-tutorial-create-vnet-and-linux-cluster.md) op Azure
- Als u een Windows-cluster implementeert, kunt u een Windows-ontwikkelomgeving instellen. Installeer [Visual Studio 2017](http://www.visualstudio.com) en de **ontwikkelen van Azure**, **ASP.NET en web ontwikkeling**, en **.NET Core platformoverschrijdende ontwikkeling**werkbelastingen.  Stel een [.NET ontwikkelomgeving](service-fabric-get-started.md).
- Als u een Linux-cluster implementeert, stelt u een Java-ontwikkelomgeving op [Linux](service-fabric-get-started-linux.md) of [Mac OS](service-fabric-get-started-mac.md).  Installeer de [Service Fabric CLI](service-fabric-cli.md). 

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Aanmelden bij uw Azure-account, selecteer uw abonnement voordat u Azure-opdrachten uitvoeren.

```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

```azurecli
az login
az account set --subscription <guid>
```

## <a name="connect-to-the-cluster"></a>Verbinding maken met het cluster

Als u wilt dit deel van de zelfstudie voltooien, moet u verbinding maken met zowel de Service Fabric-cluster en de virtuele-machineschaalset (die als host fungeert voor het cluster). De virtuele-machineschaalset is de Azure resource die als host fungeert voor Service Fabric in Azure.

Als u verbinding met een cluster, kunt u deze kunt opvragen voor meer informatie. Voor meer informatie over welke knooppunten het cluster is op de hoogte kunt u het cluster. Verbinding maken met het cluster in de volgende code gebruikt hetzelfde certificaat dat is gemaakt in het eerste deel van deze reeks. Zorg ervoor dat u de `$endpoint` en `$thumbprint` variabelen op uw waarden.

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

Nu dat u verbonden bent, kunt u een opdracht voor het ophalen van de status van elk knooppunt in het cluster. Voor PowerShell, gebruikt u de `Get-ServiceFabricClusterHealth` opdracht, en voor **sfctl** gebruiken de '' opdracht.

## <a name="scale-out"></a>Uitschalen

Wanneer u uitschalen, kunt u meer exemplaren van de virtuele machine toevoegen aan de schaalaanpassingsset. Deze instanties, worden de knooppunten die gebruikmaakt van Service Fabric. Service Fabric kent Wanneer de schaalaanpassingsset heeft meer exemplaren (door het uitbreiden) toegevoegd en automatisch reageert. De volgende code haalt een schaal instellen op naam en verhoogt de **capaciteit** van de schaal van 1 instellen.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Deze code wordt de capaciteit ingesteld op 6.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="scale-in"></a>Schalen in

Schalen hetzelfde is als uitbreiden, behalve het gebruik van een lagere **capaciteit** waarde. Wanneer u in de schaalset schaalt, kunt u exemplaren van de virtuele machine verwijderen uit de schaalaanpassingsset. Normaal gesproken Service Fabric is merkt niets van wat is er gebeurd en het geeft aan dat een knooppunt is kwijtgeraakt. Service Fabric rapporten klikt u vervolgens een onjuiste status voor het cluster. Om te voorkomen dat slechte staat, moet u het service fabric informeren dat u het knooppunt niet meer zichtbaar verwacht.

### <a name="remove-the-service-fabric-node"></a>Verwijder de service fabric-knooppunt

> [!NOTE]
> Dit onderdeel is alleen van toepassing op de *Brons* duurzaamheid laag. Zie voor meer informatie over duurzaamheid [capaciteitsplanning voor Service Fabric-cluster][durability].

Wanneer u in een virtuele-machineschaalset schaalt, verwijdert de schaal instellen (in de meeste gevallen) de virtuele machine-instantie die het laatst is gemaakt. Daarom moet u de overeenkomende, het laatst is gemaakt, service fabric-knooppunt gevonden. U vindt dit laatste knooppunt door het controleren van de grootste `NodeInstanceId` waarde van de eigenschap op de service fabric-knooppunten. De codevoorbeelden hieronder sorteren op het knooppunt exemplaar en de details van het exemplaar met de grootste id-waarde retourneren. 

```powershell
Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1
```

```azurecli
`sfctl node list --query "sort_by(items[*], &instanceId)[-1]"`
```

De service fabric-cluster moet bekend dat dit knooppunt is vereist, worden verwijderd. Er zijn drie stappen die u moet uitvoeren:

1. Het knooppunt uitschakelen zodat deze niet langer een repliceren van gegevens.  
PowerShell:`Disable-ServiceFabricNode`  
sfcli:`sfctl node disable`

2. Stop het knooppunt zodat de service fabric-runtime foutloos wordt afgesloten en uw app een terminate-aanvraag ontvangt.  
PowerShell:`Start-ServiceFabricNodeTransition -Stop`  
sfcli:`sfctl node transition --node-transition-type Stop`

2. Het knooppunt uit het cluster verwijdert.  
PowerShell:`Remove-ServiceFabricNodeState`  
sfcli:`sfctl node remove-state`

Zodra deze drie stappen op het knooppunt zijn toegepast, kan deze worden verwijderd uit de schaalaanpassingsset. Als u elke categorie duurzaamheid naast [Brons][durability], deze stappen worden uitgevoerd voor u wanneer u de schaal ingesteld exemplaar wordt verwijderd.

Het volgende codeblok opgehaald van het laatste knooppunt gemaakt, wordt uitgeschakeld, stopt en verwijdert u het knooppunt uit het cluster.

```powershell
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1

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

In de **sfctl** code hieronder, de volgende opdracht wordt gebruikt om op te halen de **knooppuntnaam** en **knooppunt-exemplaar-id** waarden van het laatste knooppunt:`sfctl node list --query "sort_by(items[*], &instanceId)[-1].[instanceId,name]"`

```azurecli
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Gebruik de volgende **sfctl** query's naar de status van elke stap controleren
>
> **Deactivering status controleren**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].nodeDeactivationInfo"`
>
> **Controleer de status van stoppen**  
> `sfctl node list --query "sort_by(items[*], &instanceId)[-1].isStopped"`
>


### <a name="scale-in-the-scale-set"></a>Schalen in de schaalset

Nu dat de service fabric-knooppunt is verwijderd uit het cluster, kunt u de virtuele-machineschaalset geschaald in. In het volgende voorbeeld wordt de capaciteit van de set scale verminderd met 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Deze code wordt de capaciteit ingesteld op 5.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het aantal clusters knooppunt lezen
> * Toevoegen van de clusterknooppunten (scale-out)
> * Verwijder de clusterknooppunten (schaal in)


Ga vervolgens naar de volgende zelfstudie voor meer informatie over het implementeren van een toepassing en het gebruik van API management.
> [!div class="nextstepaction"]
> [Implementeren van API Management](service-fabric-tutorial-deploy-api-management.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
