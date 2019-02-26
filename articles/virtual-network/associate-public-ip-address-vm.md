---
title: Een openbaar IP-adres aan een virtuele machine koppelen
titlesuffix: Azure Virtual Network
description: Leer hoe u een openbaar IP-adres aan een virtuele machine koppelen.
services: virtual-network
documentationcenter: ''
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: jdial
ms.openlocfilehash: be3e9b8003bc6eb585177b285255658c44c7fc36
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/25/2019
ms.locfileid: "56808538"
---
# <a name="associate-a-public-ip-address-to-a-virtual-machine"></a>Een openbaar IP-adres aan een virtuele machine koppelen

In dit artikel leert u hoe u een openbaar IP-adres aan een bestaande virtuele machine (VM) koppelen. Als u wilt verbinding maken met een virtuele machine vanaf het internet, moet de virtuele machine die is gekoppeld aan een openbaar IP-adres hebben. Als u maken van een nieuwe virtuele machine met een openbaar IP-adres wilt, kunt u doen met behulp van de [Azure-portal](virtual-network-deploy-static-pip-arm-portal.md), wordt de [Azure-opdrachtregelinterface (CLI)](virtual-network-deploy-static-pip-arm-cli.md), of [PowerShell](virtual-network-deploy-static-pip-arm-ps.md). Openbare IP-adressen hebben nominale kosten in rekening. Zie voor meer informatie, [prijzen](https://azure.microsoft.com/pricing/details/ip-addresses/). Er is een limiet aan het aantal openbare IP-adressen die u per abonnement kunt. Zie voor meer informatie, [limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits-1).

U kunt de [Azure-portal](#azure-portal), de Azure [opdrachtregelinterface](#azure-cli) (CLI), of [PowerShell](#powershell) om te koppelen van een openbaar IP-adres aan een virtuele machine.

## <a name="azure-portal"></a>Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Blader naar of zoek naar de virtuele machine die u wilt de openbare IP-adres toevoegen en selecteer dit.
3. Onder **instellingen**, selecteer **netwerken**, en selecteer vervolgens de netwerkinterface die u toevoegen van het openbare IP-adres, wilt zoals wordt weergegeven in de volgende afbeelding:

   ![Netwerkinterface selecteren](./media/associate-public-ip-address-vm/select-nic.png)

   > [!NOTE]
   > Openbare IP-adressen zijn gekoppeld aan netwerkinterfaces die zijn gekoppeld aan een virtuele machine. In de vorige afbeelding heeft de virtuele machine slechts één netwerkinterface. Als de virtuele machine meerdere netwerkinterfaces heeft, deze worden alle weergegeven en selecteert u de netwerkinterface die u wilt koppelen aan het openbare IP-adres.

4. Selecteer **IP-configuraties** en selecteer vervolgens een IP-configuratie, zoals wordt weergegeven in de volgende afbeelding:

   ![Selecteer IP-configuratie](./media/associate-public-ip-address-vm/select-ip-configuration.png)

   > [!NOTE]
   > Openbare IP-adressen zijn gekoppeld aan IP-configuraties voor een netwerkinterface. In de vorige afbeelding heeft de netwerkinterface één IP-configuratie. Als de netwerkinterface meerdere IP-configuraties heeft, deze allemaal worden weergegeven in de lijst en selecteert u het IP-configuratie die u wilt koppelen aan het openbare IP-adres.

5. Selecteer **ingeschakeld**en selecteer vervolgens **IP-adres (*vereiste instellingen configureren*)**. Kies een bestaande openbare IP-adres, dat automatisch wordt gesloten de **openbare IP-adres kiezen** vak. Als u geen beschikbare openbare IP-adressen die worden vermeld hebt, moet u er een maken. Voor meer informatie Zie [een openbaar IP-adres maken](virtual-network-public-ip-address.md#create-a-public-ip-address). Selecteer **opslaan**, zoals weergegeven in de afbeelding die volgt op en sluit vervolgens het selectievakje in voor de IP-configuratie.

   ![Openbare IP-adres inschakelen](./media/associate-public-ip-address-vm/enable-public-ip-address.png)

   > [!NOTE]
   > Het openbare IP-adressen die worden weergegeven, zijn die aanwezig zijn in dezelfde regio als de virtuele machine. Als u meerdere openbare IP-adressen die zijn gemaakt in de regio hebt, worden alle hier weergegeven. Als een grijs weergegeven, is omdat het adres al gekoppeld aan een andere resource is.

6. Het openbare IP-adres toegewezen aan de IP-configuratie, zoals wordt weergegeven in de volgende afbeelding weergeven Het duurt een paar seconden voor een IP-adres worden weergegeven.

   ![Toegewezen openbare IP-adres weergeven](./media/associate-public-ip-address-vm/view-assigned-public-ip-address.png)

   > [!NOTE]
   > Het adres wordt toegewezen vanuit een pool van adressen die worden gebruikt in elke Azure-regio. Zie voor een lijst met-adresgroepen gebruikt in elke regio [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Het adres is toegewezen, kan elk adres in de groepen die wordt gebruikt voor de regio zijn. Als u nodig hebt voor het adres moet worden toegewezen uit een specifieke groep in de regio, gebruikt u een [openbare IP-adresvoorvoegsel](public-ip-address-prefix.md).

7. [Netwerkverkeer naar de virtuele machine toestaan](#allow-network-traffic-to-the-vm) met beveiligingsregels in een netwerkbeveiligingsgroep.

## <a name="azure-cli"></a>Azure-CLI

Installeer de [Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), of gebruik de Azure Cloud Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Selecteer de **uitproberen** knop in het CLI-opdrachten die volgen. Selecteren **uitproberen** een Cloud-Shell die u kunt zich aanmelden bij uw Azure-account met aanroept.

1. Als u de CLI lokaal in Bash, aanmelden bij Azure met `az login`.
2. Een openbaar IP-adres is gekoppeld aan een IP-configuratie van een netwerkinterface die is gekoppeld aan een virtuele machine. Gebruik de [az network nic-ip-config update](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-update) opdracht voor het koppelen van een openbaar IP-adres aan een IP-configuratie. Het volgende voorbeeld wordt gekoppeld aan een bestaande openbare IP-adres met de naam *myVMPublicIP* aan de IP-configuratie met de naam *ipconfigmyVM* van een bestaande netwerkinterface met de naam *myVMVMNic* die zich in een resourcegroep met de naam *myResourceGroup*.
  
   ```azurecli-interactive
   az network nic ip-config update \
     --name ipconfigmyVM \
     --nic-name myVMVMNic \
     --resource-group myResourceGroup \
     --public-ip-address myVMPublicIP
   ```

  - Als u een bestaande openbare IP-adres niet hebt, gebruikt u de [az network public-ip maken](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) opdracht een te maken. Bijvoorbeeld, de volgende opdracht maakt u een openbaar IP-adres met de naam *myVMPublicIP* in een resourcegroep met de naam *myResourceGroup*.
  
    ```azurecli-interactive
    az network public-ip create --name myVMPublicIP --resource-group myResourceGroup
    ```

    > [!NOTE]
    > De vorige opdracht maakt een openbaar IP-adres met de standaardwaarden voor de verschillende instellingen die u wilt aanpassen. Zie voor meer informatie over alle openbare IP-adresinstellingen [een openbaar IP-adres maken](virtual-network-public-ip-address.md#create-a-public-ip-address). Het adres wordt toegewezen vanuit een pool van openbare IP-adressen die worden gebruikt voor elke Azure-regio. Zie voor een lijst met-adresgroepen gebruikt in elke regio [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).

  - Als u de naam van een netwerkinterface die is gekoppeld aan uw virtuele machine niet weet, gebruikt u de [az vm nic-lijst](/cli/azure/vm/nic?view=azure-cli-latest#az-vm-nic-list) opdracht om ze te bekijken. Bijvoorbeeld, de volgende opdracht worden de namen van de netwerkinterfaces die zijn gekoppeld aan een virtuele machine met de naam *myVM* in een resourcegroep met de naam *myResourceGroup*:

    ```azurecli-interactive
    az vm nic list --vm-name myVM --resource-group myResourceGroup
    ```

     De uitvoer bevat een of meer regels die vergelijkbaar met het volgende voorbeeld zijn:
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

     In het vorige voorbeeld *myVMVMNic* is de naam van de netwerkinterface.

  - Als u de naam van een IP-configuratie voor een netwerkinterface niet weet, gebruikt u de [az network nic ip-config lijst](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-list) opdracht om op te halen ze. Bijvoorbeeld, de volgende opdracht worden de namen van de IP-configuraties voor een netwerkinterface met de naam *myVMVMNic* in een resourcegroep met de naam *myResourceGroup*:

    ```azurecli-interactive
    az network nic ip-config list --nic-name myVMVMNic --resource-group myResourceGroup --out table
    ```

3. Bekijk het openbare IP-adres toegewezen aan de IP-configuratie met de [az vm list-ip-adressen](/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) opdracht. Het volgende voorbeeld ziet u de IP-adressen toegewezen aan een bestaande virtuele machine met de naam *myVM* in een resourcegroep met de naam *myResourceGroup*.

   ```azurecli-interactive
   az vm list-ip-addresses --name myVM --resource-group myResourceGroup --out table
   ```

   > [!NOTE]
   > Het adres wordt toegewezen vanuit een pool van adressen die worden gebruikt in elke Azure-regio. Zie voor een lijst met-adresgroepen gebruikt in elke regio [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Het adres is toegewezen, kan elk adres in de groepen die wordt gebruikt voor de regio zijn. Als u nodig hebt voor het adres moet worden toegewezen uit een specifieke groep in de regio, gebruikt u een [openbare IP-adresvoorvoegsel](public-ip-address-prefix.md).

4. [Netwerkverkeer naar de virtuele machine toestaan](#allow-network-traffic-to-the-vm) met beveiligingsregels in een netwerkbeveiligingsgroep.

## <a name="powershell"></a>PowerShell

Installeer [PowerShell](/powershell/azure/install-az-ps), of gebruik de Azure Cloud Shell. De Azure Cloud Shell is een gratis shell die u rechtstreeks vanuit Azure Portal kunt uitvoeren. PowerShell vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account heeft. Selecteer de **uitproberen** knop in de PowerShell-opdrachten die volgen. Selecteren **uitproberen** een Cloud-Shell die u kunt zich aanmelden bij uw Azure-account met aanroept.

1. Als u PowerShell lokaal gebruikt, aanmelden bij Azure met `Connect-AzAccount`.
2. Een openbaar IP-adres is gekoppeld aan een IP-configuratie van een netwerkinterface die is gekoppeld aan een virtuele machine. Gebruik de [Get-AzVirtualNetwork](/powershell/module/Az.Network/Get-AzVirtualNetwork) en [Get-AzVirtualNetworkSubnetConfig](/powershell/module/Az.Network/Get-AzVirtualNetworkSubnetConfig) -opdrachten voor het virtuele netwerk en subnet waarin de netwerkinterface zich bevindt. Gebruik vervolgens de [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) opdracht voor het ophalen van een netwerkinterface en de [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) opdracht voor het ophalen van een bestaande openbare IP-adres. Gebruik vervolgens de [Set AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/Set-AzNetworkInterfaceIpConfig) opdracht voor het koppelen van het openbare IP-adres aan de IP-configuratie en de [Set AzNetworkInterface](/powershell/module/Az.Network/Set-AzNetworkInterface) opdracht voor het schrijven van het nieuwe IP-configuratie naar de de netwerkinterface.

   Het volgende voorbeeld wordt gekoppeld aan een bestaande openbare IP-adres met de naam *myVMPublicIP* aan de IP-configuratie met de naam *ipconfigmyVM* van een bestaande netwerkinterface met de naam *myVMVMNic* die zich in een subnet met de naam *myVMSubnet* in een virtueel netwerk met de naam *myVMVNet*. Alle resources zich in een resourcegroep met de naam *myResourceGroup*.
  
   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name myVMVNet -ResourceGroupName myResourceGroup
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name myVMSubnet -VirtualNetwork $vnet
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $pip = Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup
   $nic | Set-AzNetworkInterfaceIpConfig -Name ipconfigmyVM -PublicIPAddress $pip -Subnet $subnet
   $nic | Set-AzNetworkInterface
   ```

  - Als u een bestaande openbare IP-adres niet hebt, gebruikt u de [New-AzPublicIpAddress](/powershell/module/Az.Network/New-AzPublicIpAddress) opdracht een te maken. De volgende opdracht maakt bijvoorbeeld een *dynamische* openbaar IP-adres met de naam *myVMPublicIP* in een resourcegroep met de naam *myResourceGroup* in de  *VS-Oost* regio.
  
    ```azurepowershell-interactive
    New-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup -AllocationMethod Dynamic -Location eastus
    ```

    > [!NOTE]
    > De vorige opdracht maakt een openbaar IP-adres met de standaardwaarden voor de verschillende instellingen die u wilt aanpassen. Zie voor meer informatie over alle openbare IP-adresinstellingen [een openbaar IP-adres maken](virtual-network-public-ip-address.md#create-a-public-ip-address). Het adres wordt toegewezen vanuit een pool van openbare IP-adressen die worden gebruikt voor elke Azure-regio. Zie voor een lijst met-adresgroepen gebruikt in elke regio [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653).

  - Als u de naam van een netwerkinterface die is gekoppeld aan uw virtuele machine niet weet, gebruikt u de [Get-AzVM](/powershell/module/Az.Compute/Get-AzVM) opdracht om ze te bekijken. Bijvoorbeeld, de volgende opdracht worden de namen van de netwerkinterfaces die zijn gekoppeld aan een virtuele machine met de naam *myVM* in een resourcegroep met de naam *myResourceGroup*:

    ```azurepowershell-interactive
    $vm = Get-AzVM -name myVM -ResourceGroupName myResourceGroup
    $vm.NetworkProfile
    ```

    De uitvoer bevat een of meer regels die vergelijkbaar zijn met het volgende voorbeeld. In de voorbeelduitvoer *myVMVMNic* is de naam van de netwerkinterface.
  
    ```
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
    ```

  - Als u niet dat de naam van het virtuele netwerk of subnet waarin de netwerkinterface zich weet bevindt, gebruikt u de `Get-AzNetworkInterface` opdracht om de informatie weer te geven. Bijvoorbeeld, met de volgende opdracht wordt het virtuele netwerk en subnet-informatie voor een netwerkinterface met de naam *myVMVMNic* in een resourcegroep met de naam *myResourceGroup*:

    ```azurepowershell-interactive
    $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
    $ipConfigs = $nic.IpConfigurations
    $ipConfigs.Subnet | Select Id
    ```

     De uitvoer bevat een of meer regels die vergelijkbaar zijn met het volgende voorbeeld. In de voorbeelduitvoer *myVMVNET* is de naam van het virtuele netwerk en *myVMSubnet* is de naam van het subnet.
  
     ```
     "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVMVNET/subnets/myVMSubnet",
     ```

  - Als u de naam van een IP-configuratie voor een netwerkinterface niet weet, gebruikt u de [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) opdracht om op te halen ze. Bijvoorbeeld, de volgende opdracht worden de namen van de IP-configuraties voor een netwerkinterface met de naam *myVMVMNic* in een resourcegroep met de naam *myResourceGroup*:

    ```azurepowershell-interactive
    $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
    $nic.IPConfigurations
    ```

    De uitvoer bevat een of meer regels die vergelijkbaar zijn met het volgende voorbeeld. In de voorbeelduitvoer *ipconfigmyVM* is de naam van een IP-configuratie.
  
    ```
    Id     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM
    ```

3. Bekijk het openbare IP-adres toegewezen aan de IP-configuratie met de [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) opdracht. Het volgende voorbeeld ziet u het adres toegewezen aan een openbaar IP-adres met de naam *myVMPublicIP* in een resourcegroep met de naam *myResourceGroup*.

   ```azurepowershell-interactive
   Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup | Select IpAddress
   ```

   Als u de naam van het openbare IP-adres toegewezen aan een IP-configuratie niet weet, voert u de volgende opdrachten om dit probleem te uit:

   ```azurepowershell-interactive
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $nic.IPConfigurations
   $address = $nic.IPConfigurations.PublicIpAddress
   $address | Select Id
   ```

   De uitvoer bevat een of meer regels die vergelijkbaar zijn met het volgende voorbeeld. In de voorbeelduitvoer *myVMPublicIP* is de naam van het openbare IP-adres toegewezen aan de IP-configuratie.

   ```
   "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP"
   ```

   > [!NOTE]
   > Het adres wordt toegewezen vanuit een pool van adressen die worden gebruikt in elke Azure-regio. Zie voor een lijst met-adresgroepen gebruikt in elke regio [Microsoft Azure Datacenter IP-adresbereiken](https://www.microsoft.com/download/details.aspx?id=41653). Het adres is toegewezen, kan elk adres in de groepen die wordt gebruikt voor de regio zijn. Als u nodig hebt voor het adres moet worden toegewezen uit een specifieke groep in de regio, gebruikt u een [openbare IP-adresvoorvoegsel](public-ip-address-prefix.md).

4. [Netwerkverkeer naar de virtuele machine toestaan](#allow-network-traffic-to-the-vm) met beveiligingsregels in een netwerkbeveiligingsgroep.

## <a name="allow-network-traffic-to-the-vm"></a>Netwerkverkeer naar de virtuele machine toestaan

Voordat u verbinding met het openbare IP-adres van het internet maken kunt, zorg ervoor dat u de vereiste poorten openen in een netwerkbeveiligingsgroep die u mogelijk hebt gekoppeld aan de netwerkinterface, het subnet dat de netwerkinterface zich hebt bevindt, of beide. Hoewel security groepen filter verkeer naar het privé IP-adres van de netwerkinterface, zodra het inkomende internetverkeer op het openbare IP-adres, Azure aankomt vertaalt het openbare adres met de persoonlijke IP-adres, dus als een netwerkbeveiligingsgroep wordt voorkomen dat de verkeersstroom, mislukt de communicatie met het openbare IP-adres. U vindt de effectieve beveiligingsregels voor een netwerkinterface en het subnet met behulp van de [Portal](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-portal), [CLI](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-cli), of [PowerShell](diagnose-network-traffic-filter-problem.md#diagnose-using-powershell).

## <a name="next-steps"></a>Volgende stappen

Binnenkomend internetverkeer naar uw virtuele machine toestaan met een netwerkbeveiligingsgroep. Zie voor meer informatie over het maken van een netwerkbeveiligingsgroep, [werkt met netwerkbeveiligingsgroepen](manage-network-security-group.md#work-with-network-security-groups). Zie voor meer informatie over netwerkbeveiligingsgroepen, [beveiligingsgroepen](security-overview.md).