---
title: Azure productienetwerk
description: Dit artikel bevat een algemene beschrijving van de Azure-productienetwerk.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2018
ms.author: terrylan
ms.openlocfilehash: afae7cc6390ea4cd8c18c687e9d99400c8da9da4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116929"
---
# <a name="the-azure-production-network"></a>De Azure-productienetwerk
De gebruikers van de Azure-productienetwerk zijn beide externe klanten die toegang hebben tot hun eigen Azure-toepassingen en interne Azure ondersteuningsmedewerkers die het productienetwerk te beheren. Dit artikel worden de beveiligingsmethoden voor toegang en beveiliging mechanismen voor het tot stand brengen van verbindingen met de Azure-productienetwerk.

## <a name="internet-routing-and-fault-tolerance"></a>Internet-Routering en fouttolerantie geboden
Een wereldwijd redundante interne en externe Azure Domain Name Service (DNS)-infrastructuur, gecombineerd met meerdere primaire en secundaire DNS-server-clusters, biedt fouttolerantie. Extra Azure-netwerk beveiligingsmechanismen, zoals NetScaler, worden op hetzelfde moment, distributed denial of service (DDoS)-aanvallen voorkomen en de integriteit van de Azure DNS-services gebruikt.

De Azure DNS-servers bevinden zich op meerdere datacenter faciliteiten. De Azure DNS-implementatie is uitgerust met een hiërarchie van secundaire als primaire DNS-servers om op te lossen openbaar Azure-klant domeinnamen. De domeinnamen worden meestal omgezet naar een adres CloudApp.net, die geschikt is voor het virtuele IP-adres (VIP)-adres voor de service van de klant. Uniek is voor Azure, de VIP-adres dat overeenkomt met interne toegewezen IP (DIP)-adres van de omzetting van de tenant wordt uitgevoerd door de Microsoft load balancers die verantwoordelijk is voor die VIP.

Azure wordt gehost in geografisch verspreide Azure-datacenters binnen de VS, en deze nu gebouwd op Routering status-of-the-art-platforms die robuuste, schaalbare architectuur standaarden implementeren. Een van de belangrijke functies zijn:

- Multiprotocol Label Switching MPLS-verkeer-engineering, waarmee u gebruik van de efficiënte verbinding en vensters vermindering van de service als er een storing kan worden gebruikt.
- Netwerken worden geïmplementeerd met 'moet plus één' (N + 1) redundantie architecturen of hoger.
- Extern, worden datacenters aangeboden door toegewezen netwerk met hoge bandbreedte-circuits die eigenschappen toch toe verbinding te met meer dan 1200 internetproviders wereldwijd in meerdere peering punten maken. Deze verbinding bevat meer dan 2000 GB per seconde (GBps) van de capaciteit van de rand.

Omdat Microsoft eigenaar is van een eigen circuits netwerk tussen datacenters, helpen deze kenmerken in de beschikbaarheid van 99,9 + procent netwerk zonder de noodzaak van traditionele van derden internetproviders Azure te bieden.

## <a name="connection-to-production-network-and-associated-firewalls"></a>Verbinding met het productienetwerk en bijbehorende firewalls
Het Azure-netwerk internet verkeer stroom beleid zorgt ervoor dat verkeer naar de Azure-productienetwerk dat in de dichtstbijzijnde regionaal datacenter binnen de Verenigde Staten bevindt zich. Omdat de productie-Azure-datacenters consistente netwerkarchitectuur en hardware onderhouden, wordt de beschrijving van de stroom verkeer die volgt consistent geldt voor alle datacenters.

