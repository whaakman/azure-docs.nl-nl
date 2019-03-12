---
title: Delta-kopie van een database met behulp van een besturingselement-tabel met Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van een oplossingssjabloon voor nieuwe of bijgewerkte rijen incrementeel kopiëren alleen uit een database met Azure Data Factory.
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
ms.date: 12/24/2018
ms.openlocfilehash: c32592ce539eeb2dec71792e4a6eb31e7d904eff
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57771154"
---
# <a name="delta-copy-from-a-database-with-a-control-table"></a>Delta-kopie van een database met een besturingselement-tabel

Dit artikel beschrijft een sjabloon die beschikbaar is voor het incrementeel laden van nieuwe of bijgewerkte rijen uit een database naar Azure met behulp van een besturingselement voor externe tabel waarin een hoge grenswaarde.

Deze sjabloon is vereist dat het schema van de brondatabase een timestamp-kolom of oplopende sleutel voor het identificeren van nieuwe of bijgewerkte rijen bevat.

>[!NOTE]
> Als u hebt een timestamp-kolom in de brondatabase nieuwe of bijgewerkte rijen te vinden, maar u niet wilt dat een externe controle-tabel moet worden gebruikt voor de delta-kopie maken, u kunt in plaats daarvan de [hulpprogramma Azure Data Factory Copy Data](copy-data-tool.md) om op te halen van een pijplijn. Dit hulpprogramma maakt gebruik van een trigger geplande tijd als een variabele lezen van nieuwe rijen uit de brondatabase.

## <a name="about-this-solution-template"></a>Over deze oplossingssjabloon

Deze sjabloon eerst worden opgehaald van de oude grenswaarde en vergelijkt deze met de huidige grenswaarde. Daarna worden alleen de wijzigingen gekopieerd vanuit de brondatabase op basis van een vergelijking tussen de twee bovengrenswaarden. Ten slotte wordt de nieuwe hoge watermerk-waarde een tabel met externe controle voor delta-gegevens laden van de volgende keer opgeslagen.

De sjabloon bevat vier activiteiten:
- **Lookup** haalt de oude hoge watermerk-waarde die is opgeslagen in een besturingselement voor externe tabel.
- Een andere **Lookup** activiteit haalt de huidige hoge watermerk-waarde vanuit de brondatabase.
- **Kopie** kopieert alleen de wijzigingen van de brondatabase naar het doelarchief. De query waarmee de wijzigingen in de brondatabase is vergelijkbaar met ' selecteren * van Data_Source_Table waar TIMESTAMP_Column > 'laatste hoog-watermerk' en TIMESTAMP_Column < = 'huidige hoge-watermerk' '.
- **SqlServerStoredProcedure** schrijft volgende keer dat de huidige hoge watermerk-waarde een tabel met externe controle voor delta-kopie.

