---
title: Betere nauwkeurigheid van de computer vision & classificatie modellen in Azure Machine Learning
description: Informatie over het verbeteren van de nauwkeurigheid van uw computer vision installatiekopie classificatie, object detectie en modellen van installatiekopie vergelijkbaar zijn met het Azure Machine Learning-pakket voor Computer Vision.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: e134e1e544c51d6756d5021fef8c049fe7d8afb0
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="improve-the-accuracy-of-computer-vision-models"></a>De nauwkeurigheid van de computer vision modellen verbeteren

Met de **Azure Machine Learning-pakket voor Computer Vision**, u kunt bouwen en implementeren van de indeling van de installatiekopie, object-detectie en image gelijkenis modellen. Meer informatie over dit pakket en hoe u deze installeert.

In dit artikel leert u hoe af te stemmen deze modellen voor de nauwkeurigheid vergroten. 

## <a name="accuracy-of-image-classification-models"></a>Nauwkeurigheid van de installatiekopie classificatie modellen

De Computer Vision-pakket worden aan goede resultaten opleveren voor een groot aantal verschillende gegevenssets weergegeven. Echter, zoals is ingesteld op true voor de meeste machine learning-projecten, het ophalen van de best mogelijke resultaten voor een nieuwe gegevensset vereist zorgvuldige parameter afstemmen, evenals het evalueren van verschillende ontwerpbeslissingen. De volgende secties bevatten richtlijnen voor het verbeteren van de nauwkeurigheid van een bepaalde gegevensset, dat wil zeggen, welke parameters zijn meestbelovende optimaliseren eerst welke waarden voor deze parameters een moet proberen, en welke valkuilen om te voorkomen.

Normaal gesproken training grondige Learning-modellen wordt geleverd met een compromis tussen trainingstijd versus nauwkeurigheid model. De Computer Vision-pakket heeft de vooraf ingestelde standaardparameters (Zie de eerste rij in de onderstaande tabel) die zich op snelle training snelheid richten tijdens doorgaans het opstellen van hoge nauwkeurigheid modellen. Deze nauwkeurigheid kan vaak worden verbeterd verder met, bijvoorbeeld hogere resolutie of dieper modellen echter koste trainingstijd met een factor van 10 x of meer verhogen.

Het is raadzaam dat u eerst met de standaardparameters werken, het model trainen, de resultaten controleren, compleet waarheid aantekeningen Corrigeer zo nodig en alleen probeer parameters die trainingstijd vertragen (Zie tabel hieronder voorgestelde parameterwaarden). Een goed begrip van deze parameters terwijl technisch niet nodig is, wordt echter aanbevolen.


### <a name="best-practices-and-tips"></a>Aanbevolen procedures en tips

* Gegevenskwaliteit: de trainings- en testset sets van hoge kwaliteit moeten zijn. Dat wil zeggen, de afbeeldingen bevat aantekeningen correct dubbelzinnige installatiekopieën verwijderd (bijvoorbeeld waar het is onduidelijk in een menselijke oog als afbeelding ziet u een volledige tennis of een citroensap) en de kenmerken sluiten elkaar wederzijds uit (dat wil zeggen, elke installatiekopie behoort tot exact één kenmerk).

* Voordat u de DNN verfijnen, moet een classificatie SVM worden getraind met een vooraf getraind en vaste DNN als featurizer. Dit wordt ondersteund in Computer Vision-pakket en vereist geen lang te trainen, omdat de DNN zelf niet is gewijzigd. Zelfs deze eenvoudige benadering vaak bereikt goede accuratesse en daarom vertegenwoordigt een sterke basislijn. De volgende stap is om de DNN die betere nauwkeurigheid geeft te verfijnen.

* Als het object van belang klein in de afbeelding is, staat de installatiekopie classificatie benaderingen niet goed bekend. In dergelijke gevallen kunt u overwegen een benadering van de detectie object zoals Computer Vision pakket sneller R-CNN op basis van Tensorflow.

