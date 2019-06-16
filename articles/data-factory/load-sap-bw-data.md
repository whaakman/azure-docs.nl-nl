---
title: Gegevens uit SAP Business Warehouse laden met behulp van Azure Data Factory | Microsoft Docs
description: Azure Data Factory gebruiken om gegevens te kopiëren van SAP Business Warehouse (BW)
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: jingwang
ms.openlocfilehash: 4cd61db3ec0e8d88c9b1c6d6ba427b120b3f1af1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66152420"
---
# <a name="copy-data-from-sap-business-warehouse-by-using-azure-data-factory"></a>Gegevens kopiëren van SAP Business Warehouse met behulp van Azure Data Factory

Dit artikel leest hoe u gegevens van SAP Business Warehouse (BW) via Open Hub kopiëren naar Azure Data Lake Storage Gen2 met Azure Data Factory. U kunt een soortgelijk proces gebruiken om gegevens te kopiëren naar andere [sinkgegevensopslag ondersteund](copy-activity-overview.md#supported-data-stores-and-formats).

> [!TIP]
> Raadpleeg voor algemene informatie over het kopiëren van gegevens uit de SAP BW, met inbegrip van integratie met SAP BW Open Hub en delta-extractie stroom, [gegevens kopiëren van SAP Business Warehouse via Open-Hub met behulp van Azure Data Factory](connector-sap-business-warehouse-open-hub.md).

## <a name="prerequisites"></a>Vereisten

- **Azure Data Factory**: Als u niet hebt, volgt u de stappen voor het [maken van een data factory](quickstart-create-data-factory-portal.md#create-a-data-factory).

- **SAP BW Open Hub bestemming (OHD) met het doeltype "Databasetabel"** : Een OHD maken of om te controleren of uw OHD correct is geconfigureerd voor integratie met Data Factory, raadpleegt u de [configuraties van SAP BW Open Hub bestemming](#sap-bw-open-hub-destination-configurations) sectie van dit artikel.

- **De SAP BW-gebruiker moet de volgende machtigingen**:

  - Autorisatie voor externe functieaanroepen (RFC) en SAP BW.
  - Machtigingen voor de activiteit "Uitvoeren" van de **S_SDSAUTH** autorisatie-object.

- **Een [zelf-hostende integratieruntime (IR)](concepts-integration-runtime.md#self-hosted-integration-runtime) met SAP .NET connector 3.0**. Volg deze instellingsstappen:

  1. Installeer en registreer de zelf-hostende integratieruntime, 3,13 of hoger. (Dit proces wordt verderop in dit artikel beschreven.)

  2. Download de [64-bits SAP-Connector voor Microsoft .NET 3.0](https://support.sap.com/en/product/connectors/msnet.html) van de SAP-website, en installeer deze op dezelfde computer als de zelf-hostende IR Tijdens de installatie, zorg ervoor dat u selecteert **assembly's installeren in GAC** in de **optionele installatiestappen uit** in het dialoogvenster, zoals in de volgende afbeelding wordt weergegeven:

     ![In het dialoogvenster voor SAP .NET Connector instellen](media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

## <a name="do-a-full-copy-from-sap-bw-open-hub"></a>Voer een volledige kopie van SAP BW Open Hub

In de Azure-portal, gaat u naar uw data factory. Selecteer **Author & Monitor** openen van de gebruikersinterface van Data Factory op een afzonderlijk tabblad.

1. Op de **aan de slag** weergeeft, schakelt **Copy Data** openen van het hulpprogramma Copy Data.

2. Op de **eigenschappen** pagina, Geef een **taaknaam**, en selecteer vervolgens **volgende**.

3. Op de **brongegevensarchief** weergeeft, schakelt **+ nieuwe verbinding maken**. Selecteer **SAP BW Open Hub** uit de galerie met connector en selecteer vervolgens **doorgaan**. Als u wilt filteren de connectors, kunt u typen **SAP** in het zoekvak in.

4. Op de **opgeven SAP BW Open hubverbinding** pagina, als volgt te werk om een nieuwe verbinding te maken.

   ![Pagina met SAP BW Open Hub-gekoppelde service maken](media/load-sap-bw-data/create-sap-bw-open-hub-linked-service.png)

   1. Uit de **verbinding maken via integratieruntime** , selecteert u een bestaande zelf-hostende IR Of kies een maken als u nog geen abonnement hebt.

      Voor het maken van een nieuwe zelf-hostende IR selecteert **+ nieuw**, en selecteer vervolgens **zelf-hostend**. Voer een **naam**, en selecteer vervolgens **volgende**. Selecteer **snelle installatie** te installeren op de huidige computer, of gaat u als volgt de **handmatige installatie** stappen die worden geleverd.

      Zoals vermeld in [vereisten](#prerequisites), zorg ervoor dat u hebt de SAP-Connector voor Microsoft .NET 3.0 is geïnstalleerd op dezelfde computer waarop de zelf-hostende IR wordt uitgevoerd.

   2. Vul in de SAP BW **servernaam**, **systeemnummer**, **Client-ID,** **taal** (als andere dan **EN**) , **Gebruikersnaam**, en **wachtwoord**.

   3. Selecteer **verbinding testen** de instellingen controleren en selecteer vervolgens **voltooien**.

   4. Een nieuwe verbinding wordt gemaakt. Selecteer **Next**.

5. Op de **Open Hub bestemmingen Selecteer** pagina, Open Hub bestemmingen die beschikbaar in uw SAP BW zijn bladeren. Selecteer de OHD wilt kopiëren van gegevens uit en selecteer vervolgens **volgende**.

   ![SAP BW Open Hub doeltabel selecteren](media/load-sap-bw-data/select-sap-bw-open-hub-table.png)

6. Geef een filter, als u nodig hebt. Als uw OHD alleen gegevens van de uitvoering van een één-gegevensoverdracht-proces (DTP) met een enkele aanvraag-ID bevat, of u zeker weet dat uw DTP is voltooid en u wilt kopiëren van gegevens, schakel de **laatste aanvraag uitsluiten** selectievakje.

   Meer informatie over deze instellingen in de [configuraties van SAP BW Open Hub bestemming](#sap-bw-open-hub-destination-configurations) sectie van dit artikel. Selecteer **valideren** om te controleren welke gegevens worden geretourneerd. Selecteer vervolgens **Volgende**.

   ![Filter voor SAP BW Open Hub configureren](media/load-sap-bw-data/configure-sap-bw-open-hub-filter.png)

7. Op de **doelgegevensarchief** weergeeft, schakelt **+ nieuwe verbinding maken** > **Azure Data Lake Storage Gen2**  >   **Doorgaan met**.

8. Op de **opgeven Azure Data Lake Storage verbinding** pagina, als volgt te werk om een verbinding te maken.

   ![Maak een pagina van de gekoppelde service ADLS Gen2](media/load-sap-bw-data/create-adls-gen2-linked-service.png)

   1. Selecteer uw Data Lake-opslag kan Gen2-account van de **naam** vervolgkeuzelijst.
   2. Selecteer **voltooien** om de verbinding te maken. Selecteer vervolgens **Volgende**.

9. Op de **uitvoerbestand of uitvoermap kiezen** pagina **copyfromopenhub** als de naam van de uitvoer-map. Selecteer vervolgens **Volgende**.

   ![Pagina van de map uitvoer kiezen](media/load-sap-bw-data/choose-output-folder.png)

10. Op de **bestandsindeling instellen** weergeeft, schakelt **volgende** om de standaardinstellingen te gebruiken.

    ![Sink-indeling pagina opgeven](media/load-sap-bw-data/specify-sink-format.png)

11. Op de **instellingen** pagina uit, vouw **prestatie-instellingen**. Voer een waarde voor **graad van parallelle uitvoering kopie** zoals 5 van SAP BW parallel laden. Selecteer vervolgens **Volgende**.

    ![Instellingen configureren](media/load-sap-bw-data/configure-copy-settings.png)

12. Op de **samenvatting** pagina, Controleer de instellingen. Selecteer vervolgens **Volgende**.

13. Op de **implementatie** weergeeft, schakelt **Monitor** voor het bewaken van de pijplijn.

    ![De pagina Implementatie](media/load-sap-bw-data/deployment.png)

14. U ziet dat de **Monitor** tabblad aan de linkerkant van de pagina wordt automatisch geselecteerd. De **acties** kolom bevat koppelingen voor meer informatie de activiteit uitvoeren en de pijplijn opnieuw uitvoeren.

    ![Pijplijn controle weergeven](media/load-sap-bw-data/pipeline-monitoring.png)

15. Als u uitvoeringen van activiteit die gekoppeld aan de pijplijnuitvoering zijn, selecteer **uitvoeringen van activiteit weergeven** in de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. U wilt terugkeren naar de weergave van de pijplijn-runs, selecteer de **pijplijnen** koppelen aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen.

    ![Scherm activiteit controleren](media/load-sap-bw-data/activity-monitoring.png)

16. Voor het controleren van de uitvoering van details voor elke activiteit kopiëren, selecteert u de **Details** koppeling, dit een onderstaande pictogram van een bril is **acties** in de weergave controle van activiteiten. Beschikbare details omvatten het aantal gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd en de duur en configuraties die worden gebruikt.

    ![Details van activiteitsbewaking](media/load-sap-bw-data/activity-monitoring-details.png)

17. Om weer te geven de **maximale aanvraag-ID**, gaat u terug naar de controle van activiteiten weergeven en selecteer **uitvoer** onder **acties**.

    ![Uitvoer activiteitenscherm](media/load-sap-bw-data/activity-output.png)

    ![Activiteit uitvoer details weergeven](media/load-sap-bw-data/activity-output-details.png)

## <a name="do-an-incremental-copy-from-sap-bw-open-hub"></a>Een incrementele kopie uitvoeren vanuit SAP BW Open Hub

> [!TIP]
> Zie [SAP BW Open Hub connector delta-extractie stroom](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow) voor meer informatie over hoe de connector SAP BW Open Hub in Data Factory incrementele gegevens opgehaald uit SAP BW. In dit artikel kunt u inzicht in de connectorconfiguratie van de basic.

Nu gaat u verder naar de incrementele kopie van SAP BW Open Hub configureren.

Incrementele kopie maakt gebruik van een mechanisme 'high-watermerk' die gebaseerd op de **aanvraag-ID**. Deze ID wordt automatisch gegenereerd in SAP BW Open Hub doel door de DTP. Het volgende diagram toont deze werkstroom:

![Stroomdiagram voor incrementele kopie-werkstroom](media/load-sap-bw-data/incremental-copy-workflow.png)

Op de data factory **aan de slag** weergeeft, schakelt **pijplijn maken met sjabloon** de ingebouwde sjabloon wilt gebruiken.

1. Zoeken naar **SAP BW** om te zoeken en selecteer de **incrementeel kopiëren van SAP BW naar Azure Data Lake Storage Gen2** sjabloon. Deze sjabloon kopieert de gegevens in Azure Data Lake Storage Gen2. U kunt een vergelijkbare werkstroom gebruiken om te kopiëren naar de andere typen sink.

2. Selecteer op de hoofdpagina van de sjabloon, of de volgende drie verbindingen maken en selecteer vervolgens **Gebruik deze sjabloon** in de rechterbenedenhoek van het venster.

   - **Azure Blob-opslag**: In dit scenario gebruiken we Azure Blob-opslag voor het opslaan van de bovengrens, is de *max gekopieerd aanvraag-ID*.
   - **SAP BW Open Hub**: Dit is de bron om te kopiëren van gegevens uit. Raadpleeg de vorige volledige kopie-overzicht voor gedetailleerde configuratie.
   - **Azure Data Lake Storage Gen2**: Dit is de sink gegevens te kopiëren. Raadpleeg de vorige volledige kopie-overzicht voor gedetailleerde configuratie.

   ![Incrementele kopie van SAP BW-sjabloon](media/load-sap-bw-data/incremental-copy-from-sap-bw-template.png)

3. Deze sjabloon een pijplijn met de volgende drie activiteiten genereert en zorgt ervoor dat ze in een keten op geslaagde pogingen: *Lookup*, *gegevens kopiëren*, en *Web*.

   Ga naar de pijplijn **Parameters** tabblad. U ziet dat de configuraties die u moet opgeven.

   ![Incrementele kopie van SAP BW-configuratie](media/load-sap-bw-data/incremental-copy-from-sap-bw-pipeline-config.png)

   - **SAPOpenHubDestinationName**: Geef de naam van de tabel Open Hub om gegevens uit te kopiëren.

   - **ADLSGen2SinkPath**: Geef het pad van de Azure Data Lake Storage Gen2 bestemming voor het kopiëren van gegevens. Als het pad niet bestaat, maakt de Data Factory-kopieeractiviteit een pad tijdens de uitvoering.

   - **HighWatermarkBlobPath**: Geef het pad voor het opslaan van de hoge watermerk-waarde, zoals `container/path`.

   - **HighWatermarkBlobName**: Geef de naam van de blob voor het opslaan van de bovengrenswaarde zoals `requestIdCache.txt`. In de Blob-opslag, gaat u naar het bijbehorende pad van HighWatermarkBlobPath + HighWatermarkBlobName, zoals *container/path/requestIdCache.txt*. Maak een blob met inhoud 0.

      ![Blobinhoud](media/load-sap-bw-data/blob.png)

   - **LogicAppURL**: In deze sjabloon gebruiken we WebActivity om aan te roepen Azure Logic Apps om in te stellen de hoge grenswaarde in Blob-opslag. Of u kunt Azure SQL Database gebruiken om op te slaan. Een opgeslagen procedure-activiteit gebruiken om bij te werken van de waarde.

      Zoals in de volgende afbeelding wordt weergegeven, moet u eerst een logische app maken. Plak het **URL voor HTTP POST**.

      ![Logic App-configuratie](media/load-sap-bw-data/logic-app-config.png)

      1. Ga naar Azure Portal. Selecteer een nieuwe **Logic Apps** service. Selecteer **+ lege logische App** naar **ontwerper van logische Apps**.

      2. Maken van een trigger van **wanneer een HTTP-aanvraag wordt ontvangen**. Geef de hoofdtekst van de HTTP-aanvraag als volgt:

         ```json
         {
            "properties": {
               "sapOpenHubMaxRequestId": {
                  "type": "string"
               }
            },
            "type": "object"
         }
         ```

      3. Voeg een **blob maken** actie. Voor **mappad** en **blobnaam**, gebruikt u dezelfde waarden die u eerder hebt geconfigureerd in **HighWatermarkBlobPath** en **HighWatermarkBlobName**.

      4. Selecteer **Opslaan**. Kopieer vervolgens de waarde van **URL voor HTTP POST** te gebruiken in de Data Factory-pijplijn.

4. Nadat u de Data Factory-pijplijn-parameters opgeven, selecteert u **Debug** > **voltooien** om aan te roepen een uitvoering voor het valideren van de configuratie. Of selecteer **Alles publiceren** voor het publiceren van de wijzigingen en selecteer vervolgens **Trigger** voor het uitvoeren van een uitvoering.

## <a name="sap-bw-open-hub-destination-configurations"></a>Configuraties van SAP BW Open Hub bestemming

In deze sectie wordt de configuratie van de SAP BW-zijde voor het gebruik van de connector SAP BW Open Hub in Data Factory om gegevens te kopiëren.

### <a name="configure-delta-extraction-in-sap-bw"></a>Delta-extractie in SAP BW configureren

Als u zowel historische kopiëren en incrementele kopie of alleen incrementele kopie moet, configureert u extractie van delta in SAP BW.

1. Het doel van de Open-Hub maken. U kunt de OHD in SAP transactie RSA1, waarmee automatisch wordt gemaakt van de vereiste gegevenstransformatie en -gegevensoverdracht proces maken. Gebruik de volgende instellingen:

   - **ObjectType**: U kunt elk objecttype. We gebruiken hier, **InfoCube** als voorbeeld.
   - **Het doeltype**: Selecteer **databasetabel**.
   - **Sleutel van de tabel**: Selecteer **technische sleutel**.
   - **Extractie**: Selecteer **bewaren van gegevens en Records invoegen in tabel**.

   ![SAP BW OHD delta-extractie-dialoogvenster maken](media/load-sap-bw-data/create-sap-bw-ohd-delta.png)

   ![SAP BW OHD delta2 extractie-dialoogvenster maken](media/load-sap-bw-data/create-sap-bw-ohd-delta2.png)

   U kunt het aantal parallelle SAP processen uitgevoerd voor de DTP verhogen:

   ![create-sap-bw-ohd-delta3](media/load-sap-bw-data/create-sap-bw-ohd-delta3.png)

2. Plan de DTP in proces ketens.

   Een delta DTP voor een kubus werkt alleen als de rijen die nodig zijn nog niet zijn gecomprimeerd. Zorg ervoor dat BW-kubus compressie wordt niet uitgevoerd voordat de DTP aan de tabel Open Hub. De eenvoudigste manier om dit te doen is de DTP integreren in uw bestaande proces ketens. In het volgende voorbeeld wordt de DTP (aan de OHD) wordt ingevoegd in de procesketen tussen de *aanpassen* (cumulatieve totalisering) en *samenvouwen* stappen (kubus compressie).

   ![Stroomdiagram van SAP BW proces keten maken](media/load-sap-bw-data/create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>Volledige extractie in SAP BW configureren

Naast de delta-extractie, kunt u een volledige extractie van de dezelfde SAP BW InfoProvider. Dit geldt doorgaans als u wilt kopiëren volledig, maar geen incrementele, of u wilt [resync delta-extractie](#resync-delta-extraction).

U kunt meer dan één DTP geen voor de dezelfde OHD. U moet dus een extra OHD vóór de delta-extractie maken.

![SAP BW OHD volledige maken](media/load-sap-bw-data/create-sap-bw-ohd-full.png)

Kies andere opties dan voor het ophalen van verschillen voor een volledig laden OHD:

- In OHD: Stel de **extractie** optie om te **verwijderen gegevens en Records invoegen**. Anders, gegevens worden geëxtraheerd vaak wanneer u de DTP in een keten van BW proces herhalen.

- In de DTP: Stel **extractie modus** naar **volledige**. Moet u de automatisch gemaakte DTP van **Delta** naar **volledige** onmiddellijk nadat de OHD is gemaakt, zoals in deze afbeelding wordt weergegeven:

   ![Maken van SAP BW OHD geconfigureerd voor het ophalen van 'Volledig' in het dialoogvenster](media/load-sap-bw-data/create-sap-bw-ohd-full2.png)

- In de Open Hub BW-connector van Data Factory: Uitschakelen **uitsluiten laatste aanvraag**. Anders wordt er niets worden geëxtraheerd.

U doorgaans uitvoeren de volledige DTP handmatig. Of u kunt een procesketen maken voor de volledige DTP. Het is doorgaans een afzonderlijke keten die onafhankelijk is van uw bestaande proces ketens. In beide gevallen *Zorg ervoor dat de DTP is voltooid voordat u begint met de extractie met behulp van Data Factory kopiëren*. Anders wordt worden alleen gedeeltelijke gegevens gekopieerd.

### <a name="run-delta-extraction-the-first-time"></a>Delta-extractie, de eerste keer uitvoeren

De eerste delta-extractie is technisch gezien een *extractie volledige*. Standaard worden de laatste aanvraag de connector SAP BW Open Hub uitgesloten wanneer er gegevens worden gekopieerd. Voor de eerste delta-extractie, worden er worden geen gegevens opgehaald door de kopieeractiviteit in Data Factory totdat een latere DTP delta-gegevens in de tabel met een afzonderlijke aanvraag-ID genereert. Er zijn twee manieren om te voorkomen dat dit scenario:

- Uit te schakelen de **uitsluiten laatste aanvraag** optie voor de eerste delta-extractie. Zorg ervoor dat de eerste delta DTP voordat u begint met de delta-extractie de eerste keer is voltooid.
-  Gebruik de procedure voor het opnieuw synchroniseren van de delta-extractie, zoals beschreven in de volgende sectie.

### <a name="resync-delta-extraction"></a>Opnieuw synchroniseren van delta-extractie

De volgende scenario's de gegevens in SAP BW-kubussen gewijzigd maar worden niet beschouwd als de delta DTP:

- SAP BW selectief verwijderen (van rijen met behulp van een filtervoorwaarde)
- SAP BW aanvraag verwijderen (van defecte aanvragen)

Een Hub-SAP Open doel is niet een doel data mart gereguleerde gegevens (in alle SAP BW-ondersteuningspakketten sinds 2015). U kunt dus gegevens verwijderen uit een kubus zonder dat u de gegevens in de OHD wijzigt. U moet vervolgens de gegevens van de kubus met Data Factory resync:

1. Voer een volledige extractie in Data Factory (via een volledige DTP in SAP).
2. Alle rijen in de tabel Open Hub voor de delta-DTP verwijderen.
3. De status van de delta DTP instellen op **opgehaald**.

Werkt zoals verwacht daarna alle latere delta DTPs en Data Factory delta extracties.

Om in te stellen van de status van de delta DTP naar **opgehaald**, kunt u de volgende optie handmatig uitvoeren van de delta DTP:

    *No Data Transfer; Delta Status in Source: Fetched*

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de ondersteuning van de connector SAP BW Open Hub:

> [!div class="nextstepaction"]
>[SAP Business Warehouse Open Hub-connector](connector-sap-business-warehouse-open-hub.md)
