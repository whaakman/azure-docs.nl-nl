---
title: Azure SQL Database managed instance connectiviteitsarchitectuur | Microsoft Docs
description: Dit artikel bevat de Azure SQL Database managed instance communicatieoverzicht en connectiviteitsarchitectuur ook wordt uitgelegd hoe de verschillende onderdelen functie voor het verkeer naar het beheerde exemplaar.
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
ms.date: 02/18/2019
ms.openlocfilehash: 70206fac7bfe34ea1a138437db35720a1f02337d
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56585365"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Azure SQL Database managed instance connectiviteitsarchitectuur

Dit artikel bevat de Azure SQL Database managed instance communicatieoverzicht en connectiviteitsarchitectuur ook wordt uitgelegd hoe de verschillende onderdelen functie voor het verkeer naar het beheerde exemplaar.  

Het beheerde exemplaar van Azure SQL Database wordt in Azure VNet en het subnet dat is toegewezen aan beheerde exemplaren geplaatst. Deze implementatie kunt de volgende scenario's:

- Beveilig privé IP-adres.
- Verbinding maken met een beheerd exemplaar rechtstreeks vanuit een on-premises netwerk.
- Een beheerd exemplaar verbinding te maken met gekoppelde server of een andere on-premises gegevensarchief.
- Verbinding maken met een beheerd exemplaar van Azure-resources.

## <a name="communication-overview"></a>Overzicht servicecommunicatie

Het volgende diagram toont de entiteiten die verbinding met beheerd exemplaar maken, evenals de resources die worden beheerd exemplaar moet contact met u opnemen om u te laten functioneren.

