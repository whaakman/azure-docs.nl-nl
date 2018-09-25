---
title: Maak een Azure-netwerk-peering - verschillende implementatiemodellen - hetzelfde abonnement | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk dat peering tussen virtuele netwerken die zijn gemaakt via verschillende Azure-implementatiemodellen die aanwezig zijn in hetzelfde Azure-abonnement.
services: virtual-network
documentationcenter: ''
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial;anavin
ms.openlocfilehash: 44cc582bfa0a6940de7eeea9b54e3979735c07e2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998224"
---
# <a name="create-a-virtual-network-peering---different-deployment-models-same-subscription"></a>Maak een virtueel-netwerkpeering - verschillende implementatiemodellen, hetzelfde abonnement

In deze zelfstudie leert u een virtueel netwerk dat peering tussen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen te maken. Beide virtuele netwerken zich in hetzelfde abonnement. Peering van twee virtuele netwerken kunnen resources in verschillende virtuele netwerken met elkaar communiceren met de bandbreedte en de latentie, alsof de resources zich in hetzelfde virtuele netwerk. Meer informatie over [peering van virtuele netwerken](virtual-network-peering-overview.md).

De stappen voor het maken van een virtueel netwerk-peering zijn verschillend, afhankelijk van of de virtuele netwerken in dezelfde of verschillende zijn, abonnementen en welke [Azure-implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de virtuele netwerken zijn gemaakt via. Informatie over het maken van een virtueel netwerk-peering in andere scenario's door te klikken op het scenario in de volgende tabel:

|Azure-implementatiemodel  | Azure-abonnement  |
|--------- |---------|
|[Beide in Resource Manager](tutorial-connect-virtual-networks-portal.md) |Dezelfde|
|[Beide in Resource Manager](create-peering-different-subscriptions.md) |Verschil|
|[Een Resource Manager, één klassiek](create-peering-different-deployment-models-subscriptions.md) |Verschil|

Peering op virtueel netwerk kan niet worden gemaakt tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel. Als u verbinding maken met virtuele netwerken die zijn beide gemaakt via het klassieke implementatiemodel wilt, kunt u een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) om de virtuele netwerken te verbinden.

