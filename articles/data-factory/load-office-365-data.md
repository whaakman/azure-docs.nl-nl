---
title: Gegevens laden van Office 365 met behulp van Azure Data Factory | Microsoft Docs
description: Azure Data Factory gebruiken om gegevens te kopiëren van Office 365
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/08/2018
ms.author: jingwang
ms.openlocfilehash: e2b441e0266ac397226b783c66df4ca54e0b3f71
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48870159"
---
# <a name="load-data-from-office-365-by-using-azure-data-factory"></a>Gegevens laden van Office 365 met behulp van Azure Data Factory

Dit artikel laat u het gebruik van de Data Factory _gegevens uit Office 365 laden in Azure Blob-opslag_. U kunt dezelfde stappen om gegevens te kopiëren naar Azure Data Lake Gen1 of Gen2. Raadpleeg [Office 365-connector artikel](connector-office-365.md) voor het kopiëren van gegevens in het algemeen van Office 365.

Bekijk de volgende video voor een 9 minuten durende inleiding en demonstratie over Data Factory verbinding te maken met Office 365-gegevens:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-cloud-scale-analytics-of-Office-365-data-with-Azure-Data-Factory/player]

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

1. Selecteer in het menu links **nieuw** > **gegevens en analyses** > **Data Factory**: 
   
   ![Een nieuwe gegevensfactory maken](./media/load-office-365-data/new-azure-data-factory-menu.png)
2. In de **nieuwe data factory** pagina, geef waarden op voor de velden die worden weergegeven in de volgende afbeelding:
      
   ![De pagina Nieuwe data factory](./media/load-office-365-data/new-azure-data-factory.png)
 
    * **Naam**: Voer een unieke naam op voor uw Azure-gegevensfactory. Als u de foutmelding ' naam Data factory \"LoadFromOffice365Demo\" is niet beschikbaar, "Voer een andere naam voor de data factory. Bijvoorbeeld, kunt u de naam van de  _**uwnaam**_**LoadFromOffice365Demo**. Probeer het opnieuw maken van de data factory. Raadpleeg het onderwerp [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
    * **Abonnement**: Selecteer uw Azure-abonnement waarin u wilt maken van de data factory. 
    * **Resourcegroep**: Selecteer een bestaande resourcegroep in de vervolgkeuzelijst of Selecteer de **nieuw** optie en voer de naam van een resourcegroep. Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
    * **Versie**: Selecteer **V2**.
    * **Locatie**: Selecteer de locatie voor de data factory. In de vervolgkeuzelijst worden alleen ondersteunde locaties weergegeven. De gegevensarchieven die worden gebruikt door data factory, kunnen zich in andere locaties of regio's. Deze gegevensarchieven bevatten Azure Data Lake Store, Azure Storage en Azure SQL Database.

3. Selecteer **Maken**.
4. Na het aanmaken is voltooid, gaat u naar uw data factory. U ziet de **Data Factory** startpagina zoals wordt weergegeven in de volgende afbeelding:
   
   ![Startpagina van de gegevensfactory](./media/load-office-365-data/data-factory-home-page.png)

5. Selecteer de **Author & Monitor** tegel om te starten van de toepassing voor gegevensintegratie in een afzonderlijk tabblad.

## <a name="create-a-pipeline"></a>Een pijplijn maken

1. Selecteer op de pagina 'Aan de slag' **pijplijn maken**.
 
    ![Pijplijn maken](./media/load-office-365-data/create-pipeline-entry.png)

2. In de **tabblad Algemeen** voor de pijplijn, voert u 'CopyPipeline' voor **naam** van de pijplijn.

3. In de activiteiten-werkset > verplaatsen en transformeren van categorie > slepen en neerzetten het **Kopieeractiviteit** uit de werkset naar het ontwerpoppervlak voor pijplijnen. Geef 'CopyFromOffice365ToBlob' als naam van de activiteit.

### <a name="configure-source"></a>Bron configureren

