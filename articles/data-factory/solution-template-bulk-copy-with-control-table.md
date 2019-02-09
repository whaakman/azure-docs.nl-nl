---
title: Bulksgewijs kopiëren van de database met controle-tabel met Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van een oplossingssjabloon volledig gegevens bulksgewijs kopiëren van een database met behulp van een besturingselement voor externe tabel voor het opslaan van Partitielijst met tabellen in gegevensbronnen met Azure Data Factory.
services: data-factory
documentationcenter: ''
author: dearandyxu
ms.author: yexu
ms.reviewer: douglasl
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/14/2018
ms.openlocfilehash: b267da18f2537e462ecda0ac265eac07a069c293
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967334"
---
# <a name="bulk-copy-from-database-with-control-table"></a>Bulksgewijs kopiëren van de database met controle-tabel

Wanneer u kopiëren van gegevens van uw datawarehouse, zoals Oracle-server, server Netezza, Teradata-server of SQL Server naar Azure wilt, hebt u de enorme hoeveelheid gegevens uit meerdere tabellen in gegevensbronnen laden. In de meeste gevallen moet gegevens verder worden gepartitioneerd in elke tabel zodat u rijen met meerdere threads parallel uit één tabel laden kunt. De huidige sjabloon is ontworpen voor deze aanvraag. 

Als u wilt het kopiëren van gegevens uit een klein aantal tabellen met kleine hoeveelheid gegevens, is het efficiënter om u naar 'data-hulpprogramma voor kopiëren' hebben slechts één activiteit of foreach-activiteit kopiëren + kopieeractiviteit in de pijplijn. Deze sjabloon is meer dan u nodig hebt voor dit eenvoudige use-case.

## <a name="about-this-solution-template"></a>Over deze oplossingssjabloon

Deze sjabloon wordt de lijst met partities van de brondatabase opgehaald uit een externe controle-tabel die moet worden gekopieerd naar het doelarchief, en vervolgens elke partitie in de brondatabase te herhalen en voert de kopieerbewerking gegevens.

De sjabloon bevat drie activiteiten:
-   Een **Lookup** activiteit naar de lijst met partities van de brondatabase op te halen uit de tabel van een externe controle.
-   Een **ForEach** activiteit voor het ophalen van de Partitielijst van lookup-activiteit en vervolgens herhalen elk van deze activiteit kopiëren.
-   Een **kopie** activiteit elke partitie van de bron-databasearchief naar doelarchief kopiëren.

De sjabloon definieert vijf parameters:
-   De parameter *Control_Table_Name* is de naam van de tabel voor de tabel voor externe controle. De tabel van het besturingselement wordt gebruikt voor het opslaan van de Partitielijst voor de brondatabase.
-   De parameter *Control_Table_Schema_PartitionID* is de naam van de kolom in de tabel extern beheer voor het opslaan van elke partitie-ID. Zorg ervoor dat de partitie-ID is uniek voor elke partitie in brondatabase.
-   De parameter *Control_Table_Schema_SourceTableName* is de naam van de kolom in de tabel extern beheer voor het opslaan van de naam van elke tabel vanuit de brondatabase.
-   De parameter *Control_Table_Schema_FilterQuery* is de naam van de kolom in de tabel extern beheer voor het opslaan van de filterquery voor de gegevens van elke partitie in brondatabase. Bijvoorbeeld, als u de gegevens gepartitioneerd op basis van elk jaar, de query die zijn opgeslagen in elke rij kan zijn vergelijkbaar als ' selecteren * van datasource waar LastModifytime > = '' 2015-01-01 00:00:00 '' en LastModifytime < = '' 31-12-2015 23:59:59.999'' '
-   De parameter *Data_Destination_Folder_Path* is het pad naar de map waar de gegevens in uw archief van de bestemming worden gekopieerd.  Deze parameter is alleen zichtbaar als de bestemming die u kiest een opslagplaats bestandsgebaseerde opslag is.  Als u ervoor SQL Data Warehouse als het doelarchief kiest, is er geen parameter die is vereist om te worden hier ingevoerde. Maar de namen van tabellen en het schema in SQL datawarehouse moeten gelijk zijn aan de blobs in brondatabase.

