---
title: Overzicht van de ontwikkeling van toepassingen voor SQL Database | Microsoft Docs
description: Kom meer te weten over beschikbare verbindingsbibliotheken en aanbevolen procedures voor toepassingen die verbinding maken met SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: stevestein
ms.author: sstein
ms.reviewer: genemi
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: 01c4bcfcea038f3e69620cdce78719c8c5128faf
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55964794"
---
# <a name="sql-database-application-development-overview"></a>Overzicht van SQL Database-toepassing voor ontwikkelaars

In dit artikel leest u meer over de algemene zaken waar ontwikkelaars van op de hoogte moeten zijn voor het schrijven van code om verbinding te maken met Azure SQL Database. In dit artikel is van toepassing op alle implementatiemodellen van Azure SQL Database (individuele database, elastische groepen en voor het beheerde exemplaar).

> [!TIP]
> Kijken de aan de slag handleidingen voor [enkelvoudige databases](sql-database-single-database-quickstart-guide.md) en [instanties die worden beheerd](sql-database-managed-instance-quickstart-guide.md) als u nodig hebt voor het instellen van uw Azure SQL Database.
>

## <a name="language-and-platform"></a>Taal en platform

U kunt verschillende [programmeertalen en platforms](sql-database-connect-query.md) verbinding maken en query uitvoeren op Azure SQL Database. U vindt [voorbeeldtoepassingen](https://azure.microsoft.com/resources/samples/?service=sql-database&sort=0) die u kunt gebruiken om verbinding met de Azure SQL-Database te maken.

U kunt gebruikmaken van open-source hulpprogramma's zoals [cheetah](https://github.com/wunderlist/cheetah), [sql cli](https://www.npmjs.com/package/sql-cli), [VS Code](https://code.visualstudio.com/). Daarnaast werkt Azure SQL Database met Microsoft-hulpprogramma's zoals [Visual Studio](https://www.visualstudio.com/downloads/) en [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). U kunt ook de Azure-portal, PowerShell en REST-API's kunt u nog productiever.

## <a name="authentication"></a>Authentication

Toegang tot Azure SQL Database is beveiligd met aanmeldingen en firewalls. Azure SQL Database ondersteunt zowel SQL Server en [verificatie van Azure Active Directory (AAD)](sql-database-aad-authentication.md) gebruikers en aanmeldgegevens. AAD-aanmeldingen zijn alleen beschikbaar in de Managed Instance. 

Meer informatie over [beheren van toegang tot de database en meld u aan](sql-database-manage-logins.md).

## <a name="connections"></a>Verbindingen

In de verbindingslogica van de client overschrijft u de standaardtime-out zodat deze 30 seconden bedraagt. De standaardwaarde van 15 seconden is te kort voor verbindingen die afhankelijk zijn van internet.

Als u een [verbindingsgroep](https://msdn.microsoft.com/library/8xx3tyca.aspx) gebruikt, zorgt u ervoor dat de verbinding wordt verbroken zodra uw programma er niet actief meer gebruik van maakt en wanneer uw programma niet wordt voorbereid om opnieuw verbinding te maken.

Vermijd langlopende transacties omdat er een storing in de infrastructuur of verbinding kan Draai de transactie terug. Indien mogelijk de transactie in de meerdere kleinere transacties splitsen en gebruik [batchverwerking voor betere prestaties](sql-database-use-batching-to-improve-performance.md).

## <a name="resiliency"></a>Flexibiliteit

Azure SQL Database is een cloudservice waar u tijdelijke fouten die optreden zou verwachten in de onderliggende infrastructuur of in de communicatie tussen cloudentiteiten. Azure SQL Database is op de transitieve infrastructuuruitval flexibele, maar deze fouten kunnen invloed hebben op de verbinding. Wanneer er een tijdelijke fout optreedt bij het verbinden met SQL-Database, uw code moet [de aanroep opnieuw](sql-database-connectivity-issues.md). Het wordt aanbevolen om voor logica voor opnieuw proberen uitstellogica te gebruiken zodat de SQL Database niet overbelast raakt door meerdere clients die tegelijk opnieuw proberen. Logica voor opnieuw proberen is afhankelijk van de [foutberichten voor SQL Database-clientprogramma's](sql-database-develop-error-messages.md).

Zie voor meer informatie over het voorbereiden voor gelijktijdige onderhoudsgebeurtenissen op uw Azure SQL database [plannen voor Azure-onderhoud-gebeurtenissen in Azure SQL Database](sql-database-planned-maintenance.md).

## <a name="network-considerations"></a>Aandachtspunten voor netwerken

- Zorg er op de computer die als host fungeert voor uw clientprogramma voor dat de firewall uitgaande TCP-communicatie toestaat via poort 1433.  Meer informatie: [Een Azure SQL Database-firewall configureren](sql-database-configure-firewall-settings.md).
- Als uw clientprogramma verbinding maakt met SQL-Database terwijl de client wordt uitgevoerd op een Azure-machine (VM), moet u bepaalde poortbereiken op de virtuele machine openen. Meer informatie: [Poorten boven 1433 voor ADO.NET 4.5 en SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).
- Clientverbindingen met Azure SQL Database soms de proxyserver wordt overgeslagen en communiceren rechtstreeks met de database. Andere poorten dan poort 1433 worden belangrijk. Voor meer informatie, [Azure SQL Database-connectiviteitsarchitectuur](sql-database-connectivity-architecture.md) en [poorten boven 1433 voor ADO.NET 4.5 en SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md).
- Zie voor netwerken configation voor een beheerd exemplaar [netwerkconfiguratie voor beheerde exemplaren](sql-database-howto-managed-instance.md#network-configuration).

## <a name="next-steps"></a>Volgende stappen

Bekijk alle [mogelijkheden van SQL Database](sql-database-technical-overview.md).
