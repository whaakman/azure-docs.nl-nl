---
title: Multi-factor authentication - Azure SQL configureren | Microsoft Docs
description: Informatie over het gebruik van Multi-Factored verificatie met SSMS voor SQL-Database en SQL Data Warehouse.
services: sql-database
documentationcenter: 
author: GithubMirek
manager: johammer
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/27/2017
ms.author: mireks
ms.openlocfilehash: fe056288fbc04db0a6892c9a501c58ed85967734
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="configure-multi-factor-authentication-for-sql-server-management-studio-and-azure-ad"></a>Multi-factor authentication voor SQL Server Management Studio en Azure AD configureren

Dit onderwerp leest u hoe u Azure Active Directory multi-factor authentication (MFA) met SQL Server Management Studio. Azure AD MFA kan worden gebruikt bij het verbinden van SSMS of SqlPackage.exe met Azure SQL Database en Azure SQL Data Warehouse.

Zie voor een overzicht van Azure SQL Database multi-factor authentication [Universal verificatie met SQL-Database en SQL Data Warehouse (SSMS ondersteuning voor MFA)](sql-database-ssms-mfa-authentication.md).

## <a name="configuration-steps"></a>Configuratiestappen

1. **Een Azure Active Directory configureren** - voor meer informatie Zie [beheer van uw Azure AD-directory](https://msdn.microsoft.com/library/azure/hh967611.aspx), [uw on-premises identiteiten integreren met Azure Active Directory](../active-directory/active-directory-aadconnect.md), [Uw eigen domeinnaam toevoegen aan Azure AD](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), [Microsoft Azure biedt nu ondersteuning voor federatie met Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/), en [beheren Azure AD dat gebruikmaakt van Windows PowerShell ](https://msdn.microsoft.com/library/azure/jj151815.aspx).
2. **MFA configureren** - voor stapsgewijze instructies, Zie [wat is Azure multi-factor Authentication?](../multi-factor-authentication/multi-factor-authentication.md), [voorwaardelijke toegang (MFA) met Azure SQL Database en datawarehouse](sql-database-conditional-access.md). (Volledige voorwaardelijke toegang is vereist voor een Premium Azure Active Directory (Azure AD). Beperkte MFA is beschikbaar met een standaard Azure AD.)
3. **Configureer SQL-Database of SQL Data Warehouse voor Azure AD Authentication** - voor stapsgewijze instructies Zie [verbinding maken met SQL-Database of SQL Data Warehouse door met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md).
4. **Downloaden van SSMS** - op de clientcomputer, download de meest recente SSMS van [Download SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx). Gebruik ten minste versie 17,2 juli 2017 voor de functies in dit onderwerp.  

## <a name="connecting-by-using-universal-authentication-with-ssms"></a>Verbinding maken met behulp van universele verificatie met SSMS

De volgende stappen laten zien hoe verbinding maken met SQL-Database of SQL Data Warehouse met behulp van de meest recente SSMS.

1. Verbinding maken via de universele verificatie op de **verbinding maken met Server** dialoogvenster, **Active Directory - Universal met ondersteuning voor MFA**. (Als u ziet **Universal verificatie van Active Directory** u zich niet op de meest recente versie van SSMS.)  
   ![1mfa-universal-verbinding][1]  
2. Voltooi de **gebruikersnaam** vak met de Azure Active Directory-referenties in de notatie `user_name@domain.com`.  
   ![1mfa-universal-connect-gebruiker](./media/sql-database-ssms-mfa-auth/1mfa-universal-connect-user.png)   
3. Als u verbinding als gastgebruiker maakt, klikt u op **opties**, en klik op de **verbindingseigenschap** in het dialoogvenster voltooid de **AD-domein naam of het tenant-ID** vak. Zie voor meer informatie [Universal verificatie met SQL-Database en SQL Data Warehouse (SSMS ondersteuning voor MFA)](sql-database-ssms-mfa-authentication.md).
   ![MFA-tenant-ssms](./media/sql-database-ssms-mfa-auth/mfa-tenant-ssms.png)   
4. Zoals gebruikelijk voor SQL-Database en SQL Data Warehouse, klikt u op **opties** en geef de database op de **opties** in het dialoogvenster. (Als de gebruiker verbonden gastgebruiker (dat wil zeggen joe@outlook.com), moet u het selectievakje en de naam van de huidige AD-domein toevoegen of tenant-ID als onderdeel van de opties. Zie [Universal verificatie met SQL-Database en SQL Data Warehouse (SSMS ondersteuning voor MFA)]()(sql-database-ssms-mfa-authentication.md. Klik vervolgens op **Connect**.  
5. Wanneer de **aanmelden bij uw account** dialoogvenster wordt weergegeven, geeft u de account en wachtwoord van uw Azure Active Directory-identiteit. Er is geen wachtwoord is vereist als een gebruiker deel van een domein dat gefedereerd met Azure AD uitmaakt.  
   ![2mfa-aanmeldingspagina][2]  

   > [!NOTE]
   > Voor universele verificatie met een account dat geen MFA vereist, u verbinding maken op dit moment. Voor gebruikers MFA verplicht stellen, doorgaan met de volgende stappen uit:
   >  
   
6. Twee dialoogvensters voor MFA-setup wordt mogelijk weergegeven. Deze eenmalige bewerking afhankelijk is van de MFA-beheerder instellen en daarom mogelijk optioneel. Voor een domein MFA ingeschakeld in deze stap is het soms vooraf gedefinieerde (bijvoorbeeld, het domein vereist voor gebruikers met een smartcard en de pincode).  
   ![3mfa setup][3]  
7. De tweede mogelijke één keer in het dialoogvenster kunt u de details van de verificatiemethode selecteren. De mogelijke opties worden geconfigureerd door de beheerder.  
   ![4mfa-controleren 1][4]  
8. De Azure Active Directory wordt de waarin informatie aan u verzonden. Wanneer u de verificatiecode ontvangt, voert u deze in de **Voer de verificatiecode** vak en klikt u op **aanmelden**.  
   ![5mfa-controleren-2][5]  

Als verificatie voltooid is, verbindt SSMS normaal vermoeden geldige referenties en firewalltoegang.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van Azure SQL Database multi-factor authentication, Universal verificatie met [SQL-Database en SQL Data Warehouse (SSMS ondersteuning voor MFA)](sql-database-ssms-mfa-authentication.md).  
- Andere gebruikers toegang verlenen tot de database: [SQL Database-verificatie en autorisatie: toegang verlenen](sql-database-manage-logins.md)  
- Zorg ervoor dat anderen verbinding kunnen maken via de firewall: [een Azure SQL Database serverniveau firewallregel met de Azure portal configureren](sql-database-configure-firewall-settings.md)  
- Wanneer u **Active Directory - universele met MFA** verificatie, ADAL tracering is beschikbaar vanaf [SSMS 17,3](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms). Uit standaard, u kunt ADAL kunt tracering inschakelen met behulp van de **extra**, **opties** menu onder **Azure Services**, **Azure-Cloud**,  **ADAL-venster uitvoer het traceringsniveau**, gevolgd door het inschakelen van **uitvoer** in de **weergave** menu. De traceringen zijn beschikbaar in het uitvoervenster weergegeven bij het selecteren van **Azure Active Directory-optie**.   


[1]: ./media/sql-database-ssms-mfa-auth/1mfa-universal-connect.png
[2]: ./media/sql-database-ssms-mfa-auth/2mfa-sign-in.png
[3]: ./media/sql-database-ssms-mfa-auth/3mfa-setup.png
[4]: ./media/sql-database-ssms-mfa-auth/4mfa-verify-1.png
[5]: ./media/sql-database-ssms-mfa-auth/5mfa-verify-2.png

