---
title: Een virtueel netwerk van Azure-peering maken - implementatie van de verschillende modellen - verschillende abonnementen | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk peering tussen virtuele netwerken die zijn gemaakt via andere Azure-implementatiemodellen die aanwezig zijn in verschillende Azure-abonnementen.
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
ms.date: 09/15/2017
ms.author: jdial;anavin
ms.openlocfilehash: 441bb0a269de400c82abc083118f5e0642523640
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-a-virtual-network-peering---different-deployment-models-and-subscriptions"></a>Maak een virtueel netwerk peering - verschillend implementatiemodellen en -abonnementen

In deze zelfstudie leert u een virtueel netwerk peering tussen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen maken. De virtuele netwerken bestaan uit verschillende abonnementen. Peering twee virtuele netwerken kunnen resources in verschillende virtuele netwerken met elkaar communiceren met de bandbreedte en de latentie, alsof de resources in hetzelfde virtuele netwerk. Meer informatie over [virtuele netwerk peering](virtual-network-peering-overview.md).

De stappen voor het maken van een virtueel netwerk peering verschillen, afhankelijk van of de virtuele netwerken zijn in dezelfde of verschillende abonnementen en die zijn [Azure implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via de virtuele netwerken worden gemaakt. Meer informatie over het maken van een virtueel netwerk peering in andere scenario's door te klikken op het scenario uit de volgende tabel:

|Azure-implementatiemodel  | Azure-abonnement  |
|--------- |---------|
|[Beide Resource Manager](virtual-network-create-peering.md) |Dezelfde|
|[Beide Resource Manager](create-peering-different-subscriptions.md) |Andere|
|[Een Resource Manager, een klassiek](create-peering-different-deployment-models.md) |Dezelfde|

Een virtueel netwerk peering kan niet worden gemaakt tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel. Kunnen virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen die bestaan uit verschillende abonnementen op hetzelfde niveau is momenteel in preview. Voor deze zelfstudie hebt voltooid, moet u eerst [registreren](#register) de mogelijkheid te gebruiken. Deze zelfstudie maakt gebruik van virtuele netwerken die bestaan in dezelfde regio. Kunnen virtuele netwerken in verschillende regio's op hetzelfde niveau is ook in preview. Als u wilt gebruiken die mogelijkheid, moet u ook [registreren](#register) voor. De twee mogelijkheden zijn onafhankelijk. Voor deze zelfstudie hebt voltooid, moet u alleen registreren voor de mogelijkheid als peer virtuele netwerken die zijn gemaakt via verschillende implementatiemodellen die bestaan uit verschillende abonnementen. 

Bij het maken van een virtueel netwerk peering tussen virtuele netwerken die bestaan uit verschillende abonnementen moeten de abonnementen beide zijn gekoppeld aan dezelfde Azure Active Directory-tenant. Als u een Azure Active Directory-tenant nog geen hebt, kunt u snel [maken van een](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). 

De mogelijkheid voor verbinden van virtuele netwerken die zijn gemaakt via implementatiemodel, verschillende implementatiemodellen, verschillende regio's of abonnementen die zijn gekoppeld aan dezelfde of verschillende Azure Active Directory-tenants met behulp van een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bevindt zich in de preview-versie en vereist geen registratie.

U kunt de [Azure-portal](#portal), de Azure [opdrachtregelinterface](#cli) (CLI) of Azure [PowerShell](#powershell) peering van een virtueel netwerk maken. Klik op een van de vorige hulpprogramma koppelingen om rechtstreeks naar de stappen voor het maken van een virtueel netwerk peering met behulp van het hulpprogramma naar keuze te gaan.

## <a name="portal"></a>Maken van de peering - Azure-portal

Deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met machtigingen voor beide abonnementen, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor logboekregistratie buiten de portal en slaat u de stappen voor het toewijzen van een andere gebruikersmachtigingen aan de virtuele netwerken. Voordat u kunt een van de volgende stappen uitvoert, moet u registreren voor de preview. Als u wilt registreren, voer de stappen in de [registreren voor de preview](#register) sectie van dit artikel. De resterende stappen mislukken als u niet beide abonnementen voor de preview registreert.
 
1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als GebruikerA. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie de [machtigingen](#permissions) sectie van dit artikel voor meer informatie.
2. Klik op **+ nieuw**, klikt u op **Networking**, klikt u vervolgens op **virtueel netwerk**.
3. In de **virtueel netwerk maken** blade invoert, of Selecteer waarden voor de volgende instellingen en klik vervolgens op **maken**:
    - **Naam**: *myVnetA*
    - **Adresruimte**: *10.0.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnet-adresbereik**: *10.0.0.0/24*
    - **Abonnement**: Selecteer abonnement A.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupA*
    - **Locatie**: *VS-Oost*
4. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myVnetA*. Klik op **myVnetA** wanneer deze wordt weergegeven in de zoekresultaten. Een blade wordt weergegeven voor de **myVnetA** virtueel netwerk.
5. In de **myVnetA** blade die wordt weergegeven, klikt u op **toegangsbeheer (IAM)** uit de verticale lijst met opties aan de linkerkant van de blade.
6. In de **myVnetA - toegangsbeheer (IAM)** blade die wordt weergegeven, klikt u op **+ toevoegen**.
7. In de **machtigingen toevoegen** blade die wordt weergegeven, selecteer **Network contributor** in de **rol** vak.
8. In de **Selecteer** selecteert gebruiker b, of typ de e-mailadres van de gebruiker b om te zoeken naar deze. De lijst met gebruikers wordt weergegeven, is van dezelfde tenant als het virtuele netwerk die u de peering instelt voor Azure Active Directory. Klik op gebruiker b wanneer deze wordt weergegeven in de lijst.
9. Klik op **Opslaan**.
10. Meld u af bij de portal als GebruikerA en meld u aan als gebruiker b.
11. Klik op **+ nieuw**, type *virtueel netwerk* in de **zoeken van de Marketplace** vak en klik vervolgens op **virtueel netwerk** in de zoekresultaten.
12. In de **virtueel netwerk** blade die wordt weergegeven, selecteer **klassieke** in de **een implementatiemodel selecteren** vak en klik vervolgens op **maken**.
13.   Voer de volgende waarden in het virtuele netwerk (klassiek) vak maken die wordt weergegeven:

    - **Naam**: *myVnetB*
    - **Adresruimte**: *10.1.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnet-adresbereik**: *10.1.0.0/24*
    - **Abonnement**: Selecteer abonnement B.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupB*
    - **Locatie**: *VS-Oost*

14. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myVnetB*. Klik op **myVnetB** wanneer deze wordt weergegeven in de zoekresultaten. Een blade wordt weergegeven voor de **myVnetB** virtueel netwerk.
15. In de **myVnetB** blade die wordt weergegeven, klikt u op **eigenschappen** uit de verticale lijst met opties aan de linkerkant van de blade. Kopieer de **RESOURCE-ID**, die wordt gebruikt in een latere stap. De resource-ID is vergelijkbaar met het volgende voorbeeld: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
16. Herhaal stap 5-9 voor myVnetB, voeren **GebruikerA** in stap 8.
17. Meld u af bij de portal aan als gebruiker b en meld u aan als gebruiker a.
18. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myVnetA*. Klik op **myVnetA** wanneer deze wordt weergegeven in de zoekresultaten. Een blade wordt weergegeven voor de **myVnet** virtueel netwerk.
19. Klik op **myVnetA**.
20. In de **myVnetA** blade die wordt weergegeven, klikt u op **Peerings** uit de verticale lijst met opties aan de linkerkant van de blade.
21. In de **myVnetA - Peerings** blade die werden weergegeven, klikt u op **+ toevoegen**
22. In de **toevoegen peering** blade die wordt weergegeven, invoeren, of de volgende opties selecteren en klik vervolgens op **OK**:
     - **Naam**: *myVnetAToMyVnetB*
     - **Virtueel netwerk implementatiemodel**: Selecteer **klassieke**.
     - **Ik weet mijn resource-ID**: Schakel dit selectievakje in.
     - **De resource-ID**: Geef de bron-ID van myVnetB uit stap 15.
     - **Toestaan van toegang tot het virtuele netwerk:** ervoor te zorgen dat **ingeschakeld** is geselecteerd.
    Er zijn geen andere instellingen worden gebruikt in deze zelfstudie. Lees voor meer informatie over instellingen voor alle peering [beheren van virtueel netwerk peerings](virtual-network-manage-peering.md#create-a-peering).
23. Wanneer u op **OK** in de vorige stap, de **toevoegen peering** blade wordt gesloten en u ziet de **myVnetA - Peerings** blade opnieuw. Na een paar seconden weergegeven de peering die u hebt gemaakt in de blade. **Verbonden** wordt vermeld in de **PEERING STATUS** kolom voor de **myVnetAToMyVnetB** peering u gemaakt. De peer wordt nu ingesteld. Er is niet nodig als peer van het virtuele netwerk (klassiek) aan het virtuele netwerk (Resource Manager).

    Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

24. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
25. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in de [resources verwijderen](#delete-portal) sectie van dit artikel.

## <a name="cli"></a>Peering - maken Azure CLI

Deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met machtigingen voor beide abonnementen, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor logboekregistratie buiten Azure en de regels van het script die gebruiker roltoewijzingen maakt verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u voor GebruikerA en verleent. 

Voordat u kunt een van de volgende stappen uitvoert, moet u registreren voor de preview. Als u wilt registreren, voer de stappen in de [registreren voor de preview](#register) sectie van dit artikel. De resterende stappen mislukken als u niet beide abonnementen voor de preview registreert.

1. [Installeer](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de Azure CLI 1.0 voor het maken van het virtuele netwerk (klassiek).
2. Open een CLI-sessie en meld u aan bij Azure als het gebruik van de gebruiker b het `azure login` opdracht.
3. De CLI in Service Management-modus uitgevoerd door te voeren de `azure config mode asm` opdracht.
4. Voer de volgende opdracht voor het maken van het virtuele netwerk (klassiek):
 
    ```azurecli
    azure network vnet create --vnet myVnetB --address-space 10.1.0.0 --cidr 16 --location "East US"
    ```
5. De resterende stappen moeten worden uitgevoerd met behulp van een bash-shell met de Azure CLI 2.0.4 of hoger [geïnstalleerd](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json), of met behulp van de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Klik op de **Try it** knop in de scripts die vervolgens een Cloud-Shell waarmee u zich bij uw Azure-account aanmeldt te openen. Zie voor opties op die wordt uitgevoerd bash-scripts CLI op een Windows-client, [uitvoeren van de Azure CLI in Windows](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 
6. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met uw abonnement-ID. Als u uw abonnements-Id niet weet, voert u de `az account show` opdracht. De waarde voor **id** in de uitvoer is de abonnement-id. Het gewijzigde script kopiëren, plak deze in op uw CLI 2.0-sessie en druk vervolgens op `Enter`. 

    ```azurecli-interactive
    az role assignment create \
      --assignee UserA@azure.com \
      --role "Classic Network Contributor" \
      --scope /subscriptions/<SubscriptionB-Id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

    Wanneer u het virtuele netwerk (klassiek) in stap 4 hebt gemaakt, Azure gemaakt met het virtuele netwerk in de *standaard netwerken* resourcegroep.
7. Meld u aan gebruiker b buiten Azure en zich aanmelden als gebruiker a in de CLI 2.0.
8. Maak een resourcegroep en een virtueel netwerk (Resource Manager). Het volgende script kopiëren, plak deze in op uw CLI-sessie en druk vervolgens op `Enter`. 

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

9. Maak een virtueel netwerk peering tussen de twee virtuele netwerken die zijn gemaakt met behulp van de verschillende implementatiemodellen. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-id>` met uw abonnements-id. Als u uw abonnements-Id niet weet, voert u de `az account show` opdracht. De waarde voor **id** in de uitvoer is de abonnement-id. Azure gemaakt in het virtuele netwerk (klassiek) die u hebt gemaakt in stap 4 in een resourcegroep met de naam *standaard netwerken*. Plak het gewijzigde script in de CLI-sessie en druk vervolgens op `Enter`.

    ```azurecli-interactive
    # Peer VNet1 to VNet2.
    az network vnet peering create \
      --name myVnetAToMyVnetB \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --remote-vnet-id  /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB \
      --allow-vnet-access
    ```

10. Nadat het script wordt uitgevoerd, moet u controleren de peering voor het virtuele netwerk (Resource Manager). Kopieer het volgende script en plak deze in de CLI-sessie:

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group $rgName \
      --vnet-name myVnetA \
      --output table
    ```
    De uitvoer wordt weergegeven **verbonden** in de **PeeringState** kolom.

    Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

11. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
12. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-cli) in dit artikel.

## <a name="powershell"></a>Maken van de peering - PowerShell

Deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met machtigingen voor beide abonnementen, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor logboekregistratie buiten Azure en de regels van het script die gebruiker roltoewijzingen maakt verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u voor GebruikerA en verleent. 

Voordat u kunt een van de volgende stappen uitvoert, moet u registreren voor de preview. Als u wilt registreren, voer de stappen in de [registreren voor de preview](#register) sectie van dit artikel. De resterende stappen mislukken als u niet beide abonnementen voor de preview registreert.

1. Installeer de nieuwste versie van de PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) en [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modules. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Een PowerShell-sessie starten.
3. In PowerShell, meld u aan bij de gebruiker b-abonnement als gebruiker b door te voeren de `Add-AzureAccount` opdracht.
4. Als een virtueel netwerk (klassiek) maken met PowerShell, moet u een nieuwe maken of wijzigen van een bestaand, netwerk-configuratiebestand. Meer informatie over hoe [exporteren, bijwerken en importeren van configuratie van netwerkbestanden](virtual-networks-using-network-configuration-file.md). Het bestand moet bevatten de volgende **VirtualNetworkSite** element voor het virtuele netwerk in deze zelfstudie gebruikt:

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
    > Een configuratiebestand gewijzigde netwerk importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen de vorige virtueel netwerk toevoegen en u niet wijzigen of verwijderen van een bestaande virtuele netwerken uit uw abonnement. 

5. Meld u aan de gebruiker b-abonnement als gebruiker b gebruik van Resource Manager-opdrachten door te voeren bij de `login-azurermaccount` opdracht.
6. Gebruiker a machtigingen toewijzen aan virtueel netwerk B. kopiëren met het volgende script naar een teksteditor op uw PC en vervang `<SubscriptionB-id>` met de ID van het abonnement B. Als u de abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Azure gemaakt in het virtuele netwerk (klassiek) die u hebt gemaakt in stap 4 in een resourcegroep met de naam *standaard netwerken*. Kopieer het gewijzigde script voor het uitvoeren van het script, plak deze in op PowerShell en druk vervolgens op `Enter`.
    
    ```powershell 
    New-AzureRmRoleAssignment `
      -SignInName UserA@azure.com `
      -RoleDefinitionName "Classic Network Contributor" `
      -Scope /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

7. Meld u af bij Azure als gebruiker b en meld u aan bij de GebruikerA abonnement als GebruikerA door te voeren de `login-azurermaccount` opdracht. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie de [machtigingen](#permissions) sectie van dit artikel voor meer informatie.
8. Het virtuele netwerk (Resource Manager) maken met het volgende script kopiëren, plakken in naar PowerShell en drukt u vervolgens op `Enter`:

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

9. Machtigingen verleent aan myVnetA toewijzen. Kopieer het volgende script naar een teksteditor op uw PC en vervang `<SubscriptionA-Id>` met de ID van het abonnement A. Als u de abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Plak de gewijzigde versie van het script in PowerShell en druk vervolgens op `Enter` uitvoeren.

    ```powershell
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

10. Kopieer het volgende script naar een teksteditor op uw PC en vervang `<SubscriptionB-id>` met de ID van het abonnement B. Kopieer het gewijzigde script, plak deze in op PowerShell en druk vervolgens op to-peer-myVnetA naar myVNetB, `Enter`.

    ```powershell
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vnetA `
      -RemoteVirtualNetworkId /subscriptions/<SubscriptionB-id>/resourceGroups/Default-Networking/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB
    ```

11. De status van de peering van myVnetA weergeven door het volgende script kopiëren, plakken in PowerShell en drukt u op `Enter`.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName $rgName `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    De status is **verbonden**. Deze wijzigingen in **verbonden** als u een peering naar myVnetA van myVnetB ingesteld.

    Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

12. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
13. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-powershell) in dit artikel.

## <a name="permissions"></a>Machtigingen

De accounts die u gebruikt voor het maken van een virtueel netwerk peering moeten de benodigde rol of machtiging hebben. Bijvoorbeeld, als u twee virtuele netwerken met de naam myVnetA en myVnetB zijn peering, uw account moet worden toegewezen aan de volgende minimale rol of machtiging voor elke virtuele netwerk:
    
|Virtueel netwerk|Implementatiemodel|Rol|Machtigingen|
|---|---|---|---|
|myVnetA|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|N.v.t.|
|myVnetB|Resource Manager|[Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassiek|[Inzender voor klassieke netwerken](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Meer informatie over [ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (alleen voor Resource Manager).

## <a name="delete"></a>Resources verwijderen
Wanneer u deze zelfstudie hebt voltooid, kunt u mogelijk wilt verwijderen van de resources die u hebt gemaakt in de zelfstudie, zodat u geen gebruik kosten. Verwijderen van een resourcegroep, verwijdert tevens alle bronnen die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Voer in het zoekvak portal **myResourceGroupA**. Klik in de zoekresultaten op **myResourceGroupA**.
2. Op de **myResourceGroupA** blade, klikt u op de **verwijderen** pictogram.
3. Het verwijderen te bevestigen, in de **TYPE de naam van een RESOURCEGROEP** Voer **myResourceGroupA**, en klik vervolgens op **verwijderen**.
4. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myVnetB*. Klik op **myVnetB** wanneer deze wordt weergegeven in de zoekresultaten. Een blade wordt weergegeven voor de **myVnetB** virtueel netwerk.
5. In de **myVnetB** blade, klikt u op **verwijderen**.
6. Het verwijderen te bevestigen, klikt u op **Ja** in de **virtuele netwerk verwijderen** vak.

### <a name="delete-cli"></a>Azure CLI

1. Aanmelden bij Azure met behulp van de CLI 2.0 verwijderen van het virtuele netwerk (Resource Manager) met de volgende opdracht:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```

2. Aanmelden bij Azure met behulp van de Azure CLI 1.0 verwijderen van het virtuele netwerk (klassiek) met de volgende opdrachten:

    ```azurecli
    azure config mode asm 

    azure network vnet delete --vnet myVnetB --quiet
    ```

### <a name="delete-powershell"></a>PowerShell

1. Voer de volgende opdracht om het verwijderen van het virtuele netwerk (Resource Manager) bij de PowerShell-opdrachtprompt:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -Force
    ```

2. Als u wilt verwijderen van het virtuele netwerk (klassiek) met PowerShell, wijzigt u een bestaand configuratiebestand van het netwerk. Meer informatie over hoe [exporteren, bijwerken en importeren van configuratie van netwerkbestanden](virtual-networks-using-network-configuration-file.md). Verwijder de volgende VirtualNetworkSite-element voor het virtuele netwerk in deze zelfstudie gebruikt:

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
    > Een configuratiebestand gewijzigde netwerk importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen het vorige virtuele netwerk verwijderen en u niet wijzigen of andere bestaande virtuele netwerken uit uw abonnement verwijderen. 

## <a name="register"></a>Registreren voor de Preview-versie

Kunnen virtuele netwerken die zijn gemaakt via andere Azure-implementatiemodellen die bestaan uit verschillende abonnementen op hetzelfde niveau is momenteel in preview. Preview-functies mogelijk niet dezelfde mate van beschikbaarheid en betrouwbaarheid als functies in het algemeen release. Voor de meest recente meldingen van beschikbaarheid en status van de preview-functies, Controleer de [Azure Virtual Network updates](https://azure.microsoft.com/updates/?product=virtual-network) pagina. 

U moet eerst registreren voor de functie abonnementoverschrijdende, cross-deployment model voordat u deze kunt gebruiken. Voer de volgende stappen uit in het abonnement dat elke virtuele netwerk dat u wilt peer in, met Azure PowerShell of Azure CLI:

### <a name="powershell"></a>PowerShell

1. Installeer de meest recente versie van de PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/)-module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Start een PowerShell-sessie en meld u aan met behulp van Azure bij de `Login-AzureRmAccount` opdracht.
3. Registreer het abonnement dat elke virtuele netwerk dat u wilt peer in voor de preview hiertoe de volgende opdrachten:

    ```powershell
    Register-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    
    Register-AzureRmResourceProvider `
      -ProviderNamespace Microsoft.Network
    ```
4. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

    ```powershell    
    Get-AzureRmProviderFeature `
      -FeatureName AllowClassicCrossSubscriptionPeering `
      -ProviderNamespace Microsoft.Network
    ```

    De stappen in de Portal, Azure CLI, PowerShell of Resource Manager template-secties van dit artikel tot kan niet worden voltooid. de **RegistrationState** uitvoer wordt weergegeven nadat het invoeren van de vorige opdrachten is  **Geregistreerd** voor beide abonnementen.

> [!NOTE]
> Deze zelfstudie maakt gebruik van virtuele netwerken die bestaan in dezelfde regio. Kunnen virtuele netwerken in verschillende regio's op hetzelfde niveau is ook in preview. Als u wilt registreren voor de regio-overschrijdende of globale-peering, Voer stappen 1-4 opnieuw via `-FeatureName AllowGlobalVnetPeering` in plaats van `-FeatureName AllowClassicCrossSubscriptionPeering`. De twee mogelijkheden zijn onafhankelijk van elkaar. U hoeft niet te registreren voor beide, tenzij u beide wilt gebruiken. De mogelijkheid is beschikbaar in een beperkt aantal regio's (in eerste instantie US West-Centraal, Canada centraal en ons West 2).

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

> [!NOTE]
> Deze zelfstudie maakt gebruik van virtuele netwerken die bestaan in dezelfde regio. Kunnen virtuele netwerken in verschillende regio's op hetzelfde niveau is ook in preview. Als u wilt registreren voor de regio-overschrijdende of globale-peering, Voer stappen 1-5 opnieuw via `--name AllowGlobalVnetPeering` in plaats van `--name AllowClassicCrossSubscriptionPeering`. De twee mogelijkheden zijn onafhankelijk van elkaar. U hoeft niet te registreren voor beide, tenzij u beide wilt gebruiken. De mogelijkheid is beschikbaar in een beperkt aantal regio's (in eerste instantie US West-Centraal, Canada centraal en ons West 2).

## <a name="next-steps"></a>Volgende stappen

- Grondig raken met belangrijke [peering beperkingen virtueel netwerk en het gedrag](virtual-network-manage-peering.md#requirements-and-constraints) voordat het maken van een virtueel netwerk voor productie-peering gebruikt.
- Meer informatie over alle [peering virtuele netwerkinstellingen](virtual-network-manage-peering.md#create-a-peering).
- Meer informatie over hoe [maken van een hub en spaak netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met het virtuele netwerk peering.
