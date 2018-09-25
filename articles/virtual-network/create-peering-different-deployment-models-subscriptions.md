---
title: Een peering op virtueel netwerk maken - implementatie van de verschillende modellen - verschillende abonnementen | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk dat peering tussen virtuele netwerken die zijn gemaakt via verschillende Azure-implementatiemodellen die aanwezig zijn in verschillende Azure-abonnementen.
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
ms.date: 09/15/2017
ms.author: jdial;anavin
ms.openlocfilehash: 01e8b5c518931411ba6e2d75168de6753bf55260
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971719"
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Maak een virtueel-netwerkpeering - verschillende implementatiemodellen en -abonnementen

In deze zelfstudie leert u een virtueel netwerk dat peering tussen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen te maken. De virtuele netwerken zich in verschillende abonnementen. Peering van twee virtuele netwerken kunnen resources in verschillende virtuele netwerken met elkaar communiceren met de bandbreedte en de latentie, alsof de resources zich in hetzelfde virtuele netwerk. Meer informatie over [peering van virtuele netwerken](virtual-network-peering-overview.md).

De stappen voor het maken van een virtueel netwerk-peering zijn verschillend, afhankelijk van of de virtuele netwerken in dezelfde of verschillende zijn, abonnementen en welke [Azure-implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de virtuele netwerken zijn gemaakt via. Informatie over het maken van een virtueel netwerk-peering in andere scenario's door te klikken op het scenario in de volgende tabel:

|Azure-implementatiemodel  | Azure-abonnement  |
|--------- |---------|
|[Beide in Resource Manager](tutorial-connect-virtual-networks-portal.md) |Dezelfde|
|[Beide in Resource Manager](create-peering-different-subscriptions.md) |Verschil|
|[Een Resource Manager, één klassiek](create-peering-different-deployment-models.md) |Dezelfde|

Peering op virtueel netwerk kan niet worden gemaakt tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel. In deze zelfstudie maakt gebruik van virtuele netwerken die bestaan in dezelfde regio. Virtuele netwerken in dezelfde regio van deze zelfstudie collega's. U kunt ook virtuele netwerken in verschillende koppelen [ondersteunde regio's](virtual-network-manage-peering.md#cross-region). Het wordt aanbevolen dat u bekend zijn met de [peering vereisten en beperkingen](virtual-network-manage-peering.md#requirements-and-constraints) voor peering van virtuele netwerken.

Bij het maken van een virtueel netwerk voor peering tussen virtuele netwerken die zich in verschillende abonnementen, moeten abonnementen beide worden gekoppeld aan dezelfde Azure Active Directory-tenant. Als u nog een Azure Active Directory-tenant hebt, kunt u snel [maakt u er een](../active-directory/develop/quickstart-create-new-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#create-a-new-azure-ad-tenant). U kunt virtuele netwerken in verschillende abonnementen verbinden en andere Azure Active Directory-tenants met behulp van een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Kunt u de [Azure-portal](#portal), de Azure [opdrachtregelinterface](#cli) (CLI), of Azure [PowerShell](#powershell) peering op virtueel netwerk maken. Klik op een van de vorige hulpprogramma koppelingen en Ga rechtstreeks naar de stappen voor het maken van een virtueel-netwerkpeering met behulp van het hulpprogramma naar keuze.

## <a name="portal"></a>Maken van de peering - Azure portal

In deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met voor beide abonnementen toegangsmachtigingen gebruikt, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor de logboekregistratie van buiten de portal en slaat u de stappen voor het toewijzen van machtigingen voor een andere gebruiker aan de virtuele netwerken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als UserA. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
2. Klik op **+ nieuw**, klikt u op **netwerken**, klikt u vervolgens op **virtueel netwerk**.
3. In de **virtueel netwerk maken** blade invoeren, of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:
    - **Name**: *myVnetA*
    - **Adresruimte**: *10.0.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnetadresbereik**: *10.0.0.0/24*
    - **Abonnement**: Selecteer abonnement A.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupA*
    - **Locatie**: *VS-Oost*
4. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myVnetA*. Klik op **myVnetA** wanneer deze wordt weergegeven in de lijst met zoekresultaten. Een blade wordt weergegeven voor de **myVnetA** virtueel netwerk.
5. In de **myVnetA** blade die wordt weergegeven, klikt u op **toegangsbeheer (IAM)** uit de verticale lijst met opties aan de linkerkant van de blade.
6. In de **myVnetA - toegangsbeheer (IAM)** blade die wordt weergegeven, klikt u op **+ toevoegen**.
7. In de **machtigingen toevoegen** blade die wordt weergegeven, selecteer **Inzender voor netwerken** in de **rol** vak.
8. In de **Selecteer** selecteert gebruiker b, of typ de e-mailadres van gebruiker b om te zoeken naar deze. De lijst met gebruikers wordt weergegeven is van dezelfde Azure Active Directory-tenant als het instellen van de peering voor virtuele-netwerk. Klik op gebruiker b wanneer deze wordt weergegeven in de lijst.
9. Klik op **Opslaan**.
10. Meld u af bij de portal aan als UserA en meld u aan als UserB.
11. Klik op **+ nieuw**, type *virtueel netwerk* in de **Marketplace doorzoeken** vak en klik vervolgens op **virtueel netwerk** in de lijst met zoekresultaten.
12. In de **Virtueelnetwerk** blade die wordt weergegeven, selecteer **klassieke** in de **een implementatiemodel selecteren** vak en klik vervolgens op **maken**.
13.   Voer de volgende waarden in het virtuele netwerk (klassiek) vak maken die wordt weergegeven:

    - **Name**: *myVnetB*
    - **Adresruimte**: *10.1.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnetadresbereik**: *10.1.0.0/24*
    - **Abonnement**: Selecteer abonnement B.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupB*
    - **Locatie**: *VS-Oost*

14. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myVnetB*. Klik op **myVnetB** wanneer deze wordt weergegeven in de lijst met zoekresultaten. Een blade wordt weergegeven voor de **myVnetB** virtueel netwerk.
15. In de **myVnetB** blade die wordt weergegeven, klikt u op **eigenschappen** uit de verticale lijst met opties aan de linkerkant van de blade. Kopieer de **RESOURCE-ID**, die wordt gebruikt in een latere stap. De resource-ID is vergelijkbaar met het volgende voorbeeld: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
16. Herhaal stap 5-9 voor myVnetB, voeren **UserA** in stap 8.
17. Meld u af bij de portal aan als UserB en aanmelden als gebruiker a.
18. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myVnetA*. Klik op **myVnetA** wanneer deze wordt weergegeven in de lijst met zoekresultaten. Een blade wordt weergegeven voor de **myVnet** virtueel netwerk.
19. Klik op **myVnetA**.
20. In de **myVnetA** blade die wordt weergegeven, klikt u op **Peerings** uit de verticale lijst met opties aan de linkerkant van de blade.
21. In de **myVnetA - Peerings** blade die wordt weergegeven, klikt u op **+ toevoegen**
22. In de **Add peering** blade die wordt weergegeven, invoeren, of Selecteer de volgende opties en klik vervolgens op **OK**:
     - **Name**: *myVnetAToMyVnetB*
     - **Virtueel netwerk implementatiemodel**: Selecteer **klassieke**.
     - **Ik weet mijn resource-ID**: Schakel dit selectievakje in.
     - **Resource-ID**: Geef de resource-ID van myVnetB uit stap 15.
     - **Toestaan van toegang tot het virtuele netwerk:** ervoor te zorgen dat **ingeschakeld** is geselecteerd.
    Er zijn geen andere instellingen worden gebruikt in deze zelfstudie. Lees voor meer informatie over alle instellingen van peering, [virtueel-netwerkpeerings beheren](virtual-network-manage-peering.md#create-a-peering).
23. Nadat u hebt geklikt **OK** in de vorige stap, de **Add peering** blade wordt gesloten en u ziet de **myVnetA - Peerings** blade opnieuw. Na enkele seconden weergegeven de peering die u hebt gemaakt in de blade. **Verbonden** wordt vermeld in de **PEERINGSTATUS** kolom voor de **myVnetAToMyVnetB** u peering gemaakt. De peering wordt nu ingesteld. Er is niet nodig om te koppelen van het virtuele netwerk (klassiek) met het virtuele netwerk (Resource Manager).

    Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

24. **Optionele**: hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
25. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in de [resources verwijderen](#delete-portal) sectie van dit artikel.

## <a name="cli"></a>Maken van de peering - Azure CLI

In deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met voor beide abonnementen toegangsmachtigingen gebruikt, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor de logboekregistratie van Azure kunt benutten en de regels van het script waarmee gebruiker roltoewijzingen verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u om UserA en UserB gebruikt. De volgende stappen met behulp van de klassieke Azure CLI en Azure CLI. U kunt de stappen van de Azure Cloud Shell, door alleen de **uitproberen** knop in een van de volgende stappen uit of door het installeren van de [klassieke CLI](/cli/azure/install-cli-version-1.0.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en [CLI](/cli/azure/install-azure-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) en de opdrachten uitvoert op uw lokale computer.

1. Als de Cloud Shell gebruikt, gaat u naar stap 2, omdat de Cloud Shell automatisch aangemeld bij Azure. Open een opdrachtsessie en meld u aan voor het gebruik van Azure de `azure login` opdracht.
2. De klassieke CLI uitvoert in Service Management-modus door op te geven de `azure config mode asm` opdracht.
3. Voer de volgende klassieke CLI-opdracht voor het maken van het virtuele netwerk (klassiek):

    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
4. De resterende stappen moeten worden uitgevoerd met behulp van een bash-shell met de Azure CLI (niet in de klassieke CLI).
5. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met uw abonnements-ID. Als u uw abonnements-Id niet weet, voert u de `az account show` opdracht. De waarde voor **id** is in de uitvoer van uw abonnements-id. Het gewijzigde script kopiëren, plak deze in uw CLI-sessie en druk vervolgens op `Enter`.

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Wanneer u het virtuele netwerk (klassiek) in stap 4 hebt gemaakt, Azure gemaakt met het virtuele netwerk in de *standaard-netwerk* resourcegroep.
6. Meld u aan gebruiker b buiten Azure en zich aanmelden als gebruiker a in de CLI.
7. Maak een resourcegroep en een virtueel netwerk (Resource Manager). Het volgende script kopiëren, plak deze in uw CLI-sessie en druk vervolgens op `Enter`.

    ```azurecli-interactive
    #!/bin/bash

    # Variables for common values used throughout the script.
    rgName="myResourceGroupA"
    location="eastus"

    # Create a resource group.
    az group create \
      --name $rgName \
      --location $location

    # Create virtual network A (Resource Manager).
    az network vnet create \
      --name myVnetA \
      --resource-group $rgName \
      --location $location \
      --address-prefix 10.0.0.0/16

    # Get the id for myVnetA.
    vNetAId=$(az network vnet show \
      --resource-group $rgName \
      --name myVnetA \
      --query id --out tsv)

    # Assign UserB permissions to myVnetA.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope $vNetAId
    ```

8. Maak een virtueel netwerk voor peering tussen de twee virtuele netwerken die zijn gemaakt via de verschillende implementatiemodellen. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-id>` met uw abonnements-id. Als u uw abonnements-Id niet weet, voert u de `az account show` opdracht. De waarde voor **id** is in de uitvoer van uw abonnements-id. Azure hebt gemaakt voor het virtuele netwerk (klassiek) die u hebt gemaakt in stap 4 in een resourcegroep met de naam *standaard-netwerk*. Plak het gewijzigde script in de CLI-sessie en druk vervolgens op `Enter`.

    ```azurecli-interactive
    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

9. Nadat het script wordt uitgevoerd, Controleer u de peering voor het virtuele netwerk (Resource Manager). Kopieer het volgende script en plak deze in de CLI-sessie:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    De uitvoer bevat **verbonden** in de **PeeringState** kolom.

    Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

10. **Optionele**: hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
11. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-cli) in dit artikel.

## <a name="powershell"></a>Maken van de peering - PowerShell

In deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met voor beide abonnementen toegangsmachtigingen gebruikt, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor de logboekregistratie van Azure kunt benutten en de regels van het script waarmee gebruiker roltoewijzingen verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u om UserA en UserB gebruikt. 

1. Installeer de nieuwste versie van de PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) en [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modules. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Start een PowerShell-sessie.
3. In PowerShell, meld u aan bij UserB van abonnement als de gebruiker b door in te voeren de `Add-AzureAccount` opdracht. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
4. Voor het maken van een virtueel netwerk (klassiek) met PowerShell, moet u een nieuwe maken of wijzigen van een bestaand, netwerk-configuratiebestand. Meer informatie over het [exporteren en importeren van netwerk-configuratiebestanden bijwerken](virtual-networks-using-network-configuration-file.md). Het bestand moet zijn onder andere de volgende **VirtualNetworkSite** -element voor het virtuele netwerk in deze zelfstudie gebruikt:

    ```xml
    <VirtualNetworkSite name="myVnetB" Location="East US">
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

5. Meld u aan bij de gebruiker b van abonnement als UserB met Resource Manager-opdrachten door te voeren de `Connect-AzureRmAccount` opdracht.
6. Gebruiker a machtigingen toewijzen aan virtueel netwerk b kopiëren in het volgende script naar een teksteditor op uw PC en vervang `<SubscriptionB-id>` met de ID van abonnement B. Als u de abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Azure hebt gemaakt voor het virtuele netwerk (klassiek) die u hebt gemaakt in stap 4 in een resourcegroep met de naam *standaard-netwerk*. Voor het uitvoeren van het script het gewijzigde script kopiëren, plak deze in op PowerShell en druk vervolgens op `Enter`.
    
    ```powershell 
    New-AzureRmRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Meld u af bij Azure als UserB en meld u aan bij de gebruiker van de a-abonnement als UserA door in te voeren de `Connect-AzureRmAccount` opdracht. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
8. Maken van het virtuele netwerk (Resource Manager) door het volgende script kopiëren, te plakken in PowerShell en drukt u op `Enter`:

    ```powershell
    # Variables for common values
      $rgName='MyResourceGroupA'
      $location='eastus'

    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name $rgName `
      -Location $location

    # Create virtual network A.
    $vnetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName $rgName `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location $location
    ```

9. Verleent machtigingen toewijzen aan myVnetA. Kopieer het volgende script naar een teksteditor op uw PC en vervang `<SubscriptionA-Id>` met de ID van abonnement A. Als u de abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Plak de gewijzigde versie van het script in PowerShell en druk vervolgens op `Enter` uitvoeren.

    ```powershell
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Kopieer het volgende script naar een teksteditor op uw PC en vervang `<SubscriptionB-id>` met de ID van abonnement B. MyVnetA naar myVNetB koppelen, het gewijzigde script kopiëren, plak deze in op PowerShell en druk vervolgens op `Enter`.

    ```powershell
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. De status van de peering van myVnetA weergeven door het volgende script kopiëren, plakken in PowerShell en drukken `Enter`.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    De status is **verbonden**. Deze wijzigingen in **verbonden** nadat het instellen van de peering naar myVnetA van myVnetB.

    Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

12. **Optionele**: hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
13. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-powershell) in dit artikel.

## <a name="delete"></a>Resources verwijderen
Wanneer u deze zelfstudie hebt voltooid, is het raadzaam om de resources die u in de zelfstudie hebt gemaakt, zodat er geen kosten voor het gebruik te verwijderen. Als u een resourcegroep verwijdert, verwijdert u ook alle resources die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Voer in de portal zoeken **myResourceGroupA**. Klik in de lijst met zoekresultaten op **myResourceGroupA**.
2. Op de **myResourceGroupA** blade, klikt u op de **verwijderen** pictogram.
3. De verwijdering te bevestigen, in de **TYPE de naam van de RESOURCEGROEP** Voer **myResourceGroupA**, en klik vervolgens op **verwijderen**.
4. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myVnetB*. Klik op **myVnetB** wanneer deze wordt weergegeven in de lijst met zoekresultaten. Een blade wordt weergegeven voor de **myVnetB** virtueel netwerk.
5. In de **myVnetB** blade, klikt u op **verwijderen**.
6. Als u wilt bevestigen, klikt u op **Ja** in de **virtueel netwerk verwijderen** vak.

### <a name="delete-cli"></a>Azure CLI

1. Meld u aan bij Azure met behulp van de CLI te verwijderen van het virtuele netwerk (Resource Manager) met de volgende opdracht uit:

   ```azurecli-interactive
   az group delete --name myResourceGroupA --yes
   ```

2. Aanmelden bij Azure met behulp van de klassieke CLI te verwijderen van het virtuele netwerk (klassiek) met de volgende opdrachten:

   ```azurecli-interactive
   azure config mode asm

   azure network vnet delete --vnet myVnetB --quiet
   ```

### <a name="delete-powershell"></a>PowerShell

1. Voer op de PowerShell-opdrachtprompt de volgende opdracht voor het verwijderen van het virtuele netwerk (Resource Manager):

   ```powershell
   Remove-AzureRmResourceGroup -Name myResourceGroupA -Force
   ```

2. Als u wilt verwijderen van het virtuele netwerk (klassiek) met PowerShell, wijzigt u een bestaand configuratiebestand van het netwerk. Meer informatie over het [exporteren en importeren van netwerk-configuratiebestanden bijwerken](virtual-networks-using-network-configuration-file.md). Verwijder de volgende VirtualNetworkSite-element voor het virtuele netwerk in deze zelfstudie gebruikt:

   ```xml
   <VirtualNetworkSite name="myVnetB" Location="East US">
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