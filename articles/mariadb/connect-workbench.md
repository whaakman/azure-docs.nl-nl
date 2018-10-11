---
title: Verbinding maken met Azure Database for MariaDB via MySQL Workbench
description: Deze snelstart bevat de stappen voor het gebruik van MySQL Workbench om verbinding te maken met en gegevens op te vragen uit Azure Database for MariaDB.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.custom: mvc
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: b3a4d00381361b5299e86b959d9775318ae81e88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998228"
---
# <a name="azure-database-for-mariadb-use-mysql-workbench-to-connect-and-query-data"></a>Azure Database for MariaDB: MySQL Workbench gebruiken om verbinding te maken en gegevens op te vragen
In deze snelstart ziet u hoe u met behulp van een MySQL Workbench-toepassing verbinding maakt met een Azure Database for MariaDB. 

## <a name="prerequisites"></a>Vereisten
In deze snelstartgids worden de resources die in een van deze handleidingen zijn gemaakt, als uitgangspunt gebruikt:
- [Een Azure Database for MariaDB-server maken met behulp van de Azure-portal](./quickstart-create-mariadb-server-database-using-azure-portal.md)
- [Een Azure Database for MariaDB-server maken met behulp van Azure CLI](./quickstart-create-mariadb-server-database-using-azure-cli.md)

