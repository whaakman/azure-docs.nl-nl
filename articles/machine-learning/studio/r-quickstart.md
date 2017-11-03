---
title: Quick Start-zelfstudie voor R-taal voor Machine Learning | Microsoft Docs
description: Gebruik deze R programming zelfstudie aan de slag snel met de taal R Azure Machine Learning Studio om een prognosemodel oplossing te maken.
keywords: Quick Start, r-taal, de programmeertaal r, programming r-zelfstudie
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: garye
ms.openlocfilehash: 40cc3728d1361b9304896bf0cc4ceed439291d45
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="quickstart-tutorial-for-the-r-programming-language-for-azure-machine-learning"></a>Beknopte zelfstudie voor programmeertaal R voor Azure Machine Learning

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Inleiding
Deze zelfstudie snel aan de slag kunt u snel starten Azure Machine Learning met behulp van de programmeertaal R uitbreiden. Volg deze programming R-zelfstudie voor het maken, testen en R-code in Azure Machine Learning uitvoeren. Als u de zelfstudie doorloopt, maakt u een volledige prognoses oplossing met behulp van de R-taal in Azure Machine Learning.  

Microsoft Azure Machine Learning bevat veel krachtige machine learning en manipulatie modules. De krachtige R taal heeft als lingua franca van analytics zijn beschreven. Manipulatie analytics en gegevens in Azure Machine Learning kan worden uitgebreid met R. Deze combinatie biedt de schaalbaarheid en het gemak van implementatie van Azure Machine Learning met de flexibiliteit en grondige analyse van R.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-the-dataset"></a>Prognose en de gegevensset
Prognose is veel werknemers en wel handig analytische methode. Algemeen gebruik bereik van het voorspellen van de verkoop van seizoensgebonden items, optimaal voorraadbeheer te voorspellen macro-economische variabelen te bepalen. Prognose gewoonlijk wordt uitgevoerd met time series-modellen.

Reeks tijdgegevens zijn gegevens waarin de waarden een tijdsindex hebben. De tijdsindex worden regelmatig, bijvoorbeeld elke maand of elke minuut of onregelmatig. Een tijdreeksmodel is gebaseerd op een reeksgegevens. Het R programmeertaal bevat een flexibele framework en de uitgebreide analytics voor tijd reeksgegevens.

In deze snelstartgids wordt worden werken met Californië zuivelproductie en prijzen van gegevens. Deze gegevens omvatten maandelijkse informatie op de productie van verschillende producten en de prijs van melkvet, een benchmark basisproduct.

De gegevens die worden gebruikt in dit artikel, samen met het R-scripts kunnen worden [hier gedownload][download]. Deze gegevens is oorspronkelijk gemaakt van gegevens op de universiteit Wisconsin op http://future.aae.wisc.edu/tab/production.html.

### <a name="organization"></a>Organisatie
We zullen verschillende stappen doorlopen als u meer informatie over het maken, testen en uitvoeren van analyses en gegevens manipulatie R-code in de Azure Machine Learning-omgeving.  

* Eerst verkennen we de basisbeginselen van het gebruik van de R-taal in de Azure Machine Learning Studio-omgeving.
* Vervolgens wordt de voortgang bespreking van de verschillende aspecten van i/o voor gegevens, R-code en afbeeldingen in de Azure Machine Learning-omgeving.
* We wordt vervolgens het eerste deel van onze prognoses oplossing maken door het maken van code voor het reinigen van gegevens en transformatie.
* Met onze gegevens voorbereid wordt uitgevoerd, er een analyse van de correlatie tussen verschillende van de variabelen in onze gegevensset.
* We gaan ten slotte een seizoen prognoses tijdreeksmodel voor maken.

## <a id="mlstudio"></a>Interactie met R-taal in Machine Learning Studio
Deze sectie leert u enkele van de basisprincipes van de interactie met de programmeertaal R in de omgeving Machine Learning Studio. De taal R biedt een krachtig hulpprogramma om aangepaste analytics en data manipulatie modules in de Azure Machine Learning-omgeving te maken.

Ik gebruik RStudio ontwikkelen, testen en foutopsporing van R-code op kleine schaal. Deze code wordt vervolgens knippen en plakken in een [R-Script uitvoeren] [ execute-r-script] module in Machine Learning Studio om uit te voeren.  

### <a name="the-execute-r-script-module"></a>De module R-Script uitvoeren
In Machine Learning Studio R scripts worden uitgevoerd binnen de [R-Script uitvoeren] [ execute-r-script] module. Een voorbeeld van de [R-Script uitvoeren] [ execute-r-script] module in Machine Learning Studio wordt weergegeven in afbeelding 1.

 ![R programmeertaal: R voor het uitvoeren van Script-module is geselecteerd in Machine Learning Studio][1]

*Afbeelding 1. De Machine Learning Studio-omgeving met de module R-Script uitvoeren is geselecteerd.*

Verwijzen naar afbeelding 1, bekijken we enkele van de belangrijkste onderdelen van de Machine Learning Studio-omgeving voor het werken met de [R-Script uitvoeren] [ execute-r-script] module.

* De modules in het experiment worden in het middelste deelvenster weergegeven.
* Het bovenste gedeelte van het rechter deelvenster bevat een venster voor het weergeven en bewerken van uw R-scripts.  
* Het onderste gedeelte van het rechter deelvenster ziet u enkele eigenschappen van de [R-Script uitvoeren][execute-r-script]. U kunt de logboeken van de fout en uitvoer weergeven door te klikken op de juiste plaatsen van dit deelvenster.

We zullen natuurlijk bespreken de [R-Script uitvoeren] [ execute-r-script] in meer detail in de rest van dit document.

Als u werkt met complexe R-functies, ik raden aan dat u bewerkt, testen en fouten in RStudio opsporen. Net als bij de ontwikkeling van alle software uw code stapsgewijs uitbreiden en het testen op kleine simpele testcases. Vervolgens knippen en plakken van uw functies in het venster R-script van de [R-Script uitvoeren] [ execute-r-script] module. Deze aanpak kunt u zowel de RStudio integrated development environment (IDE) en de kracht van Azure Machine Learning benutten.  

#### <a name="execute-r-code"></a>R code uitvoeren
Geen R-code in de [R-Script uitvoeren] [ execute-r-script] module wordt uitgevoerd wanneer u het experiment uitvoeren door te klikken op de **uitvoeren** knop. Wanneer de uitvoering is voltooid, een vinkje worden weergegeven op de [R-Script uitvoeren] [ execute-r-script] pictogram.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Defensive R coderen voor Azure Machine Learning
Als u R-code voor, bijvoorbeeld een web-service ontwikkelt met behulp van Azure Machine Learning, moet u zeker plannen hoe uw code wordt omgaan met een onverwachte gegevensinvoer en uitzonderingen. Als u wilt behouden duidelijkheid, ik niet opgenomen veel heeft op het gebied met het controleren of de afhandeling van uitzonderingen in de meeste van de codevoorbeelden die wordt weergegeven. Echter, als we gaan ik krijgt u enkele voorbeelden van functies met behulp van de mogelijkheid voor het verwerken van het R-uitzondering.  

