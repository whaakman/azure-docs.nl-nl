---
title: Bouw en implementeer een installatiekopie gelijkenis model met behulp van Azure Machine Learning-pakket voor Computer Vision.
description: Informatie over het bouwen, trainen, testen en implementeren van een computer vision-installatiekopie gelijkenis model met behulp van de Azure Machine Learning-pakket voor Computer Vision.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 05/20/2018
ROBOTS: NOINDEX
ms.openlocfilehash: b1b4e3e19adb38a69e16aaa98300972df3bb71a8
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093646"
---
# <a name="build-and-deploy-image-similarity-models-with-azure-machine-learning"></a>Afbeelding gelijkenis modellen met Azure Machine Learning bouwen en implementeren

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


In dit artikel leert u hoe u Azure Machine Learning pakket voor Computer Vision (AMLPCV) gebruiken om te trainen, testen en implementeren van een installatiekopie gelijkenis-model. Zie voor een overzicht van dit pakket en de gedetailleerde technische documentatie, https://aka.ms/aml-packages/vision.

Een groot aantal problemen in het domein van de computer vision, kan worden opgelost met behulp van de installatiekopie gelijkenis. Deze problemen zijn onder andere het ontwikkelen van modellen die antwoorden op vragen zoals:
+ _Een OBJECT aanwezig zijn in de afbeelding is? Bijvoorbeeld, "hond", "auto", "verzenden", enzovoort_
+ _Welke klasse van ogen ziekten ernst is evinced door retinal scan van de patiënt?_

Als het bouwen en implementeren van dit model met AMLPCV, gaat u door de volgende stappen:
1. Gegevensset maken
2. Afbeelding visualisatie en aantekening
3. Afbeelding uitdiepen
4. Definitie van deep Neural Network DNN ()-model
5. Classificatie training
6. Evaluatie en visualisatie
7. Implementatie van de webservice
8. Web service belastingstesten

[CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) wordt gebruikt als de deep learning-frameworks, training wordt lokaal uitgevoerd op een GPU gemaakte virtuele machine, zoals de ([Deep learning, Data Science VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)), en implementatie maakt gebruik van de CLI van Azure ML uitoefening.

## <a name="prerequisites"></a>Vereisten

1. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

1. De volgende accounts en de toepassing moeten worden ingesteld en worden geïnstalleerd:
   - Een Azure Machine Learning Experimenten-account 
   - Een Azure Machine Learning Modelbeheer-account
   - Azure Machine Learning Workbench moet zijn geïnstalleerd

   Als deze drie nog niet zijn gemaakt of is geïnstalleerd, volgt u de [installatie van Azure Machine Learning-Quickstart en Workbench](../desktop-workbench/quickstart-installation.md) artikel. 

1. De Azure Machine Learning-pakketten voor Computer Vision moet worden geïnstalleerd. Informatie over het installeren van dit pakket, zoals beschreven op https://aka.ms/aml-packages/vision.

## <a name="sample-data-and-notebook"></a>Voorbeeldgegevens en -laptops

### <a name="get-the-jupyter-notebook"></a>De Jupyter-notebook ophalen

Download het notitieblok om uit te voeren van het voorbeeld beschreven hier zelf.

> [!div class="nextstepaction"]
> [De Jupyter-notebook ophalen](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>De voorbeeldgegevens laden

Het volgende voorbeeld wordt een gegevensset die bestaat uit een 63 borden installatiekopieën. Elke afbeelding wordt aangeduid als behorend tot een van de vier verschillende klassen (bowl, cup, bestek, plaat). Het aantal afbeeldingen in dit voorbeeld is klein dat dit voorbeeld kan snel worden uitgevoerd. In de praktijk moeten ten minste 100 afbeeldingen per klasse worden opgegeven. Alle installatiekopieën bevinden zich op *".. /sample_data/imgs_recycling / "*, in de submappen met de naam"bowl","cup", 'bestek' en 'element'.

![Azure Machine Learning-gegevensset](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure Machine Learning-pakket voor Computer Vision in deze artikelen:

+ Meer informatie over het [verbeteren van de nauwkeurigheid van dit model](how-to-improve-accuracy-for-computer-vision-models.md).

+ Lees de [pakket overzicht](https://aka.ms/aml-packages/vision).

+ Verken de [referentiedocumentatie](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) voor dit pakket.

+ Meer informatie over [andere Python-pakketten voor Azure Machine Learning](reference-python-package-overview.md).
