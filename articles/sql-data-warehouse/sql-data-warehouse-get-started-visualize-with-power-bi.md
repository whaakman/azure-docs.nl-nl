---
title: SQL Data Warehouse-gegevens visualiseren met Power BI | Microsoft Azure
description: SQL Data Warehouse-gegevens visualiseren met Power BI
services: sql-data-warehouse
author: kavithaj
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: consume
ms.date: 04/17/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: 52581a87caac419a79caab647cc9c5a4ee7453ba
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
ms.locfileid: "31601573"
---
# <a name="visualize-data-with-power-bi"></a>Gegevens visualiseren met Power BI
In deze zelfstudie leert u hoe u Power BI gebruikt om verbinding te maken met SQL Data Warehouse en hoe u enkele eenvoudige visualisaties maakt.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Een SQL Data Warehouse die vooraf is geladen met de AdventureWorksDW-dataware. Zie voor het inrichten van een datawarehouse [maken van een SQL Data Warehouse](create-data-warehouse-portal.md) en kies de voorbeeldgegevens te laden. Als u al een datawarehouse hebt maar nog geen voorbeeldgegevens, kunt u [WideWorldImportersDW laden](load-data-wideworldimportersdw.md).

## <a name="1-connect-to-your-database"></a>1. Verbinding maken met uw database
Ga als volgt te werk om Power BI te openen en verbinding te maken met de database AdventureWorksDW:

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/).
2. Klik op **SQL-databases** en kies de SQL Data Warehouse-database AdventureWorks.
   
    ![De database zoeken](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png)
3. Klik op de knop Openen in Power BI.
   
    ![Power BI-knop](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png)
4. Nu ziet u de verbindingspagina voor SQL Data Warehouse met het webadres van uw database. Klik op volgende.
   
    ![Verbinding met Power BI](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png)
5. Voer uw Azure SQL server-gebruikersnaam en wachtwoord.
   
    ![Meld u in Power BI](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png)
6. De database wilt openen, klikt u op de gegevensset AdventureWorksDW op de blade links.
   
    ![AdventureWorksDW wordt geopend in Power BI](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png)

## <a name="2-create-a-report"></a>2. Een rapport maken
Nu kunt u Power BI gebruiken om de voorbeeldgegevens uit AdventureWorksDW te analyseren. Voor de analyse heeft AdventureWorksDW een weergave die AggregateSales wordt genoemd. Deze weergave bevat enkele van de belangrijkste metrische gegevens om de verkoopcijfers van het bedrijf te analyseren.

1. Als u een kaart wilt maken van de totale verkoop op postcode, klikt u in het rechterdeelvenster met velden op de weergave AggregateSales om deze uit te vouwen. Klik op de kolommen PostalCode en SalesAmount om deze te selecteren.
   
    ![Power BI AggregateSales wordt geselecteerd](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png)
   
    Power BI worden automatisch herkend geografische gegevens en plaatsen in een map voor u.
   
    ![Power BI-kaart](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png)

2. In deze stap maakt u een staafdiagram waarin de totale verkoop per klantinkomen wordt weergegeven. Voor het maken van het staafdiagram, gaat u naar de uitgevouwen weergave AggregateSales. Klik in het veld SalesAmount. Sleep het veld Customer Income naar links en zet het neer in As.
   
    ![Power BI selecteert as](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png)
   
    Het staafdiagram over het linkerdiagram geplaatst.
   
    ![Power BI-staafdiagram](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png)
3. Met deze stap maakt u een lijndiagram waarin de omzet per datum wordt weergegeven. Ga naar de uitgevouwen weergave AggregateSales voor het maken van het lijndiagram. Klik op SalesAmount en OrderDate. Klik in de kolom visualisaties op het pictogram lijndiagram, het eerste pictogram in de tweede regel onder visualisaties is.
   
    ![Power BI lijndiagram wordt geselecteerd](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png)
   
    U hebt nu een rapport met drie verschillende visualisaties van de gegevens.
   
    ![Power BI-lijndiagram](media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png)

U kunt de voortgang op elk moment opslaan door op **Bestand** te klikken en **Opslaan** te selecteren.

## <a name="using-direct-connnect"></a>Met behulp van directe Processtappen verbinden
Als met Azure SQL Database kunt SQL Data Warehouse Direct Connect logische naar beneden duwen naast de analytische functies van Power BI. Met Direct Connect query's worden verzonden naar uw Azure SQL Data Warehouse in realtime als u de gegevens verkennen.  Deze functie wordt gecombineerd met de schaal van SQL Data Warehouse, kunt u dynamische rapporten maken in minuten op basis van terabytes aan gegevens. Bovendien kan de introductie van de openen in Power BI-knop gebruikers direct verbinding maken met Power BI hun SQL Data Warehouse zonder gegevens te verzamelen uit andere delen van Azure.

Als u directe verbinding:

* Geef de volledig gekwalificeerde servernaam wanneer verbinding wordt gemaakt.
* Zorg ervoor dat de firewallregels voor de database zijn geconfigureerd voor toegang tot Azure-services toestaan.
* Elke actie zoals een kolom selecteren of een filter toe te voegen rechtstreeks een query voor het datawarehouse.
* Tegels worden automatisch en ongeveer elke 15 minuten vernieuwd.
* Met Q & A is niet beschikbaar voor gegevenssets die Direct Connect.
* Wijzigingen in het schema zijn automatisch opgenomen.
* Alle Direct verbinding maken met query's na 2 minuten.

Deze beperkingen en -opmerkingen kunnen wijzigen, omdat de ervaringen verbeteren.

## <a name="next-steps"></a>Volgende stappen
Nu u met behulp van de voorbeeldgegevens wat hebt kunnen oefenen, gaat u leren hoe u kunt [ontwikkelen](sql-data-warehouse-overview-develop.md), [laden](design-elt-data-loading.md) of [migreren](sql-data-warehouse-overview-migrate.md). Of neem eens een kijkje op de [website van Power BI](http://www.powerbi.com/).
