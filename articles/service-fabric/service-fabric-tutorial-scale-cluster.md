---
title: Een Service Fabric-cluster schalen in Azure | Microsoft Docs
description: In deze zelfstudie leert u hoe u kunt een Service Fabric-cluster schalen in Azure.
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
ms.date: 03/19/2019
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 315e3c05e23d68edb701e705fd1ed02a840a8936
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226807"
---
# <a name="tutorial-scale-a-service-fabric-cluster-in-azure"></a>Zelfstudie: Een Service Fabric-cluster schalen in Azure

Deze zelfstudie is deel drie van een reeks en laat zien hoe u een bestaand cluster in- en uitschalen u. Aan het einde van deze zelfstudie weet u hoe u een cluster kunt schalen en eventuele resterende resources kunt opschonen.  Lees voor meer informatie over het schalen van een cluster in Azure uitvoert, [schalen van Service Fabric-clusters](service-fabric-cluster-scaling.md).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Toevoegen en verwijderen van knooppunten (scale-out en inschalen)
> * Toevoegen en verwijderen van knooppunttypen (scale-out en inschalen)
> * Knooppunt-resources (Omhoog schalen) verhogen

In deze zelfstudiereeks leert u het volgende:
> [!div class="checklist"]
> * Een beveiligd [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) maken in Azure met behulp van een sjabloon
> * [Een cluster bewaken](service-fabric-tutorial-monitor-cluster.md)
> * Een cluster in- of uitschalen
> * [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)
> * [Een cluster verwijderen](service-fabric-tutorial-delete-cluster.md)

## <a name="prerequisites"></a>Vereisten

Voor u met deze zelfstudie begint:

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Installeer de [Azure Powershell-module, versie 4.1 of hoger](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps) of de [Azure CLI](/cli/azure/install-azure-cli).
* Een beveiligd [Windows-cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) maken in Azure

## <a name="important-considerations-and-guidelines"></a>Belangrijke overwegingen en richtlijnen

