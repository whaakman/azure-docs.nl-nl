---
title: Meervoudige verificatie - Azure SQL | Microsoft Docs
description: Azure SQL Database en Azure SQL Data Warehouse ondersteuning voor verbindingen van SQL Server Management Studio (SSMS) met behulp van universele verificatie van Active Directory.
services: sql-database
documentationcenter: 
author: GithubMirek
manager: johammer
editor: 
tags: 
ms.assetid: fbd6e644-0520-439c-8304-2e4fb6d6eb91
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 09/29/2017
ms.author: mireks
ms.openlocfilehash: 3fd6532fb3dc6af9b3ccdac37201552ad0475e7b
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="universal-authentication-with-sql-database-and-sql-data-warehouse-ssms-support-for-mfa"></a>Universele verificatie met SQL-Database en SQL Data Warehouse (SSMS ondersteuning voor MFA)
Azure SQL Database en Azure SQL Data Warehouse ondersteunen verbindingen van het gebruik van SQL Server Management Studio (SSMS) *Universal verificatie van Active Directory*. 
**Download de meest recente SSMS** - op de clientcomputer, download de nieuwste versie van SSMS, van [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Gebruik ten minste versie 17,2 juli 2017 voor de functies in dit onderwerp.  De meest recente verbinding in het dialoogvenster ziet er als volgt: ![1mfa universal verbinding](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "tekstvak voor de gebruikersnaam is voltooid.")  

## <a name="the-five-authentication-options"></a>De vijf verificatie-opties  
- Universele verificatie van Active Directory biedt ondersteuning voor de twee methoden voor niet-interactieve verificatie (`Active Directory - Password` verificatie en `Active Directory - Integrated` verificatie). Niet-interactieve `Active Directory - Password` en `Active Directory - Integrated` verificatiemethoden kunnen worden gebruikt in veel verschillende toepassingen (ADO.NET, JDBC, ODBC, enzovoort). Deze twee methoden worden nooit leiden tot pop-dialoogvensters.

- `Active Directory - Universal with MFA`verificatie is een interactieve methode die ook ondersteunt *Azure multi-factor Authentication* (MFA). Azure MFA helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers die een eenvoudige aanmeldprocedure willen. Levert sterke verificatie met een bereik van eenvoudige verificatie-opties (telefoongesprek, tekstbericht, smartcards en pincode of mobiele-appmelding), zodat gebruikers kunnen kiezen hoe ze de voorkeur geven. Interactieve MFA met Azure AD kan resulteren in een pop-dialoogvenster voor validatie.

Zie voor een beschrijving van multi-factor Authentication [multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md).
Zie voor configuratiestappen [Azure SQL-Database configureren multi-factor authentication voor SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD domain parameter name of tenant-ID   

Beginnen met [SSMS versie 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), gebruikers die worden geïmporteerd in de huidige Active Directory van andere Azure Active Directory's als gastgebruikers, geef de naam van het Azure AD-domein of tenant-ID wanneer ze verbinding maken. Gastgebruikers bevatten de gebruikers van andere Azure-advertenties, Microsoft-accounts, zoals outlook.com, hotmail.com, live.com of andere accounts, zoals gmail.com uitgenodigd. Deze informatie kan **Active Directory Universal met MFA verificatie** voor het identificeren van de juiste verificatie-instantie. Deze optie is ook vereist ter ondersteuning van Microsoft-accounts (MSA) zoals outlook.com, hotmail.com, live.com of niet-mailaccount. Deze gebruikers willen kunnen worden geverifieerd met Universal verificatie moeten hun Azure AD-domeinnaam opgeven of tenant-ID. Deze parameter geeft de huidige Azure AD-domein naam/tenant-ID de Azure-Server is gekoppeld aan. Bijvoorbeeld, als Azure-Server is gekoppeld aan Azure AD-domein `contosotest.onmicrosoft.com` waar gebruiker `joe@contosodev.onmicrosoft.com` wordt gehost als een geïmporteerde gebruiker in Azure AD-domein `contosodev.onmicrosoft.com`, de domeinnaam die is vereist voor de verificatie van deze gebruiker is `contosotest.onmicrosoft.com`. Wanneer de gebruiker een systeemeigen gebruiker van de Azure AD dat is gekoppeld aan Azure-Server is en niet een MSA-account is, is er geen domein naam of het tenant-ID vereist. De parameter (begin met SSMS versie 17,2) invoeren in de **verbinding maken met Database** dialoogvenster vervolledig in het dialoogvenster selecteren **Active Directory - Universal met MFA** verificatie, klikt u op **opties**, volledige de **gebruikersnaam** vak en klik vervolgens op de **verbindingseigenschappen** tabblad. Controleer de **AD-domein naam of het tenant-ID** vak en bieden verificatieautoriteit, zoals de naam van het domein (**contosotest.onmicrosoft.com**) of de GUID van de tenant-ID.  
   ![MFA-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   

### <a name="azure-ad-business-to-business-support"></a>Azure AD-ondersteuning voor bedrijven   
Azure AD-gebruikers ondersteund voor Azure AD B2B-scenario's als gastgebruikers (Zie [wat is Azure B2B-samenwerking](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)) verbinding kunnen maken met SQL-Database en SQL Data Warehouse uitsluitend als onderdeel van de leden van een groep in de huidige Azure AD hebt gemaakt en toegewezen handmatig met de Transact-SQL `CREATE USER` -instructie in een bepaalde database. Bijvoorbeeld, als `steve@gmail.com` naar Azure AD wordt verzocht `contosotest` (met het Azure Ad-domein `contosotest.onmicrosoft.com`), een Azure AD-groep, zoals `usergroup` moeten worden gemaakt in de Azure AD waarin de `steve@gmail.com` lid. Vervolgens deze groep moet worden gemaakt voor een specifieke database (dat wil zeggen MijnDatabase) door Azure AD-SQL-beheerder of Azure AD DBO door het uitvoeren van een Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` instructie. Nadat de databasegebruiker is gemaakt, klikt u vervolgens de gebruiker `steve@gmail.com` zich aanmelden op `MyDatabase` met de optie voor verificatie van SSMS `Active Directory – Universal with MFA support`. Standaard heeft alleen de machtiging connect en eventuele verdere toegang tot gegevens die moet worden verleend in de normale wijze de usergroup. Houd rekening met die gebruiker `steve@gmail.com` als gastgebruiker moet het selectievakje in en voeg de naam van het AD-domein `contosotest.onmicrosoft.com` in de SSMS **verbindingseigenschap** in het dialoogvenster. De **AD-domein naam of het tenant-ID** optie wordt alleen ondersteund voor de universele met MFA verbindingsopties, anders het wordt grijs.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universele verificatie beperkingen voor de SQL-Database en SQL Data Warehouse
- SSMS en SqlPackage.exe zijn de enige hulpprogramma's die momenteel is ingeschakeld voor MFA via universele verificatie van Active Directory.
- SSMS versie 17,2, ondersteunt gelijktijdige toegang door meerdere gebruikers met Universal verificatie met MFA. Een logboek in beperkt versie 17,0 en 17.1, voor een exemplaar van SSMS met behulp van universele verificatie aan één Azure Active Directory-account. Als u wilt zich aanmelden als een andere Azure AD-account, moet u een ander exemplaar van SSMS. (Deze beperking is beperkt tot Universal verificatie van Active Directory; u kunt aanmelden bij andere servers met Active Directory-wachtwoordverificatie of geïntegreerde verificatie van Active Directory SQL Server-verificatie).
- SSMS ondersteunt Universal verificatie van Active Directory voor Object Explorer, Query-Editor en Query Store visualisatie.
- SSMS versie 17,2 biedt DacFx Wizard ondersteuning voor de database van de gegevens uitpakken-Export/implementeren. Als een specifieke gebruiker is geverifieerd via de eerste verificatie-dialoogvenster met Universal-verificatie, de DacFx Wizard functies dezelfde manier als dit het geval is voor alle andere verificatiemethoden te gebruiken.
- De ontwerpfunctie SSMS ondersteunt geen universele verificatie.
- Er zijn geen extra software-vereisten voor universele verificatie van Active Directory, behalve dat u een ondersteunde versie van SSMS moet gebruiken.  
- De versie van de Active Directory Authentication Library (ADAL) voor Universal verificatie is bijgewerkt naar de nieuwste ADAL.dll 3.13.9 beschikbaar uitgebrachte versie. Zie [Active Directory Authentication Library 3.14.1](http://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).  


## <a name="next-steps"></a>Volgende stappen

- Zie voor configuratiestappen [Azure SQL-Database configureren multi-factor authentication voor SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).
- Andere gebruikers toegang verlenen tot de database: [SQL Database-verificatie en autorisatie: toegang verlenen](sql-database-manage-logins.md)  
- Zorg ervoor dat anderen verbinding kunnen maken via de firewall: [een Azure SQL Database serverniveau firewallregel met de Azure portal configureren](sql-database-configure-firewall-settings.md)  
- [Configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md)  
- [Framework voor Microsoft SQL Server-Gegevenslaagtoepassingen (17.0.0 GA)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx)  
- [Een Bacpac-bestand importeren in een nieuwe Azure SQL Database](../sql-database/sql-database-import.md)  
- [Een Azure SQL database naar een Bacpac-bestand exporteren](../sql-database/sql-database-export.md)  
- C#-interface [IUniversalAuthProvider-Interface](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
- Wanneer u **Active Directory - universele met MFA** verificatie, ADAL tracering is beschikbaar vanaf [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Uit standaard, u kunt ADAL kunt tracering inschakelen met behulp van de **extra**, **opties** menu onder **Azure Services**, **Azure-Cloud**,  **ADAL-venster uitvoer het traceringsniveau**, gevolgd door het inschakelen van **uitvoer** in de **weergave** menu. De traceringen zijn beschikbaar in het uitvoervenster weergegeven bij het selecteren van **Azure Active Directory-optie**.  
