---
title: Afbeelding classificatie met CNTK in Azure Machine Learning Workbench | Microsoft Docs
description: Trainen, evalueren en een aangepaste installatiekopie classificatie-model met behulp van Azure ML-Workbench implementeren.
services: machine-learning
documentationcenter: 
author: PatrickBue
ms.author: pabuehle
manager: mwinkle
ms.reviewer: mawah, marhamil, mldocs, garyericson, jasonwhowell
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 10/17/2017
ms.openlocfilehash: 53d182d84c8f28c7b4055780a5b41df00fdc8583
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="image-classification-using-azure-machine-learning-workbench"></a>Classificatie van de installatiekopie met behulp van Azure Machine Learning Workbench

Afbeelding classificatie benaderingen kunnen worden gebruikt voor het oplossen van een groot aantal Computer Vision-problemen.
Deze omvatten ontwikkelen van modellen die antwoorden op vragen zoals: *Is aanwezig in de installatiekopie van het OBJECT?* waar OBJECT kan bijvoorbeeld worden *aquaduct*, *auto*, of  *verzenden*. Of complexere vragen zoals: *welke klasse ogen ziekte ernst wordt door deze geduld retinal scan evinced?*.

Deze zelfstudie adressen dergelijke problemen op te lossen. Laten we zien hoe trainen, evalueren en implementeren van uw eigen installatiekopie classificatie model met de [Microsoft cognitieve Toolkit (CNTK) ](https://docs.microsoft.com/cognitive-toolkit/) voor grondige learning.
Voorbeeld afbeeldingen worden geleverd, maar de lezer kan ook hun eigen gegevensset brengen en hun eigen aangepaste modellen trainen.

Computer Vision oplossingen vereist normaal diepgaande kennis om handmatig te identificeren en implementeren zogenaamde *functies*, die gewenste gegevens in de afbeeldingen markeren.
Deze handmatige methode is gewijzigd in 2012 met de beroemdheid [AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) [1] diepe Learning papier, en op dit moment, Deep Neural Networks (DNN) worden gebruikt om automatisch te vinden deze functies.
DNNs heeft geleid tot een enorme verbetering in het veld niet alleen voor classificatie van de installatiekopie, maar ook voor andere Computer Vision-problemen zoals object detectie- en image-overeenkomsten.


## <a name="link-to-the-gallery-github-repository"></a>Koppeling naar de galerie GitHub-opslagplaats
[https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK](https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK)

## <a name="overview"></a>Overzicht

Deze zelfstudie wordt opgedeeld in drie delen:

- Deel 1 ziet u hoe te trainen, evalueren en implementeren van een installatiekopie van classificatie-systeem met een vooraf getraind DNN als featurizer en training van een SVM op de uitvoer ervan weergegeven.
- Deel 2 vervolgens laat zien hoe nauwkeuriger door, bijvoorbeeld de DNN verfijnen dan als een vaste featurizer gebruiken.
- Deel 3 beschreven hoe u uw eigen gegevensset wordt gebruikt in plaats van de installatiekopieën van het opgegeven voorbeeld en indien nodig, het produceren van uw eigen gegevensset door slijmen afbeeldingen vanaf het netwerk.

Hoewel ervaring met machine learning en CNTK niet vereist is, is het handig zijn om de onderliggende principes te begrijpen. Nauwkeurigheid cijfers training tijd, enz. gerapporteerd in de zelfstudie zijn alleen ter informatie en de werkelijke waarden bij het uitvoeren van de code bijna zeker verschillen.


## <a name="prerequisites"></a>Vereisten

De vereisten voor het uitvoeren van dit voorbeeld zijn als volgt:

1. Een [Azure-account](https://azure.microsoft.com/free/) (gratis proefversies beschikbaar zijn).
2. De [Azure Machine Learning Workbench](./overview-what-is-azure-ml.md) volgende de [installatie snelstartgids](./quickstart-installation.md) het programma te installeren en het maken van een werkruimte.  
3. Een Windows-machine. Windows-besturingssysteem is nodig omdat de Workbench alleen Windows- en Mac OS tijdens cognitieve Toolkit van Microsoft ondersteunt (die we gebruiken als grondige learning bibliotheek) biedt alleen ondersteuning voor Windows en Linux.
4. Een speciale GPU is niet vereist voor het uitvoeren van de training SVM in deel 1, maar dit is nodig voor het verfijnen van de DNN beschreven in deel 2. Als u niet over een sterke GPU, wilt trainen op meerdere GPU's of hebben geen Windows-computer, klikt u vervolgens Overweeg het gebruik van Azure grondige Learning virtuele Machine met Windows-besturingssysteem. Zie [hier](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning) voor een 1 Klik-Implementatiehandleiding. Zodra geïmplementeerd, verbinding maken met de virtuele machine via een verbinding met extern bureaublad, Workbench er installeren en de code lokaal uitvoeren van de virtuele machine.
5. Verschillende Python-bibliotheken zoals OpenCV moeten worden geïnstalleerd. Klik op *opdrachtprompt openen* van de *bestand* menu in de Workbench en voer de volgende opdrachten voor het installeren van deze afhankelijkheden:  
    - `pip install https://cntk.ai/PythonWheel/GPU/cntk-2.2-cp35-cp35m-win_amd64.whl`  
    - `pip install opencv_python-3.3.1-cp35-cp35m-win_amd64.whl`na het downloaden van het muiswiel OpenCV van http://www.lfd.uci.edu/~gohlke/pythonlibs/ (de exacte bestandsnaam en de versie kunnen wijzigen)
    - `conda install pillow`
    - `pip install -U numpy`
    - `pip install bqplot`
    - `jupyter nbextension enable --py --sys-prefix bqplot`

### <a name="troubleshooting--known-bugs"></a>Voor probleemoplossing / bekende fouten
- Een GPU die nodig is voor deel 2 en anders de fout 'Batch normalisatie training van CPU is nog niet geïmplementeerd' wordt gegenereerd wanneer u probeert om de DNN te verfijnen.
- Fouten tijdens de training DNN-geheugen kunnen worden vermeden door minibatch verkleinen (variabele `cntk_mb_size` in `PARAMETERS.py`).
- De code is getest met CNTK 2.2 en moet ook uitvoeren op oudere (tot v2.0) en nieuwere versies zonder of alleen kleine wijzigingen.
- Op het moment van schrijven had de Workbench van Azure Machine Learning problemen met notitieblokken groter is dan 5 MB. Notitieblokken van deze grote omvang kunnen gebeuren als de laptop is opgeslagen met alle cellen uitvoer weergegeven. Als u deze fout optreedt en vervolgens de opdrachtprompt vanuit het menu bestand in de Workbench openen, uitvoermachtigingen `jupyter notebook`, opent u de notebook Wis alle uitvoer en opslaan van de notebook. Na deze stappen uitvoert, wordt de notebook correct in de Azure Machine Learning-Workbench opnieuw openen.
- Alle scripts die zijn opgegeven in dit voorbeeld moeten lokaal worden uitgevoerd en niet op bijvoorbeeld een docker externe omgeving. Alle notitieblokken moeten worden uitgevoerd met kernel ingesteld op de kernel lokaal project met de naam 'PROJECTNAME lokale' (bijvoorbeeld ' myImgClassUsingCNTK lokale').

    
## <a name="create-a-new-workbench-project"></a>Maak een nieuw project in de workbench

Een nieuw project in dit voorbeeld gebruiken als een sjabloon maken:
1.  Open Azure Machine Learning Workbench.
2.  Op de **projecten** pagina, klikt u op de  **+**  en selecteer **nieuw Project**.
3.  In de **nieuw Project maken** deelvenster Vul de informatie voor het nieuwe project.
4.  In de **zoeken projectsjablonen** het zoekvak, typ 'Installatiekopie classificatie' en selecteer de sjabloon.
5.  Klik op **Create**.

Deze stappen uitvoert, maakt de projectstructuur die hieronder wordt weergegeven. De projectmap is beperkt tot minder dan 25 megabytes (MB) mogelijk, omdat de Azure Machine Learning-Workbench een kopie van deze map na elke uitvoering maakt (zodat uitvoeringsgeschiedenis). Daarom alle installatiekopie en tijdelijke bestanden worden opgeslagen en naar de map *~/Desktop/imgClassificationUsingCntk_data* (aangeduid als *DATA_DIR* in dit document).

  Map| Beschrijving
  ---|---
  aml_config /|                           Map met de Azure Machine Learning Workbench configuratiebestanden
  bibliotheken /|                              Map met alle Python en Jupyter hulpfuncties
  laptops /|                              Map met alle notitieblokken
  resources /|                              Map met alle resources (voor de voorbeeld-url van wijze installatiekopieën)
  scripts /|                              Map met alle scripts uitvoeren
  PARAMETERS.py|                       Python-script op alle parameters opgeven
  Readme.MD|                           Dit document Leesmij-bestand


## <a name="data-description"></a>Beschrijving van de gegevens

Deze zelfstudie wordt gebruikt als voorbeeld met een hogere hoofdtekst kleding patroon gegevensset die bestaan uit maximaal 428 installatiekopieën. Elke installatiekopie is voorzien van een van drie verschillende structuren (decimale, striped, leopard). We het aantal images kleine opgeslagen zodat deze zelfstudie kan snel worden uitgevoerd. De code is echter goed geteste en werkt met tienduizenden installatiekopieën of meer. Alle installatiekopieën zijn geslijmd met zoeken naar Bing-afbeelding en hand aangetekend zoals is beschreven in [deel 3](#using-a-custom-dataset). De installatiekopie van URL's met hun respectieve kenmerken worden vermeld in de */resources/fashionTextureUrls.tsv* bestand.

Het script `0_downloadData.py` downloadt alle installatiekopieën naar de *installatiekopieën-DATA_DIR/fashionTexture/* directory. Sommige van de 428 URL's zijn waarschijnlijk verbroken. Dit is geen probleem en houdt in dat we iets minder afbeeldingen hebben voor trainings- en testdoeleinden. Alle scripts die zijn opgegeven in dit voorbeeld moeten lokaal worden uitgevoerd en niet op bijvoorbeeld een docker externe omgeving.

De volgende afbeelding ziet u voorbeelden van de kenmerken (links) scheidingspunten striped (midden) en leopard (rechts). Aantekeningen zijn uitgevoerd volgens het bovenste hoofdtekst kleding item.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/examples_all.jpg"  alt="alt text" width="700">
</p>


## <a name="part-1---model-training-and-evaluation"></a>Deel 1 - Model training en evaluatie

We zijn een systeem dat wordt gebruikt, maar niet wijzigen, een vooraf getraind diep neurale netwerk trainen in het eerste deel van deze zelfstudie. Deze vooraf getraind DNN wordt gebruikt als een featurizer en een lineaire SVM wordt getraind te voorspellen van het kenmerk (scheidingspunten, striped of leopard) van een installatiekopie van het gegeven.

We deze benadering in detail stapsgewijze, en geeft weer welke scripts moeten worden uitgevoerd voor het nu beschreven. Het is raadzaam na elke stap om te controleren welke bestanden zijn geschreven en waar ze naar worden geschreven.

Alle belangrijke parameters zijn opgegeven en een korte uitleg hebt opgegeven, op één plaats: de `PARAMETERS.py` bestand.




### <a name="step-1-data-preparation"></a>Stap 1: Gegevens voorbereiden
`Script: 1_prepareData.py. Notebook: showImages.ipynb`

De notebook `showImages.ipynb` kunnen worden gebruikt voor het visualiseren van de installatiekopieën en corrigeren van de aantekening indien nodig. Voor het uitvoeren van de notebook, opent u het in Azure Machine Learning Workbench, klik op 'Start-Notebook Server' als deze optie wordt weergegeven, wijzigen in de kernel lokaal project met de naam 'PROJECTNAME lokale' (bijvoorbeeld de ' myImgClassUsingCNTK-lokaal'), en vervolgens uitgevoerd alle cellen in de laptop. Zie de sectie Probleemoplossing in dit document als u een fout klagen krijgt dat de laptop is te groot om te worden weergegeven.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showImages.jpg" alt="alt text" width="700"/>
</p>

Nu voert u het script met de naam `1_prepareData.py`, welke wijst alle installatiekopieën op ofwel de training ingesteld of de test. Deze toewijzing is wederzijds exclusief - er is geen afbeelding training wordt ook gebruikt voor testdoeleinden of vice versa. Standaard een willekeurige 75% van de installatiekopieën van elke kenmerkklasse zijn toegewezen aan de training en de overige 25% zijn toegewezen om te testen. Alle gegevens die zijn gegenereerd door het script worden opgeslagen in de *proc-DATA_DIR/fashionTexture/* map.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_1_white.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-2-refining-the-deep-neural-network"></a>Stap 2: Het diep Neurale netwerk verfijnen
`Script: 2_refineDNN.py`

Zoals wordt uitgelegd in deze zelfstudie, deel 1, de vooraf getraind DNN vaste wordt opgeslagen (dat wil zeggen, het is niet verfijnd). Echter, het script met de naam `2_refineDNN.py` nog steeds worden uitgevoerd in deel 1, die wordt geladen vooraf getraind [ResNet](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf) [2] model en wijzigt u dit, bijvoorbeeld, als u wilt toestaan voor hogere invoer afbeeldingsresolutie. Deze stap is snel (seconden) en is niet vereist voor een GPU.

Deel 2 van de zelfstudie een wijziging in de PARAMETERS.py van de bestandsserver oorzaken de `2_refineDNN.py` script om de vooraf getraind DNN ook verfijnen. Standaard we 45 training epoches tijdens verfijning uitgevoerd.

In beide gevallen wordt het laatste model vervolgens naar het bestand geschreven *DATA_DIR/proc/fashionTexture/cntk_fixed.model*.

### <a name="step-3-evaluate-dnn-for-all-images"></a>Stap 3: Evalueer DNN voor alle afbeeldingen
`Script: 3_runDNN.py`

We kunnen de (mogelijk verfijnd) DNN van de laatste stap naar featurize nu onze installatiekopieën gebruiken. Een installatiekopie van een opgegeven als invoer voor de DNN, is de uitvoer de vector 512 tekst uit de voorlaatste laag van het model. Deze vector is veel kleiner dan de installatiekopie zelf dimensionale. Evenwel moet deze bevatten (en zelfs Markeer) alle informatie in de afbeelding die relevant zijn voor het herkennen van de afbeelding kenmerk, dat zich bevindt, als het item kleding een decimale heeft striped, of leopard patroon.

Alle van de DNN installatiekopie representaties zijn opgeslagen in het bestand *DATA_DIR/proc/fashionTexture/cntkFiles/features.pickle*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_4_white.jpg" alt="alt text" width="700"/>
</p>


### <a name="step-4-support-vector-machine-training"></a>Stap 4: Vectormachine ondersteuning training
`Script: 4_trainSVM.py`

De 512-tekst verklaringen berekend in de laatste stap nu worden gebruikt voor het trainen van een classificatie SVM: de SVM gezien een afbeelding als invoer, uitvoer een score voor elk kenmerk aanwezig zijn. In onze gegevensset voorbeeld betekent dit een score voor 'striped', voor 'onderbroken' en 'leopard'.

Script `4_trainSVM.py` laadt de installatiekopieën van de training, een SVM voor verschillende waarden van de parameter regularisatie (slack) C traint en de SVM houdt met de hoogste nauwkeurigheid. De nauwkeurigheid van de classificatie is afgedrukt op de console en in de Workbench uitgezet. Voor de gegevens van het opgegeven patroon moet deze waarden ongeveer 100% en % 88 respectievelijk. Ten slotte het getrainde SVM is geschreven naar het bestand *DATA_DIR/proc/fashionTexture/cntkFiles/svm.np*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/vienna_svm_log_zoom.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-5-evaluation-and-visualization"></a>Stap 5: Evaluatie en visualisatie
`Script: 5_evaluate.py. Notebook: showResults.ipynb`

De nauwkeurigheid van de installatiekopie getraind classificatie kan worden gemeten met behulp van het script `5_evaluate.py`. De script-scores alle test afbeeldingen met behulp van de trained SVM classificatie wordt toegewezen elke installatiekopie van het kenmerk met de hoogste score en vergelijkt de voorspelde kenmerken met de grond waarheid aantekeningen.

De uitvoer van script `5_evaluate.py` worden hieronder weergegeven. De nauwkeurigheid van de classificatie van elke afzonderlijke klasse wordt berekend, evenals de nauwkeurigheid van de volledige testset ('algehele nauwkeurigheid') en het gemiddelde gedurende de afzonderlijke accuratesse ('algemene klasse gemiddeld nauwkeurigheid'). 100% komt overeen met de best mogelijke nauwkeurigheid en 0-% naar de slechtste. Willekeurige raden gemiddeld geeft als resultaat een gemiddelde waarde van klasse nauwkeurigheid van 1 via het aantal kenmerken: in ons geval deze nauwkeurigheid 33,33% zou zijn. Deze resultaten aanzienlijk verbeteren wanneer u een hogere resolutie invoer zoals `rf_inputResoluton = 1000`, echter ten koste van de langere DNN berekening tijden.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_6_white.jpg" alt="alt text" width="700"/>
</p>

Naast de nauwkeurigheid, wordt de ROC-curve getekend met respectieve gebied-onder-curve (links); en de matrix verwarring (rechts) wordt weergegeven:

<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat.jpg" alt="alt text" width="700"/>
</p>

Ten slotte de notebook `showResults.py` is opgegeven voor de installatiekopieën van de test schuift en hun respectieve classificatie scores visualiseren. Zoals wordt beschreven in stap 1, moet elke laptop in dit voorbeeld gebruiken de kernel lokaal project met de naam 'PROJECTNAME lokale':
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showResults.jpg" alt="alt text" width="700"/>
</p>





### <a name="step-6-deployment"></a>Stap 6: implementatie
`Scripts: 6_callWebservice.py, deploymain.py. Notebook: deploy.ipynb`

Het getrainde systeem kan nu worden gepubliceerd als een REST-API. Implementatie wordt uitgelegd in de notebook `deploy.ipynb`, en op basis van de functionaliteit in de Azure Machine Learning-Workbench (Vergeet niet in te stellen als kernel de kernel lokaal project met de naam 'local PROJECTNAME'). Zie ook de Implementatiesectie uitstekende van de [IRIS zelfstudie](https://docs.microsoft.com/azure/machine-learning/preview/tutorial-classifying-iris-part-3) voor de implementatie van meer gerelateerde informatie.

Zodra geïmplementeerd, de webservice kan worden aangeroepen met het script `6_callWebservice.py`. Houd er rekening mee dat het IP-adres (lokaal of in de cloud) van de webservice moet eerst worden ingesteld in het script. De notebook `deploy.ipynb` wordt uitgelegd hoe u deze IP-adres vinden.








## <a name="part-2---accuracy-improvements"></a>Deel 2 - nauwkeurigheid verbeteringen

Deel 1, we hebt u geleerd hoe u een installatiekopie van een door een lineaire Vectormachine ondersteuning op de uitvoer 512-tekst van een Deep Neural Network training. Deze DNN is vooraf getraind op miljoenen installatiekopieën en de voorlaatste laag geretourneerd als de functie vector. Deze methode is snel omdat de DNN wordt gebruikt als-is, maar toch vaak biedt goede resultaten.

We nu aanwezig zijn op verschillende manieren voor het verbeteren van de nauwkeurigheid van het model van deel 1. We verfijnen met name de DNN in plaats van deze vast te houden.

### <a name="dnn-refinement"></a>DNN verfijning

In plaats van een SVM kunt een de classificatie rechtstreeks in het neurale netwerk doen. Dit wordt bereikt door een nieuwe laatste laag toe te voegen aan de vooraf getraind DNN, waarbij de 512-tekst uit de voorlaatste laag als invoer wordt. De classificatie in de DNN te doen is dat het volledige netwerk kunt u retrained met backpropagation. Deze aanpak leidt vaak tot veel betere classificatie accuratesse vergeleken met het gebruik van de vooraf getraind DNN als-is echter ten koste van veel langer trainingstijd (zelfs met een GPU).

Het Neurale netwerk in plaats van een SVM wordt gerealiseerd door het wijzigen van de variabele `classifier` in `PARAMETERS.py` van `svm` naar `dnn`. Zoals wordt beschreven in deel 1, de scripts, met uitzondering van voorbereiden van gegevens (stap 1) voor en training SVM (stap 3) moeten opnieuw worden uitgevoerd. DNN verfijning vereist een GPU. Als er geen GPU is gevonden of als de GPU is vergrendeld (bijvoorbeeld door een vorige CNTK-run) script `2_refineDNN.py` een fout genereert. DNN training kunt-geheugen fout genereert op sommige GPU's die kunnen worden vermeden door minibatch verkleinen (variabele `cntk_mb_size` in `PARAMETERS.py`).

Wanneer de training is voltooid, het verfijnd model is opgeslagen in *DATA_DIR/proc/fashionTexture/cntk_refined.model*, en tekent getekend dat toont hoe de trainings- en classificatiefouten tijdens de training wijzigen. Let op dat tekent die de fout op de training ingesteld is veel kleiner is dan op de testset. Dit zogenaamde te veel passende gedrag kan worden verkleind, bijvoorbeeld met behulp van een hogere waarde voor de frequentie Drop-out `rf_dropoutRate`.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_3_plot.png" alt="alt text" height="300"/>
</p>

Zoals u kunt zien in de onderstaande tekent, is de nauwkeurigheid DNN verfijning voor de opgegeven gegevensset met 92.35% ten opzichte van de 88.92% voordat (deel 1). Installatiekopieën van het 'onderbroken' de met een ROC gebied-onder-curve van 0,98 met verfijning vs in het bijzonder aanzienlijk verbeteren. 0.94 voordat. We gebruiken een kleine gegevensset en daarom de werkelijke accuratesse uitvoering van de programmacode zijn verschillend. Dit verschil is vanwege stochastische effecten zoals willekeurige splitsen in trainings- en testdoeleinden sets afbeeldingen.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat_dnn.jpg" alt="alt text" width="700"/>
</p>

### <a name="run-history-tracking"></a>Bijhouden van de geschiedenis uitvoeren

De Azure Machine Learning-Workbench worden de geschiedenis van elke uitvoeren op Azure opdat vergelijking van twee of meer wordt uitgevoerd die zijn opgeslagen, zelfs weken elkaar. Dit wordt gedetailleerd uitgelegd in de [Iris zelfstudie](https://docs.microsoft.com/azure/machine-learning/preview/tutorial-classifying-iris-part-2). Dit wordt ook weergegeven in de volgende schermafbeeldingen waar we twee wordt uitgevoerd van het script vergelijken `5_evaluate.py`, met behulp van beide verfijning DNN dat wil zeggen, `classifier = "dnn"`(volgnummer 148) of SVM opleiding, dat wil zeggen, `classifier = "svm"` (volgnummer 150).

In de eerste schermafbeelding leidt DNN verfijning tot betere accuratesse dan SVM training voor alle klassen. De tweede schermafbeelding ziet u alle metrische gegevens die worden bijgehouden, inclusief wat de classificatie is. Deze bijhouden wordt uitgevoerd in het script `5_evaluate.py` door het aanroepen van het logboek Azure Machine Learning-Workbench. Het script opslaat bovendien ook de ROC-curve en verwarring matrix aan de *levert* map. Dit *levert* map is speciaal in dat de inhoud ervan ook wordt gevolgd door de geschiedenisfunctie Workbench en daarom de uitvoerbestanden toegankelijk zijn op elk gewenst moment, ongeacht of lokale kopieën zijn overschreven.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison1.jpg" alt="alt text" width="700"/>  
</p>

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison2b.jpg" alt="alt text" width="700"/>
</p>


### <a name="parameter-tuning"></a>Parameter afstemmen
Zoals geldt voor de meeste machine learning-projecten, vereist goede resultaten ophalen voor een nieuwe gegevensset zorgvuldige parameter afstemmen, evenals een evaluatie van verschillende ontwerpbeslissingen. Om te helpen met deze taken, alle belangrijke parameters zijn opgegeven en een korte uitleg hebt opgegeven, op één plaats: de `PARAMETERS.py` bestand.

Enkele van de meestbelovende mogelijkheden voor verbeteringen zijn:

- Gegevenskwaliteit: Zorg ervoor dat de trainings- en testset sets van hoge kwaliteit hebben. Dat wil zeggen, de afbeeldingen zijn correct aantekeningen, niet-eenduidige afbeeldingen verwijderd (bijvoorbeeld kleding items met zowel streepjes en punten), en de kenmerken sluiten elkaar wederzijds uit (dat wil zeggen, zodat elke installatiekopie tot exact één kenmerk behoort gekozen).
- Als het object van belang klein is in de installatiekopie vervolgens installatiekopie classificatie benaderingen niet te werken goed bekend. In dergelijke gevallen kunt u overwegen een benadering van de detectie object zoals beschreven in dit [zelfstudie](https://github.com/Azure/ObjectDetectionUsingCntk).
- DNN verfijning: de weliswaar belangrijkste parameter direct is het leertempo `rf_lrPerMb`. Als de nauwkeurigheid van de training ingesteld (eerste afbeelding in deel 2) is niet dicht bij 0-5%, waarschijnlijk is vanwege een onjuiste het leertempo. De andere parameters te beginnen met `rf_` minder belangrijk zijn. De fout training moet normaal gesproken exponentieel verlagen en worden dicht bij 0% na de training.
- Resolutie invoer: de standaardresolutie installatiekopie is 224 x 224 pixels. Met behulp van hogere afbeeldingsresolutie (parameter: `rf_inputResoluton`), bijvoorbeeld 448 x 448 of 896 x 896 pixels vaak aanzienlijke verbetert de nauwkeurigheid maar vertraagt de DNN verfijnen. **Met behulp van hogere afbeeldingsresolutie is bijna eenvoudig en bevordert de bijna altijd nauwkeurig**.
- Te veel aanpassen DNN: voorkomen dat een grote hiaat tussen de trainings- en test nauwkeurigheid tijdens DNN verfijning (eerste afbeelding in deel 2). Door dit gat kan worden teruggebracht met Drop-out tarieven `rf_dropoutRate` van 0,5 of meer en door te verhogen van het gewicht regularizer `rf_l2RegWeight`. Met behulp van de frequentie waarmee een hoge Drop-out kan vooral nuttig als de invoer afbeeldingsresolutie DNN hoog zijn.
- Probeer diepere DNNs door te wijzigen met `rf_pretrainedModelFilename` van `ResNet_18.model` naar elk `ResNet_34.model` of `ResNet_50.model`. Het model Resnet 50 is niet alleen beter, maar de uitvoer van de voorlaatste laag is van de grootte van 2048 tekst (vs. 512 tekst van de modellen ResNet 18 en ResNet 34). Deze verhoogde dimensie kunt vooral nuttig zijn bij het trainen van een classificatie SVM.

## <a name="part-3---custom-dataset"></a>Deel 3: aangepaste gegevensset

Deel 1 en 2, we getraind en geëvalueerd van een installatiekopie van classificatie-model met behulp van de installatiekopieën van het opgegeven bovenste hoofdtekst kleding structuren. Nu laten we zien hoe u een aangepaste gebruiker gemaakte gegevensset te gebruiken. Of, als niet beschikbaar is, hoe genereren en aantekeningen toevoegen aan deze een gegevensset met Bing installatiekopie zoeken.

### <a name="using-a-custom-dataset"></a>Met behulp van een aangepaste gegevensset

Eerst gaan we kijken de mapstructuur voor de gegevens van de structuur kleding hebben. Opmerking hoe alle afbeeldingen voor de verschillende kenmerken zijn in de bijbehorende submappen *decimale*, * leopard, en *striped* op *installatiekopieën-DATA_DIR/fashionTexture/*. U ziet ook hoe de naam van de installatiekopie-map vindt ook plaats de `PARAMETERS.py` bestand:
```python
datasetName = "fashionTexture"
```

Met behulp van een aangepaste gegevensset is net zo eenvoudig als deze mapstructuur reproduceren waarin alle installatiekopieën zijn in submappen volgens hun kenmerk en deze submappen kopiëren naar een nieuwe gebruiker opgegeven map *installatiekopieën-DATA_DIR/newDataSetName/*. De enige codewijziging vereist is om in te stellen de `datasetName` variabele *newDataSetName*. Scripts 1-5 kunnen vervolgens worden uitgevoerd in de volgorde en alle tussenliggende bestanden worden vastgelegd in *proc-DATA_DIR/newDataSetName/*. Er zijn geen andere codewijzigingen vereist.

Het is belangrijk dat elke installatiekopie kan worden toegewezen aan exact één kenmerk. Het normaal zou zijn bijvoorbeeld verkeerde kenmerken voor "dieren" en 'leopard' hebben omdat de installatiekopie van een 'leopard' zouden ook "dieren" behoren. Bovendien is het beste verwijderen van niet-eenduidige en daarom moeilijk aantekeningen toevoegen aan installatiekopieën.



### <a name="image-scraping-and-annotation"></a>Afbeelding afschrapen en aantekening

Verzamelen van een voldoende aantal aantekeningen afbeeldingen voor trainings- en testdoeleinden kan lastig zijn. Er is een manier om dit probleem oplossen bij installatiekopieën van het Internet. Zie bijvoorbeeld onder de zoekresultaten van Bing afbeelding voor de query *t-shirt striped*. Zoals verwacht, installatiekopieën van de meeste inderdaad striped t-shirts. Het paar onjuist of is niet eenduidig installatiekopieën (zoals kolom 1, 1; rij of kolom 3, rij 2) kunnen worden geïdentificeerd en eenvoudig verwijderd:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/bing_search_striped.jpg" alt="alt text" width="600"/>
</p>

Voor het genereren van een grote en diverse gegevensset moeten meerdere query's worden gebruikt. Bijvoorbeeld: 7\*3 = 21 query's kunnen worden gemaakt met automatisch alle combinaties van kleding items {bloes, hoodie, aangebreide, trui, shirt, t-shirt, uitoefenbaarheid} en {striped, decimale, leopard} kenmerken. Vervolgens de installatiekopieën van het bovenste 50 per query downloaden zou leiden tot een maximum van 21 * 50 = 1050 installatiekopieën.

In plaats van handmatig afbeeldingen worden gedownload van het zoeken naar Bing-afbeelding, is het veel eenvoudiger te gebruiken in plaats daarvan de [cognitieve Services Bing installatiekopie Search API](https://www.microsoft.com/cognitive-services/bing-image-search-api) die retourneert een set met een queryreeks opgegeven afbeelding-URL's.

Enkele van de gedownloade afbeeldingen exacte of in de buurt van duplicaten zijn (bijvoorbeeld door de installatiekopie van oplossing of jpg-artefacten verschillen). Deze dubbele vermeldingen moeten worden verwijderd, zodat de splitsing trainings- en bevatten niet dezelfde afbeeldingen. Verwijderen van dubbele afbeeldingen kan worden bereikt met een hash-gebaseerde benadering die in twee stappen werkt: (i) eerst de hash-tekenreeks is berekend voor alle installatiekopieën. (ii) in een tweede keer via de afbeeldingen, zijn alleen afbeeldingen met een hash-tekenreeks die nog niet zichtbaar bewaard. Alle andere afbeeldingen worden genegeerd. Zagen we de `dhash` benadering in de bibliotheek Python `imagehash` en wordt beschreven in dit [blog](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html) om uit te voeren, met de parameter `hash_size` is ingesteld op 16. Het is OK onjuist enkele afbeeldingen niet dubbele verwijderen zolang het merendeel van de echte duplicaten verwijderd.





## <a name="conclusion"></a>Conclusie

Er zijn een aantal belangrijke licht van dit voorbeeld:
- De code te trainen, evalueren en afbeelding classificatiemodellen kunnen implementeren.
- Demo voor installatiekopieën die zijn opgegeven, maar gemakkelijk kan worden aangepast (één regel wijzigen) gegevensset eigen installatiekopie gebruiken.
- De nieuwste deskundige functies geïmplementeerd om het trainen van modellen van hoge nauwkeurigheid op basis van Learning overdragen.
- Interactieve model ontwikkelen met Azure Machine Learning Workbench en Jupyter-Notebook.


## <a name="references"></a>Verwijzingen

[1] Alex Krizhevsky Ilya Sutskever en Geoffrey E. Hinton, [ _ImageNet classificatie met diep Convolutional Neural Networks_](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf). NIPS 2012.  
[2] Kaiming hij, Xiangyu Zhang Shaoqing Ren en Jian Sun, [ _diep achtergebleven voor opname van de installatiekopie leren_](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf). CVPR 2016.
