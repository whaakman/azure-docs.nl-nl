---
title: Veelgestelde vragen over virtuele Azure-netwerk | Microsoft Docs
description: Antwoorden op veelgestelde vragen over Microsoft Azure virtuele netwerken.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 54bee086-a8a5-4312-9866-19a1fba913d0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.openlocfilehash: 8800dc59306c349daba8f4d9703e0c713eed06ec
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Veelgestelde vragen (FAQ) virtuele Azure-netwerk

## <a name="virtual-network-basics"></a>Basisbeginselen van het virtuele netwerk

### <a name="what-is-an-azure-virtual-network-vnet"></a>Wat is een Azure-netwerk (VNet)?
Een Azure-netwerk (VNet) is een weergave van uw eigen netwerk in de cloud. Het is een logische isolatie van de Azure-cloud die is toegewezen aan uw abonnement. U kunt gebruiken VNets om in te richten en virtuele particuliere netwerken (VPN's) in Azure beheren en, desgewenst, koppelt de VNets met andere vnet's in Azure of met uw on-premises IT-infrastructuur om hybride of cross-premises oplossingen te maken. Elke VNet die u maakt, heeft een eigen CIDR-blok en kan worden gekoppeld aan andere vnet's en on-premises netwerken, zolang de CIDR-blokken niet overlappen. U hebt ook controle van DNS-serverinstellingen voor VNets en segmentering van het VNet in subnetten.

Gebruik de VNets aan:

* Een speciale persoonlijke cloudconfiguratie VNet soms u geen configuratie van een cross-premises nodig hebt voor uw oplossing maken. Wanneer u een VNet maakt, uw services en virtuele machines binnen uw VNet kunnen rechtstreeks en veilig communiceren met elkaar in de cloud. U kunt nog steeds endpoint-verbindingen voor de virtuele machines en services waarvoor internetcommunicatie als onderdeel van uw oplossing configureren.
* Veilig uitbreiden van uw datacenter met vnet's, kunt u traditionele site-naar-site (S2S) VPN-verbindingen als u wilt schalen veilig de capaciteit van uw datacenter. IPSEC S2S-VPN's gebruiken voor een beveiligde verbinding tussen uw zakelijke VPN-gateway en Azure.
* Schakel hybride scenario's voor de VNets bieden u de flexibiliteit om te ondersteunen van een bereik van hybride cloud-scenario's. Veilig kunt u cloud-gebaseerde toepassingen met elk type on-premises systeem zoals mainframes en Unix-systemen.

### <a name="how-do-i-get-started"></a>Hoe ga ik aan de slag?
Ga naar de [documentatie van uw virtuele netwerk](https://docs.microsoft.com/azure/virtual-network/) aan de slag. Deze inhoud biedt een overzicht en implementatie-informatie voor alle functies van VNet.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Kan ik VNets zonder cross-premises connectiviteit gebruiken?
Ja. U kunt een VNet gebruiken zonder verbinding maken met uw bedrijf. U kunt bijvoorbeeld Microsoft Windows Server Active Directory-domeincontrollers en SharePoint-farms uitsluitend in een Azure-VNet uitvoeren.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>Kan ik WAN-optimalisatie tussen VNets of een VNet en mijn on-premises Datacenter uitvoeren?

Ja. U kunt een [virtueel netwerkapparaat WAN-optimalisatie](https://azure.microsoft.com/marketplace/?term=wan+optimization) van verschillende leveranciers via de Azure Marketplace.

## <a name="configuration"></a>Configuratie

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Welke hulpprogramma's voor gebruik ik een VNet aanmaken?
U kunt de volgende hulpprogramma's gebruiken om te maken of een VNet configureren:

* Azure Portal
* PowerShell
* Azure-CLI
* Een netwerk-configuratiebestand (netcfg - voor klassieke vnet's). Zie de [configureren van een VNet met een netwerk configuratiebestand](virtual-networks-using-network-configuration-file.md) artikel.

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Welke adresbereiken kan ik gebruiken in mijn vnet's?
Alle IP-adresbereik is gedefinieerd in [RFC 1918](http://tools.ietf.org/html/rfc1918). Bijvoorbeeld: 10.0.0.0/16.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Kan ik openbare IP-adressen in mijn vnet's hebben?
Ja. Zie voor meer informatie over openbare IP-adresbereiken [een virtueel netwerk maken](virtual-network-manage-network.md#create-a-virtual-network). Openbare IP-adressen zijn niet rechtstreeks toegankelijk zijn vanaf internet.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>Is er een limiet aan het aantal subnetten in mijn VNet?
Ja. Zie [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) voor meer informatie. Subnet adresruimten elkaar niet overlappen elkaar.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Zijn er beperkingen voor het gebruik van IP-adressen binnen deze subnetten?
Ja. Azure bepaalde IP-adressen binnen elk subnet gereserveerd. De eerste en laatste IP-adressen van elk subnet zijn gereserveerd voor protocol overeenstemming, samen met de x.x.x.1 x.x.x.3-adressen van elk subnet die worden gebruikt voor Azure-services.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Hoe klein en hoe groot kunnen worden vnet's en subnetten?
Het kleinste ondersteunde subnet slechts/29 is en de grootste/8 die (met behulp van de CIDR-subnetdefinities).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Kan ik bieden mijn VLAN's Azure VNets met?
Nee. Vnet's zijn Layer 3-overlays. Azure biedt geen ondersteuning voor een Layer 2-semantiek.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Kan ik aangepaste routeringsbeleid opgeven op mijn vnet's en subnetten?
Ja. U kunt een routetabel maken en deze te koppelen aan een subnet. Zie voor meer informatie over routering in Azure, [Routering-overzicht](virtual-networks-udr-overview.md#custom-routes).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Ondersteunen VNets multicast- of broadcastnetwerkverkeer?
Nee. Multicast en broadcast worden niet ondersteund.

### <a name="what-protocols-can-i-use-within-vnets"></a>Welke protocollen kan ik gebruiken binnen VNets?
U kunt TCP, UDP en TCP/IP ICMP-protocollen binnen VNets gebruiken. Unicast binnen VNets, met uitzondering van Dynamic Host Configuration Protocol (DHCP) via Unicast wordt ondersteund (bronpoort UDP/68-bestemming poort UDP/67). Multicast, broadcast, IP-in-IP-encapsulated pakketten en Generic Routing Encapsulation (GRE) pakketten worden binnen VNets geblokkeerd. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Kan ik mijn routers standaard binnen een VNet pingen?
Nee.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Kan ik tracert gebruiken om op te sporen connectiviteit?
Nee.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Kan ik subnetten toevoegen nadat het VNet is gemaakt?
Ja. Subnetten kunnen worden toegevoegd aan de VNets op elk gewenst moment zolang het adresbereik subnet geen deel uit van een ander subnet maakt en er beschikbare ruimte in het adresbereik van het virtuele netwerk is.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Kan ik de grootte van mijn subnet wijzigen nadat ik deze maken?
Ja. U kunt toevoegen, verwijderen, uitbreiden of verkleinen van een subnet als er zijn geen virtuele machines of services die zijn geïmplementeerd in het.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Kan ik subnetten wijzigen nadat ik ze zijn gemaakt?
Ja. U kunt toevoegen, verwijderen en aanpassen van de CIDR-blokken die worden gebruikt door een VNet.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>Als ik mijn services in een VNet, kan ik met het internet verbinden?
Ja. Alle services die zijn geïmplementeerd in een VNet kunnen uitgaand naar het internet verbinden. Zie voor meer informatie over uitgaande internet-verbindingen in Azure, [uitgaande verbindingen](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Als u verbinding inkomende aan een resource die is geïmplementeerd via Resource Manager wilt, moet de resource een openbaar IP-adres toegewezen hebben. Zie voor meer informatie over openbare IP-adressen, [openbare IP-adressen](virtual-network-public-ip-address.md). Elke Azure Cloud Service geïmplementeerd in Azure heeft een openbaar adresseerbare VIP-adres toegewezen. U definieert invoereindpunten voor PaaS-rollen en -eindpunten voor virtuele machines naar het inschakelen van deze services om verbindingen via internet te accepteren.

### <a name="do-vnets-support-ipv6"></a>Vnet's bieden ondersteuning voor IPv6?
Nee. U kunt IPv6 niet gebruiken met Vnetten op dit moment. U kunt wel, toewijzen IPv6-adressen voor Azure load balancers laden van een balans vinden tussen virtuele machines. Zie voor meer informatie [overzicht van IPv6 voor Azure Load Balancer](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

### <a name="can-a-vnet-span-regions"></a>Kan een VNet regio's omspannen?
Nee. Een VNet is beperkt tot één regio. Een virtueel netwerk, echter beschikbaarheid zones omvatten. Zie [Overzicht van beschikbaarheidszones](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor meer informatie over beschikbaarheidszones. U kunt virtuele netwerken in verschillende regio's verbinden met het virtuele netwerk peering. Zie voor meer informatie [peering Virtual network-overzicht](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Kan ik een VNet verbinden met een andere VNet in Azure?
Ja. U kunt een VNet koppelen aan andere VNet met een:
- **Virtueel netwerk peering**: Zie voor meer informatie [overzicht van de VNet-peering](virtual-network-peering-overview.md)
- **Een Azure VPN-Gateway**: Zie voor meer informatie [een VNet-naar-VNet-verbinding configureren](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

## <a name="name-resolution-dns"></a>Naamomzetting (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Wat zijn de DNS-opties voor VNets?
Gebruik van de Beslissingstabel op de [naamomzetting voor VM's en Rolexemplaren](virtual-networks-name-resolution-for-vms-and-role-instances.md) pagina om te leiden u door de DNS-opties beschikbaar.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Kan ik DNS-servers opgeven voor een VNet?
Ja. U kunt DNS-server IP-adressen in de VNet-instellingen opgeven. De instelling wordt toegepast als de standaard DNS-server (s) voor alle virtuele machines in het VNet.

### <a name="how-many-dns-servers-can-i-specify"></a>Hoeveel DNS-servers kan ik opgeven?
Verwijzing [Azure beperkt](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Kan ik mijn DNS-servers niet wijzigen nadat ik het netwerk hebt gemaakt?
Ja. U kunt de lijst met DNS-servers voor uw VNet op elk gewenst moment wijzigen. Als u uw lijst met DNS-server wijzigt, moet u elk van de virtuele machines opnieuw opstarten in uw VNet zodat ze naar de nieuwe DNS-server kunnen worden opgepikt.

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Wat is Azure DNS- en werkt het met VNets?
Azure DNS-is een multitenant-DNS-service die worden aangeboden door Microsoft. Azure registreert alle virtuele machines en cloud service-rolexemplaren in deze service. Deze service biedt naamomzetting door de hostnaam voor VM's en rolexemplaren die zich in dezelfde cloudservice en FQDN-naam voor de VM's en rolinstanties in hetzelfde VNet. Zie voor meer informatie over DNS, [naamomzetting voor virtuele machines en Cloud Services-rolexemplaren](virtual-networks-name-resolution-for-vms-and-role-instances.md).

Er is een beperking bij de eerste 100 cloudservices in een VNet voor de naamomzetting van cross-tenant met behulp van Azure geleverde DNS. Als u uw eigen DNS-server gebruikt, is deze beperking niet van toepassing.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>Kan ik mijn DNS-instellingen op basis van per VM of in een cloud service overschrijven?
Ja. U kunt DNS-servers per virtuele machine of cloud service om te overschrijven de standaardinstellingen van het netwerk instellen. Het wordt echter aanbevolen dat u het hele netwerk DNS zo veel mogelijk.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kan ik mijn eigen DNS-achtervoegsel brengen?
Nee. U kunt een aangepaste DNS-achtervoegsel opgeven voor uw vnet's.

## <a name="connecting-virtual-machines"></a>Verbinding maken met virtuele machines

### <a name="can-i-deploy-vms-to-a-vnet"></a>Kan ik virtuele machines implementeren met een VNet?
Ja. Alle netwerkinterfaces (NIC) gekoppeld aan een virtuele machine te implementeren via het implementatiemodel van Resource Manager moeten worden verbonden met een VNet. Virtuele machines die worden geïmplementeerd via het klassieke implementatiemodel kunnen eventueel worden verbonden met een VNet.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>Wat zijn de verschillende soorten IP-adressen die ik aan VM's toewijzen kunt?
* **Persoonlijk:** toegewezen aan elke NIC binnen elke virtuele machine. Het adres is toegewezen met behulp van de methode statisch of dynamisch. Privé IP-adressen worden toegewezen uit het bereik dat is opgegeven in de subnetinstellingen van uw VNet. Resources die zijn geïmplementeerd via het klassieke implementatiemodel worden particuliere IP-adressen toegewezen, zelfs als ze niet is verbonden met een VNet. Het gedrag van de toewijzingsmethode is verschillend, afhankelijk van of een resource met het resourcemanager of klassieke implementatiemodel is geïmplementeerd: 

  - **Resource Manager**: een particulier IP-adres toegewezen met de dynamische of statische methode blijft toegewezen aan een virtuele machine (Resource Manager) tot de resource wordt verwijderd. Het verschil is dat u het adres bij gebruik van statisch toewijzen selecteren en Azure bij gebruik van dynamische kiest. 
  - **Klassieke**: een particulier IP-adres toegewezen met de dynamische methode kan worden gewijzigd wanneer een virtuele machine (klassiek) VM opnieuw wordt opgestart nadat zij gestopt (toewijzing ongedaan gemaakt). Als u nodig hebt om ervoor te zorgen dat de privé IP-adres voor een resource die is geïmplementeerd via het klassieke implementatiemodel nooit verandert, moet u een persoonlijk IP-adres toewijzen met de statische methode.

* **Openbaar:** eventueel toegewezen aan de NIC's gekoppeld aan virtuele machines die worden geïmplementeerd via het Azure Resource Manager-implementatiemodel. Het adres kan worden toegewezen aan de statische of dynamische toewijzingsmethode. Alle virtuele machines en Cloud Services-rolexemplaren die worden geïmplementeerd via het klassieke implementatiemodel bestaan binnen een cloudservice die is toegewezen een *dynamische*, openbare virtuele IP-adres (VIP)-adres. Een openbare *statische* IP-adres, aangeroepen een [gereserveerd IP-adres](virtual-networks-reserved-public-ip.md), eventueel kunnen worden toegewezen als een VIP-adres. U kunt een openbare IP-adressen toewijzen aan afzonderlijke virtuele machines of Cloud Services-rolexemplaren geïmplementeerd via het klassieke implementatiemodel. Deze adressen heten [niveau openbare IP-instantie (ILPIP](virtual-networks-instance-level-public-ip.md) lost en dynamisch kunnen worden toegewezen.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Kan ik een particulier IP-adres reserveren voor een virtuele machine die ik op een later tijdstip wilt maken?
Nee. Een persoonlijke IP-adres kan niet worden gereserveerd. Als een persoonlijk IP-adres beschikbaar is, is het toegewezen aan een exemplaar van de virtuele machine of rol door de DHCP-server. De virtuele machine al dan niet die u wilt dat de privé IP-adres wordt toegewezen aan. U kunt echter het privé IP-adres van een bestaande VM om alle beschikbare privé IP-adres te wijzigen.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>Persoonlijke IP-adressen wijzigen voor virtuele machines in een VNet doen?
Dat hangt ervan af. Als de virtuele machine is geïmplementeerd via resourcemanager Nee, ongeacht of het IP-adres is toegewezen aan de statische of dynamische toewijzingsmethode. Als de virtuele machine is geïmplementeerd via het klassieke implementatiemodel, kunnen dynamische IP-adressen wijzigen wanneer een virtuele machine wordt gestart nadat zijn gestopt (toewijzing ongedaan gemaakt). Het adres wordt vrijgegeven van een virtuele machine te implementeren via beide implementatiemodel wanneer de virtuele machine wordt verwijderd.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>Kan ik handmatig IP-adressen toewijzen aan NIC's in het besturingssysteem van de virtuele machine?
Ja, maar deze wordt niet aanbevolen, tenzij nodig, zoals wanneer meerdere IP-toewijzen aan een virtuele machine adressen. Zie voor meer informatie [toe te voegen meerdere IP-adressen aan een virtuele machine](virtual-network-multiple-ip-addresses-portal.md#os-config). Als het IP-adres dat is toegewezen aan een Azure-NIC die is gekoppeld aan een VM wijzigingen en het IP-adres in het besturingssysteem van de virtuele machine anders is, u verbinding met de virtuele machine verliezen.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Als ik een Cloudservice implementatiesleuf of het afsluiten een virtuele machine uit in het besturingssysteem stopt, wat gebeurt er met Mijn IP-adressen?
Er zijn geen. De IP-adressen (openbare VIP, openbare en persoonlijke) blijven toegewezen aan de cloud service implementatiesleuf of de virtuele machine.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>Kan ik verplaatsen virtuele machines van één subnet naar een ander subnet in een VNet zonder opnieuw te implementeren?
Ja. U vindt meer informatie in de [hoe een exemplaar van de virtuele machine of rol verplaatst naar een ander subnet](virtual-networks-move-vm-role-to-subnet.md) artikel.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Kan ik een statische MAC-adres configureren voor mijn VM
Nee. Een MAC-adres kan niet statisch worden geconfigureerd.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>Het MAC-adres blijven hetzelfde voor mijn VM zodra deze gemaakt?
Ja, het MAC-adres hetzelfde blijft voor een virtuele machine te implementeren via de resourcemanager en de klassieke implementatiemodellen totdat deze wordt verwijderd. Het MAC-adres is eerder uitgegeven als de virtuele machine is gestopt (toewijzing ongedaan gemaakt), maar nu het MAC-adres worden bewaard, zelfs wanneer de virtuele machine de status van de toewijzing ongedaan is gemaakt wordt.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Kan ik verbinden met het internet van een virtuele machine in een VNet?
Ja. Alle virtuele machines en Cloud Services-rolexemplaren die binnen een VNet zijn geïmplementeerd, kunnen verbinding maken met Internet.

## <a name="azure-services-that-connect-to-vnets"></a>Azure-services die verbinding met vnet's maken

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Kan ik Azure App Service Web Apps met een VNet gebruiken?
Ja. U kunt de Web-Apps binnen een VNet met een as-omgeving (App Service-omgeving) implementeren. Als u een punt-naar-site-verbinding geconfigureerd voor uw VNet hebt, kunt alle Web-Apps veilig verbinding en toegang tot bronnen in het VNet. Raadpleeg voor meer informatie de volgende artikelen:

* [Web-Apps maken in App Service-omgeving](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Uw app integreren met een Azure-netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [VNet-integratie en hybride verbindingen gebruiken met WebApps](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json#hybrid-connections-and-app-service-environments)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Kan ik Cloud Services implementeren met web- en werkrollen rollen (PaaS) in een VNet?
Ja. (Optioneel) kunt u Cloud Services-rolexemplaren binnen VNets implementeren. Om dit te doen, geeft u de naam VNet en de rol/subnet toewijzingen in de configuratiesectie netwerk van uw serviceconfiguratie. U hoeft niet bijwerken van de binaire bestanden.

### <a name="can-i-connect-a-virtual-machine-scale-set-vmss-to-a-vnet"></a>Kan ik een virtuele Machine schaal instellen (VMSS) verbinden met een VNet?
Ja. U moet een VMSS verbinden met een VNet.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Er is dat een volledig overzicht van Azure services of ik resources uit in een VNet implementeren kan?

Ja, voor meer informatie Zie [integratie van virtueel netwerk voor Azure-services](virtual-network-for-azure-services.md).

### <a name="which-azure-paas-resources-can-i-restrict-access-to-from-a-vnet"></a>Welke Azure PaaS-resources kan ik de toegang tot beperken van een VNet?

Resources geïmplementeerd in sommige Azure PaaS-services (zoals Azure Storage en Azure SQL Database), kunt u toegang tot het netwerk beperken tot alleen de resources in een VNet door het gebruik van virtueel netwerk service-eindpunten. Zie voor meer informatie [Virtual network service-eindpunten overzicht](virtual-network-service-endpoints-overview.md).

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Kan ik mijn services in en uit de VNets verplaatsen?
Nee. U kunt services in en uit de VNets niet verplaatsen. U hebt voor het verplaatsen van een bron naar het andere VNet, om te verwijderen en opnieuw implementeren van de resource.

## <a name="security"></a>Beveiliging

### <a name="what-is-the-security-model-for-vnets"></a>Wat is het beveiligingsmodel voor VNets?
VNets die zijn geïsoleerd van elkaar en andere services die worden gehost in de Azure-infrastructuur. Een VNet is grens van een vertrouwensrelatie.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Kan ik binnenkomend of uitgaand netwerkverkeer wordt beperkt tot VNet verbonden bronnen?
Ja. U kunt toepassen [Netwerkbeveiligingsgroepen](security-overview.md) met afzonderlijke subnetten binnen een VNet NIC's die zijn gekoppeld aan een VNet of beide.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>Kan ik een firewall tussen het VNet verbonden resources implementeren?
Ja. U kunt een [virtueel netwerkapparaat firewall](https://azure.microsoft.com/marketplace/?term=firewall) van verschillende leveranciers via de Azure Marketplace.

### <a name="is-there-information-available-about-securing-vnets"></a>Is er informatie beschikbaar over het beveiligen van VNets?
Ja. Zie voor meer informatie [Azure Network Security overzicht](../security/security-network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="apis-schemas-and-tools"></a>API's, schema's en hulpprogramma 's

### <a name="can-i-manage-vnets-from-code"></a>Kan ik VNets beheren vanuit code?
Ja. U kunt de REST-API's gebruiken voor vnet's binnen de [Azure Resource Manager](/rest/api/virtual-network) en [klassieke (Service Management)](http://go.microsoft.com/fwlink/?LinkId=296833) implementatiemodellen.

### <a name="is-there-tooling-support-for-vnets"></a>Is er tooling ondersteuning voor VNets?
Ja. Meer informatie over het gebruik van:
- De Azure-portal voor het implementeren van VNets via de [Azure Resource Manager](virtual-networks-create-vnet-arm-pportal.md) en [klassieke](virtual-networks-create-vnet-classic-pportal.md) implementatiemodellen.
- PowerShell voor het beheren van VNets die zijn geïmplementeerd via de [Resource Manager](/powershell/module/azurerm.network) en [klassieke](/powershell/module/azure/?view=azuresmps-3.7.0) implementatiemodellen.
- De Azure-opdrachtregelinterface (CLI) te implementeren en beheren van de VNets die zijn geïmplementeerd via de [Resource Manager](/cli/azure/network/vnet) en [klassieke](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-network-commands-to-manage-network-resources) implementatiemodellen.  