* De meer trainingsgegevens hoe beter. Als een regel-van-miniatuur, moet een hebben ten minste 100 voorbeelden voor elke objectklasse, dat wil zeggen, 100 afbeeldingen voor 'aquaduct', 100 afbeeldingen voor 'cat', enz. Een model met minder installatiekopieën training is mogelijk, maar mogelijk niet goed resultaat opleveren.

* De afbeeldingen training moeten zich lokaal op de machine met de GPU en zich op een SSD-station (niet een harde schijf). Latentie van installatiekopie lezen inkorten anders drastisch training snelheid (met zelfs een factor van 100 x).


### <a name="parameters-to-optimize"></a>Parameters optimaliseren

Optimale waarden vinden voor deze parameters is van belang en kunt nauwkeurigheid vaak aanzienlijk verbeteren:
* Learning snelheid (`lr_per_mb`): de parameter weliswaar belangrijkste direct. Als de nauwkeurigheid van de training nadat DNN verfijning ~ 5%, of hoger is ingesteld, is waarschijnlijk het leertempo te hoog is of het aantal training epoches te laag. Met name met kleine gegevenssets doorgaans de DNN voorrang op de trainingsgegevens passen, maar in de praktijk die op de testmachine goede modellen zal hierdoor ingesteld. We maken doorgaans gebruik van 15 epoches wanneer initiële learning wordt verlaagd tweemaal te gebruiken. training met meer epoches kan in sommige gevallen prestaties verbeteren.

* Invoer-resolutie (`image_dims`): de standaardresolutie installatiekopie is 224 x 224 pixels. Met behulp van hogere afbeeldingsresolutie van bijvoorbeeld 500 x 500 pixels of 1000 x 1000 pixels nauwkeurigheid aanzienlijk verbeterd, maar vertraagt de DNN verfijning. De Computer Vision-pakket verwacht invoer naamomzetting van een tuple van (kleur-kanalen, breedte van afbeelding, hoogte van de afbeelding), bijvoorbeeld (3, 224, 224) waarin het aantal kanalen moet worden ingesteld op 3 (de RGB stroken) heeft.

* Architectuur model (`base_model_name`): diepere DNNs zoals ResNet 34 of ResNet 50 gebruik in plaats van het standaard ResNet-18-model. Het model Resnet 50 is niet alleen beter, maar de uitvoer van de voorlaatste laag is van de grootte van 2048 tekst (vs. 512 tekst van de modellen ResNet 18 en ResNet 34). Deze verhoogde dimensionaliteit is vooral nuttig wanneer de DNN vast en training in plaats daarvan een classificatie SVM houden.

* De grootte van de minibatch (`mb_size`): hoge minibatch grootten leidt tot sneller training echter ten koste van een hogere DNN geheugenverbruik. Daarom bij het selecteren van diepere modellen (bijvoorbeeld ResNet-50 versus ResNet 18) en/of een hogere resolutie (500\*500 pixels versus 224\*224 pixels), een heeft doorgaans minibatch verkleinen-geheugen fouten te voorkomen. Bij het wijzigen van de grootte van de minibatch moet vaak het leertempo ook worden aangepast, zoals te zien is in de onderstaande tabel.
* Drop-out snelheid (`dropout_rate`) en L2 regularizer (`l2_reg_weight`): DNN te veel aanpassing van labels kan worden teruggebracht met behulp van de frequentie waarmee een Drop-out van 0,5 (de standaardwaarde is 0,5 in Computer Vision-pakket) of meer en door te verhogen van het gewicht regularizer (de standaardwaarde is 0.0005 in visie op Computer Pakket). Opmerking: hoewel dat bijzonder met kleine gegevenssets DNN te veel aanpassing van labels is moeilijk en vaak niet mogelijk om te voorkomen.


### <a name="parameter-definitions"></a>Parameterdefinities

