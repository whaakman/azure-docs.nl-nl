---
title: Meervoudige verificatie - Azure SQL | Microsoft Docs
description: Azure SQL Database en Azure SQL Data Warehouse ondersteuning voor verbindingen van SQL Server Management Studio (SSMS) met behulp van universele verificatie van Active Directory.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 90d4756c251103275fe0a37b0c36562b69a0e035
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166653"
---
# <a name="universal-authentication-with-sql-database-and-sql-data-warehouse-ssms-support-for-mfa"></a>Universele authenticatie met SQL-Database en SQL Data Warehouse (SSMS-ondersteuning voor MFA)
Azure SQL Database en Azure SQL Data Warehouse ondersteunt verbindingen van het gebruik van SQL Server Management Studio (SSMS) *Universal verificatie van Active Directory*. 
**Download de nieuwste SSMS** : op de clientcomputer, de meest recente versie van SSMS, downloaden van [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Gebruik ten minste juli 2017, versie 17,2 voor de functies in dit artikel.  De meest recente verbinding in het dialoogvenster ziet er zo: ![1mfa-universal-verbinding](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png "het gebruikersnaamveld is voltooid.")  

## <a name="the-five-authentication-options"></a>De vijf verificatie-opties  
- De twee niet-interactieve verificatiemethoden biedt ondersteuning voor universele verificatie van Active Directory (`Active Directory - Password` verificatie en `Active Directory - Integrated` verificatie). Niet-interactieve `Active Directory - Password` en `Active Directory - Integrated` verificatiemethoden kunnen worden gebruikt in veel verschillende toepassingen (ADO.NET, JDBC, ODBC, enz.). Deze twee methoden worden nooit pop-dialoogvensters leiden.

- `Active Directory - Universal with MFA` verificatie is een interactieve methode die ondersteuning biedt voor *Azure multi-factor Authentication* (MFA). Azure MFA helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers die een eenvoudige aanmeldprocedure willen. Het biedt een robuuste verificatie met een scala aan gebruiksvriendelijke verificatieopties (telefoonoproep, SMS-bericht, smartcards en pincode of mobiele app-meldingen), zodat gebruikers de methode te kiezen die ze liever. Interactieve MFA met Azure AD kan resulteren in een pop-updialoogvenster voor validatie.

Zie voor een beschrijving van multi-factor Authentication, [multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md).
Zie voor de configuratiestappen, [Azure SQL-Database configureren multi-factor authentication voor SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).

### <a name="azure-ad-domain-name-or-tenant-id-parameter"></a>Azure AD domain parameter name of tenant-ID   

Beginnen met [SSMS versie 17](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms), gebruikers die zijn geïmporteerd in de huidige Active Directory van andere Azure Active Directory als gastgebruikers, kunnen de Azure AD-domeinnaam opgeven, of tenant-ID wanneer ze verbinding maken. Gastgebruikers ook kunnen bevatten gebruikers van andere Azure Active Directory, Microsoft-accounts, zoals outlook.com, hotmail.com, live.com of andere accounts zoals gmail.com uitgenodigd. Deze informatie kan **Active Directory Universal met MFA-verificatie** voor het identificeren van de juiste verificatie-instantie. Deze optie is ook vereist voor de ondersteuning van Microsoft-accounts (MSA), zoals outlook.com, hotmail.com, live.com of niet-MSA-accounts. Deze gebruikers die moeten worden geverifieerd met behulp van universele authenticatie moeten hun Azure AD-domein invoeren of tenant-ID. Deze parameter geeft de huidige Azure AD-domein naam/tenant-ID de Azure-Server is gekoppeld aan. Bijvoorbeeld, als Azure-Server is gekoppeld aan Azure AD-domein `contosotest.onmicrosoft.com` waar gebruiker `joe@contosodev.onmicrosoft.com` wordt gehost als geïmporteerde gebruiker uit Azure AD-domein `contosodev.onmicrosoft.com`, de domeinnaam die is vereist voor het verifiëren van deze gebruiker is `contosotest.onmicrosoft.com`. Wanneer de gebruiker een systeemeigen gebruiker van de Azure AD die is gekoppeld aan Azure-Server is en niet een MSA-account is, is geen domein of tenant-ID is vereist. Om in te voeren van de parameter (die begint met SSMS versie 17,2), in de **verbinding maken met Database** dialoogvenster vak, voert u in het dialoogvenster selecteren **Active Directory - Universal met MFA** verificatie, klikt u op  **Opties voor**volledige de **gebruikersnaam** vak en klik vervolgens op de **verbindingseigenschappen** tabblad. Controleer de **AD-domein of tenant-ID** vak en bieden verificatieautoriteit, zoals de naam van het domein (**contosotest.onmicrosoft.com**) of de GUID van de tenant-ID.  
   ![mfa-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   

### <a name="azure-ad-business-to-business-support"></a>Azure AD-ondersteuning voor bedrijven   
Azure AD-gebruikers ondersteund voor Azure AD B2B-scenario's als gastgebruikers (Zie [wat is Azure B2B-samenwerking](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md)) verbinding kunt maken met SQL-Database en SQL Data Warehouse alleen als onderdeel van de leden van een groep gemaakt in de huidige Azure AD en handmatig toegewezen met de Transact-SQL `CREATE USER` -instructie in een bepaalde database. Bijvoorbeeld, als `steve@gmail.com` wordt uitgenodigd voor Azure AD `contosotest` (met het Azure Ad-domein `contosotest.onmicrosoft.com`), een Azure AD-groep, zoals `usergroup` moeten worden gemaakt in de Azure AD met de `steve@gmail.com` lid. Vervolgens deze groep moet worden gemaakt voor een specifieke database (dat wil zeggen MijnDatabase) met Azure AD-SQL-beheerder of Azure AD DBO door het uitvoeren van een Transact-SQL `CREATE USER [usergroup] FROM EXTERNAL PROVIDER` instructie. Nadat de gebruiker van de database is gemaakt, klikt u vervolgens de gebruiker `steve@gmail.com` kunt aanmelden bij `MyDatabase` met behulp van de optie voor verificatie van SSMS `Active Directory – Universal with MFA support`. Standaard heeft alleen de machtiging connect en eventuele verdere toegang tot gegevens die moet worden verleend in de normale manier waarop de gebruikersgroep. Houd er rekening mee dat de gebruiker `steve@gmail.com` als een gastgebruiker moet het selectievakje in en de naam van de AD-domein toevoegen `contosotest.onmicrosoft.com` in de SSMS **verbindingseigenschap** in het dialoogvenster. De **AD-domein of tenant-ID** optie wordt alleen ondersteund voor de universele met MFA verbindingsopties, anders wordt deze grijs.

## <a name="universal-authentication-limitations-for-sql-database-and-sql-data-warehouse"></a>Universele authenticatie beperkingen voor SQL Database en SQL Data Warehouse
- SSMS en SqlPackage.exe zijn de enige hulpprogramma's voor MFA via universele authenticatie van Active Directory op dit moment ingeschakeld.
- SSMS versie 17,2, biedt ondersteuning voor gelijktijdige toegang door meerdere gebruikers met behulp van universele authenticatie met MFA. Versie 17,0 en 17.1, beperkt een aanmelding voor een exemplaar van SSMS met behulp van universele authenticatie naar één Azure Active Directory-account. Om aan te melden als een andere Azure AD-account, moet u een ander exemplaar van SSMS. (Deze beperking is beperkt tot Universal verificatie van Active Directory, kunt u zich aanmelden bij andere servers met Active Directory-wachtwoordverificatie of geïntegreerde verificatie van Active Directory, SQL Server-verificatie).
- SSMS biedt ondersteuning voor universele verificatie van Active Directory voor Object Explorer, Query-Editor en Query Store visualisatie.
- SSMS versie 17,2 biedt DacFx Wizard ondersteuning voor exporteren/Extract/implementeren Data database. Wanneer een specifieke gebruiker wordt geverifieerd via het dialoogvenster voor initiële verificatie met behulp van universele authenticatie, de DacFx-Wizard functies dezelfde manier als dit het geval is voor alle andere verificatiemethoden te gebruiken.
- De ontwerpfunctie SSMS biedt geen ondersteuning voor universele authenticatie.
- Er zijn geen extra software-vereisten voor universele verificatie van Active Directory, behalve dat u moet een ondersteunde versie van SSMS gebruiken.  
- De versie van de Active Directory Authentication Library (ADAL) voor universele authenticatie is bijgewerkt naar de nieuwste ADAL.dll 3.13.9 beschikbaar uitgebrachte versie. Zie [Active Directory Authentication Library 3.14.1](http://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/).  


## <a name="next-steps"></a>Volgende stappen

- Zie voor de configuratiestappen, [Azure SQL-Database configureren multi-factor authentication voor SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).
- Anderen toegang verlenen tot uw database: [SQL Database-verificatie en autorisatie: toegang verlenen tot](sql-database-manage-logins.md)  
- Zorg ervoor dat anderen verbinding kunnen maken via de firewall: [configureren van een Azure SQL Database-firewallregel op serverniveau met de Azure portal](sql-database-configure-firewall-settings.md)  
- [Azure Active Directory-verificatie configureren en beheren met SQL Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md)  
- [Framework voor Microsoft SQL Server-Gegevenslaagtoepassingen (17.0.0 algemene beschikbaarheid)](https://www.microsoft.com/download/details.aspx?id=55088)  
- [SQLPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx)  
- [Een BACPAC-bestand importeren in een nieuwe Azure SQL-database](../sql-database/sql-database-import.md)  
- [Een Azure SQL-database exporteren naar een BACPAC-bestand](../sql-database/sql-database-export.md)  
- C#-interface [IUniversalAuthProvider-Interface](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx)  
- Bij het gebruik van **Active Directory - Universal met MFA** verificatie ADAL tracering is beschikbaar in [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Uitgeschakeld standaard, kunt u ADAL tracering met behulp van de **extra**, **opties** menu onder **Azure Services**, **Azure-Cloud**,  **ADAL-venster uitvoer het traceringsniveau**, gevolgd door in te schakelen **uitvoer** in de **weergave** menu. De traceringen zijn beschikbaar in het uitvoervenster weergegeven bij het selecteren van **Azure Active Directory-optie**.  
