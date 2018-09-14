---
title: Afbeeldingsclassificatie met behulp van CNTK in Azure Machine Learning Workbench | Microsoft Docs
description: Trainen, evalueren en implementeren van een aangepaste installatiekopie classificatie-model met behulp van Azure ML Workbench.
services: machine-learning
documentationcenter: ''
author: PatrickBue
ms.author: pabuehle
manager: mwinkle
ms.reviewer: marhamil, mldocs, garyericson, jasonwhowell
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 10/17/2017
ms.openlocfilehash: 667636aac49d2622ba1a6b45d7c8af61b9609c55
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579177"
---
# <a name="image-classification-using-azure-machine-learning-workbench"></a>Afbeeldingsclassificatie met behulp van Azure Machine Learning Workbench

Afbeelding classificatie methoden kunnen worden gebruikt voor het oplossen van een groot aantal Computer Vision-problemen.
Deze omvatten het ontwikkelen van modellen die antwoorden op vragen zoals: *Is een aanwezig zijn in de installatiekopie van het OBJECT?* waar OBJECT kan bijvoorbeeld worden *hond*, *auto*, of  *Verzend*. Of complexere vragen zoals: *welke klasse van ogen ziekten ernst is evinced door retinal scan van de patiënt?*.

Deze zelfstudie wordt het oplossen van dergelijke problemen. We laten zien hoe om te trainen, evalueren en implementeren van uw eigen installatiekopie classificatie model met de [Microsoft Cognitive Toolkit (CNTK) ](https://docs.microsoft.com/cognitive-toolkit/) voor deep learning.
Voorbeeld van de installatiekopieën worden geleverd, maar de lezer kan ook hun eigen gegevensset brengen en hun eigen aangepaste modellen trainen.

Computer Vision oplossingen vereist traditioneel diepgaande kennis om handmatig te identificeren en implementeren zogeheten *functies*, die de gewenste gegevens in afbeeldingen markeren.
Deze handmatige methode gewijzigd in 2012 met de beroemde [AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) [1] Deep Learning papier, en op dit moment, Deep Neural Networks (DNN) worden gebruikt om automatisch te vinden deze functies.
Dnn's heeft geleid tot een enorme verbetering in het veld niet alleen voor de classificatie van afbeeldingen, maar ook voor andere problemen met Computer Vision, zoals de objectdetectie van het en afbeelding gelijkenis.


## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie met GitHub-opslagplaats
[https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK](https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK)

## <a name="overview"></a>Overzicht

In deze zelfstudie is opgesplitst in drie delen:

- Deel 1 laat zien hoe om te trainen, evalueren en implementeren van een afbeeldingsclassificatiesysteem met behulp van een vooraf getrainde DNN als featurizer en training van een SVM op de uitvoer ervan weergegeven.
- Deel 2 wordt weergegeven over het verbeteren van nauwkeurigheid door, bijvoorbeeld de DNN verfijnen, in plaats van als een vaste featurizer gebruiken.
- Deel 3 wordt uitgelegd hoe u uw eigen gegevensset gebruiken in plaats van de voorbeeld-afbeeldingen en indien nodig, over het maken van uw eigen gegevensset door slijmen afbeeldingen uit het net.

Hoewel eerdere ervaring met machine learning en CNTK niet vereist is, is het handig om te begrijpen van de onderliggende principes. Nauwkeurigheid van getallen, training tijd, enz. die zijn gerapporteerd in de zelfstudie zijn alleen ter referentie en de werkelijke waarden bij het uitvoeren van de code bijna zeker verschillen.


## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn er als volgt uit:

1. Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies zijn beschikbaar).
2. De [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) volgende de [snel starten-installatiehandleiding](../service/quickstart-installation.md) aan het programma te installeren en een werkruimte maken.  
3. Een Windows-machine. Windows-besturingssysteem is nodig omdat de Workbench alleen Windows en MacOS, terwijl de Microsoft Cognitive Toolkit ondersteunt (die we gebruiken als deep learning-bibliotheek) biedt alleen ondersteuning voor Windows en Linux.
4. Een GPU die toegewezen is niet vereist voor het uitvoeren van de training SVM in deel 1, maar dit is nodig voor het verfijnen van de DNN in deel 2 beschreven. Als u niet over een sterke GPU, wilt trainen op meerdere GPU's of geen een Windows-machine hebt, klikt u vervolgens kunt u overwegen van Azure Deep Learning Virtual Machine met Windows-besturingssysteem. Zie [hier](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning) voor een 1-klikken-Implementatiehandleiding. Zodra geïmplementeerd, verbinding maken met de virtuele machine via een verbinding met extern bureaublad, er Workbench installeren en de code lokaal uitvoeren van de virtuele machine.
5. Verschillende Python-bibliotheken zoals OpenCV moeten worden geïnstalleerd. Klik op *Open Command Prompt* uit de *bestand* menu in de Workbench en de volgende opdrachten voor het installeren van deze afhankelijkheden:  
    - `pip install https://cntk.ai/PythonWheel/GPU/cntk-2.2-cp35-cp35m-win_amd64.whl`  
    - `pip install opencv_python-3.3.1-cp35-cp35m-win_amd64.whl` Nadat u hebt gedownload van de OpenCV steeds uit http://www.lfd.uci.edu/~gohlke/pythonlibs/ (de exacte bestandsnaam en -versie kunnen wijzigen)
    - `conda install pillow`
    - `pip install -U numpy`
    - `pip install bqplot`
    - `jupyter nbextension enable --py --sys-prefix bqplot`
    - `jupyter nbextension enable --py widgetsnbextension`

