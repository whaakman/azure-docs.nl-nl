---
title: Zelfstudie voor fiets-share - Geavanceerd gegevens voorbereiden met Azure Machine Learning Workbench
description: En een end-to-end gegevens voorbereiding zelfstudie met behulp van Azure Machine Learning Workbench
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: article
ms.date: 09/21/2017
ms.openlocfilehash: e85515c29d8f626c7eb2bfb636dc6c18da78b5c5
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Fiets-share-zelfstudie: gegevens voorbereiden met Azure Machine Learning Workbench geavanceerde
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end gegevenswetenschap en geavanceerde analyses oplossing voor professionals gegevenswetenschappers gegevens voorbereiden, experimenten te ontwikkelen en implementeren van modellen in de cloud.

In deze zelfstudie maakt u Azure Machine Learning-services (preview) voor meer informatie over hoe:
> [!div class="checklist"]
> * Voorbereiden van gegevens interactief met het hulpprogramma voor systeemvoorbereiding Azure Machine Learning-gegevens
> * Importeren, transformeren en een testgegevensset te maken
> * Genereren van een pakket gegevens voorbereiden
> * Voer de voorbereiding gegevenspakket met behulp van Python
> * Een gegevensset training door het pakket voorbereiden van gegevens voor aanvullende invoerbestanden opnieuw te genereren

> [!IMPORTANT]
> Deze zelfstudie worden alleen de gegevens voorbereid, dit niet het model prediction opbouwt.
>
> De voorbereide gegevens kunt u uw eigen voorspellende modellen trainen. U kunt bijvoorbeeld een model voor het voorspellen van fiets aanvraag tijdens een venster 2 uur maken.

## <a name="prerequisites"></a>Vereisten
* Azure Machine Learning Workbench moet lokaal worden geïnstalleerd. Volg voor meer informatie de [installatie Quick Start](quickstart-installation.md).
* Als u bekend bent met het maken van een nieuw project in de Workbench.

