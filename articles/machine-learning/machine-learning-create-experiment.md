---
title: Een eenvoudig experiment in Machine Learning Studio | Microsoft Docs
description: Deze zelfstudie over Machine Learning leidt u door een eenvoudig gegevenswetenschapexperiment. We voorspellen de prijs van een auto met behulp van een regressiealgoritme.
keywords: experiment,lineaire regressie,machine learning-algoritmen,zelfstudie over machine learning,voorspellende modelleringstechnieken,gegevenswetenschapexperiment
services: machine-learning
documentationcenter: ''
author: garyericson
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: garye

---
# Zelfstudie over Machine Learning: uw eerste gegevenswetenschapexperiment maken in Azure Machine Learning Studio
Deze zelfstudie over Machine Learning leidt u door een eenvoudig gegevenswetenschapexperiment. Wij maken een lineair regressiemodel waarmee de prijs van een auto kan worden voorspeld op basis van verschillende variabelen, zoals het merk en de technische specificaties. Hiervoor maken we gebruik van Azure Machine Learning Studio om eenvoudige predictive analytics-experimenten te ontwikkelen en te herhalen.

*Predictive analytics* is een tak in de gegevenswetenschap waarbij gebruik wordt gemaakt van huidige gegevens om toekomstige resultaten te voorspellen. Bekijk video 4 van Data Science for Beginners (Gegevenswetenschap voor beginners): [Predict an answer with a simple model](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md) (Een antwoord voorspellen met een eenvoudig model, speelduur 7:42) voor een uiterst eenvoudig voorbeeld van predictive analytics.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## Op welke wijze is Machine Learning Studio nuttig?
Met Machine Learning Studio kunt u gemakkelijk een experiment opzetten met behulp van modules die vooraf zijn geprogrammeerd met voorspellende modelleringstechnieken die u met slepen-en-neerzetten kunt gebruiken. Om uw experiment uit te voeren en een antwoord te voorspellen, gebruikt u Machine Learning Studio om *een model te maken*, *het model te trainen*, en *het model te scoren en testen*.