1. Ga naar de pijplijn > **tabblad bron**, klikt u op **+ nieuw** om een brongegevensset te maken. 

2. Selecteer in het venster nieuwe gegevensset **Office 365**, en selecteer vervolgens **voltooien**.

    ![Nieuwe Office 365-gegevensset](./media/load-office-365-data/new-office-365-dataset.png)
 
3. U ziet een nieuw tabblad geopend voor Office 365-gegevensset. Op de **tabblad Algemeen** Voer 'SourceOffice365Dataset' voor de naam van aan de onderkant van het venster Eigenschappen.

    ![Algemene configuratie Office 365-gegevensset](./media/load-office-365-data/config-office-365-dataset-general.png)
 
4. Ga naar de **tabblad verbinding** van het venster Eigenschappen. Klik naast het tekstvak gekoppelde service op **+ nieuw**.
 
    ![Configuratie van Office 365-gegevensset-verbinding](./media/load-office-365-data/config-office-365-dataset-connection.png)

5. In het venster nieuwe gekoppelde Service 'Office365LinkedService' als naam invoert, voert u de service-principal-ID en sleutel van service-principal en selecteer opslaan om de gekoppelde service te implementeren.

    ![Nieuwe Office 365 gekoppelde service](./media/load-office-365-data/new-office-365-linked-service.png)
 
6. Nadat de gekoppelde service is gemaakt, gaat u terug naar de gegevenssetinstellingen. Naast 'Tabel', kies de pijl-omlaag om uit te breiden de lijst met beschikbare gegevenssets voor Office 365 en 'BasicDataSet_v0. Contact_v0 ' uit de vervolgkeuzelijst:

    ![Configuratie van Office 365-gegevensset-tabel](./media/load-office-365-data/config-office-365-dataset-table.png)
 
7. Ga naar de **tabblad Schema** van het venster Eigenschappen en selecteer **Schema importeren**.  U ziet dat de schema- en voorbeeld van waarden voor contact op met de gegevensset wordt weergegeven.

    ![Configuratie van Office 365-gegevensset-schema](./media/load-office-365-data/config-office-365-dataset-schema.png)

8. Ga nu terug naar de pijplijn > tabblad bron, controleert u of SourceBlobDataset is geselecteerd.
 
### <a name="configure-sink"></a>Sink configureren

1. Ga naar de pijplijn > **Sink tabblad**, en selecteer **+ nieuw** om een sinkgegevensset te maken.
 
2. U ziet dat alleen de ondersteunde bestemming bij het kopiëren van Office 365 zijn geselecteerd in het venster nieuwe gegevensset. Selecteer **Azure Blob Storage**, en selecteer vervolgens **voltooien**.  In deze zelfstudie kopieert u Office 365-gegevens naar een Azure Blob-opslag.

    ![Nieuwe Blob-gegevensset](./media/load-office-365-data/new-blob-dataset.png)

4. Op de **tabblad Algemeen** Typ 'OutputBlobDataset' in naam van het venster Eigenschappen.

5. Ga naar de **tabblad verbinding** van het venster Eigenschappen. Selecteer naast het tekstvak gekoppelde service **+ nieuw**.

    ![Config Blob-gegevensset-verbinding](./media/load-office-365-data/config-blob-dataset-connection.png) 