Virtuele netwerken in dezelfde regio van deze zelfstudie collega's. U kunt ook virtuele netwerken in verschillende koppelen [ondersteunde regio's](virtual-network-manage-peering.md#cross-region). Het wordt aanbevolen dat u bekend zijn met de [peering vereisten en beperkingen](virtual-network-manage-peering.md#requirements-and-constraints) voor peering van virtuele netwerken.

U kunt de [Azure-portal](#portal), de Azure [opdrachtregelinterface](#cli) (CLI), Azure [PowerShell](#powershell), of een [Azure Resource Manager-sjabloon](#template)peering op virtueel netwerk maken. Klik op een van de vorige hulpprogramma koppelingen en Ga rechtstreeks naar de stappen voor het maken van een virtueel-netwerkpeering met behulp van het hulpprogramma naar keuze.

## <a name="create-peering---azure-portal"></a>Maken van de peering - Azure portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com). Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#requirements-and-constraints).
2. Klik op **+ nieuw**, klikt u op **netwerken**, klikt u vervolgens op **virtueel netwerk**.
3. In de **virtueel netwerk maken** blade invoeren, of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:
    - **Name**: *myVnet1*
    - **Adresruimte**: *10.0.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnetadresbereik**: *10.0.0.0/24*
    - **Abonnement**: Selecteer uw abonnement
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroup*
    - **Locatie**: *VS-Oost*
4. Klik op **+ New**. In de **Marketplace doorzoeken** in het vak *virtueel netwerk*. Klik op **virtueel netwerk** wanneer deze wordt weergegeven in de lijst met zoekresultaten. 
5. In de **virtueel netwerk** Selecteer **klassieke** in de **een implementatiemodel selecteren** vak en klik vervolgens op **maken**.
6. In de **virtueel netwerk maken** blade invoeren, of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:
    - **Name**: *myVnet2*
    - **Adresruimte**: *10.1.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnetadresbereik**: *10.1.0.0/24*
    - **Abonnement**: Selecteer uw abonnement
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer *myResourceGroup*
    - **Locatie**: *VS-Oost*
7. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myResourceGroup*. Klik op **myResourceGroup** wanneer deze wordt weergegeven in de lijst met zoekresultaten. Een blade wordt weergegeven voor de **myresourcegroup** resourcegroep. De resourcegroep bevat de twee virtuele netwerken in de vorige stappen hebt gemaakt.
8. Klik op **myVNet1**.
9. In de **myVnet1** blade die wordt weergegeven, klikt u op **Peerings** uit de verticale lijst met opties aan de linkerkant van de blade.
10. In de **myVnet1 - Peerings** blade die wordt weergegeven, klikt u op **+ toevoegen**
11. In de **Add peering** blade die wordt weergegeven, invoeren, of Selecteer de volgende opties en klik vervolgens op **OK**:
     - **Name**: *myVnet1ToMyVnet2*
     - **Virtueel netwerk implementatiemodel**: Selecteer **klassieke**. 
     - **Abonnement**: Selecteer uw abonnement
     - **Virtueel netwerk**: klik op **een virtueel netwerk kiezen**, klikt u vervolgens op **myVnet2**.
     - **Toestaan van toegang tot het virtuele netwerk:** ervoor te zorgen dat **ingeschakeld** is geselecteerd.
    Er zijn geen andere instellingen worden gebruikt in deze zelfstudie. Lees voor meer informatie over alle instellingen van peering, [virtueel-netwerkpeerings beheren](virtual-network-manage-peering.md#create-a-peering).
12. Nadat u hebt geklikt **OK** in de vorige stap, de **Add peering** blade wordt gesloten en u ziet de **myVnet1 - Peerings** blade opnieuw. Na enkele seconden weergegeven de peering die u hebt gemaakt in de blade. **Verbonden** wordt vermeld in de **PEERINGSTATUS** kolom voor de **myVnet1ToMyVnet2** u peering gemaakt.

    De peering wordt nu ingesteld. Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
13. **Optionele**: hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
14. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in de [resources verwijderen](#delete-portal) sectie van dit artikel.

## <a name="cli"></a>Maken van de peering - Azure CLI

De volgende stappen met behulp van de klassieke Azure CLI en Azure CLI. U kunt de stappen van de Azure Cloud Shell, door alleen de **uitproberen** knop in een van de volgende stappen uit of door het installeren van de [klassieke CLI](/cli/azure/install-cli-version-1.0.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [CLI](/cli/azure/install-azure-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en de opdrachten uitvoert op uw lokale computer.

1. Als de Cloud Shell gebruikt, gaat u naar stap 2, omdat de Cloud Shell automatisch aangemeld bij Azure. Open een opdrachtsessie en meld u aan voor het gebruik van Azure de `azure login` opdracht.
2. De CLI uitvoert in Service Management-modus door op te geven de `azure config mode asm` opdracht.
3. Voer de volgende opdracht om te maken van het virtuele netwerk (klassiek):

   ```azurecli-interactive
   azure network vnet create --vnet myVnet2 --address-space 10.1.0.0 --cidr 16 --location "East US"
   ```

4. Het volgende bash CLI-script met behulp van de CLI, niet de klassieke CLI uitvoeren. Zie voor opties voor het uitvoeren van bash-scripts CLI op Windows-computer, [Azure CLI installeren op Windows](/cli/azure/install-azure-cli-windows).

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

5. Maak een virtueel netwerk voor peering tussen de twee virtuele netwerken die zijn gemaakt via de verschillende implementatiemodellen met behulp van de CLI. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<subscription id>` met uw abonnements-id. Als u uw abonnements-Id niet weet, voert u de `az account show` opdracht. De waarde voor **id** is in de uitvoer van uw abonnements-id. Plak het gewijzigde script in uw CLI-sessie en druk vervolgens op `Enter`.

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

6. Nadat het script wordt uitgevoerd, Controleer u de peering voor het virtuele netwerk (Resource Manager). Kopieer de volgende opdracht, plak deze in de CLI-sessie en druk vervolgens op `Enter`:

   ```azurecli-interactive
   az network vnet peering list \
     --resource-group myResourceGroup \
     --vnet-name myVnet1 \
     --output table
   ```

   De uitvoer bevat **verbonden** in de **PeeringState** kolom.

   Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
7. **Optionele**: hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
8. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-cli) in dit artikel.

## <a name="powershell"></a>Maken van de peering - PowerShell

1. Installeer de nieuwste versie van de PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) en [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modules. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Start een PowerShell-sessie.
3. Meld u in PowerShell aan bij Azure door het commando `Add-AzureAccount` in te voeren. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#requirements-and-constraints).
4. Voor het maken van een virtueel netwerk (klassiek) met PowerShell, moet u een nieuwe maken of wijzigen van een bestaand, netwerk-configuratiebestand. Meer informatie over het [exporteren en importeren van netwerk-configuratiebestanden bijwerken](virtual-networks-using-network-configuration-file.md). Het bestand moet zijn onder andere de volgende **VirtualNetworkSite** -element voor het virtuele netwerk in deze zelfstudie gebruikt:

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
    > Een gewijzigde netwerkconfiguratiebestand importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen de vorige virtueel netwerk toevoegen en u niet wijzigen of verwijderen van een bestaande virtuele netwerken van uw abonnement. 
5. Meld u aan bij Azure om te maken van het virtuele netwerk (Resource Manager) door in te voeren de `Connect-AzureRmAccount` opdracht. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#requirements-and-constraints).
6. Maak een resourcegroep en een virtueel netwerk (Resource Manager). Het script kopiëren, plak deze in PowerShell en druk vervolgens op `Enter`.

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

7. Maak een virtueel netwerk voor peering tussen de twee virtuele netwerken die zijn gemaakt via de verschillende implementatiemodellen. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<subscription id>` met uw abonnements-id. Als u uw abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Voor het uitvoeren van het script het gewijzigde script kopiëren van de teksteditor en klik vervolgens op met de rechtermuisknop in de PowerShell-sessie en druk vervolgens op `Enter`.

    ```powershell
    # Peer VNet1 to VNet2.
    Add-AzureRmVirtualNetworkPeering `
      -Name myVnet1ToMyVnet2 `
      -VirtualNetwork $vnet1 `
      -RemoteVirtualNetworkId /subscriptions/<subscription Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnet2
    ```

8. Nadat het script wordt uitgevoerd, Controleer u de peering voor het virtuele netwerk (Resource Manager). Kopieer de volgende opdracht, plak deze in uw PowerShell-sessie en druk vervolgens op `Enter`:

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroup `
      -VirtualNetworkName myVnet1 `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    De uitvoer bevat **verbonden** in de **PeeringState** kolom.

    Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

9. **Optionele**: hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
10. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-powershell) in dit artikel.
 
## <a name="delete"></a>Resources verwijderen
Wanneer u deze zelfstudie hebt voltooid, is het raadzaam om de resources die u in de zelfstudie hebt gemaakt, zodat er geen kosten voor het gebruik te verwijderen. Als u een resourcegroep verwijdert, verwijdert u ook alle resources die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Voer in de portal zoeken **myResourceGroup**. Klik in de lijst met zoekresultaten op **myResourceGroup**.
2. Op de **myResourceGroup** blade, klikt u op de **verwijderen** pictogram.
3. De verwijdering te bevestigen, in de **TYPE de naam van de RESOURCEGROEP** Voer **myResourceGroup**, en klik vervolgens op **verwijderen**.

### <a name="delete-cli"></a>Azure CLI

1. De Azure CLI gebruiken voor het verwijderen van het virtuele netwerk (Resource Manager) met de volgende opdracht:

    ```azurecli-interactive
    az group delete --name myResourceGroup --yes
    ```

2. Gebruik de klassiek CLI om te verwijderen van het virtuele netwerk (klassiek) met de volgende opdrachten:

    ```azurecli-interactive
    azure config mode asm

    azure network vnet delete --vnet myVnet2 --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Voer de volgende opdracht om het virtuele netwerk (Resource Manager) te verwijderen:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroup -Force
    ```

2. Als u wilt verwijderen van het virtuele netwerk (klassiek) met PowerShell, wijzigt u een bestaand configuratiebestand van het netwerk. Meer informatie over het [exporteren en importeren van netwerk-configuratiebestanden bijwerken](virtual-networks-using-network-configuration-file.md). Verwijder de volgende VirtualNetworkSite-element voor het virtuele netwerk in deze zelfstudie gebruikt:

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
    > Een gewijzigde netwerkconfiguratiebestand importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen het vorige virtuele netwerk verwijderen en u niet wijzigen of verwijderen van alle andere bestaande virtuele netwerken van uw abonnement. 

## <a name="next-steps"></a>Volgende stappen

- Grondig raken met belangrijke [peering beperkingen van virtueel netwerk en het gedrag](virtual-network-manage-peering.md#requirements-and-constraints) gebruiken voor het maken van een virtueel netwerk-peering voor productie.
- Meer informatie over alle [instellingen van peering in virtuele netwerken](virtual-network-manage-peering.md#create-a-peering).
- Meer informatie over het [maken van een hub- en -spokenetwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met virtueel-netwerkpeering.