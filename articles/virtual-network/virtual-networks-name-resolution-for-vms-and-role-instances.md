---
title: Naamomzetting voor VM's en rolinstanties
description: De naam van oplossing scenario's voor Azure IaaS, oplossingen voor hybride, tussen verschillende cloud-services, Active Directory en uw eigen DNS-server gebruikt.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: tysonn
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: jdial
ms.openlocfilehash: 5ea3e4ad68fd37ccaa6e081febe4827bdb2e196d
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Naamomzetting voor VM's en rolinstanties

Afhankelijk van hoe u Azure gebruiken voor het hosten van IaaS en PaaS hybride oplossingen, moet u mogelijk maken voor de VM's en rolinstanties die u maakt om te communiceren met elkaar. Deze communicatie kan worden gedaan met behulp van IP-adressen, is het veel eenvoudiger namen die gemakkelijk kunnen worden onthouden en wijzig niet gebruiken. 

Wanneer rolinstanties en virtuele machines die worden gehost in Azure nodig hebt om te zetten domeinnamen interne IP-adressen, kunnen ze een van twee methoden gebruiken:

* [Azure verschafte naamomzetting](#azure-provided-name-resolution)
* [Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) (die mogelijk query doorsturen naar de Azure DNS-servers) 

Het type van naamomzetting die u gebruikt, is afhankelijk van hoe uw VM's en rolexemplaren moeten met elkaar communiceren. De volgende tabel ziet u scenario's en bijbehorende name resolution-oplossingen:

| **Scenario** | Oplossing | **Suffix** |
| --- | --- | --- |
| Naam resolutie tussen rolinstanties of virtuele machines zich in dezelfde cloudservice- of virtuele netwerk. |[Azure verschafte naamomzetting](#azure-provided-name-resolution) |de hostnaam of FQDN-naam |
| De naamomzetting van een Azure App Service (Web-App, functie of Bot) met integratie van virtueel netwerk rolinstanties of virtuele machines zich in hetzelfde virtuele netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| De naamomzetting tussen rolexemplaren te controleren of de virtuele machines zich in verschillende virtuele netwerken. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| Naamomzetting van App Service Web Apps naar virtuele machines zich in hetzelfde virtuele netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| Naamomzetting van App Service Web Apps naar virtuele machines zich in een ander virtueel netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server-for-web-apps). |Alleen FQDN |
| Omzetting van lokale computer en de service namen van rolexemplaren te controleren of de virtuele machines in Azure. |Door de klant beheerde DNS-servers (lokale domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS gesynchroniseerd zoneoverdrachten, bijvoorbeeld via). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| De resolutie van Azure hostnamen van lokale computers. |Doorsturen van aanvragen naar een door de klant beheerde DNS-proxyserver in de bijbehorende virtuele netwerk, de proxy-server stuurt query's naar Azure voor naamomzetting. Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| Omgekeerde DNS voor interne IP-adressen. |[Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Niet van toepassing |
| De naamomzetting tussen VM's of rolexemplaren die zich in verschillende cloud-services, niet in een virtueel netwerk. |Niet van toepassing. Connectiviteit tussen VM's en rolexemplaren in een andere cloud-services wordt niet ondersteund buiten een virtueel netwerk. |Niet van toepassing|

## <a name="azure-provided-name-resolution"></a>Azure verschafte naamomzetting

Samen met de resolutie van openbare DNS-namen biedt Azure interne naamomzetting voor VM's en rolexemplaren die zich in het hetzelfde virtuele netwerk of de service in de cloud bevinden. Virtuele machines/exemplaren in een Cloudservice delen de dezelfde DNS-achtervoegsel (zodat de hostnaam die alleen voldoende is), maar in de klassieke virtuele netwerken verschillende cloud services hebben verschillende DNS-achtervoegsels zodat de FQDN-naam nodig is voor de naamomzetting tussen verschillende cloudservices. Het DNS-achtervoegsel is consistent in het virtuele netwerk (zodat de FQDN-naam is niet nodig) en DNS-namen kunnen worden toegewezen aan zowel NIC's en virtuele machines in virtuele netwerken in het Resource Manager-implementatiemodel. Hoewel Azure verschafte naamomzetting geen configuratie vereist is, is niet de juiste keuze is voor alle implementatiescenario's, zoals in de vorige tabel.

> [!NOTE]
> Wanneer u de web-en werkrollen, kunt u ook de interne IP-adressen van de rolexemplaren op basis van de naam en het exemplaar nummer van rol met de REST-API van Azure Service Management openen. Zie voor meer informatie [Service Management REST API-verwijzing](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features"></a>Functies

* Gebruiksgemak: geen configuratie is vereist om te kunnen gebruiken van Azure verschafte naamomzetting.
* De opgegeven Azure name resolution-service is maximaal beschikbaar, zodat u de noodzaak om te maken en beheren van clusters van uw eigen DNS-servers.
* Kan worden gebruikt in combinatie met uw eigen DNS-servers om op te lossen, zowel on-premises en Azure hostnamen.
* Naamomzetting is tussen de rol exemplaren/virtuele machines in dezelfde cloudservice zonder dat nodig is voor een FQDN-naam opgegeven.
* Naamomzetting is tussen virtuele machines in virtuele netwerken die gebruikmaken van het Resource Manager-implementatiemodel, zonder dat nodig is voor een FQDN-naam opgegeven. Virtuele netwerken in het klassieke implementatiemodel vereisen een FQDN-naam bij het omzetten van namen in andere cloudservices. 
* U kunt hostnamen die het beste uw implementaties te beschrijven in plaats van het werken met de automatisch gegenereerde naam.

### <a name="considerations"></a>Overwegingen

* Het gemaakte Azure DNS-achtervoegsel kan niet worden gewijzigd.
* U kunt uw eigen records kan niet handmatig registreren.
* WINS- en NetBIOS worden niet ondersteund (u kunt niet uw virtuele machines in Windows Verkenner bekijken).
* Hostnamen moet DNS-compatibele. Namen moeten gebruiken alleen 0-9, a-z en '-', en mag niet beginnen of eindigen met een '-'. Zie RFC 3696, sectie 2.
* DNS-query-verkeer is voor elke virtuele machine beperkt. Beperking mag niet van invloed op de meeste toepassingen. Indien aanvraag beperking is waargenomen, zorg ervoor dat caching aan clientzijde is ingeschakeld. Zie voor meer informatie [ophalen van de meest uit Azure verschafte naamomzetting](#Getting-the-most-from-Azure-provided-name-resolution).
* Alleen virtuele machines in de eerste 180 cloudservices zijn geregistreerd voor elke virtuele netwerk in een klassieke implementatiemodel. Deze beperking geldt niet voor virtuele netwerken in het Resource Manager-implementatiemodel.

## <a name="dns-client-configuration"></a>Configuratie van DNS-client

### <a name="client-side-caching"></a>Caching aan clientzijde

Niet elke DNS-query moet worden verzonden via het netwerk. Caching aan clientzijde helpt de latentie te verminderen en de herstelmogelijkheden bij netwerk blips verbeteren door terugkerende DNS-query's uit de lokale cache op te lossen. DNS-records bevatten een time-to-live (TTL) waarmee de cache voor het opslaan van de record voor zo lang mogelijk zonder enige impact op record nieuwheid zodat caching aan clientzijde geschikt voor de meeste situaties is.

De standaard Windows-DNS-Client heeft een ingebouwde DNS-cache. Sommige Linux-distributies omvatten geen cache standaard. Een DNS-cache wordt toegevoegd aan elke Linux-VM (nadat u hebt gecontroleerd dat nog niet een lokale cache is), wordt aanbevolen.

Er zijn een aantal verschillende de DNS-cache van de pakketten beschikbaar. Bijvoorbeeld: dnsmasq. Dnsmasq installeren op de meest voorkomende distributies een overzicht van de volgende stappen uit:

* **Ubuntu (maakt gebruik van resolvconf)**:
  * Installeer het pakket dnsmasq met `sudo apt-get install dnsmasq`.
* **SUSE (maakt gebruik van netconf)**:
  * Installeer het pakket dnsmasq met `sudo zypper install dnsmasq`.
  * Schakel de service dnsmasq met `systemctl enable dnsmasq.service`. 
  * Start de service dnsmasq met `systemctl start dnsmasq.service`. 
  * Bewerken **/etc/sysconfig/network/config** en wijzig *NETCONFIG_DNS_FORWARDER = ""* naar *dnsmasq*.
  * Bijwerken van resolv.conf met `netconfig update` in te stellen van de cache als de lokale DNS-resolver.
* **OpenLogic (maakt gebruik van NetworkManager)**:
  * Installeer het pakket dnsmasq met `sudo yum install dnsmasq`.
  * Schakel de service dnsmasq met `systemctl enable dnsmasq.service`.
  * Start de service dnsmasq met `systemctl start dnsmasq.service`.
  * Voeg *toevoegen domain name servers 127.0.0.1;* naar **/etc/dhclient-eth0.conf**.
  * Opnieuw opstarten van de netwerkservice met `service network restart` in te stellen van de cache als de lokale DNS-resolver.

> [!NOTE]
> Het pakket 'dnsmasq' is slechts een van de vele DNS-caches beschikbaar voor Linux. Controleer voordat u deze gebruikt geschikt is voor uw specifieke behoeften en die geen andere cache is geïnstalleerd.
> 
> 
    
### <a name="client-side-retries"></a>Nieuwe pogingen voor clientzijde

DNS is voornamelijk een UDP-protocol. Als het UDP-protocol niet levering van berichten garanderen, wordt in het DNS-protocol zelf Pogingslogica afgehandeld. Elke DNS-client (besturingssysteem) kan zich voordoen verschillende Pogingslogica, afhankelijk van de maker voorkeur:

* Windows besturingssystemen opnieuw proberen na een tweede en nog een keer na een andere 2, 4 en een andere vier seconden. 
* De standaard Linux setup pogingen na vijf seconden. Wijzigen van de nieuwe poging in vijf keer op 1 seconde intervallen, wordt aanbevolen.

Controleer de huidige instellingen op een Linux-VM met `cat /etc/resolv.conf`. Bekijk de *opties* regel, bijvoorbeeld:

```bash
options timeout:1 attempts:5
```

Het bestand resolv.conf wordt doorgaans automatisch gegenereerd en mag niet worden bewerkt. De specifieke stappen voor het toevoegen van de *opties* regel verschillen per distro:

* **Ubuntu** (maakt gebruik van resolvconf):
  * De opties regel toe te voegen **/etc/resolveconf/resolv.conf.d/head**.
  * Voer `resolvconf -u` om bij te werken.
* **SUSE** (maakt gebruik van netconf):
  * Toevoegen *timeout:1 pogingen: 5* naar de **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parameter in **/etc/sysconfig/network/config**. 
  * Voer `netconfig update` om bij te werken.
* **OpenLogic** (maakt gebruik van NetworkManager):
  * Voeg *echo 'opties timeout:1 pogingen: 5'* naar **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  * Werken met `service network restart`.

## <a name="name-resolution-using-your-own-dns-server"></a>Naamomzetting met uw eigen DNS-server

### <a name="vms-and-role-instances"></a>VM's en rolinstanties

Er zijn een aantal situaties waarbij de behoeften van uw name resolution mogelijk gaat voorbij de functies van Azure, bijvoorbeeld wanneer met behulp van Active Directory-domeinen of wanneer u de DNS-omzetting tussen virtuele netwerken vereist. Ten aanzien van deze scenario's, biedt Azure de mogelijkheid voor u om uw eigen DNS-servers te gebruiken.

DNS-servers binnen een virtueel netwerk kunnen de DNS-query's doorsturen naar Azure recursieve resolvers omzetten van hostnamen binnen dit virtuele netwerk. Bijvoorbeeld, een domeincontroller (DC) uitgevoerd in Azure reageren op DNS-query's voor de domeinen en alle andere aanvragen door te sturen naar Azure. Doorsturen van query's kunt virtuele machines om te zien van uw lokale bronnen (via de domeincontroller) en de Azure verschafte hostnamen (via de doorstuurserver). Toegang tot Azure recursieve resolvers wordt opgegeven via het virtuele IP-adres 168.63.129.16.

DNS-forwarding tussen virtuele netwerken DNS-omzetting Hiermee ook- en kan uw lokale machine Azure verschafte hostnamen omzetten. Om een VM-hostnaam niet omzetten, is de DNS-server VM moet zich in hetzelfde virtuele netwerk bevinden, en worden geconfigureerd voor doorsturen hostnaam query's naar Azure. Als het DNS-achtervoegsel in elk virtueel netwerk anders is, kunt u voorwaardelijk doorsturen regels DNS-query's verzenden naar het juiste virtuele netwerk voor naamomzetting. De volgende afbeelding ziet u twee virtuele netwerken en een on-premises netwerk doen tussen virtuele netwerken DNS-omzetting met deze methode. Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie van Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Rolinstanties kunnen naamomzetting van virtuele machines binnen hetzelfde virtuele netwerk met de FQDN die gebruikmaakt van de naam van de VM samen met het achtervoegsel 'internal.cloudapp.net' uitvoeren. In dit geval naamomzetting is echter alleen geslaagd als de instantie de naam van de VM gedefinieerd is in de [rol Schema (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Rolinstanties die u wilt uitvoeren van naamomzetting van virtuele machines in een ander virtueel netwerk (FQDN-naam met behulp van de **internal.cloudapp.net** achtervoegsel) hebben om dit te doen via aangepaste DNS-servers forwarding tussen de twee virtuele netwerken, zoals beschreven in Deze sectie.
>

![Tussen virtuele netwerken DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Wanneer u Azure verschafte naamomzetting, een interne DNS-achtervoegsel (`*.internal.cloudapp.net`) met elke virtuele machine wordt verstrekt door Azure van DHCP. Dit achtervoegsel kunt hostnaamomzetting omdat de hostnaam records zich in de *internal.cloudapp.net* zone. Wanneer u uw eigen name resolution-oplossing, wordt dit achtervoegsel naar virtuele machines niet opgegeven, omdat deze andere DNS-architecturen (zoals scenario's voor het domein verstoort). In plaats daarvan Azure biedt een niet-functionerende-tijdelijke aanduiding (*reddog.microsoft.com*).

Indien nodig, kan het interne DNS-achtervoegsel worden bepaald met PowerShell of de API:

* Voor virtuele netwerken in het Resource Manager-implementatiemodel, het achtervoegsel is beschikbaar via de [netwerkinterfacekaart](virtual-network-network-interface.md) resource of via de [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) cmdlet.
* In de klassieke implementatiemodellen, het achtervoegsel is beschikbaar via de [ophalen implementatie API](https://msdn.microsoft.com/library/azure/ee460804.aspx) aanroepen of via de [Get-AzureVM-Debug](/powershell/module/azure/get-azurevm) cmdlet.

Als het doorsturen van query's in Azure wordt niet voldaan aan uw behoeften, moet u om uw eigen DNS-oplossing te bieden. Uw DNS-oplossing moet:

* Geef de juiste hostnaamomzetting, via [DDNS](virtual-networks-name-resolution-ddns.md), bijvoorbeeld. Houd er rekening mee, met behulp van DDNS moet u wellicht uitschakelen record opruiming van DNS-als Azure DHCP-leases lang zijn en opruiming van DNS verwijderen voortijdig registreert. 
* Juiste naamomzetting om toe te staan de omzetting van namen van de externe domein opgeven.
* Toegankelijk (TCP en UDP op poort 53) van de clients die het fungeert en kunnen toegang tot internet.
* Beveiligd is tegen toegang via internet, om te beperken van bedreigingen die door externe agents.

> [!NOTE]
> Voor de beste prestaties bij gebruik van Azure Virtual machines als DNS-servers, IPv6 moet zijn uitgeschakeld en een [openbare IP op exemplaarniveau](virtual-networks-instance-level-public-ip.md) moet worden toegewezen aan elke DNS-server VM. Zie voor aanvullende prestatieanalyse en optimalisaties bij gebruik van Windows Server als uw DNS-server [Name resolution-prestaties van een recursieve Windows DNS-Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web Apps
Als u uitvoeren van naamomzetting van uw App Service Web-App is gekoppeld aan een virtueel netwerk, aan virtuele machines in hetzelfde virtuele netwerk wilt, klik naast het instellen van een aangepaste DNS-server met een DNS-doorstuurserver verzendt die query's naar Azure (virtuele IP-adres 168.63.129.16) , moet u ook de volgende stappen uitvoeren:
* Integratie van virtueel netwerk voor uw App Service Web-App inschakelen als nog niet gedaan, zoals beschreven in [uw app integreren met een virtueel netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* In de Azure-portal voor het App service-plan die als host fungeert voor de Web-App, selecteer **Sync netwerk** onder **Networking**, **virtuele netwerkintegratie**, zoals wordt weergegeven in de volgende afbeelding:

    ![Naamomzetting van Web Apps virtueel netwerk](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Naamomzetting van uw App Service-Web-App is gekoppeld aan een virtueel netwerk, voor VM's in een ander virtueel netwerk vereist het gebruik van aangepaste DNS-servers op beide virtuele netwerken als volgt:
* Een DNS-server in uw virtuele doelnetwerk op een virtuele machine die u ook query's naar Azure recursieve resolver (virtueel IP-adres 168.63.129.16 doorsturen kunt) instellen. Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie van Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Instellen van een DNS-doorstuurserver in het virtuele netwerk van de bron op een virtuele machine. Deze DNS-doorstuurserver voor het doorsturen van query's naar de DNS-server in uw virtuele doelnetwerk configureren.
* Uw bron DNS-server in de bron van het virtuele netwerk-instellingen configureren.
* Integratie van virtueel netwerk voor uw App Service-Web-App te koppelen aan het virtuele Bronnetwerk, de instructies in inschakelen [uw app integreren met een virtueel netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* In de Azure-portal voor het App service-plan die als host fungeert voor de web-app, selecteer **Sync netwerk** onder **Networking**, **virtuele netwerkintegratie**. 

## <a name="specifying-dns-servers"></a>DNS-servers opgeven
Wanneer u uw eigen DNS-servers gebruikt, biedt Azure de mogelijkheid geven meerdere DNS-servers per virtueel netwerk of per netwerkinterface (Resource Manager) / cloudservice (klassiek). Prioriteit van ophalen DNS-servers die zijn opgegeven voor een cloud service/netwerkinterface via DNS-servers die zijn opgegeven voor het virtuele netwerk.

> [!NOTE]
> Verbindingseigenschappen netwerk, zoals DNS-server IP-adressen, mag niet rechtstreeks vanuit Windows virtuele machines worden bewerkt omdat ze tijdens de service kunnen ophalen gewist retoucheren wanneer de virtuele netwerkadapter wordt vervangen. 
> 
> 

Wanneer u het Resource Manager-implementatiemodel, DNS-servers kunnen worden opgegeven in de Portal API/sjablonen: [virtueel netwerk](https://msdn.microsoft.com/library/azure/mt163661.aspx) en [netwerkinterface](https://msdn.microsoft.com/library/azure/mt163668.aspx), of PowerShell: [virtueel netwerk ](/powershell/module/AzureRM.Network/New-AzureRmVirtualNetwork) en [netwerkinterface](/powershell/module/azurerm.network/new-azurermnetworkinterface).

Wanneer u het klassieke implementatiemodel, DNS-servers voor het virtuele netwerk kunnen worden opgegeven in de Portal of [de *netwerkconfiguratie* bestand](https://msdn.microsoft.com/library/azure/jj157100). Voor cloud-services, DNS-servers worden opgegeven via [de *serviceconfiguratie* bestand](https://msdn.microsoft.com/library/azure/ee758710) of met behulp van PowerShell, met [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Als u de DNS-instellingen voor een virtueel netwerk/virtuele machine die al is geïmplementeerd wijzigt, moet u opnieuw opstarten van elke betrokken VM om de wijzigingen van kracht te laten worden.
> 
> 

## <a name="next-steps"></a>Volgende stappen

Resource Manager-implementatiemodel:

* [Een virtueel netwerk maken of bijwerken](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Maken of bijwerken van een netwerkinterfacekaart](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork)
* [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface)

Klassieke implementatiemodel:

* [Azure Service Configuration Schema](https://msdn.microsoft.com/library/azure/ee758710)
* [Virtual Network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100)
* [Een virtueel netwerk configureren met behulp van een netwerk-configuratiebestand](virtual-networks-using-network-configuration-file.md)