Nadat het internetverkeer voor Azure wordt doorgestuurd naar het dichtstbijzijnde datacentrum, wordt geen verbinding gemaakt met de access-routers. Deze toegang routers fungeren voor het isoleren van verkeer tussen Azure-knooppunten en virtuele machines klant geïnstantieerd. Infrastructuur van netwerkapparaten op de toegangs- en edge-locaties zijn de grens punten waar inkomende en uitgaande filters worden toegepast. Dankzij deze routers zijn geconfigureerd door een lijst met gelaagde toegangsbeheer (ACL) ongewenste-netwerkverkeer filteren en toepassen van verkeer frequentielimieten toe, indien nodig. Verkeer dat is toegestaan door de ACL wordt doorgestuurd naar de load balancers. Distributie-routers zijn ontworpen om alleen Microsoft goedgekeurde IP-adressen toestaan, geeft u anti-adresvervalsing (spoofing) en TCP-verbindingen die gebruikmaken van ACL's tot stand brengen.

Externe taakverdeling apparaten bevinden zich achter de routers toegang netwerkadresomzetting (NAT) uitvoert van internet routeerbare IP-adressen naar Azure interne IP-adressen. De apparaten routeert pakketten ook naar geldige productie interne IP-adressen en poorten, en deze fungeren als een beveiligingsmechanisme om te beperken om de adresruimte van de interne productie-netwerk vrij te geven.

Standaard dwingt Microsoft Hypertext Transfer Protocol Secure (HTTPS) voor al het verkeer dat wordt verzonden naar klanten webbrowsers, met inbegrip van aanmelding en al het verkeer daarna. Het gebruik van TLS 1.2 kunt een beveiligde tunnel voor verkeer door kunnen stromen. ACL's op de toegangs- en core routers zorgen dat de bron van het verkeer overeenkomt met wat er wordt verwacht.

Een belangrijk verschil in deze architectuur, is wanneer deze wordt vergeleken met traditionele beveiligingsarchitectuur, dat er geen speciale hardware-firewalls, gespecialiseerde inbraakdetectie of preventie apparaten of andere beveiligingsapparaten die normaal gesproken verwacht voordat verbindingen worden aangebracht aan de productie van Azure-omgeving. Deze firewall hardwareapparaten verwachten klanten meestal in de Azure-netwerk. echter worden geen gebruikt in Azure. Bijna exclusief zijn de beveiligingsfuncties die ingebouwd in de software die wordt uitgevoerd van de Azure-omgeving voor robuuste, met meerdere lagen beveiligingsmechanismen, met inbegrip van firewall-mogelijkheden. Daarnaast is het bereik van de grens en de bijbehorende onoverzichtelijke, omslachtige van essentiële apparaten gemakkelijker te beheren en inventaris, zoals wordt weergegeven in de vorige afbeelding, omdat het wordt beheerd door de software die wordt uitgevoerd op Azure.

## <a name="core-security-and-firewall-features"></a>Kernfuncties van beveiligings- en firewall
Firewallfuncties op verschillende niveaus voor het afdwingen van beveiligingsfuncties die meestal worden verwacht in een omgeving met traditionele ter bescherming van de grens van de beveiliging autorisatie core robuuste softwarebeveiliging en de implementatie van Azure.

### <a name="azure-security-features"></a>Functies van Azure-beveiliging
De implementatie van Azure-host op basis van softwarefirewalls in het productienetwerk. Verschillende kernbeveiliging en firewall-functies zich bevinden in de belangrijkste Azure-omgeving. Deze beveiligingsfuncties weer een ingrijpende strategie in de Azure-omgeving. Klantgegevens in Azure wordt beveiligd door de volgende firewalls:

**Hypervisor-firewall (pakketfilter)**: Deze firewall wordt in de hypervisor geïmplementeerd en geconfigureerd door de infrastructuurcontroller (FC)-agent. Deze firewall beveiligt de tenant die wordt uitgevoerd binnen de virtuele machine tegen onbevoegde toegang. Standaard, wanneer een virtuele machine wordt gemaakt, al het verkeer wordt geblokkeerd en vervolgens de agent FC regels en uitzonderingen toegevoegd in het filter geautoriseerde verkeer toe te staan.

Twee categorieën regels worden hier geprogrammeerd:

