---
title: Maak een Azure virtual network peering - Resource Manager - verschillende abonnementen
titlesuffix: Azure Virtual Network
description: Informatie over het maken van een virtueel netwerk dat peering tussen virtuele netwerken die zijn gemaakt via Resource Manager en die aanwezig zijn in verschillende Azure-abonnementen.
services: virtual-network
documentationcenter: ''
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: jdial;anavin
ms.openlocfilehash: f06f0b5392ebb60cd852d3c2eb201478b31ae167
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54014961"
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Maak een virtueel-netwerkpeering - Resource Manager, verschillende abonnementen

In deze zelfstudie leert u een virtueel netwerk dat peering tussen virtuele netwerken die zijn gemaakt via Resource Manager maken. De virtuele netwerken zich in verschillende abonnementen. Peering van twee virtuele netwerken kunnen resources in verschillende virtuele netwerken met elkaar communiceren met de bandbreedte en de latentie, alsof de resources zich in hetzelfde virtuele netwerk. Meer informatie over [peering van virtuele netwerken](virtual-network-peering-overview.md).

De stappen voor het maken van een virtueel netwerk-peering zijn verschillend, afhankelijk van of de virtuele netwerken in dezelfde of verschillende zijn, abonnementen en welke [Azure-implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) de virtuele netwerken zijn gemaakt via. Informatie over het maken van een virtueel netwerk-peering in andere scenario's door het selecteren van het scenario in de volgende tabel:

|Azure-implementatiemodel  | Azure-abonnement  |
|--------- |---------|
|[Beide in Resource Manager](tutorial-connect-virtual-networks-portal.md) |Dezelfde|
|[Een Resource Manager, één klassiek](create-peering-different-deployment-models.md) |Dezelfde|
|[Een Resource Manager, één klassiek](create-peering-different-deployment-models-subscriptions.md) |Verschil|

Peering op virtueel netwerk kan niet worden gemaakt tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel. Als u verbinding maken met virtuele netwerken die zijn beide gemaakt via het klassieke implementatiemodel wilt, kunt u een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) om de virtuele netwerken te verbinden.

