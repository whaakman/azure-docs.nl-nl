---
title: Verbinding maken met Azure-Stack in Azure maken met ExpressRoute
description: Het verbinding maken met virtuele netwerken in Azure-Stack van virtuele netwerken in Azure ExpressRoute gebruiken.
services: azure-stack
documentationcenter: 
author: victorar
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: victorh
ms.openlocfilehash: 248e9cb521975e9c982684668a68214ce5a1c827
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="connect-azure-stack-to-azure-using-expressroute"></a>Verbinding maken met Azure-Stack in Azure maken met ExpressRoute

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Er zijn twee manieren verbinding maken met virtuele netwerken in Azure-Stack van virtuele netwerken in Azure:
   * **Site-to-Site**

     Een VPN-verbinding via IPsec (IKE v1 en IKE v2). Voor dit type verbinding is een VPN-apparaat of RRAS vereist. Zie voor meer informatie [Azure-Stack naar Azure via VPN verbinding](azure-stack-connect-vpn.md).
   * **ExpressRoute**

     Een directe verbinding met Azure van uw Azure-Stack-implementatie. ExpressRoute is **niet** een VPN-verbinding via het openbare Internet. Zie voor meer informatie over Azure ExpressRoute [overzicht van ExpressRoute](../expressroute/expressroute-introduction.md).

In dit artikel ziet u een voorbeeld met behulp van ExpressRoute Azure Stack verbinding te maken met Azure.
## <a name="requirements"></a>Vereisten
Hier volgen de specifieke vereisten voor het verbinding maken met Azure-Stack en Azure ExpressRoute gebruiken:
* Een Azure-abonnement maken van een ExpressRoute-circuit en de VNets in Azure.
* Een ExpressRoute-circuit via ingericht een [connectiviteitsprovider](../expressroute/expressroute-locations.md).
* Een router die het ExpressRoute-circuit dat is verbonden met de WAN-poorten heeft.
* De LAN-zijde van de router is gekoppeld aan de Azure-Stack Multitenant-Gateway.
* De router moet ondersteuning voor Site-naar-Site VPN-verbindingen tussen de LAN-interface en Azure Stack-Multitenant-Gateway.
* Als meer dan een tenant in uw Azure-Stack-implementatie wordt toegevoegd, is de router moet kunnen maken van meerdere VRFs (virtuele Routering en doorsturen).

Het volgende diagram toont een conceptueel overzicht van de netwerken na het voltooien van de configuratie:

![Conceptueel diagram](media/azure-stack-connect-expressroute/Conceptual.png)

**Diagram 1**

De volgende Architectuurdiagram ziet u hoe meerdere tenants verbinding van de Azure-Stack-infrastructuur via de ExpressRoute-router naar Azure aan de rand van Microsoft:

![Architectuurdiagram](media/azure-stack-connect-expressroute/Architecture.png)

**Diagram 2**

Het voorbeeld in dit artikel gebruikt dezelfde architectuur verbinding maken met Azure via ExpressRoute persoonlijke peering. Het is voltooid met behulp van een Site-naar-Site VPN-verbinding van de virtuele netwerkgateway in Azure-Stack naar een ExpressRoute-router. De volgende stappen in dit artikel ziet het maken van een end-to-end-verbinding tussen de twee VNets van twee verschillende tenants in Azure-Stack aan hun respectieve VNets in Azure. U kunt veel VNets tenant en de stappen voor elke tenant repliceren of in dit voorbeeld gebruiken voor het implementeren van slechts een enkele tenant VNet toevoegen.

## <a name="configure-azure-stack"></a>Azure Stack configureren
Nu maken u de resources die u moet instellen van uw Azure-Stack-omgeving als een tenant. De volgende stappen laten zien wat u moet doen. Deze instructies laten zien hoe u resources met behulp van de Stack van Azure portal maken, maar u kunt ook PowerShell gebruiken.

