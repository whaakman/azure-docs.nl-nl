---
title: Een Azure Service Fabric-cluster SKU voor virtuele machines of OS upgrade | Microsoft Docs
description: Informatie over het upgraden van de virtuele machines in een Service Fabric-cluster primaire nodetype.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: ''
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/21/2018
ms.author: ryanwi
ms.openlocfilehash: bf707bf4b1c001b5467dd9954e9c6feb55e65654
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655243"
---
# <a name="upgrade-the-primary-node-type-vms-of-a-service-fabric-cluster"></a>Upgrade van het primaire knooppunttype VM's van een Service Fabric-cluster
Dit artikel wordt beschreven voor het bijwerken van het primaire knooppunt type virtuele machines van een Service Fabric-cluster in Azure wordt uitgevoerd.  Een Service Fabric-cluster is een set netwerk verbonden virtuele of fysieke machines waarin uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster wordt een knooppunt genoemd. Virtuele-machineschaalsets zijn een Azure compute resource die u gebruikt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype dat is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke scale set](service-fabric-cluster-nodetypes.md). Elk knooppunttype kan vervolgens afzonderlijk worden beheerd. Na het maken van een Service Fabric-cluster, kunt u een cluster knooppunttype verticaal schalen (de bronnen van de knooppunten wijzigen) of werk het besturingssysteem van het type node virtuele machines.  U kunt het cluster schalen op elk gewenst moment, zelfs wanneer werkbelastingen worden uitgevoerd op het cluster.  Tijdens het cluster schalen, wordt uw toepassingen automatisch ook schalen.

