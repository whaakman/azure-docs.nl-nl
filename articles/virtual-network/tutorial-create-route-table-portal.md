---
title: Netwerkverkeer routeren - zelfstudie - Azure Portal | Microsoft Docs
description: In deze zelfstudie leert u netwerkverkeer te routeren met een routetabel met behulp van Azure Portal.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 7254e9336fca14daee2021d5bde4c5538509fe35
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
ms.locfileid: "30842319"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Zelfstudie: netwerkverkeer routeren met een routetabel met behulp van Azure Portal

Azure routeert standaard automatisch verkeer tussen alle subnetten in een virtueel netwerk. U kunt uw eigen routes maken om de standaardroutering van Azure te overschrijven. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld verkeer tussen subnetten wilt routeren via een NVA (virtueel netwerkapparaat). In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een routetabel maken
> * Een route maken
> * Een virtueel netwerk met meerdere subnetten maken
> * Een routetabel aan een subnet koppelen
> * Een NVA maken voor het routeren van verkeer
> * Virtuele machines (VM's) implementeren in verschillende subnetten
> * Verkeer van het ene subnet naar het andere leiden via een NVA

U kunt deze zelfstudie desgewenst volgen met behulp van de [Azure CLI](tutorial-create-route-table-cli.md) of [Azure PowerShell](tutorial-create-route-table-powershell.md).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op http://portal.azure.com.

## <a name="create-a-route-table"></a>Een routetabel maken

1. Selecteer **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Netwerken** en selecteer vervolgens **Routetabel**.
3. Voer de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de resterende instelling en selecteer **Maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myRouteTablePublic|
    |Abonnement| Selecteer uw abonnement.|
    |Resourcegroep | Selecteer **Nieuwe maken** en voer *myResourceGroup* in.|
    |Locatie|VS - oost|
 
    ![Routetabel maken](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Een route maken

1. Begin in het vak *Resources, services en documenten zoeken* bovenaan de portal *myRouteTablePublic* te typen. Wanneer **myRouteTablePublic** wordt weergegeven in de zoekresultaten, selecteert u dit.
2. Selecteer **Routes** onder **INSTELLINGEN** en selecteer **+Toevoegen**, zoals in de volgende afbeelding:

    ![Route toevoegen](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. Voer onder **Route toevoegen** de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:

    |Instelling|Waarde|
    |---|---|
    |Routenaam|ToPrivateSubnet|
    |Adresvoorvoegsel| 10.0.1.0/24|
    |Volgend hoptype | Selecteer **Virtueel apparaat**.|
    |Adres van de volgende hop| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Voordat u een routetabel aan een subnet kunt koppelen, moet u een virtueel netwerk en subnet maken. Daarna kunt u de routetabel aan een subnet koppelen:

1. Selecteer **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Netwerken** en selecteer vervolgens **Virtueel netwerk**.
3. Voer onder **Virtueel netwerk maken** de volgende informatie in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer **Maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVirtualNetwork|
    |Adresruimte| 10.0.0.0/16|
    |Abonnement | Selecteer uw abonnement.|
    |Resourcegroep|Selecteer **Bestaande gebruiken** en selecteer **myResourceGroup**.|
    |Locatie|Selecteer *VS Oost*|
    |Subnetnaam|Openbaar|
    |Adresbereik|10.0.0.0/24|
    
4. Begin in het vak *Resources, services en documenten zoeken* bovenaan de portal **myVirtualNetwork** te typen. Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteert u dit.
5. Selecteer **Subnetten** onder **INSTELLINGEN** en selecteer vervolgens **+ Subnet**, zoals in de volgende afbeelding:

    ![Subnet toevoegen](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Selecteer de volgende informatie of voer deze in, en selecteer **OK**:

    |Instelling|Waarde|
    |---|---|
    |Naam|Privé|
    |Adresruimte| 10.0.1.0/24|

7. Voer stap 5 en 6 nogmaals uit, en geef de volgende gegevens op:

    |Instelling|Waarde|
    |---|---|
    |Naam|DMZ|
    |Adresruimte| 10.0.2.0/24|

8. Het vak **myVirtualNetwork - Subnetten** wordt weergegeven na het voltooien van de vorige stap. Onder **INSTELLINGEN** selecteert u **Subnetten** en vervolgens **Openbaar**.
9. Zoals in de volgende afbeelding wordt weergegeven, selecteert u **Routetabel**, daarna **MyRouteTablePublic** en vervolgens **Opslaan**:

    ![Routetabel koppelen](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>Een NVA maken

Een NVA is een VM die een netwerkfunctie uitvoert, zoals routering, firewall of WAN-optimalisatie.

1. Selecteer **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. U kunt een ander besturingssysteem selecteren, maar in de overige stappen wordt ervan uitgegaan dat u **Windows Server 2016 Datacenter** hebt geselecteerd. 
3. Selecteer de volgende informatie voor **Basis** of voer deze in, en selecteer **OK**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVmNva|
    |Gebruikersnaam|Voer een gebruikersnaam naar keuze in.|
    |Wachtwoord|Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Resourcegroep| Selecteer **Bestaande gebruiken** en selecteer *myResourceGroup*.|
    |Locatie|Selecteer **VS Oost**.|
4. Selecteer een VM-grootte onder **Kies een grootte**.
5. Selecteer de volgende informatie voor **Instellingen** of voer deze in, en selecteer **OK**:

    |Instelling|Waarde|
    |---|---|
    |Virtueel netwerk|myVirtualNetwork - als dit niet is geselecteerd, selecteert u **Virtueel netwerk** en vervolgens **myVirtualNetwork** onder **Virtueel netwerk kiezen**.|
    |Subnet|Selecteer **Subnet** en vervolgens**DMZ** onder **Subnet kiezen**. |
    |Openbaar IP-adres| Selecteer **Openbaar IP-adres** en selecteer **Geen** onder **Openbaar IP-adres kiezen**. Er wordt geen openbaar IP-adres aan deze VM toegewezen omdat er geen verbinding mee wordt gemaakt vanaf internet.
6. Selecteer onder **Maken** in het **Overzicht** **Maken** om de implementatie van de VM te starten.

    Het maken van de virtuele machine duurt een paar minuten. Ga niet door met de volgende stap voordat Azure klaar is met het maken van de VM en een vak opent met informatie over de VM.

7. In het vak dat voor de VM is geopend nadat deze is gemaakt, onder **INSTELLINGEN**, kiest u **Netwerken** en vervolgens **myvmnva158** (de netwerkinterface die Azure voor uw VM heeft gemaakt, heeft een ander getal na **myvmnva**), zoals in de volgende afbeelding wordt getoond:

    ![VM-netwerken](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Een netwerkinterface kan alleen netwerkverkeer doorsturen dat niet voor zijn eigen IP-adres bestemd is, als doorsturen via IP voor de netwerkinterface is ingeschakeld. Selecteer **IP-configuraties** onder **INSTELLINGEN**, selecteer **Ingeschakeld** bij **Doorsturen via IP**, en selecteer vervolgens **Opslaan** , zoals weergegeven in de volgende afbeelding:

    ![Doorsturen via IP inschakelen](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak twee VM's in het virtuele netwerk, zodat u in een latere stap kunt valideren dat verkeer van het *Openbare* subnet via de NVA wordt doorgestuurd naar het *Privé*-subnet. Voltooi stap 1-6 van [Een NVA maken](#create-a-network-virtual-appliance). Gebruik dezelfde instellingen in stap 3 en 5, maar met de volgende wijzigingen:

|Naam van de virtuele machine      |Subnet      | Openbaar IP-adres     |
|--------- | -----------|---------              |
| myVmPublic  | Openbaar     | Accepteer de standaardportal |
| myVmPrivate | Privé    | Accepteer de standaardportal |

U kunt de VM *myVmPrivate* maken terwijl Azure de VM *myVmPublic* maakt. Ga niet verder met de volgende stappen voordat Azure klaar is met het maken van beide VM's.

## <a name="route-traffic-through-an-nva"></a>Verkeer routeren via een NVA

1. Begin in het vak *Zoeken* bovenaan de portal *myVmPrivate* te typen. Wanneer de VM **myVmPrivate** wordt weergegeven in zoekresultaten, selecteert u deze.
2. Maak een externe bureaubladverbinding met *myVmPrivate* door **Verbinding maken** te kiezen, zoals in de volgende afbeelding:

    ![Verbinding maken met de virtuele machine ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Open het gedownloade RDP-bestand om verbinding te maken met de VM. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd.
4. Voer de gebruikersnaam en het wachtwoord in die u bij het maken van de VM hebt opgegeven (mogelijk moet u **Meer opties** en **Een ander account gebruiken** selecteren om de aanmeldingsgegevens op te geven die u hebt ingevoerd tijdens het maken van de virtuele machine) en selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Selecteer **Ja** om door te gaan met de verbinding.
6. In een latere stap wordt het hulpprogramma Route traceren gebruikt voor het testen van de routering. Route traceren maakt gebruik van ICMP (Internet Control Message Protocol), wat door Windows Firewall wordt geweigerd. Schakel ICMP via de Windows-firewall in met de volgende opdracht vanuit PowerShell op de VM *myVmPrivate*:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Hoewel Route traceren wordt gebruikt voor het testen van de routering in deze zelfstudie, wordt niet aanbevolen ICMP via Windows Firewall toe te staan voor productie-implementaties.
7. U hebt doorsturen via IP in Azure voor de netwerkinterface van de VM ingeschakeld in [Doorsturen via IP inschakelen](#enable-ip-forwarding). In de VM moet het besturingssysteem of een toepassing die wordt uitgevoerd op de virtuele machine, ook netwerkverkeer kunnen doorsturen. Schakel Doorsturen via IP in het besturingssysteem van de VM *myVmNva* in:

    Maak vanaf een opdrachtprompt op de VM *myVmPrivate* een externe bureaubladverbinding met de VM *myVmNva*:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Om Doorsturen via IP binnen het besturingssysteem in te schakelen, voert u de volgende opdracht in PowerShell in vanaf de VM *myVmNva* :

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Start *myVmNva* opnieuw op. Hierdoor wordt ook de extern-bureaubladsessie verbroken.
8. Maak terwijl u nog verbinding hebt met de VM *myVmPrivate* een extern-bureaubladsessie met de VM *myVmPublic* nadat de VM *myVmNva* opnieuw is opgestart:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    Schakel ICMP via de Windows-firewall in met de volgende opdracht vanuit PowerShell op de VM *myVmPublic*:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Om de routering van netwerkverkeer naar de VM *myVmPrivate* van de VM *myVmPublic* te testen, voert u op de VM *myVmPublic* de volgende opdracht van PowerShell uit:

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
      
    U ziet dat de eerste hop 10.0.2.4 is, het privé IP-adres van het NVA. De tweede hop is 10.0.1.4, het privé- IP-adres van de VM *myVmPrivate*. Door de route die is toegevoegd aan de routetabel *myRouteTablePublic* en gekoppeld aan het *Openbare* subnet leidt Azure het verkeer via het NVA in plaats van rechtstreeks naar het *Privé*-subnet.
10.  Sluit de externe bureaubladsessie met de VM *myVmPublic*. U houdt nog verbinding met de VM *myVmPrivate*.
11. Om de routering van netwerkverkeer naar de VM *myVmPublic* van de VM *myVmPrivate* VM te testen, voert u op de VM *myVmPrivate* de volgende opdracht uit vanaf een opdrachtprompt:

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

    U ziet dat verkeer rechtstreeks vanuit *myVmPrivate* naar *myVmPublic* wordt geleid. Standaard routeert Azure verkeer rechtstreeks tussen subnetten.
12. Sluit de externe bureaubladsessie naar de VM *myVmPrivate*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u deze niet langer nodig hebt, verwijdert u de resourcegroep en alle resources die deze bevat: 

1. Voer *myResourceGroup* in het vak **Zoeken** bovenaan de portal in. Wanneer u **myResourceGroup** ziet in de zoekresultaten, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* in voor **TYP DE RESOURCEGROEPNAAM:** en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een routetabel gemaakt en die aan een subnet gekoppeld. U hebt een eenvoudig NVA gemaakt dat verkeer van een openbaar subnet naar een privé-subnet heeft geleid. U kunt allerlei vooraf geconfigureerde NVA's die netwerkfuncties uitvoeren zoals firewalls en WAN-optimalisatie, implementeren vanuit [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Zie [Routeringoverzicht](virtual-networks-udr-overview.md) en [Routetabel beheren](manage-route-table.md) voor meer informatie over routeren.


Hoewel u veel Azure-resources binnen een virtueel netwerk kunt implementeren, kunnen resources voor sommige Azure PaaS-diensten niet in een virtueel netwerk worden geïmplementeerd. U kunt de toegang tot de resources van sommige Azure PaaS-diensten echter nog steeds beperken tot alleen verkeer vanaf een subnet van een virtueel netwerk. Ga verder met de volgende zelfstudie als u wilt leren hoe u de netwerktoegang kunt beperken tot Azure PaaS-resources.

> [!div class="nextstepaction"]
> [Netwerktoegang tot PaaS-resources beperken](tutorial-restrict-network-access-to-resources.md)