![Stappen voor netwerk-resource](media/azure-stack-connect-expressroute/image2.png)
### <a name="before-you-begin"></a>Voordat u begint
Voordat u de configuratie, hebt u het volgende nodig:
* De implementatie van een Azure-Stack.

   Zie voor meer informatie over het implementeren van Azure Stack Development Kit [Azure Stack Development Kit implementatie Quick Start](azure-stack-deploy-overview.md).
* Een aanbieding op Azure-Stack die uw gebruikers kan zich abonneren op.

  Zie voor instructies [virtuele machines beschikbaar maken voor de gebruikers van uw Azure-Stack](azure-stack-tutorial-tenant-vm.md).

### <a name="create-network-resources-in-azure-stack"></a>Netwerkbronnen maken in Azure-Stack

Gebruik de volgende procedures voor het maken van de vereiste netwerkbronnen in Azure-Stack voor elke tenant:

#### <a name="create-the-virtual-network-and-vm-subnet"></a>Het virtuele netwerk en VM-subnet maken
1. Meld u aan de gebruikersportal aanmeldt met een gebruikersaccount (tenant).

2. Klik in de portal op **nieuw**.

   ![](media/azure-stack-connect-expressroute/MAS-new.png)

3. Selecteer **Netwerken** in het menu Marketplace.

4. Klik in het menu op **Virtueel netwerk**.

5. Typ in de juiste velden in de volgende tabel met de waarden:

   |Veld  |Waarde  |
   |---------|---------|
   |Naam     |Tenant1VNet1         |
   |Adresruimte     |10.1.0.0/16|
   |Subnetnaam     |Tenant1-Sub1|
   |Subnetadresbereik     |10.1.1.0/24|

6. In het veld **Abonnement** wordt nu het abonnement weergegeven dat u eerder hebt ingevuld.

    a. Voor de resourcegroep, u kunt een resourcegroep maken of als u al hebt, selecteert u **gebruik bestaande**.

    b. Controleer de standaardlocatie.

    c. Klik op **vastmaken aan dashboard**.

    d. Klik op **Create**.



#### <a name="create-the-gateway-subnet"></a>Her gatewaysubnet maken
1. Open de Virtual Network-resource die u hebt gemaakt (Tenant1VNet1) van het dashboard.
2. Selecteer op de sectie instellingen **subnetten**.
3. Klik op **Gatewaysubnet** om een gatewaysubnet toe te voegen aan het virtuele netwerk.
   
    ![](media/azure-stack-connect-expressroute/gatewaysubnet.png)
4. De naam van het subnet is standaard ingesteld op **Gatewaysubnet**.
   Gatewaysubnetten zijn speciaal en hebben deze specifieke naam nodig om goed te functioneren.
5. In de **-adresbereik** veld, controleert u of het adres is **10.1.0.0/24**.
6. Klik op **OK** het gatewaysubnet maken.

#### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken
1. Klik in de gebruikersportal van Azure-Stack **nieuw**.
   
2. Selecteer **Netwerken** in het menu Marketplace.
3. Selecteer **Gateway van het virtuele netwerk** in de lijst met netwerkresources.
4. Typ **GW1** in het veld **Naam**.
5. Klik op het item **Virtueel netwerk** om een virtueel netwerk te kiezen.
   Selecteer **Tenant1VNet1** uit de lijst.
6. Klik op het menu-item **Openbaar IP-adres**. Wanneer de **openbare IP-adres kiezen** sectie opent Klik **nieuw**.
7. Typ **GW1-PiP** in het veld **Naam** en klik op **OK**.
8. Bij **VPN-type** moet standaard **Op basis van route** zijn geselecteerd.
    Behoud deze instelling.
9. Controleer of **Abonnement** en **Locatie** juist zijn. Als u wilt, kunt u de resource toe aan het Dashboard vastmaken. Klik op **Create**.

#### <a name="create-the-local-network-gateway"></a>De lokale netwerkgateway maken

