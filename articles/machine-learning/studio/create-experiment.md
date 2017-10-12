---
title: Een eenvoudig experiment in Machine Learning Studio | Microsoft Docs
description: Deze zelfstudie over Machine Learning leidt u door een eenvoudig gegevenswetenschapexperiment. We voorspellen de prijs van een auto met behulp van een regressiealgoritme.
keywords: experiment,lineaire regressie,machine learning-algoritmen,zelfstudie over machine learning,voorspellende modelleringstechnieken,gegevenswetenschapexperiment
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 4cc8e78e3ce22d70546d8a25da17b56f4b7cc166
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="machine-learning-tutorial-create-your-first-data-science-experiment-in-azure-machine-learning-studio"></a>Zelfstudie over Machine Learning: uw eerste gegevenswetenschapexperiment maken in Azure Machine Learning Studio

Als u **Azure Machine Learning Studio** niet eerder hebt gebruikt, biedt deze zelfstudie een goede basis.

Hierin wordt beschreven hoe u aan de slag gaat met Studio en uw eerste Machine Learning-experiment maakt. Met het experiment wordt een analysemodel getest waarmee de prijs van een auto kan worden voorspeld op basis van verschillende variabelen, zoals het merk en de technische specificaties.

> [!NOTE]
> In deze zelfstudie leert u hoe u modules naar uw experiment sleept, ze aan elkaar koppelt, het experiment uitvoert en de resultaten bekijkt. We gaan het niet hebben over machine learning in het algemeen of over hoe u de meer dan 100 ingebouwde algoritmen en gegevensbewerkingsmodules in Studio selecteert en gebruikt.
>
>Als u geen ervaring hebt met machine learning, is de videoserie [Data Science for Beginners](data-science-for-beginners-the-5-questions-data-science-answers.md) (Gegevenswetenschap voor beginners) een goed startpunt. In deze videoserie maakt u op eenvoudige wijze kennis met machine learning op basis van alledaagse taal en concepten.
>
>Als u op de hoogte bent van machine learning, maar algemene informatie zoekt over Machine Learning Studio en de geïntegreerde machine learning-algoritmen, kunt u de volgende resources raadplegen:
>
- [Wat is Machine Learning Studio?](what-is-ml-studio.md) - Dit geeft een overzicht van Studio.
- [Machine learning basics with algorithm examples](basics-infographic-with-algorithm-examples.md) (Basisbeginselen van Machine Learning met algoritmevoorbeelden) - Deze infographic is handig als u meer wilt weten over de verschillende soorten machine learning-algoritmen in Machine Learning Studio.
- [Machine Learning Guide](https://gallery.cortanaintelligence.com/Tutorial/Machine-Learning-Guide-1) (Machine Learning-handleiding) - Deze handleiding bevat vergelijkbare informatie als de bovenstaande infographic, maar in een interactieve indeling.
- [Machine learning algorithm cheat sheet](algorithm-cheat-sheet.md) (Cheatsheet voor machine learning-algoritmen) en [How to choose algorithms for Microsoft Azure Machine Learning](algorithm-choice.md) (Algoritmen kiezen voor Microsoft Azure Machine Learning) - In deze downloadbare poster en het bijbehorende artikel worden de Studio-algoritmen in detail besproken.
- [Machine Learning Studio: Algorithm and Module Help](https://msdn.microsoft.com/library/azure/dn905974.aspx) (Machine Learning Studio: hulp bij algoritmen en modules) - Dit is een volledig naslagwerk voor alle Studio-modules, inclusief de machine learning-algoritmen.

<!-- -->

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="how-does-machine-learning-studio-help"></a>Op welke wijze is Machine Learning Studio nuttig?

Met Machine Learning Studio kunt u gemakkelijk een experiment opzetten met behulp van modules die vooraf zijn geprogrammeerd met voorspellende modelleringstechnieken die u met slepen-en-neerzetten kunt gebruiken.

In de interactieve, visuele werkruimte sleept u ***gegevenssets*** en analyse***modules*** naar een interactief canvas. U koppelt ze aan elkaar om een ***experiment*** te maken dat u uitvoert in Machine Learning Studio.
U ***maakt een model***, ***traint het model*** en ***beoordeelt en test het model***.

U kunt het modelontwerp herhalen, en het experiment bewerken en uitvoeren totdat dit de gewenste resultaten oplevert. Wanneer uw model klaar is, kunt u het publiceren als een ***webservice***, zodat anderen voorspellingen kunnen krijgen voor de nieuwe gegevens die zij toesturen.

## <a name="open-machine-learning-studio"></a>Machine Learning Studio openen

Ga naar [https://studio.azureml.net](https://studio.azureml.net) om aan de slag te gaan met Studio. Als u zich eerder hebt aangemeld bij Machine Learning Studio, klikt u op **Sign in**. Klik anders op **Sign Up** en kies tussen gratis en betaalde opties.

![Meld u aan bij Machine Learning Studio][sign-in-to-studio]
<br/>
***Meld u aan bij Machine Learning Studio***

## <a name="five-steps-to-create-an-experiment"></a>In vijf stappen een experiment maken

In deze machine learning-zelfstudie volgt u vijf eenvoudige stappen in Machine Learning Studio om een model te maken, te trainen en te beoordelen:

- **Een model maken**
    - [Stap 1: gegevens ophalen]
    - [Stap 2: de gegevens voorbereiden]
    - [Stap 3: functies definiëren]
- **Het model trainen**
    - [Stap 4: een leeralgoritme kiezen en toepassen]
- **Het model beoordelen en testen**
    - [Stap 5: prijzen van nieuwe auto's voorspellen]

[Stap 1: gegevens ophalen]: #step-1-get-data
[Stap 2: de gegevens voorbereiden]: #step-2-prepare-the-data
[Stap 3: functies definiëren]: #step-3-define-features
[Stap 4: een leeralgoritme kiezen en toepassen]: #step-4-choose-and-apply-a-learning-algorithm
[Stap 5: prijzen van nieuwe auto's voorspellen]: #step-5-predict-new-automobile-prices

> [!TIP] 
> U vindt een werkende kopie van het volgende experiment in de [Cortana Intelligence-galerie](https://gallery.cortanaintelligence.com). Ga naar **[Your first data science experiment - Automobile price prediction](https://gallery.cortanaintelligence.com/Experiment/Your-first-data-science-experiment-Automobile-price-prediction-1)** en klik op **Open in Studio** om een kopie van het experiment naar uw Machine Learning Studio-werkruimte te downloaden.


## <a name="step-1-get-data"></a>Stap 1: gegevens ophalen

Het eerste wat u voor machine learning nodig hebt, zijn gegevens.
Machine Learning Studio bevat een aantal voorbeeldgegevenssets die u kunt gebruiken. Daarnaast kunt u uit tal van bronnen gegevens importeren. Voor dit voorbeeld gebruiken we de voorbeeldgegevensset **Automobile price data (Raw)**. Deze is opgenomen in de werkruimte.
Deze gegevensset bevat vermeldingen voor verschillende auto's, inclusief informatie over het merk, het model, de technische specificaties en de prijs.

U voegt de gegevensset als volgt toe aan uw experiment.

1. Maak een nieuw experiment door onder aan het Machine Learning Studio-venster op **+NEW** te klikken en achtereenvolgens **EXPERIMENT** en **Blank Experiment** te selecteren.

2. Het experiment krijgt een standaardnaam die boven aan het canvas wordt weergegeven. Selecteer deze tekst en wijzig de naam in iets relevants, bijvoorbeeld **prijzen auto's voorspellen**. De naam hoeft niet uniek te zijn.

    ![De naam van het experiment wijzigen][rename-experiment]

2. Aan de linkerkant van het experimentcanvas bevindt zich een palet met gegevenssets en modules. Typ in het zoekvak boven aan dit palet **automobile** om de gegevensset met het label **Automobile price data (Raw)** te zoeken. Sleep deze gegevensset naar het experimentcanvas.

    ![Zoek de gegevensset 'automobile' en sleep deze naar het experimentcanvas][type-automobile]
    <br/>
    ***Zoek de gegevensset 'automobile' en sleep deze naar het experimentcanvas***

Als u wilt zien hoe deze gegeven eruitzien, klikt u op de uitvoerpoort onder aan de gegevensset 'automobile' en selecteert u vervolgens **Visualize**.

![Klik op de uitvoerpoort en selecteer Visualize][select-visualize]
<br/>
***Klik op de uitvoerpoort en selecteer Visualize***

> [!TIP]
> De invoer- en uitvoerpoorten van gegevenssets en modules worden aangeduid met kleine cirkels, waarbij de invoerpoorten zich boven en de uitvoerpoorten zich onder bevinden.
Als u een gegevensstroom door uw experiment wilt maken, verbindt u een uitvoerpoort van één module met een invoerpoort van een andere.
U kunt op elk gewenst moment op de uitvoerpoort van een gegevensset of module klikken om te zien hoe de gegevens eruitzien op dat punt in de gegevensstroom.

In de voorbeeldgegevensset wordt elke instantie van een auto weergegeven als een rij. De variabelen die aan elke auto zijn gekoppeld, worden weergegeven als kolommen. Aan de hand van de variabelen voor een specifieke auto gaan we proberen de prijs te voorspellen in de kolom price (kolom 26).

![Bekijk de autogegevens in het venster voor gegevensvisualisatie][visualize-auto-data]
<br/>
***Bekijk de autogegevens in het venster voor gegevensvisualisatie***

Sluit het visualisatievenster door op de **x** in de rechterbovenhoek te klikken.

## <a name="step-2-prepare-the-data"></a>Stap 2: de gegevens voorbereiden

Normaal gesproken moet een gegevensset worden voorverwerkt voordat deze kan worden geanalyseerd. Het is u mogelijk opgevallen dat er waarden ontbreken in de kolommen van verschillende rijen. Deze ontbrekende waarden moeten worden opgeschoond, zodat de gegevens correct kunnen worden geanalyseerd. In ons geval verwijderen we de rijen met ontbrekende waarden. Ook in de kolom **normalized-losses** ontbreekt een groot deel van de waarden. Deze kolom zal daarom helemaal worden uitgesloten van het model.

> [!TIP]
> Voor de meeste modules geldt dat voor het gebruik van de module de ontbrekende invoergegevens moeten worden opgeschoond.

Eerst voegt u een module toe waarmee de kolom **normalized-losses** volledig wordt verwijderd. Vervolgens voegt u een andere module toe waarmee rijen met ontbrekende gegevens worden verwijderd.

1. Typ in het zoekvak boven aan het modulepalet **select columns** om de module [Select Columns in Dataset][select-columns] te vinden en sleep de module naar het experimentcanvas. Met deze module kunt u selecteren welke kolommen met gegevens u wilt opnemen in of uitsluiten voor het model.

2. Koppel de uitvoerpoort van de gegevensset **Automobile price data (Raw)** aan de invoerpoort van de module [Select Columns in Dataset][select-columns].

    ![Voeg de module Select Columns in Dataset toe aan het experimentcanvas en koppel ze][type-select-columns]
    <br/>
    ***Voeg de module Select Columns in Dataset toe aan het experimentcanvas en koppel ze***

3. Klik op de module [Select Columns in Dataset][select-columns] en klik in het deelvenster **Properties** op **Launch column selector**.

    - Klik links op **With rules**
    - Klik onder **Begin With** op **All columns**. Zodoende zorgt u ervoor dat [Select Columns in Dataset][select-columns] alle kolommen doorgeeft (met uitzondering van de kolommen die we dadelijk zullen uitsluiten).
    - Selecteer in de vervolgkeuzelijsten **Exclude** en **column names** en klik in het tekstvak. Er wordt een lijst met kolommen weergegeven. Selecteer **normalized-losses** om dit aan het tekstvak toe te voegen.
    - Klik rechtsonder op de knop met het vinkje (OK) om de kolomkiezer te sluiten.

    ![Start de kolomkiezer en sluit de kolom normalized-losses uit][launch-column-selector]
    <br/>
    ***Start de kolomkiezer en sluit de kolom normalized-losses uit***

    Het deelvenster met eigenschappen van de module **Select Columns in Dataset** geeft nu aan dat alle kolommen uit de gegevensset worden doorgegeven, met uitzondering van **normalized-losses**.

    ![Het deelvenster met eigenschappen geeft aan dat de kolom normalized-losses wordt uitgesloten][showing-excluded-column]
    <br/>
    ***Het deelvenster met eigenschappen geeft aan dat de kolom normalized-losses wordt uitgesloten***

    > [!TIP]
    U kunt een opmerking aan een module toevoegen door te dubbelklikken op de module en tekst in te voeren. Zodoende kunt u in één oogopslag zien wat de module in uw experiment doet. Dubbelklik in dit geval op de module [Select Columns in Dataset][select-columns] en typ de opmerking 'normalized-losses uitsluiten'.
    >
    >


    ![Dubbelklik op een module om een opmerking toe te voegen][add-comment]
    <br/>
    ***Dubbelklik op een module om een opmerking toe te voegen***

3. Sleep de module [Clean Missing Data][clean-missing-data] naar het canvas en verbindt deze met de module [Select Columns in Dataset][select-columns]. Selecteer in het deelvenster **Properties** de optie **Remove entire row** onder **Cleaning mode**. De module [Clean Missing Data][clean-missing-data] schoont nu de gegevens op door rijen met ontbrekende waarden te verwijderen. Dubbelklik op de module en typ de opmerking 'Rijen met ontbrekende waarde verwijderen'.

    ![Stel de opschoonmodus van de module Clean Missing Data in op Remove entire row][set-remove-entire-row]
    <br/>
    ***Stel de opschoonmodus van de module Clean Missing Data in op Remove entire row***

4. Voer het experiment uit door onder aan de pagina op **RUN** te klikken.

    Wanneer het experiment is voltooid, wordt er bij alle modules een groen vinkje weergegeven om aan te geven dat deze zijn voltooid. In de rechterbovenhoek wordt de status **Finished running** weergegeven.

![Zo ziet het experiment er ongeveer uit nadat het is uitgevoerd][early-experiment-run]
<br/>
***Zo ziet het experiment er ongeveer uit nadat het is uitgevoerd***

> [!TIP]
> Waarom voeren we het experiment nu uit? Door het experiment uit te voeren, worden de kolomdefinities voor onze gegevens van de gegevensset doorgegeven via de module [Select Columns in Dataset][select-columns] en via de module [Clean Missing Data][clean-missing-data]. Hierdoor beschikken alle modules die u koppelt aan [Clean Missing Data][clean-missing-data] over dezelfde informatie.

Tot nu toe hebben we in het experiment alleen de gegevens opgeschoond. Als u de opgeschoonde gegevensset wilt weergeven, klikt u op de uitvoerpoort links van de module [Clean Missing Data][clean-missing-data] en selecteert u **Visualize**. Zoals u kunt zien, is de kolom **normalized-losses** verwijderd en ontbreken er geen waarden meer.

Nu de gegevens zijn opgeschoond, kunt u opgeven welke functies u wilt gebruiken in het voorspellende model.

## <a name="step-3-define-features"></a>Stap 3: functies definiëren

In machine learning zijn *functies* afzonderlijke meetbare eigenschappen van iets waarin u geïnteresseerd bent. In onze gegevensset staat elke rij voor één auto en elke kolom bevat een kenmerk van die auto.

Voor een goede set kenmerken voor het maken van een voorspellend model, moet u experimenteren en beschikken over kennis van het probleem dat u wilt oplossen. Bepaalde kenmerken zijn beter voor het voorspellen van het doel dan andere. Bovendien bestaat er tussen sommige kenmerken een nauwe correlatie. Deze kunnen daarom worden verwijderd. City-mpg en highway-mpg lijken bijvoorbeeld sterk op elkaar, waardoor we een van de twee kunnen verwijderen zonder de voorspelling al te veel te beïnvloeden.

Laten we een model bouwen dat gebruikmaakt van een subset kenmerken onze gegevensset. U kunt later terugkeren en andere kenmerken selecteren om het experiment vervolgens opnieuw uit te voeren en te zien of u betere resultaten krijgt. Maar probeer om te beginnen de volgende functies:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Sleep nog een module [Select Columns in Dataset][select-columns] naar het experimentcanvas. Koppel de linkeruitvoerpoort van de module [Clean Missing Data][clean-missing-data] aan de invoerpoort van de module [Select Columns in Dataset][select-columns].

    ![Koppel de module Select Columns in Dataset aan de module Clean Missing Data][connect-clean-to-select]
    <br/>
    ***Koppel de module Select Columns in Dataset aan de module Clean Missing Data***

2. Dubbelklik op de module en typ 'Select features for prediction' (Kenmerken voor de voorspelling selecteren).

2. Klik in het deelvenster **Properties** op **Launch column selector**.

3. Klik op **With rules**.

4. Klik onder **Begin With** op **No columns**. Selecteer in de filterrij **Include** en **column names** en selecteer onze lijst met kolomnamen in het tekstvak. Dit zorgt ervoor dat de module alleen de kolommen (kenmerken) doorgeeft die wij opgeven.

5. Klik op de knop met het vinkje (OK).

    ![Selecteer de kolommen (kenmerken) die u wilt opnemen in de voorspelling][select-columns-to-include]
    <br/>
    ***Selecteer de kolommen (kenmerken) die u wilt opnemen in de voorspelling***

Dit resulteert in een gefilterde gegevensset met alleen de kenmerken die we willen doorgeven aan het learning-algoritme dat we in de volgende stap gebruiken. U kunt later terugkeren en het opnieuw proberen met een andere selectie kenmerken.

## <a name="step-4-choose-and-apply-a-learning-algorithm"></a>Stap 4: een leeralgoritme kiezen en toepassen

Nu de gegevens klaar zijn, kunt u een voorspellend model bouwen door het model te trainen en te testen. We gebruiken onze gegevens om het model te trainen. Vervolgens testen we het model om te controleren in hoeverre de prijzen met dit model kunnen worden voorspeld.
<!-- For now, don't worry about *why* we need to train and then test a model.-->

*Classificatie* en *regressie* zijn twee soorten beheerde machine learning-algoritmen. Classificatie voorspelt een antwoord uit een gedefinieerde set categorieën, zoals een kleur (rood, blauw of groen). Regressie wordt gebruikt om een getal te voorspellen.

Omdat we de prijs willen voorspellen, wat een getal is, gebruiken we een regressiealgoritme. In dit voorbeeld gebruiken we een eenvoudig *lineair regressiemodel*.

> [!TIP]
> Als u meer wilt weten over de verschillende soorten machine learning-algoritmen en wanneer ze worden gebruikt, kunt u de eerste video bekijken van de serie Data Science for Beginners (Gegevenswetenschap voor beginners), [The five questions data science answers](data-science-for-beginners-the-5-questions-data-science-answers.md) (De vijf vragen waarop gegevenswetenschap antwoord geeft). U kunt ook de infographic [Machine learning basics with algorithm examples](basics-infographic-with-algorithm-examples.md) (Basisbeginselen van Machine Learning met algoritmevoorbeelden) bekijken of de [Machine learning algorithm cheat sheet](algorithm-cheat-sheet.md) (Cheatsheet voor machine learning-algoritmen) raadplegen.

We trainen het model met behulp van een gegevensset die de prijs bevat. Het model scant de gegevens en zoekt naar correlaties tussen de kenmerken van een auto en de prijs. Vervolgens gaan we het model testen. We voeren een aantal kenmerken in van auto's waarvan we de prijs kennen en kijken hoe dicht de voorspellingen van het model bij de bekende prijzen komen.

We gebruiken onze gegevens zowel voor trainings- als testdoeleinden door ze op te splitsen in afzonderlijke trainings- en testsets.

1. Selecteer de module [Split Data][split], sleep deze naar het experimentcanvas en koppel de module aan de laatste module [Select Columns in Dataset][select-columns].

2. Klik op de module [Split Data][split] om deze te selecteren. Stel **Fraction of rows in the first output dataset** in het deelvenster **Properties** (rechts van het canvas) in op 0,75. Zodoende gebruiken we 75 procent van de gegevens om het model te trainen en 25 procent van de gegevens om het model te testen (u kunt later experimenteren met andere percentages).

    ![Stel de splitsfractie van de module Split Data in op 0,75][set-split-data-percentage]
    <br/>
    ***Stel de splitsfractie van de module Split Data in op 0,75***

    > [!TIP]
    > Door de parameter **Random seed** te wijzigen, kunt u verschillende willekeurig samples voor trainings- en testdoeleinden gebruiken. Deze parameter bepaalt de seeding van de pseudo-willekeurige nummergenerator.

2. Voer het experiment uit. De modules [Select Columns in Dataset][select-columns] en [Split Data][split] geven daarop kolomdefinities door aan de modules die we hierna zullen toevoegen.  

3. Als u een leeralgoritme wilt selecteren, moet u de categorie **Machine Learning** in het modulepalet links van het canvas uitvouwen en vouwt u vervolgens **Initialize Model** uit. Er worden verschillende categorieën weergegeven die kunnen worden gebruikt om de machine learning-algoritmen te initialiseren. Selecteer voor dit experiment de module [Linear Regression][linear-regression] in de categorie **Regression** en sleep de module naar het experimentcanvas.
(U kunt de module ook zoeken door 'linear regression' in het zoekvak van het palet te typen.)

4. Zoek de module [Train Model][train-model] en sleep deze naar het experimentcanvas. Koppel de uitvoer van de module [Linear Regression][linear-regression] aan de linkerinvoer van de module [Train model][train-model] en koppel de trainingsgegevensuitvoer (linkerpoort) van de module [Split Data][split] aan de rechterinvoer van de module [Train Model][train-model].

    ![Koppel de module Train Model aan de modules Linear Regression en Split Data][connect-train-model]
    <br/>
    ***Koppel de module Train Model aan de modules Linear Regression en Split Data***

5. Klik op de module [Train Model][train-model], klik in het deelvenster **Properties** op **Launch column selector** en selecteer vervolgens de kolom **price**. Dit is de waarde die door het model wordt voorspeld.

    U selecteert de kolom **prijs** in de kolomkiezer door deze vanuit de lijst **Available columns** te verplaatsen naar de lijst **Selected columns**.

    ![Selecteer de kolom price voor de module Train Model][select-price-column]
    <br/>
    ***Selecteer de kolom price voor de module Train Model***

6. Voer het experiment uit.

Nu beschikken we over een getraind regressiemodel dat kan worden gebruikt om nieuwe autogegevens te beoordelen voor het maken van prijsvoorspellingen.

![Zo ziet het experiment er ongeveer uit nadat het is uitgevoerd][second-experiment-run]
<br/>
***Zo ziet het experiment er ongeveer uit nadat het is uitgevoerd***

## <a name="step-5-predict-new-automobile-prices"></a>Stap 5: prijzen van nieuwe auto's voorspellen

Nu we het model met 75 procent van de gegevens hebben getraind, kunnen we het model gebruiken om de overige 25 procent van onze gegevens te beoordelen om te zien hoe goed het model werkt.

1. Zoek de module [Score Model][score-model] en sleep deze naar het experimentcanvas. Koppel de uitvoer van de module [Train Model][train-model] aan de linkerinvoerpoort van de module [Score Model][score-model]. Koppel de testgegevensuitvoer (rechterpoort) van de module [Split Data][split] aan de rechterinvoerpoort van de module [Score Model][score-model].

    ![Koppel de module Score Model aan de modules Train Model en Split Data][connect-score-model]
    <br/>
    ***Koppel de module Score Model aan de modules Train Model en Split Data***

2. Voer het experiment uit en geef de uitvoer van de module [Score Model][score-model]. Hiervoor klikt u op de uitvoerpoort van [Score Model][score-model] en selecteert u **Visualize**. De uitvoer bevat de voorspelde waarden voor de prijs en de bekende waarden uit de testgegevens.  

    ![Uitvoer van de module Score Model][score-model-output]
    <br/>
    ***Uitvoer van de module Score Model***

3. Ten slotte testen we de kwaliteit van de resultaten. Selecteer de module [Evaluate Model][evaluate-model], sleep deze naar het experimentcanvas en koppel de uitvoer van de module [Score Model][score-model] aan de linkerinvoer van de module [Evaluate Model][evaluate-model].

    > [!TIP]
    > De module [Evaluate Model][evaluate-model] heeft twee invoerpoorten omdat deze kan worden gebruikt om twee modellen met elkaar te vergelijken. U kunt later een ander algoritme toevoegen aan het experiment en [Evaluate Model][evaluate-model] gebruiken om te zien welk algoritme betere resultaten oplevert.

4. Voer het experiment uit.

Als u de uitvoer van de module [Evaluate Model][evaluate-model] wilt weergeven, klikt u op de uitvoerpoort en selecteert u **Visualize**.

![Evaluatieresultaten van het experiment][evaluation-results]
<br/>
***Evaluatieresultaten van het experiment***

De volgende statistieken worden weergegeven voor het model:

- **Mean Absolute Error** (MAE): het gemiddelde aan absolute fouten (een *fout* is het verschil tussen de voorspelde waarde en de werkelijke waarde).
- **Root Mean Squared Error** (RMSE): de wortel uit het gemiddelde aan gekwadrateerde fouten voor de voorspellingen op basis van de testgegevensset.
- **Relative Absolute Error**: het gemiddelde aan absolute fouten ten opzichte van het absolute verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
- **Relative Squared Error**: het gemiddelde aan gekwadrateerde fouten ten opzichte van het gekwadrateerde verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
- **Coefficient of Determination**: de determinatiecoëfficiënt, ook wel **R²-waarde** genoemd, is een statistische meetwaarde die aangeeft hoe goed het model is in het voorspellen van de gegevens.

Voor elk van de foutstatistieken geldt: hoe kleiner hoe beter. Een kleine waarde geeft aan dat de voorspelling dichter bij de werkelijke waarde ligt. Hoe dichter de **determinatiecoëfficiënt** bij één (1.0) ligt, hoe nauwkeuriger de voorspellingen.

## <a name="final-experiment"></a>Laatste experiment

Het laatste experiment ziet er ongeveer als volgt uit:

![Laatste experiment][complete-linear-regression-experiment]
<br/>
***Laatste experiment***

## <a name="next-steps"></a>Volgende stappen

Nu u de aanvankelijke machine learning-zelfstudie hebt voltooid en een experiment hebt gemaakt, kunt u het model verder verbeteren en uiteindelijk implementeren als voorspellende webservice.

- **Herhalen om het model te verbeteren** - U kunt bijvoorbeeld andere kenmerken gebruiken in uw voorspelling. Of u kunt de eigenschappen wijzigen van het algoritme [Linear Regression][linear-regression] of een compleet ander algoritme proberen. U kunt zelfs meerdere machine learning-algoritmen tegelijk aan uw experiment toevoegen en de module [Evaluate Model][evaluate-model] gebruiken om twee algoritmen te vergelijken.
Zie [Compare Regressors](https://gallery.cortanaintelligence.com/Experiment/Compare-Regressors-5) in de [Cortana Intelligence-galerie](https://gallery.cortanaintelligence.com) voor een voorbeeld van hoe u meerdere modellen in één experiment vergelijkt.

    > [!TIP]
    > Gebruik de knop **SAVE AS** onder aan de pagina om herhalingen van uw experiment te kopiëren. U kunt alle herhalingen van uw experiment weergeven door onder aan de pagina op **VIEW RUN HISTORY** te klikken. Zie [Iteraties van experimenten beheren in Azure Machine Learning Studio][runhistory] voor meer informatie.

[runhistory]: manage-experiment-iterations.md

- **Het model implementeren als voorspellende webservice** - Wanneer u tevreden bent over het model, kunt u dit implementeren als een webservice om de prijzen van auto's te voorspellen op basis van nieuwe gegevens. Zie [Een Azure Machine Learning-webservice implementeren][publish] voor meer informatie.

[publish]: publish-a-machine-learning-web-service.md

Wilt u meer leren? Zie [Een voorspellende oplossing met Azure Machine Learning ontwikkelen][walkthrough] voor een uitgebreid en gedetailleerd overzicht van hoe u een model maakt, traint, beoordeelt en implementeert.

[walkthrough]: walkthrough-develop-predictive-solution.md

<!-- Images -->
[sign-in-to-studio]: ./media/create-experiment/sign-in-to-studio.png
[rename-experiment]: ./media/create-experiment/rename-experiment.png
[visualize-auto-data]:./media/create-experiment/visualize-auto-data.png
[select-visualize]: ./media/create-experiment/select-visualize.png
[showing-excluded-column]:./media/create-experiment/showing-excluded-column.png
[set-remove-entire-row]:./media/create-experiment/set-remove-entire-row.png
[early-experiment-run]:./media/create-experiment/early-experiment-run.png
[select-columns-to-include]:./media/create-experiment/select-columns-to-include.png
[second-experiment-run]:./media/create-experiment/second-experiment-run.png
[connect-score-model]:./media/create-experiment/connect-score-model.png
[evaluation-results]:./media/create-experiment/evaluation-results.png
[complete-linear-regression-experiment]:./media/create-experiment/complete-linear-regression-experiment.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[type-automobile]:./media/create-experiment/type-automobile.png
[type-select-columns]:./media/create-experiment/type-select-columns.png
[launch-column-selector]:./media/create-experiment/launch-column-selector.png
[add-comment]:./media/create-experiment/add-comment.png
[connect-clean-to-select]:./media/create-experiment/connect-clean-to-select.png

[set-split-data-percentage]:./media/create-experiment/set-split-data-percentage.png

<!-- temporarily switching GIFs to PNGs to remove animation --> 
[connect-train-model]:./media/create-experiment/connect-train-model.png
[select-price-column]:./media/create-experiment/select-price-column.png

[score-model-output]:./media/create-experiment/score-model-output.png

<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
