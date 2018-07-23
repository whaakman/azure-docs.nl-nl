---
title: Azure SQL Database-beveiligingsfuncties
description: Dit artikel bevat een algemene beschrijving van hoe Azure SQL Database klantgegevens in Azure beschermt.
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
ms.openlocfilehash: cce1ff1102c42bd1627caeba7b2c86432b228607
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170851"
---
# <a name="azure-sql-database-security-features"></a>Azure SQL Database-beveiligingsfuncties    
Azure SQL Database voorziet in een relationele database-service in Azure. Voor het beveiligen van klantgegevens en bieden krachtige beveiliging-functies die klanten van een relationele database-service verwachten, beschikt SQL Database over een eigen sets met mogelijkheden voor beveiliging. Deze mogelijkheden zijn gebaseerd op de besturingselementen die worden overgenomen van Azure.

## <a name="security-capabilities"></a>Mogelijkheden voor beveiliging

### <a name="usage-of-the-tds-protocol"></a>Gebruik van het TDS-protocol
Azure SQL Database ondersteunt alleen de gegevens in tabelvorm stream (TDS Tabular)-protocol, dat vereist dat de database toegankelijk zijn via alleen de standaard poort van de TCP/1433.

### <a name="azure-sql-database-firewall"></a>Azure SQL Database-firewall
Ter bescherming van gegevens van de klant, voorziet Azure SQL Database in een firewall-functionaliteit, die standaard voorkomt u alle toegang tot de SQL Database-server, dat zoals hieronder wordt weergegeven.

![Azure SQL Database-firewall][1]

De firewall van de gateway kunt adressen, waarmee klanten nauwkeurige controle te bereiken opgeven van acceptabele IP-adressen beperken. De firewall verleent toegang op basis van het oorspronkelijke IP-adres van elke aanvraag.

