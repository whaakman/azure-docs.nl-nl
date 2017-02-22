---
title: Een site-naar-site-VPN-verbinding maken tussen twee virtuele netwerken in verschillende Azure Stack POC-omgevingen | Microsoft Docs
description: "Stapsgewijze procedure waarmee een cloudbeheerder een site-naar-site-VPN-verbinding kan maken tussen twee POC-omgevingen met één knooppunt in TP2."
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 3f1b4e02-dbab-46a3-8e11-a777722120ec
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: scottnap
translationtype: Human Translation
ms.sourcegitcommit: 5104c7996de9dc0597e65be31c28a9aaa1243a90
ms.openlocfilehash: d324290caf5b5a085a2daf67e541c295dffda732


---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-poc-environments"></a>Een site-naar-site-VPN-verbinding maken tussen twee virtuele netwerken in verschillende Azure Stack POC-omgevingen
## <a name="overview"></a>Overzicht
In dit artikel wordt getoond hoe u een site-naar-site-VPN-verbinding kunt maken tussen twee virtuele netwerken in twee afzonderlijke Azure Stack POC-omgevingen (Proof-of-Concept). Tijdens het configureren van de verbindingen leert u hoe de VPN-gateways in Azure Stack werken.

> [!NOTE]
> Dit document geldt specifiek voor het Azure Stack TP2-POC.
> 
> 

### <a name="connection-diagram"></a>Verbindingsdiagram
In het volgende diagram wordt weergegeven hoe de configuratie eruit moet zien als u klaar bent:

![](media/azure-stack-create-vpn-connection-one-node-tp2/image1.png)

### <a name="before-you-begin"></a>Voordat u begint
Voordat u aan de slag gaat, moet u ervoor zorgen dat u de volgende items hebt om deze configuratie te voltooien:

* Twee servers die voldoen aan de hardwarevereisten van het Azure Stack POC zoals gedefinieerd in [Vereisten voor Azure Stack-implementatie](azure-stack-deploy.md) en de andere vereisten die worden gedefinieerd in dit document.
* Het Azure Stack Technical Preview 2-implementatiepakket.

## <a name="deploy-the-poc-environments"></a>De POC-omgevingen implementeren
U gaat twee Azure Stack POC-omgevingen implementeren om deze configuratie te voltooien.

* Voor elk POC dat u implementeert, kunt u de implementatie-instructies volgen die uiteen worden gezet in het artikel [Azure Stack-POC implementeren](azure-stack-run-powershell-script.md).
  Naar elke POC-omgeving in dit document wordt verwezen als *POC1* of *POC2*.

## <a name="configure-quotas-for-compute-network-and-storage"></a>Quota configureren voor berekening, netwerk en opslag
Eerst moet u *Quota* configureren voor berekening, netwerk en opslag. Deze services kunnen worden gekoppeld aan een *Plan* en vervolgens aan een *Aanbieding* waarop tenants kunnen worden geabonneerd.

> [!NOTE]
> U moet deze stappen uitvoeren voor elke Azure Stack POC-omgeving.
> 
> 

Quota maken voor services is gewijzigd ten opzichte van TP1. De stappen voor het maken van quota in TP2 vindt u op <http://aka.ms/mas-create-quotas>. U kunt de standaardwaarden voor alle quota-instellingen voor deze taak accepteren.

## <a name="create-a-plan-and-offer"></a>Een plan en aanbieding maken
[Plannen](azure-stack-key-features.md) zijn pakketten van één of meer services. Als provider kunt u plannen maken die u aan uw tenants kunt aanbieden. Tenants abonneren zich op hun beurt op uw aanbiedingen om de plannen en bijbehorende services te gebruiken.

> [!NOTE]
> U moet deze stappen uitvoeren voor elke Azure Stack POC-omgeving.
> 
> 

1. Maak eerst een plan. U kunt hiervoor de stappen volgen in het onlineartikel [Een plan maken](azure-stack-create-plan.md).
2. Maak een aanbieding met behulp van de stappen die worden beschreven in [Een aanbieding maken in Azure Stack](azure-stack-create-offer.md).
3. Meld u als een tenantbeheerder aan bij de portal en [abonneer u op de aanbieding die u hebt gemaakt](azure-stack-subscribe-plan-provision-vm.md).

