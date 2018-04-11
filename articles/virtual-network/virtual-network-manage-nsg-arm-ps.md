---
title: Netwerkbeveiligingsgroepen - Azure PowerShell beheren | Microsoft Docs
description: Informatie over het beheren van netwerkbeveiligingsgroepen met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3706ce6c-d9ae-46cb-a048-f0a4e84dc5cc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca7f4926ca4edf9d20612aca74f6ae5f0ed847b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-network-security-groups-using-powershell"></a>Netwerkbeveiligingsgroepen met behulp van PowerShell beheren

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). In dit artikel wordt behandeld met het implementatiemodel van Resource Manager, die Microsoft voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel aanbeveelt.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Informatie ophalen
U kunt uw bestaande nsg's weergeven, regels voor een bestaande NSG ophalen en weten welke resources een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande nsg's weergeven
Uitvoeren als u wilt weergeven van alle bestaande nsg's in een abonnement, het `Get-AzureRmNetworkSecurityGroup` cmdlet.

Verwachte resultaat:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Voor de lijst met nsg's in een specifieke resourcegroep, voer de `Get-AzureRmNetworkSecurityGroup` cmdlet.

Verwachte uitvoer:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

### <a name="list-all-rules-for-an-nsg"></a>Lijst met alle regels voor een NSG
Om weer te geven van de regels van een NSG met de naam **NSG-FrontEnd**, voer de volgende opdracht:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

Verwachte uitvoer:

    Name                     : rdp-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound

    Name                     : web-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

> [!NOTE]
> U kunt ook `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` voor een lijst met de standaardregels van de **NSG-FrontEnd** NSG.
> 

### <a name="view-nsgs-associations"></a>Nsg's koppelingen weergeven
Om weer te geven welke bronnen de **NSG-FrontEnd** NSG koppelen, is de volgende opdracht uitvoeren:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

Zoek naar de **NetworkInterfaces** en **subnetten** eigenschappen zoals hieronder wordt weergegeven:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