Als u een uitgebreidere behandeling van R uitzonderingsverwerking moet, ik lezen van de betreffende gedeelten van het rapport door Wickham die worden vermeld in het beste [bijlage B - verdere lezen](#appendixb).

#### <a name="debug-and-test-r-in-machine-learning-studio"></a>Debuggen en testen van R in Machine Learning Studio
Als u wilt vast, aanbevelen ik u testen en foutopsporing van uw R-code op kleine schaal in RStudio. Er zijn echter gevallen waarbij u moet voor het opsporen van R code in de [R-Script uitvoeren] [ execute-r-script] zelf. Daarnaast is het raadzaam om te controleren van de resultaten in Machine Learning Studio.

De uitvoer van de uitvoering van uw R-code en op het Azure Machine Learning-platform is voornamelijk in uitvoer.log gevonden. Aanvullende informatie wordt weergegeven in error.log.  

Als een fout in Machine Learning Studio optreedt tijdens het uitvoeren van uw code R, moet uw eerste training van actie om te kijken naar error.log. Dit bestand kunt nuttige foutberichten om te begrijpen en corrigeer de fout bevatten. Als u wilt error.log weergeven, klikt u op **foutenlogboek weergeven** op de **eigenschappendeelvenster** voor de [R-Script uitvoeren] [ execute-r-script] met de fout.

Bijvoorbeeld ik de volgende R-code wordt uitgevoerd met een niet-gedefinieerde variabele y in een [R-Script uitvoeren] [ execute-r-script] module:

    x <- 1.0
    z <- x + y

Deze code kan niet worden uitgevoerd, wat resulteert in een fout opgetreden. Te klikken op **foutenlogboek weergeven** op de **eigenschappendeelvenster** resulteert in de weergave wordt weergegeven in afbeelding 2.

  ![Foutbericht pop up][2]

*Afbeelding 2. Foutbericht pop-upvenster.*

Het lijkt erop dat we moet zoeken in uitvoer.log om de R-foutbericht te zien. Klik op de [R-Script uitvoeren] [ execute-r-script] en klik vervolgens op de **uitvoer.log weergeven** item op de **eigenschappendeelvenster** naar rechts. Een nieuw browservenster wordt geopend en ik Zie de volgende.

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Dit foutbericht bevat geen verrassingen en duidelijk identificeert het probleem.

Om te controleren van de waarde van een object in R, kunt u deze waarden naar het bestand uitvoer.log afdrukken. De regels voor het onderzoeken van objectwaarden zijn in wezen hetzelfde als in een interactieve R-sessie. Als u de naam van een variabele op een regel typt, wordt bijvoorbeeld de waarde van het object afgedrukt naar het bestand uitvoer.log.  

#### <a name="packages-in-machine-learning-studio"></a>Pakketten in Machine Learning Studio
Azure Machine Learning wordt geleverd met meer dan 350 vooraf geïnstalleerde R-taalpakketten. U kunt de volgende code in de [R-Script uitvoeren] [ execute-r-script] module voor het ophalen van een lijst van de vooraf geïnstalleerde pakketten.

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

Als u niet de laatste regel van deze code op het moment dat begrijpt, kunt u lezen op. In de rest van dit document wordt uitvoerig besproken R gebruiken in de Azure Machine Learning-omgeving.

### <a name="introduction-to-rstudio"></a>Inleiding tot RStudio
RStudio is een veelgebruikte IDE voor R. Ik gebruik RStudio bewerken, testen en foutopsporing enkele van de R-code die wordt gebruikt in deze snelstartgids. Zodra de R-code is getest en klaar zijn, u gewoon knippen en plakken in de editor RStudio naar een Machine Learning Studio [R-Script uitvoeren] [ execute-r-script] module.  

Als u niet de R-programmeertaal op uw computer geïnstalleerd hebt, ik het beste dat u doet u dat nu. Gratis downloads van open-source R taal zijn beschikbaar op de uitgebreide R archief netwerk (CRAN) op [http://www.r-project.org/](http://www.r-project.org/). Er zijn downloads beschikbaar voor Windows, Mac OS- en Linux/UNIX. Kies een mirror in de buurt en volg de aanwijzingen downloaden. CRAN bevat bovendien een schat aan nuttig analytics en data manipulatie pakketten.

Als u niet bekend met RStudio bent, moet u downloadt en installeert de bureaubladversie. U vindt dat de RStudio downloads voor Windows-, Mac OS- en Linux/UNIX op http://www.rstudio.com/products/RStudio/. Volg de aanwijzingen voor het installeren van RStudio op uw computer.  

Een zelfstudie Inleiding tot RStudio is beschikbaar op https://support.rstudio.com/hc/sections/200107586-Using-RStudio.

Ik aanvullende informatie geven over het gebruik van RStudio in [bijlage A][appendixa].  

## <a id="scriptmodule"></a>Ophalen van gegevens in en uit de module R-Script uitvoeren
In deze sectie bespreken we hoe u gegevens ophalen naar en van de [R-Script uitvoeren] [ execute-r-script] module. Bespreken we hoe u voor het afhandelen van verschillende soorten gegevens gelezen in en buiten de [R-Script uitvoeren] [ execute-r-script] module.

De volledige code voor deze sectie wordt het zip-bestand dat u eerder hebt gedownload.

### <a name="load-and-check-data-in-machine-learning-studio"></a>Laden en controleren van de gegevens in Machine Learning Studio
#### <a id="loading"></a>Laden van de gegevensset
Er wordt gestart door bij het laden van de **csdairydata.csv** -bestand in Azure Machine Learning Studio.

* Start uw Azure Machine Learning Studio-omgeving.
* Klik op **+ nieuw** in de linkerbenedenhoek van het scherm en selecteer **gegevensset**.
* Selecteer **uit lokale bestand**, en vervolgens **Bladeren** om het bestand te selecteren.
* Zorg ervoor dat u hebt geselecteerd **generieke CSV-bestand met de header (.csv)** als het type voor de gegevensset.
* Klik op het vinkje.
* Nadat de gegevensset zijn geüpload, ziet u de nieuwe gegevensset door te klikken op de **gegevenssets** tabblad.  

#### <a name="create-an-experiment"></a>Een experiment maken
Nu we hebben enkele gegevens in Machine Learning Studio, moet we maken van een experiment voor de analyse wilt uitvoeren.  

* Klik op **+ nieuw** op de lagere links en selecteer **Experiment**, klikt u vervolgens **leeg Experiment**.
* U kunt uw experiment naam door te selecteren en te wijzigen, de **Experiment op wordt gemaakt...**  titel boven aan de pagina. Bijvoorbeeld wijzigen naar **CA Zuivel Analysis**.
* Vouw op de linkerkant van de pagina experiment **gegevenssets opgeslagen**, en vervolgens **mijn gegevenssets**. U ziet de **cadairydata.csv** die u eerder hebt geüpload.
* Slepen en neerzetten de **csdairydata.csv gegevensset** naar het experiment.
* In de **zoeken items experimenteren** vak boven aan het linkerdeelvenster type [R-Script uitvoeren][execute-r-script]. Hier ziet u de module worden weergegeven in de zoeklijst.
* Slepen en neerzetten de [R-Script uitvoeren] [ execute-r-script] module naar uw palet.  
* Verbinding maken met de uitvoer van de **csdairydata.csv gegevensset** naar de meest linkse invoer (**Dataset1**) van de [R-Script uitvoeren][execute-r-script].
* **Vergeet niet op 'Opslaan' te klikken.**  

Op dit moment ziet uw experiment er ongeveer als afbeelding 3.

![De CA Zuivel analyse experimenteren met de gegevensset en module R-Script uitvoeren][3]

*Afbeelding 3. De CA Zuivel analyse experimenteren met gegevensset en module R-Script niet uitvoeren.*

#### <a name="check-on-the-data"></a>Controleer op de gegevens
We hebben een overzicht van de gegevens die we in ons experiment hebben geladen. Klik in het experiment op in de uitvoer van de **cadairydata.csv gegevensset** en selecteer **visualiseren**. U ziet er ongeveer zo afbeelding 4.  

![Samenvatting van de gegevensset cadairydata.csv][4]

*Afbeelding 4. Samenvatting van de gegevensset cadairydata.csv.*

In deze weergave ziet u een groot aantal nuttige informatie. U ziet de eerste verschillende rijen van deze dataset. Als we een kolom selecteert, ziet u de sectie statistieken meer informatie over de kolom. Bijvoorbeeld, staan de rij onderdeeltype ons welke gegevenstypen Azure Machine Learning Studio aan de kolom toegewezen. Een goede controle met een kort overzicht als volgt worden is voordat we ernstige werk.

### <a name="first-r-script"></a>Eerste R-script
We gaan een eenvoudige eerste R-script om te experimenteren met in Azure Machine Learning Studio maken. Ik heb gemaakt en getest met het volgende script in RStudio.  

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

Nu moet ik dit script overdragen naar Azure Machine Learning Studio. Ik kan eenvoudig knippen en plakken. Echter, in dit geval ik draagt mijn R-script via een zip-bestand.

### <a name="data-input-to-the-execute-r-script-module"></a>Gegevensinvoer voor de module R-Script uitvoeren
We hebben een overzicht van de invoer voor de [R-Script uitvoeren] [ execute-r-script] module. In dit voorbeeld leest de Californië melkkoeien gegevens in de [R-Script uitvoeren] [ execute-r-script] module.  

Er zijn drie mogelijke ingangen voor de [R-Script uitvoeren] [ execute-r-script] module. U kunt een of meer van deze invoer, afhankelijk van uw toepassing. Het is ook perfect redelijke een R-script waarmee geen invoer helemaal te gebruiken.  

We bekijken elk van deze invoer, die van links naar rechts. U ziet de namen van elk van de invoerwaarden door plaatst u de cursor op de invoer en de knopinfo te lezen.  

#### <a name="script-bundle"></a>Script bundel
De Script-bundel invoer kunt u de inhoud van een zip-bestand in doorgeven [R-Script uitvoeren] [ execute-r-script] module. U kunt een van de volgende opdrachten gebruiken om de inhoud van het zip-bestand in uw R-code.

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> Azure Machine Learning-bestanden in het ZIP-bestand wordt behandeld alsof ze zich in de src / Active directory, dus moet u uw bestandsnamen met deze directorynaam voorvoegsel. Bijvoorbeeld, als het ZIP-bestand bevat de bestanden `yourfile.R` en `yourData.rdata` in de hoofdmap van het ZIP-bestand, zou u deze poorten als adresseren `src/yourfile.R` en `src/yourData.rdata` bij gebruik van `source` en `load`.
> 
> 

Besproken al gegevenssets laden in [bij het laden van de gegevensset](#loading). Zodra u hebt gemaakt en getest het R-script dat wordt weergegeven in de vorige sectie, het volgende doen:

1. Sla het R-script in een. R-bestand. Ik mijn scriptbestand 'simpleplot aanroepen. R'. Hier volgt de inhoud.
   
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
2. Maak een zip-bestand en kopieer het script naar dit zipbestand. Op Windows, kunt u met de rechtermuisknop op het bestand en selecteer **verzenden naar**, en vervolgens **gecomprimeerde map**. Hiermee maakt u een nieuwe zip-bestand met de 'simpleplot. R'-bestand.
3. Toevoegen van het bestand naar de **gegevenssets** in Machine Learning Studio, op te geven als **zip**. U ziet nu het zip-bestand in de gegevenssets.
4. Slepen en neerzetten van het zip-bestand van **gegevenssets** naar de **ML Studio canvas**.
5. Verbinding maken met de uitvoer van de **zip-gegevens** pictogram voor de **Script bundel** invoer van de [R-Script uitvoeren] [ execute-r-script] module.
6. Typ de `source()` functie met de naam van uw zip-bestand in het codevenster voor de [R-Script uitvoeren] [ execute-r-script] module. Ik heb getypt in mijn geval `source("src/simpleplot.R")`.  
7. Controleer of u **opslaan**.

Als deze stappen voltooid zijn, de [R-Script uitvoeren] [ execute-r-script] module het R-script in het zip-bestand wordt uitgevoerd wanneer het experiment wordt uitgevoerd. Op dit moment ziet uw experiment er ongeveer als afbeelding 5.

![Experimenteer met gecomprimeerde R-script][6]

*Afbeelding 5. Experimenteer met gecomprimeerde R-script.*

#### <a name="dataset1"></a>Dataset1
U kunt een rechthoekig gegevenstabel doorgeven aan uw R-code met behulp van de invoer Dataset1. In onze eenvoudig script de `maml.mapInputPort(1)` functie leest de gegevens van poort 1. Deze gegevens wordt vervolgens toegewezen aan een dataframe variabelenaam in uw code. In onze eenvoudig script voert de eerste coderegel de toewijzing.

    cadairydata <- maml.mapInputPort(1)

Uw experiment uitvoeren door te klikken op de **uitvoeren** knop. Als de uitvoering is voltooid, klikt u op de [R-Script uitvoeren] [ execute-r-script] module en klik vervolgens op **weergave logboek** in het eigenschappendeelvenster. Een nieuwe pagina moet worden weergegeven in uw browser met daarin de inhoud van het bestand uitvoer.log. Wanneer u omlaag schuiven ziet u ongeveer als volgt.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Verder omlaag op de pagina is dat meer gedetailleerde informatie over de kolommen, ziet er ongeveer als volgt.

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

Deze resultaten zijn voornamelijk zoals verwacht, met 228 opmerkingen en 9 kolommen in de dataframe. U ziet de kolomnamen, het gegevenstype R en een voorbeeld van elke kolom.

> [!NOTE]
> Deze dezelfde afdruk is gemakkelijk beschikbaar vanuit de uitvoer van het R-apparaat van de [R-Script uitvoeren] [ execute-r-script] module. Bespreken we de uitvoer van de [R-Script uitvoeren] [ execute-r-script] module in de volgende sectie.  
> 
> 

#### <a name="dataset2"></a>Dataset2
Het gedrag van de invoer Dataset2 is identiek aan die van Dataset1. U kunt met behulp van deze invoer een tweede rechthoekige gegevenstabel doorgeven in uw R-code. De functie `maml.mapInputPort(2)`, met het argument 2 wordt gebruikt deze gegevens moeten worden doorgegeven.  

### <a name="execute-r-script-outputs"></a>De uitvoer R-Script uitvoeren
#### <a name="output-a-dataframe"></a>Een dataframe uitvoer
Kunt u de inhoud van een R-dataframe als een rechthoekig tabel via de poort resultaat Dataset1 uitvoeren met behulp van de `maml.mapOutputPort()` functie. In onze eenvoudig R-script wordt uitgevoerd door de volgende regel.

    maml.mapOutputPort('cadairydata')

Nadat het experiment is uitgevoerd, klikt u op de uitvoerpoort Dataset1 resultaat en klik vervolgens op **Visualize**. U ziet er ongeveer zo afbeelding 6.

![De visualisatie van de uitvoer van de Californië melkkoeien gegevens][7]

*Afbeelding 6. De visualisatie van de uitvoer van de Californië melkkoeien gegevens.*

Deze uitvoer ziet er identiek is aan de invoer, precies zoals verwacht.  

### <a name="r-device-output"></a>R-apparaat-uitvoer
De uitvoer van het apparaat van de [R-Script uitvoeren] [ execute-r-script] -module bevat de uitvoer van berichten en afbeeldingen. Beide standaard uitvoer en de standaardfout van de berichten van R worden verzonden naar de uitvoerpoort R-apparaat.  

Weergave van de uitvoer van het R-apparaat, klikt u op de poort en klik vervolgens op **Visualize**. De standaarduitvoer en de standaardfout van het R-script in afbeelding 7 ziet.

![Standaarduitvoer en de standaardfout van de poort R-apparaat][8]

*Afbeelding 7. Standaarduitvoer en de standaardfout van de poort R-apparaat.*

De uitvoer van de afbeeldingen van onze R-script in afbeelding 8 verschuift we zien.  

![Grafische uitvoer van de poort R-apparaat][9]

*Afbeelding 8. Afbeeldingen de uitvoer van de poort R-apparaat.*  

## <a id="filtering"></a>Gegevens filteren en transformatie
In deze sectie wordt uitgevoerd, er enkele elementaire gegevens filteren en transformatiebewerkingen op de Californië melkkoeien gegevens. Aan het einde van deze sectie hebben we gegevens in een indeling die geschikt is voor het bouwen van een analytische model.  

Meer specifiek, in deze sectie wordt we enkele algemene gegevens opruimen en transformatie taken uitvoeren: type transformatie, filteren op dataframes, nieuwe berekende kolommen, toe te voegen en de waarde van transformaties. Deze achtergrond kunt u te maken met de veel variaties aangetroffen in de praktische problemen.

De volledige R-code voor deze sectie is beschikbaar in het zip-bestand dat u eerder hebt gedownload.

### <a name="type-transformations"></a>Type transformaties
Nu dat we de Californië melkkoeien gegevens kunnen lezen in de R-code in de [R-Script uitvoeren] [ execute-r-script] -module, moeten we ervoor zorgen dat de gegevens in de kolommen het gewenste type en de indeling.  

R is een dynamisch getypeerde taal, wat betekent dat gegevenstypen worden gedwongen uit een naar een andere zoals vereist. De atomic gegevenstypen in R bevatten numerieke waarden, logische en -teken. Het type factor wordt gebruikt voor het opslaan van compactly categorische gegevens. U vindt meer informatie over gegevenstypen in de verwijzingen in [bijlage B – Lees hier meer over](#appendixb).

Wanneer tabelgegevens wordt gelezen in R uit een externe bron, maar het is altijd een goed idee om te controleren van de resulterende typen in de kolommen. U kunt een kolom van het typeteken, maar in veel gevallen dit wordt weergegeven als factor of vice versa. In andere gevallen een kolom die u denkt dat moet worden wordt numerieke vertegenwoordigd door tekengegevens, bijvoorbeeld '1,23' in plaats van 1,23 als een zwevende-kommagetal.  

Het is gelukkig eenvoudig één type converteren naar een andere, zolang de toewijzing is mogelijk. Bijvoorbeeld, u 'Nevada' niet converteren naar een numerieke waarde, maar u kunt deze converteren naar een factor (categorische variabele). U kunt bijvoorbeeld een numerieke 1 omzetten in een teken '1' of een factor.  

De syntaxis voor een van deze omzettingen is eenvoudig: `as.datatype()`. Deze functies voor typeconversie biedt de volgende.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Kijken naar de gegevenstypen van de kolommen die we in de vorige sectie invoer: alle kolommen van het type numeriek zijn, met uitzondering van de kolom met het label 'Maand', van het typeteken is zijn. Laten we dit converteren naar een factor en test de resultaten.  

Ik heb hebt de regel die de scatterplot matrix gemaakt en een regel die de kolom 'Maand' converteren naar een factor toegevoegd verwijderd. In mijn experiment ik net knippen en plak de code R in het codevenster van de [R-Script uitvoeren] [ execute-r-script] Module. U kunt ook het zip-bestand bijwerken en uploaden naar Azure Machine Learning Studio, maar dit verschillende stappen nodig.  

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

Laten we deze code uitvoeren en bekijk het uitvoerlogboek voor voor het R-script. De relevante gegevens uit het logboek wordt weergegeven in afbeelding 9.

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

*Afbeelding 9. Samenvatting van de dataframe met een factor-variabele.*

Het type voor de maand nu de melding '**Factor met 14 niveaus**'. Dit is een probleem, omdat er alleen 12 maanden in een jaar. U kunt ook controleren om te zien die het type in **Visualize** van de gegevensset resultaat poort is '**Categorical**'.

Het probleem is dat de kolom 'Maand' niet systematischer heeft is gecodeerd. In sommige gevallen een maand April wordt genoemd, en in andere gevallen wordt het april afgekort. We kunnen dit probleem oplossen door de tekenreeks die moet 3 tekens. De regel code ziet er nu als volgt uit:

    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

Het experiment opnieuw uitvoeren en bekijk het uitvoerlogboek voor. De verwachte resultaten worden weergegeven in afbeelding 10.  

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

*Afbeelding 10. Samenvatting van de dataframe met het juiste aantal factorniveaus.*

Onze variabele factor heeft nu de gewenste 12 niveaus.

### <a name="basic-data-frame-filtering"></a>Basisgegevens frame filteren
R dataframes ondersteunen krachtige filteropties. Gegevenssets kan subset met logische filters op rijen of kolommen zijn. In veel gevallen zijn complexe filtercriteria vereist. De verwijzingen in [bijlage B – Lees hier meer over](#appendixb) uitgebreide voorbeelden voor het filteren van dataframes bevatten.  

Er is één bit van het filter moet worden uitgevoerd op onze gegevensset. Als u de kolommen in de dataframe cadairydata bekijkt, ziet u twee overbodige kolommen. De eerste kolom bevat alleen een rijnummer, niet erg nuttig is. De tweede kolom Year.Month, bevat redundante gegevens. We kunnen deze kolommen eenvoudig uitsluiten met behulp van de volgende R-code.

> [!NOTE]
> Voortaan in deze sectie net leest u de aanvullende code ik toe te voegen in de [R-Script uitvoeren] [ execute-r-script] module. Ik zal elke nieuwe regel toevoegen **voordat** de `str()` functie. Ik gebruik deze functie om mijn resultaten in Azure Machine Learning Studio te controleren.
> 
> 

Ik de volgende regel toevoegen aan mijn R-code in de [R-Script uitvoeren] [ execute-r-script] module.

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

Deze code in uw experiment uitvoeren en controleer het resultaat van het uitvoerlogboek voor. Deze resultaten worden weergegeven in afbeelding 11.

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

*Afbeelding 11. Samenvatting van de dataframe met twee kolommen die zijn verwijderd.*

Goed nieuws! We krijgt de verwachte resultaten.

### <a name="add-a-new-column"></a>Een nieuwe kolom toevoegen
Als u wilt maken van de time series modellen zal zijn handig zijn als een kolom met de maanden sinds de start van de tijdreeks. We gaan een nieuwe kolom 'Month.Count' maken.

Voor het ordenen van de code maken we onze eerste eenvoudige functie `num.month()`. Deze functie om een nieuwe kolom maken in de dataframe wordt vervolgens toegepast. De nieuwe code is als volgt.

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

Nu de bijgewerkte experiment uitvoeren en de logboeken van de uitvoer gebruikt om de resultaten weer te geven. Deze resultaten worden weergegeven in afbeelding 12.

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

*Afbeelding 12. Samenvatting van de dataframe met de extra kolom.*

Het lijkt dat alles werkt. We hebben de nieuwe kolom met de verwachte waarden in onze dataframe.

### <a name="value-transformations"></a>Waarde transformaties
In deze sectie zullen we enkele eenvoudige transformaties uitvoeren van de waarden in een aantal van de kolommen van onze dataframe. De R-taal ondersteunt bijna willekeurige waarde transformaties. De verwijzingen in [bijlage B - verdere lezen](#appendixb) uitgebreide voorbeelden bevatten.

Als u de waarden in de samenvatting van onze dataframe bekijkt ziet u iets oneven hier. Is meer ijs dan melk geproduceerd in Californië? Nee, natuurlijk omdat dit geen zin jammer als dit feit kunnen niet tot een aantal van ons ijs Ontwikkelaarshulpmiddelen. De eenheden zijn verschillend. De prijs is in eenheden van ons pond, melk is in eenheden van 1 M VS pond, ijs in eenheden van 1000 ons gallon en Kwark in eenheden van 1000 VS pond is. Ervan uitgaande dat ijs weegt ongeveer 6,5 pond per gallon, kunnen eenvoudig doen we de vermenigvuldiging als u wilt converteren van deze waarden, zodat ze allemaal in gelijk eenheden van 1000 pond.

We gebruiken een Multiplicatieve model voor het trendanalyse en correctie van deze gegevens voor onze prognosemodel. Een transformatie logboek kan we gebruiken een lineaire model, dit proces vereenvoudigen. De transformatie logboek in dezelfde functie waarin de vermenigvuldiger is toegepast kan worden toegepast.

In de volgende code ik een nieuwe functie definiëren `log.transform()`, en dit toepassen op de rijen met numerieke waarden. Het R `Map()` functie wordt gebruikt om toe te passen de `log.transform()` functie met de geselecteerde kolommen van de dataframe. `Map()`is vergelijkbaar met `apply()` maar kunt u meer dan een lijst met argumenten voor de functie. Houd er rekening mee dat een lijst met ze het tweede argument levert de `log.transform()` functie. De `na.omit()` functie als een stukje opschonen wordt gebruikt om te controleren of er geen ontbreekt of niet-gedefinieerde waarden in de dataframe.

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

Er is zeer bits gebeurt in de `log.transform()` functie. De meeste van deze code controleert op mogelijke problemen met de argumenten of omgaan met uitzonderingen die nog steeds tijdens de berekeningen optreden kunnen. Slechts een paar regels van deze code doen daadwerkelijk berekeningen.

Het doel van de defensive programmering is om te voorkomen dat het uitvallen van één functie die voorkomt dat de verwerking niet door kan gaan. Een plotselinge storing van een analyse langlopende kan frustrerend behoorlijk voor gebruikers. Om deze situatie te voorkomen, moeten standaard retourwaarden worden gekozen dat schade aan de downstreamverwerking wordt beperkt. Een bericht wordt ook gemaakt worden gebruikers geïnformeerd dat er iets een opgetreden fout is.

Als u niet aan defensive programmering in R worden gebruikt, zijn alle deze code lijkt enigszins overweldigend. Ik begeleidt u stapsgewijs door de belangrijkste stappen:

1. Een vector van vier berichten is gedefinieerd. Deze berichten worden gebruikt om informatie over een aantal van de mogelijke fouten en uitzonderingen die zich met deze code voordoen kunnen te communiceren.
2. Ik retourneren NA de waarde voor elk geval. Er zijn tal van andere mogelijkheden die mogelijk minder neveneffecten hebben. Ik kan een vector van nullen of de oorspronkelijke invoer vector bijvoorbeeld retourneren.
3. Controles worden uitgevoerd op de argumenten voor de functie. In elk geval als een fout wordt gedetecteerd, een standaardwaarde geretourneerd en wordt een bericht wordt geproduceerd door de `warning()` functie. Ik gebruik `warning()` plaats `stop()` als de laatste uitvoering beëindigt, precies wat ik probeer om te voorkomen. Houd er rekening mee dat ik hebt geschreven deze code in een procedurele style, zoals in dit geval een functionele benadering leek complex en verborgen.
4. De logboek-berekeningen zijn samengevoegd in `tryCatch()` zodat uitzonderingen niet een abrupte stoppen op de verwerking tot leidt. Zonder `tryCatch()` de meeste fouten die worden gegenereerd door R functies resulteren in een stopsignaal ontvangen die geregeld worden.

Deze R-code in uw experiment uitvoeren en bekijk de uitvoer in het bestand uitvoer.log hebben. U ziet nu de omgezette waarden van de vier kolommen in het logboek, zoals wordt weergegeven in afbeelding 13.

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

*Afbeelding 13. Samenvatting van de getransformeerde waarden in de dataframe.*

Ziet u de waarden zijn getransformeerd. Melkproductie nu is aanzienlijk groter dan alle andere melkkoeien product productie, terughalen dat we nu een logaritmische schaal kijkt.

Op dit moment onze gegevens wordt opgeschoond en we klaar zijn voor sommige modellen. Kijken naar de samenvatting voor de uitvoer resultaat gegevensset van visualisatie onze [R-Script uitvoeren] [ execute-r-script] module, ziet u de kolom 'Maand' is 'Categorical' met 12 unieke waarden, opnieuw, net zoals we wilden.

## <a id="timeseries"></a>Time series-objecten en correlatieanalyse
In deze sectie wordt een paar basic R time series-objecten te verkennen en analyseren van de correlatie tussen enkele van de variabelen. Ons doel is om uit te voeren een dataframe waarin de pairwise correlatie-gegevens op verschillende lag.

De volledige R-code voor deze sectie wordt het zip-bestand dat u eerder hebt gedownload.

### <a name="time-series-objects-in-r"></a>Time series-objecten in R
Zoals al is vermeld, tijd reeksen al een reeks gegevenswaarden geïndexeerd door tijd zijn. R time series-objecten worden gebruikt voor het maken en beheren van de tijdsindex. Er zijn enkele voordelen van time series-objecten. Time series-objecten vrij u van veel meer informatie over het beheren van de reeks index tijdwaarden die worden ingekapseld in het object. Bovendien kunnen time series-objecten u het veel tijd reeks methoden gebruiken voor het uitzetten, afdrukken, modellering, enzovoort.

De POSIXct time series-klasse wordt vaak gebruikt en relatief eenvoudig is. Deze tijdreeks klasse metingen tijd vanaf het begin van de epoche, 1 januari 1970. In dit voorbeeld gebruiken we POSIXct time series-objecten. Andere klassen gebruikte R time series-object zijn zoo en xts, uitbreidbare tijdreeks.
<!-- Additional information on R time series objects is provided in the references in Section 5.7. [commenting because this section doesn't exist, even in the original] -->

### <a name="time-series-object-example"></a>Voorbeeld van een reeks object tijd
Laten we aan de slag met het voorbeeld. Slepen en neerzetten een **nieuwe** [R-Script uitvoeren] [ execute-r-script] module in uw experiment. Verbinding maken met de uitvoerpoort Dataset1 resultaat van de bestaande [R-Script uitvoeren] [ execute-r-script] module die u wilt de Dataset1 poort van de nieuwe invoer [R-Script uitvoeren] [ execute-r-script] module.

Zoals ik deed voor de eerste voorbeelden zoals we het voorbeeld doorlopen, op bepaalde tijdstippen leest alleen de incrementele extra regels met code R bij elke stap.  

#### <a name="reading-the-dataframe"></a>Lezen van de dataframe
Laten we als eerste stap in een dataframe gelezen en controleer of krijgen we de verwachte resultaten. De taak moet doen, de volgende code.

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check the results

Voer nu het experiment. Het logboek van de nieuwe vorm van R-Script uitvoeren moet eruitzien als afbeelding 14.

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

*Afbeelding 14. Samenvatting van de dataframe in de module R-Script niet uitvoeren.*

Deze gegevens is van het verwachte typen en de indeling. Houd er rekening mee dat de kolom "Maand" type factor en het verwachte aantal niveaus heeft.

#### <a name="creating-a-time-series-object"></a>Een time series-object maken
Er moet een time series-object toevoegen aan onze dataframe. De huidige code vervangen door de volgende, waarmee een nieuwe kolom van klasse POSIXct worden toegevoegd.

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check the results

Controleer nu het logboek. Het moet eruitzien als afbeelding 15.

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

*Afbeelding 15. Samenvatting van de dataframe met een time series-object.*

We kunt in het overzicht dat de nieuwe kolom die in feite van klasse POSIXct is zien.

### <a name="exploring-and-transforming-the-data"></a>Verkennen en de gegevens transformeren
We gaan verkennen enkele van de variabelen in deze dataset. Een matrix scatterplot is een prima manier voor het produceren van een beknopte beschrijving. Ik ben vervangen de `str()` functie in de vorige code R met de volgende regel.

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

Voer deze code en kijk wat er gebeurt. De grafiek geproduceerd op de poort R-apparaat moet eruitzien als afbeelding 16.

![Matrix van geselecteerde variabelen Scatterplot][17]

*Afbeelding 16. Scatterplot matrix van geselecteerde variabelen.*

Er is een aantal vreemd uitziet structuur in de relaties tussen deze variabelen. Hierdoor ontstaat mogelijk van trends in de gegevens en van het feit dat we hebben de variabelen niet gestandaardiseerd.

### <a name="correlation-analysis"></a>Correlatieanalyse
Als u wilt de analyse van de correlatie moeten we ongedaan trend en te standaardiseren, de variabelen. Het R kan alleen worden gebruikt `scale()` functie, die zowel datacenters en het schalen van variabelen. Deze functie mogelijk ook sneller worden uitgevoerd. Ik wil echter ziet u een voorbeeld van defensive programing in R.

De `ts.detrend()` functie hieronder beide van deze bewerkingen uitvoert. De volgende twee regels code ongedaan trend van de gegevens en vervolgens de waarden te standaardiseren.

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

Er is zeer bits gebeurt in de `ts.detrend()` functie. De meeste van deze code controleert op mogelijke problemen met de argumenten of omgaan met uitzonderingen die nog steeds tijdens de berekeningen optreden kunnen. Slechts een paar regels van deze code doen daadwerkelijk berekeningen.

We hebben al een voorbeeld van defensive programmering in besproken [transformaties waarde](#valuetransformations). Beide blokken berekening zijn samengevoegd in `tryCatch()`. Voor een aantal fouten is het verstandig om te retourneren van de oorspronkelijke invoer vector en in andere gevallen ik een vector nullen retourneren.  

Houd er rekening mee dat de lineaire regressie gebruikt voor trending ongedaan een reeks tijd regressie is. De variabele manier is een time series-object.  

Eenmaal `ts.detrend()` is gedefinieerd wordt toegepast op de variabelen in onze dataframe van belang. We moeten de resulterende lijst gemaakt met forceren `lapply()` naar dataframe gegevens met behulp van `as.data.frame()`. Vanwege defensive aspecten van `ts.detrend()`, niet-proces een van de variabelen voorkomt niet dat de juiste verwerking van de andere.  

De laatste regel van code maakt een pairwise scatterplot. Na het uitvoeren van de R-code, worden de resultaten van de scatterplot weergegeven in afbeelding 17.

![Pairwise scatterplot van ongedaan trendanalyse en gestandaardiseerde tijdreeks][18]

*Afbeelding 17. Pairwise scatterplot van ongedaan trendanalyse en gestandaardiseerde tijdreeks.*

U kunt deze resultaten aan die wordt weergegeven in afbeelding 16 vergelijken. Met het trendanalyse verwijderd en de variabelen gestandaardiseerd zien we veel minder structuur in de relaties tussen deze variabelen.

De code voor het berekenen van de correlaties als R ccf objecten is als volgt.

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

Deze code wordt het logboek wordt weergegeven in afbeelding 18.

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

Er is een waarde voor correlation voor elke vertraging. Geen van deze waarden correlatie is groot genoeg zijn om verwaarloosbaar zijn. We kunnen daarom sluiten we elke variabele onafhankelijk kunnen model.

### <a name="output-a-dataframe"></a>Een dataframe uitvoer
We hebben de pairwise correlaties berekend als een lijst met R ccf objecten. Dit vormt een bits van een probleem als de uitvoerpoort resultaat gegevensset echt een dataframe vereist. Verder kan het object ccf is zelf een lijst en kunt u alleen de waarden in het eerste element van deze lijst, de correlaties op de verschillende lag.

De waarden van de vertraging van de volgende code opgehaald uit de lijst met ccf objecten, die zelf lijsten.

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
    ## The following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

De eerste coderegel iets lastig is en een verklaring waarmee u begrijpt. Werken van binnenuit hebben we het volgende:

1. De '**[[**'operator met het argument'**1**' de vector van correlaties op de lag uit het eerste element van de lijst met ccf objecten geselecteerd.
2. De `do.call()` functie is van toepassing de `rbind()` via de elementen van de lijst met de functie wordt door `lapply()`.
3. De `data.frame()` functie koppelt het resultaat geproduceerd door `do.call()` naar een dataframe.

Houd er rekening mee dat de rijnamen van de in een kolom van de dataframe zijn. In dat geval behoudt de rij doet namen wanneer ze worden uitgevoerd via de [R-Script uitvoeren][execute-r-script].

De code wordt de uitvoer wordt weergegeven in afbeelding 19 wanneer ik **Visualize** de uitvoer op de poort resultaat gegevensset. De rijnamen van de zijn in de eerste kolom zoals bedoeld.

![Uitvoer van de resultaten van de correlatieanalyse][20]

*Afbeelding 19. Resultaten de uitvoer van de correlatieanalyse.*

## <a id="seasonalforecasting"></a>Time series-voorbeeld: seizoensgebonden prognose
Onze gegevens is nu in een formulier dat geschikt is voor analyse en we hebben vastgesteld dat er zijn geen belangrijke correlatie tussen de variabelen. We gaan en maken van een tijdreeks prognose model. Met dit model forecast we Californië melkproductie voor de 12 maanden van 2013.

Onze prognosemodel heeft twee componenten, een onderdeel van het trendanalyse en een seizoen onderdeel. De volledige prognose is het product van deze twee componenten. Dit type model staat bekend als een Multiplicatieve model. Het alternatief is een additieve model. We hebben een transformatie logboek al toegepast op de variabelen van belang, waardoor deze analyse tractable.

De volledige R-code voor deze sectie wordt het zip-bestand dat u eerder hebt gedownload.

### <a name="creating-the-dataframe-for-analysis"></a>Maken van de dataframe voor analyse
Voeg eerst een **nieuwe** [R-Script uitvoeren] [ execute-r-script] module die u wilt uw experiment. Verbinding maken met de **resultaat gegevensset** uitvoer van de bestaande [R-Script uitvoeren] [ execute-r-script] module die u wilt de **Dataset1** invoer van de nieuwe module. Het resultaat moet er ongeveer als afbeelding 20.

![Het experiment met de nieuwe R-Script uitvoeren module toegevoegd][21]

*Afbeelding 20. Het experiment met de nieuwe R-Script uitvoeren module toegevoegd.*

Als met de correlatieanalyse die we zojuist, moeten we een kolom met een POSIXct time series-object toevoegen. De volgende code wordt hiervoor alleen.

    # If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

Voer deze code en kijk in het logbestand. Het resultaat moet eruitzien als afbeelding 21.

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

*Afbeelding 21. Een samenvatting van de dataframe.*

Met dit resultaat zijn we om te beginnen met onze analyse.

### <a name="create-a-training-dataset"></a>Maken van een gegevensset training
We moeten een gegevensset met de training maken met de dataframe samengesteld. Deze gegevens omvatten alle van de metingen met uitzondering van de afgelopen 12 van het jaar 2013, die onze testgegevensset is. De volgende code subsets van de dataframe en curve van de variabelen voor de productie- en melkkoeien maakt. Ik maak vervolgens curve van de vier productie en prijs variabelen. Een anonieme functie wordt gebruikt voor het definiëren van sommige verbetert voor tekengebied en vervolgens de lijst van de andere twee argumenten met herhalen `Map()`. Als u denkt u zijn die een voor de lus zou hebt gewerkt probleemloos hier, u juist zijn. Maar aangezien R is een functionele taal ik ben waarin u een functionele benadering.

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

De code wordt de reeks time series van de uitvoer van het R-apparaat weergegeven in afbeelding 22 grafieken. Houd er rekening mee dat de tijdas in eenheden van datums nice voordeel van de reeks methode tekenen tijd.

![Eerste dag van time series curve van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-161.png)

![Tweede van time series curve van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-162.png)

![Derde van de time series curve van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-163.png)

![Vierde van time series curve van Californië melkkoeien productie- en -gegevens](./media/r-quickstart/unnamed-chunk-164.png)

*Afbeelding 22. Time series curve van Californië zuivelproductie en gegevens voor de prijs.*

### <a name="a-trend-model"></a>Een model trend
Een time series-object en bekijk de gegevens heeft gehad dat het gemaakt, begint om een model trend voor de Californië melk productie-gegevens samen te stellen. We kunnen dit doen met een reeks tijd regressie. Het is echter wissen van de grafiek wordt meer dan een helling moet en om te modelleren nauwkeurig de waargenomen trend in de trainingsgegevens onderscheppen.

Gezien de kleine schaal van de gegevens, ik het model voor de trend in RStudio maken en vervolgens knippen en plakt u de resulterende model in Azure Machine Learning. RStudio biedt een interactieve omgeving voor dit type interactieve analyses.

Als een eerste poging probeer ik een polynomiale regressie met bevoegdheden maximaal 3. Er is een echte gevaar voor dit soort modellen te veel aanpassen. Daarom is het aanbevolen om te voorkomen dat hoge voorwaarden. De `I()` functie belemmert interpretatie van de inhoud (interpreteert de inhoud, zoals is') en kunt u een letterlijk geïnterpreteerde functie in een regressievergelijking schrijven.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

Hiermee wordt gegenereerd.

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

Van P-waarden (Pr (> | t |)) in deze uitvoer kunt zien we dat de gekwadrateerde term geen aanzienlijke wellicht. Ik gebruik de `update()` functie dit model wijzigen door de gekwadrateerde term slepen en neerzetten.

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

Hiermee wordt gegenereerd.

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

Dit ziet er beter. Alle voorwaarden zijn van belang. De waarde van de 2e-16 is echter een standaardwaarde, en moet niet ernstig te worden genomen.  

Als een test sanity time series tekent van de gegevens van de zuivelproductie Californië we te maken met het trendanalyse curve weergegeven. Ik heb de volgende code toegevoegd in de Azure Machine Learning [R-Script uitvoeren] [ execute-r-script] model (geen RStudio) maken van het model en tekent. Het resultaat wordt weergegeven in afbeelding 23.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![Californië melk productiegegevens met trend model weergegeven](./media/r-quickstart/unnamed-chunk-18.png)

*Afbeelding 23. Californië melk trend model weergegeven met productiegegevens.*

Het lijkt dat het trendanalyse model past bij de gegevens vrij goed. Verder kan lijkt er niet te worden bewijs van te veel aanpassen zoals oneven wiggles in de curve model.  

### <a name="seasonal-model"></a>Seizoensgebonden model
Met een model van de trend in de hand moeten we push op en de seizoensgebonden effecten bevatten. We gebruiken de maand van het jaar als een dummy-variabele in het lineaire model voor het vastleggen van het effect per maand. Houd er rekening mee dat wanneer u factor variabelen in een model introduceren, het snijpunt moet niet worden berekend. Als u dit doet, wordt de formule te veel is opgegeven en R wordt verwijdert u een van de gewenste factoren maar de term intercept houden.

Aangezien we een model tevredenheid trend hebben kunnen we gebruiken de `update()` functie de nieuwe voorwaarden toevoegen aan het bestaande model. De formule van de update -1 wordt de term intercept neergezet. Als u doorgaat in RStudio momenteel:

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

Hiermee wordt gegenereerd.

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

We zien dat het model niet langer een term intercept en 12 aanzienlijke maand factoren heeft. Dit is precies we wilden om te zien.

We maken een andere tijd reeks tekent Californië zuivelproductie gegevens om te zien hoe goed het seizoen model werkt. Ik heb de volgende code toegevoegd in de Azure Machine Learning [R-Script uitvoeren] [ execute-r-script] maken van het model en tekent.

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

Deze code in Azure Machine Learning, wordt de grafiek wordt weergegeven in afbeelding 24.

![Californië melkproductie met model inclusief seizoensgebonden effecten](./media/r-quickstart/unnamed-chunk-20.png)

*Afbeelding 24. Melkproductie Californië met model inclusief seizoensgebonden gevolgen.*

De aanpassen aan de gegevens weergegeven in afbeelding 24 is in plaats van te bevorderen. Zowel het trendanalyse en het seizoen effect (maandelijks variatie) Zoek redelijke.

Als een nieuwe controle uit op het model, gaan we hebben een overzicht van de verschillen opgeven. De volgende code berekent de voorspelde waarden uit onze twee modellen, berekent de verschillen opgeven voor het seizoen model en vervolgens deze verschillen opgeven voor de trainingsgegevens worden uitgezet.

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot the residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

De resterende grafiek wordt weergegeven in afbeelding 25.

![Verschillen van het seizoen model voor de trainingsgegevens opgeven](./media/r-quickstart/unnamed-chunk-21.png)

*Afbeelding 25. Als u dit van het seizoen model voor de trainingsgegevens.*

Deze verschillen opgeven zoeken redelijke. Er is geen specifieke structuur, met uitzondering van het effect van de recessie 2008-2009, die het model wordt geen rekening gehouden met name goed.

De grafiek wordt weergegeven in afbeelding 25 is nuttig voor het detecteren van alle tijdsafhankelijke patronen in de dit. De expliciete benadering van computer- en uitzetten van de ik gebruikt dit wordt de verschillen opgeven in volgorde van de tijd op de grafiek geplaatst. Als, aan de andere kant ik had uitgezet `milk.lm$residuals`, de grafiek niet zouden zijn in volgorde van de tijd.

U kunt ook `plot.lm()` voor het produceren van een reeks diagnostische waarnemingspunten.

    ## Show the diagnostic plots for the model
    plot(milk.lm2, ask = FALSE)

Deze code wordt een reeks diagnostische waarnemingspunten wordt weergegeven in afbeelding 26 geproduceerd.

![Eerste dag van diagnostische waarnemingspunten voor het seizoen model](./media/r-quickstart/unnamed-chunk-221.png)

![Tweede van diagnostische waarnemingspunten voor het seizoen model](./media/r-quickstart/unnamed-chunk-222.png)

![Derde van diagnostische waarnemingspunten voor het seizoen model](./media/r-quickstart/unnamed-chunk-223.png)

![Vierde van diagnostische waarnemingspunten voor het seizoen model](./media/r-quickstart/unnamed-chunk-224.png)

*Afbeelding 26. Diagnose worden uitgezet voor het seizoen model.*

Er zijn een aantal maximaal invloedrijke punten in deze waarnemingspunten, maar niets geweldige opleveren geïdentificeerd. Verder kan kunnen we zien van het tekengebied normaal Q-Q dat de dit zich dicht bij normaal gedistribueerd, belangrijke verondersteld voor lineaire modellen.

### <a name="forecasting-and-model-evaluation"></a>Prognosemodel en model evalueren
Er is slechts één ding doen om het voorbeeld te voltooien. We moeten prognoses berekenen en de fout op basis van de werkelijke gegevens te meten. Onze prognose worden voor de 12 maanden van 2013. We kunt een meting fout voor deze prognose voor de werkelijke gegevens die geen deel uitmaakt van onze gegevensset training berekenen. Daarnaast kunnen we de prestaties van de 18 jaar trainingsgegevens met de 12 maanden van testgegevens vergelijken.  

Een aantal metrische gegevens worden gebruikt voor het meten van de prestaties van de time series-modellen. In ons geval gebruiken we de root mean vierkant (RMS)-fout. De volgende functie berekent de RMS-fout tussen twee reeksen.  

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

Net als bij de `log.transform()` functie in de sectie "Waarde transformaties besproken" Er is zeer veel fout controleren en uitzondering code voor herstel in deze functie. De principes werkzaam zijn hetzelfde. Het werk wordt uitgevoerd op twee plaatsen die zijn ingepakt `tryCatch()`. De tijdreeks zijn eerst exponentiated, omdat we hebt gewerkt met de logboeken van de waarden. Ten tweede wordt de werkelijke RMS-fout berekend.  

Uitgerust met een functie voor het meten van de RMS-fout, gaan we bouwen en uitvoeren van een dataframe met de RMS-fouten. Nemen we voorwaarden voor het trendanalyse model alleen en het model voltooid met seizoensgebonden factoren. De volgende code wordt de taak met behulp van de twee lineaire modellen die we hebt gemaakt.

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

Deze code wordt de uitvoer wordt weergegeven in afbeelding 27 op de uitvoerpoort resultaat gegevensset.

![Vergelijking van RMS-fouten voor de modellen][26]

*Afbeelding 27. Vergelijking van RMS-fouten voor de modellen.*

Van deze resultaten ziet u dat de seizoensgebonden factoren toe te voegen aan het model de RMS-fout aanzienlijk vermindert. De RMS-fout voor de trainingsgegevens is geen te verrassing een bit kleiner is dan voor de prognose.

## <a id="appendixa"></a>BIJLAGE A:-Handleiding voor het RStudio
RStudio heel goed wordt gedocumenteerd, zodat in deze bijlage ik enkele koppelingen naar de sleutel secties van de documentatie voor RStudio zodat u op weg bieden.

1. Projecten maken
   
   U kunt ordenen en beheren van uw R-code in de projecten via RStudio. De documentatie die gebruikmaakt van de projecten kan worden gevonden op https://support.rstudio.com/hc/articles/200526207-Using-Projects.
   
   Ik het beste als volgt uit te voeren en maak een project voor de R-codevoorbeelden in dit document.  
2. Bewerken en het uitvoeren van R-code
   
   RStudio biedt een geïntegreerde omgeving voor het bewerken en R-code wordt uitgevoerd. Documentatie vindt u op https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.
3. Foutopsporing
   
   RStudio omvat krachtige mogelijkheden voor foutopsporing. Documentatie voor deze functies is op https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.
   
   De onderbrekingspunt voor probleemoplossing functies zijn tijdens https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting gedocumenteerd.

## <a id="appendixb"></a>BIJLAGE B: Lees hier meer over
Deze R programming zelfstudie bevat informatie over de basisprincipes van wat u moet de taal R gebruiken met Azure Machine Learning Studio. Als u niet bekend met R bent, zijn twee inleidingen beschikbaar op CRAN:

* R voor beginnende gebruikers door Emmanuel Paradis is een goede plaats om te beginnen bij http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.  
* Een inleiding tot R door W. N. Venables et. al. in iets meer diepte op http://cran.r-project.org/doc/manuals/R-intro.html gaan.

Er zijn veel books op R waarmee u aan de slag kunt. Hier volgen enkele die ik handig:

* De illustratie van R programmeren: een rondleiding van statistische softwareontwerp door Norman Matloff is een uitstekende inleiding tot programmering in R.  
* R Cookbook door Paul Teetor biedt een benadering van het probleem en oplossing voor het gebruik van R.  
* R in actie door Robert Kabacoff is een ander nuttig inleidende rapport. De aanvullende snelle R-website is een nuttig resource op http://www.statmethods.net/.
* R Inferno door Patrick Burns is een verrassend grappige adresboek dat met een aantal lastig en moeilijk onderwerpen die u tegenkomen werkt kunt bij het programmeren in R. Het rapport is beschikbaar voor het vrije op http://www.burns-stat.com/documents/books/the-r-inferno/.
* Als u een diepgaand in geavanceerde onderwerpen in R wilt, hebt u een overzicht van het adresboek Geavanceerd R door Hadley Wickham. De online versie van deze handleiding is beschikbaar voor het vrije op http://adv-r.had.co.nz/.

Een lijst van R time series-pakketten kunt u vinden in de taakweergave CRAN voor analyse van reeks: http://cran.r-project.org/web/views/TimeSeries.html. Voor informatie over specifieke tijd reeks object pakketten, moet u de documentatie voor het desbetreffende pakket verwijzen.

Het rapport inleidende Time Series met R door Paul Cowpertwait en Andrew Metcalfe bevat een inleiding tot R gebruiken voor analyse van reeks. Veel meer theoretische teksten bevatten R voorbeelden.

Sommige geweldige internetbronnen:

* DataCamp: DataCamp leert R de deur van uw browser met video uitkomsten en codering oefeningen. Er zijn interactieve zelfstudies voor de meest recente R technieken en pakketten. Bij https://www.datacamp.com/courses/introduction-to-r worden gehouden met de gratis interactieve R-zelfstudie
* Een handleiding op aan de slag met R van Programiz https://www.programiz.com/r-programming
* Een snelle zelfstudie van R door Kelly Black van Clarkson universiteit http://www.cyclismo.org/tutorial/R/
* 60 + R-resources die wordt vermeld op http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html

<!--Image references-->
[1]: ./media/r-quickstart/fig1.png
[2]: ./media/r-quickstart/fig2.png
[3]: ./media/r-quickstart/fig3.png
[4]: ./media/r-quickstart/fig4.png
[5]: ./media/r-quickstart/fig5.png
[6]: ./media/r-quickstart/fig6.png
[7]: ./media/r-quickstart/fig7.png
[8]: ./media/r-quickstart/fig8.png
[9]: ./media/r-quickstart/fig9.png
[10]: ./media/r-quickstart/fig10.png
[11]: ./media/r-quickstart/fig11.png
[12]: ./media/r-quickstart/fig12.png
[13]: ./media/r-quickstart/fig13.png
[14]: ./media/r-quickstart/fig14.png
[15]: ./media/r-quickstart/fig15.png
[16]: ./media/r-quickstart/fig16.png
[17]: ./media/r-quickstart/fig17.png
[18]: ./media/r-quickstart/fig18.png
[19]: ./media/r-quickstart/fig19.png
[20]: ./media/r-quickstart/fig20.png
[21]: ./media/r-quickstart/fig21.png
[22]: ./media/r-quickstart/fig22.png

[26]: ./media/r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
