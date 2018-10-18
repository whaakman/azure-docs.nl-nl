---
title: Verbinding maken met behulp van ExpressRoute met Azure Stack
description: Leer hoe u virtuele netwerken in Azure Stack verbinden met virtuele netwerken in Azure met behulp van ExpressRoute.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: c30e70802d125744432f428f903f6ac6789f631e
ms.sourcegitcommit: f20e43e436bfeafd333da75754cd32d405903b07
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49389222"
---
# <a name="connect-azure-stack-to-azure-using-azure-expressroute"></a>Verbinding maken met Azure Stack met behulp van Azure ExpressRoute

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

In dit artikel wordt beschreven hoe u een virtueel netwerk van Azure Stack verbinding met een Azure-netwerk via een rechtstreekse verbinding met Microsoft Azure ExpressRoute.

U kunt in dit artikel gebruiken als een onderdeel van de zelfstudie en de voorbeelden gebruiken voor het instellen van de dezelfde testomgeving. Of u kunt het artikel gebruiken als een procedure die u begeleidt bij het instellen van uw eigen omgeving ExpressRoute.

## <a name="overview-assumptions-and-prerequisites"></a>Overzicht, uitgangspunten en vereisten

Azure ExpressRoute kunt u uw on-premises netwerken in de Microsoft cloud uitbreiden via een persoonlijke verbinding die door een connectiviteitsprovider worden verstrekt. ExpressRoute is niet een VPN-verbinding via het openbare Internet.

Zie voor meer informatie over Azure ExpressRoute, de [overzicht van ExpressRoute](../expressroute/expressroute-introduction.md).

### <a name="assumptions"></a>Veronderstellingen

In dit artikel wordt ervan uitgegaan dat:

* U hebt enige basiskennis van Azure.
* Hebt u een basiskennis hebt van Azure Stack.
* Hebt u een basiskennis hebt van netwerken.

### <a name="prerequisites"></a>Vereisten

Als u wilt verbinding maken met Azure Stack en met behulp van ExpressRoute, moet u voldoen aan de volgende vereisten:

* Een ingerichte [ExpressRoute-circuit](../expressroute/expressroute-circuit-peerings.md) via een [connectiviteitsprovider](../expressroute/expressroute-locations.md).
* Een Azure-abonnement een ExpressRoute-circuit en de vnet's maken in Azure.
* Een router die moet:
  * Ondersteuning voor Site-naar-Site VPN-verbindingen tussen de LAN-interface en Azure Stack-Multitenant-Gateway.
  * Ondersteuning voor het maken van meerdere VRFs (virtuele Routering en doorsturen via) als er meer dan één tenant in uw Azure Stack-implementatie.
* Een router die is:
  * Een WAN-poort is verbonden met het ExpressRoute-circuit.
  * Een LAN-poort is verbonden met de Azure Stack-Multitenant-Gateway.

### <a name="expressroute-network-architecture"></a>ExpressRoute-netwerkarchitectuur

Het volgende diagram toont de Azure Stack en de Azure-omgevingen nadat u klaar bent met het instellen van ExpressRoute met behulp van de voorbeelden in dit artikel.

*Afbeelding 1. ExpressRoute-netwerk*

![ExpressRoute-netwerk](media/azure-stack-connect-expressroute/Conceptual.png)

De volgende Architectuurdiagram ziet u hoe meerdere tenants verbinding maken vanaf de Azure Stack-infrastructuur via de router voor ExpressRoute met Azure in de aan de rand van Microsoft.

*Afbeelding 2. Verbindingen van meerdere tenants*

![Verbindingen van meerdere tenants met ExpressRoute](media/azure-stack-connect-expressroute/Architecture.png)

Het voorbeeld in dit artikel wordt de dezelfde multitenant-architectuur die wordt weergegeven *afbeelding 2* Azure Stack verbinden met Azure met behulp van ExpressRoute-privépeering. Dit heeft gedaan met behulp van een Site-naar-Site VPN-verbinding van de virtuele netwerkgateway in Azure Stack aan een ExpressRoute-router.

De stappen in dit artikel ziet u het maken van een end-to-end-verbinding tussen twee VNets uit twee verschillende tenants in Azure Stack met bijbehorende vnet's in Azure. Instellen van twee tenants is optioneel en u kunt deze stappen ook gebruiken voor één tenant.

## <a name="configure-azure-stack"></a>Configureren van Azure Stack