## <a name="data-acquisition"></a>Overname van gegevens
Deze zelfstudie wordt gebruikgemaakt van de [Boston Hubway gegevensset](https://s3.amazonaws.com/hubway-data/index.html) en Boston weergegevens van [NOAA](http://www.noaa.gov/).

1. Download de gegevensbestanden van de volgende koppelingen op uw lokale ontwikkelomgeving. 
   * [Boston weergegevens](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Hubway reis gegevens uit Hubway website.

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Elke ZIP-bestand uitpakken na het downloaden.

## <a name="learn-about-the-datasets"></a>Meer informatie over de gegevenssets
1. De __Boston weer__ bestand bevat de volgende, weer-gerelateerde velden fouten gerapporteerd op uurbasis:
   * DATUM
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. De __Hubway__ gegevens is onderverdeeld in bestanden op jaar en maand. Bijvoorbeeld: het bestand met de naam `201501-hubway-tripdata.zip` bevat een CSV-bestand met gegevens voor januari 2015. De gegevens bevat de volgende velden van elke rij die een fiets reis vertegenwoordigt:

   * Reis duur (in seconden)
   * Starten van de datum en tijd
   * Datum en tijd voor het stoppen
   * Start de stationsnaam & -ID
   * Einde stationsnaam & -ID
   * Fiets-ID
   * Gebruikerstype (Achteloos = 24-uurs of 72 uur Pass-gebruiker. Lid = jaarlijks of maandelijks lid)
   * Postcode (als de gebruiker lid is)
   * Geslacht (zelf gemeld door een lid)

## <a name="create-a-new-project"></a>Een nieuw project maken
1. Start de **Azure Machine Learning Workbench** vanuit het startmenu of starten.

2. Maak een nieuwe Azure Machine Learning-project.  Klik op de  **+**  knop op de **projecten** pagina of **bestand** > **nieuw**.
   - Gebruik de **leeg Project** sjabloon.
   - Naam van uw project **BikeShare**. 

## <a id="newdatasource"></a>Een nieuwe gegevensbron maken

1. Maak een nieuwe gegevensbron. Klik op de **gegevens** knop (cilinder pictogram) op de werkbalk links. U ziet nu de **gegevensweergave**.

   ![Afbeelding van het tabblad gegevens weergeven](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Een gegevensbron toevoegen. Selecteer de  **+**  pictogram en selecteer vervolgens **gegevensbron toevoegen**.

   ![Afbeelding van de vermelding gegevensbron toevoegen](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>De weergegevens toe te voegen

1. **Gegevensopslag**: het gegevensarchief die de gegevens bevat. Omdat u bestanden gebruikt, selecteert u **bestand(en) / Active Directory**. Selecteer **volgende** om door te gaan.

   ![Afbeelding van de bestanden / mapvermelding](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Selectie van het bestand**: de weergegevens toe te voegen. Bladeren en selecteer de `BostonWeather.csv` -bestand dat u eerder hebt gedownload. Klik op **Volgende**.

   ![Afbeelding van de bestandsselectie met BostonWeater.csv geselecteerd](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **Bestandsgegevens**: Controleer of het bestand-schema dat wordt gedetecteerd. Azure Machine Learning Workbench analyseert de gegevens in het bestand en het schema te gebruiken afleidt.

   ![Afbeelding van de bestandsgegevens](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > De Workbench niet het juiste schema gedetecteerd in sommige gevallen. U moet altijd controleert u of de parameters juist zijn voor uw gegevensset. Controleer de gegevens weer, of dat ze zijn ingesteld op de volgende waarden:
   >
   > * __Bestandstype__: gescheiden bestand (csv, tsv, txt, enz.)
   > * __Scheidingsteken__: komma [,]
   > * __Opmerkingen regelteken__: geen waarde is ingesteld.
   > * __Overslaan regels modus__: niet overslaan
   > * __Bestand codering__: utf-8
   > * __Promoveren Headers modus__: Headers van het eerste bestand gebruiken

   De evaluatieversie van de gegevens moet worden weergegeven in de volgende kolommen:
   * **Pad**
   * **DATUM**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Als u wilt doorgaan, selecteer **volgende**.

4. **Gegevenstypen**: Controleer de gegevenstypen die automatisch worden gedetecteerd. Azure Machine Learning Workbench analyseert de gegevens in het bestand en infereert de gegevenstypen te gebruiken.

   Voor deze gegevens wijzigen de `DATA TYPE` van alle kolommen `String`.

   > [!NOTE]
   > `String`Markeer de mogelijkheden van de Workbench verderop in deze zelfstudie wordt gebruikt. 

   ![Afbeelding van de gegevenstypen](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Als u wilt doorgaan, selecteer __volgende__. 

5. **Steekproef nemen**: voor het maken van een schema steekproeven selecteert de **+ nieuw** knop. Selecteer de nieuwe __boven 10000__ rij die is toegevoegd en selecteer vervolgens __bewerken__. Stel __voorbeeld strategie__ naar **volledige**, en selecteer vervolgens **toepassen**.

   ![Afbeelding van het toevoegen van een strategie voor een nieuwe steekproeven](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   Gebruik de __volledige__ strategie, selecteer de __volledige__ vermelding en selecteer vervolgens __ingesteld als actief__. Een ster wordt weergegeven naast __volledige__ om aan te geven dat het de actieve strategie.

   ![Afbeelding van het volledige bestand als de actieve strategie](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Als u wilt doorgaan, selecteer **volgende**.

6. **Kolom pad naar**: de __kolom pad naar__ sectie kunt u het volledige pad als een kolom in de geïmporteerde gegevens bevatten. Selecteer __omvatten geen kolom pad naar__.

   > [!TIP]
   > Inclusief het pad als een kolom is handig als u een map met veel bestanden met andere bestandsnamen importeert. Het is ook nuttig als de bestandsnamen bevatten informatie die u wilt ophalen later.

   ![Afbeelding van de kolom pad is ingesteld op niet opnemen](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Voltooien**: voor het voltooien van de gegevensbron maken, selecteer de **voltooien** knop.

    Een nieuw tabblad met de naam __BostonWeather__ wordt geopend. Een voorbeeld van de gegevens wordt in een rasterweergave weergegeven. Het voorbeeld is gebaseerd op het actieve steekproeven schema die eerder is opgegeven.

    U ziet de **stappen** deelvenster aan de rechterkant van het scherm ziet u de afzonderlijke acties die tijdens het maken van deze gegevensbron.

   ![De installatiekopie van de gegevensbron, voorbeeld en stappen om weer te geven](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Metrische gegevens weergeven gegevensbron

Selecteer de __metrische gegevens__ bovenaan op de knop links van het tabblad rasterweergave. Deze weergave bevat de distributie en andere samengevoegde statistieken van de steekproefgegevens.

![Afbeelding van de metrische gegevens worden weergegeven](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Gebruik voor het configureren van de zichtbaarheid van de statistieken, de **metriek kiezen** vervolgkeuzelijst. Controleer en schakelt u er metrische gegevens om de rasterweergave te wijzigen.

Om terug te keren de __gegevensweergave__, selecteer __gegevens__ van de linkerbovenhoek van de pagina.

## <a name="add-data-source-to-data-preparation-package"></a>Gegevensbron toevoegen aan gegevenspakket voorbereiding

1. Selecteer __voorbereiden__ om te beginnen met het voorbereiden van de gegevens. 

2. Wanneer u wordt gevraagd, typt u een naam voor het pakket voorbereiden van gegevens, zoals `BikeShare Data Prep`. 

3. Selecteer __OK__ om door te gaan.

   ![Afbeelding van het dialoogvenster voorbereiden](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Een nieuw pakket met de naam **BikeShare gegevens Prep** wordt weergegeven onder __gegevens voorbereiden__ sectie van de __gegevens__ tabblad. 

   Als u wilt weergeven van het pakket, selecteer dit item. 

5. Selecteer de  **>>**  knop om te worden uitgevouwen om de __Dataflows__ opgenomen in het pakket. In dit voorbeeld __BostonWeather__ is de enige gegevensstroom.

   > [!IMPORTANT]
   > Een pakket kan meerdere Dataflows bevatten.

   ![Afbeelding van de dataflows opgenomen in het pakket](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Gegevens filteren op waarde
1. Gegevens filteren, met de rechtermuisknop op een cel met een bepaalde waarde en selecteren __Filter__, en vervolgens het type filter.

2. Selecteer een cel met de waarde voor deze zelfstudie `FM-15` en stelt u het filter aan een filter van **gelijk is aan**.  Nu de gegevens wordt gefilterd op alleen rijen geretourneerd waar de __REPORTTYPE__ is `FM-15`.

   ![Afbeelding van het dialoogvenster filter](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 is een type van meteorologische Terminal burgerluchtvaart routinematige weer rapport (METAR). De rapporten FM 15 worden langs de meest voltooid, met weinig ontbrekende gegevens worden waargenomen.

## <a name="remove-a-column"></a>Een kolom verwijderen

U hoeft niet meer de __REPORTTYPE__ kolom. Met de rechtermuisknop op de kolomkop en selecteer **kolom verwijderen**.

   ![Afbeelding van de optie van de kolom verwijderen](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Gegevenstypen wijzigen en verwijderen van fouten
1. Drukken __Ctrl (opdracht ⌘ op Mac)__ terwijl kolomkoppen kunt u meerdere kolommen in één keer selecteren. Gebruik dit om te selecteren van de volgende kolomkoppen:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Met de rechtermuisknop op** een van de geselecteerde kolomkoppen en selecteer **Type in het veld converteren naar numerieke**. Hiermee zet het gegevenstype voor de kolommen om numerieke.

   ![Meerdere kolommen converteren naar numerieke](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filter de foutwaarden. Sommige kolommen hebben conversieproblemen voor gegevenstype. Dit probleem wordt aangegeven door de kleur rood in de __kwaliteit gegevensbalk__ voor de kolom.

   Als u wilt verwijderen van de rijen die fouten bevatten, met de rechtermuisknop op de **HOURLYDRYBULBTEMPF** kolomkop. Selecteer **filterkolom**. De standaard **I wilt** als **rijen behouden**. Wijzig de **voorwaarden** vervolgkeuzelijst selecteren **is geen fout**. Selecteer **OK** het filter wilt toepassen.

4. Herhaal dit filterproces voor om te voorkomen de overige rijen met fouten in de andere kolommen, **HOURLYRelativeHumidity** en **HOURLYWindSpeed** kolommen.

## <a name="use-by-example-transformations"></a>Voorbeeld transformaties gebruiken

Als u de gegevens in een voorspelling voor twee uur tijdblokken, moet u de voorwaarden van de gemiddelde weer voor twee uur perioden berekenen. U kunt gebruiken om dit te doen, volgende acties:

* Gesplitste de **datum** kolom in afzonderlijke **datum** en **tijd** kolommen. Zie de volgende sectie voor gedetailleerde stappen.

* Afgeleid een **Hour_Range** kolom uit de **tijd** kolom. Zie de volgende sectie voor gedetailleerde stappen.

* Afgeleid een **datum\_uur\_bereik** kolom uit de **datum** en **Hour_Range** kolommen. Zie de volgende sectie voor gedetailleerde stappen.

### <a name="split-column-by-example"></a>Kolom splitsen per voorbeeld

1. Gesplitste de **datum** kolom opdelen in afzonderlijke datum- en time-kolommen. Met de rechtermuisknop op de **datum** kolomkop en selecteer **kolom splitsen door voorbeeld**.

   ![Afbeelding van de kolom splitsen door voorbeeld van invoer](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Azure Machine Learning Workbench automatisch een zinvolle scheidingsteken identificeert en maakt twee kolommen met het verdelen van de gegevens in datum- en tijdwaarden. 

3. Selecteer __OK__ te accepteren van de resultaten split-bewerking.

   ![Afbeelding van de kolommen gesplitste DATE_1 en DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Kolom afleiden per voorbeeld

1. Als u wilt een bereik van twee uur worden afgeleid, met de rechtermuisknop op de __datum\_2__ kolomkop en selecteer **kolom afgeleid door voorbeeld**.

   ![Afbeelding van de afgeleide kolom door een voorbeeld van invoer](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Een nieuwe lege kolom wordt toegevoegd met null-waarden.

2. Klik in de eerste lege cel in de nieuwe kolom. Geef een voorbeeld van het tijdsbereik gewenst door te typen `12AM-2AM` invoeren in de nieuwe kolom en druk vervolgens op.

   ![Afbeelding van de nieuwe kolom die met een waarde van 12: 00 A.M. - 2 uur](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Azure ML-Workbench synthesizes een programma op basis van de voorbeelden die u hebt opgegeven en hetzelfde programma toegepast op de overige rijen. De overige rijen worden automatisch ingevuld op basis van het voorbeeld dat u hebt opgegeven. Workbench ook uw gegevens analyseert en probeert rand gevallen identificeren. 
  
3. De tekst **analyseren van gegevens** geeft aan dat Workbench probeert te detecteren rand gevallen boven het raster. Wanneer u klaar bent, wordt de status gewijzigd in **revisie van de volgende voorgestelde rij** of **geen suggesties**. In dit voorbeeld **revisie van de volgende voorgestelde rij** wordt geretourneerd.

4. Selecteer de voorgestelde om wijzigingen te bekijken, **revisie van de volgende voorgestelde rij**. De cel die u moet controleren en corrigeren (indien nodig) wordt gemarkeerd op de weergave.

   ![Afbeelding van een rij controleren](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Selecteer __OK__ te accepteren van de transformatie.
 
5. U keert terug naar de rasterweergave van gegevens voor __BostonWeather__. Het raster bevat nu de drie kolommen eerder toegevoegd.

   ![Afbeelding van de rasterweergave met rijen die worden toegevoegd](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Alle wijzigingen die u hebt aangebracht behouden blijven op de **stappen** deelvenster. Ga naar de stap die u hebt gemaakt in de **stappen** deelvenster, klik op de pijl en selecteer de omlaag **bewerken**. Het venster Geavanceerde voor **kolom afgeleid door voorbeeld** wordt weergegeven. Alle voorbeelden blijven hier behouden. U kunt voorbeelden ook handmatig toevoegen door te dubbelklikken op een rij in het onderstaande raster. Selecteer **annuleren** om terug te keren naar het belangrijkste raster zonder de wijzigingen toepast. U kunt deze weergave ook openen door te selecteren **geavanceerde modus** tijdens het uitvoeren van een **kolom afgeleid door voorbeeld** transformeren.

6. Wijzig de naam van de kolom, dubbelklikt u op de kolomkop en het type **uur bereik**. Druk op **Enter** de wijziging op te slaan.

   ![Naam van de kolom](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Worden afgeleid van de datum en uur bereik, meervoudige selectie de **datum\_1** en **uur bereik** kolommen, klik met de rechtermuisknop en selecteer vervolgens **afgeleide kolom door voorbeeld**.

   ![Kolom die zijn afgeleid door voorbeeld](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Type `Jan 01, 2015 12AM-2AM` als in het voorbeeld op basis van de eerste rij en druk op **Enter**.

   De Workbench bepaalt de transformatie op basis van het voorbeeld dat u opgeeft. In dit voorbeeld wordt het resultaat is dat de datumnotatie is gewijzigd en samengevoegd met het venster twee uur.

   ![Afbeelding van het voorbeeld ' 01 januari 2015 12: 00 A.M. - 2 uur](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)


8. Wacht totdat de status te wijzigen van **analyseren van gegevens** naar **revisie van de volgende voorgestelde rij**. Dit kan enkele seconden duren. Selecteer de statuskoppeling om te navigeren naar de voorgestelde rij. 

   ![Afbeelding van de voorgestelde rij om te controleren](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   De rij heeft een null-waarde, omdat de bron-date-waarde voor mm-dd-jjjj of dd-mm-jjjj worden kan. Typ de juiste waarde van `Jan 13, 2015 2AM-4AM` en druk op **Enter**. De workbench maakt gebruik van de twee voorbeelden voor het verbeteren van de afleiding voor de overige rijen.

   ![Afbeelding van juist opgemaakte gegevens](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Selecteer **OK** te accepteren van de transformatie.

   ![Afbeelding van het raster voltooide transformatie](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > U kunt de geavanceerde modus van **afgeleide kolom door voorbeeld** voor deze stap door te klikken op de pijl omlaag in de **stappen** deelvenster. In het gegevensraster zijn selectievakjes naast de kolomnamen **datum\_1** en **uur bereik** kolommen. Schakel het selectievakje uit naast de **uur bereik** kolom om te zien hoe Hiermee wijzigt u de uitvoer. Bij gebrek aan de **uur bereik** kolom als invoer, **12: 00 A.M. - 2 uur** wordt behandeld als een constante en wordt toegevoegd aan de afgeleide waarden. Selecteer **annuleren** om terug te keren naar het belangrijkste raster zonder uw wijzigingen zijn toegepast.

10. Wijzig de naam van de kolom, dubbelklikt u op de koptekst. De naam wijzigen **uur datumbereik** en druk vervolgens op **Enter**.

11. Selecteert u de **datum**, **datum\_1**, **datum\_2**, en **uur bereik** kolommen. Met de rechtermuisknop en selecteer vervolgens **verwijderen kolom**.

## <a name="summarize-data-mean"></a>Samenvatten (gemiddeld)

De volgende stap is om samen te vatten van de voorwaarden weer door middel van het gemiddelde van de waarden, gegroepeerd per uur bereik.

1. Selecteer de **uur datumbereik** kolom en selecteer vervolgens **Summarize** van de **transformeert** menu.

    ![Transformaties menu](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Als u wilt de gegevens samenvatten, sleept u kolommen uit het raster aan de onderkant van de pagina naar de linker- en deelvensters boven. Het linkerdeelvenster met de tekst die **Sleep kolommen hierheen om gegevens te groeperen**. Het rechter deelvenster bevat de tekst **Sleep kolommen hierheen om gegevens te vatten**. 

    Sleep de **uur datumbereik** kolom van het raster onder aan het linkerdeelvenster. Sleep **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity**, en **HOURLYWindSpeed** naar het rechtervenster. 

    Selecteer in het rechterdeelvenster **betekenen** als de **cumulatieve** meting voor elke kolom. Klik op **OK** voltooien van de samenvatting.

   ![Afbeelding van samengevatte gegevens scherm](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Gegevensstroom met script transformeren

Wijzigen van de gegevens in de numerieke kolommen in een bereik van 0-1, kunt sommige modellen kunt convergeren dat ze snel. Er is momenteel geen ingebouwde transformatie algemeen doen deze transformatie, maar een pythonscript kan worden gebruikt voor deze bewerking niet uitvoeren.

1. Van de **transformeren** selecteert u **transformeren gegevensstroom**.

2. Voer de volgende code in het tekstvak dat wordt weergegeven. Als u de kolomnamen hebt gebruikt, moet de code werkt zonder aanpassingen. U schrijft een eenvoudige min max normalisatie-logica in Python.

    > [!WARNING]
    > Het script verwacht de kolomnamen die eerder in deze zelfstudie worden gebruikt. Als u verschillende kolomnamen hebt, moet u de namen in het script wijzigen.

   ```python
   maxVal = max(df["HOURLYDRYBULBTEMPF_Mean"])
   minVal = min(df["HOURLYDRYBULBTEMPF_Mean"])
   df["HOURLYDRYBULBTEMPF_Mean"] = (df["HOURLYDRYBULBTEMPF_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYDRYBULBTEMPF_Mean":"N_DryBulbTemp"},inplace=True)

   maxVal = max(df["HOURLYRelativeHumidity_Mean"])
   minVal = min(df["HOURLYRelativeHumidity_Mean"])
   df["HOURLYRelativeHumidity_Mean"] = (df["HOURLYRelativeHumidity_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYRelativeHumidity_Mean":"N_RelativeHumidity"},inplace=True)

   maxVal = max(df["HOURLYWindSpeed_Mean"])
   minVal = min(df["HOURLYWindSpeed_Mean"])
   df["HOURLYWindSpeed_Mean"] = (df["HOURLYWindSpeed_Mean"]-minVal)/(maxVal-minVal)
   df.rename(columns={"HOURLYWindSpeed_Mean":"N_WindSpeed"},inplace=True)

   df
   ```

    > [!TIP]
    > Het Python-script moet retourneren `df` aan het einde. Deze waarde wordt gebruikt voor het vullen van het raster.
    
    ![Dialoogvenster voor stroom script gegevens transformeren](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Selecteer __OK__ u het script. De numerieke kolommen in het raster bevat nu de waarden in het bereik van 0-1.

    ![Raster met waarden tussen 0 en 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

U klaar bent met het voorbereiden van de weergegevens. Vervolgens de gegevens van de reis voorbereiden.

## <a name="load-trip-data"></a>Reis gegevens laden

1. Voor het importeren van de `201701-hubway-tripdata.csv` gebruikt u de stappen in de [een nieuwe gegevensbron maken](#newdatasource) sectie. Gebruik de volgende opties tijdens het importproces:

    * __Steekproeven schema__: **volledige** schema, wordt de steekproef actief, maken en 
    * __Gegevenstype__: Accepteer de standaardinstellingen.

2. Nadat de gegevens zijn geïmporteerd, selecteer de __voorbereiden__ knop om te beginnen met het voorbereiden van de gegevens. Selecteer de bestaande **BikeShare gegevens Prep.dprep** van het pakket en selecteert u vervolgens __OK__.

    Dit proces wordt toegevoegd een **Gegevensstroomblok** aan de bestaande **gegevens voorbereiden** bestanden in plaats van een nieuwe maakt.

    ![Afbeelding van het selecteren van het bestaande pakket](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Nadat het raster is geladen, vouw __DATAFLOWS__. Er zijn nu twee dataflows: **BostonWeather** en **201701-hubway-tripdata**. Selecteer de **201701-hubway-tripdata** vermelding.

    ![Afbeelding van de vermelding 201701-hubway-tripdata](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>Gebruik kaart inspector

Voor het voorbereiden van gegevens, er zijn een aantal nuttig visualisaties aangeroepen **Inspectors** voor String, numerieke en geografische gegevens waarmee de gegevens beter te begrijpen en bij het identificeren van uitschieters. Gebruik de volgende stappen uit om de kaart inspector:

1. Selecteert u de **station breedtegraad start** en **station lengtegraad start** kolommen. Met de rechtermuisknop op een van de kolommen en selecteer vervolgens **kaart**.

    > [!TIP]
    > Om meerdere selecteren, houdt u __Ctrl (opdracht ⌘ op Mac)__ en selecteer de header voor elke kolom.

    ![Afbeelding van de kaart visualisatie](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Als u wilt de kaart visualisatie maximaliseren, selecteer de **maximaliseren** pictogram. Als u wilt de kaart aan het venster past, selecteer de **E** pictogram aan de linkerkant van het hoogste van de visualisatie.

    ![Gemaximaliseerd installatiekopie](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Klik op de **minimaliseren** terug te keren naar de rasterweergave.

## <a name="use-column-statistics-inspector"></a>Kolom statistieken Inspector gebruiken

Voor het gebruik van de kolom statistieken inspector met de rechtermuisknop op de __tripduration__ en selecteert u __Kolomstatistieken__.

![Kolom statistieken vermelding](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

Dit proces voegt een nieuwe visualisatie met de titel __tripduration statistieken__ in de __INSPECTORS__ deelvenster.

![Afbeelding van de statistieken tripduration inspector](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> De maximale waarde van de duur reis is **961,814 minuten**, die ongeveer twee jaar is. Het lijkt dat er zijn enkele uitschieters in de gegevensset.

## <a name="use-histogram-inspector"></a>Gebruik histogram inspector

Wilt u de uitschieters identificeren, met de rechtermuisknop op de __tripduration__ en selecteert u __Histogram__.

![Histogram inspector](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Het histogram is niet handig, omdat de uitschieters zijn scheeftrekken de grafiek.

## <a name="add-column-using-script"></a>Kolom met script toevoegen

1. Met de rechtermuisknop op de **tripduration** en selecteert u **kolom toevoegen (Script)**.

    ![Afbeelding van het menu van de kolom (script) toevoegen](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. In de __kolom toevoegen (Script)__ dialoogvenster, gebruik de volgende waarden:

    * __Nieuwe kolomnaam__: logtripduration
    * __Voeg deze nieuwe kolom na__: tripduration
    * __Nieuwe kolomcode voor de__:`math.log(row.tripduration)`
    * __Code Bloktype__: expressie

   ![Het dialoogvenster Add Column (Script)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Selecteer __OK__ toevoegen de **logtripduration** kolom.

4. Met de rechtermuisknop op de kolom en selecteer **Histogram**.

    ![Histogram van de kolom logtripduration](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Dit histogram lijkt visueel, zoals een normale verdeling met een abnormale staart.

## <a name="use-advanced-filter"></a>Het gebruik van geavanceerde Filter

De inspecteurs met een filter op de gegevens worden bijgewerkt met de nieuwe distributiepunten. Met de rechtermuisknop op de **logtripduration** en selecteert u **filterkolom**. In de __bewerken__ dialoogvenster, gebruik de volgende waarden:

* __Dit nummer filterkolom__: logtripduration
* __Ik wil__: rijen behouden
* __Wanneer__: een van de onderstaande voorwaarden zijn waar (logische OR)
* __Als deze kolom__: kleiner dan
* __De waarde__: 9

![Filteropties](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Selecteer __OK__ het filter wilt toepassen.

![Bijgewerkte histogrammen nadat filter is toegepast](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Het effect halo

1. Maximaliseer de **logtripduration** histogram. Er is een blauwe histogram over een grijze histogram heen weergegeven. Deze informatie wilt weergeven, heet de **Halo Effect**:

    * De **grijze histogram** vertegenwoordigt de distributie voordat de bewerking (in dit geval de filterbewerking).
    * De **blauw histogram** vertegenwoordigt het histogram na het opnieuw. 

   Het effect halo helpt met het effect van een bewerking op de gegevens te visualiseren.

   ![Afbeelding van het effect halo](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > U ziet dat de blauwe histogram korter verschijnt vergeleken met de vorige sectie. Dit komt door automatische opnieuw bucketing van gegevens in het nieuwe bereik.

2. Als de halo verwijderen, selecteert __bewerken__ en schakelt u __weergeven halo__.

    ![Opties voor het histogram](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Selecteer **OK** het effect halo uitschakelen en vervolgens minimaliseert het histogram.

### <a name="remove-columns"></a>Kolommen verwijderen

In de gegevens reis vertegenwoordigt elke rij een fiets ophalen. Voor deze zelfstudie hoeft u alleen de **starttime** en **station id start** kolommen. Verwijder de andere kolommen door meerdere selecteren deze twee kolommen met de rechtermuisknop op de kolomkop en selecteer vervolgens **houden kolom**. Andere kolommen worden verwijderd.

![Afbeelding van de optie van de kolom behouden](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Samenvatten (aantal)

Samenvattend fiets vraag gedurende 2 uur afgeleide kolommen te gebruiken.

1. Met de rechtermuisknop op de **starttime** en selecteert u **kolom afgeleid door voorbeeld**

    ![Afbeelding van worden afgeleid door voorbeeld-optie](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Bijvoorbeeld, voer een waarde van `Jan 01, 2017 12AM-2AM` voor de eerste rij.

    > [!IMPORTANT]
    > In het vorige voorbeeld hiervan afgeleide kolommen gebruikt u meerdere stappen voor het afleiden van een kolom die de datum en tijd is opgenomen. In dit voorbeeld ziet u dat deze bewerking kan worden uitgevoerd als één stap door een voorbeeld van de uiteindelijke uitvoer.

    > [!NOTE]
    > U kunt een voorbeeld met een van de rijen op te geven. In dit voorbeeld is de waarde van `Jan 01, 2017 12AM-2AM` is geldig voor de eerste rij van gegevens.

    ![Afbeelding van de bijvoorbeeld gegevens](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)
   
3. Wacht totdat de waarden op basis van alle rijen door de toepassing wordt berekend. Het kan enkele seconden duren. Nadat analyseren voltooid is, gebruikt u de __revisie van de volgende voorgestelde rij__ koppeling om gegevens te bekijken.

   ![Afbeelding van de voltooide analyse met de koppeling controleren](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Zorg ervoor dat de berekende waarden correct zijn. Als dit niet het geval is, werk de waarde met de verwachte waarde en druk op enter. Wacht totdat de analyse te voltooien. Voltooi de **revisie van de volgende voorgestelde rij** verwerkt totdat er **geen suggesties**. Eenmaal u ziet **geen suggesties**, de toepassing de edge-gevallen heeft bekeken en voldoet het kunstmatige programma is. Het is een best practice om uit te voeren een visuele controle van de getransformeerde gegevens voordat de transformatie worden geaccepteerd. 

4. Selecteer **OK** te accepteren van de transformatie. Wijzig de naam van de zojuist gemaakte kolom **uur datumbereik**.

    ![Afbeelding van de nieuwe naam kolom](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Met de rechtermuisknop op de **starttime** kolomkop en selecteer **verwijderen kolom**.

6. Voor het samenvatten, selecteer __Summarize__ van de __transformeren__ menu. Voor het maken van de transformatie, moet u de volgende acties uitvoeren:

    * Sleep __uur datumbereik__ en __station id start__ aan het deelvenster links (groeperen op).
    * Sleep __station id start__ naar rechts (samenvatten) deelvenster.

   ![Een installatiekopie van de samenvatting-opties](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Selecteer **OK** te accepteren van de samenvatting resultaat.

## <a name="join-dataflows"></a>Deelnemen aan dataflows

Als u wilt deelnemen aan de weergegevens met de gegevens reis, gebruikt u de volgende stappen uit:

1. Selecteer __Join__ van de __transformeert__ menu.

2. __Tabellen__: Selecteer **BostonWeather** als de gegevensstroom naar links en **201701-hubway-tripdata** als de juiste gegevensstroom. Als u wilt doorgaan, selecteer **volgende**.

    ![Afbeelding van de selecties voor de tabellen](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Sleutelkolommen__: Selecteer de **uur datumbereik** kolom in zowel de tabellen en selecteer vervolgens __volgende__.

    ![Afbeelding van de join op sleutelkolommen](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Type samenvoeging__: Selecteer __die overeenkomen met rijen__ als het jointype en selecteer __voltooien__.

    ![Overeenkomende rijen samenvoeging type](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Dit proces maakt een nieuwe gegevensstroom met de naam __Join resultaat__.

## <a name="create-additional-features"></a>Extra functies maken

1. Voor het maken van een kolom die de dag van de week bevat met de rechtermuisknop op de **uur datumbereik** en selecteert u **afgeleide kolom door voorbeeld**. Gebruik een waarde van __Sun__ voor een datum die is opgetreden op een zondag. Bijvoorbeeld: **01 januari 2017 12: 00 A.M. - 2 uur**. Druk op **Enter** en selecteer vervolgens **OK**. Wijzig de naam van deze kolom aan __werkdag__.

    ![Afbeelding van het maken van een nieuwe kolom die de dag van de week bevat](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Voor het maken van een kolom met de periode voor een rij met de rechtermuisknop op de **uur datumbereik** en selecteert u **afgeleide kolom door voorbeeld**. Gebruik een waarde van **12: 00 A.M. - 2 uur** voor de rij die **01 januari 2017 12: 00 A.M. - 2 uur**. Druk op **Enter** en selecteer vervolgens **OK**. Wijzig de naam van deze kolom aan **periode**.

    ![Afbeelding van de kolom periode](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Verwijderen van de **uur datumbereik** en **rDate uur bereik** kolommen, drukt u op **Ctrl (opdracht ⌘ op Mac)** en selecteert u elke kolomkop. Met de rechtermuisknop en selecteer vervolgens **kolom verwijderen**.

## <a name="read-data-from-python"></a>Gegevens lezen met Python

U kunt uitvoeren van een gegevenspakket van de voorbereiding van Python of PySpark en ophalen van het resultaat als een **gegevensframe**.

Voor het genereren van een voorbeeld pythonscript met de rechtermuisknop op __BikeShare gegevens Prep__ en selecteer __gegevensbestand toegangscode genereren__. Het voorbeeld Python-bestand is gemaakt in uw **projectmap**, en is ook geladen op een tabblad in de workbench. Het volgende Python-script is een voorbeeld van de code die wordt gegenereerd:

```python
# Use the Azure Machine Learning data preparation package
from azureml.dataprep import package

# Use the Azure Machine Learning data collector to log various metrics
from azureml.logging import get_azureml_logger
logger = get_azureml_logger()

# This call will load the referenced package and return a DataFrame.
# If run in a PySpark environment, this call returns a
# Spark DataFrame. If not, it will return a Pandas DataFrame.
df = package.run('BikeShare Data Prep.dprep', dataflow_idx=0)

# Remove this line and add code that uses the DataFrame
df.head(10)
```

Voor deze zelfstudie is de naam van het bestand `BikeShare Data Prep.py`. Dit bestand wordt later in de zelfstudie gebruikt.

## <a name="save-test-data-as-a-csv-file"></a>Testgegevens opslaan als een CSV-bestand

Om op te slaan de **Join resultaat** gegevensstroom naar een. CSV-bestand, moet u de `BikeShare Data Prep.py` script. Update het Python-script met de volgende code:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Selecteer **uitvoeren** vanaf de bovenkant van het scherm. Het script wordt verzonden als een **taak** op de lokale computer. Zodra de taak de status verandert in __voltooid__, het bestand is geschreven naar de opgegeven locatie.

## <a name="substitute-data-sources"></a>Vervang gegevensbronnen

In de vorige stappen die u gebruikt de `201701-hubway-tripdata.csv` en `BostonWeather.csv` gegevensbronnen voor het voorbereiden van de testgegevens. Voor het gebruik van het pakket met de andere reis gegevensbestanden, gebruikt u de volgende stappen uit:

1. Maak een nieuwe **gegevensbron** met behulp van de stappen die eerder met de volgende wijzigingen aan het proces gegeven:

    * __Selectie van het bestand__: bij het selecteren van een bestand, meervoudige selectie zes resterende reis tripdata. CSV-bestanden.

        ![Laden van de zes overige bestanden](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > De __+ 5__ vermelding geeft aan dat er vijf extra bestanden buiten degene die wordt vermeld.

    * __Bestandsgegevens__: ingesteld __promoveren Headers modus__ naar **alle bestanden hebben dezelfde kopteksten**. Deze waarde geeft aan dat elk van de bestanden dezelfde kop bevat.

        ![Details van bestandsselectie](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   De naam van deze gegevensbron niet opslaan omdat het wordt gebruikt in latere stappen.

2. Selecteer het pictogram van de map om de bestanden in uw project weer te geven. Vouw de __aml\_config__ directory en selecteer vervolgens de `local.runconfig` bestand.

    ![Afbeelding van de locatie van local.runconfig](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. De volgende regels toevoegen aan het einde van de `local.runconfig` -bestand en selecteer vervolgens het pictogram van de schijf het bestand wilt opslaan.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Deze wijziging worden de oorspronkelijke gegevensbron vervangen door de database met de gegevensbestanden van de zes reis.

## <a name="save-training-data-as-a-csv-file"></a>Trainingsgegevens opslaan als een CSV-bestand

Ga naar het bestand Python `BikeShare Data Prep.py` dat u eerder bewerkt en geef een ander pad voor het opslaan van de trainingsgegevens.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Als u een nieuwe taak, gebruiken de **uitvoeren** pictogram aan de bovenkant van de pagina. Een **taak** wordt verzonden met de nieuwe configuratie. De uitvoer van deze taak is de trainingsgegevens. Deze gegevens worden gemaakt met dezelfde gegevens voorbereiden stappen die u eerder hebt gemaakt. Het duurt enkele minuten om de taak te voltooien.

## <a name="next-steps"></a>Volgende stappen
U kunt de fiets-share gegevens voorbereiden-zelfstudie hebt voltooid. In deze zelfstudie gebruikt u Azure Machine Learning-services (preview) voor meer informatie over hoe:
> [!div class="checklist"]
> * Voorbereiden van gegevens interactief met het hulpprogramma voor systeemvoorbereiding Azure Machine Learning-gegevens
> * Importeren, transformeren en een testgegevensset te maken
> * Genereren van een pakket gegevens voorbereiden
> * Voer de voorbereiding gegevenspakket met behulp van Python
> * Een gegevensset training door het pakket voorbereiden van gegevens voor aanvullende invoerbestanden opnieuw te genereren

Vervolgens meer informatie over het voorbereiden van gegevens:
> [!div class="nextstepaction"]
> [Gebruikershandleiding voor de voorbereiding](data-prep-user-guide.md)