- **Regels van configuratie of een infrastructuur van de computer**: Standaard wordt alle communicatie geblokkeerd. Uitzonderingen bestaan waarmee een virtuele machine te verzenden en ontvangen van Dynamic Host Configuration Protocol (DHCP)-communicatie en DNS-gegevens, en verkeer verzenden naar het 'openbare' internet uitgaand naar andere virtuele machines binnen het FC-cluster en de activeringsserver van het besturingssysteem. Omdat de virtuele machines toegestaan de lijst met uitgaande bestemmingen bevat geen Azure-routersubnetten en andere eigenschappen van Microsoft, de regels fungeren als een laag van beveiliging voor hen.
- **Rol bestand configuratieregels**: Hiermee definieert u de binnenkomende ACL's op basis van de tenants ServiceModel. Bijvoorbeeld, als een tenant een webfront-end op poort 80 op een bepaalde virtuele machine heeft, wordt poort 80 geopend voor alle IP-adressen. Als de virtuele machine een worker-functie die wordt uitgevoerd heeft, wordt de werkrol alleen voor de virtuele machine binnen dezelfde tenant geopend.

**Native hostfirewall**: Azure Service Fabric en Azure Storage worden uitgevoerd op een eigen besturingssysteem, dat geen hypervisor heeft en daarom Windows Firewall is geconfigureerd met de voorgaande twee sets met regels.

**Host-firewall**: De hostfirewall beveiligt de hostpartitie, die de hypervisor wordt uitgevoerd. De regels zijn zo geprogrammeerd toestaan alleen de FC en jumpboxes om te communiceren met de hostpartitie op een specifieke poort. De overige uitzonderingen zijn om toe te staan DHCP-reacties en DNS-antwoorden. Een machine-configuratiebestand, met een sjabloon van firewall-regels voor de hostpartitie wordt gebruikt door Azure. Een host firewall-uitzondering bestaat waarmee virtuele machines om te communiceren met de host-onderdelen, wire-server en de van metagegevensserver, via specifieke protocol/poorten.

**Gast-firewall**: De Windows Firewall-gedeelte van het gastbesturingssysteem te installeren, die kan geconfigureerd door klanten op klant virtuele machines en opslag worden.

Aanvullende beveiligingsfuncties die zijn ingebouwd in de mogelijkheden van Azure zijn onder andere:

- Onderdelen van de infrastructuur die IP-adressen die afkomstig van Spanningsdips zijn zijn toegewezen. Een aanvaller op internet kan geen verkeer naar deze adressen-, omdat het Microsoft kan niet worden bereikt. Internet-gatewayrouters filteren pakketten die zijn gericht uitsluitend aan interne-adressen, zodat ze niet het productienetwerk voert. De enige onderdelen die het verkeer dat wordt omgeleid naar de VIP's accepteren, load balancers.
- Firewalls die zijn geïmplementeerd op alle knooppunten van de interne zijn drie primaire architectuur beveiligingsoverwegingen voor een gegeven scenario:

   - Firewalls achter de load balancer worden geplaatst en pakketten vanaf elke locatie accepteren. Deze pakketten zijn bedoeld om u te verrassen extern en zou komen overeen met de poorten openen in een traditionele perimeterfirewall.
   - Firewalls accepteren pakketten alleen vanuit een beperkt aantal adressen. Deze overweging is onderdeel van de verdediging uitgebreide strategie tegen DDoS-aanvallen. Deze verbindingen zijn cryptografisch geverifieerd.
   - Firewalls zijn alleen van select interne knooppunten toegankelijk. Deze accepteren pakketten vanuit een lijst van de bron-IP-adressen, die allemaal Spanningsdips binnen het Azure-netwerk worden alleen. Bijvoorbeeld, een aanval op het bedrijfsnetwerk kan direct aanvragen voor deze adressen, maar de aanvallen zouden worden geblokkeerd, tenzij het bron-adres van het pakket in de lijst geïnventariseerde binnen het Azure-netwerk is.
     - De access-router in het perimeternetwerk blokkeert uitgaande pakketten die naar een adres dat binnen het Azure-netwerk is vanwege de geconfigureerde statische routes worden behandeld.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)
