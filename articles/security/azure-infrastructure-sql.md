---
title: Beveiligingsfuncties voor Azure SQL Database
description: In dit artikel biedt dat een algemene beschrijving van Azure SQL Database beveiligt klantgegevens in Azure.
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
ms.openlocfilehash: cca8febb004029b13b0df09a047da701c4528e8e
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102354"
---
# <a name="microsoft-azure-sql-database-security-features"></a>Beveiligingsfuncties van Microsoft Azure SQL Database    
Microsoft Azure SQL Database biedt een relationele database-service in Azure. SQL-Database heeft voor het beveiligen van gegevens van de klant en sterke beveiligingsfuncties die klanten van een relationele database-service verwachten bieden, een eigen sets beveiligingsmogelijkheden. Deze mogelijkheden bouwen voort op de besturingselementen die worden overgenomen van Azure.

## <a name="security-capabilities"></a>Beveiligingsmogelijkheden

### <a name="usage-of-tabular-data-stream-tds-protocol"></a>Informatie over het gebruik van Stream TDS (Tabular Data)-protocol
Microsoft Azure SQL Database ondersteunt alleen het TDS-protocol, dat de database toegankelijk zijn alleen via de standaard poort van TCP/1433 vereist.

### <a name="microsoft-azure-sql-database-firewall"></a>Microsoft Azure SQL Database-Firewall
Microsoft Azure SQL Database omvat ter bescherming van gegevens van klanten, een firewall-functionaliteit die standaard voorkomt alle toegang tot de SQL-Database-server, zoals hieronder wordt weergegeven.

![Azure SQL Database-firewall][1]

De firewall gateway biedt de mogelijkheid om adressen waardoor het gedetailleerde controle voor klanten om op te geven acceptabele IP-adresbereiken te beperken. De firewall verleent toegang op basis van het oorspronkelijke IP-adres van elke aanvraag.

Firewall-configuratie kan worden bereikt via een Management-Portal of programmatisch met behulp van de Microsoft Azure SQL Database Management REST API. De Gateway voor Microsoft Azure SQL Database-firewall standaard voorkomt alle klant TDS-toegang tot Microsoft Azure SQL-Databases. Toegang moet worden geconfigureerd met behulp van ACL's toe te staan van Microsoft Azure SQL Database-verbindingen met de bron en bestemming internetadressen, protocollen en poortnummers.

### <a name="dosguard"></a>DoSGuard
Denial of service (DoS) aanvallen worden gereduceerd door een SQL Database-gatewayservice DoSGuard aangeroepen. DoSGuard houdt actief mislukte aanmeldingen vanaf IP-adressen. Als er meerdere mislukte aanmeldingen van een specifiek IP-adres binnen een periode, is het IP-adres van de toegang tot bronnen in de service voor een vooraf gedefinieerde periode geblokkeerd.

Naast de bovenstaande de Microsoft Azure SQL Database-gateway ook worden uitgevoerd:

- Beveiligd kanaal mogelijkheid onderhandelingen voor het implementeren van TDS FIPS 140-2 gevalideerd versleutelde verbindingen bij het verbinden met de database-servers.
- Stateful TDS-pakketinspecties tijdens het accepteren van verbindingen van clients. De gateway valideert de verbindingsgegevens en geeft op de TDS-pakketten naar de juiste fysieke server, op basis van de databasenaam opgegeven in de verbindingsreeks.

Het overkoepelende principe voor netwerkbeveiliging van het aanbod van Microsoft Azure SQL Database is toe te staan dat connection en communicatie die nodig is zodat de service om te werken. Alle andere poorten, protocollen en verbindingen worden standaard geblokkeerd. ACL's en VLAN's worden gebruikt voor het beperken van netwerkcommunicatie die door de bron en bestemming netwerken, protocollen en poortnummers.

Goedgekeurde mechanismen om te implementeren op basis van netwerk-ACL's bevatten: ACL's op routers en netwerktaakverdelers. Deze worden beheerd door de Azure-netwerken, VM-Gast firewall en Microsoft Azure SQL Database gateway firewallregels, die zijn geconfigureerd door de klant.

## <a name="data-segregation-and-customer-isolation"></a>Scheiding en klant gegevensisolatie
Het Azure-productienetwerk is gestructureerd dat openbaar toegankelijk systeemonderdelen zijn gescheiden van interne bronnen. Fysieke en logische grenzen bestaan tussen webservers toegang tot de openbare Azure-portal en de onderliggende Azure virtuele infrastructuur, waar de klant toepassingsexemplaren en klantgegevens zich bevinden.

Alle openbaar toegankelijke informatie wordt beheerd in de Azure-productienetwerk. Het productienetwerk is onderworpen aan de verificatie met twee factoren en mechanismen voor het beveiligen van grens, gebruikt de firewall- en beveiligingsinstellingen functieset beschreven in de vorige sectie en maakt gebruik van functies voor isolatie van gegevens zoals hieronder aangegeven.

