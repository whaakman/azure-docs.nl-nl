---
title: DNS-opties voor naamomzetting voor virtuele Linux-machines in Azure
description: Naam resolutie scenario's voor virtuele Linux-machines in Azure IaaS, met inbegrip van opgegeven DNS-services, hybride externe DNS- en breng uw eigen DNS-server.
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: jeconnoc
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
ms.openlocfilehash: ae8315b2a484cddc500b5c2dd02a019cb4f46d8e
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55819141"
---
# <a name="dns-name-resolution-options-for-linux-virtual-machines-in-azure"></a>Opties voor DNS-naamomzetting voor virtuele Linux-machines in Azure
Azure biedt DNS-naamomzetting standaard voor alle virtuele machines die zich in één virtueel netwerk. U kunt uw eigen DNS-naam resolutie-oplossing implementeren met het configureren van uw eigen DNS-services op uw virtuele machines waarop Azure wordt gehost. De volgende scenario's kunt u kiezen die geschikt is voor uw situatie.

* [Naamomzetting die Azure biedt](#name-resolution-that-azure-provides)
* [Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server)

Het type van de naamomzetting die u gebruikt, is afhankelijk van hoe uw virtuele machines en rolinstanties nodig om te communiceren met elkaar.

De volgende tabel ziet u scenario's en bijbehorende naam resolutie-oplossingen:

| **Scenario** | **Oplossing** | **Suffix** |
| --- | --- | --- |
| Naamomzetting tussen rolexemplaren of virtuele machines in hetzelfde virtuele netwerk |Naamomzetting die Azure biedt |hostnaam of het volledig gekwalificeerde domeinnaam (FQDN) |
| Naamomzetting tussen rolexemplaren of virtuele machines in verschillende virtuele netwerken |Door de klant beheerde DNS-servers die doorsturen van query's tussen virtuele netwerken voor het omzetten van door Azure (DNS-proxy). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN-naam |
| De resolutie van on-premises computers en de servicenamen van de van rolinstanties of virtuele machines in Azure |Door de klant beheerde DNS-servers (bijvoorbeeld on-premises domeincontroller, lokale alleen-lezen domeincontroller of een secundaire DNS met behulp van zoneoverdrachten gesynchroniseerd). Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN-naam |
| De resolutie van Azure hostnamen van on-premises computers |Doorsturen van aanvragen naar een door de klant beheerde DNS-proxyserver in de bijbehorende virtuele netwerk. De proxyserver stuurt query's naar Azure voor het omzetten van. Zie [naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server). |Alleen FQDN-naam |
| Reverse-DNS voor interne IP-adressen |[Naamomzetting met uw eigen DNS-server](#name-resolution-using-your-own-dns-server) |N.v.t. |

## <a name="name-resolution-that-azure-provides"></a>Naamomzetting die Azure biedt
Samen met een resolutie van openbare DNS-namen biedt Azure interne naamomzetting voor virtuele machines en rolinstanties die zich in hetzelfde virtuele netwerk. In virtuele netwerken die zijn gebaseerd op Azure Resource Manager, is de DNS-achtervoegsel consistent op het virtuele netwerk. de FQDN-naam is niet nodig. DNS-namen kunnen worden toegewezen aan zowel netwerkinterfacekaarten (NIC's) en virtuele machines. Hoewel de naamomzetting die Azure biedt geen is geen configuratie vereist, is het niet de juiste keuze voor alle implementatiescenario's, zoals te zien is op de voorgaande tabel.

### <a name="features-and-considerations"></a>Functies en overwegingen
**Functies:**

* Er is geen configuratie is vereist voor naamomzetting die Azure biedt.
* De name resolution-service die Azure biedt is maximaal beschikbaar. U hoeft te maken en beheren van clusters van uw eigen DNS-servers.
* De name resolution-service waarmee Azure kan worden gebruikt samen met uw eigen DNS-servers op te lossen, zowel on-premises en Azure hostnamen.
* Naamomzetting is tussen virtuele machines in virtuele netwerken zonder dat nodig is voor de FQDN-naam opgegeven.
* U kunt hostnamen die het beste uw implementaties te beschrijven in plaats van het werken met automatisch gegenereerde naam.

**Overwegingen met betrekking tot:**

* De DNS-achtervoegsel dat Azure maakt worden niet gewijzigd.
* U kunt uw eigen records kan niet handmatig registreren.
* WINS- en NetBIOS-worden niet ondersteund.
* Hostnamen moeten de DNS-compatibel.
    Namen van rapportitems zijn alleen 0-9, a-z, en '-', en ze mag niet beginnen of eindigen met een '-'. Zie RFC 3696 sectie 2.
* DNS-query-verkeer is beperkt voor elke virtuele machine. Beperking mag niet van invloed op de meeste toepassingen.  Als aanvraagbeperking wordt waargenomen, moet u caching aan clientzijde is ingeschakeld.  Zie voor meer informatie, [optimaal gebruik van naamomzetting die Azure biedt](#getting-the-most-from-name-resolution-that-azure-provides).

### <a name="getting-the-most-from-name-resolution-that-azure-provides"></a>Als u optimaal van de naamomzetting die Azure biedt
**Caching aan clientzijde:**

Een DNS-query's worden niet verzonden via het netwerk. Caching aan clientzijde, kunt wachttijden verkorten en herstelmogelijkheden bij netwerk-inconsistenties verbeteren door de verwerking van terugkerende DNS-query's uit een lokale cache. DNS-records bevatten een Time-To-Live (TTL), waarmee de cache voor het opslaan van de record voor zo lang mogelijk zonder dat dit record webdocumenten. Als gevolg hiervan is het geschikt is voor de meeste situaties caching aan clientzijde.

Sommige Linux-distributies omvatten niet opslaan in cache standaard. Het is raadzaam dat u een cache aan elke virtuele Linux-machine, toevoegen nadat u hebt gecontroleerd dat er nog niet een lokale cache is.

Er zijn diverse verschillende DNS-caching van pakketten, zoals dnsmasq, beschikbaar. Hier volgen de stappen voor het installeren van dnsmasq op de meest voorkomende distributies:

**Ubuntu (maakt gebruik van resolvconf)**
  * Installeer het pakket dnsmasq ('sudo apt-get install dnsmasq').

**SUSE (maakt gebruik van netconf)**:
1. Installeer het pakket dnsmasq ('sudo zypper installeren dnsmasq').
2. De service dnsmasq ("systemctl inschakelen dnsmasq.service') inschakelen.
3. Start de service dnsmasq ("systemctl start dnsmasq.service').
4. Bewerken '/ etc/sysconfig/netwerk/config', en wijzig NETCONFIG_DNS_FORWARDER = "" naar 'dnsmasq'.
5. Resolv.conf ('netconfig-update') om in te stellen de cache als de lokale DNS-resolver bijwerken.

**CentOS door Rogue Wave Software (voorheen OpenLogic; maakt gebruik van NetworkManager)**
1. Installeer het pakket dnsmasq ('sudo yum install dnsmasq').
2. De service dnsmasq ("systemctl inschakelen dnsmasq.service') inschakelen.
3. Start de service dnsmasq ("systemctl start dnsmasq.service').
4. Voeg toe 'vooraf domain-name-servers 127.0.0.1;' naar '/etc/dhclient-eth0.conf'.
5. Opnieuw opstarten van de Netwerkservice ('service netwerk restart') om in te stellen de cache als de lokale DNS-resolver

> [!NOTE]
> : Het pakket 'dnsmasq' is slechts één van de vele DNS-cache die beschikbaar voor Linux zijn. Voordat u deze gebruiken, Controleer de geschiktheid voor uw behoeften en die geen andere cache is geïnstalleerd.
>
>

**Client-side nieuwe pogingen**

DNS is voornamelijk een UDP-protocol. Omdat het UDP-protocol biedt geen garantie voor bezorging van berichten, verwerkt de DNS-protocol zelf logica voor opnieuw proberen. Elke DNS-client (besturingssysteem) kan zich voordoen logica voor verschillende nieuwe pogingen, afhankelijk van de maker van voorkeur:

* Windows-besturingssystemen opnieuw proberen na een tweede en nog een keer achter elkaar twee, vier en andere vier seconden.
* De standaard Linux setup nieuwe pogingen na vijf seconden.  U moet dit om opnieuw te proberen vijf keer met 1 seconde-intervallen wijzigen.  

De huidige instellingen op een Linux virtuele machine, 'cat /etc/resolv.conf', en bekijk de regel 'opties' bijvoorbeeld controleren:

    options timeout:1 attempts:5

Het bestand resolv.conf wordt automatisch gegenereerd en mag niet worden bewerkt. De specifieke stappen, voeg de regel '-opties verschillen per distributie:

**Ubuntu** (maakt gebruik van resolvconf)
1. Toevoegen van de opties voor regel ' / etc/resolveconf/resolv.conf.d/head'.
2. 'Resolvconf -u' uitvoeren om bij te werken.

**SUSE** (maakt gebruik van netconf)
1. 'Timeout:1 pogingen: 5' toevoegen aan de NETCONFIG_DNS_RESOLVER_OPTIONS = "" parameter in '/ etc/sysconfig/netwerk/config'.
2. Voer 'netconfig update' uit om bij te werken.

**CentOS door Rogue Wave Software (voorheen OpenLogic)** (maakt gebruik van NetworkManager)
1. Add 'RES_OPTIONS="timeout:1 attempts:5"' to '/etc/sysconfig/network'.
2. Voer 'service netwerk opnieuw opstarten' om bij te werken.

## <a name="name-resolution-using-your-own-dns-server"></a>Naamomzetting met uw eigen DNS-server
De behoeften van uw naam resolutie mogelijk verder dan de functies die Azure biedt. U kunt bijvoorbeeld vereisen dat DNS-omzetting tussen virtuele netwerken. Voor dit scenario kunt u uw eigen DNS-servers.  

DNS-servers binnen een virtueel netwerk kunnen u DNS-query's doorsturen naar recursieve resolvers van Azure voor het omzetten van hostnamen die zich in hetzelfde virtuele netwerk. Bijvoorbeeld kunt een DNS-server die wordt uitgevoerd in Azure reageren op DNS-query's voor een eigen DNS-zonebestanden en alle andere query's sturen naar Azure. Deze functionaliteit kan virtuele machines om te zien van zowel de invoer in uw zone bestands- en hostnamen die Azure (via de doorstuurserver biedt). Toegang tot de recursieve resolvers van Azure wordt geboden via het virtuele IP-adres 168.63.129.16.

Doorsturen van de DNS DNS-omzetting tussen virtuele netwerken kan ook en kunt uw on-premises computers op te lossen hostnamen die Azure biedt. Om op te lossen de hostnaam van een virtuele machine, de DNS-server-machine moet zich in hetzelfde virtuele netwerk bevinden en worden geconfigureerd voor doorsturen hostnaam query's naar Azure. Omdat de DNS-achtervoegsel in elk virtueel netwerk anders is, kunt u voorwaardelijk doorsturen regels voor het verzenden van DNS-query's naar het juiste virtuele netwerk voor het omzetten van. De volgende afbeelding ziet u twee virtuele netwerken en een on-premises netwerk DNS-omzetting tussen virtuele netwerken met behulp van deze methode doen:

![DNS-omzetting tussen virtuele netwerken](./media/azure-dns/inter-vnet-dns.png)

Als u naamomzetting die Azure biedt gebruikt, wordt de interne DNS-achtervoegsel opgegeven aan elke virtuele machine met behulp van DHCP. Wanneer u uw eigen naam resolutie-oplossing gebruikt, wordt dit achtervoegsel aan virtuele machines niet opgegeven, omdat het achtervoegsel met andere DNS-architecturen functioneert. Om te verwijzen naar computers met FQDN-naam of het achtervoegsel op uw virtuele machines configureren, kunt u PowerShell of de API gebruiken om te bepalen het achtervoegsel:

* Voor virtuele netwerken die worden beheerd door Azure Resource Manager, het achtervoegsel is beschikbaar via de [netwerkinterfacekaart](https://msdn.microsoft.com/library/azure/mt163668.aspx) resource. U kunt ook uitvoeren de `azure network public-ip show <resource group> <pip name>` opdracht voor het weergeven van de details van uw openbare IP-adres, waaronder de FQDN-naam van de NIC.

Als uw niet op query's worden doorgestuurd naar Azure behoeften, moet u om uw eigen DNS-oplossing te bieden.  Uw DNS-oplossing moet:

* Bieden van de juiste hostnaamomzetting, bijvoorbeeld [DDNS](../../virtual-network/virtual-networks-name-resolution-ddns.md). Als u DDNS gebruikt, moet u mogelijk om uit te schakelen opruiming van DNS-record. DHCP-leases van Azure zijn zeer lang en opruiming kan verwijderen DNS-records voortijdig gestopt.
* Juiste recursieve oplossing om toe te staan de resolutie van externe domeinnamen te bieden.
* Toegankelijk (TCP en UDP op poort 53) van de clients die het resultaat wordt gebruikt en kunnen worden gebruikt voor toegang tot het Internet.
* Worden beveiligd tegen toegang vanaf het Internet te beperken van bedreigingen die door externe agents.

> [!NOTE]
> Voor de beste prestaties wanneer u virtuele machines in Azure DNS-servers, IPv6 uitschakelen en wijzen een [Instance-Level Public IP](../../virtual-network/virtual-networks-instance-level-public-ip.md) aan elke virtuele machine van de DNS-server.  
>
>
