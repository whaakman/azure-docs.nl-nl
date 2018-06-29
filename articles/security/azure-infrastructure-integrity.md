---
title: Integriteit van de Azure-infrastructuur
description: In dit artikel wordt de integriteit van de Azure-infrastructuur.
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
ms.openlocfilehash: 18d7fab30c0bbaa5292fe5d3003b7f2309b34d3b
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102316"
---
# <a name="integrity-of-azure-infrastructure"></a>Integriteit van de Azure-infrastructuur   

## <a name="software-installation"></a>Software-installatie
Alle onderdelen in de stack-software die zijn geïnstalleerd in de Azure-omgeving zijn maat gemaakte na het verwerken van de Microsoft Security Development Lifecycle (SDL). Alle software-onderdelen (zoals installatiekopieën van het besturingssysteem en SQL-Database) worden geïmplementeerd als onderdeel van het proces wijzigings- en Release Management. Het besturingssysteem dat wordt uitgevoerd op alle knooppunten is een aangepaste versie van Windows Server 2008 of Windows Server 2012. De exacte versie wordt gekozen door de FC volgens de rol die zij voor het besturingssysteem af te spelen. De Host-OS kan bovendien geen installatie van onderdelen ongeoorloofde software.

Sommige onderdelen van Microsoft Azure (bijvoorbeeld RDFE, Developer-Portal, enzovoort) worden geïmplementeerd als Azure-klanten in de Gast-VM uitgevoerd op het Gastbesturingssysteem.

## <a name="virus-scans-on-builds"></a>Virusscans op Builds
Azure software onderdeel (inclusief OS) builds moeten een hulpprogramma voor het Microsoft Endpoint Protection (MEP) antivirus VirusScan doorlopen. Elke VirusScan maakt een logbestand in de map gekoppelde build, met gedetailleerde informatie over wat is gescand en de resultaten van de scan. De VirusScan maakt deel uit van de broncode build voor elk onderdeel van Azure. Code wordt niet worden verplaatst naar productie zonder een schone en geslaagde virus scannen. Als er problemen vermeld, wordt de build is vastgezet en gaat vervolgens naar de Security-teams binnen Microsoft Security om te bepalen waar de build in de code 'rogue' worden ingevoerd.

## <a name="closedlocked-environment"></a>Omgeving gesloten/vergrendeld
Standaard Azure-infrastructuurknooppunten en Gast-VM's hebben geen alle gebruikersaccounts die zijn gemaakt op deze. Standaard Windows-administrator-accounts zijn bovendien ook uitgeschakeld. Beheerders van Microsoft Azure Live ondersteuning (WALS) kunnen – met de juiste verificatiegegevens – Meld u aan bij deze machines en het Azure-productienetwerk voor noodgevallen reparaties beheren.

## <a name="microsoft-azure-sql-database-authentication"></a>Microsoft Azure SQL Database-verificatie
Net als bij de implementatie van SQL Server-, Gebruikersaccountbeheer moet strikt worden beheerd. Microsoft Azure SQL Database ondersteunt alleen SQL Server-verificatie. Gebruiker van accounts met sterke wachtwoorden en geconfigureerd met specifieke rechten moeten ook worden gebruikt als aanvulling op het beveiligingsmodel van de gegevens van de klant.

## <a name="firewallacls-between-msft-corpnet-and-microsoft-azure-cluster"></a>Firewall/ACL's tussen MSFT CorpNet en Microsoft Azure-Cluster
Microsoft Azure SQL Database beveiligen-ACL's / Firewall tussen het Service-Platform en MS-bedrijfsnetwerk tegen toegang door onbevoegden insider. Bovendien wordt het eindpunt WinFabric platform management hebben toegang tot alleen gebruikers van IP-adresbereiken van Microsoft CorpNet.

## <a name="firewallacls-between-nodes-in-an-azure-sql-db-cluster"></a>Firewall/ACL's tussen knooppunten in een Azure SQL DB-cluster
Als een extra beveiliging, als onderdeel van de defense-in diepte-strategie, zijn ACL's / Firewall tussen knooppunten in een Microsoft Azure SQL DB-cluster geïmplementeerd. Alle communicatie binnen het cluster WinFabric-platform, evenals alle code uitgevoerd wordt vertrouwd.

## <a name="custom-mas-watchdogs"></a>Aangepaste MAs (Watchdogs)
Microsoft Azure SQL Database maakt gebruik van aangepaste MAs watchdogs aangeroepen voor het bewaken van de status van de Microsoft Azure SQL DB-cluster.

## <a name="web-protocols"></a>Web-protocollen

### <a name="role-instance-monitoring-and-restart"></a>Bewaking van de rol-exemplaar en opnieuw starten
Azure zorgt ervoor dat alle actieve rollen (internetgerichte webserver of verwerking van de back-end-werkrollen) geïmplementeerd zijn onderworpen aan volgehouden status controleren om ervoor te zorgen dat ze effectief en efficiënt leveren van de services waarin ze hebt ingericht. In het geval van een rol slecht functioneert, door een kritieke fout in de toepassing wordt gehost of onderliggende configuratieprobleem binnen de rolinstantie zelf, wordt Microsoft Azure FC het probleem binnen de rolinstantie detecteren en te starten van een corrigerende status .

### <a name="compute-connectivity"></a>Rekenuren voor connectiviteit
Azure zorgt ervoor dat de toepassing/service geïmplementeerd bereikbaar is via het web gebaseerde standaardprotocollen. Virtuele rolinstanties internetgerichte webserver extern verbinding met Internet heeft en bereikbaar is rechtstreeks door gebruikers. Verwerking van de back-end worker-rol virtuele instanties externe verbinding met Internet hebben maar niet toegankelijk rechtstreeks door de gebruiker van een externe website, ter bescherming van de gevoeligheid en de integriteit van de bewerkingen die werkrollen namens uitvoeren de de virtuele rolinstanties openbaar toegankelijke webserver.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over Microsoft biedt voor het beveiligen van de Azure-infrastructuur:

- [Azure opslagruimten, ruimten en fysieke beveiliging](azure-physical-security.md)
- [Beschikbaarheid van de Azure-infrastructuur](azure-infrastructure-availability.md)
- [Onderdelen van het systeem Azure informatie en grenzen](azure-infrastructure-components.md)
- [Architectuur van de Azure-netwerk](azure-infrastructure-network.md)
- [Azure productienetwerk](azure-production-network.md)
- [Beveiligingsfuncties van Microsoft Azure SQL Database](azure-infrastructure-sql.md)
- [Azure productie-uitvoering en beheer](azure-infrastructure-operations.md)
- [Bewaking van Azure-infrastructuur](azure-infrastructure-monitoring.md)
- [Beveiliging van gegevens van de klant in Azure](azure-protection-of-customer-data.md)