De sjabloon definieert vijf parameters:
- *Data_Source_Table_Name* is de tabel in de brondatabase die u wilt laden van gegevens uit.
- *Data_Source_WaterMarkColumn* is de naam van de kolom in de brontabel die is gebruikt voor het nieuwe identificeren of rijen bijgewerkt. Het type van deze kolom is meestal *datum-/*, *INT*, of vergelijkbare.
- *Data_Destination_Folder_Path* of *Data_Destination_Table_Name* is de plaats waar de gegevens in uw doelarchief om te worden gekopieerd.
- *Control_Table_Table_Name* is de externe controle-tabel waarin de hoge grenswaarde.
- *Control_Table_Column_Name* is de kolom in de externe controle-tabel die de hoge watermerk-waarde wordt opgeslagen.

## <a name="how-to-use-this-solution-template"></a>Het gebruik van deze oplossingssjabloon

1. Verken de bron-tabel u die wilt laden en de hoge grenswaardekolom die kan worden gebruikt voor het identificeren van nieuwe of bijgewerkte rijen definiëren. Het type van deze kolom mogelijk *datum-/*, *INT*, of vergelijkbare. De waarde van deze kolom wordt verhoogd als nieuwe rijen worden toegevoegd. In het volgende voorbeeld brontabel (data_source_table), gebruiken we de *LastModifytime* kolom als de hoge grenswaardekolom.

    ```sql
            PersonID    Name    LastModifytime
            1   aaaa    2017-09-01 00:56:00.000
            2   bbbb    2017-09-02 05:23:00.000
            3   cccc    2017-09-03 02:36:00.000
            4   dddd    2017-09-04 03:21:00.000
            5   eeee    2017-09-05 08:06:00.000
            6   fffffff 2017-09-06 02:23:00.000
            7   gggg    2017-09-07 09:01:00.000
            8   hhhh    2017-09-08 09:01:00.000
            9   iiiiiiiii   2017-09-09 09:01:00.000
    ```
    
2. Een besturingselement-tabel in SQL Server of Azure SQL Database voor het opslaan van de hoge grenswaarde voor het laden van deltagegevens maken. In het volgende voorbeeld wordt de naam van de tabel van het besturingselement is *watermarktable*. In deze tabel *WatermarkValue* is de kolom waarin de hoge grenswaarde en het type *datum-/*.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Een opgeslagen procedure maken in de dezelfde SQL Server of Azure SQL Database-instantie die u hebt gebruikt om de controle-tabel te maken. De opgeslagen procedure wordt gebruikt om de nieuwe hoge grenswaarde schrijven naar de externe controle-tabel voor het laden van de volgende keer dat delta-gegevens.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Ga naar de **Delta-kopie van de Database** sjabloon. Maak een **nieuw** verbinding met de brondatabase die u wilt kopiëren van gegevens uit.

    ![Maak een nieuwe verbinding met de brontabel](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Maak een **nieuw** verbinding met het doelarchief voor gegevens die u wilt de gegevens te kopiëren.

    ![Maak een nieuwe verbinding met de doeltabel](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Maak een **nieuw** verbinding met de externe controle-tabel en de opgeslagen procedure die u hebt gemaakt in stap 2 en 3.

    ![Maak een nieuwe verbinding met de gegevensopslag van het besturingselement-tabel](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Selecteer **Gebruik deze sjabloon**.

     ![Deze sjabloon gebruiken](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. U ziet de beschikbare pijplijn, zoals wordt weergegeven in het volgende voorbeeld:

     ![De pijplijn controleren](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Selecteer **opgeslagen Procedure**. Voor **opgeslagen procedurenaam**, kiest u **[update_watermark]**. Selecteer **importparameter**, en selecteer vervolgens **dynamische inhoud toevoegen**.  

     ![Stel de opgeslagen procedure-activiteit](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. De inhoud schrijven  **\@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}**, en selecteer vervolgens **voltooien**.  

     ![Schrijven van de inhoud voor de parameters van de opgeslagen procedure](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Selecteer **Debug**, voer de **Parameters**, en selecteer vervolgens **voltooien**.

    ![Selecteer ** Foutopsporing **](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Het resultaat is vergelijkbaar met het volgende voorbeeld worden weergegeven:

    ![Bekijk het resultaat](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. U kunt nieuwe rijen in de brontabel maken. Hier volgt een voorbeeld van SQL-taal te maken van nieuwe rijen:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
14. Als u wilt de pijplijn opnieuw uitvoeren, selecteert u **Debug**, voer de **Parameters**, en selecteer vervolgens **voltooien**.

    ![Selecteer ** Foutopsporing **](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

    U ziet dat alleen nieuwe rijen zijn gekopieerd naar de bestemming.

15. (Optioneel:) Als u SQL Data Warehouse hebt geselecteerd als de bestemming, moet u ook een verbinding met Azure Blob-opslag opgeven voor fasering, die nodig is voor SQL Data Warehouse Polybase. Zorg ervoor dat de container al is gemaakt in Blob-opslag.
    
    ![Polybase configureren](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Volgende stappen

- [Bulksgewijs kopiëren uit een database met behulp van een besturingselement-tabel met Azure Data Factory](solution-template-bulk-copy-with-control-table.md)
- [Bestanden kopiëren van meerdere containers met Azure Data Factory](solution-template-copy-files-multiple-containers.md)
