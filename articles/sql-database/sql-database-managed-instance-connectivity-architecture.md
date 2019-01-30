---
title: Azure SQL Database Managed Instance Connectiviteitsarchitectuur | Microsoft Docs
description: In dit artikel bevat de communicatie-overzicht van Azure SQL Database Managed Instance en connectiviteitsarchitectuur ook wordt uitgelegd hoe de verschillende onderdelen functie verkeer met het beheerde exemplaar.
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
ms.date: 12/10/2018
ms.openlocfilehash: b709bbacce23a89b8c60b77a524018b50ca1ca5e
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245664"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Azure SQL Database Managed Instance Connectiviteitsarchitectuur

In dit artikel bevat de communicatie-overzicht van Azure SQL Database Managed Instance en connectiviteitsarchitectuur ook wordt uitgelegd hoe de verschillende onderdelen functie verkeer met het beheerde exemplaar.  

De Azure SQL Database Managed Instance wordt geplaatst in een Azure VNet en het subnet die speciaal voor beheerde instanties. Deze implementatie kunt de volgende scenario's: 
- Beveilig privé IP-adres.
- Verbinding maken met een beheerd exemplaar rechtstreeks vanuit een on-premises netwerk.
- Verbinding te maken van een beheerd exemplaar met gekoppelde server of een andere on-premises gegevensarchief.
- Verbinding maken met een beheerd exemplaar voor Azure-resources.

## <a name="communication-overview"></a>Overzicht servicecommunicatie

Het volgende diagram toont de entiteiten die verbinding met de Managed Instance, evenals de resources die beheerd exemplaar maken moet contact met u opnemen om u te laten functioneren.

![connectiviteit architectuur entiteiten](./media/managed-instance-connectivity-architecture/connectivityarch001.png)

Communicatie die wordt weergegeven aan de onderkant van het diagram geeft toepassingen van klanten en hulpprogramma's verbinding maken met Managed Instance als gegevensbron.  

Managed Instance is een platform-as-a-services (PaaS) beheert aanbieding, Microsoft deze service met behulp van geautomatiseerde agents (beheer, implementatie en onderhoud) op basis van telemetrie-gegevensstromen. Klanten zijn niet beheerd exemplaar cluster virtuele machines via RDP toegang tot Managed Instance management is uitsluitend verantwoordelijk voor Microsoft.

Sommige SQL Server bewerkingen, gestart door de gebruikers of toepassingen mogelijk Managed Instance om te communiceren met het platform. Een voorbeeld is het maken van een beheerd exemplaar van database - een resource die wordt weergegeven via de portal, PowerShell en Azure CLI.

Beheerd exemplaar is afhankelijk van andere Azure-Services voor de goede werking ervan (zoals Azure Storage voor back-ups, Azure Service Bus voor telemetrie, Azure AD voor de verificatie van Azure Key Vault voor TDE, enzovoort) en start de verbindingen met deze dienovereenkomstig aan.

Alle communicatie, hierboven, worden versleuteld en ondertekend met behulp van certificaten. Om ervoor te zorgen dat communicatie partijen vertrouwd worden, controleert Managed Instance voortdurend deze certificaten of neem contact op met de certificeringsinstantie. Als de certificaten worden ingetrokken of beheerd exemplaar niet ze verifiëren kan, sluit deze de verbindingen om de gegevens te beveiligen.

## <a name="high-level-connectivity-architecture"></a>Connectiviteitsarchitectuur op hoog niveau

Op hoog niveau is Managed Instance een set van de onderdelen van de service, die worden gehost op een toegewezen set met geïsoleerde virtuele machines die worden uitgevoerd binnen een subnet voor het virtueel netwerk van de klant en een virtueel cluster vormen.

Meerdere beheerde exemplaren kan worden gehost in één virtueel cluster. Het cluster wordt automatisch uitgebreid of maak indien nodig wanneer het aantal ingerichte exemplaren in het subnet van de klant wordt gewijzigd.

Toepassingen van klanten kunnen verbinding maken met databases met beheerd exemplaar, query- en alleen als ze worden uitgevoerd binnen het virtuele netwerk of gekoppelde virtuele netwerk of VPN / Expressroute verbonden netwerk met behulp van eindpunt met particuliere IP-adres.  

![Architectuurdiagram van connectiviteit](./media/managed-instance-connectivity-architecture/connectivityarch002.png)

Services voor het beheer en de implementatie van Microsoft worden uitgevoerd buiten het virtuele netwerk, zodat de verbinding tussen de Managed Instance en Microsoft-services gaat via de eindpunten met openbare IP-adressen. Als u Managed Instance maakt uitgaande verbindingen, op de ontvangende kant lijkt het erop deze afkomstig van dit openbare IP-adres vanwege NAT (Network Address Translation).

