---
title: Multi-factor authentication - Azure SQL configureren | Microsoft Docs
description: Informatie over het gebruik van Multi-Factored verificatie met SSMS voor SQL-Database en SQL Data Warehouse.
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
ms.date: 09/25/2018
ms.openlocfilehash: 98d48659225d89f476ca1e3cc437245bc9e9f753
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564813"
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Multi-factor authentication voor SQL Server Management Studio en Azure AD configureren

Dit onderwerp ziet u hoe u Azure Active Directory multi-factor authentication (MFA) gebruiken met SQL Server Management Studio. Azure AD MFA kan worden gebruikt bij het verbinden van SSMS of SqlPackage.exe met Azure [SQL-Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md). Zie voor een overzicht van Azure SQL Database multi-factor authentication, [universele authenticatie met SQL-Database en SQL Data Warehouse (SSMS-ondersteuning voor MFA)](sql-database-ssms-mfa-authentication.md).

> [!NOTE]
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.

## <a name="configuration-steps"></a>Configuratiestappen

1. **Configureren van een Azure Active Directory** - voor meer informatie, Zie [beheren van uw Azure AD-directory](https://msdn.microsoft.com/library/azure/hh967611.aspx), [uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/hybrid/whatis-hybrid-identity.md), [ Uw eigen domeinnaam toevoegen aan Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure ondersteunt nu Federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), en [beheren Azure AD met behulp van Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **MFA configureren** - voor stapsgewijze instructies, Zie [wat is Azure multi-factor Authentication?](../active-directory/authentication/multi-factor-authentication.md), [voorwaardelijke toegang (MFA) met Azure SQL Database en Data Warehouse](sql-database-conditional-access.md). (Volledige voorwaardelijke toegang is vereist voor een Premium Azure Active Directory (Azure AD). Beperkte MFA is beschikbaar met een standaard Azure AD.)
3. **SQL-Database of SQL Data Warehouse, Azure AD-verificatie configureren** - voor stapsgewijze instructies, Zie [verbinding maken met SQL-Database of SQL Data Warehouse door met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md).
4. **SSMS downloaden** : op de clientcomputer, download de nieuwste SSMS van [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Gebruik ten minste juli 2017, versie 17,2 voor de functies in dit onderwerp.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Verbinding maken via universele authenticatie met SSMS

De volgende stappen laten zien hoe u verbinding maakt met SQL Database of SQL Data Warehouse met behulp van de nieuwste SSMS.

1. Verbinding maken met behulp van universele authenticatie op de **verbinding maken met Server** in het dialoogvenster, selecteer **Active Directory - Universal met ondersteuning voor MFA**. (Als er **Universal verificatie van Active Directory** u zich niet op de meest recente versie van SSMS.)  
   ![1mfa-universal-connect][1]  
2. Voltooi de **gebruikersnaam** vak met de Azure Active Directory-referenties, in de indeling `user_name@domain.com`.  
   ![1mfa-universal-connect-user](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Als u verbinding als een gastgebruiker maakt, klikt u op **opties**, en klik op de **verbindingseigenschap** in het dialoogvenster voltooid de **AD-domein of tenant-ID** vak. Zie voor meer informatie, [universele authenticatie met SQL-Database en SQL Data Warehouse (SSMS-ondersteuning voor MFA)](sql-database-ssms-mfa-authentication.md).
   ![mfa-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. Zoals gewoonlijk voor SQL Database en SQL Data Warehouse, moet u klikken op **opties** en geeft u de database op de **opties** in het dialoogvenster. (Als de gebruiker van de verbinding een gastgebruiker is (dat wil zeggen joe@outlook.com), moet u het selectievakje en de naam van de huidige AD-domein toevoegen of tenant-ID als onderdeel van de opties. Zie [universele authenticatie met SQL-Database en SQL Data Warehouse (SSMS-ondersteuning voor MFA)](sql-database-ssms-mfa-authentication.md). Klik vervolgens op **Verbinden**.  
5. Wanneer de **aanmelden bij uw account** in het dialoogvenster wordt weergegeven, geeft de account en het wachtwoord van uw Azure Active Directory-identiteit. Er is geen wachtwoord is vereist als een gebruiker deel van een domein dat gefedereerd met Azure AD uitmaakt.  
   ![2mfa-sign-in][2]  

   > [!NOTE]
   > Voor universele authenticatie met een account dat geen MFA vereist, verbindt u op dit moment. Voor gebruikers MFA vereisen, doorgaan met de volgende stappen uit:
   >  
   
6. Twee dialoogvensters voor MFA-instellingen mogelijk weergegeven. Deze één keer-bewerking is afhankelijk van de beheerder van de MFA instellen en daarom optioneel zijn. Voor een domein MFA is ingeschakeld in deze stap is het soms vooraf gedefinieerde (bijvoorbeeld, het domein vereist voor gebruikers met een smartcard en de pincode).  
   ![3mfa-setup][3]  
7. De tweede mogelijke één keer in het dialoogvenster kunt u de details van de verificatiemethode selecteren. De mogelijke opties zijn geconfigureerd door uw beheerder.  
   ![4mfa-verify-1][4]  
8. De Azure Active Directory verzendt de bevestigende informatie voor u. Wanneer u de verificatiecode ontvangt, voert u deze in de **Voer de verificatiecode** vak en klikt u op **aanmelden**.  
   ![5mfa-verify-2][5]  

Als verificatie voltooid is, maakt SSMS verbinding normaal ervan uit geldige referenties en firewalltoegang.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure SQL Database multi-factor authentication, universele authenticatie met [SQL-Database en SQL Data Warehouse (SSMS-ondersteuning voor MFA)](sql-database-ssms-mfa-authentication.md).  
- Anderen toegang verlenen tot uw database: [SQL Database-verificatie en autorisatie: Het verlenen van toegang](sql-database-manage-logins.md)  
- Zorg ervoor dat anderen verbinding kunnen maken via de firewall: [Een Azure SQL Database-firewallregel op serverniveau met de Azure-portal configureren](sql-database-configure-firewall-settings.md)  
- Bij het gebruik van **Active Directory - Universal met MFA** verificatie ADAL tracering is beschikbaar in [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Uitgeschakeld standaard, kunt u ADAL tracering met behulp van de **extra**, **opties** menu onder **Azure Services**, **Azure-Cloud**,  **ADAL-venster uitvoer het traceringsniveau**, gevolgd door in te schakelen **uitvoer** in de **weergave** menu. De traceringen zijn beschikbaar in het uitvoervenster weergegeven bij het selecteren van **Azure Active Directory-optie**.   


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

