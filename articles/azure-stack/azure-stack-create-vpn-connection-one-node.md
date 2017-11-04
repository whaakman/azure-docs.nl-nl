---
title: Een site-naar-site VPN-verbinding maken tussen twee virtuele netwerken in verschillende Azure-Stack Development Kit omgevingen | Microsoft Docs
description: "Stapsgewijze instructies die de beheerder van een cloud gebruikt voor het maken van een site-naar-site VPN-verbinding tussen twee Azure Stack Development Kit omgevingen met één knooppunt."
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
ms.date: 7/10/2017
ms.author: scottnap
ms.openlocfilehash: fa2a940620e06521fa110fa13dcbc3050635a502
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-site-to-site-vpn-connection-between-two-virtual-networks-in-different-azure-stack-development-kit-environments"></a>Een site-naar-site VPN-verbinding maken tussen twee virtuele netwerken in verschillende Azure-Stack Development Kit omgevingen
## <a name="overview"></a>Overzicht
In dit artikel laat zien hoe een site-naar-site VPN-verbinding maken tussen twee virtuele netwerken in twee afzonderlijke Azure Stack Development Kit omgevingen. Tijdens het configureren van de verbindingen moet u informatie over de werking van de VPN-gateways in Azure-Stack.

### <a name="connection-diagram"></a>Verbindingsdiagram
Het volgende diagram toont wat de verbindingsconfiguratie ziet er als u klaar bent.

![Configuratie van site-naar-site VPN-verbinding](media/azure-stack-create-vpn-connection-one-node-tp2/OneNodeS2SVPN.png)

### <a name="before-you-begin"></a>Voordat u begint
Zorg ervoor dat u de volgende items hebt voordat u begint voor het voltooien van de configuratie van de verbinding:

