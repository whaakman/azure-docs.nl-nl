---
title: Aan de slag met de programmeertaal R
titleSuffix: Azure Machine Learning Studio
description: Gebruik deze programmeren R-zelfstudie aan de slag met behulp van de R-taal met Azure Machine Learning Studio om een prognose-oplossing te maken.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/01/2019
ms.openlocfilehash: c18de2efff0f867213cad56b6012bb8ee6ed52bb
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57344797"
---
# <a name="getting-started-with-the-r-programming-language-in-azure-machine-learning-studio"></a>Aan de slag met de programmeertaal in Azure Machine Learning Studio R

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Inleiding

Deze zelfstudie helpt u Azure Machine Learning Studio uitgebreid met behulp van de R-programmeertaal. Volg deze programmeren R-zelfstudie voor het maken, testen en uitvoeren van R-code in Studio. Als u de zelfstudie doorloopt, maakt u een volledige prognose-oplossing met behulp van de R-taal in Studio.  

Microsoft Azure Machine Learning Studio bevat veel krachtige machine learning en manipulatie modules. De krachtige R-taal heeft als lingua franca van analytics zijn beschreven. Analyse- en manipuleren in Studio kan worden uitgebreid met behulp van R. Deze combinatie biedt van de schaalbaarheid en een eenvoudige implementatie van Studio met de flexibiliteit en diepgaande analyse van R.

### <a name="forecasting-and-the-dataset"></a>Maken van prognoses en de gegevensset

Prognose is een analysemethode veel werknemers en zeer nuttig. Algemene maakt gebruik van het bereik van het voorspellen van de verkoop van seizoensgebonden items, waarmee wordt bepaald optimale inventarisniveaus, om te voorspellen macro-economische variabelen. Prognose gebeurt meestal met time series-modellen.

Time series-gegevens zijn gegevens waarmee de waarden een tijdsindex hebt. De tijdsindex worden regelmatig, bijvoorbeeld elke maand of elke minuut of onregelmatig. Een time series-model is gebaseerd op time series-gegevens. De programmeertaal R bevat een flexibele framework en de uitgebreide analyse voor time series-gegevens.

In deze handleiding wordt worden werken met Californië zuivelproductie en gegevens over prijzen. Deze gegevens omvatten maandelijkse informatie op de productie van verschillende producten en de prijs van melkvet, een benchmark-product.

