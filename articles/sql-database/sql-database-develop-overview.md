---
title: Overzicht van de ontwikkeling van toepassingen voor SQL Database | Microsoft Docs
description: Kom meer te weten over beschikbare verbindingsbibliotheken en aanbevolen procedures voor toepassingen die verbinding maken met SQL Database.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: genemi
ms.assetid: 67c02204-d1bd-4622-acce-92115a7cde03
ms.service: sql-database
ms.custom: develop apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2016
ms.author: sstein
ms.openlocfilehash: 5948db9a52dc24d75f3fecc4ed166dd327061b37
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="sql-database-application-development-overview"></a>SQL-Database ontwikkelen-overzicht
In dit artikel leest u meer over de algemene zaken waar ontwikkelaars van op de hoogte moeten zijn voor het schrijven van code om verbinding te maken met Azure SQL Database.

> [!TIP]
> Zie de [Zelfstudie Aan de slag](sql-database-get-started-portal.md) voor een zelfstudie over het maken van een server, het maken van een op een server gebaseerde firewall, het weergeven van servereigenschappen, het maken van verbinding met behulp van SQL Server Management Studio, het uitvoeren van query's op de hoofddatabase, het maken van een voorbeelddatabase, het uitvoeren van query's op database-eigenschappen, het maken van verbinding met behulp van SQL Server Management Studio en het uitvoeren van query's op de voorbeelddatabase.
>

## <a name="language-and-platform"></a>Taal en platform
Er zijn codevoorbeelden beschikbaar voor verschillende programmeertalen en platforms. Hier vindt u koppelingen naar de codevoorbeelden: 

* Meer informatie: [verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md).

## <a name="tools"></a>Hulpprogramma's 
U kunt gebruikmaken van open-source-hulpprogramma's zoals [cheetah](https://github.com/wunderlist/cheetah), [sql cli](https://www.npmjs.com/package/sql-cli) of [VS Code](https://code.visualstudio.com/). Daarnaast werkt Azure SQL Database met Microsoft-hulpprogramma's zoals [Visual Studio](https://www.visualstudio.com/downloads/) en [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx).  U kunt ook de Azure-beheerportal, PowerShell en REST-API's gebruiken als u nog productiever wilt zijn.

## <a name="resource-limitations"></a>Resourcebeperkingen
Azure SQL Database beheert de resources die voor een database beschikbaar zijn door twee verschillende mechanismen te gebruiken: Resources Governance en Enforcement of Limits.

* Meer informatie: [limieten voor Azure SQL Database](sql-database-service-tiers.md).

## <a name="security"></a>Beveiliging
Azure SQL Database biedt resources voor het beperken van toegang, het beveiligen van gegevens en het bewaken van activiteiten in een SQL Database.

* Meer informatie: [SQL Database beveiligen](sql-database-security-overview.md).

## <a name="authentication"></a>Authentication
* Azure SQL Database biedt ondersteuning voor zowel SQL Server-verificatiegebruikers en -aanmeldingen als [Azure Active Directory](sql-database-aad-authentication.md)-verificatiegebruikers en -aanmeldingen.
* U moet een bepaalde database opgeven in plaats van standaard de *hoofd*database te gebruiken.
* U kunt geen gebruikmaken van de Transact-SQL-instructie **USE myDatabaseName;** om SQL Database om over te schakelen naar een andere database.
* Meer informatie: [SQL Database-beveiliging: beveiliging van toegang en meld u aan de database beheren](sql-database-manage-logins.md).

## <a name="resiliency"></a>Flexibiliteit
Wanneer er een tijdelijke fout optreedt bij het verbinden met SQL Database, moet met uw code de aanroep opnieuw worden uitgevoerd.  Het wordt aanbevolen om voor logica voor opnieuw proberen uitstellogica te gebruiken zodat de SQL Database niet overbelast raakt door meerdere clients die tegelijk opnieuw proberen.

* Codevoorbeelden: codevoorbeelden die aangeven Pogingslogica, vindt u voorbeelden voor de taal van uw keuze op: [verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md).
* Meer informatie: [foutberichten voor SQL Database-clientprogramma's](sql-database-develop-error-messages.md).

## <a name="managing-connections"></a>Verbindingen beheren
* In de verbindingslogica van de client overschrijft u de standaardtime-out zodat deze 30 seconden bedraagt.  De standaardwaarde van 15 seconden is te kort voor verbindingen die afhankelijk zijn van internet.
* Als u een [verbindingsgroep](http://msdn.microsoft.com/library/8xx3tyca.aspx) gebruikt, zorgt u ervoor dat de verbinding wordt verbroken zodra uw programma er niet actief meer gebruik van maakt en wanneer uw programma niet wordt voorbereid om opnieuw verbinding te maken.

## <a name="network-considerations"></a>Aandachtspunten voor netwerken
* Zorg er op de computer die als host fungeert voor uw clientprogramma voor dat de firewall uitgaande TCP-communicatie toestaat via poort 1433.  Meer informatie: [een Azure SQL Database-firewall configureren](sql-database-configure-firewall-settings.md).
* Als uw clientprogramma verbinding maakt met SQL-Database terwijl de client wordt uitgevoerd op Azure een virtuele machine (VM), moet u bepaalde poortbereiken op de virtuele machine openen. Meer informatie: [poorten buiten 1433 voor ADO.NET 4.5 en SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
* Clientverbindingen met Azure SQL Database wordt soms de proxyserver wordt overgeslagen en communiceren rechtstreeks met de database. Andere poorten dan poort 1433 worden belangrijk. Voor meer informatie [Azure SQL Database connectivity architectuur](sql-database-connectivity-architecture.md) en [poorten buiten 1433 voor ADO.NET 4.5 en SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="data-sharding-with-elastic-scale"></a>Gegevens sharding met elastisch schalen
Elastisch schalen vereenvoudigt het proces van schalen (en). 

* [Ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).
* [Gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md).
* [Aan de slag met Azure SQL Database elastisch schalen Preview](sql-database-elastic-scale-get-started.md).

## <a name="next-steps"></a>Volgende stappen
Bekijk alle [mogelijkheden van SQL Database](sql-database-technical-overview.md).