Virtuele netwerken in dezelfde regio van deze zelfstudie collega's. U kunt ook virtuele netwerken in verschillende koppelen [ondersteunde regio's](virtual-network-manage-peering.md#cross-region). Het wordt aanbevolen dat u bekend zijn met de [peering vereisten en beperkingen](virtual-network-manage-peering.md#requirements-and-constraints) voor peering van virtuele netwerken.

U kunt de [Azure-portal](#portal), de Azure [opdrachtregelinterface](#cli) (CLI), Azure [PowerShell](#powershell), of een [Azure Resource Manager-sjabloon](#template)peering op virtueel netwerk maken. Selecteer een van de vorige hulpprogramma koppelingen en Ga rechtstreeks naar de stappen voor het maken van een virtueel-netwerkpeering met behulp van het hulpprogramma naar keuze.

## <a name="portal"></a>Maken van de peering - Azure portal

Als de virtuele netwerken die u wilt koppelen in abonnementen die gekoppeld aan andere Azure Active Directory-tenants zijn, volg de stappen in de CLI en PowerShell-sectie van dit artikel. Portal biedt geen ondersteuning voor virtuele netwerken die behoren tot de abonnementen van verschillende Active Directory-Tenants.

De volgende stappen uit verschillende accounts gebruiken voor elk abonnement. Als u een account met voor beide abonnementen toegangsmachtigingen gebruikt, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor de logboekregistratie van buiten de portal en slaat u de stappen voor het toewijzen van machtigingen voor een andere gebruiker aan de virtuele netwerken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als *UserA*. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
2. Selecteer **+ een resource maken**, selecteer **netwerken**, en selecteer vervolgens **virtueel netwerk**.
3. Selecteer of Voer de volgende voorbeelden van waarden voor de volgende instellingen en selecteer vervolgens **maken**:
    - **Name**: *myVnetA*
    - **Adresruimte**: *10.0.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnetadresbereik**: *10.0.0.0/24*
    - **Abonnement**: Selecteer A.-abonnement
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupA*
    - **Locatie**: *VS-Oost*
4. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myVnetA*. Selecteer **myVnetA** wanneer deze wordt weergegeven in de lijst met zoekresultaten. 
5. Selecteer **toegangsbeheer (IAM)** uit de verticale lijst met opties aan de linkerkant.
6. Onder **myVnetA - toegangsbeheer (IAM)**, selecteer **+ roltoewijzing toevoegen**.
7. Selecteer **Inzender voor netwerken** in de **rol** vak.
8. In de **Selecteer** Selecteer *UserB*, of typ de e-mailadres van gebruiker b om te zoeken naar deze.
9. Selecteer **Opslaan**.
10. Onder **myVnetA - toegangsbeheer (IAM)**, selecteer **eigenschappen** uit de verticale lijst met opties aan de linkerkant. Kopieer de **RESOURCE-ID**, die wordt gebruikt in een latere stap. De resource-ID is vergelijkbaar met het volgende voorbeeld: /subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA.
11. Meld u af bij de portal aan als UserA en meld u aan als UserB.
12. Volg de stappen 2-3, invoeren of selecteren van de volgende waarden in stap 3:

    - **Name**: *myVnetB*
    - **Adresruimte**: *10.1.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnetadresbereik**: *10.1.0.0/24*
    - **Abonnement**: Abonnement selecteren B.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupB*
    - **Locatie**: *VS-Oost*

13. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myVnetB*. Selecteer **myVnetB** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
14. Onder **myVnetB**, selecteer **eigenschappen** uit de verticale lijst met opties aan de linkerkant. Kopieer de **RESOURCE-ID**, die wordt gebruikt in een latere stap. De resource-ID is vergelijkbaar met het volgende voorbeeld: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB.
15. Selecteer **toegangsbeheer (IAM)** onder **myVnetB**, en voer vervolgens stap 5-10 voor myVnetB, voeren **UserA** in stap 8.
16. Meld u af bij de portal aan als UserB en aanmelden als gebruiker a.
17. In de **zoeken naar resources** vak aan de bovenkant van de portal, type *myVnetA*. Selecteer **myVnetA** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
18. Selecteer **myVnetA**.
19. Onder **instellingen**, selecteer **Peerings**.
20. Onder **myVnetA - Peerings**, selecteer **+ toevoegen**
21. Onder **Add peering**, invoeren, of selecteert, de volgende opties, en selecteer vervolgens **OK**:
     - **Name**: *myVnetAToMyVnetB*
     - **Virtueel netwerk implementatiemodel**:  Selecteer **Resource Manager**.
     - **Ik weet mijn resource-ID**: schakel dit selectievakje in.
     - **Resource-ID**: Geef de resource-ID uit stap 14.
     - **Toegang tot het virtuele netwerk toestaan:** Zorg ervoor dat **ingeschakeld** is geselecteerd.
    Er zijn geen andere instellingen worden gebruikt in deze zelfstudie. Lees voor meer informatie over alle instellingen van peering, [virtueel-netwerkpeerings beheren](virtual-network-manage-peering.md#create-a-peering).
22. Enkele ogenblikken na het selecteren van de peering die u hebt gemaakt weergegeven **OK** in de vorige stap. **Gestart** wordt vermeld in de **PEERINGSTATUS** kolom voor de **myVnetAToMyVnetB** u peering gemaakt. U myVnetA naar myVnetB hebt gekoppeld, maar u moet nu myVnetB naar myVnetA koppelen. De peering moet worden gemaakt in beide richtingen om in te schakelen van resources in de virtuele netwerken met elkaar communiceren.
23. Meld u af bij de portal aan als UserA en aanmelden als gebruiker b.
24. Herhaal stap 17-21 opnieuw voor myVnetB. In stap 21, naam op voor de peering *myVnetBToMyVnetA*, selecteer *myVnetA* voor **virtueel netwerk**, en voer de ID van de stap 10 in de **Resource-ID**vak.
25. Een paar seconden na het selecteren van **OK** de peering voor myVnetB, maken de **myVnetBToMyVnetA** peering dat u zojuist hebt gemaakt wordt weergegeven met de **verbonden** in de **PEERINGSTATUS** kolom.
26. Meld u af bij de portal aan als UserB en aanmelden als gebruiker a.
27. Volg de stappen 17-19 nogmaals. De **PEERINGSTATUS** voor de **myVnetAToVNetB** peering is nu er ook **verbonden**. De peering wordt tot stand gebracht nadat er **verbonden** in de **PEERINGSTATUS** kolom voor beide virtuele netwerken in de peering. Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
28. **Optionele**: Hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
29. **Optionele**: Als u wilt verwijderen van de resources die u in deze zelfstudie maakt, u de stappen in de [resources verwijderen](#delete-portal) sectie van dit artikel.

## <a name="cli"></a>Maken van de peering - Azure CLI

In deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met voor beide abonnementen toegangsmachtigingen gebruikt, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor de logboekregistratie van Azure kunt benutten en de regels van het script waarmee gebruiker roltoewijzingen verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u om UserA en UserB gebruikt. Als de virtuele netwerken zich in verschillende abonnementen en de abonnementen gekoppeld aan andere Azure Active Directory-tenants zijn, moet u de volgende stappen uitvoeren voordat u doorgaat:
 - De gebruiker toevoegen vanuit elke Active Directory-tenant als een [gastgebruiker](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) in de tegengestelde Azure Active Directory-tenant.
 - Elke gebruiker heeft de uitnodiging Gast gebruiker van de tegengestelde Azure Active Directory-tenant te accepteren.

De volgende scripts:

- Gebruikmaken van Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om de versie te bekijken. Als u upgraden wilt, raadpleegt u [Azure CLI installeren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Werkt in een Bash-shell. Zie [De Azure CLI installeren in Windows](/cli/azure/install-azure-cli-windows) voor opties voor het uitvoeren van Azure CLI-scripts in de Windows-client.

In plaats van de CLI en de bijbehorende afhankelijkheden installeren, kunt u de Azure Cloud Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Selecteer de **uitproberen** knop in het script dat volgt, die een Cloud-Shell die u bij uw Azure-account met aanmelden kunt aanroept. 

1. Open een CLI-sessie en meld u aan bij Azure als het gebruik van gebruiker a de `azure login` opdracht. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
2. Kopieer het volgende script naar een teksteditor op uw PC, vervangt `<SubscriptionA-Id>` met de ID van SubscriptionA, moet u vervolgens het gewijzigde script kopiëren, plak deze in uw CLI-sessie en druk op `Enter`. Als u uw abonnements-Id niet weet, voert u de opdracht 'az account show'. De waarde voor **id** is in de uitvoer van uw abonnements-id.

    ```azurecli-interactive
    # Create a resource group.
    az group create \
      --name myResourceGroupA \
      --location eastus

    # Create virtual network A.
    az network vnet create \
      --name myVnetA \
      --resource-group myResourceGroupA \
      --location eastus \
      --address-prefix 10.0.0.0/16

    # Assign UserB permissions to virtual network A.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```
    
3. Meld u af bij Azure als het gebruik van gebruiker a de `az logout` opdracht en meld u aan bij Azure als gebruiker b. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
4. MyVnetB maken. Kopieer de inhoud van het script in stap 2 naar een teksteditor op uw PC. Vervang `<SubscriptionA-Id>` met de ID van SubscriptionB. Wijziging 10.0.0.0/16 10.1.0.0/16, wijziging alle over B en alle Bs naar A. kopie het gewijzigde script, plak deze in uw CLI-sessie en druk op `Enter`. 
5. Meld u af bij Azure als UserB en meld u aan bij Azure als gebruiker a.
6. Een virtueel netwerk-peering van myVnetA tot myVnetB maken. Kopieer de inhoud van het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID van SubscriptionB. Voor het uitvoeren van het script het gewijzigde script kopiëren, plak deze in de CLI-sessie en druk op Enter.

    ```azurecli-interactive
        # Get the id for myVnetA.
        vnetAId=$(az network vnet show \
          --resource-group myResourceGroupA \
          --name myVnetA \
          --query id --out tsv)

        # Peer myVNetA to myVNetB.
        az network vnet peering create \
          --name myVnetAToMyVnetB \
          --resource-group myResourceGroupA \
          --vnet-name myVnetA \
          --remote-vnet-id /subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/VirtualNetworks/myVnetB \
          --allow-vnet-access
    ```

7. De status van de peering van myVnetA weergeven.

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroupA \
      --vnet-name myVnetA \
      --output table
    ```

    De status is **gestart**. Deze wijzigingen in **verbonden** zodra u de peering naar myVnetA van myVnetB hebt gemaakt.

8. Meld u af GebruikerA van Azure en meld u aan bij Azure als gebruiker b.
9. Maak de peering vanuit myVnetB naar myVnetA. Inhoud van het script in stap 6 kopiëren naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID voor SubscriptionA en wijzig alle over B en alle Bs naar A. Nadat u de wijzigingen hebt aangebracht, het gewijzigde script kopiëren, plak deze in uw CLI-sessie en druk op `Enter`.
10. De status van de peering van myVnetB weergeven. Inhoud van het script in stap 7 kopiëren naar een teksteditor op uw PC. Wijziging A naar B voor de resourcegroep en de namen van virtueel netwerk, het script kopiëren, plak het gewijzigde script in uw CLI-sessie en druk vervolgens op `Enter`. Status van de peering is **verbonden**. De status van de peering van myVnetA gewijzigd in **verbonden** nadat u de peering vanuit myVnetB naar myVnetA hebt gemaakt. U kunt zich aanmelden gebruiker a terug in Azure en volledig stap 7 opnieuw uit om te controleren of de status van de peering van myVnetA. 

    > [!NOTE]
    > De peering is niet tot stand gebracht totdat de status van de peering is **verbonden** voor beide virtuele netwerken.

11. **Optionele**: Hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
12. **Optionele**: Als u wilt verwijderen van de resources die u in deze zelfstudie maakt, u de stappen in [resources verwijderen](#delete-cli) in dit artikel.

Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
 
## <a name="powershell"></a>Maken van de peering - PowerShell

In deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met voor beide abonnementen toegangsmachtigingen gebruikt, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor de logboekregistratie van Azure kunt benutten en de regels van het script waarmee gebruiker roltoewijzingen verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u om UserA en UserB gebruikt. Als de virtuele netwerken zich in verschillende abonnementen en de abonnementen gekoppeld aan andere Azure Active Directory-tenants zijn, moet u de volgende stappen uitvoeren voordat u doorgaat:
 - De gebruiker toevoegen vanuit elke Active Directory-tenant als een [gastgebruiker](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) in de tegengestelde Azure Active Directory-tenant.
 - Elke gebruiker heeft de uitnodiging Gast gebruiker van de tegengestelde Active Directory-tenant te accepteren.

1. Controleer of u versie 6.5.0 hebben of hoger. U kunt dit doen door het uitvoeren van de `Get-Module -Name AzureRm` het beste installeren de nieuwste versie van de PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell. 
2. Start een PowerShell-sessie.
3. In PowerShell, meld u aan bij Azure als UserA door in te voeren de `Connect-AzureRmAccount` opdracht. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
4. Maak een resourcegroep en een virtueel netwerk a kopiëren in het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionA-Id>` met de ID van SubscriptionA. Als u uw abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Voor het uitvoeren van het script het gewijzigde script kopiëren, plak deze in op PowerShell en druk vervolgens op `Enter`.

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name MyResourceGroupA `
      -Location eastus

    # Create virtual network A.
    $vNetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName MyResourceGroupA `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus

    # Assign UserB permissions to myVnetA.
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

5. Meld u af GebruikerA van Azure en meld u in gebruiker b. Het account dat u zich aan met moet de vereiste machtigingen om het maken van een virtueel netwerk-peering hebben. Zie voor een lijst van machtigingen, [machtigingen voor peering](virtual-network-manage-peering.md#permissions).
6. Kopieer de inhoud van het script in stap 4 in een teksteditor op uw PC. Vervang `<SubscriptionA-Id>` met de ID voor het abonnement 10.0.0.0/16 B. wijzigen naar 10.1.0.0/16. Wijzig alle over B en alle Bs naar A. Voor het uitvoeren van het script het gewijzigde script kopiëren, plakken in PowerShell en druk vervolgens op `Enter`.
7. Meld u af UserB van Azure en meld u in gebruiker a.
8. Maak de peering vanuit myVnetA naar myVnetB. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID van abonnement B. Voor het uitvoeren van het script het gewijzigde script kopiëren, plakken PowerShell en druk vervolgens op `Enter`.
 
    ```powershell
    # Peer myVnetA to myVnetB.
    $vNetA=Get-AzureRmVirtualNetwork -Name myVnetA -ResourceGroupName myResourceGroupA
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vNetA `
      -RemoteVirtualNetworkId "/subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/virtualNetworks/myVnetB"
    ```

9. De status van de peering van myVnetA weergeven.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroupA `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    De status is **gestart**. Deze wijzigingen in **verbonden** nadat het instellen van de peering naar myVnetA van myVnetB.

10. Meld u af GebruikerA van Azure en meld u in gebruiker b.
11. Maak de peering vanuit myVnetB naar myVnetA. Kopieer de inhoud van het script in stap 8 naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID van abonnement A en wijzig alle over B en alle Bs naar A. Voor het uitvoeren van het script het gewijzigde script kopiëren, plak deze in op PowerShell en druk vervolgens op `Enter`.
12. De status van de peering van myVnetB weergeven. Inhoud van het script in stap 9 kopiëren naar een teksteditor op uw PC. Wijziging A naar B voor de resourcegroep en de namen van virtueel netwerk. Voor het uitvoeren van het script, plak het gewijzigde script in PowerShell en druk vervolgens op `Enter`. De status is **verbonden**. De status van de peering van **myVnetA** wordt gewijzigd in **verbonden** nadat u hebt gemaakt met de peering vanuit **myVnetB** naar **myVnetA**. U kunt zich aanmelden gebruiker a terug in op Azure en volledig stap 9 opnieuw uit om te controleren of de status van de peering van myVnetA. 

    > [!NOTE]
    > De peering is niet tot stand gebracht totdat de status van de peering is **verbonden** voor beide virtuele netwerken.

    Alle Azure-resources die u in beide virtuele netwerken maakt zijn nu kan communiceren met elkaar via hun IP-adressen. Als u standaard Azure-naamomzetting voor virtuele netwerken, zijn niet de resources in de virtuele netwerken kunnen omzetten van namen voor de virtuele netwerken. Als u omzetten van namen tussen virtuele netwerken wilt in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

13. **Optionele**: Hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
14. **Optionele**: Als u wilt verwijderen van de resources die u in deze zelfstudie maakt, u de stappen in [resources verwijderen](#delete-powershell) in dit artikel.

## <a name="template"></a>Maken van de peering - Resource Manager-sjabloon

Als de virtuele netwerken zich in verschillende abonnementen en de abonnementen gekoppeld aan andere Azure Active Directory-tenants zijn, moet u de volgende stappen uitvoeren voordat u doorgaat:
 - De gebruiker toevoegen vanuit elke Active Directory-tenant als een [gastgebruiker](../active-directory/b2b/add-users-administrator.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-guest-users-to-the-directory) in de tegengestelde Azure Active Directory-tenant.
 - Elke gebruiker heeft de uitnodiging Gast gebruiker van de tegengestelde Active Directory-tenant te accepteren.
 
1. Een virtueel netwerk maken en toewijzen aan de juiste [machtigingen](virtual-network-manage-peering.md#permissions), de stappen in de [Portal](#portal), [Azure CLI](#cli), of [PowerShell](#powershell)secties van dit artikel.
2. Sla de tekst die naar een bestand op uw lokale computer volgt. Vervang `<subscription ID>` door gebruiker a de abonnement-ID. U kunt bijvoorbeeld het bestand opslaan als vnetpeeringA.json.

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
    "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "myVnetA/myVnetAToMyVnetB",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<subscription ID>/resourceGroups/PeeringTest/providers/Microsoft.Network/virtualNetworks/myVnetB"
                }
            }
            }
        ]
    }
    ```

3. Meld u aan bij Azure als UserA en implementeer de sjabloon met behulp van de [portal](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template), [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-a-template-from-your-local-machine), of de [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-local-template). Geef de naam van het bestand dat u de voorbeeld-json-tekst in stap 2 opgeslagen.
4. De voorbeeld-json uit stap 2 naar een bestand op uw computer kopiëren en wijzigingen aanbrengen aan de regels die met beginnen:
    - **Naam**: Wijziging *myVnetA/myVnetAToMyVnetB* naar *myVnetB/myVnetBToMyVnetA*.
    - **Id**: Vervang `<subscription ID>` met de abonnements-ID en het wijzigen van de gebruiker b *myVnetB* naar *myVnetA*.
5. Stap 3 van de volledige opnieuw aangemeld bij Azure als gebruiker b.
6. **Optionele**: Hoewel het maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken van een virtuele machine met de andere, te valideren.
7. **Optionele**: Als u wilt verwijderen van de resources die u in deze zelfstudie maakt, u de stappen in de [resources verwijderen](#delete) sectie van dit artikel, met behulp van de Azure-portal, PowerShell of Azure CLI.

## <a name="delete"></a>Resources verwijderen
Wanneer u deze zelfstudie hebt voltooid, is het raadzaam om de resources die u in de zelfstudie hebt gemaakt, zodat er geen kosten voor het gebruik te verwijderen. Als u een resourcegroep verwijdert, verwijdert u ook alle resources die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Meld u aan bij de Azure-portal aan als UserA.
2. Voer in de portal zoeken **myResourceGroupA**. Selecteer in de lijst met zoekresultaten **myResourceGroupA**.
3. Selecteer **Verwijderen**.
4. De verwijdering te bevestigen, in de **TYPE de naam van de RESOURCEGROEP** Voer **myResourceGroupA**, en selecteer vervolgens **verwijderen**.
5. Meld u af bij de portal aan als UserA en aanmelden als gebruiker b.
6. Volg de stappen 2-4 voor myResourceGroupB.

### <a name="delete-cli"></a>Azure CLI

1. Meld u aan bij Azure als gebruiker a en voer de volgende opdracht uit:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```
2. Meld u af bij Azure als UserA en aanmelden als gebruiker b.
3. Voer de volgende opdracht uit:

    ```azurecli-interactive
    az group delete --name myResourceGroupB --yes
    ```

### <a name="delete-powershell"></a>PowerShell

1. Meld u aan bij Azure als gebruiker a en voer de volgende opdracht uit:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -force
    ```

2. Meld u af bij Azure als UserA en aanmelden als gebruiker b.
3. Voer de volgende opdracht uit:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupB -force
    ```

## <a name="next-steps"></a>Volgende stappen

- Grondig raken met belangrijke [peering beperkingen van virtueel netwerk en het gedrag](virtual-network-manage-peering.md#requirements-and-constraints) gebruiken voor het maken van een virtueel netwerk-peering voor productie.
- Meer informatie over alle [instellingen van peering in virtuele netwerken](virtual-network-manage-peering.md#create-a-peering).
- Meer informatie over het [maken van een hub- en -spokenetwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met virtueel-netwerkpeering.
