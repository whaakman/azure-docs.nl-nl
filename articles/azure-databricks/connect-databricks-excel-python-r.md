---
title: 'Verbinding maken met Azure Databricks vanuit Excel-, Python of R '
description: Informatie over het gebruik van het stuurprogramma Simba verbinding maken met Azure Databricks Excel-, Python of R.
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: mamccrea
ms.openlocfilehash: 6c8d0c9f5471395d1552b896b4d411dcad0a280f
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50421132"
---
# <a name="connect-to-azure-databricks-from-excel-python-or-r"></a>Verbinding maken met Azure Databricks vanuit Excel-, Python of R

In dit artikel leert u hoe u het Databricks ODBC-stuurprogramma voor het verbinding maken met Azure Databricks met Microsoft Excel, Python of R-taal. Nadat u de verbinding tot stand brengen, kunt u de toegang tot de gegevens in Azure Databricks uit de Excel-, Python of R-clients. U kunt ook de clients gebruiken de gegevens verder te analyseren. 

## <a name="prerequisites"></a>Vereisten

* U moet een Azure Databricks-werkruimte, een Spark-cluster en de voorbeeldgegevens die zijn gekoppeld aan het cluster hebben. Als u nog geen deze vereisten, voert u de Quick Start op [een Spark-taak uitvoeren op Azure Databricks met behulp van de Azure-portal](quickstart-create-databricks-workspace-portal.md).

