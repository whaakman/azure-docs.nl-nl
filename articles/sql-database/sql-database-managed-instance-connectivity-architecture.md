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
ms.date: 08/16/2018
ms.openlocfilehash: 312425d3ea02d15a992b9a694f09cb2be73b6221
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161587"
---
# <a name="azure-sql-database-managed-instance-connectivity-architecture"></a>Azure SQL Database Managed Instance Connectiviteitsarchitectuur 

In dit artikel bevat de communicatie-overzicht van Azure SQL Database Managed Instance en connectiviteitsarchitectuur ook wordt uitgelegd hoe de verschillende onderdelen functie verkeer met het beheerde exemplaar.  

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
> Als u wilt de klant expirience en service-beschikbaarheid te verbeteren, past Microsoft netwerkbeleid kunt u lezen wat op virtuele Azure-netwerk Infrastructuurelementen die invloed kunnen zijn op Managed Instance werkt. Dit is een platform-mechanisme om te communiceren transparant netwerkvereisten voor eindgebruikers, met de belangrijkste doel te voorkomen dat netwerkconfiguratiefouten en ervoor zorgen dat normale bewerkingen van het beheerde exemplaar. Bij verwijdering van de Managed Instance kunt u lezen wat netwerkbeleid ook verwijderd. 

## <a name="virtual-cluster-connectivity-architecture"></a>Virtueel cluster connectiviteitsarchitectuur 

We gaan meer informatie over Managed Instance connectiviteit architectuur. Het volgende diagram toont de algemene indeling van de virtuele-cluster. 

![connectiviteit architectuur diagram virtueel cluster](./media/managed-instance-connectivity-architecture/connectivityarch003.png)

Clients verbinding maken met beheerd exemplaar met behulp van de naam van de host waaraan een formulier < mi_name >. <clusterid>. database.windows.net. Deze hostnaam wordt omgezet in privé-IP-adres, maar het is geregistreerd in de openbare DNS-zone en openbaar omgezette. 

Dit privé IP-adres behoort aan het beheerde exemplaar van interne Load Balancer (ILB) die verkeer naar het beheerd exemplaar Gateway (GW) stuurt. Als meerdere beheerde exemplaren kan mogelijk worden uitgevoerd in hetzelfde cluster, GW maakt gebruik van Managed Instance-hostnaam verkeer omleiden naar de juiste SQL-Engine-service. 

Beheer en de implementatie van services verbinden met beheerd exemplaar met behulp van openbare eindpunt dat is toegewezen aan de externe load balancer. Verkeer wordt doorgestuurd naar de knooppunten alleen als ontvangen op vooraf gedefinieerde een set van poorten die uitsluitend door beheerde exemplaar van de onderdelen worden gebruikt. Alle communicatie tussen de onderdelen en de beheerlaag is sluiten elkaar wederzijds geverifieerd certificaat. 

## <a name="next-steps"></a>Volgende stappen 

- Zie voor een overzicht [wat is een beheerd exemplaar](sql-database-managed-instance.md) 
- Zie voor meer informatie over VNet-configuratie, [beheerd exemplaar VNet-configuratie](sql-database-managed-instance-vnet-configuration.md). 
- Zie voor een snelstartgids over het maken van Managed Instance: 
  - Uit de [Azure-portal](sql-database-managed-instance-get-started.md) 
  - met behulp van [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/) 
  - met behulp van [Azure Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/101-sqlmi-new-vnet/) 
  - met behulp van [Azure Resource Manager-sjabloon (jumpbox met SSMS opgenomen)](https://portal.azure.com/) 

