---
title: 'Zelfstudie: Taakverdeling voor internetverkeer naar virtuele machines instellen - Azure Portal'
titlesuffix: Azure Load Balancer
description: In deze zelfstudie vindt u informatie over het maken en beheren van een Standard Load Balancer via Azure Portal.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create and Standard Load Balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: kumud
ms.custom: seodec18
ms.openlocfilehash: 78266e447d1ddf6daf5a9b0ad9172ab6470bf0c6
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57845202"
---
# <a name="tutorial-load-balance-internet-traffic-to-vms-using-the-azure-portal"></a>Zelfstudie: Taakverdeling voor internetverkeer naar virtuele machines instellen met behulp van Azure Portal

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines. In deze zelfstudie leert u meer over de verschillende onderdelen van Azure Standard Load Balancer die internetverkeer verdelen naar VM's en zorgen voor hoge beschikbaarheid. In deze zelfstudie leert u procedures om het volgende te doen:


> [!div class="checklist"]
> * Een Azure Load Balancer maken
> * Resources voor Load Balancer maken
> * Virtuele machines maken en IIS-server installeren
> * Weergave Load Balancer in actie
> * Toevoegen en verwijderen van virtuele machines uit een Load Balancer

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-standard-load-balancer"></a>Een Load Balancer van het type Standard maken

In deze sectie maakt u een Standard Load Balancer die helpt bij het laden van verdelen over virtuele machines. Standard Load Balancer biedt alleen ondersteuning voor een standaard, openbaar IP-adres. Wanneer u een Standard Load Balancer maakt, moet u ook een nieuw, standaard, openbaar IP-adres maken dat als de front-end (standaard *LoadBalancerFrontend* genoemd) wordt geconfigureerd voor de Standard Load Balancer. 

1. Klik linksboven in het scherm op **Een resource maken** > **Netwerken** > **Load balancer**.
2. Voer op het tabblad **Basis** van de pagina **Load balancer maken** de volgende gegevens in of selecteer deze, accepteer de standaardwaarden voor de overige instellingen en selecteer vervolgens **Controleren + maken**:

    | Instelling                 | Value                                              |
    | ---                     | ---                                                |
    | Abonnement               | Selecteer uw abonnement.    |    
    | Resourcegroep         | Selecteer **nieuw** en het type *myResourceGroupSLB* in het tekstvak in.|
    | Name                   | *myLoadBalancer*                                   |
    | Regio         | Selecteer **Europa - west**.                                        |
    | Type          | Selecteer **Openbaar**.                                        |
    | SKU           | selecteer **Standaard**.                          |
    | Openbaar IP-adres | Selecteer **Nieuw maken**. |
    | Naam openbaar IP-adres              | Typ *myPublicIP* in het tekstvak.   |
    |Beschikbaarheidszone| Selecteer **Zone-redundant**.    |

