---
title: Een Azure virtueel netwerk maken met verschillende abonnementen - Resource Manager - peering | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk peering tussen virtuele netwerken via Resource Manager gemaakt en die aanwezig zijn in verschillende Azure-abonnementen.
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
ms.openlocfilehash: b67b13f30538d21f1a4db9675ee7c13d999f842a
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34726275"
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Maak een virtueel netwerk peering - Resource Manager worden verschillende abonnementen 

In deze zelfstudie leert u een virtueel netwerk tussen virtuele netwerken die zijn gemaakt via Resource Manager-peering maken. De virtuele netwerken bestaan uit verschillende abonnementen. Peering twee virtuele netwerken kunnen resources in verschillende virtuele netwerken met elkaar communiceren met de bandbreedte en de latentie, alsof de resources in hetzelfde virtuele netwerk. Meer informatie over [virtuele netwerk peering](virtual-network-peering-overview.md). 

De stappen voor het maken van een virtueel netwerk peering verschillen, afhankelijk van of de virtuele netwerken zijn in dezelfde of verschillende abonnementen en die zijn [Azure implementatiemodel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via de virtuele netwerken worden gemaakt. Meer informatie over het maken van een virtueel netwerk peering in andere scenario's door het selecteren van het scenario in de volgende tabel:

|Azure-implementatiemodel  | Azure-abonnement  |
|--------- |---------|
|[Beide Resource Manager](tutorial-connect-virtual-networks-portal.md) |Dezelfde|
|[Een Resource Manager, een klassiek](create-peering-different-deployment-models.md) |Dezelfde|
|[Een Resource Manager, een klassiek](create-peering-different-deployment-models-subscriptions.md) |Verschil|

Een virtueel netwerk peering kan niet worden gemaakt tussen twee virtuele netwerken die zijn geïmplementeerd via het klassieke implementatiemodel. Als u verbinding maken met virtuele netwerken die zijn beide gemaakt via het klassieke implementatiemodel wilt, kunt u een Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) verbinding van de virtuele netwerken.