Het doel van de lokale gateway van de netwerkbron is om aan te geven van de RAS-gateway aan het andere einde van de VPN-verbinding. In dit voorbeeld is de andere zijde de LAN-Hiermee van de ExpressRoute-router. Het externe adres is 10.60.3.255 voor Tenant 1 in dit voorbeeld, zoals wordt weergegeven in het Diagram 2.

1. Meld u aan bij de fysieke computer van Azure Stack.
2. Aanmelden bij de gebruikersportal aan uw gebruikersaccount en klik op **nieuw**.
3. Selecteer **Netwerken** in het menu Marketplace.
4. Selecteer **lokale netwerkgateway** in de lijst met resources.
5. In de **naam** veldtype **ER-Router-GW**.
6. Voor de **IP-adres** veld, raadpleeg dan Diagram 2. Het IP-adres van de ExpressRoute-router LAN Hiermee voor Tenant-1 is 10.60.3.255. Typ het IP-adres van de bijbehorende routerinterface voor uw eigen omgeving.
7. In de **adresruimte** veld, typt u de adresruimte van de VNets die u verbinding maken wilt met in Azure. Raadpleeg Diagram 2 voor dit voorbeeld. U ziet dat de subnetten die vereist zijn voor Tenant-1, **192.168.2.0/24** (dit is het Vnet Hub in Azure) en **10.100.0.0/16** (dit is het VNet spaak in Azure). Typ de overeenkomstige subnetten voor uw eigen omgeving.
   > [!IMPORTANT]
   > In dit voorbeeld wordt ervan uitgegaan dat u bij het gebruik van statische routes voor de Site-naar-Site VPN-verbinding tussen de Azure-Stack-gateway en de ExpressRoute-router.

8. Controleer uw **abonnement**, **resourcegroep**, en **locatie** juist zijn en klik op **maken**.

#### <a name="create-the-connection"></a>De verbinding maken
1. Klik in de gebruikersportal van Azure-Stack **nieuw**.
2. Selecteer **Netwerken** in het menu Marketplace.
3. Selecteer **Verbinding** in de lijst met resources.
4. In de **basisbeginselen** sectie instellingen kiezen **Site-naar-site (IPSec)** als de **verbindingstype**.
5. Selecteer de **abonnement**, **resourcegroep**, en **locatie** en klik op **OK**.
6. In de **instellingen** sectie, klikt u op **virtuele netwerkgateway** klikt u op **GW1**.
7. Klik op **lokale netwerkgateway**, en klik op **ER Router GW**.
8. In de **verbindingsnaam** veld **ConnectToAzure**.
9. In de **gedeelde sleutel (PSK)** veld **abc123** en klik op **OK**.
10. Op de **samenvatting** sectie, klikt u op **OK**.

    Nadat de verbinding is gemaakt, kunt u het openbare IP-adres gebruikt door de virtuele netwerkgateway kunt zien. Om het adres in de Stack van Azure-portal, blader naar uw virtuele netwerkgateway. In **overzicht**, vinden de **openbaar IP-adres**. Noteer dit adres; u gebruikt als de *interne IP-adres* in het volgende gedeelte (als die van toepassing zijn voor uw implementatie).

    ![](media/azure-stack-connect-expressroute/GWPublicIP.png)

#### <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Als u wilt valideren gegevensverkeer via de VPN-verbinding, moet u virtuele machines verzenden en ontvangen van gegevens in de Stack Azure Vnet. Nu een virtuele machine maken en plaats deze in uw VM-subnet in het virtuele netwerk.

