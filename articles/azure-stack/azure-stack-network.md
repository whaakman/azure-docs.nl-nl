---
title: Van Integratieoverwegingen voor systemen met integratie van Azure Stack-netwerk | Microsoft Docs
description: Meer informatie over wat u kunt doen om te plannen voor de integratie van datacenter netwerk met meerdere knooppunten Azure Stack.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2018
ms.author: jeffgilb
ms.reviewer: wamota
ms.openlocfilehash: 9b1eb6878dcafba68c230255f3b3f43e005421ab
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43340185"
---
# <a name="network-connectivity"></a>Verbinding met het netwerk
Dit artikel bevat informatie over de infrastructuur Azure Stack-netwerk om te bepalen hoe u het beste Azure Stack integreren met uw bestaande netwerkomgeving. 

> [!NOTE]
> Om op te lossen externe DNS-namen van Azure Stack (bijvoorbeeld www.bing.com), moet u DNS-servers voor het doorsturen van DNS-aanvragen opgeven. Zie voor meer informatie over de vereisten voor Azure Stack DNS [integratie van Azure Stack datacenter - DNS-](azure-stack-integrate-dns.md).

## <a name="physical-network-design"></a>Het fysieke netwerk ontwerpen
De oplossing Azure Stack is een robuuste en maximaal beschikbare fysieke infrastructuur ter ondersteuning van de werking en -services vereist. Uplinks vanaf ToR naar rand switches zijn beperkt tot SFP + of SFP28 media en 1 GB, 10 GB of 25 GB snelheden worden bereikt. Neem contact op met uw hardwareleverancier oorspronkelijke leveranciers (OEM) voor beschikbaarheid. Het volgende diagram toont onze aanbevolen ontwerp:

![Aanbevolen ontwerp voor Azure Stack-netwerk](media/azure-stack-network/recommended-design.png)


