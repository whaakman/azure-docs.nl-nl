---
title: Scale-out van een Azure Service Fabric-cluster met virtuele-Machineschaalset toevoegen | Microsoft Docs
description: Leer hoe u een Service Fabric-cluster schalen door het toevoegen van een virtuele-Machineschaalset opgehaald.
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
ms.date: 08/21/2018
ms.author: ryanwi
ms.openlocfilehash: 57eac1dcc170e2ac7e35cab64b6980bbe053db39
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55092734"
---
# <a name="scale-a-service-fabric-cluster-out-by-adding-a-virtual-machine-scale-set"></a>Schalen van een Service Fabric-cluster af door het toevoegen van een virtuele-Machineschaalset opgehaald
In dit artikel wordt beschreven hoe u een Azure Service Fabric-cluster schalen door een nieuwe virtuele-machineschaalset aan een bestaand cluster toe te voegen. Een Service Fabric-cluster is een netwerk verbonden reeks virtuele of fysieke machines waarop uw microservices worden geïmplementeerd en beheerd. Een machine of virtuele machine die deel uitmaakt van een cluster, heet een knooppunt. Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruikt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunttype die is gedefinieerd in een Azure-cluster is [instellen als een afzonderlijke schaalset](service-fabric-cluster-nodetypes.md). Vervolgens kan elk knooppunttype afzonderlijk worden beheerd. Na het maken van een Service Fabric-cluster, kunt u een cluster-knooppunttype verticaal schalen (de bronnen van de knooppunten wijzigen), werk het besturingssysteem van het knooppunttype VM's of een nieuwe virtuele-machineschaalset aan een bestaand cluster toe te voegen.  U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster.  Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

> [!WARNING]
> Start niet het primaire nodetype VM-SKU wijzigen als de clusterstatus van het niet in orde is. Als de clusterstatus niet in orde is, wordt u alleen het cluster bovendien destabiliseren, als u probeert te wijzigen van de VM-SKU.
>
> We raden u aan de VM-SKU van een scale set/knooppunttype niet te wijzigen, tenzij deze wordt uitgevoerd op [duurzaamheid van Silver of hoger](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). VM-SKU-grootte te wijzigen, is een gegevens-destructieve ter plekke infrastructuur-bewerking. Zonder enige mogelijkheid vertraging of bewaken van deze wijziging, is het mogelijk dat de bewerking kan leiden gegevensverlies voor stateful services tot of andere onvoorziene operationele problemen, zelfs voor staatloze werkbelastingen veroorzaken. Dit betekent dat het primaire knooppunttype, die wordt uitgevoerd de stateful service fabric-systeemservices, of elk knooppunttype waarop uw werk stateful toepassingen wordt geladen.
>

## <a name="upgrade-the-size-and-operating-system-of-the-primary-node-type-vms"></a>De grootte en het besturingssysteem van het primaire knooppunttype virtuele machines bijwerken
Dit is het proces voor het bijwerken van de VM-grootte en het besturingssysteem van het primaire knooppunttype VM's.  Na de upgrade, het primaire knooppunttype VM's zijn standaard D4_V2 grootte en de actieve Windows Server 2016 Datacenter met Containers.

> [!WARNING]
> Voordat u deze procedure op een productiecluster, wordt aangeraden dat u de voorbeeldsjablonen bestuderen en controleer of het proces voor een testcluster. Het cluster is ook niet beschikbaar is gedurende een periode. U kunt geen wijzigingen aan meerdere VMSS gedeclareerd als het dezelfde NodeType parallel; maken u moet de implementatiebewerkingen gescheiden om wijzigingen te laten aan elke VMSS NodeType afzonderlijk uit te voeren.

1. Het eerste cluster met twee typen en twee scale sets (één schaalset per knooppunttype) implementeren met behulp van deze voorbeeld [sjabloon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.json) en [parameters](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-2ScaleSets.parameters.json) bestanden.  Beide schaalsets zijn grootte Standard D2_V2 en actieve Windows Server 2012 R2 Datacenter.  Wachten op het cluster om de basislijnupgrade te voltooien.   
2. Optioneel: een voorbeeld van een stateful aan het cluster implementeren.
3. Nadat u hebt gekozen om te upgraden van het primaire knooppunttype virtuele machines, Voeg een nieuwe schaalset ingesteld op het primaire knooppunttype met behulp van deze voorbeeld [sjabloon](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.json) en [parameters](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/templates/nodetype-upgrade/Deploy-2NodeTypes-3ScaleSets.parameters.json) bestanden, zodat het primaire knooppunttype nu twee-schaalsets heeft.  Systeemservices en toepassingen kunnen migreren tussen VM's in de twee verschillende schaalsets.  De nieuwe scale set VM's zijn standaard D4_V2 grootte en het uitvoeren van Windows Server 2016 Datacenter met Containers.  Een nieuwe load balancer en een openbaar IP-adres worden ook toegevoegd aan de nieuwe schaalset.  
    Als u wilt zoeken in de schaalset in de sjabloon, zoek de resource 'Microsoft.Compute/virtualMachineScaleSets' met de naam van de *vmNodeType2Name* parameter.  De nieuwe schaalset wordt toegevoegd aan het primaire knooppunt met behulp van de eigenschappen > virtualMachineProfile - > extensionProfile -> extensies -> Eigenschappen >-Instellingen -> nodeTypeRef instelling.
