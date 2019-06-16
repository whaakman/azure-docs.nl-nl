---
title: Een Azure Service Fabric-cluster implementeren in meerdere Beschikbaarheidszones | Microsoft Docs
description: Informatie over het maken van een Azure Service Fabric-cluster in meerdere Beschikbaarheidszones.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: b664c3d655ab45c89a65a0aea31622f57ddc8d9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65077453"
---
# <a name="deploy-an-azure-service-fabric-cluster-across-availability-zones"></a>Een Azure Service Fabric-cluster implementeren in meerdere Beschikbaarheidszones
Beschikbaarheidszones in Azure is een aanbieding die uw toepassingen en gegevens beveiligt tegen storingen in datacenters hoge beschikbaarheid. Een Beschikbaarheidszone is een unieke fysieke locatie zijn uitgerust met onafhankelijke voeding, koeling en netwerken binnen een Azure-regio.

Service Fabric biedt ondersteuning voor clusters die door te typen die zijn gekoppeld aan specifieke zones implementeren in meerdere Beschikbaarheidszones. Dit zorgt ervoor dat hoge beschikbaarheid van uw toepassingen. Azure-Beschikbaarheidszones zijn alleen beschikbaar in bepaalde regio's. Zie voor meer informatie, [overzicht van Beschikbaarheidszones Azure](https://docs.microsoft.com/azure/availability-zones/az-overview).

Voorbeeldsjablonen zijn beschikbaar: [Service Fabric cross-sjabloon van de zone beschikbaarheid](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="recommended-topology-for-primary-node-type-of-azure-service-fabric-clusters-spanning-across-availability-zones"></a>Aanbevolen topologie voor het primaire knooppunttype van Azure Service Fabric-clusters verdelen over Beschikbaarheidszones
Een Service Fabric-cluster verdeeld over Beschikbaarheidszones zorgt voor hoge beschikbaarheid van de clusterstatus. Als u wilt een Service Fabric-cluster in zones omvatten, moet u een primaire knooppunttype maken in elke Beschikbaarheidszone ondersteund door de regio. Dit wordt seed-knooppunten gelijkmatig verdelen over elk van de typen van het primaire knooppunt.

De aanbevolen topologie voor het primaire knooppunttype is vereist voor de resources worden hieronder beschreven:

* Het niveau van het cluster-betrouwbaarheid ingesteld op Platinum.
* Drie typen gemarkeerd als primair.
    * Elk knooppunttype moet worden toegewezen aan een eigen virtuele-machineschaalset die zich in verschillende zones.
    * Elke virtuele-machineschaalset moet ten minste vijf knooppunten (Silver duurzaamheid) hebben.
* Een enkel openbaar IP-Resource met behulp van standaard-SKU.
* Een eenmalige Load Balancer-Resource met behulp van standaard-SKU.
* Een NSG waarnaar wordt verwezen door het subnet waarin u uw virtuele machine-schaalsets implementeren.

>[!NOTE]
> De virtuele-machineschaalset één plaatsing ResourceGroup-eigenschap moet worden ingesteld op true, omdat de Service Fabric biedt geen ondersteuning voor een enkele virtuele machine van een schaalset die meerdere zones.

 ![Architectuur van Azure Service Fabric-Beschikbaarheidszone][sf-architecture]

## <a name="networking-requirements"></a>Netwerkvereisten
### <a name="public-ip-and-load-balancer-resource"></a>Openbare IP-Adressen en Load Balancer-Resource
Om in te schakelen van de zones die de eigenschap voor een virtuele-machineschaalset ingesteld resource, de load balancer en het IP-resource waarnaar wordt verwezen door deze virtuele-machineschaalset moeten beide gebruikmaken van een *Standard* SKU. Het maken van een load balancer of een IP-resource zonder de SKU-eigenschap maakt u een basis-SKU, die geen ondersteuning biedt voor Beschikbaarheidszones. Een standaard-SKU load balancer blokkeert al het verkeer van buiten standaard; om toe te staan buiten verkeer, moet een NSG aan het subnet worden geïmplementeerd.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
```

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
        {
            "name": "[parameters('subnet0Name')]",
            "properties": {
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> Het is niet mogelijk een wijziging in plaats van de SKU op het openbare IP-adres en de load balancer-resources. Als u uit bestaande resources waarvoor een basis-SKU migreert, ziet u de migratie-sectie van dit artikel.

### <a name="virtual-machine-scale-set-nat-rules"></a>Virtuele-machineschaalset NAT-regels
De load balancer inkomende NAT-regels moeten overeenkomen met de NAT-groepen van de virtuele-machineschaalset. Elke virtuele-machineschaalset moet een unieke binnenkomende NAT-pool hebben.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Standaard SKU Load Balancer regels voor uitgaand verkeer
Standard Load Balancer en Standard openbaar IP-adres leiden tot nieuwe mogelijkheden en ander gedrag voor uitgaande connectiviteit vergelijking met basis-SKU's. Als u uitgaande connectiviteit wilt bij het werken met standaard-SKU's, moet u deze met standaard openbare IP-adressen of standaard openbare Load Balancer expliciet definiëren. Zie voor meer informatie, [uitgaande verbindingen](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#snatexhaust) en [Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

>[!NOTE]
> De standaard sjabloon verwijst naar een NSG waarmee al het uitgaande verkeer standaard. Binnenkomend verkeer is beperkt tot de poorten die vereist voor Service Fabric-beheerbewerkingen zijn. De NSG-regels kunnen worden gewijzigd om te voldoen aan uw vereisten.

### <a name="enabling-zones-on-a-virtual-machine-scale-set"></a>Inschakelen van de zones op een virtuele-machineschaalset instellen
Inschakelen van een zone, ingesteld op een virtuele-machineschaalset die u moet de volgende drie waarden bevatten in de virtual machine scale set-resource.

* De eerste waarde is de **zones** eigenschap, die aangeeft welke de virtuele-machineschaalset wordt geïmplementeerd binnen een Beschikbaarheidszone.
* De tweede waarde is de eigenschap "singlePlacementGroup", die moet worden ingesteld op true.
* De derde waarde is de eigenschap 'faultDomainOverride' in de extensie van de Service Fabric virtuele-machineschaalset. De waarde voor deze eigenschap moet bevatten de regio en zone waarin deze virtuele-machineschaalset wordt geplaatst. Voorbeeld: "faultDomainOverride": "VS-Oost/az1' alle virtuele-resources machineschaalset moeten in dezelfde regio worden geplaatst, omdat Azure Service Fabric-clusters nog geen cross-ondersteuning voor regio.

```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "zones": ["1"],
    "properties": {
        "singlePlacementGroup": "true",
    },
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric.Test",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Silver",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
            "faultDomainOverride": "eastus/az1"
        },
        "typeHandlerVersion": "1.0"
    }
}
```

### <a name="enabling-multiple-primary-node-types-in-the-service-fabric-cluster-resource"></a>Inschakelen van meerdere primaire knooppunttypen in de Service Fabric-Cluster-bron
Als u wilt een of meer knooppunttypen instellen als primaire in een cluster-bron, stelt u de eigenschap 'isPrimary' op 'true'. Wanneer u een Service Fabric-cluster in meerdere Beschikbaarheidszones implementeert, moet u drie typen hebben in verschillende zones.

```json
{
    "reliabilityLevel": "Platinum",
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    },
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
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt2InstanceCount')]"
    }
    ],
}
```

## <a name="migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Migreren met Beschikbaarheidszones uit een cluster met behulp van een basis SKU Load Balancer en een Basic SKU-IP-adres
Als u wilt migreren van een cluster, wat is met een Load Balancer en het IP-met een basis-SKU, moet u eerst een geheel nieuwe Load Balancer en IP-resource met behulp van de standaard-SKU maken. Het is niet mogelijk om bij te werken van deze resources in-place.

De nieuwe LB en IP moet naar worden verwezen in de nieuwe cross binnen een Beschikbaarheidszone knooppunttypen dat u wilt gebruiken. In het voorbeeld hierboven drie nieuwe virtuele-machineschaalset zijn set resources toegevoegd in zones 1,2 en 3. Deze virtuele-machineschaalset verwijzing naar de zojuist gemaakte LB en IP-ingesteld en zijn gemarkeerd als primaire knooppunttypen in de Resource voor de Service Fabric-Cluster.

Als u wilt beginnen, moet u de nieuwe resources toevoegen aan uw bestaande Resource Manager-sjabloon. Deze resources omvatten:
* Een openbare IP-Resource met behulp van standaard-SKU.
* Een Load Balancer-Resource met behulp van standaard-SKU.
* Een NSG waarnaar wordt verwezen door het subnet waarin u uw virtuele machine-schaalsets implementeren.
* Drie typen gemarkeerd als primair.
    * Elk knooppunttype moet worden toegewezen aan een eigen virtuele-machineschaalset die zich in verschillende zones.
    * Elke virtuele-machineschaalset moet ten minste vijf knooppunten (Silver duurzaamheid) hebben.

Een voorbeeld van deze resources vindt u de [voorbeeldsjabloon](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/10-VM-Ubuntu-2-NodeType-Secure).

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $Template `
    -TemplateParameterFile $Parameters
