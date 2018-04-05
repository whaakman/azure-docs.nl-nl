---
title: Virtuele netwerken te verbinden met het virtuele netwerk peering - Azure-portal | Microsoft Docs
description: In dit artikel leert u hoe u virtuele netwerken te verbinden met het virtuele netwerk peering, met behulp van de Azure-portal.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: b864c71a62289b3abef13a98b52683f7d928b8e1
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Virtuele netwerken te verbinden met het virtuele netwerk peering met de Azure portal

U kunt virtuele netwerken met elkaar verbinden met het virtuele netwerk peering. Als u virtuele netwerken brengen, zijn resources in beide virtuele netwerken met elkaar communiceren met de dezelfde latentie en bandbreedte, alsof de bronnen zich in hetzelfde virtuele netwerk. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Twee virtuele netwerken maken
> * Twee virtuele netwerken te verbinden met een virtueel netwerk peering
> * Een virtuele machine (VM) in elk virtueel netwerk implementeren
> * Communicatie tussen virtuele machines

Als u liever, kunt u voltooien in dit artikel met behulp van de [Azure CLI](tutorial-connect-virtual-networks-cli.md) of [Azure PowerShell](tutorial-connect-virtual-networks-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-virtual-networks"></a>Virtuele netwerken maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Voer, of selecteert, worden de volgende informatie, accepteer de standaardwaarden voor de overige instellingen en selecteer **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVirtualNetwork1|
    |Adresruimte|10.0.0.0/16|
    |Abonnement| Selecteer uw abonnement.|
    |Resourcegroep| Selecteer **nieuw** en voer *myResourceGroup*.|
    |Locatie| Selecteer **VS-Oost**.|
    |Subnetnaam|Subnet1|
    |Subnet-adresbereik|10.0.0.0/24|

      ![Een virtueel netwerk maken](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Volg de stappen 1-3 opnieuw met de volgende wijzigingen:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVirtualNetwork2|
    |Adresruimte|10.1.0.0/16|
    |Resourcegroep| Selecteer **gebruik bestaande** en selecteer vervolgens **myResourceGroup**.|
    |Subnet-adresbereik|10.1.0.0/24|

## <a name="peer-virtual-networks"></a>Peer virtuele netwerken

1. Begint te typen in het zoekvak boven aan de Azure-portal *MyVirtualNetwork1*. Wanneer **myVirtualNetwork1** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Selecteer **Peerings**onder **instellingen**, en selecteer vervolgens **+ toevoegen**, zoals wordt weergegeven in de volgende afbeelding:

    ![-Peering maken](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Voer, of selecteert, worden de volgende informatie, accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**.

    |Instelling|Waarde|
    |---|---|
    |Naam|myVirtualNetwork1-myVirtualNetwork2|
    |Abonnement| Selecteer uw abonnement.|
    |Virtueel netwerk|myVirtualNetwork2 - selecteren de *myVirtualNetwork2* virtueel netwerk, en selecteer **virtueel netwerk**, selecteer daarna **myVirtualNetwork2**.|

    ![Instellingen voor peering](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    De **PEERING STATUS** is *gestarte*, zoals wordt weergegeven in de volgende afbeelding:

    ![De status van de peering](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Als u de status niet ziet, vernieuwt u de browser.

4. In de **Search** vak aan de bovenkant van de Azure-portal, begint te typen *MyVirtualNetwork2*. Wanneer **myVirtualNetwork2** wordt weergegeven in de zoekresultaten, selecteer deze.
5. Stap 2-3 opnieuw met de volgende wijzigingen en selecteer vervolgens **OK**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVirtualNetwork2-myVirtualNetwork1|
    |Virtueel netwerk|myVirtualNetwork1|

    De **PEERING STATUS** is *verbonden*. Azure gewijzigd ook de status van de peering van de *myVirtualNetwork2 myVirtualNetwork1* peering van *gestarte* naar *verbonden.* Virtueel netwerk peer wordt niet volledig ingesteld totdat de status van de peering voor beide virtuele netwerken *verbonden.* 

## <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtuele machine in elk virtueel netwerk maken, zodat u tussen deze twee een latere stap kan communiceren.

### <a name="create-the-first-vm"></a>De eerste virtuele machine maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. U kunt een ander besturingssysteem selecteren, maar de resterende stappen wordt ervan uitgegaan dat u hebt geselecteerd **Windows Server 2016 Datacenter**. 
3. Invoeren of selecteren, de volgende informatie voor **basisbeginselen**, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVm1|
    |Gebruikersnaam| Voer een gebruikersnaam van uw keuze.|
    |Wachtwoord| Voer een wachtwoord van uw keuze. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Resourcegroep| Selecteer **gebruik bestaande** en selecteer vervolgens **myResourceGroup**.|
    |Locatie| Selecteer **VS-Oost**.|
4. Selecteer een VM-grootte onder **een grootte kiezen**.
5. Selecteer de volgende waarden voor **instellingen**, selecteer daarna **OK**:
    |Instelling|Waarde|
    |---|---|
    |Virtueel netwerk| myVirtualNetwork1 - als deze nog niet is geselecteerd, selecteert u **virtueel netwerk** en selecteer vervolgens **myVirtualNetwork1** onder **virtueel netwerk kiezen**.|
    |Subnet| Subnet1 - als deze nog niet is geselecteerd, selecteert u **Subnet** en selecteer vervolgens **Subnet1** onder **Kies subnet**.|
    
    ![Instellingen voor virtuele machines](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. Onder **maken** in de **samenvatting**, selecteer **maken** implementatie van virtuele machine te starten.

### <a name="create-the-second-vm"></a>De tweede virtuele machine maken

Volg de stappen 1-6 opnieuw met de volgende wijzigingen:

|Instelling|Waarde|
|---|---|
|Naam | myVm2|
|Virtueel netwerk | myVirtualNetwork2|

De virtuele machines duren maken in een paar minuten. Ga niet verder met de resterende stappen totdat beide VM's zijn gemaakt.

## <a name="communicate-between-vms"></a>Communicatie tussen virtuele machines

1. In de *Search* vak aan de bovenkant van de portal, begint te typen *myVm1*. Wanneer **myVm1** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Een extern bureaublad verbinding maken met de *myVm1* VM door te selecteren **Connect**, zoals wordt weergegeven in de volgende afbeelding:

    ![Verbinding maken met de virtuele machine](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Voor verbinding met de virtuele machine, het gedownloade RDP-bestand te openen. Als u wordt gevraagd, selecteert u **Connect**.
4. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven), Selecteer vervolgens **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.
6. In een later stadium ping wordt gebruikt om te communiceren met de *myVm2* VM van de *myVm1* VM. Ping maakt gebruik van het Internet Control Message Protocol (ICMP), die via de Windows Firewall standaard is geweigerd. Op de *myVm1* VM, ICMP inschakelen via de Windows firewall, zodat u kunt deze VM van pingen *myVm2* in een latere stap, met behulp van PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```
    
    Hoewel ping wordt gebruikt voor communicatie tussen VM's in dit artikel, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.

7. Verbinding maken met de *myVm2* virtuele machine, voer de volgende opdracht vanaf een opdrachtprompt op de *myVm1* VM:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Aangezien u ping ingeschakeld op *myVm1*, kunt u nu een ping het IP-adres:

    ```
    ping 10.0.0.4
    ```
    
9. De RDP-sessies op beide *myVm1* en *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat: 

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken in dezelfde Azure-regio, verbinden met het virtuele netwerk peering. U kunt ook virtuele netwerken in verschillende peer [ondersteunde regio's](virtual-network-manage-peering.md#cross-region) en in [verschillende Azure-abonnementen](create-peering-different-subscriptions.md#portal), evenals maken [hub en spoke-netwerk ontwerpen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met peering. Zie voor meer informatie over het virtuele netwerk peering, [peering Virtual network-overzicht](virtual-network-peering-overview.md) en [beheren van virtueel netwerk peerings](virtual-network-manage-peering.md).

Als u uw eigen computer verbinden met een virtueel netwerk via een VPN-verbinding en communiceren met resources in een virtueel netwerk of in virtuele netwerken peer is ingesteld, Zie [uw computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