Als u de Azure Stack-omgeving instelt voor de eerste tenant, door de stappen in het volgende diagram als richtlijn te gebruiken. Als u meer dan één tenant instelt, herhaalt u deze stappen.

>[!NOTE]
>Deze stappen laten zien hoe u resources met behulp van de Azure Stack-portal maakt, maar u kunt ook PowerShell gebruiken.

![Azure Stack-netwerk instellen](media/azure-stack-connect-expressroute/image2.png)

### <a name="before-you-begin"></a>Voordat u begint

Voordat u begint met het configureren van Azure Stack, hebt u het volgende nodig:

* De implementatie van een geïntegreerde Azure Stack-systeem of de implementatie van een Azure Stack Development Kit (ASDK). Zie voor meer informatie over het implementeren van de ASDK de [Azure Stack Development Kit implementatiesnelstart](azure-stack-deploy-overview.md).
* Een aanbieding in Azure Stack die uw gebruikers kunnen zich abonneren op. Zie voor meer informatie, [plannen, aanbiedingen en abonnementen](azure-stack-plan-offer-quota-overview.md).

### <a name="create-network-resources-in-azure-stack"></a>Netwerkbronnen maken in Azure Stack

Gebruik de volgende procedures om u te maken van de vereiste netwerkresources in Azure Stack voor een tenant.

#### <a name="create-the-virtual-network-and-vm-subnet"></a>Het virtuele netwerk en VM-subnet maken

1. Meld u aan bij de gebruikersportal aanmeldt met een gebruikersaccount (tenant).
1. Selecteer in de portal **+ een resource maken**.

1. Onder **Azure Marketplace**, selecteer **netwerken**.

1. Onder **aanbevolen**, selecteer **virtueel netwerk**.

1. Onder **virtueel netwerk maken**, voert u de waarden die in de volgende tabel in de juiste velden worden weergegeven.

   |Veld  |Waarde  |
   |---------|---------|
   |Naam     |Tenant1VNet1         |
   |Adresruimte     |10.1.0.0/16|
   |Subnetnaam     |Tenant1-Sub1|
   |Subnetadresbereik     |10.1.1.0/24|

1. In het veld **Abonnement** wordt nu het abonnement weergegeven dat u eerder hebt ingevuld. Voor de resterende velden:

    * Onder **resourcegroep**, selecteer **nieuw** te maken van een nieuwe resource-groep of als u nog niet hebt, selecteert u **gebruik bestaande**.
    * Controleer of de standaard **locatie**.
    * Selecteer **Maken**.
    * (Optioneel) Selecteer **vastmaken aan dashboard**.

#### <a name="create-the-gateway-subnet"></a>Her gatewaysubnet maken

1. Onder **virtueel netwerk**, Tenant1VNet1 selecteren.
1. Selecteer onder **INSTELLINGEN** **Subnetten**.
1. Selecteer **+ gatewaysubnet** een gatewaysubnet toevoegen aan het virtuele netwerk.
1. De naam van het subnet is standaard ingesteld op **Gatewaysubnet**. Gateway-subnetten vormen een speciaal geval, en moeten deze naam gebruiken om u te laten functioneren.
1. Controleer de **adresbereik** is **10.1.0.0/24**.
1. Selecteer **OK** om het gatewaysubnet te maken.

#### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken

1. Selecteer in de gebruikersportal van Azure Stack **+ een resource maken**.
1. Onder **Azure Marketplace**, selecteer **netwerken**.
1. Selecteer **Gateway van het virtuele netwerk** in de lijst met netwerkresources.
1. In de **naam** veld **GW1**.
1. Selecteer **virtueel netwerk**.
1. Selecteer **Tenant1VNet1** uit de vervolgkeuzelijst.
1. Selecteer **openbaar IP-adres**>**openbare IP-adres kiezen**, en selecteer vervolgens **nieuw**.
1. In de **naam** veld **GW1-PiP** en selecteer **OK**.
1. Bij **VPN-type** moet standaard **Op basis van route** zijn geselecteerd. Behoud deze instelling.
1. Controleer of **Abonnement** en **Locatie** juist zijn. Selecteer **Maken**.

#### <a name="create-the-local-network-gateway"></a>De lokale netwerkgateway maken

