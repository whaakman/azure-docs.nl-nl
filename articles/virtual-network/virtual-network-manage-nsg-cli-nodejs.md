---
title: Beheren van netwerkbeveiligingsgroepen - Azure CLI 1.0 | Microsoft Docs
description: Informatie over het beheren van netwerkbeveiligingsgroepen met behulp van de Azure-opdrachtregelinterface (CLI) 1.0.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2e53c3ff2ffbef95d6b72ca6afb3b4de377f0389
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-network-security-groups-using-the-azure-cli-10"></a>Netwerkbeveiligingsgroepen met behulp van de Azure CLI 1.0 beheren

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren 

U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies: 

- [Azure CLI 1.0](#View-existing-NSGs): onze CLI voor het klassieke implementatiemodel en het Resource Manager-implementatiemodel 
- [Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) -onze volgende generatie CLI voor de resource management-implementatiemodel (in dit artikel)

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel wordt behandeld met het implementatiemodel van Resource Manager, die Microsoft voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel aanbeveelt.
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a>Informatie ophalen
U kunt uw bestaande nsg's weergeven, regels voor een bestaande NSG ophalen en weten welke resources een NSG is gekoppeld aan.

### <a name="view-existing-nsgs"></a>Bestaande nsg's weergeven
Voor de lijst met nsg's in een specifieke resourcegroep, voer de `azure network nsg list` opdracht zoals hieronder wordt weergegeven.

```azurecli
azure network nsg list --resource-group RG-NSG
```

Verwachte uitvoer:

    info:    Executing command network nsg list
    + Getting the network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK

### <a name="list-all-rules-for-an-nsg"></a>Lijst met alle regels voor een NSG
Om weer te geven van de regels van een NSG met de naam **NSG-FrontEnd**, voert de `azure network nsg show` opdracht zoals hieronder wordt weergegeven. 

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd
```

Verwachte uitvoer:

    info:    Executing command network nsg show
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

> [!NOTE]
> U kunt ook `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` voor een lijst met regels uit de **NSG-FrontEnd** NSG.
>

### <a name="view-nsg-associations"></a>Weergave NSG koppelingen

Om weer te geven welke bronnen de **NSG-FrontEnd** NSG koppelen aan de run is de `azure network nsg show` opdracht zoals hieronder wordt weergegeven. U ziet dat het enige verschil is dat het gebruik van de **--json** parameter.

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json
```

Zoek naar de **networkInterfaces** en **subnetten** eigenschappen zoals hieronder wordt weergegeven:

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

In het bovenstaande voorbeeld het NSG is niet gekoppeld aan alle netwerkinterfaces (NIC's) en is gekoppeld aan een subnet met de naam **FrontEnd**.

## <a name="manage-rules"></a>Regels beheren
U kunt regels toevoegen aan een bestaande NSG, bestaande regels bewerken en regels te verwijderen.

### <a name="add-a-rule"></a>Een regel toevoegen
Een regel voor toestaan toevoegen **inkomende** verkeer op poort **443** vanaf een willekeurige computer naar de **NSG-FrontEnd** NSG, voer de volgende opdracht:

```azurecli
azure network nsg rule create --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --description "Allow access to port 443 for HTTPS" \
    --protocol Tcp \
    --source-address-prefix * \
    --source-port-range * \
    --destination-address-prefix * \
    --destination-port-range 443 \
    --access Allow \
    --priority 102 \
    --direction Inbound
```

Verwachte uitvoer:

    info:    Executing command network nsg rule create
    + Looking up the network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a>Een regel wijzigen
Wijzigen van de regel die eerder is gemaakt waarmee binnenkomend verkeer van de **Internet** alleen, voer de volgende opdracht:

```azurecli
azure network nsg rule set --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --source-address-prefix Internet
```

Verwachte uitvoer:

    info:    Executing command network nsg rule set
    + Looking up the network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up the network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access to port 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a>Een regel verwijderen
Voer de volgende opdracht voor het verwijderen van de regel die eerder is gemaakt:

```azurecli
azure network nsg rule delete --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --quiet
```

> [!NOTE]
> De `--quiet` parameter zorgt ervoor dat u hoeft niet te bevestigen.
>

Verwachte uitvoer:

    info:    Executing command network nsg rule delete
    + Looking up the network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a>Koppelingen beheren
U kunt een NSG aan subnetten en NIC's kunt koppelen. U kunt ook een NSG van elke bron die deze gekoppeld ontkoppelen.

### <a name="associate-an-nsg-to-a-nic"></a>Een NSG aan een NIC koppelt
Om te koppelen de **NSG-FrontEnd** NSG aan de **TestNICWeb1** NIC, voer de volgende opdracht:

```azurecli
azure network nic set --resource-group RG-NSG \
    --name TestNICWeb1 \
    --network-security-group-name NSG-FrontEnd
```

Verwachte uitvoer:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Looking up the network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a>Een NSG van een NIC ontkoppelen

Ontkoppelen de **NSG-FrontEnd** NSG van de **TestNICWeb1** NIC, voer de volgende opdracht:

```azurecli
azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""
```

> [!NOTE]
> U ziet de ' ' (lege) waarde voor de `network-security-group-id` parameter. Dit is hoe u een koppeling naar een NSG verwijderen. Niet hetzelfde zijn als de `network-security-group-name` parameter.
> 

Verwachte resultaat:

    info:    Executing command network nic set
    + Looking up the network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up the network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a>Een NSG van een subnet ontkoppelen
Ontkoppelen de **NSG-FrontEnd** NSG van de **FrontEnd** subnet, voer de volgende opdracht:

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-id ""
```

Verwachte uitvoer:

    info:    Executing command network vnet subnet set
    + Looking up the subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up the subnet "FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-to-a-subnet"></a>Een NSG aan een subnet koppelt
Om te koppelen de **NSG-FrontEnd** NSG aan de **FronEnd** subnet opnieuw in en voer de volgende opdracht:

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-name NSG-FronEnd
```

> [!NOTE]
> De bovenstaande opdracht werkt alleen omdat de **NSG-FrontEnd** NSG bevindt zich in dezelfde resourcegroep bevinden als het virtuele netwerk **TestVNet**. Als de NSG in een andere resourcegroep is, moet u de `--network-security-group-id` parameter in plaats daarvan en geef de volledige-id voor het NSG. U kunt de-id ophalen door het uitvoeren van `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` en zoekt u de **id** eigenschap. 
> 

Verwachte uitvoer:

        info:    Executing command network vnet subnet set
        + Looking up the subnet "FrontEnd"
        + Looking up the network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a>Een NSG verwijderen
U kunt een NSG alleen verwijderen als deze niet aan een resource zijn gekoppeld. Volg de onderstaande stappen voor het verwijderen van een NSG.

1. Om te controleren van de resources die zijn gekoppeld aan een NSG, voer de `azure network nsg show` zoals weergegeven in [weergave nsg's koppelingen](#View-NSGs-associations).
2. Als de NSG gekoppeld aan een NIC's is, voert u de `azure network nic set` zoals in [ontkoppelen van een NSG van een NIC](#Dissociate-an-NSG-from-a-NIC) voor elke NIC. 
3. Als de NSG gekoppeld aan een subnet is, voert u de `azure network vnet subnet set` zoals in [een NSG van een subnet ontkoppelen](#Dissociate-an-NSG-from-a-subnet) voor elk subnet.
4. Voer de volgende opdracht voor het verwijderen van de NSG:

    ```azurecli
    azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet
    ```

    Verwachte uitvoer:

        info:    Executing command network nsg delete
        + Looking up the network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a>Volgende stappen
* [Logboekregistratie inschakelen](virtual-network-nsg-manage-log.md) voor nsg's.

