---
title: Afbeelding van classificatie met het Azure Machine Learning-pakket voor computer vision en Team gegevens wetenschap proces (TDSP) | Microsoft Docs
description: Beschrijft het gebruik van het Team gegevens wetenschap proces (TDSP) en het Azure Machine Learning-pakket voor computer vision voor classificatie van de installatiekopie.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: xibingao
ms.openlocfilehash: f9e88cfb7185845e96f287b39bebaaa24320f537
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300787"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Weergavekader kanker installatiekopie classificatie met het Azure Machine Learning-pakket voor computer vision en Team gegevens wetenschappelijke processen

Dit artikel laat zien hoe u de [Azure Machine Learning-pakket voor computer vision](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) om te trainen, testen en implementeren van een *installatiekopie classificatie* model. Dit voorbeeld worden de structuur Team gegevens wetenschap proces (TDSP) en sjablonen in [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). In dit scenario biedt het complete voorbeeld. Gebruikt de [Microsoft cognitieve Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) als de deep framework leren en training wordt uitgevoerd op een [Gegevenswetenschap virtuele machine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) GPU-machine. Implementatie gebruikmaakt van de Azure Machine Learning uitoefening CLI.

De frame van veel toepassingen in het domein van de visie computer kunnen worden geplaatst als afbeelding classificatie problemen. Deze omvatten ontwikkelen van modellen dat eenvoudige vragen beantwoorden als 'Is een aanwezig is in de installatiekopie van het object?' waar het object is mogelijk een aquaduct, auto of verzenden. Dit omvat ook antwoorden op complexere vragen zoals 'welke klasse ogen ziekte ernst evinced door deze geduld retinal scan?' Het Azure Machine Learning-pakket voor computer vision stroomlijnt installatiekopie classificatie gegevensverwerking en de pijplijn modelleren. 

## <a name="link-to-the-github-repository"></a>Koppeling naar de GitHub-opslagplaats
Dit artikel bevat een samenvatting document over het voorbeeld. U vindt meer uitgebreide documentatie op de [GitHub site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Team gegevens wetenschap proces scenario

In dit scenario maakt gebruik van de [Team gegevens wetenschap proces](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) levenscyclus. Het scenario omvat de volgende stappen in de levenscyclus.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Overname van gegevens](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
De internationale Skin Imaging samenwerking ISIC ()-gegevensset wordt gebruikt voor de installatiekopie van classificatie-taak. ISIC is een samenwerking tussen academische wereld en bedrijfstak van de toepassing van digitale skin imaging te bestuderen te reduceren melanoma sterfte te vergemakkelijken. De [ISIC archief](https://isic-archive.com/#images) meer dan 13.000 skin beschadiging installatiekopieën die zijn gelabeld als ze goedaardig of kwaadaardig bevat. Een voorbeeld van de installatiekopieën uit het archief van ISIC downloaden.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modeling](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
In de stap modellering, worden de volgende substappen uitgevoerd.

#### <a name="dataset-creation"></a>Het maken van de gegevensset

Een dataset-object in het Azure Machine Learning-pakket voor computer vision genereren door op te geven van een hoofdmap van installatiekopieën op de lokale schijf. 

#### <a name="image-visualization-and-annotation"></a>Visualisatie van de installatiekopie en aantekening

De afbeeldingen in het object dataset visualiseren en de labels Corrigeer zo nodig.

#### <a name="image-augmentation"></a>Afbeelding uitdiepen

Uitbreiden van een dataset-object met behulp van de transformaties die worden beschreven in de [imgaug](https://github.com/aleju/imgaug) bibliotheek.

#### <a name="dnn-model-definition"></a>DNN modeldefinitie

De model-architectuur die wordt gebruikt in de stap training definiëren. Zes vooraf getraind diep neural network-modellen worden ondersteund in het Azure Machine Learning-pakket voor computer vision: AlexNet, Resnet 18 Resnet 34, Resnet 50, Resnet 101 en Resnet 152.

#### <a name="classifier-training"></a>Training van classificatie

Training van de neural netwerken met standaard of aangepaste parameters.

#### <a name="evaluation-and-visualization"></a>Evaluatie en visualisatie

Functionaliteit voor het evalueren van de prestaties van het getrainde model op een onafhankelijke testgegevensset bieden. De evaluatie van metrische gegevens behoren nauwkeurigheid, precisie, intrekken en ROC-curve.

De substappen worden gedetailleerd beschreven in de bijbehorende Jupyter-Notebook. De notebook bevat ook richtlijnen voor het inschakelen van de parameters, zoals de leertempo, mini batchgrootte en Drop-out snelheid de prestaties van het model verder te verbeteren.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Implementatie](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Deze stap operationalizes het model dat wordt gemaakt van de stap modelleren. Hierdoor worden de vereisten en de vereiste instellingen. Het verbruik van de webservice wordt ook uitgelegd. In deze zelfstudie leert u grondige learning-modellen met het Azure Machine Learning-pakket voor computer vision maken en het model in Azure operationeel.

## <a name="next-steps"></a>Volgende stappen
- Lezen van aanvullende documentatie over [Azure Machine Learning-pakket voor computer vision](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Lees de [Team gegevens wetenschap proces](https://aka.ms/tdsp) documentatie om te beginnen.


## <a name="references"></a>Verwijzingen

* [Azure Machine Learning-pakket voor visie op computer](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [Gegevens wetenschappelijke virtuele machine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

