---
title: VNet Peering maken met Resource Manager-sjablonen | Microsoft Docs
description: Leer hoe u een virtueel netwerk-peering maakt met de sjablonen in Resource Manager.
services: virtual-network
documentationcenter: 
author: narayanannamalai
manager: jefco
editor: 
tags: azure-resource-manager
ms.assetid: 75f8d10e-23e8-44bd-9972-aab74048cf38
ms.service: virtual-network
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: narayanannamalai;annahar
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 5af02963f139648d9f1b662f2da913ffa0d6f128


---
# <a name="create-vnet-peering-using-resource-manager-templates"></a>VNet Peering maken met Resource Manager-sjablonen
[!INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[!INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Volg de onderstaande stappen om met behulp van Resource Manager-sjablonen een VNet-peering te maken:

1. Als u Azure PowerShell nog niet eerder hebt gebruikt, raadpleegt u [Azure PowerShell installeren en configureren](../powershell-install-configure.md) en volgt u de instructies helemaal tot aan het einde om u aan te melden bij Azure en uw abonnement te selecteren.
   
   > [!NOTE]
   > De PowerShell-cmdlet voor het beheer van VNet peering wordt geleverd met [Azure PowerShell 1.6.](http://www.powershellgallery.com/packages/Azure/1.6.0)
   > 
   > 
2. De onderstaande tekst bevat de definitie van een VNet peeringkoppeling voor VNet1 naar VNet2, op basis van het voorgaande scenario. Kopieer de onderstaande inhoud en sla deze op als bestand met de naam VNetPeeringVNet1.json.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }
3. Het onderstaande deel bevat de definitie van een VNet peeringkoppeling voor VNet2 naar VNet1, op basis van het voorgaande scenario.  Kopieer de onderstaande inhoud en sla deze op als bestand met de naam VNetPeeringVNet2.json.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }
   
    Zoals in de bovenstaande sjabloon zijn er enkele configureerbare eigenschappen voor VNet-peering:
   
   | Optie | Beschrijving | Standaard |
   |:--- |:--- |:--- |
   | AllowVirtualNetworkAccess |Of de adresruimte van een gelijkwaardig VNet al dan niet moet worden opgenomen als onderdeel van de tag virtual_network. |Ja |
   | AllowForwardedTraffic |Of verkeer dat niet afkomstig is van gelijkwaardig VNet wordt geaccepteerd of verwijderd. |Nee |
   | AllowGatewayTransit |Hiermee kan de gelijkwaardige VNet uw VNet-gateway gebruiken. |Nee |
   | UseRemoteGateways |Gebruik de VNet-gateway van uw peer. De gelijkwaardige VNet moeten een geconfigureerde gateway hebben en AllowGatewayTransit moet zijn geselecteerd. U kunt deze optie niet gebruiken als u een gateway hebt geconfigureerd. |Nee |
   
    Elke koppeling in VNet-peering heeft de set eigenschappen die hierboven staan vermeld. U kunt bijvoorbeeld AllowVirtualNetworkAccess instellen op Waar voor de VNET-peeringkoppeling VNet1-naar-VNet2, en instellen op Onwaar voor de VNET-peeringkoppeling in de omgekeerde richting.
4. Om het sjabloonbestand te implementeren kunt u de New-AzureRmResourceGroupDeployment cmdlet uitvoeren om de implementatie te maken of bij te werken. Raadpleeg dit [artikel](../resource-group-template-deploy.md) voor meer informatie over het gebruik van Resource Manager-sjablonen.
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all
   
   > [!NOTE]
   > Vervang de resourcegroepsnaam en het sjabloonbestand waar nodig.
   > 
   > 
   
    Hieronder volgt een voorbeeld op basis van het bovenstaande scenario:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all
   
    Weergave uitvoer:
   
        DeploymentName        : VNetPeeringVNet1
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all
   
    Weergave uitvoer:
   
        DeploymentName        : VNetPeeringVNet2
        ResourceGroupName    : VNet101
        ProvisioningState        : Succeeded
        Timestamp            : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters            :
        Outputs            :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
5. Als de implementatie is voltooid, kunt u de cmdlet hieronder uitvoeren om de peeringstatus weer te geven:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2
   
    Weergave uitvoer:
   
        Name            : LinkToVNet2
        Id                : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName    : VNet101
        VirtualNetworkName    : VNet1
        ProvisioningState        : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess    : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null
   
    Nadat peering in dit scenario tot stand is gebracht, moet u de verbindingen van elke virtuele machine naar elke andere virtuele machine kunnen initiëren in beide VNetten. Standaard is AllowVirtualNetworkAccess ingesteld op Waar en zal VNet de juiste ACL's inrichten zodat de communicatie tussen VNetten is toegestaan. U kunt nog steeds regels voor netwerkbeveiligingsgroepen toepassen om verbindingen tussen specifieke subnetten te blokkeren of te voorkomen dat virtuele machines gedetailleerde controle krijgen over de toegang tussen twee virtuele netwerken.  Raadpleeg dit [artikel](virtual-networks-create-nsg-arm-ps.md) voor meer informatie over het maken van regels voor netwerkbeveiligingsgroepen.

[!INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Volg de onderstaande stappen als u VNet-peering voor meerdere abonnementen wilt maken:

1. Meld u aan bij Azure met bevoegdheden van het account van gebruiker A in abonnement A en voer de volgende cmdlet uit:
   
        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5
   
    Dit is geen vereiste. Peering kan ook tot stand worden gebracht als gebruikers afzonderlijk een peeringaanvraag voor hun respectieve Vnets genereren, mits de aanvragen met elkaar overeenkomen. Door de bevoegde gebruiker van de andere VNet toe te voegen als gebruiker in de lokale VNet, kunt u de peering gemakkelijker instellen.
2. Meld u aan bij Azure met bevoegdheden van het account van gebruiker B voor abonnement B en voer de volgende cmdlet uit:
   
        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3
3. Voer tijdens de sessie met aanmelding voor Gebruiker A de volgende cmdlet uit:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all
   
    Hier ziet u hoe de JSON-bestand wordt gedefinieerd.  
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }
4. Voer tijdens de sessie met aanmelding voor Gebruiker B de volgende cmdlet uit:
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all
   
    Hier ziet u hoe de JSON-bestand wordt gedefinieerd.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }
   
     Nadat peering in dit scenario tot stand is gebracht, moet u de verbindingen van elke virtuele machine naar elke andere virtuele machine kunnen initiëren in beide VNetten over verschillende abonnementen.

