---
title: Naamomzetting voor resources in Azure-netwerken
titlesuffix: Azure Virtual Network
description: De naam van oplossing scenario's voor Azure IaaS, hybride oplossingen, tussen verschillende cloudservices, Active Directory en uw eigen DNS-server.
services: virtual-network
documentationcenter: na
author: subsarma
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: subsarma
ms.openlocfilehash: e63db9dd5dcd48e19c75608c39c4c0b62db24527
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221195"
---
# <a name="name-resolution-for-resources-in-azure-virtual-networks"></a>Naamomzetting voor resources in Azure-netwerken

Afhankelijk van hoe u Azure gebruikt voor het hosten van IaaS, PaaS en hybride oplossingen, moet u mogelijk de virtuele machines (VM's) en andere resources die zijn geïmplementeerd in een virtueel netwerk om te communiceren met elkaar toestaan. Hoewel u communicatie inschakelen kunt met behulp van IP-adressen, is het veel eenvoudiger namen te gebruiken die eenvoudig kunnen worden onthouden, en niet te wijzigen. 

Wanneer resources die zijn geïmplementeerd in virtuele netwerken domeinnamen omzetten in het interne IP-adressen moeten, kunnen ze een van twee methoden gebruiken:

* [Azure geleverd naamomzetting](#azure-provided-name-resolution)
* [Naamomzetting die gebruikmaakt van uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server) (die mogelijk doorsturen van query's naar de Azure DNS-servers) 

Het type van de naamomzetting die u gebruikt, is afhankelijk van hoe uw resources moeten met elkaar communiceren. De volgende tabel ziet u scenario's en bijbehorende naam resolutie-oplossingen:

> [!NOTE]
> Afhankelijk van uw scenario, kunt u de functie Azure DNS Private Zones, dat zich momenteel in openbare preview-versie gebruiken. Zie voor meer informatie [Azure DNS gebruiken voor privédomeinen](../dns/private-dns-overview.md).
>

| **Scenario** | **Oplossing** | **Suffix** |
| --- | --- | --- |
| Naamomzetting tussen virtuele machines zich bevinden in het hetzelfde virtuele netwerk of de Azure Cloud Services-rolexemplaren in dezelfde cloudservice. | [Azure DNS Private Zones](../dns/private-dns-overview.md) of [Azure geleverd naamomzetting](#azure-provided-name-resolution) |De hostnaam of FQDN-naam |
| Naamomzetting tussen virtuele machines in verschillende virtuele netwerken of rolinstanties in verschillende cloudservices. |[Azure DNS Private Zones](../dns/private-dns-overview.md) of door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor het omzetten van door Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN-naam |
| Naamomzetting van een Azure App Service (Web-App, functie of Bot) met behulp van de integratie van virtuele netwerken op rolinstanties of virtuele machines in hetzelfde virtuele netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor het omzetten van door Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN-naam |
| Naam resolutie van App Service Web Apps naar virtuele machines in hetzelfde virtuele netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor het omzetten van door Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN-naam |
| Naam resolutie van App Service Web Apps in één virtueel netwerk met VM's in een ander virtueel netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor het omzetten van door Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server-for-web-apps). |Alleen FQDN-naam |
| Resolutie van on-premises computer- en servicenamen van VM's of rolexemplaren in Azure. |Door de klant beheerde DNS-servers (on-premises domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS gesynchroniseerd met behulp van zoneoverdrachten, bijvoorbeeld). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN-naam |
| Resolutie van Azure hostnamen van on-premises computers. |Doorsturen van aanvragen naar een door de klant beheerde DNS-proxyserver in de bijbehorende virtuele netwerk, de proxy-server stuurt query's naar Azure voor het omzetten van. Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN-naam |
| Reverse-DNS voor interne IP-adressen. |[Naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Niet van toepassing |
| Naamomzetting tussen virtuele machines of rolinstanties die zich in andere cloudservices, niet in een virtueel netwerk. |Niet van toepassing. Connectiviteit tussen virtuele machines en rolinstanties in verschillende cloudservices wordt niet ondersteund buiten een virtueel netwerk. |Niet van toepassing|

## <a name="azure-provided-name-resolution"></a>Azure geleverd naamomzetting

Samen met een resolutie van openbare DNS-namen biedt Azure interne naamomzetting voor virtuele machines en rolinstanties die zich in hetzelfde virtuele netwerk of cloudservice bevinden. VM's en -exemplaren in een cloudservice delen de dezelfde DNS-achtervoegsel, zodat alleen de naam van de host voldoende is. Maar in virtuele netwerken die zijn geïmplementeerd met behulp van het klassieke implementatiemodel, andere cloudservices hebben verschillende DNS-achtervoegsels. In dit geval moet u de FQDN-naam voor de naamomzetting tussen verschillende cloudservices. In virtuele netwerken die zijn geïmplementeerd met behulp van het implementatiemodel Azure Resource Manager, is het DNS-achtervoegsel consistent zijn tussen het virtuele netwerk, zodat de FQDN-naam is niet nodig. DNS-namen kunnen worden toegewezen aan zowel virtuele machines en netwerkinterfaces. Hoewel Azure geleverd naamomzetting geen configuratie vereist is, maar het is niet de juiste keuze voor alle implementatiescenario's, zoals beschreven in de vorige tabel.

> [!NOTE]
> Wanneer met behulp van cloud services-web-en werkrollen, kunt u ook toegang tot het interne IP-adressen van de rolinstanties met behulp van de Azure Service Management REST API. Zie voor meer informatie de [Service Management REST API-verwijzing](https://msdn.microsoft.com/library/azure/ee460799.aspx). Het adres is gebaseerd op het nummer van de rol van naam en het exemplaar. 
> 
> 

### <a name="features"></a>Functies

Azure geleverd naamomzetting omvat de volgende functies:
* Gebruiksgemak. Er is geen configuratie vereist.
* Hoge beschikbaarheid. U hoeft te maken en beheren van clusters van uw eigen DNS-servers.
* U kunt de service gebruiken in combinatie met uw eigen DNS-servers, zowel on-premises en namen van de Azure-host op te lossen.
* U kunt de naamomzetting tussen virtuele machines en rollen binnen dezelfde cloudservice, zonder de noodzaak van een FQDN-naam gebruiken.
* U kunt de naamomzetting tussen virtuele machines in virtuele netwerken die gebruikmaken van het Azure Resource Manager-implementatiemodel, zonder dat nodig is voor een FQDN-naam gebruiken. Virtuele netwerken in het klassieke implementatiemodel vereisen een FQDN-naam wanneer u het omzetten van namen in andere cloudservices. 
* U kunt de namen van de hosts die het beste uw implementaties te beschrijven in plaats van het werken met automatisch gegenereerde naam.

### <a name="considerations"></a>Overwegingen

Punten om te overwegen wanneer u Azure geleverd naamomzetting gebruikt:
* Het hebt gemaakt van Azure DNS-achtervoegsel kan niet worden gewijzigd.
* U kunt uw eigen records kan niet handmatig registreren.
* WINS- en NetBIOS-worden niet ondersteund. U kunt uw virtuele machines in Windows Explorer niet zien.
* Hostnamen moet de DNS-compatibel. Namen van rapportitems zijn alleen 0-9, a-z, en '-', en mag niet beginnen of eindigen met een '-'.
* DNS-query-verkeer is beperkt voor elke virtuele machine. Beperking mag niet van invloed op de meeste toepassingen. Als aanvraagbeperking wordt waargenomen, moet u caching aan clientzijde is ingeschakeld. Zie voor meer informatie, [DNS client-configuratie](#dns-client-configuration).
* Alleen virtuele machines in de eerste 180 cloudservices zijn geregistreerd voor elk virtueel netwerk in een klassieke implementatiemodel. Deze limiet geldt niet voor virtuele netwerken in Azure Resource Manager.

## <a name="dns-client-configuration"></a>Configuratie van DNS-client

In deze sectie bevat informatie over caching aan clientzijde en client-side nieuwe pogingen.

### <a name="client-side-caching"></a>Caching aan clientzijde

Niet elke DNS-query moet worden verzonden via het netwerk. Caching aan clientzijde, kunt wachttijden verkorten en herstelmogelijkheden bij netwerkproblemen, verbeteren door de verwerking van terugkerende DNS-query's uit een lokale cache. DNS-records bevatten een mechanisme time-to-live (TTL) waarmee de cache voor het opslaan van de record voor zo lang mogelijk zonder dat dit record webdocumenten. Daarom is het geschikt is voor de meeste situaties caching aan clientzijde.

De standaard Windows-DNS-client heeft een ingebouwde DNS-cache. Sommige Linux-distributies omvatten niet opslaan in cache standaard. Als u vindt dat er nog niet een lokale cache is, voegt u een DNS-cache toe naar elke Linux-VM.

Er zijn een aantal verschillende DNS-caching pakketten beschikbaar zijn (zoals dnsmasq). Dit is het dnsmasq installeren op de meest voorkomende distributies:

* **Ubuntu (maakt gebruik van resolvconf)**:
  * Installeer het pakket dnsmasq met `sudo apt-get install dnsmasq`.
* **SUSE (maakt gebruik van netconf)**:
  * Installeer het pakket dnsmasq met `sudo zypper install dnsmasq`.
  * Schakel de service dnsmasq met `systemctl enable dnsmasq.service`. 
  * Start de service dnsmasq met `systemctl start dnsmasq.service`. 
  * Bewerken **/etc/sysconfig/network/config**, en wijzig *NETCONFIG_DNS_FORWARDER = ""* naar *dnsmasq*.
  * Bijwerken van resolv.conf met `netconfig update`om in te stellen de cache als de lokale DNS-resolver.
* **OpenLogic (uses NetworkManager)**:
  * Installeer het pakket dnsmasq met `sudo yum install dnsmasq`.
  * Schakel de service dnsmasq met `systemctl enable dnsmasq.service`.
  * Start de service dnsmasq met `systemctl start dnsmasq.service`.
  * Voeg *domain-name-servers 127.0.0.1; Voeg* naar **/etc/dhclient-eth0.conf**.
  * Opnieuw opstarten van de netwerkservice met `service network restart`om in te stellen de cache als de lokale DNS-resolver.

> [!NOTE]
> Het pakket dnsmasq is slechts een aantal DNS-caches beschikbaar voor Linux. Voordat u deze gebruikt, controleert u de geschiktheid voor uw specifieke behoeften en controleer of er geen andere cache is geïnstalleerd.
> 
> 
    
### <a name="client-side-retries"></a>Client-side nieuwe pogingen

DNS is voornamelijk een UDP-protocol. Omdat het UDP-protocol biedt geen garantie voor bezorging van berichten, wordt in de DNS-protocol zelf logica voor opnieuw proberen verwerkt. Elke DNS-client (besturingssysteem) kan zich voordoen logica voor verschillende nieuwe pogingen, afhankelijk van de maker van voorkeur:

* Windows-besturingssystemen opnieuw proberen na één seconde, en vervolgens opnieuw na de andere twee seconden, vier seconden en een andere vier seconden. 
* De standaard Linux setup nieuwe pogingen na vijf seconden. U wordt aangeraden de specificaties voor nieuwe pogingen wijzigen in vijf keer met 1 seconde intervallen.

Controleer de huidige instellingen op een Linux-VM met `cat /etc/resolv.conf`. Bekijk de *opties* regel, bijvoorbeeld:

```bash
options timeout:1 attempts:5
```

Het bestand resolv.conf wordt doorgaans automatisch gegenereerd en mag niet worden bewerkt. De specifieke stappen voor het toevoegen van de *opties* regel variëren per distributie:

* **Ubuntu** (maakt gebruik van resolvconf):
  1. Voeg de *opties* te **/etc/resolvconf/resolv.conf.d/tail**.
  2. Voer `resolvconf -u` om bij te werken.
* **SUSE** (maakt gebruik van netconf):
  1. Toevoegen *timeout:1 pogingen: 5* naar de **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parameter in **/etc/sysconfig/network/config**. 
  2. Voer `netconfig update` om bij te werken.
* **OpenLogic** (uses NetworkManager):
  1. Voeg *echo "opties timeout:1 pogingen: 5'* naar **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  2. Werken met `service network restart`.

## <a name="name-resolution-that-uses-your-own-dns-server"></a>Naamomzetting die gebruikmaakt van uw eigen DNS-server

In deze sectie bevat informatie over virtuele machines en instanties van de web-apps.

### <a name="vms-and-role-instances"></a>Virtuele machines en rollen

De behoeften van uw naam resolutie mogelijk verder gaan dan de functies van Azure. Bijvoorbeeld, moet u mogelijk het gebruik van Microsoft Windows Server Active Directory-domeinen kunt oplossen door de DNS-namen tussen virtuele netwerken. Azure biedt de mogelijkheid voor gebruik van uw eigen DNS-servers voor deze scenario's.

DNS-servers binnen een virtueel netwerk kunnen u DNS-query's doorsturen naar de recursieve resolvers in Azure. Hiermee kunt u voor het omzetten van hostnamen in dit virtuele netwerk. Een domeincontroller (DC) die worden uitgevoerd in Azure kunt bijvoorbeeld reageren op DNS-query's voor de domeinen en alle andere query's sturen naar Azure. Doorsturen van query's, kunt virtuele machines om te zien van uw on-premises bronnen (via de domeincontroller) en de namen van de opgegeven Azure host (via de doorstuurserver). Toegang tot de recursieve resolvers in Azure wordt geboden via het virtuele IP-adres 168.63.129.16.

Doorsturen van de DNS ook kan DNS-omzetting tussen virtuele netwerken en kunt uw on-premises computers Azure verschafte hostnamen om te zetten. Om om te zetten van een VM-hostnaam, de virtuele machine van de DNS-server moet zich in hetzelfde virtuele netwerk bevinden, en worden geconfigureerd voor doorsturen host de naam van query's naar Azure. Omdat de DNS-achtervoegsel in elk virtueel netwerk anders is, kunt u voorwaardelijk doorsturen regels voor het verzenden van DNS-query's naar het juiste virtuele netwerk voor het omzetten van. De volgende afbeelding ziet u twee virtuele netwerken en een on-premises netwerk DNS-omzetting tussen virtuele netwerken, met behulp van deze methode doet. Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie met Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Een rolinstantie kan naamomzetting van virtuele machines binnen hetzelfde virtuele netwerk uitvoeren. Dit gebeurt met behulp van de FQDN-naam, die uit de hostnaam van de virtuele machine bestaat en **internal.cloudapp.net** DNS-achtervoegsel. In dit geval naamomzetting is echter alleen als de rolinstantie is de naam van de VM gedefinieerd in de [Rolschema (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Rolinstanties die nodig hebt voor het uitvoeren van naamomzetting van virtuele machines in een ander virtueel netwerk (FQDN-naam met behulp van de **internal.cloudapp.net** achtervoegsel) hebben om dit te doen met behulp van de methode die wordt beschreven in deze sectie (aangepaste DNS-servers tussen doorsturen de twee virtuele netwerken).
>

![Diagram van DNS tussen virtuele netwerken](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Wanneer u van Azure geleverd naamomzetting gebruikmaakt, Azure Dynamic Host Configuration Protocol (DHCP) biedt een interne DNS-achtervoegsel (**. internal.cloudapp.net**) aan elke virtuele machine. Dit achtervoegsel kan omzetten van de hostnaam, omdat de host naamrecords in de **internal.cloudapp.net** zone. Wanneer u uw eigen naam resolutie-oplossing gebruikt, wordt dit achtervoegsel aan virtuele machines niet geleverd, omdat deze een conflict met andere DNS-architecturen (zoals scenario's voor het domein veroorzaakt). In plaats daarvan Azure biedt een tijdelijke aanduiding voor niet-functionerende (*reddog.microsoft.com*).

Indien nodig, kunt u de interne DNS-achtervoegsel bepalen met behulp van PowerShell of de API.

* Voor virtuele netwerken in Azure Resource Manager-implementatiemodel, het achtervoegsel is beschikbaar via de [netwerkinterface REST-API](/rest/api/virtualnetwork/networkinterfaces), wordt de [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) PowerShell-cmdlet en de [az network nic show](/cli/azure/network/nic#az-network-nic-show) Azure CLI-opdracht.
* In het klassieke implementatiemodel, het achtervoegsel is beschikbaar via de [implementatie API ophalen](https://msdn.microsoft.com/library/azure/ee460804.aspx) aanroepen of de [Get-AzureVM-Debug](/powershell/module/servicemanagement/azure/get-azurevm) cmdlet.

Als het doorsturen van query's naar Azure niet passen aan uw behoeften, kunt u uw eigen DNS-oplossing moet opgeven. Uw DNS-oplossing moet:

* Het bieden van de meest geschikte host naamomzetting [DDNS](virtual-networks-name-resolution-ddns.md), bijvoorbeeld. Als u DDNS gebruikt, moet u mogelijk uitschakelen opruiming van DNS-record. Azure DHCP-leases lang zijn en opruiming mogelijk verwijderen DNS-records voortijdig gestopt. 
* Juiste recursieve oplossing om toe te staan de resolutie van externe domeinnamen te bieden.
* Toegankelijk (TCP en UDP op poort 53) van de clients die het resultaat wordt gebruikt, en kunnen voor toegang tot het internet.
* Worden beveiligd tegen toegang via internet, om bedreigingen die door externe agents.

> [!NOTE]
> Voor de beste prestaties wanneer u Azure-VM's als DNS-servers, moet IPv6 worden uitgeschakeld. Een [openbaar IP-adres](virtual-network-public-ip-address.md) moet worden toegewezen aan elke virtuele machine van de DNS-server. Voor analyse van prestaties en optimalisatie van wanneer u van Windows Server als uw DNS-server gebruikmaakt, Zie [naam resolutie-prestaties van een recursieve Windows DNS-Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web-apps
Stel dat u wilt uitvoeren van naamomzetting van uw web-app gebouwd met behulp van App Service is gekoppeld aan een virtueel netwerk, op virtuele machines in hetzelfde virtuele netwerk. Naast het instellen van een aangepaste DNS-server die een DNS-doorstuurserver waarmee query's worden doorgestuurd naar Azure (virtueel IP-adres 168.63.129.16), heeft de volgende stappen uitvoeren:
1. Integratie van virtuele netwerken voor uw web-app inschakelen als nog niet gedaan, zoals beschreven in [uw app integreren met een virtueel netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. In de Azure-portal voor het App Service-plan die als host fungeert voor de web-app, selecteer **netwerk synchroniseren** onder **netwerken**, **Virtual Network-integratie**.

    ![Schermopname van het virtuele netwerk-naamomzetting](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Als u uitvoeren van naamomzetting van uw web-app gebouwd wilt met behulp van App Service is gekoppeld aan een virtueel netwerk, op virtuele machines in een ander virtueel netwerk, hebt u aangepaste DNS-servers op beide virtuele netwerken als volgt gebruiken: 
* Instellen van een DNS-server in uw virtuele netwerk van doel op een virtuele machine die u ook query's naar de recursieve naamomzetting in Azure (virtueel IP-adres 168.63.129.16 doorsturen kunt). Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie met Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Instellen van een DNS-doorstuurserver in de bron van virtueel netwerk op een virtuele machine. Deze DNS-doorstuurserver voor het doorsturen van query's naar de DNS-server in uw virtuele doelnetwerk configureren.
* De bron-DNS-server in uw bron van het virtuele netwerk-instellingen configureren.
* Integratie van virtueel netwerk voor uw web-app om te koppelen aan het virtuele netwerk van bron, de instructies in inschakelen [uw app integreren met een virtueel netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* In de Azure-portal voor het App Service-plan die als host fungeert voor de web-app, selecteer **netwerk synchroniseren** onder **netwerken**, **Virtual Network-integratie**. 

## <a name="specify-dns-servers"></a>DNS-servers opgeven
Wanneer u uw eigen DNS-servers gebruikt, is Azure biedt de mogelijkheid om op te geven meerdere DNS-servers per virtueel netwerk. U kunt ook meerdere DNS-servers per netwerkinterface (voor Azure Resource Manager) of per cloudservice (voor het klassieke implementatiemodel) opgeven. DNS-servers die zijn opgegeven voor een netwerk-interface of cloud service krijgen voorrang via DNS-servers die zijn opgegeven voor het virtuele netwerk.

> [!NOTE]
> De eigenschappen van het netwerk verbinding, zoals DNS-server IP-adressen, mag niet worden bewerkt rechtstreeks in de Windows-VM's. Dit is omdat ze tijdens de service kunnen ophalen gewist herstel knooppuntservice wanneer de virtuele netwerkadapter wordt vervangen. 
> 
> 

Wanneer u van het Azure Resource Manager-implementatiemodel gebruikmaakt, kunt u DNS-servers voor een virtueel netwerk en een netwerkinterface opgeven. Zie voor meer informatie, [beheren van een virtueel netwerk](manage-virtual-network.md) en [beheren van een netwerkinterface](virtual-network-network-interface.md).

> [!NOTE]
> Als u ervoor voor aangepaste DNS-server voor het virtuele netwerk kiest, moet u ten minste één DNS-server IP-adres; virtueel netwerk wordt anders negeren van de configuratie en gebruik in plaats daarvan Azure verschafte DNS.
> 
> 

Wanneer u van het klassieke implementatiemodel gebruikmaakt, kunt u DNS-servers voor het virtuele netwerk in Azure portal of de [netwerkconfiguratiebestand](https://msdn.microsoft.com/library/azure/jj157100). Voor cloudservices, kunt u DNS-servers via de [Service configuratiebestand](https://msdn.microsoft.com/library/azure/ee758710) of met behulp van PowerShell, met [New-AzureVM](/powershell/module/servicemanagement/azure/new-azurevm).

> [!NOTE]
> Als u de DNS-instellingen voor een virtueel netwerk of de virtuele machine die al is geïmplementeerd wijzigt, moet u opnieuw opstarten van elke betreffende virtuele machine om de wijzigingen te laten treden.
> 
> 

## <a name="next-steps"></a>Volgende stappen

Azure Resource Manager-implementatiemodel:

* [Een virtueel netwerk beheren](manage-virtual-network.md)
* [Een netwerkinterface beheren](virtual-network-network-interface.md)

Klassieke implementatiemodel:

* [Het Schema van Azure-Service](https://msdn.microsoft.com/library/azure/ee758710)
* [Virtual Network Configuration Schema](https://msdn.microsoft.com/library/azure/jj157100)
* [Een Virtueelnetwerk configureren met behulp van een netwerkconfiguratiebestand](virtual-networks-using-network-configuration-file.md)