1. Klik in de gebruikersportal van Azure-Stack **nieuw**.
2. Selecteer **Virtuele machines** in het menu Marketplace.
3. Selecteer in de lijst van installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie en klikt u op **maken**.
4. Op de **basisbeginselen** sectie in het **naam** veldtype **VM01**.
5. Typ een geldige gebruikersnaam en een geldig wachtwoord. U gaat dit account gebruiken om u aan te melden bij de virtuele machine nadat deze is gemaakt.
6. Geef een **abonnement**, **resourcegroep**, en **locatie** en klik vervolgens op **OK**.
7. Op de **grootte** sectie, klikt u op de virtuele machine een grootte voor dit exemplaar en klik vervolgens op **Selecteer**.
8. Op de **instellingen** sectie, kunt u de standaardinstellingen accepteren. Zorg ervoor dat het virtuele netwerk dat is geselecteerd, maar **Tenant1VNet1** en het subnet is ingesteld op **10.1.1.0/24**. Klik op **OK**.
9. Controleer de instellingen op de **samenvatting** sectie en klik op **OK**.

Herhaal de vorige stappen uit voor elke tenant VNet dat u verbinding wilt maken, **maken van het virtuele netwerk en de VM-subnet** via **maken van een virtuele machine** secties.

### <a name="configure-the-nat-virtual-machine-for-gateway-traversal"></a>De NAT-virtuele machine voor gateway traversal configureren
> [!IMPORTANT]
> Deze sectie is voor alleen implementaties van Azure Stack Development Kit. Het NAT-apparaat is niet nodig voor implementaties met meerdere knooppunten.

De Azure-Stack Development Kit is ingesloten en geïsoleerd van het netwerk waarop de fysieke host is geïmplementeerd. Dus het netwerk 'Extern' VIP die de gateways zijn verbonden met is geen externe, maar in plaats daarvan achter een router die tijdens het doorzoeken van NAT (Network Address Translation) wordt verborgen.
 
De router is een virtuele machine van Windows Server (**AzS BGPNAT01**) de rol van de Routing and Remote Access Services (RRAS) uitgevoerd in de Azure-Stack Development Kit-infrastructuur. NAT moet u op de virtuele machine van AzS BGPNAT01 om toe te staan de Site-naar-Site VPN-verbinding om verbinding aan beide uiteinden te configureren.

#### <a name="configure-the-nat"></a>Het NAT-apparaat configureren

1. Aanmelden bij de Azure-Stack fysieke machine met uw beheerdersaccount.
2. Kopiëren en bewerken van de volgende PowerShell-script en uitvoeren in een verhoogde Windows PowerShell ISE. Vervang het administrator-wachtwoord. Het adres dat wordt gegeven is uw *externe BGPNAT adres*.

   ```
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "azs-bgpnat01" `
    -Password $Password
   ```
4. Het NAT-apparaat configureren, kopiëren en bewerken van de volgende PowerShell-script en uitvoeren in een verhoogde Windows PowerShell ISE. Bewerk het script vervangen de *externe BGPNAT adres* en *interne IP-adres* (die u eerder hebt genoteerd de **maken van de verbinding** sectie).

   In de diagrammen voorbeeld de *externe BGPNAT adres* 10.10.0.62 is en de *interne IP-adres* 192.168.102.1 is.

   ```
   $ExtBgpNat = '<External BGPNAT address>'
   $IntBgpNat = '<Internal IP address>'

   # Designate the external NAT address for the ports that use the IKE authentication.
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress $Using:ExtBgpNat `
      -PortStart 499 `
      -PortEnd 501}
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatExternalAddress `
      -NatName BGPNAT `
      -IPAddress $Using:ExtBgpNat `
      -PortStart 4499 `
      -PortEnd 4501}
   # create a static NAT mapping to map the external address to the Gateway
   # Public IP Address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 500 `
      -InternalPort 500}
   # Finally, configure NAT traversal which uses port 4500 to
   # successfully establish the complete IPSEC tunnel over NAT devices
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 4500 `
      -InternalPort 4500}
   ```

## <a name="configure-azure"></a>Azure configureren
Nu dat u de Azure-Stack-configuratie hebt voltooid, kunt u sommige Azure-resources kunt implementeren. Het volgende diagram toont een voorbeeld-tenant virtuele netwerk in Azure. U kunt een naam en het adresseringsschema gebruiken voor uw VNet in Azure. Echter, het adresbereik van de VNets in Azure en Azure-Stack moet uniek en niet overlappen.