> [!WARNING]
> We raden aan de VM-SKU van een scale set/knooppunttype niet te wijzigen wanneer deze wordt uitgevoerd op [Silver duurzaamheid of groter](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM-SKU-grootte wijzigen is een gegevens-destructieve in-place infrastructuur-bewerking. Zonder enige mogelijkheid om te stellen of het bewaken van deze wijziging is het mogelijk dat de bewerking kan leiden gegevensverlies van voor stateful services tot of andere onvoorziene operationele problemen kan, zelfs voor staatloze werkbelastingen veroorzaken. 
>

## <a name="upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>De grootte en het besturingssysteem van het primaire knooppunttype virtuele machines bijwerken
Dit is het proces voor het bijwerken van de VM-grootte en het besturingssysteem van het primaire knooppunttype virtuele machines.  Na de upgrade zijn het primaire knooppunttype VMs grootte standaard D4_V2 en actieve Windows Server 2016 Datacenter met Containers.

> [!WARNING]
> Voordat u deze procedure op een productiecluster, wordt aangeraden dat u de voorbeeldsjablonen bestuderen en controleer of het proces op een testcluster. Het cluster is ook niet beschikbaar voor een periode.

1. De eerste cluster met twee knooppunttypen en twee schaalsets (één per knooppunttype instellen schaal) implementeren met behulp van deze voorbeeld [sjabloon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) en [parameters](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) bestanden.  Beide schaalsets zijn standaard D2_V2 grootte en actieve Windows Server 2012 R2 Datacenter.  Wacht tot het cluster om de basislijn-upgrade te voltooien.   
2. Optioneel - implementeert u een stateful aan het cluster.
3. Na het bijwerken van het primaire knooppunttype VMs bepaalt, Voeg een nieuwe schaal ingesteld op het primaire knooppunttype met behulp van deze voorbeeld [sjabloon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) en [parameters](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) bestanden, zodat het primaire knooppunttype nu twee-schaalsets heeft.  Systeemservices en gebruikerstoepassingen zich op migreren tussen VM's in twee verschillende-schaalsets.  De nieuwe scale set VM's zijn standaard D4_V2 grootte en Windows Server 2016 Datacenter worden uitgevoerd met Containers.  Een nieuwe load balancer en het openbare IP-adres ook toegevoegd aan de nieuwe scale set.  
    Als u wilt zoeken naar de nieuwe schaal in de sjabloon is ingesteld, zoekt u de 'Microsoft.Compute/virtualMachineScaleSets'-resource met de naam door de *vmNodeType2Name* parameter.  De nieuwe scale set wordt toegevoegd aan het primaire knooppunt type met behulp van de eigenschappen -> virtualMachineProfile - > extensionProfile -> extensies -> Eigenschappen -> Instellingen -> nodeTypeRef-instelling.
4. De status van het cluster en controleer of dat alle knooppunten in orde zijn.
5. Uitschakelen van de knooppunten in de oude scale set van het primaire knooppunttype met de bedoeling knooppunt verwijderen. U kunt in één keer uitschakelen en de bewerkingen in de wachtrij. Wacht totdat alle knooppunten zijn uitgeschakeld, dit kan enige tijd duren.  Als de oudere knooppunten in het knooppunttype zijn uitgeschakeld, wordt de systeem-services en seed-knooppunten migreren naar de virtuele machines van de nieuwe schaal ingesteld in het primaire knooppunttype.
6. Verwijder de oudere schaal ingesteld op basis van het primaire knooppunttype.
7. De load balancer die zijn gekoppeld aan de oude schaalset verwijderen. Het cluster is niet beschikbaar terwijl de nieuwe openbare IP-adres en de load balancer zijn geconfigureerd voor de nieuwe scale set.  
8. Store DNS-instellingen van het openbare IP-adres die zijn gekoppeld aan het oude primaire knooppunt schaalset typt in een variabele en verwijder dit openbare IP-adres.
9. Vervang de DNS-instellingen van het openbare IP-adres die zijn gekoppeld aan de nieuwe primaire knooppunt type schaal ingesteld met de DNS-instellingen van het verwijderde openbare IP-adres.  Het cluster is nu weer bereikbaar.
10. De status van de knooppunten uit het cluster verwijdert.  Als het niveau van de duurzaamheid van de oude schaalset zilver of goud, wordt deze stap automatisch uitgevoerd door het systeem.
11. Als u de stateful-toepassing in de vorige stap hebt geïmplementeerd, controleert u of de toepassing functioneert.

```powershell
# Variables.
$groupname = "sfupgradetestgroup"
$clusterloc="southcentralus"  
$subscriptionID="<your subscription ID>"

# sign in to your Azure account and select your subscription
Login-AzureRmAccount -SubscriptionId $subscriptionID 

# Create a new resource group for your deployment and give it a name and a location.
New-AzureRmResourceGroup -Name $groupname -Location $clusterloc

# Deploy the two node type cluster.
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-2ScaleSets.json" -Verbose

# Connect to the cluster and check the cluster health.
$ClusterName= "sfupgradetest.southcentralus.cloudapp.azure.com:19000"
$thumb="F361720F4BD5449F6F083DDE99DC51A86985B25B"

Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Get-ServiceFabricClusterHealth

# Deploy a new scale set into the primary node type.  Create a new load balancer and public IP address for the new scale set.
New-AzureRmResourceGroupDeployment -ResourceGroupName $groupname -TemplateParameterFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.parameters.json" `
    -TemplateFile "C:\temp\cluster\Deploy-2NodeTypes-3ScaleSets.json" -Verbose

# Check the cluster health again. All 15 nodes should be healthy.
Get-ServiceFabricClusterHealth

# Disable the nodes in the original scale set.
$nodeNames = @("_NTvm1_0","_NTvm1_1","_NTvm1_2","_NTvm1_3","_NTvm1_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames){
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}

Write-Host "Checking node status..."
foreach($name in $nodeNames){
 
    $state = Get-ServiceFabricNode -NodeName $name 

    $loopTimeout = 50

    do{
        Start-Sleep 5
        $loopTimeout -= 1
        $state = Get-ServiceFabricNode -NodeName $name
        Write-Host "$name state: " $state.NodeDeactivationInfo.Status
    }

    while (($state.NodeDeactivationInfo.Status -ne "Completed") -and ($loopTimeout -ne 0))
    

    if ($state.NodeStatus -ne [System.Fabric.Query.NodeStatus]::Disabled)
    {
        Write-Error "$name node deactivation failed with state" $state.NodeStatus
        exit
    }
}

# Remove the scale set
$scaleSetName="NTvm1"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force
Write-Host "Removed scale set $scaleSetName"

$lbname="LB-sfupgradetest-NTvm1"
$oldPublicIpName="PublicIP-LB-FE-0"
$newPublicIpName="PublicIP-LB-FE-2"

# Store DNS settings of public IP address related to old Primary NodeType into variable 
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname

$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel

$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

# Remove Load Balancer related to old Primary NodeType. This will cause a brief period of downtime for the cluster
Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force

# Remove the old public IP
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

# Replace DNS settings of Public IP address related to new Primary Node Type with DNS settings of Public IP address related to old Primary Node Type
$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

# Check the cluster health
Get-ServiceFabricClusterHealth

# Remove node state for the deleted nodes.
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
* [Een Azure-cluster in- of schalen](service-fabric-tutorial-scale-cluster.md).
* [Een Azure-cluster via een programma schalen](service-fabric-cluster-programmatic-scaling.md) SDK met de beheersen Azure berekenen.
* [Schalen van een cluster standaone in- of](service-fabric-cluster-windows-server-add-remove-nodes.md).