## <a name="create-the-network-resources-in-poc-1"></a>De netwerkresources in POC 1 maken
Nu gaat u daadwerkelijk de resources maken die u nodig hebt voor de configuratie. In de volgende stappen wordt geïllustreerd wat u gaat doen. In deze instructies wordt uitgelegd hoe u resources kunt maken via de portal. Hetzelfde kan echter ook worden gerealiseerd met behulp van PowerShell.

![](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="log-in-as-a-tenant"></a>Aanmelden als tenant
Een servicebeheerder kan zich aanmelden als tenant om de plannen, aanbiedingen en abonnementen te testen die zijn of haar tenants mogelijk gebruiken. Als u nog geen account hebt, kunt u [een tenantaccount maken](azure-stack-add-new-user-aad.md) voordat u zich aanmeldt.

### <a name="create-the-virtual-network--vm-subnet"></a>Het virtuele netwerk en VM-subnet maken
1. Meld u aan met een tenantaccount.
2. Klik in Azure Portal op **Nieuw**.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)
3. Selecteer **Netwerken** in het menu Marketplace.
4. Klik in het menu op het item **Virtueel netwerk**.
5. Klik aan de onderkant van de blade met de resourcebeschrijving op **Maken**. Voer op basis van deze tabel de volgende waarden in de juiste velden in.
   
   | **Veld** | **Waarde** |
   | --- | --- |
   | Naam |vnet-01 |
   | Adresruimte |10.0.10.0/23 |
   | Subnetnaam |subnet-01 |
   | Subnetadresbereik |10.0.10.0/24 |
6. In het veld **Abonnement** wordt nu het abonnement weergegeven dat u eerder hebt ingevuld.
7. Voor de resourcegroep kunt u een nieuwe resourcegroep maken of **Bestaande gebruiken** selecteren als u al een resourcegroep hebt.
8. Controleer de standaardlocatie.
9. Klik op **Create**.

### <a name="create-the-gateway-subnet"></a>Het gatewaysubnet maken
1. Open op het dashboard de VM-resource die u zojuist hebt gemaakt (Vnet-01).
2. Selecteer op de blade Instellingen de optie **Subnetten**.
3. Klik op **Gatewaysubnet** om een gatewaysubnet toe te voegen aan het virtuele netwerk.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)
4. De naam van het subnet is standaard ingesteld op **Gatewaysubnet**.
   Gatewaysubnetten zijn speciaal en hebben deze specifieke naam nodig om goed te functioneren.
5. Typ **10.0.11.0/24** in het veld **Adresbereik**.
6. Klik op **Maken** om het gatewaysubnet te maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken
1. Klik in Azure Portal op **Nieuw**.
   
2. Selecteer **Netwerken** in het menu Marketplace.
3. Selecteer **Gateway van het virtuele netwerk** in de lijst met netwerkresources.
4. Controleer de beschrijving en klik op **Maken**.
5. Typ **GW1** in het veld **Naam**.
6. Klik op het item **Virtueel netwerk** om een virtueel netwerk te kiezen.
   Selecteer **Vnet-01** in de lijst.
7. Klik op het menu-item **Openbaar IP-adres**. Klik in de geopende blade **Openbaar IP-adres kiezen** op **Nieuwe maken**.
8. Typ **GW1-PiP** in het veld **Naam** en klik op **OK**.
9. Bij **Gatewaytype** moet standaard **VPN** zijn geselecteerd. Behoud deze instelling.
10. Bij **VPN-type** moet standaard **Op basis van route** zijn geselecteerd.
    Behoud deze instelling.
11. Controleer of **Abonnement** en **Locatie** juist zijn. Als u wilt, kunt u de resource vastmaken aan het dashboard. Klik op **Create**.

### <a name="create-the-local-network-gateway"></a>De lokale netwerkgateway maken
De implementatie van een *lokale netwerkgateway* in deze implementatie van een Azure Stack-evaluatie verschilt iets van een werkelijke Azure-implementatie.

Net als in Azure is er het concept van een lokale netwerkgateway. In een Azure-implementatie vertegenwoordigt een lokale netwerkgateway echter een on-premises fysiek apparaat (in de tenant) dat u gebruikt om verbinding te maken met een virtuele netwerkgateway in Azure. Maar in deze implementatie van een Azure Stack-evaluatie zijn beide kanten van de verbinding virtuele netwerkgateways.

