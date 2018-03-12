---
title: Een virtueel Azure-netwerk maken met meerdere subnetten - Portal | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met meerdere subnetten met de Azure portal.
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 898bdef779282d7312c76696f744b97ec2dfcded
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-portal"></a>Een virtueel netwerk maken met meerdere subnetten met de Azure portal

Een virtueel netwerk kunt verschillende soorten Azure-resources met het Internet en privé met elkaar communiceren. Meerdere subnetten in een virtueel netwerk maken, kunt u bij het segmenteren van uw netwerk, zodat u kunt filteren of het besturingselement de verkeersstroom tussen subnetten. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een virtueel netwerk maken
> * Een subnet maken
> * Testen van netwerkcommunicatie tussen virtuele machines

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Zoals u in de volgende afbeelding, voer *myVirtualNetwork* voor **naam**, *10.0.0.0/16* voor **adresruimte**,  **myResourceGroup** voor **resourcegroep**, *openbare* voor Subnet **naam**, 10.0.0.0/24 voor Subnet **-adresbereik**, selecteer een **locatie** en uw **abonnement**, accepteer de standaardinstellingen van de resterende en selecteer vervolgens **maken**:

    ![Een virtueel netwerk maken](./media/virtual-networks-create-vnet-arm-pportal/create-virtual-network.png)

    De **adresruimte** en **-adresbereik** zijn opgegeven in CIDR-notatie. De opgegeven **adresruimte** bevat het IP-adressen 10.0.0.0-10.0.255.254. De **adresbereik** voor een subnet opgegeven, moet binnen het **adresruimte** gedefinieerd voor het virtuele netwerk. Azure DHCP IP-adressen worden toegewezen uit het adresbereik van een subnet naar bronnen die zijn geïmplementeerd in een subnet. Azure alleen de 10.0.0.4-10.0.0.254 adressen worden toegewezen aan resources die zijn geïmplementeerd in de **openbare** subnet, omdat de eerste vier adressen zijn gereserveerd Azure (10.0.0.0-10.0.0.3 voor het subnet van de in dit voorbeeld) en de laatste adres () 10.0.0.255 voor het subnet van de in dit voorbeeld) in elk subnet.

## <a name="create-a-subnet"></a>Een subnet maken

1. In de **zoeken in resources, services en -documenten** vak aan de bovenkant van de portal, begint te typen *myVirtualNetwork*. Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Selecteer **subnetten** en selecteer vervolgens **+ Subnet**, zoals wordt weergegeven in de volgende afbeelding:

     ![Een subnet toevoegen](./media/virtual-networks-create-vnet-arm-pportal/add-subnet.png)
     
3. In de **subnet toevoegen** dat verschijnt, voer *persoonlijke* voor **naam**, voer *10.0.1.0/24* voor **-adresbereik**, en selecteer vervolgens **OK**.  Een subnet-adresbereik overlappen niet met de adresbereiken van andere subnetten binnen een virtueel netwerk. 

Voordat u deze implementeert in Azure virtuele netwerken en subnetten voor gebruik in productieomgevingen, wordt aanbevolen dat u zorgvuldig vertrouwd raken met de adresruimte [overwegingen](manage-virtual-network.md#create-a-virtual-network) en [virtueel netwerk limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Zodra resources zijn geïmplementeerd in subnetten, kunnen sommige virtueel netwerk en subnet wijzigingen, zoals het wijzigen van-adresbereiken opnieuw distribueren van bestaande Azure-resources geïmplementeerd in subnetten vereisen.

## <a name="test-network-communication"></a>Test de netwerkcommunicatie

Een virtueel netwerk kunt verschillende soorten Azure-resources met het Internet en privé met elkaar communiceren. Een type resource dat u in een virtueel netwerk implementeren kunt is een virtuele machine. Twee virtuele machines maken in het virtuele netwerk, zodat u de netwerkcommunicatie tussen deze en het Internet in een later stadium testen kunt.

### <a name="create-virtual-machines"></a>Virtuele machines maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. U kunt een ander besturingssysteem selecteren, maar de resterende stappen wordt ervan uitgegaan dat u hebt geselecteerd **Windows Server 2016 Datacenter**. 
3. Selecteer of typ de volgende informatie voor **basisbeginselen**, selecteer daarna **OK**:
    - **Name**: *myVmWeb*
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer vervolgens *myResourceGroup*.
    - **Locatie**: Selecteer *VS-Oost*.

    De **gebruikersnaam** en **wachtwoord** u invoeren in een latere stap worden gebruikt. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). De **locatie** en **abonnement** geselecteerd moet hetzelfde zijn als de locatie en het virtuele netwerk is in-abonnement. Dit is niet vereist dat u dezelfde resourcegroep die in het virtuele netwerk is gemaakt, maar dezelfde resourcegroep is geselecteerd voor deze zelfstudie selecteert.