[!INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. In dit scenario kunt u dezelfde voorbeeldsjabloon uitvoeren om VNet-peering tot stand te brengen.  U moet de eigenschap AllowForwardedTraffic instellen op True, zodat het virtuele netwerkapparaat in het peered-VNet verkeer kan verzenden en ontvangen.
   
    Hier is de sjabloon voor het maken van een VNet-peering van HubVNet naar VNet1. Houd er rekening mee dat AllowForwardedTraffic is ingesteld op false.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }
2. Hier is de sjabloon voor het maken van een VNet-peering van VNet1 naar HubVnet. Houd er rekening mee dat AllowForwardedTraffic is ingesteld op true.
   
        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }
3. Nadat peering tot stand is gebracht, kunt u verwijzen naar dit [artikel](virtual-network-create-udr-arm-ps.md) om een door de gebruiker gedefinieerde route (UDR) definiëren om VNet1-verkeer om te leiden via een virtueel apparaat om de mogelijkheden te gebruiken. Wanneer u het 'volgende hop'-adres in de route opgeeft, kunt u dit instellen op het IP-adres van het virtuele apparaat in het peer-VNet HubVNet.

[!INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Volg onderstaande stappen om een peering tussen virtuele netwerken van verschillende implementatiemodellen te maken:

1. De onderstaande tekst bevat de definitie van een VNet-peeringkoppeling voor VNET1 naar VNET2 in dit scenario. Er is maar één koppeling vereist om een klassiek virtueel netwerk te koppelen aan een virtueel netwerk van Azure Resource Manager.
   
    Zorg ervoor dat u de abonnement-id invoert voor waar het klassieke virtuele netwerk of VNET2 zich bevindt en wijzig MyResouceGroup in de juiste resourcegroepsnaam.
   
    {  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",  "contentVersion": "1.0.0.0",  "parameters": {  },  "variables": {  },  "resources": [
   
        {
        "apiVersion": "2016-06-01",
        "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
        "name": "VNET1/LinkToVNET2",
        "location": "[resourceGroup().location]",
        "properties": {
        "allowVirtualNetworkAccess": true,
        "allowForwardedTraffic": false,
        "allowGatewayTransit": false,
        "useRemoteGateways": false,
            "remoteVirtualNetwork": {
            "id": "[resourceId('Microsoft.ClassicNetwork/virtualNetworks', 'VNET2')]"
    }
   
        }
        }
    ]  }
2. Als u het sjabloonbestand wilt implementeren, voert u de volgende cmdlet uit om de implementatie te maken of bij te werken.
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all
   
        Output shows:
   
        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent
3. Als de implementatie is gelukt, kunt u de volgende cmdlet uitvoeren om de peeringstatus weer te geven:
   
        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2
   
        Output shows:
   
        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Nadat er peering tot stand is gebracht tussen een klassieke VNet en een resourcemanager-VNet, moet u verbindingen kunnen initiëren van elke virtuele machine in VNET1 naar elke virtuele machine in VNET2 en andersom.




<!--HONumber=Nov16_HO2-->