Een meer algemene manier om dit te beschrijven is dat de lokale netwerkgatewayresource altijd is bedoeld als indicatie van de externe gateway aan de andere kant van de verbinding. Vanwege de manier waarop het POC is ontworpen, moet u het IP-adres van de externe netwerkadapter op de virtuele NAT-machine van het andere POC opgeven als het openbare IP-adres van de lokale netwerkgateway. Vervolgens maakt u NAT-toewijzingen op de virtuele NAT-machine om ervoor te zorgen dat beide kanten juist zijn verbonden.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Het IP-adres ophalen van de externe adapter van de virtuele NAT-machine
1. Meld u aan bij de fysieke computer van Azure Stack voor POC2.
2. Druk op de [Windows-toets] + R om het menu **Uitvoeren** te openen. Typ **mstsc** en druk op Enter.
3. Typ in het veld **Computer** de naam **MAS-BGPNAT01** en klik op  **Verbinden**.
4. Klik op het menu Start, klik met de rechtermuisknop op **Windows PowerShell** en selecteer **Als administrator uitvoeren**.
5. Typ **ipconfig /all**.
6. Zoek de ethernetadapter die is verbonden met het on-premises netwerk, en noteer het IPv4-adres dat bij deze adapter hoort. In de voorbeeldomgeving is dit **10.16.167.195**, maar u hebt een ander adres.
7. Noteer dit adres. Dit adres gaat u gebruiken als het openbare IP-adres van de lokale netwerkgatewayresource die u hebt gemaakt in POC1.

### <a name="create-the-local-network-gateway-resource"></a>De lokale netwerkgatewayresource maken
1. Meld u aan bij de fysieke computer van Azure Stack voor POC1.
2. Typ in het veld **Computer** de naam **MAS-CON01** en klik op **Verbinden**.
3. Klik in Azure Portal op **Nieuw**.
   
4. Selecteer **Netwerken** in het menu Marketplace.
5. Selecteer **lokale netwerkgateway** in de lijst met resources.
6. Typ **POC2-GW** in het veld **Naam**.
7. U kent het IP-adres van de andere gateway nog niet. Dit is niet erg want u kunt later terugkomen om dit adres te wijzigen. Typ voorlopig **10.16.167.195** in de veld **IP-adres**.
8. Typ in het veld **Adresruimte** de adresruimte van de Vnet die u gaat maken in POC2. Dit wordt **10.0.20.0/23**. Typ dus deze waarde.
9. Controleer of **Abonnement**, **Resourcegroep** en **Locatie** juist zijn, en klik op **Maken**.

### <a name="create-the-connection"></a>De verbinding maken
1. Klik in Azure Portal op **Nieuw**.
   
2. Selecteer **Netwerken** in het menu Marketplace.
3. Selecteer **Verbinding** in de lijst met resources.
4. Kies op de blade **Basisinstellingen** de optie **Site-naar-site (IPSec)** als het **Verbindingstype**.
5. Selecteer **Abonnement**, **Resourcegroep** en **Locatie**, en klik op **OK**.
6. Kies op de blade **Instellingen** de **virtuele netwerkgateway** (**GW1**) die u eerder hebt gemaakt.
7. Kies de **lokale netwerkgateway** (**POC2 GW**) die u eerder hebt gemaakt.
8. Typ **POC1-POC2** in het veld **Verbindingsnaam**.
9. Typ **12345** in het veld **Gedeelde sleutel (PSK)** en klik op **OK**.

### <a name="create-a-vm"></a>Een virtuele machine maken
Als u gegevens wilt valideren die via de VPN-verbinding worden verzonden, hebt u in elk POC virtuele machines nodig om gegevens te verzenden en te ontvangen. Maak nu een virtuele machine in POC1 en koppel deze aan het VM-subnet in uw virtuele netwerk.

1. Klik in Azure Portal op **Nieuw**.
   
