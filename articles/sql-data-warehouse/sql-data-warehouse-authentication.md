---
title: Verificatie met Azure SQL datawarehouse | Microsoft Docs
description: Azure Active Directory (AAD) en SQL Server-verificatie met Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
tags: 
ms.assetid: fefaaa75-2d0c-4e5d-aadb-410342d1ad73
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.custom: security
ms.date: 03/21/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 92f48027051bc4aff4d6b8d66fdd6de81bba3657
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="authentication-to-azure-sql-data-warehouse"></a>Verificatie met Azure SQL Data Warehouse
> [!div class="op_single_selector"]
> * [Overzicht van beveiliging](sql-data-warehouse-overview-manage-security.md)
> * [Verificatie](sql-data-warehouse-authentication.md)
> * [Versleuteling (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Versleuteling (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

Voor verbinding met SQL Data Warehouse, moet u de beveiligingsreferenties doorgeven voor verificatiedoeleinden. Bij het maken van een verbinding, worden bepaalde verbindingsinstellingen geconfigureerd als onderdeel van uw query-sessie tot stand brengen.  

Zie voor meer informatie over beveiliging en het inschakelen van verbindingen met uw datawarehouse [beveiligen van een database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL-verificatie
Voor verbinding met SQL Data Warehouse, moet u de volgende informatie opgeven:

* Volledig gekwalificeerde servernaam
* Geef de SQL-verificatie
* Gebruikersnaam
* Wachtwoord
* Standaard-database (optioneel)

Standaard wordt de verbinding maken met de *master* database en niet de gebruikersdatabase. Voor verbinding met de gebruikersdatabase, kunt u een van twee dingen doen:

* Geef de standaarddatabase bij het registreren van uw server met SQL Server Object Explorer in SSDT, SSMS, of in de verbindingsreeks voor de toepassing. Zo bevatten de parameter InitialCatalog, is voor een ODBC-verbinding.
* Markeer de database voordat u een sessie in SSDT maakt.

> [!NOTE]
> De Transact-SQL-instructie **gebruik MijnDatabase;** wordt niet ondersteund voor het wijzigen van de database voor een verbinding. Voor richtlijnen die verbinding maken met SQL Data Warehouse met SSDT, raadpleegt u de [Query met Visual Studio] [ Query with Visual Studio] artikel.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Verificatie met Azure Active Directory (AAD)
[Azure Active Directory] [ What is Azure Active Directory] verificatie is een mechanisme voor verbinding maken met Microsoft Azure SQL Data Warehouse met behulp van identiteiten in Azure Active Directory (Azure AD). Azure Active Directory-verificatie, kunt u de identiteit van gebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centrale ID-beheer biedt één plaats voor het beheren van gebruikers van de SQL Data Warehouse en vereenvoudigt het beheer van machtigingen. 

### <a name="benefits"></a>Voordelen
Azure Active Directory-voordelen zijn:

* Biedt een alternatief voor SQL Server-verificatie.
* Houdt de komst van gebruikersidentiteiten voor databaseservers.
* Hiermee kunt wachtwoord draaien op één plaats
* Databasemachtigingen met behulp van externe (AAD)-groepen beheren.
* Elimineert wachtwoorden moet opslaan door het inschakelen van geïntegreerde Windows-verificatie en andere soorten authenticatie wordt ondersteund door Azure Active Directory.
* Maakt gebruik van opgenomen databasegebruikers om identiteiten op het databaseniveau van de te verifiëren.
* Biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL Data Warehouse.
* Ondersteunt multi-factor authentication via universele verificatie van Active Directory voor SQL Server Management Studio. Zie voor een beschrijving van multi-factor Authentication [SSMS ondersteuning voor Azure AD MFA met SQL-Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Azure Active Directory is nog relatief nieuw en bevat enkele beperkingen. Om ervoor te zorgen dat Azure Active Directory geschikt voor uw omgeving is, Zie [Azure AD-functies en beperkingen][Azure AD features and limitations], specifiek voor de aanvullende overwegingen.
> 
> 

### <a name="configuration-steps"></a>Configuratiestappen
Volg deze stappen voor het configureren van Azure Active Directory-verificatie.

1. U maakt en vult u een Azure Active Directory
2. Optioneel: Koppelen of de active directory die momenteel is gekoppeld aan uw Azure-abonnement wijzigen
3. Een Azure Active Directory-beheerder voor Azure SQL Data Warehouse maken.
4. De clientcomputers configureren
5. Ingesloten databasegebruikers in de database die is toegewezen aan Azure AD-identiteiten maken
6. Verbinding maken met uw datawarehouse met behulp van Azure AD-identiteiten

Azure Active Directory-gebruikers worden momenteel niet weergegeven in de Objectverkenner SSDT. Als tijdelijke oplossing de gebruikers in bekijken [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-the-details"></a>De details
* De stappen voor het configureren en gebruiken van Azure Active Directory-verificatie zijn bijna identiek zijn voor Azure SQL Database en Azure SQL Data Warehouse. De gedetailleerde stappen in het onderwerp [verbinding maken met SQL-Database of SQL Data Warehouse door met behulp van Azure Active Directory-verificatie](../sql-database/sql-database-aad-authentication.md).
* Aangepaste database functies maken en gebruikers toevoegen aan de rollen. Vervolgens gedetailleerde machtigingen verlenen voor de rollen. Zie voor meer informatie [aan de slag met machtigingen voor Database-Engine](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Volgende stappen
Zie het opvragen van uw datawarehouse met Visual Studio en andere toepassingen starten [Query met Visual Studio][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]: ../active-directory/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
