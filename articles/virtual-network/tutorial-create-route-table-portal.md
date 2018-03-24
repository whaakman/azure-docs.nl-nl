---
title: Doorsturen van netwerkverkeer - Azure-portal | Microsoft Docs
description: Informatie over het netwerkverkeer omgeleid met een routetabel met de Azure portal.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 980cf7b59ed16778bbb6cd1b657e3522407c79c9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Doorsturen van netwerkverkeer met een routetabel met de Azure portal

Azure automatisch routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt uw eigen routes voor het overschrijven van Azure maken standaardroutering. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld het routeren van verkeer tussen subnetten via een netwerk (NVA) voor virtueel apparaat. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een routetabel maken
> * Een route maken
> * Een virtueel netwerk maken met meerdere subnetten
> * Een routetabel aan een subnet koppelen
> * Maken van een NVA waarmee verkeer
> * Virtuele machines (VM) implementeren in verschillende subnetten
> * Verkeer leiden uit één subnet naar een andere door middel van een NVA

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Aanmelden bij de Azure portal op http://portal.azure.com.

## <a name="create-a-route-table"></a>Een routetabel maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **routetabel**.
3. Voer, of selecteert, worden de volgende informatie, accepteer de standaardinstelling voor de resterende instelling en selecteer **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myRouteTablePublic|
    |Abonnement| Selecteer uw abonnement.|
    |Resourcegroep | Selecteer **nieuw** en voer *myResourceGroup*.|
    |Locatie|VS - oost|
 
    ![Routetabel maken](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Een route maken

1. In de *zoeken in resources, services en -documenten* vak aan de bovenkant van de portal, begint te typen *myRouteTablePublic*. Wanneer **myRouteTablePublic** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Onder **instellingen**, selecteer **Routes** en selecteer vervolgens **+ toevoegen**, zoals wordt weergegeven in de volgende afbeelding:

    ![Route toevoegen](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. Onder **Add route**, invoert, of selecteert, worden de volgende informatie, accepteer de standaardinstelling voor de overige instellingen en selecteer vervolgens **maken**:

    |Instelling|Waarde|
    |---|---|
    |De routenaam van de|ToPrivateSubnet|
    |Adresvoorvoegsel| 10.0.1.0/24|
    |Volgend hoptype | Selecteer **virtueel apparaat**.|
    |Adres van de volgende hop| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

U moet een virtueel netwerk en subnet maken voordat u een routetabel aan een subnet koppelen kunt, en koppelt u de routetabel aan een subnet:

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Onder **virtueel netwerk maken**, invoert, of selecteert, worden de volgende informatie, accepteer de standaardinstelling voor de overige instellingen en selecteer vervolgens **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVirtualNetwork|
    |Adresruimte| 10.0.0.0/16|
    |Abonnement | Selecteer uw abonnement.|
    |Resourcegroep|Selecteer **gebruik bestaande** en selecteer vervolgens **myResourceGroup**.|
    |Locatie|Selecteer *VS-Oost*|
    |Subnetnaam|Openbaar|
    |Adresbereik|10.0.0.0/24|
    
4. In de **zoeken in resources, services en -documenten** vak aan de bovenkant van de portal, begint te typen *myVirtualNetwork*. Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteer deze.
5. Onder **instellingen**, selecteer **subnetten** en selecteer vervolgens **+ Subnet**, zoals wordt weergegeven in de volgende afbeelding:

    ![Subnet toevoegen](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Selecteer of Voer de volgende informatie en vervolgens **OK**:

    |Instelling|Waarde|
    |---|---|
    |Naam|Privé|
    |Adresruimte| 10.0.1.0/24|

7. Voer stap 5 en 6 opnieuw met de volgende informatie:

    |Instelling|Waarde|
    |---|---|
    |Naam|DMZ|
    |Adresruimte| 10.0.2.0/24|

8. De **myVirtualNetwork - subnetten** wordt weergegeven na het voltooien van de vorige stap. Onder **instellingen**, selecteer **subnetten** en selecteer vervolgens **openbare**.
9. In de volgende afbeelding wordt weergegeven, selecteert u de **routetabel**, selecteer **MyRouteTablePublic**, en selecteer vervolgens **opslaan**:

    ![Routetabel koppelen](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>Maken van een NVA

Een NVA is een virtuele machine die een netwerkfunctie, zoals routering, gebruik of WAN-optimalisatie uitvoert.

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. U kunt een ander besturingssysteem selecteren, maar de resterende stappen wordt ervan uitgegaan dat u hebt geselecteerd **Windows Server 2016 Datacenter**. 
3. Selecteer of typ de volgende informatie voor **basisbeginselen**, selecteer daarna **OK**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVmNva|
    |Gebruikersnaam|Voer een gebruikersnaam van uw keuze.|
    |Wachtwoord|Voer een wachtwoord van uw keuze. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Resourcegroep| Selecteer **gebruik bestaande** en selecteer vervolgens *myResourceGroup*.|
    |Locatie|Selecteer **VS-Oost**.|
4. Selecteer een VM-grootte onder **een grootte kiezen**.
5. Selecteer of typ de volgende informatie voor **instellingen**, selecteer daarna **OK**:

    |Instelling|Waarde|
    |---|---|
    |Virtueel netwerk|myVirtualNetwork - als deze niet is geselecteerd, selecteert u **virtueel netwerk**, selecteer daarna **myVirtualNetwork** onder **virtueel netwerk kiezen**.|
    |Subnet|Selecteer **Subnet** en selecteer vervolgens **DMZ** onder **Kies subnet**. |
    |Openbaar IP-adres| Selecteer **openbaar IP-adres** en selecteer **geen** onder **openbare IP-adres kiezen**. Er is geen openbaar IP-adres is toegewezen aan deze virtuele machine omdat deze niet met vanaf Internet verbonden.
6. Onder **maken** in de **samenvatting**, selecteer **maken** implementatie van virtuele machine te starten.

    De virtuele machine duurt een paar minuten maken. Ga niet door met de volgende stap totdat Azure is gemaakt van de virtuele machine en Hiermee opent u het met informatie over de virtuele machine.

7. In het vak dat geopend voor de virtuele machine nadat deze is gemaakt, klikt u onder **instellingen**, selecteer **Networking**, en selecteer vervolgens **myvmnva158** (de netwerkinterface Azure gemaakt voor uw Virtuele machine heeft een ander nummer na **myvmnva**), zoals wordt weergegeven in de volgende afbeelding:

    ![VM-netwerken](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Voor een netwerkinterface om te kunnen doorsturen van netwerkverkeer dat is verzonden, en dat niet bestemd is voor eigen IP-adres, moet doorsturen via IP worden ingeschakeld voor de netwerkinterface. Onder **instellingen**, selecteer **IP-configuraties**, selecteer **ingeschakeld** voor **doorsturen via IP**, en selecteer vervolgens **opslaan** , zoals wordt weergegeven in de volgende afbeelding:

    ![Doorsturen via IP inschakelen](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines maken in het virtuele netwerk, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via de NVA in een later stadium. Voltooi de stappen 1-6 van [maken van een NVA](#create-a-network-virtual-appliance). Gebruik dezelfde instellingen in stap 3 en 5, met uitzondering van de volgende wijzigingen:

|Naam van virtuele machine      |Subnet      | Openbaar IP-adres     |
|--------- | -----------|---------              |
| myVmPublic  | Openbaar     | Accepteer de standaardgrootte portal |
| myVmPrivate | Privé    | Accepteer de standaardgrootte portal |

Kunt u de *myVmPrivate* VM terwijl Azure maakt de *myVmPublic* VM. Ga niet verder met de volgende stappen totdat beide virtuele machines maken in Azure is.

## <a name="route-traffic-through-an-nva"></a>-Routeverkeer via een NVA

1. In de *Search* vak aan de bovenkant van de portal, begint te typen *myVmPrivate*. Wanneer de **myVmPrivate** VM wordt weergegeven in zoekresultaten wilt weergeven, selecteert u deze.
2. Een extern bureaublad verbinding maken met de *myVmPrivate* VM door te selecteren **Connect**, zoals wordt weergegeven in de volgende afbeelding:

    ![Verbinding maken met de virtuele machine ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Voor verbinding met de virtuele machine, het gedownloade RDP-bestand te openen. Als u wordt gevraagd, selecteert u **Connect**.
4. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine (mogelijk moet u selecteren **meer opties**, vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven), Selecteer vervolgens **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.
6. In een latere stap, de opdracht tracert.exe gebruikt voor het testen van routering. Tracert maakt gebruik van het Internet Control Message Protocol (ICMP), die via de Windows Firewall is geweigerd. ICMP inschakelen via de Windows firewall met de volgende opdracht vanuit PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Hoewel tracert wordt gebruikt voor het testen van routering in dit artikel, wordt zodat ICMP via de Windows Firewall voor productie-implementaties niet aanbevolen.
7. U doorsturen via IP in Azure voor de netwerkinterface van de VM in ingeschakeld [Enable IP fowarding](#enable-ip-forwarding). In de VM moet het besturingssysteem of een toepassing die wordt uitgevoerd vanuit de virtuele machine, ook kunnen voor het doorsturen van netwerkverkeer. Inschakelen van doorsturen via IP in het besturingssysteem van de *myVmNva* VM door te voeren van de volgende stappen uit de *myVmPrivate* VM:

    Extern bureaublad naar de *myVmNva* met de volgende opdracht uit vanaf de opdrachtprompt:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Voer de volgende opdracht in PowerShell zodat doorsturen via in het besturingssysteem IP:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Start opnieuw op de virtuele machine, die ook ontkoppeld van de extern bureaublad-sessiehost.
8. Tijdens het nog wordt verbonden met de *myVmPrivate* VM, een extern-bureaubladsessie met de *myVmPublic* VM met de volgende opdracht, nadat de *myVmNva* VM opnieuw wordt opgestart:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    ICMP inschakelen via de Windows firewall met de volgende opdracht vanuit PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Voor het testen van routering van netwerkverkeer naar de *myVmPrivate* VM van de *myVmPublic* virtuele machine, voer de volgende opdracht vanuit PowerShell:

    ```
    tracert myVmPrivate
    ```

    Het antwoord is vergelijkbaar met het volgende voorbeeld:
    
    ```
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    U ziet dat de eerste hop 10.0.2.4 die de NVA privé IP-adres is. De tweede hop is 10.0.1.4, de persoonlijke IP-adres van de *myVmPrivate* VM. De route toegevoegd aan de *myRouteTablePublic* routetabel en die is gekoppeld aan de *openbare* subnet veroorzaakt Azure voor het routeren van het verkeer via de NVA in plaats van rechtstreeks naar de *persoonlijke* subnet.
10.  Sluit de extern-bureaubladsessie naar de *myVmPublic* VM, zodat u nog steeds is verbonden met de *myVmPrivate* VM.
11. Voor het testen van routering van netwerkverkeer naar de *myVmPublic* VM van de *myVmPrivate* virtuele machine, voer de volgende opdracht uit vanaf de opdrachtprompt:

    ```
    tracert myVmPublic
    ```

    Het antwoord is vergelijkbaar met het volgende voorbeeld:

    ```
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    U kunt zien dat verkeer wordt gerouteerd rechtstreeks vanuit de *myVmPrivate* VM naar de *myVmPublic* VM. Standaard Azure routes verkeer rechtstreeks tussen subnetten.
12. Sluit de extern-bureaubladsessie naar de *myVmPrivate* VM.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat: 

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een routetabel gemaakt en gekoppeld aan een subnet. U hebt gemaakt met een eenvoudige NVA dat verkeer van een openbare subnet naar een persoonlijke subnet gerouteerd. Implementeren van tal van vooraf geconfigureerde NVAs waarmee de netwerkfuncties, zoals firewall- en WAN-optimalisatie van de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Voordat u implementeert routetabellen voor gebruik in productieomgevingen, wordt aanbevolen dat u zorgvuldig vertrouwd raken met [routering in Azure](virtual-networks-udr-overview.md), [beheren routetabellen](manage-route-table.md), en [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).


Terwijl u veel Azure-resources binnen een virtueel netwerk implementeren kunt, kunnen geen resources voor sommige Azure PaaS-services worden geïmplementeerd in een virtueel netwerk. U kunt nog steeds toegang beperken tot de bronnen van sommige services Azure PaaS verkeer alleen via een virtueel netwerksubnet al. Ga naar de volgende zelfstudie voor informatie over het netwerktoegang tot Azure PaaS-resources te beperken.

> [!div class="nextstepaction"]
> [Beperken van toegang tot het netwerk voor PaaS-resources](tutorial-restrict-network-access-to-resources.md)
