---
title: 'Azure Portal: een SQL-database maken | Microsoft Docs'
description: Leer hoe u met Azure Portal een logische SQL Database-server, een firewallregel op serverniveau en een database maakt.
keywords: zelfstudie over sql-database, een sql-database maken
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: Active
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: quickstart
ms.date: 01/10/2018
ms.author: ninarn
ms.openlocfilehash: e438613e3913eb88232c7b2a4b5280f6890f478e
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
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

2. Selecteer **Databases** op de pagina **Nieuw** en selecteer **Maken** onder **SQL-database** op de pagina **Nieuw**.

   ![database-1 maken](./media/sql-database-get-started-portal/create-database-1.png)

3. Vul het formulier SQL Database in met de volgende informatie, zoals in de voorgaande afbeelding wordt weergegeven:   

   | Instelling       | Voorgestelde waarde | Beschrijving |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Databasenaam** | mySampleDatabase | Zie [Database-id's](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) voor geldige databasenamen. |
   | **Abonnement** | Uw abonnement  | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep**  | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Bron selecteren** | Sample (AdventureWorksLT) | Hiermee worden het schema en de gegevens van AdventureWorksLT in de nieuwe database geladen. |

   > [!IMPORTANT]
   > U moet de voorbeelddatabase op dit formulier selecteren. Deze wordt namelijk in de rest van deze Quick Start gebruikt.
   >

4. Klik onder **Server** op **Vereiste instellingen configureren** en vul het formulier SQL-server (logische server) in met de volgende informatie, zoals in de volgende afbeelding wordt weergegeven:   

   | Instelling       | Voorgestelde waarde | Beschrijving |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Servernaam** | Een wereldwijd unieke naam | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige servernamen. |
   | **Aanmeldgegevens van serverbeheerder** | Een geldige naam | Zie [Database-id's](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) voor geldige aanmeldingsnamen. |
   | **Wachtwoord** | Een geldig wachtwoord | Uw wachtwoord moet uit ten minste 8 tekens bestaan en moet tekens bevatten uit drie van de volgende categorieën: hoofdletters, kleine letters, cijfers en niet-alfanumerieke tekens. |
   | **Abonnement** | Uw abonnement | Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen. |
   | **Resourcegroep** | myResourceGroup | Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige resourcegroepnamen. |
   | **Locatie** | Een geldige locatie | Zie [Azure-regio's](https://azure.microsoft.com/regions/) voor informatie over regio's. |

   > [!IMPORTANT]
   > De beheerdersaanmelding bij de server en het wachtwoord die u hier opgeeft, zijn vereist voor aanmelding bij de server en de bijbehorende databases verderop in deze Quick Start. Onthoud of noteer deze informatie voor later gebruik.
   >  

   ![database-server maken](./media/sql-database-get-started-portal/create-database-server.png)

5. Wanneer u het formulier hebt ingevuld, klikt u op **Selecteren**.

6. Klik op **Prijscategorie** om de servicelaag, het aantal DTU's en de hoeveelheid opslag op te geven. Bekijk de opties voor de hoeveelheid DTU's en opslag die voor elke servicelaag beschikbaar zijn.

   > [!IMPORTANT]
   > \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie.
   >
   >\* In de Premium-laag is op dit moment opslag van meer dan 1 TB beschikbaar in de volgende regio's: VS Oost 2, VS West, VS (overheid) Virginia, West-Europa, Duitsland Centraal, Zuidoost-Azië, Japan - oost, Australië - oost, Canada Centraal en Canada Oost. Zie [P11-P15: huidige beperkingen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   >

7. Voor deze zelfstudie selecteert u de servicelaag **Standard** en gebruikt u vervolgens de schuifregelaar om **100 DTU's (S3)** en **400** GB aan opslagruimte te selecteren.

   ![database-s1 maken](./media/sql-database-get-started-portal/create-database-s1.png)

8. Accepteer de gebruiksvoorwaarden voor de preview om de optie **Extra opslag** te gebruiken.

   > [!IMPORTANT]
   > \* Opslagruimten groter dan de hoeveelheid inbegrepen opslagruimte zijn in preview en hiervoor gelden extra kosten. Zie [de prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie.
   >
   >\* In de Premium-laag is op dit moment opslag van meer dan 1 TB beschikbaar in de volgende regio's: VS Oost 2, VS West, VS (overheid) Virginia, West-Europa, Duitsland Centraal, Zuidoost-Azië, Japan - oost, Australië - oost, Canada Centraal en Canada Oost. Zie [P11-P15: huidige beperkingen](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
   >

9. Als u de servicelaag, het aantal DTU's en de hoeveelheid opslagruimte hebt geselecteerd, klikt u op **Toepassen**.  

10. Nu u het SQL Database-formulier hebt ingevuld, klikt u op **Maken** om de database in te richten. De inrichting duurt een paar minuten.

11. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.

     ![melding](./media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

De service SQL Database maakt een firewall op serverniveau die voorkomt dat externe toepassingen en hulpmiddelen verbinding maken met de server of databases op de server, tenzij er een firewallregel is gemaakt om de firewall te openen voor specifieke IP-adressen. Volg deze stappen om een [SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) te maken voor het IP-adres van de client en connectiviteit via de SQL Database-firewall alleen voor uw IP-adres toe te staan.

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 1433 openstelt.
>

1. Wanneer de implementatie is voltooid, klikt u op **SQL Databases** in het menu aan de linkerkant. Klik vervolgens op de pagina **SQL Databases** op **mySampleDatabase**. De overzichtspagina voor uw database wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170824.database.windows.net**) en opties voor verdere configuratie.

2. Kopieer deze volledig gekwalificeerde servernaam om in volgende Quick Starts verbinding te maken met de server en de bijbehorende databases.

   ![servernaam](./media/sql-database-get-started-portal/server-name.png)

3. Klik op de werkbalk op **Serverfirewall instellen** zoals in de vorige afbeelding is weergegeven. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend.

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Klik op **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

5. Klik op **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de logische server wordt geopend.

6. Klik op **OK** en sluit de pagina **Firewallinstellingen**.

U kunt nu verbinding maken met de SQL Database-server en de bijbehorende databases met behulp van SQL Server Management Studio of een ander hulpprogramma naar keuze. Dit doet u vanaf dit IP-adres via het serverbeheerdersaccount dat eerder is gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL Database-firewall standaard ingeschakeld. Klik op **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.
>

## <a name="query-the-sql-database"></a>Query's uitvoeren op de SQL-database

Nu u een voorbeelddatabase in Azure hebt gemaakt, gebruiken we het ingebouwde hulpprogramma voor query's binnen Azure Portal om te bevestigen dat u verbinding kunt maken met de database en query's kunt uitvoeren voor de gegevens.

1. Ga op de SQL Database-pagina voor uw database in het linkermenu naar **Data Explorer (preview)** en klik hierop

   ![queryeditor zoeken](./media/sql-database-get-started-portal/find-query-editor.PNG)

2. Klik op **Aanmelden**, controleer de aanmeldingsgegevens en klik op **OK** om u aan te melden met behulp van verificatie bij de SQL-server met de aanmeldingsgegevens en het wachtwoord voor de server die u eerder hebt gemaakt.

   ![aanmelding](./media/sql-database-get-started-portal/login-menu.png)

3. Klik op **OK** om u aan te melden.

4. Wanneer u bent geverifieerd als **ServerAdmin**, typt u de volgende query in het deelvenster van de query-editor:

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

5. Klik op **Uitvoeren** en bekijk de resultaten van de query in het deelvenster **Resultaten**.

   ![resultaten queryeditor](./media/sql-database-get-started-portal/query-editor-results.png)

6. Sluit de pagina **Data explorer**, klik op **OK** om uw niet-opgeslagen bewerkingen te verwijderen.

## <a name="clean-up-resources"></a>Resources opschonen

Sla deze resources op als u verder wilt gaan met [Volgende stappen](#next-steps) en wilt leren hoe u op verschillende manieren verbinding kunt maken met uw database en deze kunt bevragen. Als u echter de resources wilt verwijderen die u hebt gemaakt in deze Quick Start, voert u de volgende stappen uit.


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
