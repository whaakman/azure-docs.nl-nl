---
title: 'Azure Portal: Azure SQL Database doorzoeken met de queryeditor | Microsoft Docs'
description: Leer hoe u verbinding maakt met SQL Database in Azure Portal door gebruik te maken van de SQL-queryeditor. Voer daarna Transact-SQL-instructies (T-SQL) uit om query's uit te voeren voor gegevens en om gegevens te bewerken.
keywords: verbinding maken met sql database, azure portal, portal, queryeditor
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: AyoOlubeko
ms.author: ayolubek
ms.reviewer: carlrab
manager: craigg
ms.date: 12/05/2018
ms.openlocfilehash: 86120e65ad6a3b7fab2a5db78a338bed0e277cb8
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53651944"
---
# <a name="quickstart-use-the-azure-portals-sql-query-editor-to-connect-and-query-data"></a>Snelstartgids: Gebruik de SQL-queryeditor van Azure Portal om verbinding te maken en query's op gegevens uit te voeren

De SQL-queryeditor is een Azure Portal-browsertool die een eenvoudige manier biedt om SQL-query's uit te voeren op uw Azure SQL Database of Azure SQL Data Warehouse. In deze snelstart gebruikt u de query-editor om verbinding te maken met een SQL-database en vervolgens Transact-SQL-instructies uit te voeren om gegevens te zoeken, in te voegen, bij te werken en te verwijderen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

[!INCLUDE [prerequisites-create-db](../../includes/sql-database-connect-query-prerequisites-create-db-includes.md)]

> [!NOTE]
> Zorg ervoor dat de optie **Toegang tot Azure-services toestaan** is ingesteld op **AAN** in de firewall-instellingen van SQL Server. Met deze optie geeft de SQL-queryeditor toegang tot uw databases en datawarehouses.

## <a name="sign-in-the-azure-portal"></a>Meld u aan bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="connect-using-sql-authentication"></a>Verbinding maken met behulp van SQL-verificatie

1. Selecteer **SQL-databases** in het menu links en selecteer vervolgens **mySampleDatabase**.

2. Zoek in het linkermenu **Query-editor (preview)** en selecteer deze optie. De pagina **Aanmelden** wordt weergegeven.

    ![queryeditor zoeken](./media/sql-database-connect-query-portal/find-query-editor.PNG)

3. Selecteer in de vervolgkeuzelijst **Autorisatietype** de optie **SQL Server-verificatie** en voer de gebruikersnaam en het wachtwoord van het serverbeheerdersaccount in dat wordt gebruikt om de database te maken.

    ![aanmelden](./media/sql-database-connect-query-portal/login-menu.png) 

4. Selecteer **OK**.


## <a name="connect-using-azure-active-directory"></a>Verbinding maken met Azure Active Directory

Als u een Active Directory-beheerder (AD) instelt, kunt u gebruikmaken van één identiteit om u aan te melden bij Azure Portal en uw SQL-database. Volg de onderstaande stappen om een AD-beheerder te configureren voor uw SQL Server.

> [!NOTE]
* E-mailaccounts (bijvoorbeeld outlook.com, gmail.com, yahoo.com, enzovoort) worden nog niet ondersteund als AD-beheerders. Kies een gebruiker die ofwel systeemeigen is gemaakt in de Azure AD ofwel federatief in Azure AD.
* Azure AD-beheerdersaanmelding werkt niet met accounts waarvoor tweeledige verificatie is ingeschakeld.

1. Selecteer **Alle resources** in het menu links en selecteer vervolgens uw SQL Server.

2. Selecteer in het menu **Instellingen** van uw SQL Server de optie **Active Directory-beheerder**.

3. Selecteer op de werkbalk van de AD-beheerderspagina de optie **Beheerder instellen** en kies de gebruiker of groep als uw AD-beheerder.

    ![active directory selecteren](./media/sql-database-connect-query-portal/select-active-directory.png)

4. Selecteer **Opslaan** op de werkbalk van de AD-beheerderspagina.

5. Navigeer naar de database **mySampleDatabase** en selecteer in het menu links **Query-editor (preview)**. De pagina **Aanmelden** wordt weergegeven. Als u een AD-beheerder bent, wordt aan de rechterkant, onder **Active Directory: eenmalige aanmelding** een bericht weergegeven met de mededeling dat u bent aangemeld. 
   
6. Selecteer **OK**.


## <a name="view-data"></a>Gegevens weergeven

1. Nadat u bent geverifieerd, plakt u de volgende SQL in de queryeditor om de belangrijkste twintig producten op categorie op te halen.

   ```sql
    SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
    FROM SalesLT.ProductCategory pc
    JOIN SalesLT.Product p
    ON pc.productcategoryid = p.productcategoryid;
   ```

2. Selecteer op de werkbalk de optie **Uitvoeren** en bekijk de uitvoer in het deelvenster **Resultaten**.

![resultaten queryeditor](./media/sql-database-connect-query-portal/query-editor-results.png)

## <a name="insert-data"></a>Gegevens invoegen

Voer de volgende Transact-SQL [INSERT](https://msdn.microsoft.com/library/ms174335.aspx)-instructie uit om een nieuw product toe te voegen in de tabel `SalesLT.Product`.

1. Vervang de vorige query door deze.

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```


2. Selecteer **Uitvoeren** om een nieuwe rij in te voegen in de tabel `Product`. Het deelvenster **Berichten** toont **Query voltooid: Betroffen rijen: 1**.


## <a name="update-data"></a>Gegevens bijwerken

Voer de volgende Transact-SQL [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx)-instructie uit om uw nieuwe product te wijzigen.

1. Vervang de vorige query door deze.

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Selecteer **Uitvoeren** om de opgegeven rij in de tabel `Product` bij te werken. Het deelvenster **Berichten** toont **Query voltooid: Betroffen rijen: 1**.

## <a name="delete-data"></a>Gegevens verwijderen

Gebruik de volgende Transact-SQL [DELETE](https://msdn.microsoft.com/library/ms189835.aspx)-instructie om uw nieuwe product te verwijderen.

1. Vervang de vorige query door deze:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Selecteer **Uitvoeren** om de opgegeven rij in de tabel `Product` te verwijderen. Het deelvenster **Berichten** toont **Query voltooid: Betroffen rijen: 1**.


## <a name="query-editor-considerations"></a>Overwegingen met betrekking tot de queryeditor

U moet enkele dingen weten voordat u met de queryeditor gaat werken.

* U kunt de query-editor niet gebruiken om query's uit te voeren op SQL Server-databases in een virtueel netwerk.

* Wanneer u op F5 drukt, wordt de pagina van de queryeditor vernieuwd en gaan query's waaraan wordt gewerkt, verloren.

* De query-editor biedt geen ondersteuning voor het maken van verbinding met de `master`-database.

* Er is een time-out van vijf minuten voor uitvoering van de query.

* De queryeditor ondersteunt alleen cilindrische projectie voor geografiegegevenstypen.

* Er is geen ondersteuning voor IntelliSense voor databasetabellen en -weergaven. Het automatisch aanvullen van namen die al eerder zijn getypt, wordt wel ondersteund.


## <a name="next-steps"></a>Volgende stappen

Zie [Transact-SQL-verschillen oplossen tijdens migratie naar SQL Database](sql-database-transact-sql-information.md) voor informatie over de Transact-SQL die in Azure SQL-databases wordt ondersteund.
