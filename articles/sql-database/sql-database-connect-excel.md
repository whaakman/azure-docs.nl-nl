---
title: Excel verbinden met de SQL Database | Microsoft Docs
description: Leer hoe u Microsoft Excel verbindt met Azure SQL Database in de cloud. Gegevens importeren in Excel voor rapportage en gegevens verkenning.
services: sql-database
keywords: excel verbinden met sql, gegevens importeren in excel
author: joseidz
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 03/10/2017
ms.author: craigg
ms.openlocfilehash: 6f2894d65240580346b99d203f8289652d8e6618
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="connect-excel-to-an-azure-sql-database-and-create-a-report"></a>Excel verbinden met een Azure SQL database en een rapport maken

Excel verbinden met een SQL-database in de cloud en gegevens importeren en tabellen en grafieken op basis van waarden in de database maken. In deze zelfstudie stelt u de verbinding tussen Excel en een databasetabel in, slaat u het bestand met de gegevens en de verbindingsinformatie voor Excel op en maakt u vervolgens een draaigrafiek uit de databasewaarden.

Voordat u begint hebt u een SQL Database in Azure nodig. Als u deze niet hebt, raadpleegt u [Uw eerste SQL Database maken](sql-database-get-started-portal.md) om een database met voorbeeldgegevens binnen enkele minuten in te stellen. In dit artikel, zult u voorbeeldgegevens importeren in Excel van dat artikel, maar u kunt dezelfde stappen volgen met uw eigen gegevens.