2. Selecteer **Virtuele machines** in het menu Marketplace.
3. Selecteer in de lijst met installatiekopieën van virtuele machines de installatiekopie **Windows Server 2012 R2 Datacenter**.
4. Typ **VM01** in het veld **Naam** van de blade **Basisbeginselen**.
5. Typ een geldige gebruikersnaam en een geldig wachtwoord. U gaat dit account gebruiken om u aan te melden bij de virtuele machine nadat deze is gemaakt.
6. Geef een **Abonnement**, **Resourcegroep** en **Locatie** op, en klik op **OK**.
7. Kies op de blade **Grootte** een VM-grootte voor dit exemplaar en klik vervolgens op **Selecteren**.
8. Op de blade **Instellingen** kunt u de standaardwaarden accepteren. Zorg er wel voor dat **VNET-01** is geselecteerd als het virtuele netwerk en dat het subnet is ingesteld op **10.0.10.0/24**. Klik op **OK**.
9. Controleer de instellingen op de blade **Samenvatting** en klik op **OK**.

## <a name="create-the-network-resources-in-poc-2"></a>De netwerkresources in POC 2 maken
### <a name="log-in-as-a-tenant"></a>Aanmelden als tenant
Een servicebeheerder kan zich aanmelden als tenant om de plannen, aanbiedingen en abonnementen te testen die zijn of haar tenants mogelijk gebruiken. Als u nog geen account hebt, kunt u [een tenantaccount maken](azure-stack-add-new-user-aad.md) voordat u zich aanmeldt.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Het virtuele netwerk en VM-subnet maken
1. Meld u aan met een tenantaccount.
2. Klik in Azure Portal op **Nieuw**.
   
3. Selecteer **Netwerken** in het menu Marketplace.
4. Klik in het menu op **Virtueel netwerk**.
5. Klik aan de onderkant van de blade met de resourcebeschrijving op **Maken**. Typ de volgende waarden voor de juiste velden van de onderstaande tabel.
   
   | **Veld** | **Waarde** |
   | --- | --- |
   | Naam |vnet-02 |
   | Adresruimte |10.0.20.0/23 |
   | Subnetnaam |subnet-02 |
   | Subnetadresbereik |10.0.20.0/24 |
6. In het veld **Abonnement** wordt nu het abonnement weergegeven dat u eerder hebt ingevuld.
7. Voor de resourcegroep kunt u een nieuwe resourcegroep maken of **Bestaande gebruiken** selecteren als u al een resourcegroep hebt.
8. Controleer de **standaardlocatie**. Als u wilt, kunt u het virtuele netwerk vastmaken aan het dashboard zodat u het gemakkelijk kunt openen.
9. Klik op **Create**.

### <a name="create-the-gateway-subnet"></a>Het gatewaysubnet maken
1. Open op het dashboard de VM-resource die u zojuist hebt gemaakt (**Vnet-02**).
2. Selecteer op de blade **Instellingen** de optie **Subnetten**.
3. Klik op **Gatewaysubnet** om een gatewaysubnet toe te voegen aan het virtuele netwerk.
   
4. De naam van het subnet is standaard ingesteld op **Gatewaysubnet**.
   Gatewaysubnetten zijn speciaal en hebben deze specifieke naam nodig om goed te functioneren.
5. Typ **10.0.20.0/24** in het veld **Adresbereik**.
6. Klik op **Maken** om het gatewaysubnet te maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken
1. Klik in Azure Portal op **Nieuw**.
   
2. Selecteer **Netwerken** in het menu Marketplace.
3. Selecteer **Gateway van het virtuele netwerk** in de lijst met netwerkresources.
4. Controleer de beschrijving en klik op **Maken**.
5. Typ **GW2** in het veld **Naam**.
6. Klik op **Virtueel netwerk** om een virtueel netwerk te kiezen.
   Selecteer **Vnet-02** in de lijst.
7. Klik op **Openbaar IP-adres**. Klik in de geopende blade **Openbaar IP-adres kiezen** op **Nieuwe maken**.
8. Typ **GW2-PiP** in het veld **Naam** en klik op **OK**.
9. Bij **Gatewaytype** moet standaard **VPN** zijn geselecteerd. Behoud deze instelling.
10. Bij **VPN-type** moet standaard **Op basis van route** zijn geselecteerd.
    Behoud deze instelling.
11. Controleer of **Abonnement** en **Locatie** juist zijn. Als u wilt, kunt u de resource vastmaken aan het dashboard. Klik op **Create**.

