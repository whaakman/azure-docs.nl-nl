---
title: 'Azure Portal: Een individuele database maken - Azure SQL Database | Microsoft Docs'
description: Eén database maken en er query's op uitvoeren in Azure SQL Database via de Azure-portal.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 02/25/2019
ms.openlocfilehash: 64d16073062c0fd7f0b97a64950ceab9efc0eacf
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56984880"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-portal"></a>Quickstart: Eén database maken in Azure SQL Database via de Azure-portal

Het maken van [één database](sql-database-single-database.md) is de snelste een eenvoudigste implementatieoptie om een database te maken in Azure SQL Database. Aan de hand van deze quickstart kunt u een individuele database maken via de Azure-portal.

Als u nog geen abonnement op Azure hebt, [maak dan een gratis account](https://azure.microsoft.com/free/).

Voor alle stappen in deze snelstartgids moet u zich aanmelden bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-single-database"></a>Een individuele database maken

Een individuele database bevat een gedefinieerde set reken-, geheugen-, IO- en opslagresources die gebruikmaakt van één van twee [aankoopmodellen](sql-database-purchase-models.md). Wanneer u een individuele database maakt, definieert u ook een [SQL Database-server](sql-database-servers.md) om die te beheren en in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) in een opgegeven regio te plaatsen.

U maakt als volgt een individuele database met de voorbeeldgegevens van Adventure Works LT:

1. Selecteer in de linkerbovenhoek van Azure Portal **Een resource maken**.
2. Selecteer **Databases** en selecteer vervolgens **SQL Database**.
3. Typ of selecteer de volgende waarden in het formulier **SQL-database maken**:

   - **Databasenaam**: Voer *mySampleDatabase* in.
   - **Abonnement**: Selecteer het juiste abonnement in de vervolgkeuzelijst als deze niet wordt weergegeven.
   - **Resourcegroep**: Selecteer **Nieuwe maken**, typ *myResourceGroup* en selecteer **OK**.
   - **Bron selecteren**: Selecteer **Voorbeeld (AdventureWorksLT)** in de vervolgkeuzelijst.

    > [!IMPORTANT]
    > Selecteer de gegevens uit **Voorbeeld (AdventureWorksLT)** zodat u deze en andere quickstarts voor Azure SQL Database gemakkelijk kunt volgen waarbij deze gegevens ook worden gebruikt.
  
   ![Individuele database maken](./media/sql-database-get-started-portal/create-database-1.png)

4. Selecteer onder **Server** de optie **Nieuwe maken**.
5. In het formulier **Nieuwe server** typt of selecteert u de volgende waarden:

   - **Servernaam**: Voer *mysqlserver* in.
   - **Aanmeldgegevens van serverbeheerder**: typ *azureuser*.
   - **Wachtwoord**: Voer *Azure1234567* in.
   - **Wachtwoord bevestigen**: Voer het wachtwoord opnieuw in.
   - **Locatie**: Selecteer een geldige locatie in de vervolgkeuzelijst.  

   > [!IMPORTANT]
   > Vergeet niet de aanmeldgegevens en het wachtwoord van de server te noteren zodat u zich bij de server en databases voor deze en andere quickstarts kunt aanmelden. Als u uw aanmeldgegevens of wachtwoord vergeet, kunt u de aanmeldnaam ophalen of het wachtwoord opnieuw instellen op de pagina **SQL Server**. U kunt de pagina **SQL Server** openen door de servernaam te selecteren op de **Overzichtspagina** van de database nadat u de database hebt gemaakt.

    ![Server maken](./media/sql-database-get-started-portal/create-database-server.png)