4. Controleer de clusterstatus van het en controleer of dat alle knooppunten in orde zijn.
5. Schakel de knooppunten in de oude schaalset van het primaire knooppunttype met de bedoeling knooppunt verwijderen uit. U kunt in één keer uitschakelen en de bewerkingen in de wachtrij zijn geplaatst. Wacht totdat alle knooppunten zijn uitgeschakeld, dit kan enige tijd duren.  Als de oudere knooppunten in het knooppunttype zijn uitgeschakeld, wordt de systeem-services en seed-knooppunten migreren naar de virtuele machines van de nieuwe schaalset in het primaire knooppunttype.
6. Verwijder de oudere schaalset op basis van het primaire knooppunttype.
7. De load balancer die zijn gekoppeld aan de oude schaalset verwijderen. Het cluster is niet beschikbaar terwijl de nieuwe openbare IP-adres en de load balancer zijn geconfigureerd voor de nieuwe schaalset.  
8. Store-DNS-instellingen van het openbare IP-adres dat is gekoppeld aan het oude primaire knooppunt Typ schaalset in een variabele en openbare IP-adres verwijderen.
9. Vervang de DNS-instellingen van het openbare IP-adres dat is gekoppeld aan de nieuwe primaire knooppunt type schaalset met de DNS-instellingen van het openbare IP-adres verwijderd.  Het cluster is nu weer bereikbaar.
10. De status van de knooppunten uit het cluster verwijdert.  Als het duurzaamheidsniveau van de van de oude schaalset silver- of gold is, wordt deze stap automatisch uitgevoerd door het systeem.
11. Als u de stateful-toepassing in de vorige stap hebt geïmplementeerd, moet u bevestigen dat de toepassing functioneel is.

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

## <a name="adding-an-additional-scale-set-to-an-existing-cluster"></a>Een aanvullende schaal toe te voegen instellen aan een bestaand cluster
Een nieuwe NodeType virtuele-Machineschaalset aan een bestaand cluster toevoegt, is vergelijkbaar met de bovengenoemde upgraden van het primaire knooppunttype, tenzij u de dezelfde NodeTypeRef; niet gebruikt natuurlijk niet uitschakelen van een actief gebruikte virtuele Machine-schaalsets, en u niet beschikbaarheid van het cluster verliezen als het primaire knooppunttype, niet bijwerken. 

NodeTypeRef-eigenschap is gedeclareerd in de Virtual Machine Scale Service Fabric extensie-eigenschappen:
```json
<snip>
"publisher": "Microsoft.Azure.ServiceFabric",
     "settings": {
     "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
     "nodeTypeRef": "[parameters('vmNodeType2Name')]",
     "dataPath": "D:\\\\SvcFab",
     "durabilityLevel": "Silver",
<snip>
```

Daarnaast moet u dit nieuwe knooppunttype toevoegen aan uw Service Fabric-cluster-bron:

```json
<snip>
"nodeTypes": [
      {
      "name": "[parameters('vmNodeType2Name')]",
      "applicationPorts": {
                "endPort": "[parameters('nt2applicationEndPort')]",
                "startPort": "[parameters('nt2applicationStartPort')]"
      },
      "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
      "durabilityLevel": "Silver",
       "ephemeralPorts": {
                "endPort": "[parameters('nt2ephemeralEndPort')]",
                "startPort": "[parameters('nt2ephemeralStartPort')]"
      },
      "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
      "isPrimary": false,
      "vmInstanceCount": "[parameters('nt2InstanceCount')]"
},
<snip>
```

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [toepassing schaalbaarheid](service-fabric-concepts-scalability.md).
* [Schalen van een Azure-cluster in- of uitgeschaald](service-fabric-tutorial-scale-cluster.md).
* [Een Azure-cluster programmatisch schalen](service-fabric-cluster-programmatic-scaling.md) SDK met de fluent Azure compute.
* [Een zelfstandige-cluster in- of uitschalen](service-fabric-cluster-windows-server-add-remove-nodes.md).

