---
title: 'Azure Portal: een SQL-database maken | Microsoft Docs'
description: Meer informatie over hoe u met behulp van Azure Portal een logische SQL Database-server, een firewallregel op serverniveau en databases maakt. U leert ook hoe u query&quot;s uitvoert op een Azure SQL-database met behulp van Azure Portal.
keywords: zelfstudie over sql-database, een sql-database maken
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 04/03/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 303cb9950f46916fbdd58762acd1608c925c1328
ms.openlocfilehash: c0c6cdf8aa48568b7a4468dd87b2896f94fc1bf6
ms.lasthandoff: 04/04/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Een Azure SQL-database maken in Azure Portal

In deze Quick Start leert u hoe u een SQL-database maakt in Azure.  Azure SQL Database is een 'Database-as-a-Service'-aanbieding waarmee u maximaal beschikbare SQL Server-databases kunt uitvoeren en schalen in de cloud.  In deze Quick Start ziet u hoe u aan de slag gaat door een nieuwe SQL-database te maken met behulp van Azure Portal.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Een SQL-database maken

Een Azure SQL-database wordt gemaakt met een gedefinieerde set [reken- en opslagresources](sql-database-service-tiers.md). De database is gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en in een [logische Azure SQL Database-server](sql-database-features.md). 

Volg deze stappen voor het maken van een SQL-database met de voorbeeldgegevens van Adventure Works LT. 

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **SQL-database** op de pagina **Databases**.

    ![database-1 maken](./media/sql-database-get-started/create-database-1.png)

3. Vul het formulier SQL Database in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:     
   - Databasenaam: **mySampleDatabase**
   - Resourcegroep: **myResourceGroup**
   - Bron: **Voorbeeld (AdventureWorksLT)**

4. Klik op **Server** als u een nieuwe server voor de nieuwe database wilt maken en configureren. Vul het **nieuwe serverformulier** in, waarbij u een unieke servernaam opgeeft. Verstrek ook een naam voor de beheerdersaanmelding bij de server en een zelfgekozen wachtwoord. 

    ![database-server maken](./media/sql-database-get-started/create-database-server.png)
5. Klik op **Selecteren**.

6. Klik op **Prijscategorie** om de servicelaag en het prestatieniveau voor de nieuwe database op te geven. Voor deze Quick Start selecteert u **20 DTU's** en **250** GB opslag

    ![database-s1 maken](./media/sql-database-get-started/create-database-s1.png)

7. Klik op **Toepassen**.  

8. Klik op **Maken** om de database in te richten. De inrichting duurt een paar minuten. 

9. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.

    ![melding](./media/sql-database-get-started/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

De service SQL Database maakt een firewall op serverniveau die voorkomt dat externe toepassingen en hulpmiddelen verbinding maken met de server of databases op de server, tenzij er een firewallregel is gemaakt om de firewall te openen voor specifieke IP-adressen. Volg deze stappen om een [SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) te maken voor het IP-adres van de client en connectiviteit via de SQL Database-firewall alleen voor uw IP-adres toe te staan. 

1. Nadat de implementatie is voltooid, klikt u op **SQL-databases** in het menu links en klikt u op de pagina **SQL-databases** op de nieuwe database **mySampleDatabase**. De overzichtspagina voor de database wordt geopend, waarin de volledig gekwalificeerde naam (zoals **mynewserver20170327.database.windows.net**) en opties voor verdere configuratie worden weergegeven.

      ![serverfirewallregel](./media/sql-database-get-started/server-firewall-rule.png) 

2. Klik op de werkbalk op **Serverfirewall instellen** zoals in de vorige afbeelding is weergegeven. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend. 

3. Klik op **IP van client toevoegen** op de werkbalk en klik vervolgens op **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres.

      ![serverfirewallregel instellen](./media/sql-database-get-started/server-firewall-rule-set.png) 

4. Klik op **OK** en vervolgens op **X** om de pagina **Firewallinstellingen** te sluiten.

U kunt nu verbinding maken met de database en de bijbehorende server met behulp van SQL Server Management Studio of een ander hulpprogramma naar keuze. Dit doet u vanaf dit IP-adres via het serverbeheerdersaccount dat eerder is gemaakt.

## <a name="query-the-sql-database"></a>Query's uitvoeren op de SQL-database

Tijdens het maken van de SQL-database hebben we deze gevuld met de voorbeelddatabase **AdventureWorksLT** (dit was een van de opties die we in Gebruikersinterface maken, eerder in deze QuickStart, hebben geselecteerd). Nu gaan we het ingebouwde queryprogramma in Azure Portal gebruiken om de gegevens op te vragen. 

1. Klik op de pagina SQL Database voor uw database op **Extra** op de werkbalk. De pagina **Extra** wordt geopend.

     ![menu extra](./media/sql-database-get-started/tools-menu.png) 

2. Klik achtereenvolgens op **Query-editor (preview)**, op het selectievakje **Preview-voorwaarden** en op **OK**. De pagina Query-editor wordt geopend.

3. Klik op **Aanmelden** en selecteer wanneer hierom wordt gevraagd **SQL serververificatie**. Geef vervolgens de gebruikersnaam en het wachtwoord van de serverbeheerder op die u eerder hebt gemaakt.

    ![aanmelding](./media/sql-database-get-started/login.png) 

4. Klik op **OK** om u aan te melden.

5. Wanneer u bent geverifieerd, typt u de volgende query in het deelvenster van de query-editor:

   ```
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klik op **Uitvoeren** en bekijk de resultaten van de query in het deelvenster **Resultaten**.

    ![resultaten queryeditor](./media/sql-database-get-started/query-editor-results.png)

7. Klik op **X** om de pagina **Query-editor** te sluiten en klik nogmaals op **X** om de pagina **Extra** te sluiten.

## <a name="clean-up-resources"></a>Resources opschonen

Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. Als u van plan bent om door te gaan met andere Quick Starts of met de zelfstudies, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die door deze Quick Start in Azure Portal zijn gemaakt.

1. Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myResourceGroup**. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myResourceGroup** in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Als u verbinding wilt maken en query's wilt uitvoeren met behulp van SQL Server Management Studio, raadpleegt u [Verbinding maken en query's uitvoeren met SSMS](sql-database-connect-query-ssms.md)
- Zie [Verbinding maken en query's uitvoeren met Visual Studio](sql-database-connect-query.md) als u verbinding wilt maken met Visual Studio.
- Zie voor een technisch overzicht van de SQL Database [Over de SQL Database-service](sql-database-technical-overview.md).