4. Selecteer een VM-grootte onder **een grootte kiezen**.
5. Selecteer of typ de volgende informatie voor **instellingen**, selecteer daarna **OK**:
    - **Virtueel netwerk**: Zorg ervoor dat **myVirtualNetwork** is geselecteerd. Zo niet, selecteer **virtueel netwerk** en selecteer vervolgens **myVirtualNetwork** onder **virtueel netwerk kiezen**.
    - **Subnet**: Zorg ervoor dat **openbare** is geselecteerd. Zo niet, selecteer **Subnet** en selecteer vervolgens **openbare** onder **Kies subnet**, zoals wordt weergegeven in de volgende afbeelding:
    
        ![Instellingen voor virtuele machines](./media/virtual-networks-create-vnet-arm-pportal/virtual-machine-settings.png)
 
6. Onder **maken** in de **samenvatting**, selecteer **maken** implementatie van virtuele machine te starten.
7. Stap 1-6 opnieuw uitvoeren, maar voert *myVmMgmt* voor de **naam** van de virtuele machine en selecteer **persoonlijke** voor de **Subnet**.

De virtuele machines maken in een paar minuten duren. Ga niet verder met de resterende stappen totdat beide virtuele machines worden gemaakt.

De virtuele machines die in dit artikel hebt [netwerkinterface](virtual-network-network-interface.md) met één IP-adres dat dynamisch wordt toegewezen aan de netwerkinterface. Nadat u de virtuele machine hebt geïmplementeerd, kunt u [meerdere openbare en particuliere IP-adressen toevoegen of wijzigen van de toewijzingsmethode van IP-adres naar statisch](virtual-network-network-interface-addresses.md#add-ip-addresses). U kunt [netwerkinterfaces toevoegen](virtual-network-network-interface-vm.md#vm-add-nic), tot de limiet wordt ondersteund door de [VM-grootte](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dat u selecteert wanneer u een virtuele machine maken. U kunt ook [één hoofdelement i/o-virtualisatie (SR-IOV) inschakelen](create-vm-accelerated-networking-powershell.md) voor een VM, maar alleen wanneer u een virtuele machine maken met een VM-grootte die ondersteuning de mogelijkheid biedt.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Communiceren tussen virtuele machines en met het internet

1. In de *Search* vak aan de bovenkant van de portal, begint te typen *myVmMgmt*. Wanneer **myVmMgmt** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Een extern bureaublad verbinding maken met de *myVmMgmt* virtuele machine door het selecteren van **Connect**, zoals wordt weergegeven in de volgende afbeelding:

    ![Verbinding maken met de virtuele machine](./media/virtual-networks-create-vnet-arm-pportal/connect-to-virtual-machine.png)  

3. Voor verbinding met de virtuele machine, het gedownloade RDP-bestand te openen. Als u wordt gevraagd, selecteert u **Connect**.
4. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, om op te geven de referenties die u hebt ingevoerd wanneer u de virtuele machine gemaakt), selecteer vervolgens **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.
6. In een later stadium ping wordt gebruikt om te communiceren met de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine. Ping maakt gebruik van ICMP dat via de Windows Firewall standaard is geweigerd. ICMP inschakelen via de Windows firewall met de volgende opdracht vanaf een opdrachtprompt:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Hoewel ping in dit artikel wordt gebruikt, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.
7. Uit veiligheidsoverwegingen is het gebruikelijk om het aantal virtuele machines die worden op afstand verbinding met een virtueel netwerk gemaakt kunnen te beperken. In deze zelfstudie de *myVmMgmt* virtuele machine wordt gebruikt voor het beheren van de *myVmWeb* virtuele machine in het virtuele netwerk. Met extern bureaublad naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine, voer de volgende opdracht uit vanaf de opdrachtprompt:

    ``` 
    mstsc /v:myVmWeb
    ```
