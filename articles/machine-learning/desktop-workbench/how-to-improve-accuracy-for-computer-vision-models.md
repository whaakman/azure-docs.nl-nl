---
title: Betere nauwkeurigheid van de computer vision en -classificatie modellen in Azure Machine Learning
description: Leer hoe u de nauwkeurigheid van uw afbeeldingsclassificatie voor computer vision, objectdetectie van het en installatiekopie gelijkenis modellen met het Azure Machine Learning-pakket voor Computer Vision verbeteren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: 6d6c540cd8711ae89784cdc797ca56d312522a83
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46989308"
---
# <a name="improve-the-accuracy-of-computer-vision-models"></a>De nauwkeurigheid van de computer vision-modellen

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Met de **Azure Machine Learning-pakket voor Computer Vision**, u kunt bouwen en implementeren van de classificatie van afbeeldingen, object-detectie en afbeelding gelijkenis modellen. Meer informatie over dit pakket en hoe ze deze willen installeren.

In dit artikel leert u hoe u voor het afstemmen van deze modellen om de nauwkeurigheid vergroten. 

## <a name="accuracy-of-image-classification-models"></a>Nauwkeurigheid van modellen voor classificatie van afbeeldingen

De Computer Vision-pakket wordt weergegeven aan het goede resultaten opleveren voor een groot aantal gegevenssets. Als is ingesteld op true voor de meeste machine learning-projecten, het ophalen van de best mogelijke resultaten voor een nieuwe gegevensset vereist echter zorgvuldig parameter afstemmen, evenals het evalueren van verschillende ontwerpbeslissingen. De volgende secties vindt u richtlijnen over het verbeteren van de nauwkeurigheid van de gegevens op een bepaalde gegevensset, dat wil zeggen, welke parameters zijn meest veelbelovende optimaliseren eerst welke waarden voor deze parameters een moet proberen, en welke valkuilen om te voorkomen.

In het algemeen, Deep Learning-modellen voor training is voorzien van een balans tussen trainingstijd ten opzichte van de nauwkeurigheid van model. De Computer Vision-pakket heeft de vooraf ingestelde standaardparameters (Zie de eerste rij in de onderstaande tabel) die zich op snelle training snelheid richten tijdens normaal gesproken produceren hoge nauwkeurigheid modellen. Deze nauwkeurigheid kan vaak worden verbeterd verder met, bijvoorbeeld hogere resolutie of diepere modellen, echter ten koste van trainingstijd verhogen met een factor van 10 x of meer.

Het is raadzaam dat u eerst met de standaardparameters werken, een model te trainen, de resultaten controleren, begin waarheid aantekeningen Corrigeer indien nodig, en alleen probeer parameters die trainingstijd vertragen (Zie tabel hieronder voorgestelde parameterwaarden). Een goed begrip van deze parameters terwijl technisch niet die nodig zijn, wordt echter aanbevolen.


### <a name="best-practices-and-tips"></a>Aanbevolen procedures en tips

* Kwaliteit van de gegevens: de trainings- en testset sets moeten van hoge kwaliteit. Dat wil zeggen, de afbeeldingen van aantekeningen worden voorzien correct, niet-eenduidige installatiekopieën verwijderd (bijvoorbeeld waar het is onduidelijk aan een eerste oog als de afbeelding ziet u een bal tennis of een citroensap) en de kenmerken zijn sluiten elkaar wederzijds uit (dat wil zeggen, elke installatiekopie behoort tot exact één kenmerk).

* Voordat u de DNN verfijnen, moet een classificatie SVM worden getraind met een vooraf getrainde en vaste DNN als featurizer. Dit wordt ondersteund in de Computer Vision-pakket en vereist niet lang te trainen, omdat de DNN zelf wordt niet gewijzigd. Ook deze eenvoudige benadering vaak bereikt goede nauwkeurigheden en kan daarom vertegenwoordigt een sterke basislijn. De volgende stap is om de DNN die betere nauwkeurigheid geeft te verfijnen.

* Als het object van belang klein in de afbeelding is, klikt u vervolgens installatiekopie classificatie benaderingen niet goed bekend. In dergelijke gevallen kunt u overwegen een benadering voor het detecteren van object zoals Computer Vision-pakket sneller R-CNN op basis van Tensorflow.

