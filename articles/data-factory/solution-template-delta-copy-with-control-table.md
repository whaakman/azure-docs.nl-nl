---
title: Delta-kopie van de database met controle-tabel met Azure Data Factory | Microsoft Docs
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
ms.openlocfilehash: 23e1255013cd5e52166fe0e59a8931dd9ecd81a0
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55967229"
---
# <a name="delta-copy-from-database-with-control-table"></a>Delta-kopie van de database met controle-tabel

Als u wilt incrementeel wijzigingen (nieuwe of bijgewerkte rijen) alleen uit een tabel in een database naar Azure met een besturingselement voor externe tabel opslaan van de hoge grenswaarde.  De huidige sjabloon is ontworpen voor deze aanvraag. 

Deze sjabloon moet dat het schema van de brondatabase moet met timestamp-kolom of oplopende sleutel voor het identificeren van de nieuwe of bijgewerkte rijen.

Als u timestamp-kolom in de brondatabase hebt identificeren van de nieuwe of bijgewerkte rijen, maar niet wilt maken van een besturingselement voor externe tabel om in te schakelen van delta-kopie, kunt u het hulpprogramma Copy Data gebruiken om op te halen van een pijplijn, die een trigger geplande tijd als een variabele gebruikt om te lezen de nieuwe rijen uit de brondatabase.

## <a name="about-this-solution-template"></a>Over deze oplossingssjabloon

Deze sjabloon worden de oude grenswaarde altijd eerst opgehaald en vergelijkt deze met de huidige grenswaarde. Hierna kopieert het de wijzigingen alleen vanuit de brondatabase op basis van de vergelijking tussen de 2 grenswaarden.  Als u klaar bent, wordt de nieuwe hoge watermerk-waarde een tabel met externe controle voor delta-gegevens laden van de volgende keer opgeslagen.

De sjabloon bevat vier activiteiten:
-   Een **Lookup** activiteit voor het ophalen van de oude hoge watermerk-waarde opgeslagen in een besturingselement voor externe tabel.
-   Een **Lookup** activiteit voor het ophalen van de huidige hoge watermerk-waarde uit de brondatabase.
-   Een **kopie** activiteit voor het kopiëren van de wijzigingen alleen vanuit de brondatabase naar de doel-store. De query gebruikt voor het identificeren van de wijzigingen van de brondatabase in de copy-activiteit is vergelijkbaar als ' selecteren * van Data_Source_Table waar TIMESTAMP_Column > 'laatste hoog-watermerk' en TIMESTAMP_Column < = 'huidige hoge-watermerk' '.
-   Een **SqlServerStoredProcedure** activiteit voor het schrijven van de huidige hoge watermerk-waarde een tabel met externe controle voor deltakopieerbewerkingen volgende keer.

De sjabloon definieert vijf parameters:
-   De parameter *Data_Source_Table_Name* is de naam van de tabel uit de brondatabase waar u gegevens wilt laden uit.
-   De parameter *Data_Source_WaterMarkColumn* is de naam van de kolom in de brontabel die kan worden gebruikt voor het identificeren van de nieuwe of bijgewerkte rijen. Het type van deze kolom kan normaal gesproken worden datetime of INT enzovoort.
-   De parameter *Data_Destination_Folder_Path* of *Data_Destination_Table_Name* is de plaats waar de gegevens in uw archief van de bestemming worden gekopieerd.
-   De parameter *Control_Table_Table_Name* is de naam van besturingselement voor externe tabel voor het opslaan van de hoge grenswaarde.
-   De parameter *Control_Table_Column_Name* is de naam van de kolom in de tabel extern beheer voor het opslaan van de hoge grenswaarde.

## <a name="how-to-use-this-solution-template"></a>Het gebruik van deze oplossingssjabloon

1. Verken de brontabel die u wilt laden en de hoge watermerk kolom die kan worden gebruikt voor het segmenteren van de nieuwe of bijgewerkte rijen definiëren. Het type van deze kolom kan normaal gesproken worden datum-/ of de INT enz. en de bijbehorende gegevens bijhouden toe wanneer de nieuwe rijen worden toegevoegd.  In de brontabel voorbeeld (tabelnaam: data_source_table), gebruiken we kolom *LastModifytime* als de hoge grenswaardekolom.

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
    