## <a name="logical-networks"></a>Logische netwerken
Logische netwerken vertegenwoordigen een abstractie van de onderliggende fysieke netwerkinfrastructuur. Ze worden gebruikt voor het ordenen en vereenvoudigen voor hosts, virtuele machines en services. Als onderdeel van het maken van logische netwerken, netwerksites gemaakt voor het definiëren van de virtuele LAN (VLAN's), IP-subnetten en IP-subnet/VLAN-paren die gekoppeld aan het logische netwerk in elke fysieke locatie zijn.

De volgende tabel ziet u de logische netwerken en gekoppelde bereiken van de IPv4-subnet die u moet plannen:

| Logisch netwerk | Beschrijving | Grootte | 
| -------- | ------------- | ------------ | 
| Openbare VIP | Een totaal van 31-adressen van dit netwerk maakt gebruik van Azure Stack. Acht openbare IP-adressen worden gebruikt voor een kleine set Azure Stack-services en de rest worden gebruikt door virtuele machines van tenants. Als u van plan bent te gebruiken van App Service en de SQL-resourceproviders, worden 7 meer adressen gebruikt. De resterende 15 IP-adressen zijn gereserveerd voor toekomstige Azure-services. | / 26 (62 hosts) - /22 (1022 hosts)<br><br>Aanbevolen = /24 (254 hosts) | 
| Switch-infrastructuur | Point-to-Point IP-adressen voor routeringsdoeleinden, toegewezen overschakelen beheerinterfaces en loopback-adressen die zijn toegewezen aan de switch. | /26 | 
| Infrastructuur | Gebruikt voor interne onderdelen van Azure Stack om te communiceren. | /24 |
| Privé | Gebruikt voor het opslagnetwerk en privé-VIP. | /24 | 
| BMC | Gebruikt om te communiceren met de BMC's op de fysieke hosts. | /26 | 
| | | |

## <a name="network-infrastructure"></a>Netwerkinfrastructuur
De netwerkinfrastructuur voor Azure Stack bestaat uit meerdere logische netwerken die zijn geconfigureerd op de switches. Het volgende diagram toont deze logische netwerken en hoe ze integreren met de top-of-rack (TOR), BMC (baseboard management controller), en de rand (klant) netwerkswitches.

![Logisch diagram en switch netwerkverbindingen](media/azure-stack-network/NetworkDiagram.png)

### <a name="bmc-network"></a>BMC-netwerk
Dit netwerk is toegewezen aan alle de baseboard management controllers (ook wel bekend als serviceprocessors, bijvoorbeeld iDRAC, iLO, iBMC, enzovoort) verbinding te maken met het beheernetwerk. Indien aanwezig, wordt de Hardware Lifecycle Host (HLH) bevindt zich op dit netwerk en OEM-specifieke software voor hardwareonderhoud of bewaking kan bieden. 

De HLH ook als host fungeert voor de implementatie van virtuele machine (DVM). De DVM wordt gebruikt tijdens de implementatie van Azure Stack en wordt verwijderd wanneer de implementatie is voltooid. De DVM vereist toegang tot internet in verbonden implementatiescenario's testen en valideren en toegang tot meerdere onderdelen. Deze onderdelen kunnen worden binnen en buiten uw bedrijfsnetwerk. bijvoorbeeld, NTP, DNS en Azure. Zie voor meer informatie over vereisten voor connectiviteit, de [NAT-sectie in Azure Stack-firewall-integratie](azure-stack-firewall.md#network-address-translation). 

### <a name="private-network"></a>Privénetwerk
Deze /24 (254 host IP-adressen) netwerk zijn exclusief voor de Azure Stack-regio (wordt niet verder uitgebreid dan de apparaten van de switch rand van de Azure Stack-regio) en is onderverdeeld in twee subnetten:

- **Opslagnetwerk**. Een /25 (126 host IP-adressen) netwerk dat wordt gebruikt ter ondersteuning van het gebruik van opslagruimten Direct en Server Message Block (SMB) opslagverkeer en live migratie van virtuele machines. 
- **Interne virtuele IP-netwerk**. A/25 netwerk speciaal voor intern gebruik bestemde VIP's voor de software load balancer.

### <a name="azure-stack-infrastructure-network"></a>Netwerk van Azure Stack-infrastructuur
Dit/24 netwerk is toegewezen aan interne Azure Stack-onderdelen, zodat ze kunnen communiceren en uitwisselen van gegevens worden gedeeld. Dit subnet routeerbare IP-adressen vereist, maar is privé gehouden aan de oplossing met behulp van toegangsbeheerlijsten (ACL's). Het is niet waarschijnlijk worden gerouteerd buiten de rand-switches, met uitzondering van een klein bereik gelijk in grootte aan een/27 netwerk die door enkele van deze services wordt gebruikt wanneer ze nodig toegang tot externe bronnen en/of het internet hebben. 

### <a name="public-infrastructure-network"></a>Infrastructuur voor openbare-netwerk
Dit/27 netwerk is het kleine bereik van de Azure Stack-infrastructuur subnet eerder vermeld, hoeven er geen openbare IP-adressen, maar er is internettoegang via een NAT- of transparante Proxy vereist. Dit netwerk worden toegewezen voor de EMS Recovery-Console (ERCS), de VM ERCS is internettoegang vereist tijdens de registratie naar Azure en tijdens de back-ups van infrastructuur. De VM ERCS moet routeerbaar zijn met uw beheernetwerk voor het oplossen van problemen.

### <a name="public-vip-network"></a>Openbare VIP-netwerk
Het openbare VIP-netwerk is toegewezen aan de netwerkcontroller in Azure Stack. Het is niet een logisch netwerk op de switch. De SLB maakt gebruik van de groep met adressen en wijst/32 netwerken voor tenantwerkbelastingen. Op de switch-routeringstabel, worden deze 32 IP-adressen aangekondigd als een beschikbare route via BGP. Dit netwerk bevat de extern toegankelijke of openbare IP-adressen. De Azure Stack-infrastructuur reserveert de eerste 31 adressen van deze openbare VIP-netwerk terwijl de rest wordt gebruikt door tenant-VM's. De netwerkgrootte van het op dit subnet kan variëren van een minimum van /26 (64-hosts) tot een maximum van /22 (1022 hosts), is het raadzaam dat u van plan bent voor een/24 netwerk.

### <a name="switch-infrastructure-network"></a>Switch-infrastructuurnetwerk
Dit/26 netwerk is het subnet waarin de routeerbaar point-to-point IP/30 (2 host IP-adressen)-subnetten en de loopbacks, die zijn toegewezen/32 subnetten voor in-band overschakelen beheer- en BGP-router-ID. Dit bereik van IP-adressen moeten routeerbaar extern van de oplossing Azure Stack uw datacenter, zij je misschien persoonlijke of openbare IP-adressen.

### <a name="switch-management-network"></a>Switch-beheernetwerk
Deze /29 (6 host IP-adressen) netwerk is toegewezen aan de beheerpoorten van de schakelopties voor de verbinding te maken. Hiermee kunt de out-of-band-toegang voor implementatie, beheer en oplossen van problemen. Deze wordt berekend op basis van het netwerk van de switch-infrastructuur zoals hierboven vermeld.

## <a name="publish-azure-stack-services"></a>Azure Stack-services publiceren
U moet Azure Stack-services beschikbaar te maken voor gebruikers van buiten Azure Stack. Azure Stack, stelt u verschillende eindpunten voor de functies van de infrastructuur. Deze eindpunten toegewezen VIP's uit de openbare IP-adresgroep. Een DNS-vermelding wordt voor elk eindpunt in de externe DNS-zone, dat is opgegeven tijdens de implementatie gemaakt. De gebruikersportal is bijvoorbeeld de DNS-host-vermelding van de portal toegewezen.  *&lt;regio >.&lt; FQDN-naam >*.

### <a name="ports-and-urls"></a>Poorten en URL 's
Om te maken van Azure Stack-services (zoals de portals, Azure Resource Manager, DNS, enzovoort) beschikbaar voor externe netwerken, moet u inkomend verkeer naar deze eindpunten toestaan voor specifieke URL's, poorten en protocollen.
 
In een implementatie waarbij een transparante proxy uplinks met een traditionele proxyserver, moet u toestaan specifieke poorten en URL's voor zowel [inkomende](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound) en [uitgaande](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound) communicatie. Deze bevatten poorten en URL's voor identiteit, de marketplace-, patch- en update-, registratie-en gebruiksgegevens.

## <a name="next-steps"></a>Volgende stappen
[Rand-connectiviteit](azure-stack-border-connectivity.md)