![Azure Vnets](media/azure-stack-connect-expressroute/AzureArchitecture.png)

**Diagram 3**

De resources die u in Azure implementeert zijn vergelijkbaar met de resources die u hebt geïmplementeerd in Azure-Stack. Op dezelfde manier implementeren:
* Virtuele netwerken en subnetten
* Een gatewaysubnet
* Een virtuele netwerkgateway
* Een verbinding
* Een ExpressRoute-circuit

De voorbeeld-netwerk van Azure-infrastructuur is geconfigureerd in de volgende manier:
* Een standaard (192.168.2.0/24)-hub en spoke (10.100.0.0./16) VNet model wordt gebruikt.
* De werkbelastingen zijn geïmplementeerd in de spoke Vnet en het ExpressRoute-circuit is verbonden met de hub VNet.
* De twee VNets die zijn gekoppeld met de functie van VNet-peering.

### <a name="configure-vnets"></a>Vnet's configureren
1. Aanmelden bij de Azure-portal met uw Azure-referenties.
2. De hub met behulp van de adresruimte 192.168.2.0/24 VNet maken. Een subnet met het adresbereik 192.168.2.0/25 maken en een gatewaysubnet met behulp van het adresbereik 192.168.2.128/27 toevoegen.
3. Maken van de spoke VNet en een subnet met de 10.100.0.0/16-adresbereik.