### <a name="create-the-local-network-gateway"></a>De lokale netwerkgateway maken
#### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Het IP-adres ophalen van de externe adapter van de virtuele NAT-machine
1. Meld u aan bij de fysieke computer van Azure Stack voor POC1.
2. Houd [Windows-toets] + R ingedrukt om het menu **Uitvoeren** te openen. Typ **mstsc** en druk op Enter.
3. Typ in het veld **Computer** de naam **MAS-BGPNAT01** en klik op  **Verbinden**.
4. Klik op het menu Start, klik met de rechtermuisknop op **Windows PowerShell** en selecteer **Als administrator uitvoeren**.
5. Typ **ipconfig /all**.
6. Zoek de ethernetadapter die is verbonden met het on-premises netwerk, en noteer het IPv4-adres dat bij deze adapter hoort. In de voorbeeldomgeving is dit **10.16.169.131**, maar u hebt een ander adres.
7. Noteer dit adres. Dit adres gebruikt u later als het openbare IP-adres van de lokale netwerkgatewayresource die u hebt gemaakt in POC1.

#### <a name="create-the-local-network-gateway-resource"></a>De lokale netwerkgatewayresource maken
1. Meld u aan bij de fysieke computer van Azure Stack voor POC2.
2. Typ in het veld **Computer** de naam **MAS-CON01** en klik op **Verbinden**.
3. Klik in Azure Portal op **Nieuw**.
   
4. Selecteer **Netwerken** in het menu Marketplace.
5. Selecteer **lokale netwerkgateway** in de lijst met resources.
6. Typ **POC1-GW** in het veld **Naam**.
7. U hebt nu het openbare IP-adres nodig dat u hebt genoteerd voor de virtuele netwerkgateway in POC1. Typ **10.16.169.131** in de veld **IP-adres**.
8. In de **adresruimte** veld typt u de adresruimte van **Vnet-01** van POC1 - **10.0.0.0/16**.
9. Controleer of **Abonnement**, **Resourcegroep** en **Locatie** juist zijn, en klik op **Maken**.

## <a name="create-the-connection"></a>De verbinding maken
1. Klik in Azure Portal op **Nieuw**.
   
2. Selecteer **Netwerken** in het menu Marketplace.
3. Selecteer **Verbinding** in de lijst met resources.
4. Kies op de blade **Basisinstellingen** de optie **Site-naar-site (IPSec)** als het **Verbindingstype**.
5. Selecteer **Abonnement**, **Resourcegroep** en **Locatie**, en klik op **OK**.
6. Kies op de blade **Instellingen** de **virtuele netwerkgateway** (**GW1**) die u eerder hebt gemaakt.
7. Kies de **lokale netwerkgateway** (**POC1 GW**) die u eerder hebt gemaakt.
8. Typ **POC2-POC1** in het veld **Verbindingsnaam**.
9. Typ **12345** in het veld **Gedeelde sleutel (PSK)**. Let op: als u een andere waarde kiest, MOET deze overeenkomen met de waarde voor de gedeelde sleutel die u hebt gemaakt in POC1. Klik op **OK**.

## <a name="create-a-vm"></a>Een virtuele machine maken
Maak nu een virtuele machine in POC1 en koppel deze aan het VM-subnet in uw virtuele netwerk.

1. Klik in Azure Portal op **Nieuw**.
   
2. Selecteer **Virtuele machines** in het menu Marketplace.
3. Selecteer in de lijst met installatiekopieën van virtuele machines de installatiekopie **Windows Server 2012 R2 Datacenter**.
4. Typ **VM02** in het veld **Naam** van de blade **Basisbeginselen**.
5. Typ een geldige gebruikersnaam en een geldig wachtwoord. U gaat dit account gebruiken om u aan te melden bij de virtuele machine nadat deze is gemaakt.
6. Geef een **Abonnement**, **Resourcegroep** en **Locatie** op, en klik op **OK**.
7. Kies op de blade **Grootte** een VM-grootte voor dit exemplaar en klik vervolgens op **Selecteren**.
8. Op de blade Instellingen kunt u de standaardwaarden accepteren. Zorg er wel voor dat **VNET-02** is geselecteerd als het virtuele netwerk en dat het subnet is ingesteld op **20.0.0.0/24**. Klik op **OK**.
9. Controleer de instellingen op de blade **Samenvatting** en klik op **OK**.