```

Zodra de resources hebt geïmplementeerd, kunt u beginnen met het uitschakelen van de knooppunten in het primaire knooppunttype uit het oorspronkelijke cluster. Als de knooppunten zijn uitgeschakeld, wordt de systeemservices worden gemigreerd naar de nieuwe primaire knooppunttype die in de bovenstaande stap had is geïmplementeerd.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"

$nodeNames = @("_nt0_0", "_nt0_1", "_nt0_2", "_nt0_3", "_nt0_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames) {
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Nadat de knooppunten zijn alle uitgeschakeld, wordt het system-services worden uitgevoerd op het primaire knooppunttype, verdeeld over zones. U kunt vervolgens de uitgeschakelde knooppunten verwijderen uit het cluster. Wanneer de knooppunten zijn verwijderd, kunt u het oorspronkelijke IP-adres, Load Balancer, verwijderen en resources voor virtuele-machineschaalset.

```powershell
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}

$scaleSetName="nt0"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force

$lbname="LB-cluster-nt0"
$oldPublicIpName="LBIP-cluster-0"
$newPublicIpName="LBIP-cluster-1"

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force
```

Vervolgens moet u de verwijzingen naar deze resources van de Resource Manager-sjabloon die u had geïmplementeerd.

De laatste stap wordt hebben betrekking op de DNS-naam en een openbaar IP-adres bijwerken.

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

```

[sf-architecture]: ./media/service-fabric-cross-availability-zones/sf-cross-az-topology.png
