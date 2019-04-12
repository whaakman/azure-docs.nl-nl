---
title: Architectuur van de verbinding voor een beheerd exemplaar in Azure SQL Database | Microsoft Docs
description: Meer informatie over Azure SQL Database managed instance-communicatie en connectiviteitsarchitectuur ook hoe de onderdelen van verkeer naar het beheerd exemplaar te regelen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: fasttrack-edit
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 02/26/2019
ms.openlocfilehash: 82b533f7293e00469a5b92b02e8d58967379a585
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59497063"
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

Beheerde exemplaren afhankelijk zijn van Azure-services zoals Azure Storage voor back-ups, Azure Event Hubs voor telemetrie, Azure Active Directory voor verificatie, Azure Key Vault voor transparante gegevensversleuteling (TDE) en een aantal Azure-platform-services die bieden functies voor beveiliging en ondersteuning. De beheerde exemplaren verbindingen maakt met deze services.

Alle communicatie worden versleuteld en ondertekend met behulp van certificaten. Om te controleren of de betrouwbaarheid van communicerende partijen, beheerde exemplaren voortdurend controleren of deze certificaten via certificaatintrekkingslijsten. Als de certificaten worden ingetrokken, sluit het beheerde exemplaar de verbindingen om de gegevens te beveiligen.

## <a name="high-level-connectivity-architecture"></a>Connectiviteitsarchitectuur op hoog niveau

Op hoog niveau is een beheerd exemplaar van een set met serviceonderdelen. Deze onderdelen worden gehost op een toegewezen set met geïsoleerde virtuele machines die worden uitgevoerd binnen het subnet van de klant virtuele netwerk. Deze machines vormen een virtueel cluster.

Een virtueel cluster kan meerdere beheerde exemplaren hosten. Indien nodig, wordt het cluster automatisch uitgebreid of opdrachten wanneer het aantal ingerichte exemplaren in het subnet van de klant wordt gewijzigd.

Toepassingen van klanten kunnen verbinding maken met beheerde exemplaren en kunnen opvragen en bijwerken databases binnen het virtuele netwerk, gekoppelde virtuele netwerk, of het netwerk verbonden via VPN of Azure ExpressRoute. Dit netwerk moet een eindpunt en een privé IP-adres gebruiken.  

