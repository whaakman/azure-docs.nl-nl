---
title: Verbinding maken met Azure Databricks van Excel, Python of R | Microsoft Docs
description: Informatie over het gebruik van het stuurprogramma Simba verbinding maken met Azure Databricks Excel, Python of R.
services: azure-databricks
documentationcenter: ''
author: nitinme
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: bbf75a03fb771aa415a26e151614cecfaa14c485
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34598875"
---
# <a name="connect-to-azure-databricks-from-excel-python-or-r"></a>Verbinding maken met Azure Databricks van Excel, Python of R

Informatie over het gebruik van het Databricks ODBC-stuurprogramma voor Azure Databricks verbinding met Microsoft Excel, Python of R-taal in dit artikel. Zodra u de verbinding tot stand brengen, kunt u toegang tot de gegevens in Azure Databricks van de Excel-, Python of R-clients. U kunt ook de clients gebruiken de gegevens verder te analyseren. 

## <a name="prerequisites"></a>Vereisten

* U moet een Azure-Databricks-werkruimte, een Spark-cluster en de voorbeeldgegevens die zijn gekoppeld aan het cluster hebben. Als u nog geen deze vereisten, voert u de Quick Start op [een Spark-taak uitvoeren op Azure Databricks met de Azure portal](quickstart-create-databricks-workspace-portal.md).

