---
title: SQL Data Warehouse-gegevens visualiseren met Power BI | Microsoft Docs
description: SQL Data Warehouse-gegevens visualiseren met Power BI
services: sql-data-warehouse
documentationcenter: NA
author: lodipalm
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/16/2016
ms.author: lodipalm;barbkess;sonyama

---
# Gegevens visualiseren met Power BI
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> 
> 

In deze zelfstudie leert u hoe u Power BI gebruikt om verbinding te maken met SQL Data Warehouse en hoe u enkele eenvoudige visualisaties maakt.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een SQL Data Warehouse die vooraf is geladen met de AdventureWorksDW-dataware. Zie voor het inrichten hiervan [Een SQL Data Warehouse maken][Een SQL Data Warehouse maken] en kies ervoor om de voorbeeldgegevens te laden. Als u wel een datawarehouse hebt maar nog geen voorbeeldgegevens, kunt u [voorbeeldgegevens handmatig laden][voorbeeldgegevens handmatig laden].

## 1. Verbinding maken met uw database
Ga als volgt te werk om Power BI te openen en verbinding te maken met de database AdventureWorksDW:

1. Meld u aan bij de [Azure-portal][Azure-portal].
2. Klik op **SQL-databases** en kies de SQL Data Warehouse-database AdventureWorks.
   
    ![De database zoeken][1]
3. Klik op de knop Openen in Power BI.
   
    ![Power BI-knop][2]
4. Nu ziet u de verbindingspagina voor SQL Data Warehouse met het webadres van uw database. Klik op Volgende.
   
    ![Verbinding met Power BI][3]
5. Voer de gebruikersnaam en het wachtwoord van uw Azure SQL server in, waarna u volledig wordt verbonden met uw SQL Data Warehouse-database.
   
    ![Aanmelden bij Power BI][4]
6. Wanneer u bent aangemeld bij Power BI, klikt u in de linkerblade op de gegevensset AdventureWorksDW. De database wordt geopend.
   
    ![AdventureWorksDW wordt geopend in Power BI][5]

## 2. Een rapport maken
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

## Volgende stappen
Nu u met behulp van de voorbeeldgegevens wat hebt kunnen oefenen, gaat u leren hoe u kunt [ontwikkelen][ontwikkelen], [laden][laden] of [migreren][migreren]. Of neem eens een kijkje op de [website van Power BI][website van Power BI].

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
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[voorbeeldgegevens handmatig laden]: sql-data-warehouse-load-sample-databases.md
[verbinding maken met SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Een SQL Data Warehouse maken]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure-portal]: https://portal.azure.com/
[website van Power BI]: http://www.powerbi.com/



<!--HONumber=Sep16_HO3-->