6. Kies **Selecteren**.
7. Selecteer in het **SQL Database**-formulier de optie **Prijscategorie**. Bekijk de hoeveelheid DTU's en opslag die voor elke servicelaag beschikbaar zijn.

   > [!NOTE]
   > In deze snelstartgids wordt gebruik gemaakt van het [op DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md). Het [model op basis van vCore](sql-database-service-tiers-vcore.md) is echter ook beschikbaar.
   > [!IMPORTANT]
   > Voor de Premium-laag is er meer dan 1 TB aan opslagruimte beschikbaar in alle regio's, met uitzondering van: China - oost, China - noord, Duitsland - centraal, Duitsland - noordoost, US - west-centraal, US - DoD-regio's en US Government - centraal. In deze regio’s is de maximale opslagruimte in de Premium-laag beperkt tot 1 TB.  Raadpleeg [P11-P15 huidige beperkingen](sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb) voor meer informatie.  

8. Voor deze snelstart selecteert u de servicelaag **Standard** en gebruikt u vervolgens de schuifregelaar om **10 DTU's (S0)** en **1** GB aan opslagruimte te selecteren.
9. Selecteer **Toepassen**.  

   ![Prijzen selecteren](./media/sql-database-get-started-portal/create-database-s1.png)

10. Selecteer in het **SQL Database**-formulier de optie **Maken** om de resourcegroep, server en database te implementeren en in te richten.

   De implementatie duurt een paar minuten. Selecteer **Meldingen** op de werkbalk om de implementatievoortgang te bewaken.

   ![Melding](./media/sql-database-get-started-portal/notification.png)

## <a name="query-the-database"></a>Een query uitvoeren op de database

Nu u de database hebt gemaakt, gebruikt u het ingebouwde hulpprogramma voor query's in de Azure-portal om verbinding te maken met de database en query's uit te voeren op de gegevens.

1. Selecteer op de **SQL Database**-pagina voor uw database in het linkermenu de optie **Query-editor (preview)**.

   ![Aanmelden bij query-editor](./media/sql-database-get-started-portal/query-editor-login.png)

2. Voer uw aanmeldgegevens in en selecteer **OK**.
3. Voer de volgende query in het deelvenster **Query-editor** in.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. Klik op **Uitvoeren** en bekijk de resultaten van de query in het deelvenster **Resultaten**.

   ![Resultaten query-editor](./media/sql-database-get-started-portal/query-editor-results.png)

5. Sluit de pagina **Query-editor** en selecteer **OK** wanneer u wordt gevraagd om uw niet-opgeslagen bewerkingen te verwijderen.

## <a name="clean-up-resources"></a>Resources opschonen

Behoud deze resourcegroep, databaseserver en individuele database als u naar de [Volgende stappen](#next-steps) wilt gaan. De volgende stappen laten zien hoe u verbinding maakt met en een query uitvoert op uw database met behulp van verschillende methoden.

Wanneer u klaar bent met deze resources, kunt u ze als volgt verwijderen:

1. Selecteer in het linkermenu in Azure Portal **Resourcegroepen** en selecteer vervolgens **myResourceGroup**.
2. Selecteer **Resourcegroep verwijderen** op de pagina van de resourcegroep.
3. Voer *myResourceGroup* in het tekstvak in en selecteer vervolgens **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Maak een firewallregel op serverniveau om via on-premises of externe hulpprogramma's verbinding met de individuele database te maken. Zie [Een serverfirewallregel maken](sql-database-server-level-firewall-rule.md) voor meer informatie.
- Nadat u een serverfirewallregel hebt gemaakt, kunt u met verschillende hulpprogramma's en programmeertalen [verbinding maken met uw database en query's uitvoeren](sql-database-connect-query.md) op uw database.
  - [Verbinding maken en query's uitvoeren met behulp van SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Verbinding maken en query's uitvoeren met behulp van Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Zie [Azure CLI-voorbeelden](sql-database-cli-samples.md) voor het maken van individuele databases met behulp van Azure CLI.
- Zie [Azure PowerShell-voorbeelden](sql-database-powershell-samples.md) voor het maken van individuele databases met behulp van Azure PowerShell.
