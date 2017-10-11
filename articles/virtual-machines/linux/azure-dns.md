---
title: DNS-opties voor naamomzetting voor Linux virtuele machines in Azure
description: Naam resolutie scenario's voor virtuele Linux-machines in Azure IaaS, met inbegrip van opgegeven DNS-services, hybride externe DNS- en Bring Your Own DNS-server.
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: timlt
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
ms.openlocfilehash: cc06ee9305b4d3034154a0825c1aea53fe446f80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>Opties voor Linux virtuele machines in Azure DNS-naamomzetting
Azure biedt DNS-naamomzetting standaard voor alle virtuele machines die zich in één virtueel netwerk. U kunt uw eigen DNS-naam resolutie oplossing implementeren door uw eigen DNS-services op uw virtuele machines te configureren die Azure als host fungeert. De volgende scenario's kunt u kiezen die geschikt is voor uw situatie.

* [Naamomzetting door Azure worden geboden](#azure-provided-name-resolution)
* [Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)

Het type van naamomzetting die u gebruikt, is afhankelijk van hoe uw virtuele machines en rolinstanties moeten met elkaar communiceren.

De volgende tabel ziet u scenario's en bijbehorende name resolution-oplossingen:

| **Scenario** | **Oplossing** | **Achtervoegsel** |
| --- | --- | --- |
| Naamomzetting tussen de rolinstanties of virtuele machines in hetzelfde virtuele netwerk |[Naamomzetting door Azure worden geboden](#azure-provided-name-resolution) |hostnaam of FQDN-naam (fully qualified domain name) |
| Naamomzetting tussen de rolinstanties of virtuele machines in verschillende virtuele netwerken |Door de klant beheerde DNS-servers doorsturen van query's tussen virtuele netwerken voor de omzetting van Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| De resolutie van lokale computers en -namen van rolexemplaren te controleren of de virtuele machines in Azure |Door de klant beheerde DNS-servers (bijvoorbeeld op de lokale domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS gesynchroniseerd met behulp van zoneoverdrachten). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| De resolutie van Azure hostnamen van lokale computers |Doorsturen van aanvragen naar een door de klant beheerde DNS-proxyserver in de bijbehorende virtuele netwerk. De proxyserver stuurt query's naar Azure voor naamomzetting. Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN |
| Omgekeerde DNS voor interne IP-adressen |[Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) |N.v.t. |

## <a name="name-resolution-that-azure-provides"></a>Naamomzetting door Azure worden geboden
Samen met de resolutie van openbare DNS-namen biedt Azure interne naamomzetting voor virtuele machines en rolexemplaren die zich in hetzelfde virtuele netwerk. In virtuele netwerken die zijn gebaseerd op Azure Resource Manager, is het DNS-achtervoegsel consistent op het virtuele netwerk. de FQDN-naam is niet nodig. DNS-namen kunnen worden toegewezen aan virtuele machines en netwerkinterfacekaarten (NIC's). Hoewel de naamomzetting die Azure biedt geen configuratie vereist, is niet de juiste keuze is voor alle implementatiescenario's, zoals te zien op de voorgaande tabel.

### <a name="features-and-considerations"></a>Functies en overwegingen
**Functies:**

* Er is geen configuratie is vereist voor naamomzetting die Azure biedt gebruiken.
* De service voor naamomzetting die Azure biedt is maximaal beschikbaar. U hoeft niet te maken en beheren van clusters van uw eigen DNS-servers.
* De service voor naamomzetting die Azure biedt kan worden gebruikt samen met uw eigen DNS-servers om op te lossen, zowel on-premises en Azure hostnamen.
* Naamomzetting is tussen virtuele machines in virtuele netwerken zonder dat nodig is voor de FQDN-naam opgegeven.
* U kunt hostnamen die het beste uw implementaties te beschrijven in plaats van het werken met de automatisch gegenereerde naam.

**Overwegingen:**

* De DNS-achtervoegsel dat Azure maakt, kan niet worden gewijzigd.
* U kunt uw eigen records kan niet handmatig registreren.
* WINS- en NetBIOS worden niet ondersteund.
* Hostnamen moet DNS-compatibele.
    Namen moeten alleen 0-9, a-z, gebruiken en '-', en ze mag niet beginnen of eindigen met een '-'. Zie RFC 3696 sectie 2.
* DNS-query-verkeer is voor elke virtuele machine beperkt. Beperking mag niet van invloed op de meeste toepassingen.  Indien aanvraag beperking is waargenomen, zorg ervoor dat caching aan clientzijde is ingeschakeld.  Zie voor meer informatie [ophalen van de meest uit naamomzetting die Azure biedt](#getting-the-most-from-name-resolution-that-azure-provides).

### <a name="getting-the-most-from-name-resolution-that-azure-provides"></a>Als u optimaal van naamomzetting die Azure biedt
**Caching aan clientzijde:**

Sommige DNS-query's worden niet verzonden via het netwerk. Caching aan clientzijde helpt de latentie te verminderen en de herstelmogelijkheden bij netwerk inconsistenties verbeteren door terugkerende DNS-query's uit de lokale cache op te lossen. DNS-records bevatten een Time-To-Live (TTL), waardoor de cache voor het opslaan van de record voor zo lang mogelijk zonder enige impact op record versheid. Als gevolg hiervan is het geschikt is voor de meeste situaties caching aan clientzijde.

Sommige Linux-distributies omvatten geen cache standaard. Het is raadzaam dat u een cache aan elke virtuele Linux-machine, toevoegen nadat u hebt gecontroleerd dat nog niet een lokale cache is.

Enkele andere DNS-cache van pakketten, zoals dnsmasq, zijn beschikbaar. Hier volgen de stappen voor het installeren van dnsmasq op de meest voorkomende distributies:

**Ubuntu (maakt gebruik van resolvconf)**
  * Installeer het pakket dnsmasq ('sudo apt get-installatie dnsmasq').

**SUSE (maakt gebruik van netconf)**:
1. Installeer het pakket dnsmasq ('sudo zypper installeren dnsmasq').
2. De service dnsmasq ('systemctl inschakelen dnsmasq.service') inschakelen.
3. Start de service dnsmasq ('systemctl start dnsmasq.service').
4. Bewerken '/ etc/sysconfig/netwerk/config' en wijzigt u NETCONFIG_DNS_FORWARDER = "' naar 'dnsmasq'.
5. Resolv.conf ('netconfig-update') om in te stellen van de cache bijwerken als de lokale DNS-resolver.

**CentOS door Rogue Wave-Software (voorheen OpenLogic; maakt gebruik van NetworkManager)**
1. Installeer het pakket dnsmasq ('sudo yum installeren dnsmasq').
2. De service dnsmasq ('systemctl inschakelen dnsmasq.service') inschakelen.
3. Start de service dnsmasq ('systemctl start dnsmasq.service').
4. Voeg toe 'toevoegen domain name servers 127.0.0.1;' naar '/etc/dhclient-eth0.conf'.
5. opnieuw opstarten van de Netwerkservice ('service netwerk opnieuw starten') om in te stellen van de cache als de lokale DNS-resolver

> [!NOTE]
> : Het pakket 'dnsmasq' is slechts een van de vele DNS-caches die beschikbaar voor Linux zijn. Voordat u deze kunt gebruiken, Controleer de geschiktheid voor uw behoeften en die geen andere cache is geïnstalleerd.
>
>

**Nieuwe pogingen voor clientzijde**

DNS is voornamelijk een UDP-protocol. Omdat het UDP-protocol niet levering van berichten garanderen, verwerkt de DNS-protocol zelf Pogingslogica. Elke DNS-client (besturingssysteem) kan zich voordoen verschillende Pogingslogica, afhankelijk van de maker voorkeur:

* Windows-besturingssystemen opnieuw proberen na een tweede en nog een keer na de andere twee, vier en andere vier seconden.
* De standaard Linux setup pogingen na vijf seconden.  U moet dit om opnieuw te proberen vijf keer met een interval van één seconde wijzigen.  

De huidige instellingen op een Linux-virtuele machine, 'cat /etc/resolv.conf' en bekijk de regel 'options' bijvoorbeeld controleren:

    options timeout:1 attempts:5

Het bestand resolv.conf automatisch wordt gegenereerd en mag niet worden bewerkt. De specifieke stappen die de regel 'options' toevoegen verschillen per distributiepunt:

**Ubuntu** (maakt gebruik van resolvconf)
1. De opties regel toe te voegen ' / etc/resolveconf/resolv.conf.d/head'.
2. Resolvconf -u uitvoeren om bij te werken.

**SUSE** (maakt gebruik van netconf)
1. 'Timeout:1 pogingen: 5' toevoegen aan de NETCONFIG_DNS_RESOLVER_OPTIONS = "" parameter in '/ etc/sysconfig/netwerk/config'.
2. Netconfig-update uitvoeren om bij te werken.

**CentOS door Rogue Wave-Software (voorheen OpenLogic)** (maakt gebruik van NetworkManager)
1. 'Echo 'opties timeout:1 pogingen: 5' ' toevoegen aan ' / etc/NetworkManager/dispatcher.d/11-dhclient'.
2. Voer 'service netwerk opnieuw is opgestart, bij te werken.

## <a name="name-resolution-using-your-own-dns-server"></a>Naamomzetting met uw eigen DNS-server
De behoeften van uw name resolution mogelijk verder dan de functies die Azure biedt. U kunt bijvoorbeeld vereisen dat DNS-omzetting tussen virtuele netwerken. Ten aanzien van dit scenario kunt u uw eigen DNS-servers.  

DNS-servers binnen een virtueel netwerk kunnen de DNS-query's doorsturen naar recursieve resolvers van Azure omzetten van hostnamen die zich in hetzelfde virtuele netwerk. Bijvoorbeeld, een DNS-server die wordt uitgevoerd in Azure reageren op DNS-query's voor de eigen DNS-zone-bestanden en andere query's sturen naar Azure. Deze functionaliteit kan virtuele machines om te zien van zowel de vermeldingen in de zonebestanden en hostnamen die Azure (via de doorstuurserver biedt). Toegang tot de recursieve resolvers van Azure wordt opgegeven via het virtuele IP-adres 168.63.129.16.

DNS-doorsturen ook kunt DNS-omzetting tussen virtuele netwerken en kan uw lokale machines omzetten van hostnamen die Azure biedt. Los de hostnaam van een virtuele machine door de virtuele machine van de DNS-server moet zich bevinden in hetzelfde virtuele netwerk en worden geconfigureerd voor doorsturen hostnaam query's naar Azure. Omdat het DNS-achtervoegsel in elk virtueel netwerk verschilt, kunt u voorwaardelijk doorsturen regels DNS-query's verzenden naar het juiste virtuele netwerk voor naamomzetting. De volgende afbeelding ziet u twee virtuele netwerken en een on-premises netwerk tijdens het doorzoeken van DNS-omzetting tussen virtuele netwerken met deze methode:

![DNS-omzetting tussen virtuele netwerken](./media/azure-dns/inter-vnet-dns.png)

Wanneer u naamomzetting die Azure biedt, wordt de interne DNS-achtervoegsel opgegeven aan elke virtuele machine met behulp van DHCP. Wanneer u uw eigen name resolution-oplossing gebruikt, wordt dit achtervoegsel aan virtuele machines niet opgegeven, omdat het achtervoegsel andere DNS-architecturen verstoort. Om te verwijzen naar machines met FQDN-naam of het achtervoegsel configureren op uw virtuele machines, kunt u PowerShell of de API om te bepalen van het achtervoegsel:

* Voor virtuele netwerken die worden beheerd met Azure Resource Manager, het achtervoegsel is beschikbaar via de [netwerkinterfacekaart](https://msdn.microsoft.com/library/azure/mt163668.aspx) resource. U kunt ook uitvoeren de `azure network public-ip show <resource group> <pip name>` opdracht voor het weergeven van de details van uw openbare IP-adres, waaronder de FQDN-naam van de NIC.

Als het doorsturen van query's in Azure wordt niet voldaan aan uw behoeften, moet u om uw eigen DNS-oplossing te bieden.  Uw DNS-oplossing moet:

* Geef juiste hostnaamomzetting, bijvoorbeeld [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md). Als u DDNS gebruikt, moet u mogelijk uitschakelen opruiming van DNS-record. DHCP-leases van Azure erg lang zijn en opruiming kan DNS-records verwijderen voortijdig.
* Juiste naamomzetting om toe te staan de omzetting van namen van de externe domein opgeven.
* Toegankelijk (TCP en UDP op poort 53) van de clients die het fungeert en kunnen toegang tot Internet.
* Beveiligd is tegen toegang vanaf het Internet te beperken van bedreigingen die door externe agents.

> [!NOTE]
> Voor de beste prestaties wanneer u virtuele machines in Azure DNS-servers, IPv6 uitschakelen en wijs een [openbare IP op exemplaarniveau](../../virtual-network/virtual-networks-instance-level-public-ip.md) aan elke virtuele machine van de DNS-server.  
>
>
