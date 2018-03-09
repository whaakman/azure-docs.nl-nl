---
title: Doorsturen van netwerkverkeer - Azure-portal | Microsoft Docs
description: Informatie over het netwerkverkeer omgeleid met een routetabel met de Azure portal.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 4cc814e1fd3ee8979662695f9dec3dcce335dc2a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Doorsturen van netwerkverkeer met een routetabel met de Azure portal

Azure automatisch routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt uw eigen routes voor het overschrijven van Azure maken standaardroutering. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld het routeren van verkeer tussen subnetten door een firewall. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een routetabel maken
> * Een route maken
> * Een routetabel aan het subnet van een virtueel netwerk koppelen
> * Test routering
> * Routering oplossen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u via http://portal.azure.com aan bij Azure Portal.

## <a name="create-a-route-table"></a>Een routetabel maken

Azure routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. Zie voor meer informatie over Azure standaardroutes, [systeemroutes](virtual-networks-udr-overview.md). Als u wilt overschrijven Azure standaard routering, een routetabel met routes maken en koppelen van de routetabel een subnet van het virtuele netwerk.

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **routetabel**.
3. Selecteer uw **abonnement** en selecteer of Voer de volgende informatie op en selecteer **maken**:
 
    ![Routetabel maken](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Een route maken

Een routetabel bevat nul of meer routes. 

1. In de *zoeken in resources, services en -documenten* vak aan de bovenkant van de portal, begint te typen *myRouteTablePublic*. Wanneer **myRouteTablePublic** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Onder **instellingen**, selecteer **Routes** en selecteer vervolgens **+ toevoegen**, zoals wordt weergegeven in de volgende afbeelding:

    ![Route toevoegen](./media/tutorial-create-route-table-portal/add-route.png) 
 
4. Onder **Add route**, selecteer of Voer de volgende informatie en selecteer vervolgens **OK**:
    - **De routenaam**: *ToPrivateSubnet*
    - **Adresvoorvoegsel**: 10.0.1.0/24
    - **Type van volgende hop**: Selecteer **virtueel apparaat**.
    - **Adres van volgende hop**: 10.0.2.4

    De route wordt al het verkeer dat is bestemd voor het adresvoorvoegsel 10.0.1.0/24 via een virtueel netwerk-apparaat met het IP-adres 10.0.2.4 doorverwezen. De virtuele netwerkapparaat en het subnet met het opgegeven adres-voorvoegsel worden in latere stappen gemaakt. De route overschrijft van Azure-standaard routering, die verkeer tussen subnetten rechtstreeks gerouteerd. Elke route geeft een volgend hoptype. Het volgende hoptype geïnstrueerd Azure hoe het verkeer te routeren. In dit voorbeeld wordt het volgende hoptype is *VirtualAppliance*. Zie voor meer informatie over alle beschikbare hoptypen in de volgende in Azure, en wanneer u ze gebruikt, [volgende hop typen](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Voordat u een routetabel aan een subnet koppelt kunt, hebt u een virtueel netwerk en subnet maken:

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Onder **virtueel netwerk maken**, selecteert, of Voer de volgende informatie in en selecteer **maken**:
    
    - **Name**: *myVirtualNetwork*
    - **Adresruimte**: *10.0.0.0/16*
    - **Abonnement**: uw abonnement te selecteren.
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer **myResourceGroup**.
    - **Locatie**: Selecteer *VS-Oost*
    - **Subnetnaam**: *openbare*
    - **-Adresbereik:** *10.0.0.0/24*

4. In de **zoeken in resources, services en -documenten** vak aan de bovenkant van de portal, begint te typen *myVirtualNetwork*. Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteer deze.
5. Twee extra subnetten toevoegen aan het virtuele netwerk. Onder **instellingen**, selecteer **subnetten** en selecteer vervolgens **+ Subnet**, zoals wordt weergegeven in de volgende afbeelding:

    ![Subnet toevoegen](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Selecteer of Voer de volgende informatie en vervolgens **OK**:
    - **Naam**: persoonlijke
    - **Adresruimte**: *10.0.1.0/24*

7. Voer stap 5 en 6 opnieuw met de volgende informatie:
    - **Naam**: DMZ
    - **Adresruimte**: *10.0.2.0/24*

U kunt een routetabel met nul of meer subnetten koppelen. Een subnet kan nul of één routetabel gekoppeld hebben. Uitgaand verkeer van een subnet wordt doorgestuurd op basis van Azure standaardroutes en eventuele aangepaste routes die u hebt toegevoegd aan een routetabel die u aan een subnet koppelen. Koppel de *myRouteTablePublic* routetabel aan de *openbare* subnet:

1. De **myVirtualNetwork - subnetten** wordt weergegeven na het voltooien van de vorige stap. Onder **instellingen**, selecteer **subnetten** en selecteer vervolgens **openbare**.
2. In de volgende afbeelding wordt weergegeven, selecteert u de **routetabel**, selecteer daarna **MyRouteTablePublic**.

    ![Routetabel koppelen](./media/tutorial-create-route-table-portal/associate-route-table.png) 

3. Selecteer **Opslaan**.

## <a name="test-routing"></a>Test routering

Als u wilt testen routering, maakt u een virtuele machine die als het virtuele netwerk-apparaat fungeert dat de route die u in de vorige stap hebt gemaakt door middel van routes. Na het maken van het virtuele netwerk-apparaat, implementeert u een virtuele machine in de *openbare* en *persoonlijke* subnetten. U moet vervolgens doorsturen verkeer van de *openbare* subnet moet de *persoonlijke* subnet via het netwerk virtuele apparaat.

### <a name="create-a-network-virtual-appliance"></a>Maken van een virtueel netwerkapparaat

In de vorige stap, moet u een route die een virtueel netwerkapparaat als het volgende hoptype opgegeven gemaakt. Een virtuele machine met een netwerktoepassing wordt vaak een virtueel netwerkapparaat genoemd. In productieomgevingen is het netwerk virtuele apparaat die u implementeert vaak een vooraf geconfigureerde virtuele machine. Verschillende virtuele netwerkapparaten zijn beschikbaar via de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). In dit artikel wordt wordt een eenvoudige virtuele machine gemaakt.

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. U kunt een ander besturingssysteem selecteren, maar de resterende stappen wordt ervan uitgegaan dat u hebt geselecteerd **Windows Server 2016 Datacenter**. 
3. Selecteer of typ de volgende informatie voor **basisbeginselen**, selecteer daarna **OK**:
    - **Name**: *myVmNva*
    - **Resourcegroep**: Selecteer **gebruik bestaande** en selecteer vervolgens *myResourceGroup*.
    - **Locatie**: Selecteer *VS-Oost*.

    De **gebruikersnaam** en **wachtwoord** u invoeren in een latere stap worden gebruikt. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). De **locatie** en **abonnement** geselecteerd moet hetzelfde zijn als de locatie en het virtuele netwerk is in-abonnement. Dit is niet vereist dat u dezelfde resourcegroep die in het virtuele netwerk is gemaakt, maar dezelfde resourcegroep is geselecteerd voor deze zelfstudie selecteert.
4. Selecteer een VM-grootte onder **een grootte kiezen**.
5. Selecteer of typ de volgende informatie voor **instellingen**, selecteer daarna **OK**:
    - **Virtueel netwerk**: Zorg ervoor dat **myVirtualNetwork** is geselecteerd. Zo niet, selecteer **virtueel netwerk**, selecteer daarna **myVirtualNetwork** onder **virtueel netwerk kiezen**.
    - **Subnet**: Selecteer **Subnet** en selecteer vervolgens **DMZ** onder **Kies subnet**.
    - **Openbaar IP-adres**: Selecteer **openbaar IP-adres** en selecteer **geen** onder **openbare IP-adres kiezen**. Er is geen openbaar IP-adres is toegewezen aan deze virtuele machine omdat deze niet met vanaf Internet verbonden.
6. Onder **maken** in de **samenvatting**, selecteer **maken** implementatie van virtuele machine te starten.

De virtuele machine duurt een paar minuten maken. Ga niet door met de volgende stap totdat Azure is gemaakt van de virtuele machine en Hiermee opent u het met informatie over de virtuele machine.

Wanneer u Azure maakt de virtuele machine, het ook gemaakt een [netwerkinterface](virtual-network-network-interface.md) in de *DMZ* subnet en de netwerkinterface gekoppeld aan de virtuele machine. Doorsturen via IP moet zijn ingeschakeld voor elke Azure-netwerk-interface die bestemd zijn voor alle IP-adres dat niet is toegewezen aan de netwerkinterface verkeer doorstuurt.

1. In het vak dat geopend voor de virtuele machine nadat deze is gemaakt, klikt u onder **instellingen**, selecteer **Networking**, en selecteer vervolgens **myvmnva158** (de netwerkinterface Azure gemaakt voor uw virtuele machine heeft een ander nummer na **myvmnva**), zoals wordt weergegeven in de volgende afbeelding:

    ![VM-netwerken](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

    Tijdens het maken van het virtuele netwerk-apparaat in de *DMZ* subnet, Azure automatisch gemaakt van de netwerkinterface, de netwerkinterface gekoppeld aan de virtuele machine en de persoonlijke IP-adres toegewezen  *10.0.2.4* aan de netwerkinterface. 

2. Onder **instellingen**, selecteer **IP-configuraties**, selecteer **ingeschakeld** voor **doorsturen via IP**, en selecteer vervolgens **opslaan** , zoals wordt weergegeven in de volgende afbeelding:

    ![Doorsturen via IP inschakelen](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

### <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via het virtuele netwerk-apparaat in een later stadium.

Voltooi de stappen 1-6 van [maken van een virtueel netwerkapparaat](#create-a-network-virtual-appliance). Gebruik dezelfde instellingen in stap 3 en 5, met uitzondering van de volgende wijzigingen:

|Virtuele machine   |Naam      |Subnet      | Openbaar IP-adres     |
|---------         |--------- | -----------|---------              |
|Virtuele machine 1 | myVmWeb  | Openbaar     | Accepteer de standaardgrootte portal |
|virtuele machine 2 | myVmMgmt | Privé    | Accepteer de standaardgrootte portal |

Kunt u de *myVmMgmt* virtuele machine bij het maken van Azure de *myVmWeb* virtuele machine. Ga niet verder met de volgende stappen totdat beide virtuele machines maken in Azure is.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>-Routeverkeer via een virtueel netwerkapparaat

1. In de *Search* vak aan de bovenkant van de portal, begint te typen *myVmMgmt*. Wanneer **myVmMgmt** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Een extern bureaublad verbinding maken met de *myVmMgmt* virtuele machine door het selecteren van **Connect**, zoals wordt weergegeven in de volgende afbeelding:

    ![Verbinding maken met de virtuele machine](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Voor verbinding met de virtuele machine, het gedownloade RDP-bestand te openen. Als u wordt gevraagd, selecteert u **Connect**.
4. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, om op te geven de referenties die u hebt ingevoerd wanneer u de virtuele machine gemaakt), selecteer vervolgens **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.
6. In een latere stap, de opdracht tracert.exe gebruikt voor het testen van routering. Tracert maakt gebruik van ICMP dat via de Windows Firewall is geweigerd. ICMP inschakelen via de Windows firewall met de volgende opdracht vanaf een opdrachtprompt:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Hoewel tracert wordt gebruikt voor het testen van routering in dit artikel, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.
7. U doorsturen via IP in Azure voor de netwerkinterface van de virtuele machine in ingeschakeld [Enable IP fowarding](#enable-ip-forwarding). Vanuit de virtuele machine moet het besturingssysteem of een toepassing die wordt uitgevoerd op de virtuele machine ook kunnen voor het doorsturen van netwerkverkeer. Wanneer u een virtueel netwerkapparaat in een productieomgeving implementeert, het toestel doorgaans filters, Logboeken of een andere functie uitgevoerd voordat het doorsturen van verkeer. In dit artikel, het besturingssysteem gewoon alle verkeer wordt doorgestuurd die het ontvangt. Inschakelen van doorsturen via IP in het besturingssysteem van de *myVmNva* door de volgende stappen uit te voeren de *myVmMgmt* virtuele machine:

    Extern bureaublad naar de *myVmNva* virtuele machine met de volgende opdracht uit vanaf de opdrachtprompt:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Voer de volgende opdracht in PowerShell zodat doorsturen via in het besturingssysteem IP:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Start opnieuw op de virtuele machine, die ook de extern bureaublad-sessiehost verbreekt.
8. Tijdens het nog wordt verbonden met de *myVmMgmt* virtuele machine na de *myVmNva* opnieuw opstarten van de virtuele machine maken een extern-bureaubladsessie naar de *myVmWeb* virtuele machine met de volgende opdracht:

    ``` 
    mstsc /v:myVmWeb
    ```
    
    ICMP inschakelen via de Windows firewall met de volgende opdracht vanaf een opdrachtprompt:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

9. Voor het testen van routering van netwerkverkeer naar de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine, voer de volgende opdracht uit vanaf de opdrachtprompt:

    ```
    tracert myvmmgmt
    ```

    Het antwoord is vergelijkbaar met het volgende voorbeeld:
    
    ```
    Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    U ziet dat de eerste hop 10.0.2.4 die de virtuele netwerkapparaat van privé IP-adres is. De tweede hop is 10.0.1.4, de persoonlijke IP-adres van de *myVmMgmt* virtuele machine. De route toegevoegd aan de *myRouteTablePublic* routetabel en die is gekoppeld aan de *openbare* subnet veroorzaakt Azure voor het routeren van het verkeer via de NVA in plaats van rechtstreeks naar de *persoonlijke* subnet.
10.  Sluit de extern-bureaubladsessie naar de *myVmWeb* virtuele machine, zodat u nog steeds is verbonden met de *myVmMgmt* virtuele machine.
11. Voor het testen van routering van netwerkverkeer naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine, voer de volgende opdracht uit vanaf de opdrachtprompt:

    ```
    tracert myvmweb
    ```

    Het antwoord is vergelijkbaar met het volgende voorbeeld:

    ```
    Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    U kunt zien dat verkeer wordt gerouteerd rechtstreeks vanuit de *myVmMgmt* virtuele machine naar de *myVmWeb* virtuele machine. Standaard Azure routes verkeer rechtstreeks tussen subnetten.
12. Sluit de extern-bureaubladsessie naar de *myVmMgmt* virtuele machine.

## <a name="troubleshoot-routing"></a>Routering oplossen

Zoals u in de vorige stappen hebt geleerd, geldt Azure standaardroutes die u desgewenst met uw eigen routes overschrijven kunt. Verkeer kan soms niet zoals verwacht dat het worden gerouteerd. U kunt de [volgende hop](../network-watcher/network-watcher-check-next-hop-portal.md) mogelijkheden van Azure-netwerk-Watcher om te bepalen hoe Azure routeren van verkeer tussen twee virtuele machines. 

1. In de *Search* vak aan de bovenkant van de portal, begint te typen *netwerk-Watcher*. Wanneer **netwerk-Watcher** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Onder **NETWERKDIAGNOSEHULPMIDDELEN**, selecteer **volgende hop**.
3. Test voor verkeersroutering van de *myVmWeb* (10.0.0.4) virtuele machine naar de *myVmMgmt* (10.0.1.4) virtuele machine, selecteer uw abonnement, voert u de gegevens in de volgende afbeelding wordt weergegeven (uw **Netwerkinterface** naam iets anders is), en selecteer vervolgens **volgende hop**:

    ![Volgende hop](./media/tutorial-create-route-table-portal/next-hop.png)  

    De **resultaat** uitvoer wordt gemeld dat de volgende hop-IP-adres voor verkeer van *myVmWeb* naar *myVmMgmt* 10.0.2.4 wordt (de *myVmNva* virtuele machine), het volgende hoptype is *VirtualAppliance*, en dat de routetabel zorgt ervoor dat de routering *myRouteTablePublic*.

De effectieve routes voor elke netwerkinterface zijn een combinatie van Azure standaardroutes en alle routes die u definieert. Overzicht van alle routes effectieve voor een netwerkinterface in een virtuele machine, moet u de volgende stappen uitvoeren:

1. In de *Search* vak aan de bovenkant van de portal, begint te typen *myVmWeb*. Wanneer **myVmWeb** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Onder **instellingen**, selecteer **Networking**, en selecteer vervolgens **myvmweb369** (de netwerkinterface Azure gemaakt voor uw virtuele machine een ander nummer na heeft**myvmweb**).
3. Onder **ondersteuning + probleemoplossing**, selecteer **effectieve routes**, zoals wordt weergegeven in de volgende afbeelding:

    ![Effectieve routes](./media/tutorial-create-route-table-portal/effective-routes.png) 

    U ziet standaardroutes van Azure en de route die u hebt toegevoegd in de *myRouteTablePublic* routetabel. Zie voor meer informatie over hoe Azure een route geselecteerd uit de lijst met routes, [hoe Azure selecteert een route](virtual-networks-udr-overview.md#how-azure-selects-a-route).

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat: 

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een routetabel gemaakt en gekoppeld aan een subnet. U hebt gemaakt met een virtueel netwerkapparaat dat verkeer van een openbare subnet naar een persoonlijke subnet gerouteerd. Terwijl u veel Azure-resources binnen een virtueel netwerk implementeren kunt, kunnen geen resources voor sommige Azure PaaS-services worden geïmplementeerd in een virtueel netwerk. U kunt nog steeds toegang beperken tot de bronnen van sommige services Azure PaaS verkeer alleen via een virtueel netwerksubnet al. Ga naar de volgende zelfstudie voor informatie over het netwerktoegang tot Azure PaaS-resources te beperken.

> [!div class="nextstepaction"]
> [Beperken van toegang tot het netwerk voor PaaS-resources](virtual-network-service-endpoints-configure.md#azure-portal)