![Architectuurdiagram van connectiviteit](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Services voor het beheer en de implementatie van Microsoft worden uitgevoerd buiten het virtuele netwerk. Een beheerd exemplaar en Microsoft-services verbinding maken via de eindpunten met openbare IP-adressen. Als een beheerd exemplaar maakt een uitgaande verbinding op de ontvangende kant NAT (Network Address Translation) wordt de verbinding eruit deze afkomstig van dit openbare IP-adres.

Beheer van verkeer stroomt via virtueel netwerk van de klant. Dit betekent dat dat elementen van de infrastructuur van het virtuele netwerk schadelijk voor beheer van verkeer zijn kunnen door te maken van het exemplaar mislukt en niet beschikbaar.

> [!IMPORTANT]
> Voor het verbeteren van klantervaring en beschikbaarheid van de service, past Microsoft een intentie netwerkbeleid op Azure virtual network-Infrastructuurelementen. Het beleid kan van invloed op de werking van het beheerde exemplaar. Dit mechanisme platform communiceert transparant netwerkvereisten voor gebruikers. Het belangrijkste doel van het beleid is om te voorkomen dat netwerk onjuiste configuratie en het zorgen voor beheerd exemplaar voor normale bewerkingen. Wanneer u een beheerd exemplaar verwijdert, wordt de intentie netwerkbeleid ook verwijderd.

## <a name="virtual-cluster-connectivity-architecture"></a>Virtueel cluster connectiviteitsarchitectuur

We gaan meer informatie over in de connectiviteitsarchitectuur van de voor beheerde exemplaren. Het volgende diagram toont de algemene indeling van de virtuele-cluster.

![Connectiviteitsarchitectuur van de virtuele-cluster](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Clients verbinding maken met een beheerd exemplaar met behulp van de naam van een host met het formulier `<mi_name>.<dns_zone>.database.windows.net`. Deze hostnaam wordt omgezet in een privé IP-adres, maar het geregistreerd in een openbare domein Name System (DNS)-zone en openbaar omgezette. De `zone-id` wordt automatisch gegenereerd bij het maken van het cluster. Als een nieuw cluster als host fungeert voor een secundaire beheerd exemplaar, wordt deze de zone-ID deelt met de primaire cluster. Zie voor meer informatie, [automatische failover-groepen gebruiken om transparante en gecoördineerd failover van meerdere databases mogelijk te](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets).

Dit privé IP-adres behoort tot het beheerde exemplaar van interne load balancer. De load balancer zorgt ervoor dat verkeer naar de gateway van het beheerde exemplaar. Omdat meerdere beheerde exemplaren kunnen worden uitgevoerd in hetzelfde cluster, de gateway maakt gebruik van de hostnaam van het beheerde exemplaar verkeer omleiden naar de juiste SQL-engine-service.

Beheer-en implementatie met een beheerd exemplaar te maken met een [beheereindpunt](#management-endpoint) dat is toegewezen aan een externe netwerktaakverdeler. Verkeer wordt doorgestuurd naar de knooppunten alleen als deze is ontvangen op een vooraf gedefinieerde set van poorten die alleen onderdelen voor het beheerde exemplaar gebruiken. Een ingebouwde firewall op de knooppunten wordt ingesteld om toe te staan alleen verkeer van Microsoft IP-bereiken. Certificaten alle communicatie tussen de onderdelen en het beheervlak wederzijds te verifiëren.

## <a name="management-endpoint"></a>Beheereindpunt

Microsoft beheert het beheerde exemplaar met behulp van een eindpunt. Dit eindpunt is in de virtuele cluster van het exemplaar. Het eindpunt is beveiligd door een ingebouwde firewall op niveau van het netwerk. Op het niveau van de toepassing, wordt deze beveiligd door wederzijdse certificaatverificatie. IP-adres van het eindpunt, Zie [vast IP-adres voor het beheereindpunt](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Wanneer verbindingen start in het beheerde exemplaar (net als bij back-ups en logboeken voor controle), verkeer wordt weergegeven om te starten vanaf het openbare IP-adres van het eindpunt. U kunt toegang beperken tot openbare services van een beheerd exemplaar door in te stellen van firewallregels om toe te staan alleen IP-adres van het beheerde exemplaar. Zie voor meer informatie, [controleren of de ingebouwde firewall van het beheerde exemplaar](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Traffice naar de Azure-services die binnen het gebied van het beheerde exemplaar is geoptimaliseerd en voor reden dan ook niet gestaan naar beheerde exemplaar management eindpunt openbare IP-adres. Om die reden als u wilt gebruiken op basis van IP-firewallregels, meestal voor opslag, service moet zich in een andere regio uit beheerde exemplaar.

## <a name="network-requirements"></a>Netwerkvereisten

Implementeer een beheerd exemplaar in een speciaal subnet binnen het virtuele netwerk. Het subnet moet beschikken over deze kenmerken:

- **Toegewezen subnet:** Subnet van het beheerde exemplaar kan niet een andere cloudservice die is gekoppeld aan deze bevatten en mag niet een gatewaysubnet. Het subnet mag niet een resource, maar het beheerde exemplaar en later kunt u resources in het subnet toevoegen.
- **Netwerkbeveiligingsgroep (NSG):** Een NSG die is gekoppeld aan het virtuele netwerk moet definiëren [inkomende beveiligingsregels](#mandatory-inbound-security-rules) en [uitgaande beveiligingsregels](#mandatory-outbound-security-rules) vóór alle andere regels. U kunt een NSG gebruiken voor het beheren van toegang tot het beheerde exemplaar gegevens eindpunt door te filteren van het verkeer op poort 1433 en poorten 11000-11999 bij het beheerde exemplaar is geconfigureerd voor omleiden van verbindingen.
- **Gebruiker gedefinieerde route (UDR) tabel:** Een UDR-tabel die is gekoppeld aan het virtuele netwerk moet bevatten specifieke [vermeldingen](#user-defined-routes).
- **Er is geen service-eindpunten:** Er is geen service-eindpunt moet worden gekoppeld aan het beheerde exemplaar subnet. Zorg ervoor dat de service-eindpunten-optie is uitgeschakeld bij het maken van het virtuele netwerk.
- **Voldoende IP-adressen:** Het subnet beheerd exemplaar moet ten minste 16 IP-adressen hebben. Het aanbevolen minimum is 32 IP-adressen. Zie voor meer informatie, [bepalen van de grootte van het subnet voor beheerde exemplaren](sql-database-managed-instance-determine-size-vnet-subnet.md). U kunt beheerde exemplaren in implementeren [het bestaande netwerk](sql-database-managed-instance-configure-vnet-subnet.md) nadat u hebt geconfigureerd om te voldoen aan [de netwerkvereisten voor beheerde exemplaren](#network-requirements). Anders maakt u een [nieuwe netwerk en subnet](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> U kunt een nieuwe beheerde exemplaar niet implementeren als het doelsubnet beschikt niet over deze kenmerken. Wanneer u een beheerd exemplaar maakt, wordt een intentie netwerkbeleid toegepast op het subnet om te voorkomen dat niet-compatibele wijzigingen tot het instellen van netwerken. Nadat het laatste exemplaar van het subnet is verwijderd, wordt de intentie netwerkbeleid ook verwijderd.

### <a name="mandatory-inbound-security-rules"></a>Verplichte beveiligingsregels voor binnenkomend verkeer

| Name       |Poort                        |Protocol|Bron           |Doel|Bewerking|
|------------|----------------------------|--------|-----------------|-----------|------|
|beheer  |9000, 9003, 1438, 1440, 1452|TCP     |Alle              |MI-SUBNET  |Toestaan |
|mi_subnet   |Alle                         |Alle     |MI-SUBNET        |MI-SUBNET  |Toestaan |
|health_probe|Alle                         |Alle     |AzureLoadBalancer|MI-SUBNET  |Toestaan |

### <a name="mandatory-outbound-security-rules"></a>Verplichte uitgaande beveiligingsregels

| Name       |Poort          |Protocol|Bron           |Doel|Bewerking|
|------------|--------------|--------|-----------------|-----------|------|
|beheer  |80, 443, 12000|TCP     |MI-SUBNET        |AzureCloud |Toestaan |
|mi_subnet   |Alle           |Alle     |MI-SUBNET        |MI-SUBNET  |Toestaan |

> [!IMPORTANT]
> Zorg ervoor dat er slechts één binnenkomende regel voor poorten 9000, 9003, 1438, 1440, 1452 en een uitgaande regel voor de poorten 80, 443, 12000. Beheerd exemplaar inrichten via Azure Resource Manager implementaties mislukken als er regels voor binnenkomende en uitvoer afzonderlijk voor elke poort zijn geconfigureerd. Als deze poorten in afzonderlijke regels worden, mislukt de implementatie met foutcode `VnetSubnetConflictWithIntendedPolicy`

\* MI-SUBNET verwijst naar het IP-adresbereik voor het subnet in het formulier 10.x.x.x/y. U kunt deze informatie vinden in de Azure-portal in de subneteigenschappen van het.

> [!IMPORTANT]
> Hoewel de vereiste beveiligingsregels voor binnenkomend verkeer toestaan verkeer van _eventuele_ bron op poorten 9000, 9003, 1438 1440 en 1452, deze poorten worden beveiligd door een ingebouwde firewall. Zie voor meer informatie, [bepalen de eindpuntadres management](sql-database-managed-instance-find-management-endpoint-ip-address.md).
> [!NOTE]
> Als u transactionele replicatie in een beheerd exemplaar gebruiken, en als u een exemplaar in de database als een publisher of een distributor, opent poort 445 (TCP uitgaand) in van het subnet beveiligingsregels voor verbindingen. Deze poort krijgt toegang tot de Azure-bestandsshare.

### <a name="user-defined-routes"></a>Door de gebruiker gedefinieerde routes

|Name|Adresvoorvoegsel|Volgende Hop|
|----|--------------|-------|
|subnet_to_vnetlocal|MI-SUBNET|Virtueel netwerk|
|mi-13-64-11-nexthop-internet|13.64.0.0/11|Internet|
|MI-13-96-13-nexthop-Internet|13.96.0.0/13|Internet|
|MI-13-104-14-nexthop-Internet|13.104.0.0/14|Internet|
|mi-20-8-nexthop-internet|20.0.0.0/8|Internet|
|mi-23-96-13-nexthop-internet|23.96.0.0/13|Internet|
|mi-40-64-10-nexthop-internet|40.64.0.0/10|Internet|
|MI-42-159-16-nexthop-Internet|42.159.0.0/16|Internet|
|mi-51-8-nexthop-internet|51.0.0.0/8|Internet|
|mi-52-8-nexthop-internet|52.0.0.0/8|Internet|
|MI-64-4-18-nexthop-Internet|64.4.0.0/18|Internet|
|mi-65-52-14-nexthop-internet|65.52.0.0/14|Internet|
|mi-66-119-144-20-nexthop-internet|66.119.144.0/20|Internet|
|mi-70-37-17-nexthop-internet|70.37.0.0/17|Internet|
|MI-70-37-128-18-nexthop-Internet|70.37.128.0/18|Internet|
|mi-91-190-216-21-nexthop-internet|91.190.216.0/21|Internet|
|MI-94-245-64-18-nexthop-Internet|94.245.64.0/18|Internet|
|mi-103-9-8-22-nexthop-internet|103.9.8.0/22|Internet|
|mi-103-25-156-22-nexthop-internet|103.25.156.0/22|Internet|
|mi-103-36-96-22-nexthop-internet|103.36.96.0/22|Internet|
|mi-103-255-140-22-nexthop-internet|103.255.140.0/22|Internet|
|MI-104-40-13-nexthop-Internet|104.40.0.0/13|Internet|
|MI-104-146-15-nexthop-Internet|104.146.0.0/15|Internet|
|MI-104-208-13-nexthop-Internet|104.208.0.0/13|Internet|
|MI-111-221-16-20-nexthop-Internet|111.221.16.0/20|Internet|
|MI-111-221-64-18-nexthop-Internet|111.221.64.0/18|Internet|
|MI-129-75-16-nexthop-Internet|129.75.0.0/16|Internet|
|mi-131-253-16-nexthop-internet|131.253.0.0/16|Internet|
|MI-132-245-16-nexthop-Internet|132.245.0.0/16|Internet|
|MI-134-170-16-nexthop-Internet|134.170.0.0/16|Internet|
|mi-134-177-16-nexthop-internet|134.177.0.0/16|Internet|
|MI-137-116-15-nexthop-Internet|137.116.0.0/15|Internet|
|MI-137-135-16-nexthop-Internet|137.135.0.0/16|Internet|
|MI-138-91-16-nexthop-Internet|138.91.0.0/16|Internet|
|MI-138-196-16-nexthop-Internet|138.196.0.0/16|Internet|
|MI-139-217-16-nexthop-Internet|139.217.0.0/16|Internet|
|MI-139-219-16-nexthop-Internet|139.219.0.0/16|Internet|
|MI-141-251-16-nexthop-Internet|141.251.0.0/16|Internet|
|MI-146-147-16-nexthop-Internet|146.147.0.0/16|Internet|
|mi-147-243-16-nexthop-internet|147.243.0.0/16|Internet|
|mi-150-171-16-nexthop-internet|150.171.0.0/16|Internet|
|mi-150-242-48-22-nexthop-internet|150.242.48.0/22|Internet|
|mi-157-54-15-nexthop-internet|157.54.0.0/15|Internet|
|mi-157-56-14-nexthop-internet|157.56.0.0/14|Internet|
|MI-157-60-16-nexthop-Internet|157.60.0.0/16|Internet|
|MI-167-220-16-nexthop-Internet|167.220.0.0/16|Internet|
|MI-168-61-16-nexthop-Internet|168.61.0.0/16|Internet|
|mi-168-62-15-nexthop-internet|168.62.0.0/15|Internet|
|mi-191-232-13-nexthop-internet|191.232.0.0/13|Internet|
|mi-192-32-16-nexthop-internet|192.32.0.0/16|Internet|
|mi-192-48-225-24-nexthop-internet|192.48.225.0/24|Internet|
|mi-192-84-159-24-nexthop-internet|192.84.159.0/24|Internet|
|mi-192-84-160-23-nexthop-internet|192.84.160.0/23|Internet|
|mi-192-100-102-24-nexthop-internet|192.100.102.0/24|Internet|
|mi-192-100-103-24-nexthop-internet|192.100.103.0/24|Internet|
|mi-192-197-157-24-nexthop-internet|192.197.157.0/24|Internet|
|mi-193-149-64-19-nexthop-internet|193.149.64.0/19|Internet|
|mi-193-221-113-24-nexthop-internet|193.221.113.0/24|Internet|
|mi-194-69-96-19-nexthop-internet|194.69.96.0/19|Internet|
|mi-194-110-197-24-nexthop-internet|194.110.197.0/24|Internet|
|mi-198-105-232-22-nexthop-internet|198.105.232.0/22|Internet|
|MI-198-200-130-24-nexthop-Internet|198.200.130.0/24|Internet|
|mi-198-206-164-24-nexthop-internet|198.206.164.0/24|Internet|
|MI-199-60-28-24-nexthop-Internet|199.60.28.0/24|Internet|
|mi-199-74-210-24-nexthop-internet|199.74.210.0/24|Internet|
|mi-199-103-90-23-nexthop-internet|199.103.90.0/23|Internet|
|MI-199-103-122-24-nexthop-Internet|199.103.122.0/24|Internet|
|mi-199-242-32-20-nexthop-internet|199.242.32.0/20|Internet|
|mi-199-242-48-21-nexthop-internet|199.242.48.0/21|Internet|
|mi-202-89-224-20-nexthop-internet|202.89.224.0/20|Internet|
|mi-204-13-120-21-nexthop-internet|204.13.120.0/21|Internet|
|mi-204-14-180-22-nexthop-internet|204.14.180.0/22|Internet|
|mi-204-79-135-24-nexthop-internet|204.79.135.0/24|Internet|
|mi-204-79-179-24-nexthop-internet|204.79.179.0/24|Internet|
|mi-204-79-181-24-nexthop-internet|204.79.181.0/24|Internet|
|mi-204-79-188-24-nexthop-internet|204.79.188.0/24|Internet|
|mi-204-79-195-24-nexthop-internet|204.79.195.0/24|Internet|
|mi-204-79-196-23-nexthop-internet|204.79.196.0/23|Internet|
|mi-204-79-252-24-nexthop-internet|204.79.252.0/24|Internet|
|mi-204-152-18-23-nexthop-internet|204.152.18.0/23|Internet|
|mi-204-152-140-23-nexthop-internet|204.152.140.0/23|Internet|
|mi-204-231-192-24-nexthop-internet|204.231.192.0/24|Internet|
|mi-204-231-194-23-nexthop-internet|204.231.194.0/23|Internet|
|mi-204-231-197-24-nexthop-internet|204.231.197.0/24|Internet|
|mi-204-231-198-23-nexthop-internet|204.231.198.0/23|Internet|
|mi-204-231-200-21-nexthop-internet|204.231.200.0/21|Internet|
|mi-204-231-208-20-nexthop-internet|204.231.208.0/20|Internet|
|mi-204-231-236-24-nexthop-internet|204.231.236.0/24|Internet|
|mi-205-174-224-20-nexthop-internet|205.174.224.0/20|Internet|
|mi-206-138-168-21-nexthop-internet|206.138.168.0/21|Internet|
|MI-206-191-224-19-nexthop-Internet|206.191.224.0/19|Internet|
|mi-207-46-16-nexthop-internet|207.46.0.0/16|Internet|
|MI-207-68-128-18-nexthop-Internet|207.68.128.0/18|Internet|
|mi-208-68-136-21-nexthop-internet|208.68.136.0/21|Internet|
|mi-208-76-44-22-nexthop-internet|208.76.44.0/22|Internet|
|mi-208-84-21-nexthop-internet|208.84.0.0/21|Internet|
|mi-209-240-192-19-nexthop-internet|209.240.192.0/19|Internet|
|MI-213-199-128-18-nexthop-Internet|213.199.128.0/18|Internet|
|mi-216-32-180-22-nexthop-internet|216.32.180.0/22|Internet|
|mi-216-220-208-20-nexthop-internet|216.220.208.0/20|Internet|
||||

U kunt bovendien vermeldingen toevoegen aan de routetabel voor het routeren van verkeer dat on-premises privé IP-adresbereiken als doel via de gateway van virtueel netwerk of een virtueel netwerkapparaat (NVA heeft).

Als het virtuele netwerk een aangepaste DNS-server bevat, de aangepaste DNS-server moet kunnen omzetten van hostnamen in \*. core.windows.net zone. Met behulp van aanvullende functies, zoals Azure AD-verificatie mogelijk extra FQDN's oplossen. Zie voor meer informatie, [instellen van een aangepaste DNS-server](sql-database-managed-instance-custom-dns.md).

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [geavanceerde beveiliging van gegevens van SQL-Database](sql-database-managed-instance.md).
- Meer informatie over het [instellen van een nieuwe Azure-netwerk](sql-database-managed-instance-create-vnet-subnet.md) of een [bestaande Azure-netwerk](sql-database-managed-instance-configure-vnet-subnet.md) waarin u beheerde exemplaren kunt implementeren.
- [De grootte van het subnet berekenen](sql-database-managed-instance-determine-size-vnet-subnet.md) waar u de beheerde exemplaren implementeren.
- Informatie over het maken van een beheerd exemplaar:
  - Uit de [Azure-portal](sql-database-managed-instance-get-started.md).
  - Met behulp van [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md).
  - Met behulp van [een Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/).
  - Met behulp van [een Azure Resource Manager-sjabloon (met JumpBox, met SSMS opgenomen)](https://portal.azure.com/). 