### <a name="unauthorized-systems-and-isolation-of-fc"></a>Niet-geautoriseerde systemen en isolatie van FC
Aangezien de FC de centrale orchestrator van de Microsoft Azure-infrastructuur is, zijn belangrijke besturingselementen geïmplementeerd bedreigingen, met name van mogelijk aangetaste door vanuit klanttoepassingen te verhelpen. FC herkent niet alle hardware waarvan apparaatgegevens (bijvoorbeeld MAC-adres) niet vooraf in de FC geladen is. De DHCP-servers in de FC hebt lijsten met MAC-adressen van de knooppunten die ze wil opstarten zijn geconfigureerd. Zelfs als niet-geautoriseerde systemen zijn verbonden, zijn ze niet opgenomen in de Fabric-inventaris, en daarom niet is verbonden of gemachtigd zijn om te communiceren met elk systeem in de Fabric-inventarisatie. Dit vermindert het risico van niet-geautoriseerde systemen communiceren met de FC en toegang krijgen tot de VLAN- en Azure.

### <a name="vlan-isolation"></a>VLAN-isolatie
De Azure-productienetwerk is logisch onderverdeeld in drie primaire VLAN's:

- De belangrijkste VLAN-verbindingen niet-vertrouwde klant knooppunten
- De VLAN FC – bevat vertrouwde FCs- en ondersteunende systemen
- Het apparaat VLAN – bevat vertrouwd netwerk en andere infrastructuurapparaten

### <a name="packet-filtering"></a>Filteren van pakketten
De IPFilter en de softwarefirewalls geïmplementeerd op de basis-besturingssysteem en het Gastbesturingssysteem van de knooppunten connectiviteit beperkingen afdwingen en te voorkomen dat onbevoegde verkeer tussen VM's.

### <a name="hypervisor-root-os-and-guest-vms"></a>-Hypervisor, hoofdmap OS en Gast-VM 's
De isolatie van het besturingssysteem van de hoofdmap van de Gast-VM's en de Gast-VM's van elkaar wordt beheerd door de Hypervisor en het basis-besturingssysteem.

### <a name="types-of-rules-on-firewalls"></a>Typen regels op Firewalls
Een regel wordt gedefinieerd als:

{Security Response Center (Src) IP-adres bronpoort, doel-IP-, doelpoort bestemming Protocol, In/Out Stateless/Stateful, Stateful stroom time-out}.

SYN-pakketten zijn toegestaan in- of alleen als een van de regels is toegestaan. Voor TCP gebruikt Microsoft Azure staatloze regels waar het principe is dat alleen alle pakketten die niet SYN van of naar de virtuele machine kunnen. De lokale beveiliging is dat elke host-stack robuuste van een niet-SYN wordt genegeerd als een SYN-pakket heeft niet eerder hebt gezien. De TCP-protocol zelf stateful is, en in combinatie met de stateless SYNbased regel een algemene gedrag van een implementatie met een stateful bereikt.

Voor User Datagram Protocol (UDP) een stateful regel maakt gebruik van Microsoft Azure. Telkens wanneer een UDP-pakket overeenkomt met een regel, wordt een omgekeerde stroom gemaakt in de andere richting. Deze stroom bevat een ingebouwde time-out.

Er zijn klanten zelf verantwoordelijk voor het instellen van hun eigen firewalls boven op wat er in Microsoft Azure biedt. Hier vindt u klanten kunnen de regels voor binnenkomend en uitgaand verkeer te definiëren.

### <a name="production-configuration-management"></a>Productie-Configuratiebeheer
Beveiligde standaardconfiguraties worden beheerd door de respectieve operations teams in Azure en Microsoft Azure SQL Database. Alle configuratiewijzigingen productiesystemen zijn gedocumenteerd en gevolgd door een centrale traceersysteem. Software en hardware wijzigingen worden bijgehouden door het systeem centrale bijhouden. Netwerkwijzigingen met betrekking tot ACL worden bijgehouden met ACL Management Service (AMS).

Alle configuratiewijzigingen in Microsoft Azure zijn ontwikkeld en getest in de testomgeving; en daarna geïmplementeerde in productie-omgeving. Softwarebuilds worden beoordeeld als onderdeel van de testen. Beveiliging en privacy controles worden beoordeeld als onderdeel van het criterium voor controlelijst. Wijzigingen worden geïmplementeerd op geplande tijden door de respectieve implementatieteam. Versies zijn gecontroleerd en afgemeld door de medewerkers van de betreffende implementatie team voordat ze in productie zijn geïmplementeerd.

Wijzigingen worden bewaakt voor succes. De wijziging is teruggedraaid terug naar de vorige status van een foutenscenario of een hotfix is geïmplementeerd om de fout met goedkeuring van de aangewezen personeel op te lossen. Bron Depot, Git, TFS, MDS, uitlopers, Azure Security Monitoring (ASM), de FC en het platform WinFabric worden centraal beheren, toepassen en controleer of de configuratie-instellingen in de Azure-virtuele omgeving gebruikt.

Hardware- en wijzigingen hebben op dezelfde manier tot stand gebracht Validatiestappen om te evalueren hun voldoen aan de build-vereisten. De versies zijn gecontroleerd en gemachtigd door middel van een gecoördineerde wijziging Advisory Mededelingenbord (CAB) van de bijbehorende groepen op de stack.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Integriteit van de Azure-infrastructuur](azure-infrastructure-integrity.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)

<!--Image references-->
[1]: ./media/azure-infrastructure-sql/sql-database-firewall.png
