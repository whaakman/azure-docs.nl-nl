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
ms.openlocfilehash: e1d35bcd51349e6460d50acec0d9706fcd291e89
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="scale-a-service-fabric-cluster"></a>Een Service Fabric-cluster schalen

Deze zelfstudie maakt deel uit drie van een reeks en ziet u hoe u het bestaande cluster uit- en schalen. Wanneer u hebt voltooid, hebt u wellicht schalen van uw cluster en het opschonen van eventuele resterende resources.

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

Met de `Get-ServiceFabricClusterHealth` opdracht status aan u wordt geretourneerd met informatie over de status van elk knooppunt in het cluster.

## <a name="scale-out"></a>Uitschalen

Wanneer u uitschalen, kunt u meer exemplaren van de virtuele machine toevoegen aan de schaalaanpassingsset. Deze instanties, worden de knooppunten die gebruikmaakt van Service Fabric. Service Fabric kent Wanneer de schaalaanpassingsset heeft meer exemplaren (door het uitbreiden) toegevoegd en automatisch reageert. De volgende code haalt een schaal instellen op naam en verhoogt de **capaciteit** van de schaal van 1 instellen.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Nadat de update-bewerking is voltooid, voert de `Get-ServiceFabricClusterHealth` opdracht om te zien van de nieuwe knooppuntgegevens.

## <a name="scale-in"></a>Schalen in

Schalen hetzelfde is als uitbreiden, behalve het gebruik van een lagere **capaciteit** waarde. Wanneer u in de schaalset schaalt, kunt u exemplaren van de virtuele machine verwijderen uit de schaalaanpassingsset. Normaal gesproken Service Fabric is merkt niets van wat is er gebeurd en het geeft aan dat een knooppunt is kwijtgeraakt. Service Fabric rapporten klikt u vervolgens een onjuiste status voor het cluster. Om te voorkomen dat slechte staat, moet u het service fabric informeren dat u het knooppunt niet meer zichtbaar verwacht.

### <a name="remove-the-service-fabric-node"></a>Verwijder de service fabric-knooppunt

> [!NOTE]
> Dit onderdeel is alleen van toepassing op de *Brons* duurzaamheid laag. Zie voor meer informatie over duurzaamheid [capaciteitsplanning voor Service Fabric-cluster][durability].

Wanneer u in een virtuele-machineschaalset schaalt, verwijdert de schaal instellen (in de meeste gevallen) de virtuele machine-instantie die het laatst is gemaakt. Daarom moet u de overeenkomende, het laatst is gemaakt, service fabric-knooppunt gevonden. U vindt dit laatste knooppunt door het controleren van de grootste `NodeInstanceId` waarde van de eigenschap op de service fabric-knooppunten. 

```powershell
Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1
```

De service fabric-cluster moet bekend dat dit knooppunt is vereist, worden verwijderd. Er zijn drie stappen die u moet uitvoeren:

1. Het knooppunt uitschakelen zodat deze niet langer een repliceren van gegevens.  
`Disable-ServiceFabricNode`

2. Stop het knooppunt zodat de service fabric-runtime foutloos wordt afgesloten en uw app een terminate-aanvraag ontvangt.  
`Start-ServiceFabricNodeTransition -Stop`

2. Het knooppunt uit het cluster verwijdert.  
`Remove-ServiceFabricNodeState`

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

### <a name="scale-in-the-scale-set"></a>Schalen in de schaalset

Nu dat de service fabric-knooppunt is verwijderd uit het cluster, kunt u de virtuele-machineschaalset geschaald in. In het volgende voorbeeld wordt de capaciteit van de set scale verminderd met 1.

```powershell
$scaleset = Get-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzureRmVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het aantal clusters knooppunt lezen
> * Toevoegen van de clusterknooppunten (scale-out)
> * Verwijder de clusterknooppunten (schaal in)


Ga vervolgens naar de volgende zelfstudie voor meer informatie over het implementeren van een toepassing en het gebruik van API management.
> [!div class="nextstepaction"]
> [Een toepassing implementeren](service-fabric-tutorial-deploy-api-management.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster