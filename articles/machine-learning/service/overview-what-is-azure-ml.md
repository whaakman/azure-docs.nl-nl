---
title: Wat is?
titleSuffix: Azure Machine Learning service
description: 'Overzicht van de Azure Machine Learning-service: een geïntegreerde end-to-end-oplossing voor de gegevenswetenschap, die professionele gegevenswetenschappers in staat stelt geavanceerde analysetoepassingen te ontwikkelen, hiermee te experimenteren en deze op cloudschaal te implementeren.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
author: j-martens
ms.author: jmartens
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 201ee251b195845e33ed3829be8540664811f2ab
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025294"
---
# <a name="what-is-azure-machine-learning-service"></a>Wat is de Azure Machine Learning-service?

Azure Machine Learning Service is een cloudservice waarmee u Machine Learning-modellen kunt trainen, implementeren, automatiseren en beheren, en dit allemaal op de grote schaal van de cloud.

## <a name="what-is-machine-learning"></a>Wat is machine learning?

Machine learning is een techniek van gegevenswetenschap die computers in staat stelt om op basis van bestaande gegevens, toekomstig gedrag, resultaten en trends te voorspellen. Met behulp van machine learning kunnen computers leren zonder dat ze expliciet worden geprogrammeerd.

Prognoses of voorspellingen op basis van machine learning kunnen apps en apparaten slimmer maken. Wanneer u bijvoorbeeld online winkelt, helpt machine learning op basis van wat u eerder hebt gekocht, bij het aanraden van andere producten waarvoor u mogelijk belangstelling hebt. Of wanneer uw creditcard wordt gebruikt, vergelijkt machine learning de transactie met een database van transacties om fraude te detecteren. En wanneer uw robotstofzuiger een kamer stofzuigt, helpt machine learning bij het besluiten wanneer de taak klaar is.

## <a name="what-is-azure-machine-learning-service"></a>Wat is de Azure Machine Learning-service?

Azure Machine Learning Service biedt een omgeving in de cloud die u kunt gebruiken om machine learning-modellen te trainen, testen, implementeren, beheren en volgen en er gegevens voor voor te bereiden. Training op uw lokale computer starten en schaal vervolgens naar de cloud. De service biedt volledige ondersteuning voor open-source technologieën zoals PyTorch, TensorFlow en scikit-informatie en kan worden gebruikt voor alle soorten machine learning, van klassieke ml, deep learning, onder supervisie en leren zonder supervisie. 

Verkennen en gegevens voorbereiden, trainen en testen van modellen en deze implementeren met behulp van krachtige hulpprogramma's zoals:
+ Een [visuele interface](ui-quickstart-run-experiment.md) in die u kunt slepen en neerzetten van modules uw experimenten opzetten en vervolgens implementeert modellen
+ [Jupyter-notebooks](https://jupyter.org) in die u gebruikt de [SDK's](https://docs.microsoft.com/azure/machine-learning/service/#reference) zoals uw eigen code schrijven [deze voorbeeld-notebooks](https://aka.ms/aml-notebooks)
+ [Visual Studio Code-extensie](how-to-vscode-tools.md)

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Wat kan ik doen met de Azure Machine Learning-service?

Met behulp van de <a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Python SDK</a> met open-source Python-pakketten of met behulp van de [visuele interface (preview)](ui-quickstart-run-experiment.md), u kunt bouwen en uiterst nauwkeurige machine learning en deep learning te trainen zelf-modellen in de werkruimte van een Azure Machine Learning-service.

U kunt kiezen uit veel machine learning onderdelen beschikbaar in open-source Python-pakketten, zoals <a href="https://scikit-learn.org/stable/" target="_blank">Scikit meer</a>, <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>, <a href="https://pytorch.org" target="_blank">PyTorch</a>, en <a href="https://mxnet.io" target="_blank">MXNet</a>.

Of u code schrijven of gebruik de visuele interface, kunt u verschillende runs kunt bijhouden, zoals u kunt experimenteren om uit te vinden van de beste oplossing en de geïmplementeerde modellen beheren.

### <a name="code-first-experience"></a>Code op de eerste ervaring

Training starten op uw lokale computer met de <a href="https://aka.ms/aml-sdk" target="_blank">Azure Machine Learning Python SDK</a> en schaal vervolgens naar de cloud. Dankzij de vele beschikbare [rekendoelen](how-to-set-up-training-targets.md) zoals Azure Machine Learning Compute en [Azure Databricks](/azure/azure-databricks/what-is-azure-databricks), en [geavanceerde afstemmingsservices van hyperparameters](how-to-tune-hyperparameters.md), kunt u sneller betere modellen bouwen, met de kracht van de cloud.

U kunt ook [modeltraining en afstemmen automatiseren](tutorial-auto-train-models.md) met behulp van de SDK.

### <a name="code-free--low-code-experience"></a>Zonder code / geringe code-ervaring

Voor de training zonder code kunt u in het volgende proberen:

+ De visuele interface voor slepen en neerzetten te experimenteren en implementatie
    
    ![Visuele interface voor Azure Machine Learning-service](media/overview-what-is-azure-ml/visual-interface.png)

+ De optie in de Azure portal voor geautomatiseerde ML experimenten

### <a name="operationalization-mlops"></a>Uitoefening (MLOps)

Wanneer u het juiste model hebt, kunt u deze eenvoudig in een webservice op een IoT-apparaat of vanuit Power BI gebruiken. Zie het artikel [Hoe implementeren en waar](how-to-deploy-and-where.md) voor meer informatie. 

Daarna kunt u uw geïmplementeerde modellen beheren met de [Azure Machine Learning SDK voor Python](https://aka.ms/aml-sdk) of [Azure Portal](https://portal.azure.com/). 

Deze modellen kunnen worden gebruikt en terug te keren voorspellingen in [realtime](how-to-consume-web-service.md) of [asynchroon](how-to-run-batch-predictions.md) op grote hoeveelheden gegevens.

En met geavanceerde [machine learning-pijplijnen](concept-ml-pipelines.md), kan samenwerken aan elke stap van de gegevens voor te bereiden, modeltraining en evaluatie, tot implementatie.

Bekijk [Volgende stappen](#next-steps) om te beginnen met de Azure Machine Learning-service.

## <a name="how-does-azure-machine-learning-service-differ-from-studio"></a>Hoe verschilt Azure Machine Learning-service van de Studio?

[Machine Learning Studio](../studio/what-is-ml-studio.md) is een samenwerkingsverband, slepen en neerzetten visuele werkruimte waar u kunt bouwen, testen en implementeren van machine learning-oplossingen zonder code te schrijven. Het maakt gebruik van vooraf gedefinieerde en vooraf geconfigureerde machine learning-algoritmen en modules van de verwerking van gegevens, evenals een bedrijfslicentie compute-platform.

Azure Machine Learning-service biedt zowel SDK's **- en -** een visual interface(preview), snel klaarmaken voor gegevens, trainen en implementeren van machine learning-modellen. Deze visuele interface (preview) biedt een vergelijkbare ervaring met slepen en neerzetten naar Studio. Echter, in tegenstelling tot de eigen computerplatform van Studio, de visuele interface maakt gebruik van uw eigen rekenresources en is volledig geïntegreerd in Azure Machine Learning-service.

Hier volgt een snelle vergelijking.

|| Machine Learning Studio | Azure Machine Learning-service:<br/>Visuele interface|
|---| --- | --- |
|| In het algemeen beschikbaar is (GA) | In preview|
|Modules voor interface| Many | Eerste reeks populaire modules|
|Training compute-doelen| Bedrijfseigen compute-doel, CPU-ondersteuning| Biedt ondersteuning voor Azure Machine Learning-Computing, GPU of CPU.<br/>(Andere berekeningen worden ondersteund in de SDK)|
|Implementatie van compute-doelen| Eigen web service indeling, die niet aanpasbaar zijn | Enterprise beveiligingsopties & Azure Kubernetes Service. <br/>([Andere berekeningen](how-to-deploy-and-where.md) ondersteund in de SDK) |
|Geautomatiseerde modeltraining en hyperparameter afstemmen | Nee | Nog niet in de visuele interface. <br/> (Ondersteund in de SDK en Azure-portal.) | 

De visuele interface (preview) uitproberen met [Quick Start: Voorbereiden en visualiseren van gegevens zonder code te schrijven](ui-quickstart-run-experiment.md)

> [!NOTE]
> Modellen die zijn gemaakt in Studio kunnen niet worden geïmplementeerd of beheerd door Azure Machine Learning-service. Modellen die zijn gemaakt en geïmplementeerd in de visuele interface service kunnen echter worden beheerd via de werkruimte van de Azure Machine Learning-service.

## <a name="free-trial"></a>Gratis proefversie

Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning-service](https://aka.ms/AMLFree).

U ontvangt tegoed wat u kunt uitgeven aan Azure-services. Als uw tegoed op is, kunt u het account behouden en de [gratis Azure-services](https://azure.microsoft.com/free/) gebruiken. Er worden nooit kosten in rekening gebracht bij uw creditcard tenzij u de instellingen expliciet wijzigt en aangeeft dat u wilt betalen. U kunt ook [de voordelen voor MSDN-abonnees activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F), waardoor u maandelijks tegoeden krijgt die u kunt gebruiken voor betaalde Azure-services.

## <a name="next-steps"></a>Volgende stappen

- [Maken van een werkruimte van de service Machine Learning](setup-create-workspace.md) aan de slag.

- Volg de volledige zelfstudies: 
  + [Een model voor de classificatie van afbeeldingen trainen met de Azure Machine Learning Service](tutorial-train-models-with-aml.md) 
  + [Gegevens voorbereiden en geautomatiseerde machine learning gebruiken om een regressiemodel automatisch te trainen](tutorial-data-prep.md)

- Gebruik de [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk) om uw gegevens voor te bereiden.

- Leer meer over [Machine Learning-pijplijnen](/azure/machine-learning/service/concept-ml-pipelines) om uw Machine Learning-scenario’s te bouwen, te optimaliseren en te beheren.

- Lees het gedetailleerde artikel [Azure Machine Learning service architecture and concepts](concept-azure-machine-learning-architecture.md) (Architectuur en begrippen van de Azure Machine Learning-service).

- Raadpleeg [andere machine learning-producten van Microsoft](/azure/architecture/data-guide/technology-choices/data-science-and-machine-learning) voor meer informatie.
