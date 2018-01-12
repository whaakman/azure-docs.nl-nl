---
title: Voorwaardelijke toegang - Azure SQL Database en datawarehouse | Microsoft-document
description: Informatie over het configureren van voorwaardelijke toegang voor Azure SQL Database en datawarehouse.
services: sql-database
author: GithubMirek
manager: johammer
ms.custom: security
ms.service: sql-database
ms.topic: article
ms.date: 06/07/2017
ms.author: mireks
ms.workload: Inactive
ms.openlocfilehash: b1c569e414de292b59e0915483d45bf011edb1a6
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Voorwaardelijke toegang (MFA) met Azure SQL Database en datawarehouse  

Ondersteuning voor voorwaardelijke toegang van Microsoft SQL Database- en SQL Data Warehouse. De volgende stappen laten zien hoe SQL-Database voor het afdwingen van beleid voor voorwaardelijke toegang configureren.  

## <a name="prerequisites"></a>Vereisten  
- U moet uw SQL-Database of SQL Data Warehouse ter ondersteuning van Azure Active Directory-verificatie configureren. Zie voor specifieke stappen [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md).  
- Als multi-factor authentication-server is ingeschakeld, moet u verbinding maken met op ondersteunde hulpprogramma, zoals de meest recente SSMS. Zie voor meer informatie [Azure SQL-Database configureren multi-factor authentication voor SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>CA configureren voor Azure SQL DB/DW  
1.  Aanmelden bij de Portal, selecteer **Azure Active Directory**, en selecteer vervolgens **voorwaardelijke toegang**. Zie voor meer informatie [technische documentatie voor Azure Active Directory voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
  ![blade voor voorwaardelijke toegang](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2.  In de **-beleid voor voorwaardelijke toegang** blade, klikt u op **nieuw beleid**, Geef een naam op en klik vervolgens op **regels configureren**.  
3.  Onder **toewijzingen**, selecteer **gebruikers en groepen**, Controleer **gebruikers en groepen selecteren**, en selecteer vervolgens de gebruiker of groep voor voorwaardelijke toegang. Klik op **Selecteer**, en klik vervolgens op **gedaan** te accepteren van uw selectie.  
  ![gebruikers en groepen selecteren](./media/sql-database-conditional-access/select-users-and-groups.png)  

4.  Selecteer **Cloud-apps**, klikt u op **apps selecteren**. Ziet u alle apps beschikbaar voor voorwaardelijke toegang. Selecteer **Azure SQL Database**, onderaan op **Selecteer**, en klik vervolgens op **gedaan**.  
  ![SQL-Database selecteren](./media/sql-database-conditional-access/select-sql-database.png)  
  Als u niet kunt vinden **Azure SQL Database** wordt vermeld in de volgende derde schermopname, de volgende stappen uitvoeren:   
  - Aanmelden bij uw Azure SQL DB/DW-exemplaar met behulp van SSMS met een AAD-beheerdersaccount.  
  - Uitvoeren van `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
  - Aanmelden bij AAD en controleer of Azure SQL Database en datawarehouse worden vermeld in de toepassingen in uw AAD.  

5.  Selecteer **toegangscontroles**, selecteer **Grant**, en controleert u het beleid dat u wilt toepassen. In dit voorbeeld selecteren we **meervoudige authenticatie**.  
  ![Selecteer toegang verlenen](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Samenvatting  
De geselecteerde toepassing (Azure SQL Database), zodat verbinding maken naar Azure SQL DB/DW met behulp van Azure AD Premium worden nu afgedwongen voor het geselecteerde beleid voor voorwaardelijke toegang, **vereist multi-factor authentication-server.**  
Voor vragen over Azure SQL Database en datawarehouse met betrekking tot de multi-factor authentication-server contact op met MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Volgende stappen  

Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md).
