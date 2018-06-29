---
title: Azure productienetwerk
description: In dit artikel biedt een algemene beschrijving van de Microsoft Azure-productienetwerk.
services: security
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: 5c0bfae35464e73278a1efd9c04a03123bb9018a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102287"
---
# <a name="azure-production-network"></a>Azure productienetwerk
De gebruikers van de Azure-productienetwerk omvatten externe klanten toegang krijgen tot hun eigen Microsoft Azure-toepassingen, evenals de interne medewerkers van de Microsoft Azure-ondersteuning die het productienetwerk te beheren. De beveiligingsmethoden voor toegang en beveiliging mechanismen voor het tot stand brengen van verbindingen met de Azure-productienetwerk worden besproken in dit artikel.

## <a name="internet-routing-and-fault-tolerance"></a>Routering van Internet en fouttolerantie
Een globaal redundante interne en externe Microsoft Azure Domain Name Service (de WADNS) infrastructuur samen met meerdere primaire en secundaire Service DNS (Domain Name) server-clusters bieden voor fouttolerantie tijdens extra Microsoft Azure-netwerk beveiligingsmaatregelen zoals NetScaler gedistribueerde Denial of Service (DDoS) aanvallen te voorkomen en beveiligt u de integriteit van Microsoft Azure DNS-services worden gebruikt.

De WADNS-servers bevinden zich op meerdere datacenter faciliteiten. De uitvoering van de WADNS opgenomen met een hiërarchie van secundaire/primaire DNS-servers om op te lossen openbaar Azure klant domeinnamen. De domeinnamen kan doorgaans worden omgezet in een adres CloudApp.net, die geschikt is voor het virtuele IP-adres (VIP)-adres voor de service van de klant. Uniek zijn in Azure, het VIP van de interne toegewezen IP (DIP)-adres van de vertaling tenant wordt uitgevoerd door de Microsoft load balancers die verantwoordelijk is voor die VIP.

Azure wordt gehost in Azure-geografisch verspreide datacenters binnen de Verenigde Staten en is gebaseerd op de nieuwste routering platforms robuust en schaalbaar architecturen te implementeren. Enkele van de belangrijke functies zijn:

- Verkeer engineering koppeling efficiënt gebruik en correcte vermindering van de service te bieden als er een storing op basis van Multiprotocol Label overschakelen (MPLS)
- Netwerken worden geïmplementeerd met 'nodig plus één' (N + 1) redundantie architecturen of hoger.
- Extern, worden datacenters door specifieke netwerken met hoge bandbreedte circuits die eigenschappen toch toe verbinding te met meer dan 1200 internetproviders globaal op meerdere peering punten bieden meer dan 2.000 GB per seconde (Gbps maken) van behandeld capaciteit van de rand.

Als Microsoft eigenaar is van een eigen netwerk circuits tussen datacenters, deze kenmerken helpen bij de Azure-aanbieding bereiken 99.9 + % netwerkbeschikbaarheid zonder de noodzaak van traditionele internetproviders van derden.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Verbinding maken met productienetwerk en bijbehorende Firewalls
Het netwerk van Azure Internet verkeer stroom beleid stuurt het verkeer naar het Azure-productienetwerk zich in de dichtstbijzijnde regionale datacenter binnen de Verenigde Staten. Omdat de productie-Azure-datacenters consistent netwerkarchitectuur en hardware onderhoudt, de onderstaande netwerkverkeer beschrijving van toepassing is consistent op alle datacenters.

