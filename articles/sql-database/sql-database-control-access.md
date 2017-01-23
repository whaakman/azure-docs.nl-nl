---
title: Toegangsbeheer voor Azure SQL Database | Microsoft Docs
description: Meer informatie over het beheer van de toegang tot Microsoft Azure SQL Database.
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/18/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: f4712d70c0323e607ddcc021809f8097a621730d
ms.openlocfilehash: 82508d01f9f1aaee11ebb5a5f936dfc71ace9cad


---
# <a name="azure-sql-database-access-control"></a>Toegangsbeheer voor Azure SQL Database
Voor de beveiliging regelt SQL Database toegang met firewallregels die de connectiviteit beperken op basis van IP-adres. Met behulp van verificatiemechanismen moeten gebruikers hun identiteit kunnen aantonen en met autorisatiemechanismen worden gebruikers beperkt tot bepaalde gegevens en acties. 

> [!IMPORTANT]
> Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van de beveiligingsfuncties van SQL Database.
>

## <a name="firewall-and-firewall-rules"></a>Firewall en firewallregels
Microsoft Azure SQL Database levert een relationele-databaseservice voor Azure en andere op internet gebaseerde toepassingen. Om uw gegevens te beschermen, verhinderen firewalls alle toegang tot de databaseserver totdat u opgeeft welke computers zijn gemachtigd. De firewall verleent toegang tot databases op basis van het IP-adres waar de aanvraag vandaan komt. Zie [Overzicht van de firewallregels voor SQL Database](sql-database-firewall-configure.md) voor meer informatie.

De Azure SQL Database-service is alleen beschikbaar via TCP-poort 1433. Zorg voor toegang tot een SQL Database vanaf uw computer ervoor dat de firewall van uw clientcomputer uitgaande TCP-communicatie op TCP-poort 1433 toestaat. Blokkeer binnenkomende verbindingen op TCP-poort 1433 als u deze niet nodig hebt voor andere toepassingen. 

Als onderdeel van het verbindingsproces worden verbindingen van virtuele Azure-machines omgeleid naar een ander IP-adres en poort, die uniek zijn voor elke werkrol. Het poortnummer ligt in het bereik van 11000 tot 11999. Zie [Poorten boven 1433 voor ADO.NET 4.5 en SQL Database V12](sql-database-develop-direct-route-ports-adonet-v12.md) voor meer informatie over TCP-poorten.

## <a name="authentication"></a>Authentication

SQL Database ondersteunt twee typen verificatie:

