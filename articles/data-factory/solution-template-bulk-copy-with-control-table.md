---
title: Bulksgewijs kopiëren uit een database met behulp van een besturingselement-tabel met Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van een oplossingssjabloon bulksgewijs om gegevens te kopiëren uit een database met behulp van een besturingselement voor externe tabel voor het opslaan van een Partitielijst met tabellen in gegevensbronnen met behulp van Azure Data Factory.
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
ms.openlocfilehash: c4224693642e8c9f76deedc0c8ad8586e122cc23
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57530566"
---
# <a name="bulk-copy-from-a-database-with-a-control-table"></a>Bulksgewijs kopiëren van een database met een besturingselement-tabel

Als u wilt gegevens kopiëren van een datawarehouse in de Oracle-Server, Netezza, Teradata of SQL Server naar Azure SQL Data Warehouse, moet u enorme hoeveelheden gegevens uit meerdere tabellen te laden. De gegevens hebben meestal moet worden gepartitioneerd in elke tabel zodat u rijen met meerdere threads parallel uit één tabel laden kunt. Dit artikel wordt een sjabloon die u wilt gebruiken in deze scenario's beschreven.

 >! Houd er rekening mee als u gegevens kopiëren van een klein aantal tabellen met een relatief klein gegevensvolume naar SQL Data Warehouse wilt, is het efficiënter gebruik van de [hulpprogramma Azure Data Factory Copy Data](copy-data-tool.md). De sjabloon die wordt beschreven in dit artikel is meer dan u nodig hebt voor het betreffende scenario.

## <a name="about-this-solution-template"></a>Over deze oplossingssjabloon

Deze sjabloon wordt een lijst met partities van de bron-database te kopiëren uit een externe controle-tabel opgehaald. Vervolgens doorloopt over elke partitie in de brondatabase en de gegevens worden gekopieerd naar de bestemming.

De sjabloon bevat drie activiteiten:
- **Lookup** wordt de lijst of databasepartities uit een externe controle-tabel opgehaald.
- **ForEach** de Partitielijst ophalen uit de Lookup-activiteit en elke partitie aan de Copy-activiteit doorloopt.
- **Kopie** elke partitie worden gekopieerd van het archief van de database bron naar het doelarchief.

De sjabloon definieert vijf parameters:
- *Control_Table_Name* is uw externe controle-tabel, waarin de Partitielijst met voor de brondatabase worden opgeslagen.
- *Control_Table_Schema_PartitionID* is de naam van de naam van de kolom in de externe controle-tabel waarin elke partitie-ID. Zorg ervoor dat de partitie-ID uniek voor elke partitie in de brondatabase is.
- *Control_Table_Schema_SourceTableName* is uw externe controle-tabel waarin de naam van elke tabel vanuit de brondatabase.
- *Control_Table_Schema_FilterQuery* is de naam van de kolom in een besturingselement voor externe tabel waarin de filterquery voor de gegevens van elke partitie in de brondatabase. Bijvoorbeeld, als u de gegevens gepartitioneerd per jaar, de query die opgeslagen in elke rij mogelijk die vergelijkbaar is met ' selecteren * van datasource waar LastModifytime > = '' 2015-01-01 00:00:00 '' en LastModifytime < = '' 31-12-2015 23:59:59.999'' '.
- *Data_Destination_Folder_Path* is het pad waar de gegevens in uw archief van de bestemming worden gekopieerd. Deze parameter is alleen zichtbaar als de bestemming die u kiest bestandsgebaseerde opslag is. Als u ervoor SQL Data Warehouse als het doelarchief kiest, is deze parameter niet vereist. Maar de namen van tabellen en het schema in SQL Data Warehouse moeten gelijk zijn aan de blobs in de brondatabase.

## <a name="how-to-use-this-solution-template"></a>Het gebruik van deze oplossingssjabloon

1. Een besturingselement-tabel maken in SQL Server of Azure SQL Database voor het opslaan van de bron-partitie databaselijst voor bulksgewijs kopiëren. In het volgende voorbeeld zijn er vijf partities in de brondatabase. Er zijn drie partities voor de *datasource_table*, en twee zijn voor de *project_table*. De kolom *LastModifytime* wordt gebruikt voor het partitioneren van de gegevens in de tabel *datasource_table* vanuit de brondatabase. De query die wordt gebruikt voor het lezen van de eerste partitie is ' Selecteer * uit datasource_table waar LastModifytime > = '' 2015-01-01 00:00:00 '' en LastModifytime < = '' 31-12-2015 23:59:59.999'' '. U kunt een vergelijkbare query gegevens lezen uit andere partities.

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

2. Ga naar de **bulksgewijs kopiëren van de Database** sjabloon. Maak een **nieuw** verbinding met de externe controle-tabel die u in stap 1 hebt gemaakt.

    ![Maak een nieuwe verbinding met de besturingselement-tabel](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable2.png)

3. Maak een **nieuw** verbinding met de brondatabase die u bij het kopiëren van gegevens uit.

     ![Maak een nieuwe verbinding met de brondatabase](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable3.png)
    
4. Maak een **nieuw** verbinding met de doelgegevens opslaan dat u de gegevens wilt kopiëren.

    ![Maak een nieuwe verbinding met het doelarchief](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable4.png)

5. Selecteer **Gebruik deze sjabloon**.

    ![Deze sjabloon gebruiken](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable5.png)
    
6. U ziet de pijplijn, zoals wordt weergegeven in het volgende voorbeeld:

    ![De pijplijn controleren](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable6.png)

7. Selecteer **Debug**, voer de **Parameters**, en selecteer vervolgens **voltooien**.

    ![Klik op ** foutopsporing **](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable7.png)

8. Ziet u de resultaten vergelijkbaar met het volgende voorbeeld zijn:

    ![Bekijk het resultaat](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable8.png)

9. (Optioneel) Als u SQL Data Warehouse als de bestemming kiest, moet u een verbinding naar Azure Blob-opslag voor fasering, zoals vereist door SQL Data Warehouse Polybase invoeren. Zorg ervoor dat de container in Blob-opslag al zijn gemaakt.
    
    ![Polybase-instelling](media/solution-template-bulk-copy-with-control-table/BulkCopyfromDB_with_ControlTable9.png)
       
## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Data Factory](introduction.md)
