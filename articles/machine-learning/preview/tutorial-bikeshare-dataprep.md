---
title: Zelfstudie BikeShare - Geavanceerde gegevensvoorbereiding met Azure Machine Learning Workbench
description: Een zelfstudie voor end-to-end gegevensvoorbereiding met behulp van Azure Machine Learning Workbench
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc, tutorial, azure
ms.topic: tutorial
ms.date: 09/21/2017
ms.openlocfilehash: 69f6911a95be382b06313d984f09c7e85aec10df
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="bike-share-tutorial-advanced-data-preparation-with-azure-machine-learning-workbench"></a>Zelfstudie BikeShare: Geavanceerde gegevensvoorbereiding met Azure Machine Learning Workbench
Azure Machine Learning-services (preview) is een geïntegreerde, end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

In deze zelfstudie wordt uitgelegd hoe u Azure Machine Learning-services (preview) gebruikt om de volgende bewerkingen uit te voeren:
> [!div class="checklist"]
> * Gegevens interactief voorbereiden met het hulpprogramma voor gegevensvoorbereiding van Azure Machine Learning
> * Een testgegevensset importeren, transformeren en maken
> * Een gegevensvoorbereidingspakket genereren
> * Het gegevensvoorbereidingspakket uitvoeren met behulp van Python
> * Een trainingsgegevensset genereren door het gegevensvoorbereidingspakket opnieuw te gebruiken voor aanvullende invoerbestanden

> [!IMPORTANT]
> In deze zelfstudie worden alleen de gegevens voorbereid, er wordt geen voorspellingsmodel gemaakt.
>
> U kunt de voorbereide gegevens gebruiken om uw eigen voorspellingsmodellen te trainen. U kunt bijvoorbeeld een model maken om de vraag naar fietsen gedurende twee uur te voorspellen.

## <a name="prerequisites"></a>Vereisten
* Azure Machine Learning Workbench moet lokaal worden geïnstalleerd. Volg voor meer informatie de [snelstartgids voor installatie](quickstart-installation.md).
* Kennis van het maken van een nieuw project in Workbench.

