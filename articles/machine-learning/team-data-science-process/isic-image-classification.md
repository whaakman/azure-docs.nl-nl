---
title: Classificatie van de installatiekopie met Azure Machine Learning (AML)-pakket voor Computer Vision (AMLPCV) en Team gegevens wetenschappelijke processen (TDSP) | Microsoft Docs
description: Gebruik van TDSP (Team gegevens wetenschap proces) en AMLPCV voor classificatie van de afbeelding beschrijft
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 18/05/2018
ms.author: xibingao
ms.openlocfilehash: cfb49f08a245fc08ba3fe78333e801ea2d358c4a
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="skin-cancer-image-classification-with-azure-machine-learning-aml-package-for-computer-vision-amlpcv-and-team-data-science-process-tdsp"></a>Skin kanker installatiekopie classificatie met Azure Machine Learning (AML)-pakket voor Computer Vision (AMLPCV) en Team gegevens wetenschap proces (TDSP)

## <a name="introduction"></a>Inleiding

Dit artikel laat zien hoe u de [Azure Machine Learning-pakket voor Computer Vision (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) om te trainen, testen en implementeren van een **installatiekopie classificatie** model. Dit voorbeeld worden de TDSP structuur en sjablonen in [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). Het complete voorbeeld vindt u in dit scenario. Hierbij [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) als de deep framework leren en training wordt uitgevoerd op een [gegevens wetenschappelijke VM](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) GPU-machine. Implementatie gebruikmaakt van de Azure ML uitoefening CLI.

De frame van veel toepassingen in het domein van de visie computer kunnen worden geplaatst als afbeelding classificatie problemen. Deze omvatten ontwikkelen van modellen dat antwoorden op vragen zoals 'Is een aanwezig is in de installatiekopie van het object?' (object kan worden *aquaduct*, *auto*, of *verzenden*) en hoe complexer vragen zoals 'welke klasse ogen ziekte ernst evinced door deze geduld retinal scan?' AMLPCV stroomlijnt installatiekopie classificatie gegevensverwerking en modellering pijplijn. 

## <a name="link-to-github-repository"></a>Koppeling naar de GitHub-opslagplaats
We bieden hier samenvatting documentatie over het voorbeeld. Uitgebreidere documentatie vindt u op de [GitHub site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-tdsp-walkthrough-with-amlpcv"></a>Team gegevens wetenschap proces (TDSP)-scenario met AMLPCV

In dit scenario maakt gebruik van de [Team gegevens wetenschap proces (TDSP)](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) levenscyclus.

Het scenario omvat de volgende stappen in de levenscyclus van:

### <a name="1-data-acquisionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Gegevens acquision](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
ISIC gegevensset wordt gebruikt voor de installatiekopie van classificatie-taak. ISIC (de internationale Skin Imaging samenwerking) is een partership tussen academische wereld en industrie ter bevordering van de toepassing van digitale skin te bestuderen te reduceren melanoma sterfte imaging. De [ISIC archief](https://isic-archive.com/#images) meer dan 13.000 skin beschadiging afbeeldingen met labels onschadelijk of kwaadaardig bevat. We downloaden een voorbeeld van de installatiekopieën van het van ISIC archief.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modeling](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
In stap modellering, worden de volgende substappen uitgevoerd. 

<b>2.1 gegevensset maken</b><br>

Om een Dataset-object in AMLPCV genereren, bieden een hoofdmap van installatiekopieën op de lokale schijf. 

<b>2.2 visualisatie en aantekening images</b><br>

De afbeeldingen in het object dataset visualiseren en enkele van de labels Corrigeer zo nodig.

<b>2.3 installatiekopie uitdiepen</b><br>

Uitbreiden van een dataset-object met behulp van de transformaties beschreven in de [imgaug](https://github.com/aleju/imgaug) bibliotheek.

<b>2.4 DNN modeldefinitie</b><br>

De model-architectuur gebruikt in de stap training definiëren. Zes verschillende per getraind diep neurale netwerk modellen worden ondersteund in AMLPCV: AlexNet, Resnet 18 Resnet-34 en Resnet 50, Resnet 101 en Resnet 152.

<b>2.5 classificatie Training</b><br>

Training van de neural netwerken met standaard of aangepaste parameters.

<b>2.6-evaluatie en visualisatie</b><br>

De Substap biedt functionaliteit om het evalueren van de prestaties van het getrainde model op een onafhankelijke testgegevensset. De evaluatie van metrische gegevens behoren nauwkeurigheid, precisie en intrekken en ROC-curve.

Deze substappen worden gedetailleerd beschreven in de bijbehorende Jupyter-Notebook. We ook richtlijnen gegeven voor het inschakelen van de parameters zoals learning snelheid, mini batchgrootte en Drop-out snelheid de prestaties van het model verder te verbeteren.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Implementatie](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Deze stap operationalizes het model van de stap modellering geproduceerd. Het geeft de uitoefening vereisten en installatie. Ten slotte wordt het verbruik van de webservice ook uitgelegd. Voor deze zelfstudie leert u hoe diep learning-modellen met AMLPCV maken en het model in Azure operationeel.

## <a name="next-steps"></a>Volgende stappen
Lees meer documentatie op [Azure Machine Learning-pakket voor Computer Vision (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) en [Team gegevens wetenschap proces (TDSP)](https://aka.ms/tdsp) aan de slag.


## <a name="references"></a>Verwijzingen

* [Azure Machine Learning-pakket voor Computer Vision (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [VM voor Gegevenswetenschap](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

