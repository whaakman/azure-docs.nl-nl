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
ms.date: 03/08/2019
ms.author: jingwang
ms.openlocfilehash: 607c3ff128c82c7baa268cf8f068232428ec5331
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57733217"
---
# <a name="load-data-from-sap-business-warehouse-bw-by-using-azure-data-factory"></a>Het laden van de gegevens van SAP Business Warehouse (BW) met behulp van Azure Data Factory

Dit artikel ziet u een overzicht over het gebruik van de Data Factory _gegevens van SAP Business Warehouse (BW) via Open Hub laden in Azure Data Lake Storage Gen2_. U kunt uitvoeren van gelijksoortige stappen als u wilt kopiëren van gegevens naar andere [sinkgegevensopslag ondersteund](copy-activity-overview.md#supported-data-stores-and-formats). 

> [!TIP]
> Raadpleeg [SAP BW Open Hub connector artikel](connector-sap-business-warehouse-open-hub.md) op in het algemeen kopiëren van gegevens uit de SAP BW, met inbegrip van inleiding voor SAP BW Open Hub integratie- en delta-extractie stroom.

## <a name="prerequisites"></a>Vereisten

- **Azure Data Factory (ADF):** Als u een data factory hebt, volgt u de '[maken van een data factory](quickstart-create-data-factory-portal.md#create-a-data-factory)"sectie een te maken. 

- **SAP BW Open Hub bestemming (OHD) met het doeltype als 'Databasetabel'.** Ga als volgt [configuraties van SAP BW Open Hub bestemming](#sap-bw-open-hub-destination-configurations) sectie een te maken of om te controleren of uw bestaande OHD correct is geconfigureerd voor het worden geïntegreerd met ADF.

- **SAP BW-gebruiker die wordt gebruikt, moet de volgende bevoegdheden hebben:**

  - Autorisatie voor RFC en SAP BW.
  - Machtigingen voor de '**uitvoeren**"Activiteit van autorisatie-Object"**S_SDSAUTH**'.

- **[Zelf-hostende Integration Runtime](concepts-integration-runtime.md#self-hosted-integration-runtime) met SAP .NET connector 3.0 zijn vereist**. Hieronder vindt u de gedetailleerde voorbereidingen die moeten worden uitgevoerd:

  1. Installeren en registreren van de IR zelfgehoste met versie > = 3.13 (behandeld in de volgende procedure). 

  2. Download de [64-bits SAP .NET Connector 3.0](https://support.sap.com/en/product/connectors/msnet.html) van de SAP-website, en op de machine zelfgehoste IR installeren.  Wanneer u installeert, in het venster 'optionele installatiestappen uit', zorg ervoor dat u selecteert de "**assembly's installeren in GAC**" optie, zoals wordt weergegeven in de volgende afbeelding.

     ![SAP .NET Connector instellen](media\connector-sap-business-warehouse-open-hub\install-sap-dotnet-connector.png)

## <a name="full-copy-from-sap-bw-open-hub"></a>Volledige kopie van SAP BW Open Hub

In Azure portal, Ga naar uw data factory -> Selecteer **Author & Monitor** om de ADF-UI in een afzonderlijk tabblad te openen. 

1. Selecteer op de pagina **Aan de slag** **Copy Data** om het hulpprogramma Copy Data te starten. 

2. Op de **eigenschappen** pagina, Geef een naam voor de **taaknaam** veld en selecteer **volgende**.

3. Op de **brongegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken** -> Selecteer **SAP BW Open Hub** uit de galerie connector -> Selecteer **doorgaan**. U kunt "SAP" typen in het zoekvak om te filteren van de connectors.

4. Op de **opgeven SAP BW Open hubverbinding** pagina 

   ![SAP BW Open Hub gekoppelde service maken](media\load-sap-bw-data\create-sap-bw-open-hub-linked-service.png)

   1. Kies de **verbinding maken via Integratieruntime**: klik op de vervolgkeuzelijst om te selecteren van een bestaande zelfgehoste IR of er een maken als u geen zelfgehoste IR nog instellen. 

      Voor het maken van nieuwe, klikt u op **+ nieuw** in de vervolgkeuzelijst select-type -> **zelf-hostend** -> Geef een **naam** en klikt u op **volgende** -> kiezen **Snelle installatie** te installeren op de huidige computer of gaat u als volgt de **handmatige installatie** er stappen.

      Zoals vermeld in [vereisten](#prerequisites), zorg ervoor dat u hebt ook de **SAP .NET connector 3.0** geïnstalleerd op dezelfde computer waarop de zelfgehoste IR wordt uitgevoerd.

   2. Geef de SAP BW **servernaam**, **systeemnummer**, **Client-ID,** **taal** (indien dan nl), **gebruikersnaam**, en **wachtwoord**.

   3. Klik op **verbinding testen** voor het valideren van de instellingen, selecteer **voltooien**.

   4. U ziet dat een nieuwe verbinding wordt gemaakt. Selecteer **Volgende**.

5. Op de **Open Hub selecteren bestemmingen** pagina, de bestemmingen die zijn beschikbaar op de SAP BW Open Hub bladeren en selecteren die u wilt kopiëren van gegevens uit en klik vervolgens op **volgende**.

   ![SAP BW Open Hub tabel selecteren](media\load-sap-bw-data\select-sap-bw-open-hub-table.png)

6. Geef het filter indien nodig. Als uw Open Hub bestemming alleen gegevens uit één Data Transfer proces (DTP) kan worden uitgevoerd met één aanvraag-ID bevat, of als u weet zeker dat uw DTP is voltooid en u wilt kopiëren van alle gegevens, schakel het selectievakje de **laatste aanvraag uitsluiten**. U kunt meer informatie over hoe deze instellingen hebben betrekking op uw SAP BW-configuratie in [configuraties van SAP BW Open Hub bestemming](#sap-bw-open-hub-destination-configurations) sectie. Klik op **valideren** om te controleren op dubbele gegevens geretourneerd, schakelt u **volgende**.

   ![Filter voor SAP BW Open Hub configureren](media\load-sap-bw-data\configure-sap-bw-open-hub-filter.png)

7. In de **doelgegevensarchief** pagina, klikt u op **+ nieuwe verbinding maken**, en selecteer vervolgens **Azure Data Lake Storage Gen2**, en selecteer **doorgaan**.

8. In de **opgeven Azure Data Lake Storage verbinding** pagina 

   ![ADLS Gen2 gekoppelde service maken](media\load-sap-bw-data\create-adls-gen2-linked-service.png)

   1. Selecteer uw Data Lake Storage Gen2 geschikt account van de "naam van het Opslagaccount" vervolgkeuzelijst.
   2. Selecteer **voltooien** om de verbinding te maken. Selecteer vervolgens **Volgende**.

9. In de **uitvoerbestand of uitvoermap kiezen** pagina, voert u 'copyfromopenhub' als de naam van de uitvoer-map en selecteer **volgende**.

   ![Uitvoermap kiezen](media\load-sap-bw-data\choose-output-folder.png)

10. In de **bestandsindeling instellen** weergeeft, schakelt **volgende** om de standaardinstellingen te gebruiken.

   ![Sink-indeling opgeven](media\load-sap-bw-data\specify-sink-format.png)

11. In de **instellingen** pagina uit, vouw de **prestatie-instellingen**, en stel **graad van parallelle uitvoering kopie** zoals 5 als u wilt laden uit SAP BW parallel. Klik op **volgende**.

    ![Instellingen configureren](media\load-sap-bw-data\configure-copy-settings.png)

12. In de **samenvatting** pagina, Controleer de instellingen en selecteer **volgende**.

13. In de **implementatie** weergeeft, schakelt **Monitor** voor het bewaken van de pijplijn.

    ![De pagina Implementatie](media\load-sap-bw-data\deployment.png)

14. U ziet dat het tabblad **Controleren** aan de linkerkant automatisch wordt geselecteerd. De **acties** kolom bevat koppelingen om details uitvoering van activiteit weer te geven en de pijplijn opnieuw uitvoeren:

    ![Pijplijn bewaken](media\load-sap-bw-data\pipeline-monitoring.png)

15. Als u uitvoeringen van activiteit die gekoppeld aan de pijplijnuitvoering zijn, selecteer de **uitvoeringen van activiteit weergeven** herstelkoppeling in de **acties** kolom. Omdat er slechts één activiteit (kopieeractiviteit) in de pijplijn is, ziet u slechts één vermelding in de lijst. Overschakelen naar de vorige weergave, selecteert u de **pijplijnen** koppelen aan de bovenkant. Selecteer **Vernieuwen** om de lijst te vernieuwen.

    ![Controle van activiteiten](media\load-sap-bw-data\activity-monitoring.png)

16. Voor het controleren van de uitvoering van details voor elke activiteit kopiëren, selecteert u de **Details** koppeling (van een bril) onder **acties** in de activiteit controle weergeven. U kunt details controleren zoals de hoeveelheid gegevens gekopieerd van de bron naar het sink, de doorvoer van gegevens, de stappen worden uitgevoerd met de bijbehorende tijdsduur en configuraties gebruikt:

    ![Details van activiteitsbewaking](media\load-sap-bw-data\activity-monitoring-details.png)

17. Controleer de **maximale aanvraag-ID** die wordt gekopieerd. Ga terug naar de activiteit controle weergeven, klikt u op de **uitvoer** onder **acties**.

    ![Uitvoer van activiteit](media\load-sap-bw-data\activity-output.png)

    ![Details van computeractiviteit uitvoer](media\load-sap-bw-data\activity-output-details.png)

## <a name="incremental-copy-from-sap-bw-open-hub"></a>Incrementele kopie van SAP BW Open Hub

> [!TIP]

> Raadpleeg [SAP BW Open Hub connector delta-extractie stroom](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow) voor meer informatie over de werking van ADF kopieeractiviteit voor het kopiëren van incrementele gegevens van SAP BW.

Nu gaat u verder naar de incrementele kopie van SAP BW Open Hub configureren. 

De incrementele kopie maakt gebruik van bovengrens mechanisme op basis van aanvraag-ID wordt automatisch gegenereerd in SAP BW Open Hub doel door DTP. De werkstroom voor deze benadering wordt verduidelijkt in het volgende diagram:

![Werkstroom van de incrementele kopie](media\load-sap-bw-data\incremental-copy-workflow.png)

Op de UI ADF **aan de slag** weergeeft, schakelt **pijplijn maken**. 

1. Sleep de drie activiteiten - **opzoeken, kopiëren van gegevens en Web** : naar het canvas en maken ze on-success gekoppeld. In dit scenario gebruiken we Azure-Blob voor het opslaan van de bovengrens - max gekopieerde aanvraag-ID. U kunt ook SQL-database gebruiken om op te slaan, en Stored Procedure-activiteit gebruiken in plaats van webactiviteit bij te werken.

   ![De pijplijn incrementele kopie](media\load-sap-bw-data\incremental-copy-pipeline.png)

2. Lookup-activiteit kunt configureren:

   1. In de Lookup-activiteit **instellingen** tabblad controle **First row alleen** optie.

      ![Lookup-instellingen](media\load-sap-bw-data\lookup-settings.png)

   2. Configureer **brongegevensset** als een Blob-gegevensset in de **bestandspad**, wijst u naar de blob waar u de maximale gekopieerde aanvraag-ID als de bovengrens worden opgeslagen en houdt u de indeling als de indeling tekst.

      ![Instellingen voor BLOB-gegevensset](media\load-sap-bw-data\blob-dataset.png)

   3. In het bijbehorende blobpad, een blob te maken met inhoud 0.

      ![Blobinhoud](media\load-sap-bw-data\blob.png)

3. Configureer de Kopieeractiviteit: 

   1. In **bron** tabblad, configureer dan **brongegevensset** als een gegevensset SAP BW Open Hub. 

   2. Bewerk de **SAP BW Open Hub gegevensset**:

      1. In **Parameters** tabblad, stelt u een parameter met de naam "requestId"
      2. In **verbinding** tabblad, Geef een naam voor de Open-Hub, behouden 'Uitsluiten laatste aanvraag-ID' geselecteerd en stel Base aanvraag-ID te gebruiken expressie (dynamische inhoud toevoegen) `@dataset().requestId`.

   3. Ga terug naar activiteit kopiëren **bron** tabblad, configureer dan de waarde 'requestId' voor het gebruik van de expressie (dynamische inhoud toevoegen) `@{activity('<look up activity name>').output.firstRow.Prop_0}`. Het 'uiterlijk van de naam van de activiteit"in deze expressie dienovereenkomstig wijzigen.

      ![Broninstellingen kopiëren](media\load-sap-bw-data\copy-source.png)

4. Webactiviteit configureren: deze Web-activiteit moet worden gebeld logische App voor het opslaan van maximale gekopieerde aanvraag-ID in de blob.

   1. Ga naar Azure portal-Nieuw -> een **logische App** met een **HTTP-aanvraag** en één **blob maken** als volgt. Gebruik de dezelfde blob-bestand dat is geconfigureerd in de bron van de bovenstaande Lookup-activiteit. En kopieer de **URL voor HTTP POST** die worden gebruikt voor de webactiviteit.

      ![Logic App-configuratie](media\load-sap-bw-data\logic-app-config.png)

   2. Ga terug naar het bewerken van de ADF **activiteit Web** instellingen zoals hieronder. De hoofdtekst als expressie configureren (dynamische inhoud toevoegen) `{"sapOpenHubMaxRequestId":"@{activity('CopyFromSap').output.sapOpenHubMaxRequestId}"}`.

      ![Instellingen voor web-activiteit](media\load-sap-bw-data\web-activity-settings.png)

5. Vervolgens kunt u op **Debug** Valideer de configuratie, of selecteer **Alles publiceren** naar alle wijzigingen publiceren en klik vervolgens op **Trigger** voor het uitvoeren van een uitvoering.

## <a name="sap-bw-open-hub-destination-configurations"></a>Configuraties van SAP BW Open Hub bestemming

In deze sectie wordt de benodigde configuratie SAP BW aan om te kunnen openen Hub van SAP BW-connector in ADF gebruiken om gegevens te kopiëren.

### <a name="configure-delta-extraction-in-sap-bw"></a>Delta-extractie in SAP BW configureren

Als u zowel historische kopiëren en incrementele kopie of alleen incrementele kopie moet, configureert u de delta-extractie in SAP BW.

1. De bestemming Open Hub (OHD) maken

   U kunt de OHD in SAP transactie RSA1 maken. Dit maakt automatisch de vereiste transformatie- en Data Transfer proces (DTP). Gebruik de volgende instellingen:

   - Objecttype kan zijn. We gebruiken hier InfoCube als voorbeeld.
   - **Het doeltype:** *Database-tabel*
   - **Sleutel van de tabel:** *Technische sleutel*
   - **Extractie:** *Bewaren van gegevens en Records invoegen in tabel*

   ![SAP BW OHD delta-extractie maken](media\load-sap-bw-data\create-sap-bw-ohd-delta.png)

   ![create-sap-bw-ohd-delta2](media\load-sap-bw-data\create-sap-bw-ohd-delta2.png)

   U kunt het aantal parallelle SAP processen uitgevoerd voor de DTP verhogen:

   ![create-sap-bw-ohd-delta3](media/load-sap-bw-data\create-sap-bw-ohd-delta3.png)

2. De DTP in proces ketens plannen

   Een Delta-DTP voor een kubus werkt alleen wanneer de benodigde rijen niet gecomprimeerd nog. Daarom moet u ervoor dat BW-kubus compressie wordt niet uitgevoerd voordat de DTP aan de tabel Open Hub. De eenvoudigste manier is deze DTP integreren in uw bestaande proces ketens. In het onderstaande voorbeeld de DTP (aan de OHD) wordt ingevoegd in de procesketen tussen de stap aanpassen (cumulatieve Totalisering) en samenvouwen (kubus compressie).

   ![create-sap-bw-process-chain](media\load-sap-bw-data\create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>Volledige extractie in SAP BW configureren

Naast de extractie verschillen, is het raadzaam om een volledige extractie van de dezelfde InfoProvider. Dit geldt doorgaans als u wilt kopiëren zonder incrementele volledig of u wilt [opnieuw synchroniseren van delta-extractie](#re-sync-delta-extraction).

U moet meer dan één DTP hebt voor de dezelfde OHD. Daarom moet u een extra OHD dan delta-extractie maken.

![create-sap-bw-ohd-full](media\load-sap-bw-data\create-sap-bw-ohd-full.png)

Kies andere opties dan delta-extractie voor een volledig laden OHD:

- In OHD: Stel de optie 'Extractie' als '*verwijderen gegevens en Records invoegen*'. Gegevens zouden anders worden geëxtraheerd vaak wanneer de DTP in een keten van BW proces te herhalen.

- In DTP: 'Extractie modus' ingesteld als "*volledige*'. U moet de automatisch gemaakte DTP basis van Delta in volledige wijzigen nadat de OHD is gemaakt:

   ![create-sap-bw-ohd-full2](media\load-sap-bw-data\create-sap-bw-ohd-full2.png)

- In ADF SAP BW Open Hub-connector: Schakel de optie '*uitsluiten laatste aanvraag*'. Anders zou er niets worden geëxtraheerd. 

U doorgaans uitvoeren de volledige DTP handmatig. Of u kunt ook een procesketen maken voor de volledige DTP - dit is meestal een afzonderlijk procesketen onafhankelijk van uw bestaande proces ketens. In beide gevallen moet u **Zorg ervoor dat de DTP is voltooid voordat u begint met de extractie met ADF-kopie**, anders gedeeltelijke gegevens worden gekopieerd.

### <a name="run-delta-extraction-the-first-time"></a>Delta-extractie, de eerste keer uitvoeren

De eerste Delta-extractie is technisch gezien een **volledige extractie**. Houd er rekening mee door standaard Open Hub van ADF SAP BW-connector niet van toepassing op de laatste aanvraag bij het kopiëren van de gegevens. In het geval van delta-extractie voor de eerste keer in ADF kopiëren activtiy, er worden geen gegevens worden geëxtraheerd totdat er verdere DTP genereert delta-gegevens in de tabel met afzonderlijke aanvraag-ID. Terwijl er twee manieren zijn om te voorkomen dat dit scenario:

1. Schakel de optie 'Uitsluiten laatste aanvraag' voor de eerste Delta-extractie In dit geval moet u om ervoor te zorgen dat de eerste Delta DTP is voltooid voordat u begint met de eerste keer dat de Delta-extractie
2. Gebruik de procedure voor het opnieuw synchroniseren van de Delta-extractie zoals hieronder wordt beschreven.

### <a name="re-sync-delta-extraction"></a>Delta-extractie opnieuw synchroniseren

Er zijn enkele scenario's die de gegevens in SAP BW kubussen gewijzigd, maar worden niet beschouwd als de DTP verschillen:

- SAP BW selectief verwijderen (van rijen met behulp van een filtervoorwaarde)
- SAP BW aanvraag verwijderen (van onjuiste aanvraag)

Een Hub-SAP Open doel is geen doel data mart gereguleerde gegevens (in alle SAP BW ondersteuningspakketten sinds het jaar 2015). Daarom is het mogelijk om gegevens te verwijderen uit een kubus zonder dat u de gegevens in de OHD wijzigt. In dit geval moet u de gegevens van de kubus met de gegevens in ADF opnieuw synchroniseren door de volgende stappen uit:

1. Uitvoeren van een volledige extractie in ADF (via een volledige DTP in SAP)
2. Alle rijen in de tabel Open Hub voor de Delta-DTP verwijderen
3. Zet de status van de Delta-DTP op opgehaald

Daarna werkt alle latere Delta DTPs en ADF Delta extracties goed zoals verwacht.

U kunt de status van de Delta-DTP instellen op opgehaald door het uitvoeren van de Delta-DTP handmatig met behulp van de volgende optie: "*Er gegevens worden overgebracht; De Status van de verschillen in de bron: Opgehaalde*'.

## <a name="next-steps"></a>Volgende stappen

Ga verder met het volgende artikel voor meer informatie over ondersteuning voor Open Hub van SAP BW-connector: 

> [!div class="nextstepaction"]
>[SAP Business Warehouse Open Hub-connector](connector-sap-business-warehouse-open-hub.md)
