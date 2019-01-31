---
title: Verifiëren bij Azure SQL datawarehouse | Microsoft Docs
description: Ontdek hoe u verifieert met Azure SQL Data Warehouse met behulp van Azure Active Directory (AAD) of SQL Server-verificatie.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/12/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 7f26ded5fae1bde5b2e6485ae2a2f99eebb17563
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55463324"
---
# <a name="authenticate-to-azure-sql-data-warehouse"></a>Verifiëren bij Azure SQL datawarehouse
Ontdek hoe u verifieert met Azure SQL Data Warehouse met behulp van Azure Active Directory (AAD) of SQL Server-verificatie.

Voor verbinding met SQL Data Warehouse, moet u in de beveiligingsreferenties doorgeven voor verificatiedoeleinden wordt gebruikt. Bij het maken van een verbinding, worden bepaalde instellingen geconfigureerd als onderdeel van het tot stand brengen van uw query-sessie.  

Zie voor meer informatie over beveiliging en het inschakelen van verbindingen met uw datawarehouse [beveiligen van een database in SQL Data Warehouse][Secure a database in SQL Data Warehouse].

## <a name="sql-authentication"></a>SQL-verificatie
Voor verbinding met SQL Data Warehouse, moet u de volgende informatie opgeven:

* Volledig gekwalificeerde servernaam
* Geef de SQL-verificatie
* Gebruikersnaam
* Wachtwoord
* Standaard-database (optioneel)

Standaard wordt de verbinding maken met de *master* database en niet de gebruikersdatabase. Voor verbinding met uw gebruikersdatabase, kunt u een van twee dingen doen:

* Geef de standaarddatabase bij het registreren van uw server met de SQL Server-Objectverkenner in SSDT, SSMS, of in de verbindingsreeks van uw toepassing. Zo bevatten de parameter InitialCatalog, is voor een ODBC-verbinding.
* Markeer de gebruikersdatabase voordat u een sessie in SSDT.

> [!NOTE]
> De Transact-SQL-instructie **gebruik MijnDatabase;** wordt niet ondersteund voor het wijzigen van de database voor een verbinding. Voor richtlijnen voor het verbinding maken met SQL Data Warehouse met SSDT, raadpleegt u de [query's uitvoeren met Visual Studio] [ Query with Visual Studio] artikel.
> 
> 

## <a name="azure-active-directory-aad-authentication"></a>Verificatie van Azure Active Directory (AAD)
[Azure Active Directory] [ What is Azure Active Directory] verificatie is een mechanisme van verbinding maken met Microsoft Azure SQL Data Warehouse met behulp van identiteiten in Azure Active Directory (Azure AD). Met Azure Active Directory-verificatie, kunt u de identiteit van databasegebruikers en andere Microsoft-services op één centrale locatie centraal beheren. Centrale ID-beheer biedt één plek voor het beheren van gebruikers van SQL Data Warehouse en vereenvoudigt het beheer van machtigingen. 

### <a name="benefits"></a>Voordelen
Azure Active Directory-voordelen zijn:

* Biedt een alternatief voor SQL Server-verificatie.
* Houdt de verspreiding van gebruikers-id's in de database-servers.
* Wisseling van het wachtwoord op één plek kunt
* Machtigingen van de database met behulp van externe (AAD)-groepen beheren.
* Voorkomen dat wachtwoorden moet opslaan te maken met geïntegreerde Windows-verificatie en andere vormen van authenticatie wordt ondersteund door Azure Active Directory.
* Maakt gebruik van ingesloten databasegebruikers om identiteiten op databaseniveau te verifiëren.
* Biedt ondersteuning voor verificatie op basis van tokens voor toepassingen die verbinding maken met SQL Data Warehouse.
* Biedt ondersteuning voor multi-factor authentication via universele authenticatie van Active Directory voor SQL Server Management Studio. Zie voor een beschrijving van multi-factor Authentication, [SSMS-ondersteuning voor Azure AD MFA met SQL Database en SQL Data Warehouse](../sql-database/sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Azure Active Directory nog relatief nieuw is en heeft bepaalde beperkingen. Om ervoor te zorgen dat Azure Active Directory geschikt is voor uw omgeving, Zie [Azure AD-functies en beperkingen][Azure AD features and limitations], specifiek de aanvullende overwegingen.
> 
> 

### <a name="configuration-steps"></a>Configuratiestappen
Volg deze stappen voor het configureren van Azure Active Directory-verificatie.

1. Maken en vullen van een Azure Active Directory
2. Optioneel: Koppelen of de active directory die is gekoppeld aan uw Azure-abonnement wijzigen
3. Maak een Azure Active Directory-beheerder voor Azure SQL Data Warehouse.
4. Uw clientcomputers configureren
5. Maak ingesloten databasegebruikers in de database die is toegewezen aan Azure AD-identiteiten
6. Verbinding maken met uw datawarehouse met behulp van Azure AD-identiteiten

Azure Active Directory-gebruikers worden momenteel niet weergegeven in SSDT-Objectverkenner. Als tijdelijke oplossing, bekijk de gebruikers in [sys.database_principals](https://msdn.microsoft.com/library/ms187328.aspx).

### <a name="find-the-details"></a>De details
* De stappen voor het configureren en gebruiken van Azure Active Directory-verificatie zijn bijna identiek zijn voor Azure SQL Database en Azure SQL Data Warehouse. Volg de gedetailleerde stappen in het onderwerp [verbinding maken met SQL-Database of SQL Data Warehouse door met behulp van Azure Active Directory-verificatie](../sql-database/sql-database-aad-authentication.md).
* Aangepaste databaserollen maken en gebruikers toevoegen aan de rollen. Verleen gedetailleerde machtigingen aan de rollen. Zie voor meer informatie, [aan de slag met machtigingen voor Database-Engine](https://msdn.microsoft.com/library/mt667986.aspx).

## <a name="next-steps"></a>Volgende stappen
Als u wilt gaan met het uitvoeren van query's uw datawarehouse met Visual Studio en andere toepassingen, Zie [query's uitvoeren met Visual Studio][Query with Visual Studio].

<!-- Article references -->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[What is Azure Active Directory]:../active-directory/fundamentals/active-directory-whatis.md
[Azure AD features and limitations]: ../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations
