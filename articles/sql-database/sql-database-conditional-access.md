---
title: Voorwaardelijke toegang - Azure SQL Database en datawarehouse | Microsoft Docs
description: Informatie over het configureren van voorwaardelijke toegang voor Azure SQL Database en Data Warehouse.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: sql-data-warehouse
ms.devlang: ''
ms.topic: conceptual
author: GithubMirek
ms.author: mireks
ms.reviewer: vanto
manager: craigg
ms.date: 03/29/2019
ms.openlocfilehash: 79d15a46affb2a6b7159ba080d4235073c59919c
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648980"
---
# <a name="conditional-access-mfa-with-azure-sql-database-and-data-warehouse"></a>Voorwaardelijke toegang (MFA) met Azure SQL Database en datawarehouse  

Azure [SQL-Database](sql-database-technical-overview.md), [Managed Instance](sql-database-managed-instance.md), en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) ondersteuning voor voorwaardelijke toegang van Microsoft. 

> [!NOTE]
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.

De volgende stappen laten zien hoe SQL Database configureren voor het afdwingen van beleid voor voorwaardelijke toegang.  

## <a name="prerequisites"></a>Vereisten  
- U moet uw SQL-Database of SQL Data Warehouse ter ondersteuning van Azure Active Directory-verificatie configureren. Zie voor specifieke stappen [configureren en beheren van Azure Active Directory-verificatie met SQL-Database of SQL Data Warehouse](sql-database-aad-authentication-configure.md).  
- Wanneer multi-factor authentication is ingeschakeld, moet u verbinding maken met op ondersteunde hulpprogramma, zoals de nieuwste SSMS. Zie voor meer informatie, [Azure SQL-Database configureren multi-factor authentication voor SQL Server Management Studio](sql-database-ssms-mfa-authentication-configure.md).  

## <a name="configure-ca-for-azure-sql-dbdw"></a>CA configureren voor Azure SQL DB/DW  
1. Meld u aan bij de Portal, selecteer **Azure Active Directory**, en selecteer vervolgens **voorwaardelijke toegang**. Zie voor meer informatie, [technische documentatie voor Azure Active Directory voor voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference).  
   ![blade voor voorwaardelijke toegang](./media/sql-database-conditional-access/conditional-access-blade.png) 
     
2. In de **beleid voor voorwaardelijke toegang** blade, klikt u op **nieuw beleid**, Geef een naam op en klik vervolgens op **regels configureren**.  
3. Onder **toewijzingen**, selecteer **gebruikers en groepen**, Controleer **gebruikers en groepen selecteren**, en selecteer vervolgens de gebruiker of groep voor voorwaardelijke toegang. Klik op **Selecteer**, en klik vervolgens op **gedaan** te accepteren van uw selectie.  
   ![Gebruikers en groepen selecteren](./media/sql-database-conditional-access/select-users-and-groups.png)  

4. Selecteer **Cloud-apps**, klikt u op **apps selecteren**. Ziet u alle beschikbare apps voor voorwaardelijke toegang. Selecteer **Azure SQL Database**, klik onderaan op **Selecteer**, en klik vervolgens op **gedaan**.  
   ![SQL-Database selecteren](./media/sql-database-conditional-access/select-sql-database.png)  
   Als u niet kunt vinden **Azure SQL Database** wordt vermeld in de volgende schermafbeelding van de derde, de volgende stappen uitvoeren:   
   - Aanmelden bij uw Azure SQL DB/DW-exemplaar met behulp van SSMS met een AAD-beheerdersaccount.  
   - Voer `CREATE USER [user@yourtenant.com] FROM EXTERNAL PROVIDER`.  
   - Aanmelden bij AAD en controleer of dat Azure SQL Database en datawarehouse worden weergegeven in de toepassingen in uw AAD.  

5. Selecteer **besturingselementen voor toegang**, selecteer **verlenen**, en controleer vervolgens of het beleid dat u wilt toepassen. In dit voorbeeld selecteren we **meervoudige verificatie vereisen**.  
   ![Selecteer toegang verlenen](./media/sql-database-conditional-access/grant-access.png)  

## <a name="summary"></a>Samenvatting  
De geselecteerde toepassing (Azure SQL Database) verbinding naar Azure SQL DB/DW met behulp van Azure AD Premium, zodat wordt nu afgedwongen voor het geselecteerde beleid voor voorwaardelijke toegang, **vereist meervoudige verificatie.**  
Als u vragen hebt over Azure SQL Database en Data Warehouse met betrekking tot verificatie met meerdere factoren, neem contact op met MFAforSQLDB@microsoft.com.  

## <a name="next-steps"></a>Volgende stappen  

Zie voor een zelfstudie [beveiligen van uw Azure SQL Database](sql-database-security-tutorial.md).