De gegevens die worden gebruikt in dit artikel, samen met R-scripts, kan worden gedownload vanaf [MachineLearningSamples-notitieblokken/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples). Gegevens in het bestand `cadairydata.csv` oorspronkelijk is gemaakt van gegevens op de universiteit van Wisconsin op [ https://dairymarkets.com ](https://dairymarkets.com).

### <a name="organization"></a>Organisatie

We zullen navigatie door de verschillende stappen als u meer informatie over het maken, testen en analyse- en bewerken van R-code uitvoeren in de Azure Machine Learning Studio-omgeving.  

* Eerst zullen we de basisprincipes van het gebruik van de R-taal in de Azure Machine Learning Studio-omgeving verkennen.
* Vervolgens voortgang we bespreken van verschillende aspecten van i/o voor gegevens, R-code en afbeeldingen in de Azure Machine Learning Studio-omgeving.
* We wordt vervolgens het eerste deel van onze prognoseoplossing maken door het maken van code voor het opschonen van gegevens en -transformaties.
* Met onze voorbereid gaat een analyse van de correlatie tussen verschillende van de variabelen in onze gegevensset uitvoeren.
* Ten slotte maken we een prognoses seizoensgebonden time series-model voor.

## <a id="mlstudio"></a>Communiceren met de programmeertaal R in Machine Learning Studio

In deze sectie doorloopt u enkele basisprincipes van de interactie met de programmeertaal R in de Machine Learning Studio-omgeving. De R-taal biedt een krachtig hulpprogramma voor het maken van aangepaste analyses en gegevens manipuleren modules in de Azure Machine Learning Studio-omgeving.

Ik zal RStudio gebruiken om te ontwikkelen, testen en fouten opsporen in R-code op kleine schaal. Deze code wordt vervolgens knippen en plakken in een [R-Script uitvoeren] [ execute-r-script] module in Machine Learning Studio kan worden uitgevoerd.  

### <a name="the-execute-r-script-module"></a>De module R-Script uitvoeren

In Machine Learning Studio, R-scripts worden uitgevoerd binnen de [R-Script uitvoeren] [ execute-r-script] module. Een voorbeeld van de [R-Script uitvoeren] [ execute-r-script] module in Machine Learning Studio wordt weergegeven in afbeelding 1.

 ![R-programmeertaal: De R-Script uitvoeren-module in Machine Learning Studio geselecteerd](./media/r-quickstart/fig1.png)

*Afbeelding 1. De Machine Learning Studio-omgeving met de R-Script uitvoeren-module die is geselecteerd.*

Verwijzen naar afbeelding 1, laten we kijken enkele van de belangrijkste onderdelen van de Machine Learning Studio-omgeving voor het werken met de [R-Script uitvoeren] [ execute-r-script] module.

* De modules in het experiment worden weergegeven in het middelste deelvenster.
* Het bovenste gedeelte van het rechterdeelvenster bevat een venster wilt weergeven en bewerken van uw R-scripts.  
* Het onderste gedeelte van het rechter deelvenster ziet u enkele eigenschappen van de [R-Script uitvoeren][execute-r-script]. U kunt Logboeken van de fout en uitvoer weergeven door de juiste punten van dit deelvenster.

We zullen natuurlijk worden besproken in hoeverre de [R-Script uitvoeren] [ execute-r-script] in meer detail in de rest van dit artikel.

Als u werkt met complexe R-functies, ik u aan dat u bewerken, testen en fouten in RStudio opsporen. Net als bij de ontwikkeling van alle software, incrementeel uitbreiden van uw code en deze testen op kleine simpele testcases. Vervolgens knippen en plakken van uw functies in de R-script-venster van de [R-Script uitvoeren] [ execute-r-script] module. Deze aanpak kunt u gebruikmaken van zowel de RStudio integrated development environment (IDE) en de kracht van Azure Machine Learning Studio.  

#### <a name="execute-r-code"></a>R-code uitvoeren

Alle R-code in de [R-Script uitvoeren] [ execute-r-script] module worden uitgevoerd wanneer u het experiment door het selecteren van uitvoeren de **uitvoeren** knop. Wanneer de uitvoering is voltooid, een vinkje wordt weergegeven op de [R-Script uitvoeren] [ execute-r-script] pictogram.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Defensieve R coderen voor Azure Machine Learning

Als u R-code voor bijvoorbeeld een web-service ontwikkelt met behulp van Azure Machine Learning Studio, moet u zeker plannen hoe uw code wordt omgaan met een onverwachte invoer en uitzonderingen. Als u wilt behouden duidelijkheid, ik niet opgenomen veel heeft op het gebied controleren of de afhandeling van uitzonderingen in de meeste van de codevoorbeelden die wordt weergegeven. Echter, als we gaan ik krijgt u enkele voorbeelden van functies met behulp van de mogelijkheid voor het verwerken van de R-uitzondering.  

Als u nodig hebt voor een volledigere behandeling van de afhandeling van uitzonderingen R, ik raadzaam te lezen van de betreffende gedeelten van het boek door Wickham in onderstaande [meer weten](#appendixb).

#### <a name="debug-and-test-r-in-machine-learning-studio"></a>Fouten opsporen en testen van R in Machine Learning Studio

Samenvattend: ik u aan u testen en fouten opsporen in uw R-code op kleine schaal in RStudio. Er zijn echter gevallen waarin u wordt voor het opsporen van problemen met R-code in de [R-Script uitvoeren] [ execute-r-script] zelf. Bovendien is het verstandig om te controleren of de resultaten in Machine Learning Studio.

Uitvoer van de uitvoering van uw R-code en op het platform van Azure Machine Learning Studio is voornamelijk in uitvoer.log gevonden. Aanvullende informatie is zichtbaar in error.log.  

Als er een fout optreedt in Machine Learning Studio tijdens het uitvoeren van uw R-code, moet de eerste cursus van actie om te kijken naar error.log. Dit bestand kan handig zijn foutberichten om te begrijpen en corrigeer de fout bevatten. Als u wilt error.log weergeven, selecteert u **foutenlogboek weergeven** op de **deelvenster met eigenschappen** voor de [R-Script uitvoeren] [ execute-r-script] met de fout.

Bijvoorbeeld: ik de volgende R-code wordt uitgevoerd met een niet-gedefinieerde variabele y, in een [R-Script uitvoeren] [ execute-r-script] module:

```R
x <- 1.0
z <- x + y
```

Deze code niet kan worden uitgevoerd, wat resulteert in een foutstatus. Selecteren **foutenlogboek weergeven** op de **eigenschappendeelvenster** produceert de weergave wordt weergegeven in afbeelding 2.

  ![Pop-upvenster foutbericht](./media/r-quickstart/fig2.png)

*Afbeelding 2. Foutbericht pop-upvenster.*

Het lijkt, moeten we zoeken in uitvoer.log om de R-foutbericht te zien. Selecteer de [R-Script uitvoeren] [ execute-r-script] en selecteer vervolgens de **uitvoer.log weergeven** item op de **deelvenster met eigenschappen** aan de rechterkant. Hiermee opent u een nieuw browservenster en ik Zie de volgende.

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Dit bericht bevat geen verrassingen en duidelijk identificeert het probleem.

Als u wilt controleren van de waarde van een object in R, kunt u deze waarden naar het bestand uitvoer.log afdrukken. De regels voor het onderzoeken van objectwaarden zijn in wezen hetzelfde als in een interactieve R-sessie. Bijvoorbeeld, als u de naam van een variabele op een regel typt, dan worden de waarde van het object naar het bestand uitvoer.log afgedrukt.  

#### <a name="packages-in-machine-learning-studio"></a>Pakketten in Machine Learning Studio

Studio wordt gekenmerkt door meer dan 350 pakketten voor vooraf geïnstalleerde R-taal. U kunt de volgende code in de [R-Script uitvoeren] [ execute-r-script] module voor het ophalen van een lijst van de vooraf geïnstalleerde pakketten.

```R
data.set <- data.frame(installed.packages())
maml.mapOutputPort("data.set")
```

Als u niet weet de laatste regel van deze code op dit moment wat, lees dan verder. In de rest van dit artikel wordt uitvoerig besproken R gebruiken in de Studio-omgeving.

### <a name="introduction-to-rstudio"></a>Inleiding tot RStudio

RStudio is een veel gebruikte IDE voor R. Ik zal RStudio gebruiken voor het bewerken, testen en foutopsporing in enkele van de R-code die wordt gebruikt in deze handleiding. Zodra de R-code getest en klaar zijn is, u kunt gewoon knippen en plakken in de RStudio-editor naar een Machine Learning Studio [R-Script uitvoeren] [ execute-r-script] module.  

Als u de programmeertaal R op uw computer is geïnstalleerd hebt, raden ik dat u dit nu doen. Gratis downloads van open-sourcetaal R zijn beschikbaar op de uitgebreide R Archive Network (CRAN) op [ http://www.r-project.org/ ](http://www.r-project.org/). Er zijn downloads beschikbaar voor Windows, Mac OS en Linux/UNIX. Kies in de buurt mirror en volg de aanwijzingen downloaden. CRAN bevat bovendien een schat aan handig pakketten voor analyse- en manipuleren.

Als u niet bekend bent met RStudio, moet u downloadt en installeert de desktopversie. U vindt de RStudio-downloads voor Windows, Mac OS en Linux/UNIX op http://www.rstudio.com/products/RStudio/. Volg de instructies voor het installeren van RStudio op uw computer.  

Een inleiding van zelfstudie rstudio is beschikbaar op [met behulp van de RStudio IDE](https://support.rstudio.com/hc/sections/200107586-Using-RStudio).

Ik geef aanvullende informatie over het gebruik van RStudio in [handleiding RStudio documentatie](#appendixa) hieronder.  

## <a id="scriptmodule"></a>Gegevens vanuit de R-Script uitvoeren-module ophalen

In deze sectie bespreken we hoe u gegevens ophalen naar en van de [R-Script uitvoeren] [ execute-r-script] module. Tenslotte leest hoe u voor het afhandelen van verschillende soorten gegevens naar en van de [R-Script uitvoeren] [ execute-r-script] module.

De volledige code voor deze sectie is in [MachineLearningSamples-notitieblokken/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="load-and-check-data-in-machine-learning-studio"></a>Laden en controleer de gegevens in Machine Learning Studio

#### <a id="loading"></a>Laden van de gegevensset

We zullen beginnen bij het laden van de **csdairydata.csv** -bestand in Azure Machine Learning Studio.

1. Start uw Azure Machine Learning Studio-omgeving.
1. Selecteer **+ nieuw** in de linkerbenedenhoek van het scherm en selecteer **gegevensset**.
1. Selecteer **uit lokale bestand**, en vervolgens **Bladeren** om het bestand te selecteren.
1. Zorg ervoor dat u hebt geselecteerd **generieke CSV-bestand met de header (.csv)** als het type voor de gegevensset.
1. Klik op het vinkje.
1. Nadat de gegevensset is geüpload, ziet u de nieuwe gegevensset door het selecteren van de **gegevenssets** tabblad.  

#### <a name="create-an-experiment"></a>Een experiment maken

Nu dat we enkele gegevens in Machine Learning Studio hebben, moeten we het maken van een experiment voor de analyse.  

1. Selecteer **+ nieuw** op de onderste links en selecteer **Experiment**, klikt u vervolgens **leeg Experiment**.
1. U kunt uw experiment naam door te selecteren en te wijzigen, de **Experiment gemaakt op...**  titel boven aan de pagina. Zo wijzigen dat **CA Zuivel Analysis**.
1. Vouw aan de linkerkant van de pagina experiment **opgeslagen gegevenssets**, en vervolgens **mijn gegevenssets**. U ziet de **cadairydata.csv** die u eerder hebt geüpload.
1. Sleep en zet de **csdairydata.csv gegevensset** naar het experimentcanvas.
1. In de **zoeken items experimenteren** vak boven aan het linkerdeelvenster, type [R-Script uitvoeren][execute-r-script]. Hier ziet u de module worden weergegeven in de zoeklijst.
1. Sleep en zet de [R-Script uitvoeren] [ execute-r-script] module naar het palet.  
1. Koppel de uitvoer van de **csdairydata.csv gegevensset** naar de meest linkse invoer (**Dataset1**) van de [R-Script uitvoeren][execute-r-script].
1. **Vergeet niet om te selecteren van 'Opslaan'!**  

Op dit moment ziet uw experiment er ongeveer als in afbeelding 3.

![De CA Zuivel analyse experimenteren met de gegevensset en het uitvoeren van R-Script-module](./media/r-quickstart/fig3.png)

*Afbeelding 3. De CA Zuivel analyse experimenteren met de gegevensset en het uitvoeren van R-Script-module.*

#### <a name="check-on-the-data"></a>Controleer op de gegevens

We hebben een overzicht van de gegevens die we in onze experiment hebben geladen. Selecteer in het experiment, de uitvoer van de **cadairydata.csv gegevensset** en selecteer **visualiseren**. U ziet er ongeveer als afbeelding 4.  

![Samenvatting van de gegevensset cadairydata.csv](./media/r-quickstart/fig4.png)

*Afbeelding 4. Samenvatting van de gegevensset cadairydata.csv.*

In deze weergave ziet u een groot aantal nuttige informatie. U ziet de eerste verschillende rijen van de gegevensset. Als we een kolom selecteert, wordt de sectie statistieken meer informatie over de kolom. Bijvoorbeeld, laat de rij functietype zien welke gegevenstypen Azure Machine Learning Studio wordt toegewezen aan de kolom. Het is een goede controle dat als volgt: een beknopt overzicht hebben, voordat we ernstige werk.

### <a name="first-r-script"></a>Eerste R-script

We maken een eenvoudige eerste R-script om te experimenteren met in Azure Machine Learning Studio. Ik heb gemaakt en getest met het volgende script in RStudio.  

```R
## Only one of the following two lines should be used
## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
## If in RStudio, use the second line with read.csv()
cadairydata <- maml.mapInputPort(1)
# cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
str(cadairydata)
pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
## The following line should be executed only when running in
## Azure Machine Learning Studio
maml.mapOutputPort('cadairydata')
```

Nu wil ik dit script overbrengen naar Azure Machine Learning Studio. Ik kan gewoon knippen en plakken. Echter, in dit geval ik worden overgebracht mijn R-script via een zip-bestand.

### <a name="data-input-to-the-execute-r-script-module"></a>Gegevensinvoer voor de module R-Script uitvoeren

We hebben een overzicht van de invoer voor de [R-Script uitvoeren] [ execute-r-script] module. In dit voorbeeld leest de Californië melkkoeien gegevens in de [R-Script uitvoeren] [ execute-r-script] module.  

Er zijn drie mogelijke invoer voor de [R-Script uitvoeren] [ execute-r-script] module. U kunt een of meer van deze invoer, afhankelijk van uw toepassing. Het is ook perfect redelijke een R-script die helemaal geen invoer te gebruiken.  

Laten we bekijken op elk van deze invoer, die van links naar rechts. Ziet u de namen van elk van de invoerwaarden door plaatst u de cursor op de invoer en de knopinfo te lezen.  

#### <a name="script-bundle"></a>Script-bundel

De Script-bundel invoer zodat u de inhoud van een zip-bestand in [R-Script uitvoeren] [ execute-r-script] module. U kunt een van de volgende opdrachten gebruiken om de inhoud van het zip-bestand in uw R-code.

```R
source("src/yourfile.R") # Reads a zipped R script
load("src/yourData.rdata") # Reads a zipped R data file
```

> [!NOTE]
> Azure Machine Learning Studio-bestanden in het ZIP-bestand worden behandeld alsof ze zich in de src / map, dus moet u het voorvoegsel van de bestandsnamen van uw met de naam van deze map. Bijvoorbeeld, als het ZIP-bestand bevat de bestanden `yourfile.R` en `yourData.rdata` in de hoofdmap van het ZIP-bestand, wilt u deze als oplossen `src/yourfile.R` en `src/yourData.rdata` bij het gebruik van `source` en `load`.

We gegevenssets laden in de eerder besproken [laden van de gegevensset](#loading). Nadat u hebt gemaakt en getest het R-script dat wordt weergegeven in de vorige sectie, het volgende doen:

1. Sla het R-script in een. R-bestand. Roep ik mijn scriptbestand "simpleplot. R'. Hier volgt de inhoud.

   ```R
   ## Only one of the following two lines should be used
   ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
   ## If in RStudio, use the second line with read.csv()
   cadairydata <- maml.mapInputPort(1)
   # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
   str(cadairydata)
   pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
   ## The following line should be executed only when running in
   ## Azure Machine Learning Studio
   maml.mapOutputPort('cadairydata')
   ```

1. Maak een zip-bestand en kopieer het script in dit zip-bestand. Op Windows, kunt u met de rechtermuisknop op het bestand en selecteer **verzenden naar**, en vervolgens **gecomprimeerde map**. Hiermee maakt u een nieuw zipbestand met de 'simpleplot. R'-bestand.

1. Toevoegen van het bestand naar de **gegevenssets** in Machine Learning Studio, op te geven het type als **zip**. U ziet nu het zip-bestand in uw gegevenssets.

1. Slepen en neerzetten van het zip-bestand van **gegevenssets** naar de **ML Studio canvas**.

1. Koppel de uitvoer van de **zip-gegevens** pictogram aan de **Script bundel** invoer van de [R-Script uitvoeren] [ execute-r-script] module.

1. Type de `source()` functie met de naam van uw zip-bestand in het codevenster voor de [R-Script uitvoeren] [ execute-r-script] module. In mijn geval ik hebt getypt `source("src/simpleplot.R")`.  

1. Zorg ervoor dat u selecteert **opslaan**.

Als deze stappen voltooid zijn, de [R-Script uitvoeren] [ execute-r-script] module wordt het R-script uitgevoerd in het zip-bestand wanneer het experiment wordt uitgevoerd. Op dit moment ziet uw experiment er ongeveer als afbeelding 5.

![Experimenteer met behulp van ZIP R-script](./media/r-quickstart/fig6.png)

*Afbeelding 5. Experimenteer met behulp van ZIP R-script.*

#### <a name="dataset1"></a>Dataset1

U kunt een rechthoekig tabel met gegevens doorgeven aan uw R-code met behulp van de invoer Dataset1. In onze eenvoudig script de `maml.mapInputPort(1)` functie leest de gegevens van poort 1. Deze gegevens wordt vervolgens toegewezen aan een gegevensframe variabelenaam in uw code. In onze eenvoudig script voert de eerste coderegel de toewijzing.

```R
cadairydata <- maml.mapInputPort(1)
```

Uw experiment uitvoeren door het selecteren van de **uitvoeren** knop. Wanneer de uitvoering is voltooid, selecteert u de [R-Script uitvoeren] [ execute-r-script] -module en selecteer vervolgens **logboek van de uitvoer weergeven** in het deelvenster met eigenschappen. Een nieuwe pagina moet worden weergegeven in uw browser met daarin de inhoud van het bestand uitvoer.log. Wanneer u naar beneden schuift ziet u er ongeveer als volgt uit.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Verder omlaag op de pagina is dat meer gedetailleerde informatie over de kolommen die er ongeveer als volgt ziet.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

Deze resultaten zijn zoals verwacht, met 228 opmerkingen en 9 kolommen van het gegevensframe voornamelijk. We kunnen zien de namen van kolommen, het gegevenstype van R en een voorbeeld van elke kolom.

> [!NOTE]
> Deze dezelfde afdruk is eenvoudig beschikbaar vanuit de R-apparaat-uitvoer van de [R-Script uitvoeren] [ execute-r-script] module. Bespreken we de uitvoer van de [R-Script uitvoeren] [ execute-r-script] module in de volgende sectie.  

#### <a name="dataset2"></a>Dataset2

Het gedrag van de invoer Dataset2 is identiek aan die van Dataset1. Met behulp van deze gegevens kunt u een tweede rechthoekige tabel met gegevens in uw R-code doorgeven. De functie `maml.mapInputPort(2)`, met het argument 2 wordt gebruikt om door te geven van deze gegevens.  

### <a name="execute-r-script-outputs"></a>Uitvoer van de R-Script uitvoeren

#### <a name="output-a-dataframe"></a>Uitvoer van een dataframe

U kunt de inhoud van een R-dataframe uitvoer als een rechthoekig tabel via de poort resultaat Dataset1 met behulp van de `maml.mapOutputPort()` functie. In onze eenvoudige R-script wordt deze uitgevoerd door de volgende regel.

```
maml.mapOutputPort('cadairydata')
```

Nadat het experiment is uitgevoerd, selecteert u het resultaat Dataset1 uitvoerpoort en selecteer vervolgens **Visualize**. U ziet er ongeveer als afbeelding 6.

![De visualisatie van de uitvoer van de melkkoeien Californië-gegevens](./media/r-quickstart/fig7.png)

*Afbeelding 6. De visualisatie van de uitvoer van de melkkoeien Californië-gegevens.*

Deze uitvoer ziet er uit identiek zijn aan de input, precies zoals verwacht.  

### <a name="r-device-output"></a>Uitvoer van de R-apparaat

De uitvoer van het apparaat van de [R-Script uitvoeren] [ execute-r-script] -module bevat de uitvoer van berichten en afbeeldingen. Beide standard standard- en uitvoer foutberichten van R worden verzonden naar de uitvoerpoort van R-apparaat.  

De R-apparaat als uitvoer wilt bekijken, selecteert u de poort en klik vervolgens op **Visualize**. Ziet u de standaarduitvoer en de standaardfout van de R-script in afbeelding 7.

![Standaarduitvoer en de standaardfout van de poort-R-apparaat](./media/r-quickstart/fig8.png)

*Afbeelding 7. Standaarduitvoer en de standaardfout van de poort-R-apparaat.*

We bladeren ziet de uitvoer van de afbeeldingen van onze R-script in afbeelding 8.  

![Grafische uitvoer van de poort-R-apparaat](./media/r-quickstart/fig9.png)

*Afbeelding 8. Grafische uitvoer van de poort-R-apparaat.*  

## <a id="filtering"></a>Gegevens filteren en transformatie

In deze sectie wordt we enkele elementaire gegevens filteren en transformatiebewerkingen op de Californië melkkoeien gegevens uitvoeren. Aan het einde van deze sectie hebben we gegevens in een indeling die geschikt zijn voor het bouwen van een Analytics-model.  

Meer specifiek, in deze sectie wordt we enkele algemene gegevens opschonen en transformatie van taken uitvoeren: transformatie, filteren op dataframes, toe te voegen nieuwe berekende kolommen, typen en de waarde van transformaties. Deze achtergrond kunt u om te gaan met de veel variaties aangetroffen in de praktijk problemen.

De volledige R-code voor deze sectie is beschikbaar in [MachineLearningSamples-notitieblokken/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="type-transformations"></a>Type transformaties

Nu dat we de Californië melkkoeien gegevens kunt lezen in de R-code in de [R-Script uitvoeren] [ execute-r-script] -module, moeten we om ervoor te zorgen dat de gegevens in de kolommen het gewenste type en de indeling heeft.  

R is een dynamisch getypeerde taal, wat betekent dat gegevenstypen van één naar een andere als vereist worden afgedwongen. De atomische gegevenstypen in R bevatten numeriek, logische en -teken. Het type van meerdere factoren wordt gebruikt voor het opslaan van compactly categorische gegevens. U vindt meer informatie over gegevenstypen in de verwijzingen in [meer weten](#appendixb) hieronder.

Als gegevens in tabelvorm wordt gelezen in R vanuit een externe bron, maar het is altijd een goed idee om te controleren of de resulterende typen in de kolommen. U kunt een kolom van het typeteken, maar in veel gevallen deze wordt weergegeven als factor of vice versa. In andere gevallen een kolom die u denkt dat moet worden wordt numerieke vertegenwoordigd door tekengegevens, bijvoorbeeld '1,23' in plaats van 1,23 als een zwevende-kommagetal.  

Het is gelukkig eenvoudig één type converteren naar een andere, zolang de toewijzing is mogelijk. Bijvoorbeeld, u 'Nevada' niet converteren naar een numerieke waarde, maar u kunt converteren naar een factor (categorische variabele). Als een ander voorbeeld kunt u een numerieke 1 converteren naar een teken '1' of een van meerdere factoren.  

De syntaxis voor het gebruik van deze conversies is eenvoudig: `as.datatype()`. Deze conversiefuncties omvatten het volgende.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Kijken naar de gegevenstypen van de kolommen die we in de vorige sectie invoer: alle kolommen van het type Numeriek, met uitzondering van de kolom met het label 'Maand', van het typeteken is zijn. Laten we deze converteren naar een multi-factor Authentication en de resultaten testen.  

Ik heb de regel die de matrix teststappen gemaakt en een regel die de kolom 'Maand' converteren naar een factor toegevoegd hebt verwijderd. In mijn experiment wordt ik net Knip en plak de R-code in het code-venster van de [R-Script uitvoeren] [ execute-r-script] Module. U kunt ook bijwerken van het zip-bestand en dit uploaden naar Azure Machine Learning Studio, maar dit duurt een aantal stappen uitvoeren.  

```R
## Only one of the following two lines should be used
## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
## If in RStudio, use the second line with read.csv()
cadairydata <- maml.mapInputPort(1)
# cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
## Ensure the coding is consistent and convert column to a factor
cadairydata$Month <- as.factor(cadairydata$Month)
str(cadairydata) # Check the result
## The following line should be executed only when running in
## Azure Machine Learning Studio
maml.mapOutputPort('cadairydata')
```

Laten we deze code uitvoeren en kijken naar het uitvoerlogboek voor voor de R-script. De relevante gegevens van het logboek wordt weergegeven in afbeelding 9.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Afbeelding 9. Samenvatting van het gegevensframe met een factor-variabele.*

Het type voor de maand moet nu de melding ontvangen '**Factor met 14 niveaus**'. Dit is een probleem omdat er alleen 12 maanden van het jaar. U kunt ook controleren om te zien dat het type in **Visualize** van de resultaat-gegevensset-poort is '**Categorisch**'.

Het probleem is dat de kolom 'Maand' niet systematisch heeft zijn gecodeerd. In sommige gevallen een maand April wordt genoemd en in andere gevallen wordt het Apr afgekort. We kunnen dit probleem oplossen door de tekenreeks die moet 3 tekens. De regel code ziet er nu als volgt uit:

```R
## Ensure the coding is consistent and convert column to a factor
cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))
```

Het experiment opnieuw uitvoeren en het uitvoerlogboek weergeven. De verwachte resultaten worden weergegeven in afbeelding 10.  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Afbeelding 10. Samenvatting van het gegevensframe met het juiste aantal factor levels.*

Onze variabele factor heeft nu de gewenste 12 niveaus.

### <a name="basic-data-frame-filtering"></a>Basisgegevens frame filteren

R dataframes ondersteunen krachtige filteropties. Gegevenssets zijn subset met behulp van logische filters op rijen of kolommen. In veel gevallen is complexe filtercriteria vereist. De verwijzingen in [meer weten](#appendixb) hieronder bevat uitgebreide voorbeelden van dataframes filteren.  

Er is één bit filteren we altijd moeten doen op de gegevensset. Als u de kolommen van het gegevensframe cadairydata bekijkt, ziet u twee overbodige kolommen. De eerste kolom bevat alleen een rijnummer, niet erg nuttig is. De tweede kolom, Year.Month, bevat redundante informatie. We kunnen deze kolommen eenvoudig uitsluiten met behulp van de volgende R-code.

> [!NOTE]
> Van nu af in deze sectie alleen ziet u de aanvullende code die ik toe te voegen in de [R-Script uitvoeren] [ execute-r-script] module. Ik zal elke nieuwe regel toevoegen **voordat** de `str()` functie. Kan ik deze functie gebruiken om te controleren of de resultaten in Azure Machine Learning Studio.

Kan ik de volgende regel toevoegen aan mijn R-code in de [R-Script uitvoeren] [ execute-r-script] module.

```R
# Remove two columns we do not need
cadairydata <- cadairydata[, c(-1, -2)]
```

Voer deze code in uw experiment uit en controleer het resultaat van het uitvoerlogboek voor. Deze resultaten worden weergegeven in afbeelding 11.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Afbeelding 11. Samenvatting van het gegevensframe met twee kolommen zijn verwijderd.*

Goed nieuws! Krijgen we de verwachte resultaten.

### <a name="add-a-new-column"></a>Een nieuwe kolom toevoegen

Voor het maken van time series modellen is het handig zijn als een kolom met de maanden sinds de start van de tijdreeks. We gaan een nieuwe kolom 'Month.Count' maken.

Om u te helpen met het organiseren van de code maken we onze eerste eenvoudige functie `num.month()`. Deze functie voor het maken van een nieuwe kolom in het dataframe zullen we vervolgens toepassen. De nieuwe code is als volgt.

```R
## Create a new column with the month count
## Function to find the number of months from the first
## month of the time series
num.month <- function(Year, Month) {
  ## Find the starting year
  min.year  <- min(Year)

  ## Compute the number of months from the start of the time series
  12 * (Year - min.year) + Month - 1
}

## Compute the new column for the dataframe
cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)
```

Nu de bijgewerkte experiment uit te voeren en het uitvoerlogboek voor gebruiken om de resultaten weer te geven. Deze resultaten worden weergegeven in afbeelding 12.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Afbeelding 12. Samenvatting van het gegevensframe met de extra kolom.*

Het lijkt erop dat alles werkt. De nieuwe kolom met de verwachte waarden hebben we in onze dataframe.

### <a name="value-transformations"></a>Waarde transformaties

In deze sectie wordt we enkele eenvoudige transformaties uitvoeren op de waarden in enkele van de kolommen van onze dataframe. De R-taal ondersteunt bijna willekeurige waarde transformaties. De verwijzingen in [meer weten](#appendixb) hieronder bevat uitgebreide voorbeelden.

Als u de waarden in de samenvattingen van onze dataframe bekijkt ziet u iets oneven hier. Meer ice cream dan melk geproduceerd in Californië? Nee, natuurlijk niet, omdat dit geen begrijpelijk is, als dit feit sad mogelijk tot een aantal van ons ice cream Ontwikkelaarshulpmiddelen. De eenheden zijn verschillend. De prijs is in eenheden van ons pond, melk is in eenheden van 1 miljoen US pond, ice cream in eenheden van 1000 ons gallon en Kwark in eenheden van 1000 VS pond is. Ervan uitgaande dat ice cream weegt ongeveer 6,5 pond per gallon, kunnen eenvoudig doen we de vermenigvuldiging als u wilt converteren van deze waarden, zodat ze allemaal op gelijke eenheden van 1000 pond.

Voor onze prognosemodel gebruiken we een Multiplicatieve model voor de trend en seizoensgebonden aanpassing van deze gegevens. Een transformatie logboek kan we een lineair model, vereenvoudiging van dit proces gebruiken. We kunt de transformatie log in dezelfde functie waar de vermenigvuldiger is toegepast toepassen.

In de volgende code, kan ik een nieuwe functie definieert `log.transform()`, en dit toepassen op de rijen die de numerieke waarden bevat. De R `Map()` functie wordt gebruikt om toe te passen de `log.transform()` functie met de geselecteerde kolommen van het gegevensframe. `Map()` komt overeen met `apply()` , maar kunt u meer dan een lijst met argumenten voor de functie. Houd er rekening mee dat een lijst met vermenigvuldigers levert het tweede argument voor de `log.transform()` functie. De `na.omit()` functie als een deel van de opruimen wordt gebruikt om te controleren of er geen waarden ontbreekt of is niet gedefinieerd in het dataframe.

```R
log.transform <- function(invec, multiplier = 1) {
  ## Function for the transformation, which is the log
  ## of the input value times a multiplier

  warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                       "ERROR: Arguments to function log.transform must be greate than zero",
                       "ERROR: Aggurment multiplier to funcition log.transform must be a scaler",
                       "ERROR: Invalid time seies value encountered in function log.transform"
                       )

  ## Check the input arguments
  if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
  if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
  if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

  ## Wrap the multiplication in tryCatch
  ## If there is an exception, print the warningmessage to
  ## standard error and return NA
  tryCatch(log(multiplier * invec),
           error = function(e){warning(warningmessages[4]); NA})
}


## Apply the transformation function to the 4 columns
## of the dataframe with production data
multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

## Get rid of any rows with NA values
cadairydata <- na.omit(cadairydata)  
```

Er is een behoorlijke bits gebeurt in de `log.transform()` functie. De meeste van deze code controleert op mogelijke problemen met de argumenten of omgaan met uitzonderingen die nog steeds tijdens de berekeningen optreden kunnen. Alleen een paar regels van deze code daadwerkelijk doen de berekeningen kunt.

Het doel van de verdediging programmering is om te voorkomen dat het uitvallen van één functie waarmee wordt voorkomen dat de verwerking van u doorgaat. Een plotselinge storing van een langlopende-analyse is erg vervelend. Om te voorkomen dat deze situatie, moeten standaard retourwaarden worden gekozen die schade aan downstream-verwerkingen worden beperkt. Een bericht wordt ook geproduceerd Waarschuw gebruikers dat is iets fout gegaan.

Als u worden niet voor verdediging programmeren in R gebruikt, kan alle deze code een beetje verwarrend lijken. Ik begeleidt u stapsgewijs door de belangrijkste stappen:

1. Een vector van vier berichten is gedefinieerd. Deze berichten worden gebruikt om informatie over een aantal van de mogelijke fouten en uitzonderingen die met deze code optreden kunnen te communiceren.
2. Ik retourneren NA de waarde voor elk geval. Er zijn tal van andere mogelijkheden die mogelijk minder neveneffecten. Ik kan een vector nullen of de oorspronkelijke invoer vector, bijvoorbeeld retourneren.
3. Controles worden uitgevoerd op de argumenten voor de functie. In elk geval, als er een fout wordt gedetecteerd, een standaardwaarde geretourneerd en een bericht wordt geproduceerd door de `warning()` functie. Ik gebruik `warning()` in plaats van `stop()` zoals deze wordt beëindigd uitvoering, precies wat ik probeer om te voorkomen. Houd er rekening mee dat ik hebt geschreven deze code in een procedurele stijl, zoals in dit geval een functionele aanpak is complex en verborgen.
4. Het logboek-berekeningen zijn verpakt in `tryCatch()` zodat uitzonderingen leiden niet een plotselinge stilstand te verwerken tot. Zonder `tryCatch()` de meeste fouten die worden gegenereerd door een stopsignaal, die precies dat leiden van R-functies.

Deze R-code uitvoeren in uw experiment en bekijk de uitvoer in het bestand uitvoer.log hebben. U ziet nu de getransformeerde waarden van de vier kolommen in het logboek, zoals wordt weergegeven in afbeelding 13.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Afbeelding 13. Samenvatting van de getransformeerde waarden in het dataframe.*

Ziet u de waarden zijn getransformeerd. Nu melkproductie is aanzienlijk groter dan alle andere melkkoeien product productie, terughalen dat we nu op zoek op een logaritmische schaal.

Op dit moment onze gegevens worden gewist en we gereed zijn voor sommige modellen. Kijken naar de samenvatting voor de resultaat-gegevensset-uitvoer van de visualisatie onze [R-Script uitvoeren] [ execute-r-script] -module, ziet u de kolom 'Maand' is 'Categorisch' met 12 unieke waarden, opnieuw, net zoals we wilden.

## <a id="timeseries"></a>Time series-objecten en correlatieanalyse

In deze sectie wordt we een paar basic R time series-objecten verkennen en analyseren van de correlatie tussen enkele van de variabelen. Ons doel is om uit te voeren een gegevensframe met de pairwise correlatie-informatie op verschillende lag.

De volledige R-code voor deze sectie is in [MachineLearningSamples-notitieblokken/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="time-series-objects-in-r"></a>Time series-objecten in R

Als al is vermeld, tijd zijn-reeksen een reeks waarden geïndexeerd door de tijd. R time series-objecten worden gebruikt voor het maken en beheren van de tijdsindex. Er zijn diverse voordelen voor het gebruik van time series-objecten. Time series-objecten is een gratis uit de tal van details van het beheer van de serie index tijdwaarden die worden opgenomen in het object. Bovendien kunnen time series-objecten u veel tijd reeks methoden gebruiken voor het plotten, afdrukken, modellering van kredietrisico, enzovoort.

De POSIXct time series-klasse wordt vaak gebruikt en relatief eenvoudig is. Deze tijdreeks klasse metingen tijd vanaf het begin van de epoche, 1 januari 1970. In dit voorbeeld gebruiken we POSIXct time series-objecten. Andere klassen gebruikte R time series-object zijn zoo en xts, uitbreidbare tijdreeksen.

### <a name="time-series-object-example"></a>Voorbeeld van het object time series

Aan de slag met het voorbeeld. Slepen en neerzetten een **nieuwe** [R-Script uitvoeren] [ execute-r-script] module in uw experiment. Koppel de uitvoerpoort Dataset1 resultaat van de bestaande [R-Script uitvoeren] [ execute-r-script] module aan de Dataset1 poort van de nieuwe invoer [R-Script uitvoeren] [ execute-r-script] module.

Zoals ik deed voor de eerste voorbeelden zoals we in het voorbeeld doorneemt, op bepaalde tijdstippen ziet alleen de incrementele extra regels met R-code bij elke stap.  

#### <a name="reading-the-dataframe"></a>Lezen van het gegevensframe

Laten we als eerste stap lezen in een dataframe en zorg ervoor dat we de verwachte resultaten ophalen. De volgende code moet de taak te doen.

```R
# Comment the following if using RStudio
cadairydata <- maml.mapInputPort(1)
str(cadairydata) # Check the results
```

Voer het experiment nu uit. Het logboek van de nieuwe vorm van R-Script uitvoeren moet eruitzien als afbeelding 14.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

*Afbeelding 14. Samenvatting van het gegevensframe in de module R-Script uitvoeren.*

Deze gegevens zijn van de verwachte typen en -indeling. Houd er rekening mee dat de kolom 'Maand' van het type van meerdere factoren is en het verwachte aantal niveaus is.

#### <a name="creating-a-time-series-object"></a>Het maken van een time series-object

Er moet een time series-object toevoegen aan onze dataframe. Vervang de huidige code door het volgende, waarmee een nieuwe kolom van de klasse POSIXct worden toegevoegd.

```R
# Comment the following if using RStudio
cadairydata <- maml.mapInputPort(1)

## Create a new column as a POSIXct object
Sys.setenv(TZ = "PST8PDT")
cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

str(cadairydata) # Check the results
```

Controleer het logboek. Het moet eruitzien als afbeelding 15.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Figure 15. Samenvatting van het gegevensframe met een time series-object.*

We kunt in het overzicht dat de nieuwe kolom in feite van klasse POSIXct is zien.

### <a name="exploring-and-transforming-the-data"></a>Verkennen en de gegevens transformeren

Laten we enkele van de variabelen in deze gegevensset verkennen. Een matrix teststappen is een goede manier voor het produceren van een beknopt overzicht. Ik ben vervangen de `str()` functie in de vorige R-code met de volgende regel.

```R
pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")
```

Deze code uitvoeren en kijken wat er gebeurt. De grafiek die worden geproduceerd op de poort-R-apparaat moet eruitzien als afbeelding 16.

![Matrix van de geselecteerde variabelen Teststappen](./media/r-quickstart/fig17.png)

*Afbeelding 16. Teststappen matrix van de geselecteerde variabelen.*

Er is een vreemd uitziet structuur in de relaties tussen deze variabelen. Hierdoor ontstaat misschien van trends in de gegevens en van het feit dat we niet de variabelen zijn gestandaardiseerd.

### <a name="correlation-analysis"></a>Correlatieanalyse

Om uit te voeren van de correlatieanalyse van de moet zowel trend ongedaan maken en te standaardiseren, de variabelen. We gebruiken gewoon de R `scale()` functie, die zowel datacenters en kan worden geschaald variabelen. Deze functie kan ook sneller worden uitgevoerd. Ik wil echter ziet u een voorbeeld van defensieve programmeren in R.

De `ts.detrend()` functie onderstaande beide van deze bewerkingen uitvoert. De volgende twee regels code trend ongedaan maken de gegevens en vervolgens de waarden te standaardiseren.

```R
ts.detrend <- function(ts, Time, min.length = 3){
  ## Function to de-trend and standardize a time series

  ## Define some messages if they are NULL  
  messages <- c('ERROR: ts.detrend requires arguments ts and Time to have the same length',
                'ERROR: ts.detrend requires argument ts to be of type numeric',
                paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                'ERROR: Detrend regression has failed in ts.detrend',
                'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
  )
  # Create a vector of zeros to return as a default in some cases
  zerovec  <- rep(length(ts), 0.0)

  # The input arguments are not of the same length, return ts and quit
  if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

  # If the ts is not numeric, just return a zero vector and quit
  if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

  # If the ts is too short, just return it and quit
  if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

  ## Check that the Time variable is of class POSIXct
  if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

  ## De-trend the time series by using a linear model
  ts.frame  <- data.frame(ts = ts, Time = Time)
  tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
           error = function(e){warning(messages[5]); zerovec})

  tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
             ts <- ts/stdev},
            error = function(e){warning(messages[6]); zerovec})

  ts
}  
## Apply the detrend.ts function to the variables of interest
df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

## Plot the results to look at the relationships
pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")
```

Er is een behoorlijke bits gebeurt in de `ts.detrend()` functie. De meeste van deze code controleert op mogelijke problemen met de argumenten of omgaan met uitzonderingen die nog steeds tijdens de berekeningen optreden kunnen. Alleen een paar regels van deze code daadwerkelijk doen de berekeningen kunt.

We hebben al een voorbeeld van defensieve programmeren in waarde transformaties besproken. Beide blokken berekening verpakt in `tryCatch()`. Voor bepaalde fouten is het zinvol om terug te keren van de oorspronkelijke invoer vector en in andere gevallen kan ik een vector uit nullen retourneren.  

Houd er rekening mee dat de lineaire regressie gebruikt voor het ongedaan maken populair een time series regressie is. De variabele voorspelde is een time series-object.  

Eenmaal `ts.detrend()` is gedefinieerd wordt toegepast op de variabelen van belang zijn in onze dataframe. We moeten de resulterende lijst die zijn gemaakt door forceren `lapply()` naar dataframe gegevens met behulp van `as.data.frame()`. Vanwege de verdediging aspecten van `ts.detrend()`, niet-proces een van de variabelen wordt niet voorkomen dat de juiste verwerking van de andere.  

De laatste regel van de code maakt een pairwise teststappen. Na het uitvoeren van de R-code, worden de resultaten van de teststappen weergegeven in afbeelding 17.

![Pairwise teststappen van ongedaan maken trendanalyse en gestandaardiseerde tijdreeks](./media/r-quickstart/fig18.png)

*Afbeelding 17. Pairwise teststappen van ongedaan maken trendanalyse en gestandaardiseerde tijdreeks.*

U kunt deze resultaten die wordt weergegeven in afbeelding 16 vergelijken. Met de trend verwijderd en de variabelen gestandaardiseerd, zien we veel minder structuur in de relaties tussen deze variabelen.

De code voor het berekenen van de correlaties als R ccf objecten is als volgt.

```R
## A function to compute pairwise correlations from a
## list of time series value vectors
pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
  ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
}

## A list of the pairwise indices
corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

## Compute the list of ccf objects
cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

cadairycorrelations
```

Deze code wordt uitgevoerd, maakt het logboek wordt weergegeven in afbeelding 18.

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

*Afbeelding 18. Lijst met ccf objecten uit de pairwise correlatieanalyse.*

Er is een Correlatiewaarde voor elke vertraging. Geen van deze waarden correlatie is groot genoeg is voor aanzienlijk zijn. We kunnen daarom sluiten we elke variabele onafhankelijk kunt modelleren.

### <a name="output-a-dataframe"></a>Uitvoer van een dataframe
We hebben de pairwise correlaties berekend als een lijst met R ccf objecten. Dit geeft een deel van een probleem als de uitvoerpoort resultaat gegevensset echt een dataframe is vereist. Bovendien is het object ccf is zelf een lijst en willen we alleen de waarden in het eerste element van deze lijst, de correlaties op de verschillende lag.

De volgende code haalt de waarden van de vertraging in de lijst van ccf objecten, die zelf lijsten.

```R
df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
r.names  <- c("Corr Cot Cheese - Ice Cream",
              "Corr Cot Cheese - Milk Prod",
              "Corr Cot Cheese - Fat Price",
              "Corr Ice Cream - Mik Prod",
              "Corr Ice Cream - Fat Price",
              "Corr Milk Prod - Fat Price")

## Build a dataframe with the row names column and the
## correlation data frame and assign the column names
outframe <- cbind(r.names, df.correlations)
colnames(outframe) <- c.names
outframe


## WARNING!
## The following line works only in Azure Machine Learning Studio
## When running in RStudio, this code will result in an error
#maml.mapOutputPort('outframe')
```

De eerste coderegel is enigszins moeilijk en een verklaring kunt u begrijpt. Werken op basis van binnenuit hebben we het volgende:

1. De '**[[**'operator met het argument'**1**' de vector van correlaties op de lag uit het eerste element van de lijst met ccf objecten geselecteerd.
2. De `do.call()` functie is van toepassing de `rbind()` functie via de elementen van de lijst wordt geretourneerd door `lapply()`.
3. De `data.frame()` functie converteert het resultaat is geproduceerd door `do.call()` naar een dataframe.

Houd er rekening mee dat de rijnamen van de in een kolom van het gegevensframe zijn. Doet dit het geval is, behoudt de rij namen wanneer ze worden uitgevoerd vanuit de [R-Script uitvoeren][execute-r-script].

De code wordt uitgevoerd, wordt de uitvoer die wordt weergegeven in afbeelding 19 gegenereerd wanneer ik **Visualize** de uitvoer op de resultaat-gegevensset-poort. De rijnamen van de zijn in de eerste kolom, zoals bedoeld.

![Uitvoer van de resultaten van de correlatieanalyse](./media/r-quickstart/fig20.png)

*Figure 19. Resultaten uitvoer van de correlatieanalyse.*

## <a id="seasonalforecasting"></a>Time series-voorbeeld: seizoensgebonden prognoses

Onze gegevens is nu in een formulier die geschikt zijn voor analyse en er zijn geen belangrijke correlaties tussen de variabelen hebt vastgesteld. Laten we gaan en een tijdreeks prognose model maken. Met dit model wordt we Californië melkproductie voor de 12 maanden van 2013 prognose.

Onze prognosemodel heeft twee componenten, een onderdeel van de trend en een seizoensgebonden onderdeel. De volledige prognose is het product van deze twee onderdelen. Dit type model staat bekend als een Multiplicatieve model. Het alternatief is een additieve model. We hebben al een transformatie log toegepast op de variabelen van belang zijn, waardoor deze analyse tractable.

De volledige R-code voor deze sectie is in [MachineLearningSamples-notitieblokken/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="creating-the-dataframe-for-analysis"></a>Het maken van het dataframe voor analyse

Begin met het toevoegen van een **nieuwe** [R-Script uitvoeren] [ execute-r-script] module naar het experimentcanvas. Verbinding maken met de **resultaat gegevensset** uitvoer van de bestaande [R-Script uitvoeren] [ execute-r-script] module aan de **Dataset1** invoer van de nieuwe module. Het resultaat ziet er ongeveer als afbeelding 20.

![Het experiment met de nieuwe R-Script uitvoeren-module toegevoegd](./media/r-quickstart/fig21.png)

*Afbeelding 20. Het experiment met de nieuwe R-Script uitvoeren-module toegevoegd.*

Als met de correlatieanalyse die we zojuist hebt voltooid, moeten we een kolom met een POSIXct time series-object toe te voegen. De volgende code wordt alleen dit doen.

```R
# If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
cadairydata <- maml.mapInputPort(1)

## Create a new column as a POSIXct object
Sys.setenv(TZ = "PST8PDT")
cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

str(cadairydata)
```

Voer deze code uit en bekijk het logboek. Het resultaat ziet er als afbeelding 21.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Afbeelding 21. Een samenvatting van het gegevensframe.*

Met dit resultaat zijn we beginnen met onze analyse.

### <a name="create-a-training-dataset"></a>Maken van een gegevensset training

We moeten maken van een gegevensset training met het gegevensframe dat is gebouwd. Deze gegevens omvatten alle van de metingen, behalve de laatste 12 van het jaar 2013, is onze testgegevensset. De volgende code subsets van het gegevensframe en maakt u grafieken van de melkkoeien variabelen voor de productie- en prijs. Ik maak grafieken van de vier productie en variabelen voor de prijs. Een anonieme functie wordt gebruikt voor het definiëren van sommige verbetert voor plot en vervolgens aan de lijst met de andere twee argumenten met herhalen `Map()`. Als u weet ik denk dat een voor lus zou hebben goed zijn uitgevoerd hier, u juist zijn. Maar omdat R is een functionele taal die ik heb waarin u een functionele aanpak.

```R
cadairytrain <- cadairydata[1:216, ]

Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
               "Log CA Ice Cream Production, 1000s lb",
               "Log CA Milk Production 1000s lb",
               "Log North CA Milk Milk Fat Price per 1000 lb")

Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)
```

De code uitvoeren, produceert de reeks tijd reeks die zichtbaar zijn in de uitvoer van de R-apparaat wordt weergegeven in afbeelding 22. Houd er rekening mee dat de tijdas in eenheden van datums, een mooie voordeel van de tijd reeks methode tekenen.

![Eerste dag van time series grafieken van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-161.png)

![Tweede van time series grafieken van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-162.png)

![Derde van time series grafieken van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-163.png)

![Vierde van time series grafieken van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-164.png)

*Afbeelding 22. Time series grafieken van Californië zuivelproductie en gegevens voor de prijs.*

### <a name="a-trend-model"></a>Een trend-model

Een time series-object en een overzicht van de gegevens heeft gehad gemaakt, laten we beginnen om een model trend van de productiegegevens Californië melk samen te stellen. We kunnen dit doen met een reeks tijd regressie. Het is echter duidelijk van de grafiek die we meer dan een helling nodig hebt en voor het modelleren van de geobserveerde trend in de trainingsgegevens nauwkeurig worden onderschept.

De kleine schaal van de gegevens worden gegeven, wordt ik het model voor de trend in RStudio bouwen en knip en plak vervolgens de resulterende model in Azure Machine Learning Studio. RStudio biedt een interactieve omgeving voor dit type interactieve analyses.

Als een eerste poging wordt ik probeer een polynomiale regressie met bevoegdheden maximaal 3. Er is een echte gevaar te veel geschikt zijn voor dit soort modellen. Het is daarom raadzaam om te voorkomen dat hoge voorwaarden. De `I()` functie belemmert interpretatie van de inhoud (interpreteert de inhoud, zoals is') en kunt u een letterlijk geïnterpreteerde functie schrijven in een regressievergelijking.

```R
milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
summary(milk.lm)
```

Hiermee wordt het volgende gegenereerd.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

Van P-waarden (`Pr(>|t|)`) in deze uitvoer kunt zien we dat de gekwadrateerde term mag niet significant. Ik gebruik de `update()` functie dit model wijzigen door het verwijderen van de kwadraten term.

```R
milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
summary(milk.lm)
```

Hiermee wordt het volgende gegenereerd.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

Dit ziet er beter uit. Alle voorwaarden zijn van belang. De waarde van de 2e-16 is echter een standaardwaarde, en niet te serieus nemen moet worden genomen.  

Als een test sanity laten we een diagram van de reeks tijd van de gegevens van de zuivelproductie Californië met de trend-curve wordt weergegeven. Ik heb de volgende code hebt toegevoegd in de Azure Machine Learning Studio [R-Script uitvoeren] [ execute-r-script] -model (niet RStudio) voor het maken van het model en een grafiek. Het resultaat wordt weergegeven in afbeelding 23.

```R
milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)
```

![Californië melk productiegegevens met trend model wordt weergegeven](./media/r-quickstart/unnamed-chunk-18.png)

*Afbeelding 23. Californië melk productiegegevens met trend model weergegeven.*

Het lijkt erop dat de trend-model past bij de gegevens vrij goed. Bovendien wordt lijkt er niet te duiden dat te veel aanpassing van labels, zoals afwijkende wiggles in de curve model.  

### <a name="seasonal-model"></a>Seizoensgebonden model

Met een model van de trend in voorraad moeten we pushen op en de seizoensgebonden effecten. We gebruiken de maand van het jaar als een dummy-variabele in het lineaire model voor het vastleggen van het effect per maand. Houd er rekening mee dat wanneer u factor variabelen in een model introduceren, de intercept moet niet worden berekend. Als u dit doet, wordt de formule te veel is opgegeven en R wordt verwijdert u een van de gewenste factoren maar houden de intercept term.

Aangezien we een goede trend-model hebben gebruiken we de `update()` functie naar de nieuwe voorwaarden toevoegen aan het bestaande model. De -1 in de formule van de update wordt de term intercept geweigerd. Als u doorgaat in RStudio voor het moment dat:

```R
milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
summary(milk.lm2)
```

Hiermee wordt het volgende gegenereerd.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

We zien dat het model niet meer een term intercept is en 12 maand belangrijke factoren heeft. Dit is precies wat we wilden om te zien.

Laten we een andere tijd reeks diagram van de gegevens van de zuivelproductie Californië om te zien hoe goed het seizoensgebonden model werkt. Ik heb de volgende code hebt toegevoegd in de Azure Machine Learning Studio [R-Script uitvoeren] [ execute-r-script] maken van het model en een grafiek.

```R
milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)
```

Deze code uitvoeren in Azure Machine Learning Studio maakt het diagram wordt weergegeven in afbeelding 24 uur per dag.

![Californië melkproductie met model inclusief seizoensgebonden effecten](./media/r-quickstart/unnamed-chunk-20.png)

*Afbeelding 24 uur per dag. Melkproductie Californië met model inclusief seizoensgebonden effecten.*

De aanpassen aan de gegevens weergegeven in afbeelding 24 uur per dag wordt in plaats daarvan bevorderen. Zoeken naar redelijke zowel de trend en het seizoensgebonden effect (maandelijkse variatie).

Als een andere selectievakje op het model hebt u laten we eens naar de dit. De volgende code de voorspelde waarden van onze twee modellen worden berekend, berekent de dit voor het seizoen model en vervolgens deze dit voor het trainen van gegevens die zichtbaar zijn.

```R
## Compute predictions from our models
predict1  <- predict(milk.lm, cadairydata)
predict2  <- predict(milk.lm2, cadairydata)

## Compute and plot the residuals
residuals <- cadairydata$Milk.Prod - predict2
plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")
```

De resterende plot wordt weergegeven in afbeelding 25.

![Dit van het seizoen model voor het trainen van gegevens](./media/r-quickstart/unnamed-chunk-21.png)

*Afbeelding 25. Als u dit van het seizoen model voor het trainen van gegevens.*

Deze dit bekijken redelijke. Er is geen specifieke structuur, met uitzondering van het effect van de recessie 2008-2009, die ons model geen rekening gehouden met name goed.

Het diagram wordt weergegeven in afbeelding 25 is handig voor het detecteren van alle tijdsafhankelijke patronen in de dit. De expliciete aanpak van computing en de ik gebruikt dit uitzetten plaatst de dit in de volgorde van de tijd op de grafiek. Als, aan de andere kant, ik had uitgezet `milk.lm$residuals`, de plot niet zouden zijn in volgorde van tijd.

U kunt ook `plot.lm()` voor het produceren van een reeks diagnostische grafieken.

```R
## Show the diagnostic plots for the model
plot(milk.lm2, ask = FALSE)
```

Deze code maakt een reeks diagnostische grafieken weergegeven in afbeelding 26.

![Eerste dag van de diagnostische grafieken voor het seizoen model](./media/r-quickstart/unnamed-chunk-221.png)

![Tweede van diagnostische grafieken voor het seizoen model](./media/r-quickstart/unnamed-chunk-222.png)

![Derde van diagnostische grafieken voor het seizoen model](./media/r-quickstart/unnamed-chunk-223.png)

![Vierde van diagnostische grafieken voor het seizoen model](./media/r-quickstart/unnamed-chunk-224.png)

*Afbeelding 26. Diagnostische gegevens die zichtbaar zijn voor het seizoen model.*

Er zijn enkele zeer invloedrijke punten vermeld in deze grafieken, maar niets geweldige opleveren. Verder, we kunnen zien van de normale Q-Q plot dat de dit zich dicht bij normaal gesproken worden gedistribueerd, een belangrijk hypothese voor lineaire modellen.

### <a name="forecasting-and-model-evaluation"></a>Prognoses en model evalueren

Er is slechts één ding te doen om te voltooien in ons voorbeeld. We moeten compute prognoses en meten van de fout op basis van de werkelijke gegevens. Onze prognose worden voor de 12 maanden van 2013. We kunnen een meting fout voor deze voorspelling voor de werkelijke gegevens die geen deel uitmaakt van onze gegevensset training compute. Daarnaast kunnen we de prestaties van de trainingsgegevens aan de 12 maanden van testgegevens 18 jaar vergelijken.  

Een aantal metrische gegevens worden gebruikt voor het meten van de prestaties van time series-modellen. In ons geval gebruiken we de root mean vierkant (RMS)-fout. De volgende functie berekent de RMS-fout tussen twee reeksen.  

```R
RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
  ## Function to compute the RMS error or difference between two
  ## series or vectors

  messages <- c("ERROR: Input arguments to function RMS.error of wrong type encountered",
                "ERROR: Input vector to function RMS.error is too short",
                "ERROR: Input vectors to function RMS.error must be of same length",
                "WARNING: Funtion rms.error has received invald input time series.")

  ## Check the arguments
  if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
    warning(messages[1])
    return(NA)}

  if(length(series1) < min.length) {
    warning(messages[2])
    return(NA)}

  if((length(series1) != length(series2))) {
       warning(messages[3])
    return(NA)}

  ## If is.log is TRUE exponentiate the values, else just copy
  if(is.log) {
    tryCatch( {
      temp1 <- exp(series1)
      temp2 <- exp(series2) },
      error = function(e){warning(messages[4]); NA}
    )
  } else {
    temp1 <- series1
    temp2 <- series2
  }

 ## Compute predictions from our models
predict1  <- predict(milk.lm, cadairydata)
predict2  <- predict(milk.lm2, cadairydata)

## Compute the RMS error in a dataframe
  tryCatch( {
    sqrt(sum((temp1 - temp2)^2) / length(temp1))},
    error = function(e){warning(messages[4]); NA})
}
```

Net als bij de `log.transform()` functie in de sectie "Waarde transformaties besproken" Er is zeer veel fout controleren en uitzondering code voor herstel in deze functie. De principes die werkzaam zijn hetzelfde. Het werk wordt uitgevoerd op twee plaatsen die zijn ingepakt in `tryCatch()`. De eerste, zijn de tijdreeksen exponentiated, aangezien we hebben gewerkt met de logboeken van de waarden. Ten tweede wordt de werkelijke RMS-fout berekend.  

Uitgerust met een functie voor het meten van de RMS-fout, laten we bouwen en uitvoeren van een gegevensframe met de RMS-fouten. Nemen we de voorwaarden voor het alleen trend-model en het volledige model met seizoensgebonden factoren. De volgende code wordt de taak met behulp van de twee lineaire modellen die we hebben gebouwd.

```R
## Compute the RMS error in a dataframe
## Include the row names in the first column so they will
## appear in the output of the Execute R Script
RMS.df  <-  data.frame(
rowNames = c("Trend Model", "Seasonal Model"),
  Traing = c(
  RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
  RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
  Forecast = c(
    RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
    RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
)
RMS.df

## The following line should be executed only when running in
## Azure Machine Learning Studio
maml.mapOutputPort('RMS.df')
```

Deze code wordt uitgevoerd, wordt de uitvoer die wordt weergegeven in afbeelding 27 op de uitvoerpoort resultaat gegevensset gegenereerd.

![Vergelijking van RMS-fouten voor de modellen](./media/r-quickstart/fig26.png)

*Afbeelding 27. Vergelijking van RMS-fouten voor de modellen.*

Met deze resultaten zien we dat de seizoensgebonden factoren toe te voegen aan het model de RMS-fout aanzienlijk vermindert. De RMS-fout voor het trainen van gegevens is niet te verrassend een bit kleiner is dan voor de prognose.

## <a id="appendixa"></a>Handleiding voor de RStudio-documentatie

RStudio wordt goed gedocumenteerd. Hier volgen enkele koppelingen naar de belangrijke onderdelen van de RStudio-documentatie aan de slag te gaan.

* **Het maken van projecten** -u kunt indelen en beheren van uw R-code in projecten met behulp van RStudio. Zie [projecten met behulp van](https://support.rstudio.com/hc/articles/200526207-Using-Projects) voor meer informatie. Ik aangeraden dat u als te werk volgt en maak een project voor de R-codevoorbeelden in dit artikel.  
* **Bewerken en uitvoeren van R-code** -RStudio biedt een geïntegreerde omgeving voor het bewerken en uitvoeren van R-code. Zie [bewerken en uitvoeren van Code](https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code) voor meer informatie.
* **Foutopsporing** -RStudio bevat krachtige mogelijkheden voor foutopsporing. Zie [foutopsporing met RStudio](https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio) voor meer informatie over deze functies. Zie voor meer informatie over probleemoplossing functies onderbrekingspunt [onderbrekingspunt probleemoplossing](https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting).

## <a id="appendixb"></a>Lees hier meer over

Deze programmeren R-zelfstudie bevat informatie over de basisprincipes van wat u moet de R-taal gebruiken met Azure Machine Learning Studio. Als u niet bekend met R bent, zijn twee inleidingen beschikbaar op CRAN:

* [R voor Beginners](http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf) Emmanuel Paradis is een goede plaats om te starten.  
* [Een inleiding tot R](http://cran.r-project.org/doc/manuals/R-intro.html) door W. N. Venables et. al. dieper in krijgt.

Er zijn veel boeken op R waarmee u aan de slag kunt. Hier volgen enkele die ik handig:

* De **illustraties van R-programmering: Een ontwerp rondleiding door de statistische Software** Norman Matloff is een uitstekende inleiding in programmeren in R.  
* **R Cookbook** door Paul Teetor biedt een oplossing en probleem aanpak voor het gebruik van R.  
* **R in actie** Robert Kabacoff is een ander handig inleidende rapport. De bijbehorende [snelle R website](http://www.statmethods.net/) is een nuttig middel.
* **R Inferno** door Patrick Burns is een verrassend humoristische boek die met een aantal lastig en moeilijk onderwerpen die kunnen worden aangetroffen bij het programmeren in R. Het rapport is beschikbaar voor gratis op [de R-Inferno](http://www.burns-stat.com/documents/books/the-r-inferno/).
* Als u een nauwkeuriger geavanceerde onderwerpen van R wilt, hebt u een overzicht van het boek **geavanceerde R** door Hadley Wickham. De online versie van deze handleiding vindt u gratis op [ http://adv-r.had.co.nz/ ](http://adv-r.had.co.nz/).

Een catalogus van R-pakketten voor time series kunt u vinden in [CRAN taak weergeven: Time Series-analyse](http://cran.r-project.org/web/views/TimeSeries.html). Voor informatie over specifieke tijd reeks object pakketten, moet u verwijzen naar de documentatie voor dit pakket.

Het boek **inleidende Time Series** met R door Paul Cowpertwait en Andrew Metcalfe bevat een inleiding tot het gebruik van R voor tijdseries analyseren. Veel meer theoretische teksten vindt u voorbeelden van R.

Hier volgen enkele fantastische resources op internet:

* DataCamp leert hoe u R in het gemak van uw browser met video lessen en codering oefeningen. Er zijn interactieve zelfstudies op de meest recente R technieken en pakketten. De gratis nemen [interactieve R zelfstudie](https://www.datacamp.com/courses/introduction-to-r).
* [Informatie over programmeren R, de definitieve handleiding](https://www.programiz.com/r-programming) van Programiz.
* Een snelle [R zelfstudie](http://www.cyclismo.org/tutorial/R/) door Kelly Black van Clarkson University.
* Er zijn gedurende 60 R-resources die worden vermeld op [boven R taalbronnen voor het verbeteren van de vaardigheden van uw gegevens](http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html).

<!-- Module References -->
[execute-r-script]: /azure/machine-learning/studio-module-reference/execute-r-script