Zodra het internetverkeer voor Azure wordt doorgestuurd naar het dichtstbijzijnde datacentrum, wordt geen verbinding gemaakt met de toegang tot routers. Deze toegang routers fungeren voor het isoleren van verkeer tussen Azure knooppunten en virtuele machines klant geïnstantieerd. Infrastructuur van netwerkapparaten op de locaties voor toegang en rand zijn de grens punten waar inkomend en/of uitgaande filters zijn toegepast. Deze routers zijn geconfigureerd door middel van een gelaagde ACL ongewenste netwerkverkeer filteren en toepassen van frequentielimieten voor verkeer, indien nodig. Verkeer dat is toegestaan door de ACL wordt doorgestuurd naar de load balancers. Distributie routers zijn ontworpen om alleen Microsoft-goedgekeurde IP-adressen toestaan, anti-adresvervalsing (spoofing) en tot stand gebrachte TCP-verbindingen met ACL's bieden.

Externe Load Balancing apparaten bevinden zich achter de routers toegang Network Address Translation (NAT) uitvoert van Internet routeerbare IP-adressen naar Azure interne IP-adressen. Ze ook pakketten te routeren naar een geldige, productie interne IP-adressen en poorten en fungeren als een beveiligingsmechanisme beperken interne productienetwerk adresruimte blootstellen.

Standaard wordt Microsoft Hypertext Transfer Protocol Secure (HTTPS) afgedwongen voor al het verkeer wordt verzonden naar de klant web browsers, waaronder inloggen en alle verkeer daarna. Het gebruik van TLS 1.2 kunt een beveiligde tunnel voor verkeer kunnen stromen. ACL's op routers toegang en de belangrijkste Zorg ervoor dat de bron van het verkeer komt overeen met wat er wordt verwacht.

Een belangrijk verschil in deze architectuur vergeleken met traditionele beveiligingsarchitectuur is dat er zijn geen specifieke hardwarefirewalls, gespecialiseerde inbraakdetectie detectie/preventie van apparaten of andere beveiligingsapparaten normaal voordat verwacht verbindingen zijn gemaakt met de Azure-productieomgeving. Deze firewall hardwareapparaten verwacht klanten doorgaans in het netwerk van Azure; Er zijn echter geen ingezet binnen Azure. Bijna uitsluitend zijn die beveiligingsfuncties ingebouwd in de Azure-omgeving voor robuuste meerlaagse beveiligingsmechanismen zowel in de firewall-software. Daarnaast is het bereik van de grens en de bijbehorende kabels kritieke beveiligingsapparaten eenvoudiger kunt beheren en zoals weergegeven in de afbeelding hierboven, omdat deze wordt beheerd door de Azure-software inventariseren.

## <a name="core-security-and-firewall-features"></a>Beveiliging en firewall kernfuncties
Azure implementeert robuuste software beveiligings- en firewall-functies op verschillende niveaus af te dwingen beveiligingsfuncties die doorgaans wordt verwacht in een omgeving met traditionele de grens van de beveiliging autorisatie core beveiligen.

### <a name="azure-security-features"></a>Functies van Azure-beveiliging
Azure implementeert op basis van een host softwarefirewalls in het productienetwerk. Verschillende kernbeveiliging en firewall-functies bevinden zich in de kern van het Azure-omgeving. Deze beveiligingsfuncties weerspiegelen de strategie voor een defense-in-depth binnen de Azure-omgeving. Klantgegevens in Microsoft Azure wordt beveiligd door de volgende firewalls:

**Hypervisor Firewall (pakketfilter)**: deze firewall wordt in de Hypervisor geïmplementeerd en geconfigureerd door de FC-agent. Deze firewall beveiligt de tenant in de virtuele machine wordt uitgevoerd tegen onbevoegde toegang. Standaard, wanneer een virtuele machine wordt gemaakt. al het verkeer wordt geblokkeerd en vervolgens de agent FC regels/uitzonderingen toegevoegd in het filter geautoriseerde verkeer toestaan.

Er zijn twee categorieën regels die hier worden geprogrammeerd:

- Machine Config of infrastructuur regels: standaard alle communicatie is geblokkeerd. Er zijn uitzonderingen op een virtuele machine te verzenden en ontvangen van de communicatie van Dynamic Host Configuration Protocol (DHCP), DNS-gegevens, verkeer worden verzonden naar het 'openbare' Internet uitgaand naar andere virtuele machines binnen de FC-cluster en de OS-activeringsserver toestaan. Omdat de VM's toegestaan lijst met uitgaande bestemmingen omvat geen Microsoft Azure-router subnetten en andere Microsoft-eigenschappen, de regels voor deze fungeren als een laag van beveiliging.
- Configuratiebestand van de rol: Definieert de inkomende ACL's op basis van de tenants ServiceModel. Bijvoorbeeld, als er een tenant heeft een webfront-end op poort 80 op een bepaalde virtuele machine, poort 80 is geopend voor alle IP-adressen. Als de virtuele machine een werkrol die is uitgevoerd heeft, is alleen voor de virtuele machine binnen dezelfde tenant de werkrol geopend.

**Systeemeigen hostfirewall**: Microsoft Azure Fabric en opslag worden uitgevoerd op een eigen besturingssysteem waarvoor geen Hypervisor en daarom de Windows Firewall is geconfigureerd met de bovenstaande twee sets met regels.

**Firewall host**: de hostfirewall beveiligt de Host-partitie, die wordt uitgevoerd van de Hypervisor. De regels worden geprogrammeerd toestaan alleen de FC en dialoogvensters Neem contact op met de hostpartitie op een specifieke poort gaan. De overige uitzonderingen zijn bedoeld om DHCP-reacties en DNS-antwoorden toe te staan. Azure maakt gebruik van een Machine-configuratiebestand, waarvoor de sjabloon van de firewallregels voor de hostpartitie. Er is een firewall-uitzondering voor host die virtuele machines om te communiceren met de Host-onderdelen, kabel server & metagegevensserver, via specifieke protocol/poorten toestaat.

**Gast Firewall**: Windows Firewall-onderdeel van het Gastbesturingssysteem is configureerbaar door de klant op klant VM's en opslag.

Aanvullende beveiligingsfuncties ingebouwd in de mogelijkheden van Azure:

- Onderdelen van de infrastructuur zijn toegewezen IP-adressen die afkomstig van specifieke IP-adressen (DIPs zijn). Een aanvaller op Internet kan geen verkeer aan de adressen die adres omdat ze Microsoft zou niet worden bereikt. Internet-Gateway routers filteren pakketten geadresseerd uitsluitend aan interne adressen zodat ze niet het productienetwerk voert. De enige onderdelen die verkeer omgeleid naar de VIP's geaccepteerd zijn netwerktaakverdelers.
- De firewalls geïmplementeerd op alle knooppunten van de interne hebben drie primaire architectuur beveiligingsoverwegingen voor een gegeven scenario:

   - Deze achter de Load Balancer (LB) worden geplaatst en pakketten vanaf elke locatie accepteren. Deze zijn bedoeld om extern worden weergegeven en zou komen overeen met de poorten openen in een traditionele perimeterfirewall.
   - Alleen pakketten van een beperkt aantal adressen accepteren. Dit is onderdeel van de diepgaande strategie voor beveiliging tegen aanvallen. Dergelijke verbindingen worden cryptografisch geverifieerd.
   - Firewalls alleen toegankelijk is vanaf Selecteer interne knooppunten, in welk geval ze pakketten van een lijst van de bron-IP-adressen, die allemaal DIPs binnen het Azure-netwerk worden alleen geaccepteerd. Bijvoorbeeld, een aanval op het bedrijfsnetwerk aanvragen naar deze adressen kan sturen, maar ze zou worden geblokkeerd, tenzij het bronadres van het pakket in de lijst geïnventariseerde binnen het Azure-netwerk is.
   - De-router in het perimeternetwerk blokkeert uitgaande pakketten die zijn geadresseerd aan een adres dat is opgenomen in het netwerk van Azure vanwege de geconfigureerde statische routes.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)