## <a name="how-to-use-this-solution-template"></a>Het gebruik van deze oplossingssjabloon

1. Een besturingselement-tabel maken in een SQL-server of SQL Azure voor het opslaan van de Partitielijst van de brondatabase voor bulksgewijs kopiëren.  In het onderstaande voorbeeld ziet u er zijn vijf partities in de brondatabase, waarbij drie partities voor één tabel zijn:*datasource_table* en er zijn twee partities voor een andere tabel:*project_table*. De kolom *LastModifytime* wordt gebruikt voor het partitioneren van de gegevens in de tabel *datasource_table* uit de brondatabase. De query gebruikt om te lezen van de eerste partitie is ' Selecteer * uit datasource_table waar LastModifytime > = '' 2015-01-01 00:00:00 '' en LastModifytime < = '' 31-12-2015 23:59:59.999'' '.  U ziet ook de vergelijkbare query voor het lezen van gegevens uit andere partities. 

     ```sql
            Create table ControlTableForTemplate
            (
            PartitionID int,
            SourceTableName  varchar(255),
            FilterQuery varchar(255)
            );

            INSERT INTO ControlTableForTemplate
            (PartitionID, SourceTableName, FilterQuery)
            VALUES
            (1, 'datasource_table','select * from datasource_table where LastModifytime >= ''2015-01-01 00:00:00'' and LastModifytime <= ''2015-12-31 23:59:59.999'''),
            (2, 'datasource_table','select * from datasource_table where LastModifytime >= ''2016-01-01 00:00:00'' and LastModifytime <= ''2016-12-31 23:59:59.999'''),
            (3, 'datasource_table','select * from datasource_table where LastModifytime >= ''2017-01-01 00:00:00'' and LastModifytime <= ''2017-12-31 23:59:59.999'''),
            (4, 'project_table','select * from project_table where ID >= 0 and ID < 1000'),
            (5, 'project_table','select * from project_table where ID >= 1000 and ID < 2000');
    ```

2. Ga naar de sjabloon **bulksgewijs kopiëren van de Database**, en maak een **nieuwe verbinding** aan uw tabel met externe controle.  Deze verbinding maakt verbinding met de database waar u de controle-tabel hebt gemaakt in stap #1.

    ![Maak een nieuwe verbinding met de besturingselement-tabel](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Maak een **nieuwe verbinding** met uw brondatabase waarin de gegevens van kopiëren.

     ![Maak een nieuwe verbinding met de brondatabase](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Maak een **nieuwe verbinding** naar de doelgegevensopslagplaats waarin de gegevens te kopiëren.

    ![Maak een nieuwe verbinding met het doelarchief](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Klik op **Gebruik deze sjabloon**.

    ![Deze sjabloon gebruiken](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. U ziet de pijplijn die beschikbaar zijn in het deelvenster, zoals wordt weergegeven in het volgende voorbeeld:

    ![De pijplijn controleren](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Klik op **Debug**, de invoerparameters en klik vervolgens op **voltooien**

    ![Klik op de foutopsporing](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Zoals wordt weergegeven in het volgende voorbeeld ziet u het resultaat in het deelvenster beschikbaar:

    ![Bekijk het resultaat](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (Optioneel) Als u SQL Data Warehouse als bestemming selecteert, moet u ook voor het invoeren van de verbinding van een Azure blob-opslag als een fasering, die nodig is voor SQL Data Warehouse Polybase.  Zorg ervoor dat de container in blob-opslag is al gemaakt.  
    
    ![Polybase-instelling](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Data Factory](introduction.md)