3. Klik op het tabblad **Beoordelen en maken** op **Maken**.

   ![Een Load Balancer van het type Standard maken](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Resources voor Load Balancer maken

In deze sectie maakt u Load Balancer configureren voor een back-endadresgroep, een statustest en een balancer-regel opgeven.

### <a name="create-a-backend-address-pool"></a>Een back-endadresgroep maken

Voor het distribueren van verkeer naar de virtuele machines, een back-end-adresgroep bevat de IP-adressen van de virtuele (NIC's) die zijn verbonden met de Load Balancer. Maak de back-endadresgroep *myBackendPool* om op te nemen van virtuele machines voor taakverdeling internetverkeer.

1. Selecteer **alle services** Selecteer in het menu links **alle resources**, en klik vervolgens op **myLoadBalancer** in de lijst met resources.
2. Klik onder **Instellingen** op **Back-endpools** en vervolgens op **Toevoegen**.
3. Op de **toevoegen van een back-endpool** pagina voor de naam, type *myBackendPool*, als de naam voor uw back-endpool en selecteer vervolgens **toevoegen**.

### <a name="create-a-health-probe"></a>Een statustest maken

Als u wilt toestaan dat de Load Balancer voor het bewaken van de status van uw app, gebruikt u een statustest. De statustest wordt dynamisch toegevoegd of verwijderd van virtuele machines uit de Load Balancer-rotatie op basis van hun reactie op statuscontroles. Maak een statustest (*myHealthProbe*) om de status van de VM's te bewaken.

1. Selecteer **alle services** Selecteer in het menu links **alle resources**, en klik vervolgens op **myLoadBalancer** in de lijst met resources.
2. Klik onder **Instellingen** op **Tests** en klik op **Toevoegen**.
3. Gebruik deze waarden om de statustest te maken:
     
    | Instelling | Waarde |
    | ------- | ----- |
    | Name | Enter *myHealthProbe*. |
    | Protocol | Selecteer **HTTP**. |
    | Poort | Voer *80*.|
    | Interval | Voer *15* voor aantal **Interval** in seconden tussen tests. |
    | Drempelwaarde voor onjuiste status | Selecteer *2* voor aantal **drempelwaarde voor onjuiste status** of opeenvolgende testfouten dat optreden moeten voordat een virtuele machine als slecht beschouwd.|
    | Test | Select *myHealthProbe*. |
    
4. Selecteer **OK**.

### <a name="create-a-load-balancer-rule"></a>Een load balancer-regel maken

Een load balancer-regel wordt gebruikt om de verdeling van het verkeer over de VM's te definiëren. U definieert de front-end-IP-configuratie voor het inkomende verkeer en de back-end-IP-groep om het verkeer te ontvangen, samen met de gewenste bron- en doelpoort. Maak een Load Balancer-regel *myLoadBalancerRuleWeb* voor het luisteren naar poort 80 in de front-end *FrontendLoadBalancer* en het verzenden van netwerkverkeer met load balancing naar de back-endadresgroep *myBackEndPool* ook van poort 80.

1. Selecteer **alle services** Selecteer in het menu links **alle resources**, en klik vervolgens op **myLoadBalancer** in de lijst met resources.
2. Klik onder **Instellingen** op **Taakverdelingsregels** en vervolgens op **Toevoegen**.
3. Gebruik deze waarden om de regel load balancing te configureren:

    | Instelling | Waarde |
    | ------- | ----- |
    | Name | Voer *myHTTPRule*. |
    | Protocol | selecteer **TCP**. |
    | Poort | Voer *80*.|
    | Back-endpoort | Voer *80*. |
    | Back-endpool | Select *myBackendPool*.|
    | Test | Select *myHealthProbe*. |
    
4. Laat de overige standaardwaarden staan en selecteer **OK**.

## <a name="create-backend-servers"></a>Back-endservers maken

In deze sectie maakt u een virtueel netwerk maken, drie virtuele machines voor de back-endpool van de Load Balancer maken en installeert u IIS op de virtuele machines om de Load Balancer te testen.

### <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer linksboven in het scherm **Een resource maken** > **Netwerken** > **Virtueel netwerk**.
2. Typ of selecteer in **Virtueel netwerk maken** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Name | Voer *myVNet* in. |
    | Adresruimte | Voer *10.1.0.0/16* in. |
    | Abonnement | Selecteer uw abonnement.|
    | Resourcegroep | Select existing resource - *myResourceGroupSLB*. |
    | Locatie | Selecteer **Europa - west**.|
    | Subnet - Naam | Voer *myBackendSubnet* in. |
    | Subnet - adresbereik | Voer *10.1.0.0/24* in. |
    
3. Laat de overige standaardwaarden staan en selecteer **Maken**.

### <a name="create-virtual-machines"></a>Virtuele machines maken

Standard Load Balancer biedt alleen ondersteuning voor VM's met standaard IP-adressen in de back-endpool. In deze sectie maakt u drie virtuele machines (*myVM1*, *myVM2*, en *myVM3*) met een Standard openbaar IP-adres in drie verschillende zones (*Zone 1*, *Zone 2*, en *Zone 3*) die zijn toegevoegd aan de back-endpool van de Standard Load Balancer die eerder is gemaakt.

1. Selecteer **Een resource maken** > **Compute** > **Windows Server 2016 Datacenter** linksboven in de portal. 
   
1. In **Een virtuele machine maken** typt of selecteert u de volgende waarden op het tabblad **Basisinformatie**:
   - **Abonnement** > **Resourcegroep**: Select **myResourceGroupSLB**.
   - **Instantiedetails** > **Naam van virtuele machine**: Type *myVM1*.
   - **Details van exemplaar** > **regio** > Selecteer **West-Europa**.
   - **Details van exemplaar** > **Beschikbaarheidsopties** > Selecteer **beschikbaarheidszones**. 
   - **Details van exemplaar** > **binnen een beschikbaarheidszone** > Selecteer **1**.
  
1. Selecteer het tabblad **Netwerken** of selecteer **Volgende: Schijven** en vervolgens **Volgende: Netwerken**. 
   
   - Zorg ervoor dat de volgende opties zijn geselecteerd:
       - **Virtueel netwerk**: **myVnet**
       - **Subnet**: **myBackendSubnet**
       - **Openbaar IP-adres** > Selecteer **nieuw**, en klik in de **openbare IP-adres maken** venster voor **SKU**, selecteer **Standard**, en voor **binnen een beschikbaarheidszone**, selecteer **Zone-redundante**
      
   - Als u een nieuwe netwerkbeveiligingsgroep (NSG) wilt maken, een soort firewall, selecteert u onder **Netwerkbeveiligingsgroep** de optie **Geavanceerd**. 
       1. Selecteer in het veld **Netwerkbeveiligingsgroep configureren** de optie **Nieuwe maken**. 
       1. Type *myNetworkSecurityGroup*, en selecteer **OK**.

   - Als u de virtuele machine een deel van de Load Balancer back-endpool, voert u de volgende stappen uit:
        - In **Load Balancing**, voor **plaatsen van deze virtuele machine achter een bestaande oplossing voor taakverdeling?**, selecteer **Ja**.
        - In **Load balancing-instellingen**, voor **opties voor taakverdeling**, selecteer **Azure load balancer**.
        - Voor **selecteert u een load balancer**, *myLoadBalancer*. 
1. Selecteer het tabblad **Beheer** of selecteer **Volgende** > **Beheer**. Stel bij **Bewaking** **Diagnostische gegevens over opstarten** in op **Uit**. 
1. Selecteer **Controleren + maken**.   
1. Controleer de instellingen en selecteer vervolgens **Maken**.
1. Volg de stappen voor het maken van twee extra virtuele machines - *myVM2* en *myVM3*, met een standaard-SKU openbaar IP-adres in **binnen een beschikbaarheidszone** **2** en **3** respectievelijk, en alle andere instellingen hetzelfde zijn als *myVM1*.  

### <a name="create-network-security-group-rule"></a>Regel voor netwerkbeveiligingsgroep maken

In deze sectie maakt u een regel voor de netwerkbeveiligingsgroep voor binnenkomende verbindingen via HTTP.

1. Selecteer **alle services** Selecteer in het menu links **alle resources**, en klik in de lijst met resources vervolgens op **myNetworkSecurityGroup** die bevindt zich in de **myResourceGroupSLB** resourcegroep.
2. Klik onder **Instellingen** op **Inkomende beveiligingsregels** en vervolgens op **Toevoegen**.
3. Voer deze waarden in voor de inkomende beveiligingsregel met de naam *myHTTPRule* om een binnenkomende HTTP-verbinding via poort 80 toe te staan:
    - *Service Tag* bij **Bron**.
    - *Internet* bij **Bronservicetag**
    - *80* bij **Poortbereiken van doel**
    - *TCP* bij **Protocol**
    - *Allow* bij **Actie**
    - *100* bij **Prioriteit**
    - *myHTTPRule* als naam
    - *Allow HTTP* als beschrijving
4. Selecteer **Toevoegen**.

### <a name="install-iis-on-vms"></a>IIS installeren op VM's

1. Selecteer **alle services** Selecteer in het menu links **alle resources**, en klik in de lijst met resources vervolgens op **myVM1** die bevindt zich in de  *myResourceGroupSLB* resourcegroep.
2. Klik op de pagina **Overzicht** op **Verbinding maken** om extern verbinding te maken met de VM.
3. Selecteer in het pop-upvenster **Verbinding maken met virtuele machine** **RDP-bestand downloaden**, en open vervolgens het gedownloade RDP-bestand.
4. In het venster **Verbinding met extern bureaublad** klikt u op **Verbinden**.
5. Meld u aan bij de virtuele machine met de referenties die u hebt opgegeven tijdens het maken van deze virtuele machine. Hiermee wordt een sessie met extern bureaublad met virtuele machine *myVM1* gestart.
6. Ga op de serverdesktop naar **Windows Systeembeheer**>**Windows Powershell**.
7. Voer in het venster PowerShell de volgende opdrachten uit om de IIS-server te installeren, het standaardbestand iisstart.htm te verwijderen en een nieuw bestand iisstart.htm toe te voegen dat de naam van de VM weergeeft:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. Sluit de RDP-sessie met *myVM1*.
7. Herhaal stappen 1 tot en met 6 om IIS en het bijgewerkte bestand iisstart.htm te installeren op *myVM2* en *myVM3*.

## <a name="test-the-load-balancer"></a>Test de Load Balancer
1. Zoek op het scherm **Overzicht** het openbare IP-adres voor de load balancer. Selecteer **alle services** Selecteer in het menu links **alle resources**, en klik vervolgens op **myPublicIP**.

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De standaardpagina van IIS-webserver wordt weergegeven in de browser.

      ![IIS-webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Als u wilt zien hoe de Load Balancer verkeer distribueert naar de drie virtuele machines waarop uw app wordt uitgevoerd, u kunt forceren vernieuwing van uw webbrowser.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Virtuele machines toevoegen aan of verwijderen uit de back-endpool
Het is mogelijk dat u onderhoud moet uitvoeren op de VM's waarop uw app wordt uitgevoerd, zoals het installeren van besturingssysteemupdates. U moet mogelijk extra VM's toevoegen vanwege toegenomen verkeer naar uw app. Deze sectie leest u hoe u kunt verwijderen of toevoegen van een virtuele machine (*myVM1*) van de Load Balancer.

### <a name="remove-vm-from-a-backend-pool"></a>Virtuele machine verwijderen uit een back-endpool
Verwijderen van *myVM1* van de back-endpool, de volgende stappen uitvoeren:

1. Selecteer **alle services** Selecteer in het menu links **alle resources**, en klik vervolgens op **myLoadBalancer** in de lijst met resources.
2. Klik onder **Instellingen** op **Back-endpools**. Klik vervolgens in de lijst van de back-endpool op **myBackendPool**.
3. Op de **myBackendPool** pagina verwijderen *VM1* Selecteer het verwijderpictogram aan het einde van de rij waarin *myVM1*, en klik vervolgens op **opslaan**.

Nu *myVM1* niet meer in de back-endadresgroep zit, kunt u onderhoudstaken uitvoeren op *myVM1*, zoals het installeren van software-updates. In de afwezigheid van *VM1*, de belasting is nu verdeeld over *myVM2* en *myVM3*. 

### <a name="add-vm-to-a-backend-pool"></a>Virtuele machine toevoegen aan een back-endpool
Om toe te voegen *myVM1* terug naar de back-endpool, de volgende stappen uitvoeren:

1. Selecteer **alle services** Selecteer in het menu links **alle resources**, en selecteer vervolgens **myVM1** in de lijst met resources.
2. In de **VM1** pagina onder **instellingen**, selecteer **netwerken**.
3. In de **netwerken** weergeeft, schakelt de **taakverdeling** tabblad, en selecteer vervolgens **toevoegen taakverdeling**.
4. In de **toevoegen taakverdeling** pagina, de volgende handelingen uit:
   1. Voor **opties voor taakverdeling**, selecteer **Azure load balancer**.
   2. Voor **selecteert u een load balancer**, selecteer *myLoadBalancer*.
   3. Voor **selecteert u een back-endpool**, selecteer *myBackendPool*. 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer ze niet meer nodig hebt zijn, verwijdert u de resourcegroep, Load Balancer en alle gerelateerde resources. Om dit te doen, selecteert u de *myResouceGroupSLB* resourcegroep die de Load Balancer bevat, en selecteer vervolgens **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Standard Load Balancer gemaakt, VM's die zijn gekoppeld aan het statustest van de Load Balancer verkeersregel geconfigureerd en vervolgens de Load Balancer getest. U hebt ook een VM verwijderd uit de set load balancers en deze weer toegevoegd aan de back-endadresgroep. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
