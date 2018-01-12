---
title: Toegang verlenen tot Azure SQL Database | Microsoft Docs
description: Meer informatie over het verlenen van toegang tot Microsoft Azure SQL Database.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 02/06/2017
ms.author: carlrab
ms.openlocfilehash: 28c1ec79752f822939fefe6ce3686ace8ad1b6b0
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="azure-sql-database-access-control"></a>Toegangsbeheer voor Azure SQL Database
Voor de beveiliging regelt SQL Database toegang met firewallregels die de connectiviteit beperken op basis van IP-adres. Met behulp van verificatiemechanismen moeten gebruikers hun identiteit kunnen aantonen en met autorisatiemechanismen worden gebruikers beperkt tot bepaalde gegevens en acties. 

> [!IMPORTANT]
> Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van de beveiligingsfuncties van SQL Database. Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md).

## <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels
Microsoft Azure SQL Database levert een relationele-databaseservice voor Azure en andere op internet gebaseerde toepassingen. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt. Zie [Overzicht van de firewallregels voor SQL Database](sql-database-firewall-configure.md) voor meer informatie.

De Azure SQL Database-service is alleen beschikbaar via TCP-poort 1433. Zorg voor toegang tot een SQL Database vanaf uw computer ervoor dat de firewall van uw clientcomputer uitgaande TCP-communicatie op TCP-poort 1433 toestaat. Blokkeer binnenkomende verbindingen op TCP-poort 1433 als u deze niet nodig hebt voor andere toepassingen. 

Als onderdeel van het verbindingsproces worden verbindingen van virtuele Azure-machines omgeleid naar een ander IP-adres en poort, die uniek zijn voor elke werkrol. Het poortnummer ligt in het bereik van 11000 tot 11999. Zie voor meer informatie over de TCP-poorten, [poorten buiten 1433 voor ADO.NET 4.5 en SQL Database2](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Verificatie

SQL Database ondersteunt twee typen verificatie:

* **SQL-verificatie**, waarbij een gebruikersnaam en wachtwoord worden gebruikt. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze aanmeldingsgegevens kunt u zich bij elke database op die server als de database-eigenaar of 'dbo' verifiëren. 
* **Azure Active Directory-verificatie**, waarbij identiteiten worden gebruikt die worden beheerd in Azure Active Directory. Deze methode wordt ondersteund voor beheerde en geïntegreerde domeinen. Gebruik [waar mogelijk](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode) Active Directory-verificatie (geïntegreerde beveiliging). Als u Azure Active Directory-verificatie wilt gebruiken, moet u een andere serverbeheerder maken, de 'Azure AD-beheerder' genaamd, die Azure AD-gebruikers en -groepen kan beheren. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren. Zie [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een Azure AD-beheerder om Azure Active Directory-verificatie in te schakelen.

De Database-engine sluit verbindingen die gedurende meer dan 30 minuten inactief zijn. De verbinding moet zich opnieuw aanmelden voordat deze kan worden gebruikt. Continu actieve verbindingen met SQL Database moeten ten minste elke 10 uur opnieuw worden geautoriseerd (uitgevoerd door de database-engine). De database-engine probeert opnieuw te autoriseren met het oorspronkelijk opgegeven wachtwoord en er is geen gebruikersinvoer vereist. Wanneer een wachtwoord opnieuw wordt ingesteld in SQL Database wordt vanwege prestatieredenen de verbinding niet opnieuw geverifieerd, zelfs als de verbinding wordt hersteld vanwege een groepsgewijze verbinding. Dit wijkt af van het gedrag van een lokale SQL Server. Als het wachtwoord is gewijzigd sinds de verbinding de eerste keer is geautoriseerd, moet de verbinding worden beëindigd en wordt er een nieuwe verbinding gemaakt met het nieuwe wachtwoord. Een gebruiker met de `KILL DATABASE CONNECTION`-machtiging kan een verbinding met SQL Database expliciet afsluiten met behulp van de opdracht [KILL](https://docs.microsoft.com/sql/t-sql/language-elements/kill-transact-sql).

Gebruikersaccounts kunnen worden gemaakt in de hoofddatabase en kunnen machtigingen krijgen toegewezen in alle databases op de server, of ze kunnen worden gemaakt in de database zelf (dit worden ingesloten gebruikers genoemd). Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor informatie over het maken en beheren van aanmeldingen. Gebruik ingesloten databasegebruikers om de draagbaarheid en schaalbaarheid te verbeteren. Zie [Contained Database Users - Making Your Database Portable](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) (Ingesloten databasegebruikers: een draagbare database maken), [CREATE USER (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) (GEBRUIKER MAKEN (Transact-SQL)) en [Contained Databases](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases) (Ingesloten databases) voor meer informatie.

Als best practice moet uw toepassing een specifiek account gebruiken om te verifiëren. Op deze manier kunt u de machtigingen die aan de toepassing worden verleend, beperken en het risico op schadelijke activiteiten verminderen in het geval uw toepassingscode kwetsbaar is voor SQL-injectieaanvallen. U wordt aanbevolen om een [ingesloten databasegebruiker](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) te maken, zodat uw app zich rechtstreeks bij de database kan verifiëren. 

## <a name="authorization"></a>Autorisatie

Autorisatie verwijst naar wat een gebruiker kan doen binnen een Azure SQL Database. Dit wordt bepaald door de [rollidmaatschappen](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/database-level-roles) en [objectmachtigingen](https://docs.microsoft.com/sql/relational-databases/security/permissions-database-engine) voor de database van uw gebruikersaccount. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing. Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor meer informatie.

Normaal gesproken hebben alleen beheerders toegang tot de `master`database nodig. Routinematige toegang tot elke gebruikersdatabase moet verlopen via ingesloten databasegebruikers die geen beheerder zijn die in elke database worden gemaakt. Wanneer u ingesloten databasegebruikers gebruikt, hoeft u geen aanmeldingen te maken in de `master`database. Zie [Ingesloten databasegebruikers: een draagbare database maken](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) voor meer informatie.

Zorg ervoor dat u de volgende functies kunt gebruiken voor het beperken of het verhogen van machtigingen:   
* [Imitatie](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/customizing-permissions-with-impersonation-in-sql-server) en [module-ondertekening](https://docs.microsoft.com/dotnet/framework/data/adonet/sql/signing-stored-procedures-in-sql-server) kunnen worden gebruikt om machtigingen tijdelijk veilig te verhogen.
* [Beveiliging op rijniveau](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kan worden gebruikt om de rijen waartoe een gebruiker toegang heeft te beperken.
* [Gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) kan worden gebruikt om de weergave van gevoelige gegevens te beperken.
* [Opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/stored-procedures/stored-procedures-database-engine) kunnen worden gebruikt om de acties die op de database kunnen worden uitgevoerd te beperken.

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van de beveiligingsfuncties van SQL Database.
- Zie voor meer informatie over firewallregels, [Firewall-regels](sql-database-firewall-configure.md).
- Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor meer informatie over gebruikers en aanmeldingen. 
- Zie voor een beschrijving van de proactieve controle, [Database Auditing](sql-database-auditing.md) en [SQL Database met detectie van dreigingen](sql-database-threat-detection.md).
- Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md).