* Download het Databricks ODBC-stuurprogramma van [Databricks stuurprogramma downloadpagina](https://databricks.com/spark/odbc-driver-download). De 32-bits of 64-bits versie afhankelijk van de toepassing waaruit u wilt verbinding maken met Azure Databricks installeren. Bijvoorbeeld wilt maken van Excel, installeert u de 32-bits versie van het stuurprogramma. Installeer de 64-bits versie van het stuurprogramma voor de verbinding van het R- en Python wordt.

* Een persoonlijk toegangstoken in Databricks instellen. Zie voor instructies [management Token](https://docs.azuredatabricks.net/api/latest/authentication.html#token-management).

## <a name="set-up-a-dsn"></a>Een DSN instellen

Een data source name (DSN) bevat informatie over een specifieke gegevensbron. Een ODBC-stuurprogramma moet deze DSN verbinding maken met een gegevensbron. In deze sectie maakt u een DSN die kan worden gebruikt met het Databricks ODBC-stuurprogramma verbinding maken met Azure Databricks van clients, zoals Microsoft Excel, Python of R. hebt ingesteld

1. Ga naar het cluster Databricks vanuit de werkruimte Azure Databricks.

    ![Open Databricks cluster](./media/connect-databricks-excel-python-r/open-databricks-cluster.png "Open Databricks cluster")

2. Onder de **configuratie** en klik op de **JDBC/ODBC** tabblad en kopieer de waarden voor **hostnaam van de synchronisatieserver** en **HTTP-pad**. U moet deze waarden om de stappen in dit artikel te voltooien.

    ![Configuratie van Databricks ophalen](./media/connect-databricks-excel-python-r/get-databricks-jdbc-configuration.png "Databricks ophalen-configuratie")

3. Start op uw computer **ODBC-gegevensbronnen** toepassing (32-bits of 64-bits), afhankelijk van de toepassing. Voor verbinding vanuit Excel, moet u de 32-bits-versie gebruiken. Voor de verbinding van het R- en Python wordt door de 64-bits versie te gebruiken.

    ![Starten van ODBC](./media/connect-databricks-excel-python-r/launch-odbc-app.png "ODBC-app starten")

4. Onder de **gebruiker DSN** tabblad **toevoegen**. In de **nieuwe gegevensbron maken** selecteert u de **Simba Spark ODBC-stuurprogramma**, en klik vervolgens op **voltooien**.

    ![Starten van ODBC](./media/connect-databricks-excel-python-r/add-new-user-dsn.png "ODBC-app starten")

5. In de **Simba Spark ODBC-stuurprogramma** dialoogvenster Geef de volgende waarden:

    ![Configureer DSN](./media/connect-databricks-excel-python-r/odbc-dsn-setup.png "DSN configureren")

    De volgende tabel bevat informatie over de waarden die u moet opgeven in het dialoogvenster.
    
    |Veld  | Waarde  |
    |---------|---------|
    |**De naam van gegevensbron**     | Geef een naam voor de gegevensbron.        |
    |**Host(s)**     | Geef de waarde die u hebt gekopieerd uit de werkruimte Databricks voor *hostnaam van de synchronisatieserver*.        |
    |**Poort**     | Voer *443*.        |
    |**Verificatie** > **mechanisme**     | Selecteer *gebruikersnaam en wachtwoord*.        |
    |**Gebruikersnaam**     | Voer *token*.        |
    |**Wachtwoord**     | Voer een waarde voor de token die u hebt gekopieerd uit de werkruimte Databricks. |
    
    De volgende aanvullende stappen uitvoeren in het dialoogvenster van de DSN-instelling.
    
    * Klik op **opties voor HTTP**. Plak in het dialoogvenster dat wordt geopend, de waarde voor *HTTP-pad* die u hebt gekopieerd uit Databricks werkruimte. Klik op **OK**.
    * Klik op **SSL opties**. Selecteer in het dialoogvenster dat wordt geopend, de **SSL inschakelen** selectievakje. Klik op **OK**.
    * Klik op **testen** voor het testen van de verbinding met Azure Databricks. Klik op **OK** aan de configuratie op te slaan.
    * In de **ODBC Data Source Administrator** in het dialoogvenster, klikt u op **OK**.

U hebt nu uw DSN instellen. In de volgende secties vindt u deze DSN gebruiken voor verbinding met Azure Databricks vanuit Excel, Python of R.

## <a name="connect-from-microsoft-excel"></a>Verbinding maken via Microsoft Excel

In deze sectie kunt ophalen u gegevens uit Azure Databricks in Microsoft Excel met behulp van de DSN die u eerder hebt gemaakt. Zorg voordat u begint, hebt u Microsoft Excel op uw computer geïnstalleerd. U kunt een evaluatieversie van Excel van [Microsoft Excel-proefabonnement koppeling](https://products.office.com/excel).

1. Een lege werkmap openen in Microsoft Excel. Van de **gegevens** lint, klikt u op **gegevens ophalen**. Klik op **van andere bronnen** en klik vervolgens op **van ODBC**.

    ![ODBC vanuit Excel starten](./media/connect-databricks-excel-python-r/launch-odbc-from-excel.png "ODBC vanuit Excel starten")

2. In de **van ODBC** in het dialoogvenster, selecteer de DSN die u eerder hebt gemaakt en klik vervolgens op **OK**.

    ![Selecteer DSN](./media/connect-databricks-excel-python-r/excel-select-dsn.png "DSN selecteren")

3. Als u wordt gevraagd om referenties, voor de gebruikersnaam van de voert **token**. Geef een waarde voor de token die u hebt opgehaald uit de werkruimte Databricks voor wachtwoord.

    ![Geef referenties op voor Databricks](./media/connect-databricks-excel-python-r/excel-databricks-token.png "DSN selecteren")

4. Selecteer in het venster navigator in de tabel in Databricks die u wilt laden naar Excel en klik vervolgens op **laden**. 

    ![Dta laden in Excel](./media/connect-databricks-excel-python-r/excel-load-data.png "Load dta in Excel")

Zodra u de gegevens in uw Excel-werkmap hebt, kunt u analytische bewerkingen op kunt uitvoeren.

## <a name="connect-from-r"></a>Verbinding maken vanaf R

In deze sectie maakt u een taal R IDE verwijzen naar gegevens beschikbaar zijn in Azure Databricks gebruiken. Voordat u begint, moet u het volgende op de computer geïnstalleerd hebben.

* Een IDE voor R-taal. Dit artikel wordt RStudio gebruikt voor het bureaublad. Kunt u het installeren van [R Studio downloaden](https://www.rstudio.com/products/rstudio/download/).
* Als u RStudio voor bureaublad als uw IDE, installeer dan ook Microsoft R Client [ http://aka.ms/rclient/ ](http://aka.ms/rclient/). 

Open RStudio en voer de volgende stappen uit:

- Verwijzing naar de `RODBC` pakket. Hiermee kunt u verbinding maken met Azure Databricks met behulp van de DSN die u eerder hebt gemaakt.
- Stel een verbinding met de DSN-naam.
- Een SQL-query uitvoeren op de gegevens in Azure Databricks. In het volgende fragment *radio_sample_data* is een tabel die al in Azure Databricks bestaat.
- Bepaalde bewerkingen uitvoeren op de query om te controleren of de uitvoer. 

Het volgende codefragment voert deze taken uit:

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

In deze sectie maakt u een Python IDE (zoals niet-actief) verwijzen naar gegevens beschikbaar zijn in Azure Databricks gebruiken. Voordat u begint, moet u de volgende vereisten voldoen:

* Installeren van Python van [hier](https://www.python.org/downloads/). Het installeren van Python van deze koppeling, installeert ook niet-actief.

* Installeren vanaf een opdrachtprompt op de computer, de `pyodbc` pakket. Voer de volgende opdracht uit:

      pip install pyodbc

Open van niet-actief en voer de volgende stappen uit:

- Importeer de `pyodbc` pakket. Hiermee kunt u verbinding maken met Azure Databricks met behulp van de DSN die u eerder hebt gemaakt.
- Stel een verbinding met de DSN die u eerder hebt gemaakt.
-  Voer een SQL-query met behulp van de verbinding dat u hebt gemaakt. In het volgende fragment *radio_sample_data* is een tabel die al in Azure Databricks bestaat.
- Bewerkingen uitvoeren op de query om te controleren of de uitvoer.

Het volgende codefragment voert deze taken uit:

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

* Zie voor meer informatie over bronnen vanaf waar u gegevens in Azure Databricks importeren kunt, [gegevensbronnen voor Azure Databricks](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#)