Beheer van verkeer stroomt via het virtuele netwerk van de klant. Dat betekent dat van invloed op elementen van de infrastructuur van het virtuele netwerk en kan mogelijk beheerverkeer schade veroorzaken exemplaar beschadigde status heeft en niet beschikbaar.

> [!IMPORTANT]
> Voor het verbeteren van klantervaring en beschikbaarheid van de service, past Microsoft bedoeling netwerkbeleid op virtuele Azure-netwerk Infrastructuurelementen die invloed kunnen zijn op Managed Instance werkt. Dit is een platform-mechanisme om te communiceren transparant netwerkvereisten voor eindgebruikers, met de belangrijkste doel te voorkomen dat netwerkconfiguratiefouten en ervoor zorgen dat normale bewerkingen van het beheerde exemplaar. Bij verwijdering van de Managed Instance kunt u lezen wat netwerkbeleid ook verwijderd.

## <a name="virtual-cluster-connectivity-architecture"></a>Virtueel cluster connectiviteitsarchitectuur

We gaan meer informatie over Managed Instance connectiviteit architectuur. Het volgende diagram toont de algemene indeling van de virtuele-cluster.

![connectiviteit architectuur diagram virtueel cluster](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Clients verbinding maken met beheerd exemplaar met behulp van de naam van de host met een formulier `<mi_name>.<dns_zone>.database.windows.net`. Deze hostnaam wordt omgezet in privé-IP-adres, maar het is geregistreerd in de openbare DNS-zone en openbaar omgezette. De `zone-id` automatisch wordt gegenereerd wanneer het cluster is gemaakt. Als een nieuw cluster een secundaire beheerd exemplaar host is, wordt deze de zone-ID deelt met de primaire-cluster. Zie voor meer informatie, [automatische failover-groepen](sql-database-auto-failover-group.md##enabling-geo-replication-between-managed-instances-and-their-vnets)

Dit privé IP-adres behoort aan het beheerde exemplaar van interne Load Balancer (ILB) die verkeer naar het beheerd exemplaar Gateway (GW) stuurt. Als meerdere beheerde exemplaren kan mogelijk worden uitgevoerd in hetzelfde cluster, GW maakt gebruik van Managed Instance-hostnaam verkeer omleiden naar de juiste SQL-Engine-service.

Beheer en de implementatie services verbinding maken met behulp van Managed Instance [beheereindpunt](#management-endpoint) die wordt toegewezen aan de externe load balancer. Verkeer wordt doorgestuurd naar de knooppunten alleen als ontvangen op een vooraf gedefinieerde set van poorten die uitsluitend door beheerde exemplaar van de onderdelen worden gebruikt. Ingebouwde firewall op de knooppunten is geconfigureerd, zodat alleen verkeer van specifieke IP-adresbereiken van Microsoft. Alle communicatie tussen de onderdelen en de beheerlaag is sluiten elkaar wederzijds geverifieerd certificaat.

## <a name="management-endpoint"></a>Beheereindpunt

De virtuele Azure SQL Database Managed Instance-cluster bevat een beheereindpunt die gebruikmaakt van Microsoft voor het beheren van het beheerde exemplaar. Het eindpunt is beveiligd met ingebouwde firewall netwerk certificaat niveau en wederzijdse verificatie op toepassingsniveau. U kunt [management eindpunt ip-adres vinden](sql-database-managed-instance-find-management-endpoint-ip-address.md).

Wanneer verbindingen geïnitieerd worden vanuit binnen het beheerde exemplaar (back-up, auditlogboek) wordt weergegeven dat verkeer afkomstig is uit het beheer van eindpunt openbare IP-adres. U kunt toegang beperken tot openbare services van Managed Instance door in te stellen van firewallregels om toe te staan alleen het beheerd exemplaar IP-adres. Meer informatie over de methode die u kunt [controleren of de ingebouwde Managed Instance-firewall](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md).

> [!NOTE]
> Dit is niet van toepassing op het instellen van de firewallregels voor Azure-services die zich in dezelfde regio als beheerd exemplaar als het Azure-platform heeft een optimalisatie voor het verkeer dat wordt verstuurd tussen de services die zijn geplaatst.

## <a name="network-requirements"></a>Netwerkvereisten

Managed Instance kunt u implementeren in een speciaal subnet (de Managed Instance-subnet) binnen het virtuele netwerk dat aan de volgende vereisten voldoet:
- **Toegewezen subnet**: De Managed Instance-subnet mag geen andere cloud-services die zijn gekoppeld aan deze bevatten en mag geen een gatewaysubnet. Kunt u zich niet aan een beheerd exemplaar maakt in een subnet met andere resources dan Managed Instance en u de andere resources in het subnet niet op de later kunt toevoegen.
- **De Netwerkbeveiligingsgroep (NSG) compatibele**: Een NSG die is gekoppeld aan een Managed Instance-subnet moet regels die wordt weergegeven in de volgende tabellen (verplichte beveiligingsregels voor binnenkomend verkeer en verplichte uitgaande beveiligingsregels) voor alle andere regels bevatten. U kunt een NSG gebruiken voor het beheren van toegang tot het eindpunt van de gegevens Managed Instance volledig door te filteren van het verkeer op poort 1433. 
- **Tabel compatibel is door de gebruiker gedefinieerde route (UDR)**: De Managed Instance-subnet moet de routetabel van een gebruiker met **0.0.0.0/0 Next Hop Internet** als de verplichte UDR zijn toegewezen. Bovendien kunt u een UDR dat verkeer van routes met on-premises privé IP-adresbereiken als een doel via de gateway van virtueel netwerk of een virtueel netwerkapparaat (NVA) toevoegen. 
- **Optionele aangepaste DNS**: Als een aangepaste DNS-server is opgegeven in het virtuele netwerk, moet de recursieve naamomzetting IP-adres (zoals 168.63.129.16) van Azure worden toegevoegd aan de lijst. Zie voor meer informatie, [configureren van aangepaste DNS](sql-database-managed-instance-custom-dns.md). De aangepaste DNS-server moeten kunnen omzetten van hostnamen in de volgende domeinen bevinden en hun subdomeinen: *microsoft.com*, *windows.net*, *windows.com*, *msocsp.com*, *digicert.com*, *live.com*, *microsoftonline.com*, en *microsoftonline-p.com*. 
- **Er is geen service-eindpunten**: De Managed Instance-subnet moet een service-eindpunt dat is gekoppeld aan deze niet hebben. Zorg ervoor dat de service-eindpunten-optie is uitgeschakeld bij het maken van het virtuele netwerk.
- **Voldoende IP-adressen**: De Managed Instance-subnet moet de minimumwaarde van 16 IP-adressen (aanbevolen minimum is 32 IP-adressen). Zie voor meer informatie, [bepaalt de grootte van het subnet voor beheerde instanties](sql-database-managed-instance-determine-size-vnet-subnet.md). U kunt beheerde instanties in implementeren [het bestaande netwerk](sql-database-managed-instance-configure-vnet-subnet.md) zodra u deze om te voldoen aan configureren [Managed Instance netwerkvereisten](#network-requirements), of maak een [nieuwe netwerk en subnet](sql-database-managed-instance-create-vnet-subnet.md).

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
|mi_subnet   |Alle           |Alle     |Alle              |MI-SUBNET  |Toestaan |

  > [!Note]
  > MI-SUBNET verwijst naar het IP-adresbereik voor het subnet in het formulier 10.x.x.x/y. Deze informatie kan worden gevonden in de Azure-portal (via de subneteigenschappen).
  
  > [!Note]
  > Hoewel de verplichte beveiligingsregels voor binnenkomend verkeer toestaan verkeer van _eventuele_ bron op poorten 9000, 9003, 1438, 1440, 1452 deze poorten worden beveiligd door ingebouwde firewall. Dit [artikel](sql-database-managed-instance-find-management-endpoint-ip-address.md) laat zien hoe u IP-adres voor beheer-eindpunt detecteren en controleer of de firewall-regels. 
  
  > [!Note]
  > Als u transactionele replicatie worden gebruikt in het beheerde exemplaar en elke database in het beheerde exemplaar wordt gebruikt als publisher of distributor, moet poort 445 (TCP uitgaand) ook worden geopend in de regels van het subnet voor toegang tot de Azure-bestandsshare.
  
## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md)
- Meer informatie over het [nieuwe VNet configureren](sql-database-managed-instance-create-vnet-subnet.md) of [bestaande VNet configureren](sql-database-managed-instance-configure-vnet-subnet.md) waarin u beheerde instanties kunt implementeren.
- [Berekenen van de grootte van het subnet](sql-database-managed-instance-determine-size-vnet-subnet.md) waar u beheerde instanties implementeren. 
- Zie voor een snelstartgids over het maken van Managed Instance:
  - Uit de [Azure-portal](sql-database-managed-instance-get-started.md)
  - met behulp van [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
  - met behulp van [Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/)
  - met behulp van [Azure Resource Manager-sjabloon (jumpbox met SSMS opgenomen)](https://portal.azure.com/)
