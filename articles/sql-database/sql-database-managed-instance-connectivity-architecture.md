---
title: Architectuur van de verbinding voor een beheerd exemplaar in Azure SQL Database | Microsoft Docs
description: Meer informatie over Azure SQL Database managed instance-communicatie en connectiviteitsarchitectuur ook hoe de onderdelen van verkeer naar het beheerd exemplaar te regelen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: 6ef020ff1054416e2b9af5af824b9aa27f0b1e64
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57247236"
---
# <a name="connectivity-architecture-for-a-managed-instance-in-azure-sql-database"></a>Architectuur van de verbinding voor een beheerd exemplaar in Azure SQL Database 

In dit artikel wordt uitgelegd communicatie in een beheerd exemplaar voor Azure SQL Database. Ook wordt beschreven connectiviteitsarchitectuur en hoe de onderdelen van verkeer naar het beheerd exemplaar te regelen.  

Het beheerde exemplaar van SQL-Database wordt in de Azure-netwerk en het subnet dat toegewezen aan beheerde exemplaren geplaatst. Deze implementatie biedt:

- Een veilige persoonlijke IP-adres.
- De mogelijkheid om een on-premises netwerk verbinden met een beheerd exemplaar.
- De mogelijkheid om te verbinden met een beheerd exemplaar van een gekoppelde server of een andere on-premises gegevensarchief.
- De mogelijkheid om te verbinden met een beheerd exemplaar van Azure-resources.

## <a name="communication-overview"></a>Overzicht servicecommunicatie

Het volgende diagram toont de entiteiten die verbinding met een beheerd exemplaar maken. U ziet ook de bronnen die nodig zijn om te communiceren met het beheerde exemplaar. Het communicatieproces aan de onderkant van het diagram geeft klanttoepassingen en hulpprogramma's die verbinding met het beheerde exemplaar als gegevensbronnen maken.  