* Twee servers die voldoen aan de hardwarevereisten Azure Stack Development Kit die worden gedefinieerd door de [vereisten voor implementatie van Azure-Stack](azure-stack-deploy.md). Zorg ervoor dat de vereisten die worden weergegeven in de [artikel](azure-stack-deploy.md) te is voldaan.
* De [Azure Stack Development Kit](https://azure.microsoft.com/en-us/overview/azure-stack/try/) implementatiepakket.

## <a name="deploy-the-azure-stack-development-kit-environments"></a>De Azure-Stack Development Kit omgevingen implementeren
Voor het voltooien van de configuratie van de verbinding, moet u twee Azure Stack Development Kit omgevingen implementeren.
> [!NOTE] 
> Voor elke Azure-Stack Development Kit die u implementeert, volgt u de [implementatie-instructies](azure-stack-run-powershell-script.md). In dit artikel wordt de Azure-Stack Development Kit-omgevingen worden genoemd *POC1* en *POC2*.


## <a name="prepare-an-offer-on-poc1-and-poc2"></a>Een aanbieding op POC1 en POC2 voorbereiden
Bereid een aanbieding voor op POC1 en POC2, zodat een gebruiker kan zich op de aanbieding abonneren en implementeren van de virtuele machines. Zie voor meer informatie over het maken van een aanbieding [virtuele machines beschikbaar maken voor de gebruikers van uw Azure-Stack](azure-stack-tutorial-tenant-vm.md).

## <a name="review-and-complete-the-network-configuration-table"></a>Controleer en voltooi de configuratie van netwerktabel
De volgende tabel geeft een overzicht van de netwerkconfiguratie voor beide omgevingen Azure Stack Development Kit. Gebruik de procedure die wordt weergegeven na de tabel om toe te voegen van de externe BGPNAT-adres dat specifiek is voor uw netwerk.

**Netwerk configuratietabel**
|   |POC1|POC2|
|---------|---------|---------|
|Virtuele-netwerknaam     |VNET-01|VNET-02 |
|Adresruimte voor virtueel netwerk |10.0.10.0/23|10.0.20.0/23|
|Subnetnaam     |Subnet-01|Subnet-02|
|Subnetadresbereik|10.0.10.0/24 |10.0.20.0/24 |
|Gatewaysubnet     |10.0.11.0/24|10.0.21.0/24|
|Externe BGPNAT adres     |         |         |

> [!NOTE]
> De externe BGPNAT IP-adressen in de voorbeeldomgeving zijn 10.16.167.195 voor POC1 en 10.16.169.131 voor POC2. De volgende procedure gebruiken om te bepalen van de externe BGPNAT IP-adressen voor uw Azure-Stack Development Kit hosts en voeg ze toe aan de vorige tabel van de netwerk-configuratie.


### <a name="get-the-ip-address-of-the-external-adapter-of-the-nat-vm"></a>Het IP-adres ophalen van de externe adapter van de virtuele NAT-machine
1. Meld u aan de fysieke machine van Azure-Stack voor POC1.
2. De volgende Powershell-code ter vervanging van het administrator-wachtwoord bewerken en voert u de code op de host Implementatiemodel:

   ```powershell
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "AzS-bgpnat01" `
    -Password $Password
   ```
3. Het IP-adres toevoegen aan de netwerk-configuratie-tabel die wordt weergegeven in de vorige sectie.

4. Herhaal deze procedure op POC2.

## <a name="create-the-network-resources-in-poc1"></a>De netwerkbronnen in POC1 maken
Nu maken u de POC1 netwerkbronnen die u nodig hebt om in te stellen uw gateways. De volgende instructies hoe u de resources maken met behulp van de gebruikersportal. U kunt ook PowerShell-code gebruiken om de resources te maken.

![Werkstroom die wordt gebruikt om resources te maken](media/azure-stack-create-vpn-connection-one-node-tp2/image2.png)

### <a name="sign-in-as-a-tenant"></a>Meld u aan als een tenant
Een servicebeheerder kunt aanmelden als een tenant voor het testen van het plannen, aanbiedingen en abonnementen die van hun tenants gebruikmaken mogelijk. Als u nog geen hebt, [een tenantaccount maken](azure-stack-add-new-user-aad.md) voordat u zich aanmeldt.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Het virtuele netwerk en VM-subnet maken
1. Gebruik een tenantaccount aan te melden bij de gebruikersportal.
2. Selecteer in de gebruikersportal **nieuw**.

    ![Nieuw virtueel netwerk maken](media/azure-stack-create-vpn-connection-one-node-tp2/image3.png)

3. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
4. Selecteer **virtueel netwerk**.
5. Voor **naam**, **adresruimte**, **subnetnaam**, en **adresbereik**, gebruik de waarden die eerder in het netwerk worden weergegeven van de configuratietabel.
6. In **abonnement**, het abonnement dat u eerder hebt gemaakt wordt weergegeven.
7. Voor **resourcegroep**, kunt u een resourcegroep maken of als u al hebt, selecteert u **gebruik bestaande**.
8. Controleer de standaardlocatie.
9. Selecteer **Vastmaken aan dashboard**.
10. Selecteer **Maken**.

### <a name="create-the-gateway-subnet"></a>Her gatewaysubnet maken
1. Open de VNET-01 virtueel netwerk-resource die u eerder hebt gemaakt op het dashboard.
2. Selecteer op de blade **Instellingen** de optie **Subnetten**.
3. Selecteer om een gatewaysubnet toe aan het virtuele netwerk **Gatewaysubnet**.
   
    ![Gatewaysubnet toevoegen](media/azure-stack-create-vpn-connection-one-node-tp2/image4.png)

4. De subnetnaam is standaard ingesteld op **GatewaySubnet**.
   Gateway-subnetten zijn speciale. Werkt alleen goed als ze moeten gebruiken de *GatewaySubnet* naam.
5. In **-adresbereik**, Controleer of het adres **10.0.11.0/24**.
6. Selecteer **OK** het gatewaysubnet maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken
1. Selecteer in de Azure-portal **nieuw**. 
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst van netwerkbronnen **virtuele netwerkgateway**.
4. In **naam**, voer **GW1**.
5. Selecteer de **virtueel netwerk** item een virtueel netwerk kiezen.
   Selecteer **VNET-01** uit de lijst.
6. Selecteer de **openbaar IP-adres** menu-item. Wanneer de **openbare IP-adres kiezen** blade wordt geopend, selecteer **nieuw**.
7. In **naam**, voer **GW1 PiP**, en selecteer vervolgens **OK**.
8.  Standaard voor **VPN-type**, **op Route gebaseerde** is geselecteerd.
    Houd de **op Route gebaseerde** VPN-type.
9. Controleer of **Abonnement** en **Locatie** juist zijn. U kunt de resource toe aan het dashboard vastmaken. Selecteer **Maken**.

### <a name="create-the-local-network-gateway"></a>De lokale netwerkgateway maken
De implementatie van een *lokale netwerkgateway* in deze implementatie van een Azure Stack-evaluatie verschilt iets van een werkelijke Azure-implementatie.

In een Azure-implementatie vertegenwoordigt een lokale netwerkgateway een on-premises (op de tenant) fysieke apparaat, waarmee u verbinding maken met een virtuele netwerkgateway in Azure. In deze implementatie van de evaluatie van Azure-Stack zijn beide kanten van de verbinding virtuele netwerkgateways!

Een manier om na te denken over deze meer algemeen is dat de lokale gateway netwerkbron wordt altijd aangegeven met de RAS-gateway aan het andere einde van de verbinding. Vanwege de manier waarop die de Azure-Stack Development Kit is ontworpen, moet u voor het IP-adres van de externe netwerkadapter op de netwerkadresomzetting (NAT) VM van de andere Azure-Stack Development Kit als het openbare IP-adres van de lokale netwerkgateway. Vervolgens maakt u NAT-toewijzingen op de NAT-VM om ervoor te zorgen dat beide kanten goed zijn aangesloten.


### <a name="create-the-local-network-gateway-resource"></a>Het lokale netwerk gateway-resource maken
1. Meld u aan de fysieke machine van Azure-Stack voor POC1.
2. Selecteer in de gebruikersportal **nieuw**.
3. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
4. Selecteer in de lijst met resources **lokale netwerkgateway**.
5. In **naam**, voer **POC2 GW**.
6. In **IP-adres**, voer het adres van de externe BGPNAT voor POC2. Dit adres wordt eerder in de tabel van de configuratie van netwerk weergegeven.
7. In **adresruimte**, voor de adresruimte van het VNET POC2 die u later maakt, voert u **10.0.20.0/23**.
8. Controleer uw **abonnement**, **resourcegroep**, en **locatie** juist zijn en selecteer vervolgens **maken**.

### <a name="create-the-connection"></a>De verbinding maken
1. Selecteer in de gebruikersportal **nieuw**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst met resources **verbinding**.
4. Op de **basisbeginselen** instellingenblade voor de **verbindingstype**, selecteer **Site-naar-site (IPSec)**.
5. Selecteer de **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
6. Op de **instellingen** blade Selecteer **virtuele netwerkgateway**, en selecteer vervolgens **GW1**.
7. Selecteer **lokale netwerkgateway**, en selecteer vervolgens **POC2 GW**.
8. In **verbindingsnaam**, voer **POC1 POC2**.
9. In **gedeelde sleutel (PSK)**, voer **12345**, en selecteer vervolgens **OK**.
10. Op de **samenvatting** blade Selecteer **OK**.

### <a name="create-a-vm"></a>Een virtuele machine maken
Voor het valideren van de gegevens die worden uitgewisseld via de VPN-verbinding, moet u de virtuele machines verzenden en ontvangen van gegevens in elke Azure-Stack Development Kit. Maak nu een virtuele machine in POC1, en deze vervolgens in het virtuele netwerk op de VM-subnet.

1. Selecteer in de Azure-portal **nieuw**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Compute**.
3. Selecteer in de lijst van installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie.
4. Op de **basisbeginselen** blade in **naam**, voer **VM01**.
5. Voer een geldige gebruikersnaam en wachtwoord. Dit account kunt u zich aanmeldt bij de virtuele machine nadat deze gemaakt.
6. Geef een **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
7. Op de **grootte** blade voor dit exemplaar, selecteer de grootte van een virtuele machine en selecteer vervolgens **Selecteer**.
8. Op de **instellingen** blade, accepteer de standaardinstellingen. Zorg ervoor dat de **VNET-01** virtueel netwerk is geselecteerd. Controleren of het subnet is ingesteld op **10.0.10.0/24**. Selecteer vervolgens **OK**.
9. Op de **samenvatting** blade, Controleer de instellingen en selecteer vervolgens **OK**.



## <a name="create-the-network-resources-in-poc2"></a>De netwerkbronnen in POC2 maken

De volgende stap is het maken van de netwerkbronnen voor POC2. De volgende instructies wordt aangegeven hoe de om resources te maken met behulp van de gebruikersportal.

### <a name="sign-in-as-a-tenant"></a>Meld u aan als een tenant
Een servicebeheerder kunt aanmelden als een tenant voor het testen van het plannen, aanbiedingen en abonnementen die van hun tenants gebruikmaken mogelijk. Als u nog geen hebt, [een tenantaccount maken](azure-stack-add-new-user-aad.md) voordat u zich aanmeldt.

### <a name="create-the-virtual-network-and-vm-subnet"></a>Het virtuele netwerk en VM-subnet maken

1. Meld u met behulp van een tenantaccount.
2. Selecteer in de gebruikersportal **nieuw**.
3. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
4. Selecteer **virtueel netwerk**.
5. Gebruik de informatie die eerder in de tabel van network configuration voor het identificeren van de waarden van de POC2 **naam**, **adresruimte**, **subnetnaam**, en **Adresbereik**.
6. In **abonnement**, het abonnement dat u eerder hebt gemaakt wordt weergegeven.
7. Voor **resourcegroep**, een nieuwe resourcegroep maken of als u al hebt, selecteert u **gebruik bestaande**.
8. Controleer of de standaard **locatie**.
9. Selecteer **Vastmaken aan dashboard**.
10. Selecteer **Maken**.

### <a name="create-the-gateway-subnet"></a>Het gatewaysubnet maken
1. Open de virtuele netwerk resource die u hebt gemaakt (**VNET-02**) vanuit het dashboard.
2. Selecteer op de blade **Instellingen** de optie **Subnetten**.
3. Selecteer **gatewaysubnet** een gatewaysubnet toevoegen aan het virtuele netwerk.
4. De naam van het subnet is standaard ingesteld op **Gatewaysubnet**.
   Gatewaysubnetten zijn speciaal en hebben deze specifieke naam nodig om goed te functioneren.
5. In de **-adresbereik** veld, controleert u of het adres is **10.0.21.0/24**.
6. Selecteer **OK** het gatewaysubnet maken.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken
1. Selecteer in de Azure-portal **nieuw**.  
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst van netwerkbronnen **virtuele netwerkgateway**.
4. In **naam**, voer **GW2**.
5. Als u een virtueel netwerk, selecteer **virtueel netwerk**. Selecteer vervolgens **VNET-02** uit de lijst.
6. Selecteer **openbaar IP-adres**. Wanneer de **openbare IP-adres kiezen** blade wordt geopend, selecteer **nieuw**.
7. In **naam**, voer **GW2 PiP**, en selecteer vervolgens **OK**.
8. Standaard voor **VPN-type**, **op Route gebaseerde** is geselecteerd.
    Houd de **op Route gebaseerde** VPN-type.
9. Controleer of **Abonnement** en **Locatie** juist zijn. U kunt de resource toe aan het dashboard vastmaken. Selecteer **Maken**.

### <a name="create-the-local-network-gateway-resource"></a>Het lokale netwerk gateway-resource maken

1. Selecteer in de gebruikersportal POC2 **nieuw**. 
4. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
5. Selecteer in de lijst met resources **lokale netwerkgateway**.
6. In **naam**, voer **POC1 GW**.
7. In **IP-adres**, voer het adres van de externe BGPNAT voor POC1 die eerder in de tabel van de configuratie van netwerk wordt vermeld.
8. In **adresruimte**, van POC1, voer de **10.0.10.0/23** -adresruimte van **VNET-01**.
9. Controleer uw **abonnement**, **resourcegroep**, en **locatie** juist zijn en selecteer vervolgens **maken**.

## <a name="create-the-connection"></a>De verbinding maken
1. Selecteer in de gebruikersportal **nieuw**. 
2. Ga naar **Marketplace**, en selecteer vervolgens **Networking**.
3. Selecteer in de lijst met resources **verbinding**.
4. Op de **Basic** instellingenblade voor de **verbindingstype**, kies **Site-naar-site (IPSec)**.
5. Selecteer de **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
6. Op de **instellingen** blade Selecteer **virtuele netwerkgateway**, en selecteer vervolgens **GW2**.
7. Selecteer **lokale netwerkgateway**, en selecteer vervolgens **POC1 GW**.
8. In **verbindingsnaam**, voer **POC2 POC1**.
9. In **gedeelde sleutel (PSK)**, voer **12345**. Als u een andere waarde kiest, houd er rekening mee dat deze *moet* overeenkomen met de waarde voor de gedeelde sleutel die u op POC1 gemaakt. Selecteer **OK**.
10. Controleer de **samenvatting** blade en selecteer vervolgens **OK**.

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Een virtuele machine nu in POC2 maken en op uw VM-subnet in het virtuele netwerk plaatsen.

1. Selecteer in de Azure-portal **nieuw**.
2. Ga naar **Marketplace**, en selecteer vervolgens **Compute**.
3. Selecteer in de lijst van installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie.
4. Op de **basisbeginselen** blade voor **naam**, voer **VM02**.
5. Voer een geldige gebruikersnaam en wachtwoord. Dit account kunt u zich aanmeldt bij de virtuele machine nadat deze gemaakt.
6. Geef een **abonnement**, **resourcegroep**, en **locatie**, en selecteer vervolgens **OK**.
7. Op de **grootte** blade, selecteer een virtuele machine grootte voor dit exemplaar en selecteer vervolgens **Selecteer**.
8. Op de **instellingen** blade kunt u de standaardinstellingen accepteren. Zorg ervoor dat de **VNET-02** virtueel netwerk is geselecteerd en controleren of het subnet is ingesteld op **10.0.20.0/24**. Selecteer **OK**.
9. Controleer de instellingen op de **samenvatting** blade en selecteer vervolgens **OK**.

## <a name="configure-the-nat-virtual-machine-on-each-azure-stack-development-kit-for-gateway-traversal"></a>De NAT-virtuele machine configureren op elke Azure-Stack Development Kit voor gateway traversal
Omdat de Azure-Stack Development Kit zichzelf staand en worden geïsoleerd van het netwerk waarop de fysieke host is geïmplementeerd, de *externe* VIP-netwerk die de gateways zijn verbonden met is niet daadwerkelijk externe. In plaats daarvan is het VIP-netwerk verborgen achter een router die netwerkadresomzetting uitvoert. 

De router is de virtuele machine van een Windows Server, genaamd *AzS bgpnat01*, die de rol van de Routing and Remote Access Services (RRAS) wordt uitgevoerd in de infrastructuur van Azure Stack Development Kit. NAT moet u op de virtuele machine van AzS bgpnat01 om toe te staan de site-naar-site VPN-verbinding om verbinding aan beide uiteinden te configureren. 

U moet een statische route uit NAT-kaart die de externe interface op de BGPNAT virtuele machine wordt toegewezen aan het VIP van de Gateway Edgegroep maken voor het configureren van de VPN-verbinding. Een statische route van de NAT-kaart is vereist voor elke poort in een VPN-verbinding.

> [!NOTE]
> Deze configuratie is vereist voor alleen in omgevingen met Azure Stack Development Kit.
> 
> 

### <a name="configure-the-nat"></a>Het NAT-apparaat configureren
> [!IMPORTANT]
> Voltooi deze procedure voor het eerst *beide* Azure Stack Development Kit omgevingen.

1. Bepaal de **interne IP-adres** moet worden gebruikt in de volgende PowerShell-script. Openen van de virtuele netwerkgateway (GW1 en GW2), en klik vervolgens op de **overzicht** blade, slaat u de waarde voor de **openbaar IP-adres** voor later gebruik.
![Interne IP-adres](media/azure-stack-create-vpn-connection-one-node-tp2/InternalIP.PNG)
2. Meld u aan de fysieke machine van Azure-Stack voor POC1.
3. Kopiëren en bewerken van de volgende PowerShell-script. Als u wilt het NAT-apparaat op elke Azure-Stack Development Kit configureert, moet u het script uitvoeren in een verhoogde Windows PowerShell ISE. In het script, voeg waarden toe aan de *externe BGPNAT adres* en *interne IP-adres* tijdelijke aanduidingen:

   ```powershell
   # Designate the external NAT address for the ports that use the IKE authentication.
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 499 `
      -PortEnd 501}
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress <External BGPNAT address> `
      -PortStart 4499 `
      -PortEnd 4501}
   # create a static NAT mapping to map the external address to the Gateway
   # Public IP Address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 500 `
      -InternalPort 500}
   # Finally, configure NAT traversal which uses port 4500 to
   # successfully establish the complete IPSEC tunnel over NAT devices
   Invoke-Command `
    -ComputerName AzS-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress <External BGPNAT address> `
      -InternalIPAddress <Internal IP address> `
      -ExternalPort 4500 `
      -InternalPort 4500}
   ```

4. Herhaal deze procedure op POC2.

## <a name="test-the-connection"></a>De verbinding testen
Nu dat de site-naar-site-verbinding tot stand is gebracht, moet u controleren dat u verkeer ermee kunt krijgen. Als u wilt valideren, moet u zich aanmelden bij een van de virtuele machines die u hebt gemaakt in een Azure-Stack Development Kit-omgeving. Pingt de virtuele machine die u hebt gemaakt in de andere omgeving. 

Zorg ervoor dat u het Direct IP (DIP)-adres van de virtuele machine op de externe subnet, niet het VIP pingen om ervoor te zorgen dat u het verkeer via de site-naar-site-verbinding verzenden. U doet dit door het DIP-adres op het andere einde van de verbinding te vinden. Sla het adres voor later gebruik.

### <a name="sign-in-to-the-tenant-vm-in-poc1"></a>Aanmelden bij de virtuele machine in POC1-tenant
1. Aanmelden bij de fysieke machine van Azure-Stack voor POC1 en vervolgens een tenantaccount aan te melden bij de gebruikersportal gebruiken.
2. Selecteer in de linkernavigatiebalk **Compute**.
3. Zoek in de lijst van virtuele machines **VM01** die u eerder hebt gemaakt en selecteert u vervolgens.
4. Klik op de blade voor de virtuele machine op **Connect**, en open vervolgens het bestand VM01.rdp.
   
     ![Verbinding maken met de knop](media/azure-stack-create-vpn-connection-one-node-tp2/image17.png)
5. Aanmelden met het account dat u hebt geconfigureerd tijdens het maken van de virtuele machine.
6. Open een opdrachtprompt met verhoogde bevoegdheid **Windows PowerShell** venster.
7. Voer **ipconfig/all**.
8. Zoek in de uitvoer naar de **IPv4-adres**, en sla het adres voor later gebruik. Dit is het adres dat u van POC2 pingt. In de voorbeeldomgeving, het adres is **10.0.10.4**, maar in uw omgeving mogelijk andere. Deze moet vallen binnen de **10.0.10.0/24** subnet dat u eerder hebt gemaakt.
9. Voer de volgende PowerShell-opdracht voor het maken van een firewallregel waarmee de virtuele machine op pings reageren:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="sign-in-to-the-tenant-vm-in-poc2"></a>Aanmelden bij de virtuele machine in POC2-tenant
1. Aanmelden bij de fysieke machine van Azure-Stack voor POC2 en vervolgens een tenantaccount aan te melden bij de gebruikersportal gebruiken.
2. Klik in de linkernavigatiebalk op **Compute**.
3. Zoek in de lijst met virtuele machines, **VM02** die u eerder hebt gemaakt en selecteert u vervolgens.
4. Klik op de blade voor de virtuele machine op **Verbinden**.
5. Aanmelden met het account dat u hebt geconfigureerd tijdens het maken van de virtuele machine.
6. Open een opdrachtprompt met verhoogde bevoegdheid **Windows PowerShell** venster.
7. Voer **ipconfig/all**.
8. Er is een IPv4-adres valt binnen **10.0.20.0/24**. In de voorbeeldomgeving, het adres is **10.0.20.4**, maar uw adres kan anders zijn.
9. Voer de volgende PowerShell-opdracht voor het maken van een firewallregel waarmee de virtuele machine op pings reageren:

   ```powershell
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