![connectiviteit architectuur entiteiten](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Communicatie die wordt weergegeven aan de onderkant van het diagram geeft toepassingen van klanten en hulpprogramma's verbinding maken met beheerde instantie als gegevensbron.  

Beheerd exemplaar is een platform-as-a-services (PaaS) aanbieding, beheert Microsoft deze service met behulp van geautomatiseerde agents (beheer, implementatie en onderhoud) op basis van telemetrie-gegevensstromen. Als beheerde instantiebeheer is uitsluitend de verantwoordelijkheid van Microsoft, klanten zijn niet in staat is toegang tot de beheerde exemplaar cluster virtuele machines via RDP.

Bepaalde bewerkingen, gestart door de gebruikers of toepassingen mogelijk SQL-Server beheerd exemplaren om te communiceren met het platform. Een voorbeeld is het maken van een beheerd exemplaar van database - een resource die wordt weergegeven via de Azure-portal, PowerShell, Azure CLI en de REST-API.

Beheerd exemplaar is afhankelijk van andere Azure-Services voor de goede werking ervan (zoals Azure Storage voor back-ups, Azure Service Bus voor telemetrie, Azure AD voor de verificatie van Azure Key Vault voor TDE, enzovoort) en start de verbindingen met deze dienovereenkomstig aan.

Alle communicatie, hierboven, worden versleuteld en ondertekend met behulp van certificaten. Om ervoor te zorgen dat communicatie partijen vertrouwd worden, controleert beheerd exemplaar voortdurend deze certificaten of neem contact op met de certificeringsinstantie. Als de certificaten worden ingetrokken of beheerd exemplaar niet ze verifiëren kan, sluit deze de verbindingen om de gegevens te beveiligen.

## <a name="high-level-connectivity-architecture"></a>Connectiviteitsarchitectuur op hoog niveau

Op hoog niveau is een beheerd exemplaar van een set van de onderdelen van de service, die worden gehost op een toegewezen set met geïsoleerde virtuele machines die worden uitgevoerd binnen een subnet voor het virtueel netwerk van de klant en een virtueel cluster vormen.

Meerdere beheerde exemplaren kunnen worden gehost in één virtueel cluster. Het cluster wordt automatisch uitgebreid of maak indien nodig wanneer het aantal ingerichte exemplaren in het subnet van de klant wordt gewijzigd.

Toepassingen van klanten kunnen verbinding maken met beheerde exemplaren, query- en databases alleen als ze worden uitgevoerd binnen het virtuele netwerk of in een gekoppeld virtueel netwerk of VPN / Expressroute verbonden netwerk met behulp van eindpunt met particuliere IP-adres.  

![Architectuurdiagram van connectiviteit](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Services voor het beheer en de implementatie van Microsoft worden uitgevoerd buiten het virtuele netwerk, zodat de verbindingen tussen een beheerd exemplaar en Microsoft-services gaat via de eindpunten met openbare IP-adressen. Wanneer beheerd exemplaar maakt een uitgaande verbinding, bij ontvangst van end lijkt het alsof deze afkomstig van dit openbare IP-adres vanwege NAT (Network Address Translation).

Beheer van verkeer stroomt via het virtuele netwerk van de klant. Dat betekent dat van invloed op elementen van de infrastructuur van het virtuele netwerk en kan mogelijk beheerverkeer schade veroorzaken exemplaar beschadigde status heeft en niet beschikbaar.

> [!IMPORTANT]
> Voor het verbeteren van klantervaring en beschikbaarheid van de service, past Microsoft bedoeling netwerkbeleid op virtuele Azure-netwerk Infrastructuurelementen die invloed kunnen zijn op beheerde exemplaar werkt. Dit is een platform-mechanisme om te communiceren transparant netwerkvereisten voor eindgebruikers, met de belangrijkste doel om te voorkomen netwerkconfiguratiefouten en zorgen voor beheerd exemplaar voor normale bewerkingen. Bij het verwijderen van beheerde instantie netwerkbeleid kunt u lezen wat ook verwijderd.

## <a name="virtual-cluster-connectivity-architecture"></a>Virtueel cluster connectiviteitsarchitectuur

We gaan meer informatie over in connectiviteitsarchitectuur beheerd exemplaar. Het volgende diagram toont de algemene indeling van de virtuele-cluster.

![connectiviteit architectuur diagram virtueel cluster](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Clients verbinding maken met beheerd exemplaar met behulp van de naam van de host met een formulier `<mi_name>.<dns_zone>.database.windows.net`. Deze hostnaam wordt omgezet in privé-IP-adres, maar het is geregistreerd in de openbare DNS-zone en openbaar omgezette. De `zone-id` automatisch wordt gegenereerd wanneer het cluster is gemaakt. Als een nieuw cluster een secundaire beheerd exemplaar host is, wordt deze de zone-ID deelt met de primaire-cluster. Zie voor meer informatie, [automatische failover-groepen](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets)

Dit privé IP-adres behoort tot het beheerde exemplaar van interne load balancer (ILB) die verkeer doorgestuurd naar de gateway beheerd exemplaar (GW). Als meerdere beheerde exemplaren kunnen mogelijk worden uitgevoerd in hetzelfde cluster, de GW maakt gebruik van de naam van het beheerde exemplaar host verkeer omleiden naar de juiste SQL-Engine-service.

Beheer en de implementatie van services verbinding maken met een beheerd exemplaar met een [beheereindpunt](#management-endpoint) dat is toegewezen aan een externe netwerktaakverdeler. Verkeer wordt doorgestuurd naar de knooppunten alleen als ontvangen op een vooraf gedefinieerde set van poorten die uitsluitend door de onderdelen van de beheerde instantie worden gebruikt. Ingebouwde firewall op de knooppunten is geconfigureerd, zodat alleen verkeer van specifieke IP-adresbereiken van Microsoft. Alle communicatie tussen de onderdelen en de beheerlaag is sluiten elkaar wederzijds geverifieerd certificaat.

## <a name="management-endpoint"></a>Beheereindpunt

Het beheerde exemplaar virtuele cluster bevat een beheereindpunt die gebruikmaakt van Microsoft voor het beheren van het beheerde exemplaar. Het eindpunt is beveiligd met ingebouwde firewall netwerk certificaat niveau en wederzijdse verificatie op toepassingsniveau. U kunt [management eindpunt ip-adres vinden](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Wanneer verbindingen geïnitieerd worden vanuit binnen het beheerde exemplaar (back-up, auditlogboek) wordt weergegeven dat verkeer afkomstig is uit het beheer van eindpunt openbare IP-adres. U kunt toegang beperken tot openbare services van een beheerd exemplaar door in te stellen van firewall-regels waarmee het beheerde exemplaar IP-adres alleen. Meer informatie over de methode die u kunt [controleren of de ingebouwde firewall voor beheerd exemplaar](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Dit is niet van toepassing op het instellen van de firewallregels voor Azure-services die zich in dezelfde regio als het beheerde exemplaar als het Azure-platform heeft een optimalisatie voor het verkeer dat wordt verstuurd tussen de services die zijn geplaatst.

## <a name="network-requirements"></a>Netwerkvereisten

U implementeert een beheerd exemplaar in een speciaal subnet (het subnet van een beheerd exemplaar) binnen het virtuele netwerk dat aan de volgende vereisten voldoet:

- **Toegewezen subnet**: Het beheerde exemplaar subnet mag geen andere cloud-services die zijn gekoppeld aan deze bevatten en mag geen een gatewaysubnet. Kunt u zich niet aan een beheerd exemplaar maken in een subnet dat resources dan het beheerde exemplaar bevat, en u de andere resources in het subnet niet op de later kunt toevoegen.
- **Netwerkbeveiligingsgroep (NSG)**: Een NSG die is gekoppeld aan het virtuele netwerk mag deze gedefinieerde verplicht [inkomende beveiligingsregels](#mandatory-inbound-security-rules) en [uitgaande beveiligingsregels](#mandatory-outbound-security-rules) vóór alle andere regels. U kunt een NSG gebruiken voor het beheren van toegang tot het eindpunt van de gegevens beheerd exemplaar volledig door te filteren van het verkeer op poort 1433.
- **Tabel van de gebruiker gedefinieerde route (UDR)**: Een gebruiker gedefinieerde routetabel die zijn gekoppeld aan het virtuele netwerk moet zijn deze [vermeldingen](#user-defined-routes) in een door de gebruiker gedefinieerde routetabel.
- **Er is geen service-eindpunten**: Het subnet beheerd exemplaar moet een service-eindpunt dat is gekoppeld aan deze niet hebben. Zorg ervoor dat de service-eindpunten-optie is uitgeschakeld bij het maken van het virtuele netwerk.
- **Voldoende IP-adressen**: Het beheerde exemplaar subnet moet de minimumwaarde van 16 IP-adressen (aanbevolen minimum is 32 IP-adressen). Zie voor meer informatie, [bepalen van de grootte van het subnet voor beheerde exemplaren](sql-database-managed-instance-determine-size-vnet-subnet.md). U kunt beheerde exemplaren in implementeren [het bestaande netwerk](sql-database-managed-instance-configure-vnet-subnet.md) zodra u deze om te voldoen aan configureren [beheerd exemplaar netwerkvereisten](#network-requirements), of maak een [nieuwe netwerk en subnet](sql-database-managed-instance-create-vnet-subnet.md).

> [!IMPORTANT]
> Kunt u zich niet aan het implementeren van een nieuwe beheerd exemplaar als het doelsubnet niet compatibel met alle van de volgende vereisten is. Als u een beheerd exemplaar maakt, een *bedoeling netwerkbeleid* wordt toegepast op het subnet om te voorkomen dat niet-compatibele wijzigingen aan de configuratie van netwerken. Nadat het laatste exemplaar is verwijderd uit het subnet, de *bedoeling netwerkbeleid* ook is verwijderd

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

\* MI-SUBNET verwijst naar het IP-adresbereik voor het subnet in het formulier 10.x.x.x/y. Deze informatie kan worden gevonden in de Azure-portal (via de subneteigenschappen).

> [!IMPORTANT]
> Hoewel de verplichte beveiligingsregels voor binnenkomend verkeer toestaan verkeer van _eventuele_ bron op poorten 9000, 9003, 1438, 1440, 1452 deze poorten worden beveiligd door ingebouwde firewall. Dit [artikel](sql-database-managed-instance-find-management-endpoint-ip-address.md) laat zien hoe u IP-adres voor beheer-eindpunt detecteren en controleer of de firewall-regels.
> [!NOTE]
> Als u transactionele replicatie in een beheerd exemplaar gebruikt en een exemplaar in de database wordt gebruikt als een publisher of een distributor, moet poort 445 (TCP uitgaand) ook worden geopend in de regels van het subnet voor toegang tot de Azure-bestandsshare.

### <a name="user-defined-routes"></a>Door de gebruiker gedefinieerde routes

|Name|Adresvoorvoegsel|NET Hop|
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

U kunt bovendien vermeldingen toevoegen de tabel routes om verkeer te routeren met on-premises privé IP-adresbereiken als een doel via de gateway van virtueel netwerk of een virtueel netwerkapparaat (NVA).

- **Optionele aangepaste DNS**: Als een aangepaste DNS-server is opgegeven in het virtuele netwerk, moet de recursieve naamomzetting IP-adres (zoals 168.63.129.16) van Azure worden toegevoegd aan de lijst. Zie voor meer informatie, [configureren van aangepaste DNS](sql-database-managed-instance-custom-dns.md). De aangepaste DNS-server moeten kunnen omzetten van hostnamen in de volgende domeinen bevinden en hun subdomeinen: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, en *microsoftonline-p.com*.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is er een beheerd exemplaar](sql-database-managed-instance.md)
- Meer informatie over het [nieuwe VNet configureren](sql-database-managed-instance-create-vnet-subnet.md) of [bestaande VNet configureren](sql-database-managed-instance-configure-vnet-subnet.md) waarin u beheerde exemplaren kunt implementeren.
- [Berekenen van de grootte van het subnet](sql-database-managed-instance-determine-size-vnet-subnet.md) waar u de beheerde exemplaren implementeren.
- Zie voor snelstartgidsen, over het maken van beheerde exemplaar:
  - Uit de [Azure-portal](sql-database-managed-instance-get-started.md)
  - met behulp van [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - met behulp van [Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - met behulp van [Azure Resource Manager-sjabloon (jumpbox met SSMS opgenomen)](https://portal.azure.com/)