* De meer trainingsgegevens hoe beter. Als een regel-van-miniatuur moet een ten minste 100 voorbeelden voor elke objectklasse, dat wil zeggen, 100 afbeeldingen voor "hond", 100 afbeeldingen voor 'cat', enzovoort. Een model met minder installatiekopieën training is mogelijk, maar kan geen goede resultaten opleveren.

* De trainingsafbeeldingen moeten zich lokaal op de machine met de GPU en zich op een SSD-station (niet een harde schijf). Latentie van installatiekopie lezen kunt anders hebt u aanzienlijk minder training snelheid (met zelfs een factor 100 x).


### <a name="parameters-to-optimize"></a>Parameters optimaliseren

Vinden van optimale waarden voor deze parameters is belangrijk en nauwkeurigheid vaak aanzienlijk kunt verbeteren:
* Learning-tarief (`lr_per_mb`): de parameter weliswaar belangrijkste meteen. Als de nauwkeurigheid van de training ingesteld nadat DNN verfijning hoger is dan ongeveer 5%, is waarschijnlijk het leertempo te hoog is, of het aantal training epoches te laag. Met name met kleine gegevenssets doorgaans de DNN te veel op de trainingsgegevens passen, maar in de praktijk die dit tot goede modellen op de testmachine leiden zal ingesteld. We gebruiken doorgaans 15 epoches waarbij de eerste leertempo tweemaal wordt verlaagd training over het gebruik van meer epoches kan in sommige gevallen prestaties verbeteren.

* Voer resolutie (`image_dims`): de standaardresolutie van de installatiekopie is 224 x 224 pixels. Met behulp van hogere resolutie van bijvoorbeeld 500 x 500 pixels of 1000 x 1000 pixels kunt nauwkeurigheid aanzienlijk verbeteren, maar DNN verfijning vertraagt. De Computer Vision-pakket wordt verwacht dat de invoerresolutie moeten een tuple van (kleur-kanalen, breedte van afbeelding en hoogte van de afbeelding), bijvoorbeeld (3, 224, 224), waarbij het aantal kanalen heeft moet worden ingesteld op 3 (de stroken rood-groen-blauw).

* Model van de architectuur (`base_model_name`): diepere dnn's, zoals ResNet 34 of ResNet 50 gebruik in plaats van het standaard ResNet-18-model. Het model Resnet 50 is niet alleen beter, maar de uitvoer van de voorlaatste laag is van de grootte van 2048 tekst (in plaats van 512 tekst van de modellen ResNet robmazz van 18 en ResNet 34). Deze verbeterde dimensionaliteit kan zijn met name nuttig wanneer de DNN vast en training in plaats daarvan een classificatie SVM te houden.

* Minibatch grootte (`mb_size`): hoge minibatch grootten zal leiden tot snellere training echter ten koste van een verbeterde DNN geheugenverbruik. Daarom kan bij het selecteren van diepere modellen (bijvoorbeeld ResNet 50 versus ResNet robmazz van 18) en/of hogere resolutie (500\*500 pixels versus 224\*224 pixels), een doorgaans minibatch verkleinen om te voorkomen dat fouten-geheugen heeft. Bij het wijzigen van de grootte van de minibatch, moet vaak het leertempo ook worden aangepast, zoals te zien is in de onderstaande tabel.
* Drop-out tarief (`dropout_rate`) en L2-regularizer (`l2_reg_weight`): DNN sprake van redundante aanpassing van labels kan worden teruggebracht met een tarief Drop-out van 0,5 (de standaardwaarde is 0,5 in Computer Vision-pakket) of meer, en door te verhogen van het gewicht regularizer (de standaardinstelling is 0.0005 in Computer Vision Pakket). Houd er echter rekening mee dat u dat met name met kleine gegevenssets DNN sprake van redundante aanpassing van labels is moeilijk en vaak niet mogelijk om te voorkomen.


### <a name="parameter-definitions"></a>Parameterdefinities