6. In het venster nieuwe gekoppelde Service 'AzureStorageLinkedService' als naam invoeren, ' Service-Principal selecteren ' in de vervolgkeuzelijst van de ondersteunde verificatiemethoden, vult u het Service-eindpunt, Tenant-Service-principal-ID, en Service-principal sleutel, en vervolgens selecteert u opslaan in Implementeer de gekoppelde service.  Raadpleeg [hier](connector-azure-blob-storage.md#service-principal-authentication) voor informatie over het instellen van verificatie van service-principal voor Azure Blob Storage.

    ![Nieuwe gekoppelde blobservice](./media/load-office-365-data/new-blob-linked-service.png)

7. Nadat de gekoppelde service is gemaakt, gaat u terug naar de gegevenssetinstellingen. Selecteer naast bestandspad, **Bladeren** de uitvoermap waar de Office 365-gegevens worden geëxtraheerd om te kiezen.  Kies onder 'instellingen bestandsindelingen", naast de bestandsindeling,"**JSON-indeling**', en kies naast bestandspatroon, "**Set van objecten**'.

    ![Configuratieblob gegevensset pad en de indeling](./media/load-office-365-data/config-blob-dataset-path-and-format.png) 

8. Ga terug naar de pijplijn > tabblad Sink, controleert u of OutputBlobDataset is geselecteerd.

## <a name="validate-the-pipeline"></a>De pijplijn valideren

Selecteer in de werkbalk **Valideren** om de pijplijn te valideren.

U ziet ook de JSON-code die is gekoppeld aan de pijplijn door te klikken op Code in de rechterbovenhoek.

## <a name="publish-the-pipeline"></a>De pijplijn publiceren

Selecteer in de bovenste werkbalk **Alles publiceren**. Met deze actie publiceert u entiteiten (gegevenssets en pijplijnen) die u hebt gemaakt met Data Factory.

![Wijzigingen publiceren](./media/load-office-365-data/publish-changes.png) 

## <a name="trigger-the-pipeline-manually"></a>De pijplijn handmatig activeren

Selecteer op de werkbalk de optie **Activeren** en selecteer vervolgens **Nu activeren**. Selecteer op de pagina Pijplijnuitvoering **voltooien**. 

## <a name="monitor-the-pipeline"></a>De pijplijn bewaken

Ga naar de **controletabblad** aan de linkerkant. U ziet een pijplijn die wordt geactiveerd door een handmatige trigger. U kunt koppelingen in de **acties-kolom** om details van activiteit weer te geven en de pijplijn opnieuw uitvoeren.

![De pijplijn bewaken](./media/load-office-365-data/pipeline-monitoring.png) 

Uitvoeringen van activiteit die is gekoppeld aan de pijplijnuitvoering, selecteer de **uitvoeringen van activiteit weergeven** koppeling in de acties-kolom. Omdat er in dit voorbeeld slechts één activiteit in de pijplijn is, ziet u slechts één vermelding in de lijst. Voor meer informatie over de kopieerbewerking, selecteert u de **Details-koppeling (pictogram van een bril)** in de acties-kolom.

![Activiteit controleren](./media/load-office-365-data/activity-monitoring.png) 

Als dit de eerste keer dat u gegevens voor deze context aanvraagt (een combinatie van welke gegevens worden tabel toegang, welke bestemming-account is de gegevens worden geladen in en welke gebruikers-id is, zodat de gegevens toegangsaanvraag), ziet u de copy-activiteit Als de status '**Bezig**', en alleen wanneer u klikt op in de koppeling 'Details' onder acties wordt de status wordt weergegeven als '**RequesetingConsent**'.  Een lid van de groep data access goedkeurder nodig voor het goedkeuren van de aanvraag in het Privileged Access Management voordat de gegevensextractie verder kunt gaan.

_Status als aanvragende toestemming:_
![uitvoeringsdetails van activiteit - toestemming van de aanvraag](./media/load-office-365-data/activity-details-request-consent.png) 

_Status als het ophalen van gegevens:_

![Details van de uitvoering van activiteit - ophalen gegevens](./media/load-office-365-data/activity-details-extract-data.png) 

Zodra de toestemming is opgegeven, gegevensextractie blijft en, na enige tijd, wordt de pijplijnuitvoering weergegeven als voltooid.

![De pijplijn bewaken - geslaagd](./media/load-office-365-data/pipeline-monitoring-succeeded.png) 

Nu gaat u naar de bestemming Azure Blob Storage en controleer of dat Office 365-gegevens in JSON-indeling is uitgepakt.

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over ondersteuning voor Azure SQL Data Warehouse: 

> [!div class="nextstepaction"]
>[Office 365-connector](connector-office-365.md)