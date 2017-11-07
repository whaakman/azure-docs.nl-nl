---
title: Maak een virtueel netwerk van Azure-peering - andere implementatiemodellen - hetzelfde abonnement | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk peering tussen virtuele netwerken via andere Azure-implementatiemodellen die aanwezig zijn in dezelfde Azure-abonnement is gemaakt.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial;anavin
ms.openlocfilehash: ebe489b6e0993dad42950acdafac48e662da7f77
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Maak een virtueel netwerk peering - verschillend implementatiemodellen, hetzelfde abonnement 

In deze zelfstudie leert u een virtueel netwerk peering tussen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen maken. Beide virtuele netwerken zich in hetzelfde abonnement. Peering twee virtuele netwerken kunnen resources in verschillende virtuele netwerken met elkaar communiceren met de bandbreedte en de latentie, alsof de resources in hetzelfde virtuele netwerk. Meer informatie over [virtuele netwerk peering](virtual-network-peering-overview.md). 

De stappen voor het maken van een virtueel netwerk peering verschillen, afhankelijk van of de virtuele netwerken zijn in dezelfde of verschillende abonnementen en die zijn [Azure implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via de virtuele netwerken worden gemaakt. Meer informatie over het maken van een virtueel netwerk peering in andere scenario's door te klikken op het scenario uit de volgende tabel:

|Azure-implementatiemodel  | Azure-abonnement  |
|--------- |---------|
|[Beide Resource Manager](virtual-network-create-peering.md) |Dezelfde|
|[Beide Resource Manager](create-peering-different-subscriptions.md) |Andere|
|[Een Resource Manager, een klassiek](create-peering-different-deployment-models-subscriptions.md) |Andere|

Een virtueel netwerk peering kan niet worden gemaakt tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel. Als u verbinding maken met virtuele netwerken die zijn beide gemaakt via het klassieke implementatiemodel wilt, kunt u een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verbinding van de virtuele netwerken. 

Deze zelfstudie samenwerkt virtuele netwerken in dezelfde regio. De mogelijkheid als peer virtuele netwerken in verschillende regio's is momenteel in preview. Voer de stappen in [registreren voor het algemene virtueel netwerk peering](#register) voordat u virtuele netwerken in verschillende regio's of de peering werkt niet op hetzelfde niveau. De mogelijkheid voor virtuele netwerken in verschillende regio's verbinden met een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) algemeen beschikbaar is en is registratie niet vereist.

U kunt de [Azure-portal](#portal), de Azure [opdrachtregelinterface](#cli) (CLI) Azure [PowerShell](#powershell), of een [Azure Resource Manager-sjabloon](#template)peering van een virtueel netwerk maken. Klik op een van de vorige hulpprogramma koppelingen om rechtstreeks naar de stappen voor het maken van een virtueel netwerk peering met behulp van het hulpprogramma naar keuze te gaan.

## <a name="create-peering---azure-portal"></a>Maken van de peering - Azure-portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie de [machtigingen](#permissions) sectie van dit artikel voor meer informatie.
2. Klik op **+ nieuw**, klikt u op **Networking**, klikt u vervolgens op **virtueel netwerk**.
3. In de **virtueel netwerk maken** blade invoert, of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:
    - **Naam**: *myVnet1*
    - **Adresruimte**: *10.0.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnet-adresbereik**: *10.0.0.0/24*
    - **Abonnement**: uw abonnement te selecteren
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroup*
    - **Locatie**: *VS-Oost*
4. Klik op **+ nieuw**. In de **zoeken van de Marketplace** in het vak *virtueel netwerk*. Klik op **virtueel netwerk** wanneer deze wordt weergegeven in de zoekresultaten. 
5. In de **virtueel netwerk** blade Selecteer **klassieke** in de **een implementatiemodel selecteren** vak en klik vervolgens op **maken**.
6. In de **virtueel netwerk maken** blade invoert, of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:
    - **Naam**: *myVnet2*
    - **Adresruimte**: *10.1.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnet-adresbereik**: *10.1.0.0/24*
    - **Abonnement**: uw abonnement te selecteren
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer *myResourceGroup*
    - **Locatie**: *VS-Oost*
7. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myResourceGroup*. Klik op **myResourceGroup** wanneer deze wordt weergegeven in de zoekresultaten. Een blade wordt weergegeven voor de **myresourcegroup** resourcegroep. De resourcegroep bevat de twee virtuele netwerken in de vorige stappen hebt gemaakt.
8. Klik op **myVNet1**.
9. In de **myVnet1** blade die wordt weergegeven, klikt u op **Peerings** uit de verticale lijst met opties aan de linkerkant van de blade.
10. In de **myVnet1 - Peerings** blade die werden weergegeven, klikt u op **+ toevoegen**
11. In de **toevoegen peering** blade die wordt weergegeven, invoeren, of de volgende opties selecteren en klik vervolgens op **OK**:
     - **Naam**: *myVnet1ToMyVnet2*
     - **Virtueel netwerk implementatiemodel**: Selecteer **klassieke**. 
     - **Abonnement**: uw abonnement te selecteren
     - **Virtueel netwerk**: klik op **Kies een virtueel netwerk**, klikt u vervolgens op **myVnet2**.
     - **Toestaan van toegang tot het virtuele netwerk:** ervoor te zorgen dat **ingeschakeld** is geselecteerd.
    Er zijn geen andere instellingen worden gebruikt in deze zelfstudie. Lees voor meer informatie over instellingen voor alle peering [beheren van virtueel netwerk peerings](virtual-network-manage-peering.md#create-a-peering).
12. Wanneer u op **OK** in de vorige stap, de **toevoegen peering** blade wordt gesloten en u ziet de **myVnet1 - Peerings** blade opnieuw. Na een paar seconden weergegeven de peering die u hebt gemaakt in de blade. **Verbonden** wordt vermeld in de **PEERING STATUS** kolom voor de **myVnet1ToMyVnet2** peering u gemaakt.

    De peer wordt nu ingesteld. Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
13. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
14. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in de [resources verwijderen](#delete-portal) sectie van dit artikel.

## <a name="cli"></a>Peering - maken Azure CLI

1. [Installeer](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de Azure CLI 1.0 voor het maken van het virtuele netwerk (klassiek).
2. Open een opdrachtsessie en meld u aan met behulp van Azure bij de `azure login` opdracht.
3. De CLI in Service Management-modus uitgevoerd door te voeren de `azure config mode asm` opdracht.
4. Voer de volgende opdracht voor het maken van het virtuele netwerk (klassiek):
 
    ```azurecli
    azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```

5. Maak een resourcegroep en een virtueel netwerk (Resource Manager). U kunt de CLI 1.0 of 2.0 gebruiken ([installeren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)). In deze zelfstudie wordt de CLI 2.0 gebruikt voor het maken van het virtuele netwerk (Resource Manager), aangezien 2.0 moet worden gebruikt voor het maken van de peering. Voer de volgende bash CLI script vanaf uw lokale computer met de CLI 2.0.4 of hoger is geïnstalleerd. Zie voor opties op die wordt uitgevoerd, bash-scripts CLI op Windows-client, [uitvoeren van de Azure CLI in Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). U kunt ook het script met behulp van de Azure-Cloud-Shell uitvoeren. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Klik op de **Try it** knop in het script dat volgt, waardoor een Cloud-Shell waarmee u zich aanmeldt bij uw Azure-account met aanmelden kunnen. Voor het uitvoeren van het script, klikt u op de **kopie** knop en plakken, de inhoud in uw Cloud-Shell, drukt u vervolgens op `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus

    # Create the virtual network (Resource Manager).
    az network vnet create \
      --name myVnet1 \
      --resource-group myResourceGroup \
      --location eastus \
      --address-prefix 10.0.0.0/16
    ```

6. Maak een virtueel netwerk peering tussen de twee virtuele netwerken die zijn gemaakt met behulp van de verschillende implementatiemodellen. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<subscription id>` met uw abonnements-id. Als u uw abonnements-Id niet weet, voert u de `az account show` opdracht. De waarde voor **id** in de uitvoer is de abonnement-id. Plak het gewijzigde script in op uw CLI-sessie en druk vervolgens op `Enter`.

    ```azurecli-interactive
    # Get the id for VNet1.
    vnet1Id=$(az network vnet show \
      --resource-group myResourceGroup \
      --name myVnet1 \
      --query id --out tsv)

    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnet1ToMyVnet2 \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --remote-vnet-id /subscriptions/<subscription id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2 \
      --allow-vnet-access
    ```
7. Nadat het script wordt uitgevoerd, moet u controleren de peering voor het virtuele netwerk (Resource Manager). Kopieer de volgende opdracht, plak deze in de CLI-sessie en druk vervolgens op `Enter`:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroup \
      --vnet-name myVnet1 \
      --output table
    ```
    
    De uitvoer wordt weergegeven **verbonden** in de **PeeringState** kolom. 

    Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
8. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
9. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-cli) in dit artikel.

## <a name="powershell"></a>Maken van de peering - PowerShell

1. Installeer de nieuwste versie van de PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) en [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modules. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Een PowerShell-sessie starten.
3. Meld u in PowerShell aan bij Azure door het commando `Add-AzureAccount` in te voeren.
4. Als een virtueel netwerk (klassiek) maken met PowerShell, moet u een nieuwe maken of wijzigen van een bestaand, netwerk-configuratiebestand. Meer informatie over hoe [exporteren, bijwerken en importeren van configuratie van netwerkbestanden](virtual-networks-using-network-configuration-file.md). Het bestand moet bevatten de volgende **VirtualNetworkSite** element voor het virtuele netwerk in deze zelfstudie gebruikt:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Een configuratiebestand gewijzigde netwerk importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen de vorige virtueel netwerk toevoegen en u niet wijzigen of verwijderen van een bestaande virtuele netwerken uit uw abonnement. 
5. Aanmelden bij Azure te maken van het virtuele netwerk (Resource Manager) door te voeren de `login-azurermaccount` opdracht. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie de [machtigingen](#permissions) sectie van dit artikel voor meer informatie.
6. Maak een resourcegroep en een virtueel netwerk (Resource Manager). Kopieer het script, plakt u deze in PowerShell en druk vervolgens op `Enter`.

    ```powershell
    # Create a resource group.
      New-AzureRmResourceGroup -Name myResourceGroup -Location eastus

    # Create the virtual network (Resource Manager).
      $vnet1 = New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Name 'myVnet1' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus
    ```

7. Maak een virtueel netwerk peering tussen de twee virtuele netwerken die zijn gemaakt met behulp van de verschillende implementatiemodellen. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<subscription id>` met uw abonnements-id. Als u uw abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Voor het uitvoeren van het script het gewijzigde script kopiëren van uw teksteditor en vervolgens met de rechtermuisknop in de PowerShell-sessie en druk vervolgens op `Enter`.

    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Nadat het script wordt uitgevoerd, moet u controleren de peering voor het virtuele netwerk (Resource Manager). Kopieer de volgende opdracht, plak deze in uw PowerShell-sessie en druk vervolgens op `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    De uitvoer wordt weergegeven **verbonden** in de **PeeringState** kolom.

    Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

9. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
10. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-powershell) in dit artikel.
 
## <a name="permissions"></a>Machtigingen

De accounts die u gebruikt voor het maken van een virtueel netwerk peering moeten de benodigde rol of machtiging hebben. Bijvoorbeeld, als u twee virtuele netwerken met de naam myVnet1 en myVnet2 zijn peering, uw account moet worden toegewezen aan de volgende minimale rol of machtiging voor elke virtuele netwerk:
    
|Virtueel netwerk|Implementatiemodel|Rol|Machtigingen|
|---|---|---|---|
|myVnet1|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N.v.t.|
|myVnet2|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Meer informatie over [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (alleen voor Resource Manager).

## <a name="delete"></a>Resources verwijderen
Wanneer u deze zelfstudie hebt voltooid, kunt u mogelijk wilt verwijderen van de resources die u hebt gemaakt in de zelfstudie, zodat u geen gebruik kosten. Verwijderen van een resourcegroep, verwijdert tevens alle bronnen die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Voer in het zoekvak portal **myResourceGroup**. Klik in de zoekresultaten op **myResourceGroup**.
2. Op de **myResourceGroup** blade, klikt u op de **verwijderen** pictogram.
3. Het verwijderen te bevestigen, in de **TYPE de naam van een RESOURCEGROEP** Voer **myResourceGroup**, en klik vervolgens op **verwijderen**.

### <a name="delete-cli"></a>Azure CLI

1. Gebruik de Azure CLI 2.0 verwijderen van het virtuele netwerk (Resource Manager) met de volgende opdracht:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Gebruik de Azure CLI 1.0 om te verwijderen van het virtuele netwerk (klassiek) met de volgende opdrachten:

    ```azurecli
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Voer de volgende opdracht om het verwijderen van het virtuele netwerk (Resource Manager):

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroup -Force
    ```

2. Als u wilt verwijderen van het virtuele netwerk (klassiek) met PowerShell, wijzigt u een bestaand configuratiebestand van het netwerk. Meer informatie over hoe [exporteren, bijwerken en importeren van configuratie van netwerkbestanden](virtual-networks-using-network-configuration-file.md). Verwijder de volgende VirtualNetworkSite-element voor het virtuele netwerk in deze zelfstudie gebruikt:

    ```xml
    <VirtualNetworkSite name="myVnet2" Location="East US">
      <AddressSpace>
        <AddressPrefix>10.1.0.0/16</AddressPrefix>
      </AddressSpace>
      <Subnets>
        <Subnet name="default">
          <AddressPrefix>10.1.0.0/24</AddressPrefix>
        </Subnet>
      </Subnets>
    </VirtualNetworkSite>
    ```

    > [!WARNING]
    > Een configuratiebestand gewijzigde netwerk importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen het vorige virtuele netwerk verwijderen en u niet wijzigen of andere bestaande virtuele netwerken uit uw abonnement verwijderen. 

## <a name="register"></a>Registreren voor de peering preview globale virtueel netwerk

De mogelijkheid als peer virtuele netwerken in verschillende regio's is momenteel in preview. De mogelijkheid is beschikbaar in een beperkt aantal regio's (in eerste instantie US West-Centraal, Canada centraal en ons West 2). Virtueel netwerk peerings gemaakt tussen virtuele netwerken in verschillende regio's mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid als een peering tussen virtuele netwerken in dezelfde regio. Controleer de pagina [Azure Virtual Network-updates](https://azure.microsoft.com/updates/?product=virtual-network) voor de meest recente meldingen over de beschikbaarheid en de status van deze functie.

Virtuele netwerken to-peer tussen regio's, moet u eerst registreren voor de preview, door de volgende stappen (binnen het abonnement elk virtueel netwerk dat u wilt peer wordt) met Azure PowerShell of Azure CLI:

### <a name="powershell"></a>PowerShell

1. Installeer de meest recente versie van de PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/)-module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Start een PowerShell-sessie en meld u aan met behulp van Azure bij de `Login-AzureRmAccount` opdracht.
3. Registreer het abonnement dat elke virtuele netwerk dat u wilt peer in voor de preview hiertoe de volgende opdrachten:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
4. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowGlobalVnetPeering `
      -ProviderNamespace Microsoft.Network
    ```

    De stappen in de Portal, Azure CLI, PowerShell of Resource Manager template-secties van dit artikel tot kan niet worden voltooid. de **RegistrationState** uitvoer wordt weergegeven nadat het invoeren van de vorige opdracht is **geregistreerd**  voor beide abonnementen.

### <a name="azure-cli"></a>Azure CLI

1. [Installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2Fazure%2Fvirtual-network%2Ftoc.json).
2. Verzeker u ervan dat u versie 2.0.18 of hoger van de Azure CLI door te voeren de `az --version` opdracht. Als u niet het geval is, installeert u de meest recente versie.
3. Aanmelden bij Azure met de `az login` opdracht.
4. Registreren voor de preview hiertoe de volgende opdrachten:

    ```azurecli-interactive
    az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
    az provider register --name Microsoft.Network
    ```

5. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

    ```azurecli-interactive
    az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
    ```

    De stappen in de Portal, Azure CLI, PowerShell of Resource Manager template-secties van dit artikel tot kan niet worden voltooid. de **RegistrationState** uitvoer wordt weergegeven nadat het invoeren van de vorige opdracht is **geregistreerd**  voor beide abonnementen.

## <a name="next-steps"></a>Volgende stappen

- Grondig raken met belangrijke [peering beperkingen virtueel netwerk en het gedrag](virtual-network-manage-peering.md#requirements-and-constraints) voordat het maken van een virtueel netwerk voor productie-peering gebruikt.
- Meer informatie over alle [peering virtuele netwerkinstellingen](virtual-network-manage-peering.md#create-a-peering).
- Meer informatie over hoe [maken van een hub en spaak netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met het virtuele netwerk peering.
