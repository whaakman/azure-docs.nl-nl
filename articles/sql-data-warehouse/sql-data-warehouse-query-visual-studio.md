---
title: "Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio) | Microsoft Docs"
description: "Query’s uitvoeren bij SQL Data Warehouse met Visual Studio."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: daace889-95e5-4826-b2fc-047eac9d6d95
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5e97fefdf3cc13f2fd3e060da901e90ef5ef9a29


---
# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Gebruik Visual Studio om binnen enkele minuten query’s uit te voeren bij Azure SQL Data Warehouse. Bij deze methode wordt gebruikgemaakt van de SSDT-uitbreiding (SQL Server Data Tools) in Visual Studio. 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een bestaande SQL-datawarehouse. Zie [Een SQL Data Warehouse maken][Een SQL Data Warehouse maken] als u er een wilt maken.
* SSDT voor Visual Studio. Als u Visual Studio hebt, hebt u dit waarschijnlijk al. Voor installatie-instructies en -opties raadpleegt u [Visual Studio en SSDT installeren][Visual Studio en SSDT installeren].
* De volledig gekwalificeerde SQL-servernaam. Zie [Verbinding maken met SQL Data Warehouse][Verbinding maken met SQL Data Warehouse] om dit te vinden.

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Verbinding maken met uw SQL Data Warehouse
1. Open Visual Studio 2013 of 2015.
2. Open SQL Server-objectverkenner. Daartoe selecteert u **View** > **SQL Server Object Explorer**.
   
    ![SQL Server-objectverkenner][1]
3. Klik op het pictogram **SQL Server toevoegen**.
   
    ![SQL Server toevoegen][2]
4. Vul de velden in het venster Connect to Server (Verbinding maken met server) in.
   
    ![Verbinding maken met server][3]
   
   * **Server name** (Servernaam). Voer de eerder vastgestelde **servernaam** in.
   * **Authentication** (Verificatie). Selecteer **SQL Server Authentication** (SQL Server-verificatie) of **Active Directory Integrated Authentication** (Geïntegreerde Active Directory-verificatie).
   * **User Name** (Gebruikersnaam) en **Password** (Wachtwoord). Voer de gebruikersnaam en het wachtwoord in als u hierboven SQL Server-verificatie hebt geselecteerd.
   * Klik op **Connect** (Verbinden).
5. U kunt de Azure SQL-server uitvouwen als u deze wilt verkennen. U kunt de databases weergeven die aan de server zijn gekoppeld. Vouw AdventureWorksDW uit als u de tabellen in de voorbeelddatabase wilt zien.
   
    ![AdventureWorksDW verkennen][4]

## <a name="2-run-a-sample-query"></a>2. Een voorbeeldquery uitvoeren
Nu er een verbinding met uw database is ingesteld, gaat u een query schrijven.

1. Klik met de rechtermuisknop op de database in SQL Server-objectverkenner.
2. Selecteer **New Query** (Nieuwe query). Een nieuwe queryvenster wordt geopend.
   
    ![Nieuwe query][5]
3. Kopieer de volgende TSQL-query naar het queryvenster:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Voer de query uit. Daartoe klikt u op de groene pijl of gebruikt u de volgende snelkoppeling: `CTRL`+`SHIFT`+`E`.
   
    ![Query uitvoeren][6]
5. Bekijk de resultaten van de query. In dit voorbeeld heeft de tabel FactInternetSales 60398 rijen.
   
    ![Queryresultaten][7]

## <a name="next-steps"></a>Volgende stappen
Nu u weet hoe u verbinding maakt en een query uitvoert, kunt u proberen [de gegevens te visualiseren met Power BI][de gegevens visualiseren met Power BI].

Zie [Verifiëren bij SQL Data Warehouse][Verifiëren bij SQL Data Warehouse] om uw omgeving te configureren voor Azure Active Directory-verificatie.

<!--Arcticles-->
[Verbinding maken met SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Een SQL Data Warehouse maken]: sql-data-warehouse-get-started-provision.md
[Visual Studio en SSDT installeren]: sql-data-warehouse-install-visual-studio.md
[Verifiëren bij SQL Data Warehouse]: sql-data-warehouse-authentication.md
[de gegevens visualiseren met Power BI]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure Portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png



<!--HONumber=Nov16_HO2-->