![Entiteiten in connectiviteitsarchitectuur](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Een beheerd exemplaar is een platform als service (PaaS) te kunnen aanbieden. Microsoft maakt gebruik van geautomatiseerde agents (beheer, implementatie en onderhoud) voor het beheren van deze service op basis van telemetrie-gegevensstromen. Omdat Microsoft verantwoordelijk voor het beheer is, klanten geen toegang tot de beheerde exemplaar cluster virtuele machines via Remote Desktop Protocol (RDP).

Een SQL-Server bewerkingen aan de slag door gebruikers of toepassingen mogelijk beheerde instanties om te communiceren met het platform. Een voorbeeld is het maken van een beheerd exemplaar in de database. Deze resource is toegankelijk via de Azure-portal, PowerShell, Azure CLI en de REST-API.

Beheerde exemplaren afhankelijk zijn van de Azure-services zoals Azure Storage voor back-ups, Azure Service Bus voor telemetrie, Azure Active Directory voor verificatie en Azure Key Vault voor transparante gegevensversleuteling (TDE). De beheerde exemplaren maken verbindingen met deze services.

Alle communicatie certificaten gebruiken voor versleuteling en ondertekening. Om te controleren of de betrouwbaarheid van communicerende partijen, beheerde verifiëren exemplaren voortdurend deze certificaten contact opnemen met een certificeringsinstantie. Als de certificaten worden ingetrokken of niet kunnen worden geverifieerd, sluit het beheerde exemplaar de verbindingen om de gegevens te beveiligen.

## <a name="high-level-connectivity-architecture"></a>Connectiviteitsarchitectuur op hoog niveau

Op hoog niveau is een beheerd exemplaar van een set met serviceonderdelen. Deze onderdelen worden gehost op een toegewezen set met geïsoleerde virtuele machines die worden uitgevoerd binnen het subnet van de klant virtuele netwerk. Deze machines vormen een virtueel cluster.

Een virtueel cluster kan meerdere beheerde exemplaren hosten. Indien nodig, wordt het cluster automatisch uitgebreid of opdrachten wanneer het aantal ingerichte exemplaren in het subnet van de klant wordt gewijzigd.

Toepassingen van klanten verbinding kunnen maken met beheerde exemplaren en query's kunnen uitvoeren en databases van de update alleen als ze worden uitgevoerd binnen het virtuele netwerk gekoppeld virtueel netwerk, of een netwerk verbonden via VPN of Azure ExpressRoute. Dit netwerk moet een eindpunt en een privé IP-adres gebruiken.  

![Architectuurdiagram van connectiviteit](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Services voor het beheer en de implementatie van Microsoft worden uitgevoerd buiten het virtuele netwerk. Een beheerd exemplaar en Microsoft-services verbinding maken via de eindpunten met openbare IP-adressen. Als een beheerd exemplaar maakt een uitgaande verbinding op de ontvangende kant NAT (Network Address Translation) wordt de verbinding eruit deze afkomstig van dit openbare IP-adres.

Beheer van verkeer stroomt via virtueel netwerk van de klant. Dit betekent dat dat elementen van de infrastructuur van het virtuele netwerk schadelijk voor beheer van verkeer zijn kunnen door te maken van het exemplaar mislukt en niet beschikbaar.

> [!IMPORTANT]
> Voor het verbeteren van klantervaring en beschikbaarheid van de service, past Microsoft een intentie netwerkbeleid op Azure virtual network-Infrastructuurelementen. Het beleid kan van invloed op de werking van het beheerde exemplaar. Dit mechanisme platform communiceert transparant netwerkvereisten voor gebruikers. Het belangrijkste doel van het beleid is om te voorkomen dat netwerk onjuiste configuratie en het zorgen voor beheerd exemplaar voor normale bewerkingen. Wanneer u een beheerd exemplaar verwijdert, wordt de intentie netwerkbeleid ook verwijderd.

## <a name="virtual-cluster-connectivity-architecture"></a>Virtueel cluster connectiviteitsarchitectuur

We gaan meer informatie over in de connectiviteitsarchitectuur van de voor beheerde exemplaren. Het volgende diagram toont de algemene indeling van de virtuele-cluster.

![Connectiviteitsarchitectuur van de virtuele-cluster](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Clients verbinding maken met een beheerd exemplaar met behulp van de naam van een host met het formulier `<mi_name>.<dns_zone>.database.windows.net`. Deze hostnaam wordt omgezet in een privé IP-adres, maar het geregistreerd in een openbare domein Name System (DNS)-zone en openbaar omgezette. De `zone-id` wordt automatisch gegenereerd bij het maken van het cluster. Als een nieuw cluster als host fungeert voor een secundaire beheerd exemplaar, wordt deze de zone-ID deelt met de primaire cluster. Zie voor meer informatie, [autofailover groepen gebruiken voor het inschakelen van transparante en gecoördineerd failover van meerdere databases](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Dit privé IP-adres behoort tot het beheerde exemplaar van interne load balancer. De load balancer zorgt ervoor dat verkeer naar de gateway van het beheerde exemplaar. Omdat meerdere beheerde exemplaren kunnen worden uitgevoerd in hetzelfde cluster, de gateway maakt gebruik van de hostnaam van het beheerde exemplaar verkeer omleiden naar de juiste SQL-engine-service.

Beheer-en implementatie met een beheerd exemplaar te maken met een [beheereindpunt](#management-endpoint) dat is toegewezen aan een externe netwerktaakverdeler. Verkeer wordt doorgestuurd naar de knooppunten alleen als deze is ontvangen op een vooraf gedefinieerde set van poorten die alleen onderdelen voor het beheerde exemplaar gebruiken. Een ingebouwde firewall op de knooppunten wordt ingesteld om toe te staan alleen verkeer van Microsoft IP-bereiken. Certificaten alle communicatie tussen de onderdelen en het beheervlak wederzijds te verifiëren.

## <a name="management-endpoint"></a>Beheereindpunt

Microsoft beheert het beheerde exemplaar met behulp van een eindpunt. Dit eindpunt is in de virtuele cluster van het exemplaar. Het eindpunt is beveiligd door een ingebouwde firewall op niveau van het netwerk. Op het niveau van de toepassing, wordt deze beveiligd door wederzijdse certificaatverificatie. IP-adres van het eindpunt, Zie [vast IP-adres voor het beheereindpunt](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Wanneer verbindingen start in het beheerde exemplaar (net als bij back-ups en logboeken voor controle), verkeer wordt weergegeven om te starten vanaf het openbare IP-adres van het eindpunt. U kunt toegang beperken tot openbare services van een beheerd exemplaar door in te stellen van firewallregels om toe te staan alleen IP-adres van het beheerde exemplaar. Zie voor meer informatie, [controleren of de ingebouwde firewall van het beheerde exemplaar](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> In tegenstelling tot de firewall voor verbindingen die binnen het beheerde exemplaar worden gestart, hebben de Azure-services die binnen het gebied van het beheerde exemplaar firewall die geoptimaliseerd voor het verkeer dat wordt verstuurd tussen deze services.

## <a name="network-requirements"></a>Netwerkvereisten

Implementeer een beheerd exemplaar in een speciaal subnet binnen het virtuele netwerk. Het subnet moet beschikken over deze kenmerken:

- **Toegewezen subnet:** Subnet van het beheerde exemplaar kan niet een andere cloudservice die is gekoppeld aan deze bevatten en mag niet een gatewaysubnet. Het subnet mag niet een resource, maar het beheerde exemplaar en later kunt u resources in het subnet toevoegen.
- **Netwerkbeveiligingsgroep (NSG):** Een NSG die is gekoppeld aan het virtuele netwerk moet definiëren [inkomende beveiligingsregels](#mandatory-inbound-security-rules) en [uitgaande beveiligingsregels](#mandatory-outbound-security-rules) vóór alle andere regels. U kunt een NSG gebruiken voor het beheren van toegang tot het beheerde exemplaar gegevens eindpunt door te filteren van het verkeer op poort 1433.
- **Gebruiker gedefinieerde route (UDR) tabel:** Een UDR-tabel die is gekoppeld aan het virtuele netwerk moet bevatten specifieke [vermeldingen](#user-defined-routes).
- **Er is geen service-eindpunten:** Er is geen service-eindpunt moet worden gekoppeld aan het beheerde exemplaar subnet. Zorg ervoor dat de service-eindpunten-optie is uitgeschakeld bij het maken van het virtuele netwerk.
- **Voldoende IP-adressen:** Het subnet beheerd exemplaar moet ten minste 16 IP-adressen hebben. Het aanbevolen minimum is 32 IP-adressen. Zie voor meer informatie, [bepalen van de grootte van het subnet voor beheerde exemplaren](sql-database-managed-instance-determine-size-vnet-subnet.md). U kunt beheerde exemplaren in implementeren [het bestaande netwerk](sql-database-managed-instance-configure-vnet-subnet.md) nadat u hebt geconfigureerd om te voldoen aan [de netwerkvereisten voor beheerde exemplaren](#network-requirements). Anders maakt u een [nieuwe netwerk en subnet](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> U kunt een nieuwe beheerde exemplaar niet implementeren als het doelsubnet beschikt niet over deze kenmerken. Wanneer u een beheerd exemplaar maakt, wordt een intentie netwerkbeleid toegepast op het subnet om te voorkomen dat niet-compatibele wijzigingen tot het instellen van netwerken. Nadat het laatste exemplaar van het subnet is verwijderd, wordt de intentie netwerkbeleid ook verwijderd.

### <a name="mandatory-inbound-security-rules"></a>Verplichte beveiligingsregels voor binnenkomend verkeer

| Name       |Poort                        |Protocol|Bron           |Doel|Bewerking|
|------------|----------------------------|--------|-----------------|-----------|------|
|beheer  |9000, 9003, 1438, 1440, 1452|TCP     |Alle              |Alle        |Toestaan |
|mi_subnet   |Alle                         |Alle     |MI-SUBNET        |Alle        |Toestaan |
|health_probe|Alle                         |Alle     |AzureLoadBalancer|Alle        |Toestaan |

### <a name="mandatory-outbound-security-rules"></a>Verplichte uitgaande beveiligingsregels

| Name       |Poort          |Protocol|Bron           |Doel|Bewerking|
|------------|--------------|--------|-----------------|-----------|------|
|beheer  |80, 443, 12000|TCP     |Alle              |Internet   |Toestaan |
|mi_subnet   |Alle           |Alle     |Alle              |MI-SUBNET *  |Toestaan |

\* MI-SUBNET verwijst naar het IP-adresbereik voor het subnet in het formulier 10.x.x.x/y. U kunt deze informatie vinden in de Azure-portal in de subneteigenschappen van het.

> [!IMPORTANT]
> Hoewel de vereiste beveiligingsregels voor binnenkomend verkeer toestaan verkeer van _eventuele_ bron op poorten 9000, 9003, 1438 1440 en 1452, deze poorten worden beveiligd door een ingebouwde firewall. Zie voor meer informatie, [bepalen de eindpuntadres management](sql-database-managed-instance-find-management-endpoint-ip-address.md).

> [!NOTE]
> Als u transactionele replicatie in een beheerd exemplaar gebruiken, en als u een exemplaar in de database als een publisher of een distributor, opent poort 445 (TCP uitgaand) in van het subnet beveiligingsregels voor verbindingen. Deze poort krijgt toegang tot de Azure-bestandsshare.

### <a name="user-defined-routes"></a>Door de gebruiker gedefinieerde routes

|Name|Adresvoorvoegsel|Volgende Hop|
|----|--------------|-------|
|subnet_to_vnetlocal|[mi_subnet]|Virtueel netwerk|
|mi-0-5-next-hop-internet|0.0.0.0/5|Internet|
|MI-11-8-nexthop-internet|11.0.0.0/8|Internet|
|mi-12-6-nexthop-internet|12.0.0.0/6|Internet|
|mi-128-3-nexthop-internet|128.0.0.0/3|Internet|
|mi-16-4-nexthop-internet|16.0.0.0/4|Internet|
|mi-160-5-nexthop-internet|160.0.0.0/5|Internet|
|mi-168-6-nexthop-internet|168.0.0.0/6|Internet|
|MI-172-12-nexthop-internet|172.0.0.0/12|Internet|
|mi-172-128-9-nexthop-internet|172.128.0.0/9|Internet|
|mi-172-32-11-nexthop-internet|172.32.0.0/11|Internet|
|mi-172-64-10-nexthop-internet|172.64.0.0/10|Internet|
|mi-173-8-nexthop-internet|173.0.0.0/8|Internet|
|mi-174-7-nexthop-internet|174.0.0.0/7|Internet|
|mi-176-4-nexthop-internet|176.0.0.0/4|Internet|
|mi-192-128-11-nexthop-internet|192.128.0.0/11|Internet|
|mi-192-160-13-nexthop-internet|192.160.0.0/13|Internet|
|mi-192-169-16-nexthop-internet|192.169.0.0/16|Internet|
|mi-192-170-15-nexthop-internet|192.170.0.0/15|Internet|
|mi-192-172-14-nexthop-internet|192.172.0.0/14|Internet|
|mi-192-176-12-nexthop-internet|192.176.0.0/12|Internet|
|mi-192-192-10-nexthop-internet|192.192.0.0/10|Internet|
|mi-192-9-nexthop-internet|192.0.0.0/9|Internet|
|mi-193-8-nexthop-internet|193.0.0.0/8|Internet|
|mi-194-7-nexthop-internet|194.0.0.0/7|Internet|
|mi-196-6-nexthop-internet|196.0.0.0/6|Internet|
|mi-200-5-nexthop-internet|200.0.0.0/5|Internet|
|mi-208-4-nexthop-internet|208.0.0.0/4|Internet|
|mi-224-3-nexthop-internet|224.0.0.0/3|Internet|
|mi-32-3-nexthop-internet|32.0.0.0/3|Internet|
|mi-64-2-nexthop-internet|64.0.0.0/2|Internet|
|mi-8-7-nexthop-internet|8.0.0.0/7|Internet|
||||

U kunt bovendien vermeldingen toevoegen aan de routetabel voor het routeren van verkeer dat on-premises privé IP-adresbereiken als doel via de gateway van virtueel netwerk of een virtueel netwerkapparaat (NVA heeft).

Als het virtuele netwerk een aangepaste DNS-server bevat, moet u een vermelding voor de Azure recursieve naamomzetting IP-adres (zoals 168.63.129.16) toevoegen. Zie voor meer informatie, [instellen van een aangepaste DNS-server](sql-database-managed-instance-custom-dns.md). De aangepaste DNS-server moeten kunnen omzetten van hostnamen in deze domeinen en hun subdomeinen: *microsoft.com*, *windows.net*, *windows.com*,  *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, en *microsoftonline-p.com*.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [geavanceerde beveiliging van gegevens van SQL-Database](sql-database-managed-instance.md).
- Meer informatie over het [instellen van een nieuwe Azure-netwerk](sql-database-managed-instance-create-vnet-subnet.md) of een [bestaande Azure-netwerk](sql-database-managed-instance-configure-vnet-subnet.md) waarin u beheerde exemplaren kunt implementeren.
- [De grootte van het subnet berekenen](sql-database-managed-instance-determine-size-vnet-subnet.md) waar u de beheerde exemplaren implementeren.
- Informatie over het maken van een beheerd exemplaar:
  - Uit de [Azure-portal](sql-database-managed-instance-get-started.md).
  - Met behulp van [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/).
  - Met behulp van [een Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/).
  - Met behulp van [een Azure Resource Manager-sjabloon (met JumpBox, met SSMS opgenomen)](https://portal.azure.com/).
