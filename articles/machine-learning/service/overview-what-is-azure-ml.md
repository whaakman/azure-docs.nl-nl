---
title: Info
titleSuffix: Azure Machine Learning service
description: Overzicht van de Azure Machine Learning service, een geïntegreerde end-to-end-oplossing voor de gegevenswetenschap, die professionele gegevenswetenschappers in staat stelt geavanceerde analysetoepassingen te ontwikkelen, hiermee te experimenteren en deze op cloudschaal te implementeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: a9e91a2c11dbef3b3d9b887bfd6bb4b3372f3523
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408609"
---
# <a name="what-is-azure-machine-learning-service"></a>Wat is de Azure Machine Learning-service?

De Azure Machine Learning service is een cloudservice waarmee u machine learning-modellen kunt trainen, implementeren, automatiseren en beheren, en dit allemaal op de grote schaal van de cloud.

## <a name="what-is-machine-learning"></a>Wat is machine learning?

Machine learning is een techniek van gegevenswetenschap die computers in staat stelt om op basis van bestaande gegevens, toekomstig gedrag, resultaten en trends te voorspellen. Met behulp van machine learning kunnen computers leren zonder dat ze expliciet worden geprogrammeerd.

Prognoses of voorspellingen op basis van machine learning kunnen apps en apparaten slimmer maken. Wanneer u bijvoorbeeld online winkelt, helpt machine learning op basis van wat u eerder hebt gekocht, bij het aanraden van andere producten waarvoor u mogelijk belangstelling hebt. Of wanneer uw creditcard wordt gebruikt, vergelijkt machine learning de transactie met een database van transacties om fraude te detecteren. En wanneer uw robotstofzuiger een kamer stofzuigt, helpt machine learning bij het besluiten wanneer de taak klaar is.

## <a name="what-is-azure-machine-learning-service"></a>Wat is Azure Machine Learning-service?

Azure Machine Learning-service biedt een omgeving in de cloud die u kunt gebruiken om machine learning-modellen te ontwikkelen, trainen, testen, implementeren, beheren en volgen.

