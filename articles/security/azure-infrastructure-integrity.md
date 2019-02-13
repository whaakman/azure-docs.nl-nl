---
title: Integriteit van de Azure-infrastructuur
description: In dit artikel komen de integriteit van de Azure-infrastructuur.
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
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 24d54fa7a8985a6af58cddfc969b8023485c73c2
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104938"
---
# <a name="azure-infrastructure-integrity"></a>Integriteit van de Azure-infrastructuur

## <a name="software-installation"></a>Software-installatie
Alle onderdelen van de softwarestack die zijn geïnstalleerd in de Azure-omgeving zijn maat gemaakte na het proces van het Microsoft Security Development Lifecycle (SDL). Alle softwareonderdelen, met inbegrip van installatiekopieën van besturingssysteem (OS) en SQL-Database, worden geïmplementeerd als onderdeel van wijzigingsbeheer en release management-proces. Het besturingssysteem die wordt uitgevoerd op alle knooppunten is een aangepaste versie van Windows Server 2008 of Windows Server 2012. De exacte versie wordt gekozen door de infrastructuurcontroller (FC) op basis van de rol zal voor het besturingssysteem af te spelen. Bovendien is het hostbesturingssysteem niet toegestaan voor installatie van een niet-geautoriseerde softwareonderdelen.

Sommige Azure-onderdelen worden geïmplementeerd als Azure-klanten op een gast-VM wordt uitgevoerd op een gastbesturingssysteem.

## <a name="virus-scans-on-builds"></a>Virusscans op builds
Azure-software (inclusief OS) onderdeel builds hebben een virusscan die gebruikmaakt van de Endpoint Protection antivirusprogramma's hulpprogramma ondergaan. Elke VirusScan maakt een logbestand in de map van de bijbehorende build, met gedetailleerde informatie over wat is gescand en de resultaten van de scan. De VirusScan maakt deel uit van de build-broncode voor elk onderdeel van Azure. Code niet wordt verplaatst naar productie zonder scannen schoon en geslaagde virussen. Als u eventuele problemen worden vermeld, wordt de build is geblokkeerd en vervolgens gaat u naar de beveiligingsteams in Microsoft Security om te bepalen waar de build in de code 'rogue' hebt ingevoerd.

## <a name="closed-and-locked-environment"></a>Gesloten en vergrendeld omgeving
Standaard Azure-infrastructuurknooppunten en Gast-VM's geen gebruikersaccounts die zijn gemaakt op deze. Standaard Windows-administrator-accounts zijn bovendien ook uitgeschakeld. Beheerders van live Azure-ondersteuning kunnen, met de juiste verificatie, meld u aan bij deze machines en beheren van de-Azure-productienetwerk voor noodreparaties.

## <a name="azure-sql-database-authentication"></a>Azure SQL Database-verificatie
Net als bij elke uitvoering van SQL Server, beheer van gebruikersaccounts moet strikt worden beheerd. Azure SQL Database ondersteunt alleen SQL Server-verificatie. Als aanvulling op beveiligingsmodel voor gegevens van een klant, gebruiker van accounts met sterke wachtwoorden en geconfigureerd met specifieke rechten moeten ook worden gebruikt.

## <a name="acls-and-firewalls-between-the-microsoft-corporate-network-and-an-azure-cluster"></a>ACL's en firewalls tussen het bedrijfsnetwerk van Microsoft en een Azure-cluster
Toegangsbeheerlijsten (ACL's) en firewalls tussen het service-platform en het bedrijfsnetwerk van Microsoft SQL Database-exemplaren te beschermen tegen onbevoegde insider-toegang. Bovendien kunnen alleen gebruikers van IP-adresbereiken van het Microsoft-bedrijfsnetwerk toegang tot het eindpunt van de platform-management Windows Fabric.

## <a name="acls-and-firewalls-between-nodes-in-a-sql-database-cluster"></a>ACL's en firewalls tussen knooppunten in een cluster met SQL-Database
Als een extra beveiliging, als onderdeel van de verdediging in de diepte-strategie, hebben de ACL's en een firewall tussen knooppunten in een SQL-Database-cluster is geïmplementeerd. Alle communicatie binnen het cluster Windows Fabric-platform en alle actieve programmacode wordt vertrouwd.

## <a name="custom-monitoring-agents"></a>Aangepaste monitoring-agents
SQL-Database maakt gebruik van aangepaste bewakingsagents (MAs), ook wel genoemd watchdogs, voor het bewaken van de status van de SQL-Database-cluster.

## <a name="web-protocols"></a>Web-protocollen

### <a name="role-instance-monitoring-and-restart"></a>Rol van controle en opnieuw starten
Azure zorgt ervoor dat alle geïmplementeerd, met functies (internetgerichte web of verwerking van de back-end-werkrollen) zijn afhankelijk van continue statuscontrole om ervoor te zorgen dat ze effectief en efficiënt leveren van de services waarvoor ze hebt ingericht. Als een rol niet in orde, met een kritieke fout in de toepassing die wordt gehost of een probleem met de onderliggende configuratie binnen de rolinstantie zelf wordt, detecteert het probleem binnen de rolinstantie de FC- en initieert een corrigerende staat.

### <a name="compute-connectivity"></a>Rekenverbindingen
Azure zorgt ervoor dat de geïmplementeerde toepassing of service bereikbaar is via het web gebaseerde protocollen. Virtuele exemplaren van internet gerichte webrollen externe verbinding met internet hebben en bereikbaar zijn voor gebruikers van web direct. Ter bescherming van de vertrouwelijkheid en integriteit van de bewerkingen die werkrollen namens de openbaar toegankelijke virtuele exemplaren van de Webrol uitvoeren, virtuele instanties van de verwerking van de back-end-werkrollen externe verbinding met internet hebben, maar kunnen niet worden rechtstreeks geopend door externe gebruikers.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Azure-infrastructuur bewaken](azure-infrastructure-monitoring.md)
- [Azure-klant-gegevensbeveiliging](azure-protection-of-customer-data.md)