## <a name="data-acquisition"></a>Gegevens ophalen
In deze zelfstudie wordt gebruikgemaakt van de [Boston Hubway-gegevensset](https://s3.amazonaws.com/hubway-data/index.html) en de Boston-weergegevens van [NOAA](http://www.noaa.gov/).

1. Download de gegevensbestanden via de volgende koppelingen in uw lokale ontwikkelomgeving. 
   * [Boston-weergegevens](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv). 
   * Hubway-reisgegevens op de Hubway-website.

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Pak de ZIP-bestanden na het downloaden uit.

## <a name="learn-about-the-datasets"></a>Informatie over de gegevenssets
1. Het bestand __Boston weather__ bevat de volgende weergerelateerde velden die op uurbasis zijn gerapporteerd:
   * DATE
   * REPORTTPYE
   * HOURLYDRYBULBTEMPF
   * HOURLYRelativeHumidity
   * HOURLYWindSpeed

2. De __Hubway__-gegevens zijn onderverdeeld in bestanden op jaar en maand. Bijvoorbeeld: het bestand met de naam `201501-hubway-tripdata.zip` bevat een CSV-bestand met gegevens voor januari 2015. De gegevens bevatten de volgende velden, waarbij elke rij een fietstocht weergeeft:

   * Duur van de tocht (in seconden)
   * Begintijd en -datum
   * Eindtijd en -datum
   * Naam en id van het beginstation
   * Naam en id van het eindstation
   * Fiets-id
   * Gebruikerstype (Informeel = Gebruiker van 24-uur of 72-uur pas. Lid = Jaarlijks of maandelijks lid)
   * Postcode (als de gebruiker lid is)
   * Geslacht (zelf opgegeven door lid)

## <a name="create-a-new-project"></a>Een nieuw project maken
1. Start de **Azure Machine Learning Workbench** vanuit het menu Start of het startprogramma.

2. Maak een nieuw Azure Machine Learning-project.  Klik op de knop **+** op de pagina **Projecten** of klik op **Bestand** > **Nieuw**.
   - Gebruik de sjabloon **Leeg project**.
   - Geef uw project de naam **BikeShare**. 

## <a id="newdatasource"></a>Een nieuwe gegevensbron maken

1. Maak een nieuwe gegevensbron. Klik op de knop **Gegevens** (cilinderpictogram) op de linkerwerkbalk. U ziet nu de **gegevensweergave**.

   ![Afbeelding van het tabblad Gegevensweergave](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Voeg een gegevensbron toe. Selecteer het pictogram **+** en selecteer vervolgens **Gegevensbron toevoegen**.

   ![Afbeelding van Gegevensbron toevoegen](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Weergegevens toevoegen

1. **Gegevensopslag**: selecteer het gegevensarchief dat de gegevens bevat. Omdat u bestanden gebruikt, selecteert u **Bestand(en)/Directory**. Selecteer **Volgende** om door te gaan.

   ![Afbeelding van Bestand(en)/Directory](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Bestandsselectie**: voeg de weergegevens toe. Blader en selecteer het bestand `BostonWeather.csv` dat u eerder hebt gedownload. Klik op **Volgende**.

   ![Afbeelding van de bestandsselectie waarbij BostonWeater.csv is geselecteerd](media/tutorial-bikeshare-dataprep/pickweatherdatafile.png)

3. **Bestandsgegevens**: controleer het bestandsschema dat wordt gedetecteerd. Azure Machine Learning Workbench analyseert de gegevens in het bestand en bepaalt welk schema moet worden gebruikt.

   ![Afbeelding van de bestandsgegevens](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > In sommige gevallen wordt niet het juiste schema gedetecteerd. U moet altijd controleren of de parameters juist zijn voor uw gegevensset. Controleer of de weergegevens zijn ingesteld op de volgende waarden:
   >
   > * __Bestandstype__: gescheiden bestand (csv, tsv, txt, enz.)
   > * __Scheidingsteken__: komma [,]
   > * __Teken voor opmerkingenregel__: er is geen waarde ingesteld.
   > * __Modus voor regels overslaan__: niet overslaan
   > * __Bestandscodering__: utf-8
   > * __Modus voor kopteksten verhogen__: kopteksten van het eerste bestand gebruiken

   In de preview van de gegevens moeten de volgende kolommen worden weergegeven:
   * **Pad**
   * **DATE**
   * **REPORTTYPE**
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

   Selecteer **Volgende** om door te gaan.

4. **Gegevenstypen**: controleer de gegevenstypen die automatisch worden gedetecteerd. Azure Machine Learning Workbench analyseert de gegevens in het bestand en bepaalt welke gegevenstypen moeten worden gebruikt.

   Voor deze gegevens wijzigt u de `DATA TYPE` van alle kolommen in `String`.

   > [!NOTE]
   > `String` wordt gebruikt om de mogelijkheden van Workbench verderop in deze zelfstudie te markeren. 

   ![Afbeelding van de gegevenstypen](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   Selecteer __Volgende__ om door te gaan. 

5. **Sampling**: voor het maken van een schema voor sampling selecteert u de knop **+ Nieuw**. Selecteer de nieuwe rij __Top 10000__ die is toegevoegd en selecteer vervolgens __Bewerken__. Stel __Sample Strategy__ (Samplingstrategie) in op **Volledig bestand** en selecteer vervolgens **Toepassen**.

   ![Afbeelding van het toevoegen van een nieuwe samplingstrategie](media/tutorial-bikeshare-dataprep/weatherdatasampling.png)

   Als u de strategie __Volledig bestand__ wilt gebruiken, selecteert u __Volledig bestand__ en selecteert u vervolgens __Instellen als actief__. Er wordt een ster weergegeven naast __Volledig bestand__ om aan te geven dat dit de actieve strategie is.

   ![Afbeelding van Volledig bestand als de actieve strategie](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Selecteer **Volgende** om door te gaan.

6. **Path Column** (Padkolom): in het gedeelte __Path Column__ (Padkolom) kunt u het volledige bestandspad als een kolom in de geïmporteerde gegevens opnemen. Selecteer __Do Not Include Path Column__ (Padkolom niet opnemen).

   > [!TIP]
   > Het pad opnemen als een kolom is handig als u een map importeert met veel bestanden met verschillende bestandsnamen. Het is ook handig als de bestandsnamen informatie bevatten die u later wilt uitpakken.

   ![Afbeelding waarin het padkolom niet is opgenomen](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Voltooien**: om het maken van de gegevensbron te voltooien, selecteert u de knop **Voltooien**.

    Er wordt een nieuw tabblad met de naam __BostonWeather__ geopend. Er wordt een voorbeeld van de gegevens in een rasterweergave weergegeven. Het voorbeeld is gebaseerd op het actieve samplingschema dat u eerder hebt opgegeven.

    In het deelvenster **Stappen** aan de rechterkant van het scherm ziet u de afzonderlijke acties die zijn uitgevoerd tijdens het maken van deze gegevensbron.

   ![Afbeelding van de gegevensbron, de sample en de stappen](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Metrische gegevens van gegevensbron weergeven

Selecteer de knop __Metrische gegevens__ links boven in de rasterweergave van het tabblad. Deze weergave bevat de distributie en andere samengevoegde statistische gegevens van de samplinggegevens.

![Afbeelding van de weergave van de metrische gegevens](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Gebruik voor het configureren van de zichtbaarheid van de statistieken de vervolgkeuzelijst **Choose Metric** (Metriek kiezen). Schakel metrische gegevens in en uit om de rasterweergave te wijzigen.

Om terug te keren naar de __gegevensweergave__, selecteert u __Gegevens__ in de linkerbovenhoek van de pagina.

## <a name="add-data-source-to-data-preparation-package"></a>Gegevensbron toevoegen aan het gegevensvoorbereidingspakket

1. Selecteer __Voorbereiden__ om te beginnen met het voorbereiden van de gegevens. 

2. Voer een naam in voor het gegevensvoorbereidingspakket, bijvoorbeeld `BikeShare Data Prep`. 

3. Selecteer __OK__ om door te gaan.

   ![Afbeelding van het dialoogvenster Voorbereiden](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Een nieuw pakket met de naam **BikeShare Data Prep** wordt weergegeven onder het gedeelte __Gegevensvoorbereiding__ van het tabblad __Gegevens__. 

   Als u het pakket wilt weergeven, selecteert u deze vermelding. 

5. Selecteer de knop **>>** om de __Gegevensstromen__ weer te geven die zijn opgenomen in het pakket. In dit voorbeeld is __BostonWeather__ de enige gegevensstroom.

   > [!IMPORTANT]
   > Een pakket kan meerdere gegevensstromen bevatten.

   ![Afbeelding van de gegevensstromen in het pakket](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Gegevens filteren op waarde
1. Als u gegevens wilt filteren, klikt u met de rechtermuisknop op een cel met een bepaalde waarde en selecteert u __Filteren__. Vervolgens selecteert u het type filter.

2. Selecteer voor deze zelfstudie een cel met de waarde `FM-15` en stel het filter in op een filter van **Is gelijk aan**.  De gegevens worden gefilterd en er worden alleen rijen weergegeven waarvoor __REPORTTYPE__ `FM-15` is.

   ![Afbeelding van het dialoogvenster Filter](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 is een type Meteorological Terminal Aviation Routine Weather Report (METAR). FM-15-rapporten worden empirisch als de meest volledige rapporten beschouwd, doordat slechts weinig gegevens ontbreken.

## <a name="remove-a-column"></a>Een kolom verwijderen

U hebt de kolom __REPORTTYPE__ niet meer nodig. Klik met de rechtermuisknop op de kolomkop en selecteer **Kolom verwijderen**.

   ![Afbeelding van de optie om een kolom te verwijderen](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Gegevenstypen wijzigen en fouten verwijderen
1. Als u drukt op __Ctrl (Command ⌘ op de Mac)__ terwijl u kolomkoppen selecteert, kunt u meerdere kolommen tegelijk selecteren. Doe dit om de volgende kolomkoppen te selecteren:
   * **HOURLYDRYBULBTEMPF**
   * **HOURLYRelativeHumidity**
   * **HOURLYWindSpeed**

2. **Klik met de rechtermuisknop** op een van de geselecteerde kolomkoppen en selecteer **Convert Field Type to Numeric** (Veldtype converteren naar numeriek). Hiermee zet u het gegevenstype voor de kolommen om in numerieke gegevens.

   ![Meerdere kolommen converteren naar numerieke gegevens](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filter de foutwaarden eruit. Sommige kolommen hebben conversieproblemen voor het gegevenstype. Dit probleem wordt aangegeven door de rode kleur in de __Data Quality Bar__ (gegevenskwaliteitbalk) voor de kolom.

   Als u de rijen wilt verwijderen die fouten bevatten, klikt u met de rechtermuisknop op de kolomkop **HOURLYDRYBULBTEMPF**. Selecteer **Kolom filteren**. Gebruik de standaardinstelling **Ik wil** voor **Keep Rows** (Rijen behouden). Wijzig de vervolgkeuzelijst **Voorwaarden** om **is not error** (is geen fout) te selecteren. Selecteer **OK** om het filter toe te passen.

4. Herhaal dit filterproces voor de kolommen **HOURLYRelativeHumidity** en **HOURLYWindSpeed** om de resterende foute rijen in de andere kolommen te verwijderen.

## <a name="use-by-example-transformations"></a>Voorbeeldtransformaties gebruiken

Als u de gegevens in een voorspelling voor tijdblokken van twee uur wilt gebruiken, moet u de gemiddelde weersomstandigheden voor perioden van twee uur berekenen. Hiervoor kunt u de volgende acties gebruiken:

* Splits de kolom **DATE** in de afzonderlijke kolommen **Date** en **Time**. Zie de volgende sectie voor gedetailleerde stappen.

* Leid een kolom **Hour_Range** af uit de kolom **Time**. Zie de volgende sectie voor gedetailleerde stappen.

* Leid een kolom **Date\_Hour\_Range** af uit de kolommen **DATE** en **Hour_Range**. Zie de volgende sectie voor gedetailleerde stappen.

### <a name="split-column-by-example"></a>Kolom splitsen per voorbeeld

1. Splits de kolom **DATE** in de afzonderlijke kolommen Date en Time. Klik met de rechtermuisknop op de kolomkop **DATE** en selecteer **Kolom splitsen per voorbeeld**.

   ![Afbeelding van het splitsen van een kolom per voorbeeld](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Azure Machine Learning Workbench identificeert automatisch een betekenisvol scheidingsteken en maakt twee kolommen door de gegevens te splitsen in datum- en tijdwaarden. 

3. Selecteer __OK__ om de resultaten van de splitsbewerking te accepteren.

   ![Afbeelding van de gesplitste kolommen DATE_1 en DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Kolom afleiden per voorbeeld

1. Als u een bereik van twee uur wilt afleiden, klikt u met de rechtermuisknop op de kolomkop __DATE\_2__ en selecteert u **Kolom afleiden per voorbeeld**.

   ![Afbeelding van het afleiden van een kolom per voorbeeld](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Er wordt een nieuwe lege kolom toegevoegd met null-waarden.

2. Klik in de nieuwe kolom in de eerste lege cel. Geef een voorbeeld van het gewenste tijdsbereik op door `12AM-2AM` te typen in de nieuwe kolom en druk vervolgens op Enter.

   ![Afbeelding van de nieuwe kolom met een waarde van 12AM-2AM](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Azure ML Workbench synthetiseert een programma op basis van de voorbeelden die u hebt opgegeven en past hetzelfde programma toe op de overige rijen. De overige rijen worden automatisch ingevuld op basis van het voorbeeld dat u hebt opgegeven. Workbench analyseert ook uw gegevens en probeert randgevallen te identificeren. 
  
3. De tekst **Gegevens analyseren** boven het raster geeft aan dat Workbench randgevallen probeert te identificeren. Zodra dit is voltooid, wordt de status gewijzigd in **Review next suggested row** (Volgende voorgestelde rij controleren) of **No suggestions** (Geen suggesties). In dit voorbeeld wordt **Review next suggested row** (Volgende voorgestelde rij controleren) geretourneerd.

4. Om de voorgestelde wijzigingen te bekijken selecteert u **Review next suggested row** (Volgende voorgestelde rij controleren). De cel die u moet controleren en corrigeren (indien nodig) wordt gemarkeerd.

   ![Afbeelding van een rij die moet worden gecontroleerd](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Selecteer __OK__ om de transformatie te accepteren.
 
5. U keert terug naar de rasterweergave van de gegevens voor __BostonWeather__. Het raster bevat nu de drie kolommen die u eerder hebt toegevoegd.

   ![Afbeelding van de rasterweergave met toegevoegde rijen](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   >  Alle wijzigingen die u hebt aangebracht, blijven behouden in het deelvenster **Stappen**. Ga naar de stap die u hebt gemaakt in het deelvenster **Stappen**, klik op de pijl omlaag en selecteer **Bewerken**. Het geavanceerde venster voor **Kolom afleiden per voorbeeld** wordt weergegeven. Alle voorbeelden blijven hier behouden. U kunt ook voorbeelden handmatig toevoegen door te dubbelklikken op een rij in het onderstaande raster. Selecteer **Annuleren** om terug te gaan naar het hoofdraster zonder de wijzigingen toe te passen. U kunt deze weergave ook openen door **Geavanceerde modus** te selecteren tijdens het uitvoeren van een transformatie van **Kolom afleiden per voorbeeld**.

6. Om de naam van kolom te wijzigen, dubbelklikt u op de kolomkop en typt u **Hour Range**. Druk op **Enter** om de wijziging op te slaan.

   ![De naam van een kolom wijzigen](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Om de datum en het uurbereik af te leiden, selecteert u de kolommen **Date\_1** en **Hour Range**, klikt u met de rechtermuisknop en selecteert u **Kolom afleiden per voorbeeld**.

   ![Kolom afleiden per voorbeeld](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Typ `Jan 01, 2015 12AM-2AM` als het voorbeeld op basis van de eerste rij en druk op **Enter**.

   Workbench bepaalt de transformatie op basis van het voorbeeld dat u opgeeft. In dit voorbeeld is de datumnotatie gewijzigd en samengevoegd met het tijdsbestek van twee uur.

   ![Afbeelding van het voorbeeld Jan 01, 2015 12AM-2AM](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)


8. Wacht totdat de status is gewijzigd van **Gegevens analyseren** naar **Review next suggested row** (Volgende voorgestelde rij controleren). Dit kan enkele seconden duren. Selecteer de statuskoppeling om te navigeren naar de voorgestelde rij. 

   ![Afbeelding van de voorgestelde rij die moet worden gecontroleerd](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   De rij heeft een null-waarde, omdat de waarde van de brondatum zowel dd/mm/jjjj als mm/dd/jjjj kan zijn. Typ de juiste waarde van `Jan 13, 2015 2AM-4AM` en druk op **Enter**. Workbench maakt gebruik van de twee voorbeelden om de afleiding voor de overige rijen te verbeteren.

   ![Afbeelding van juist opgemaakte gegevens](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Selecteer **OK** om de transformatie te accepteren.

   ![Afbeelding van het voltooide transformatieraster](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > U kunt de geavanceerde modus van **Kolom afleiden per voorbeeld** voor deze stap gebruiken door te klikken op de pijl omlaag in het deelvenster **Stappen**. In het gegevensraster staan selectievakjes naast de kolomnamen **DATE\_1** en **Hour Range**. Schakel het selectievakje uit naast de kolom **Hour Range** om te zien hoe de uitvoer verandert. Als de kolom **Hour Range** niet is gebruikt als invoer, wordt **12: 12AM-2AM** beschouwd als een constante en toegevoegd aan de afgeleide waarden. Selecteer **Annuleren** om terug te gaan naar het hoofdraster zonder de wijzigingen toe te passen.

10. Om de naam van de kolom te wijzigen, dubbelklikt u op de koptekst. Wijzig de naam in **Date Hour Range** en druk vervolgens op **Enter**.

11. Selecteer de kolommen **DATE**, **DATE\_1**, **DATE\_2** en **Hour Range**. Klik met de rechtermuisknop en selecteer vervolgens **Kolom verwijderen**.

## <a name="summarize-data-mean"></a>Gegevens samenvatten (gemiddelde)

De volgende stap is om de weersomstandigheden samen te vatten door het gemiddelde van de waarden te nemen, gegroepeerd op uurbereik.

1. Selecteer de kolom **Date Hour Range** en selecteer vervolgens **Samenvatten** in het menu **Transformatie**.

    ![Het menu Transformatie](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Om de gegevens samen te vatten, sleept u kolommen uit het raster onder in de pagina naar de linker- en rechterdeelvensters boven in de pagina. Het linkerdeelvenster bevat de tekst **Sleep kolommen hierheen om gegevens te groeperen**. Het rechterdeelvenster bevat de tekst **Sleep kolommen hierheen om gegevens samen te vatten**. 

    Sleep de kolom **Date Hour Range** uit het raster onder in de pagina naar het linkerdeelvenster. Sleep **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** en **HOURLYWindSpeed** naar het rechterdeelvenster. 

    Selecteer in het rechterdeelvenster **Gemiddelde** als de **aggregatie**meting voor elke kolom. Klik op **OK** om de samenvatting te voltooien.

   ![Afbeelding van het scherm met een samenvatting van de gegevens](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-using-script"></a>Gegevensstroom met script transformeren

Als u de gegevens in de numerieke kolommen wijzigt in een bereik van 0-1, kunnen sommige modellen snel worden geconvergeerd. Er is momenteel geen ingebouwde transformatie om deze transformatie generiek uit te voeren. U kunt hiervoor echter een Python-script gebruiken.

1. Selecteer **Gegevensstroom transformeren**in het menu **Transformatie**.

2. Voer de volgende code in het tekstvak dat wordt weergegeven. Als u de kolomnamen hebt gebruikt, werkt de code zonder dat u deze hoeft aan te passen. U schrijft een eenvoudige logica voor min-max in Python.

    > [!WARNING]
    > Het script verwacht de kolomnamen die eerder in deze zelfstudie zijn gebruikt. Als u andere kolomnamen hebt, moet u de namen in het script wijzigen.

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
    > Het Python-script moet `df` aan het einde retourneren. Deze waarde wordt gebruikt voor het vullen van het raster.
    
    ![Dialoogvenster voor het script voor het transformeren van de gegevensstroom](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Selecteer __OK__ om het script te gebruiken. De numerieke kolommen in het raster bevat nu de waarden in het bereik van 0-1.

    ![Raster met waarden tussen 0 en 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

U bent nu klaar met het voorbereiden van de weergegevens. Vervolgens bereidt u de gegevens van de fietstocht voor.

## <a name="load-trip-data"></a>Gegevens van de fietstocht laden

1. Voor het importeren van het bestand `201701-hubway-tripdata.csv` gebruikt u de stappen in het gedeelte [Een nieuwe gegevensbron maken](#newdatasource). Gebruik de volgende opties tijdens het importeren:

    * __Sampling scheme__ (Samplingschema): **Volledig bestand**, maak de sample actief en 
    * __Gegevenstype__: accepteer de standaardwaarden.

2. Nadat de gegevens zijn geïmporteerd, selecteert u de knop __Voorbereiden__ om de gegevens voor te bereiden. Selecteer het bestaande pakket **BikeShare Data Prep.dprep** en selecteer vervolgens __OK__.

    In dit proces voegt u een **gegevensstroom** toe aan het bestaande bestand **Gegevensvoorbereiding**. Er wordt dus geen nieuwe gegevensstroom gemaakt.

    ![Afbeelding van het selecteren van het bestaande pakket](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Zodra het raster is geladen, vouwt u __DATAFLOWS__ uit. Er zijn nu twee gegevensstromen: **BostonWeather** en **201701-hubway-tripdata**. Selecteer **201701-hubway-tripdata**.

    ![Afbeelding van 201701-hubway-tripdata](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-map-inspector"></a>De kaartinspector gebruiken

Er zijn verschillende handige visualisaties beschikbaar voor gegevensvoorbereiding, de zogenaamde **inspectors** voor tekenreeksen, numerieke gegevens en geografische gegevens waarmee u de gegevens beter kunt begrijpen en uitschieters kunt identificeren. Gebruik de volgende stappen uit om de kaartinspector te gebruiken:

1. Selecteer de kolommen **start station latitude** en **start station longitude**. Klik met de rechtermuisknop op een van de kolommen en selecteer vervolgens **Kaart**.

    > [!TIP]
    > Om meerdere kolommen te selecteren, houdt u __Ctrl (Command ⌘ op de Mac)__ ingedrukt en selecteert u de koptekst van elke kolom.

    ![Afbeelding van de kaartvisualisatie](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Als u de kaartvisualisatie wilt maximaliseren, selecteert u het pictogram **Maximaliseren**. Als u de kaart vullend in het venster wilt weergegeven, selecteert u het pictogram **E** linksboven in de visualisatie.

    ![Gemaximaliseerde weergave](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Klik op de knop **Minimaliseren** om terug te gaan naar de rasterweergave.

## <a name="use-column-statistics-inspector"></a>De inspector Kolomstatistieken gebruiken

Als u de inspector Kolomstatistieken wilt gebruiken, klikt u met de rechtermuisknop op de kolom __tripduration__ en selecteert u __Kolomstatistieken__.

![Kolomstatistieken](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

In dit proces voegt u een nieuwe visualisatie met de titel __tripduration Statistics__ toe aan het deelvenster __INSPECTORS__.

![Afbeelding van de inspector tripduration Statistics](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> De maximale waarde van de duur van de fietstocht is **961.814 minuten**, wat gelijk is aan ongeveer twee jaar. Er lijken enkele uitschieters in de gegevensset te zijn.

## <a name="use-histogram-inspector"></a>De inspector Histogram gebruiken

Om de uitschieters te identificeren, klikt u met de rechtermuisknop op de kolom __tripduration__ en selecteert u __Histogram__.

![De inspector Histogram](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Het histogram is niet echt bruikbaar, omdat de uitschieters het diagram vertekenen.

## <a name="add-column-using-script"></a>Een kolom met het script toevoegen

1. Klik met de rechtermuisknop op de kolom **tripduration** en selecteer **Kolom toevoegen (script)**.

    ![Afbeelding van het menu Kolom toevoegen (script)](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. Gebruik in het dialoogvenster __Kolom toevoegen (script)__ de volgende waarden:

    * __Nieuwe kolomnaam__: logtripduration
    * __Insert this New Column After__ (Deze kolom invoegen na): tripduration
    * __Nieuwe kolomcode__: `math.log(row.tripduration)`
    * __Code Bloktype__ (Code voor bloktype): Expressie

   ![Het dialoogvenster Kolom toevoegen (script)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Selecteer __OK__ om de kolom **logtripduration** toe te voegen.

4. Klik met de rechtermuisknop op de kolom en selecteer **Histogram**.

    ![Histogram van de kolom logtripduration](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

  Visueel lijkt deze kolom een normale distributie met een abnormale staart.

## <a name="use-advanced-filter"></a>Geavanceerd filter gebruiken

Als u een filter op de gegevens toepast, worden de inspectors met de nieuwe distributie bijgewerkt. Klik met de rechtermuisknop op de kolom **logtripduration** en selecteer **Kolom filteren**. Gebruik in het dialoogvenster __Bewerken__ de volgende waarden:

* __Filter this Number Column__ (Deze nummerkolom filteren): logtripduration
* __Ik wil__: Rijen behouden
* __Wanneer__: een van de onderstaande voorwaarden is waar (logische OF)
* __If this Column__ (Als deze kolom): kleiner dan
* __De waarde__: 9

![Filteropties](media/tutorial-bikeshare-dataprep/loftripfilter.png)

Selecteer __OK__ om het filter toe te passen.

![Bijgewerkte histogrammen nadat het filter is toegepast](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Het halo-effect

1. Maximaliseer het histogram **logtripduration**. Er wordt een blauw histogram over een grijs histogram weergegeven. Dit wordt het **halo-effect** genoemd:

    * Het **grijze histogram** geeft de distributie aan voordat de bewerking (in dit geval de filterbewerking) is toegepast.
    * Het **blauwe histogram** geeft het histogram na de bewerking aan. 

   Het halo-effect is handig voor de visualisatie van het effect van een bewerking op de gegevens.

   ![Afbeelding van het halo-effect](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > U ziet dat het blauwe histogram korter wordt weergegeven dan het vorige. Dit komt doordat de gegevens in het nieuwe bereik automatisch opnieuw worden verzameld.

2. Als u de halo wilt verwijderen, selecteert u __Bewerken__ en schakelt u __Show halo__ (Halo weergeven) uit.

    ![Opties voor het histogram](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Selecteer **OK** om het halo-effect uit te schakelen en het histogram te minimaliseren.

### <a name="remove-columns"></a>Kolommen verwijderen

In de reisgegevens staat elke rij voor een keer dat een fiets is opgehaald. Voor deze zelfstudie hebt u alleen de kolommen **starttime** en **start station id** nodig. Verwijder de andere kolommen door deze twee kolommen te selecteren, met de rechtermuisknop op de kolomkop te klikken en vervolgens **Keep Column** (Kolom behouden) te selecteren. Andere kolommen worden verwijderd.

![Afbeelding van de optie om kolommen te behouden](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Gegevens samenvatten (aantal)

Om de vraag naar fietsen gedurende een periode van twee uur samen te vatten, kunt u gebruikmaken van afgeleide kolommen.

1. Klik met de rechtermuisknop op de kolom **starttime** en selecteer **Kolom afleiden per voorbeeld**

    ![Afbeelding van de optie om kolommen af te leiden per voorbeeld](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Voer een waarde van `Jan 01, 2017 12AM-2AM` in voor de eerste rij.

    > [!IMPORTANT]
    > In het vorige voorbeeld van het afleiden van kolommen hebt u meerdere stappen uitgevoerd om een kolom af te leiden die de datum- en tijdsperiode bevatten. In dit voorbeeld ziet u dat deze bewerking kan worden uitgevoerd als één stap door een voorbeeld van de uiteindelijke uitvoer op te geven.

    > [!NOTE]
    > U kunt een voorbeeld van een van deze rijen opgeven. In dit voorbeeld is de waarde van `Jan 01, 2017 12AM-2AM` geldig voor de eerste rij gegevens.

    ![Afbeelding van de voorbeeldgegevens](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)
   
3. Wacht totdat de waarden worden berekend op basis van alle rijen. Dit kan enkele seconden duren. Als de analyse is voltooid, gebruikt u de koppeling __Review next suggested row__ (Volgende voorgestelde rij controleren) om de gegevens te controleren.

   ![Afbeelding van de voltooide analyse met de controlekoppeling](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Controleer of de berekende waarden correct zijn. Als dit niet het geval is, werkt u de waarde bij met de verwachte waarde en drukt u op Enter. Wacht totdat de analyse is voltooid. Voltooi het proces voor **Review next suggested row** (Volgende voorgestelde rij controleren) totdat **Geen suggesties** wordt weergegeven. Als **Geen suggesties** wordt weergegeven, zijn de randgevallen bekeken en goedgekeurd door het synchronisatieprogramma. U kunt de getransformeerde gegevens het best visueel inspecteren voordat u de transformatie accepteert. 

4. Selecteer **OK** om de transformatie te accepteren. Wijzig de naam van de zojuist gemaakte kolom in **Date Hour Range**.

    ![Afbeelding van de hernoemde kolom](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Klik met de rechtermuisknop op de kolomkop **starttime** en selecteer **Kolom verwijderen**.

6. Als u de gegevens wilt samenvatten, selecteert u __Samenvatten__ in het menu __Transformatie__. Om de transformatie te maken, moet u de volgende handelingen uitvoeren:

    * Sleep __Date Hour Range__ en __start station id__ naar het linkerdeelvenster (groeperen op).
    * Sleep __start station id__ naar het rechterdeelvenster (samenvatten).

   ![Een afbeelding van de samenvattingsopties](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Selecteer **OK** om het resultaat van de samenvatting te accepteren.

## <a name="join-dataflows"></a>Gegevensstromen koppelen

Om de weergegevens te koppelen met de gegevens van de fietstocht, voert u de volgende stappen uit:

1. Selecteer __Koppelen__  in het menu __Transformatie__.

2. __Tabellen__: Selecteer **BostonWeather** als de linkergegevensstroom en **201701-hubway-tripdata** als de rechtergegevensstroom. Selecteer **Volgende** om door te gaan.

    ![Afbeelding van de tabelselecties](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Sleutelkolommen__: Selecteer de kolom **Date Hour Range** in beide tabellen en selecteer vervolgens __Volgende__.

    ![Afbeelding van de koppeling van sleutelkolommen](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Koppelingstype__: Selecteer __Overeenkomende rijen__ als het koppelingstype en selecteer vervolgens __Voltooien__.

    ![Koppelingstype voor overeenkomende rijen](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Dit proces maakt een nieuwe gegevensstroom met de naam __Resultaat van de koppeling__.

## <a name="create-additional-features"></a>Extra functies maken

1. Om een kolom te maken die de dag van de week bevat, klikt u met de rechtermuisknop op de kolom **Date Hour Range** en selecteert u **Kolom afleiden per voorbeeld**. Gebruik een waarde van __Zo__ voor een datum op een zondag. Bijvoorbeeld: **Jan 01, 2017 12AM-2AM**. Druk op **Enter** en selecteer vervolgens **OK**. Wijzig de naam van deze kolom in __Weekday__.

    ![Afbeelding van het maken van een nieuwe kolom die de dag van de week bevat](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Om een kolom te maken die de tijdsperiode van een rij bevat, klikt u met de rechtermuisknop op de kolom **Date Hour Range** en selecteert u **Kolom afleiden per voorbeeld**. Gebruik een waarde van **12AM-2AM** voor de rij die **Jan 01, 2017 12AM-2AM** bevat. Druk op **Enter** en selecteer vervolgens **OK**. Wijzig de naam van deze kolom in **Period**.

    ![Afbeelding van de kolom Period](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Om de kolommen **Date Hour Range** en **rDate Hour Range** te verwijderen, drukt u op **Ctrl (Command ⌘ op de Mac)** en selecteert u elke kolomkop. Klik met de rechtermuisknop en selecteer vervolgens **Kolom verwijderen**.

## <a name="read-data-from-python"></a>Gegevens lezen van Python

U kunt een gegevensvoorbereidingspakket uitvoeren met Python of PySpark en het resultaat ophalen als een **gegevensframe**.

Om een Python-voorbeeldscript te genereren, klikt u met de rechtermuisknop op __BikeShare Data Prep__ en selecteert u __Generate Data Access Code File__ (Toegangscode voor gegevenstoegang genereren). Het Python-voorbeeldbestand wordt gemaakt in uw **projectmap** en wordt ook geladen op een tabblad in Workbench. Het volgende Python-script is een voorbeeld van de code die wordt gegenereerd:

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

Om de gegevensstroom **Resultaat van de koppeling** op te slaan als een CSV-bestand, moet u het script `BikeShare Data Prep.py` wijzigen. Werk het Python-script bij met de volgende code:

```python
from azureml.dataprep.package import run

# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTest.csv
df.to_csv('Your Test Data File Path here')
```

Selecteer **Uitvoeren** boven in het scherm. Het script wordt verzonden als een **taak** op de lokale computer. Zodra de status van de taak verandert in __Voltooid__, is het bestand geschreven naar de opgegeven locatie.

## <a name="substitute-data-sources"></a>Gegevensbronnen vervangen

In de vorige stappen hebt u de gegevensbronnen `201701-hubway-tripdata.csv` en `BostonWeather.csv` gebruikt voor de voorbereiding van de testgegevens. Om het pakket te gebruiken voor de overige gegevens van de fietstocht, voert u de volgende stappen uit:

1. Maak een nieuwe **gegevensbron** met behulp van de stappen hiervoor en breng hierbij de volgende wijzigingen aan:

    * __Bestandsselectie__: als u een bestand selecteert, selecteert u de zes overige CSV-bestanden met de gegevens van de fietstocht.

        ![De zes overige bestanden laden](media/tutorial-bikeshare-dataprep/selectsixfiles.png)

        > [!NOTE]
        > __+ 5__ geeft aan dat er vijf extra bestanden staan onder het vermelde bestand.

    * __Bestandsgegevens__: stel __Modus voor kopteksten verhogen__ in op **All Files Have The Same Headers** (Alle bestanden hebben dezelfde kopteksten). Deze waarde geeft aan dat elk bestand dezelfde koptekst bevat.

        ![Selectie van bestandsgegevens](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Sla de naam van deze gegevensbron op. Deze wordt namelijk later gebruikt.

2. Selecteer het pictogram van de map om de bestanden in uw project weer te geven. Vouw de map __aml\_config__ uit en selecteer vervolgens het bestand `local.runconfig`.

    ![Afbeelding van de locatie van local.runconfig](media/tutorial-bikeshare-dataprep/localrunconfig.png) 

3. Voeg de volgende regels aan het einde van het bestand `local.runconfig` toe en selecteer vervolgens het schijfpictogram om het bestand op te slaan.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Hiermee vervangt u de oorspronkelijke gegevensbron met de gegevensbron die de zes gegevensbestanden van de fietstocht bevat.

## <a name="save-training-data-as-a-csv-file"></a>Trainingsgegevens opslaan als een CSV-bestand

Ga naar het Python-bestand `BikeShare Data Prep.py` dat u eerder hebt bewerkt en geef een ander bestandspad op om de trainingsgegevens op te slaan.

```python
from azureml.dataprep.package import run
# dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
df = run('BikeShare Data Prep.dprep', dataflow_idx=2)

# Example file path: C:\\Users\\Jayaram\\BikeDataOut\\BikeShareTrain.csv
df.to_csv('Your Training Data File Path here')
```

Om een nieuwe taak te verzenden, gebruikt u het pictogram **Uitvoeren** boven in de pagina. Er wordt een **taak** verzonden met de nieuwe configuratie. De uitvoer van deze taak is de trainingsgegevens. Deze gegevens worden gemaakt met dezelfde stappen voor gegevensvoorbereiding die u eerder hebt gemaakt. Het duurt enkele minuten om de taak te voltooien.

## <a name="next-steps"></a>Volgende stappen
U kunt de zelfstudie Gegevensvoorbereiding voor BikeShare voltooid. In deze zelfstudie hebt u Azure Machine Learning-services (preview) gebruikt om de volgende bewerkingen uit te voeren:
> [!div class="checklist"]
> * Gegevens interactief voorbereiden met het hulpprogramma voor gegevensvoorbereiding van Azure Machine Learning
> * Een testgegevensset importeren, transformeren en maken
> * Een gegevensvoorbereidingspakket genereren
> * Het gegevensvoorbereidingspakket uitvoeren met behulp van Python
> * Een trainingsgegevensset genereren door het gegevensvoorbereidingspakket opnieuw te gebruiken voor aanvullende invoerbestanden

Meer informatie over het voorbereiden van gegevens vindt u hier:
> [!div class="nextstepaction"]
> [Data Preparation User Guide](data-prep-user-guide.md)
