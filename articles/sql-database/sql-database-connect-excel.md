---
title: Excel verbinden met een individuele database in Azure SQL Database | Microsoft Docs
description: Leer hoe u Microsoft Excel verbindt met een individuele database in Azure SQL-database. Gegevens importeren in Excel voor rapportage en gegevens verkenning.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: joseidz
ms.author: craigg
ms.reviewer: ''
manager: craigg
ms.date: 02/12/2019
ms.openlocfilehash: e1cd0d8462c31c8b843f7962f923accc6b63ae00
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58103397"
---
# <a name="connect-excel-to-a-single-database-in-azure-sql-database-and-create-a-report"></a>Excel verbinden met een individuele database in Azure SQL-database en een rapport maken

Excel verbinden met een individuele database in Azure SQL Database en gegevens importeren en tabellen en grafieken op basis van waarden in de database maken. In deze zelfstudie stelt u de verbinding tussen Excel en een databasetabel in, slaat u het bestand met de gegevens en de verbindingsinformatie voor Excel op en maakt u vervolgens een draaigrafiek uit de databasewaarden.

U moet een individuele database voordat u begint. Als u niet hebt, raadpleegt u [een individuele database maken](sql-database-single-database-get-started.md) en [IP-firewall op serverniveau maken](sql-database-server-level-firewall-rule.md) om op te halen van een individuele database met voorbeeldgegevens van en worden uitgevoerd in een paar minuten.

In dit artikel, zult u voorbeeldgegevens importeren in Excel van dat artikel, maar u kunt dezelfde stappen volgen met uw eigen gegevens.