Deze zelfstudie samenwerkt virtuele netwerken in dezelfde regio. U kunt ook virtuele netwerken in verschillende peer [ondersteunde regio's](virtual-network-manage-peering.md#cross-region). Het raadzaam dat u vertrouwd raken met de [peering vereisten en beperkingen](virtual-network-manage-peering.md#requirements-and-constraints) voordat peering virtuele netwerken.

U kunt de [Azure-portal](#portal), de Azure [opdrachtregelinterface](#cli) (CLI) Azure [PowerShell](#powershell), of een [Azure Resource Manager-sjabloon](#template)peering van een virtueel netwerk maken. Selecteer een van de vorige hulpprogramma koppelingen om rechtstreeks naar de stappen voor het maken van een virtueel netwerk peering met behulp van het hulpprogramma naar keuze te gaan.

## <a name="portal"></a>Maken van de peering - Azure-portal

Deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met machtigingen voor beide abonnementen, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor logboekregistratie buiten de portal en slaat u de stappen voor het toewijzen van een andere gebruikersmachtigingen aan de virtuele netwerken.

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als *GebruikerA*. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie voor een lijst met machtigingen [virtuele netwerk peering machtigingen](virtual-network-manage-peering.md#permissions).
2. Selecteer **+ maken van een resource**, selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Selecteer of Voer de volgende voorbeelden van waarden voor de volgende instellingen en selecteer vervolgens **maken**:
    - **Name**: *myVnetA*
    - **Adresruimte**: *10.0.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnet-adresbereik**: *10.0.0.0/24*
    - **Abonnement**: Selecteer abonnement A.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupA*
    - **Locatie**: *VS-Oost*
4. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myVnetA*. Selecteer **myVnetA** wanneer deze wordt weergegeven in de zoekresultaten. 
5. Selecteer **toegangsbeheer (IAM)** uit de verticale lijst met opties aan de linkerkant.
6. Onder **myVnetA - toegangsbeheer (IAM)**, selecteer **+ toevoegen**.
7. Selecteer **Network contributor** in de **rol** vak.
8. In de **Selecteer** Selecteer *gebruiker b*, of typ de e-mailadres van de gebruiker b om te zoeken naar deze. De lijst met gebruikers wordt weergegeven, is van dezelfde tenant als het virtuele netwerk die u de peering instelt voor Azure Active Directory. Als er geen gebruiker b, is het waarschijnlijk omdat gebruiker b in een andere Active Directory-tenant dan GebruikerA. Als u verbinding maken met virtuele netwerken in verschillende Active Directory-tenants wilt, kunt u deze met een [Azure VPN-Gateway](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), in plaats van een virtueel netwerk peering.
9. Selecteer **Opslaan**.
10. Onder **myVnetA - toegangsbeheer (IAM)**, selecteer **eigenschappen** uit de verticale lijst met opties aan de linkerkant. Kopieer de **RESOURCE-ID**, die wordt gebruikt in een latere stap. De resource-ID is vergelijkbaar met het volgende voorbeeld: /subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA.
11. Meld u af bij de portal als GebruikerA en meld u aan als gebruiker b.
12. Volg de stappen 2-3, invoeren of selecteren van de volgende waarden in stap 3:

    - **Name**: *myVnetB*
    - **Adresruimte**: *10.1.0.0/16*
    - **Subnetnaam**: *standaard*
    - **Subnet-adresbereik**: *10.1.0.0/24*
    - **Abonnement**: Selecteer abonnement B.
    - **Resourcegroep**: Selecteer **nieuw** en voer *myResourceGroupB*
    - **Locatie**: *VS-Oost*

13. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myVnetB*. Selecteer **myVnetB** wanneer deze wordt weergegeven in de zoekresultaten.
14. Onder **myVnetB**, selecteer **eigenschappen** uit de verticale lijst met opties aan de linkerkant. Kopieer de **RESOURCE-ID**, die wordt gebruikt in een latere stap. De resource-ID is vergelijkbaar met het volgende voorbeeld: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB.
15. Selecteer **toegangsbeheer (IAM)** onder **myVnetB**, en voltooi vervolgens de stappen 5 tot 10 voor myVnetB, voeren **GebruikerA** in stap 8.
16. Meld u af bij de portal aan als gebruiker b en meld u aan als gebruiker a.
17. In de **zoeken bronnen** vak aan de bovenkant van de portal type *myVnetA*. Selecteer **myVnetA** wanneer deze wordt weergegeven in de zoekresultaten.
18. Selecteer **myVnetA**.
19. Onder **instellingen**, selecteer **Peerings**.
20. Onder **myVnetA - Peerings**, selecteer **+ toevoegen**
21. Onder **toevoegen peering**, invoert, of de volgende opties, selecteren en selecteer vervolgens **OK**:
     - **Name**: *myVnetAToMyVnetB*
     - **Virtueel netwerk implementatiemodel**: Selecteer **Resource Manager**.
     - **Ik weet mijn resource-ID**: Schakel dit selectievakje in.
     - **De resource-ID**: Geef de bron-ID uit stap 14.
     - **Toestaan van toegang tot het virtuele netwerk:** ervoor te zorgen dat **ingeschakeld** is geselecteerd.
    Er zijn geen andere instellingen worden gebruikt in deze zelfstudie. Lees voor meer informatie over instellingen voor alle peering [beheren van virtueel netwerk peerings](virtual-network-manage-peering.md#create-a-peering).
22. Een korte wachten na het selecteren van de peering die u hebt gemaakt weergegeven **OK** in de vorige stap. **Geïnitieerd** wordt vermeld in de **PEERING STATUS** kolom voor de **myVnetAToMyVnetB** peering u gemaakt. U hebt ingesteld als peer myVnetA naar myVnetB, maar u moet nu myVnetB naar myVnetA peer. De peering moet worden gemaakt in beide richtingen waarmee bronnen in de virtuele netwerken met elkaar communiceren.
23. Meld u af bij de portal als GebruikerA en aanmelden als gebruiker b.
24. Herhaal stap 17-21 opnieuw voor myVnetB. In stap 21 naam op voor de peering *myVnetBToMyVnetA*, selecteer *myVnetA* voor **virtueel netwerk**, en voer de ID van stap 10 in de **Resource-ID**vak.
25. Een paar seconden na het selecteren van **OK** peering voor myVnetB, maken de **myVnetBToMyVnetA** peering u zojuist hebt gemaakt wordt vermeld met **verbonden** in de **PEERING STATUS** kolom.
26. Meld u af bij de portal aan als gebruiker b en meld u aan als gebruiker a.
27. Voltooi stap 17-19 opnieuw. De **PEERING STATUS** voor de **myVnetAToVNetB** peering is nu er ook **verbonden**. De peering is tot stand gebracht nadat er **verbonden** in de **PEERING STATUS** kolom voor beide virtuele netwerken in de peering. Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
28. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
29. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in de [resources verwijderen](#delete-portal) sectie van dit artikel.

## <a name="cli"></a>Peering - maken Azure CLI

Deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met machtigingen voor beide abonnementen, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor logboekregistratie buiten Azure en de regels van het script die gebruiker roltoewijzingen maakt verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u voor GebruikerA en verleent. Beide van de virtuele netwerken die u wilt peer moeten zich in abonnementen die is gekoppeld aan dezelfde Azure Active Directory-tenant.  Als u verbinding maken met virtuele netwerken in verschillende Active Directory-tenants wilt, kunt u deze met een [Azure VPN-Gateway](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), in plaats van een virtueel netwerk peering.

De volgende scripts:

- De Azure CLI versie 2.0.4 vereist of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Werkt in een Bash-shell. Zie voor opties op Azure CLI-scripts die worden uitgevoerd op Windows-client [Azure CLI installeren op Windows](/cli/azure/install-azure-cli-windows). 

In plaats van installatie van de CLI en de bijbehorende afhankelijkheden, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Selecteer de **Try it** knop in het script dat volgt, die een Cloud-Shell die u bij uw Azure-account met aanmelden kunt aanroept. 

1. Open een CLI-sessie en meld u aan bij Azure als het gebruik van gebruiker a de `azure login` opdracht. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie voor een lijst met machtigingen [virtuele netwerk peering machtigingen](virtual-network-manage-peering.md#permissions).
2. Het volgende script kopiëren naar een teksteditor op uw PC, vervangt u `<SubscriptionA-Id>` met de ID SubscriptionA, moet u vervolgens het gewijzigde script kopiëren, plak het in de CLI-sessie en druk op `Enter`. Als u uw abonnements-Id niet weet, voert u de opdracht 'az account weergeven'. De waarde voor **id** in de uitvoer is de abonnement-id.

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
    
3. Meld u af bij Azure als het gebruik van gebruiker a de `az logout` opdracht en klik vervolgens aanmelden bij Azure als gebruiker b. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie voor een lijst met machtigingen [virtuele netwerk peering machtigingen](virtual-network-manage-peering.md#permissions).
4. MyVnetB maken. Kopieer de inhoud van het script in stap 2 naar een teksteditor op uw PC. Vervang `<SubscriptionA-Id>` met de ID van SubscriptionB. Wijziging 10.0.0.0/16 10.1.0.0/16, Wijzig alle garantie B en alle Bs voor het kopiëren van A. het gewijzigde script plak deze in de CLI-sessie en druk op `Enter`. 
5. Meld u af bij Azure als gebruiker b en aanmelden bij Azure als GebruikerA.
6. Maak een virtueel netwerk peering van myVnetA naar myVnetB. Kopieer de inhoud van het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID van SubscriptionB. Kopieer het gewijzigde script voor het uitvoeren van het script, plak deze in de CLI-sessie en druk op Enter.
 
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

    De status is **gestarte**. Deze wijzigingen in **verbonden** zodra u de peering te myVnetA van myVnetB maakt.

8. Gebruiker a uit Azure afmelden en aanmelden bij Azure als gebruiker b.
9. De peering van myVnetB om myVnetA te maken. Inhoud van het script in stap 6 kopiëren naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID voor SubscriptionA en wijzig alle garantie B en alle Bs naar A. Zodra u de wijzigingen hebt aangebracht, het gewijzigde script kopiëren, plak het in uw CLI-sessie en druk op `Enter`.
10. De status van de peering van myVnetB weergeven. Inhoud van het script in stap 7 kopiëren naar een teksteditor op uw PC. Wijziging A naar B voor de resourcegroep en de namen van virtueel netwerk, Kopieer het script, plak het gewijzigde script in op uw CLI-sessie en druk vervolgens op `Enter`. De status van de peering is **verbonden**. De status van de peering van myVnetA wijzigingen in **verbonden** nadat u de peering van myVnetB naar myVnetA hebt gemaakt. U kunt zich aanmelden voor GebruikerA terug in naar Azure en volledig stap 7 opnieuw uit om te controleren of de status van de peering van myVnetA. 

    > [!NOTE]
    > De peering is niet tot stand gebracht totdat de status van de peering is **verbonden** voor beide virtuele netwerken.

11. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
12. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-cli) in dit artikel.

Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).
 
## <a name="powershell"></a>Maken van de peering - PowerShell

Deze zelfstudie maakt gebruik van verschillende accounts voor elk abonnement. Als u een account met machtigingen voor beide abonnementen, kunt u hetzelfde account gebruiken voor alle stappen, slaat u de stappen voor logboekregistratie buiten Azure en de regels van het script die gebruiker roltoewijzingen maakt verwijderen. Vervang UserA@azure.com en UserB@azure.com in alle van de volgende scripts met de gebruikersnamen die u voor GebruikerA en verleent. Beide van de virtuele netwerken die u wilt peer moeten zich in abonnementen die is gekoppeld aan dezelfde Azure Active Directory-tenant.  Als u verbinding maken met virtuele netwerken in verschillende Active Directory-tenants wilt, kunt u deze met een [Azure VPN-Gateway](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md), in plaats van een virtueel netwerk peering.

1. Installeer de meest recente versie van de PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/)-module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Een PowerShell-sessie starten.
3. In PowerShell, meld u aan bij Azure als GebruikerA door te voeren de `Connect-AzureRmAccount` opdracht. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie voor een lijst met machtigingen [virtuele netwerk peering machtigingen](virtual-network-manage-peering.md#permissions).
4. Maak een resourcegroep en virtueel netwerk A. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionA-Id>` met de ID van SubscriptionA. Als u uw abonnements-Id niet weet, voert u de `Get-AzureRmSubscription` opdracht weer te geven. De waarde voor **Id** in de resulterende uitvoer wordt uw abonnement-ID. Kopieer het gewijzigde script voor het uitvoeren van het script, plak deze in op PowerShell en druk vervolgens op `Enter`.

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

5. Gebruiker a uit Azure afmelden en aanmelden gebruiker b. Het account dat u zich met aanmeldt moet hebben de vereiste machtigingen om de peering van een virtueel netwerk maken. Zie voor een lijst met machtigingen [virtuele netwerk peering machtigingen](virtual-network-manage-peering.md#permissions).
6. Inhoud van het script in stap 4 kopiëren naar een teksteditor op uw PC. Vervang `<SubscriptionA-Id>` met de ID voor het abonnement B. wijziging 10.0.0.0/16 naar 10.1.0.0/16. Wijzig alle garantie B en alle Bs naar A. Voor het uitvoeren van het script het gewijzigde script kopiëren, plak in PowerShell en druk vervolgens op `Enter`.
7. Gebruiker b van Azure afmelden en aanmelden gebruiker a.
8. De peering van myVnetA om myVnetB te maken. Kopieer het volgende script naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID van het abonnement B. Voor het uitvoeren van het script het gewijzigde script kopiëren, plakken PowerShell en druk vervolgens op `Enter`.
 
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

    De status is **gestarte**. Deze wijzigingen in **verbonden** als u een peering naar myVnetA van myVnetB ingesteld.

10. Gebruiker a uit Azure afmelden en aanmelden gebruiker b.
11. De peering van myVnetB om myVnetA te maken. Kopieer de inhoud van het script in stap 8 naar een teksteditor op uw PC. Vervang `<SubscriptionB-Id>` met de ID van een abonnement en wijzig alle garantie B en alle Bs naar A. Kopieer het gewijzigde script voor het uitvoeren van het script, plak deze in op PowerShell en druk vervolgens op `Enter`.
12. De status van de peering van myVnetB weergeven. Inhoud van het script in stap 9 kopiëren naar een teksteditor op uw PC. Wijziging A naar B voor de resourcegroep en de namen van het virtuele netwerk. Voor het uitvoeren van het script, plak het gewijzigde script in PowerShell en druk vervolgens op `Enter`. De status is **verbonden**. De status van de peering van **myVnetA** wijzigingen in **verbonden** nadat u hebt gemaakt met de peering van **myVnetB** naar **myVnetA**. U kunt zich aanmelden gebruiker a weer naar Azure en volledig stap 9 opnieuw uit om te controleren of de status van de peering van myVnetA. 

    > [!NOTE]
    > De peering is niet tot stand gebracht totdat de status van de peering is **verbonden** voor beide virtuele netwerken.

    Alle Azure-resources die in een virtueel netwerk hebt u zich nu met elkaar communiceren via hun IP-adressen. Als u van standaard Azure-naamomzetting voor de virtuele netwerken gebruikmaakt, zich de resources in de virtuele netwerken niet voor de naamomzetting tussen de virtuele netwerken. Als u wilt voor het omzetten van namen over virtuele netwerken in een peering, moet u uw eigen DNS-server maken. Meer informatie over het instellen van [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server).

13. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
14. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in [resources verwijderen](#delete-powershell) in dit artikel.

## <a name="template"></a>Maken van de peering - Resource Manager-sjabloon

1. Een virtueel netwerk maken en toewijzen van de juiste [machtigingen](virtual-network-manage-peering.md#permissions), voer de stappen in de [Portal](#portal), [Azure CLI](#cli), of [PowerShell](#powershell)secties van dit artikel.
2. Sla de tekst die naar een bestand op uw lokale computer volgt. Vervang `<subscription ID>` met GebruikerA van abonnements-ID. U kunt bijvoorbeeld het bestand opslaan als vnetpeeringA.json.

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

3. Aanmelden bij Azure als GebruikerA en implementeert de sjabloon met de [portal](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template), [PowerShell](../azure-resource-manager/resource-group-template-deploy.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-a-template-from-your-local-machine), of de [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-local-template). Geef de naam van het bestand dat u de json-tekst van het voorbeeld in stap 2 hebt opgeslagen.
4. De voorbeeld-json naar een bestand op uw computer kopiëren uit stap 2 en wijzigingen aanbrengen in de regels die met beginnen:
    - **naam**: wijziging *myVnetA/myVnetAToMyVnetB* naar *myVnetB/myVnetBToMyVnetA*.
    - **id**: Vervang `<subscription ID>` met de abonnements-ID en het wijzigen van de gebruiker b *myVnetB* naar *myVnetA*.
5. Stap 3 van de volledige opnieuw aangemeld bij Azure als gebruiker b.
6. **Optionele**: hoewel maken van virtuele machines wordt niet behandeld in deze zelfstudie, kunt u een virtuele machine maken in elk virtueel netwerk en verbinding maken vanaf een virtuele machine op een andere, te valideren.
7. **Optionele**: voor het verwijderen van de resources die u in deze zelfstudie maakt, voert u de stappen in de [resources verwijderen](#delete) sectie van dit artikel, met behulp van de Azure-portal, PowerShell of Azure CLI.

## <a name="delete"></a>Resources verwijderen
Wanneer u deze zelfstudie hebt voltooid, kunt u mogelijk wilt verwijderen van de resources die u hebt gemaakt in de zelfstudie, zodat u geen gebruik kosten. Verwijderen van een resourcegroep, verwijdert tevens alle bronnen die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Meld u aan bij de Azure-portal als GebruikerA.
2. Voer in het zoekvak portal **myResourceGroupA**. Selecteer in de lijst met zoekresultaten **myResourceGroupA**.
3. Selecteer **Verwijderen**.
4. Het verwijderen te bevestigen, in de **TYPE de naam van een RESOURCEGROEP** Voer **myResourceGroupA**, en selecteer vervolgens **verwijderen**.
5. Meld u af bij de portal als GebruikerA en aanmelden als gebruiker b.
6. Herhaal stap 2-4 voor myResourceGroupB.

### <a name="delete-cli"></a>Azure CLI

1. Aanmelden bij Azure als GebruikerA en voer de volgende opdracht:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```
2. Meld u af bij Azure als GebruikerA en aanmelden als gebruiker b.
3. Voer de volgende opdracht:

    ```azurecli-interactive
    az group delete --name myResourceGroupB --yes
    ```

### <a name="delete-powershell"></a>PowerShell

1. Aanmelden bij Azure als GebruikerA en voer de volgende opdracht:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -force
    ```

2. Meld u af bij Azure als GebruikerA en aanmelden als gebruiker b.
3. Voer de volgende opdracht:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupB -force
    ```

## <a name="next-steps"></a>Volgende stappen

- Grondig raken met belangrijke [peering beperkingen virtueel netwerk en het gedrag](virtual-network-manage-peering.md#requirements-and-constraints) voordat het maken van een virtueel netwerk voor productie-peering gebruikt.
- Meer informatie over alle [peering virtuele netwerkinstellingen](virtual-network-manage-peering.md#create-a-peering).
- Meer informatie over hoe [maken van een hub en spaak netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met het virtuele netwerk peering.
