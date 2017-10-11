---
title: SQL Data Warehouse-gegevens visualiseren met Power BI | Microsoft Azure
description: SQL Data Warehouse-gegevens visualiseren met Power BI
services: sql-data-warehouse
documentationcenter: NA
author: mlee3gsd
manager: jhubbard
editor: 
ms.assetid: d7fb89d1-da1d-4788-a111-68d0e3fda799
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: integrate
ms.date: 10/31/2016
ms.author: martinle;barbkess
ms.openlocfilehash: a41393730143b14e91318a61858d989fff3786c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-data-with-power-bi"></a>Gegevens visualiseren met Power BI
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

In deze zelfstudie leert u hoe u Power BI gebruikt om verbinding te maken met SQL Data Warehouse en hoe u enkele eenvoudige visualisaties maakt.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een SQL Data Warehouse die vooraf is geladen met de AdventureWorksDW-dataware. Zie voor het inrichten hiervan [Een SQL Data Warehouse maken][Create a SQL Data Warehouse] en kies ervoor om de voorbeeldgegevens te laden. Als u wel een datawarehouse hebt maar nog geen voorbeeldgegevens, kunt u [voorbeeldgegevens handmatig laden][load sample data manually].

## <a name="1-connect-to-your-database"></a>1. Verbinding maken met uw database
Ga als volgt te werk om Power BI te openen en verbinding te maken met de database AdventureWorksDW:

1. Meld u aan bij [Azure Portal][Azure portal].
2. Klik op **SQL-databases** en kies de SQL Data Warehouse-database AdventureWorks.
   
    ![De database zoeken][1]
3. Klik op de knop Openen in Power BI.
   
    ![Power BI-knop][2]
4. Nu ziet u de verbindingspagina voor SQL Data Warehouse met het webadres van uw database. Klik op volgende.
   
    ![Verbinding met Power BI][3]
5. Voer de gebruikersnaam en het wachtwoord van uw Azure SQL server in, waarna u volledig wordt verbonden met uw SQL Data Warehouse-database.
   
    ![Aanmelden bij Power BI][4]
6. Wanneer u bent aangemeld bij Power BI, klikt u in de linkerblade op de gegevensset AdventureWorksDW. De database wordt geopend.
   
    ![AdventureWorksDW wordt geopend in Power BI][5]

## <a name="2-create-a-report"></a>2. Een rapport maken
Nu kunt u Power BI gebruiken om de voorbeeldgegevens uit AdventureWorksDW te analyseren. Voor de analyse heeft AdventureWorksDW een weergave die AggregateSales wordt genoemd. Deze weergave bevat enkele van de belangrijkste metrische gegevens om de verkoopcijfers van het bedrijf te analyseren.

1. Als u een kaart wilt maken van de totale verkoop op postcode, klikt u in het rechterdeelvenster met velden op de weergave AggregateSales om deze uit te vouwen. Klik op de kolommen PostalCode en SalesAmount om deze te selecteren.
   
    ![AggregateSales selecteren in Power BI][6]
   
    De gegevens, die in Power BI automatisch worden herkend als geografische gegevens, worden in een kaart geplaatst.
   
    ![Power BI-kaart][7]
2. In deze stap maakt u een staafdiagram waarin de totale verkoop per klantinkomen wordt weergegeven. Daarvoor gaat u naar de uitgevouwen weergave AggregateSales. Klik in het veld SalesAmount. Sleep het veld Customer Income naar links en zet het neer in As.
   
    ![As selecteren in Power BI][8]
   
    We hebben het staafdiagram over het linkerdiagram geplaatst.
   
    ![Power BI-staafdiagram][9]
3. Met deze stap maakt u een lijndiagram waarin de omzet per datum wordt weergegeven. Daarvoor gaat u naar de uitgevouwen weergave AggregateSales. Klik op SalesAmount en OrderDate. Klik in de kolom Visualisaties op het pictogram Lijndiagram. Dit is het eerste pictogram in de tweede regel onder Visualisaties.
   
    ![Lijndiagram selecteren in Power BI][10]
   
    U hebt nu een rapport met drie verschillende visualisaties van de gegevens.
   
    ![Power BI-lijndiagram][11]

U kunt de voortgang op elk moment opslaan door op **Bestand** te klikken en **Opslaan** te selecteren.

## <a name="next-steps"></a>Volgende stappen
U hebt wat kunnen oefenen met behulp van de voorbeeldgegevens en gaat nu leren hoe u kunt [ontwikkelen][develop], [laden][load] of [migreren][migrate]. U kunt natuurlijk ook een kijkje nemen op de [website van Power BI][Power BI website].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data manually]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure portal]: https://portal.azure.com/
[Power BI website]: http://www.powerbi.com/