## <a name="install-mysql-workbench"></a>MySQL Workbench installeren
MySQL Workbench downloaden en installeren op de computer vanaf de [MySQL-website](https://dev.mysql.com/downloads/workbench/).

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen
Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure Database for MariaDB. U hebt de volledig gekwalificeerde servernaam en aanmeldingsreferenties nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Klik in het menu aan de linkerkant in Azure Portal op **Alle resources** en zoek naar de server die u hebt gemaakt (bijvoorbeeld **mydemoserver**).

3. Klik op de servernaam.

4. Ga naar het venster **Overzicht** van de server en noteer de **Servernaam** en de **Aanmeldingsnaam van de serverbeheerder**. Als u uw wachtwoord vergeet, kunt u het wachtwoord in dit venster opnieuw instellen.
 ![Naam Azure Database for MariaDB-server](./media/connect-workbench/1_server-overview-name-login.png)

## <a name="connect-to-server-using-mysql-workbench"></a>Verbinding maken met de server met behulp van MySQL Workbench 
Gebruik de volgende stappen om een Azure Database for MariaDB-server te verbinden met behulp van MySQL Workbench:

1.  Start de toepassing MySQL Workbench op de computer. 

2.  Voer in het dialoogvenster **Nieuwe verbinding instellen** de volgende gegevens in op het tabblad **Parameters**:

    ![nieuwe verbinding instellen](./media/connect-workbench/2-setup-new-connection.png)

    | **Instelling** | **Voorgestelde waarde** | **Beschrijving van veld** |
    |---|---|---|
    |   Verbindingsnaam | Demo-verbinding | Geef een label op voor deze verbinding. |
    | Verbindingsmethode | Standard (TCP/IP) | Standard (TCP/IP) is voldoende. |
    | Hostnaam | *servernaam* | Geef de waarde van de servernaam op die u hebt gebruikt toen u de Azure Database for MariaDB-server eerder hebt gemaakt. De server in ons voorbeeld is mydemoserver.mariadb.database.azure.com. Gebruik de FQDN (Fully Qualified Domain Name) (\*.mariadb.database.azure.com) zoals weergegeven in het voorbeeld. Volg de stappen in de vorige sectie om de verbindingsgegevens op te halen als u de servernaam bent vergeten.  |
    | Poort | 3306 | Gebruik altijd poort 3306 bij het verbinden met Azure Database for MariaDB. |
    | Gebruikersnaam |  *aanmeldnaam van serverbeheerder* | Typ de gebruikersnaam van de serverbeheerder die u hebt opgegeven toen u de Azure Database for MariaDB-server eerder hebt gemaakt. De gebruikersnaam in ons voorbeeld is myadmin@mydemoserver. Volg de stappen in de vorige sectie om de verbindingsgegevens op te halen als u de gebruikersnaam bent vergeten. De indeling is *username@servername*.
    | Wachtwoord | Uw wachtwoord | Klik op de knop **Opslaan in de kluis...** om het wachtwoord op te slaan. |

3.   Klik op **Verbinding testen** om te controleren of alle parameters juist zijn geconfigureerd. 

4.   Klik op **OK** om de verbinding op te slaan. 

5.   Klik in de lijst met **MySQL Connections** op de tegel die voor uw server is bedoeld en wacht tot de verbinding tot stand is gebracht.

        Er wordt een nieuw SQL-tabblad geopend met een lege editor waar u uw query's kunt typen.
    
        > [!NOTE]
        > Standaard wordt SSL-verbindingsbeveiliging vereist. Deze wordt afgedwongen op uw Azure Database for MariaDB-server. Hoewel er gewoonlijk geen aanvullende configuratie met SSL-certificaten is vereist om MySQL Workbench verbinding met de server te laten maken, verdient het aanbeveling de SSL CA-certificering te binden aan MySQL Workbench. Als u SSL wilt uitschakelen, gaat u naar Azure Portal en klikt u op de pagina Verbindingsbeveiliging om de wisselknop SSL-verbinding afdwingen op Uit te zetten.

## <a name="create-table-insert-read-update-and-delete-data"></a>Tabel maken, gegevens invoegen, lezen, bijwerken en verwijderen
1. Kopieer de SQL-voorbeeldcode en plak deze in een leeg SQL-tabblad om enkele voorbeeldgegevens te illustreren.

    Met deze code wordt een lege database gemaakt met de naam quickstartdb. Vervolgens wordt er een voorbeeldtabel gemaakt met de naam inventory (voorraad). Er worden een paar rijen ingevoegd, die vervolgens gelezen worden. De gegevens worden gewijzigd met een update-instructie, waarna de rijen opnieuw worden gelezen. Ten slotte wordt er een rij verwijderd, en worden de rijen opnieuw gelezen.
    
    ```sql
    -- Create a database
    -- DROP DATABASE IF EXISTS quickstartdb;
    CREATE DATABASE quickstartdb;
    USE quickstartdb;
    
    -- Create a table and insert rows
    DROP TABLE IF EXISTS inventory;
    CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);
    INSERT INTO inventory (name, quantity) VALUES ('banana', 150);
    INSERT INTO inventory (name, quantity) VALUES ('orange', 154);
    INSERT INTO inventory (name, quantity) VALUES ('apple', 100);
    
    -- Read
    SELECT * FROM inventory;
    
    -- Update
    UPDATE inventory SET quantity = 200 WHERE id = 1;
    SELECT * FROM inventory;
    
    -- Delete
    DELETE FROM inventory WHERE id = 2;
    SELECT * FROM inventory;
    ```

    In de schermopname wordt een voorbeeld getoond van SQL-code in SQL Workbench, alsmede de uitvoer nadat de code is uitgevoerd.
    
    ![Tabblad MySQL Workbench voor het uitvoeren van SQL-code](media/connect-workbench/3-workbench-sql-tab.png)

2. Als u de SQL-code wilt uitvoeren, klikt u op het bliksemstraalpictogram in de werkbalk van het tabblad **SQL File**.
3. Let op de drie tabbladen met resultaten in de sectie **Result Grid** in het midden van de pagina. 
4. Let ook op de lijst **Output** onderaan de pagina. De status van elke opdracht wordt weergegeven. 

U hebt nu verbinding gemaakt met Azure Database for MariaDB met behulp van MyQL Workbench en u hebt query's uitgevoerd op gegevens met behulp van de taal SQL.

<!--
## Next steps
> [!div class="nextstepaction"]
> [Migrate your database using Export and Import](./concepts-migrate-import-export.md)
-->