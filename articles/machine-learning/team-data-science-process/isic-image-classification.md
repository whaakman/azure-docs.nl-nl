---
title: Afbeeldingsclassificatie met de Azure Machine Learning-pakket voor computer vision en Team Data Science Process (TDSP) | Microsoft Docs
description: Beschrijft het gebruik van Team Data Science Process (TDSP) en het Azure Machine Learning-pakket voor computer vision voor classificatie van afbeeldingen.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: ee2e797f3838b8b6b36174d14c73e97fe9790315
ms.sourcegitcommit: b4a46897fa52b1e04dd31e30677023a29d9ee0d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49392809"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Weergavekader kanker afbeeldingsclassificatie met de Azure Machine Learning-pakket voor computer vision en Team Data Science Process

In dit artikel leest u hoe u de [Azure Machine Learning-pakket voor computer vision-](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) om te trainen, testen en implementeren van een *afbeeldingsclassificatie* model. In het voorbeeld wordt de structuur van Team Data Science Process (TDSP) en de sjablonen in [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation). In dit scenario biedt het complete voorbeeld. Hierbij de [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) als de deep learning framework en training wordt uitgevoerd op een [Data Science virtual machine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview) GPU-machine. Implementatie maakt gebruik van de uitoefening van de Azure Machine Learning CLI.

Veel toepassingen in het domein van de computer vision kunnen worden opgesteld als afbeelding classificatie problemen. Deze omvatten het ontwikkelen van modellen die eenvoudige vragen beantwoorden als 'Is een aanwezig zijn in de installatiekopie van het object?' het object kan waar zijn een hond-, auto- of verzenden. Dit omvat ook antwoorden op complexere vragen zoals "welke klasse van ogen ziekten ernst is evinced door retinal scan van de patiënt?" De Azure Machine Learning-pakketten voor computer vision-stroomlijnt installatiekopie classificatie gegevens verwerken en de pijplijn modelleren. 

## <a name="link-to-the-github-repository"></a>Koppeling naar de GitHub-opslagplaats
Dit artikel bevat een samenvatting over de voorbeeld. U kunt uitgebreidere documentatie vinden op de [GitHub-site](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Overzicht van team Data Science Process

In deze walkthrough wordt de [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) levenscyclus. Het scenario omvat de volgende stappen in de levenscyclus.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Gegevens ophalen](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
De gegevensset internationale weergavestijl Imaging samenwerking (ISIC) wordt gebruikt voor de installatiekopie-classificatietaak. ISIC is een partnerschap tussen academische wereld en de bedrijfstak van de toepassing van digitale weergavestijl imaging om te bestuderen en het verminderen van melanoma sterfte te vergemakkelijken. De [ISIC archief](https://isic-archive.com/#images) meer dan 13.000 weergavestijl beschadiging installatiekopieën die zijn gelabeld als onschadelijke of kwaadaardige bevat. Een voorbeeld van de installatiekopieën uit het archief van ISIC downloaden.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Model maken](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
In de stap modellering van worden de volgende substappen uitgevoerd.

#### <a name="dataset-creation"></a>Gegevensset maken

Een gegevensset-object in de Azure Machine Learning-pakketten voor computer vision-genereren door op te geven van een hoofdmap van installatiekopieën op de lokale schijf. 

#### <a name="image-visualization-and-annotation"></a>Afbeelding visualisatie en aantekening

Visualiseer de installatiekopieën in de gegevensset-object en de labels Corrigeer indien nodig.

#### <a name="image-augmentation"></a>Afbeelding uitdiepen

Een object voor gegevensset verbeteren met behulp van de transformaties die worden beschreven in de [imgaug](https://github.com/aleju/imgaug) bibliotheek.

#### <a name="dnn-model-definition"></a>Definitie van DNN-model

Definieer de model-architectuur die wordt gebruikt in de stap training. Zes vooraf getrainde modellen voor deep neural network worden ondersteund in de Azure Machine Learning-pakketten voor computer vision: AlexNet, Resnet robmazz van 18, Resnet 34, Resnet 50, Resnet 101 en Resnet 152.

#### <a name="classifier-training"></a>Classificatie training

Het neurale netwerken met standaard of aangepaste parameters trainen.

#### <a name="evaluation-and-visualization"></a>Evaluatie en visualisatie

Bieden functionaliteit waarmee u kunt de prestaties van het getrainde model op een set met onafhankelijke testgegevens evalueren. De metrische evaluatiegegevens bevatten nauwkeurigheid, precisie, intrekken en ROC-curve

De substappen worden in detail uitgelegd in de bijbehorende Jupyter-Notebook. De notebook bevat ook richtlijnen voor het inschakelen van de parameters, zoals het leertempo, mini batchgrootte en Drop-out snelheid de modelprestaties verder te verbeteren.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Implementatie](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

Deze stap operationalizes het model dat wordt gemaakt van de stap modelleren. Het bevat de vereisten en de vereiste instellingen. Het verbruik van de webservice wordt ook uitgelegd. In deze zelfstudie leert u deep learning-modellen met de Azure Machine Learning-pakketten voor computer vision-bouwen en het model in Azure uitvoeren.

## <a name="next-steps"></a>Volgende stappen
- Aanvullende documentatie voor meer informatie over [Azure Machine Learning-pakket voor computer vision-](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Lees de [Team Data Science Process](https://aka.ms/tdsp) documentatie aan de slag.


## <a name="references"></a>Verwijzingen

* [Azure Machine Learning-pakket voor computer vision](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/azure/machine-learning/service/quickstart-installation)
* [Virtuele machine voor Datatechnologie](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