### <a name="troubleshooting--known-bugs"></a>Het oplossen van problemen / bekende fouten
- Een GPU die nodig is voor deel 2, en anders de fout 'Batch training normalisering van CPU is nog niet geïmplementeerd' is een opgetreden tijdens het verfijnen van de DNN.
- Fouten tijdens de training DNN-geheugen kunnen worden vermeden door minibatch verkleinen (variabele `cntk_mb_size` in `PARAMETERS.py`).
- De code is getest met behulp van CNTK 2.2 en moet ook uitvoeren op oudere (tot v2.0) en nieuwere versies zonder of alleen kleine wijzigingen.
- De Azure Machine Learning Workbench was op het moment van schrijven, problemen met notitieblokken groter is dan 5 MB. Notitieblokken van deze grote omvang kunnen gebeuren als de notebook wordt opgeslagen met alle cellen uitvoer weergegeven. Als u deze fout optreedt, en vervolgens de opdrachtprompt vanuit het menu bestand in Workbench openen, voeren `jupyter notebook`, open het notitieblok, schakel alle uitvoer en sla de notebook. Nadat u deze stappen uitvoert, wordt de notebook goed in de Azure Machine Learning Workbench opnieuw openen.
- Alle scripts die zijn opgegeven in dit voorbeeld moeten lokaal worden uitgevoerd en niet op bijvoorbeeld een docker externe omgeving. Alle notitieblokken moeten worden uitgevoerd met kernel ingesteld op de kernel lokale-project met de naam 'PROJECTNAME lokale' (bijvoorbeeld ' myImgClassUsingCNTK lokale").

    
## <a name="create-a-new-workbench-project"></a>Een nieuw workbench-project maken

Een nieuw project te maken met behulp van dit voorbeeld als een sjabloon:
1.  Open Azure Machine Learning Workbench.
2.  Op de **projecten** pagina, klikt u op de **+** Meld u aan en selecteer **nieuw Project**.
3.  In de **nieuw Project maken** deelvenster, vult u in de gegevens voor het nieuwe project.
4.  In de **zoeken naar projectsjablonen** het zoekvak, typ 'Afbeeldingsclassificatie' en selecteer de sjabloon.
5.  Klik op **Create**.

Deze stappen uitvoert, maakt de projectstructuur die hieronder wordt weergegeven. De projectmap is beperkt tot minder dan 25 megabytes (MB) zijn omdat met de Azure Machine Learning Workbench maakt een kopie van deze map na elke uitvoering (voor het uitvoeringsgeschiedenis). Daarom kan alle installatiekopie en de tijdelijke bestanden worden opgeslagen en naar de map *~/Desktop/imgClassificationUsingCntk_data* (aangeduid als *DATA_DIR* in dit document).

  Map| Beschrijving
  ---|---
  aml_config/|                           Map met de Azure Machine Learning Workbench-configuratiebestanden
  bibliotheken /|                              Map met alle Python en Jupyter ondersteunende functies
  notitieblokken /|                              Map met alle notitieblokken
  resources /|                              Map met alle resources (voor de voorbeeld-url van manier installatiekopieën)
  scripts /|                              Map met alle scripts
  PARAMETERS.py|                       Python-script alle parameters op te geven
  Readme.MD|                           Dit Leesmij-document


## <a name="data-description"></a>Beschrijving van de gegevens

Deze zelfstudie wordt gebruikt als voorbeeld een bovenste hoofdtekst kleding patroon gegevensset die bestaan uit maximaal 428 installatiekopieën wordt uitgevoerd. Elke installatiekopie is gemarkeerd als een van drie verschillende structuren (onderbroken, striped, leopard). We het aantal afbeeldingen kleine opgeslagen zodat deze zelfstudie kan snel worden uitgevoerd. De code is echter goed geteste en werkt samen met tienduizenden afbeeldingen of meer. Alle installatiekopieën zijn zoals wordt uitgelegd in voorraad aantekeningen [deel 3](#using-a-custom-dataset). De URL's met hun respectieve kenmerken worden weergegeven in afbeelding de */resources/fashionTextureUrls.tsv* bestand.

Het script `0_downloadData.py` downloadt alle installatiekopieën op de *DATA_DIR/afbeeldingen/fashionTexture/* directory. Sommige van de 428 URL's zijn waarschijnlijk verbroken. Dit is niet een probleem en houdt in dat we iets minder installatiekopieën hebben voor trainings- en testdoeleinden. Alle scripts die zijn opgegeven in dit voorbeeld moeten lokaal worden uitgevoerd en niet op bijvoorbeeld een docker externe omgeving.

De volgende afbeelding ziet u voorbeelden van de kenmerken die met punten (links), striped (midden) en leopard (rechts). Aantekeningen zijn gedaan op basis van de hoofdtekst van de bovenste kleding-item.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/examples_all.jpg"  alt="alt text" width="700">
</p>


## <a name="part-1---model-training-and-evaluation"></a>Deel 1: Model voor training en evaluatie

In het eerste deel van deze zelfstudie, zijn we een systeem dat wordt gebruikt, maar niet wijzigen, een vooraf getrainde deep neural network training. Deze vooraf getrainde DNN wordt gebruikt als een featurizer en een lineaire SVM wordt getraind om te voorspellen van het kenmerk (gestippeld, striped of leopard) van een bepaalde installatiekopie.

We nu beschreven deze benadering in detail, stap voor stap en weergeven welke scripts moeten worden uitgevoerd. We raden aan na elke stap om te controleren welke bestanden zijn geschreven en waar ze worden geschreven naar.

Alle belangrijke parameters zijn opgegeven en een korte uitleg te vinden, op één plek: de `PARAMETERS.py` bestand.




### <a name="step-1-data-preparation"></a>Stap 1: Gegevens voorbereiden
`Script: 1_prepareData.py. Notebook: showImages.ipynb`

De notebook `showImages.ipynb` kan worden gebruikt voor het visualiseren van de installatiekopieën, en om op te lossen van de aantekening indien nodig. Als u wilt uitvoeren van het notitieblok, opent u het in Azure Machine Learning Workbench, klik op 'Start-Notebook Server' als deze optie wordt weergegeven, wijzigt in de kernel lokale-project met de naam 'PROJECTNAME lokale' (bijvoorbeeld de "myImgClassUsingCNTK-lokaal'), en voer vervolgens alle cellen in de laptop. Zie de sectie Probleemoplossing in dit document als er een fout optreedt klagen dat de laptop is te groot om te worden weergegeven.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showImages.jpg" alt="alt text" width="700"/>
</p>

Nu Voer het script met de naam `1_prepareData.py`, welke wijst alle installatiekopieën op een van beide de training ingesteld of de test is ingesteld. Deze toewijzing is sluiten elkaar wederzijds uit - er is geen afbeelding training wordt ook gebruikt voor het testen of vice versa. Standaard een willekeurige 75% van de installatiekopieën van de kenmerkklasse zijn toegewezen aan de training en de overige 25% zijn toegewezen aan een test. Alle gegevens die zijn gegenereerd door het script worden opgeslagen in de *DATA_DIR/proc/fashionTexture/* map.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_1_white.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-2-refining-the-deep-neural-network"></a>Stap 2: De Deep Neural Network verfijnen
`Script: 2_refineDNN.py`

Zoals we in deel 1 van deze zelfstudie is uitgelegd, de vooraf getrainde DNN vaste wordt opgeslagen (dat wil zeggen, het is niet verfijnd). Echter, het script met de naam `2_refineDNN.py` nog steeds wordt uitgevoerd in deel 1, tijdens het laden van een vooraf getrainde [ResNet](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf) [2]-model en wijzigt u dit bijvoorbeeld, als u wilt toestaan voor hogere resolutie van de afbeelding. Deze stap is (seconden) van een snelle en een GPU die niet is vereist.

In deel 2 van de zelfstudie een wijziging in de PARAMETERS.py bestand oorzaken de `2_refineDNN.py` script om de vooraf getrainde DNN ook te verfijnen. Standaard uitvoeren we 45 training epoches tijdens verfijning.

In beide gevallen klikt u vervolgens het uiteindelijke model is geschreven naar het bestand *DATA_DIR/proc/fashionTexture/cntk_fixed.model*.

### <a name="step-3-evaluate-dnn-for-all-images"></a>Stap 3: Evalueer DNN voor alle installatiekopieën
`Script: 3_runDNN.py`

We kunnen de (mogelijk verfijnd) DNN van de laatste stap op parametriseer nu onze installatiekopieën gebruiken. Een installatiekopie van een opgegeven als invoer voor de DNN, is de uitvoer de vector 512-tekst uit de voorlaatste laag van het model. Deze vector is veel kleiner dan de afbeelding zelf dimensionale. Niettemin moet bevatten (en zelfs markeren) alle informatie in de afbeelding die relevant zijn voor het herkennen van de installatiekopie-kenmerk, dat is, als het item kleding een stippellijn met heeft striped, of leopard patroon.

Alle van de weergaven van de installatiekopie van DNN worden opgeslagen in het bestand *DATA_DIR/proc/fashionTexture/cntkFiles/features.pickle*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_4_white.jpg" alt="alt text" width="700"/>
</p>


### <a name="step-4-support-vector-machine-training"></a>Stap 4: Support Vector Machine training
`Script: 4_trainSVM.py`

De 512-tekst voorstellingen berekend in de vorige stap zijn nu gebruikt voor het trainen van een classificatie SVM: een afbeelding als invoer worden gegeven, de SVM levert een score voor elk kenmerk aanwezig zijn. In onze gegevensset voorbeeld betekent dit een score voor 'striped', voor 'onderbroken' en 'leopard'.

Script `4_trainSVM.py` laadt de training-installatiekopieën, een SVM voor verschillende waarden van de parameter regularisatie (slack) C traint en de SVM houdt met de hoogste nauwkeurigheid. De nauwkeurigheid van de classificatie is afgedrukt op de console en in de Workbench getekend. Voor de gegevens van het opgegeven patroon moet deze waarden ongeveer 100% en % 88 respectievelijk. Ten slotte het getrainde SVM is geschreven naar het bestand *DATA_DIR/proc/fashionTexture/cntkFiles/svm.np*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/vienna_svm_log_zoom.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-5-evaluation-and-visualization"></a>Stap 5: Evaluatie en visualisatie
`Script: 5_evaluate.py. Notebook: showResults.ipynb`

De nauwkeurigheid van de classificatie getrainde installatiekopie kan worden gemeten met behulp van het script `5_evaluate.py`. De script-scores alle test-installatiekopieën met behulp van het getrainde SVM classificatie, wijst elke installatiekopie het kenmerk met de hoogste score en vergelijkt de voorspelde kenmerken met de grond waarheid aantekeningen.

De uitvoer van het script `5_evaluate.py` wordt hieronder weergegeven. De nauwkeurigheid van de classificatie van elke afzonderlijke categorie is en de nauwkeurigheid van de volledige testset ('algehele nauwkeurigheid') en het gemiddelde berekend voor de afzonderlijke nauwkeurigheden ('algemene klasse gemiddelde nauwkeurigheid'). 100% komt overeen met de best mogelijke nauwkeurigheid en 0% tot de slechtste. Willekeurige raden gemiddeld geeft als resultaat een klasse gemiddelde nauwkeurigheid van 1 over het aantal kenmerken: in ons geval deze nauwkeurigheid 33,33% zou zijn. Deze resultaten aanzienlijk verbeteren bij het gebruik van een hogere resolutie invoer zoals `rf_inputResoluton = 1000`echter ten koste van de langere DNN berekening tijden.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_6_white.jpg" alt="alt text" width="700"/>
</p>

Naast de nauwkeurigheid van wordt de ROC-curve getekend met het desbetreffende gebied-onder-curve (links); en de verwarringsmatrix (rechts) wordt weergegeven:

<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat.jpg" alt="alt text" width="700"/>
</p>

Ten slotte de notebook `showResults.py` wordt geleverd door de test-installatiekopieën bladeren en visualiseren van hun respectieve classificatie scores. Zoals wordt beschreven in stap 1, moet elke laptop in dit voorbeeld gebruikt u de kernel lokale-project met de naam 'PROJECTNAME lokale':
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showResults.jpg" alt="alt text" width="700"/>
</p>





### <a name="step-6-deployment"></a>Stap 6: implementatie
`Scripts: 6_callWebservice.py, deploymain.py. Notebook: deploy.ipynb`

Het getrainde systeem kan nu worden gepubliceerd als een REST-API. Implementatie wordt uitgelegd in de notebook `deploy.ipynb`, en op basis van functionaliteit in de Azure Machine Learning Workbench (Vergeet niet om in te stellen als een kernel van de kernel lokale-project met de naam "PROJECTNAME lokale"). Zie ook de Implementatiesectie uitstekende van de [zelfstudie IRIS](tutorial-classifying-iris-part-3.md) gerelateerde voor de implementatie van meer informatie.

Zodra geïmplementeerd, de webservice kan worden aangeroepen met behulp van het script `6_callWebservice.py`. Houd er rekening mee dat het IP-adres (lokaal of in de cloud) van de webservice moet eerst worden ingesteld in het script. De notebook `deploy.ipynb` wordt uitgelegd hoe u dit IP-adres vinden.








## <a name="part-2---accuracy-improvements"></a>Deel 2 - nauwkeurigheid verbeteringen

In deel 1, we hebt u geleerd hoe u een installatiekopie van een door een lineaire Support Vector Machine op de 512-tekst-uitvoer van een Deep Neural Network training classificeren. Deze DNN is vooraf getrainde op miljoenen afbeeldingen en de voorlaatste laag geretourneerd als de functie vector. Deze aanpak is snel omdat de DNN wordt gebruikt als-is, maar toch vaak goede resultaten oplevert.

We bieden nu verschillende manieren voor het verbeteren van de nauwkeurigheid van het model van deel 1. Met name verfijnen we de DNN in plaats van deze vast te houden.

### <a name="dnn-refinement"></a>DNN verfijning

In plaats van een SVM kunt een de classificatie rechtstreeks in het neurale netwerk doen. Dit wordt bereikt door een nieuwe laatste laag toe te voegen aan de vooraf getrainde DNN, waarbij de 512-tekst uit de voorlaatste laag als invoer. Het voordeel van de classificatie in de DNN doen is dat nu het volledige netwerk kan opnieuw worden getraind met behulp van backpropagation. Deze benadering leidt vaak tot veel betere classificatie nauwkeurigheden vergeleken met het gebruik van de vooraf getrainde DNN as-is echter ten koste van veel langere trainingstijd (zelfs met GPU).

Het Neurale netwerk in plaats van een SVM wordt gedaan door het veranderen van de variabele `classifier` in `PARAMETERS.py` van `svm` naar `dnn`. Klik, zoals beschreven in deel 1, moeten de scripts, met uitzondering van gegevens voor te bereiden (stap 1) en SVM training (stap 4) moet opnieuw worden uitgevoerd. DNN verfijning vereist een GPU. Als er geen GPU is gevonden of als de GPU is vergrendeld (bijvoorbeeld door een vorige uitvoering voor CNTK) script `2_refineDNN.py` genereert een fout. DNN training-geheugen fout op sommige GPU's, die kunnen worden vermeden door minibatch verkleinen kunt genereren (variabele `cntk_mb_size` in `PARAMETERS.py`).

Zodra training is voltooid, het verfijnde model is opgeslagen in *DATA_DIR/proc/fashionTexture/cntk_refined.model*, en een diagram waarin wordt getoond hoe de trainings- en testset classificatiefouten tijdens de training wijzigen. Houd er rekening mee in dit diagram die de fout op de trainingsset veel kleiner is dan voor de test is. Dit zogenaamde te veel passende gedrag kan worden verkleind, bijvoorbeeld met behulp van een hogere waarde voor het tarief Drop-out `rf_dropoutRate`.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_3_plot.png" alt="alt text" height="300"/>
</p>

Zoals te zien in het onderstaande diagram, wordt de nauwkeurigheid met DNN verfijning voor de opgegeven gegevensset 92.35% ten opzichte van de % 88.92 voordat (deel 1). In het bijzonder verbeteren de 'onderbroken' afbeeldingen aanzienlijk, met een ROC gebied-onder-curve van 0,98 met verfijning vs. 0.94 voordat. Gebruiken we een kleine gegevensset, en daarom zijn de werkelijke nauwkeurigheden uitvoeren van de code verschillen. Dit verschil is vanwege stochastische effecten, zoals de willekeurige splitsing van de afbeeldingen in trainings- en testsets.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat_dnn.jpg" alt="alt text" width="700"/>
</p>

### <a name="run-history-tracking"></a>Bijhouden van de geschiedenis uitvoeren

De Azure Machine Learning Workbench-winkels de geschiedenis van elke uitvoeren op Azure om toe te staan van de vergelijking van twee of meer uitvoeringen die zijn zelfs weken elkaar. Dit wordt gedetailleerd uitgelegd in de [zelfstudie Iris](tutorial-classifying-iris-part-2.md). Het wordt ook weergegeven in de volgende schermafbeeldingen waar we twee uitvoeringen van het script vergelijken `5_evaluate.py`, met behulp van een van beide verfijning DNN dat wil zeggen, `classifier = "dnn"`(uitvoeringsnummer 148) of SVM opleiding, dat wil zeggen, `classifier = "svm"` (uitvoeringsnummer 150).

In de eerste schermafbeelding leidt DNN verfijning tot betere nauwkeurigheden dan SVM training voor alle klassen. De tweede schermafbeelding ziet u alle metrische gegevens die worden bijgehouden, met inbegrip van wat de classificatie is. Deze tracering wordt uitgevoerd in het script `5_evaluate.py` door het aanroepen van het logboek van Azure Machine Learning Workbench. Bovendien het script ook worden opgeslagen in de matrix voor ROC-curve en verwarring om de *levert* map. Dit *levert* map is bijzonder in dat de inhoud ook wordt gevolgd door de functie van de geschiedenis van Workbench en kan daarom de uitvoerbestanden op elk gewenst moment, ongeacht of lokale kopieën zijn overschreven kunnen worden geopend.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison1.jpg" alt="alt text" width="700"/>
</p>

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison2b.jpg" alt="alt text" width="700"/>
</p>


### <a name="parameter-tuning"></a>Parameter afstemmen

Zoals geldt voor de meeste machine learning-projecten, vereist goede resultaten ophalen voor een nieuwe gegevensset zorgvuldige parameter afstemmen, evenals verschillende ontwerpbeslissingen te evalueren. Om te helpen bij deze taken, alle belangrijke parameters zijn opgegeven en een korte uitleg te vinden, op één plek: de `PARAMETERS.py` bestand.

Enkele van de meest veelbelovende mogelijkheden voor verbeteringen zijn:

- Gegevenskwaliteit: Zorg ervoor dat de trainings- en testset sets van hoge kwaliteit zijn. Dat wil zeggen, de afbeeldingen met aantekeningen correct, niet-eenduidige afbeeldingen zijn verwijderd (bijvoorbeeld kleding items met streepjes en punten) en de kenmerken zijn sluiten elkaar wederzijds uit (dat wil zeggen, dat deel uitmaakt van elke afbeelding naar exact één kenmerk gekozen).

- Als het object van belang klein in de afbeelding is vervolgens Afbeeldingsclassificatie benaderingen bekend is niet goed werken. In dergelijke gevallen kunt u overwegen een benadering voor het detecteren van object zoals beschreven in dit [zelfstudie](https://github.com/Azure/ObjectDetectionUsingCntk).
- DNN verfijning: de weliswaar belangrijkste parameter meteen is het leertempo `rf_lrPerMb`. Als de nauwkeurigheid van de training ingesteld (eerste afbeelding in deel 2) is niet dicht bij 0-5%, waarschijnlijk te wijten is een onjuiste het leertempo. De andere parameters te beginnen met `rf_` minder belangrijk zijn. Normaal gesproken moet de training fout exponentieel verlagen en worden dicht bij 0% na de training.

- Resolutie van invoer: de standaardresolutie van de installatiekopie is 224 x 224 pixels. Met behulp van hogere resolutie (parameter: `rf_inputResoluton`), bijvoorbeeld 448 x 448 of 896 x 896 pixels vaak aanzienlijke verbetert de nauwkeurigheid van de gegevens, maar DNN verfijning vertraagt. **Met behulp van hogere resolutie is bijna gaat de zon en bijna altijd verhoogt de nauwkeurigheid**.

- DNN sprake van redundante aanpassing van labels: te voorkomen dat een grote hiaat tussen de trainings- en test nauwkeurigheid tijdens verfijning van DNN (eerste afbeelding in deel 2). De lege ruimte kan worden teruggebracht tarieven voor Drop-out `rf_dropoutRate` van 0,5 of meer, en door het gewicht regularizer `rf_l2RegWeight`. Met een hoge Drop-out-tarief is vooral nuttig zijn als de invoer resolutie DNN hoog is.

- Probeer met behulp van diepere dnn's door het veranderen van `rf_pretrainedModelFilename` van `ResNet_18.model` aan `ResNet_34.model` of `ResNet_50.model`. Het model Resnet 50 is niet alleen beter, maar de uitvoer van de voorlaatste laag is van de grootte van 2048 tekst (in plaats van 512 tekst van de modellen ResNet robmazz van 18 en ResNet 34). Deze verbeterde dimensie kan met name nuttig zijn bij het trainen van een classificatie SVM.

## <a name="part-3---custom-dataset"></a>Deel 3 - aangepaste gegevensset

In deel 1 en 2, we getraind en geëvalueerd van een installatiekopie classificeringsmodel met behulp van de opgegeven hoofdtekst van de bovenste kleding structuren afbeeldingen. Nu laten we zien hoe u een aangepaste gebruiker gemaakte gegevensset in plaats daarvan gebruikt. 

### <a name="using-a-custom-dataset"></a>Met behulp van een aangepaste gegevensset

Eerst hebben we een overzicht van de mappenstructuur voor de gegevens van de structuur kleding. Houd er rekening mee hoe alle installatiekopieën voor de verschillende kenmerken zijn in de bijbehorende submappen *gestippelde*, *leopard*, en *striped* op *DATA_DIR/images / fashionTexture /*. U ziet ook hoe de naam van de installatiekopie-map vindt ook plaats de `PARAMETERS.py` bestand:
```python
datasetName = "fashionTexture"
```

Met behulp van een aangepaste gegevensset is net zo eenvoudig als deze mapstructuur reproduceren waarin alle installatiekopieën in submappen op basis van hun kenmerk en deze submappen kopiëren naar een nieuwe gebruiker opgegeven map zijn *DATA_DIR/afbeeldingen/newDataSetName/*. De enige codewijziging vereist is om in te stellen de `datasetName` variabele *newDataSetName*. Scripts 1-5 kunnen vervolgens worden uitgevoerd in volgorde en alle tussenliggende bestanden worden geschreven naar *DATA_DIR/proc/newDataSetName/*. Er zijn geen codewijzigingen vereist.

Het is belangrijk dat elke installatiekopie kan worden toegewezen aan exact één kenmerk. Zo zou het verkeerde kenmerken voor 'dieren' en 'leopard' hebben omdat de installatiekopie van een 'leopard' ook behoren zouden "dieren" zijn. Het is ook raadzaam om te verwijderen van installatiekopieën die niet-eenduidige en kan daarom moeilijk aantekeningen toevoegen aan.



### <a name="image-scraping-and-annotation"></a>Afbeelding afschrapen en aantekening

Verzamelen van een voldoende aantal interpretaties installatiekopieën voor trainings- als testdoeleinden kan lastig zijn. Er is een manier om dit probleem oplossen bij installatiekopieën van het Internet.

> [!IMPORTANT] 
> Voor alle installatiekopieën die u gebruikt, zorg ervoor dat schenden geen u auteursrechten. 

<!--
For example, see below the Bing Image Search results for the query *t-shirt striped*. As expected, most images indeed are striped t-shirts. The few incorrect or ambiguous images (such as column 1, row 1; or column 3, row 2) can be identified and removed easily:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/bing_search_striped.jpg" alt="alt text" width="600"/>
</p>
-->

Voor het genereren van een grote en diverse gegevensset moeten meerdere query's worden gebruikt. Bijvoorbeeld: 7\*3 = 21-query's kunnen worden gemaakt met automatisch alle combinaties van kleding items {bloes, hoodie, aangebreide, trui, shirt, t-shirt, uitoefenbaarheid} en {striped, decimale, leopard} kenmerken. Vervolgens downloaden van de bovenste 50 afbeeldingen per query zou leiden tot een maximum van 21 * 50 = 1050 installatiekopieën.

<!--
Rather than manually downloading images from Bing Image Search, it is much easier to instead use the [Cognitive Services Bing Image Search API](https://www.microsoft.com/cognitive-services/bing-image-search-api) which returns a set of image URLs given a query string.
-->

Sommige van de gedownloade afbeeldingen exact of in de buurt van dubbele waarden zijn (bijvoorbeeld door installatiekopie resolutie of jpg-artefacten verschillen). Deze dubbele vermeldingen moeten worden verwijderd zodat de splitsing trainings- en testset niet dezelfde afbeeldingen bevatten. Verwijderen van dubbele installatiekopieën kan worden bereikt met behulp van een hash-gebaseerde benadering waarmee u in twee stappen werkt: (i) eerst de tekenreeks hash wordt berekend voor alle installatiekopieën. (ii) in een tweede keer via de installatiekopieën, zijn alleen afbeeldingen worden bewaard met een hash-tekenreeks is die nog niet zichtbaar. Alle andere installatiekopieën worden verwijderd. We vinden het `dhash` benadering in de Python-bibliotheek `imagehash` en die worden beschreven in deze [blog](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html) om uit te voeren, met de parameter `hash_size` is ingesteld op 16. Het is OK onjuist verwijderen van enkele afbeeldingen unieke, zolang het merendeel van de echte dubbele records worden verwijderd.





## <a name="conclusion"></a>Conclusie

Er zijn enkele belangrijke kenmerken van het volgende voorbeeld:
- De code om te trainen, evalueren en implementeren van modellen voor classificatie van afbeeldingen.
- Demo-installatiekopieën die zijn opgegeven, maar gemakkelijk kan worden aangepast (één regel wijzigen) in de gegevensset van de eigen installatiekopie gebruiken.
- Status-of-the-art deskundige functies geïmplementeerd om te hoge nauwkeurigheid modellen op basis van Transfer Learning te trainen.
- Interactieve model ontwikkelen met Azure Machine Learning Workbench en Jupyter-Notebook.


## <a name="references"></a>Verwijzingen

[1] Alex Krizhevsky en Ilya Sutskever Geoffrey E. Hinton, [ _ImageNet classificatie met uitgebreide Convolutional Neural Networks_](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf). NIPS 2012.  
[2] Kaiming hij, Xiangyu Zhang Shaoqing Ren en Jian Sun, [ _diep resterende Learning voor Beeldherkenning_](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf). CVPR 2016.
