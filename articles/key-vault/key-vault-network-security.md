---
ms.assetid: ''
title: Azure Key Vault-firewalls en virtuele netwerken - Azure Key Vault configureren
description: Stapsgewijze instructies voor het configureren van Key Vault-firewalls en virtuele netwerken
services: key-vault
author: amitbapat
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.workload: identity
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: c54b78a24068758fabb0918cfeb7d6516fd1bce5
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487232"
---
# <a name="configure-azure-key-vault-firewalls-and-virtual-networks"></a>Azure Key Vault-firewalls en virtuele netwerken configureren

In dit artikel biedt stapsgewijze instructies voor het configureren van Azure Key Vault-firewalls en virtuele netwerken om te beperken van toegang tot uw key vault. De [virtual network-service-eindpunten voor Key Vault](key-vault-overview-vnet-service-endpoints.md) kunt u toegang tot een opgegeven virtuele netwerk en een set met IPv4 (internet protocolversie 4)-adresbereiken beperken.

> [!IMPORTANT]
> Nadat de firewall-regels zijn van kracht, gebruikers alleen Key Vault kunnen uitvoeren [gegevenslaag](../key-vault/key-vault-secure-your-key-vault.md#data-plane-access-control) operations wanneer hun aanvragen afkomstig uit de zijn toegestane virtuele netwerken of een IPv4-adresbereik. Dit geldt ook voor toegang tot Key Vault vanuit Azure portal. Hoewel gebruikers naar een key vault vanuit Azure portal bladeren kunnen, ze niet mogelijk een lijst met sleutels, geheimen of certificaten als hun clientmachine zich niet in de lijst met toegestane. Dit ook van invloed op de kiezer voor Key Vault met andere Azure-services. Gebruikers mogelijk overzicht van sleutelkluizen, maar niet sleutels, weergeven als de firewall-regels te voorkomen dat de client-computer.

## <a name="use-the-azure-portal"></a>Azure Portal gebruiken

Dit is hoe u Key Vault-firewalls en virtuele netwerken configureren met behulp van de Azure-portal:

1. Blader naar de sleutelkluis die u wilt beveiligen.
2. Selecteer **Firewalls en virtuele netwerken**.
3. Onder **zodat toegang vanaf**, selecteer **geselecteerde netwerken**.
4. Selecteer om bestaande virtuele netwerken toe aan firewalls en virtuele netwerkregels **+ bestaande virtuele netwerken toevoegen**.
5. Selecteer in de nieuwe blade die wordt geopend, het abonnement, virtuele netwerken en subnetten die u wilt toestaan dat toegang tot deze sleutelkluis. Als de virtuele netwerken en subnetten die u selecteert geen service-eindpunten ingeschakeld, moet u bevestigen dat u wilt inschakelen van service-eindpunten en selecteert u **inschakelen**. Het duurt maximaal 15 minuten te activeren.
6. Onder **IP-netwerken**, IPv4-adresbereiken toevoegen door te typen van IPv4-adresbereiken in [(Classless Inter-Domain Routing) CIDR-notatie](https://tools.ietf.org/html/rfc4632) of afzonderlijke IP-adressen.
7. Selecteer **Opslaan**.

U kunt ook nieuwe virtuele netwerken en subnetten toevoegen en schakel vervolgens de service-eindpunten voor de zojuist gemaakte virtuele netwerken en subnetten, door te selecteren **+ nieuw virtueel netwerk toevoegen**. Volg de instructies.

## <a name="use-the-azure-cli"></a>Azure CLI gebruiken 

Hier wordt beschreven hoe u Key Vault-firewalls en virtuele netwerken configureren met behulp van de Azure CLI

1. [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) en [aanmelden](https://docs.microsoft.com/cli/azure/authenticate-azure-cli).

2. Lijst met beschikbare virtual network-regels. Als u geen regels voor deze key vault hebt ingesteld, is de lijst niet leeg zijn.
   ```azurecli
   az keyvault network-rule list --resource-group myresourcegroup --name mykeyvault
   ```

3. Een service-eindpunt voor Key Vault inschakelen op een bestaand virtueel netwerk en subnet.
   ```azurecli
   az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.KeyVault"
   ```

4. Voeg een regel voor een virtueel netwerk en subnet toe.
   ```azurecli
   subnetid=$(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
   az keyvault network-rule add --resource-group "demo9311" --name "demo9311premium" --subnet $subnetid
   ```

5. Toevoegen van een IP-adresbereik van waaruit u verkeer toe te staan.
   ```azurecli
   az keyvault network-rule add --resource-group "myresourcegroup" --name "mykeyvault" --ip-address "191.10.18.0/24"
   ```

6. Als deze sleutelkluis moet toegankelijk zijn via een vertrouwde services, stelt u `bypass` naar `AzureServices`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mykeyvault" --bypass AzureServices
   ```

7. De netwerkregels inschakelen door het instellen van de standaardactie op `Deny`.
   ```azurecli
   az keyvault update --resource-group "myresourcegroup" --name "mekeyvault" --default-action Deny
   ```

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dit is het configureren van Key Vault-firewalls en virtuele netwerken met behulp van PowerShell:

1. Installeer de meest recente [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps), en [aanmelden](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

2. Lijst met beschikbare virtual network-regels. Als u niet alle regels voor deze key vault hebt ingesteld, is de lijst niet leeg zijn.
   ```powershell
   (Get-AzKeyVault -VaultName "mykeyvault").NetworkAcls
   ```

3. Service-eindpunt voor Key Vault inschakelen op een bestaand virtueel netwerk en subnet.
   ```powershell
   Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.1.1.0/24" -ServiceEndpoint "Microsoft.KeyVault" | Set-AzVirtualNetwork
   ```

4. Voeg een regel voor een virtueel netwerk en subnet toe.
   ```powershell
   $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -VirtualNetworkResourceId $subnet.Id
   ```

5. Toevoegen van een IP-adresbereik van waaruit u verkeer toe te staan.
   ```powershell
   Add-AzKeyVaultNetworkRule -VaultName "mykeyvault" -IpAddressRange "16.17.18.0/24"
   ```

6. Als deze sleutelkluis moet toegankelijk zijn via een vertrouwde services, stelt u `bypass` naar `AzureServices`.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -Bypass AzureServices
   ```

7. De netwerkregels inschakelen door het instellen van de standaardactie op `Deny`.
   ```powershell
   Update-AzKeyVaultNetworkRuleSet -VaultName "mykeyvault" -DefaultAction Deny
   ```

## <a name="references"></a>Verwijzingen

* Azure CLI-opdrachten: [az keyvault-regel van het netwerk](https://docs.microsoft.com/cli/azure/keyvault/network-rule?view=azure-cli-latest)
* Azure PowerShell-cmdlets: [Get-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvault), [Add-AzKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/az.KeyVault/Add-azKeyVaultNetworkRule), [Remove-AzKeyVaultNetworkRule](https://docs.microsoft.com/powershell/module/az.KeyVault/Remove-azKeyVaultNetworkRule), [Update-AzKeyVaultNetworkRuleSet](https://docs.microsoft.com/powershell/module/az.KeyVault/Update-azKeyVaultNetworkRuleSet)

## <a name="next-steps"></a>Volgende stappen

* [Virtual network-service-eindpunten voor Key Vault](key-vault-overview-vnet-service-endpoints.md)
* [Uw Key Vault beveiligen](key-vault-secure-your-key-vault.md)
