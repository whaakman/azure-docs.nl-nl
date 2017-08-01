---
title: 'Azure Portal: een SQL-database maken | Microsoft Docs'
description: Meer informatie over hoe u met behulp van Azure Portal een logische SQL Database-server, een firewallregel op serverniveau en databases maakt. U leert ook hoe u query's uitvoert op een Azure SQL-database met behulp van Azure Portal.
keywords: zelfstudie over sql-database, een sql-database maken
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 05/30/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 1571d30890a428fb73d31861c4f1ae395f9de374
ms.contentlocale: nl-nl
ms.lasthandoff: 06/15/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Een Azure SQL-database maken in Azure Portal

In deze Quick Start leert u hoe u een SQL-database maakt in Azure. Azure SQL Database is een Database-as-a-Service-oplossing waarmee u maximaal beschikbare SQL Server-databases kunt uitvoeren en schalen in de cloud. In deze Quick Start ziet u hoe u aan de slag gaat door een SQL-database te maken met behulp van Azure Portal.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Een SQL-database maken

Een Azure SQL-database wordt gemaakt met een gedefinieerde set [reken- en opslagresources](sql-database-service-tiers.md). De database is gemaakt in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en in een [logische Azure SQL Database-server](sql-database-features.md). 

Volg deze stappen voor het maken van een SQL-database met de voorbeeldgegevens van Adventure Works LT. 

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **SQL-database** op de pagina **Databases**.

   ![database-1 maken](./media/sql-database-get-started-portal/create-database-1.png)

3. Vul het formulier SQL Database in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:   

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Databasenaam** | mySampleDatabase | Zie [Database-id's](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) voor geldige databasenamen. | 
   | **Abonnement** | Uw abonnement  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep**  | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Bron selecteren** | Sample (AdventureWorksLT) | Hiermee worden het schema en de gegevens van AdventureWorksLT in de nieuwe database geladen. |

   > [!IMPORTANT]
   > U moet de voorbeelddatabase op dit formulier selecteren. Deze wordt namelijk in de rest van deze Quick Start gebruikt.
   > 

4. Vul het formulier SQL-server (logische server) in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:   

   | Instelling       | Voorgestelde waarde | Beschrijving | 
   | ------------ | ------------------ | ------------------------------------------------- | 
   | **Servernaam** | Een wereldwijd unieke naam | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige servernamen. | 
   | **Aanmeldgegevens van serverbeheerder** | Een geldige naam | Zie [Database-id's](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers) voor geldige aanmeldingsnamen. |
   | **Wachtwoord** | Een geldig wachtwoord | Uw wachtwoord moet uit ten minste 8 tekens bestaan en moet tekens bevatten uit drie van de volgende categorieën: hoofdletters, kleine letters, cijfers en niet-alfanumerieke tekens. |
   | **Abonnement** | Uw abonnement | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep** | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |

   > [!IMPORTANT]
   > De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik. 
   >  

   ![database-server maken](./media/sql-database-get-started-portal/create-database-server.png)

5. Wanneer u het formulier hebt ingevuld, klikt u op **Selecteren**.

6. Klik op **Prijscategorie** om de servicelaag en het prestatieniveau voor de nieuwe database op te geven. Gebruik de schuifregelaar om **20 DTU's** en **250** GB aan opslagruimte te selecteren. Zie [Wat is een DTU?](sql-database-what-is-a-dtu.md) voor meer informatie over DTU's.

   ![database-s1 maken](./media/sql-database-get-started-portal/create-database-s1.png)

7. Nadat u de hoeveelheid DTU's hebt geselecteerd, klikt u op **Toepassen**.  

8. Nu u het SQL Database-formulier hebt ingevuld, klikt u op **Maken** om de database in te richten. De inrichting duurt een paar minuten. 

9. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.

   ![melding](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

De service SQL Database maakt een firewall op serverniveau die voorkomt dat externe toepassingen en hulpmiddelen verbinding maken met de server of databases op de server, tenzij er een firewallregel is gemaakt om de firewall te openen voor specifieke IP-adressen. Volg deze stappen om een [SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) te maken voor het IP-adres van de client en connectiviteit via de SQL Database-firewall alleen voor uw IP-adres toe te staan. 

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 1433 openstelt.
>

1. Wanneer de implementatie is voltooid, klikt u op **SQL Databases** in het menu aan de linkerkant. Klik vervolgens op de pagina **SQL Databases** op **mySampleDatabase**. De overzichtspagina voor uw database wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170313.database.windows.net**) en opties voor verdere configuratie. Kopieer deze volledig gekwalificeerde servernaam voor later gebruik.

   > [!IMPORTANT]
   > U hebt deze volledig gekwalificeerde servernaam nodig om in de volgende Quick Starts verbinding te maken met de server en de bijbehorende databases.
   > 

   ![servernaam](./media/sql-database-connect-query-dotnet/server-name.png) 

2. Klik op de werkbalk op **Serverfirewall instellen** zoals in de vorige afbeelding is weergegeven. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend. 

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png) 

3. Klik op **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

4. Klik op **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de logische server wordt geopend.

   ![serverfirewallregel instellen](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Klik op **OK** en sluit de pagina **Firewallinstellingen**.

U kunt nu verbinding maken met de SQL Database-server en de bijbehorende databases met behulp van SQL Server Management Studio of een ander hulpprogramma naar keuze. Dit doet u vanaf dit IP-adres via het serverbeheerdersaccount dat eerder is gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL Database-firewall standaard ingeschakeld. Klik op **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.
>

## <a name="query-the-sql-database"></a>Query's uitvoeren op de SQL-database

Nu u een voorbeelddatabase in Azure hebt gemaakt, gebruiken we het ingebouwde hulpprogramma voor query's binnen Azure Portal om te bevestigen dat u verbinding kunt maken met de database en query's kunt uitvoeren voor de gegevens. 

1. Klik op de pagina SQL Database voor uw database op **Extra** op de werkbalk. De pagina **Extra** wordt geopend.

   ![menu extra](./media/sql-database-get-started-portal/tools-menu.png) 

2. Klik achtereenvolgens op **Query-editor (preview)**, op het selectievakje **Preview-voorwaarden** en op **OK**. De pagina Query-editor wordt geopend.

3. Klik op **Aanmelden** en selecteer wanneer hierom wordt gevraagd **SQL serververificatie**. Geef vervolgens de gebruikersnaam en het wachtwoord van de serverbeheerder op die u eerder hebt gemaakt.

   ![aanmelding](./media/sql-database-get-started-portal/login.png) 

4. Klik op **OK** om u aan te melden.

5. Wanneer u bent geverifieerd, typt u de volgende query in het deelvenster van de query-editor:

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Klik op **Uitvoeren** en bekijk de resultaten van de query in het deelvenster **Resultaten**.

   ![resultaten queryeditor](./media/sql-database-get-started-portal/query-editor-results.png)

7. Sluit de pagina **Query-editor** en de pagina **Extra**.

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze resources niet voor een andere Quick Start/zelfstudie nodig hebt (zie [Volgende stappen](#next-steps)), kunt u ze verwijderen door het volgende te doen:


1. Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myResourceGroup**. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myResourceGroup** in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Nu u een database hebt, kunt u verbinding maken met een hulpprogramma naar keuze en hiermee query's uitvoeren. Klik op de onderstaande hulpprogramma's voor meer informatie:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

