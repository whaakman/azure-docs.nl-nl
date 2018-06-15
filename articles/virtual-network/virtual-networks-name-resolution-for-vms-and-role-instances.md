---
title: De naamomzetting voor resources in Azure virtuele netwerken | Microsoft Docs
description: De naam van oplossing scenario's voor Azure IaaS, oplossingen voor hybride, tussen verschillende cloud-services, Active Directory en uw eigen DNS-server gebruikt.
services: virtual-network
documentationcenter: na
author: subsarma
manager: vitinnan
editor: ''
ms.assetid: 5d73edde-979a-470a-b28c-e103fcf07e3e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/14/2018
ms.author: subsarma
ms.openlocfilehash: 32d4f72afb4cd18e6b66c52eb78b2fc7b6b75cbd
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
ms.locfileid: "31603654"
---
# <a name="name-resolution-for-resources-in-azure-virtual-networks"></a>Naamomzetting voor resources in Azure virtuele netwerken

Afhankelijk van hoe u Azure gebruiken voor het hosten van IaaS en PaaS hybride oplossingen, moet u mogelijk te maken voor de virtuele machines (VM's) en andere resources zijn geïmplementeerd in een virtueel netwerk met elkaar communiceren. U kunt de communicatie inschakelen met behulp van IP-adressen, is het veel eenvoudiger namen die gemakkelijk kunnen worden onthouden en wijzig niet gebruiken. 

Wanneer resources geïmplementeerd in virtuele netwerken domeinnamen omzetten in het interne IP-adressen moeten, kunnen ze een van twee methoden gebruiken:

* [Azure verschafte naamomzetting](#azure-provided-name-resolution)
* [Naamomzetting die gebruikmaakt van uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server) (die mogelijk query doorsturen naar de Azure DNS-servers) 

Het type van naamomzetting die u gebruikt, is afhankelijk van hoe uw resources moeten met elkaar communiceren. De volgende tabel ziet u scenario's en bijbehorende name resolution-oplossingen:

> [!NOTE]
> Afhankelijk van uw scenario mogelijk u wilt gebruiken van de functie Azure persoonlijke DNS-Zones, waar dit momenteel in de openbare Preview is. Zie voor meer informatie [Azure DNS gebruiken voor persoonlijke domeinen](../dns/private-dns-overview.md).
>

| **Scenario** | **Oplossing** | **Suffix** |
| --- | --- | --- |
| Naamomzetting tussen virtuele machines zich in het hetzelfde virtuele netwerk of Azure Cloud Services-rolexemplaren in dezelfde cloudservice. | [Azure persoonlijke DNS-Zones](../dns/private-dns-overview.md) of [Azure verschafte naamomzetting](#azure-provided-name-resolution) |de hostnaam of FQDN-naam |
| De naamomzetting tussen VM's in verschillende virtuele netwerken of rolexemplaren in een andere cloud-services. |[Azure persoonlijke DNS-Zones](../dns/private-dns-overview.md) of door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN |
| Naamomzetting van een Azure App Service (Web-App, functie of Bot) met behulp van de integratie van virtueel netwerk rolinstanties of virtuele machines in hetzelfde virtuele netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN |
| Naam resolutie van App Service Web Apps naar virtuele machines in hetzelfde virtuele netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN |
| Naam resolutie van App Service Web Apps in een virtueel netwerk voor virtuele machines in een ander virtueel netwerk. |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server-for-web-apps). |Alleen FQDN |
| Omzetting van lokale computer en de service namen van virtuele machines of rolinstanties in Azure. |Door de klant beheerde DNS-servers (lokale domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS gesynchroniseerd zoneoverdrachten, bijvoorbeeld via). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN |
| De resolutie van Azure hostnamen van lokale computers. |Doorsturen van aanvragen naar een door de klant beheerde DNS-proxyserver in de bijbehorende virtuele netwerk, de proxy-server stuurt query's naar Azure voor naamomzetting. Zie [naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Alleen FQDN |
| Omgekeerde DNS voor interne IP-adressen. |[Naamomzetting met uw eigen DNS-server](#name-resolution-that-uses-your-own-dns-server). |Niet van toepassing |
| De naamomzetting tussen VM's of rolexemplaren die zich in verschillende cloud-services, niet in een virtueel netwerk. |Niet van toepassing. Connectiviteit tussen VM's en rolexemplaren in een andere cloud-services wordt niet ondersteund buiten een virtueel netwerk. |Niet van toepassing|

## <a name="azure-provided-name-resolution"></a>Azure verschafte naamomzetting

Samen met de resolutie van openbare DNS-namen biedt Azure interne naamomzetting voor VM's en rolexemplaren die zich in het hetzelfde virtuele netwerk of de service in de cloud bevinden. VM's en -exemplaren in een cloudservice delen de dezelfde DNS-achtervoegsel, zodat alleen de naam van de host voldoende is. Maar in virtuele netwerken zijn geïmplementeerd met behulp van het klassieke implementatiemodel, andere cloud-services hebben verschillende DNS-achtervoegsels. In dit geval moet u de FQDN-naam voor de naamomzetting tussen verschillende cloudservices. In virtuele netwerken zijn geïmplementeerd met het Azure Resource Manager-implementatiemodel, is het DNS-achtervoegsel consistent zijn tussen het virtuele netwerk, zodat de FQDN-naam is niet nodig. DNS-namen kunnen worden toegewezen aan zowel virtuele machines en netwerkinterfaces. Hoewel Azure verschafte naamomzetting geen configuratie vereist is, maar het is niet de juiste keuze is voor alle implementatiescenario's, zoals beschreven in de vorige tabel.

> [!NOTE]
> Wanneer via cloud services-web-en werkrollen, kunt u ook toegang tot het interne IP-adressen met de Azure Service Management REST API rolinstanties. Zie voor meer informatie de [Service Management REST API-verwijzing](https://msdn.microsoft.com/library/azure/ee460799.aspx). Het adres is gebaseerd op het nummer van de rol en het exemplaar. 
> 
> 

### <a name="features"></a>Functies

Azure verschafte naamomzetting omvat de volgende functies:
* Eenvoudig te gebruiken. Er is geen configuratie vereist.
* Hoge beschikbaarheid. U hoeft niet te maken en beheren van clusters van uw eigen DNS-servers.
* U kunt de service gebruiken in combinatie met uw eigen DNS-servers, zowel on-premises en Azure hostnamen omzetten.
* U kunt de naamomzetting tussen VM's en rolinstanties in dezelfde cloudservice, zonder de noodzaak van een FQDN-naam gebruiken.
* U kunt de naamomzetting tussen virtuele machines in virtuele netwerken die gebruikmaken van het Azure Resource Manager-implementatiemodel, zonder dat nodig is voor een FQDN-naam gebruiken. Virtuele netwerken in het klassieke implementatiemodel vereisen een FQDN-naam wanneer u zijn het omzetten van namen in andere cloudservices. 
* U kunt de namen van de hosts die het beste uw implementaties te beschrijven in plaats van het werken met de automatisch gegenereerde naam.

### <a name="considerations"></a>Overwegingen

Punten in overweging moet nemen wanneer u Azure verschafte naamomzetting gebruikt:
* Het gemaakte Azure DNS-achtervoegsel kan niet worden gewijzigd.
* U kunt uw eigen records kan niet handmatig registreren.
* WINS- en NetBIOS worden niet ondersteund. U kunt uw virtuele machines in Windows Verkenner niet zien.
* Hostnamen moet DNS-compatibele. Namen moeten alleen 0-9, a-z, gebruiken en '-', en mag niet beginnen of eindigen met een '-'.
* DNS-query-verkeer is voor elke virtuele machine beperkt. Beperking mag niet van invloed op de meeste toepassingen. Indien aanvraag beperking is waargenomen, zorg ervoor dat caching aan clientzijde is ingeschakeld. Zie voor meer informatie [DNS-clientconfiguratie](#dns-client-configuration).
* Alleen virtuele machines in de eerste 180 cloudservices zijn geregistreerd voor elke virtuele netwerk in een klassieke implementatiemodel. Deze beperking geldt niet voor virtuele netwerken in Azure Resource Manager.

## <a name="dns-client-configuration"></a>Configuratie van DNS-client

Deze sectie behandelt caching aan clientzijde en clientzijde nieuwe pogingen.

### <a name="client-side-caching"></a>Caching aan clientzijde

Niet elke DNS-query moet worden verzonden via het netwerk. Caching aan clientzijde helpt de latentie te verminderen en de herstelmogelijkheden bij netwerk-blips verbeteren door terugkerende DNS-query's uit de lokale cache op te lossen. DNS-records bevatten een mechanisme time to live (TTL) waarmee de cache voor het opslaan van de record voor zo lang mogelijk zonder enige impact op record versheid. Caching aan clientzijde is dus geschikt is voor de meeste situaties.

De standaard Windows-DNS-client heeft een ingebouwde DNS-cache. Sommige Linux-distributies omvatten geen cache standaard. Als u dat nog niet een lokale cache is vinden, moet u een DNS-cache toevoegen aan elke Linux-VM.

Er zijn een aantal verschillende de DNS-cache van de pakketten beschikbaar (zoals dnsmasq). Dit is het dnsmasq installeren op de meest voorkomende distributies:

* **Ubuntu (maakt gebruik van resolvconf)**:
  * Installeer het pakket dnsmasq met `sudo apt-get install dnsmasq`.
* **SUSE (maakt gebruik van netconf)**:
  * Installeer het pakket dnsmasq met `sudo zypper install dnsmasq`.
  * Schakel de service dnsmasq met `systemctl enable dnsmasq.service`. 
  * Start de service dnsmasq met `systemctl start dnsmasq.service`. 
  * Bewerken **/etc/sysconfig/network/config**, en wijzig *NETCONFIG_DNS_FORWARDER = ""* naar *dnsmasq*.
  * Bijwerken van resolv.conf met `netconfig update`, in te stellen van de cache als de lokale DNS-resolver.
* **OpenLogic (maakt gebruik van NetworkManager)**:
  * Installeer het pakket dnsmasq met `sudo yum install dnsmasq`.
  * Schakel de service dnsmasq met `systemctl enable dnsmasq.service`.
  * Start de service dnsmasq met `systemctl start dnsmasq.service`.
  * Voeg *toevoegen domain name servers 127.0.0.1;* naar **/etc/dhclient-eth0.conf**.
  * Opnieuw opstarten van de netwerkservice met `service network restart`, in te stellen van de cache als de lokale DNS-resolver.

> [!NOTE]
> Het pakket dnsmasq is slechts één van veel DNS-caches beschikbaar voor Linux. Voordat u deze gebruikt, Controleer geschikt is voor uw specifieke behoeften en controleer of er geen andere cache is geïnstalleerd.
> 
> 
    
### <a name="client-side-retries"></a>Nieuwe pogingen voor clientzijde

DNS is voornamelijk een UDP-protocol. Omdat het UDP-protocol niet levering van berichten garanderen, wordt in het DNS-protocol zelf Pogingslogica afgehandeld. Elke DNS-client (besturingssysteem) kan zich voordoen verschillende Pogingslogica, afhankelijk van de maker voorkeur:

* Windows besturingssystemen opnieuw proberen na een tweede en nog een keer na de andere twee seconden, vier, en een andere vier seconden. 
* De standaard Linux setup pogingen na vijf seconden. U wordt aangeraden de specificaties opnieuw wijzigen in vijf keer met een interval van één seconde.

Controleer de huidige instellingen op een Linux-VM met `cat /etc/resolv.conf`. Bekijk de *opties* regel, bijvoorbeeld:

```bash
options timeout:1 attempts:5
```

Het bestand resolv.conf meestal automatisch wordt gegenereerd en mag niet worden bewerkt. De specifieke stappen voor het toevoegen van de *opties* regel verschillen per distributiepunt:

* **Ubuntu** (maakt gebruik van resolvconf):
  1. Voeg de *opties* te **/etc/resolveconf/resolv.conf.d/head**.
  2. Voer `resolvconf -u` om bij te werken.
* **SUSE** (maakt gebruik van netconf):
  1. Toevoegen *timeout:1 pogingen: 5* naar de **NETCONFIG_DNS_RESOLVER_OPTIONS = ""** parameter in **/etc/sysconfig/network/config**. 
  2. Voer `netconfig update` om bij te werken.
* **OpenLogic** (maakt gebruik van NetworkManager):
  1. Voeg *echo 'opties timeout:1 pogingen: 5'* naar **/etc/NetworkManager/dispatcher.d/11-dhclient**. 
  2. Werken met `service network restart`.

## <a name="name-resolution-that-uses-your-own-dns-server"></a>Naamomzetting die gebruikmaakt van uw eigen DNS-server

Deze sectie bevat informatie over virtuele machines, rolinstanties en web-apps.

### <a name="vms-and-role-instances"></a>VM's en rolinstanties

De behoeften van uw name resolution mogelijk verder dan de functies van Azure. U wilt bijvoorbeeld gebruik van Microsoft Windows Server Active Directory-domeinen, DNS-naamomzetting tussen virtuele netwerken. Ten aanzien van deze scenario's, biedt Azure de mogelijkheid voor u om uw eigen DNS-servers te gebruiken.

DNS-servers binnen een virtueel netwerk kunnen de DNS-query's doorsturen naar het recursieve resolvers in Azure. Hiermee kunt u voor het omzetten van hostnamen binnen dit virtuele netwerk. Een domeincontroller (DC) uitgevoerd in Azure kan bijvoorbeeld reageren op DNS-query's voor de domeinen en alle andere aanvragen door te sturen naar Azure. Doorsturen van query's kunt virtuele machines om te zien van uw lokale bronnen (via de domeincontroller) en de Azure verschafte hostnamen (via de doorstuurserver). Toegang tot de recursieve resolvers in Azure wordt opgegeven via het virtuele IP-adres 168.63.129.16.

DNS-doorsturen ook kan DNS-omzetting tussen virtuele netwerken en kan uw lokale machine Azure verschafte hostnamen om te zetten. Om om te zetten in een VM-hostnaam, de DNS-server VM moet zich in hetzelfde virtuele netwerk bevinden, en worden geconfigureerd voor doorsturen host naam query's naar Azure. Omdat het DNS-achtervoegsel in elk virtueel netwerk verschilt, kunt u voorwaardelijk doorsturen regels DNS-query's verzenden naar het juiste virtuele netwerk voor naamomzetting. De volgende afbeelding ziet u twee virtuele netwerken en een on-premises netwerk tijdens het doorzoeken van DNS-omzetting tussen virtuele netwerken met deze methode. Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie van Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

> [!NOTE]
> Een rolinstantie kan naamomzetting van virtuele machines binnen hetzelfde virtuele netwerk uitvoeren. Dit gebeurt met behulp van de FQDN-naam, die uit de VM-hostnaam bestaat en **internal.cloudapp.net** DNS-achtervoegsel. In dit geval naamomzetting is echter alleen geslaagd als de instantie de naam van de VM gedefinieerd is in de [rol Schema (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/jj156212.aspx). 
>    <Role name="<role-name>" vmName="<vm-name>">
> 
> Rolinstanties die u wilt uitvoeren van naamomzetting van virtuele machines in een ander virtueel netwerk (FQDN-naam met behulp van de **internal.cloudapp.net** achtervoegsel) hebben om dit te doen met behulp van de methode die wordt beschreven in deze sectie (aangepaste DNS-servers tussen doorsturen de twee virtuele netwerken).
>

![Diagram van DNS tussen virtuele netwerken](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Wanneer u van Azure verschafte naamomzetting gebruikmaakt, Azure Dynamic Host Configuration Protocol (DHCP) biedt een interne DNS-achtervoegsel (**. internal.cloudapp.net**) met elke virtuele machine. Dit achtervoegsel kan omzetten van de hostnaam, omdat de hostrecords in de **internal.cloudapp.net** zone. Wanneer u uw eigen name resolution-oplossing gebruikt, wordt dit achtervoegsel naar virtuele machines niet opgegeven, omdat deze andere DNS-architecturen (zoals scenario's voor het domein verstoort). In plaats daarvan Azure biedt een niet-functionerende-tijdelijke aanduiding (*reddog.microsoft.com*).

Indien nodig, kunt u de interne DNS-achtervoegsel bepalen met behulp van PowerShell of de API.

* Voor virtuele netwerken in Azure Resource Manager-implementatiemodel, het achtervoegsel is beschikbaar via de [netwerkinterface REST-API](/rest/api/virtualnetwork/networkinterfaces/get), wordt de [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) PowerShell-cmdlet en de [az netwerk nic weergeven](/cli/azure/network/nic#az-network-nic-show) Azure CLI-opdracht.
* In de klassieke implementatiemodellen, het achtervoegsel is beschikbaar via de [ophalen implementatie API](https://msdn.microsoft.com/library/azure/ee460804.aspx) aanroepen of de [Get-AzureVM-Debug](/powershell/module/azure/get-azurevm) cmdlet.

Als het doorsturen van query's in Azure wordt niet voldaan aan uw behoeften, dient u uw eigen DNS-oplossing. Uw DNS-oplossing moet:

* Geef een juiste naamomzetting [DDNS](virtual-networks-name-resolution-ddns.md), bijvoorbeeld. Als u DDNS gebruikt, moet u mogelijk uitschakelen opruiming van DNS-record. Azure DHCP-leases lang zijn en opruiming mogelijk verwijderen DNS-records voortijdig. 
* Juiste naamomzetting om toe te staan de omzetting van namen van de externe domein opgeven.
* Toegankelijk (TCP en UDP op poort 53) van de clients die het fungeert, en kunnen toegang tot internet.
* Beveiligd is tegen toegang via internet, om te beperken van bedreigingen die door externe agents.

> [!NOTE]
> Voor de beste prestaties wanneer u virtuele Azure-machines als DNS-servers, moet IPv6 worden uitgeschakeld. Een [openbaar IP-adres](virtual-network-public-ip-address.md) moet worden toegewezen aan elke DNS-server VM. Voor extra prestatieanalyse en optimalisatie van wanneer u van Windows Server als uw DNS-server gebruikmaakt, raadpleegt u [Name resolution-prestaties van een recursieve Windows DNS-Server 2012 R2](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx).
> 
> 

### <a name="web-apps"></a>Web-apps
Stel dat u wilt uitvoeren van naamomzetting van uw web-app die is gebouwd met App Service, gekoppeld aan een virtueel netwerk, aan virtuele machines in hetzelfde virtuele netwerk. Naast het instellen van een aangepaste DNS-server die een DNS-doorstuurserver waarmee query's worden doorgestuurd naar Azure (virtuele IP-adres 168.63.129.16) heeft de volgende stappen uitvoeren:
1. Integratie van virtueel netwerk voor uw web-app inschakelen als nog niet gedaan, zoals beschreven in [uw app integreren met een virtueel netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. In de Azure-portal voor het App Service-plan die als host fungeert voor de web-app, selecteer **Sync netwerk** onder **Networking**, **virtuele netwerkintegratie**.

    ![Schermopname van het virtuele netwerk-naamomzetting](./media/virtual-networks-name-resolution-for-vms-and-role-instances/webapps-dns.png)

Als u uitvoeren van naamomzetting van uw web-app die is gebouwd met App Service wilt, gekoppeld aan een virtueel netwerk, voor VM's in een ander virtueel netwerk hebt u aangepaste DNS-servers op beide virtuele netwerken als volgt gebruiken: 
* Een DNS-server in uw virtuele netwerk van doel op een virtuele machine die u ook query's naar het recursieve resolver in Azure (virtuele IP-adres 168.63.129.16 doorsturen kunt) instellen. Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie van Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder). 
* Instellen van een DNS-doorstuurserver in het virtuele netwerk van de bron op een virtuele machine. Deze DNS-doorstuurserver voor het doorsturen van query's naar de DNS-server in uw virtuele doelnetwerk configureren.
* Uw bron DNS-server in de bron van het virtuele netwerk-instellingen configureren.
* Integratie van virtueel netwerk voor uw web-app te koppelen aan het virtuele Bronnetwerk, de instructies in inschakelen [uw app integreren met een virtueel netwerk](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* In de Azure-portal voor het App Service-plan die als host fungeert voor de web-app, selecteer **Sync netwerk** onder **Networking**, **virtuele netwerkintegratie**. 

## <a name="specify-dns-servers"></a>Geef DNS-servers
Wanneer u uw eigen DNS-servers gebruikt, biedt Azure de mogelijkheid om op te geven van meerdere DNS-servers per virtueel netwerk. U kunt ook meerdere DNS-servers per netwerkinterface (voor Azure Resource Manager) of per cloudservice (voor het klassieke implementatiemodel) opgeven. DNS-servers die zijn opgegeven voor een interface of cloud netwerkservice krijgen voorrang via DNS-servers die zijn opgegeven voor het virtuele netwerk.

> [!NOTE]
> Eigenschappen van netwerkverbinding, zoals DNS-server IP-adressen, mag niet worden bewerkt rechtstreeks vanuit Windows virtuele machines. Dit is omdat ze tijdens de service mogelijk ophalen gewist retoucheren wanneer de virtuele netwerkadapter wordt vervangen. 
> 
> 

Wanneer u van het Azure Resource Manager-implementatiemodel gebruikmaakt, kunt u DNS-servers voor een virtueel netwerk en een netwerkinterface opgeven. Zie voor meer informatie [beheren van een virtueel netwerk](manage-virtual-network.md) en [beheren van een netwerkinterface](virtual-network-network-interface.md).

Wanneer u van het klassieke implementatiemodel gebruikmaakt, kunt u DNS-servers voor het virtuele netwerk opgeven in de Azure portal of de [netwerkconfiguratiebestand](https://msdn.microsoft.com/library/azure/jj157100). Voor cloud-services, kunt u DNS-servers via de [Service configuratiebestand](https://msdn.microsoft.com/library/azure/ee758710) of met behulp van PowerShell, met [New-AzureVM](/powershell/module/azure/new-azurevm).

> [!NOTE]
> Als u de DNS-instellingen voor een virtueel netwerk of de virtuele machine die al is geïmplementeerd wijzigt, moet u opnieuw opstarten van elke betrokken VM om de wijzigingen van kracht te laten worden.
> 
> 

## <a name="next-steps"></a>Volgende stappen

Azure Resource Manager-implementatiemodel:

* [Een virtueel netwerk beheren](manage-virtual-network.md)
* [Een netwerkinterface beheren](virtual-network-network-interface.md)

Klassieke implementatiemodel:

* [Het Schema van Azure-Service](https://msdn.microsoft.com/library/azure/ee758710)
* [Virtual Network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100)
* [Een virtueel netwerk configureren met behulp van een netwerk-configuratiebestand](virtual-networks-using-network-configuration-file.md)