## <a name="configure-the-nat-vm-in-each-poc-for-gateway-traversal"></a>De virtuele NAT-machine configureren in elk POC voor gateway-traversal
Omdat het POC zelfstandig en geïsoleerd is ontworpen van het netwerk waarop de fysieke host is geïmplementeerd, is het ´externe´ VIP-netwerk waarmee de gateways zijn verbonden, niet daadwerkelijk extern. Dit netwerk is in plaats hiervan verborgen achter een router waarop NAT (Network Address Translation) wordt uitgevoerd. De router is eigenlijk een virtuele Windows Server-machine (**MAS-BGPNAT01**) waarop de RRAS-rol (Routing and Remote Access Services) wordt uitgevoerd in de POC-infrastructuur. U moet NAT configureren op de virtuele MAS-BGPNAT01-machine om ervoor te zorgen dat de site-naar-site-VPN-verbinding aan beide kanten is verbonden. Hiervoor moet u een statische NAT-toewijzing maken die de externe interface op de BGPNAT-VM toewijst aan de VIP van de beveiligde gatewaygroep voor de poorten die zijn vereist voor een VPN-verbinding.

> [!NOTE]
> Deze configuratie is alleen vereist voor POC-omgevingen.
> 
> 

### <a name="configure-nat"></a>NAT configureren
U moet deze stappen volgen in BEIDE POC-omgevingen.

1. Meld u aan bij de fysieke computer van Azure Stack voor POC1.
2. Houd [Windows-toets] + R ingedrukt om het menu **Uitvoeren** te openen. Typ **mstsc** en druk op **Enter**.
3. Typ in het veld **Computer** de naam **MAS-BGPNAT01** en klik op **Verbinden**.
4. Klik op het menu Start Menu, klik met de rechtermuisknop op **Windows PowerShell** en selecteer **Als administrator uitvoeren**.
5. Typ **ipconfig /all**.
6. Zoek de ethernetadapter die is verbonden met het on-premises netwerk, en noteer het IPv4-adres dat bij deze adapter hoort. In de voorbeeldomgeving is dit **10.16.169.131** (hieronder met rood omcirkeld), maar u hebt een ander adres.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image16.png)
7. Typ de volgende PowerShell-opdracht om het externe NAT-adres aan te wijzen voor de poorten waar de IKE-verificatie wordt uitgevoerd. Vergeet niet om het IP-adres te wijzigen in een adres dat overeenkomt met uw omgeving.
   
       Add-NetNatExternalAddress -NatName BGPNAT -IPAddress 10.16.169.131 -PortStart 499 -PortEnd 501
8. Vervolgens maakt u een statische NAT-toewijzing om het externe adres toe te wijzen aan het openbare IP-adres van de gateway om de ISAKMP-poort 500  toe te wijzen voor PHASE 1 van de IPSEC-tunnel.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 500 -InternalPort 500
> [!NOTE] 
> De parameter `-InternalAddress` hier is het openbare IP-adres van de virtuele netwerkgateway die u eerder hebt gemaakt.  Als u dit IP-adres wilt zoeken, kijkt u naar de eigenschappen van de blade Virtuele netwerkgateway en zoekt u de waarde voor het openbare IP-adres.       

9. Ten slotte moet u NAT-traversal configureren. Hiervoor moet u poort 4500 gebruiken om de volledige IPEC-tunnel tot stand te brengen via NAT-apparaten.
   
        Add-NetNatStaticMapping -NatName BGPNAT -Protocol UDP -ExternalIPAddress 10.16.169.131 -InternalIPAddress 192.168.102.1 -ExternalPort 4500 -InternalPort 4500
> [!NOTE] 
> De parameter `-InternalAddress` hier is het openbare IP-adres van de virtuele netwerkgateway die u eerder hebt gemaakt.  Als u dit IP-adres wilt zoeken, kijkt u naar de eigenschappen van de blade Virtuele netwerkgateway en zoekt u de waarde voor het openbare IP-adres.       

10. Herhaal de stappen 1 tot en met 9 in POC2.

