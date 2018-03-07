---
title: Zelfstudie BikeShare - Geavanceerde gegevensvoorbereiding met Azure Machine Learning Workbench
description: In deze zelfstudie voert u een end-to-end gegevensvoorbereidingstaak uit met behulp van Azure Machine Learning Workbench
services: machine-learning
author: ranvijaykumar
ms.author: ranku
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: tutorial
ms.date: 09/21/2017
ms.openlocfilehash: ca7239fd3e31c7a6cfc6fb64e04afb376e01c190
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="tutorial-use-azure-machine-learning-workbench-for-advanced-data-preparation-bike-share-data"></a>Zelfstudie: Geavanceerde gegevensvoorbereiding (BikeShare-gegevens) met Azure Machine Learning Workbench
Azure Machine Learning (preview) is een geïntegreerde end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

In deze zelfstudie wordt uitgelegd hoe u Machine Learning (preview) gebruikt om de volgende bewerkingen uit te voeren:
> [!div class="checklist"]
> * Gegevens interactief voorbereiden met het hulpprogramma voor gegevensvoorbereiding van Azure Machine Learning.
> * Een testgegevensset importeren, transformeren en maken.
> * Een gegevensvoorbereidingspakket genereren.
> * Het gegevensvoorbereidingspakket uitvoeren met behulp van Python.
> * Een trainingsgegevensset genereren door het gegevensvoorbereidingspakket opnieuw te gebruiken voor aanvullende invoerbestanden.
> * Scripts uit te voeren in een lokaal Azure CLI-venster.
> * Scripts uit te voeren in een HDInsight-cloudomgeving.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een lokale installatie van Azure Machine Learning Workbench. Volg voor meer informatie de [Quick Start voor installatie](quickstart-installation.md).
* Als u Azure CLI niet hebt geïnstalleerd, volgt u de instructies voor het [installeren van de nieuwste versie van Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).
* Een [HDInsight Spark-cluster](how-to-create-dsvm-hdi.md#create-an-apache-spark-for-azure-hdinsight-cluster-in-azure-portal) dat is gemaakt in Azure.
* Een Azure Storage-account.
* Kennis van het maken van een nieuw project in Workbench.
* Het is geen vereiste, maar wel handig als [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) is geïnstalleerd zodat u de blobs in uw opslagaccount kunt uploaden, downloaden en bekijken.

## <a name="data-acquisition"></a>Gegevens ophalen
In deze zelfstudie wordt gebruikgemaakt van de [Boston-hubway-gegevensset](https://s3.amazonaws.com/hubway-data/index.html) en de Boston-weergegevens van [NOAA](http://www.noaa.gov/).

1. Download de gegevensbestanden via de volgende koppelingen in uw lokale ontwikkelomgeving:

   * [Boston-weergegevens](https://azuremluxcdnprod001.blob.core.windows.net/docs/azureml/bikeshare/BostonWeather.csv)

   * Hubway-reisgegevens op de hubway-website:

      - [201501-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201501-hubway-tripdata.zip)
      - [201504-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201504-hubway-tripdata.zip)
      - [201510-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201510-hubway-tripdata.zip)
      - [201601-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201601-hubway-tripdata.zip)
      - [201604-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201604-hubway-tripdata.zip)
      - [201610-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201610-hubway-tripdata.zip)
      - [201701-hubway-tripdata.zip](https://s3.amazonaws.com/hubway-data/201701-hubway-tripdata.zip)

2. Pak de ZIP-bestanden na het downloaden uit.

## <a name="upload-data-files-to-azure-blob-storage"></a>Gegevensbestanden uploaden naar Azure Blob-opslag
U kunt Azure Blob-opslag gebruiken voor het hosten van uw gegevensbestanden.

1. Gebruik hiervoor hetzelfde opslagaccount als het opslagaccount dat is gebruikt voor het HDInsight-cluster.

    ![HDInsight-clusteropslagaccount](media/tutorial-bikeshare-dataprep/hdinsightstorageaccount.png)

2. Maak een nieuwe container met de naam **gegevensbestanden** waarin u de **BikeShare**-gegevensbestanden kunt opslaan.

3. Upload de gegevensbestanden. Upload `BostonWeather.csv` naar een map met de naam `weather`. Upload de reisgegevensbestanden naar een map met de naam `tripdata`.

    ![Gegevensbestanden uploaden](media/tutorial-bikeshare-dataprep/azurestoragedatafile.png)

> [!TIP]
> U kunt ook Storage Explorer gebruiken voor het uploaden van blobs. Met dit hulpprogramma kunt u ook de inhoud bekijken van alle bestanden die worden gegenereerd in de zelfstudie.

## <a name="learn-about-the-datasets"></a>Informatie over de gegevenssets
1. Het bestand __Boston weather__ bevat de volgende weergerelateerde velden die op uurbasis zijn gerapporteerd:

   * **DATE**

   * **REPORTTPYE**

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

2. De __hubway__-gegevens zijn onderverdeeld in bestanden op jaar en maand. Bijvoorbeeld: het bestand met de naam `201501-hubway-tripdata.zip` bevat een CSV-bestand met gegevens voor januari 2015. De gegevens bevatten de volgende velden, waarbij elke rij een fietstocht weergeeft:

   * **Duur van de tocht (in seconden)**

   * **Begintijd en -datum**

   * **Eindtijd en -datum**

   * **Naam en id van het beginstation**

   * **Naam en id van het eindstation**

   * **Fiets-id**

   * **Gebruikerstype (Informeel = Gebruiker van 24-uur of 72-uur pas. Lid = Jaarlijks of maandelijks lid)**

   * **Postcode (als de gebruiker lid is)**

   * **Geslacht (zelf opgegeven door lid)**

## <a name="create-a-new-project"></a>Een nieuw project maken
1. Start **Azure Machine Learning Workbench** vanuit het menu Start of het startprogramma.

2. Maak een nieuw Machine Learning-project. Selecteer de knop **+** op de pagina **Projecten** of selecteer **Bestand** > **Nieuw**.

   * Gebruik de sjabloon **Bike Share**.

   * Geef uw project de naam **BikeShare**. 

## <a id="newdatasource"></a>Een nieuwe gegevensbron maken

1. Maak een nieuwe gegevensbron. Selecteer de knop **Gegevens** (cilinderpictogram) op de linkerwerkbalk om de weergave **Gegevens** weer te geven.

   ![Tabblad met weergave Gegevens](media/tutorial-bikeshare-dataprep/navigatetodatatab.png)

2. Voeg een gegevensbron toe. Selecteer het pictogram **+** en selecteer vervolgens **Gegevensbron toevoegen**.

   ![Optie Gegevensbron toevoegen](media/tutorial-bikeshare-dataprep/newdatasource.png)

## <a name="add-weather-data"></a>Weergegevens toevoegen

1. **Gegevensopslag**: selecteer het gegevensarchief dat de gegevens bevat. Omdat u bestanden gebruikt, selecteert u **Bestand(en)/Map**. Selecteer **Volgende** om door te gaan.

   ![Vermelding van Bestand(en)/Map](media/tutorial-bikeshare-dataprep/datasources.png)

2. **Bestandsselectie**: voeg de weergegevens toe. Blader en selecteer het bestand `BostonWeather.csv` dat u eerder naar Blob Storage hebt geüpload. Selecteer **Volgende**.

   ![Bestandsselectie waarbij BostonWeather.csv is geselecteerd](media/tutorial-bikeshare-dataprep/azureblobpickweatherdatafile.png)

3. **Bestandsgegevens**: controleer het bestandsschema dat wordt gedetecteerd. Machine Learning Workbench analyseert de gegevens in het bestand en bepaalt welk schema moet worden gebruikt.

   ![Bestandsgegevens controleren](media/tutorial-bikeshare-dataprep/fileparameters.png)

   > [!IMPORTANT]
   > In sommige gevallen wordt niet het juiste schema gedetecteerd. Controleer altijd of de parameters juist zijn voor uw gegevensset. Controleer of de weergegevens zijn ingesteld op de volgende waarden:
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

4. **Gegevenstypen**: controleer de gegevenstypen die automatisch worden gedetecteerd. Machine Learning Workbench analyseert de gegevens in het bestand en bepaalt welke gegevenstypen moeten worden gebruikt.

   a. Voor deze gegevens wijzigt u **DATA TYPE** (Gegevenstype) voor alle kolommen in **Tekenreeks**.

   > [!NOTE]
   > Tekenreeks wordt gebruikt om de mogelijkheden van Workbench verderop in deze zelfstudie te markeren. 

   ![Gegevenstypen controleren](media/tutorial-bikeshare-dataprep/datatypedetection.png)

   b. Selecteer __Volgende__ om door te gaan. 

5. **Sampling**: voor het maken van een schema voor sampling selecteert u **Bewerken**. Selecteer de nieuwe rij __Top 10000__ die is toegevoegd en selecteer vervolgens __Bewerken__. Stel __Sample Strategy__ (Samplingstrategie) in op **Volledig bestand** en selecteer vervolgens **Toepassen**.

   ![Een nieuwe samplingstrategie toevoegen](media/tutorial-bikeshare-dataprep/weatherdatasamplingfullfile.png)

   Als u de strategie __Volledig bestand__ wilt gebruiken, selecteert u de vermelding __Volledig bestand__ en selecteert u vervolgens __Instellen als actief__. Er wordt een ster weergegeven naast __Volledig bestand__ om aan te geven dat dit de actieve strategie is.

   ![Volledig bestand ingesteld als actieve strategie](media/tutorial-bikeshare-dataprep/fullfileactive.png)

   Selecteer **Volgende** om door te gaan.

6. **Path Column** (Padkolom): in de sectie __Path Column__ kunt u het volledige bestandspad als een kolom in de geïmporteerde gegevens opnemen. Selecteer __Do Not Include Path Column__ (Padkolom niet opnemen).

   > [!TIP]
   > Het pad opnemen als een kolom is handig als u een map importeert met veel bestanden met verschillende bestandsnamen. Het is ook handig als de bestandsnamen informatie bevatten die u later wilt uitpakken.

   ![Do Not Include Path Column (Padkolom niet opnemen) ingesteld](media/tutorial-bikeshare-dataprep/pathcolumn.png)

7. **Voltooien**: selecteer **Voltooien** om het maken van de gegevensbron te voltooien.

    Er wordt een nieuw tabblad met de naam __BostonWeather__ geopend. Er wordt een voorbeeld van de gegevens in een rasterweergave weergegeven. Het voorbeeld is gebaseerd op het actieve samplingschema dat u eerder hebt opgegeven.

    In het deelvenster **Stappen** aan de rechterkant van het scherm ziet u de afzonderlijke acties die zijn uitgevoerd tijdens het maken van deze gegevensbron.

   ![Gegevensbron, voorbeeld en stappen weergeven](media/tutorial-bikeshare-dataprep/weatherdataloaded.png)

### <a name="view-data-source-metrics"></a>Metrische gegevens van gegevensbron weergeven

Selecteer __Metrics__ (Metrische gegevens) in de linkerbovenhoek van de rasterweergave van het tabblad. Deze weergave bevat de distributie en andere samengevoegde statistische gegevens van de samplinggegevens.

![Metrische gegevens weergeven](media/tutorial-bikeshare-dataprep/weathermetrics.png)

> [!NOTE]
> Gebruik voor het configureren van de zichtbaarheid van de statistieken de vervolgkeuzelijst **Choose Metric** (Metriek kiezen). Schakel metrische gegevens in en uit om de rasterweergave te wijzigen.

Om terug te keren naar de weergave __Gegevens__, selecteert u __Gegevens__ in de linkerbovenhoek van de pagina.

## <a name="add-a-data-source-to-the-data-preparation-package"></a>Een gegevensbron toevoegen aan het gegevensvoorbereidingspakket

1. Selecteer __Voorbereiden__ om te beginnen met het voorbereiden van de gegevens. 

2. Voer een naam in voor het gegevensvoorbereidingspakket, bijvoorbeeld **BikeShare Data Prep** (BikeShare-gegevens voorbereiden). 

3. Selecteer __OK__ om door te gaan.

   ![Dialoogvenster Voorbereiden](media/tutorial-bikeshare-dataprep/dataprepdialog.png)

4. Een nieuw pakket met de naam **BikeShare Data Prep** wordt weergegeven onder de sectie __Gegevensvoorbereiding__ van het tabblad __Gegevens__. 

   Als u het pakket wilt weergeven, selecteert u deze vermelding. 

5. Selecteer de knop **>>** om __Gegevensstromen__ uit te vouwen en de gegevensstromen weer te geven die zijn opgenomen in het pakket. In dit voorbeeld is __BostonWeather__ de enige gegevensstroom.

   > [!IMPORTANT]
   > Een pakket kan meerdere gegevensstromen bevatten.

   ![Gegevensstromen in het pakket](media/tutorial-bikeshare-dataprep/weatherdataloadedingrid.png)

## <a name="filter-data-by-value"></a>Gegevens filteren op waarde
1. Als u gegevens wilt filteren, klikt u met de rechtermuisknop op een cel met een bepaalde waarde en selecteert u __Filteren__. Vervolgens selecteert u het type filter.

2. Selecteer een cel met de waarde `FM-15` voor deze zelfstudie. Stel het filter vervolgens in op **equals** (is gelijk aan).  De gegevens worden gefilterd en er worden alleen rijen weergegeven waarvoor __REPORTTYPE__ gelijk is aan `FM-15`.

   ![Dialoogvenster Filteren](media/tutorial-bikeshare-dataprep/weatherfilterinfm15.png)

   > [!NOTE]
   > FM-15 is een type Meteorological Terminal Aviation Routine-weerrapport (METAR). FM-15-rapporten worden empirisch als de meest volledige rapporten beschouwd, doordat slechts weinig gegevens ontbreken.

## <a name="remove-a-column"></a>Een kolom verwijderen

U hebt de kolom __REPORTTYPE__ niet meer nodig. Klik met de rechtermuisknop op de kolomkop en selecteer **Kolom verwijderen**.

   ![Optie Kolom verwijderen](media/tutorial-bikeshare-dataprep/weatherremovereporttype.png)

## <a name="change-datatypes-and-remove-errors"></a>Gegevenstypen wijzigen en fouten verwijderen
1. Als u op Ctrl (Command ⌘ op de Mac) drukt terwijl u kolomkoppen selecteert, kunt u meerdere kolommen tegelijk selecteren. Doe dit om de volgende kolomkoppen te selecteren:

   * **HOURLYDRYBULBTEMPF**

   * **HOURLYRelativeHumidity**

   * **HOURLYWindSpeed**

2. Klik met de rechtermuisknop op een van de geselecteerde kolomkoppen en selecteer **Convert Field Type to Numeric** (Veldtype converteren naar numeriek). Met deze optie zet u het gegevenstype voor de kolommen om in numerieke gegevens.

   ![Meerdere kolommen converteren naar numerieke gegevens](media/tutorial-bikeshare-dataprep/weatherconverttonumeric.png)

3. Filter de foutwaarden eruit. Sommige kolommen hebben conversieproblemen voor het gegevenstype. Dit probleem wordt aangegeven door de rode kleur in de __Data Quality Bar__ (gegevenskwaliteitbalk) voor de kolom.

   Als u de rijen wilt verwijderen die fouten bevatten, klikt u met de rechtermuisknop op de kolomkop **HOURLYDRYBULBTEMPF**. Selecteer **Kolom filteren**. Gebruik de standaardinstelling **Ik wil** voor **Keep Rows** (Rijen behouden). Wijzig de vervolgkeuzelijst **Voorwaarden** om **is not error** (is geen fout) te selecteren. Selecteer **OK** om het filter toe te passen.

   ![Foutwaarden filteren](media/tutorial-bikeshare-dataprep/filtererrorvalues.png)

4. Herhaal dit filterproces voor de kolommen **HOURLYRelativeHumidity** en **HOURLYWindSpeed** om de resterende foute rijen in de andere kolommen te verwijderen.

## <a name="use-by-example-transformations"></a>Voorbeeldtransformaties gebruiken

Als u de gegevens in een voorspelling voor tijdblokken van twee uur wilt gebruiken, moet u de gemiddelde weersomstandigheden voor perioden van twee uur berekenen. Voer de volgende acties uit:

* Splits de kolom **DATE** in de afzonderlijke kolommen **Date** en **Time**. Zie de volgende sectie voor gedetailleerde stappen.

* Leid een kolom **Hour_Range** af uit de kolom **Time**. Zie de volgende sectie voor gedetailleerde stappen.

* Leid een kolom **Date\_Hour\_Range** af uit de kolommen **DATE** en **Hour_Range**. Zie de volgende sectie voor gedetailleerde stappen.

### <a name="split-column-by-example"></a>Kolom splitsen per voorbeeld

1. Splits de kolom **DATE** in de afzonderlijke kolommen **Date** en **Time**. Klik met de rechtermuisknop op de kolomkop **DATE** en selecteer **Kolom splitsen per voorbeeld**.

   ![Vermelding van Kolom splitsen per voorbeeld](media/tutorial-bikeshare-dataprep/weathersplitcolumnbyexample.png)

2. Machine Learning Workbench identificeert automatisch een betekenisvol scheidingsteken en maakt twee kolommen door de gegevens te splitsen in datum- en tijdwaarden. 

3. Selecteer __OK__ om de resultaten van de splitsbewerking te accepteren.

   ![Gesplitste kolommen DATE_1 en DATE_2](media/tutorial-bikeshare-dataprep/weatherdatesplitted.png)

### <a name="derive-column-by-example"></a>Kolom afleiden per voorbeeld

1. Als u een bereik van twee uur wilt afleiden, klikt u met de rechtermuisknop op de kolomkop __DATE\_2__ en selecteert u **Kolom afleiden per voorbeeld**.

   ![Vermelding van Kolom afleiden per voorbeeld](media/tutorial-bikeshare-dataprep/weatherdate2range.png)

   Er wordt een nieuwe lege kolom toegevoegd met null-waarden.

2. Klik in de nieuwe kolom in de eerste lege cel. Geef een voorbeeld van het gewenste tijdsbereik op door **12AM-2AM** (12:00-14:00 uur) te typen in de nieuwe kolom en druk vervolgens op Enter.

   ![Nieuwe kolom met een waarde van 12AM-2AM](media/tutorial-bikeshare-dataprep/weathertimerangeexample.png)

   > [!NOTE]
   > Machine Learning Workbench synthetiseert een programma op basis van de voorbeelden die u hebt opgegeven en past hetzelfde programma toe op de overige rijen. De overige rijen worden automatisch ingevuld op basis van het voorbeeld dat u hebt opgegeven. Workbench analyseert ook uw gegevens en probeert randgevallen te identificeren. 

   > [!IMPORTANT]
   > Identificatie van de randgevallen werkt mogelijk niet op een Mac in de huidige versie van Workbench. Sla stap 3 en stap 4 over op een Mac. Selecteer in plaats daarvan __OK__ nadat alle rijen zijn ingevuld met de afgeleide waarden.
   
3. De tekst **Gegevens analyseren** boven het raster geeft aan dat Workbench randgevallen probeert te identificeren. Zodra dit is voltooid, wordt de status gewijzigd in **Review next suggested row** (Volgende voorgestelde rij controleren) of **No suggestions** (Geen suggesties). In dit voorbeeld wordt **Review next suggested row** (Volgende voorgestelde rij controleren) geretourneerd.

4. Om de voorgestelde wijzigingen te bekijken selecteert u **Review next suggested row** (Volgende voorgestelde rij controleren). De cel die u moet controleren en corrigeren (indien nodig) wordt gemarkeerd.

   ![Volgende voorgestelde rij controleren](media/tutorial-bikeshare-dataprep/weatherreviewnextsuggested.png)

    Selecteer __OK__ om de transformatie te accepteren.
 
5. U keert terug naar de rasterweergave van de gegevens voor __BostonWeather__. Het raster bevat nu de drie kolommen die u eerder hebt toegevoegd.

   ![Rasterweergave met toegevoegde rijen](media/tutorial-bikeshare-dataprep/timerangecomputed.png)

   > [!TIP]
   > Alle wijzigingen die u hebt aangebracht, blijven behouden in het deelvenster **Stappen**. Ga naar de stap die u hebt gemaakt in het deelvenster **Stappen**, selecteer de pijl omlaag en selecteer **Bewerken**. Het geavanceerde venster voor **Kolom afleiden per voorbeeld** wordt weergegeven. Alle voorbeelden blijven hier behouden. U kunt voorbeelden ook handmatig toevoegen door te dubbelklikken op een rij in het onderstaande raster. Selecteer **Annuleren** om terug te gaan naar het hoofdraster zonder de wijzigingen toe te passen. U kunt deze weergave ook openen door **Geavanceerde modus** te selecteren tijdens het uitvoeren van een transformatie van **Kolom afleiden per voorbeeld**.

6. Om de naam van kolom te wijzigen, dubbelklikt u op de kolomkop en typt u **Hour Range**. Selecteer Enter om de wijziging op te slaan.

   ![De kolomnaam wijzigen](media/tutorial-bikeshare-dataprep/weatherhourrangecolumnrename.png)

7. Om de datum en het uurbereik af te leiden, selecteert u de kolommen **Date\_1** en **Hour Range**, klikt u met de rechtermuisknop en selecteert u **Kolom afleiden per voorbeeld**.

   ![Kolom afleiden per voorbeeld](media/tutorial-bikeshare-dataprep/weatherderivedatehourrange.png)

   Typ **Jan 01, 2015 12AM-2AM** zoals in het voorbeeld op de eerste rij, en selecteer Enter.

   Workbench bepaalt de transformatie op basis van het voorbeeld dat u opgeeft. In dit voorbeeld is de datumnotatie gewijzigd en samengevoegd met het tijdsbestek van twee uur.

   ![Het voorbeeld Jan 01, 2015 12AM-2AM](media/tutorial-bikeshare-dataprep/wetherdatehourrangeexample.png)

   > [!IMPORTANT]
   > Op een Mac moet u de volgende stap uitvoeren in plaats van stap 8:
   >
   > * Ga naar de eerste cel met **Feb 01, 2015 12AM-2AM**. Dat moet rij 15 zijn. Wijzig de waarde in **Jan 02, 2015 12AM-2AM**, en selecteer Enter. 
   

8. Wacht totdat de status is gewijzigd van **Gegevens analyseren** naar **Review next suggested row** (Volgende voorgestelde rij controleren). Deze wijziging kan enkele seconden duren. Selecteer de statuskoppeling om naar de voorgestelde rij te navigeren. 

   ![Voorgestelde rij die moet worden gecontroleerd](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguate.png)

   De rij heeft een null-waarde, omdat de waarde van de brondatum zowel dd/mm/jjjj als mm/dd/jjjj kan zijn. Typ de juiste waarde, **Jan 13, 2015 2AM-4AM**, en selecteer Enter. Workbench maakt gebruik van de twee voorbeelden om de afleiding voor de overige rijen te verbeteren.

   ![Juist opgemaakte gegevens](media/tutorial-bikeshare-dataprep/wetherdatehourrangedisambiguated.png)

9. Selecteer **OK** om de transformatie te accepteren.

   ![Voltooid transformatieraster](media/tutorial-bikeshare-dataprep/weatherdatehourrangecomputed.png)

   > [!TIP]
   > Om de **Geavanceerde modus** van **Kolom afleiden per voorbeeld** voor deze stap te gebruiken, gaat u als volgt te werk. Selecteer de pijl omlaag in het deelvenster **Stappen**. In het gegevensraster staan selectievakjes naast de kolommen **DATE\_1** en **Hour Range**. Schakel het selectievakje naast de kolom **Hour Range** uit om te zien hoe de uitvoer verandert. Als de kolom **Hour Range** niet is gebruikt als invoer, wordt **12: 12AM-2AM** beschouwd als een constante en toegevoegd aan de afgeleide waarden. Selecteer **Annuleren** om terug te gaan naar het hoofdraster zonder de wijzigingen toe te passen.
   ![Geavanceerde modus](media/tutorial-bikeshare-dataprep/derivedcolumnadvancededitdeselectcolumn.png)

10. Om de naam van de kolom te wijzigen, dubbelklikt u op de koptekst. Wijzig de naam in **Date Hour Range** en selecteer vervolgens Enter.

11. Selecteer de kolommen **DATE**, **DATE\_1**, **DATE\_2** en **Hour Range**. Klik met de rechtermuisknop en selecteer vervolgens **Kolom verwijderen**.

## <a name="summarize-data-mean"></a>Gegevens samenvatten (gemiddelde)

De volgende stap is om de weersomstandigheden samen te vatten door het gemiddelde van de waarden te nemen, gegroepeerd op uurbereik.

1. Selecteer de kolom **Date Hour Range** en selecteer vervolgens **Samenvatten** in het menu **Transformatie**.

    ![Het menu Transformatie](media/tutorial-bikeshare-dataprep/weathersummarizemenu.png)

2. Om de gegevens samen te vatten, sleept u kolommen uit het raster onder aan de pagina naar de linker- en rechterdeelvensters boven aan de pagina. Het linkerdeelvenster bevat de tekst **Sleep kolommen hierheen om gegevens te groeperen**. Het rechterdeelvenster bevat de tekst **Sleep kolommen hierheen om gegevens samen te vatten**. 

    a. Sleep de kolom **Date Hour Range** uit het raster onder in de pagina naar het linkerdeelvenster. Sleep **HOURLYDRYBULBTEMPF**, **HOURLYRelativeHumidity** en **HOURLYWindSpeed** naar het rechterdeelvenster. 

    b. Selecteer in het rechterdeelvenster **Gemiddelde** als de **aggregatie**meting voor elke kolom. Selecteer **OK** om de samenvatting te voltooien.

    ![Scherm met samengevatte gegevens](media/tutorial-bikeshare-dataprep/weathersummarize.png)

## <a name="transform-dataflow-by-using-script"></a>Gegevensstroom met script transformeren

Als u de gegevens in de numerieke kolommen wijzigt in een bereik van 0 tot 1, kunnen sommige modellen snel worden geconvergeerd. Er is momenteel geen ingebouwde algemene functionaliteit voor deze transformatie. Gebruik een Python-script om deze bewerking uit te voeren.

1. Selecteer **Gegevensstroom transformeren (script)** in het menu **Transformatie**.

2. Voer de volgende code in in het tekstvak dat wordt weergegeven. Als u de kolomnamen hebt gebruikt, werkt de code zonder dat u deze hoeft aan te passen. U schrijft een eenvoudige logica voor min-max in Python.

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
    
   ![Dialoogvenster Gegevensstroom transformeren (script)](media/tutorial-bikeshare-dataprep/transformdataflowscript.png)

3. Selecteer __OK__ om het script te gebruiken. De numerieke kolommen in het raster bevatten nu waarden in het bereik van 0 tot 1.

    ![Raster met waarden tussen 0 en 1](media/tutorial-bikeshare-dataprep/datagridwithdecimals.png)

U bent nu klaar met het voorbereiden van de weergegevens. Vervolgens bereidt u de gegevens van de fietstocht voor.

## <a name="load-trip-data"></a>Gegevens van de fietstocht laden

1. Voor het importeren van het bestand `201701-hubway-tripdata.csv` gebruikt u de stappen in de sectie [Een nieuwe gegevensbron maken](#newdatasource). Gebruik de volgende opties tijdens het importeren:

    * __Bestandsselectie__: selecteer **Azure Blob** wanneer u bladert om het bestand te selecteren.

    * __Samplingschema__: selecteer het samplingschema **Volledig bestand** en activeer de sample.

    * __Gegevenstype__: accepteer de standaardwaarden.

2. Nadat u de gegevens hebt geïmporteerd, selecteert u __Voorbereiden__ om te beginnen met het voorbereiden van de gegevens. Selecteer het bestaande pakket **BikeShare Data Prep.dprep** en selecteer vervolgens __OK__.

    In dit proces voegt u een **gegevensstroom** toe aan het bestaande bestand **Gegevensvoorbereiding**. Er wordt dus geen nieuwe gegevensstroom gemaakt.

    ![Het bestaande pakket selecteren](media/tutorial-bikeshare-dataprep/addjandatatodprep.png)

3. Zodra het raster is geladen, vouwt u __DATAFLOWS__ uit. Er zijn nu twee gegevensstromen: **BostonWeather** en **201701-hubway-tripdata**. Selecteer **201701-hubway-tripdata**.

    ![Vermelding 201701-hubway-tripdata](media/tutorial-bikeshare-dataprep/twodfsindprep.png)

## <a name="use-the-map-inspector"></a>De kaartinspector gebruiken

Voor het voorbereiden van de gegevens zijn nuttige visualisaties (ook wel inspectors genoemd) beschikbaar voor tekenreeks-, numerieke en geografische gegevens. Ze helpen u de gegevens beter te begrijpen en uitbijters te identificeren. Voer deze stappen uit om de kaartinspector te gebruiken:

1. Selecteer de kolommen **start station latitude** en **start station longitude**. Klik met de rechtermuisknop op een van de kolommen en selecteer vervolgens **Kaart**.

    > [!TIP]
    > Om meerdere kolommen te selecteren, houdt u de Ctrl-toets (Command ⌘ op de Mac) ingedrukt en selecteert u de koptekst van elke kolom.

    ![Kaartvisualisatie](media/tutorial-bikeshare-dataprep/launchMapInspector.png)

2. Als u de kaartvisualisatie wilt maximaliseren, selecteert u het pictogram **Maximaliseren**. Als u de kaart vullend in het venster wilt weergegeven, selecteert u het pictogram **E** linksboven in de visualisatie.

    ![Gemaximaliseerde weergave](media/tutorial-bikeshare-dataprep/maximizedmap.png)

3. Selecteer de knop **Minimaliseren** om terug te gaan naar de rasterweergave.

## <a name="use-the-column-statistics-inspector"></a>De inspector Kolomstatistieken gebruiken

Als u de inspector Kolomstatistieken wilt gebruiken, klikt u met de rechtermuisknop op de kolom __tripduration__ en selecteert u __Kolomstatistieken__.

![Kolomstatistieken](media/tutorial-bikeshare-dataprep/tripdurationcolstats.png)

In dit proces voegt u een nieuwe visualisatie met de titel __tripduration Statistics__ toe aan het deelvenster __INSPECTORS__.

![De inspector tripduration Statistics](media/tutorial-bikeshare-dataprep/tripdurationcolstatsinwell.png)

> [!IMPORTANT]
> De maximumwaarde van de duur van de fietstocht is 961.814 minuten, wat gelijk is aan ongeveer twee jaar. Er lijken enkele uitschieters in de gegevensset te zijn.

## <a name="use-the-histogram-inspector"></a>De inspector Histogram gebruiken

Om uitbijters te identificeren, klikt u met de rechtermuisknop op de kolom __tripduration__ en selecteert u __Histogram__.

![De inspector Histogram](media/tutorial-bikeshare-dataprep/tripdurationhistogram.png)

Het histogram is niet echt bruikbaar, omdat de uitbijters het diagram vertekenen.

## <a name="add-a-column-by-using-script"></a>Een kolom toevoegen met behulp van een script

1. Klik met de rechtermuisknop op de kolom **tripduration** en selecteer **Kolom toevoegen (script)**.

    ![Menu Kolom toevoegen (script)](media/tutorial-bikeshare-dataprep/computecolscript.png)

2. Gebruik in het dialoogvenster __Kolom toevoegen (script)__ de volgende waarden:

    * __Nieuwe kolomnaam__: logtripduration

    * __Insert this New Column After__ (Deze kolom invoegen na): tripduration

    * __Nieuwe kolomcode__: `math.log(row.tripduration)`

    * __Code Bloktype__ (Code voor bloktype): Expressie

   ![Dialoogvenster Kolom toevoegen (script)](media/tutorial-bikeshare-dataprep/computecolscriptdialog.png)

3. Selecteer __OK__ om de kolom **logtripduration** toe te voegen.

4. Klik met de rechtermuisknop op de kolom en selecteer **Histogram**.

    ![Histogram van de kolom logtripduration](media/tutorial-bikeshare-dataprep/logtriphistogram.png)

    Visueel lijkt deze kolom een normale distributie met een abnormale staart.

## <a name="use-an-advanced-filter"></a>Een geavanceerd filter gebruiken

Als u een filter op de gegevens toepast, worden de inspectors met de nieuwe distributie bijgewerkt. 

1. Klik met de rechtermuisknop op de kolom **logtripduration** en selecteer **Kolom filteren**. 

2. Gebruik in het dialoogvenster __Bewerken__ de volgende waarden:

    * __Filter this Number Column__ (Deze nummerkolom filteren): logtripduration

    * __Ik wil__: Rijen behouden

    * __Wanneer__: een van de onderstaande voorwaarden is waar (logische OF)

    * __If this Column__ (Als deze kolom): kleiner dan

    * __De waarde__: 9

    ![Filteropties](media/tutorial-bikeshare-dataprep/loftripfilter.png)

3. Selecteer __OK__ om het filter toe te passen.

    ![Bijgewerkte histogrammen nadat het filter is toegepast](media/tutorial-bikeshare-dataprep/loftripfilteredinspector.png)

### <a name="the-halo-effect"></a>Het halo-effect

1. Maximaliseer het histogram **logtripduration**. Er wordt een blauw histogram over een grijs histogram weergegeven. Dit wordt het **halo-effect** genoemd:

    * Het grijze histogram geeft de distributie aan voordat de bewerking (in dit geval de filterbewerking) is toegepast.

    * Het blauwe histogram geeft het histogram na de bewerking aan. 

   Het halo-effect is handig voor de visualisatie van het effect van een bewerking op de gegevens.

   ![Halo-effect](media/tutorial-bikeshare-dataprep/loftripfilteredinspectormaximized.png)

    > [!NOTE]
    > Het blauwe histogram lijkt korter dan het vorige histogram. Dit komt doordat de gegevens in het nieuwe bereik automatisch opnieuw worden verzameld.

2. Als u de halo wilt verwijderen, selecteert u __Bewerken__ en schakelt u __Show halo__ (Halo weergeven) uit.

    ![Opties voor het histogram](media/tutorial-bikeshare-dataprep/uncheckhalo.png)

3. Selecteer **OK** om het halo-effect halo uit te schakelen. Minimaliseer het histogram.

### <a name="remove-columns"></a>Kolommen verwijderen

In de reisgegevens staat elke rij voor een keer dat een fiets is opgehaald. Voor deze zelfstudie hebt u alleen de kolommen **starttime** en **start station id** nodig. Verwijder de andere kolommen door deze twee kolommen te selecteren, met de rechtermuisknop op de kolomkop te klikken en vervolgens **Keep Column** (Kolom behouden) te selecteren. Andere kolommen worden verwijderd.

![Optie Kolom behouden](media/tutorial-bikeshare-dataprep/tripdatakeepcolumn.png)

### <a name="summarize-data-count"></a>Gegevens samenvatten (aantal)

Om de vraag naar fietsen gedurende een periode van twee uur samen te vatten, kunt u gebruikmaken van afgeleide kolommen.

1. Klik met de rechtermuisknop op de kolom **starttime** en selecteer **Kolom afleiden per voorbeeld**.

    ![Optie Kolom afleiden per voorbeeld](media/tutorial-bikeshare-dataprep/tripdataderivebyexample.png)

2. Voer de waarde **Jan 01, 2017 12AM-2AM** in voor de eerste rij in dit voorbeeld.

    > [!IMPORTANT]
    > In het vorige voorbeeld van het afleiden van kolommen hebt u meerdere stappen uitgevoerd om een kolom af te leiden die de datum- en tijdsperiode bevatten. In dit voorbeeld ziet u dat deze bewerking kan worden uitgevoerd als één stap door een voorbeeld van de uiteindelijke uitvoer op te geven.

    > [!NOTE]
    > U kunt een voorbeeld van een van deze rijen opgeven. In dit voorbeeld is de waarde **Jan 01, 2017 12AM-2AM** geldig voor de eerste rij gegevens.

    ![Voorbeeldgegevens](media/tutorial-bikeshare-dataprep/tripdataderivebyexamplefirstexample.png)

   > [!IMPORTANT]
   > Op een Mac voert u deze stap uit in plaats van stap 3:
   >
   > * Ga naar de eerste cel met **Jan 01, 2017 1AM-2AM**. Dat moet rij 14 zijn. Wijzig de waarde in **Jan 01, 2017 12AM-2AM**, en selecteer Enter. 

3. Wacht totdat de waarden worden berekend op basis van alle rijen. Dit proces kan enkele seconden duren. Als de analyse is voltooid, gebruikt u de koppeling __Review next suggested row__ (Volgende voorgestelde rij controleren) om de gegevens te controleren.

   ![Voltooide analyse met koppeling voor controle](media/tutorial-bikeshare-dataprep/tripdatabyexanalysiscomplete.png)

    Controleer of de berekende waarden correct zijn. Als dit niet het geval is, werkt u de waarde bij met de verwachte waarde en selecteert u Enter. Wacht totdat de analyse is voltooid. Voltooi het proces voor **Review next suggested row** (Volgende voorgestelde rij controleren) totdat **Geen suggesties** wordt weergegeven. Als **Geen suggesties** wordt weergegeven, zijn de randgevallen bekeken en goedgekeurd door het synchronisatieprogramma. U kunt de getransformeerde gegevens het best visueel inspecteren voordat u de transformatie accepteert. 

4. Selecteer **OK** om de transformatie te accepteren. Wijzig de naam van de zojuist gemaakte kolom in **Date Hour Range**.

    ![Naam van kolom gewijzigd](media/tutorial-bikeshare-dataprep/tripdatasummarize.png)

5. Klik met de rechtermuisknop op de kolomkop **starttime** en selecteer **Kolom verwijderen**.

6. Als u de gegevens wilt samenvatten, selecteert u __Samenvatten__ in het menu __Transformatie__. Om de transformatie te maken, moet u de volgende stappen uitvoeren:

    * Sleep __Date Hour Range__ en __start station id__ naar het linkerdeelvenster (**Groeperen op**).

    * Sleep __start station id__ naar het rechterdeelvenster (**Gegevens samenvatten**).

   ![Samenvattingsopties](media/tutorial-bikeshare-dataprep/tripdatacount.png)

7. Selecteer **OK** om het resultaat van de samenvatting te accepteren.

## <a name="join-dataflows"></a>Gegevensstromen koppelen

Om de weergegevens te koppelen met de gegevens van de fietstocht, voert u de volgende stappen uit:

1. Selecteer __Koppelen__ in het menu __Transformatie__.

2. __Tabellen__: Selecteer **BostonWeather** als de **linker**gegevensstroom en **201701-hubway-tripdata** als de **rechter**gegevensstroom. Selecteer **Volgende** om door te gaan.

    ![Selecties in tabellen](media/tutorial-bikeshare-dataprep/jointableselection.png)

3. __Sleutelkolommen__: Selecteer de kolom **Date Hour Range** in beide tabellen en selecteer vervolgens __Volgende__.

    ![Selecties in sleutelkolommen](media/tutorial-bikeshare-dataprep/joinkeyselection.png)

4. __Koppelingstype__: Selecteer __Overeenkomende rijen__ als het koppelingstype en selecteer vervolgens __Voltooien__.

    ![Koppelingstype voor overeenkomende rijen](media/tutorial-bikeshare-dataprep/joinscreen.png)

    Dit proces maakt een nieuwe gegevensstroom met de naam __Resultaat van de koppeling__.

## <a name="create-additional-features"></a>Extra functies maken

1. Om een kolom te maken die de dag van de week bevat, klikt u met de rechtermuisknop op de kolom **Date Hour Range** en selecteert u **Kolom afleiden per voorbeeld**. Gebruik een waarde van __Zo__ voor een datum op een zondag. Bijvoorbeeld: **Jan 01, 2017 12AM-2AM**. Selecteer Enter en vervolgens **OK**. Wijzig de naam van deze kolom in __Weekday__.

    ![Nieuwe kolom voor de dag van de week maken](media/tutorial-bikeshare-dataprep/featureweekday.png)

2. Om een kolom te maken die de tijdsperiode van een rij bevat, klikt u met de rechtermuisknop op de kolom **Date Hour Range** en selecteert u **Kolom afleiden per voorbeeld**. Gebruik de waarde **12AM-2AM** voor de rij die **Jan 01, 2017 12AM-2AM** bevat. Selecteer Enter en vervolgens **OK**. Wijzig de naam van deze kolom in **Period**.

    ![Kolom Period](media/tutorial-bikeshare-dataprep/featurehourrange.png)

3. Om de kolommen **Date Hour Range** en **r_Date Hour Range** te verwijderen, selecteert u Ctrl (Command ⌘ op de Mac) en selecteert u vervolgens elke kolomkop. Klik met de rechtermuisknop en selecteer **Kolom verwijderen**.

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

Om de gegevensstroom **Resultaat van de koppeling** op te slaan als een CSV-bestand, moet u het script `BikeShare Data Prep.py` wijzigen. 

1. Open het project voor bewerking in Visual Studio Code.

    ![Project geopend in Visual Studio Code](media/tutorial-bikeshare-dataprep/openprojectinvscode.png)

2. Werk het Python-script in het bestand `BikeShare Data Prep.py` bij met de volgende code:

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/testata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

3. Vervang `Your Azure Storage blob path` door het pad naar het uitvoerbestand dat u wilt maken. Voer de vervanging uit voor zowel de variabele `blobfolder` als `csvfiles`.

## <a name="create-an-hdinsight-run-configuration"></a>Een configuratie voor het uitvoeren van HDInsight maken

1. Open het opdrachtregelvenster in Machine Learning Workbench, selecteer het menu **File** en selecteer vervolgens **Open Command Prompt**. De opdrachtprompt wordt gestart in de projectmap met de prompt `C:\Projects\BikeShare>`.

    ![Opdrachtprompt geopend](media/tutorial-bikeshare-dataprep/opencommandprompt.png)

   >[!IMPORTANT]
   >U moet het opdrachtregelvenster (geopend vanuit Workbench) gebruiken om de volgende stappen uit te voeren.

2. Gebruik de opdrachtprompt om u aan te melden bij Azure. 

   De Workbench-app en de CLI gebruiken onafhankelijk referentiecaches voor verificatie bij Azure-resources. U hoeft dit slechts één keer te doen, totdat het token in de cache verloopt. Gebruik de opdracht `az account list` om een lijst met abonnementen op te vragen die u kunt gebruiken voor aanmelding. Als er meer dan één abonnement is, gebruikt u de id-waarde van het gewenste abonnement. Stel dit abonnement in als het standaardaccount voor gebruik met de opdracht `az account set -s` en geef vervolgens de abonnements-id op. Bevestig de instelling vervolgens met behulp van de accountopdracht `show`.

   ```azurecli
   REM login by using the aka.ms/devicelogin site
   az login
   
   REM lists all Azure subscriptions you have access to 
   az account list -o table
   
   REM sets the current Azure subscription to the one you want to use
   az account set -s <subscriptionId>
   
   REM verifies that your current subscription is set correctly
   az account show
   ```

3. Maak de configuratie voor het uitvoeren van HDInsight. U hebt de naam van het cluster en het wachtwoord voor `sshuser` nodig.

    ```azurecli
    az ml computetarget attach --name hdinsight --address <yourclustername>.azurehdinsight.net --username sshuser --password <your password> --type cluster
    az ml experiment prepare -c hdinsight
    ```
> [!NOTE]
> Wanneer u een leeg project maakt, zijn **local** en **docker** de standaarduitvoeringsconfiguraties. In deze stap maakt u een nieuwe uitvoeringsconfiguratie die beschikbaar is in Workbench wanneer u uw scripts uitvoert. 

## <a name="run-in-an-hdinsight-cluster"></a>Uitvoeren in een nieuw HDInsight-cluster

Ga terug naar de Machine Learning Workbench-toepassing om uw script uit te voeren in het HDInsight-cluster.

1. Ga terug naar het startscherm van uw project door het pictogram **Start** aan de linkerkant te selecteren.

2. Selecteer **hdinsight** in de vervolgkeuzelijst om uw script uit te voeren in het HDInsight-cluster.

3. Selecteer **Uitvoeren**. Het script wordt verzonden als een taak. De status van de taak verandert in __Voltooid__ als het bestand naar de opgegeven locatie in de opslagcontainer is geschreven.

    ![HDInsight-script uitgevoerd](media/tutorial-bikeshare-dataprep/hdinsightrunscript.png)


## <a name="substitute-data-sources"></a>Gegevensbronnen vervangen

In de vorige stappen hebt u de gegevensbronnen `201701-hubway-tripdata.csv` en `BostonWeather.csv` gebruikt voor de voorbereiding van de testgegevens. Om het pakket te gebruiken voor de overige gegevens van de fietstocht, voert u de volgende stappen uit:

1. Maak een nieuwe gegevensbron met behulp van de stappen hiervoor en breng hierbij de volgende wijzigingen aan:

    * __Bestandsselectie__: als u een bestand selecteert, selecteert u de zes overige CSV-bestanden met de gegevens van de fietstocht.

    ![Zes overige bestanden laden](media/tutorial-bikeshare-dataprep/browseazurestoragefortripdatafiles.png)

    > [!NOTE]
     > __+ 5__ geeft aan dat er vijf extra bestanden staan onder het vermelde bestand.

    * __Bestandsgegevens__: stel __Modus voor kopteksten verhogen__ in op **All Files Have The Same Headers** (Alle bestanden hebben dezelfde kopteksten). Deze waarde geeft aan dat elk bestand dezelfde koptekst bevat.

    ![Selectie van bestandsgegevens](media/tutorial-bikeshare-dataprep/headerfromeachfile.png) 

   Sla de naam van deze gegevensbron op. Deze wordt namelijk later gebruikt.

2. Selecteer het pictogram van de map om de bestanden in uw project weer te geven. Vouw de map __aml\_config__ uit en selecteer vervolgens het bestand `hdinsight.runconfig`.

    ![Locatie van hdinsight.runconfig](media/tutorial-bikeshare-dataprep/hdinsightsubstitutedatasources.png) 

3. Selecteer de knop **Bewerken** om het bestand te openen in Visual Studio Code.

4. Voeg de volgende regels toe aan het einde van het bestand `hdinsight.runconfig`, en selecteer vervolgens het schijfpictogram om het bestand op te slaan.

    ```yaml
    DataSourceSubstitutions:
      201701-hubway-tripdata.dsource: 201501-hubway-tripdata.dsource
    ```

    Hiermee vervangt u de oorspronkelijke gegevensbron met de gegevensbron die de zes gegevensbestanden van de fietstocht bevat.

## <a name="save-training-data-as-a-csv-file"></a>Trainingsgegevens opslaan als een CSV-bestand

1. Blader naar het Python-bestand `BikeShare Data Prep.py` dat u eerder hebt bewerkt. Geef een ander bestandspad op voor het opslaan van de trainingsgegevens.

    ```python
    import pyspark

    from azureml.dataprep.package import run
    from pyspark.sql.functions import *

    # start Spark session
    spark = pyspark.sql.SparkSession.builder.appName('BikeShare').getOrCreate()

    # dataflow_idx=2 sets the dataflow to the 3rd dataflow (the index starts at 0), the Join Result.
    df = run('BikeShare Data Prep.dprep', dataflow_idx=2)
    df.show(n=10)
    row_count_first = df.count()

    # Example file name: 'wasb://data-files@bikesharestorage.blob.core.windows.net/traindata'
    # 'wasb://<your container name>@<your azure storage name>.blob.core.windows.net/<csv folder name>
    blobfolder = 'Your Azure Storage blob path'

    df.write.csv(blobfolder, mode='overwrite') 

    # retrieve csv file parts into one data frame
    csvfiles = "<Your Azure Storage blob path>/*.csv"
    df = spark.read.option("header", "false").csv(csvfiles)
    row_count_result = df.count()
    print(row_count_result)
    if (row_count_first == row_count_result):
        print('counts match')
    else:
        print('counts do not match')
    print('done')
    ```

2. Gebruik de map met de naam `traindata` voor de trainingsgegevensuitvoer.

3. Als u een nieuwe taak wilt verzenden, selecteert u **Uitvoeren**. Zorg ervoor dat **hdinsight** is geselecteerd. Er wordt een taak verzonden met de nieuwe configuratie. De uitvoer van deze taak is de trainingsgegevens. Deze gegevens worden gemaakt met dezelfde stappen voor gegevensvoorbereiding die u eerder hebt uitgevoerd. Het uitvoeren van de taak kan enkele minuten duren.


## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen
U hebt de zelfstudie Gegevensvoorbereiding voor BikeShare voltooid. In deze zelfstudie hebt u Machine Learning (preview) gebruikt om de volgende bewerkingen uit te voeren:
> [!div class="checklist"]
> * Gegevens interactief voorbereiden met het hulpprogramma voor gegevensvoorbereiding van Azure Machine Learning.
> * Een testgegevensset importeren, transformeren en maken.
> * Een gegevensvoorbereidingspakket genereren.
> * Het gegevensvoorbereidingspakket uitvoeren met behulp van Python.
> * Een trainingsgegevensset genereren door het gegevensvoorbereidingspakket opnieuw te gebruiken voor aanvullende invoerbestanden.

Meer informatie over het voorbereiden van gegevens vindt u hier:
> [!div class="nextstepaction"]
> [Data Preparation User Guide](data-prep-user-guide.md)