Klanten kunnen bereiken van firewall-configuratie met behulp van een beheerportal of programmatisch met behulp van de Azure SQL Database Management REST API. De firewall van de Azure SQL Database-gateway standaard voorkomt alle klant TDS-toegang tot Azure SQL database-exemplaren. Klanten moeten toegang met behulp van toegangsbeheerlijsten (ACL's) om toe te staan van Azure SQL Database-verbindingen configureren met bron en bestemming internet-adressen, protocollen en poortnummers.

### <a name="dosguard"></a>DoSGuard
Denial of service (DoS) aanval worden beperkt door een gateway-service van het SQL-Database met de naam DoSGuard. DoSGuard actief houdt bij of mislukte aanmeldingen vanaf IP-adressen. Als er meerdere mislukte aanmeldingen van een specifiek IP-adres binnen een bepaalde periode, wordt het IP-adres geblokkeerd toegang tot bronnen in de service voor een vooraf gedefinieerde periode.

Bovendien de Azure SQL Database-gateway worden uitgevoerd:

- Beveiligd kanaal mogelijkheid onderhandelingen voor het implementeren van TDS FIPS 140-2 gevalideerd versleutelde verbindingen wanneer deze verbinding met de database-servers maakt.
- Stateful TDS-pakketinspectie terwijl deze verbindingen van clients aanvaardt. De gateway valideert de verbindingsgegevens en geeft op de TDS-pakketten naar de juiste fysieke server, op basis van de naam van de database die opgegeven in de verbindingsreeks.

Het overkoepelende principe van netwerkbeveiliging van de Azure SQL Database-aanbieding is dat alleen de verbinding en communicatie die nodig is zodat de service te werken. Alle andere poorten, protocollen en -verbindingen worden standaard geblokkeerd. Virtuele LAN (VLAN's)-netwerken en ACL's worden gebruikt voor netwerkcommunicatie beperken door het bron- en doel netwerken, protocollen en poortnummers.

Mechanismen die zijn goedgekeurd als u wilt implementeren op basis van netwerk-ACL's zijn onder andere ACL's op routers en load balancers. Deze methoden worden beheerd door Azure-netwerken, firewall Gast-VM en Azure SQL Database-gateway firewallregels, die zijn geconfigureerd door de klant.

## <a name="data-segregation-and-customer-isolation"></a>Gegevensisolatie scheiding en de klant
De Azure-productienetwerk is gestructureerd zodat openbaar toegankelijke systeemonderdelen zijn gescheiden van interne bronnen. Fysiek en logisch grenzen bestaan tussen de webservers die toegang tot de openbare Azure-portal bieden en de onderliggende Azure virtuele infrastructuur, waar de toepassingsexemplaren klant en gegevens van de klant zich bevinden.

Alle openbaar toegankelijke informatie wordt beheerd in de Azure-productienetwerk. Het productienetwerk is onderhevig aan tweeledige verificatie en grens mechanismen voor het beveiligen, gebruikt de firewall- en beveiligingsinstellingen functieset die wordt beschreven in de vorige sectie en maakt gebruik van functies voor isolatie van gegevens, zoals vermeld in de volgende secties.

### <a name="unauthorized-systems-and-isolation-of-the-fc"></a>Niet-geautoriseerde systemen en isolatie van de FC
Omdat de infrastructuurcontroller (FC) de centrale orchestrator van de Azure-infrastructuur is, zijn belangrijke besturingselementen aanwezig op bedreigingen, met name van potentieel aangetast FAs binnen toepassingen van klanten worden verminderd. Geen hardware waarvan apparaatgegevens (bijvoorbeeld MAC-adres) niet vooraf in de FC geladen is wordt niet herkend door de FC. De DHCP-servers in de FC zijn lijsten met MAC-adressen van de knooppunten die ze wil opstarten zijn geconfigureerd. Zelfs als niet-geautoriseerde systemen zijn verbonden, zijn ze niet opgenomen in de fabric-inventarisatie, en daarom niet verbonden of gemachtigd om te communiceren met elk systeem in de fabric-inventarisatie. Dit vermindert het risico van niet-geautoriseerde systemen communiceren met de FC en toegang krijgen tot de VLAN- en Azure.

### <a name="vlan-isolation"></a>VLAN-isolatie
De Azure-productienetwerk is logisch onderverdeeld in drie primaire VLAN's:

- Het belangrijkste VLAN: koppelt klant niet-vertrouwde knooppunten.
- De FC-VLAN: Bevat vertrouwde FCs en ondersteunende systemen.
- Het apparaat VLAN: vertrouwd netwerk en andere infrastructuurapparaten bevat.

### <a name="packet-filtering"></a>Filteren van netwerkpakketten
De IPFilter en de softwarefirewalls die zijn geïmplementeerd op de basis-besturingssysteem en gastbesturingssysteem van de knooppunten verbinding beperkingen afdwingen en te voorkomen dat niet-geautoriseerde verkeer tussen virtuele machines.

### <a name="hypervisor-root-os-and-guest-vms"></a>Hypervisor, hoofdmap OS en Gast-VM 's
De isolatie van de hoofdmap van besturingssysteem van de Gast-VM's en de Gast-VM's van elkaar wordt beheerd door de hypervisor en de basis-besturingssysteem.

### <a name="types-of-rules-on-firewalls"></a>Typen regels op firewalls
Een regel wordt gedefinieerd als:

{Security Response Center (Src) IP-adres, bronpoort, doel IP, doelpoort, doel-Protocol, In/uit Stateless/Stateful, Stateful stroom Timeout}.

Synchrone niet-actief-teken (SYN)-pakketten zijn toegestaan in of uit alleen als een van de regels toestaat. Voor TCP gebruikt Azure stateless regels waar het principe is dat alleen alle niet-SYN-pakketten naar of uit de virtuele machine. De locatie van de beveiliging is dat elke host-stack bestand tegen een niet-SYN te negeren is als een SYN-pakket is niet eerder hebt gezien. De TCP-protocol zelf is stateful en realiseert een algehele gedrag van een stateful-implementatie in combinatie met de stateless SYN gebaseerde regel.

Voor User Datagram Protocol (UDP), een stateful regel maakt gebruik van Azure. Telkens wanneer een UDP-pakket overeenkomt met een regel, wordt een stroom gemaakt in de andere richting. Deze stroom heeft een ingebouwde time-out.

Klanten zijn verantwoordelijk voor het instellen van hun eigen firewalls boven op wat Azure biedt. Hier vindt u klanten kunnen definiëren de regels voor binnenkomend en uitgaand verkeer.

### <a name="production-configuration-management"></a>Productie-Configuratiebeheer
Beveiligde standaardconfiguraties worden beheerd door de desbetreffende operations-teams in Azure en Azure SQL Database. Alle configuratiewijzigingen op productiesystemen zijn gedocumenteerd en gevolgd door een centrale volgsysteem. Hardware- en wijzigingen worden bijgehouden door het centrale volgsysteem. Netwerkwijzigingen die betrekking op ACL hebben bijgehouden met behulp van een service voor het beheer van ACL.

Alle configuratiewijzigingen naar Azure zijn ontwikkeld en getest in de testomgeving, en deze vervolgens in productie-omgeving worden geïmplementeerd. Softwarebuilds worden beoordeeld als onderdeel van het testen. Controles voor beveiliging en privacy zijn beoordeeld als onderdeel van de vermelding controlelijst criteria. Wijzigingen worden geïmplementeerd op geplande tijden door de respectieve implementatieteam. Releases worden bekeken en goedgekeurd door de betreffende implementatie team personeel voordat ze in productie zijn geïmplementeerd.

Wijzigingen worden bewaakt voor succes. De wijziging wordt teruggedraaid naar de oorspronkelijke staat op een of een hotfix is geïmplementeerd om de fout met goedkeuring van het aangewezen personeel op te lossen. Bron Depot, Git, TFS, Master Data Services (MDS), runners-up, Azure security monitoring, de FC en het platform WinFabric worden centraal beheren, toepassing en controleer of de configuratie-instellingen in de Azure-virtuele omgeving gebruikt.

Hardware- en wijzigingen hebben op dezelfde manier tot stand gebracht Validatiestappen automatisch om te evalueren hun voldoet aan de vereisten. De versies zijn gecontroleerd en gemachtigd door een gecoördineerde adviesraad (CAB) van de betreffende groepen in de stack.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-sql/sql-database-firewall.png
