---
title: Naamomzetting voor VM's en Rolinstanties
description: 'Naam van oplossing scenario''s voor Azure IaaS, oplossingen voor hybride, tussen verschillende cloud-services, Active Directory en met uw eigen DNS-server '
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
ms.date: 12/06/2016
ms.author: jdial
ms.openlocfilehash: 5a298f535308cff90ddd249594b7bb5e36909867
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="name-resolution-for-vms-and-role-instances"></a>Name Resolution for VMs and Role Instances (Naamomzetting voor virtuele machines en rolinstanties)
Afhankelijk van hoe u Azure gebruiken voor het hosten van IaaS en PaaS hybride oplossingen, moet u mogelijk maken voor de VM's en rolinstanties die u maakt om te communiceren met elkaar. Deze communicatie kan worden gedaan met behulp van IP-adressen, is het veel eenvoudiger namen die gemakkelijk kunnen worden onthouden en wijzig niet gebruiken. 

Wanneer rolinstanties en virtuele machines die worden gehost in Azure nodig hebt om te zetten domeinnamen interne IP-adressen, kunnen ze een van twee methoden gebruiken:

* [Azure verschafte naamomzetting](#azure-provided-name-resolution)
* [Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) (die mogelijk query doorsturen naar de Azure DNS-servers) 

Het type van naamomzetting die u gebruikt, is afhankelijk van hoe uw VM's en rolexemplaren moeten met elkaar communiceren.

**De volgende tabel ziet u scenario's en bijbehorende name resolution-oplossingen:**

| **Scenario** | **Oplossing** | **Suffix** |
| --- | --- | --- |
| Naamomzetting tussen de rolinstanties of virtuele machines zich in dezelfde cloudservice- of virtueel netwerk |[Azure verschafte naamomzetting](#azure-provided-name-resolution) |de hostnaam of FQDN-naam |
| De naamomzetting van een Azure App Service (Web-App, functie, Bot, enzovoort) met VNET integratie rolinstanties of virtuele machines zich in hetzelfde virtuele netwerk |Door de klant beheerde DNS-servers doorsturen van query's tussen vnets voor de omzetting van Azure (DNS-proxy).  Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) |Alleen FQDN |
| Naamomzetting tussen de rolinstanties of virtuele machines zich in verschillende virtuele netwerken |Door de klant beheerde DNS-servers doorsturen van query's tussen vnets voor de omzetting van Azure (DNS-proxy).  Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) |Alleen FQDN |
| Omzetting van lokale computer en de service namen van rolexemplaren te controleren of de virtuele machines in Azure |Door de klant beheerde DNS-servers (bijvoorbeeld op de lokale domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS gesynchroniseerd met zoneoverdrachten).  Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) |Alleen FQDN |
| De resolutie van Azure hostnamen van lokale computers |Doorsturen van aanvragen naar een door de klant beheerde DNS-proxyserver in het bijbehorende vnet, de proxy-server stuurt query's naar Azure voor naamomzetting. Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) |Alleen FQDN |
| Omgekeerde DNS voor interne IP-adressen |[Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) |N.v.t. |
| Naamomzetting tussen VM's of rolexemplaren die zich in verschillende cloud-services, niet in een virtueel netwerk |Niet van toepassing. Connectiviteit tussen VM's en rolexemplaren in een andere cloud-services wordt niet ondersteund buiten een virtueel netwerk. |N.v.t. |

## <a name="azure-provided-name-resolution"></a>Azure verschafte naamomzetting
Samen met de resolutie van openbare DNS-namen biedt Azure interne naamomzetting voor VM's en rolexemplaren die zich in het hetzelfde virtuele netwerk of de service in de cloud bevinden.  Virtuele machines/exemplaren in een cloudservice delen de dezelfde DNS-achtervoegsel (zodat de hostnaam die alleen voldoende is), maar in de klassieke virtuele netwerken verschillende cloud services hebben verschillende DNS-achtervoegsels zodat de FQDN-naam nodig is voor de naamomzetting tussen verschillende cloudservices.  Het DNS-achtervoegsel is consistent in het virtuele netwerk (zodat de FQDN-naam is niet nodig) en DNS-namen kunnen worden toegewezen aan zowel NIC's en virtuele machines in virtuele netwerken in het Resource Manager-implementatiemodel. Hoewel Azure verschafte naamomzetting geen configuratie vereist is, is niet de juiste keuze is voor alle implementatiescenario's, zoals in de vorige tabel.

> [!NOTE]
> In het geval van web- en werkrollen rollen, kunt u ook de interne IP-adressen van de rolexemplaren op basis van de naam en het exemplaar nummer van rol met de REST-API van Azure Service Management openen. Zie voor meer informatie [Service Management REST API-verwijzing](https://msdn.microsoft.com/library/azure/ee460799.aspx).
> 
> 

### <a name="features-and-considerations"></a>Functies en overwegingen
**Functies:**

* Gebruiksgemak: geen configuratie is vereist om te kunnen gebruiken van Azure verschafte naamomzetting.
* De opgegeven Azure name resolution-service is maximaal beschikbaar, zodat u de noodzaak om te maken en beheren van clusters van uw eigen DNS-servers.
* Kan worden gebruikt in combinatie met uw eigen DNS-servers om op te lossen, zowel on-premises en Azure hostnamen.
* Naamomzetting is tussen de rol exemplaren/virtuele machines in dezelfde cloudservice zonder dat nodig is voor een FQDN-naam opgegeven.
* Naamomzetting is tussen virtuele machines in virtuele netwerken die gebruikmaken van het Resource Manager-implementatiemodel, zonder dat nodig is voor de FQDN-naam opgegeven. Virtuele netwerken in het klassieke implementatiemodel moeten de FQDN-naam bij het omzetten van namen in andere cloudservices. 
* U kunt hostnamen die het beste uw implementaties te beschrijven in plaats van het werken met de automatisch gegenereerde naam.

**Overwegingen:**

* Het gemaakte Azure DNS-achtervoegsel kan niet worden gewijzigd.
* U kunt uw eigen records kan niet handmatig registreren.
* WINS- en NetBIOS worden niet ondersteund. (U kunt uw virtuele machines in Windows Verkenner niet zien.)
* Hostnamen moet DNS-compatibele (ze moeten gebruiken alleen 0-9, a-z en '-', en mag niet beginnen of eindigen met een '-'. Zie RFC 3696 sectie 2.)
* DNS-query-verkeer is voor elke virtuele machine beperkt. Dit mag niet van invloed op de meeste toepassingen.  Indien aanvraag beperking is waargenomen, zorg ervoor dat caching aan clientzijde is ingeschakeld.  Zie voor meer informatie [ophalen van de meest uit Azure verschafte naamomzetting](#Getting-the-most-from-Azure-provided-name-resolution).
* Alleen virtuele machines in de eerste 180 cloudservices zijn geregistreerd voor elke virtuele netwerk in een klassieke implementatiemodel. Dit geldt niet voor virtuele netwerken in het Resource Manager-implementatiemodel.

### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Ophalen van de meest uit Azure verschafte naamomzetting
**Caching aan clientzijde:**

Niet elke DNS-query moet worden verzonden via het netwerk.  Caching aan clientzijde helpt de latentie te verminderen en de herstelmogelijkheden bij netwerk blips verbeteren door terugkerende DNS-query's uit de lokale cache op te lossen.  DNS-records bevatten een Time-To-Live (TTL) waarmee de cache voor het opslaan van de record voor zo lang mogelijk zonder enige impact op record nieuwheid zodat caching aan clientzijde geschikt voor de meeste situaties is.

De standaard Windows-DNS-Client heeft een ingebouwde DNS-cache.  Sommige Linux distributies omvatten geen cache standaard, wordt aangeraden dat een worden toegevoegd aan elke Linux VM (na controleren dat nog niet is een lokale cache).

Er zijn een aantal verschillende de DNS-cache van de pakketten beschikbaar. Bijvoorbeeld: dnsmasq. Dnsmasq installeren op de meest voorkomende distributies een overzicht van de volgende stappen uit:

* **Ubuntu (maakt gebruik van resolvconf)**:
  * Installeer alleen het dnsmasq-pakket ('sudo apt get-installatie dnsmasq').
* **SUSE (maakt gebruik van netconf)**:
  * Installeer het pakket dnsmasq ('sudo zypper installeren dnsmasq') 
  * de service dnsmasq ('systemctl inschakelen dnsmasq.service') inschakelen 
  * Start de service dnsmasq ('systemctl start dnsmasq.service") 
  * bewerken '/ etc/sysconfig/netwerk/config' en wijzigt u NETCONFIG_DNS_FORWARDER = "' naar 'dnsmasq'
  * resolv.conf ('netconfig-update') om in te stellen van de cache als de lokale DNS-resolver bijwerken
* **OpenLogic (maakt gebruik van NetworkManager)**:
  * Installeer het pakket dnsmasq ('sudo yum installeren dnsmasq')
  * de service dnsmasq ('systemctl inschakelen dnsmasq.service') inschakelen
  * Start de service dnsmasq ('systemctl start dnsmasq.service")
  * Voeg toe 'toevoegen domain name servers 127.0.0.1;' naar '/etc/dhclient-eth0.conf'
  * opnieuw opstarten van de Netwerkservice ('service netwerk opnieuw starten') om in te stellen van de cache als de lokale DNS-resolver

> [!NOTE]
> Het pakket 'dnsmasq' is slechts een van de vele DNS-caches beschikbaar voor Linux.  Controleer voordat u deze gebruikt geschikt is voor uw specifieke behoeften en die geen andere cache is geïnstalleerd.
> 
> 

**Client-side nieuwe pogingen:**

DNS is voornamelijk een UDP-protocol.  Als het UDP-protocol niet levering van berichten garanderen, wordt in het DNS-protocol zelf Pogingslogica afgehandeld.  Elke DNS-client (besturingssysteem) kan verschillende Pogingslogica, afhankelijk van de voorkeur auteurs vertonen:

* Windows besturingssystemen opnieuw proberen na 1 seconde en vervolgens na een andere 2, 4 en een andere 4 seconden. 
* De standaard Linux setup pogingen na vijf seconden.  Het is raadzaam deze opnieuw met een 1 tweede interval 5 keer wijzigen.  

Gebruik de opdracht 'cat /etc/resolv.conf' en controleer de huidige instellingen op een Linux-VM vervolgens kijkt naar de regel 'options', bijvoorbeeld:

    options timeout:1 attempts:5

Het bestand resolv.conf wordt doorgaans automatisch gegenereerd en mag niet worden bewerkt.  De specifieke stappen voor het toevoegen van de regel 'options' verschillen per distro:

* **Ubuntu** (maakt gebruik van resolvconf):
  * de opties regel toe te voegen ' / etc/resolveconf/resolv.conf.d/head' 
  * Voer resolvconf -u bijwerken
* **SUSE** (maakt gebruik van netconf):
  * 'timeout:1 pogingen: 5' toevoegen aan de NETCONFIG_DNS_RESOLVER_OPTIONS = "" parameter in '/ etc/sysconfig/netwerk/config' 
  * netconfig-update uitvoeren om bij te werken
* **OpenLogic** (maakt gebruik van NetworkManager):
  * 'echo 'opties timeout:1 pogingen: 5' ' toevoegen aan ' / etc/NetworkManager/dispatcher.d/11-dhclient' 
  * Voer 'service netwerk opnieuw is opgestart, bijwerken

## <a name="name-resolution-using-your-own-dns-server"></a>Naamomzetting met uw eigen DNS-server
Er zijn een aantal situaties waarbij de behoeften van uw name resolution mogelijk gaat voorbij de functies van Azure, bijvoorbeeld wanneer met behulp van Active Directory-domeinen of wanneer u de DNS-omzetting tussen virtuele netwerken vereist.  Ten aanzien van deze scenario's, biedt Azure de mogelijkheid voor u om uw eigen DNS-servers te gebruiken.  

DNS-servers binnen een virtueel netwerk kunnen de DNS-query's doorsturen naar Azure recursieve resolvers omzetten van hostnamen binnen dit virtuele netwerk.  Bijvoorbeeld, een domeincontroller (DC) uitgevoerd in Azure reageren op DNS-query's voor de domeinen en alle andere aanvragen door te sturen naar Azure.  Hierdoor kunnen virtuele machines om te zien van uw lokale bronnen (via de domeincontroller) en de Azure verschafte hostnamen (via de doorstuurserver).  Toegang tot Azure recursieve resolvers wordt opgegeven via het virtuele IP-adres 168.63.129.16.

DNS-forwarding tussen virtuele netwerken DNS-omzetting Hiermee ook- en kan uw lokale machine Azure verschafte hostnamen omzetten.  Om een VM-hostnaam niet omzetten, is de DNS-server VM moet zich in hetzelfde virtuele netwerk bevinden, en worden geconfigureerd voor doorsturen hostnaam query's naar Azure.  Als het DNS-achtervoegsel in elke vnet anders is, kunt u voorwaardelijk doorsturen regels voor het verzenden van DNS-query's naar het juiste vnet voor de omzetting.  De volgende afbeelding ziet u twee virtuele netwerken en een on-premises netwerk doen tussen virtuele netwerken DNS-omzetting met deze methode.  Een voorbeeld van de DNS-doorstuurserver is beschikbaar in de [galerie van Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/301-dns-forwarder/) en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/301-dns-forwarder).

![Tussen virtuele netwerken DNS](./media/virtual-networks-name-resolution-for-vms-and-role-instances/inter-vnet-dns.png)

Wanneer u Azure verschafte naamomzetting, een interne DNS-achtervoegsel (*. internal.cloudapp.net) is opgegeven voor elke virtuele machine met behulp van DHCP.  Zo kunnen hostnaamomzetting als de hostnaam records in de zone internal.cloudapp.net worden.  Wanneer u uw eigen name resolution-oplossing, is het achtervoegsel IDN's verstrekt aan virtuele machines omdat deze andere DNS-architecturen (zoals scenario's voor het domein verstoort).  In plaats daarvan bieden we een niet-functionerende tijdelijke aanduiding (reddog.microsoft.com).  

Indien nodig, kan het interne DNS-achtervoegsel worden bepaald met PowerShell of de API:

* Voor virtuele netwerken in het Resource Manager-implementatiemodel, het achtervoegsel is beschikbaar via de [netwerkinterfacekaart](https://msdn.microsoft.com/library/azure/mt163668.aspx) resource of via de [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) cmdlet.    
* In de klassieke implementatiemodellen, het achtervoegsel is beschikbaar via de [ophalen implementatie API](https://msdn.microsoft.com/library/azure/ee460804.aspx) aanroepen of via de [Get-AzureVM-Debug](https://msdn.microsoft.com/library/azure/dn495236.aspx) cmdlet.

Als het doorsturen van query's in Azure wordt niet voldaan aan uw behoeften, moet u om uw eigen DNS-oplossing te bieden.  Uw DNS-oplossing moet:

* Geef juiste hostnaamomzetting, bijvoorbeeld [DDNS](virtual-networks-name-resolution-ddns.md).  Houd er rekening mee, moet u wellicht uitschakelen record opruiming van DNS-als Azure DHCP-leases zeer lange en opruiming zijn DDNS verwijderen DNS-records voortijdig. 
* Juiste naamomzetting om toe te staan de omzetting van namen van de externe domein opgeven.
* Toegankelijk (TCP en UDP op poort 53) van de clients die het fungeert en kunnen toegang tot internet.
* Beveiligd is tegen toegang via internet, om te beperken van bedreigingen die door externe agents.

> [!NOTE]
> Voor de beste prestaties bij gebruik van Azure Virtual machines als DNS-servers, IPv6 moet zijn uitgeschakeld en een [openbare IP op exemplaarniveau](virtual-networks-instance-level-public-ip.md) moet worden toegewezen aan elke DNS-server VM.  Als u Windows Server gebruiken als uw DNS-server, [in dit artikel](http://blogs.technet.com/b/networking/archive/2015/08/19/name-resolution-performance-of-a-recursive-windows-dns-server-2012-r2.aspx) biedt extra prestatieanalyse en optimalisatie.
> 
> 

### <a name="specifying-dns-servers"></a>DNS-servers opgeven
Wanneer u uw eigen DNS-servers gebruikt, biedt Azure de mogelijkheid geven meerdere DNS-servers per virtueel netwerk of per netwerkinterface (Resource Manager) / cloudservice (klassiek).  DNS-servers die zijn opgegeven voor een cloud service/netwerkinterface ophalen voorrang boven die zijn opgegeven voor het virtuele netwerk.

> [!NOTE]
> Verbindingseigenschappen netwerk, zoals DNS-server IP-adressen, mag niet rechtstreeks vanuit Windows virtuele machines worden bewerkt omdat ze tijdens de service kunnen ophalen gewist retoucheren wanneer de virtuele netwerkadapter wordt vervangen. 
> 
> 

Wanneer u het Resource Manager-implementatiemodel, DNS-servers kunnen worden opgegeven in de Portal API/sjablonen ([vnet](https://msdn.microsoft.com/library/azure/mt163661.aspx), [nic](https://msdn.microsoft.com/library/azure/mt163668.aspx)) of PowerShell ([vnet](https://msdn.microsoft.com/library/mt603657.aspx), [nic](https://msdn.microsoft.com/library/mt619370.aspx)).

Wanneer u het klassieke implementatiemodel, DNS-servers voor het virtuele netwerk kunnen worden opgegeven in de Portal of [de *netwerkconfiguratie* bestand](https://msdn.microsoft.com/library/azure/jj157100).  Voor cloud-services, de DNS-servers worden opgegeven via [de *serviceconfiguratie* bestand](https://msdn.microsoft.com/library/azure/ee758710) of in PowerShell ([New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx)).

> [!NOTE]
> Als u de DNS-instellingen voor een virtueel netwerk/virtuele machine die al is geïmplementeerd wijzigt, moet u opnieuw opstarten van elke betrokken VM om de wijzigingen van kracht te laten worden.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Resource Manager-implementatiemodel:

* [Een virtueel netwerk maken of bijwerken](https://msdn.microsoft.com/library/azure/mt163661.aspx)
* [Maken of bijwerken van een netwerkinterfacekaart](https://msdn.microsoft.com/library/azure/mt163668.aspx)
* [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx)
* [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx)

Klassieke implementatiemodel:

* [Azure Service Configuration Schema](https://msdn.microsoft.com/library/azure/ee758710)
* [Virtual Network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100)
* [Een virtueel netwerk configureren met behulp van een netwerk-configuratiebestand](virtual-networks-using-network-configuration-file.md) 