Werkbelastingen van toepassingen na verloop van tijd veranderen, hoeft uw bestaande services meer (of minder) bronnen?  [Toevoegen of verwijderen van knooppunten](#add-nodes-to-or-remove-nodes-from-a-node-type) van een knooppunt dat u wilt vergroten of verkleinen van clusterresources.

Wilt u meer dan 100 knooppunten toevoegen aan uw cluster?  Een enkele Service Fabric-knooppunt type/scale set kan niet meer dan 100 knooppunten/virtuele machines bevatten.  Voor het schalen van een cluster met meer dan 100 knooppunten [toevoegen van extra knooppunttypen](#add-nodes-to-or-remove-nodes-from-a-node-type).

Uw toepassing beschikt over meerdere services en moet een van deze openbare of internetservices zijn?  Typische toepassingen bevatten een gateway in de front-end-service die de invoer van een client ontvangt en een of meer back-end-services die communiceren met de front-end-services. In dit geval raden we u [toevoegen ten minste twee typen](#add-nodes-to-or-remove-nodes-from-a-node-type) aan het cluster.  

Uw services beschikt over de infrastructuur voor verschillende behoeften, zoals meer RAM-geheugen of hogere CPU-cycli? Uw toepassing bevat bijvoorbeeld een front-end-service en een back-endservice. De front-end-service kunt uitvoeren op kleinere virtuele machines (VM-grootten, zoals D2) die poorten zijn geopend met het internet. De back-endservice echter is berekening intensieve en moet worden uitgevoerd op grotere virtuele machines (met VM-grootten, zoals D4, D6, D15) die niet internet gericht. In dit geval het wordt aangeraden dat u [toevoegen van twee of meer knooppunttypen](#add-nodes-to-or-remove-nodes-from-a-node-type) met uw cluster. Hierdoor kan elk knooppunttype om afzonderlijke eigenschappen zoals verbinding met internet of VM-grootte. Het aantal virtuele machines kan worden geschaald, onafhankelijk van elkaar, maar ook.

Wanneer u een Azure-cluster, houd rekening met de volgende richtlijnen:

* Een enkele Service Fabric-knooppunt type/scale set kan niet meer dan 100 knooppunten/virtuele machines bevatten.  Om te schalen in een cluster met meer dan 100 knooppunten, moet u extra knooppunttypen toevoegen.
* Primaire knooppunttypen uitvoeren van productieworkloads moet een [duurzaamheidsniveau] [ durability] Gold of zilver en hebben altijd het vijf of meer knooppunten.
* niet-primaire knooppunttypen uitvoeren van stateful productieworkloads moeten altijd vijf of meer knooppunten hebben.
* niet-primaire knooppunttypen uitvoeren van productieworkloads stateless moeten altijd twee of meer knooppunten hebben.
* Elk knooppunttype [duurzaamheidsniveau] [ durability] Gold of Silver moet altijd zijn vijf of meer knooppunten.
* Als inschalen (verwijderen van knooppunten van) een primaire knooppunttype, moet u nooit het aantal exemplaren die lager is dan wat een lager de [betrouwbaarheidsniveau] [ reliability] is vereist.

Lees voor meer informatie, [richtlijnen van de capaciteit van cluster](service-fabric-cluster-capacity.md).

## <a name="export-the-template-for-the-resource-group"></a>De sjabloon voor de resourcegroep exporteren

Na het maken van een veilige [Windows cluster](service-fabric-tutorial-create-vnet-and-windows-cluster.md) en instellen van de resourcegroep, de Resource Manager-sjabloon voor de resourcegroep exporteren. De sjabloon exporteren, kunt u voor het automatiseren van toekomstige implementaties van het cluster en de resources, omdat de sjabloon de volledige infrastructuur bevat.  Lees voor meer informatie over het exporteren van sjablonen [resourcegroepen beheren van Azure Resource Manager met behulp van de Azure-portal](/azure/azure-resource-manager/manage-resource-groups-portal).

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de resourcegroep met het cluster (**sfclustertutorialgroup**, als u deze zelfstudie volgt). 

2. Selecteer in het linkerdeelvenster **implementaties**, of Selecteer de koppeling onder **implementaties**. 

3. Selecteer de meest recente geslaagde implementatie in de lijst.

4. Selecteer in het linkerdeelvenster **sjabloon** en selecteer vervolgens **downloaden** de sjabloon exporteren als een ZIP-bestand.  Sla de sjabloon en parameters op uw lokale computer.

## <a name="add-nodes-to-or-remove-nodes-from-a-node-type"></a>Knooppunten toevoegen of verwijderen uit een knooppunttype

In en uit te schalen of horizontaal schalen, wijzigt het aantal knooppunten in het cluster. Wanneer u in of uit te schalen, kunt u meer exemplaren van de virtuele machine toevoegen aan de schaalset. Deze instanties worden de knooppunten die door Service Fabric worden gebruikt. Service Fabric weet wanneer er meer exemplaren aan de schaalset zijn toegevoegd (door uitschaling) en reageert daar automatisch op. U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster.

### <a name="update-the-template"></a>De sjabloon bijwerken

[Een sjabloon en parameters-bestand exporteren](#export-the-template-for-the-resource-group) uit de resourcegroep voor de meest recente implementatie.  Open de *parameters.json* bestand.  Als u het cluster met behulp geïmplementeerd de [voorbeeldsjabloon] [ template] in deze zelfstudie, er zijn drie typen in het cluster en drie parameters die het aantal knooppunten ingesteld voor elk knooppunttype:  *nt0InstanceCount*, *nt1InstanceCount*, en *nt2InstanceCount*.  De *nt1InstanceCount* parameter, bijvoorbeeld: Hiermee stelt u het aantal exemplaren voor het tweede knooppunttype en wordt het aantal virtuele machines in de bijbehorende virtuele-machineschaalset ingesteld.

Dit het geval is, door het bijwerken van de waarde van de *nt1InstanceCount* wijzigen van het aantal knooppunten in het tweede knooppunttype.  Denk eraan dat u een knooppunttype uit met meer dan 100 knooppunten kan niet schalen.  niet-primaire knooppunttypen uitvoeren van stateful productieworkloads moeten altijd vijf of meer knooppunten hebben. niet-primaire knooppunttypen uitvoeren van productieworkloads stateless moeten altijd twee of meer knooppunten hebben.

Als u inschalen bent, verwijderen van knooppunten van een knooppunttype Brons [duurzaamheidsniveau] [ durability] moet u [handmatig verwijderen van de status van deze knooppunten](service-fabric-cluster-scale-up-down.md#manually-remove-vms-from-a-node-typevirtual-machine-scale-set).  Voor informatie over de duurzaamheidslaag van Silver en Gold, worden deze stappen automatisch uitgevoerd door het platform.

### <a name="deploy-the-updated-template"></a>De bijgewerkte sjabloon implementeren
Opslaan van wijzigingen in de *template.json* en *parameters.json* bestanden.  Voer de volgende opdracht voor het implementeren van de bijgewerkte sjabloon:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "ChangingInstanceCount"
```
Of de volgende Azure CLI-opdracht uit:
```azure-cli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="add-a-node-type-to-the-cluster"></a>Een knooppunttype toevoegen aan het cluster

Elk knooppunttype die is gedefinieerd in een Service Fabric-cluster worden uitgevoerd in Azure is ingesteld als een [afzonderlijke virtuele-machineschaalset](service-fabric-cluster-nodetypes.md). Vervolgens kan elk knooppunttype afzonderlijk worden beheerd. U kunt onafhankelijk elk knooppunttype omhoog of omlaag schalen, verschillende open poorten bevatten en verschillende capaciteitsstatistieken gebruiken. U kunt ook afzonderlijk de SKU van het besturingssysteem die worden uitgevoerd op elk clusterknooppunt wijzigen, maar houd er rekening mee dat er geen een combinatie van Windows en Linux die worden uitgevoerd in het voorbeeld-cluster. Een enkel knooppunt/schaalset mag niet meer dan 100 knooppunten bevatten.  U kunt een cluster horizontaal op meer dan 100 knooppunten schalen door extra knooppunt typen/schaalsets toe te voegen. U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster.

### <a name="update-the-template"></a>De sjabloon bijwerken

[Een sjabloon en parameters-bestand exporteren](#export-the-template-for-the-resource-group) uit de resourcegroep voor de meest recente implementatie.  Open de *parameters.json* bestand.  Als u het cluster met behulp geïmplementeerd de [voorbeeldsjabloon] [ template] in deze zelfstudie, er zijn drie typen van de knooppunten in het cluster.  In deze sectie kunt u een vierde knooppunttype toevoegen door te werken en implementeren van een Resource Manager-sjabloon. 

Naast de nieuwe knooppunttype u ook de bijbehorende VM-schaalset (die wordt uitgevoerd in een apart subnet van het virtuele netwerk) toevoegen en beveiligingsgroep.  U kunt nieuwe of bestaande openbare IP-adres en resources voor Azure load balancer voor de nieuwe schaalset toe te voegen.  Het nieuwe knooppunttype heeft een [duurzaamheidsniveau] [ durability] van Silver en de grootte van 'Standard_D2_V2'.

In de *template.json* bestand, zijn de volgende nieuwe parameters toegevoegd:
```json
"nt3InstanceCount": {
    "defaultValue": 5,
    "type": "Int",
    "metadata": {
        "description": "Instance count for node type"
    }
},
"vmNodeType3Size": {
    "defaultValue": "Standard_D2_V2",
    "type": "String"
},
```

In de *template.json* bestand, voeg de volgende nieuwe variabelen:
```json
"lbID3": "[resourceId('Microsoft.Network/loadBalancers',concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name')))]",
"lbIPConfig3": "[concat(variables('lbID3'),'/frontendIPConfigurations/LoadBalancerIPConfig')]",
"lbPoolID3": "[concat(variables('lbID3'),'/backendAddressPools/LoadBalancerBEAddressPool')]",
"lbProbeID3": "[concat(variables('lbID3'),'/probes/FabricGatewayProbe')]",
"lbHttpProbeID3": "[concat(variables('lbID3'),'/probes/FabricHttpGatewayProbe')]",
"lbNatPoolID3": "[concat(variables('lbID3'),'/inboundNatPools/LoadBalancerBEAddressNatPool')]",
"vmNodeType3Name": "[toLower(concat('NT4', variables('vmName')))]",
"vmStorageAccountName3": "[toLower(concat(uniqueString(resourceGroup().id), '1', '3' ))]",
"nt3applicationStartPort": "20000",
"nt3applicationEndPort": "30000",
"nt3ephemeralStartPort": "49152",
"nt3ephemeralEndPort": "65534",
"nt3fabricTcpGatewayPort": "19000",
"nt3fabricHttpGatewayPort": "19080",
"nt3reverseProxyEndpointPort": "19081",
"subnet3Name": "Subnet-3",
"subnet3Prefix": "10.0.3.0/24",
"subnet3Ref": "[concat(variables('vnetID'),'/subnets/',variables('subnet3Name'))]",
```

In de *template.json* bestand, een nieuw subnet toevoegen aan de VM-resource:
```json
{
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            ...
            {
                "name": "[variables('subnet3Name')]",
                "properties": {
                    "addressPrefix": "[variables('subnet3Prefix')]",
                    "networkSecurityGroup": {
                        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', variables('subnet3Name')))]"
                    }
                }
            }
        ]
    },
    "dependsOn": [
        ...
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', variables('subnet3Name')))]"
    ]
},
```

In de *template.json* bestand, het toevoegen van nieuwe openbare IP-adres en de load balancer-resources:
```json
{
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat(variables('lbIPName'),'-',variables('vmNodeType3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "dnsSettings": {
            "domainNameLabel": "[concat(variables('dnsName'),'-','nt4')]"
        },
        "publicIPAllocationMethod": "Dynamic"
    }
},
        {
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "frontendIPConfigurations": [
            {
                "name": "LoadBalancerIPConfig",
                "properties": {
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',concat(variables('lbIPName'),'-',variables('vmNodeType3Name')))]"
                    }
                }
            }
        ],
        "backendAddressPools": [
            {
                "name": "LoadBalancerBEAddressPool",
                "properties": {}
            }
        ],
        "loadBalancingRules": [
            {
                "name": "LBRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[variables('nt3fabricTcpGatewayPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[variables('nt3fabricTcpGatewayPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[variables('lbProbeID3')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "LBHttpRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[variables('nt3fabricHttpGatewayPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[variables('nt3fabricHttpGatewayPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[variables('lbHttpProbeID3')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortLBRule1",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[parameters('loadBalancedAppPort1')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[parameters('loadBalancedAppPort1')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID3'),'/probes/AppPortProbe1')]"
                    },
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortLBRule2",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID3')]"
                    },
                    "backendPort": "[parameters('loadBalancedAppPort2')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPort": "[parameters('loadBalancedAppPort2')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID3'),'/probes/AppPortProbe2')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            {
                "name": "FabricGatewayProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[variables('nt3fabricTcpGatewayPort')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "FabricHttpGatewayProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[variables('nt3fabricHttpGatewayPort')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortProbe1",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[parameters('loadBalancedAppPort1')]",
                    "protocol": "tcp"
                }
            },
            {
                "name": "AppPortProbe2",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port": "[parameters('loadBalancedAppPort2')]",
                    "protocol": "tcp"
                }
            }
        ],
        "inboundNatPools": [
            {
                "name": "LoadBalancerBEAddressNatPool",
                "properties": {
                    "backendPort": "3389",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig3')]"
                    },
                    "frontendPortRangeEnd": "4500",
                    "frontendPortRangeStart": "3389",
                    "protocol": "tcp"
                }
            }
        ]
    },
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/',concat(variables('lbIPName'),'-',variables('vmNodeType3Name')))]"
    ]
},
```

In de *template.json* bestand, nieuwe netwerk beveiliging groep en virtual machine scale set resources toevoegen.  De eigenschap NodeTypeRef binnen de Service Fabric-extensie-eigenschappen van de virtuele-machineschaalset wordt het type opgegeven knooppunt toegewezen aan de schaalset.

```json
{
    "type": "Microsoft.Network/networkSecurityGroups",
    "name": "[concat('nsg', variables('subnet3Name'))]",
    "apiVersion": "2018-08-01",
    "location": "[resourceGroup().location]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "securityRules": [
            {
                "name": "allowSvcFabSMB",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "445",
                    "direction": "Inbound",
                    "priority": 3950,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow SMB traffic within the net, used by fabric to move packages around"
                }
            },
            {
                "name": "allowSvcFabCluser",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "1025-1027",
                    "direction": "Inbound",
                    "priority": 3920,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow ports within vnet that are used by the fabric to talk between nodes"
                }
            },
            {
                "name": "allowSvcFabEphemeral",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[concat(variables('nt3ephemeralStartPort'), '-', variables('nt3ephemeralEndPort'))]",
                    "direction": "Inbound",
                    "priority": 3930,
                    "protocol": "*",
                    "sourceAddressPrefix": "VirtualNetwork",
                    "sourcePortRange": "*",
                    "description": "allow fabric ephemeral ports within the vnet"
                }
            },
            {
                "name": "allowSvcFabPortal",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3fabricHttpGatewayPort')]",
                    "direction": "Inbound",
                    "priority": 3900,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used to access the fabric cluster web portal"
                }
            },
            {
                "name": "allowSvcFabClient",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3fabricTcpGatewayPort')]",
                    "direction": "Inbound",
                    "priority": 3910,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used by the fabric client (includes powershell)"
                }
            },
            {
                "name": "allowSvcFabApplication",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[concat(variables('nt3applicationStartPort'), '-', variables('nt3applicationEndPort'))]",
                    "direction": "Inbound",
                    "priority": 3940,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow fabric application ports within the vnet"
                }
            },
            {
                "name": "blockAll",
                "properties": {
                    "access": "Deny",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "*",
                    "direction": "Inbound",
                    "priority": 4095,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "block all traffic except what we've explicitly allowed"
                }
            },
            {
                "name": "allowVNetRDP",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "3389",
                    "direction": "Inbound",
                    "priority": 3960,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow RDP within the net"
                }
            },
            {
                "name": "allowSvcFabReverseProxy",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[variables('nt3reverseProxyEndpointPort')]",
                    "direction": "Inbound",
                    "priority": 3980,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow port used to access the fabric cluster using reverse proxy"
                }
            },
            {
                "name": "allowAppPort1",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[parameters('loadBalancedAppPort1')]",
                    "direction": "Inbound",
                    "priority": 2001,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow public application port 1"
                }
            },
            {
                "name": "allowAppPort2",
                "properties": {
                    "access": "Allow",
                    "destinationAddressPrefix": "*",
                    "destinationPortRange": "[parameters('loadBalancedAppPort2')]",
                    "direction": "Inbound",
                    "priority": 2002,
                    "protocol": "*",
                    "sourceAddressPrefix": "*",
                    "sourcePortRange": "*",
                    "description": "allow public application port 2"
                }
            }
        ]
    }
},
{
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "sku": {
        "name": "[parameters('vmNodeType3Size')]",
        "capacity": "[parameters('nt3InstanceCount')]",
        "tier": "Standard"
    },
    "name": "[variables('vmNodeType3Name')]",
    "apiVersion": "2018-10-01",
    "location": "[variables('computeLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "overprovision": "[variables('overProvision')]",
        "upgradePolicy": {
            "mode": "Automatic"
        },
        "virtualMachineProfile": {
            "extensionProfile": {
                "extensions": [
                    {
                        "name": "[concat(variables('vmNodeType3Name'),'OMS')]",
                        "properties": {
                            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
                            "type": "MicrosoftMonitoringAgent",
                            "typeHandlerVersion": "1.0",
                            "autoUpgradeMinorVersion": true,
                            "settings": {
                                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
                            },
                            "protectedSettings": {
                                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
                            }
                        }
                    },
                    {
                        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType3Name')]",
                        "properties": {
                            "type": "ServiceFabricNode",
                            "autoUpgradeMinorVersion": true,
                            "protectedSettings": {
                                "StorageAccountKey1": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key1]",
                                "StorageAccountKey2": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('supportLogStorageAccountName')),'2015-05-01-preview').key2]"
                            },
                            "publisher": "Microsoft.Azure.ServiceFabric",
                            "settings": {
                                "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                                "nodeTypeRef": "[variables('vmNodeType3Name')]",
                                "dataPath": "D:\\SvcFab",
                                "durabilityLevel": "Silver",
                                "enableParallelJobs": true,
                                "nicPrefixOverride": "[variables('subnet3Prefix')]",
                                "certificate": {
                                    "thumbprint": "[parameters('certificateThumbprint')]",
                                    "x509StoreName": "[parameters('certificateStoreValue')]"
                                }
                            },
                            "typeHandlerVersion": "1.0"
                        }
                    },
                    {
                        "name": "[concat('VMDiagnosticsVmExt','_vmNodeType3Name')]",
                        "properties": {
                            "type": "IaaSDiagnostics",
                            "autoUpgradeMinorVersion": true,
                            "protectedSettings": {
                                "storageAccountName": "[variables('applicationDiagnosticsStorageAccountName')]",
                                "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-05-01-preview').key1]",
                                "storageAccountEndPoint": "https://core.windows.net/"
                            },
                            "publisher": "Microsoft.Azure.Diagnostics",
                            "settings": {
                                "WadCfg": {
                                    "DiagnosticMonitorConfiguration": {
                                        "overallQuotaInMB": "50000",
                                        "EtwProviders": {
                                            "EtwEventSourceProviderConfiguration": [
                                                {
                                                    "provider": "Microsoft-ServiceFabric-Actors",
                                                    "scheduledTransferKeywordFilter": "1",
                                                    "scheduledTransferPeriod": "PT5M",
                                                    "DefaultEvents": {
                                                        "eventDestination": "ServiceFabricReliableActorEventTable"
                                                    }
                                                },
                                                {
                                                    "provider": "Microsoft-ServiceFabric-Services",
                                                    "scheduledTransferPeriod": "PT5M",
                                                    "DefaultEvents": {
                                                        "eventDestination": "ServiceFabricReliableServiceEventTable"
                                                    }
                                                }
                                            ],
                                            "EtwManifestProviderConfiguration": [
                                                {
                                                    "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8",
                                                    "scheduledTransferLogLevelFilter": "Information",
                                                    "scheduledTransferKeywordFilter": "4611686018427387904",
                                                    "scheduledTransferPeriod": "PT5M",
                                                    "DefaultEvents": {
                                                        "eventDestination": "ServiceFabricSystemEventTable"
                                                    }
                                                }
                                            ]
                                        }
                                    }
                                },
                                "StorageAccount": "[variables('applicationDiagnosticsStorageAccountName')]"
                            },
                            "typeHandlerVersion": "1.5"
                        }
                    },
                    {
                        "name": "[concat('VMIaaSAntimalware','_vmNodeType3Name')]",
                        "properties": {
                            "publisher": "Microsoft.Azure.Security",
                            "type": "IaaSAntimalware",
                            "typeHandlerVersion": "1.5",
                            "settings": {
                                "AntimalwareEnabled": "true",
                                "Exclusions": {
                                    "Paths": "D:\\SvcFab;D:\\SvcFab\\Log;C:\\Program Files\\Microsoft Service Fabric",
                                    "Processes": "Fabric.exe;FabricHost.exe;FabricInstallerService.exe;FabricSetup.exe;FabricDeployer.exe;ImageBuilder.exe;FabricGateway.exe;FabricDCA.exe;FabricFAS.exe;FabricUOS.exe;FabricRM.exe;FileStoreService.exe"
                                },
                                "RealtimeProtectionEnabled": "true",
                                "ScheduledScanSettings": {
                                    "isEnabled": "true",
                                    "scanType": "Quick",
                                    "day": "7",
                                    "time": "120"
                                }
                            },
                            "protectedSettings": null
                        }
                    }
                ]
            },
            "networkProfile": {
                "networkInterfaceConfigurations": [
                    {
                        "name": "[concat(variables('nicName'), '-2')]",
                        "properties": {
                            "ipConfigurations": [
                                {
                                    "name": "[concat(variables('nicName'),'-',2)]",
                                    "properties": {
                                        "loadBalancerBackendAddressPools": [
                                            {
                                                "id": "[variables('lbPoolID3')]"
                                            }
                                        ],
                                        "loadBalancerInboundNatPools": [
                                            {
                                                "id": "[variables('lbNatPoolID3')]"
                                            }
                                        ],
                                        "subnet": {
                                            "id": "[variables('subnet3Ref')]"
                                        }
                                    }
                                }
                            ],
                            "primary": true
                        }
                    }
                ]
            },
            "osProfile": {
                "adminPassword": "[parameters('adminPassword')]",
                "adminUsername": "[parameters('adminUsername')]",
                "computernamePrefix": "[variables('vmNodeType3Name')]",
                "secrets": [
                    {
                        "sourceVault": {
                            "id": "[parameters('sourceVaultValue')]"
                        },
                        "vaultCertificates": [
                            {
                                "certificateStore": "[parameters('certificateStoreValue')]",
                                "certificateUrl": "[parameters('certificateUrlValue')]"
                            }
                        ]
                    }
                ]
            },
            "storageProfile": {
                "imageReference": {
                    "publisher": "[parameters('vmImagePublisher')]",
                    "offer": "[parameters('vmImageOffer')]",
                    "sku": "[parameters('vmImageSku')]",
                    "version": "[parameters('vmImageVersion')]"
                },
                "osDisk": {
                    "caching": "ReadOnly",
                    "createOption": "FromImage",
                    "managedDisk": {
                        "storageAccountType": "[parameters('storageAccountType')]"
                    }
                }
            }
        }
    },
    "dependsOn": [
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', concat('LB','-', parameters('clusterName'),'-',variables('vmNodeType3Name')))]",
        "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]",
        "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
    ]
},
```

In de *template.json* bestand, bijwerken van de cluster-bron en een nieuw knooppunttype toevoegen:
```json
{
    "type": "Microsoft.ServiceFabric/clusters",
    "name": "[parameters('clusterName')]",
    "apiVersion": "2018-02-01",
    "location": "[parameters('clusterLocation')]",
    "tags": {
        "resourceType": "Service Fabric",
        "clusterName": "[parameters('clusterName')]"
    },
    "properties": {
        "nodeTypes": [
            ...
            {
                "name": "[variables('vmNodeType3Name')]",
                "applicationPorts": {
                    "endPort": "[variables('nt3applicationEndPort')]",
                    "startPort": "[variables('nt3applicationStartPort')]"
                },
                "clientConnectionEndpointPort": "[variables('nt3fabricTcpGatewayPort')]",
                "durabilityLevel": "Silver",
                "ephemeralPorts": {
                    "endPort": "[variables('nt3ephemeralEndPort')]",
                    "startPort": "[variables('nt3ephemeralStartPort')]"
                },
                "httpGatewayEndpointPort": "[variables('nt3fabricHttpGatewayPort')]",
                "isPrimary": false,
                "reverseProxyEndpointPort": "[variables('nt3reverseProxyEndpointPort')]",
                "vmInstanceCount": "[parameters('nt3InstanceCount')]"
            }
        ],    
    }
}                
```

In de *parameters.json* bestand, voeg de volgende nieuwe parameters en waarden toe:
```json
"nt3InstanceCount": {
    "Value": 5    
},
"vmNodeType3Size": {
    "Value": "Standard_D2_V2"
},
```

### <a name="deploy-the-updated-template"></a>De bijgewerkte sjabloon implementeren
Opslaan van wijzigingen in de *template.json* en *parameters.json* bestanden.  Voer de volgende opdracht voor het implementeren van de bijgewerkte sjabloon:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "AddingNodeType"
```
Of de volgende Azure CLI-opdracht uit:
```azure-cli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="remove-a-node-type-from-the-cluster"></a>Een knooppunttype uit het cluster verwijderen
Na het maken van een Service Fabric-cluster, kunt u een cluster horizontaal schalen door een knooppunttype (virtuele-machineschaalset) en alle bijbehorende knooppunten te verwijderen. U kunt het cluster schalen op elk gewenst moment, zelfs wanneer workloads worden uitgevoerd op het cluster. Als het cluster wordt geschaald, wordt uw toepassingen automatisch ook schalen.

> [!WARNING]
> Remove-AzureRmServiceFabricNodeType gebruiken om te verwijderen van een knooppunttype uit een productiecluster wordt niet aanbevolen moet regelmatig worden gebruikt. Het is een opdracht als de resource VM scale set achter het knooppunttype worden verwijderd. 

Als u wilt verwijderen van het knooppunttype, voer de [Remove-AzureRmServiceFabricNodeType](/powershell/module/azurerm.servicefabric/remove-azurermservicefabricnodetype) cmdlet.  Het knooppunttype moet Silver- of Gold [duurzaamheidsniveau] [ durability] de cmdlet Hiermee verwijdert u de schaalset die is gekoppeld aan het knooppunttype en duurt enige tijd om te voltooien.  Voer vervolgens de [Remove-ServiceFabricNodeState](/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) cmdlet uit op elk van de knooppunten te verwijderen, verwijdert de status en Hiermee verwijdert u de knooppunten van het cluster. Als er services op de knooppunten zijn, klikt u vervolgens de services eerst verplaatst uit naar een ander knooppunt. Als de clustermanager een knooppunt niet voor de replica/service vinden kan, klikt u vervolgens is de bewerking vertraagd/geblokkeerd.

```powershell
$groupname = "sfclustertutorialgroup"
$nodetype = "nt4vm"
$clustername = "mysfcluster123"

Remove-AzureRmServiceFabricNodeType -Name $clustername  -NodeType $nodetype -ResourceGroupName $groupname

Connect-ServiceFabricCluster -ConnectionEndpoint mysfcluster123.eastus.cloudapp.azure.com:19000 `
          -KeepAliveIntervalInSec 10 `
          -X509Credential -ServerCertThumbprint <thumbprint> `
          -FindType FindByThumbprint -FindValue <thumbprint> `
          -StoreLocation CurrentUser -StoreName My

$nodes = Get-ServiceFabricNode | Where-Object {$_.NodeType -eq $nodetype} | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending

Foreach($node in $nodes)
{
    Remove-ServiceFabricNodeState -NodeName $node.NodeName -TimeoutSec 300 -Force 
}
```

## <a name="increase-node-resources"></a>Knooppunt resources verhogen 
Na het maken van een Service Fabric-cluster, kunt u een cluster-knooppunttype verticaal schalen (de resources van de knooppunten wijzigen) of een upgrade van het besturingssysteem van het knooppunttype VM's.  

> [!WARNING]
> We raden u aan de VM-SKU van een scale set/knooppunttype niet te wijzigen, tenzij het wordt uitgevoerd op Silver duurzaamheid of hoger. De VM-SKU-grootte te wijzigen, is een gegevens-destructieve ter plekke infrastructuur-bewerking. Zonder enige mogelijkheid vertraging of bewaken van deze wijziging, is het mogelijk dat de bewerking kan leiden gegevensverlies voor stateful services tot of andere onvoorziene operationele problemen, zelfs voor staatloze werkbelastingen veroorzaken.

> [!WARNING]
> We raden u aan de VM-SKU van het primaire knooppunttype, dat is een bewerking op schadelijke en niet-ondersteunde niet te wijzigen.  Als u meer clustercapaciteit nodig hebt, kunt u meer VM-exemplaren of extra knooppunttypen toevoegen.  Als dat niet mogelijk is, kunt u een nieuw cluster maken en [toepassingsstatus terugzetten](service-fabric-reliable-services-backup-restore.md) (indien van toepassing) van het oude cluster.  Als dat niet mogelijk is, kunt u [wijzigen van de VM-SKU van het primaire knooppunttype](service-fabric-scale-up-node-type.md).

### <a name="update-the-template"></a>De sjabloon bijwerken

[Een sjabloon en parameters-bestand exporteren](#export-the-template-for-the-resource-group) uit de resourcegroep voor de meest recente implementatie.  Open de *parameters.json* bestand.  Als u het cluster met behulp geïmplementeerd de [voorbeeldsjabloon] [ template] in deze zelfstudie, er zijn drie typen van de knooppunten in het cluster.  

De grootte van de virtuele machines in het tweede knooppunttype is ingesteld in de *vmNodeType1Size* parameter.  Wijziging de *vmNodeType1Size* parameterwaarde van Standard_D2_V2 naar [Standard_D3_V2](/azure/virtual-machines/windows/sizes-general#dv2-series), die de resources van elke VM-exemplaar verdubbeld.

De VM-SKU voor alle drie typen is ingesteld in de *vmImageSku* parameter.  Nogmaals, wijzigen van de VM-SKU van een knooppunttype moet worden gerealiseerd met waarschuwing en wordt niet aanbevolen voor het primaire knooppunttype.

### <a name="deploy-the-updated-template"></a>De bijgewerkte sjabloon implementeren
Opslaan van wijzigingen in de *template.json* en *parameters.json* bestanden.  Voer de volgende opdracht voor het implementeren van de bijgewerkte sjabloon:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName sfclustertutorialgroup -TemplateFile c:\temp\template.json -TemplateParameterFile c:\temp\parameters.json -Name "ScaleUpNodeType"
```
Of de volgende Azure CLI-opdracht uit:
```azure-cli
az group deployment create --resource-group sfclustertutorialgroup --template-file c:\temp\template.json --parameters c:\temp\parameters.json
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Toevoegen en verwijderen van knooppunten (scale-out en inschalen)
> * Toevoegen en verwijderen van knooppunttypen (scale-out en inschalen)
> * Knooppunt-resources (Omhoog schalen) verhogen

Daarna gaat u verder met de volgende zelfstudie, waarin u leert hoe u een upgrade uitvoert van de runtime van een cluster.
> [!div class="nextstepaction"]
> [De runtime van een cluster upgraden](service-fabric-tutorial-upgrade-cluster.md)

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[reliability]: service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster
[template]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.json
[parameters]:https://github.com/Azure-Samples/service-fabric-cluster-templates/blob/master/7-VM-Windows-3-NodeTypes-Secure-NSG/AzureDeploy.Parameters.json
