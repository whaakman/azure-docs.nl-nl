---
title: Importeren en exporteren in Azure Database for MySQL
description: In dit artikel wordt uitgelegd gebruikelijke manieren om te importeren en exporteren van databases in Azure Database voor MySQL, met behulp van hulpprogramma's, zoals MySQL Workbench.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/01/2018
ms.openlocfilehash: fa72037c8f54271f5651667765c5d5e2e9c03619
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/17/2018
ms.locfileid: "53545515"
---
# <a name="migrate-your-mysql-database-by-using-import-and-export"></a>Migreren van uw MySQL-database met behulp van importeren en exporteren
Dit artikel wordt uitgelegd twee algemene benaderingen gebruikt om te importeren en exporteren van gegevens naar een Azure Database for MySQL-server met behulp van MySQL Workbench. 

## <a name="before-you-begin"></a>Voordat u begint
Als u wilt in deze gebruiksaanwijzing kunt doorlopen, hebt u het volgende nodig:
- Een Azure Database for MySQL-server door het volgende [maken van een Azure Database for MySQL-server met behulp van Azure portal](quickstart-create-mysql-server-database-using-azure-portal.md).
- MySQL Workbench [gedownload](https://dev.mysql.com/downloads/workbench/), of een ander MySQL-hulpprogramma voor importeren en exporteren.

## <a name="use-common-tools"></a>Gebruik algemene hulpprogramma 's
Gebruik algemene hulpprogramma's zoals MySQL Workbench, Toad of Navicat op afstand verbinding maken en importeren of exporteren van gegevens in Azure Database voor MySQL. 

Gebruik deze hulpprogramma's op de clientcomputer met een internetverbinding verbinding maakt met Azure Database voor MySQL. Een met SSL versleutelde verbinding gebruiken voor aanbevolen procedures voor beveiliging, zoals beschreven in [configureren van SSL-verbindingen in Azure Database for MySQL](concepts-ssl-connection-security.md).

U hoeft niet te verplaatsen van uw importeren en exporteren van bestanden naar een locatie speciale cloud bij het migreren naar Azure Database voor MySQL. 

## <a name="create-a-database-on-the-azure-database-for-mysql-server"></a>Maak een database in de Azure Database for MySQL-server
Een lege database maken op de Azure Database for MySQL-server waar u om de gegevens te migreren. Gebruik een hulpprogramma zoals MySQL Workbench, Toad of Navicat om de database te maken. De database kan hebben dezelfde naam als de database die de gedumpte gegevens bevat, of u kunt een database maken met een andere naam.

Als u wilt verbinding maken, zoekt u de verbindingsgegevens in de **overzicht** van uw Azure Database for MySQL.

![Informatie over de verbinding in Azure portal](./media/concepts-migrate-import-export/1_server-overview-name-login.png)

De verbindingsgegevens toevoegen aan MySQL Workbench.

![MySQL Workbench-verbindingsreeks](./media/concepts-migrate-import-export/2_setup-new-connection.png)

## <a name="determine-when-to-use-import-and-export-techniques-instead-of-a-dump-and-restore"></a>Bepalen wanneer te gebruiken importeren en exporteren van technieken in plaats van een dump en herstellen
MySQL-hulpprogramma's gebruiken om te importeren en exporteren van databases naar Azure MySQL-Database in de volgende scenario's. In andere scenario's, profiteert u mogelijk met behulp van de [dump maken en terugzetten](concepts-migrate-dump-restore.md) benadering in plaats daarvan. 

- Als u een keuze maken uit een paar tabellen van een bestaande database voor MySQL in Azure MySQL-Database importeren wilt, is het raadzaam te importeren en exporteren van techniek.  Op deze manier kunt u eventuele overbodige tabellen van de migratie naar Bespaar tijd en resources weglaten. Gebruik bijvoorbeeld de `--include-tables` of `--exclude-tables` overschakelen met [mysqlpump](https://dev.mysql.com/doc/refman/5.7/en/mysqlpump.html#option_mysqlpump_include-tables) en de `--tables` overschakelen met [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html#option_mysqldump_tables).
- Wanneer u de database-objecten dan tabellen verplaatsen wilt, expliciet die objecten te maken. Zijn beperkingen (primaire sleutel, refererende sleutel, indexen), weergaven, functies, procedures, triggers en eventuele andere databaseobjecten die u wilt migreren.
- Als uit externe gegevensbronnen dan een MySQL-database u gegevens migreert waarnaar, platte bestanden maken en deze importeren met behulp van [mysqlimport](https://dev.mysql.com/doc/refman/5.7/en/mysqlimport.html).

Zorg ervoor dat alle tabellen in de database de InnoDB-opslag-engine gebruiken wanneer u gegevens in Azure Database voor MySQL laadt. Azure Database for MySQL ondersteunt alleen de InnoDB opslag-engine, zodat deze biedt geen ondersteuning voor alternatieve opslag-engines. Als uw tabellen alternatieve opslag-engines moet, zorg er dan voor dat ze voor het gebruik van de InnoDB-engine-indeling vóór de migratie naar Azure Database voor MySQL. 

Bijvoorbeeld, als u een WordPress- of web-app die gebruikmaakt van de MyISAM-engine hebt, converteert u eerst de tabellen door de gegevens te migreren naar InnoDB-tabellen. Vervolgens herstellen met Azure Database voor MySQL. Gebruik de component `ENGINE=INNODB` in te stellen de engine voor het maken van een tabel, draagt u de gegevens in de tabel compatibel vóór de migratie. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```

## <a name="performance-recommendations-for-import-and-export"></a>Aanbevelingen voor prestaties voor importeren en exporteren
-   Geclusterde indexen en primaire sleutels maken voor het laden van gegevens. Laden van gegevens in de volgorde van de primaire sleutel. 
-   Vertraging maken van secundaire indexen nadat gegevens zijn geladen. Alle secundaire indexen maken na het laden. 
-   Beperkingen voor referentiële sleutels voordat deze worden geladen uitschakelen. Refererende sleutel controles uit te schakelen biedt aanzienlijke prestatievoordelen. Inschakelen van de beperkingen en controleer of de gegevens na het laden om referentiële integriteit te waarborgen.
-   Gegevens parallel laden. Voorkomen dat te veel parallelle uitvoering die zou u een resourcelimiet bereikt en resources bewaken met behulp van de metrische gegevens beschikbaar zijn in de Azure-portal. 
-   Gepartitioneerde tabellen, indien van toepassing gebruiken.

## <a name="import-and-export-by-using-mysql-workbench"></a>Importeren en exporteren met behulp van MySQL Workbench
Er zijn twee manieren om te exporteren en importeren van gegevens in MySQL Workbench. Elk fungeert een ander doel. 

### <a name="table-data-export-and-import-wizards-from-the-object-browsers-context-menu"></a>Gegevens in een tabel exporteren en importeren van wizards in het contextmenu van de object-browser
![MySQL Workbench wizards in het contextmenu van de object-browser](./media/concepts-migrate-import-export/p1.png)

De wizards voor het tabelgegevens ondersteuning voor importeren en exporteren van bewerkingen met behulp van CSV en JSON-bestanden. Ze bevatten verschillende configuratieopties, zoals scheidingstekens, kolom selecteren en codering selecteren. U kunt elke wizard tegen lokale of extern verbonden MySQL-servers uitvoeren. De importactie bevat een tabel-, kolom- en toewijzing van het type. 

U kunt deze wizards in het contextmenu van het object browser openen met de rechtermuisknop op een tabel. Kies vervolgens een **Wizard tabel gegevens exporteren** of **Wizard tabel importeren**. 

#### <a name="table-data-export-wizard"></a>Wizard tabel gegevens exporteren
Het volgende voorbeeld wordt de tabel geëxporteerd naar een CSV-bestand: 
1. Met de rechtermuisknop op de tabel van de database moet worden geëxporteerd. 
2. Selecteer **tabel van de Wizard Export Data**. Selecteer de kolommen die moeten worden geëxporteerd, rij-offset (indien aanwezig) en tellen (indien aanwezig). 
3. Op de **Selecteer gegevens voor het exporteren van** pagina, klikt u op **volgende**. Selecteer het bestandspad, CSV of JSON-bestandstype. Ook het regelscheidingsteken, de methode van tekenreeksen en veldscheidingsteken insluitende selecteren. 
4. Op de **Selecteer uitvoer bestandslocatie** pagina, klikt u op **volgende**. 
5. Op de **gegevens exporteren** pagina, klikt u op **volgende**.

#### <a name="table-data-import-wizard"></a>Wizard tabel importeren
Het volgende voorbeeld worden geïmporteerd in de tabel uit een CSV-bestand:
1. Met de rechtermuisknop op de tabel van de database moet worden geïmporteerd. 
2. Blader naar en selecteer het CSV-bestand worden geïmporteerd en klik vervolgens op **volgende**. 
3. Selecteer de doeltabel (nieuw of bestaand), en selecteer of schakel de **afkappen tabel voor het importeren** selectievakje. Klik op **volgende**.
4. Schakel codering en de kolommen die moeten worden geïmporteerd en klik vervolgens op **volgende**. 
5. Op de **gegevens importeren** pagina, klikt u op **volgende**. De wizard importeert de gegevens dienovereenkomstig.

### <a name="sql-data-export-and-import-wizards-from-the-navigator-pane"></a>SQL-gegevens exporteren en importeren van wizards in het deelvenster Navigator
Een wizard exporteert of importeert SQL gegenereerd op basis van MySQL Workbench of gegenereerd op basis van de opdracht mysqldump gebruiken. Toegang tot deze wizards in de **Navigator** deelvenster of door te selecteren **Server** in het hoofdmenu. Selecteer vervolgens **gegevensexport** of **gegevensimport**. 

#### <a name="data-export"></a>Gegevensexport
![MySQL Workbench-gegevens exporteren met behulp van het navigatievenster](./media/concepts-migrate-import-export/p2.png)

U kunt de **gegevensexport** tabblad om uw MySQL-gegevens te exporteren. 
1. Selecteer elk schema dat u wilt exporteren, (optioneel) Kies specifieke schema objecten/tabellen uit elke schema en de uitvoer te genereren. Configuratie-opties zijn exporteren naar een zelfstandige SQL-bestand of projectmap, dump opgeslagen routines en gebeurtenissen of tabelgegevens overslaan. 
 
   U kunt ook **exporteren van een resultatenset** voor het exporteren van een specifieke resultatenset in de SQL-editor naar een andere indeling, zoals CSV, JSON, HTML en XML. 
3. Selecteer de databaseobjecten te exporteren en configureer de bijbehorende opties.
4. Klik op **vernieuwen** laden van de objecten in de huidige.
5. (Optioneel) open de **geavanceerde opties** tabblad om de exportbewerking te verfijnen. Bijvoorbeeld, toevoegen tabel wordt vergrendeld, vervangen voor gebruik in plaats van insert-instructies en prijsopgave-id's met backtick tekens.
6. Klik op **Start exporteren** om te beginnen met het exportproces begeleidt.


#### <a name="data-import"></a>Gegevens importeren
![MySQL Workbench-gegevens importeren met behulp van beheer van Navigator](./media/concepts-migrate-import-export/p3.png)

U kunt de **gegevens importeren** tabblad om te importeren of terugzetten geëxporteerde gegevens van de exportbewerking gegevens of van de opdracht mysqldump. 
1. Kies de projectmap of zelfstandige SQL-bestand, kiest u het schema dat u wilt importeren in of kies **nieuw** om een nieuw schema te definiëren. 
2. Klik op **Start importeren** om het importproces te starten.

## <a name="next-steps"></a>Volgende stappen
- Als een andere aanpak van databasemigratie, lezen [migreren uw MySQL-database met dump maken en terugzetten in Azure Database for MySQL](concepts-migrate-dump-restore.md).
- Voor meer informatie over het migreren van databases met Azure Database voor MySQL, Zie de [handleiding voor databasemigratie](https://aka.ms/datamigration). 
