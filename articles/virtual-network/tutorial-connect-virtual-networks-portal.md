---
title: Virtuele netwerken te verbinden met het virtuele netwerk peering - Azure-portal | Microsoft Docs
description: Ontdek hoe u virtuele netwerken te verbinden met het virtuele netwerk peering.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ac0c1033546758a77b43298a5fa8cba5f5204650
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Virtuele netwerken te verbinden met het virtuele netwerk peering met de Azure portal

U kunt virtuele netwerken met elkaar verbinden met het virtuele netwerk peering. Als u virtuele netwerken brengen, zijn resources in beide virtuele netwerken met elkaar communiceren met de dezelfde latentie en bandbreedte, alsof de bronnen zich in hetzelfde virtuele netwerk. In dit artikel bevat informatie over het maken en peering van twee virtuele netwerken. Procedures voor:

> [!div class="checklist"]
> * Twee virtuele netwerken maken
> * Een peering tussen virtuele netwerken maken
> * Test-peering

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-virtual-networks"></a>Virtuele netwerken maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Zoals u in de volgende afbeelding, voer *myVirtualNetwork1* voor **naam**, *10.0.0.0/16* voor **adresruimte**,  **myResourceGroup** voor **resourcegroep**, *Subnet1* voor Subnet **naam**, 10.0.0.0/24 voor Subnet **-adresbereik** , selecteer een **locatie** en uw **abonnement**, accepteer de standaardinstellingen van de resterende en selecteer vervolgens **maken**:

    ![Een virtueel netwerk maken](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Volg de stappen 1-3 opnieuw met de volgende wijzigingen:
    - **Name**: *myVirtualNetwork2*
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer vervolgens **myResourceGroup**.
    - **Adresruimte**: *10.1.0.0/16*
    - **Subnet-adresbereik**: *10.1.0.0/24*

    Het adresvoorvoegsel voor de *myVirtualNetwork2* virtueel netwerk niet overlapt met de adresruimte van de *myVirtualNetwork1* virtueel netwerk. U kunt geen virtuele netwerken met overlappende adresruimten peer.

## <a name="peer-virtual-networks"></a>Peer virtuele netwerken

1. Begint te typen in het zoekvak boven aan de Azure-portal *MyVirtualNetwork1*. Wanneer **myVirtualNetwork1** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Selecteer **Peerings**onder **instellingen**, en selecteer vervolgens **+ toevoegen**, zoals wordt weergegeven in de volgende afbeelding:

    ![-Peering maken](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Voer in of Selecteer de gegevens in de volgende afbeelding wordt weergegeven en selecteer **OK**. Selecteren de *myVirtualNetwork2* virtueel netwerk, en selecteer **virtueel netwerk**, selecteer daarna *myVirtualNetwork2*.

    ![Instellingen voor peering](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    De **PEERING STATUS** is *gestarte*, zoals wordt weergegeven in de volgende afbeelding:

    ![De status van de peering](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Als u de status niet ziet, vernieuwt u de browser.

4. Zoeken naar de *myVirtualNetwork2* virtueel netwerk. Wanneer deze wordt geretourneerd in de zoekresultaten, selecteert u deze.
5. Stap 1-3 opnieuw met de volgende wijzigingen en selecteer vervolgens **OK**:
    - **Name**: *myVirtualNetwork2-myVirtualNetwork1*
    - **Virtueel netwerk**: *myVirtualNetwork1*

    De **PEERING STATUS** is *verbonden*. Azure gewijzigd ook de status van de peering van de *myVirtualNetwork2 myVirtualNetwork1* peering van *gestarte* naar *verbonden.* Virtueel netwerk peer wordt niet volledig ingesteld totdat de status van de peering voor beide virtuele netwerken *verbonden.* 

Peerings tussen twee virtuele netwerken zijn, maar niet transitief zijn. Ja, bijvoorbeeld, als u wilt ook peer *myVirtualNetwork2* naar *myVirtualNetwork3*, moet u een extra peering tussen virtuele netwerken maken *myVirtualNetwork2* en *myVirtualNetwork3*. Hoewel *myVirtualNetwork1* is gekoppeld *myVirtualNetwork2*, resources binnen *myVirtualNetwork1* kan alleen toegang tot bronnen in  *myVirtualNetwork3* als *myVirtualNetwork1* is ook gekoppeld *myVirtualNetwork3*. 

Voordat de peering productie virtuele netwerken, het raadzaam dat u zorgvuldig vertrouwd raken met de [peering overzicht](virtual-network-peering-overview.md), [beheren peering](virtual-network-manage-peering.md), en [limieten voor virtueel netwerk ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Hoewel dit artikel ziet u een peering tussen twee virtuele netwerken in hetzelfde abonnement en locatie, kunt u ook virtuele netwerken in peer [verschillende regio's](#register) en [verschillende Azure-abonnementen](create-peering-different-subscriptions.md#portal). U kunt ook maken [hub en spoke-netwerk ontwerpen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met peering.

## <a name="test-peering"></a>Test-peering

Een virtuele machine implementeren in elk subnet om te testen netwerkcommunicatie tussen virtuele machines in verschillende virtuele netwerken via een peering, en vervolgens communiceren tussen de virtuele machines. 

### <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtuele machine in elk virtueel netwerk maken, zodat u kunt de communicatie tussen deze in een later stadium valideren.

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. U kunt een ander besturingssysteem selecteren, maar de resterende stappen wordt ervan uitgegaan dat u hebt geselecteerd **Windows Server 2016 Datacenter**. 
3. Selecteer of typ de volgende informatie voor **basisbeginselen**, selecteer daarna **OK**:
    - **Name**: *myVm1*
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer vervolgens *myResourceGroup*.
    - **Locatie**: Selecteer *VS-Oost*.

    De **gebruikersnaam** en **wachtwoord** u invoeren in een latere stap worden gebruikt. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). De **locatie** en **abonnement** geselecteerd moet hetzelfde zijn als de locatie en het virtuele netwerk is in-abonnement. Dit is niet vereist dat u selecteert dat dezelfde resourcegroep die in het virtuele netwerk is gemaakt, maar dezelfde resourcegroep is geselecteerd voor dit artikel.
4. Selecteer een VM-grootte onder **een grootte kiezen**.
5. Selecteer of typ de volgende informatie voor **instellingen**, selecteer daarna **OK**:
    - **Virtueel netwerk**: Zorg ervoor dat **myVirtualNetwork1** is geselecteerd. Zo niet, selecteer **virtueel netwerk** en selecteer vervolgens **myVirtualNetwork1** onder **virtueel netwerk kiezen**.
    - **Subnet**: Zorg ervoor dat **Subnet1** is geselecteerd. Zo niet, selecteer **Subnet** en selecteer vervolgens **Subnet1** onder **Kies subnet**, zoals wordt weergegeven in de volgende afbeelding:
    
        ![Instellingen voor virtuele machines](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. Onder **maken** in de **samenvatting**, selecteer **maken** implementatie van virtuele machine te starten.
7. Stap 1-6 opnieuw uitvoeren, maar voert *myVm2* voor de **naam** van de virtuele machine en selecteer *myVirtualNetwork2* voor **virtueel netwerk**.

Azure toegewezen *10.0.0.4* als het privé IP-adres van de *myVm1* virtuele machine en 10.1.0.4 naar de *myVm2* virtuele machine, omdat ze de eerste beschikbare IP-adres adressen in *Subnet1* van de *myVirtualNetwork1* en *myVirtualNetwork2* virtuele netwerken, respectievelijk.

De virtuele machines maken in een paar minuten duren. Ga niet verder met de resterende stappen totdat beide virtuele machines worden gemaakt.

### <a name="test-virtual-machine-communication"></a>Communicatie van de virtuele machine testen

1. In de *Search* vak aan de bovenkant van de portal, begint te typen *myVm1*. Wanneer **myVm1** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Een extern bureaublad verbinding maken met de *myVm1* virtuele machine door het selecteren van **Connect**, zoals wordt weergegeven in de volgende afbeelding:

    ![Verbinding maken met de virtuele machine](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Voor verbinding met de virtuele machine, het gedownloade RDP-bestand te openen. Als u wordt gevraagd, selecteert u **Connect**.
4. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, om op te geven de referenties die u hebt ingevoerd wanneer u de virtuele machine gemaakt), selecteer vervolgens **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.
6. In een later stadium ping wordt gebruikt om te communiceren met de *myVm2* virtuele machine van de *myVmWeb* virtuele machine. Ping maakt gebruik van ICMP dat via de Windows Firewall standaard is geweigerd. ICMP inschakelen via de Windows firewall met de volgende opdracht vanaf een opdrachtprompt:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Hoewel ping in dit artikel wordt gebruikt voor het testen, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.

7. Verbinding maken met de *myVm2* virtuele machine, voer de volgende opdracht vanaf een opdrachtprompt op de *myVm1* virtuele machine:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Aangezien u ping ingeschakeld op *myVm1*, kunt u nu een ping het IP-adres:

    ```
    ping 10.0.0.4
    ```
    
    U ontvangt vier antwoorden. Als u op de naam van de virtuele machine pingen (*myVm1*), in plaats van het IP-adres, pingen mislukt, omdat *myVm1* is een onbekende host-naam. Azure standaard-naamomzetting werkt tussen virtuele machines in hetzelfde virtuele netwerk, maar niet tussen virtuele machines in verschillende virtuele netwerken. Voor het omzetten van namen in virtuele netwerken, moet u [uw eigen DNS-server implementeren](virtual-networks-name-resolution-for-vms-and-role-instances.md) of gebruik [persoonlijke Azure DNS-domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. De RDP-sessies op beide *myVm1* en *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat: 

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

**<a name="register"></a>Registreren voor de peering preview globale virtueel netwerk**

Peering van virtuele netwerken in dezelfde regio's is algemeen beschikbaar. Virtuele netwerken in verschillende regio's is momenteel in preview-peering. Zie [virtuele netwerk updates](https://azure.microsoft.com/updates/?product=virtual-network) voor beschikbare regio's. Virtuele netwerken to-peer tussen regio's, moet u eerst registreren voor de preview. U kunt niet registreren via de portal, maar u kunt registreren met behulp van [PowerShell](tutorial-connect-virtual-networks-powershell.md#register) of de [Azure CLI](tutorial-connect-virtual-networks-cli.md#register). Als u virtuele netwerken in verschillende regio's probeert voordat u zich registreert voor de mogelijkheid van peer, peering mislukt.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken te verbinden met het virtuele netwerk peering.

Blijven uw eigen computer verbinden met een virtueel netwerk via een VPN-verbinding en communiceren met resources in een virtueel netwerk of in virtuele netwerken peer is ingesteld.

> [!div class="nextstepaction"]
> [Uw computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