U hebt ook een kopie van Excel nodig. Dit artikel gebruikt [Microsoft Excel 2016](https://products.office.com/).

## <a name="connect-excel-to-a-sql-database-and-load-data"></a>Excel verbinden met een SQL-database en gegevens laden
1. Open Excel om het programme te verbinden met SQL Database, en maak een nieuwe werkmap of open een bestaande Excel-werkmap.
2. Selecteer in de menubalk boven aan de pagina de **gegevens** tabblad **gegevens ophalen**, selecteer van Azure en selecteer vervolgens **van Azure SQL Database**. 
   
   ![Selecteer gegevensbron: Excel verbinden met SQL Database.](./media/sql-database-connect-excel/excel_data_source.png)
   
   De wizard Gegevensverbinding wordt geopend.
3. In het dialoogvenster **Verbinding maken met databaseserver** typt u de **servernaam** van de SQL Database die u wilt verbinden in het formulier <*servername*>**. database.windows.net**. Bijvoorbeeld: **msftestserver.database.windows.net**. Voer eventueel op de naam van uw database. Selecteer **OK** om de referenties venster te openen. 

   ![Server name.png](media/sql-database-connect-excel/server-name.png)

1. In de **SQL Server-Database** dialoogvenster, **Database** aan de linkerkant zijde en voert u uw **gebruikersnaam** en **wachtwoord** voor de U verbinding maken wilt met SQL database-server. Selecteer **Connect** openen de **Navigator**. 

  ![Typ de servernaam en aanmeldingsreferenties in](./media/sql-database-connect-excel/connect-to-server.png)
   
  > [!TIP]
  > Afhankelijk van uw netwerkomgeving kunt u mogelijk geen verbinding maken of kan de verbinding verbroken worden als de server van de SQL Database geen verkeer van het IP-adres van de client toestaat. Ga naar de [Azure-portal](https://portal.azure.com/), klik op SQL-servers, klik op uw server, klik op firewall onder instellingen en voeg uw client-IP-adres toe. Zie [Firewallinstellingen configureren](sql-database-configure-firewall-settings.md) voor meer informatie.
   
   
5. In de **Navigator**, selecteer de database die u wilt werken uit de lijst, selecteer de tabellen of weergaven die u wilt werken (We hebben gekozen **vGetAllCategories**), en selecteer vervolgens **Load**voor het verplaatsen van de gegevens van uw Azure SQL-database naar uw excel-werkblad.
   
    ![Selecteer een database en tabel.](./media/sql-database-connect-excel/select-database-and-table.png)
   

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Importeer de gegevens in Excel en maak een draaigrafiek.
Nu dat u de verbinding hebt gemaakt, hebben verschillende opties met hoe de gegevens te laden. De volgende stappen wordt bijvoorbeeld een draaigrafiek op basis van de gegevens gevonden in de SQL-Database maken. 

1. Volg de stappen in de vorige sectie, maar deze tijd, in plaats van het selecteren van **Load**, selecteer **laden naar** van de **Load** vervolgkeuzelijst.
2. Selecteer vervolgens hoe u wilt weergeven van deze gegevens in uw werkmap. We hebben **draaigrafiek** gekozen. U kunt ook kiezen voor **Nieuw werkblad** of **Deze gegevens toevoegen aan een gegevensmodel**. Zie voor meer informatie over gegevensmodellen [Een gegevensmodel maken in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). 
   
    ![De indeling kiezen voor gegevens in Excel](./media/sql-database-connect-excel/import-data.png)
   
    Het werkblad heeft nu een lege draaitabel en grafiek.
2. Onder **PivotTable-velden**, selecteert u alle selectievakjes voor de velden die u wilt weergeven.
   
    ![Databaserapport configureren.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Als u andere Excel-werkmappen en werkbladen voor de database verbinding wilt maken, selecteert u de **gegevens** tabblad en selecteer **recente bronnen** starten de **recente bronnen** in het dialoogvenster. Van daaruit, kiest u de verbinding die u hebt gemaakt in de lijst en klik vervolgens op **Open**.
> ![Recente verbindingen](media/sql-database-connect-excel/recent-connections.png)
 
## <a name="create-a-permanent-connection-using-odc-file"></a>Een permanente verbinding maken met ODC-bestand
Als u wilt de verbindingsdetails permanent opslaan, kunt u een ODC-bestand maken en deze verbinding maakt een selecteerbare optie binnen de **bestaande verbindingen** in het dialoogvenster. 

1. Selecteer in de menubalk boven aan de pagina de **gegevens** tabblad en selecteer vervolgens **bestaande verbindingen** starten de **bestaande verbindingen** in het dialoogvenster. 
    1. Selecteer **bladeren naar meer** openen de **gegevensbron selecteren** in het dialoogvenster.   
    2. Selecteer de **+NewSqlServerConnection.odc** -bestand en selecteer vervolgens **openen** openen de **Wizard Gegevensverbinding**.

    ![Nieuwe verbinding](media/sql-database-connect-excel/new-connection.png)

2. In de **Wizard Gegevensverbinding**, typt u de servernaam van uw en de referenties van uw SQL-Database. Selecteer **Volgende**. 
    1. Selecteer de database met uw gegevens in de vervolgkeuzelijst. 
    2. Selecteer de tabel of weergave die u geïnteresseerd bent in. We vGetAllCategories hebt gekozen.
    3. Selecteer **Volgende**. 

    ![Wizard Gegevensverbinding](media/sql-database-connect-excel/data-connection-wizard.png) 

3. Selecteer de locatie van het bestand, de **bestandsnaam**, en de **beschrijvende naam** in het volgende scherm van de Wizard Gegevensverbinding. U kunt ook het wachtwoord in het bestand opslaan hoewel dit uw gegevens om ongewenste toegang kunt blootstellen. Selecteer **voltooien** als u klaar bent. 

    ![Gegevensverbinding opslaan](media/sql-database-connect-excel/save-data-connection.png)

4. Selecteer hoe u wilt uw gegevens importeren. We doen een draaitabel gekozen. U kunt ook de eigenschappen van de verbinding wijzigen door Selecteer **eigenschappen**. Selecteer **OK** als u klaar bent. Als u geen wachtwoord op te slaan de met het bestand hebt gekozen, wordt u gevraagd uw referenties in te voeren. 

    ![Gegevens importeren](media/sql-database-connect-excel/import-data2.png)

5. Controleer of de nieuwe verbinding is opgeslagen door het uitbreiden van de **gegevens** tabblad en het selecteren van **bestaande verbindingen**. 

    ![Bestaande verbinding](media/sql-database-connect-excel/existing-connection.png)

## <a name="next-steps"></a>Volgende stappen
* Leer hoe u [SQL Database koppelt met SQL Server Management Studio](sql-database-connect-query-ssms.md) voor geavanceerd uitvoeren van query's en analyses.
* Meer informatie over de voordelen van [elastische groepen](sql-database-elastic-pool.md).
* Leer hoe u [ een webtoepassing maakt die verbinding maakt met SQL Database op de back-end](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md).

