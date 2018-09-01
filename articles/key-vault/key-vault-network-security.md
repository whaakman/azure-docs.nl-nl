---
ms.assetid: ''
title: Azure Key Vault-Firewalls en virtuele netwerken configureren
description: Stapsgewijze instructies voor het configureren van Key Vault-firewalls en virtuele netwerken
services: key-vault
author: amitbapat
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/31/2018
ms.author: ambapat
ms.openlocfilehash: c58fc56742c0a11771bdd84e4195e6f476622a3b
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43345086"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Azure Key Vault-Firewalls en virtuele netwerken configureren

Deze handleiding beschrijft de stapsgewijze instructies voor het configureren van Key Vault-firewalls en virtuele netwerken om te beperken van toegang tot uw key vault. De [Virtual Network-Service-eindpunten voor Key Vault](key-vault-overview-vnet-service-endpoints.md) kunt u toegang tot key vault voor de opgegeven Virtueelnetwerk en/of een set met IPv4 (Internet Protocol versie 4)-adresbereiken beperken.

> [!IMPORTANT]
> Wanneer de firewall en virtual network-regels in feite alle Key Vault zijn [gegevenslaag](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) bewerkingen kunnen alleen worden uitgevoerd wanneer de oproepende functie aanvragen afkomstig zijn uit de toegestane virtuele netwerken of een IPV4-adresbereik. Dit geldt ook voor toegang tot key vault vanuit Azure portal. Wanneer een gebruiker kan de browser tot een key vault vanuit Azure portal, ze niet mogelijk aan de lijst met sleutels/geheimen/certificaten als hun clientmachine zich niet in de lijst met toegestane. Dit ook van invloed op de 'Key Vault kiezer' door andere Azure-services. Gebruikers mogelijk om te bekijken van de lijst met sleutelkluizen maar sleutels, niet wordt vermeld als de firewall-regels te voorkomen dat de client-computer.

## <a name="azure-portal"></a>Azure Portal

1. Navigeer naar de sleutelkluis die u wilt beveiligen.
2. Klik op **Firewalls en virtuele netwerken**.
3. Klik op **geselecteerde netwerken** onder **zodat toegang vanaf**.
4. Bestaande virtuele netwerken toevoegen aan firewalls en regels voor virtueel netwerk, klikt u op **+ bestaande virtuele netwerken toevoegen**.
5. In de nieuwe blade die wordt weergegeven, selecteert u het abonnement, virtuele netwerken en subnetten die u wilt toestaan dat toegang tot deze sleutelkluis. Als de virtuele netwerken en subnetten die u selecteert, hebben geen service-eindpunten ingeschakeld ziet u een bericht, ' de volgende netwerken zijn geen service-eindpunten enavled...'. Klik op **inschakelen** wanneer blijkt dat u wilt inschakelen van service-eindpunten voor de lijst de virtuele netwerken en subnetten. Het duurt maximaal 15 minuten pas van kracht.
6. U kunt ook nieuwe virtuele netwerken en subnetten toevoegen en schakel vervolgens de service-eindpunten voor de zojuist gemaakte virtuele netwerken en subnetten, door te klikken op **+ nieuw virtueel netwerk toevoegen** en de prompts te volgen.
7. Onder **IP-netwerken**, kunt u IPv4-adresbereiken toevoegen door te typen van IPv4-adresbereiken in [(Classless Inter-Domain Routing) CIDR-notatie](https://tools.ietf.org/html/rfc4632) of afzonderlijke IP-adressen.
8. Klik op **Opslaan**.

## <a name="azure-cli-20"></a>Azure CLI 2.0

1. [Azure CLI 2.0 installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) en [aanmelding](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Lijst met beschikbare virtual network-regels, als u niet alle regels voor deze key vault hebt ingesteld, de lijst wordt niet leeg zijn.
```azurecli
az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
```

3. Service-eindpunt voor Key Vault op een bestaand Virtueelnetwerk en subnet inschakelen
```azurecli
az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
```

4. Een regel voor een virtueel netwerk en subnet toevoegen
```azurecli
subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
```

5. IP-adresbereik voor het toestaan van verkeer van toevoegen
```azurecli
az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
```

6. Als deze sleutelkluis moet toegankelijk zijn via een vertrouwde services, set 'negeren' tot Azure-Services
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
```

7. Nu de laatste en belangrijke stap, schakel het netwerk op regels door in te stellen de standaardactie op 'Weigeren'
```azurecli
az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
```

## <a name="azure-powershell"></a>Azure PowerShell

1. Installeer de meest recente [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) en [aanmelding](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Lijst met beschikbare virtual network-regels, als u niet alle regels voor deze key vault hebt ingesteld, de lijst wordt niet leeg zijn.
```PowerShell
(Get-AzureRmKeyVault -VaultName "mykeyvault").NetworkAcls
```

3. Service-eindpunt voor Key Vault op een bestaand Virtueelnetwerk en subnet inschakelen
```PowerShell
Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzureRmVirtualNetwork
```

4. Een regel voor een virtueel netwerk en subnet toevoegen
```PowerShell
$subnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mysubnet"
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
```

5. IP-adresbereik voor het toestaan van verkeer van toevoegen
```PowerShell
Add-AzureRmKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
```

6. Als deze sleutelkluis moet toegankelijk zijn via een vertrouwde services, set 'negeren' tot Azure-Services
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
```

7. Nu de laatste en belangrijke stap, schakel het netwerk op regels door in te stellen de standaardactie op 'Weigeren'
```PowerShell
Update-AzureRmKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
```

## <a name="references"></a>Verwijzingen

* Azure CLI 2.0-opdrachten - [az keyvault-regel van het netwerk](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell-cmdlets - [Get-AzureRmKeyVault](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurermkeyvault), [toevoegen AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureRmKeyVaultNetworkRule), [Remove-AzureRmKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureRmKeyVaultNetworkRule), [ Update-AzureRmKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureRmKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Volgende stappen

* [Virtual Network-Service-eindpunten voor Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Uw Key Vault beveiligen](key-vault-secure-your-key-vault.md)