De lokale netwerkgatewayresource identificeert de RAS-gateway op het andere uiteinde van de VPN-verbinding. In dit voorbeeld is de externe kant van de verbinding de Hiermee LAN van de router voor ExpressRoute. Voor Tenant-1, wordt weergegeven in *afbeelding 2*, het externe adres 10.60.3.255 is.

1. Meld u aan bij de gebruikersportal van Azure Stack met uw gebruikersaccount en selecteer **+ een resource maken**.
1. Onder **Azure Marketplace**, selecteer **netwerken**.
1. Selecteer **lokale netwerkgateway** in de lijst met resources.
1. In de **naam** veld **ER-Router-GW**.
1. Voor de **IP-adres** veld, raadpleeg dan *afbeelding 2*. Het IP-adres van de ExpressRoute-router LAN Hiermee voor Tenant-1 is 10.60.3.255. Voer het IP-adres van de bijbehorende routerinterface voor uw eigen omgeving.
1. In de **adresruimte** en voer de adresruimte van de vnet's die u wilt verbinden in Azure. De subnetten voor Tenant-1 in *afbeelding 2* zijn:

   * 192.168.2.0/24 is de hub VNet in Azure.
   * 10.100.0.0/16 is het knooppunt VNet in Azure.

   > [!IMPORTANT]
   > In dit voorbeeld wordt ervan uitgegaan dat u statische routes voor de Site-naar-Site VPN-verbinding tussen de Azure Stack-gateway en de router voor ExpressRoute gebruiken.

1. Controleer uw **abonnement**, **resourcegroep**, en **locatie** juist zijn. Selecteer **Maken**.

#### <a name="create-the-connection"></a>De verbinding maken

1. Selecteer in de gebruikersportal van Azure Stack **+ een resource maken**.
1. Onder **Azure Marketplace**, selecteer **netwerken**.
1. Selecteer **Verbinding** in de lijst met resources.
1. Onder **basisbeginselen**, kiest u **Site-naar-site (IPSec)** als de **verbindingstype**.
1. Selecteer de **abonnement**, **resourcegroep**, en **locatie**. Selecteer **OK**.
1. Onder **instellingen**, selecteer **gateway van virtueel netwerk**, en selecteer vervolgens **GW1**.
1. Selecteer **lokale netwerkgateway**, en selecteer vervolgens **ER Router GW**.
1. In de **verbindingsnaam** veld **ConnectToAzure**.
1. In de **gedeelde sleutel (PSK)** veld **abc123** en selecteer vervolgens **OK**.
1. Onder **samenvatting**, selecteer **OK**.

**Het virtuele netwerk openbare IP-adres ophalen**

Nadat u de gateway van virtueel netwerk hebt gemaakt, kunt u het openbare IP-adres van de gateway kunt ophalen. Noteer dit adres in het geval u deze later nodig voor uw implementatie. Afhankelijk van uw implementatie, dit adres wordt gebruikt als de ***intern IP-adres***.

1. Selecteer in de gebruikersportal van Azure Stack **alle resources**.
1. Onder **alle resources**, selecteert u de virtuele netwerkgateway, is **GW1** in het voorbeeld.
1. Onder **gateway van virtueel netwerk**, selecteer **overzicht** uit de lijst met resources. U kunt ook selecteren **eigenschappen**.
1. Het IP-adres dat u wilt Houd er rekening mee wordt vermeld onder **openbaar IP-adres**. Voor de voorbeeldconfiguratie van het is dit adres 192.68.102.1.

#### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Als u wilt testen gegevensverkeer via de VPN-verbinding, moet u virtuele machines te verzenden en ontvangen van gegevens in de Azure Stack-VNet. Een virtuele machine maken en deze implementeren in het VM-subnet voor het virtuele netwerk.

1. Selecteer in de gebruikersportal van Azure Stack **+ een resource maken**.
1. Onder **Azure Marketplace**, selecteer **Compute**.
1. Selecteer in de lijst met installatiekopieën van virtuele machines, de **Windows Server 2016 Datacenter Eval** installatiekopie.

   >[!NOTE]
   >Als de installatiekopie die wordt gebruikt voor dit artikel niet beschikbaar is, vraagt u uw Azure Stack-operators voor een andere Windows Server-installatiekopie.

