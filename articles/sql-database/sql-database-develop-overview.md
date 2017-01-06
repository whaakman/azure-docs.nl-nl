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
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: e5b5751facb68ae4a62e3071fe4dfefc02434a9f
ms.openlocfilehash: 18dc3cce7451d90b6b65b990b80c05e7f6decb56


---
# <a name="sql-database-application-development-overview"></a>Overzicht van de ontwikkeling van toepassingen voor SQL Database
In dit artikel leest u meer over de algemene zaken waar ontwikkelaars van op de hoogte moeten zijn voor het schrijven van code om verbinding te maken met Azure SQL Database.

> [!TIP]
> Zie de [Zelfstudie Aan de slag](sql-database-get-started.md) voor een zelfstudie over het maken van een server, het maken van een op een server gebaseerde firewall, het weergeven van servereigenschappen, het maken van verbinding met behulp van SQL Server Management Studio, het uitvoeren van query's op de hoofddatabase, het maken van een voorbeelddatabase, het uitvoeren van query's op database-eigenschappen, het maken van verbinding met behulp van SQL Server Management Studio en het uitvoeren van query's op de voorbeelddatabase.
>

## <a name="language-and-platform"></a>Taal en platform
Er zijn codevoorbeelden beschikbaar voor verschillende programmeertalen en platforms. Hier vindt u koppelingen naar de codevoorbeelden: 

* Meer informatie: [Verbindingsbibliotheken voor SQL Database en SQL Server](sql-database-libraries.md)

## <a name="resource-limitations"></a>Resourcebeperkingen
Azure SQL Database beheert de resources die voor een database beschikbaar zijn door twee verschillende mechanismen te gebruiken: Resources Governance en Enforcement of Limits.

* Meer informatie: [Azure SQL Database-resourcebeperkingen](sql-database-resource-limits.md)

## <a name="security"></a>Beveiliging
Azure SQL Database biedt resources voor het beperken van toegang, het beveiligen van gegevens en het bewaken van activiteiten in een SQL Database.

* Meer informatie: [Uw SQL Database beveiligen](sql-database-security-overview.md)

## <a name="authentication"></a>Authentication
* Azure SQL Database biedt ondersteuning voor zowel SQL Server-verificatiegebruikers en -aanmeldingen als [Azure Active Directory](sql-database-aad-authentication.md)-verificatiegebruikers en -aanmeldingen.
* U moet een bepaalde database opgeven in plaats van standaard de *hoofd*database te gebruiken.
* U kunt geen gebruikmaken van de Transact-SQL-instructie **USE myDatabaseName;** om SQL Database om over te schakelen naar een andere database.
* Meer informatie: [SQL Database-beveiliging: databasetoegang en aanmeldingsbeveiliging beheren](sql-database-manage-logins.md)

## <a name="resiliency"></a>Flexibiliteit
Wanneer er een tijdelijke fout optreedt bij het verbinden met SQL Database, moet met uw code de aanroep opnieuw worden uitgevoerd.  Het wordt aanbevolen om voor logica voor opnieuw proberen uitstellogica te gebruiken zodat de SQL Database niet overbelast raakt door meerdere clients die tegelijk opnieuw proberen.

* Codevoorbeelden: zie [Verbindingsbibliotheken voor SQL Database en SQL Server](sql-database-libraries.md) voor codevoorbeelden met daarin logica voor opnieuw proberen en voorbeelden van de taal van uw keuze
* Meer informatie: [Foutberichten voor SQL Database-clientprogramma’s](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Verbindingen beheren
* In de verbindingslogica van de client overschrijft u de standaardtime-out zodat deze 30 seconden bedraagt.  De standaardwaarde van 15 seconden is te kort voor verbindingen die afhankelijk zijn van internet.
* Als u een [verbindingsgroep](http://msdn.microsoft.com/library/8xx3tyca.aspx) gebruikt, zorgt u ervoor dat de verbinding wordt verbroken zodra uw programma er niet actief meer gebruik van maakt en wanneer uw programma niet wordt voorbereid om opnieuw verbinding te maken.

## <a name="network-considerations"></a>Netwerkoverwegingen
* Zorg er op de computer die als host fungeert voor uw clientprogramma voor dat de firewall uitgaande TCP-communicatie toestaat via poort 1433.  Meer informatie: [Een Azure SQL Database-firewall configureren](sql-database-configure-firewall-settings.md)
* Als uw clientprogramma verbinding maakt met SQL Database V12 terwijl de client wordt uitgevoerd op een virtuele machine van Azure (VM), moet u bepaalde poortbereiken openen op de virtuele machine. Meer informatie: [Poorten boven 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)
* Clientverbindingen met Azure SQL Database V12 slaan soms de proxyserver over en communiceren rechtstreeks met de database. Andere poorten dan poort 1433 worden belangrijk. Meer informatie: [Poorten boven 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Gegevenssharding met elastisch schalen
Met elastisch schalen wordt omhoog en omlaag schalen eenvoudiger. 

* [Ontwerppatronen voor SaaS-toepassingen met meerdere tenants met behulp van Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md)
* [Aan de slag met het voorbeeld voor elastisch schalen voor Azure SQL Database](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Volgende stappen
Bekijk alle [mogelijkheden van SQL Database](https://azure.microsoft.com/services/sql-database/).




<!--HONumber=Dec16_HO4-->