10. Ping van de virtuele machine op POC2, de virtuele machine op POC1, via de tunnel. Om dit te doen, moet u het DIP die u hebt vastgelegd vanaf VM01 pingen.
   In de voorbeeldomgeving is dit **10.0.10.4**, maar zorg ervoor dat het adres dat u hebt genoteerd in uw testomgeving pingen. Hier ziet u een resultaat dat op het volgende lijkt:
   
    ![Geslaagde ping](media/azure-stack-create-vpn-connection-one-node-tp2/image19b.png)
11. Een antwoord van de externe virtuele-machine geeft aan dat een geslaagde test! U kunt de virtuele machine-venster sluiten. U kunt uw om verbinding te testen, andere soorten gegevensoverdrachten zoals het kopiëren van een bestand.

### <a name="viewing-data-transfer-statistics-through-the-gateway-connection"></a>Statistieken van gegevensoverdracht via de gatewayverbinding weergeven
Als u wilt weten op hoeveel gegevens worden doorgegeven via de site-naar-site-verbinding, deze informatie is beschikbaar op de **verbinding** blade. Deze test is ook een andere manier om te verifiëren dat het pingen die u zojuist hebt verzonden daadwerkelijk is gegaan via de VPN-verbinding.

1. Terwijl u bent aangemeld bij de virtuele tenantmachine in POC2, gebruikt u de tenant-serviceaccount aan te melden bij de gebruikersportal.
2. Ga naar **alle resources**, en selecteer vervolgens de **POC2 POC1** verbinding. **Verbindingen** wordt weergegeven.
4. Op de **verbinding** blade, de statistieken voor **gegevens in** en **gegevensuitvoer** worden weergegeven. In de volgende schermafbeelding worden grote aantallen toegeschreven aan extra bestandsoverdracht. U ziet er sommige andere waarden.
   
    ![Gegevens in en uit](media/azure-stack-create-vpn-connection-one-node-tp2/image20.png)