## <a name="test-the-connection"></a>De verbinding testen
Nu de site-naar-site-verbinding tot stand is gebracht, moet u valideren dat verkeer gebruik kan maken van deze verbinding. Deze taak is eenvoudig. U hoeft zich alleen maar aan te melden bij een van de virtuele machines die u in een van de POC-omgevingen hebt gemaakt, en de virtuele machine te pingen die u in de andere omgeving hebt gemaakt. Controleer of DIP-adres (Direct IP) van de virtuele machine op het externe subnet wordt gepingd (niet de VIP) om ervoor te zorgen dat u het verkeer via de site-naar-site-verbinding stuurt. Hiervoor moet u het adres aan de andere kant van de verbinding zoeken en noteren.

### <a name="log-in-to-the-tenant-vm-in-poc1"></a>Aanmelden bij de tenant-VM in POC1
1. Meld u aan bij de fysieke machine van Azure Stack voor POC1 en meld u met behulp van een tenantaccount aan bij de portal.
2. Klik in de linkernavigatiebalk op **Virtuele machines**.
3. Ga in de lijst met virtuele machines naar **VM01** die u eerder hebt gemaakt, en klik erop.
4. Klik op de blade voor de virtuele machine op **Verbinden**.
   
     ![](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Open een opdrachtprompt vanuit de virtuele machine en typ **ipconfig/all**.
6. Ga in de uitvoer naar het **IPv4-adres** en noteer dit. Dit is het adres dat u gaat pingen vanuit POC2. In de voorbeeldomgeving, het adres is **10.0.10.4**, maar in uw omgeving mogelijk andere. Het adres moet echter binnen het subnet **10.0.10.0/24** vallen dat u eerder hebt gemaakt.

### <a name="log-in-to-the-tenant-vm-in-poc2"></a>Aanmelden bij de tenant-VM in POC2
1. Meld u aan bij de fysieke machine van Azure Stack voor POC2 en meld u met behulp van een tenantaccount aan bij de portal.
2. Klik in de linkernavigatiebalk op **Virtuele machines**.
3. Ga in de lijst met virtuele machines naar **VM02** die u eerder hebt gemaakt en klik erop.
4. Klik op de blade voor de virtuele machine op **Verbinden**.
   
5. Open een opdrachtprompt vanuit de virtuele machine en typ **ipconfig/all**.
6. Als het goed is, ziet u een IPv4-adres dat binnen 10.0.20.0/24 valt. In de voorbeeldomgeving is het adres 10.0.20.4, maar uw adres is mogelijk anders.
7. Nu moet u vanuit de virtuele machine in POC2 pingen naar de virtuele machine in POC1, via de tunnel. Hiervoor moet u het DIP pingen dat u hebt genoteerd bij VM01.
   In de voorbeeldomgeving is dit 10.0.10.4, maar zorg ervoor dat u het adres pingt dat u hebt genoteerd in de testomgeving. Het resultaat ziet er ongeveer zo uit:
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
8. Als u een antwoord van de externe virtuele machine ontvangt, wil dit zeggen dat de test is geslaagd. U kunt het venster voor het maken van verbinding met de virtuele machine sluiten. Of u kunt een andere gegevensoverdracht uitvoeren, zoals het kopiëren van bestanden om de verbinding te testen.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Statistieken van gegevensoverdracht via de gatewayverbinding weergeven
Als u weten hoeveel gegevens via de site-naar-site-verbinding worden doorgegeven, kunt u dit bekijken op de blade Verbinding. Deze test is ook een goede manier om te controleren of de ping die u zojuist hebt verzonden, daadwerkelijk via de VPN-verbinding is gegaan.

1. Meld u, terwijl u nog steeds bent aangemeld bij de tenant-VM in POC2, aan bij **Microsoft Azure Stack POC Portal** met behulp van uw tenantaccount.
2. Klik op het menu-item **Bladeren** en selecteer **Verbindingen**.
3. Klik op de **POC2 POC1**-verbinding in de lijst.
4. Op de blade Verbinding ziet u de statistieken voor **Gegevens in** en **Gegevens uit**. In de volgende schermafbeelding ziet u enkele grotere aantallen dan met één ping worden gemaakt. Dit komt door een aantal extra bestandsoverdrachten. Als het goed is, ziet u hier enkele waarden die niet nul zijn.
   
    ![](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)




<!--HONumber=Feb17_HO2-->