[ ![Azure Machine Learning-serviceworkflow](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

Azure Machine Learning-service ondersteunt open-source technologieën volledig, zodat u tienduizenden open-source Python-pakketten kunt gebruiken met machine learning-componenten als TensorFlow en scikit-learn.
Ondersteuning voor uitgebreide hulpprogramma’s als [Jupyter Notebooks](http://jupyter.org) of de [Azure Machine Learning for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai#overview)-extensie maakt het eenvoudig om interactief gegevens te verkennen, te transformeren en dan modellen te ontwikkelen en te testen.
Azure Machine Learning-service omvat ook functies die [modelgeneratie automatiseren en afstemmen](tutorial-auto-train-models.md)om u te helpen met gemak, efficiënt en nauwkeurig modellen te maken.

Met Azure Machine Learning-service kunt u op uw lokale machine beginnen met trainen en dan opschalen naar de cloud. Dankzij de vele beschikbare [rekendoelen](how-to-set-up-training-targets.md) zoals Azure Machine Learning Compute en [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks), en [geavanceerde afstemmingsservices van hyperparameters](how-to-tune-hyperparameters.md), kunt u sneller betere modellen bouwen, met de kracht van de cloud.

Al u het juiste model hebt, kunt u het eenvoudig implementeren in een container zoals Docker. Dit betekent dat het eenvoudig is Azure Container Instances of Azure Kubernetes Service te implementeren, of u kunt de container gebruiken in uw eigen implementaties, zowel on-premises als in de cloud. Zie het document [Hoe implementeren en waar](how-to-deploy-and-where.md) voor meer informatie.
U kunt de geïmplementeerde modellen beheren en meerdere uitvoeringen volgen terwijl u experimenteert om de beste oplossing te vinden.
Wanneer uw model is geïmplementeerd, kan het in [realtime](how-to-consume-web-service.md) of [asynchroon](how-to-run-batch-predictions.md) voorspellingen retourneren voor grote hoeveelheden gegevens.

En met geavanceerde [machine learning-pijplijnen](concept-ml-pipelines.md) kunt u samenwerken aan alle stappen van de gegevensvoorbereiding, modeltraining en -evaluatie, en implementatie.

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Wat kan ik doen met de Azure Machine Learning-service?

Met de Azure Machine Learning service kunt u een model automatisch trainen en voor u laten afstemmen.
Zie [Zelfstudie: Automatisch een classificatiemodel trainen met Azure Automated Machine Learning](tutorial-auto-train-models.md) voor een voorbeeld.

Met de Azure Machine Learning <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> voor Python kunt u samen met open-source Python-pakketten uiterst nauwkeurige machine learning- en deep learning-modellen bouwen en trainen in een werkruimte van de Azure Machine Learning service.
U kunt kiezen uit veel machine learning-componenten die beschikbaar zijn in open-source Python-pakketten, zoals de volgende:

- <a href="https://scikit-learn.org/stable/" target="_blank">Scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/en-us/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Zodra u een model hebt, kunt u het gebruiken om een container (zoals Docker) te maken die lokaal kan worden geïmplementeerd voor testdoeleinden. Zodra het testen is voltooid, kan het model als een productiewebservice worden geïmplementeerd in Azure Container Instances of Azure Kubernetes Service. Zie het document [Hoe implementeren en waar](how-to-deploy-and-where.md) voor meer informatie.

Daarna kunt u uw geïmplementeerde modellen beheren met de [Azure Machine Learning SDK voor Python](https://aka.ms/aml-sdk) of de [Azure-portal](https://portal.azure.com/).
U kunt de metrische gegevens van een model evalueren, nieuwe versies van het model opnieuw trainen en implementeren en dat allemaal terwijl u de experimenten van het model volgt.

Bekijk [Volgende stappen](#next-steps) hieronder om te beginnen met Azure Machine Learning-service.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>Waarin verschilt Azure Machine Learning-service van Studio?

Azure Machine Learning Studio is een visuele werkruimte voor samenwerking met slepen en neerzetten waar u machine learning-oplossingen kunt bouwen, testen en implementeren zonder dat u code hoeft te programmeren. Er wordt gebruikgemaakt van vooraf gebouwde en geconfigureerde machine learning-algoritmes en modules voor gegevensverwerking.

Gebruik Machine Learning Studio wanneer u snel en eenvoudig wilt experimenteren met machine learning-modellen en de ingebouwde machine learning-algoritmen voldoende zijn voor uw oplossingen.

Gebruik Machine Learning-service als u werkt in een Python-omgeving, u meer controle wenst over uw machine learning-algoritmen of als u open-source machine learning-bibliotheken wilt gebruiken.

> [!NOTE]
> Modellen die zijn gemaakt in Azure Machine Learning Studio kunnen niet worden geïmplementeerd of beheerd door Azure Machine Learning-service.

## <a name="free-trial"></a>Gratis proefversie

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning-service](http://aka.ms/AMLFree).

U ontvangt tegoed wat u kunt uitgeven aan Azure-services. Als uw tegoed op is, kunt u het account behouden en de [gratis Azure-services](https://azure.microsoft.com/free/) gebruiken. Er worden nooit kosten in rekening gebracht bij uw creditcard tenzij u de instellingen expliciet wijzigt en aangeeft dat u wilt betalen. U kunt ook [de voordelen voor MSDN-abonnees activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F), waardoor u maandelijks tegoeden krijgt die u kunt gebruiken voor betaalde Azure-services.

## <a name="next-steps"></a>Volgende stappen

- Maak een werkruimte voor de Machine Learning service om aan de slag te gaan [met de Azure-portal](quickstart-get-started.md) of [in Python](quickstart-create-workspace-with-python.md).

- Volg de volledige zelfstudie [Een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning-service](tutorial-train-models-with-aml.md).

- [Gebruik Azure Machine Learning om automatisch een model te genereren en af te stemmen](tutorial-auto-train-models.md).

- Gebruik de [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk) om uw gegevens voor te bereiden.

- Leer meer over [Machine Learning-pijplijnen](/azure/machine-learning/service/concept-ml-pipelines) om uw Machine Learning-scenario’s te bouwen, te optimaliseren en te beheren.

- Lees het gedetailleerde artikel [Azure Machine Learning service architecture and concepts](concept-azure-machine-learning-architecture.md) (Architectuur en begrippen van de Azure Machine Learning-service).

- Zie [Wat zijn de machine learning-producten van Microsoft?](./overview-more-machine-learning.md) voor meer informatie over andere machine learning-producten van Microsoft.


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->
