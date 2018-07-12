---
title: Privé-IP-adressen voor VM's - Azure CLI configureren | Microsoft Docs
description: Informatie over het configureren van privé IP-adressen voor virtuele machines met de Azure-opdrachtregelinterface (CLI).
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
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
ms.openlocfilehash: f4f6a40fde23ee70391c5057762f17ce1eb44123
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38561125"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-cli"></a>Configureer persoonlijke IP-adressen voor een virtuele machine met de Azure CLI

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het Resource Manager-implementatiemodel. U kunt ook [statisch privé IP-adres in het klassieke implementatiemodel beheren](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> Het volgende voorbeeld van Azure CLI-opdrachten kunt verwachten van een bestaande eenvoudige omgeving. Als u wilt dat de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst samenstellen van de testomgeving die wordt beschreven [een vnet maken](quick-create-cli.md).

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statisch privé IP-adres opgeven bij het maken van een virtuele machine

Het maken van een virtuele machine met de naam *DNS01* in de *FrontEnd* subnet van een VNet met de naam *TestVNet* met een statisch privé IP-adres van *192.168.1.101*, voltooid de volgende stappen uit:

1. Als u dit nog niet hebt nog, installeren en configureren van de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en aan te melden bij een Azure-account met [az login](/cli/azure/reference-index#az_login). 

2. Maak een openbaar IP-adres voor de virtuele machine met de [az network public-ip maken](/cli/azure/network/public-ip#az_network_public_ip_create) opdracht. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.

    > [!NOTE]
    > U wilt of moet het gebruik van verschillende waarden voor uw argumenten in deze en de daaropvolgende stappen, afhankelijk van uw omgeving.
   
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

   * `--resource-group`: De naam van de resourcegroep waarin u wilt maken van het openbare IP-adres.
   * `--name`: De naam van het openbare IP-adres.
   * `--location`: Azure-regio waarin u wilt maken van het openbare IP-adres.

3. Voer de [az network nic maken](/cli/azure/network/nic#az_network_nic_create) opdracht voor het maken van een NIC met een statisch privé IP-adres. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters. 
   
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

    * `--private-ip-address`: Het statische privé IP-adres voor de NIC.
    * `--vnet-name`: De naam van het VNet waarin u wilt maken van de NIC.
    * `--subnet`: De naam van het subnet waarin u wilt maken van de NIC.

4. Voer de [azure-vm maken](/cli/azure/vm/nic#az_vm_nic_create) opdracht voor het maken van de virtuele machine met behulp van de openbare IP- en NIC eerder hebt gemaakt. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.
   
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
   
   Parameters dan de basic [az vm maken](/cli/azure/vm#az_vm_create) parameters.

   * `--nics`: De naam van de NIC waarop de virtuele machine is gekoppeld.
   
Het verdient aanbeveling dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een virtuele machine, toewijst tenzij die nodig zijn, bijvoorbeeld wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-cli.md). Als u het particuliere IP-adres binnen het besturingssysteem handmatig instellen wilt, controleert u of deze hetzelfde adres als de privé IP-adres toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of het verlies van connectiviteit met de virtuele machine. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen.

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a>Statische privé IP-adresgegevens voor een virtuele machine ophalen

Voer de volgende Azure CLI-opdracht om te bekijken van de waarden voor *toewijzingseenheid privé-IP-methode* en *privé IP-adres*:

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

Verwachte uitvoer:

```json
"192.168.1.101"
```

Als u wilt weergeven van de specifieke IP-informatie van de NIC voor die VM, moet u de NIC specifiek query:

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

U kunt een statisch privé IP-adres niet verwijderen uit een NIC in Azure CLI voor Azure Resource Manager-implementaties. U moet:
- Maak een nieuwe NIC die gebruikmaakt van een dynamische IP-adres
- Instellen van de NIC waarop de virtuele machine de zojuist gemaakte NIC. 

Als u wilt wijzigen van de NIC voor de virtuele machine die wordt gebruikt in de vorige opdrachten, voert u de volgende stappen uit:

1. Voer de **maken van azure network nic** opdracht voor het maken van een nieuwe NIC met behulp van dynamische IP-toewijzing met een nieuw IP-adres. Omdat er geen IP-adres is opgegeven, is het de toewijzingsmethode **dynamische**.

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

2. Voer de **azure vm instellen** opdracht om de NIC die wordt gebruikt door de virtuele machine.
   
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
    > Als de virtuele machine groot genoeg is voor het hebben van meer dan één NIC, voert u de **verwijderen van de azure-netwerk nic** opdracht voor het verwijderen van de oude NIC

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het beheren van [IP-adresinstellingen](virtual-network-network-interface-addresses.md).