2. Een besturingselement-tabel maken in een SQL-server of SQL Azure voor het opslaan van de hoge grenswaarde voor het laden van deltagegevens. In het onderstaande voorbeeld ziet u de naam van besturingselement tabel is *watermarktable*. Binnen deze de naam van de kolom voor het opslaan van de hoge grenswaarde is *WatermarkValue* en het type *datum-/*.

    ```sql
            create table watermarktable
            (
            WatermarkValue datetime,
            );
            INSERT INTO watermarktable
            VALUES ('1/1/2010 12:00:00 AM')
    ```
    
3. Een opgeslagen procedure maken in dezelfde SQL-server of SQL Azure gebruikt om controle-tabel te maken. De opgeslagen procedure wordt gebruikt voor het schrijven van de nieuwe hoge grenswaarde in de tabel met externe controle voor het laden van de volgende keer dat delta-gegevens.

    ```sql
            CREATE PROCEDURE update_watermark @LastModifiedtime datetime
            AS

            BEGIN

                UPDATE watermarktable
                SET [WatermarkValue] = @LastModifiedtime 

            END
    ```
    
4. Ga naar de sjabloon **Delta-kopie van de Database**, en maak een **nieuwe verbinding** met uw brondatabase waarin de gegevens van kopiëren.

    ![Maak een nieuwe verbinding met de brontabel](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable4.png)

5. Maak een **nieuwe verbinding** naar de doelgegevensopslagplaats waarin de gegevens te kopiëren.

    ![Maak een nieuwe verbinding met de doeltabel](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable5.png)

6. Maak een **nieuwe verbinding** naar uw externe controle-tabel en de opgeslagen procedure.  Deze verbinding maakt met de database waarin u had de besturingselement-tabel gemaakt en opgeslagen procedure in stap #2 en 3 #.

    ![Maak een nieuwe verbinding met de gegevensopslag van het besturingselement-tabel](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable6.png)

7. Klik op **Gebruik deze sjabloon**.

     ![Deze sjabloon gebruiken](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable7.png)
    
8. U ziet de pijplijn die beschikbaar zijn in het deelvenster, zoals wordt weergegeven in het volgende voorbeeld:

     ![Controleer pijplijn](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable8.png)

9. Klik op Stored Procedure-activiteit en selecteer **opgeslagen procedurenaam**, klikt u op **importparameter** en klikt u op **dynamische inhoud toevoegen**.  

     ![Set Stored Procedure-activiteit](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable9.png) 

10. De inhoud schrijven **@{activity('LookupCurrentWaterMark').output.firstRow.NewWatermarkValue}** en klikt u op **voltooien**.  

     ![Schrijven van de inhoud voor de parameter voor de opgeslagen Procedure](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable10.png)      
     
11. Klik op **Debug**, de invoerparameters en klikt u op **voltooien**.

    ![Klik op de foutopsporing](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

12. Zoals wordt weergegeven in het volgende voorbeeld ziet u het resultaat in het deelvenster beschikbaar:

    ![Bekijk het resultaat](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable12.png)

13. U kunt nieuwe rijen in de brontabel maken.  De voorbeeld-sql te maken van nieuwe rijen kan worden als volgt:

    ```sql
            INSERT INTO data_source_table
            VALUES (10, 'newdata','9/10/2017 2:23:00 AM')

            INSERT INTO data_source_table
            VALUES (11, 'newdata','9/11/2017 9:01:00 AM')
    ```
13. Voer de pijplijn opnieuw uit door te klikken op **Debug**, de invoerparameters en klikt u op **voltooien**.

    ![Klik op de foutopsporing](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable11.png)

14. U ziet alleen nieuwe rijen zijn gekopieerd naar de bestemming.

15. (Optioneel) Als u SQL Data Warehouse als bestemming selecteert, moet u ook voor het invoeren van de verbinding van een Azure blob-opslag als een fasering, die nodig is voor SQL Data Warehouse Polybase.  Zorg ervoor dat de container in blob-opslag is al gemaakt.  
    
    ![Polybase configureren](media/solution-template-delta-copy-with-control-table/DeltaCopyfromDB_with_ControlTable15.png)
    
## <a name="next-steps"></a>Volgende stappen

- [Inleiding tot Azure Data Factory](introduction.md)