U hebt ook een kopie van Excel nodig. Dit artikel gebruikt [Microsoft Excel 2016](https://products.office.com/).

## <a name="connect-excel-to-a-sql-database-and-load-data"></a>Excel verbinden met een SQL-database en gegevens laden

1. Open Excel om het programme te verbinden met SQL Database, en maak een nieuwe werkmap of open een bestaande Excel-werkmap.
2. Selecteer in de menubalk boven aan de pagina, de **gegevens** tabblad **gegevens ophalen**, selecteert u uit Azure en selecteer vervolgens **van Azure SQL Database**. 

   ![Selecteer gegevensbron: Excel verbinden met SQL-database.](./media/sql-database-connect-excel/excel_data_source.png)

   De wizard Gegevensverbinding wordt geopend.
3. In het dialoogvenster **Verbinding maken met databaseserver** typt u de **servernaam** van de SQL Database die u wilt verbinden in het formulier <*servername*>**. database.windows.net**. Bijvoorbeeld, **msftestserver.database.windows.net**. Geef eventueel de naam van uw database. Selecteer **OK** om de referenties-venster te openen. 

   ![server-name.png](media/sql-database-connect-excel/server-name.png)

4. In de **SQL Server-Database** in het dialoogvenster, selecteer **Database** aan de linkerkant zijde en voer vervolgens uw **gebruikersnaam** en **wachtwoord** voor de U verbinding maken wilt met SQL Database-server. Selecteer **Connect** openen de **Navigator**. 

   ![Typ de servernaam en aanmeldingsreferenties in](./media/sql-database-connect-excel/connect-to-server.png)

   > [!TIP]
   > Afhankelijk van uw netwerkomgeving kunt u mogelijk geen verbinding maken of kan de verbinding verbroken worden als de server van de SQL Database geen verkeer van het IP-adres van de client toestaat. Ga naar de [Azure-portal](https://portal.azure.com/), klik op SQL-servers, klik op uw server, klik op firewall onder instellingen en voeg uw client-IP-adres toe. Zie [Firewallinstellingen configureren](sql-database-configure-firewall-settings.md) voor meer informatie.

5. In de **Navigator**, selecteer de database die u wilt werken met in de lijst, selecteer de tabellen of weergaven die u wilt werken (We hebben gekozen voor **vGetAllCategories**), en selecteer vervolgens **Load**naar de gegevens van uw database verplaatsen naar het Excel-werkblad.

    ![Selecteer een database en tabel.](./media/sql-database-connect-excel/select-database-and-table.png)

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Importeer de gegevens in Excel en maak een draaigrafiek.

Nu dat u de verbinding tot stand hebt gebracht, hebt u diverse opties met het laden van de gegevens. De volgende stappen maakt bijvoorbeeld een draaigrafiek op basis van de gegevens in uw SQL-Database gevonden. 

1. Volg de stappen in de vorige sectie, maar deze keer plaats **Load**, selecteer **laden naar** uit de **Load** vervolgkeuzelijst.
2. Selecteer vervolgens hoe u wilt weergeven van deze gegevens in uw werkmap. We hebben **draaigrafiek** gekozen. U kunt ook kiezen voor **Nieuw werkblad** of **Deze gegevens toevoegen aan een gegevensmodel**. Zie voor meer informatie over gegevensmodellen [Een gegevensmodel maken in Excel](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). 

    ![De indeling kiezen voor gegevens in Excel](./media/sql-database-connect-excel/import-data.png)

    Het werkblad heeft nu een lege draaitabel en grafiek.
3. Onder **PivotTable-velden**, selecteert u alle selectievakjes voor de velden die u wilt weergeven.

    ![Databaserapport configureren.](./media/sql-database-connect-excel/power-pivot-results.png)

> [!TIP]
> Als u andere Excel-werkmappen en werkbladen voor het verbinden met de database wilt, selecteert u de **gegevens** tabblad, en selecteer **recente bronnen** starten de **recente bronnen** in het dialoogvenster. Van daaruit, kiest u de verbinding die u hebt gemaakt in de lijst en klik vervolgens op **Open**.
> ![Recente verbindingen](media/sql-database-connect-excel/recent-connections.png)

## <a name="create-a-permanent-connection-using-odc-file"></a>Een permanente verbinding maken met behulp .odc-bestand

Om de verbindingsgegevens permanent opgeslagen, kunt u een ODC-bestand maken en deze verbinding maken als een selecteerbaar-optie in de **bestaande verbindingen** in het dialoogvenster. 

1. Selecteer in de menubalk boven aan de pagina, de **gegevens** tabblad, en selecteer vervolgens **bestaande verbindingen** starten de **bestaande verbindingen** in het dialoogvenster. 
   1. Selecteer **laderen naar meer** openen de **gegevensbron selecteren** in het dialoogvenster.   
   2. Selecteer de **+NewSqlServerConnection.odc** -bestand en selecteer vervolgens **Open** openen de **Wizard Gegevensverbinding**.

      ![Nieuwe verbinding](media/sql-database-connect-excel/new-connection.png)

2. In de **Wizard Gegevensverbinding**, typ de servernaam van uw en de referenties van uw SQL-Database. Selecteer **Volgende**. 
   1. Selecteer de database met uw gegevens uit de vervolgkeuzelijst. 
   2. Selecteer de tabel of weergave die u geïnteresseerd bent in. We hebben gekozen vGetAllCategories.
   3. Selecteer **Volgende**. 

      ![Wizard Gegevensverbinding](media/sql-database-connect-excel/data-connection-wizard.png) 

3. Selecteer de locatie van het bestand, de **bestandsnaam**, en de **beschrijvende naam** in het volgende scherm van de Wizard Gegevensverbinding. U kunt ook het wachtwoord opslaan in het bestand, maar dit aan ongewenste toegang tot gegevens blootstellen kunt. Selecteer **voltooien** als u klaar bent. 

    ![Gegevensverbinding opslaan](media/sql-database-connect-excel/save-data-connection.png)

4. Selecteer hoe u uw gegevens te importeren. We hebben gekozen voor een draaitabel doen. U kunt ook de eigenschappen van de verbinding wijzigen door selecteren **eigenschappen**. Selecteer **OK** als u klaar bent. Als u geen om op te slaan van het wachtwoord met het bestand hebt gekozen, wordt klikt u vervolgens u gevraagd uw referenties in te voeren. 

    ![Gegevens importeren](media/sql-database-connect-excel/import-data2.png)

5. Controleer of de nieuwe verbinding die is opgeslagen door het uitbreiden van de **gegevens** tabblad en het selecteren van **bestaande verbindingen**. 

    ![Bestaande verbinding](media/sql-database-connect-excel/existing-connection.png)

## <a name="next-steps"></a>Volgende stappen

* Leer hoe u [SQL Database koppelt met SQL Server Management Studio](sql-database-connect-query-ssms.md) voor geavanceerd uitvoeren van query's en analyses.
* Meer informatie over de voordelen van [elastische groepen](sql-database-elastic-pool.md).
* Leer hoe u [ een webtoepassing maakt die verbinding maakt met SQL Database op de back-end](../app-service/app-service-web-tutorial-dotnet-sqldatabase.md).