Zie voor meer informatie over het maken van virtuele netwerken in Azure [een virtueel netwerk maken met meerdere subnetten](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

### <a name="configure-an-expressroute-circuit"></a>Een ExpressRoute-circuit configureren

1. Raadpleeg de vereisten voor ExpressRoute in [ExpressRoute vereisten en Controlelijst voor](../expressroute/expressroute-prerequisites.md).
2. Volg de stappen in [maken en een ExpressRoute-circuit wijzigen](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) voor het maken van een ExpressRoute-circuit met behulp van uw Azure-abonnement.
3. De sleutel van de vorige stap delen met uw hoster/provider voor het inrichten van uw ExpressRoute-circuit aan het einde.
4. Volg de stappen in [maken en wijzigen van de peering voor een ExpressRoute-circuit](../expressroute/expressroute-howto-routing-portal-resource-manager.md) privépeering op het ExpressRoute-circuit configureren.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken

* Volg de stappen in [configureren van een virtuele netwerkgateway voor ExpressRoute met behulp van PowerShell](../expressroute/expressroute-howto-add-gateway-resource-manager.md) maken van een virtuele netwerkgateway voor ExpressRoute in de hub VNet.

### <a name="create-the-connection"></a>De verbinding maken

* Als u wilt koppelen het ExpressRoute-circuit met de hub VNet, volg de stappen in [een virtueel netwerk verbinden met een ExpressRoute-circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).

### <a name="peer-the-vnets"></a>De Vnets peer

* Peer-Hub en spaak VNets met behulp van de stappen in [maken van een virtueel netwerk peering met de Azure portal](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md). Bij het configureren van VNet-peering, zorg ervoor dat u de volgende opties selecteren:
   * Van hub en spoke: **gateway onderweg toestaan**
   * Van spoke-hub: **externe gateway gebruiken**

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

* Uw werkbelasting van virtuele machines implementeren in de spoke VNet.

Herhaal deze stappen voor elke extra VNets die u wilt verbinding maken met hun respectieve ExpressRoute-circuits maken in Azure-tenant.

## <a name="configure-the-router"></a>De router configureren

U kunt het volgende infrastructuurdiagram voor end-to-end-gebruiken om te leiden van de configuratie van uw ExpressRoute Router. Dit diagram toont twee tenants (Tenant 1 en 2 van de Tenant) met hun respectieve Express Route-circuits. Elk is gekoppeld aan hun eigen VRF (virtuele Routering en doorsturen) in de LAN en WAN-zijde van de ExpressRoute-router om end-to-end-isolatie tussen de twee tenants. Noteer de IP-adressen in interfaces van de router uitvoeren van de voorbeeldconfiguratie.

![Einde diagram beëindigen](media/azure-stack-connect-expressroute/EndToEnd.png)

**Diagram 4**

U kunt een router die ondersteuning biedt voor IKEv2 VPN en BGP om te beëindigen van de Site-naar-Site VPN-verbinding uit Azure-Stack gebruiken. Dezelfde router wordt gebruikt om verbinding maken met Azure met behulp van een ExpressRoute-circuit. 

Hier volgt een voorbeeldconfiguratie van een Cisco ASR 1000 die ondersteuning biedt voor de netwerkinfrastructuur in Diagram 4 wordt weergegeven:

```
ip vrf Tenant 1
 description Routing Domain for PRIVATE peering to Azure for Tenant 1
 rd 1:1
!
ip vrf Tenant 2
 description Routing Domain for PRIVATE peering to Azure for Tenant 2
 rd 1:5
!
crypto ikev2 proposal V2-PROPOSAL2 
description IKEv2 proposal for Tenant 1 
encryption aes-cbc-256
 integrity sha256
 group 2
crypto ikev2 proposal V4-PROPOSAL2 
description IKEv2 proposal for Tenant 2 
encryption aes-cbc-256
 integrity sha256
 group 2
!
crypto ikev2 policy V2-POLICY2 
description IKEv2 Policy for Tenant 1 
match fvrf Tenant 1
 match address local 10.60.3.255
 proposal V2-PROPOSAL2
description IKEv2 Policy for Tenant 2
crypto ikev2 policy V4-POLICY2 
 match fvrf Tenant 2
 match address local 10.60.3.251
 proposal V4-PROPOSAL2
!
crypto ikev2 profile V2-PROFILE
description IKEv2 profile for Tenant 1 
match fvrf Tenant 1
 match address local 10.60.3.255
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 1
!
crypto ikev2 profile V4-PROFILE
description IKEv2 profile for Tenant 2
 match fvrf Tenant 2
 match address local 10.60.3.251
 match identity remote any
 authentication remote pre-share key abc123
 authentication local pre-share key abc123
 ivrf Tenant 2
!
crypto ipsec transform-set V2-TRANSFORM2 esp-gcm 256 
 mode tunnel
crypto ipsec transform-set V4-TRANSFORM2 esp-gcm 256 
 mode tunnel
!
crypto ipsec profile V2-PROFILE
 set transform-set V2-TRANSFORM2 
 set ikev2-profile V2-PROFILE
!
crypto ipsec profile V4-PROFILE
 set transform-set V4-TRANSFORM2 
 set ikev2-profile V4-PROFILE
!
interface Tunnel10
description S2S VPN Tunnel for Tenant 1
 ip vrf forwarding Tenant 1
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.211
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf Tenant 1
 tunnel protection ipsec profile V2-PROFILE
!
interface Tunnel20
description S2S VPN Tunnel for Tenant 2
 ip vrf forwarding Tenant 2
 ip address 11.0.0.2 255.255.255.252
 ip tcp adjust-mss 1350
 tunnel source TenGigabitEthernet0/1/0.213
 tunnel mode ipsec ipv4
 tunnel destination 10.10.0.62
 tunnel vrf VNET3
 tunnel protection ipsec profile V4-PROFILE
!
interface GigabitEthernet0/0/1
 description PRIMARY Express Route Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/1.100
description Primary WAN interface of Tenant 1
 description PRIMARY ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.1 255.255.255.252
!
interface GigabitEthernet0/0/1.102
description Primary WAN interface of Tenant 2
 description PRIMARY ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.17 255.255.255.252
!
interface GigabitEthernet0/0/2
 description BACKUP Express Route Link to AZURE over Equinix
 no ip address
 negotiation auto
!
interface GigabitEthernet0/0/2.100
description Secondary WAN interface of Tenant 1
 description BACKUP ER link supporting Tenant 1 to Azure
 encapsulation dot1Q 101
 ip vrf forwarding Tenant 1
 ip address 192.168.1.5 255.255.255.252
!
interface GigabitEthernet0/0/2.102
description Secondary WAN interface of Tenant 2 
description BACKUP ER link supporting Tenant 2 to Azure
 encapsulation dot1Q 102
 ip vrf forwarding Tenant 2
 ip address 192.168.1.21 255.255.255.252
!
interface TenGigabitEthernet0/1/0
 description Downlink to ---Port 1/47
 no ip address
!
interface TenGigabitEthernet0/1/0.211
 description LAN interface of Tenant 1
description Downlink to --- Port 1/47.211
 encapsulation dot1Q 211
 ip vrf forwarding Tenant 1
 ip address 10.60.3.255 255.255.255.254
!
interface TenGigabitEthernet0/1/0.213
description LAN interface of Tenant 2
 description Downlink to --- Port 1/47.213
 encapsulation dot1Q 213
 ip vrf forwarding Tenant 2
 ip address 10.60.3.251 255.255.255.254
!
router bgp 65530
 bgp router-id <removed>
 bgp log-neighbor-changes
 description BGP neighbor config and route advertisement for Tenant 1 VRF 
 address-family ipv4 vrf Tenant 1
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.254 mask 255.255.255.254
  network 192.168.1.0 mask 255.255.255.252
  network 192.168.1.4 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65100
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 1
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.254 remote-as 4232570301
  neighbor 10.60.3.254 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.254 activate
  neighbor 10.60.3.254 route-map BLOCK-ALL out
  neighbor 192.168.1.2 remote-as 12076
  neighbor 192.168.1.2 description PRIMARY ER peer for Tenant 1 to Azure
  neighbor 192.168.1.2 ebgp-multihop 5
  neighbor 192.168.1.2 activate
  neighbor 192.168.1.2 soft-reconfiguration inbound
  neighbor 192.168.1.2 route-map Tenant 1-ONLY out
  neighbor 192.168.1.6 remote-as 12076
  neighbor 192.168.1.6 description BACKUP ER peer for Tenant 1 to Azure
  neighbor 192.168.1.6 ebgp-multihop 5
  neighbor 192.168.1.6 activate
  neighbor 192.168.1.6 soft-reconfiguration inbound
  neighbor 192.168.1.6 route-map Tenant 1-ONLY out
  maximum-paths 8
 exit-address-family
 !
description BGP neighbor config and route advertisement for Tenant 2 VRF 
address-family ipv4 vrf Tenant 2
  network 10.1.0.0 mask 255.255.0.0
  network 10.60.3.250 mask 255.255.255.254
  network 192.168.1.16 mask 255.255.255.252
  network 192.168.1.20 mask 255.255.255.252
  neighbor 10.10.0.62 remote-as 65300
  neighbor 10.10.0.62 description VPN-BGP-PEER-for-Tenant 2
  neighbor 10.10.0.62 ebgp-multihop 5
  neighbor 10.10.0.62 activate
  neighbor 10.60.3.250 remote-as 4232570301
  neighbor 10.60.3.250 description LAN peer for CPEC:INET:2112 VRF
  neighbor 10.60.3.250 activate
  neighbor 10.60.3.250 route-map BLOCK-ALL out
  neighbor 192.168.1.18 remote-as 12076
  neighbor 192.168.1.18 description PRIMARY ER peer for Tenant 2 to Azure
  neighbor 192.168.1.18 ebgp-multihop 5
  neighbor 192.168.1.18 activate
  neighbor 192.168.1.18 soft-reconfiguration inbound
  neighbor 192.168.1.18 route-map VNET-ONLY out
  neighbor 192.168.1.22 remote-as 12076
  neighbor 192.168.1.22 description BACKUP ER peer for Tenant 2 to Azure
  neighbor 192.168.1.22 ebgp-multihop 5
  neighbor 192.168.1.22 activate
  neighbor 192.168.1.22 soft-reconfiguration inbound
  neighbor 192.168.1.22 route-map VNET-ONLY out
  maximum-paths 8
 exit-address-family
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$
ip route vrf Tenant 1 10.1.0.0 255.255.0.0 Tunnel10
ip route vrf Tenant 2 10.1.0.0 255.255.0.0 Tunnel20
!
ip prefix-list BLOCK-ALL seq 5 deny 0.0.0.0/0 le 32
!
route-map BLOCK-ALL permit 10
 match ip address prefix-list BLOCK-ALL
!
route-map VNET-ONLY permit 10
 match as-path 1
!
```

## <a name="test-the-connection"></a>De verbinding testen

Test de verbinding na het instellen van de Site-naar-Site-verbinding en het ExpressRoute-circuit. Deze taak is eenvoudig.  Meld u aan bij een van de virtuele machines die u hebt gemaakt in uw Azure VNet en ping van de virtuele machine die u hebt gemaakt in de Azure-Stack-omgeving, en vice versa. 

Om te controleren of dat u het verkeer via de Site-naar-Site en een ExpressRoute-verbindingen wilt verzenden, moet u een ping de toegewijde IP (DIP)-adres van de virtuele machine aan beide uiteinden en niet het VIP-adres van de virtuele machine. U moet dus, zoeken en noteer het adres op het andere einde van de verbinding.

### <a name="allow-icmp-in-through-the-firewall"></a>Toestaan van ICMP in via de firewall
Standaard staat Windows Server 2016 ICMP-pakketten in via de firewall niet toe. Dus voor elke virtuele machine die in de test u de volgende cmdlet uitvoeren in een PowerShell-venster met verhoogde bevoegdheid:


   ```
   New-NetFirewallRule `
    –DisplayName “Allow ICMPv4-In” `
    –Protocol ICMPv4
   ```

### <a name="ping-the-azure-stack-virtual-machine"></a>Ping de Stack Azure virtuele machine

1. Aanmelden bij de gebruikersportal van Azure-Stack met behulp van een tenantaccount.
2. Klik in de linkernavigatiebalk op **Virtuele machines**.
3. De virtuele machine gevonden die u eerder hebt gemaakt en klik erop.
4. Klik in de sectie voor de virtuele machine op **Connect**.
5. Open een verhoogde PowerShell-venster en typ **ipconfig/all**.
6. Het IPv4-adres vinden in de uitvoer en noteer deze. Dit adres van de virtuele machine in de Azure-VNet ping. In de voorbeeldomgeving is het adres van het subnet 10.1.1.x/24. In uw omgeving, kan het adres afwijken. Echter, moet binnen het subnet dat u hebt gemaakt voor de Tenant-VNet-subnet.


### <a name="view-data-transfer-statistics"></a>Statistieken voor gegevensoverdracht weergeven

Als u weten hoeveel verkeer dat via de verbinding is doorgegeven wilt, kunt u deze informatie in de sectie verbinding vinden in de gebruikersportal van Azure-Stack. Deze informatie is ook een andere goede manier om te verifiëren dat het pingen die u zojuist hebt verzonden daadwerkelijk is gegaan via de VPN- en ExpressRoute-verbindingen.

1. Aanmelden bij de gebruikersportal van Microsoft Azure-Stack met behulp van de tenant-serviceaccount.
2. Navigeer naar de resourcegroep waar uw VPN-Gateway is gemaakt en selecteer de **verbindingen** -objecttype.
3. Klik op de **ConnectToAzure** verbinding in de lijst.
4. Op de **verbinding** sectie ziet u statistieken voor **gegevens in** en **gegevensuitvoer**. Als het goed is, ziet u hier enkele waarden die niet nul zijn.

   ![Gegevens In de gegevensuitvoer](media/azure-stack-connect-expressroute/DataInDataOut.png)

## <a name="next-steps"></a>Volgende stappen
[Apps implementeren op Azure en Azure Stack](azure-stack-solution-pipeline.md)