Ga naar Machine Learning Studio: [https://studio.azureml.net](https://studio.azureml.net). Als u zich eerder hebt aangemeld bij Machine Learning Studio, klikt u op **Sign in here** (Hier aanmelden). Klik anders op **Sign Up** (Registreren) en kies tussen gratis en betaalde opties.

Zie [What is Machine Learning Studio?](machine-learning-what-is-ml-studio.md) (Wat is Machine Learning Studio?) voor meer algemene informatie over Machine Learning Studio.

## In vijf stappen een experiment maken
In deze machine learning-zelfstudie volgt u vijf eenvoudige stappen in Machine Learning Studio om een model te maken, te trainen en te beoordelen:

* Een model maken
  * [Stap 1: gegevens ophalen]
  * [Stap 2: gegevens voorverwerken]
  * [Stap 3: functies definiëren]
* Het model trainen
  * [Stap 4: een leeralgoritme kiezen en toepassen]
* Het model beoordelen en testen
  * [Stap 5: prijzen van nieuwe auto's voorspellen]

[Stap 1: gegevens ophalen]: #step-1-get-data
[Stap 2: gegevens voorverwerken]: #step-2-preprocess-data
[Stap 3: functies definiëren]: #step-3-define-features
[Stap 4: een leeralgoritme kiezen en toepassen]: #step-4-choose-and-apply-a-learning-algorithm
[Stap 5: prijzen van nieuwe auto's voorspellen]: #step-5-predict-new-automobile-prices


## Stap 1: gegevens ophalen
Machine Learning Studio bevat een aantal voorbeeldgegevenssets waaruit u kunt kiezen. Daarnaast kun u uit tal van bronnen gegevens importeren. Voor dit voorbeeld gebruiken we de bijgeleverde voorbeeldgegevensset **Automobile price data (Raw)**.
Deze gegevensset bevat vermeldingen voor een aantal afzonderlijke auto’s, inclusief informatie over het merk, het model, de technische specificaties en de prijs.

1. Start een nieuw experiment door onder aan het Machine Learning Studio-venster op **+NEW** (+NIEUW) te klikken en achtereenvolgens **EXPERIMENT** en **Blank Experiment** (Leeg experiment) te selecteren. Selecteer boven aan het canvas de standaardnaam voor een experiment en wijzig deze in een beschrijvende naam **Prijzen auto's voorspellen**.
2. Aan de linkerkant van het experimentcanvas bevindt zich een palet met gegevenssets en modules. Typ in het zoekvak boven aan dit palet **automobile** om de gegevensset met het label **Automobile price data (Raw)** te zoeken.
   
    ![Palet zoeken][screen1a]
3. Sleep de gegevensset naar het experimentcanvas.
   
    ![Gegevensset][screen1]

Als u wilt zien hoe deze gegeven eruitzien, klikt u op de uitvoerpoort onder aan de gegevensset automobile en selecteert u vervolgens **Visualize** (Visualiseren).

![Module-uitvoerpoort][screen1c]

De variabelen in de gegevensset worden weergegeven als kolommen en elk exemplaar van een auto wordt weergegeven als een rij. De meest rechtse kolom (kolom 26, genaamd 'price') is de doelvariabele die we proberen te voorspellen.

![Visualisatie van de gegevensset][screen1b]

Sluit het visualisatievenster door op de **x** in de rechterbovenhoek te klikken.

## Stap 2: gegevens voorverwerken
Normaal gesproken moet een gegevensset worden voorverwerkt voordat deze kan worden geanalyseerd. Het is u mogelijk opgevallen dat er in verschillende rijen ontbreken in de kolommen van verschillende rijen. Deze ontbrekende waarden moeten worden opgeschoond, zodat de gegevens correct kunnen worden geanalyseerd. In ons geval verwijderen we de rijen met ontbrekende waarden. Ook in de kolom **normalized-losses** ontbreekt een groot deel van de waarden. Deze kolom zal daarom helemaal worden uitgesloten van het model.

> [!TIP]
> Voor de meeste modules geldt dat voor het gebruik van de module de ontbrekende invoergegevens moeten worden opgeschoond.
> 
> 

Eerst verwijderen we de kolom **normalized-losses**. Vervolgens worden de rijen met ontbrekende gegevens verwijderd.

1. Typ in het zoekvak boven aan het modulepalet **select columns** om de module [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren), sleep de module naar het experimentcanvas en koppel de module aan de uitvoerpoort van de gegevensset **Automobile price data (Raw)**. Met deze module kunt u selecteren welke kolommen met gegevens u wilt opnemen in of uitsluiten voor het model.
2. Selecteer de module [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren) en klik in het deelvenster **Properties** (Eigenschappen) op **Launch column selector** (Kolomselector starten).
   
   * Klik links op **With rules** (Met regels)
   * Klik onder **Begin With** (Beginnen met) op **All columns** (Alle kolommen). Zodoende zorgt u ervoor dat [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren) alle kolommen doorgeeft (met uitzondering van de kolommen die we dadelijk zullen uitsluiten).
   * Selecteer in de vervolgkeuzelijsten **Exclude** (Uitsluiten) en **column names** (kolomnamen) en klik in het tekstvak. Er wordt een lijst met kolommen weergegeven. Selecteer **normalized-losses** om aan het tekstvak toe te voegen.
   * Klik op de knop met het vinkje (OK) om de kolomkiezer te sluiten.
     
     ![Kolommen selecteren][screen3]
     
     Het deelvenster met eigenschappen van de module **Select Columns in Dataset** (Kolommen in gegevensset selecteren) geeft aan dat alle kolommen uit de gegevensset worden doorgegeven, met uitzondering van **normalized-losses**.
     
     ![Kolommen in gegevensseteigenschappen selecteren][screen4]
     
     > [!TIP]
     > U kunt een opmerking aan een module toevoegen door te dubbelklikken op de module en tekst in te voeren. Zodoende kunt u in één oogopslag zien wat de module in uw experiment doet. Dubbelklik in dit geval op de module [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren) en typ de opmerking 'normalized-losses uitsluiten'.
     > 
     > 
3. Sleep de module [Clean Missing Data][clean-missing-data] (Ontbrekende gegevens opschonen) naar het canvas en verbindt deze met de module [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren). Selecteer in het deelvenster **Properties** (Eigenschappen) de optie **Remove entire row** (Hele rij verwijderen) onder **Cleaning mode** (Opschoonmodus) om de gegevens op te schonen door rijen met ontbrekende waarden te verwijderen. Dubbelklik op de module en typ de opmerking 'Rijen met ontbrekende waarde verwijderen'.
   
    ![Ontbrekende gegevenseigenschappen opschonen][screen4a]
4. Voer het experiment uit door op **RUN** (UITVOEREN) onder het experimentcanvas te klikken.

Wanneer het experiment is voltooid, wordt er bij alle modules een groen vinkje weergegeven om aan te geven dat deze zijn voltooid. In de rechterbovenhoek wordt de status **Finished running** (Uitvoeren voltooid) weergegeven.

![Eerste experiment uitvoeren][screen5]

Tot nu toe hebben we in het experiment alleen de gegevens opgeschoond. Als u de opgeschoonde gegevensset wilt weergeven, klikt u op de uitvoerpoort links van de module [Clean Missing Data][clean-missing-data] (Ontbrekende gegevens opschonen) ('Opgeschoonde gegevensset') en selecteer **Visualize** (Visualiseren). Zoals u kunt zien, is de kolom **normalized-losses** verwijderd en ontbreken er geen waarden meer.

Nu de gegevens zijn opgeschoond, kunt u opgeven welke functies u wilt gebruiken in het voorspellende model.

## Stap 3: functies definiëren
In machine learning zijn *functies* afzonderlijke meetbare eigenschappen van iets waarin u geïnteresseerd bent. In onze gegevensset staat elke rij voor één auto en elke kolom bevat een kenmerk van die auto.

Voor een goede set kenmerken voor het maken van een voorspellend model, moet u experimenteren en beschikken over kennis van het probleem dat u wilt oplossen. Bepaalde kenmerken zijn beter voor het voorspellen van het doel dan andere. Bovendien bestaat er tussen sommige kenmerken een nauwe correlatie (bijvoorbeeld tussen city-mpg en highway-mpg), waardoor ze weinig nieuwe informatie aan het model toevoegen en ze kunnen worden verwijderd.

Laten we een model bouwen dat gebruikmaakt van een subset kenmerken onze gegevensset. U kunt terugkeren en andere kenmerken selecteren om het experiment vervolgens opnieuw uit te voeren en te zien of u betere resultaten krijgt. Maar probeer om te beginnen de volgende functies:

    make, body-style, wheel-base, engine-size, horsepower, peak-rpm, highway-mpg, price


1. Sleep nog een module [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren) naar het experimentcanvas en koppel de module aan de linkeruitvoerpoort van de module [Clean Missing Data][clean-missing-data] (Ontbrekende gegevens opschonen). Dubbelklik op de module en typ 'Select features for prediction' (Kenmerken voor de voorspelling selecteren).
2. Klik in het deelvenster **Properties** (Eigenschappen) op **Launch column selector** (Kolomselector starten).
3. Klik op **With rules** (Met regels).
4. Klik onder **Begin With** (Beginnen met) op **No columns** (Geen kolommen) en selecteer **Include** (Inclusief) en **column names** (Kolomnamen) in de filterrij. Geef onze lijst met kolomnamen op. Dit zorgt ervoor dat de module alleen die kolommen doorgeeft die wij opgeven.
   
   > [!TIP]
   > Door het uitvoeren van het experiment hebben we ervoor gezorgd dat de kolomdefinities voor onze gegevens van de gegevensset via de module [Clean Missing Data][clean-missing-data] (Schone, ontbrekende gegevens) worden doorgegeven. Dit betekent dat andere modules waarmee u verbinding maakt, ook informatie uit de gegevensset hebben.
   > 
   > 
5. Klik op de knop met het vinkje (OK).

![Kolommen selecteren][screen6]

Hiermee produceert u de gegevensset die wordt gebruikt in het leeralgoritme in de volgende stappen. U kunt later terugkeren en het opnieuw proberen met een andere selectie kenmerken.

## Stap 4: een leeralgoritme kiezen en toepassen
Nu de gegevens klaar zijn, kunt u een voorspellend model bouwen door het model te trainen en te testen. We gebruiken onze gegevens om het model te trainen. Vervolgens testen we het model om te controleren in hoeverre de prijzen met dit model kunnen worden voorspeld. U hoeft zich in op dit moment nog niet af te vragen waarom we een model trainen en vervolgens testen.

*Classificatie* en *regressie* zijn twee soorten beheerde machine learning-technieken. Classificatie voorspelt een antwoord uit een gedefinieerde set categorieën, zoals een kleur (rood, blauw of groen). Regressie wordt gebruikt om een getal te voorspellen.

Omdat we de prijs willen voorspellen, wat een getal is, gebruiken we een regressiemodel. Voor dit voorbeeld trainen we een eenvoudig *lineair regressiemodel* dat we in de volgende stap zullen testen.

1. We gebruiken onze gegevens zowel voor trainings- als testdoeleinden door ze op te splitsen in twee afzonderlijke trainings- en testsets. Selecteer de module [Split Data][split] (Gegevens splitsen), sleep deze naar het experimentcanvas en koppel de module aan de uitvoer van de laatste module [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren). Stel **Fraction of rows in the first output dataset** (Fractie van rijen in de eerste uitvoergegevensset) in op 0,75. Zodoende gebruiken we 75 procent van de gegevens om het model te trainen en gebruiken we 25 procent van de gegevens om het model te testen.
   
   > [!TIP]
   > Door de parameter **Random seed** (Willekeurige seed) te wijzigen, kunt u verschillende willekeurig samples voor trainings- en testdoeleinden gebruiken. Deze parameter bepaalt de seeding van de pseudo-willekeurige nummergenerator.
   > 
   > 
2. Voer het experiment uit. Zodoende kunnen de modules [Select Columns in Dataset][select-columns] (Kolommen in gegevensset selecteren) en [Split Data][split] (Gegevens splitsen) kolomdefinities doorgeven aan de modules die we hierna zullen toevoegen.  
3. Als u een leeralgoritme wilt selecteren, moet u de categorie **Machine Learning** in het modulepalet links van het canvas uitvouwen en vouwt u vervolgens **Initialize Model** (Model initialiseren) uit. Er worden verschillende categorieën weergegeven die kunnen worden gebruikt om de machine learning-algoritmen te initialiseren.
   
    Selecteer voor dit experiment de module [Linear Regression][linear-regression] (Lineaire regressie) in de categorie **Regression** (Regressie) (u kunt de module ook zoeken door 'linear regression' in het zoekvak van het palet te typen) en sleep de module naar het experimentcanvas.
4. Zoek de module [Train Model][train-model] (Model trainen) en sleep deze naar het experimentcanvas. Koppel de linkerinvoerpoort aan de uitvoer van de module [Linear Regression][linear-regression] (Lineaire regressie). Koppel de rechteruitvoerpoort aan de trainingsgegevensuitvoer (linkerpoort) van de module [Split Data][split] (Gegevens splitsen).
5. Selecteer de module [Train Model][train-model] (Model trainen), klik in het deelvenster **Properties** (Eigenschappen) op **Launch column selector** (Kolomselector starten) en selecteer vervolgens de kolom **price** (prijs). Dit is de waarde die door het model wordt voorspeld.
   
    ![De kolom 'price' (prijs) selecteren][screen7]
6. Voer het experiment uit.

Het resultaat is een getraind regressiemodel dat kan worden gebruikt om nieuwe samples te beoordelen voor het maken van voorspellingen.

![Het machine learning-algoritme toepassen][screen8]

## Stap 5: prijzen van nieuwe auto's voorspellen
Nu we het model met 75 procent van de gegevens hebben getraind, kunnen we het model gebruiken om de overige 25 procent van onze gegevens te beoordelen om te zien hoe goed het model werkt.

1. Zoek de module [Score Model][score-model] (Scoremodel), sleep deze naar het experimentcanvas en koppel de module aan de linkerinvoerpoort van de module [Train Model][train-model] (Model trainen). Koppel de rechterinvoerpoort om de gegevensuitvoer (rechterpoort) van de module [Split Data][split] (Gegevens splitsen) te testen.  
   
    ![De module Score Model (Scoremodel)][screen8a]
2. Als u het experiment wilt uitvoeren en de uitvoer van de module [Score Model][score-model] (Scoremodel) wilt weergeven, klikt u op de uitvoerpoort en selecteert u **Visualize** (Visualiseren). De uitvoer bevat de voorspelde waarden voor de prijs en de bekende waarden uit de testgegevens.  
3. Als u tot slot de kwaliteit van de resultaten wilt testen, selecteert u de module [Evaluate Model][evaluate-model] (Model evalueren), sleept u deze naar het experimentcanvas en koppelt u de module aan de linkerinvoerpoort van de module [Score Model][score-model] (Scoremodel). (Er zijn twee invoerpoorten omdat de module [Evaluate Model][evaluate-model] (Model evalueren) kan worden gebruikt om twee modellen met elkaar te vergelijken.)
4. Voer het experiment uit.

Als u de uitvoer van de module [Evaluate Model][evaluate-model] (Model evalueren) wilt weergeven, klikt u op de uitvoerpoort en selecteert u **Visualize** (Visualiseren). De volgende statistieken worden weergegeven voor het model:

* **Mean Absolute Error** (MAE): het gemiddelde aan absolute fouten (een *fout* is het verschil tussen de voorspelde waarde en de werkelijke waarde).
* **Root Mean Squared Error** (RMSE): de wortel uit het gemiddelde aan gekwadrateerde fouten voor de voorspellingen op basis van de testgegevensset.
* **Relative Absolute Error**: het gemiddelde aan absolute fouten ten opzichte van het absolute verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
* **Relative Squared Error**: het gemiddelde aan gekwadrateerde fouten ten opzichte van het gekwadrateerde verschil tussen de werkelijke waarden en het gemiddelde van alle werkelijke waarden.
* **Coefficient of Determination**: de determinatiecoëfficiënt, ook wel **R²-waarde** genoemd, is een statistische meetwaarde die aangeeft hoe goed het model is in het voorspellen van de gegevens.

Voor elk van de foutstatistieken geldt: hoe kleiner hoe beter. Een kleine waarde geeft aan dat de voorspelling dichter bij de werkelijke waarde ligt. Hoe dichter de **determinatiecoëfficiënt** bij één (1.0) ligt, hoe nauwkeuriger de voorspellingen.

![Evaluatieresultaten][screen9]

Het laatste experiment ziet er als volgt uit:

![Machine learning-zelfstudie: volledig lineair regressie-experiment waarbij gebruik wordt gemaakt van voorspellende modelleringstechnieken.][screen10]

## Volgende stappen
Nu u de aanvankelijke machine learning-zelfstudie hebt voltooid en u een experiment hebt gemaakt, kunt u het experiment herhalen om het model te verbeteren. U kunt bijvoorbeeld de kenmerken wijzigen die u voor uw voorspelling gebruikt. Of u kunt de eigenschappen wijzigen van het algoritme [Linear Regression][linear-regression] (Lineaire regressie) of een compleet ander algoritme proberen. U kunt zelfs meerdere machine learning-algoritmen tegelijk aan uw experiment toevoegen en de module [Evaluate Model][evaluate-model] (Model evalueren) gebruiken om ze te vergelijken.

> [!TIP]
> Gebruik de knop **SAVE AS** (OPSLAAN ALS) onder het experimentcanvas om herhalingen van uw experiment op te slaan. U kunt alle herhalingen van uw experiment weergeven door onder het canvas te klikken op **VIEW RUN HISTORY** (UITVOERINGSGESCHIEDENIS WEERGEVEN). Zie [Experimentherhalingen in Azure Machine Learning Studio beheren][runhistory] voor meer informatie.
> 
> 

[runhistory]: machine-learning-manage-experiment-iterations.md

Als u tevreden bent over het model, kunt u dit implementeren als een webservice om de prijzen van auto's te voorspellen op basis van nieuwe gegevens. Zie [Een Azure Machine Learning-webservice implementeren][publish] voor meer informatie.

[publish]: machine-learning-publish-a-machine-learning-web-service.md

Zie [Een voorspellende oplossing met Azure Machine Learning ontwikkelen][walkthrough] voor een uitgebreid en gedetailleerd overzicht van de voorspellende modelleringstechnieken die kunnen worden gebruikt om een model te maken, te trainen, te beoordelen en te implementeren.

[walkthrough]: machine-learning-walkthrough-develop-predictive-solution.md

<!-- Images -->
[screen1]:./media/machine-learning-create-experiment/screen1.png
[screen1a]:./media/machine-learning-create-experiment/screen1a.png
[screen1b]:./media/machine-learning-create-experiment/screen1b.png
[screen1c]: ./media/machine-learning-create-experiment/screen1c.png
[screen2]:./media/machine-learning-create-experiment/screen2.png
[screen3]:./media/machine-learning-create-experiment/screen3.png
[screen4]:./media/machine-learning-create-experiment/screen4.png
[screen4a]:./media/machine-learning-create-experiment/screen4a.png
[screen5]:./media/machine-learning-create-experiment/screen5.png
[screen6]:./media/machine-learning-create-experiment/screen6.png
[screen7]:./media/machine-learning-create-experiment/screen7.png
[screen8]:./media/machine-learning-create-experiment/screen8.png
[screen8a]:./media/machine-learning-create-experiment/screen8a.png
[screen9]:./media/machine-learning-create-experiment/screen9.png
[screen10]:./media/machine-learning-create-experiment/complete-linear-regression-experiment.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[clean-missing-data]: https://msdn.microsoft.com/library/azure/d2c5ca2f-7323-41a3-9b7e-da917c99f0c4/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/



<!--HONumber=Sep16_HO3-->