8. Om te communiceren met de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine, voer de volgende opdracht uit vanaf de opdrachtprompt:

    ```
    ping myvmmgmt
    ```

    De uitvoer is vergelijkbaar met de volgende voorbeelduitvoer wordt:
    
    ```
    Pinging myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net [10.0.1.4] with 32 bytes of data:
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.1.4: bytes=32 time<1ms TTL=128
    
    Ping statistics for 10.0.1.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 0ms, Average = 0ms
    ```
      
    Kunt u zien dat het adres van de *myVmMgmt* virtuele machine is 10.0.1.4. 10.0.1.4 is het eerste beschikbare IP-adres in het adresbereik van de *persoonlijke* subnet dat u hebt geïmplementeerd de *myVmMgmt* virtuele machine in de vorige stap.  U ziet dat de volledig gekwalificeerde domeinnaam van de virtuele machine *myvmmgmt.dar5p44cif3ulfq00wxznl3i3f.bx.internal.cloudapp.net*. Hoewel de *dar5p44cif3ulfq00wxznl3i3f* gedeelte van de domeinnaam is verschillend voor de virtuele machine, de resterende gedeelten van de domeinnaam zijn hetzelfde. 

    Alle virtuele machines in Azure gebruiken de standaard Azure DNS-service. Alle virtuele machines binnen een virtueel netwerk, kunnen de namen van alle andere virtuele machines in hetzelfde virtuele netwerk met behulp van Azure standaard DNS-service omzetten. In plaats van Azure standaard DNS-service, kunt u uw eigen DNS-server of de mogelijkheid persoonlijke domein van de Azure DNS-service. Zie voor meer informatie [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) of [Azure DNS gebruiken voor persoonlijke domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. Internet Information Services (IIS) voor Windows Server installeren op de *myVmWeb* virtuele machine, voer de volgende opdracht vanuit een PowerShell-sessie:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

10. Nadat de installatie van IIS voltooid is, Verbreek de verbinding met de *myVmWeb* extern bureaublad-sessiehost, waarbij u in de *myVmMgmt* extern bureaublad-sessiehost. Open een webbrowser en Ga naar http://myvmweb. U ziet de pagina Welkom IIS.
11. Verbreek de verbinding met de *myVmMgmt* extern bureaublad-sessiehost.
12. Zoeken naar het openbare IP-adres van de *myVmWeb* virtuele machine. Wanneer Azure gemaakt de *myVmWeb* virtuele machine, een openbare IP-adres-resource met de naam *myVmWeb* ook is gemaakt en toegewezen aan de virtuele machine. U kunt zien dat 52.170.5.92 is toegewezen voor **openbaar IP-adres** naar de *myVmMgmt* virtuele machine in de afbeelding in stap 2. Vinden van het openbare IP-adres is toegewezen aan de *myVmWeb* virtuele machine, zoekt u *myVmWeb* in het zoekvak van de portal, selecteert u vervolgens wanneer deze wordt weergegeven in de zoekresultaten.

    Hoewel een virtuele machine is niet vereist voor het openbare IP-adres toegewezen, wijst Azure een openbaar IP-adres toe aan elke virtuele machine die u, standaard maakt. Om te communiceren via Internet met een virtuele machine, moet een openbaar IP-adres worden toegewezen aan de virtuele machine. Alle virtuele machines kunnen communiceren met het Internet, uitgaande of er een openbaar IP-adres is toegewezen aan de virtuele machine. Zie voor meer informatie over uitgaande Internet-verbindingen in Azure, [uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
13. Blader op uw eigen computer naar het openbare IP-adres van de *myVmWeb* virtuele machine. De poging om weer te geven van de IIS-welkomstpagina van uw eigen computer is mislukt. De poging is mislukt omdat wanneer de virtuele machines zijn geïmplementeerd, Azure standaard een netwerkbeveiligingsgroep voor elke virtuele machine gemaakt. 

     Een netwerkbeveiligingsgroep bevat beveiligingsregels voor verbindingen toestaan of weigeren van binnenkomende en uitgaande netwerkverkeer op poort en IP-adres. De standaard netwerkbeveiligingsgroep die Azure gemaakt kan communicatie via alle poorten tussen resources in hetzelfde virtuele netwerk. Voor Windows virtuele machines weigert de netwerkbeveiligingsgroep voor de standaard alle binnenkomend verkeer van Internet via alle poorten, met uitzondering van TCP-poort 3389 (RDP). Als gevolg hiervan standaard kunt u ook RDP rechtstreeks naar de *myVmWeb* virtuele machine van het Internet, zelfs als u wilt niet poort 3389 openen met een webserver. Omdat websurfen via poort 80 communiceert, wordt de communicatie van het Internet mislukt omdat er is geen regel in de standaard netwerkbeveiligingsgroep verkeer toestaat via poort 80.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat: 

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een virtueel netwerk met meerdere subnetten te implementeren. U hebt ook geleerd wanneer u een virtuele Windows-computer maakt, Azure een netwerkinterface maakt dat wordt gekoppeld aan de virtuele machine en wordt gemaakt van een netwerkbeveiligingsgroep waarmee alleen verkeer via poort 3389 van Internet. Ga naar de volgende zelfstudie voor informatie over het filteren van netwerkverkeer naar subnetten in plaats van afzonderlijke virtuele machines.

> [!div class="nextstepaction"]
> [Filteren van netwerkverkeer naar subnetten](./virtual-networks-create-nsg-arm-pportal.md)
