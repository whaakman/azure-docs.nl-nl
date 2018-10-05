---
title: Power BI-zelfstudie voor Azure Cosmos DB-connector | Microsoft Docs
description: Gebruik deze zelfstudie Power BI importeren JSON, inzichtelijke rapporten maken en gegevens visualiseren met behulp van de Azure Cosmos DB en Power BI-connector.
keywords: Power bi-zelfstudie, visualiseren, power bi-connector
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/03/2018
ms.author: sngun
ms.openlocfilehash: 686f21aa805560bb4c2a7fbf9b0c61e1edef14bb
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785525"
---
# <a name="visualize-azure-cosmos-db-data-by-using-the-power-bi-connector"></a>Azure Cosmos DB-gegevens visualiseren met behulp van de Power BI-connector

[Power BI](https://powerbi.microsoft.com/) is een online service waar u kunt maken en delen van dashboards en rapporten. Power BI Desktop is een hulpprogramma waarmee u gegevens ophalen uit verschillende gegevensbronnen voor rapportontwerp. Azure Cosmos DB is een van de gegevensbron die u met Power BI Desktop gebruiken kunt. U kunt Power BI Desktop verbinding maken met Azure Cosmos DB-account met de Azure Cosmos DB-connector voor Power BI.  Nadat u Azure Cosmos DB-gegevens in Power BI importeren, kunt u het te transformeren, rapporten maken en de rapporten publiceren naar Power BI.   

Dit artikel beschrijft de stappen die nodig zijn om Azure Cosmos DB-account voor Power BI Desktop verbinding te maken. Nadat u verbinding hebt u gaat u naar een verzameling, Extraheer de gegevens, zet de JSON-gegevens in tabelvorm en een rapport publiceren naar Power BI.

> [!NOTE]
> De Power BI-connector voor Azure Cosmos DB verbindt met Power BI Desktop. Rapporten die zijn gemaakt in Power BI Desktop kunnen worden gepubliceerd naar PowerBI.com. Directe extractie van Azure Cosmos DB-gegevens kan niet worden uitgevoerd in PowerBI.com. 

> [!NOTE]
> Verbinding maken met Azure Cosmos DB met de Power BI-connector wordt momenteel ondersteund voor SQL-API van Azure Cosmos DB en Gremlin-API-accounts.

## <a name="prerequisites"></a>Vereisten
Voordat u de instructies in deze zelfstudie Power BI, zorg ervoor dat u toegang tot de volgende bronnen hebt:

* [Download de nieuwste versie van Power BI Desktop](https://powerbi.microsoft.com/desktop).

* Download de [Vulkaan voorbeeldgegevens](https://github.com/Azure-Samples/azure-cosmos-db-sample-data/blob/master/SampleData/VolcanoData.json) vanuit GitHub.

* [Een Azure Cosmos DB-databaseaccount maken](https://azure.microsoft.com/documentation/articles/create-account/) en de Vulkaan-gegevens importeren met behulp van de [hulpprogramma voor gegevensmigratie Azure Cosmos DB](import-data.md).

Als u wilt delen in uw rapporten in PowerBI.com, moet u een account hebben in PowerBI.com.  Zie voor meer informatie over Power BI en Power BI Pro, [ https://powerbi.microsoft.com/pricing ](https://powerbi.microsoft.com/pricing).

## <a name="lets-get-started"></a>Aan de slag
In deze zelfstudie gaan we Stel dat u bent een geologist studie vulkanen over de hele wereld. De Vulkaan-gegevens worden opgeslagen in een Azure Cosmos DB-account en de indeling van JSON-document is als volgt:

    {
        "Volcano Name": "Rainier",
           "Country": "United States",
          "Region": "US-Washington",
          "Location": {
            "type": "Point",
            "coordinates": [
              -121.758,
              46.87
            ]
          },
          "Elevation": 4392,
          "Type": "Stratovolcano",
          "Status": "Dendrochronology",
          "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
    }

U de Vulkaan gegevens opgehaald uit de Azure Cosmos DB-account en visualiseren van gegevens in een interactieve Power BI-rapport.

1. Power BI Desktop worden uitgevoerd.

2. U kunt **gegevens ophalen**, Zie **recente bronnen**, of **Open andere rapporten** rechtstreeks vanuit het welkomstscherm wordt weergegeven. Selecteer de 'X' in de rechterbovenhoek om te sluiten van het scherm. De **rapport** weergave van Power BI Desktop wordt weergegeven.
   
   ![Power BI Desktop rapportweergave - Power BI-connector](./media/powerbi-visualize/power_bi_connector_pbireportview.png)

3. Selecteer de **Start** lint en klik vervolgens op **gegevens ophalen**.  De **gegevens ophalen** venster moet worden weergegeven.

4. Klik op **Azure**, selecteer **Azure Cosmos DB (bèta)**, en klik vervolgens op **Connect**. 

    ![Ophalen Power BI Desktop - Power BI-connector](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   

5. Op de **Preview-Connector** pagina, klikt u op **doorgaan**. De **Azure Cosmos DB** venster wordt weergegeven.

6. Geef de URL Azure Cosmos DB-account eindpunt u wilt ophalen van de gegevens uit zoals hieronder wordt weergegeven, en klik vervolgens op **OK**. Voor het gebruik van uw eigen account, kunt u de URL niet ophalen uit de URI in de **[sleutels](manage-account.md#keys)** blade van de Azure-portal. U kunt eventueel de databasenaam, de verzamelingsnaam van opgeven of de navigator gebruiken om te selecteren van de database en verzameling om te bepalen waar de gegevens vandaan komen.
   
7. Als u verbinding met dit eindpunt voor de eerste keer, wordt u gevraagd de accountsleutel op te geven. Voor uw eigen account ophalen van de sleutel van de **primaire sleutel** vak de **[alleen-Lezensleutels](manage-account.md#keys)** blade van de Azure-portal. Voer de juiste sleutel en klik vervolgens op **Connect**.
   
   U wordt aangeraden dat u de alleen-lezen-sleutel gebruikt als het maken van rapporten. Dit voorkomt dat beter beschermd van de hoofdsleutel aan potentiële beveiligingsrisico's. De sleutel alleen-lezen is beschikbaar via de [sleutels](manage-account.md#keys) blade van de Azure-portal. 
    
8. Wanneer het account is is verbonden, de **Navigator** deelvenster wordt weergegeven. De **Navigator** ziet u een lijst met databases onder het account.

9. Klik op en Ga op de database waar de gegevens voor het rapport is afkomstig uit, selecteer **volcanodb** (de databasenaam van uw kan afwijken).   

10. Selecteer nu een verzameling met de gegevens wilt ophalen, selecteert u **volcano1** (de verzamelingsnaam van uw kan afwijken).
    
    Het voorbeeldvenster bevat een overzicht van **Record** items.  Een Document wordt weergegeven als een **Record** type in Power BI. Op deze manier een geneste JSON-blok in een document is ook een **Record**.
    
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector - Navigator-venster](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Klik op **bewerken** om te starten van de Query-Editor in een nieuw venster om de gegevens te transformeren.

## <a name="flattening-and-transforming-json-documents"></a>Plat maken en JSON-documenten transformeren
1. Schakel over naar het venster Query-Editor van Power BI, waar de **Document** kolom in het middelste deelvenster.
   ![Query-Editor van Power BI Desktop](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. Klik op de uitbreidingsmodule aan de rechterkant van de **Document** kolomkop.  Het contextmenu met een lijst met velden wordt weergegeven.  Selecteer de velden die u nodig hebt voor uw rapport, bijvoorbeeld Vulkaan naam, land, regio, locatie, uitbreiding van bevoegdheden, Type, Status en laatste uitbreken weten. Schakel de **oorspronkelijke kolomnaam gebruiken als voorvoegsel** vak en klik vervolgens op **OK**.
   
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector - documenten uitvouwen](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. Het middelste deelvenster wordt een Preview-versie van het resultaat weergegeven met de velden geselecteerd.
   
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector - resultaten samenvoegen](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. In ons voorbeeld is de locatie-eigenschap een GeoJSON-blok in een document.  Zoals u zien kunt, de locatie wordt weergegeven als een **Record** type in Power BI Desktop.  
5. Klik op de uitbreidingsmodule aan de rechterkant van de kolomkop Document.Location.  Het contextmenu met type en de coördinaten velden worden weergegeven.  Laten we het veld met coördinaten selecteert, zorg ervoor dat **oorspronkelijke kolomnaam gebruiken als voorvoegsel** niet is geselecteerd en klik op **OK**.
   
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector - locatie-record](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. Het middelste deelvenster ziet u nu een kolom coördinaten van **lijst** type.  Zoals wordt weergegeven aan het begin van de zelfstudie, wordt de status van de GeoJSON-gegevens in deze zelfstudie is van het type met de breedtegraad en lengtegraad waarden die zijn opgenomen in de matrix van coördinaten.
   
    Het element coördinaten [0] geeft lengtegraad terwijl coördinaten [1] breedtegraad aangeeft.
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector - coördinaten lijst](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. Als u wilt de matrix van coördinaten afvlakken, maakt u een **aangepaste kolom** LatLong genoemd.  Selecteer de **kolom toevoegen** lint en klikt u op **aangepaste kolom**.  De **aangepaste kolom** venster wordt weergegeven.
8. Geef een naam voor de nieuwe kolom, bijvoorbeeld LatLong.
9. Daarna geeft u de aangepaste formule voor de nieuwe kolom.  In ons voorbeeld zullen we de breedtegraad en lengtegraad waarden worden gescheiden door een door komma's, zoals hieronder met behulp van de volgende formule weergegeven samenvoegen: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`. Klik op **OK**.
   
    Ga voor meer informatie over Data Analysis Expressions (DAX) met inbegrip van DAX-functies, [Basic DAX in Power BI Desktop](https://support.powerbi.com/knowledgebase/articles/554619-dax-basics-in-power-bi-desktop).
   
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector, aangepaste kolom toevoegen](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. Het middelste deelvenster ziet u nu de nieuwe LatLong kolommen ingevuld met de waarden.
    
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector, aangepaste LatLong kolom](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Als u een fout in de nieuwe kolom ontvangt, zorg ervoor dat de toegepaste stappen onder de Query-instellingen overeenkomen met de volgende afbeelding:
    
    ![Toegepaste stappen moet bron, navigatie, uitgebreid Document, Document.Location uitgevouwen, aanpassing toegevoegd](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Als uw stappen verschillen, verwijder de extra stappen en probeer de aangepaste kolom opnieuw toe te voegen. 

11. Klik op **sluiten en toepassen** om op te slaan van het gegevensmodel.
    
    ![Power BI-zelfstudie voor Azure Cosmos DB Power BI-connector - sluiten & toepassen](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>De rapporten bouwen
Weergave van Power BI Desktop-rapport is waar u kunt gaan maken van rapporten om gegevens te visualiseren.  U kunt rapporten maken via slepen en neerzetten van velden in de **rapport** canvas.

![Power BI Desktop rapportweergave - Power BI-connector](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

U moet in de rapportweergave vinden:

1. De **velden** deelvenster, dit is waar u een lijst met gegevensmodellen met velden die u voor uw rapporten gebruiken kunt kunt zien.
2. De **visualisaties** deelvenster. Een rapport kan één of meerdere visuele elementen bevatten.  Kies de typen visuele elementen ook geschikt zijn voor uw behoeften van de **visualisaties** deelvenster.
3. De **rapport** canvas, dit is waar u de visuele elementen maken voor uw rapport.
4. De **rapport** pagina. U kunt meerdere rapportpagina's toevoegen in Power BI Desktop.

Hieronder ziet u de basisstappen voor het maken van een rapport eenvoudig interactieve kaart weergeven.

1. In ons voorbeeld maken we een overzichtsweergave van de locatie van elke Vulkaan.  In de **visualisaties** deelvenster, klikt u op de visual type toewijzing als gemarkeerd in de bovenstaande schermafbeelding.  Ziet u de kaart visuele element getekend op de **rapport** canvas.  De **visualisatie** deelvenster moet ook een set eigenschappen die betrekking hebben op het visuele element van de kaart worden weergegeven.
2. Nu slepen en neerzetten van het veld LatLong uit de **velden** deelvenster aan de **locatie** eigenschap in **visualisaties** deelvenster.
3. Vervolgens slepen en neerzetten van het veld Naam Vulkaan de **legenda** eigenschap.  
4. Vervolgens slepen en neerzetten van het veld uitbreiding naar de **grootte** eigenschap.  
5. U ziet nu de kaart visual met een set met bellen die wijzen op de locatie van elke Vulkaan met de grootte van de Bel voor de uitbreiding van de Vulkaan te correleren.
6. U hebt nu een eenvoudig rapport gemaakt.  Verder kunt u het rapport aanpassen door meer visualisaties toe te voegen.  In ons geval hebben we een slicer Vulkaan Type zodat het rapport interactieve toegevoegd.  
   
7. Klik op het menu bestand **opslaan** en sla het bestand als PowerBITutorial.pbix.

## <a name="publish-and-share-your-report"></a>Publiceren en delen van uw rapport
Als u wilt uw rapport delen, moet u een account hebben in PowerBI.com.

1. Klik in de Power BI Desktop, op de **Start** lint.
2. Klik op **Publish**.  U wordt gevraagd om in te voeren van de gebruikersnaam en het wachtwoord voor uw PowerBI.com-account.
3. Zodra de referentie is geverifieerd, wordt het rapport is gepubliceerd naar de bestemming die u hebt geselecteerd.
4. Klik op **Open 'PowerBITutorial.pbix' in Power BI** om te zien en delen van uw rapport op PowerBI.com.
   
    ![Publiceren naar Power BI succes! Open in Power BI-zelfstudie](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Een dashboard maken in PowerBI.com
Nu dat u een rapport hebt, kunnen delen op PowerBI.com

Wanneer u uw rapport vanuit Power BI Desktop naar PowerBI.com publiceren, genereert een **rapport** en een **gegevensset** in uw tenant PowerBI.com. Bijvoorbeeld, nadat u een rapport met de naam gepubliceerd **PowerBITutorial** naar PowerBI.com, ziet u PowerBITutorial in zowel de **rapporten** en **gegevenssets** secties op PowerBI.com.

   ![Schermopname van het nieuwe rapport en gegevensset in PowerBI.com](./media/powerbi-visualize/powerbi-reports-datasets.png)

Voor het maken van een dashboard worden gedeeld, klikt u op de **Live-pagina vastmaken** knop op uw rapport PowerBI.com.

   ![Schermopname van het nieuwe rapport en gegevensset in PowerBI.com](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Volg de instructies in [een tegel uit een rapport vastmaken](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) naar een nieuw dashboard maken. 

U kunt ad-hoc wijzigingen in het rapport ook doen voordat u een dashboard maakt. Het wordt echter aanbevolen dat u Power BI Desktop gebruiken voor het uitvoeren van de wijzigingen aan en publiceren van het rapport naar PowerBI.com.

<!-- ## Refresh data in PowerBI.com
There are two ways to refresh data, ad hoc and scheduled.

For an ad hoc refresh, simply click on the eclipses (…) by the **Dataset**, e.g. PowerBITutorial. You should see a list of actions including **Refresh Now**. Click **Refresh Now** to refresh the data.

![Screenshot of Refresh Now in PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

For a scheduled refresh, do the following.

1. Click **Schedule Refresh** in the action list. 

    ![Screenshot of the Schedule Refresh in PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. In the **Settings** page, expand **Data source credentials**. 
3. Click on **Edit credentials**. 
   
    The Configure popup appears. 
4. Enter the key to connect to the Azure Cosmos DB account for that data set, then click **Sign in**. 
5. Expand **Schedule Refresh** and set up the schedule you want to refresh the dataset. 
6. Click **Apply** and you are done setting up the scheduled refresh.
-->
## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over Power BI, [aan de slag met Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* Zie voor meer informatie over Azure Cosmos DB, de [landingspagina voor Azure Cosmos DB-documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/).