* Download het Databricks ODBC-stuurprogramma van [Databricks stuurprogramma-downloadpagina](https://databricks.com/spark/odbc-driver-download). Installeer de 32-bits of 64-bits versie, afhankelijk van de toepassing vanaf waar u verbinding maken met Azure Databricks. Bijvoorbeeld als u wilt verbinding maken vanuit Excel, installeert u de 32-bits versie van het stuurprogramma. Als u wilt verbinding maken vanuit R en Python, installeert u de 64-bits versie van het stuurprogramma.

* Instellen van een persoonlijk toegangstoken in Databricks. Zie voor instructies [management Token](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="set-up-a-dsn"></a>Instellen van een DSN-naam

De naam van een gegevensbron (DSN) bevat informatie over een specifieke gegevensbron. Een ODBC-stuurprogramma moet deze DSN verbinding maken met een gegevensbron. In deze sectie stelt u een DSN-naam die kan worden gebruikt met het Databricks ODBC-stuurprogramma om verbinding maken met Azure Databricks van clients, zoals Microsoft Excel, Python of R.

1. Navigeer naar de Databricks-cluster van de Azure Databricks-werkruimte.

    ![Open Databricks-cluster](./media/connect-databricks-excel-python-r/open-databricks-cluster.png "Open Databricks-cluster")

2. Onder de **configuratie** en klik op de **JDBC/ODBC** tabblad en kopieer de waarden voor **hostnaam Server** en **HTTP-pad**. U moet deze waarden om de stappen in dit artikel te voltooien.

    ![Databricks-configuratie](./media/connect-databricks-excel-python-r/get-databricks-jdbc-configuration.png "Databricks ophalen-configuratie")

3. Start op uw computer **ODBC-gegevensbronnen** toepassing (32-bits of 64-bits), afhankelijk van de toepassing. Voor verbinding vanuit Excel, gebruikt u de 32-bits versie. Als u wilt verbinding maken vanuit R en Python, gebruikt u de 64-bits versie.

    ![Starten van ODBC](./media/connect-databricks-excel-python-r/launch-odbc-app.png "ODBC-app starten")

4. Onder de **gebruiker DSN** tabblad **toevoegen**. In de **nieuwe gegevensbron maken** in het dialoogvenster, selecteer de **Simba Spark ODBC-stuurprogramma**, en klik vervolgens op **voltooien**.

    ![Starten van ODBC](./media/connect-databricks-excel-python-r/add-new-user-dsn.png "ODBC-app starten")

5. In de **Simba Spark ODBC-stuurprogramma** dialoogvenster Geef de volgende waarden:

    ![Configureren van DSN](./media/connect-databricks-excel-python-r/odbc-dsn-setup.png "-DSN configureren")

    De volgende tabel bevat informatie over de waarden die u moet opgeven in het dialoogvenster.
    
    |Veld  | Waarde  |
    |---------|---------|
    |**Naam van de gegevensbron**     | Geef een naam voor de gegevensbron.        |
    |**Host(s)**     | Geef de waarde die u hebt gekopieerd uit de Databricks-werkruimte voor *Server hostnaam*.        |
    |**Poort**     | Voer *443*.        |
    |**Verificatie** > **mechanisme**     | Selecteer *gebruikersnaam en wachtwoord*.        |
    |**Gebruikersnaam**     | Voer *token*.        |
    |**Wachtwoord**     | Voer een waarde voor de token die u hebt gekopieerd uit de Databricks-werkruimte. |
    
    De volgende aanvullende stappen uitvoeren in het dialoogvenster van de DSN-instelling.
    
    * Klik op **opties voor HTTP-**. Plak in het dialoogvenster dat wordt geopend, de waarde voor *HTTP-pad* die u hebt gekopieerd uit de Databricks-werkruimte. Klik op **OK**.
    * Klik op **SSL-opties**. Selecteer in het dialoogvenster dat wordt geopend, de **SSL inschakelen** selectievakje. Klik op **OK**.
    * Klik op **testen** voor het testen van de verbinding met Azure Databricks. Klik op **OK** aan de configuratie op te slaan.
    * In de **ODBC-gegevensbronbeheer** in het dialoogvenster, klikt u op **OK**.

U hebt nu uw DSN instellen. In de volgende secties gebruikt u deze DSN verbinding maken met Azure Databricks vanuit Excel-, Python of R.

## <a name="connect-from-microsoft-excel"></a>Verbinding maken via Microsoft Excel

In deze sectie maakt ophalen u gegevens uit Azure Databricks in Microsoft Excel met behulp van de DSN die u eerder hebt gemaakt. Voordat u begint, zorg ervoor dat u Microsoft Excel is geïnstalleerd op uw computer hebt. U kunt een evaluatieversie van Excel van [Microsoft Excel-proefversie koppeling](https://products.office.com/excel).

1. Een lege werkmap openen in Microsoft Excel. Uit de **gegevens** lint, klikt u op **gegevens ophalen**. Klik op **van andere bronnen** en klik vervolgens op **uit ODBC**.

    ![ODBC vanuit Excel starten](./media/connect-databricks-excel-python-r/launch-odbc-from-excel.png "ODBC vanuit Excel starten")

2. In de **uit ODBC** in het dialoogvenster, selecteer de DSN-naam die u eerder hebt gemaakt en klik vervolgens op **OK**.

    ![Selecteer DSN](./media/connect-databricks-excel-python-r/excel-select-dsn.png "DSN selecteren")

3. Als u wordt gevraagd om referenties, voor de gebruikersnaam van de voert **token**. Voor wachtwoord, geeft u de token waarde die u hebt opgehaald via de Databricks-werkruimte.

    ![Geef referenties op voor Databricks](./media/connect-databricks-excel-python-r/excel-databricks-token.png "DSN selecteren")

4. Selecteer in het venster navigator, de tabel in Databricks die u wilt laden naar Excel en klik vervolgens op **laden**. 

    ![Dta laden in Excel](./media/connect-databricks-excel-python-r/excel-load-data.png "dta laden in Excel")

Zodra u de gegevens in uw Excel-werkmap hebt, kunt u analytische bewerkingen op kunt uitvoeren.

## <a name="connect-from-r"></a>Verbinding maken vanuit R

> [!NOTE]
> In deze sectie bevat informatie over het integreren van een R Studio-client die wordt uitgevoerd op het bureaublad met Azure Databricks. Zie voor instructies over het gebruik van R Studio op de Azure Databricks-cluster zelf [R Studio op Azure Databricks](https://docs.azuredatabricks.net/spark/latest/sparkr/rstudio.html).

In deze sectie maakt u een taal R IDE verwijzen naar gegevens die beschikbaar zijn in Azure Databricks. Voordat u begint, moet u het volgende zijn geïnstalleerd op de computer hebben.

* Een IDE voor R-taal. Dit artikel wordt gebruikgemaakt van RStudio Desktop. U kunt installeren vanuit [R Studio downloaden](https://www.rstudio.com/products/rstudio/download/).
* Als u RStudio Desktop als uw IDE, ook installatie van Microsoft R Client [ http://aka.ms/rclient/ ](http://aka.ms/rclient/). 

RStudio openen en voer de volgende stappen uit:

- Naslaginformatie over de `RODBC` pakket. Hiermee kunt u verbinding maken met Azure Databricks met behulp van de DSN die u eerder hebt gemaakt.
- Stel een verbinding met behulp van de DSN-naam.
- Een SQL-query uitvoeren op de gegevens in Azure Databricks. In het volgende codefragment *radio_sample_data* is een tabel die al in Azure Databricks bestaat.
- Sommige bewerkingen uitvoeren op de query om te controleren of de uitvoer. 

Deze taken worden uitgevoerd door het volgende codefragment:

    # reference the 'RODBC' package
    require(RODBC)
    
    # establish a connection using the DSN you created earlier
    conn <- odbcConnect("<ENTER DSN NAME HERE>")
    
    # run a SQL query using the connection you created
    res <- sqlQuery(conn, "SELECT * FROM radio_sample_data")
    
    # print out the column names in the query output
    names(res) 
        
    # print out the number of rows in the query output
    nrow (res)

## <a name="connect-from-python"></a>Verbinding maken met Python

In deze sectie maakt u een Python IDE (zoals niet-actief) verwijzen naar gegevens die beschikbaar zijn in Azure Databricks. Voordat u begint, moet u de volgende vereisten voldoen:

* Installeer Python van [hier](https://www.python.org/downloads/). Python installeren via deze koppeling, installeert ook niet-actief.

* Installeren vanaf een opdrachtprompt op de computer, de `pyodbc` pakket. Voer de volgende opdracht uit:

      pip install pyodbc

Niet-actief te openen en voer de volgende stappen uit:

- Importeren van de `pyodbc` pakket. Hiermee kunt u verbinding maken met Azure Databricks met behulp van de DSN die u eerder hebt gemaakt.
- Stel een verbinding met behulp van de DSN die u eerder hebt gemaakt.
-  Een SQL-query uitvoeren met de verbinding dat u hebt gemaakt. In het volgende codefragment *radio_sample_data* is een tabel die al in Azure Databricks bestaat.
- Bewerkingen uitvoeren op de query om te controleren of de uitvoer.

Deze taken worden uitgevoerd door het volgende codefragment:

```python
# import the `pyodbc` package:
import pyodbc

# establish a connection using the DSN you created earlier
conn = pyodbc.connect("DSN=<ENTER DSN NAME HERE>", autocommit = True)

# run a SQL query using the connection you created
cursor = conn.cursor()
cursor.execute("SELECT * FROM radio_sample_data")

# print the rows retrieved by the query.
for row in cursor.fetchall():
    print(row)

```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over bronnen van waar u gegevens in Azure Databricks importeren kunt, [gegevensbronnen voor Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#)