* **SQL-verificatie**, waarbij een gebruikersnaam en wachtwoord worden gebruikt. Wanneer u de logische server voor uw database hebt gemaakt, hebt u een aanmelding 'serverbeheerder' opgegeven met een gebruikersnaam en wachtwoord. Met deze aanmeldingsgegevens kunt u zich bij elke database op die server als de database-eigenaar of 'dbo' verifiëren. 
* **Azure Active Directory-verificatie**, waarbij identiteiten worden gebruikt die worden beheerd in Azure Active Directory. Deze methode wordt ondersteund voor beheerde en geïntegreerde domeinen. Gebruik [waar mogelijk](https://msdn.microsoft.com/library/ms144284.aspx) Active Directory-verificatie (geïntegreerde beveiliging). Als u Azure Active Directory-verificatie wilt gebruiken, moet u een andere serverbeheerder maken, de 'Azure AD-beheerder' genaamd, die Azure AD-gebruikers en -groepen kan beheren. Deze beheerder kan ook alle bewerkingen uitvoeren die reguliere serverbeheerders kunnen uitvoeren. Zie [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md) voor een overzicht van het maken van een Azure AD-beheerder om Azure Active Directory-verificatie in te schakelen.

De Database-engine sluit verbindingen die gedurende meer dan 30 minuten inactief zijn. De verbinding moet opnieuw worden aangemeld voordat deze kan worden gebruikt. Continu actieve verbindingen met SQL Database moeten ten minste elke 10 uur opnieuw worden geautoriseerd (uitgevoerd door de database-engine). De database-engine probeert opnieuw te autoriseren met het oorspronkelijk opgegeven wachtwoord en er is geen gebruikersinvoer vereist. Wanneer een wachtwoord opnieuw wordt ingesteld in SQL Database wordt vanwege prestatieredenen de verbinding niet opnieuw geverifieerd, zelfs als de verbinding wordt hersteld vanwege een groepsgewijze verbinding. Dit wijkt af van het gedrag van een lokale SQL Server. Als het wachtwoord is gewijzigd sinds de verbinding de eerste keer is geautoriseerd, moet de verbinding worden beëindigd en wordt er een nieuwe verbinding gemaakt met het nieuwe wachtwoord. Een gebruiker met de KILL DATABASE CONNECTION-machtiging kan een verbinding met SQL Database expliciet afsluiten met behulp van de opdracht [KILL](https://msdn.microsoft.com/library/ms173730.aspx).

Gebruikersaccounts kunnen worden gemaakt in de hoofddatabase en kunnen machtigingen krijgen toegewezen in alle databases op de server, of ze kunnen worden gemaakt in de database zelf (dit worden ingesloten gebruikers genoemd). Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor informatie over het maken en beheren van aanmeldingen. Gebruik ingesloten databasegebruikers om de draagbaarheid en schaalbaarheid te verbeteren. Zie [Contained Database Users - Making Your Database Portable](https://msdn.microsoft.com/library/ff929188.aspx) (Ingesloten databasegebruikers: een draagbare database maken), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx) (GEBRUIKER MAKEN (Transact-SQL)) en [Contained Databases](https://technet.microsoft.com/library/ff929071.aspx) (Ingesloten databases) voor meer informatie.

Als best practice moet uw toepassing een ander account gebruiken om te verifiëren - op deze manier kunt u de machtigingen die aan de toepassing worden verleend, beperken en het risico op schadelijke activiteiten in het geval waarin uw toepassingscode kwetsbaar is voor een SQL-injectieaanvallen verminderen. U wordt aanbevolen om een [ingesloten databasegebruiker](https://msdn.microsoft.com/library/ff929188) te maken, zodat uw app zich rechtstreeks bij de database kan verifiëren. U kunt een ingesloten databasegebruiker maken die SQL-verificatie gebruikt door de volgende T-SQL-opdracht uit te voeren terwijl u als serverbeheerder verbinding hebt met uw gebruikersdatabase:

## <a name="authorization"></a>Autorisatie

Autorisatie verwijst naar wat een gebruiker kan doen binnen een Azure SQL Database. Dit wordt bepaald door de [rollidmaatschappen](https://msdn.microsoft.com/library/ms189121) en [objectmachtigingen](https://msdn.microsoft.com/library/ms191291.aspx) voor de database van uw gebruikersaccount. Het wordt aanbevolen om gebruikers de minimaal benodigde bevoegdheden te verlenen. Het serverbeheerdersaccount waarmee u verbinding maakt is lid van db_owner, die geautoriseerd is om alle bewerkingen binnen de database uit te voeren. Gebruik dit account voor het implementeren van schema-updates en andere beheerbewerkingen. Gebruik het 'ApplicationUser'-account met beperktere machtigingen om vanuit van uw toepassing verbinding te maken met de database met de minste bevoegdheden die nodig zijn voor uw toepassing. Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor meer informatie.

Normaal gesproken hebben alleen beheerders toegang tot de hoofddatabase nodig. Routinematige toegang tot elke gebruikersdatabase moet verlopen via ingesloten databasegebruikers die geen beheerder zijn die in elke database worden gemaakt. Wanneer u ingesloten databasegebruikers gebruikt, hoeft u geen aanmeldingen te maken in de hoofddatabase. Zie [Ingesloten databasegebruikers: een draagbare database maken](https://msdn.microsoft.com/library/ff929188.aspx) voor meer informatie.

Daarnaast kunnen de volgende functies worden gebruikt om machtigingen te beperken of uit te breiden.

* [Imitatie](https://msdn.microsoft.com/library/vstudio/bb669087) en [module-ondertekening](https://msdn.microsoft.com/library/bb669102) kunnen worden gebruikt om machtigingen tijdelijk veilig te verhogen.
* [Beveiliging op rijniveau](https://msdn.microsoft.com/library/dn765131) kan worden gebruikt om de rijen waartoe een gebruiker toegang heeft te beperken.
* [Gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) kan worden gebruikt om de weergave van gevoelige gegevens te beperken.
* [Opgeslagen procedures](https://msdn.microsoft.com/library/ms190782) kunnen worden gebruikt om de acties die op de database kunnen worden uitgevoerd te beperken.

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL security overview](sql-database-security-overview.md) (SQL-beveiligingsoverzicht) voor een overzicht van de SQL Database-beveiligingsfuncties.
- Zie [Azure SQL Database-firewall](sql-database-firewall-configure.md) voor meer informatie over firewallregels.
- Zie [Aanmeldingen beheren](sql-database-manage-logins.md) voor meer informatie over gebruikers en aanmeldingen. 
- Zie [Data protection and security](sql-database-protect-data.md) (Gegevensbescherming en beveiliging) voor een discussie over het gebruik van de functies voor gegevensbeveiliging in SQL Database.
- Zie [Get started with SQL Database Auditing](sql-database-auditing-get-started.md) (Aan de slag met SQL Database Auditing) en [Get started with SQL Database Threat Detection](sql-database-threat-detection-get-started.md) (Aan de slag met SQL Database Threat Detection) voor een discussie over proactieve controle.




<!--HONumber=Dec16_HO4-->