- **Frequentie Learning**: grootte die wordt gebruikt tijdens kleurovergang daalgradiënt learning stap. Als te laag ingesteld en vervolgens het model veel epoches te trainen, duurt als set te hoog en vervolgens het model wordt alleen geconvergeerd naar een goede oplossing. Een planning wordt doorgaans gebruikt wanneer learning wordt verlaagd na een bepaald aantal epoches. E.For voorbeeld de learning beoordelen planning `[0.05]*7 + [0.005]*7 + [0.0005]` komt overeen met het gebruik van een initiële leertempo van 0,05 voor de eerste zeven epoches, gevolgd door een 10 x verminderde leertempo van 0,005 voor een andere zeven epoches en ten slotte het model aan te passen voor een enkele epoche met een 100 x verminderd leertempo van 0.0005.

- **De grootte van de minibatch**: GPU's kunnen meerdere installatiekopieën om te versnellen berekening parallel worden verwerkt. Deze parallelle verwerkte afbeeldingen worden ook wel aangeduid als een minibatch. Hoe hoger de grootte van de minibatch de snellere training dient echter ten koste van een hogere DNN geheugengebruik.

### <a name="recommended-parameter-values"></a>Aanbevolen parameterwaarden

De volgende tabel bevat verschillende parametersets die werden weergegeven voor het produceren van hoge nauwkeurigheid modellen in een groot aantal taken voor bestandsclassificatie van installatiekopie. De optimale parameters zijn afhankelijk van de specifieke gegevensset en de exacte GPU gebruikt, dus de tabel die moet worden weergegeven als richtlijn alleen. Nadat u hebt geprobeerd deze parameters, houd ook rekening met oplossingen van meer dan 500 x 500 pixels of dieper modellen zoals Resnet 101 of Resnet 152 installatiekopie.

De eerste rij in de tabel overeen met de standaardparameters die zijn ingesteld in Computer Vision-pakket. De overige rijen langer duren om te trainen echter (aangegeven in de eerste kolom) op het voordeel van grotere nauwkeurigheid (Zie de tweede kolom voor de gemiddelde nauwkeurigheid gedurende drie interne gegevenssets). Bijvoorbeeld, nemen de parameters in de laatste rij 5 tot 15 x langer te trainen, uitgewezen echter nauwkeuriger (gemiddelde) op drie interne testsets van 82.6% 92.8%.

Diepere modellen en hogere invoer resolutie nemen meer DNN geheugen en daarom moet de grootte van de minibatch worden teruggebracht met verbeterde model complexiteit om out van geheugen fouten te voorkomen. Zoals u kunt zien in de onderstaande tabel, is het nuttig om het verlagen van het leertempo met een factor van twee wanneer de minibatch verlagen formaat van de dezelfde vermenigvuldiger. Mogelijk moet de grootte van de minibatch ophalen verminderd verder GPU's met een kleinere hoeveelheid geheugen.

| Trainingstijd (ruwe schatting) | Voorbeeld nauwkeurigheid | De grootte van de minibatch (*mb_size*) | Learning snelheid (*lr_per_mb*) | Afbeelding resolutie (*image_dims*) | DNN-architectuur (*base_model_name*) |
|------------- |:-------------:|:-------------:|:-----:|:-----:|:---:|
| 1 x (verwijzing) | 82.6% | 32 | [0,05] \*7 + [0,005]\*7 + [0.0005]  | (3, 224, 224) | ResNet18_ImageNet_CNTK |
| 2-5 x    | 90.2% | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 500, 500) | ResNet18_ImageNet_CNTK |
| 2-5 x    | 87,5% | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 224, 224) | ResNet50_ImageNet_CNTK |
| 5 tot 15 x        | 92.8% |  8 | [0,01] \*7 + [0,001]\*7 + [0,0001]  | (3, 500, 500) | ResNet50_ImageNet_CNTK |


## <a name="next-steps"></a>Volgende stappen

Voor informatie over het Azure Machine Learning-pakket voor Computer visie:
+ Bekijk de naslagdocumentatie

+ Meer informatie over [andere Python-pakketten voor Azure Machine Learning](reference-python-package-overview.md)