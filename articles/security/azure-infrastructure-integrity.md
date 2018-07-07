---
title: Integriteit van de Azure-infrastructuur
description: In dit artikel komen de integriteit van de Azure-infrastructuur.
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
ms.date: 07/06/2018
ms.author: terrylan
ms.openlocfilehash: 867bc66a68bec662153d8336e649cf46df02f101
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37901313"
---
# <a name="azure-infrastructure-integrity"></a>Integriteit van de Azure-infrastructuur

## <a name="software-installation"></a>Software-installatie
Alle onderdelen van de softwarestack die zijn geïnstalleerd in de Azure-omgeving zijn maat gemaakte volgen van de Microsoft Security Development Lifecycle (SDL) proces. Alle software-onderdelen (met inbegrip van installatiekopieën van het besturingssysteem en SQL Database) worden geïmplementeerd als onderdeel van het proces wijzigings- en Releasebeheer. Het besturingssysteem die wordt uitgevoerd op alle knooppunten is een aangepaste versie van Windows Server 2008 of Windows Server 2012. De exacte versie wordt gekozen door de FC op basis van de rol zal voor het besturingssysteem af te spelen. Bovendien is het Hostbesturingssysteem niet toegestaan voor installatie van een niet-geautoriseerde softwareonderdelen.

Sommige onderdelen van Microsoft Azure (bijvoorbeeld RDFE, Developer-Portal, enzovoort) worden geïmplementeerd als Azure-klanten op de Gast-VM die wordt uitgevoerd op het Gastbesturingssysteem.

## <a name="virus-scans-on-builds"></a>Virusscans op Builds
Azure-software (inclusief OS) onderdeel builds hebben doorlopen een virusscan met het hulpprogramma voor het antivirusprogramma's van Microsoft Endpoint Protection (MEP). Elke VirusScan maakt een logbestand in de map van de bijbehorende build, met gedetailleerde informatie over wat is gescand en de resultaten van de scan. De VirusScan maakt deel uit van de build-broncode voor elk onderdeel van Azure. Code wordt niet worden verplaatst naar productie zonder scannen schoon en geslaagde virussen. Als er eventuele problemen die u hebt genoteerd, wordt de build is geblokkeerd en wordt vervolgens gaat u naar de teams beveiliging in Microsoft Security om te bepalen waar de build in de code 'rogue' hebt ingevoerd.

## <a name="closedlocked-environment"></a>Omgeving gesloten/vergrendeld
Standaard Azure-infrastructuurknooppunten en Gast-VM's geen alle gebruikersaccounts die op deze zijn gemaakt. Standaard Windows-administrator-accounts zijn bovendien ook uitgeschakeld. Beheerders van Microsoft Azure Live ondersteuning (WALS) kunnen – met de juiste verificatie-Meld u aan bij deze machines en de Azure-productienetwerk voor noodreparaties beheren.

## <a name="microsoft-azure-sql-database-authentication"></a>Microsoft Azure SQL Database-verificatie
Net als bij elke uitvoering van SQL Server, beheer van gebruikersaccounts moet strikt worden beheerd. Microsoft Azure SQL Database biedt alleen ondersteuning voor SQL Server-verificatie. Gebruiker van accounts met sterke wachtwoorden en geconfigureerd met specifieke rechten moeten ook worden gebruikt als aanvulling op beveiligingsmodel voor gegevens van de klant.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Firewall/ACL's tussen MSFT CorpNet en Microsoft Azure-Cluster
Microsoft Azure SQL Database beveiligen-ACL's / Firewall tussen het Service-Platform en MS-bedrijfsnetwerk tegen toegang door onbevoegden insider. Bovendien kunnen alleen gebruikers van IP-adresbereiken van Microsoft CorpNet toegang tot het beheereindpunt WinFabric-platform.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Firewall/ACL's tussen knooppunten in een Azure SQL DB-cluster
Als een extra beveiliging, als onderdeel van de verdediging in de diepte-strategie, zijn ACL's / Firewall tussen knooppunten in een Microsoft Azure SQL DB-cluster geïmplementeerd. Alle communicatie binnen het cluster WinFabric-platform en alle actieve programmacode wordt vertrouwd.

## <a name="custom-mas-watchdogs"></a>Aangepaste MAs (Watchdogs)
Microsoft Azure SQL Database maakt gebruik van aangepaste MAs watchdogs aangeroepen voor het bewaken van de status van de Microsoft Azure SQL DB-cluster.

## <a name="web-protocols"></a>Web-protocollen

### <a name="role-instance-monitoring-and-restart"></a>Rol van controle en opnieuw starten
Azure zorgt ervoor dat alle actieve rollen (internetgerichte web of verwerking van de back-end-werkrollen) geïmplementeerd zijn afhankelijk van de gezondheid van continue bewaking om ervoor te zorgen dat ze effectief en efficiënt leveren van de services waarin ze hebt ingericht. In het geval van een rol wordt niet in orde, met een kritieke fout in de toepassing wordt gehost of onderliggende configuratieprobleem binnen de rolinstantie zelf, wordt Microsoft Azure FC het probleem binnen de rolinstantie detecteren en te starten van een corrigerende status .

### <a name="compute-connectivity"></a>Rekenverbindingen
Azure zorgt ervoor dat de toepassing/service geïmplementeerd bereikbaar is via het web gebaseerde protocollen. Internetgerichte web virtuele instanties van de externe internetverbinding zal hebben en bereikbaar is rechtstreeks door gebruikers. Verwerking van de back-end virtuele werkrolinstanties externe verbinding met Internet hebben, maar kan niet rechtstreeks door de gebruiker van een externe website, worden geopend om u te beschermen van de vertrouwelijkheid en integriteit van de bewerkingen die werkrollen namens uitvoeren de openbaar toegankelijke virtuele exemplaren van de Webrol.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over wat Microsoft doet voor het beveiligen van de Azure-infrastructuur:

- [Azure faciliteiten, lokale en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Microsoft Azure SQL Database-beveiligingsfuncties](azure-infrastructure-sql.md)
- [-Azure-productiebewerkingen en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Beveiliging van klantgegevens in Azure](azure-protection-of-customer-data.md)