1. In **maken van virtuele machine**>**basisbeginselen**, voer **VM01** als de **naam**.
1. Voer een geldige gebruikersnaam en wachtwoord. U gebruikt dit account aanmelden bij de virtuele machine nadat deze gemaakt.
1. Geef een **abonnement**, **resourcegroep**, en een **locatie**. Selecteer **OK**.
1. Onder **Kies een grootte**, selecteert u een VM-grootte voor dit exemplaar en selecteer vervolgens **Selecteer**.
1. Onder **instellingen**, controleert u of:

   * Het virtuele netwerk is **Tenant1VNet1**.
   * Het subnet is ingesteld op **10.1.1.0/24**.

   Gebruik de standaardinstellingen en selecteer **OK**.

1. Onder **samenvatting**, controleert u de configuratie van de virtuele machine en selecteer vervolgens **OK**.

>[!NOTE]
>
>Als u wilt toevoegen van meer tenants, Herhaal de stappen die u in deze secties:
>
>* Het virtuele netwerk en VM-subnet maken
>* Her gatewaysubnet maken
>* De gateway van het virtuele netwerk maken
>* De lokale netwerkgateway maken
>* De verbinding maken
>* Een virtuele machine maken
>
>Als u verder Tenant 2 als voorbeeld gebruiken, vergeet dan niet te wijzigen van de IP-adressen om te voorkomen dat overlapt.

### <a name="configure-the-nat-virtual-machine-for-gateway-traversal"></a>De NAT-virtuele machine voor gateway-traversal configureren

> [!IMPORTANT]
> In deze sectie is alleen voor implementaties van Azure Stack Development Kit. De NAT niet nodig is voor implementaties met meerdere knooppunten.

De Azure Stack Development Kit is zichzelf en is geïsoleerd van het netwerk waar de fysieke host is geïmplementeerd. Het VIP-netwerk dat de gateways zijn verbonden met niet extern is, het verborgen achter een NAT (Network Address Translation) doen router.

De router is een Windows Server virtuele machine (AzS-BGPNAT01) met de rol van de Routing and Remote Access Services (RRAS). U moet NAT configureren op de AzS-BGPNAT01-machine om in te schakelen van de Site-naar-Site VPN-verbinding aan beide kanten.

#### <a name="configure-the-nat"></a>De NAT configureren

1. Aanmelden bij de Azure Stack-hostcomputer met uw beheerdersaccount.
1. Kopiëren en bewerken van de volgende PowerShell-script.  Vervang `"<your administrator password>"` met uw administrator-wachtwoord, en voer het script in een verhoogde PowerShell ISE. Met dit script retourneert de *externe BGPNAT adres*.

   ```PowerShell
   cd \AzureStack-Tools-master\connect
   Import-Module .\AzureStack.Connect.psm1
   $Password = ConvertTo-SecureString "<your administrator password>" `
    -AsPlainText `
    -Force
   Get-AzureStackNatServerAddress `
    -HostComputer "azs-bgpnat01" `
    -Password $Password

   ```

