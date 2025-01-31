---
title: Verbinding maken met Azure SQL datawarehouse - SSMS | Microsoft Docs
description: Gebruik SQL Server Management Studio (SSMS) verbinding maken met en query uitvoeren op Azure SQL Data Warehouse.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 64ea7c175b733f974eba6c081ee2c98814cbcda2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873710"
---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a>Verbinding maken met SQL datawarehouse met SQL Server Management Studio (SSMS)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Gebruik SQL Server Management Studio (SSMS) verbinding maken met en query uitvoeren op Azure SQL Data Warehouse. 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een bestaande SQL-datawarehouse. Zie [Een SQL-datawarehouse maken][Create a SQL Data Warehouse] om een datawarehouse te maken.
* SQL Server Management Studio (SSMS) geïnstalleerd. [Installatie van SSMS] [ Install SSMS] gratis als u nog geen hebt.
* De volledig gekwalificeerde SQL-servernaam. Zie [Verbinding maken met SQL Data Warehouse][Connect to SQL Data Warehouse] om dit te vinden.

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Verbinding maken met uw SQL Data Warehouse
1. Open SQL Server Management Studio.
2. Open Object Explorer. Om dit te doen, selecteert u **bestand** > **verbinding maken met Objectverkenner**.
   
    ![SQL Server-objectverkenner][1]
3. Vul de velden in het venster Connect to Server (Verbinding maken met server) in.
   
    ![Verbinding maken met server][2]
   
   * **Server name** (Servernaam). Voer de eerder vastgestelde **servernaam** in.
   * **Authentication** (Verificatie). Selecteer **SQL Server Authentication** (SQL Server-verificatie) of **Active Directory Integrated Authentication** (Geïntegreerde Active Directory-verificatie).
   * **User Name** (Gebruikersnaam) en **Password** (Wachtwoord). Voer de gebruikersnaam en het wachtwoord in als u hierboven SQL Server-verificatie hebt geselecteerd.
   * Klik op **Connect** (Verbinden).
4. U kunt de Azure SQL-server uitvouwen als u deze wilt verkennen. U kunt de databases weergeven die aan de server zijn gekoppeld. Vouw AdventureWorksDW uit als u de tabellen in de voorbeelddatabase wilt zien.
   
    ![AdventureWorksDW verkennen][3]

## <a name="2-run-a-sample-query"></a>2. Een voorbeeldquery uitvoeren
Nu er een verbinding met uw database is ingesteld, gaat u een query schrijven.

1. Klik met de rechtermuisknop op de database in SQL Server-objectverkenner.
2. Selecteer **New Query** (Nieuwe query). Een nieuwe queryvenster wordt geopend.
   
    ![Nieuwe query][4]
3. Kopieer de volgende TSQL-query naar het queryvenster:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Voer de query uit. Om dit te doen, klikt u op `Execute` of gebruikt u de volgende snelkoppeling: `F5`.
   
    ![Query uitvoeren][5]
5. Bekijk de resultaten van de query. In dit voorbeeld heeft de tabel FactInternetSales 60398 rijen.
   
    ![Queryresultaten][6]

## <a name="next-steps"></a>Volgende stappen
Nu u weet hoe u verbinding maakt en een query uitvoert, kunt u proberen [de gegevens te visualiseren met Power BI][visualizing the data with PowerBI].

Zie [Verifiëren bij SQL Data Warehouse][Authenticate to SQL Data Warehouse] om uw omgeving te configureren voor Azure Active Directory-verificatie.

<!--Arcticles-->
[Connect to SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authenticate to SQL Data Warehouse]: sql-data-warehouse-authentication.md
[visualizing the data with PowerBI]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure portal]: https://portal.azure.com
[Install SSMS]: https://msdn.microsoft.com/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png