- **Learning-tarief**: stap tijdens leren met stochastische gradiëntdaling gebruikte grootte. Als set te laag en het model veel epoches te trainen, onderneemt wordt als set te hoog en vervolgens het model wordt niet naar een goede oplossing worden geconvergeerd. Een planning wordt meestal gebruikt wanneer het leertempo na een bepaald aantal epoches wordt verminderd. E.For voorbeeld de learning beoordelen planning `[0.05]*7 + [0.005]*7 + [0.0005]` komt overeen met het gebruik van een eerste leertempo van 0,05 voor de eerste zeven epoches, gevolgd door een 10 keer minder leertempo van 0,005 voor een andere zeven epoches en ten slotte het model voor een enkele epoche aan te passen met een 100 x verminderd leertempo van 0.0005.

- **Grootte van minibatch**: GPU's kunnen u meerdere installatiekopieën versnellen berekening parallel verwerken. Deze parallelle verwerkte installatiekopieën worden ook wel aangeduid als een minibatch. Hoe hoger de grootte van de minibatch de snellere training dient echter ten koste van een verbeterde DNN geheugenverbruik.

### <a name="recommended-parameter-values"></a>Aanbevolen parameterwaarden

De onderstaande tabel bevat verschillende parametersets die is getoond voor het produceren van hoge nauwkeurigheid modellen op een groot aantal taken voor bestandsclassificatie installatiekopie. De optimale parameters zijn afhankelijk van de specifieke gegevensset en de exacte GPU die wordt gebruikt, dus de tabel moet worden gezien als een richtlijn alleen. Nadat u hebt geprobeerd deze parameters, kunt u ook de installatiekopie van oplossingen met meer dan 500 x 500 pixels of diepere modellen zoals Resnet 101 of Resnet 152.

De eerste rij in de tabel komt overeen met de standaardparameters die zijn ingesteld in Computer Vision-pakket. Alle andere rijen trainen (aangegeven in de eerste kolom) echter langer duren bij het voordeel van een grotere nauwkeurigheid (Zie de tweede kolom voor de gemiddelde nauwkeurigheid in drie interne gegevenssets). Bijvoorbeeld, nemen de parameters in de laatste rij 5 tot 15 x langer te trainen, maar heeft geresulteerd in grotere (gemiddelde) nauwkeurigheid op drie interne testsets van 82.6% 92.8%.

Diepere modellen en hogere invoerresolutie nemen meer DNN geheugen en kan daarom de minibatch grootte moet worden teruggebracht met verbeterde model complexiteit om out van geheugen fouten te voorkomen. Zoals te zien in de onderstaande tabel, is het nuttig is om het leertempo verlagen met een factor van twee wanneer de minibatch verlagen formaat van de dezelfde vermenigvuldiger. De grootte van de minibatch mogelijk ophalen teruggebracht naar een meer op GPU's met een kleinere hoeveelheid geheugen.

| Trainingstijd (ruwe schatting) | Voorbeeld van de nauwkeurigheid | Minibatch grootte (*mb_size*) | Learning-tarief (*lr_per_mb*) | Afbeelding van het probleem zou moeten (*image_dims*) | Architectuur van DNN (*base_model_name*) |
|------------- |:-------------:|:-------------:|:-----:|:-----:|:---:|
| 1 x (verwijzing) | 82.6% | 32 | [0,05] \*7 + [0,005]\*7 + [0.0005]  | (3, 224, 224) | ResNet18_ImageNet_CNTK |
| 2-5 x    | 90.2% | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 500, 500) | ResNet18_ImageNet_CNTK |
| 2-5 x    | 87,5% | 16 | [0,025] \*7 + [0.0025]\*7 + [0.00025] | (3, 224, 224) | ResNet50_ImageNet_CNTK |
| 5 tot 15 x        | 92.8% |  8 | [0,01] \*7 + [0,001]\*7 + [0,0001]  | (3, 500, 500) | ResNet50_ImageNet_CNTK |


## <a name="next-steps"></a>Volgende stappen

Voor informatie over het Azure Machine Learning-pakket voor Computer Vision:
+ Bekijk de naslagdocumentatie

+ Meer informatie over [andere Python-pakketten voor Azure Machine Learning](reference-python-package-overview.md)