1. De NAT configureren, kopiëren en bewerken van de volgende PowerShell-script. Bewerk het script te vervangen door de `'<External BGPNAT address>'` en `'<Internal IP address>'` met de volgende voorbeelden van waarden:

   * Voor *externe BGPNAT adres* 10.10.0.62 gebruiken
   * Voor *intern IP-adres* 192.168.102.1 gebruiken

   Voer het volgende script vanaf een verhoogde PowerShell ISE:

   ```PowerShell
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
   # Create a static NAT mapping to map the external address to the Gateway public IP address to map the ISAKMP port 500 for PHASE 1 of the IPSEC tunnel.
   Invoke-Command `
    -ComputerName azs-bgpnat01 `
     {Add-NetNatStaticMapping `
      -NatName BGPNAT `
      -Protocol UDP `
      -ExternalIPAddress $Using:ExtBgpNat `
      -InternalIPAddress $Using:IntBgpNat `
      -ExternalPort 500 `
      -InternalPort 500}
   # Configure NAT traversal which uses port 4500 to  establish the complete IPSEC tunnel over NAT devices.
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

Nadat u klaar bent met het configureren van Azure Stack, kunt u de Azure-resources kunt implementeren. Het volgende diagram toont een voorbeeld van een virtueel tenantnetwerk in Azure. U kunt een naam en het adresschema gebruiken voor uw VNet in Azure. Het adresbereik van de vnet's in Azure en Azure Stack moet echter uniek zijn en niet overlappen.

*Afbeelding 3. Azure VNets*

![Azure VNets](media/azure-stack-connect-expressroute/AzureArchitecture.png)

De resources die u in Azure implementeert zijn vergelijkbaar met de resources die u hebt geïmplementeerd in Azure Stack. Implementeert u de volgende onderdelen:

* Virtuele netwerken en subnetten
* Een gateway-subnet
* Een virtuele netwerkgateway
* Een verbinding
* Een ExpressRoute-circuit

Het voorbeeld van de Azure-netwerkinfrastructuur is als volgt geconfigureerd:

* Een standaard (192.168.2.0/24)-hub en spoke (10.100.0.0./16) VNet-model. Zie voor meer informatie over een ster-netwerktopologie [een ster-netwerktopologie implementeren in Azure](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke).
* De werkbelastingen in het knooppunt VNet zijn geïmplementeerd en het ExpressRoute-circuit is verbonden met de hub VNet.
* De twee vnet's zijn verbonden met behulp van VNet-peering.

### <a name="configure-the-azure-vnets"></a>Het Azure vnet's configureren

1. Meld u aan bij de Azure portal met uw Azure-referenties.
1. De hub VNet met behulp van het adresbereik 192.168.2.0/24 maken.
1. Maak een subnet met behulp van het adresbereik 192.168.2.0/25 en een gatewaysubnet met behulp van het adresbereik 192.168.2.128/27 toevoegen.
1. Maken van het knooppunt VNet en subnet met behulp van de 10.100.0.0/16-adresbereik.

Zie voor meer informatie over het maken van virtuele netwerken in Azure [maken van een virtueel netwerk](../virtual-network/manage-virtual-network.md#create-a-virtual-network).

### <a name="configure-an-expressroute-circuit"></a>Een ExpressRoute-circuit configureren

1. Controleer de vereisten voor ExpressRoute in [ExpressRoute vereisten en Controlelijst voor](../expressroute/expressroute-prerequisites.md).

1. Volg de stappen in [maken en aanpassen van een ExpressRoute-circuit](../expressroute/expressroute-howto-circuit-portal-resource-manager.md) te maken van een ExpressRoute-circuit met behulp van uw Azure-abonnement.

   >[!NOTE]
   >Geef de servicesleutel voor uw circuit aan uw service, zodat ze uw ExpressRoute-circuit aan het einde kunnen instellen.

1. Volg de stappen in [maken en wijzigen van de peering voor een ExpressRoute-circuit](../expressroute/expressroute-howto-routing-portal-resource-manager.md) privépeering op het ExpressRoute-circuit configureren.

### <a name="create-the-virtual-network-gateway"></a>De gateway van het virtuele netwerk maken

Volg de stappen in [een virtuele netwerkgateway configureren voor ExpressRoute met behulp van PowerShell](../expressroute/expressroute-howto-add-gateway-resource-manager.md) een virtuele netwerkgateway maken voor ExpressRoute in de hub VNet.

### <a name="create-the-connection"></a>De verbinding maken

Als u wilt het ExpressRoute-circuit koppelen met de hub VNet, volg de stappen in [een virtueel netwerk verbinden met een ExpressRoute-circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).

### <a name="peer-the-vnets"></a>De VNets instellen als peers

De hub koppelen en knooppunt VNets met behulp van de stappen in [een virtueel netwerk-peering maken met Azure portal](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md). Bij het configureren van VNet-peering, zorg ervoor dat u de volgende opties:

* Van de hub voor het knooppunt **gatewayoverdracht toestaan**.
* Vanuit het knooppunt naar de hub **externe gateway gebruiken**.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

Implementeer uw werkbelasting van virtuele machines in het knooppunt VNet.

Herhaal deze stappen voor elke extra tenant vnet's die u wilt verbinding maken in Azure via de desbetreffende ExpressRoute-circuits.

## <a name="configure-the-router"></a>De router configureren

U kunt de volgende *ExpressRoute routerconfiguratie* diagram als richtlijn voor het configureren van uw ExpressRoute-Router. In dit diagram ziet twee tenants (Tenant-1 en 2 van de Tenant) met de desbetreffende ExpressRoute-circuits. Elke tenant is gekoppeld aan hun eigen VRF (virtuele Routering en doorsturen via) in de LAN en WAN-zijde van de router voor ExpressRoute. Deze configuratie zorgt ervoor dat de end-to-end-isolatie tussen de twee tenants. Noteer de IP-adressen in de routerinterfaces gebruikt als u de van configuratievoorbeeld volgen.

*Afbeelding 4. De configuratie van de ExpressRoute-router*

![De configuratie van de ExpressRoute-router](media/azure-stack-connect-expressroute/EndToEnd.png)

U kunt elke router die ondersteuning biedt voor IKEv2 VPN en BGP voor het beëindigen van de Site-naar-Site VPN-verbinding van Azure Stack kunt gebruiken. Dezelfde router wordt gebruikt om te verbinden met behulp van een ExpressRoute-circuit.

De volgende configuratievoorbeeld van Cisco ASR 1000 reeks aggregatie Services Router ondersteunt de infrastructuur van het netwerk wordt weergegeven in de *ExpressRoute routerconfiguratie* diagram.

**Configuratievoorbeeld van Cisco ASR-1000**

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
 description PRIMARY ExpressRoute Link to AZURE over Equinix
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
 description BACKUP ExpressRoute Link to AZURE over Equinix
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

Test de verbinding nadat u de Site-naar-Site-verbinding en het ExpressRoute-circuit tot stand brengen.

Voer de volgende ping-tests:

* Aanmelden bij een van de virtuele machines in uw Azure VNet en ping van de virtuele machine die u hebt gemaakt in Azure Stack.
* Aanmelden bij een van de virtuele machines die u hebt gemaakt in Azure Stack en de virtuele machine die u hebt gemaakt in het Azure-VNet-ping.

>[!NOTE]
>Als u wilt controleren of dat u bent verkeer verzenden via de Site-naar-Site en ExpressRoute-verbindingen, moet u het toegewezen IP (DIP)-adres van de virtuele machine aan beide uiteinden en niet het VIP-adres van de virtuele machine te pingen.

### <a name="allow-icmp-in-through-the-firewall"></a>ICMP in via de firewall toestaan

Standaard kunnen Windows Server 2016 geen binnenkomende ICMP-pakketten via de firewall. Voor elke virtuele machine die u voor ping-tests gebruikt die u wilt toestaan van inkomende ICMP-pakketten. Voor het maken van een firewallregel voor ICMP, moet u de volgende cmdlet uitvoeren in een PowerShell-venster met verhoogde bevoegdheid:

```PowerShell
# Create ICMP firewall rule.
New-NetFirewallRule `
  –DisplayName “Allow ICMPv4-In” `
  –Protocol ICMPv4