In het vorige voorbeeld is het NSG niet gekoppeld aan alle netwerkinterfaces (NIC's); het is gekoppeld aan een subnet met de naam **FrontEnd**.

## <a name="manage-rules"></a>Regels beheren
U kunt regels toevoegen aan een bestaande NSG, bestaande regels bewerken en regels te verwijderen.

### <a name="add-a-rule"></a>Een regel toevoegen
Een regel voor toestaan toevoegen **inkomende** verkeer op poort **443** vanaf een willekeurige computer naar de **NSG-FrontEnd** NSG, de volgende stappen uit:

1. Voer de volgende opdracht om de bestaande NSG ophalen en opslaan in een variabele:

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Voer de volgende opdracht om een regel toevoegen aan het NSG:

    ```powershell
    Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. Voer de volgende opdracht voor het opslaan van de wijzigingen in de NSG:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    Verwachte uitvoer met alleen de beveiligingsregels voor verbindingen:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Een regel wijzigen
Wijzigen van de regel die eerder is gemaakt waarmee binnenkomend verkeer van de **Internet** alleen de volgende stappen.

1. Voer de volgende opdracht om de bestaande NSG ophalen en opslaan in een variabele:

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Voer de volgende opdracht met de regelinstellingen van de nieuwe:

    ```powershell
    Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix Internet `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. Voer de volgende opdracht voor het opslaan van de wijzigingen in de NSG:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Verwachte uitvoer met alleen de beveiligingsregels voor verbindingen:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Een regel verwijderen
1. Voer de volgende opdracht om de bestaande NSG ophalen en opslaan in een variabele:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Voer de volgende opdracht om de regel verwijderen uit het NSG:

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. Sla de wijzigingen in de NSG met de volgende opdracht:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Verwachte uitvoer met alleen de beveiligingsregels, kennisgeving de **https-regel** wordt niet meer vermeld:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Koppelingen beheren
U kunt een NSG aan subnetten en NIC's kunt koppelen. U kunt ook een NSG van elke bron die deze gekoppeld ontkoppelen.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG aan een NIC koppelt
Om te koppelen de **NSG-FrontEnd** NSG aan de **TestNICWeb1** NIC, de volgende stappen uit:

1. Voer de volgende opdracht om de bestaande NSG ophalen en opslaan in een variabele:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Voer de volgende opdracht op te halen van de bestaande NIC en opslaan in een variabele:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. Stel de **NetworkSecurityGroup** eigenschap van de **NIC** variabele met de waarde van de **NSG** variabele met de volgende opdracht:

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. Voer de volgende opdracht voor het opslaan van de wijzigingen in de NIC:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Verwachte uitvoer waarin alleen de **NetworkSecurityGroup** eigenschap:
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Een NSG van een NIC ontkoppelen
Ontkoppelen de **NSG-FrontEnd** NSG van de **TestNICWeb1** NIC, de volgende stappen uit:

1. Voer de volgende opdracht op te halen van de bestaande NIC en opslaan in een variabele:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. Stel de **NetworkSecurityGroup** eigenschap van de **NIC** variabele **$null** met de volgende opdracht:

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. Voer de volgende opdracht voor het opslaan van de wijzigingen in de NIC:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Verwachte uitvoer waarin alleen de **NetworkSecurityGroup** eigenschap:
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Een NSG van een subnet ontkoppelen
Ontkoppelen de **NSG-FrontEnd** NSG van de **FrontEnd** het subnet van de volgende stappen uit:

1. Voer de volgende opdracht op te halen van de bestaande VNet en opslaan in een variabele:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Voer de volgende opdracht voor het ophalen van de **FrontEnd** subnet en op te slaan in een variabele:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Stel de **NetworkSecurityGroup** eigenschap van de **subnet** variabele **$null** met de volgende opdracht:

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. Sla de wijzigingen aangebracht in het subnet moet u de volgende opdracht uitvoeren:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Verwachte uitvoer waarin alleen de eigenschappen van de **FrontEnd** subnet. U ziet dat er zich geen eigenschap voor **NetworkSecurityGroup**:
   
            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG aan een subnet koppelt
Om te koppelen de **NSG-FrontEnd** NSG aan de **FronEnd** subnet opnieuw, de volgende stappen uitvoeren:

1. Voer de volgende opdracht op te halen van de bestaande VNet en opslaan in een variabele:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Voer de volgende opdracht voor het ophalen van de **FrontEnd** subnet en op te slaan in een variabele:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Voer de volgende opdracht om de bestaande NSG ophalen en opslaan in een variabele:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. Stel de **NetworkSecurityGroup** eigenschap van de **subnet** variabele **$null** met de volgende opdracht:

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. Sla de wijzigingen aangebracht in het subnet moet u de volgende opdracht uitvoeren:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Verwachte uitvoer waarin alleen de **NetworkSecurityGroup** eigenschap van de **FrontEnd** subnet:
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Een NSG verwijderen
U kunt een NSG alleen verwijderen als deze niet aan een resource zijn gekoppeld. Volg de onderstaande stappen voor het verwijderen van een NSG.

1. Om te controleren van de resources die zijn gekoppeld aan een NSG, voer de `azure network nsg show` zoals weergegeven in [weergave nsg's koppelingen](#View-NSGs-associations).
2. Als de NSG gekoppeld aan een NIC's is, voert u de `azure network nic set` zoals in [ontkoppelen van een NSG van een NIC](#Dissociate-an-NSG-from-a-NIC) voor elke NIC. 
3. Als de NSG gekoppeld aan een subnet is, voert u de `azure network vnet subnet set` zoals in [een NSG van een subnet ontkoppelen](#Dissociate-an-NSG-from-a-subnet) voor elk subnet.
4. Voer de volgende opdracht voor het verwijderen van de NSG:

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > De `-Force` parameter zorgt ervoor dat u hoeft niet te bevestigen.
   > 

## <a name="next-steps"></a>Volgende stappen
* [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor nsg's.

