---
title: "Configureren van privé IP-adressen voor virtuele machines - Azure CLI | Microsoft Docs"
description: "Informatie over het configureren van privé IP-adressen voor virtuele machines met de Azure-opdrachtregelinterface (CLI)."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d9925b29a60fc46e9ecc775ca132bd2365f64b15
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-cli"></a>Configureer persoonlijke IP-adressen voor een virtuele machine met de Azure CLI

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het Resource Manager-implementatiemodel. U kunt ook [statisch privé IP-adres in het klassieke implementatiemodel beheren](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> De volgende voorbeeldopdrachten van Azure CLI verwachten dat een bestaande eenvoudige omgeving. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in bouwen [een vnet maken](virtual-networks-create-vnet-arm-cli.md).

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a>Bij het maken van een virtuele machine een statisch privé IP-adres opgeven

Maken van een virtuele machine met de naam *DNS01* in de *FrontEnd* subnet van een VNet met de naam *TestVNet* met een statisch privé IP-adres van *192.168.1.101*, voltooid de volgende stappen uit:

1. Als u dit nog niet hebt nog installeren en configureren van de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en meld u aan op een Azure-account met [az aanmelding](/cli/azure/#login). 

2. Maken van een openbaar IP-adres voor de virtuele machine met de [az netwerk openbare ip-maken](/cli/azure/network/public-ip#create) opdracht. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.

    > [!NOTE]
    > Mogelijk moet of wilt u met verschillende waarden voor de argumenten in deze en daaropvolgende stappen, afhankelijk van uw omgeving.
   
    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    Verwachte uitvoer:
   
   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * `--resource-group`: De naam van de resourcegroep waarin u het openbare IP-adres maken.
   * `--name`: De naam van het openbare IP-adres.
   * `--location`: De azure-regio waarin u het openbare IP-adres maken.

3. Voer de [az netwerk nic maken](/cli/azure/network/nic#create) opdracht voor het maken van een NIC met een statisch privé IP-adres. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters. 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    Verwachte uitvoer:
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
    ```
    
    Parameters:

    * `--private-ip-address`: Statische privé IP-adres voor de NIC.
    * `--vnet-name`: De naam van de VNet in te maken van de NIC.
    * `--subnet`: De naam van het subnet in te maken van de NIC.

4. Voer de [azure vm maken](/cli/azure/vm/nic#create) opdracht voor de virtuele machine maken met het openbare IP- en NIC eerder hebt gemaakt. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    Verwachte uitvoer:
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   Andere parameters dan het basic [az vm maken](/cli/azure/vm#create) parameters.

   * `--nics`: De naam van de NIC waaraan de virtuele machine is gekoppeld.
   

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a>Statische persoonlijke IP-adresgegevens voor een virtuele machine ophalen

Voer de volgende opdracht in de Azure CLI om te zien van de waarden voor *toewijzingseenheid particuliere IP-methode* en *particuliere IP-adres*:

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

Verwachte uitvoer:

```json
"192.168.1.101"
```

Om de specifieke IP-informatie van de NIC voor deze VM weergeven, query uitvoeren op de NIC specifiek:

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

De uitvoer ziet er ongeveer zo uit:

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>Een statisch privé IP-adres van een virtuele machine verwijderen

U kunt een statisch privé IP-adres niet verwijderen uit een NIC in Azure CLI voor Azure Resource Manager-implementaties. U moet doen:
- Maak een nieuwe NIC die gebruikmaakt van een dynamische IP-adres
- De NIC ingesteld op de virtuele machine komen de zojuist gemaakte NIC. 

Als u wilt wijzigen de NIC voor de virtuele machine in de vorige opdrachten gebruikt, moet u de volgende stappen uitvoeren:

1. Voer de **nic azure-netwerk maken** opdracht voor het maken van een nieuwe NIC met dynamische IP-toewijzing met een nieuw IP-adres. Omdat er geen IP-adres is opgegeven, is het de toewijzingsmethode **dynamische**.

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    Verwachte uitvoer:

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. Voer de **azure vm set** opdracht de NIC die wordt gebruikt door de virtuele machine wijzigen.
   
    ```azurecli
    azure vm set -g TestRG -n DNS01 -N TestNIC2
    ```

    Verwachte uitvoer:
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > Als de virtuele machine groot genoeg zijn is om meer dan één NIC hebt, voert u de **azure-netwerk nic verwijderen** opdracht voor het verwijderen van de oude NIC.
   
## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [gereserveerde openbare IP-adres](virtual-networks-reserved-public-ip.md) adressen.
* Meer informatie over [instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adressen.
* Raadpleeg de [gereserveerd IP-REST-API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).