```

### <a name="ping-the-azure-stack-virtual-machine"></a>Ping de virtuele machine van Azure Stack

1. Meld u aan bij de gebruikersportal van Azure Stack met behulp van een tenantaccount.

1. Zoek de virtuele machine die u hebt gemaakt en selecteer de virtuele machine.

1. Selecteer **Verbinden**.

1. Voer vanaf een opdrachtprompt met verhoogde bevoegdheid Windows- of PowerShell, **ipconfig/all**. Noteer het IPv4-adres in de uitvoer geretourneerd.

1. Ping de IPv4-adres van de virtuele machine in het Azure-VNet.

   In de voorbeeldomgeving is het IPv4-adres van het subnet 10.1.1.x/24. In uw omgeving, kan het adres afwijken. Maar deze moet in het subnet dat u hebt gemaakt voor de tenant-VNet-subnet.

### <a name="view-data-transfer-statistics"></a>Gegevens overbrengen-statistieken weergeven

Als u weten hoeveel verkeer wordt doorgegeven via de verbinding wilt, kunt u deze informatie op de gebruikersportal van Azure Stack kunt vinden. Dit is ook een goede manier om erachter te komen of uw testgegevens ping doorlopen van de VPN- en ExpressRoute-verbindingen.

1. Meld u aan bij de gebruikersportal van Azure Stack met behulp van uw tenantaccount en selecteer **alle resources**.
1. Navigeer naar de resourcegroep voor uw VPN-Gateway en selecteer de **verbinding** objecttype.
1. Selecteer de **ConnectToAzure** verbinding in de lijst.
1. Onder **verbindingen**>**overzicht**, ziet u statistieken voor **gegevens in** en **uitgaande gegevens**. Hier ziet u enkele waarden dan nul.

   ![Gegevens In en uitgaande gegevens](media/azure-stack-connect-expressroute/DataInDataOut.png)

## <a name="next-steps"></a>Volgende stappen

[Apps implementeren op Azure en Azure Stack](azure-stack-solution-pipeline.md)
